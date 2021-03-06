---
title: Wechseln zu einem B2C-Mandanten in Azure Active Directory B2C | Microsoft-Dokumentation
description: Wechseln zum Kontext des Active Directory B2C-Mandanten
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 4/13/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 8c0b6cb411618b01adfc23fa124ff624206da7b2
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52836099"
---
# <a name="switching-to-your-azure-ad-b2c-tenant"></a>Wechseln zum Azure AD B2C-Mandanten

Damit Sie Azure AD B2C konfigurieren können, müssen Sie sich im Kontext des Azure AD B2C-Mandanten befinden.

## <a name="log-into-azure-ad-b2c-tenant"></a>Anmelden bei einem Azure AD-B2C-Mandanten

Damit Sie zu Ihrem Azure AD B2C-Mandanten navigieren können, müssen Sie beim Azure-Portal als globaler Administrator des Azure AD B2C-Mandanten angemeldet sein.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Klicken Sie auf Ihre E-Mail-Adresse oder das Bild in der oberen rechten Ecke, um Mandanten zu wechseln.
1. Wählen Sie in der daraufhin angezeigten `Directory`-Liste den Azure AD B2C-Mandanten aus, den Sie verwalten möchten.

Das Azure-Portal wird aktualisiert.  Nun sind Sie beim Azure-Portal im Kontext Ihres Azure AD B2C-Mandanten angemeldet.

## <a name="navigate-to-the-b2c-features-pane"></a>Navigieren zum Bereich „B2C-Funktionen“

1. Klicken Sie im linken Navigationsbereich auf **Durchsuchen**.
1. Klicken Sie im linken Navigationsbereich auf **Alle Dienste**, und suchen Sie nach `Azure AD B2C`.  (Klicken Sie zum Anheften an das linke Startmenü auf das Sternchen links neben Azure AD B2C).
1. Klicken Sie auf **Azure AD B2C**, um auf den Bereich „B2C-Funktionen“ zuzugreifen.
   
    ![Screenshot der Navigation zum Bereich „B2C-Funktionen“](./media/active-directory-b2c-get-started/b2c-browse.png)

> [!IMPORTANT]
> Sie müssen ein globaler Administrator des B2C-Mandanten sein, um auf den Bereich mit den B2C-Features zugreifen zu können. Globale Administratoren anderer Mandanten oder Benutzer von Mandanten haben keinen Zugriff.  Mit dem Mandantenumschalter oben rechts im Azure-Portals können Sie zu Ihrem B2C-Mandanten wechseln.
