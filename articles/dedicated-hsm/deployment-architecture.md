---
title: Azure Dedicated HSM-Bereitstellungsarchitektur | Microsoft-Dokumentation
description: Azure Dedicated HSM bietet Schlüsselspeicherfunktionen innerhalb von Azure, die die Anforderungen der Zertifizierung „FIPS 140-2 Level 3“ erfüllen.
services: dedicated-hsm
author: barclayn
manager: mbaldwin
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/19/2018
ms.author: barclayn
ms.openlocfilehash: 8481f444464135a24d42612f334ac6096cba7b21
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2018
ms.locfileid: "52318989"
---
# <a name="deployment-architecture"></a>Bereitstellungsarchitektur

Azure Dedicated HSM bietet Speicherung von kryptografischen Schlüsseln in Azure. Azure Dedicated HSM erfüllt strenge Sicherheitsanforderungen. Kunden profitieren von der Verwendung von Azure Dedicated HSM, wenn sie:

* Die FIPS 140-2 Level 3-Zertifizierung erfüllen müssen.
* Exklusiven Zugriff auf das HSM benötigen.
* Vollständige Kontrolle über ihre Geräte haben müssen.

Die HSMs sind auf die Datencenter von Microsoft verteilt und können problemlos als Gerätepaar als Basis für eine hochverfügbare Lösung bereitgestellt werden. Sie können auch regionsübergreifend bereitgestellt werden, um eine notfallresistente Lösung zu bieten. Die Regionen, in denen Dedicated HSM zurzeit verfügbar ist, sind:

* USA (Ost)
* USA (Ost) 2
* USA (Westen)
* USA Süd Mitte
* Südostasien
* Asien, Osten
* Nordeuropa
* Europa, Westen

Jede dieser Regionen verfügt über HSM-Racks, die entweder in zwei unabhängigen Datencentern oder in mindestens zwei unabhängigen Verfügbarkeitszonen bereitgestellt werden. „Asien, Südosten“ verfügt über drei Verfügbarkeitszonen und „USA, Osten 2“ über zwei. Es gibt insgesamt acht Regionen in Europa, Asien und den USA, die den Dedicated HSM-Dienst zur Verfügung stellen. Weitere Informationen zu Azure-Regionen finden Sie in den offiziellen [Informationen zu Azure-Regionen](https://azure.microsoft.com/global-infrastructure/regions/).
Einige Entwurfsfaktoren für jede Dedicated HSM-basierte Lösung sind Standort/Latenz, Hochverfügbarkeit und Unterstützung für andere verteilte Anwendungen.

## <a name="device-location"></a>Gerätestandort

Der optimale Standort der HSM-Geräte befindet sich in unmittelbarer Nähe zu den Anwendungen, die kryptographische Vorgänge ausführen. Die Latenzzeit innerhalb der Region sollte im einstelligen Millisekundenbereich liegen. Regionsübergreifende Latenz kann 5 bis 10 Mal höher als dieser Wert sein.

## <a name="high-availability"></a>Hochverfügbarkeit

Um Hochverfügbarkeit zu erreichen, muss ein Kunde zwei HSM-Geräte in einer Region verwenden, die mit der Gemalto-Software als Hochverfügbarkeitspaar konfiguriert werden. Diese Art der Bereitstellung stellt die Verfügbarkeit von Schlüsseln sicher, wenn ein einzelnes Gerät auf ein Problem stößt, das es daran hindert, wichtige Vorgänge zu verarbeiten. Sie verringert auch das Risiko bei der Durchführung von Wartungsarbeiten (z.B. dem Austausch der Stromversorgung) erheblich. Es ist wichtig, dass ein Entwurf jede Art von Versagen auf regionaler Ebene berücksichtigt. Ausfälle auf regionaler Ebene können bei Naturkatastrophen wie Orkanen, Überschwemmungen oder Erdbeben auftreten. Diese Art von Ereignissen sollte durch die Bereitstellung von HSM-Geräten in einer anderen Region abgemildert werden. Geräte, die in einer anderen Region bereitgestellt werden, können über die Gemalto-Softwarekonfiguration gekoppelt werden. Dies bedeutet, dass die Mindestbereitstellung für eine hochverfügbare und notfallresistente Lösung aus vier HSM-Geräten in zwei Regionen besteht. Lokale Redundanz und Redundanz über Regionen hinweg können als Grundlage für weitere HSM-Gerätebereitstellungen verwendet werden, um Latenzzeiten und Kapazitäten zu unterstützen oder andere anwendungsspezifische Anforderungen zu erfüllen.

## <a name="distributed-application-support"></a>Unterstützung verteilter Anwendungen

Dedicated HSM-Geräte werden typischerweise zur Unterstützung von Anwendungen bereitgestellt, die Schlüsselspeicher- und Schlüsselabrufvorgänge durchführen müssen. Dedicated HSM-Geräte verfügen über 10 Partitionen für die Unterstützung unabhängiger Anwendungen. Der Gerätestandort sollte auf einer ganzheitlichen Sichtweise aller Anwendungen basieren, die den Dienst nutzen müssen.

## <a name="next-steps"></a>Nächste Schritte

Sobald die Bereitstellungsarchitektur festgelegt wurde, werden die meisten Konfigurationsaktivitäten zur Implementierung dieser Architektur von Gemalto bereitgestellt. Dazu gehören sowohl die Gerätekonfiguration als auch Anwendungsintegrationsszenarien. Weitere Informationen finden Sie im [Gemalto-Kundensupportportal ](https://supportportal.gemalto.com/csm/). Sie können außerdem die Administrations- und Konfigurationsleitfäden herunterladen. Die Microsoft-Partnerwebsite verfügt über eine Vielzahl von Integrationsleitfäden.
Es wird empfohlen, sich mit allen Schlüsselkonzepten des Diensts (z.B. Hochverfügbarkeit und Sicherheit) vor der Gerätebereitstellung oder dem Entwurf und der Bereitstellung von Anwendungen vertraut zu machen.
Weitere Themen auf Konzeptebene:

* [Hochverfügbarkeit](high-availability.md)
* [Physische Sicherheit](physical-security.md)
* [Netzwerk](networking.md)
* [Unterstützungsmöglichkeiten](supportability.md)
* [Überwachung](monitoring.md)
