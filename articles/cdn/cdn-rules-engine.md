---
title: Überschreiben des HTTP-Verhaltens mithilfe der Azure CDN-Regel-Engine | Microsoft-Dokumentation
description: Mit der Regel-Engine können Sie anpassen, wie HTTP-Anforderungen von Azure CDN behandelt werden, z.B. Sperren der Übermittlung bestimmter Inhaltstypen, Definieren einer Zwischenspeicherungsrichtlinie oder Ändern von HTTP-Headern.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: 625a912b-91f2-485d-8991-128cc194ee71
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: magattus
ms.openlocfilehash: 2ac43b472758f3403bc87bf3d64321eb97109f53
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2018
ms.locfileid: "49092393"
---
# <a name="override-http-behavior-using-the-azure-cdn-rules-engine"></a>Überschreiben des HTTP-Verhaltens mithilfe der Azure CDN-Regel-Engine
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Übersicht
Über die Azure CDN-Regel-Engine können Sie anpassen, wie HTTP-Anforderungen verarbeitet werden. Hierzu zählt z.B. die Blockierung der Übermittlung bestimmter Inhaltstypen, die Definition einer Cacherichtlinie oder die Änderung eines HTTP-Headers. In diesem Tutorial wird die Erstellung einer Regel erläutert, die das Zwischenspeicherungsverhalten von CDN-Objekten ändert. Weitere Informationen zur Regel-Engine finden Sie unter [Referenz für die Azure CDN-Regel-Engine](cdn-rules-engine-reference.md).

## <a name="access"></a>Access
Um auf die Regel-Engine zuzugreifen, müssen Sie für den Zugriff auf die Azure CDN-Verwaltungsseite zuerst **Verwalten** im oberen Bereich der Seite **CDN-Profil** auswählen. Je nachdem, ob der Endpunkt für die Beschleunigung dynamischer Websites (Dynamic Site Acceleration, DSA) optimiert ist, greifen Sie mit der Gruppe von Regeln, die für den Typ des Endpunkts geeignet sind, dann auf die Regel-Engine zu:

- Endpunkte, die für die allgemeine Webbereitstellung oder andere Optimierungsmethoden außer DSA optimiert sind: 
    
    Wählen Sie die Registerkarte **HTTP Large** und dann **Regel-Engine** aus.

    ![Regel-Engine für HTTP](./media/cdn-rules-engine/cdn-http-rules-engine.png)

- Für die DSA optimierte Endpunkte: 
    
    Wählen Sie die Registerkarte **ADN** und dann **Regel-Engine** aus. 
    
    ADN ist eine von Verizon verwendete Benennung für DSA-Inhalte. Die hier von Ihnen erstellten Regeln werden von Endpunkten in Ihrem Profil ignoriert, die nicht für die DSA optimiert sind. 

    ![Regel-Engine für die DSA](./media/cdn-rules-engine/cdn-dsa-rules-engine.png)

## <a name="tutorial"></a>Tutorial
1. Klicken Sie auf der Seite **CDN-Profil** auf **Verwalten**.
   
    ![Schaltfläche „Verwalten“ für CDN-Profile](./media/cdn-rules-engine/cdn-manage-btn.png)
   
    Das CDN-Verwaltungsportal wird geöffnet.

2. Wählen Sie die Registerkarte **HTTP Large** und dann **Regel-Engine** aus.
   
    Die Optionen für eine neue Regel werden angezeigt.
   
    ![Optionen für neue CDN-Regeln](./media/cdn-rules-engine/cdn-new-rule.png)
   
   > [!IMPORTANT]
   > Die Reihenfolge, in der mehrere Regeln aufgelistet sind, beeinflusst deren Verarbeitung. Eine Regel kann die von einer vorherigen Regel angegebenen Aktionen überschreiben.
   > 

3. Geben Sie einen Namen in das Textfeld **Name/Beschreibung** ein.

4. Ermitteln Sie die Anforderungstypen, auf die die Regel angewendet wird. Verwenden Sie die Standardübereinstimmungsbedingung **Immer**. 
   
   ![Übereinstimmungsbedingung für CDN-Regeln](./media/cdn-rules-engine/cdn-request-type.png)
   
   > [!NOTE]
   > In der Dropdownliste sind mehrere Übereinstimmungsbedingungen verfügbar. Um Informationen zur aktuell ausgewählten Übereinstimmungsbedingung zu erhalten, klicken Sie auf das blaue Informationssymbol auf der linken Seite.
   > 
   >  Eine ausführliche Liste von bedingten Ausdrücken finden Sie unter [Bedingte Ausdrücke der Regel-Engine](cdn-rules-engine-reference-match-conditions.md).
   >  
   > Eine ausführliche Liste von Übereinstimmungsbedingungen finden Sie unter [Übereinstimmungsbedingungen der Regel-Engine](cdn-rules-engine-reference-match-conditions.md).
   > 
   > 

5. Um ein neues Feature hinzuzufügen, klicken Sie auf die Schaltfläche **+** neben **Features**.  Wählen Sie in der Dropdownliste auf der linken Seite **Force Internal Max-Age**aus.  Geben Sie **300**in das angezeigte Textfeld ein. Ändern Sie nicht die übrigen Standardwerte.
   
   ![Features der CDN-Regel-Engine](./media/cdn-rules-engine/cdn-new-feature.png)
   
   > [!NOTE]
   > In der Dropdownliste sind mehrere Features verfügbar. Um Informationen zum aktuell ausgewählten Feature zu erhalten, klicken Sie auf das blaue Informationssymbol auf der linken Seite. 
   >
   > Bei **Force Internal Max-Age** werden die `Cache-Control`- und `Expires`-Header des Medienobjekts überschrieben, um zu steuern, wann der CDN-Edgeknoten das Objekt vom Ursprung aktualisiert. In diesem Beispiel speichert der CDN-Edgeknoten das Objekt 300 Sekunden bzw. 5 Minuten lang zwischen, bevor er das Objekt von dessen Ursprung aktualisiert.
   > 
   > Eine detaillierte Liste der Features finden Sie unter [Features der Azure CDN-Regel-Engine](cdn-rules-engine-reference-features.md).
   > 
   > 

6. Klicken Sie auf **Hinzufügen**, um die neue Regel zu speichern.  Die neue Regel wartet jetzt auf ihre Genehmigung. Nachdem sie genehmigt wurde, wird der Status von **Pending XML** in **Active XML** geändert.
   
   > [!IMPORTANT]
   > Es kann bis zu zehn Minuten dauern, bis Regeländerungen über das Azure CDN verteilt wurden.
   > 
   > 

## <a name="see-also"></a>Weitere Informationen
* [Übersicht über das Azure Content Delivery Network](cdn-overview.md)
* [Regel-Engine – Referenz](cdn-rules-engine-reference.md)
* [Übereinstimmungsbedingungen der Regel-Engine](cdn-rules-engine-reference-match-conditions.md)
* [Regel-Engine – bedingte Ausdrücke](cdn-rules-engine-reference-conditional-expressions.md)
* [Regel-Engine – Features](cdn-rules-engine-reference-features.md)
* [Azure Friday: Azure CDN's powerful new Premium Features](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (Die leistungsstarken neuen Premium-Features des Azure CDN) (Video)