---
title: 'Schnellstart: Erstellen einer Abfrage für die visuelle Suche, Python – Visuelle Bing-Suche'
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie ein Bild in die API für die visuelle Bing-Suche hochladen und dadurch Erkenntnisse zu diesem Bild erhalten.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: quickstart
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 3a0d92e42eed097e244118a60ec0a4223c9cedf5
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52440940"
---
# <a name="quickstart-your-first-bing-visual-search-query-in-python"></a>Schnellstart: Ihre erste Abfrage für die visuelle Bing-Suche in Python

Die API für die visuelle Bing-Suche gibt Informationen zu einem von Ihnen bereitgestellten Bild zurück. Sie können ein Bild über die Bild-URL, ein Erkenntnistoken oder durch Hochladen des Bilds bereitstellen. Informationen zu diesen Optionen finden Sie unter [Was ist die API für die visuelle Bing-Suche?](../overview.md). In diesem Artikel wird gezeigt, wie Sie ein Bild hochladen. Das Hochladen eines Bilds ist besonders in Szenarien mit einem mobilen Gerät nützlich, wenn Sie eine bekannte Sehenswürdigkeit fotografiert haben und Informationen dazu erhalten möchten. Die Informationen können z.B. Wissenswertes zur Sehenswürdigkeit beinhalten. 

Wenn Sie ein lokales Bild hochladen, müssen Sie die folgenden Formulardaten in den Text der POST-Anforderung einfügen. Die Formulardaten müssen den Header „Content-Disposition“ enthalten. Der `name`-Parameter muss auf „image“ festgelegt werden. Der `filename`-Parameter kann auf eine beliebige Zeichenfolge festgelegt werden. Der Inhalt des Formulars sind die Binärdaten des Bilds. Sie können eine maximale Bildgröße von 1 MB hochladen. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Dieser Artikel zeigt eine einfache Konsolenanwendung, die eine Anforderung an die API für die visuelle Bing-Suche sendet und die Suchergebnisse im JSON-Format anzeigt. Die Anwendung ist zwar in Python geschrieben, an sich ist die API aber ein RESTful-Webdienst, der mit jeder Programmiersprache kompatibel ist, die HTTP-Anforderungen senden und JSON analysieren kann. 

## <a name="prerequisites"></a>Voraussetzungen

Um diesen Code auszuführen, benötigen Sie [Python 3](https://www.python.org/).

Für diese Schnellstartanleitung benötigen Sie ein Abonnement im Tarif „S9“ wie unter [Cognitive Services-Preise – Bing-Suche-API](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/search-api/) gezeigt. 

So erstellen Sie ein Abonnement im Azure-Portal:
1. Geben Sie oben im Azure-Portal im Suchfeld `Search resources, services, and docs` den Suchbegriff „BingSearchV7“ ein.  
2. Wählen Sie unter „Marketplace“ in der Dropdownliste `Bing Search v7` aus.
3. Geben Sie `Name` für die neue Ressource ein.
4. Wählen Sie das Abonnement `Pay-As-You-Go` aus.
5. Wählen Sie den Tarif `S9` aus.
6. Klicken Sie auf `Enable`, um das Abonnement zu erstellen.

## <a name="running-the-walkthrough"></a>Ausführen der exemplarischen Vorgehensweise

Führen Sie die folgenden Schritte aus, um diese Anwendung auszuführen:

1. Erstellen Sie in Ihrer bevorzugten IDE oder Ihrem bevorzugten Editor ein neues Python-Projekt.
2. Erstellen Sie eine Datei namens „visualsearch.py“, und fügen Sie den in dieser Schnellstartanleitung gezeigten Code hinzu.
3. Ersetzen Sie den `SUBSCRIPTION_KEY`-Wert durch Ihren Abonnementschlüssel.
3. Ersetzen Sie den `imagePath`-Wert durch den Pfad des Bilds, das Sie hochladen möchten.
4. Führen Sie das Programm aus.



Das folgende Beispiel zeigt, wie Sie eine Nachricht mit mehrteiligen Formulardaten in Python senden.

```python
"""Bing Visual Search upload image example"""

# Download and install Python at https://www.python.org/
# Run the following in a command console window
# pip3 install requests

import requests, json


BASE_URI = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch'

SUBSCRIPTION_KEY = '<yoursubscriptionkeygoeshere>'

HEADERS = {'Ocp-Apim-Subscription-Key': SUBSCRIPTION_KEY}

imagePath = '<pathtoyourimagetouploadgoeshere>'

file = {'image' : ('myfile', open(imagePath, 'rb'))}

def main():
    
    try:
        response = requests.post(BASE_URI, headers=HEADERS, files=file)
        response.raise_for_status()
        print_json(response.json())

    except Exception as ex:
        raise ex


def print_json(obj):
    """Print the object as json"""
    print(json.dumps(obj, sort_keys=True, indent=2, separators=(',', ': ')))



# Main execution
if __name__ == '__main__':
    main()
```


## <a name="next-steps"></a>Nächste Schritte

[Abrufen von Informationen zu einem Bild mithilfe eines Erkenntnistokens](../use-insights-token.md)  
[Tutorial zum Bildupload für die visuelle Bing-Suche](../tutorial-visual-search-image-upload.md)
[Tutorial zu einer Single-Page-App für die visuelle Bing-Suche](../tutorial-bing-visual-search-single-page-app.md)  
[Übersicht über die visuelle Bing-Suche](../overview.md)  
[Testen](https://aka.ms/bingvisualsearchtryforfree)  
[Abrufen eines Zugriffsschlüssels für eine kostenlose Testversion](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Referenz zur API für die visuelle Bing-Suche](https://aka.ms/bingvisualsearchreferencedoc)
