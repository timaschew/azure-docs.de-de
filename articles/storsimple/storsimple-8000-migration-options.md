---
title: Bewerten der Optionen zum Migrieren von Daten aus StorSimple Serie 5000/7000 | Microsoft-Dokumentation
description: Bietet eine Übersicht über die Optionen zum Migrieren von Daten aus StorSimple Serie 5000/7000.
services: storsimple
documentationcenter: NA
author: alkohli
manager: twooley
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/30/2018
ms.author: alkohli
ms.openlocfilehash: 5166e154e1a67cea777933b6bf8757661a9c3ad5
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50242958"
---
# <a name="options-to-migrate-data-from-storsimple-5000-7000-series"></a>Optionen zum Migrieren von Daten aus StorSimple Serie 5000/7000 

> [!IMPORTANT]
> Ab dem 31. Juli 2019 wird die StorSimple-Serie 5000/7000 nicht mehr unterstützt. Es wird empfohlen, dass Kunden der StorSimple-Serie 5000/7000 zu einer der in diesem Dokument beschriebenen Alternativen migrieren.

Im Juli 2019 ist für StorSimple Serie 5000/7000 das [Ende des Supports](https://support.microsoft.com/lifecycle/search?alpha=StorSimple%205000%2F7000%20Series) erreicht. Kunden, die StorSimple Serie 5000/7000 ausführen, können auf andere Erstanbieter-Hybriddienste von Azure aktualisieren. In diesem Artikel werden die verfügbaren Azure-Hybridoptionen zum Migrieren von Daten beschrieben. 

## <a name="migration-options"></a>Migrationsoptionen

Für Kunden, die die StorSimple-Serie 5000/7000 verwenden, sind die folgenden Optionen verfügbar:

- **Azure-Optionen**:

    - **Upgrade auf StorSimple Serie 8000**: Ein Upgrade auf StorSimple Serie 8000 und damit weiterhin Verwendung der StorSimple-Plattform.  Für diesen Upgradepfad müssen Kunden ihre Geräte der Serie 5000/7000 durch Geräte der Serie 8000 ersetzen. Die Daten werden mit dem Migrationstool aus dem Gerät der Serie 5000/7000 migriert. Sobald die Migration erfolgreich abgeschlossen ist, speichern die Geräte der StorSimple-Serie 8000 weiterhin Daten in Azure Blob Storage. 

    Weitere Informationen zum Migrieren von Daten mithilfe von StorSimple Serie 8000 finden Sie unter [Migrieren von Daten von einem Gerät der Serie StorSimple 5000/7000 zu einem Gerät der Serie 8000](storsimple-8000-migrate-from-5000-7000.md).

    - **Migrieren zur Azure-Dateisynchronisierung**: Diese völlig neue Migrationsoption ermöglicht Kunden, die Dateifreigaben ihrer Organisation in Azure Files zu speichern. Diese Dateifreigaben werden dann mithilfe der Azure-Dateisynchronisierung (Azure File Sync, AFS) für den lokalen Zugriff zentralisiert. AFS kann auf einem Windows Server-Host bereitgestellt werden. Die tatsächliche Datenmigration wird dann als Hostkopie oder mit dem Migrationstool ausgeführt.

    Weitere Informationen zum Migrieren von Daten mithilfe der Azure-Dateisynchronisierung finden Sie unter [Migrieren von Daten aus StorSimple Serie 5000/7000 zur Azure-Dateisynchronisierung](https://aka.ms/StorSimpleMigrationAFS).

- **Drittanbieteroptionen**:

    - **Migrieren zu Panzura Freedom NAS**: Kunden von StorSimple 5000-7000 können zu Panzura Freedom NAS migrieren, um ihre Daten in Azure zu speichern. Die Panzura Freedom-Lösung stellt eine NAS-Lösung bereit, die Datencenter, Unternehmen sowie öffentliche und private Clouds umfasst. Die Lösung ermöglicht lokale, Hybrid- und In-Cloud-Datenworkflows für NFS, SMB und mobile Clients. Diese Migration wird von Panzura unterstützt, und können Kunden als ersten Schritt Migrationsunterstützung auf der [Panzura-Website](https://panzura.com/storsimple-migration/) anfordern.

## <a name="migration---frequently-asked-questions"></a>Häufig gestellte Fragen zur Migration

### <a name="q-when-do-the-storsimple-5000-and-7000-series-devices-reach-end-of-service"></a>F: Wenn ist das Dienstende der StorSimple-Geräte der Serie 5000/7000 erreicht? 

A. Das [Dienstende](https://support.microsoft.com/lifecycle/search?alpha=StorSimple%205000%2F7000%20Series) der StorSimple-Serie 5000/7000 ist Juli 2019 erreicht. Das Dienstende bedeutet, dass Microsoft nach Juli 2019 weder für Hardware noch Software dieser Geräte Support leisten kann. Sie sollten jetzt unbedingt beginnen, einen Plan zum Migrieren der Daten von Ihren Geräten zu formulieren.

### <a name="q-what-happens-to-the-data-i-have-stored-in-azure"></a>F: Was geschieht mit den Daten, die ich in Azure gespeichert habe?  

A. Sie können die Daten weiterhin in Azure verwenden, nachdem Sie sie zu einem neueren Dienst migriert haben. 


### <a name="q-what-happens-to-the-data-i-have-stored-locally-on-my-storsimple-device"></a>F: Was geschieht mit den Daten, die ich lokal auf meinem StorSimple-Gerät gespeichert habe? 

A. Die auf dem lokalen Gerät gespeicherten Daten können wie in den Dokumenten für die Migration beschrieben auf den neueren Dienst kopiert werden.

### <a name="q-what-happens-if-i-want-to-keep-my-storsimple-50007000-series-appliance"></a>F: Was geschieht, wenn ich mein Gerät der StorSimple-Serie 5000/7000 behalten möchte? 

A. Die Dienste könnten zwar weiterhin funktionieren, aber Microsoft wird nicht mehr in der Lage sein, Support für Hardware und Software zu leisten. Die Migration wird im Interesse der Geschäftskontinuität dringend empfohlen.

### <a name="q-what-options-are-available-to-migrate-data-from-storsimple-5000-7000-series-devices"></a>F: Welche Optionen sind verfügbar, um Daten von StorSimple-Geräten der Serie 5000/7000 zu migrieren? 

A. Abhängig von ihren Szenarios können Benutzer der StorSimple-Serie 5000/7000 die folgenden Migrationsoptionen nutzen. 

 - **Upgrade auf Serie 8000**: Verwenden Sie diese Option, wenn Sie die StorSimple-Plattform weiterhin nutzen möchten. 
 - **Migrieren zur Azure-Dateisynchronisierung**: Verwenden Sie diese Option, wenn Sie zum nativen Azure-Format wechseln möchten. Sie können die Azure-Dateisynchronisierung zur zentralen Verwaltung von Dateifreigaben verwenden. 

Um hier nicht aufgeführte Migrationsoptionen zu erörtern, kontaktieren Sie den Microsoft-Support.

### <a name="q-is-migration-to-other-storage-solutions-supported"></a>F: Wird die Migration zu anderen Speicherlösungen unterstützt?

A. Ja. Migration zu anderen Speicherlösungen mithilfe der Hostkopie der Daten wird unterstützt.

### <a name="q-is-migration-supported-by-microsoft"></a>F: Wird die Migration von Microsoft unterstützt? 

A. Die Migration von der Serie 5000 oder 7000 wird vollständig unterstützt. Microsoft empfiehlt Ihnen generell, sich vor Beginn der Migration an den Support zu wenden. Die Migration ist derzeit ein unterstützter Vorgang. Wenn Sie beabsichtigen, Daten von Ihrem StorSimple-Gerät der Serie 5000/7000 zu migrieren, [öffnen Sie ein Supportticket](storsimple-8000-contact-microsoft-support.md).

### <a name="q-what-is-the-pricing-model-for-both-the-migration-options"></a>F: Welches Preismodell gilt für die beiden Migrationsoptionen?

A. Die Kosten für die Migration variieren abhängig von der gewählten Option. Die Migration selbst ist zwar kostenlos, aber wenn Sie sich für ein Upgrade auf die StorSimple-Serie 8000 entscheiden, entstehen Kosten für das Hardwaregerät. 

Ebenso können bei Nutzung der Azure-Dateisynchronisierung Abonnementgebühren für den Dienst anfallen. In jedem Fall müssen Kunden auch die laufenden Speicherkosten zahlen. Eine Schätzung können Sie anhand der folgenden Informationen vornehmen: 
- [StorSimple-Preise](https://azure.microsoft.com/pricing/details/storsimple/)  
- [AFS-Preise]( https://azure.microsoft.com/pricing/details/storage/files/)

### <a name="q--how-long-does-it-take-to-complete-a-migration"></a>F:  Wie lange dauert es, bis die Migration abgeschlossen ist?

A. Die Dauer der Datenmigration hängt von der Menge der Daten und der ausgewählten Upgradeoption ab. 

### <a name="q-what-is-the-end-of-support-date-for-storsimple-8000-series"></a>F: An welchem Datum endet der Support für die StorSimple 8000-Serie?

A. Das Datum, an dem der Support für die StorSimple 8000-Serie endet, wird [hier](https://support.microsoft.com/lifecycle/search?alpha=Azure%20StorSimple%208000%20Series) veröffentlicht.


## <a name="next-steps"></a>Nächste Schritte
 - [Migrieren von Daten von einem Gerät der Serie StorSimple 5000/7000 zu einem Gerät der Serie 8000](storsimple-8000-migrate-from-5000-7000.md)
 - [Migrieren von Daten aus StorSimple Serie 5000/7000 zur Azure-Dateisynchronisierung](storsimple-5000-7000-afs-migration.md)
