---
title: Verwenden von Lastenausgleichs-Integritätstests zum Schützen Ihres Diensts | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Integritätstests verwenden, um Instanzen hinter einem Lastenausgleichsmodul zu überwachen.
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/04/2018
ms.author: kumud
ms.openlocfilehash: 58bc0c0669992b8b3884e24c39862f47412b9110
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2018
ms.locfileid: "52584654"
---
# <a name="load-balancer-health-probes"></a>Lastenausgleichs-Integritätstests

Azure Load Balancer nutzt Integritätstests, um zu bestimmen, welche Back-End-Poolinstanz neue Datenflüsse erhält. Anhand von Integritätstests können Sie einen Fehler in einer Anwendung auf einer Back-End-Instanz erkennen. Sie können auch eine benutzerdefinierte Antwort auf einen Integritätstest generieren und für die Ablaufsteuerung verwenden sowie, um dem Lastenausgleich zu signalisieren, ob weiterhin neue Flows gesendet oder keine weiteren neuen Flows mehr an eine Back-End-Instanz gesendet werden sollen. Dies kann verwendet werden, um die Last oder geplante Downtime zu verwalten. Wenn ein einem Integritätstest ein Fehler auftritt, beendet der Load Balancer das Senden neuer Flows an die entsprechende fehlerhafte Instanz.

Die verfügbaren Typen von Integritätstests und die Art und Weise, wie sich die Integritätstests verhalten, hängen von der SKU des Load Balancers ab, die Sie verwenden. Das Verhalten neuer und vorhandener Flows hängt z.B. davon ab, ob es sich um einen TCP- oder UDP-Flow handelt und welche Load Balancer-SKU Sie verwenden.

| | Standard-SKU | Basic-SKU |
| --- | --- | --- |
| [Testtypen](#types) | TCP, HTTP, HTTPS | TCP, HTTP |
| [Verhalten bei Tests mit Fehlern](#probedown) | Alle Tests mit Fehlern, alle TCP-Flows werden fortgesetzt. | Alle Tests mit Fehlern, alle TCP-Flows werden beendet. | 

> [!IMPORTANT]
> Lastenausgleichs-Integritätstests stammen von der IP-Adresse 168.63.129.16 und dürfen nicht blockiert werden, damit Ihre Instanz beim Test als online markiert wird.  Einzelheiten finden Sie unter [Quell-IP-Adresse von Tests](#probesource).

## <a name="types"></a>Testtypen

Bei Integritätstests kann ein beliebiger Port an einer Back-End-Instanz, einschließlich des Ports, an dem der eigentliche Dienst bereitgestellt wird, überwacht werden. Das Integritätstestprotokoll kann für drei verschiedene Typen von Integritätstests konfiguriert werden:

- [TCP-Listener](#tcpprobe)
- [HTTP-Endpunkte](#httpprobe)
- [HTTPS-Endpunkte](#httpsprobe)

Die verfügbaren Typen von Integritätstests sind abhängig von der ausgewählten Load Balancer-SKU unterschiedlich:

|| TCP | HTTP | HTTPS |
| --- | --- | --- | --- |
| Standard-SKU |    &#9989; |   &#9989; |   &#9989; |
| Basic-SKU |   &#9989; |   &#9989; | &#10060; |

Bei einem UDP-Lastenausgleich sollten Sie ein benutzerdefiniertes Integritätstestsignal für die Back-End-Instanz mithilfe eines TCP-, HTTP- oder HTTPS-Integritätstests generieren.

Bei Verwendung von [Lastenausgleichsregeln für Hochverfügbarkeitsports](load-balancer-ha-ports-overview.md) mit [Load Balancer Standard](load-balancer-standard-overview.md) wird für alle Ports ein Lastenausgleich ausgeführt, sodass eine Antwort vom Integritätstest den Status der gesamten Instanz widerspiegeln muss.  

Sie sollten weder NAT noch einen Proxy für einen Integritätstest der Instanz, die den Integritätstest empfängt, zu einer anderen Instanz in Ihrem VNET verwenden, da dies zu kaskadierenden Fehlern in Ihrem Szenario führen kann.

Wenn Sie einen Integritätstestfehler überprüfen oder eine einzelne Instanz als offline kennzeichnen möchten, können Sie eine Sicherheitsgruppe verwenden, um den Integritätstest (Ziel oder [Quelle](#probesource)) zu blockieren.

### <a name="tcpprobe"></a> TCP-Test

TCP-Tests leiten eine Verbindung über einen offenen Drei-Wege-TCP-Handshake mit dem definierten Port ein.  Darauf folgt dann ein geschlossener 4-Wege-TCP-Handshake.

Das Mindestintervall für Tests beträgt 5 Sekunden, und die Mindestanzahl von fehlerhaften Antworten ist 2.  Die gesamte Dauer darf 120 Sekunden nicht überschreiten.

TCP-Tests führen in folgenden Fällen zu Fehlern:
* Der TCP-Listener für die Instanz reagiert innerhalb des Zeitlimits gar nicht.  Wann der Test als nicht ausgeführt markiert wird, hängt von der konfigurierten Anzahl unbeantworteter fehlerhafter Testanforderungen vor dem Markieren des Tests als nicht ausgeführt ab.
* Der Test empfängt ein TCP-Reset von der Instanz.

#### <a name="resource-manager-template"></a>Resource Manager-Vorlage

```json
    {
      "name": "tcp",
      "properties": {
        "protocol": "Tcp",
        "port": 1234,
        "intervalInSeconds": 5,
        "numberOfProbes": 2
      },
```

### <a name="httpprobe"></a> <a name="httpsprobe"></a> HTTP-/HTTPS-Test

> [!NOTE]
> Der HTTPS-Test ist nur für [Load Balancer Standard](load-balancer-standard-overview.md) verfügbar.

HTTP- und HTTPS-Tests stellen eine TCP-Verbindung her und geben eine HTTP GET-Anforderung mit dem angegebenen Pfad aus. Diese beiden Tests unterstützen für HTTP GET relative Pfade. HTTPS-Tests sind mit HTTP-Tests identisch, weisen jedoch zusätzlich einen Transport Layer Security-Wrapper (TLS, früher als SSL bezeichnet) auf. Der Integritätstest kennzeichnet die Instanz als online, wenn diese innerhalb des Zeitlimits mit dem HTTP-Statuscode 200 antwortet.  Bei diesen Integritätstests wird standardmäßig versucht, den konfigurierten Integritätstestport alle 15 Sekunden zu prüfen. Das minimale Testintervall beträgt 5 Sekunden. Die gesamte Dauer darf 120 Sekunden nicht überschreiten. 

HTTP-/HTTPS-Tests können auch für die Implementierung Ihrer eigenen Logik zum Entfernen von Instanzen aus der Lastenausgleichrotation nützlich sein. Sie können z.B. eine Instanz entfernen, wenn sie über 90 % CPU beansprucht und einen anderen HTTP-Status als 200 zurückgibt. 

Wenn Sie Cloud Services verwenden und über Webrollen verfügen, die „w3wp.exe“ verwenden, erreichen Sie auch eine automatische Überwachung Ihrer Website. Fehler in Ihrem Websitecode geben einen anderen Status als 200 an den Lastenausgleichstest zurück.  Der HTTP-Test setzt den Gast-Agent-Standardtest außer Kraft. 

HTTP-/HTTPS-Tests führen in folgenden Fällen zu Fehlern:
* Der Testendpunkt gibt einen anderen HTTP-Antwortcode als 200 zurück (z.B. 403, 404 oder 500). Dadurch wird der Integritätstest sofort als offline gekennzeichnet. 
* Der Testendpunkt reagiert während des Zeitlimits von 31 Sekunden gar nicht. Je nach dem festgelegten Timeoutwert können mehrere Testanforderungen unbeantwortet bleiben, bevor der Test als nicht ausgeführt markiert wird (d. h. bevor SuccessFailCount-Tests gesendet werden).
* Der Testendpunkt schließt die Verbindung über ein TCP-Reset.

#### <a name="resource-manager-templates"></a>Resource Manager-Vorlagen

```json
    {
      "name": "http",
      "properties": {
        "protocol": "Http",
        "port": 80,
        "requestPath": "/",
        "intervalInSeconds": 5,
        "numberOfProbes": 2
      },
```

```json
    {
      "name": "https",
      "properties": {
        "protocol": "Https",
        "port": 443,
        "requestPath": "/",
        "intervalInSeconds": 5,
        "numberOfProbes": 2
      },
```

### <a name="guestagent"></a>Gast-Agent-Test (nur klassisch)

Clouddienstrollen (Workerrollen und Webrollen) verwenden standardmäßig einen Gast-Agent für die Testüberwachung.   Sie sollten dies als eine letzte Option erwägen.  Sie sollten einen Integritätstest immer explizit mit einem TCP- oder HTTP-Test definieren. Ein Gast-Agent-Test ist bei den meisten Anwendungsszenarien nicht so effektiv wie explizit definierte Tests.  

Ein Gast-Agent-Test ist eine Überprüfung des Gast-Agents auf dem virtuellen Computer. Er lauscht dann und antwortet nur mit einer HTTP-OK-200-Antwort, wenn sich die Instanz im Zustand „Bereit“ befindet. (Andere Zustände sind „Beschäftigt“, „Wird wiederverwendet“ oder „Wird beendet“.)

Weitere Informationen finden Sie unter [Konfigurieren der Dienstdefinitionsdatei (CSDEF) für Integritätstests](https://msdn.microsoft.com/library/azure/ee758710.aspx) oder [Erste Schritte durch Erstellen eines öffentlichen Lastenausgleichs für Clouddienste](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

Wenn der Gast-Agent nicht mit dem HTTP-OK-Code 200 antwortet, kennzeichnet der Lastenausgleich die Instanz als nicht reagierend. Er sendet dann keine Flows mehr an diese Instanz. Das Lastenausgleichsmodul überprüft die Instanz weiterhin. 

Wenn der Gast-Agent mit dem HTTP-Code 200 antwortet, sendet das Lastenausgleichsmodul wieder Flows an diese Instanz.

Wenn Sie eine Webrolle verwenden, wird der Websitecode in der Regel in „w3wp.exe“ ausgeführt. Dieses Programm wird nicht von der Azure-Fabric oder vom Gast-Agent überwacht. Fehler in „w3wp.exe“ (z. B. HTTP 500-Antworten) werden dem Gast-Agent nicht gemeldet. Folglich nimmt der Lastenausgleich diese Instanz nicht aus der Rotation.

## <a name="probehealth"></a>Testen der Integrität

TCP-, HTTP- und HTTPS-Integritätstests werden in den folgenden Fällen als fehlerfrei eingestuft und markieren die Rolleninstanz als fehlerfrei:

* Der Integritätstest ist beim ersten Mal, wenn der virtuelle Computer gestartet wird, erfolgreich.
* Der Wert für „SuccessFailCount“ (oben beschrieben) definiert die Anzahl erfolgreicher Tests, die erforderlich sind, um die Rolleninstanz als fehlerfrei zu markieren. Wenn eine Rolleninstanz entfernt wurde, muss die Anzahl der erfolgreichen, aufeinanderfolgenden Tests gleich oder größer sein als der Wert von SuccessFailCount, damit die Rolleninstanz als aktiv markiert wird.

> [!NOTE]
> Wenn die Integrität einer Rolleninstanz schwankt, wartet der Lastenausgleich länger, ehe die Rolleninstanz wieder in den fehlerfreien Zustand versetzt wird. Diese zusätzliche Wartezeit schützt den Benutzer und die Infrastruktur und ist eine bewusste Richtlinie.

## <a name="probe-count-and-timeout"></a>Anzahl und Timeout von Tests

Das Verhalten von Tests hängt von folgenden Faktoren ab:

* Anzahl der erfolgreichen Tests, bei der eine Instanz als online gekennzeichnet werden kann
* Anzahl der fehlerhaften Tests, bei der eine Instanz als offline gekennzeichnet wird

Die in „SuccessFailCount“ festgelegten Werte für Timeout und Häufigkeit legen fest, ob eine Instanz ausgeführt oder nicht ausgeführt wird. Im Azure-Portal wird das Timeout auf das Doppelte des Werts für die Häufigkeit festgelegt.

Für eine Lastenausgleichsregel wird durch einen Integritätstest der entsprechende Back-End-Pool definiert.

## <a name="probedown"></a>Verhalten bei Tests mit Fehlern

### <a name="tcp-connections"></a>TCP-Verbindungen

Neue TCP-Verbindungen mit der Back-End-Instanz erfolgen ohne Fehler, wenn diese fehlerfrei ist und über ein Gastbetriebssystem und eine Anwendung verfügt, um einen neuen Flow zu akzeptieren.

Wenn beim Integritätstest einer Back-End-Instanz ein Fehler auftritt, bleiben die für diese Back-End-Instanz eingerichteten TCP-Verbindungen bestehen.

Wenn bei allen Überprüfungen für sämtliche Instanzen in einem Back-End-Pool ein Fehler auftritt, werden keine neue Flows an den Back-End-Pool gesendet. Load Balancer Standard erlaubt aber die Fortführung der eingerichteten TCP-Flows.  Load Balancer Basic beendet alle vorhandenen TCP-Flows an den Back-End-Pool.
 
Da der Flow immer zwischen dem Client und dem Gastbetriebssystem der VM abläuft, führt ein Pool, bei dem alle Tests zu Fehlern führen, dazu, dass das Front-End nicht auf Versuche zum Öffnen von TCP-Verbindungen reagiert, da keine fehlerfreien Back-End-Instanzen zum Empfangen des Flows vorhanden sind.

### <a name="udp-datagrams"></a>UDP-Datagramme

UDP-Datagramme werden an fehlerfreie Back-End-Instanzen übermittelt.

Das UDP ist verbindungslos und es werden kein Flusszustände für das UDP nachverfolgt. Wenn beim Integritätstest einer beliebigen Back-End-Instanz ein Fehler auftritt, können vorhandene UDP-Flows an eine andere fehlerfreie Instanz im Back-End-Pool umgeleitet werden.

Wenn bei allen Tests für einen Back-End-Pool Fehler auftreten, werden alle vorhandenen UDP-Flows für Load Balancer Basic und Standard beendet.

## <a name="probesource"></a>Quell-IP-Adresse von Tests

Load Balancer verwendet einen verteilten Dienst für die Stichprobenentnahme für sein internes Integritätsmodell. Jeder Host mit VMs kann so programmiert werden, dass Integritätstests gemäß Kundenkonfiguration generiert werden. Der Datenverkehr im Rahmen von Integritätstests findet direkt zwischen der Infrastrukturkomponente, auf der der Integritätstest generiert wird, und der Kunden-VM statt. Bei allen Lastenausgleichs-Integritätstests lautet die Quell-IP-Adresse 168.63.129.16.  Wenn Sie Ihre eigenen IP-Adressen in Azure Virtual Network verwenden, ist gewährleistet, dass diese Quell-IP-Adresse für Integritätstests eindeutig ist, da sie global für Microsoft reserviert ist.  Diese Adresse ist in allen Regionen identisch und wird nicht geändert. Dies dürfte kein Sicherheitsrisiko darstellen, da nur die interne Azure-Plattform ein Paket von dieser IP-Adresse senden kann. 

Zusätzlich zu Load Balancer-Integritätstests wird diese IP-Adresse für die folgenden Vorgänge verwendet:

- Ermöglichen der Kommunikation zwischen VM-Agent und Plattform, um zu signalisieren, dass sich der VM-Agent im Zustand „Bereit“ befindet.
- Ermöglichen der Kommunikation mit dem virtuellen DNS-Server, um für die Kunden eine gefilterte Namensauflösung bereitzustellen, die keine benutzerdefinierten DNS-Server definieren.  Diese Filterung stellt sicher, dass Kunden nur die Hostnamen ihrer Bereitstellung auflösen können.

Damit der Lastenausgleichs-Integritätstest Ihre Instanz als online kennzeichnen kann, **müssen** Sie diese IP-Adresse in allen Azure-[Sicherheitsgruppen](../virtual-network/security-overview.md) und lokalen Firewallrichtlinien zulassen.

Wenn Sie diese IP-Adresse in Ihren Firewallrichtlinien nicht zulassen, tritt ein Fehler beim Integritätstest auf, da er Ihre Instanz nicht erreichen kann.  Daraufhin kennzeichnet das Lastenausgleichsmodul Ihre Instanz als offline, da beim Integritätstest ein Fehler aufgetreten ist.  Dadurch kann Ihr Lastenausgleichdienst ausfallen. 

Außerdem sollten Sie Ihr VNET nicht mit dem für Microsoft reservierten IP-Adressbereich mit 168.63.129.16 konfigurieren.  Dies führt zu einem Konflikt mit der IP-Adresse für den Integritätstest.

Wenn Sie über mehrere Schnittstellen auf dem virtuellen Computer verfügen, müssen Sie sicherstellen, dass auf den Test über die Schnittstelle geantwortet wird, über die er empfangen wurde.  Dies erfordert möglicherweise eine eindeutige Netzwerkadressenübersetzung für die Quelle dieser Adresse auf dem virtuellen Computer auf Schnittstellenebene.

## <a name="monitoring"></a>Überwachung

Öffentliche und interne [Load Balancer Standard](load-balancer-standard-overview.md) stellen pro Endpunkt und Back-End-Instanz den Integritätsteststatus als mehrdimensionale Metriken über Azure Monitor bereit. Diese können dann von anderen Azure-Diensten oder Anwendungen von Drittanbietern genutzt werden. 

Der öffentliche Load Balancer Basic stellt den Integritätsteststatus zusammengefasst pro Back-End-Pool über Log Analytics bereit.  Dies steht für interne Load Balancer Basic nicht zur Verfügung.  Mit [Log Analytics](load-balancer-monitor-log.md) können Sie den Testintegritätsstatus und die Testanzahl für den öffentlichen Lastenausgleich überprüfen. Die Protokollierung kann mit Power BI oder Azure Operational Insights verwendet werden, um Statistiken zum Integritätsstatus des Lastenausgleichs bereitzustellen.

## <a name="limitations"></a>Einschränkungen

-  HTTPS-Tests bieten keine Unterstützung für die gegenseitige Authentifizierung mit einem Clientzertifikat.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum [Load Balancer Standard](load-balancer-standard-overview.md).
- [Schnellstart: Erstellen eines öffentlichen Lastenausgleichs im Ressourcen-Manager mit PowerShell](load-balancer-get-started-internet-arm-ps.md)
- [REST-API für Integritätstests](https://docs.microsoft.com/rest/api/load-balancer/loadbalancerprobes/)
- Anfordern neuer Integritätstestfunktionen mit [Uservoice von Load Balancer](https://aka.ms/lbuservoice)
