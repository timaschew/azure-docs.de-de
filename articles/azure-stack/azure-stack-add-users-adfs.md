---
title: Hinzufügen von Benutzern für ADFS-Bereitstellungen von Azure Stack | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Benutzer für ADFS-Bereitstellungen von Azure Stack hinzufügen.
services: azure-stack
documentationcenter: ''
author: patricka
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: patricka
ms.reviewer: unknown
ms.openlocfilehash: f8abacbcb05d1346931b5c2e1097660cfbd8e594
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49344165"
---
# <a name="add-azure-stack-users-in-ad-fs"></a>Hinzufügen von Azure Stack-Benutzern in AD FS
Sie können das Snap-In **Active Directory-Benutzer und -Computer** zum Hinzufügen weiterer Benutzer zu einer Azure Stack-Umgebung verwenden und dabei AD FS als Identitätsanbieter nutzen.

## <a name="add-windows-server-active-directory-users"></a>Hinzufügen von Windows Server Active Directory-Benutzern
> [!TIP]
> In diesem Beispiel wird das standardmäßige ASDK-Active Directory „azurestack.local“ verwendet. 

1.  Melden Sie sich bei einem Computer mit einem Konto an, das Zugriff auf die Windows-Verwaltungstools bietet, und öffnen Sie eine neue Microsoft Management Console (MMC).
2.  Klicken Sie auf **Datei > Snap-Ins hinzufügen bzw. entfernen**.
3.  Klicken Sie auf **Active Directory-Benutzer und -Computer** > **AzureStack.local** > **Benutzer**.
4.  Klicken Sie auf **Aktion** > **Neu** > **Benutzer**.
5.  Geben Sie im Fenster „Neues Objekt – Benutzer“ ein Kennwort ein, und bestätigen Sie es.
6.  Klicken Sie auf **Weiter**, um die Eingabe von Werten abzuschließen, und dann auf „Fertig stellen“, um den Benutzer zu erstellen.


## <a name="next-steps"></a>Nächste Schritte
[Erstellen von Dienstprinzipalen](azure-stack-create-service-principals.md)