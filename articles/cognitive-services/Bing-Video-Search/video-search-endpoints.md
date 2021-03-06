---
title: Videosuche-Endpunkte – Bing-Videosuche
titlesuffix: Azure Cognitive Services
description: Überblick über den Endpunkt für die Bing-Videosuche-API.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: conceptual
ms.date: 12/04/2017
ms.author: rosh
ms.openlocfilehash: c153f577f76944d22f9a1b0fb4b24d332d2a02c8
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47220770"
---
# <a name="video-search-endpoints"></a>Videosuche-Endpunkte
Die **Videosuche-API**  umfasst drei Endpunkte.  Endpunkt 1 gibt Videos aus dem Internet basierend auf einer Abfrage zurück. Endpunkt 2 gibt Einblicke über Videos auf Basis des `modules`-URL-Parameters aus.  Endpunkt 3 gibt beliebte Videos zurück.

## <a name="endpoints"></a>Endpunkte
Um mithilfe der Bing-API Videoergebnisse zu erhalten, senden Sie eine `GET`-Anforderung an einen der folgenden Endpunkte. Verwenden Sie die Header und die URL-Parameter, um Spezifikationen genauer zu definieren.

**Endpunkt 1:** gibt Videos zurück, die für die von `?q=""` definierte Suchabfrage des Benutzers relevant sind.
``` 
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search
```

**Endpunkt 2:** gibt Einblicke in ein Video zurück, wie z.B. verwandte Videos. Integrieren Sie den `modules` [-Abfrageparameter](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#query-parameters), der eine durch Komma getrennte Liste der anzufordernden Einblicke ist.
``` 
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/details
```

**Endpunkt 3:** gibt Videos zurück, die auf der Grundlage von Suchanfragen anderer beliebt sind. Die Videos sind in verschiedene Kategorien eingeteilt, z.B. nach Personen oder Ereignissen.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/trending
```

Weitere Informationen zu Headern, Parametern, Marktcodes, Antwortobjekten, Fehlern usw. finden Sie in der Referenz [API für die Bing-Videosuche v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference).
## <a name="response-json"></a>JSON-Antwort
Die Antwort auf eine Videosuchanforderung enthält die Ergebnisse als JSON-Objekten. Beispiele für die Analyse der Ergebnisse finden Sie unter [Tutorial](https://docs.microsoft.com/azure/cognitive-services/bing-video-search/tutorial-bing-video-search-single-page-app) und [Quellcode](https://docs.microsoft.com/azure/cognitive-services/bing-video-search/tutorial-bing-video-search-single-page-app-source).

## <a name="next-steps"></a>Nächste Schritte
Die **Bing**-APIs unterstützen Suchaktionen, die Ergebnisse gemäß ihrem Typ zurückgeben. Alle Suchendpunkte geben Ergebnisse als JSON-Antwortobjekte zurück.  Alle Endpunkte unterstützen Abfragen, die eine bestimmte Sprache und/oder einen bestimmten Ort nach Längengrad, Breitengrad und Suchradius zurückgeben.

Die umfassenden Informationen zu den Parametern, die von jedem Endpunkt unterstützt werden, finden Sie auf den Referenzseiten des jeweiligen Typs.
Beispiele für grundlegende Anforderung mithilfe der Videosuche-API finden Sie in den [Videosuche-Schnellstarts](https://docs.microsoft.com/azure/cognitive-services/bing-video-search).