---
title: Verbindung mit Azure-VMs kann nicht hergestellt werden, da der RDP-Port in der NSG nicht aktiviert ist | Microsoft-Dokumentation
description: Beschreibung der Behandlung eines Problems, bei dem für RDP aufgrund der NSG-Konfiguration im Azure-Portal ein Fehler auftritt | Microsoft-Dokumentation
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: cshepard
editor: v-jesits
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/20/2018
ms.author: genli
ms.openlocfilehash: cb9058d4f68b2dc202edeeaa6cafb2eefa82470b
ms.sourcegitcommit: 022cf0f3f6a227e09ea1120b09a7f4638c78b3e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2018
ms.locfileid: "52284726"
---
#  <a name="cannot-connect-remotely-to-a-vm-because-rdp-port-is-not-enabled-in-nsg"></a>Remoteverbindung mit einer VM kann nicht hergestellt werden, da der RDP-Port in der NSG nicht aktiviert ist

In diesem Artikel wird die Lösung eines Problems beschrieben, bei dem Sie keine Verbindung mit einem virtuellen Azure-Computer (VM) unter Windows herstellen können, weil der RDP-Port (Remotedesktopprotokoll) in der Netzwerksicherheitsgruppe (NSG) nicht aktiviert ist.


> [!NOTE] 
> Azure verfügt über zwei Bereitstellungsmodelle zum Erstellen und Verwenden von Ressourcen: [Resource Manager-Modell und klassisches Modell](../../azure-resource-manager/resource-manager-deployment-model.md). Wir empfehlen Ihnen, für neue Bereitstellungen anstelle des klassischen Bereitstellungsmodells das Resource Manager-Bereitstellungsmodell zu verwenden. 

## <a name="symptom"></a>Symptom

Sie können keine RDP-Verbindung mit einer VM in Azure herstellen, weil der RDP-Port in der Netzwerksicherheitsgruppe nicht geöffnet ist.

## <a name="solution"></a>Lösung 

Wenn Sie einen neuen virtuellen Computer erstellen, wird der gesamte Datenverkehr aus dem Internet standardmäßig blockiert. 

Führen Sie diese Schritte aus, um den RDP-Port in einer NSG zu aktivieren:
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie unter **Virtuelle Computer** die VM aus, für die das Problem besteht. 
3. Wählen Sie unter **Einstellungen** die Option **Netzwerk**. 
4. Überprüfen Sie unter **Regeln für eingehende Ports**, ob der Port für RDP richtig festgelegt ist. Hier ist ein Beispiel für die Konfiguration angegeben: 

    **Priorität**: 300 </br>
    **Port**: 3389 </br>
    **Name**: Port_3389 </br>
    **Port**: 3389 </br>
    **Protokoll**: TCP </br>
    **Quelle**: Beliebig </br>
    **Ziele**:Beliebig </br>
    **Aktion**: Zulassen </br>

Wenn Sie die IP-Quelladresse angeben, wird mit dieser Einstellung nur Datenverkehr von einer bestimmten IP-Adresse bzw. einem IP-Adressbereich zugelassen, um die Verbindung mit der VM herzustellen. Stellen Sie sicher, dass sich der Computer, den Sie zum Starten der RDP-Sitzung verwenden, in diesem Bereich befindet.

Weitere Informationen zu Netzwerksicherheitsgruppen finden Sie unter [Netzwerksicherheitsgruppe](../../virtual-network/security-overview.md).

> [!NOTE]
> RDP-Port 3389 wird für das Internet verfügbar gemacht. Daher empfehlen wir Ihnen, diesen Port nur für Testzwecke zu nutzen. Für Produktionsumgebungen empfehlen wir die Verwendung einer VPN- oder privaten Verbindung.

## <a name="next-steps"></a>Nächste Schritte

Falls der RDP-Port in der Netzwerksicherheitsgruppe bereits aktiviert ist, helfen Ihnen die Informationen unter [Beheben eines allgemeinen RDP-Fehlers auf einer Azure-VM](./troubleshoot-rdp-general-error.md) weiter.



