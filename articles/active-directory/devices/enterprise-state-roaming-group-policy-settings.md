---
title: Gruppenrichtlinien- und MDM-Einstellungen | Microsoft Docs
description: Enthält Informationen zu Gruppenrichtlinien- und MDM-Einstellungen (Mobile Device Management, Verwaltung mobiler Geräte), die auf unternehmenseigenen Geräten verwendet werden sollen. Diese Richtlinien werden auf das gesamte Gerät eines Benutzers angewendet.
services: active-directory
keywords: Was sind Gruppenrichtlinien- und MDM-Einstellungen für Enterprise State Roaming, Enterprise State Roaming, Windows-Cloud
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: curtand
ms.component: devices
ms.assetid: 6471a9b3-8dd4-4237-89d1-bfbeca9f8252
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2018
ms.author: markvi
ms.openlocfilehash: c6ec20b7467998d221858dfd852461ad33a64494
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2018
ms.locfileid: "51035212"
---
# <a name="group-policy-and-mdm-settings"></a>Gruppenrichtlinien- und MDM-Einstellungen
Verwenden Sie diese Gruppenrichtlinien- und MDM-Einstellungen (Mobile Device Management, Verwaltung mobiler Geräte) nur für unternehmenseigene Geräte, da diese Richtlinien auf das gesamte Gerät eines Benutzers angewendet werden. Das Anwenden einer MDM-Richtlinie zum Deaktivieren der Einstellungssynchronisierung für ein persönliches, benutzereigenes Gerät hat eine negative Auswirkung auf die Nutzung dieses Geräts. Außerdem werden weitere Benutzerkonten auf dem Gerät von der Richtlinie ebenfalls beeinflusst.

Unternehmen, die das Roaming für persönliche (nicht verwaltete) Geräte verwalten möchten, können das Azure-Portal zum Aktivieren oder Deaktivieren des Roamings verwenden, anstatt die Gruppenrichtlinie oder MDM.
In den folgenden Tabellen sind die verfügbaren Richtlinieneinstellungen beschrieben.

## <a name="mdm-settings"></a>MDM-Einstellungen
Die MDM-Richtlinieneinstellungen gelten sowohl für Windows 10 als auch für Windows 10 Mobile.  Für Windows 10 Mobile wird das auf einem Microsoft-Konto basierende Roaming nur über das OneDrive-Konto eines Benutzers unterstützt.  Unter [Geräte und Endpunkte](enterprise-state-roaming-windows-settings-reference.md) finden Sie Informationen zu den Geräten, die bei der Azure AD-basierten Synchronisierung unterstützt werden.

| NAME | BESCHREIBUNG |
| --- | --- |
| Microsoft-Konto-Verbindung zulassen |Ermöglicht Benutzern die Authentifizierung mit einem Microsoft-Konto auf dem Gerät. |
| Einstellungen synchronisieren zulassen |Ermöglicht Benutzern das Roaming von Windows-Einstellungen und App-Daten; durch Deaktivieren dieser Richtlinie wird die Synchronisierung mobiler Geräte ebenso wie Sicherungen auf mobilen Geräten deaktiviert. |

## <a name="group-policy-settings"></a>Gruppenrichtlinien-Einstellungen
Die Gruppenrichtlinieneinstellungen gelten für Windows 10-Geräte, die in eine Active Directory-Domäne eingebunden sind. Die Tabelle enthält Legacyeinstellungen, die scheinbar zum Verwalten von Synchronisierungseinstellungen geeignet sind, aber für Enterprise State Roaming für Windows 10 nicht funktionieren (in der Beschreibung als „Nicht verwenden“ gekennzeichnet).

Diese Einstellungen befinden sich unter `Computer Configuration > Administrative Templates > Windows Components > Sync your settings`. 

| NAME | BESCHREIBUNG |
| --- | --- |
| Konten: Microsoft-Konten blockieren |Diese Richtlinieneinstellung verhindert, dass Benutzer auf diesem Computer neue Microsoft-Konten hinzufügen. |
| Nicht synchronisieren |Verhindert das Roaming von Windows-Einstellungen und App-Daten durch Benutzer. |
| Personalisierung nicht synchronisieren |Deaktiviert die Synchronisierung der Gruppe „Designs“. |
| Browsereinstellungen nicht synchronisieren |Deaktiviert die Synchronisierung der Gruppe „Internet Explorer“. |
| Kennwörter nicht synchronisieren |Deaktiviert die Synchronisierung der Gruppe „Kennwörter“. |
| Weitere Windows-Einstellungen nicht synchronisieren |Deaktiviert die Synchronisierung der Gruppe „Weitere Windows-Einstellungen“. |
| Desktoppersonalisierung nicht synchronisieren |Nicht verwenden, hat keine Auswirkung. |
| Keine Synchronisierung über getaktete Verbindungen |Deaktiviert das Roaming über getaktete Verbindungen, z. B. eine 3G-Mobilfunkverbindung. |
| Apps nicht synchronisieren |Nicht verwenden, hat keine Auswirkung. |
| App-Einstellungen nicht synchronisieren |Deaktiviert das Roaming von App-Daten. |
| Starteinstellungen nicht synchronisieren |Nicht verwenden, hat keine Auswirkung. |

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die Übersicht zu [Enterprise State Roaming](enterprise-state-roaming-overview.md) an.


