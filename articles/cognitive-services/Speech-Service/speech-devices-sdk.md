---
title: Informationen zum Speech-Geräte-SDK
titleSuffix: Azure Cognitive Services
description: Hier erhalten Sie eine Einführung in das Speech-Geräte-SDK.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: erhopf
ms.openlocfilehash: eac3542059f1bc5d32a91ef871e5185fad1d2798
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49464096"
---
# <a name="about-the-speech-devices-sdk-preview"></a>Informationen zum Speech-Geräte-SDK (Vorschau)

Der [Speech-Dienst](overview.md) kann mit verschiedensten Geräten und Audioquellen verwendet werden. Jetzt können Sie einen Schritt weiter gehen und Ihre Sprachanwendungen mit angepasster Hardware und Software nutzen. Beim Speech-Geräte-SDK handelt es sich um eine vorab optimierte Bibliothek, die mit spezifischen mikrofonfähigen Development Kits kombiniert wurde. 

Anwendungsmöglichkeiten für das Speech-Geräte-SDK:
* Testen Sie neue Sprachszenarien schnell.
* Integrieren Sie den cloudbasierten Speech-Dienst einfacher auf Ihrem Gerät.
* Sorgen Sie für eine herausragende Benutzerfreundlichkeit für Ihre Kunden. 

Das Speech-Geräte-SDK nutzt das [Speech-SDK](speech-sdk.md). Es verwendet das Speech-SDK zum Senden von Audioinhalten, die mit unserem Audioverarbeitungsalgorithmus bearbeitet wurden, vom Mikrofonarray des Geräts an den [Speech-Dienst](overview.md). Es verwendet Mehrkanal-Audio für eine präzisere Fernfeld-[Spracherkennung](speech-to-text.md) mit Geräuschunterdrückung, Echounterdrückung, Beamforming und Hallunterdrückung.

Mit dem Speech-Geräte-SDK können Sie auch Ambient Devices mit einem [benutzerdefinierten Aktivierungswort](speech-devices-sdk-create-kws.md) erstellen, sodass der Auslöser zum Initiieren einer Benutzerinteraktion in Bezug auf Ihre Marke einmalig ist. 

Mit dem Speech-Geräte-SDK lassen sich verschiedenste stimmaktivierte Szenarien entwickeln, z.B. Drive-through-Bestellsysteme, Assistenzsysteme für Geschäfte oder im Haushalt und intelligente Lautsprecher. Es ist möglich, Benutzern mit Textausgaben oder mit Sprachausgaben in einer Standard- oder [benutzerdefinierten Stimme](how-to-customize-voice-font.md) zu antworten, Suchergebnisse bereitzustellen, [Übersetzungen](speech-translation.md) in andere Sprachen durchzuführen und vieles mehr. Wir freuen uns darauf, zu sehen, was Sie erstellen!

## <a name="development-kit-providers"></a>Anbieter von Development Kits

Derzeit stehen diese umfassenden Referenzentwürfe für End-to-End-Systeme zur Verfügung: 

|||
|-|-|
|[![ROOBO-Logo](media/speech-devices-sdk/roobo-logo.png)](http://ddk.roobo.com/)|ROOBO bietet vollständige Lösungen für KI-Systeme (künstliche Intelligenz) für elektrische Haushaltsgeräte, Fahrzeuge, Roboter, Spielzeug und andere Branchen. Über die Integration in den Microsoft Speech-Dienst verkürzen die Referenzentwürfe von ROOBO die Time-to-Market erheblich. [Besuchen Sie ROOBO](http://ddk.roobo.com/).|

## <a name="next-steps"></a>Nächste Schritte

Richten Sie zunächst ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/ai/) ein, und registrieren Sie sich für das Speech-Geräte-SDK.

> [!div class="nextstepaction"]
> [Registrieren für das Speech-Geräte-SDK](get-speech-devices-sdk.md)

