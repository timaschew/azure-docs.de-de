---
title: Allgemeine Fragen zu Azure Service Fabric Mesh | Microsoft-Dokumentation
description: Informationen zu häufig gestellten Fragen und Antworten zu Azure Service Fabric-Mesh.
services: service-fabric-mesh
keywords: ''
author: chackdan
ms.author: chackdan
ms.date: 06/25/2018
ms.topic: troubleshooting
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: f80f61cbfc1f7b719e73d7d29c6948bebe84aa6c
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2018
ms.locfileid: "51278309"
---
# <a name="commonly-asked-service-fabric-mesh-questions"></a>Häufig gestellte Fragen zu Service Fabric Mesh
Azure Service Fabric Mesh ist ein vollständig verwalteter Dienst, der es Entwicklern ermöglicht, Microservicesanwendungen zu implementieren, ohne virtuelle Computer, Speicher oder Netzwerke verwalten zu müssen. Dieser Artikel bietet Antworten auf häufig gestellte Fragen.

## <a name="how-do-i-report-an-issue-or-ask-a-question"></a>Wie melde ich ein Problem oder stelle ich eine Frage?

Im [GitHub-Repository service-fabric-mesh-preview](https://aka.ms/sfmeshissues) können Sie Fragen stellen, Antworten von Microsoft-Entwicklern erhalten und Probleme melden.

## <a name="quota-and-cost"></a>Kontingent und Kosten

**Wie hoch sind die Kosten für die Teilnahme an der Vorschauversion?**

Es fallen derzeit keine Kosten für die Bereitstellung von Anwendungen oder Containern in der Vorschauversion von Mesh an. Sie werden jedoch gebeten, die von Ihnen bereitgestellten Ressourcen zu löschen und nicht weiter auszuführen, es sei denn, Sie testen sie aktiv.

**Gibt es eine Kontingentgrenze für die Anzahl der Kerne und den Arbeitsspeicher?**

Ja, die Kontingente für jedes Abonnement sind wie folgt festgelegt:

- Anzahl von Anwendungen: 5 
- Kerne pro Anwendung: 12 
- RAM insgesamt pro Anwendung: 48 GB 
- Netzwerk- und Eingangsendpunkte: 5  
- Anfügbare Azure-Volumes: 10 
- Anzahl der Dienstreplikate: 3 
- Der größte Container, den Sie bereitstellen können, ist auf 4 Kerne und 16 GB RAM beschränkt.
- Sie können Ihren Containern Teilkerne in Schritten von 0,5 Kernen bis maximal 6 Kernen zuweisen.

**Wie lange kann ich meine Anwendung bereitgestellt lassen?**

Wir haben die Lebensdauer einer Anwendung zurzeit auf zwei Tage beschränkt. Dadurch soll die Verwendung der für die Vorschau reservierten freien Kerne maximiert werden. Daher dürfen Sie eine bestimmten Bereitstellung nur für 48 Stunden durchgehend ausführen. Nach Ablauf dieser Zeit wird sie durch das System heruntergefahren. Wenn dies geschieht, können Sie bestätigen, dass das System sie heruntergefahren hat, indem Sie in der Azure-Befehlszeilenschnittstelle den Befehl `az mesh app show` ausführen und überprüfen, ob er `"status": "Failed", "statusDetails": "Stopped resource due to max lifetime policies for an application during preview. Delete the resource to continue."` zurückgibt. 

Beispiel:  

```cli
chackdan@Azure:~$ az mesh app show --resource-group myResourceGroup --name helloWorldApp
{
  "debugParams": null,
  "description": "Service Fabric Mesh HelloWorld Application!",
  "diagnostics": null,
  "healthState": "Ok",
  "id": "/subscriptions/1134234-b756-4979-84re-09d671c0c345/resourcegroups/myResourceGroup/providers/Microsoft.ServiceFabricMesh/applications/helloWorldApp",
  "location": "eastus",
  "name": "helloWorldApp",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "serviceNames": [
    "helloWorldService"
  ],
  "services": null,
  "status": "Failed",
  "statusDetails": "Stopped resource due to max lifetime policies for an application during preview. Delete the resource to continue.",
  "tags": {},
  "type": "Microsoft.ServiceFabricMesh/applications",
  "unhealthyEvaluation": null
}
```

Um dieselbe Anwendung in Mesh weiterhin bereitzustellen, sollten Sie die der Anwendung zugeordnete Ressourcengruppe löschen oder die Anwendung und alle zugehörigen Mesh-Ressourcen (einschließlich des Netzwerks) einzeln entfernen. 

Um die Ressourcengruppe zu löschen, verwenden Sie den Befehl `az group delete <nameOfResourceGroup>`. 

## <a name="supported-container-os-images"></a>Unterstützte Images von Containerbetriebssystemen
Die folgenden Images von Containerbetriebssystemen können beim Bereitstellen von Diensten verwendet werden.

- Windows: windowsservercore und nanoserver
    - Windows Server 2016
    - Windows Server, Version 1709
- Linux
    - Keine bekannten Einschränkungen

## <a name="features-gaps-and-known-issues"></a>Funktionslücken und bekannte Probleme

**Nach der Bereitstellung meiner Anwendung scheint die damit verbundene Netzwerkressource keine IP-Adresse zu haben**

Es gibt derzeit ein bekanntes Problem mit der IP-Adresse, das nach einer Verzögerung auftritt. Überprüfen Sie den Status der Netzwerkressource nach wenigen Minuten. Die zugehörige IP-Adresse wird dann angezeigt.

**Meine Anwendung kann nicht auf die richtige Netzwerk-/Volumeressource zugreifen**

In Ihrem Anwendungsmodell müssen Sie die vollständige Ressourcen-ID für Netzwerke und Volumes verwenden, um auf die zugehörige Ressource zugreifen zu können. So sieht dies im Schnellstartbeispiel aus:

```json
"networkRefs": [
    {
    "name":  "[resourceId('Microsoft.ServiceFabric/networks', 'SbzVotingNetwork')]" 
    }
]
```

**Ich kann nicht erkennen, wie das gegenwärtige Anwendungsmodell meine Geheimnisse verschlüsseln kann**

Die Verschlüsselung von Geheimnissen wird in der aktuellen privaten Vorschau nicht unterstützt. 

**DNS funktioniert in meinem Service Fabric-Entwicklungscluster und in Mesh nicht gleich**

Es gibt ein bekanntes Problem, aufgrund dessen Sie auf Dienste in Ihrem lokalen Entwicklungscluster und in Azure Mesh ggf. anders verweisen müssen. Verwenden Sie in Ihrem lokalen Entwicklungscluster {Dienstname}.{Anwendungsname}. Verwenden Sie in Azure Service Fabric Mesh {Dienstname}. Azure Mesh unterstützt derzeit nicht die anwendungsübergreifende DNS-Auflösung.

Informationen zu anderen bekannten DNS-Problemen bei der Ausführung eines Service Fabric-Entwicklungsclusters unter Windows 10 finden Sie hier: [Debuggen von Windows-Containern](/azure/service-fabric/service-fabric-how-to-debug-windows-containers).

**Beim Verwenden des CLI-Moduls erhalte ich diese Fehlermeldung: _ImportError: cannot import name 'sdk_no_wait'**

Wenn Sie eine ältere CLI-Version als 2.0.30 verwenden, können Sie diese Fehlermeldung erhalten.

```
cannot import name 'sdk_no_wait'
Traceback (most recent call last):
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\knack\cli.py", line 193, in invoke cmd_result = self.invocation.execute(args)
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core\commands_init_.py", line 241, in execute self.commands_loader.load_arguments(command)
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core_init_.py", line 201, in load_arguments self.command_table[command].load_arguments() # this loads the arguments via reflection
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core\commands_init_.py", line 142, in load_arguments super(AzCliCommand, self).load_arguments()
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\knack\commands.py", line 76, in load_arguments cmd_args = self.arguments_loader()
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core_init_.py", line 332, in default_arguments_loader op = handler or self.get_op_handler(operation)
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core_init_.py", line 375, in get_op_handler op = import_module(mod_to_import)
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\importlib_init_.py", line 126, in import_module return _bootstrap._gcd_import(name[level:], package, level)
File "", line 978, in _gcd_import
File "", line 961, in _find_and_load
File "", line 950, in _find_and_load_unlocked
File "", line 655, in _load_unlocked
File "", line 678, in exec_module
File "", line 205, in _call_with_frames_removed
File "C:\Users\annayak.azure\cliextensions\azure-cli-sbz\azext_sbz\custom.py", line 18, in 
from azure.cli.core.util import get_file_json, shell_safe_json_parse, sdk_no_wait
ImportError: cannot import name 'sdk_no_wait'.
```

**Ich erhalte einen Fehler beim Installieren des CLI-Erweiterungspakets aufgrund eines nicht übereinstimmenden Distributionsnamens**

Dies bedeutet nicht, dass die Erweiterung nicht installiert wurde. Sie sollten die CLI-Befehle weiterhin problemlos verwenden können.

**Wenn ich horizontal hochskalieren, sehe ich, dass alle meine Container betroffen sind, einschließlich derjenigen, die gerade ausgeführt werden**

Dies ist ein Programmfehler, der mit der nächsten Aktualisierung behoben werden sollte.

## <a name="next-steps"></a>Nächste Schritte

In der [Übersicht](service-fabric-mesh-overview.md) erfahren Sie mehr über Service Fabric Mesh.
