---
title: Deaktivieren der E-Mail-Überprüfung während der Registrierung von Endbenutzern in Azure Active Directory B2C | Microsoft-Dokumentation
description: Dieses Thema veranschaulicht, wie Sie die E-Mail-Überprüfung während der Registrierung von Endbenutzern in Azure Active Directory B2C deaktivieren.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 2/06/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: e36dd19aa020b8cb2a623cda904cf7fa8a0b26da
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51004591"
---
# <a name="disable-email-verification-during-consumer-sign-up-in-azure-active-directory-b2c"></a>Deaktivieren der E-Mail-Überprüfung während der Registrierung von Endbenutzern – Azure Active Directory B2C 
Wenn diese Funktionalität aktiviert ist, ermöglicht Azure Active Directory B2C es Endbenutzern, sich durch Angeben einer E-Mail-Adresse und Erstellen eines lokalen Kontos für Anwendungen zu registrieren. Azure AD B2C stellt sicher, dass gültige E-Mail-Adressen verwendet werden, indem Endbenutzer diese während des Registrierungsvorgangs verifizieren müssen. B2C verhindert auch, dass ein automatisierter Prozess böswillig gefälschte Konten für die Anwendungen generiert.

Einige Anwendungsentwickler bevorzugen, die E-Mail-Überprüfung während des Registrierungsvorgangs zu überspringen und diese erst später vom Endbenutzer anzufordern. Um dies zu unterstützen, kann Azure AD B2C so konfiguriert werden, dass die E-Mail-Überprüfung deaktiviert wird. Diese Vorgehensweise sorgt für einen einfacheren Registrierungsvorgang und bietet Entwicklern die Flexibilität, zwischen Endbenutzern zu unterscheiden, die ihre E-Mail-Adresse bereits verifiziert haben, und solchen, die dies noch nicht getan haben.

Standardmäßig ist die E-Mail-Überprüfung in Registrierungsrichtlinien aktiviert. Führen Sie zum Aktivieren der Funktion folgende Schritte aus:

1. Klicken Sie auf **Registrierungsrichtlinien** oder auf **Registrierungs- oder Anmelderichtlinien**, je nachdem, welche Art Richtlinien Sie für die Registrierung konfiguriert haben.
2. Klicken Sie auf Ihre Richtlinie (z.B. „B2C_1_SiUp“), um sie zu öffnen. 
3. Klicken Sie oben auf dem Blatt auf **Bearbeiten**.
4. Klicken Sie auf **Seite für die Benutzeroberflächenanpassung**.
5. Klicken Sie auf **Registrierungsseite für lokales Konto**.
6. Klicken Sie im Abschnitt **Registrierungsattribute** in der Spalte **Name** auf **E-Mail-Adresse**.
7. Legen Sie die Option **Überprüfung anfordern** auf **Nein** fest.
8. Klicken Sie im unteren Bereich auf **OK**, bis Sie zum Blatt **Richtlinie bearbeiten** gelangen.
9. Klicken Sie oben auf dem Blatt auf **Speichern** . Sie haben es geschafft!

> [!NOTE]
> Die Deaktivierung der E-Mail-Überprüfung während des Registrierungsvorgangs kann zu Spam führen. Wenn Sie den Standardprozess deaktivieren, empfiehlt es sich, ein eigenes Überprüfungssystem einzurichten.
> 
>