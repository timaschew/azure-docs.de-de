---
title: Azure Stack-SQL-Ressourcenanbieter 1.1.30.0 – Versionshinweise | Microsoft-Dokumentation
description: Es wird beschrieben, was im neuesten Update für den Azure Stack-SQL-Ressourcenanbieter enthalten ist, z.B. bekannte Probleme und der Downloadort.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: d2dda25c63a6e4a73205b9e4a830a211d025b3ed
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688273"
---
# <a name="sql-resource-provider-11300-release-notes"></a>Versionshinweise zum SQL-Ressourcenanbieter 1.1.30.0

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

In diesen Versionshinweisen werden die Verbesserungen und bekannten Probleme in Version 1.1.30.0 des SQL-Ressourcenanbieters beschrieben.

## <a name="build-reference"></a>Buildreferenz
Laden Sie die Binärdatei des SQL-Ressourcenanbieters herunter, und führen Sie dann den Self-Extractor aus, um den Inhalt in ein temporäres Verzeichnis zu extrahieren. Der Ressourcenanbieter verfügt über einen entsprechenden Mindestbuild für Azure Stack. Hier ist angegeben, welche Azure Stack-Releaseversion für die Installation dieser Version des SQL-Ressourcenanbieters erforderlich ist:

> |Azure Stack-Mindestversion|Version des SQL-Ressourcenanbieters|
> |-----|-----|
> |Version 1808 (1.1808.0.97)|[1.1.30.0](https://aka.ms/azurestacksqlrp11300)|
> |     |     |

> [!IMPORTANT]
> Wenden Sie das mindestens erforderliche Azure Stack-Update auf Ihr integriertes Azure Stack-System an, oder stellen Sie das aktuelle Azure Stack Development Kit (ASDK) bereit, bevor Sie die neueste Version des SQL-Ressourcenanbieters bereitstellen.

## <a name="new-features-and-fixes"></a>Neue Features und Fehlerbehebungen
Diese Version des Azure Stack-SQL-Ressourcenanbieters enthält die folgenden Verbesserungen und Fehlerbehebungen:

- **Aktivierte Telemetriedaten für Bereitstellungen des SQL-Ressourcenanbieters**: Die Erfassung von Telemetriedaten wurde für Bereitstellungen des SQL-Ressourcenanbieters aktiviert. Zu den erfassten Telemetriedaten gehören beispielsweise die Ressourcenanbieterbereitstellung, Start- und Stoppzeiten, Beendigungsstatus, Beendigungsmeldungen und Fehlerdetails (falls zutreffend).

- **Update der TLS 1.2-Verschlüsselung**: Die ausschließliche Unterstützung von TLS 1.2 für die Ressourcenanbieterkommunikation mit internen Azure Stack-Komponenten wurde aktiviert. 

### <a name="fixes"></a>Fehlerbehebungen

- **Azure Stack-PowerShell-Kompatibilität für SQL-Ressourcenanbieter**: Der SQL-Ressourcenanbieter wurde aktualisiert, damit er mit dem PowerShell-Profil „Azure Stack 2018-03-01-hybrid“ funktioniert und Kompatibilität mit AzureRM 1.3.0 und höher besteht.

- **Blatt für Änderung des SQL-Anmeldekennworts**: Es wurde ein Problem behoben, bei dem das Kennwort auf dem entsprechenden Blatt nicht geändert werden kann. Links wurden aus Benachrichtigungen zur Kennwortänderung entfernt.

- **Aktualisierung des Blatts mit SQL-Hostservereinstellungen**: Es wurde ein Problem behoben, bei dem das Blatt mit den Einstellungen fälschlicherweise den Titel „Kennwort“ erhalten hat.

## <a name="known-issues"></a>Bekannte Probleme 

- **Dauer von bis zu einer Stunde bis zur Anzeige von SQL SKUs im Portal**: Es kann bis zu einer Stunde dauern, bis die neu erstellten SKUs beim Erstellen neuer SQL-Datenbanken angezeigt werden. 

    **Problemumgehung**: Keine.

- **Wiederverwendung von SQL-Anmeldungen**: Der Versuch einer neuen SQL-Anmeldung mit dem gleichen Benutzernamen wie eine vorhandene Anmeldung unter demselben Abonnement führt dazu, dass die gleiche Anmeldung und das vorhandene Kennwort wiederverwendet werden. 

    **Problemumgehung**: Nutzen Sie unterschiedliche Benutzernamen, wenn Sie neue Anmeldungen unter demselben Abonnement oder Anmeldungen mit dem gleichen Benutzernamen unter unterschiedlichen Abonnements erstellen.

- **Dateninkonsistenz aufgrund von gemeinsam genutzten SQL-Anmeldungen**: Wenn eine SQL-Anmeldung für mehrere SQL-Datenbanken unter demselben Abonnement gemeinsam genutzt wird, führt eine Änderung des Anmeldekennworts zu Dateninkonsistenz.

    **Problemumgehung**: Verwenden Sie für unterschiedliche Datenbanken unter demselben Abonnement immer unterschiedliche Anmeldungen.

### <a name="known-issues-for-cloud-admins-operating-azure-stack"></a>Bekannte Probleme von Cloudadministratoren, die Azure Stack betreiben
Lesen Sie die Dokumentation in den [Versionshinweisen zu Azure Stack](azure-stack-servicing-policy.md).

## <a name="next-steps"></a>Nächste Schritte
[Informieren Sie sich über den SQL-Ressourcenanbieter](azure-stack-sql-resource-provider.md).

[Bereiten Sie sich auf die Bereitstellung des SQL-Ressourcenanbieters vor](azure-stack-sql-resource-provider-deploy.md#prerequisites).

[Führen Sie für den SQL-Ressourcenanbieter ein Upgrade von einer früheren Version durch](azure-stack-sql-resource-provider-update.md). 