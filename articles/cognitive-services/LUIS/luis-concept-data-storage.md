---
title: Datenspeicher
titleSuffix: Language Understanding - Azure Cognitive Services
description: LUIS speichert Daten verschlüsselt in einem Azure-Datenspeicher für die im Schlüssel angegebene Region.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: df8af9dc86e26a073b76bff33f35bad2ba32b5e8
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53105749"
---
# <a name="data-storage-and-removal-in-language-understanding-luis-cognitive-services"></a>Speicherung und Entfernung von Daten in Language Understanding (LUIS) Cognitive Services
LUIS speichert Daten verschlüsselt in einem Azure-Datenspeicher für die im Schlüssel angegebene Region. Diese Daten werden für 30 Tage gespeichert. 

## <a name="export-and-delete-app"></a>Exportieren und Löschen von Apps
Benutzer haben volle Kontrolle über das [Exportieren](luis-how-to-start-new-app.md#export-app) und [Löschen](luis-how-to-start-new-app.md#delete-app) ihrer Apps. 

## <a name="utterances-in-an-intent"></a>Äußerungen in Absichten
Löschen Sie die Beispieläußerungen für das Training von [LUIS](luis-reference-regions.md). Wenn Sie eine Beispieläußerung aus Ihrer LUIS-App löschen, wird sie vom LUIS-Webdienst entfernt und ist nicht für den Export verfügbar.

## <a name="utterances-in-review"></a>Äußerungen in Überprüfungen
Sie können Äußerungen aus der Liste der Benutzeräußerungen, die LUIS auf der Seite **[Review endpoint utterance](luis-how-to-review-endoint-utt.md)** (Endpunktäußerungen überprüfen) vorschlägt, löschen. Das Löschen von Äußerungen aus dieser Liste verhindert, dass diese empfohlen werden. Sie werden aber nicht aus den Protokollen gelöscht.

## <a name="accounts"></a>Konten
Wenn Sie ein Konto löschen, werden alle Apps zusammen mit ihren Beispieläußerungen und Protokollen gelöscht. Die Daten werden für 60 Tage beibehalten, bevor das Konto und die Daten dauerhaft gelöscht werden.

Das Löschen von Konten erfolgt über die Seite **Einstellungen**. Wählen Sie Ihren Kontonamen rechts oben auf der Navigationsleiste aus, um die Seite **Einstellungen** anzuzeigen.

## <a name="data-inactivity-as-an-expired-subscription"></a>Dateninaktivität als Zeichen für ein abgelaufenes Abonnement
Im Zusammenhang mit der Beibehaltung und Löschung von Daten kann eine inaktive LUIS-App nach dem _Ermessen von Microsoft_ als abgelaufenes Abonnement behandelt werden. Eine App gilt als inaktiv, wenn sie für die letzten 90 Tage die folgenden Kriterien erfüllt: 

* Es wurden **keine** Aufrufe getätigt.
* Sie wurde nicht geändert.
* Ihr ist zurzeit kein aktueller Schlüssel zugewiesen.
* Es hat sich kein Benutzer bei ihr angemeldet.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erfahren Sie mehr zum Exportieren und Löschen von Apps.](luis-how-to-start-new-app.md)