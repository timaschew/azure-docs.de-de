---
title: Durchblättern der verfügbaren Bilder – Bing-Bildersuche-API
titleSuffix: Azure Cognitive Services
description: Hier wird gezeigt, wie Sie alle Bilder durchblättern, die Bing zurückgeben kann.
services: cognitive-services
author: swhite-msft
manager: cgonlun
ms.assetid: 3C8423F8-41E0-4F89-86B6-697E840610A7
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: conceptual
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 0db8c62bbb4da1a6fa1230b439c5074325bbe9c3
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2018
ms.locfileid: "50739365"
---
# <a name="paging-results"></a>Auslagerungsergebnisse

Wenn Sie die Bildersuche-API aufrufen, gibt Bing eine Liste mit Ergebnissen zurück. Bei der Liste handelt es sich um eine Teilmenge der gesamten Ergebnisse, die für die Abfrage relevant sind. Um die geschätzte Gesamtzahl der verfügbaren Ergebnisse aufzurufen, greifen Sie auf das Feld [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#totalestimatedmatches) des Antwortobjekts zu.  

Das folgende Beispiel zeigt das `totalEstimatedMatches`-Feld mit einer Bilderantwort.  

```json
{
    "_type" : "Images",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=73118C8...",
    "totalEstimatedMatches" : 838,
    "value" : [...]  
}  
```  

Wenn Sie die verfügbaren Bilder durchblättern möchten, verwenden Sie die Abfrageparameter [count](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#count) und [offset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#offset).  

Der `count`-Parameter gibt die Anzahl der Ergebnisse an, die in der Antwort zurückgegeben werden sollen. Die maximale Anzahl an Ergebnissen, die Sie in der Antwort anfordern können, ist 150. Der Standardwert ist 35. Die tatsächlich gelieferte Anzahl kann geringer sein als angefordert.

Der `offset`-Parameter gibt die Anzahl der zu überspringenden Ergebnisse an. Der `offset`-Parameter ist nullbasiert und sollte kleiner sein als (`totalEstimatedMatches` - `count`).  

Wenn Sie pro Seite 20 Bilder anzeigen möchten, setzen Sie `count` auf 20 und `offset` auf 0, um die erste Ergebnisseite anzuzeigen. Unten sehen Sie ein Beispiel, bei dem 20 Bilder ab Offset 40 angefordert werden.  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Wenn der `count`-Standardwert für Ihre Implementierung das gewünschte Ergebnis erzielt, müssen Sie nur den `offset`-Abfrageparameter angeben.  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Sie gehen vielleicht davon aus, dass Sie, wenn Sie 35 Bilder gleichzeitig durchblättern, den Abfrageparameter `offset` bei Ihrer ersten Anforderung auf 0 setzen und `offset` dann bei jeder weiteren Anfragen um 35 erhöhen würden. Einige der Ergebnisse in der folgenden Antwort können jedoch Duplikate der vorherigen Antwort sein. Beispielsweise können die ersten beiden Bilder in der Antwort die gleichen sein wie die letzten beiden Bilder der vorherigen Antwort.

Um doppelte Ergebnisse zu eliminieren, verwenden Sie das [nextOffset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#nextoffset)-Feld des `Images`-Objekts. Das `nextOffset`-Feld gibt an, welchen `offset` Sie für die nächste Anforderung verwenden sollen. Wenn Sie beispielsweise 30 Bilder gleichzeitig durchblättern möchten, setzen Sie `count` in Ihrer ersten Anforderung auf 30 und `offset` auf 0. In Ihrer nächsten Anforderung legen Sie `count` auf 30 und `offset` auf den Wert von `nextOffset` aus der vorherigen Antwort fest. Für die Rückwärtspaginierung wird empfohlen, einen Stapel mit den vorherigen Offsets beizubehalten und die und letzten per Pop zu entfernen.

> [!NOTE]
> Das Durchblättern gilt nur für die Bildsuche (/images/search) und nicht für Bildeinblicke oder beliebte Bilder (/images/trending).

> [!NOTE]
> Das Feld `TotalEstimatedAnswers` gibt eine Schätzung der Gesamtzahl der Suchergebnisse an, die Sie für die aktuelle Abfrage abrufen können.  Wenn Sie die Parameter `count` und `offset` festlegen, kann sich der Wert von `TotalEstimatedAnswers` ändern. 
