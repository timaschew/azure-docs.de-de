---
title: Azure Stack-MySQL-Ressourcenanbieter 1.1.30.0 – Versionshinweise | Microsoft-Dokumentation
description: Erfahren Sie, was im neuesten Update für den Azure Stack-MySQL-Ressourcenanbieter enthalten ist, welche bekannten Probleme vorliegen und wo Sie den Download finden.
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
ms.openlocfilehash: 6354212eb95fbefb217dd5339613d050da55f4ba
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688269"
---
# <a name="mysql-resource-provider-11300--release-notes"></a>Versionshinweise zum MySQL-Ressourcenanbieter 1.1.30.0

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

In diesen Versionshinweisen werden die Verbesserungen und bekannten Probleme in Version 1.1.30.0 des MySQL-Ressourcenanbieters beschrieben.

## <a name="build-reference"></a>Buildreferenz
Laden Sie die Binärdatei des MySQL-Ressourcenanbieters herunter, und führen Sie dann den Self-Extractor aus, um den Inhalt in ein temporäres Verzeichnis zu extrahieren. Der Ressourcenanbieter verfügt über einen entsprechenden Mindestbuild für Azure Stack. Hier ist angegeben, welche Azure Stack-Releaseversion für die Installation dieser Version des MySQL-Ressourcenanbieters erforderlich ist:

> |Azure Stack-Mindestversion|Version des MySQL-Ressourcenanbieters|
> |-----|-----|
> |Azure Stack-Update 1808 (1.1808.0.97)|[1.1.30.0](https://aka.ms/azurestackmysqlrp11300)|
> |     |     |

> [!IMPORTANT]
> Wenden Sie das mindestens erforderliche Azure Stack-Update auf Ihr integriertes Azure Stack-System an, oder stellen Sie das aktuelle Azure Stack Development Kit (ASDK) bereit, bevor Sie die neueste Version des MySQL-Ressourcenanbieters bereitstellen.

## <a name="new-features-and-fixes"></a>Neue Features und Fehlerbehebungen
Diese Version des Azure Stack-MySQL-Ressourcenanbieters enthält die folgenden Verbesserungen und Fehlerbehebungen:

- **Aktivierte Telemetriedaten für Bereitstellungen des MySQL-Ressourcenanbieters**. Die Erfassung von Telemetriedaten wurde für Bereitstellungen des MySQL-Ressourcenanbieters aktiviert. Zu den erfassten Telemetriedaten gehören beispielsweise die Ressourcenanbieterbereitstellung, Start- und Stoppzeiten, Beendigungsstatus, Beendigungsmeldungen und Fehlerdetails (falls zutreffend).

- **Update der TLS 1.2-Verschlüsselung**. Die ausschließliche Unterstützung von TLS 1.2 für die Ressourcenanbieterkommunikation mit internen Azure Stack-Komponenten wurde aktiviert. 

### <a name="fixes"></a>Fehlerbehebungen

- **Azure Stack-PowerShell-Kompatibilität für MySQL-Ressourcenanbieter**. Der MySQL-Ressourcenanbieter wurde aktualisiert, damit er mit dem PowerShell-Profil „Azure Stack 2018-03-01-hybrid“ funktioniert und Kompatibilität mit AzureRM 1.3.0 und höher gewährleistet wird.

- **Blatt zur Änderung des MySQL-Anmeldekennworts**. Es wurde ein Problem behoben, bei dem das Kennwort auf dem entsprechenden Blatt nicht geändert werden kann. Links wurden aus Benachrichtigungen zur Kennwortänderung entfernt.

## <a name="known-issues"></a>Bekannte Probleme 

- **Dauer von bis zu einer Stunde bis zur Anzeige von MySQL SKUs im Portal**. Es kann bis zu einer Stunde dauern, bis die neu erstellten SKUs beim Erstellen neuer MySQL-Datenbanken angezeigt werden. 

    **Problemumgehung**: Keine.

- **Wiederverwendung von MySQL-Anmeldungen**. Der Versuch einer neuen MySQL-Anmeldung mit dem gleichen Benutzernamen wie eine vorhandene Anmeldung unter demselben Abonnement führt dazu, dass die gleiche Anmeldung und das vorhandene Kennwort wiederverwendet werden. 

    **Problemumgehung**: Nutzen Sie unterschiedliche Benutzernamen, wenn Sie neue Anmeldungen unter demselben Abonnement oder Anmeldungen mit dem gleichen Benutzernamen unter unterschiedlichen Abonnements erstellen.


### <a name="known-issues-for-cloud-admins-operating-azure-stack"></a>Bekannte Probleme von Cloudadministratoren, die Azure Stack betreiben
Lesen Sie die Dokumentation in den [Versionshinweisen zu Azure Stack](azure-stack-servicing-policy.md).

## <a name="next-steps"></a>Nächste Schritte
[Informieren Sie sich über den MySQL-Ressourcenanbieter](azure-stack-mysql-resource-provider.md).

[Bereiten Sie sich auf die Bereitstellung des MySQL-Ressourcenanbieters vor](azure-stack-mysql-resource-provider-deploy.md#prerequisites).

[Führen Sie für den MySQL-Ressourcenanbieter ein Upgrade von einer früheren Version durch](azure-stack-mysql-resource-provider-update.md). 