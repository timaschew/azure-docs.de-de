---
title: Erstellen eines HTTPS-Eingangscontrollers mit einem Azure Kubernetes Service-Cluster (AKS)
description: Erfahren Sie, wie Sie einen NGINX-Eingangscontroller installieren und konfigurieren, der Let‘s Encrypt für die automatische SSL-Zertifikatsgenerierung in einem Azure Kubernetes Service-Cluster (AKS) verwendet.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 08/30/2018
ms.author: iainfou
ms.openlocfilehash: 7a7acb7da2e5999fc844fe6c6c2e191783df9190
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50093960"
---
# <a name="create-an-https-ingress-controller-on-azure-kubernetes-service-aks"></a>Erstellen eines HTTPS-Eingangscontrollers in Azure Kubernetes Service (AKS)

Ein Eingangscontroller ist eine Softwarekomponente, die einen Reverseproxy, konfigurierbare Datenverkehrsweiterleitung und TLS-Terminierung für Kubernetes-Dienste bereitstellt. Mithilfe von Ressourcen für eingehende Kubernetes-Daten werden Eingangsregeln und Routen für einzelne Kubernetes-Dienste konfiguriert. Durch die Verwendung von einem Eingangscontroller und Eingangsregeln kann eine einzelne IP-Adresse zum Weiterleiten von Datenverkehr an mehrere Dienste in einem Kubernetes-Cluster verwendet werden.

Dieser Artikel beschreibt, wie Sie den [NGINX-Eingangscontroller][nginx-ingress] in einem Azure Kubernetes Service-Cluster (AKS) bereitstellen. Das Projekt [cert-manager][cert-manager] wird verwendet, um automatisch [Let‘s Encrypt][lets-encrypt]-Zertifikate zu generieren und zu konfigurieren. Schließlich werden zwei Anwendungen im AKS-Cluster ausgeführt, die jeweils über eine einzelne IP-Adresse zugänglich sind.

Weitere Funktionen:

- [Erstellen eines einfachen Eingangscontrollers mit Verbindung mit einem externen Netzwerk][aks-ingress-basic]
- [Aktivieren des Add-Ons für das HTTP-Anwendungsrouting][aks-http-app-routing]
- [Erstellen eines Eingangscontrollers, der ein internes, privates Netzwerk und eine IP-Adresse verwendet][aks-ingress-internal]
- [Erstellen eines Eingangscontrollers, der Ihre eigenen TLS-Zertifikate verwendet][aks-ingress-own-tls]
- [Erstellen eines Eingangscontrollers, der mit Let's Encrypt automatisch TLS-Zertifikate mit einer statischen öffentlichen IP-Adresse generiert][aks-ingress-static-tls]

## <a name="before-you-begin"></a>Voraussetzungen

In diesem Artikel wird Helm verwendet, um den NGINX-Eingangscontroller, cert-manager und eine Beispiel-Web-App zu installieren. Helm muss im AKS-Cluster initialisiert sein und ein Dienstkonto für Tiller verwenden. Stellen Sie sicher, dass Sie das neueste Release von Helm verwenden. Anweisungen zum Upgrade finden Sie in der [Helm-Installationsdokumentation][helm-install]. Weitere Informationen zum Konfigurieren und Verwenden von Helm finden Sie unter [Installieren von Anwendungen mit Helm in Azure Kubernetes Service (AKS)][use-helm].

Für den Artikel müssen Sie mindestens Version 2.0.41 der Azure-Befehlszeilenschnittstelle (Azure CLI) ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0][azure-cli-install] Informationen dazu.

## <a name="create-an-ingress-controller"></a>Erstellen eines Eingangscontrollers

Verwenden Sie zum Erstellen des Eingangscontrollers `Helm` zum Installieren von *nginx-ingress*. Für zusätzliche Redundanz werden zwei Replikate der NGINX-Eingangscontroller mit dem Parameter `--set controller.replicaCount` bereitgestellt. Um vollständig von der Ausführung von Replikaten des Eingangscontrollers zu profitieren, stellen Sie sicher, dass sich mehr als ein Knoten im AKS-Cluster befindet.

> [!TIP]
> Im folgenden Beispiel wird der Eingangscontroller im `kube-system`-Namespace installiert. Sie können bei Bedarf einen anderen Namespace für Ihre eigene Umgebung angeben. Wenn in Ihrem AKS-Cluster RBAC nicht aktiviert ist, fügen Sie den Befehlen `--set rbac.create=false` hinzu.

```console
helm install stable/nginx-ingress --namespace kube-system --set controller.replicaCount=2
```

Während der Installation wird eine öffentliche Azure-IP-Adresse für den Eingangscontroller erstellt. Diese öffentliche IP-Adresse ist für die Lebensdauer des Eingangscontrollers statisch. Wenn Sie den Eingangscontroller löschen, geht die Zuweisung der öffentlichen IP-Adresse verloren. Wenn Sie dann einen weiteren Eingangscontroller erstellen, wird eine neue öffentliche IP-Adresse zugewiesen. Wenn Sie die Verwendung der öffentlichen IP-Adresse beibehalten möchten, können Sie stattdessen [einen Eingangscontroller mit einer statischen öffentlichen IP-Adresse erstellen][aks-ingress-static-tls].

Sie rufen die öffentliche IP-Adresse mit dem Befehl `kubectl get service` ab. Es dauert möglicherweise einige Minuten, bis die IP-Adresse dem Dienst zugewiesen wird.

```
$ kubectl get service -l app=nginx-ingress --namespace kube-system

NAME                                       TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
eager-crab-nginx-ingress-controller        LoadBalancer   10.0.182.160   51.145.155.210  80:30920/TCP,443:30426/TCP   20m
eager-crab-nginx-ingress-default-backend   ClusterIP      10.0.255.77    <none>          80/TCP                       20m
```

Es wurden noch keine Eingangsregeln erstellt. Wenn Sie zu der öffentlichen IP-Adresse navigieren, wird die Standard-404-Seite des NGINX-Eingangscontrollers angezeigt.

## <a name="configure-a-dns-name"></a>Konfigurieren des DNS-Namens

Damit die HTTPS-Zertifikate ordnungsgemäß ausgeführt werden, konfigurieren Sie einen vollqualifizierten Domänennamen (FQDN) für die IP-Adresse des Eingangscontrollers. Aktualisieren Sie das folgende Skript mit der IP-Adresse des Eingangscontrollers und einem eindeutigen Namen, den Sie im FQDN verwenden möchten:

```console
#!/bin/bash

# Public IP address of your ingress controller
IP="51.145.155.210"

# Name to associate with public IP address
DNSNAME="demo-aks-ingress"

# Get the resource-id of the public ip
PUBLICIPID=$(az network public-ip list --query "[?ipAddress!=null]|[?contains(ipAddress, '$IP')].[id]" --output tsv)

# Update public ip address with DNS name
az network public-ip update --ids $PUBLICIPID --dns-name $DNSNAME
```

Der Eingangscontroller ist jetzt über den vollqualifizierten Domänennamen zugänglich.

## <a name="install-cert-manager"></a>Installieren von cert-manager

Der NGINX-Eingangscontroller unterstützt TLS-Terminierung. Es gibt verschiedene Möglichkeiten zum Abrufen und Konfigurieren von Zertifikaten für HTTPS. Dieser Artikel veranschaulicht die Verwendung von [cert-manager][cert-manager], über den die automatische Zertifikatsgenerierung und Verwaltungsfunktionalität mit [Let‘s Encrypt][lets-encrypt] bereitgestellt werden.

> [!NOTE]
> In diesem Artikel wird die `staging`-Umgebung für Let‘s Encrypt verwendet. Verwenden Sie in Produktionsbereitstellungen `letsencrypt-prod` und `https://acme-v02.api.letsencrypt.org/directory` in den Ressourcendefinitionen und bei der Installation des Helm-Diagramms.

Verwenden Sie zum Installieren des cert-manager-Controllers in einem RBAC-fähigen Cluster den folgenden `helm install`-Befehl:

```console
helm install stable/cert-manager \
    --namespace kube-system \
    --set ingressShim.defaultIssuerName=letsencrypt-staging \
    --set ingressShim.defaultIssuerKind=ClusterIssuer
```

Wenn Ihr Cluster nicht RBAC-fähig ist, verwenden Sie stattdessen den folgenden Befehl:

```console
helm install stable/cert-manager \
    --namespace kube-system \
    --set ingressShim.defaultIssuerName=letsencrypt-staging \
    --set ingressShim.defaultIssuerKind=ClusterIssuer \
    --set rbac.create=false \
    --set serviceAccount.create=false
```

Weitere Informationen zur cert-manager-Konfiguration finden Sie unter [cert-manager-Projekt][cert-manager].

## <a name="create-a-ca-cluster-issuer"></a>Erstellen eines CA-Clusterausstellers

Bevor Zertifikate ausgestellt werden können, benötigt cert-manager eine [Aussteller][cert-manager-issuer]- oder [Clusteraussteller][cert-manager-cluster-issuer]-Ressource. Die Kubernetes-Ressourcen sind funktionell identisch, aber `Issuer` arbeitet in einem einzelnen Namespace, `ClusterIssuer` hingegen in allen Namespaces. Weitere Informationen finden Sie in der Dokumentation zum [cert-manager-Aussteller][cert-manager-issuer].

Erstellen Sie einen Clusteraussteller, wie z.B. `cluster-issuer.yaml`, mithilfe des folgenden Beispielmanifests. Aktualisieren Sie die E-Mail-Adresse mit einer gültigen Adresse aus Ihrer Organisation:

```yaml
apiVersion: certmanager.k8s.io/v1alpha1
kind: ClusterIssuer
metadata:
  name: letsencrypt-staging
spec:
  acme:
    server: https://acme-staging-v02.api.letsencrypt.org/directory
    email: user@contoso.com
    privateKeySecretRef:
      name: letsencrypt-staging
    http01: {}
```

Um den Aussteller zu erstellen, verwenden Sie den Befehl `kubectl apply -f cluster-issuer.yaml`.

```
$ kubectl apply -f cluster-issuer.yaml

clusterissuer.certmanager.k8s.io/letsencrypt-staging created
```

## <a name="create-a-certificate-object"></a>Erstellen eines Zertifikatsobjekts

Als Nächstes muss eine Zertifikatressource erstellt werden. Die Zertifikatressource definiert das gewünschte X.509-Zertifikat. Weitere Informationen finden Sie unter [cert-manager-Zertifikate][cert-manager-certificates].

Erstellen Sie die Zertifikatsressource, z.B. `certificates.yaml`, mit dem folgenden Beispielmanifest. Aktualisieren Sie *dnsNames* und *domains* auf den DNS-Namen, den Sie in einem vorherigen Schritt erstellt haben. Wenn Sie einen rein internen Eingangscontroller verwenden, geben Sie den internen DNS-Namen für Ihren Dienst an.

```yaml
apiVersion: certmanager.k8s.io/v1alpha1
kind: Certificate
metadata:
  name: tls-secret
spec:
  secretName: tls-secret
  dnsNames:
  - demo-aks-ingress.eastus.cloudapp.azure.com
  acme:
    config:
    - http01:
        ingressClass: nginx
      domains:
      - demo-aks-ingress.eastus.cloudapp.azure.com
  issuerRef:
    name: letsencrypt-staging
    kind: ClusterIssuer
```

Verwenden Sie zum Erstellen der Zertifikatsressource den Befehl `kubectl apply -f certificates.yaml`.

```
$ kubectl apply -f certificates.yaml

certificate.certmanager.k8s.io/tls-secret created
```

## <a name="run-demo-applications"></a>Ausführen von Demoanwendungen

Ein Eingangscontroller und eine Zertifikatsverwaltungslösung wurden konfiguriert. Nun führen wir zwei Demoanwendungen im AKS-Cluster-aus. In diesem Beispiel wird Helm verwendet, um mehrere Instanzen einer einfachen Hallo-Welt-Anwendung auszuführen.

Bevor Sie Helm-Beispieldiagramme installieren können, fügen Sie das Azure-Beispielrepository wie folgt Ihrer Helm-Umgebung hinzu:

```console
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

Erstellen Sie die erste Demoanwendung aus einem Helm-Diagramm mit dem folgenden Befehl:

```console
helm install azure-samples/aks-helloworld
```

Installieren Sie nun eine zweite Instanz der Demoanwendung. Geben Sie für die zweite Instanz einen neuen Titel an, sodass sich die beiden Anwendungen visuell unterscheiden. Sie geben auch einen eindeutigen Dienstnamen an:

```console
helm install azure-samples/aks-helloworld --set title="AKS Ingress Demo" --set serviceName="ingress-demo"
```

## <a name="create-an-ingress-route"></a>Erstellen einer Eingangsroute

Beide Anwendungen werden nun in Ihrem Kubernetes-Cluster ausgeführt, wurden jedoch mit einem Dienst vom Typ `ClusterIP` konfiguriert. Daher kann nicht über das Internet auf die Anwendungen zugegriffen werden. Um sie öffentlich verfügbar zu machen, erstellen Sie eine Kubernetes-Eingangsressource. Die Eingangsressource konfiguriert Regeln, die den Datenverkehr an eine der beiden Anwendungen weiterleiten.

Im folgenden Beispiel wird der Datenverkehr an die Adresse `https://demo-aks-ingress.eastus.cloudapp.azure.com/` an den Dienst mit dem Namen `aks-helloworld` weitergeleitet. Datenverkehr an die Adresse `https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two` wird an den Dienst `ingress-demo` weitergeleitet. Aktualisieren Sie *hosts* und *host* auf den DNS-Namen, den Sie in einem vorherigen Schritt erstellt haben.

Erstellen Sie eine Datei mit dem Namen `hello-world-ingress.yaml`, und fügen Sie den folgenden YAML-Beispielcode ein.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    certmanager.k8s.io/cluster-issuer: letsencrypt-staging
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  tls:
  - hosts:
    - demo-aks-ingress.eastus.cloudapp.azure.com
    secretName: tls-secret
  rules:
  - host: demo-aks-ingress.eastus.cloudapp.azure.com
    http:
      paths:
      - path: /
        backend:
          serviceName: aks-helloworld
          servicePort: 80
      - path: /hello-world-two
        backend:
          serviceName: ingress-demo
          servicePort: 80
```

Erstellen Sie die Eingangsressource mit dem Befehl `kubectl apply -f hello-world-ingress.yaml`.

```
$ kubectl apply -f hello-world-ingress.yaml

ingress.extensions/hello-world-ingress created
```

## <a name="test-the-ingress-configuration"></a>Testen der Konfiguration für eingehende Daten

Öffnen Sie einen Webbrowser für den FQDN des Kubernetes-Eingangscontrollers, z.B. *https://demo-aks-ingress.eastus.cloudapp.azure.com*.

Da diese Beispiele `letsencrypt-staging` verwenden, ist das ausgestellte SSL-Zertifikat für den Browser nicht vertrauenswürdig. Akzeptieren Sie die Eingabeaufforderung der Warnung, um Ihre Anwendung zu öffnen. Die Zertifikatinformationen zeigen, dass dieses *Fake LE Intermediate X1*-Zertifikat von Let‘s Encrypt ausgestellt wurde. Dieses gefälschte Zertifikat gibt an, dass `cert-manager` die Anforderung korrekt verarbeitet und ein Zertifikat vom Anbieter erhalten hat:

![Let's Encrypt-Bereitstellungszertifikat](media/ingress/staging-certificate.png)

Wenn Sie Let‘s Encrypt so ändern, dass `prod` anstelle von `staging` verwendet wird, wird ein von Let‘s Encrypt ausgestelltes vertrauenswürdiges verwendet, wie im folgenden Beispiel gezeigt:

![Let‘s Encrypt-Zertifikat](media/ingress/certificate.png)

Die Demoanwendung wird im Webbrowser angezeigt:

![Anwendungsbeispiel 1](media/ingress/app-one.png)

Fügen Sie nun den Pfad */hello-world-two* dem FQDN hinzu, wie z.B. *https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two*. Die zweite Demoanwendung mit dem benutzerdefinierten Titel wird angezeigt:

![Anwendungsbeispiel 2](media/ingress/app-two.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

In diesem Artikel wird Helm verwendet, um die Eingangskomponenten, die Zertifikate und die Beispiel-Apps zu installieren. Wenn Sie ein Helm-Diagramm bereitstellen, werden eine Reihe von Kubernetes-Ressourcen erstellt. Diese Ressourcen enthalten Pods, Bereitstellungen und Dienste. Entfernen Sie zum Bereinigen zuerst die Zertifikatressourcen:

```console
kubectl delete -f certificates.yaml
kubectl delete -f cluster-issuer.yaml
```

Listen Sie nun mit dem Befehl `helm list` die Helm-Versionen auf. Suchen Sie nach Diagrammen mit den Namen *nginx-ingress*, *cert-manager* und *aks-helloworld*, wie in der folgenden Beispielausgabe gezeigt:

```
$ helm list

NAME                    REVISION    UPDATED                     STATUS      CHART                   APP VERSION NAMESPACE
billowing-kitten        1           Tue Oct 16 17:24:05 2018    DEPLOYED    nginx-ingress-0.22.1    0.15.0      kube-system
loitering-waterbuffalo  1           Tue Oct 16 17:26:16 2018    DEPLOYED    cert-manager-v0.3.4     v0.3.2      kube-system
flabby-deer             1           Tue Oct 16 17:27:06 2018    DEPLOYED    aks-helloworld-0.1.0                default
linting-echidna         1           Tue Oct 16 17:27:02 2018    DEPLOYED    aks-helloworld-0.1.0                default
```

Löschen Sie die Versionen mit dem Befehl `helm delete`. Im folgenden Beispiel werden die NGINX-Eingangsbereitstellung, der Zertifikat-Manager und die beiden Hallo-Welt-AKS-Beispiel-Apps gelöscht.

```
$ helm delete billowing-kitten loitering-waterbuffalo flabby-deer linting-echidna

release "billowing-kitten" deleted
release "loitering-waterbuffalo" deleted
release "flabby-deer" deleted
release "linting-echidna" deleted
```

Entfernen Sie als Nächstes das Helm-Repository für die Hallo-Welt-AKS-App:

```console
helm repo remove azure-samples
```

Entfernen Sie abschließend die Eingangsroute, die Datenverkehr an die Beispiel-Apps weitergeleitet hat:

```console
kubectl delete -f hello-world-ingress.yaml
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel werden einige externe Komponenten in AKS berücksichtigt. Weitere Informationen zu diesen Komponenten finden Sie auf den folgenden Projektseiten:

- [Helm CLI][helm-cli]
- [NGINX-Eingangscontroller][nginx-ingress]
- [cert-manager][cert-manager]

Weitere Funktionen:

- [Erstellen eines einfachen Eingangscontrollers mit Verbindung mit einem externen Netzwerk][aks-ingress-basic]
- [Aktivieren des Add-Ons für das HTTP-Anwendungsrouting][aks-http-app-routing]
- [Erstellen eines Eingangscontrollers, der ein internes, privates Netzwerk und eine IP-Adresse verwendet][aks-ingress-internal]
- [Erstellen eines Eingangscontrollers, der Ihre eigenen TLS-Zertifikate verwendet][aks-ingress-own-tls]
- [Erstellen eines Eingangscontrollers, der mit Let's Encrypt automatisch TLS-Zertifikate mit einer statischen öffentlichen IP-Adresse generiert][aks-ingress-static-tls]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm#install-helm-cli
[cert-manager]: https://github.com/jetstack/cert-manager
[cert-manager-certificates]: https://cert-manager.readthedocs.io/en/latest/reference/certificates.html
[cert-manager-cluster-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/clusterissuers.html
[cert-manager-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/issuers.html
[lets-encrypt]: https://letsencrypt.org/
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
[helm-install]: https://docs.helm.sh/using_helm/#installing-helm

<!-- LINKS - internal -->
[use-helm]: kubernetes-helm.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[aks-ingress-internal]: ingress-internal-ip.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-ingress-basic]: ingress-basic.md
[aks-http-app-routing]: http-application-routing.md
[aks-ingress-own-tls]: ingress-own-tls.md
