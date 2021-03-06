---
title: Includedatei
description: Includedatei
services: log-analytics
author: MGoedtel
ms.service: log-analytics
ms.topic: include
ms.date: 05/16/2018
ms.author: magoedte
ms.custom: include file
ms.openlocfilehash: 66cd09df128d454973d008adf4ffc5dd1017a18f
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38755758"
---
Für Log Analytics-Ressourcen gelten pro Abonnement die folgenden Einschränkungen:

| Ressource | Standardlimit | Kommentare
| --- | --- | --- |
| Anzahl von kostenlosen Arbeitsbereichen pro Abonnement | 10 | Dieser Grenzwert kann nicht erhöht werden. |
| Anzahl von kostenpflichtigen Arbeitsbereichen pro Abonnement | N/V | Sie sind durch die Anzahl von Ressourcen innerhalb einer Ressourcengruppe und die Anzahl von Ressourcengruppen pro Abonnement eingeschränkt. | 

>[!NOTE]
>Ab 2. April 2018 verwenden neue Arbeitsbereiche in einem neuen Abonnement automatisch den Tarif *Pro GB*.  Bei Abonnements, die vor dem 2. April erstellt wurden oder mit einer vorhandenen EA-Registrierung verknüpft sind, können Sie für neue Arbeitsbereiche weiterhin zwischen drei Tarifen wählen. 
>

Die folgenden Grenzwerte gelten für jeden Log Analytics-Arbeitsbereich:

|  | Kostenlos | Standard | Premium | Eigenständig | OMS | Pro GB |
| --- | --- | --- | --- | --- | --- |--- |
| Pro Tag erfasstes Datenvolumen |500 MB<sup>1</sup> |Keine |Keine | Keine | Keine | Keine
| Aufbewahrungszeitraum |7 Tage |1 Monat |12 Monate | 1 Monat<sup>2</sup> | 1 Monat<sup>2</sup>| 1 Monat<sup>2</sup>|

<sup>1</sup>Wenn Kunden bei der Datenübertragung das Tageslimit von 500 MB erreichen, wird die Datenanalyse beendet und am nächsten Morgen fortgesetzt. Ein Tag wird in UTC-Zeit ausgegeben.

<sup>2</sup> Der Aufbewahrungszeitraum für die Tarife „Eigenständig“, „OMS“ und „Pro GB“ kann auf 730 Tage erhöht werden.

| Category (Kategorie) | Einschränkungen | Kommentare
| --- | --- | --- |
| Data Collector API (Datensammler-API) | Die maximale Größe für einen einzelnen Beitrag beträgt 30 MB.<br>Die maximale Größe für Feldwerte beträgt 32 KB. | Teilen Sie größere Volumen auf mehrere Beiträge auf.<br>Felder mit einer Länge von mehr als 32 KB werden abgeschnitten. |
| Search-API | 5000 zurückgegebene Datensätze (nicht aggregierte Daten)<br>500.000 Datensätze (aggregierte Daten) | Bei aggregierten Daten handelt es sich um eine Suche, die den Befehl `summarize` enthält.
 
