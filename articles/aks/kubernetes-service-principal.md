---
title: Dienstprinzipale für Azure Kubernetes Service (AKS)
description: Erstellen und Verwalten eines Azure Active Directory-Dienstprinzipals für einen Cluster im Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: get-started-article
ms.date: 09/26/2018
ms.author: iainfou
ms.openlocfilehash: 2bc0579d3dd60d66a23a29dabff7e43ca8dfee76
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/15/2018
ms.locfileid: "53435394"
---
# <a name="service-principals-with-azure-kubernetes-service-aks"></a>Dienstprinzipale mit Azure Kubernetes Service (AKS)

Für die Interaktion mit Azure-APIs benötigt ein AKS-Cluster einen [Azure Active Directory-Dienstprinzipal (AD)][aad-service-principal]. Der Dienstprinzipal wird für die dynamische Erstellung und Verwaltung anderer Azure-Ressourcen wie Azure Load Balancer oder Azure Container Registry (ACR) benötigt.

Dieser Artikel veranschaulicht das Erstellen und Verwenden eines Dienstprinzipals für Ihre AKS-Cluster.

## <a name="before-you-begin"></a>Voraussetzungen

Für die Erstellung eines Azure AD-Dienstprinzipals müssen Sie dazu berechtigt sein, eine Anwendung bei Ihrem Azure AD-Mandanten zu registrieren und die Anwendung einer Rolle in Ihrem Abonnement zuzuweisen. Sollten Sie nicht über die erforderlichen Berechtigungen verfügen, können Sie ggf. Ihren Azure AD- oder Abonnementadministrator bitten, Ihnen die erforderlichen Berechtigungen zuzuweisen oder vorab einen Dienstprinzipal für die Verwendung mit dem AKS-Cluster für Sie zu erstellen.

Außerdem muss die Version 2.0.46 oder höher der Azure-Befehlszeilenschnittstelle installiert und konfiguriert sein. Führen Sie  `az --version` aus, um die Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie weitere Informationen unter [Installieren der Azure CLI][install-azure-cli].

## <a name="automatically-create-and-use-a-service-principal"></a>Automatisches Erstellen und Verwenden eines Dienstprinzipals

Beim Erstellen eines AKS-Clusters im Azure-Portal oder mithilfe des Befehls [az aks create][az-aks-create] kann Azure automatisch einen Dienstprinzipal generieren.

Im folgenden Azure CLI-Beispiel wird kein Dienstprinzipal angegeben. In diesem Szenario erstellt die Azure-Befehlszeilenschnittstelle einen Dienstprinzipal für den AKS-Cluster. Für den erfolgreichen Abschluss dieses Vorgangs muss Ihr Azure-Konto über die erforderlichen Rechte zum Erstellen eines Dienstprinzipals verfügen.

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup --generate-ssh-keys
```

## <a name="manually-create-a-service-principal"></a>Manuelles Erstellen eines Dienstprinzipals

Verwenden Sie den Befehl [az ad sp create-for-rbac][az-ad-sp-create], um den Dienstprinzipal mit der Azure-Befehlszeilenschnittstelle manuell zu erstellen. Im folgenden Beispiel verhindert der `--skip-assignment`-Parameter, dass zusätzliche Standardzuweisungen durchgeführt werden:

```azurecli-interactive
az ad sp create-for-rbac --skip-assignment
```

Die Ausgabe sieht in etwa wie das folgende Beispiel aus: Notieren Sie sich Ihre persönlichen Angaben für `appId` und `password`. Diese Werte werden verwendet, wenn Sie im nächsten Abschnitt einen AKS-Cluster erstellen.

```json
{
  "appId": "559513bd-0c19-4c1a-87cd-851a26afd5fc",
  "displayName": "azure-cli-2018-09-25-21-10-19",
  "name": "http://azure-cli-2018-09-25-21-10-19",
  "password": "e763725a-5eee-40e8-a466-dc88d980f415",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db48"
}
```

## <a name="specify-a-service-principal-for-an-aks-cluster"></a>Angeben eines Dienstprinzipals für einen AKS-Cluster

Um einen vorhandenen Dienstprinzipal bei der Erstellung eines AKS-Clusters mithilfe des [az aks create][az-aks-create]-Befehls zu verwenden, geben Sie mit den Parametern `--service-principal` und `--client-secret` `appId` und `password` aus der Ausgabe des [az ad sp create-for-rbac][az-ad-sp-create]-Befehls an:

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --service-principal <appId> \
    --client-secret <password>
```

Wenn Sie einen AKS-Cluster über das Azure-Portal bereitstellen, wählen Sie auf der Seite *Authentication (Authentifizierung)* des **Create Kubernetes cluster (Kubernetes-Cluster erstellen)**-Dialogfelds **Configure service principal (Dienstprinzipal konfigurieren)** aus. Wählen Sie **Use existing (Vorhandene verwenden)** aus, und geben Sie die folgenden Werte an:

- **Service principal client ID (Client-ID des Dienstprinzipals)** ist Ihre *appId*.
- **Service principal client secret (Clientgeheimnis des Dienstprinzipals)** ist der Wert *password*.

![Abbildung der Navigation zu Azure Vote](media/kubernetes-service-principal/portal-configure-service-principal.png)

## <a name="delegate-access-to-other-azure-resources"></a>Delegieren des Zugriffs auf andere Azure-Ressourcen

Der Dienstprinzipal für den AKS-Cluster kann verwendet werden, um auf andere Ressourcen zuzugreifen. Wenn Sie beispielsweise über erweiterte Netzwerke eine Verbindung mit vorhandenen Netzwerken oder eine Verbindung mit Azure Container Registry (ACR) herstellen möchten, müssen Sie den Zugriff an den Dienstprinzipal delegieren.

Um Berechtigungen zu delegieren, erstellen Sie mit dem Befehl [az role assignment create][az-role-assignment-create] eine Rollenzuweisung. Sie weisen die `appId` einem bestimmten Bereich zu, beispielsweise einer Ressourcengruppe oder einer virtuellen Netzwerkressource. Eine Rolle definiert dann wie im folgenden Beispiel gezeigt die Berechtigungen, die der Dienstprinzipal für diese Ressource besitzt:

```azurecli
az role assignment create --assignee <appId> --scope <resourceScope> --role Contributor
```

Bei dem `--scope` für eine Ressource muss es sich um eine vollständige Ressourcen-ID handeln, z. B. */subscriptions/\<guid\>/resourceGroups/myResourceGroup* oder */subscriptions/\<guid\>/resourceGroups/myResourceGroupVnet/providers/Microsoft.Network/virtualNetworks/myVnet*.

In den folgenden Abschnitten werden allgemeine Delegierungen, die Sie möglicherweise vornehmen müssen, ausführlicher erläutert.

### <a name="azure-container-registry"></a>Azure Container Registry

Wenn Sie Azure Container Registry (ACR) als Speicher für Containerimages verwenden, müssen Sie Ihrem AKS-Cluster Berechtigungen zum Lesen und Pullen von Images erteilen. An den Dienstprinzipal des AKS-Clusters muss die Rolle *Leser* für die Registrierung delegiert werden. Ausführliche Anweisungen hierzu finden Sie unter [Gewähren von AKS-Zugriff auf ACR][aks-to-acr].

### <a name="networking"></a>Netzwerk

Möglicherweise verwenden Sie erweiterte Netzwerke, in denen sich das virtuelle Netzwerk und das Subnetz oder öffentliche IP-Adressen in einer anderen Ressourcengruppe befinden. Weisen Sie eine der folgenden Gruppen von Rollenberechtigungen zu:

- Erstellen Sie eine [benutzerdefinierte Rolle][rbac-custom-role], und definieren Sie die folgenden Rollenberechtigungen:
  - *Microsoft.Network/virtualNetworks/subnets/join/action*
  - *Microsoft.Network/virtualNetworks/subnets/read*
  - *Microsoft.Network/publicIPAddresses/read*
  - *Microsoft.Network/publicIPAddresses/write*
  - *Microsoft.Network/publicIPAddresses/join/action*
- Alternativ können Sie die integrierte Rolle [Netzwerkmitwirkender][rbac-network-contributor] für das Subnetz im virtuellen Netzwerk zuweisen.

### <a name="storage"></a>Storage

Möglicherweise müssen Sie auf vorhandene Datenträgerressourcen in einer anderen Ressourcengruppe zugreifen. Weisen Sie eine der folgenden Gruppen von Rollenberechtigungen zu:

- Erstellen Sie eine [benutzerdefinierte Rolle][rbac-custom-role], und definieren Sie die folgenden Rollenberechtigungen:
  - *Microsoft.Compute/disks/read*
  - *Microsoft.Compute/disks/write*
- Alternativ können Sie die integrierte Rolle [Speicherkontomitwirkender][rbac-storage-contributor] für die Ressourcengruppe zuweisen.

### <a name="azure-container-instances"></a>Azure Container Instances

Wenn Sie Virtual Kubelet für die Integration in AKS verwenden und Azure Container Instances (ACI) in einer Ressourcengruppe außerhalb des AKS-Clusters ausführen, muss dem AKS-Dienstprinzipal in der ACI-Ressourcengruppe die Berechtigung *Mitwirkender* gewährt werden.

## <a name="additional-considerations"></a>Zusätzliche Überlegungen

Beachten Sie bei Verwendung von AKS und Azure AD-Dienstprinzipalen die folgenden Kriterien.

- Der Dienstprinzipal für Kubernetes ist Bestandteil der Clusterkonfiguration. Verwenden Sie diese Identität jedoch nicht zum Bereitstellen des Clusters.
- Jeder Dienstprinzipal ist einer Azure AD-Anwendung zugeordnet. Der Dienstprinzipal für einen Kubernetes-Cluster kann einem beliebigen gültigen Azure AD-Anwendungsnamen zugeordnet sein (z.B. *https://www.contoso.org/example*). Bei der URL für die Anwendung muss es sich nicht um einen realen Endpunkt handeln.
- Geben Sie als **Client-ID** des Dienstprinzipals den `appId`-Wert an.
- Auf dem virtuellen Mastercomputer und den virtuellen Knotencomputern des Kubernetes-Clusters werden die Dienstprinzipal-Anmeldeinformationen in der Datei `/etc/kubernetes/azure.json` gespeichert.
- Wenn Sie den Dienstprinzipal mithilfe des Befehls [az aks create][az-aks-create] automatisch generieren, werden die Dienstprinzipal-Anmeldeinformationen auf dem Computer, auf dem der Befehl ausgeführt wird, in die Datei `~/.azure/aksServicePrincipal.json` geschrieben.
- Beim Löschen eines AKS-Clusters, der mit [az aks create][az-aks-create] erstellt wurde, wird der automatisch erstellte Dienstprinzipal nicht gelöscht.
    - Zum Löschen des Dienstprinzipals rufen Sie zuerst mit [az ad app list][az-ad-app-list] die ID für den Dienstprinzipal ab. Das folgende Beispiel fragt den Cluster mit dem Namen *myAKSCluster* ab und löscht dann die App-ID mit [az ad app delete][az-ad-app-delete]. Ersetzen Sie diese Namen durch Ihre eigenen Werte:

        ```azurecli
        az ad app list --query "[?displayName=='myAKSCluster'].{Name:displayName,Id:appId}" --output table
        az ad app delete --id <appId>
        ```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Active Directory-Dienstprinzipalen finden Sie unter [Anwendungs- und Dienstprinzipalobjekte in Azure Active Directory][service-principal].

<!-- LINKS - internal -->
[aad-service-principal]:../active-directory/develop/app-objects-and-service-principals.md
[acr-intro]: ../container-registry/container-registry-intro.md
[az-ad-sp-create]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[azure-load-balancer-overview]: ../load-balancer/load-balancer-overview.md
[install-azure-cli]: /cli/azure/install-azure-cli
[service-principal]:../active-directory/develop/app-objects-and-service-principals.md
[user-defined-routes]: ../load-balancer/load-balancer-overview.md
[az-ad-app-list]: /cli/azure/ad/app#az-ad-app-list
[az-ad-app-delete]: /cli/azure/ad/app#az-ad-app-delete
[az-aks-create]: /cli/azure/aks#az-aks-create
[rbac-network-contributor]: ../role-based-access-control/built-in-roles.md#network-contributor
[rbac-custom-role]: ../role-based-access-control/custom-roles.md
[rbac-storage-contributor]: ../role-based-access-control/built-in-roles.md#storage-account-contributor
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[aks-to-acr]: ../container-registry/container-registry-auth-aks.md?toc=%2fazure%2faks%2ftoc.json#grant-aks-access-to-acr
