---
title: Verwenden des Speech C# SDK
titleSuffix: Azure Cognitive Services
description: Der Spracherkennungsdienst ermöglicht Ihnen, mit einer einzigen Anforderung Audio zu empfangen und JSON-Objekte der LUIS-Vorhersage zurückgeben zu lassen. In diesem Artikel laden Sie ein C#-Projekt herunter und verwenden es in Visual Studio, um eine Äußerung in ein Mikrofon zu sprechen und von LUIS Vorhersageinformationen zu empfangen.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/07/2018
ms.author: diberry
ms.openlocfilehash: 1bc3b9e016bed59f6453c26371cce7bd089568aa
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53162630"
---
# <a name="integrate-speech-service"></a>Integrieren des Spracherkennungsdiensts
Der [Spracherkennungsdienst](https://docs.microsoft.com/azure/cognitive-services/Speech-Service/) ermöglicht Ihnen, mit einer einzigen Anforderung Audio zu empfangen und JSON-Objekte der LUIS-Vorhersage zurückgeben zu lassen. In diesem Artikel laden Sie ein C#-Projekt herunter und verwenden es in Visual Studio, um eine Äußerung in ein Mikrofon zu sprechen und von LUIS Vorhersageinformationen zu empfangen. Das Projekt verwendet das Spracherkennungspaket [NuGet](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech/), das bereits als Referenz enthalten ist. 

Für diesen Artikel benötigen Sie ein kostenloses [LUIS][LUIS]-Websitekonto, um die Anwendung zu importieren.

## <a name="create-luis-endpoint-key"></a>Erstellen eines LUIS-Endpunktschlüssels
[Erstellen](luis-how-to-azure-subscription.md#create-luis-endpoint-key) Sie im Azure-Portal einen **Sprachverständnis**-Schlüssel (LUIS). 

## <a name="import-human-resources-luis-app"></a>Importieren der LUIS-App „Personalwesen“
Die Absichten und Äußerungen für diesen Artikel stammen aus der LUIS-App für das Personalwesen, die im GitHub-Repository [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples) verfügbar ist. Laden Sie die Datei [HumanResources.json](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/HumanResources.json) herunter, speichern Sie sie mit der Erweiterung `.json`, und [importieren](luis-how-to-start-new-app.md#import-new-app) Sie sie in LUIS. 

Diese App enthält Absichten, Entitäten und Äußerungen aus dem Bereich „Personalwesen“. Hier sehen Sie einige einfache Beispieläußerungen:

|Beispiele für Äußerungen|
|--|
|Who is John W. Smith's manager? (Wer ist der Vorgesetzte von John Smith?)|
|Who does John Smith manage? (Von wem ist John Smith der Vorgesetzte?)|
|Where is Form 123456? (Wo befindet sich Formular 123456?)|
|Do I have any paid time off? (Habe ich irgendwelche bezahlte Freizeit?)|


## <a name="add-keyphrase-prebuilt-entity"></a>Hinzufügen einer vordefinierten Add KeyPhrase-Entität
Nach dem Importieren der App wählen Sie **Entitäten** und dann **Add prebuilt entity** (Vordefinierte Entität hinzufügen) aus. Fügen Sie die **KeyPhrase**-Entität hinzu. Die KeyPhrase-Entität extrahiert wichtige fachliche Inhalte aus der Äußerung.

## <a name="train-and-publish-the-app"></a>Trainieren und Veröffentlichen der App
1. Wählen Sie in der oberen, rechten Navigationsleiste die Schaltfläche **Trainieren** aus, um die LUIS-App zu trainieren.

2. Wählen Sie in der oberen Leiste **Verwalten** und dann im linken Navigationsbereich **Schlüssel und Endpunkte** aus. 

3. Weisen Sie auf der Seite **Schlüssel und Endpunkte** den im Abschnitt [Erstellen eines LUIS-Endpunktschlüssels](#create-luis-endpoint-key) erstellten LUIS-Schlüssel zu.

  Erfassen Sie auf dieser Seite die App-ID, die Veröffentlichungsregion und die Abonnement-ID des im Abschnitt [Erstellen eines LUIS-Endpunktschlüssels](#create-luis-endpoint-key) erstellten LUIS Schlüssels. Sie müssen den Code später in diesem Artikel entsprechend mit diesen Werten ändern. 
  
  Verwenden Sie **nicht** den kostenlosen Starterschlüssel für diese Übung. Nur ein im Azure-Portal erstellter **Sprachverständnis**-Schlüssel funktioniert für diese Übung. 

  https://**REGION**.api.cognitive.microsoft.com/luis/v2.0/apps/**APPID**?subscription-key=**LUISKEY**&q=


4. Veröffentlichen Sie die LUIS-App, indem Sie die Schaltfläche **Veröffentlichen** in der Leiste rechts oben auswählen. 

## <a name="audio-device"></a>Audiogerät
In diesem Artikel wird das Audiogerät Ihres Computers verwendet. Das kann ein Headset mit Mikrofon oder ein eingebautes Audiogerät sein. Überprüfen Sie anhand der Audioeingangspegel, ob Sie lauter sprechen sollten, als Sie es normalerweise tun würden, damit Ihre Sprache vom Audiogerät erkannt wird. 

## <a name="download-the-luis-sample-project"></a>Herunterladen des LUIS-Beispielprojekts
 Klonen Sie das Repository [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples), oder laden Sie es herunter. Öffnen Sie das Projekt [Sprache-Absichts-Umsetzung](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/tutorial-speech-intent-recognition) mit Visual Studio und stellen Sie die NuGet-Pakete wieder her. Die Visual Studio-Projektmappendatei ist .\LUIS-Samples-master\documentation-samples\tutorial-speech-intent-recognition\csharp\csharp_samples.sln.

Das Speech SDK ist bereits als Referenz enthalten. 

[![Screenshot von Visual Studio 2017 mit dem Paket „Microsoft.CognitiveServices.Speech NuGet“](./media/luis-tutorial-speech-to-intent/nuget-package.png "Screenshot von Visual Studio 2017 mit dem Paket „Microsoft.CognitiveServices.Speech NuGet“")](./media/luis-tutorial-speech-to-intent/nuget-package.png#lightbox)

## <a name="modify-the-c-code"></a>Ändern des C#-Codes
Öffnen Sie die Datei `Program.cs`, und ändern Sie die folgenden Variablen:

|Variablenname|Zweck|
|--|--|
|LUIS_assigned_endpoint_key|Entspricht dem zugewiesenen Wert „subscription-key“ der Endpunkt-URL auf der Seite „Veröffentlichen“.|
|LUIS_endpoint_key_region|Entspricht der ersten Unterdomäne der Endpunkt-URL (Beispiel: `westus`).|
|LUIS_app_ID|Entspricht der Route der Endpunkt-URL nach **apps/**|

Der Datei `Program.cs` sind bereits die Absichten aus „Personalwesen“ zugeordnet.

Erstellen Sie die App, und führen Sie sie aus. 

## <a name="test-code-with-utterance"></a>Testen von Code mit Äußerungen
Sprechen Sie in das Mikrofon: „Who are the approved dentists in Redmond?“

[!code-console[Command line response from spoken utterance](~/samples-luis/documentation-samples/tutorial-speech-intent-recognition/console-output.txt "Command line response from spoken utterance")]

Die richtige Absicht (**GetEmployeeBenefits**) wurde mit einer Zuverlässigkeit von 85 Prozent gefunden. Die keyPhrase-Entität wurde zurückgegeben. 

Das Speech SDK gibt die gesamte LUIS-Antwort zurück. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Löschen Sie die LUIS-App „HumanResources“, falls Sie sie nicht mehr benötigen. Wählen Sie hierzu die App aus, und klicken Sie auf der kontextbezogenen Symbolleiste über der Liste auf **Löschen**. Wählen Sie im Popupdialogfenster **Delete App?** (App löschen?) **OK** aus.

Denken Sie daran, das Verzeichnis „LUIS-Samples“ zu löschen, wenn Sie mit der Verwendung des Beispielcodes fertig sind.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Integrieren von LUIS mit einem Bot](luis-csharp-tutorial-build-bot-framework-sample.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website