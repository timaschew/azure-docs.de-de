---
title: Schnellstart zum Erstellen eines neuen Mandanten und zum Zugriff auf ihn – Azure Active Directory | Microsoft-Dokumentation
description: Anweisungen zum Finden von Azure Active Directory und zum Erstellen eines neuen Mandanten für Ihre Organisation.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: lizross
ms.custom: it-pro, seodec18
ms.openlocfilehash: cd179d371ec75a2e6ca258a1efbc2361e50a5438
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53083266"
---
# <a name="quickstart-create-a-new-tenant-in-azure-active-directory"></a>Schnellstart: Erstellen eines neuen Mandanten in Azure Active Directory
Sie können alle Verwaltungsaufgaben über das Azure Active Directory-Portal (Azure AD-Portal) ausführen. Dazu gehört auch das Erstellen eines neuen Mandanten für Ihre Organisation. 

In diesem Schnellstart erfahren Sie, wie Sie das Azure-Portal und Azure Active Directory aufrufen, und wie Sie einen Basismandanten für Ihre Organisation erstellen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.
Melden Sie sich mit dem Konto eines globalen Administrators im [Azure-Portal](https://portal.azure.com/) Ihrer Organisation an.

![Screenshot des Azure-Portals](media/active-directory-access-create-new-tenant/azure-ad-portal.png)

## <a name="create-a-new-tenant-for-your-organization"></a>Erstellen eines neuen Mandanten für Ihre Organisation
Nach der Anmeldung im Azure-Portal können Sie einen neuen Mandanten für Ihre Organisation erstellen. Ihr neuer Mandant stellt Ihre Organisation dar und unterstützt Sie dabei, eine bestimmte Microsoft Cloud Services-Instanz für Ihre internen und externen Benutzer zu verwalten.

### <a name="to-create-a-new-tenant"></a>Erstellen eines neuen Mandanten
1. Wählen Sie **Azure Active Directory**, **Ressourcen erstellen**, **Identität** und dann **Azure Active Directory** aus.

    Daraufhin wird die Seite **Verzeichnis erstellen** angezeigt.

    ![Seite zum Erstellen eines Azure Active Directory-Verzeichnisses](media/active-directory-access-create-new-tenant/azure-ad-create-new-tenant.png)

2.  Geben Sie auf der Seite **Verzeichnis erstellen** die folgenden Informationen ein:
    
    - Geben Sie in das Feld **Organisationsname** _Contoso_ ein.

    - Geben Sie in das Feld **Name der Anfangsdomäne** _Contoso_ ein.

    - Übernehmen Sie im Feld **Land oder Region** die Option _USA_.

3. Klicken Sie auf **Erstellen**.

Ihr neuer Mandant wird mit der Domäne „contoso.onmicrosoft.com“ erstellt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn Sie diese Anwendung nicht mehr verwenden möchten, können Sie den Mandanten folgendermaßen löschen:

- Wählen Sie **Azure Active Directory**, die Seite **Contoso – Übersicht** und dann **Verzeichnis löschen** aus.

    Der Mandant und die dazugehörigen Informationen werden gelöscht.

    ![Seite zum Erstellen eines Verzeichnisses](media/active-directory-access-create-new-tenant/azure-ad-delete-new-tenant.png)

## <a name="next-steps"></a>Nächste Schritte
- Wie Sie zusätzliche Domänennamen ändern oder hinzufügen, erfahren Sie unter [Hinzufügen eines benutzerdefinierten Domänennamens zu Azure Active Directory](add-custom-domain.md).

- Weitere Informationen zum Hinzufügen von Benutzern finden Sie unter [Hinzufügen oder Löschen eines neuen Benutzers](add-users-azure-active-directory.md).

- Der Artikel [Erstellen einer Basisgruppe und Hinzufügen von Mitgliedern](active-directory-groups-create-azure-portal.md) erläutert, wie Sie Gruppen und Mitglieder hinzufügen.

- Erfahren Sie mehr über [rollenbasierten Zugriff mit Privileged Identity Management](../../role-based-access-control/pim-azure-resource.md) und [bedingten Zugriff](../../role-based-access-control/conditional-access-azure-management.md), um den Zugriff auf die Anwendungen und Ressourcen Ihrer Organisation zu verwalten.

- Weitere Informationen zu Azure AD finden Sie unter [Was ist Azure Active Directory?](active-directory-whatis.md). Dieser Artikel enthält allgemeine Lizenzierungsinformationen sowie Details zu Terminologie und zugehörigen Features.
