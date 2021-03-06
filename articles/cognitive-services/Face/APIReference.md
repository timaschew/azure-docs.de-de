---
title: 'API-Referenz: Gesichtserkennungs-API'
titleSuffix: Azure Cognitive Services
description: Die API-Referenz bietet Informationen zu den APIs „Person Management“, „LargePersonGroup/PersonGroup Management“, „LargeFaceList/FaceList Management“ und „Face Algorithms“.
services: cognitive-services
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: reference
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 283a042bb8ea32f8f6db9bebb211bc2762a3a4c1
ms.sourcegitcommit: 0fc99ab4fbc6922064fc27d64161be6072896b21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2018
ms.locfileid: "51578051"
---
# <a name="api-reference"></a>API-Referenz

Die Microsoft-Gesichtserkennungs-API ist eine cloudbasierte API, die die fortschrittlichsten Algorithmen zur Gesichtserkennung und -wiedererkennung bietet.

Gesichtserkennungs-APIs decken folgende Kategorien ab:

- [Face Algorithm APIs](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236): Decken grundlegende Funktionen wie [Detection](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) (Erkennung), [Find Similar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) (Suchen ähnlicher Elemente), [Verification](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a) (Überprüfung), [Identification](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) (Identifikation) und [Group](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238) (Gruppe) ab.
- [FaceList Management APIs](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b): Werden zum Verwalten eines FaceList-Elements für [Find Similar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) (Suchen ähnlicher Elemente) verwendet.
- [LargePersonGroup Person Management APIs](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40): Werden zur Verwaltung von LargePersonGroup Person Faces-Elementen für [Identification](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) (Identifikation) verwendet.
- [LargePersonGroup Management APIs](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d): Werden zur Verwaltung von LargePersonGroup-Datasets für [Identification](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) (Identifikation) verwendet.
- [LargeFaceList Management APIs](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc): Werden zum Verwalten eines LargeFaceList-Elements für [Find Similar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) (Suchen ähnlicher Elemente) verwendet.
- [PersonGroup Person Management APIs](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c): Werden zur Verwaltung von PersonGroup Person Faces-Elementen für [Identification](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) (Identifikation) verwendet.
- [PersonGroup Management APIs](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244): Werden zur Verwaltung von PersonGroup-Datasets für [Identification](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) (Identifikation) verwendet.

<!-- Linguist question: Please confirm that the following are API names and should be left as is: "Person Management, LargePersonGroup/PersonGroup Management, LargeFaceList/FaceList Management, and Face Algorithms" -->
