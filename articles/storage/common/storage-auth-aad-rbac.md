---
title: Verwenden von RBAC zum Verwalten von Zugriffsrechten für Container und Warteschlangen (Vorschauversion) – Azure Storage | Microsoft-Dokumentation
description: Verwenden Sie die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC), um Benutzern, Gruppen, Anwendungsdienstprinzipalen oder verwalteten Dienstidentitäten Rollen für den Zugriff auf Blob- und Warteschlangendaten zuzuweisen. Azure Storage unterstützt integrierte und benutzerdefinierte Rollen für Zugriffsrechte für Container und Warteschlangen.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 10/15/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: 5da34e8e79c0df88cfad0b398aa8e1c22f158489
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49428877"
---
# <a name="manage-access-rights-to-azure-blob-and-queue-data-with-rbac-preview"></a>Verwalten von Zugriffsrechten für Blob- und Warteschlangendaten von Azure mit RBAC (Vorschau)

Azure Active Directory (Azure AD) autorisiert Rechte für den Zugriff auf abgesicherte Ressourcen über die [rollenbasierte Zugriffssteuerung (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview). Azure Storage bietet eine Reihe integrierter RBAC-Rollen mit üblichen Berechtigungssätzen für den Zugriff auf Container und Warteschlangen. Wenn einer Azure AD-Identität eine RBAC-Rolle zugewiesen wird, wird dieser Identität Zugriff auf die jeweiligen Ressourcen gemäß dem angegebenen Umfang gewährt. Der Zugriff kann auf die Ebene des Abonnements, der Ressourcengruppe, des Speicherkontos oder eines einzelnen Containers oder einer Warteschlange begrenzt werden. Sie können diese Zugriffsrechte für Azure Storage-Ressourcen über das Azure-Portal, Azure-Befehlszeilentools und Azure-Verwaltungs-APIs zuweisen. 

Eine Azure AD-Identität kann ein Benutzer, eine Gruppe, ein Anwendungsdienstprinzipal oder eine verwaltete Identität für Azure-Ressourcen sein. Ein Sicherheitsprinzipal kann ein Benutzer, eine Gruppe oder ein Anwendungsdienstprinzipal sein. Eine [verwaltete Identität für Azure-Ressourcen](../../active-directory/managed-identities-azure-resources/overview.md) ist eine automatisch verwaltete Identität, die zur Authentifizierung über Anwendungen dient, die unter anderem auf virtuellen Azure-Computern, in Funktions-Apps und in VM-Skalierungsgruppen ausgeführt werden. Einen Überblick über Identität in Azure AD finden Sie unter [Grundlegendes zu Azure-Identitätslösungen](https://docs.microsoft.com/azure/active-directory/understand-azure-identity-solutions).

[!INCLUDE [storage-auth-aad-note-include](../../../includes/storage-auth-aad-note-include.md)]

## <a name="rbac-roles-for-blobs-and-queues"></a>RBAC-Rollen für Blobs und Warteschlangen

Azure Storage unterstützt sowohl integrierte als auch benutzerdefinierte RBAC-Rollen. Azure Storage bietet diese integrierten RBAC-Rollen für die Verwendung mit Azure AD:

- [Mitwirkender an Storage-Blobdaten (Vorschau)](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-contributor-preview)
- [Storage-Blobdatenleser (Vorschau)](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-reader-preview)
- [Mitwirkender an Storage-Warteschlangendaten (Vorschau)](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-contributor-preview)
- [Storage-Warteschlangendatenleser (Vorschau)](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-reader-preview)

Weitere Informationen dazu, wie integrierte Rollen für Azure Storage definiert sind, finden Sie unter [Grundlegendes zu Rollendefinitionen](https://docs.microsoft.com/azure/role-based-access-control/role-definitions#management-and-data-operations-preview).

Sie können für die Verwendung mit Containern und Warteschlangen auch eigene Rollen definieren. Weitere Informationen finden Sie unter [Erstellen benutzerdefinierter Rollen für die rollenbasierte Zugriffssteuerung in Azure](https://docs.microsoft.com/azure/role-based-access-control/custom-roles). 

[!INCLUDE [storage-auth-aad-note-include](../../../includes/storage-auth-aad-note-include.md)]

## <a name="assign-a-role-to-a-security-principal"></a>Zuweisen einer Rolle zu einem Sicherheitsprinzipal

Weisen Sie einer Azure-Identität eine RBAC-Rolle zu, um Berechtigungen für Container oder Warteschlangen in Ihrem Speicherkonto zu erteilen. Sie können die Rollenzuweisung auf das Speicherkonto oder auf einen bestimmten Container oder eine bestimmte Warteschlange begrenzen. In der folgenden Tabelle sind die von den integrierten Rollen gewährten Zugriffsrechte je nach Geltungsbereich zusammengefasst: 

|                                 |     Mitwirkender an Blobdaten                                                 |     Leser von Blobdaten                                                |     Mitwirkender an Warteschlangendaten                                  |     Leser von Warteschlangendaten                                 |
|---------------------------------|------------------------------------------------------------------------------|------------------------------------------------------------------------|----------------------------------------------------------------|----------------------------------------------------------|
|    Auf Abonnement begrenzt       |    Lese-/Schreibzugriff auf alle Container und Blobs im Abonnement       |    Lesezugriff auf alle Container und Blobs im Abonnement       |    Lese-/Schreibzugriff auf alle Warteschlangen im Abonnement       |    Lesezugriff auf alle Warteschlangen im Abonnement         |
|    Auf Ressourcengruppe begrenzt     |    Lese-/Schreibzugriff auf alle Container und Blobs in der Ressourcengruppe     |    Lesezugriff auf alle Container und Blobs in der Ressourcengruppe     |    Lese-/Schreibzugriff auf alle Warteschlangen in der Ressourcengruppe     |    Lesezugriff auf alle Warteschlangen in der Ressourcengruppe     |
|    Auf Speicherkonto begrenzt    |    Lese-/Schreibzugriff auf alle Container und Blobs im Speicherkonto    |    Lesezugriff auf alle Container und Blobs im Speicherkonto    |    Lese-/Schreibzugriff auf alle Warteschlangen im Speicherkonto    |    Lesezugriff auf alle Warteschlangen im Speicherkonto    |
|    Auf Container/Warteschlange begrenzt    |    Lese-/Schreibzugriff auf den angegebenen Container und seine Blobs              |    Lesezugriff auf den angegebenen Container und seine Blobs              |    Lese-/Schreibzugriff auf die angegebene Warteschlange                  |    Lesezugriff auf die angegebene Warteschlange                    |

> [!NOTE]
> Als Besitzer Ihres Azure Storage-Kontos erhalten Sie nicht automatisch Berechtigungen für den Zugriff auf Daten. Sie müssen sich selbst explizit eine RBAC-Rolle für Azure Storage zuweisen. Sie können sie auf der Ebene Ihres Abonnements, einer Ressourcengruppe, eines Speicherkontos oder eines Containers oder einer Warteschlange zuordnen.

Einzelheiten zu den Berechtigungen, die für das Aufrufen von Azure Storage-Vorgängen erforderlich sind, finden Sie unter [Berechtigungen für das Aufrufen von REST-Vorgängen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations).

In den folgenden Abschnitten wird gezeigt, wie Sie dem Speicherkonto eine Rolle zuweisen, die auf ein einzelnes Speicherkonto oder einen einzelnen Container begrenzt ist.

### <a name="assign-a-role-scoped-to-the-storage-account-in-the-azure-portal"></a>Zuweisen einer auf das Speicherkonto begrenzten Rolle im Azure-Portal

So weisen Sie im Azure-Portal eine integrierte Rolle zu, die Zugriff auf alle Container oder Warteschlangen im Speicherkonto gewährt

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem Speicherkonto.
2. Wählen Sie Ihr Speicherkonto und dann **Zugriffssteuerung (IAM)** aus, um Zugriffssteuerungseinstellungen für das Konto anzuzeigen. Klicken Sie auf die Schaltfläche **Hinzufügen**, um eine neue Rolle hinzuzufügen.

    ![Screenshot mit Zugriffssteuerungseinstellungen für Speicher](media/storage-auth-aad-rbac/portal-access-control.png)

3. Wählen Sie im Fenster **Berechtigungen hinzufügen** die Rolle aus, die einer Azure AD-Identität zugewiesen werden soll. Suchen Sie dann die Identität, der Sie diese Rolle zuweisen möchten. Die folgende Abbildung zeigt beispielsweise die einem Benutzer zugewiesene Rolle **Storage-Blobdatenleser (Vorschau)**.

    ![Screenshot, der das Zuweisen einer RBAC-Rolle zeigt](media/storage-auth-aad-rbac/add-rbac-role.png)

4. Klicken Sie auf **Speichern**. Die Identität, der Sie die Rolle zugewiesen haben, wird unter dieser Rolle angezeigt. Die folgende Abbildung zeigt z.B., dass die hinzugefügten Benutzer nun Leseberechtigungen für alle Blobdaten im Speicherkonto haben.

    ![Screenshot mit einer Liste von Benutzern, die einer Rolle zugewiesen sind](media/storage-auth-aad-rbac/account-scoped-role.png)

### <a name="assign-a-role-scoped-to-a-container-or-queue-in-the-azure-portal"></a>Zuweisen einer auf einen Container oder eine Warteschlange begrenzten Rolle im Azure-Portal

Die Schritte zum Zuweisen einer auf einen Container oder eine Warteschlange begrenzten integrierten Rolle sind ähnlich. Die hier gezeigte Vorgehensweise weist eine auf einen Container begrenzte Rolle zu, aber Sie können die gleichen Schritte ausführen, um eine auf eine Warteschlange begrenzte Rolle zuzuweisen: 

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem Speicherkonto, und zeigen Sie die **Übersicht** für das Konto an.
2. Wählen Sie unter „Blob-Dienst“ die Option **Blobs durchsuchen** aus. 
3. Navigieren Sie zum Container, dem Sie eine Rolle zuweisen möchten, und zeigen Sie die Einstellungen des Containers an. 
4. Wählen Sie **Zugriffssteuerung (IAM)** aus, um Zugriffssteuerungseinstellungen für den Container anzuzeigen.
5. Wählen Sie im Fenster **Berechtigungen hinzufügen** die Rolle aus, die Sie einer Azure AD-Identität zuweisen möchten. Suchen Sie dann die Identität, der Sie diese Rolle zuweisen möchten.
6. Klicken Sie auf **Speichern**. Die Identität, der Sie die Rolle zugewiesen haben, wird unter dieser Rolle angezeigt. Die folgende Abbildung zeigt z.B., dass der hinzugefügte Benutzer nun Leseberechtigungen für Daten im Container *sample-container* hat.

    ![Screenshot mit einer Liste von Benutzern, die einer Rolle zugewiesen sind](media/storage-auth-aad-rbac/container-scoped-role.png)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur rollenbasierte Zugriffssteuerung finden Sie unter [Erste Schritte mit der rollenbasierten Zugriffssteuerung im Azure-Portal](../../role-based-access-control/overview.md).
- Informationen zum Zuweisen und Verwalten von RBAC-Rollenzuweisungen mit Azure PowerShell, Azure CLI oder der REST-API finden Sie in diesen Artikeln:
    - [Verwalten der rollenbasierten Zugriffssteuerung mit Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md)
    - [Verwalten der rollenbasierten Zugriffssteuerung mit der Azure-Befehlszeilenschnittstelle](../../role-based-access-control/role-assignments-cli.md)
    - [Verwalten der rollenbasierten Zugriffssteuerung mit der REST-API](../../role-based-access-control/role-assignments-rest.md)
- Informationen zum Autorisieren des Zugriffs auf Container und Warteschlangen in Ihren Speicheranwendungen finden Sie unter [Verwenden von Azure AD mit Azure Storage-Anwendungen](storage-auth-aad-app.md).
- Weitere Informationen zur Azure AD-Integration für Azure-Container und -Warteschlangen finden Sie im Blogbeitrag des Azure Storage-Teams [Announcing the Preview of Azure AD Authentication for Azure Storage](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/) (Ankündigung der Vorschau der Azure AD-Authentifizierung für Azure Storage).
- 
