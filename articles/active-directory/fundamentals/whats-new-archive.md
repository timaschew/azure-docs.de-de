---
title: Archiv für Neuerungen in Azure Active Directory | Microsoft-Dokumentation
description: Die Versionshinweise zu Neuerungen im Abschnitt „Übersicht“ dieses Inhaltssatzes enthalten sechs Monate Aktivität. Nach 6 Monaten werden die Elemente aus den Hauptartikel entfernt und in diesem Archivartikel abgelegt.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 11/14/2018
ms.author: lizross
ms.reviewer: dhanyahk
ms.openlocfilehash: 54e63cf90d72b64dbe00ab8b65179f015c23b646
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2018
ms.locfileid: "52427606"
---
# <a name="archive-for-whats-new-in-azure-active-directory"></a>Archiv für Neuerungen in Azure Active Directory

Der primäre Artikel [Neuerungen in Azure Active Directory](whats-new.md) enthält die Informationen der letzten sechs Monate, während dieser Artikel alle älteren Informationen enthält.

Die Versionshinweise mit Neuerungen bieten Ihnen Informationen über:

- Neueste Versionen
- Bekannte Probleme
- Fehlerbehebungen
- Veraltete Funktionen
- Pläne für Änderungen

---
 
## <a name="april-2018"></a>April 2018 

### <a name="azure-ad-b2c-access-token-are-ga"></a>Azure AD B2C-Zugriffstoken jetzt allgemein verfügbar

**Typ:** Neue Funktion  
**Dienstkategorie**: B2C – Kundenidentitätsverwaltung  
**Produktfunktion**: B2B/B2C 

Sie können nun mit Zugriffstoken auf Web-APIs zugreifen, die von Azure AD B2C geschützt werden. Das Feature wird von der Public Preview in die allgemeine Verfügbarkeit verlegt. Die Benutzeroberfläche zum Konfigurieren von Azure AD B2C-Anwendungen und Web-APIs wurde verbessert, und andere kleinere Verbesserungen wurden vorgenommen.
 
Weitere Informationen finden Sie unter [Azure AD B2C: Anfordern von Zugriffstoken](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-access-tokens).

---

### <a name="test-single-sign-on-configuration-for-saml-based-applications"></a>Testen der SSO-Konfiguration für SAML-basierte Anwendungen

**Typ:** Neue Funktion  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion**: SSO

Beim Konfigurieren von SAML-basierten SSO-Anwendungen können Sie die Integration auf der Konfigurationsseite testen. Wenn während des Anmeldens ein Fehler auftritt, können Sie den Fehler auf der Testbenutzeroberfläche bereitstellen, und Azure AD bietet Ihnen Lösungsschritte für das spezifische Problem an.

Weitere Informationen finden Sie unter

- [Konfigurieren des einmaligen Anmeldens für Anwendungen, die nicht im Azure Active Directory-Anwendungskatalog enthalten sind](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)
- [Debuggen des SAML-basierten einmaligen Anmeldens bei Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)

---
 
### <a name="azure-ad-terms-of-use-now-has-per-user-reporting"></a>Azure AD-Nutzungsbedingungen jetzt mit benutzerbasierter Berichterstellung

**Typ:** Neue Funktion  
**Dienstkategorie**: Nutzungsbedingungen  
**Produktfunktion:** Konformität
 
Administratoren können jetzt bestimmte Nutzungsbedingungen auswählen und alle Benutzer anzeigen, die diesen Nutzungsbedingungen zugestimmt haben, sowie Datum/Uhrzeit der Zustimmung.

Weitere Informationen finden Sie unter [Nutzungsbedingungsfeature für Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-tou).

---
 
### <a name="azure-ad-connect-health-risky-ip-for-ad-fs-extranet-lockout-protection"></a>Azure AD Connect Health: AD FS-Extranetsperrschutz für riskante IP-Adressen 

**Typ:** Neue Funktion  
**Dienstkategorie:** Sonstige  
**Produktfunktion:** Überwachung und Berichterstellung

Connect Health unterstützt nun die Möglichkeit, IP-Adressen zu erkennen, die einen Schwellenwert stündlicher oder täglicher fehlerhafter Benutzername/Kennwort-Anmeldungen überschreiten. Dieses Feature bietet folgende Funktionen:

- Umfassender Bericht, der IP-Adressen und die Anzahl der fehlerhaften stündlichen/täglichen Anmeldungen auf der Basis eines anpassbaren Schwellenwerts anzeigt.
- E-Mail-basierte Warnungen, die anzeigen, wenn eine bestimmte IP-Adresse den Schwellenwert fehlerhafter stündlicher/täglicher Benutzername/Kennwort-Anmeldungen überschritten hat.
- Eine Downloadoption für eine detaillierte Datenanalyse

Weitere Informationen finden Sie unter [Bericht über riskante IP-Adressen (Public Preview)](https://aka.ms/aadchriskyip).

---
 
### <a name="easy-app-config-with-metadata-file-or-url"></a>Einfache App-Konfiguration mit Metadatendatei oder URL

**Typ:** Neue Funktion  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion**: SSO

Auf der Enterprise-Anwendungsseite können Administratoren eine SAML-Metadatendatei zum Konfigurieren von SAML-basiertem Anmelden für AAD-Katalog- und Nicht-Katalog-Anwendung hochladen.

Darüber hinaus können Sie die Azure AD-Anwendungsverbundmetadaten-URL zum Konfigurieren von einmaligem Anmelden mit der entsprechenden Anwendung verwenden.

Weitere Informationen finden Sie unter [Konfigurieren des einmaligen Anmeldens für Anwendungen, die nicht im Azure Active Directory-Anwendungskatalog enthalten sind](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps).

---

### <a name="azure-ad-terms-of-use-now-generally-available"></a>Azure AD-Nutzungsbedingungen jetzt allgemein verfügbar

**Typ:** Neue Funktion  
**Dienstkategorie**: Nutzungsbedingungen  
**Produktfunktion:** Konformität
 

Azure AD-Nutzungsbedingungen wurden von „Public Preview“ in „Allgemein verfügbar“ geändert.

Weitere Informationen finden Sie unter [Nutzungsbedingungsfeature für Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-tou).

---

### <a name="allow-or-block-invitations-to-b2b-users-from-specific-organizations"></a>Zulassen oder Blockieren von Einladungen für B2B-Benutzer von bestimmten Organisationen

**Typ:** Neue Funktion  
**Dienstkategorie:** B2B  
**Produktfunktion**: B2B/B2C
 

Sie können jetzt angeben, für welche Partnerorganisationen Sie die Azure AD B2B-Zusammenarbeit und Freigaben einrichten möchten. Zu diesem Zweck können Sie wahlweise eine Liste der spezifischen Zulassungs- oder Verweigerungsdomänen erstellen. Wenn eine Domäne mit diesen Funktionen blockiert wird, können Mitarbeiter nicht mehr Einladungen an Personen in dieser Domäne senden.

So können Sie leichter den Zugriff auf Ihre Ressourcen steuern und dabei die Benutzerfreundlichkeit für berechtigte Benutzer garantieren.

Dieses B2B-Zusammenarbeitsfeature steht allen Kunden von Azure Active Directory zur Verfügung, und in Verbindung mit Azure AD Premium-Features wie bedingtem Zugriff und Identitätsschutz können Sie damit differenzierter steuern, wann und wie sich externe Geschäftsbenutzer anmelden und Zugriff erhalten.

Weitere Informationen finden Sie unter [Zulassen oder Blockieren von Einladungen für B2B-Benutzer von bestimmten Organisationen](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-allow-deny-list).

---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar

**Typ:** Neue Funktion  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion**: Integration von Drittanbieteranwendungen

Im April 2018 haben wir diese 13 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

Criterion HCM, [FiscalNote](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fiscalnote-tutorial), [Secret Server (lokal)](https://docs.microsoft.com/azure/active-directory/active-directory-saas-secretserver-on-premises-tutorial), [Dynamic Signal](https://docs.microsoft.com/azure/active-directory/active-directory-saas-dynamicsignal-tutorial), [mindWireless](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mindwireless-tutorial), [OrgChart Now](https://docs.microsoft.com/azure/active-directory/active-directory-saas-orgchartnow-tutorial), [Ziflow](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ziflow-tutorial), [AppNeta Performance Monitor](https://docs.microsoft.com/azure/active-directory/active-directory-saas-appneta-tutorial), [Elium](https://docs.microsoft.com/azure/active-directory/active-directory-saas-elium-tutorial) , [Fluxx Labs](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fluxxlabs-tutorial), [Cisco Cloud](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ciscocloud-tutorial), Shelf, [SafetyNet](https://docs.microsoft.com/azure/active-directory/active-directory-saas-safetynet-tutorial)

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial).

Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

---
 
### <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-applications-public-preview"></a>Gewähren des Zugriffs auf Ihre lokalen Anwendungen für B2B-Benutzer in Azure AD (Public Preview)

**Typ:** Neue Funktion  
**Dienstkategorie:** B2B  
**Produktfunktion**: B2B/B2C

Wenn Ihr Unternehmen Funktionen der Azure Active Directory B2B-Zusammenarbeit (Azure AD) verwendet, um Gastbenutzer aus Partnerunternehmen zu Ihrem Azure AD einzuladen, können Sie für diese B2B-Benutzer jetzt Zugriff auf lokale Apps bereitstellen. Diese lokalen Apps können die SAML-basierte Authentifizierung oder die integrierte Windows-Authentifizierung (Integrated Windows Authentication, IWA) mit eingeschränkter Kerberos-Delegierung (Kerberos Constrained Delegation, KCD) verwenden.

Weitere Informationen finden Sie unter [Gewähren des Zugriffs auf Ihre lokalen Anwendungen für B2B-Benutzer in Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-hybrid-cloud-to-on-premises).

---
 
### <a name="get-sso-integration-tutorials-from-the-azure-marketplace"></a>Tutorials zur SSO-Integration im Azure Marketplace verfügbar

**Typ:** Geänderte Funktion  
**Dienstkategorie:** Sonstige  
**Produktfunktion**: Integration von Drittanbieteranwendungen

Wenn eine im [Azure-Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1) aufgelistete Anwendung einmaliges Anmelden auf SAML-Basis unterstützt, erhalten Sie beim Klicken auf **Jetzt anfordern** das der Anwendung zugeordnete Integrationstutorial. 

---

### <a name="faster-performance-of-azure-ad-automatic-user-provisioning-to-saas-applications"></a>Verbesserte Leistung bei der automatischen Azure AD-Benutzerbereitstellung in SaaS-Anwendungen

**Typ:** Geänderte Funktion  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion**: Integration von Drittanbieteranwendungen
 
Bisher konnten Kunden, die die Connectors für Azure Active Directory-Benutzerbereitstellung für SaaS-Anwendungen nutzten (z.B. Salesforce, ServiceNow und Box) eine schwache Leistung feststellen, wenn ihre Azure AD-Mandanten über 100.000 kombinierte Benutzer und Gruppen enthielten, und sie legten mit Benutzer- und Gruppenzuweisungen fest, welche Benutzer bereitgestellt werden sollten.

Am 2. April 2018 wurden sehr bedeutende Verbesserungen der Leistung des Azure AD-Bereitstellungsdiensts wirksam, die die erforderliche Zeitspanne zur Ausführung der Erstsynchronisierung von Azure Active Directory mit SaaS-Zielanwendungen erheblich reduzieren.

In der Folge können viele Kunden, bei denen die Erstsynchronisierungen mit Apps mehrere Tage dauerten oder niemals abgeschlossen wurden, den Vorgang jetzt innerhalb Minuten oder Stunden abschließen.

Weitere Informationen finden Sie unter [Vorgänge während der Bereitstellung](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning#what-happens-during-provisioning).

---

### <a name="self-service-password-reset-from-windows-10-lock-screen-for-hybrid-azure-ad-joined-machines"></a>Self-Service-Kennwortzurücksetzung über Windows 10-Sperrbildschirm für hybride Computer mit Azure AD-Einbindung

**Typ:** Geänderte Funktion  
**Dienstkategorie**: Self-Service-Kennwortzurücksetzung  
**Produktfunktion**: Benutzerauthentifizierung
 
Das Windows 10-SSPR-Feature wurde aktualisiert, sodass es die Unterstützung für hybride Computer mit Azure AD-Einbindung einschließt. Dieses in Windows 10 RS4 verfügbare Feature ermöglicht Benutzern, ihre Kennwörter vom Sperrbildschirm eines Windows 10-Computers aus zurückzusetzen. Benutzer, die für die Self-Service-Kennwortzurücksetzung zugelassen und registriert sind, können dieses Feature nutzen.

Weitere Informationen finden Sie unter [Azure AD-Kennwortzurücksetzung über den Anmeldebildschirm](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-sspr-windows).

---

## <a name="march-2018"></a>März 2018
 
### <a name="certificate-expire-notification"></a>Benachrichtigung über Zertifikatablauf

**Typ:** Korrigiert  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion**: SSO
 
Azure AD sendet eine Benachrichtigung, wenn ein Zertifikat für eine Katalog- oder Nicht-Kataloganwendung kurz vor dem Ablauf steht. 

Einige Benutzer haben für Unternehmensanwendungen, die für SAML-basiertes einmaliges Anmelden konfiguriert waren, keine Benachrichtigungen erhalten. Dieses Problem wurde behoben. Azure AD sendet Benachrichtigungen für Zertifikate, die in 7, 30 und 60 Tagen ablaufen. Sie können dieses Ereignis in den Überwachungsprotokollen sehen. 

Weitere Informationen finden Sie unter

- [Verwalten von Zertifikaten für die einmalige Verbundanmeldung in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-sso-certs)
- [Berichte zu Überwachungsaktivitäten im Azure Active Directory-Portal](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
 
---
 
### <a name="twitter-and-github-identity-providers-in-azure-ad-b2c"></a>Identitätsanbieter Twitter und GitHub in Azure AD B2C

**Typ:** Neue Funktion  
**Dienstkategorie**: B2C – Kundenidentitätsverwaltung  
**Produktfunktion**: B2B/B2C
 
Sie können in Azure AD B2C jetzt Twitter oder GitHub als Identitätsanbieter hinzufügen. Twitter stellt von der öffentlichen Vorschau auf allgemeine Verfügbarkeit um. GitHub wird als öffentliche Vorschau veröffentlicht.

Weitere Informationen finden Sie unter [Was ist die Azure AD B2B-Zusammenarbeit?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
 
---

### <a name="restrict-browser-access-using-intune-managed-browser-with-azure-ad-application-based-conditional-access-for-ios-and-android"></a>Einschränken des Browserzugriffs durch Verwendung von Intune Managed Browser mit anwendungsbasiertem bedingtem Azure AD-Zugriff für iOS und Android

**Typ:** Neue Funktion  
**Dienstkategorie**: Bedingter Zugriff  
**Produktfunktion**: Identitätssicherheit und -schutz
 
**Jetzt in der Public Preview!**

**Intune Managed Browser – SSO:** Ihre Mitarbeiter können einmaliges Anmelden übergreifend für native Clients (beispielsweise Microsoft Outlook) und Intune Managed Browser für alle mit Azure AD verbundenen Apps verwenden.

**Intune Managed Browser – Unterstützung von bedingtem Zugriff:** Mithilfe von Richtlinien für den bedingten Zugriff können Sie nun für Mitarbeiter die Verwendung von Intune Managed Browser erzwingen.

Weitere Informationen hierzu finden Sie in unserem [Blogbeitrag](https://cloudblogs.microsoft.com/enterprisemobility/2018/03/15/the-intune-managed-browser-now-supports-azure-ad-sso-and-conditional-access/).

Weitere Informationen finden Sie unter

- [App-basierter bedingter Zugriff mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

- [Verwalten des Internetzugriffs mittels Richtlinien für Managed Browser mit Microsoft Intune](https://aka.ms/managedbrowser)  

---
 
### <a name="app-proxy-cmdlets-in-powershell-ga-module"></a>App-Proxy-Cmdlets im allgemein verfügbaren Powershell-Modul

**Typ:** Neue Funktion  
**Dienstkategorie:** App-Proxy  
**Produktfunktion**: Zugriffssteuerung
 
Das allgemein verfügbare PowerShell-Modul bietet jetzt Unterstützung für Anwendungsproxy-Cmdlets! Dass erfordert, dass Sie bei PowerShell-Modulen auf dem neuesten Stand bleiben müssen. Wenn Sie mehr als ein Jahr in Rückstand geraten, funktionieren einige Module möglicherweise nicht mehr. 

Weitere Informationen finden Sie unter [AzureAD](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0).
 
---
 
### <a name="office-365-native-clients-are-supported-by-seamless-sso-using-a-non-interactive-protocol"></a>Native Office 365-Clients werden vom nahtlosen einmaligen Anmelden mithilfe eines nicht interaktiven Protokolls unterstützt

**Typ:** Neue Funktion  
**Dienstkategorie**: Authentifizierungen (Anmeldungen)  
**Produktfunktion**: Benutzerauthentifizierung
 
Benutzer, die native Office 365-Clients (Version 16.0.8730.xxxx und höher) einsetzen, genießen mit nahtlosem einmaligem Anmelden eine Anmeldung ohne Benutzereingriff. Diese Unterstützung wird durch die Ergänzung von Azure AD durch ein nicht interaktives Protokoll (WS-Trust) bereitgestellt.

Weitere Informationen finden Sie unter [Wie funktioniert die Anmeldung auf einem nativen Client mit dem nahtlosen einmaligen Anmelden?](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-how-it-works#how-does-sign-in-on-a-native-client-with-seamless-sso-work).
 
---

### <a name="users-get-a-silent-sign-on-experience-with-seamless-sso-if-an-application-sends-sign-in-requests-to-azure-ads-tenant-endpoints"></a>Benutzer kommen mit dem nahtlosen einmaligen Anmelden in den Genuss einer Anmeldung ohne Benutzereingriff, wenn eine Anwendung Anmeldeanforderungen an Azure AD-Mandantenendpunkte sendet

**Typ:** Neue Funktion  
**Dienstkategorie**: Authentifizierungen (Anmeldungen)  
**Produktfunktion**: Benutzerauthentifizierung
 
Benutzer erhalten mit dem nahtlosen einmaligen Anmelden eine Anmeldung ohne Benutzereingriff, wenn eine Anwendung (z.B. `https://contoso.sharepoint.com`) Anmeldeanforderungen nicht mehr an den allgemeinen Azure AD-Endpunkt (`https://login.microsoftonline.com/common/<...>`) sendet, sondern an Mandantenendpunkte von Azure AD – d.h. `https://login.microsoftonline.com/contoso.com/<..>` oder `https://login.microsoftonline.com/<tenant_ID>/<..>`.

Weitere Informationen finden Sie unter [Nahtlose einmalige Anmeldung mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). 

---
 
### <a name="need-to-add-only-one-azure-ad-url-instead-of-two-urls-previously-to-users-intranet-zone-settings-to-roll-out-seamless-sso"></a>Für das Rollout des nahtlosen einmaligen Anmeldens muss den Intranet-Zoneneinstellungen des Benutzers nur noch eine Azure AD-URL statt zwei wie bisher hinzugefügt werden

**Typ:** Neue Funktion  
**Dienstkategorie**: Authentifizierungen (Anmeldungen)  
**Produktfunktion**: Benutzerauthentifizierung
 
Für das Rollout des nahtlosen einmaligen Anmeldens für Ihre Benutzer müssen Sie den Intranet-Zoneneinstellungen Ihrer Benutzer nur eine Azure AD-URL mithilfe einer Gruppenrichtlinie in Active Directory hinzufügen: `https://autologon.microsoftazuread-sso.com`. Bisher mussten Kunden zwei URLs hinzufügen.

Weitere Informationen finden Sie unter [Nahtlose einmalige Anmeldung mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). 
 
---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar

**Typ:** Neue Funktion  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion**: Integration von Drittanbieteranwendungen

Im März 2018 haben wir diese 15 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Boxcryptor](https://docs.microsoft.com/azure/active-directory/active-directory-saas-boxcryptor-tutorial), [CylancePROTECT](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cylanceprotect-tutorial), Wrike, [SignalFx](https://docs.microsoft.com/azure/active-directory/active-directory-saas-signalfx-tutorial), Assistant von FirstAgenda, [YardiOne](https://docs.microsoft.com/azure/active-directory/active-directory-saas-yardione-tutorial), Vtiger CRM, inwink, [Amplitude](https://docs.microsoft.com/azure/active-directory/active-directory-saas-amplitude-tutorial), [Spacio](https://docs.microsoft.com/azure/active-directory/active-directory-saas-spacio-tutorial), [ContractWorks](https://docs.microsoft.com/azure/active-directory/active-directory-saas-contractworks-tutorial), [Bersin](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bersin-tutorial), [Mercell](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mercell-tutorial), [Trisotech Digital Enterprise Server](https://docs.microsoft.com/azure/active-directory/active-directory-saas-trisotechdigitalenterpriseserver-tutorial), [Qumu Cloud](https://docs.microsoft.com/azure/active-directory/active-directory-saas-qumucloud-tutorial).
 
Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial).

Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 

---
 
### <a name="pim-for-azure-resources-is-generally-available"></a>PIM für Azure-Ressourcen ist allgemein verfügbar

**Typ:** Neue Funktion  
**Dienstkategorie**: Privileged Identity Management  
**Produktfunktion**: Privileged Identity Management
 
Wenn Sie Azure AD Privileged Identity Management für Verzeichnisrollen verwenden, können Sie jetzt die PIM-Funktionen für zeitgesteuerten Zugriff und Zuweisungen für Azure-Ressourcenrollen verwenden, wie etwa Abonnements, Ressourcengruppen, virtuelle Computer und beliebige andere vom Azure Resource Manager unterstützte Ressourcen. Erzwingen der Multi-Factor Authentication bei der Just-In-Time-Aktivierung von Rollen und Planen von Aktivierungen in Abstimmung mit genehmigten Änderungsfenstern. Darüber hinaus wurden in dieser Version Verbesserungen hinzugefügt, die in der öffentlichen Vorschau nicht verfügbar waren, darunter eine aktualisierte Benutzeroberfläche, Genehmigungsworkflows und die Möglichkeit zur Verlängerung bald ablaufender Rollen und zur Erneuerung abgelaufener Rollen.

Weitere Informationen finden Sie unter [PIM für Azure-Ressourcen (Vorschauversion)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).
 
---
 
### <a name="adding-optional-claims-to-your-apps-tokens-public-preview"></a>Hinzufügen von optionalen Ansprüchen zu App-Token (öffentliche Vorschau)

**Typ:** Neue Funktion  
**Dienstkategorie**: Authentifizierungen (Anmeldungen)  
**Produktfunktion**: Benutzerauthentifizierung
 
Ihre Azure AD-App kann jetzt benutzerdefinierte oder optionale Ansprüche in JWTs oder SAML-Token anfordern.  Dabei handelt es sich um den Benutzer oder Mandanten betreffende Ansprüche, die aufgrund von Größen- oder Anwendbarkeitseinschränkungen nicht standardmäßig im Token enthalten sind.  Diese Funktion befindet sich aktuell in der öffentlichen Vorschau für Azure AD-Apps v1.0- und v2.0-Endpunkten.  Informationen zu den Ansprüchen, die hinzugefügt werden können, und zum Bearbeiten Ihres Anwendungsmanifests für ihre Anforderung finden Sie in der Dokumentation.  

Weitere Informationen finden Sie unter [Optionale Ansprüche in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims) (Optionale Ansprüche in Azure AD).
 
---
 
### <a name="azure-ad-supports-pkce-for-more-secure-oauth-flows"></a>Azure AD unterstützt PKCE für sicherere OAuth-Flows

**Typ:** Neue Funktion  
**Dienstkategorie**: Authentifizierungen (Anmeldungen)  
**Produktfunktion**: Benutzerauthentifizierung
 
Die Azure AD-Dokumentation wurde um Unterstützung für PKCE ergänzt, das eine sicherere Kommunikation beim Gewährungsflow des OAuth 2.0-Autorisierungscodes ermöglicht.  An v1.0- und v2.0-Endpunkten werden sowohl S256- als auch Klartext-Abfragecodes unterstützt. 

Weitere Informationen finden Sie unter [Anfordern eines Autorisierungscodes](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols-oauth-code#request-an-authorization-code). 
 
---
 
### <a name="support-for-provisioning-all-user-attribute-values-available-in-the-workday-getworkers-api"></a>Unterstützung der Bereitstellung aller in der Workday Get_Workers-API verfügbaren Benutzerattributwerte

**Typ:** Neue Funktion  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion**: Integration von Drittanbieteranwendungen
 
Die öffentliche Vorschau der eingehenden Bereitstellung von Workday in Active Directory und Azure AD unterstützt jetzt die Funktion zum Extrahieren und Bereitstellen aller in der Workday Get_Workers-API verfügbaren Attributwerte. Dadurch wird jenseits der in der ursprünglichen Version des Workday-Connectors für eingehende Bereitstellung mitgelieferten Attribute Unterstützung für Hunderte weiterer Standard- und benutzerdefinierter Attribute hinzugefügt.

Weitere Informationen finden Sie unter: [Anpassen der Liste der Workday-Benutzerattribute](https://docs.microsoft.com/azure/active-directory/active-directory-saas-workday-inbound-tutorial#customizing-the-list-of-workday-user-attributes)

---

### <a name="changing-group-membership-from-dynamic-to-static-and-vice-versa"></a>Ändern der Gruppenmitgliedschaft von dynamisch in statisch (und umgekehrt)

**Typ:** Neue Funktion  
**Dienstkategorie**: Gruppenverwaltung  
**Produktfunktion:** Kollaboration
 
Sie können ändern, wie die Mitgliedschaft in einer Gruppe verwaltet wird. Dies ist hilfreich, wenn Sie im System den gleichen Gruppennamen und die dazugehörige ID beibehalten möchten, damit alle vorhandenen Verweise auf die Gruppe weiterhin gültig sind. Wenn eine neue Gruppe erstellt wird, müssen diese Verweise aktualisiert werden.
Wir haben das Azure AD Admin Center um Unterstützung für diese Funktionalität erweitert. Kunden können jetzt vorhandene Gruppen von dynamischer Mitgliedschaft auf zugewiesene Mitgliedschaft umstellen und umgekehrt. Die vorhandenen PowerShell-Cmdlets stehen ebenfalls weiterhin zur Verfügung.

Weitere Informationen finden Sie unter: [Ändern der dynamischen Mitgliedschaft in statisch (und umgekehrt)](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal#changing-dynamic-membership-to-static-and-vice-versa)

---

### <a name="improved-sign-out-behavior-with-seamless-sso"></a>Verbessertes Abmeldeverhalten bei nahtlosem einmaligem Anmelden

**Typ:** Geänderte Funktion  
**Dienstkategorie**: Authentifizierungen (Anmeldungen)  
**Produktfunktion**: Benutzerauthentifizierung
 
Selbst wenn sie sich explizit bei einer durch Azure AD geschützten Anwendung abgemeldet hatten, wurden Benutzer bei einem erneuten Zugriffsversuch auf eine Azure AD-Anwendung aus dem Unternehmensnetzwerk von in der Domäne registrierten Geräten aus bisher mithilfe von nahtlosem einmaligem Anmelden automatisch wieder angemeldet. Mit dieser Änderung wird die Abmeldung unterstützt.  Dadurch können Benutzer für die erneute Anmeldung das gleiche oder ein anderes Azure AD-Konto auswählen, statt mithilfe von nahtlosem Anmelden automatisch angemeldet zu werden.

Weitere Informationen finden Sie unter [Nahtlose einmalige Anmeldung mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso).
 
---
 
### <a name="application-proxy-connector-version-154020-released"></a>Anwendungsproxyconnector Version 1.5.402.0 veröffentlicht

**Typ:** Geänderte Funktion  
**Dienstkategorie:** App-Proxy  
**Produktfunktion**: Identitätssicherheit und -schutz
 
Das Rollout dieser Connectorversion erfolgt nach und nach im Lauf des Novembers. Diese neue Connectorversion enthält die folgenden Änderungen:

- Der Connector legt jetzt Cookies auf Domänen- statt auf Unterdomänenebene fest. Dadurch wird ein reibungsloseres Verhalten beim einmaligen Anmelden sichergestellt, und redundante Authentifizierungsaufforderungen werden vermieden.
- Unterstützung für aufgeteilte Codierungsanforderungen
- Verbesserte Connector-Integritätsüberwachung 
- Verschiedene behobene Programmfehler und verbesserte Stabilität

Weitere Informationen finden Sie unter [Grundlegendes zu Azure AD-Anwendungsproxyconnectors](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).
 
---

## <a name="february-2018"></a>Februar 2018
 
### <a name="improved-navigation-for-managing-users-and-groups"></a>Verbesserte Navigation zum Verwalten von Benutzern und Gruppen

**Typ:** Plan für Änderung  
**Dienstkategorie:** Verzeichnisverwaltung  
**Produktfunktion**: Verzeichnis

Die Navigationserfahrung zum Verwalten von Benutzern und Gruppen wurde optimiert. Sie können nun von der Verzeichnisübersicht aus direkt zur Liste aller Benutzer navigieren, um einfacher auf die Liste der gelöschten Benutzer zugreifen zu können. Sie können auch aus der Verzeichnisübersicht direkt zur Liste aller Gruppen navigieren, was den Zugriff auf die Einstellungen der Gruppenverwaltung erleichtert. Und von der Verzeichnisübersichtsseite aus können Sie außerdem nach einem Benutzer, einer Gruppe, einer Unternehmensanwendung oder einer App-Registrierung suchen. 

---

### <a name="availability-of-sign-ins-and-audit-reports-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet"></a>Verfügbarkeit von Anmeldungen und Überwachungsberichten in Microsoft Azure, betrieben von 21Vianet (Azure China 21Vianet)

**Typ:** Neue Funktion  
**Dienstkategorie**: Azure Stack  
**Produktfunktion:** Überwachung und Berichterstellung

Azure AD-Aktivitätsprotokollberichte sind nun in Microsoft Azure-Instanzen verfügbar, betrieben von 21Vianet (Azure China 21Vianet). Die folgenden Protokolle sind enthalten:

- **Aktivitätsprotokolle zu Anmeldevorgängen**: Umfasst alle Anmeldungsprotokolle, die Ihrem Mandanten zugeordnet sind.

- **Self-Service-Kennwortüberwachungsprotokolle**: Umfasst alle SSPR-Überwachungsprotokolle.

- **Verzeichnisverwaltungs-Überwachungsprotokolle**: Umfasst alle mit der Verzeichnisverwaltung zusammenhängenden Überwachungsprotokolle wie Benutzerverwaltung, App-Verwaltung und andere.

Mit diesen Protokollen können Sie Einblicke in den Zustand Ihrer Umgebung gewinnen. Die bereitgestellten Daten ermöglichen Ihnen Folgendes:

- Ermitteln, wie Ihre Apps und Dienste von Ihren Benutzern genutzt werden.

- Behandeln von Problemen, die Ihre Benutzer an der Erledigung ihrer Arbeit hindern.

Weitere Informationen zum Verwenden dieser Berichte finden Sie unter [Azure Active Directory-Berichterstellung](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal).

---

### <a name="use-report-reader-role-non-admin-role-to-view-azure-ad-activity-reports"></a>Verwenden der Rolle „Berichtsleser“ (Nicht-Administratorrolle) zum Anzeigen von Azure AD-Aktivitätsberichten

**Typ:** Neue Funktion  
**Dienstkategorie**: Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung

Als Teil des Kundenfeedbacks, Nicht-Administratorrollen den Zugriff auf die Azure AD-Aktivitätsprotokolle zu ermöglichen, haben wir eine Möglichkeit geschaffen, dass Benutzer, die die Rolle „Berichtsleser“ ausüben, auf die Anmelde- und Überwachungsaktivitäten im Azure-Portal sowie auf unsere Graph-APIs zugreifen können. 

Weitere Informationen zum Verwenden dieser Berichte finden Sie unter [Azure Active Directory-Berichterstellung](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal). 

---

### <a name="employeeid-claim-available-as-user-attribute-and-user-identifier"></a>EmployeeID-Anspruch als Benutzerattribut und Benutzer-ID verfügbar

**Typ:** Neue Funktion  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion**: SSO
 
Sie können **EmployeeID** als Benutzerbezeichner und Benutzerattribut für Mitgliederbenutzer und B2B-Gäste in SAML-basierten Anmeldeanwendungen von der Benutzeroberfläche der Unternehmensanwendung aus konfigurieren.

Weitere Informationen finden Sie unter [Anpassen ausgestellter Ansprüche im SAML-Token für Unternehmensanwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).

---

### <a name="simplified-application-management-using-wildcards-in-azure-ad-application-proxy"></a>Vereinfachte Anwendungsverwaltung mithilfe von Platzhaltern im Azure AD-Anwendungsproxy

**Typ:** Neue Funktion  
**Dienstkategorie:** App-Proxy  
**Produktfunktion**: Benutzerauthentifizierung
 
Um die Anwendungsbereitstellung zu vereinfachen und Ihren Verwaltungsaufwand zu verringern, unterstützen wir nun die Möglichkeit, Anwendungen mithilfe von Platzhaltern zu veröffentlichen. Um eine Platzhalteranwendung zu veröffentlichen, können Sie dem Standardablauf bei der Veröffentlichung von Anwendungen folgen, aber einen Platzhalter in den internen und externen URLs verwenden.

Weitere Informationen finden Sie unter [Platzhalteranwendungen im Azure Active Directory-Anwendungsproxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-wildcard)

---

### <a name="new-cmdlets-to-support-configuration-of-application-proxy"></a>Neue Cmdlets zur Unterstützung der Konfiguration des Anwendungsproxys

**Typ:** Neue Funktion  
**Dienstkategorie:** App-Proxy  
**Produktfunktion**: Plattform

Das neueste Release der Vorschau des Moduls AzureAD PowerShell Preview enthält neue Cmdlets, die es Kunden ermöglichen, Anwendungsproxyanwendungen mithilfe von PowerShell zu konfigurieren.

Die folgenden Cmdlets sind neu: 

- Get-AzureADApplicationProxyApplication
- Get-AzureADApplicationProxyApplicationConnectorGroup
- Get-AzureADApplicationProxyConnector
- Get-AzureADApplicationProxyConnectorGroup
- Get-AzureADApplicationProxyConnectorGroupMembers
- Get-AzureADApplicationProxyConnectorMemberOf
- New-AzureADApplicationProxyApplication
- New-AzureADApplicationProxyConnectorGroup
- Remove-AzureADApplicationProxyApplication
- Remove-AzureADApplicationProxyApplicationConnectorGroup
- Remove-AzureADApplicationProxyConnectorGroup
- Set-AzureADApplicationProxyApplication
- Set-AzureADApplicationProxyApplicationConnectorGroup
- Set-AzureADApplicationProxyApplicationCustomDomainCertificate
- Set-AzureADApplicationProxyApplicationSingleSignOn
- Set-AzureADApplicationProxyConnector
- Set-AzureADApplicationProxyConnectorGroup

---
 
### <a name="new-cmdlets-to-support-configuration-of-groups"></a>Neue Cmdlets zur Unterstützung der Konfiguration von Gruppen

**Typ:** Neue Funktion  
**Dienstkategorie:** App-Proxy  
**Produktfunktion**: Plattform

Das neueste Release des AzureAD PowerShell-Moduls enthält Cmdlets zum Verwalten von Gruppen in Azure AD. Diese Cmdlets waren zuvor im AzureADPreview-Modul verfügbar und werden nun dem AzureAD-Modul hinzugefügt.

Folgende Gruppen-Cmdlets werden nun für die allgemeine Verfügbarkeit freigegeben: 

- Get-AzureADMSGroup
- New-AzureADMSGroup
- Remove-AzureADMSGroup
- Set-AzureADMSGroup
- Get-AzureADMSGroupLifecyclePolicy
- New-AzureADMSGroupLifecyclePolicy
- Remove-AzureADMSGroupLifecyclePolicy
- Add-AzureADMSLifecyclePolicyGroup
- Remove-AzureADMSLifecyclePolicyGroup
- Reset-AzureADMSLifeCycleGroup   
- Get-AzureADMSLifecyclePolicyGroup

---
 
### <a name="a-new-release-of-azure-ad-connect-is-available"></a>Ein neues Release von Azure AD Connect ist verfügbar

**Typ:** Neue Funktion  
**Dienstkategorie**: AD Sync  
**Produktfunktion**: Plattform
 
Azure AD Connect ist das bevorzugte Tool zur Synchronisierung von Daten zwischen Azure AD und lokalen Datenquellen, einschließlich Windows Server Active Directory und LDAP.

>[!Important]
>Bei diesem Build wurden Änderungen an Schemas und Synchronisierungsregeln vorgenommen. Der Azure AD Connect-Synchronisierungsdienst löst nach einem Upgrade die Schritte „Vollständiger Import“ und „Vollständige Synchronisierung“ aus. Informationen zum Ändern dieses Verhaltens finden Sie unter [Zurückstellen der vollständigen Synchronisierung nach dem Upgrade](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version#how-to-defer-full-synchronization-after-upgrade).

Dieses Release enthält die folgenden Updates und Änderungen:

**Behobene Probleme**

- Es wurde ein Problem mit dem Zeitfenster für Hintergrundaufgaben auf der Seite „Partitionsfilterung“ behoben, das beim Wechsel auf die nächste Seite entstand.

- Es wurde ein Problem behoben, das während einer benutzerdefinierten ConfigDB-Aktion zu einer Zugriffsverletzung führte.

- Es wurde ein Problem mit der Wiederherstellung nach einem SQL-Verbindungstimeout behoben.

- Es wurde ein Problem behoben, bei dem Zertifikate mit SAN-Platzhaltern eine Voraussetzungsprüfung nicht bestanden.

- Es wurde ein Problem behoben, das zu einem Absturz von „miiserver.exe“ während eines AAD-Connectorexports führte.

- Es wurde ein Problem mit ungültigen Kennworteingabeversuchen behoben, die beim Ausführen des AAD Connect-Assistenten zum Ändern der Konfiguration auf dem Domänencontroller protokolliert wurden

**Neue Features und Verbesserungen**
 
- Anwendungstelemetrie: Administratoren können diese Datenklasse aktivieren/deaktivieren.

- Azure AD-Integritätsdaten: Administratoren müssen das Integritätsportal öffnen, um die Integritätseinstellungen zu steuern. Sobald die Dienstrichtlinie geändert wurde, wird sie von den Agents gelesen und erzwungen.

- Zusätzliche Aktionen zum Konfigurieren des Geräterückschreibens und eine Statusanzeige für die Seiteninitialisierung.

- Verbesserte allgemeine Diagnose mit HTML-Berichten und vollständiger Datenerfassung in einem ZIP-Text- bzw. HTML-Bericht.

- Die Zuverlässigkeit des automatischen Upgrades wurde verbessert, und es wurden zusätzliche Telemetriedaten hinzugefügt, um sicherzustellen, dass die Integrität des Servers ermittelt werden kann.

- Eingeschränkte Berechtigungen für privilegierte Konten im AD Connector-Konto. Bei Neuinstallationen schränkt der Assistent die Berechtigungen ein, über die privilegierte Konten im MSOL-Konto verfügen, nachdem das MSOL-Konto erstellt wurde. Die Änderungen betreffen Express-Installationen und benutzerdefinierte Installationen mit automatischer Kontoerstellung.

- Das Installationsprogramm wurde so geändert, dass bei einer Neuinstallation von AAD Connect keine SA-Berechtigung mehr erforderlich ist.

- Neues Hilfsprogramm, um Synchronisierungsprobleme für ein bestimmtes Objekt zu beheben. Zurzeit überprüft das Hilfsprogramm Folgendes:

    - Diskrepanz beim UserPrincipalName zwischen dem synchronisierten Benutzerobjekt und dem Benutzerkonto im Azure AD-Mandanten.
  
    - Ausschluss des Objekts von der Synchronisierung aufgrund der Domänenfilterung.
  
    - Ausschluss des Objekts von der Synchronisierung aufgrund der Filterung nach Organisationseinheit.

- Neues Hilfsprogramm, um den aktuellen Kennworthash zu synchronisieren, der im lokalen Active Directory-Verzeichnis für ein bestimmtes Benutzerkonto gespeichert ist. Das Hilfsprogramm erfordert keine Kennwortänderung. 

---
 
### <a name="applications-supporting-intune-app-protection-policies-added-for-use-with-azure-ad-application-based-conditional-access"></a>Anwendungen, die die Intune App-Schutzrichtlinien unterstützen, wurden für die Verwendung mit anwendungsbasiertem bedingten Azure AD-Zugriff hinzugefügt

**Typ:** Geänderte Funktion  
**Dienstkategorie**: Bedingter Zugriff  
**Produktfunktion**: Identitätssicherheit und -schutz

Wir haben weitere Anwendungen hinzugefügt, die anwendungsbasierten bedingten Zugriff unterstützen. Jetzt können Sie Zugriff auf Office 365 und andere mit Azure AD verbundene Cloudanwendungen erhalten, indem Sie diese genehmigten Client-Apps verwenden.

Die folgenden Anwendungen werden bis Ende Februar hinzugefügt:

- Microsoft Power BI

- Microsoft Launcher

- Microsoft Invoicing

Weitere Informationen finden Sie unter

- [Genehmigte Client-App als Voraussetzung](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [App-basierter bedingter Zugriff mit Azure AD](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="terms-of-use-update-to-mobile-experience"></a>Update der Nutzungsbedingungen für Mobilgeräte 

**Typ:** Geänderte Funktion  
**Dienstkategorie**: Nutzungsbedingungen  
**Produktfunktion:** Konformität

Wenn die Nutzungsbedingungen angezeigt werden, können Sie nun auf Folgendes klicken: **Probleme mit der Anzeige? Klicken Sie hier**. Durch Klicken auf diesen Link werden die Nutzungsbedingungen nativ auf Ihrem Gerät geöffnet. Unabhängig vom Schriftgrad des Dokuments oder der Bildschirmgröße des Geräts können Sie das Dokument bei Bedarf zoomen und lesen. 

---
 
## <a name="january-2018"></a>Januar 2018
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar 

**Typ:** Neue Funktion  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion**: Integration von Drittanbieteranwendungen

Im Januar 2018 wurden die folgenden neuen Apps mit Verbundunterstützung im App-Katalog hinzugefügt:

[IBM OpenPages](https://go.microsoft.com/fwlink/?linkid=864698), [OneTrust Privacy Management Software](https://go.microsoft.com/fwlink/?linkid=861660), [Dealpath](https://go.microsoft.com/fwlink/?linkid=863526), IriusRisk Federated Directory und [Fidelity NetBenefits](https://go.microsoft.com/fwlink/?linkid=864701).

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial).

Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 

---
 
### <a name="sign-in-with-additional-risk-detected"></a>Anmeldung mit erhöhtem Risiko erkannt

**Typ:** Neue Funktion  
**Dienstkategorie**: Identitätsschutz  
**Produktfunktion**: Identitätssicherheit und -schutz

Welche Informationen Sie zu einem erkannten Risikoereignis erhalten, richtet sich nach Ihrem Azure AD-Abonnement. Die Azure AD Premium P2-Edition bietet die ausführlichsten Informationen zu allen zugrunde liegenden erkannten Ereignissen.

Mit der Azure AD Premium P1-Edition werden erkannte Ereignisse, die nicht durch Ihre Lizenz abgedeckt sind, als das Risikoereignis „Anmeldung mit erhöhtem Risiko erkannt“ angezeigt.

Weitere Informationen finden Sie unter [Azure Active Directory-Risikoereignisse](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-risk-events).
 
---

### <a name="hide-office-365-applications-from-end-users-access-panels"></a>Ausblenden von Office 365-Anwendungen in Zugriffsbereichen von Endbenutzern

**Typ:** Neue Funktion  
**Dienstkategorie**: Meine Apps  
**Produktfunktion**: SSO

Durch eine neue Benutzereinstellung können Sie jetzt besser steuern, wie Office 365-Anwendungen in den Zugriffsbereichen Ihrer Benutzer angezeigt werden. Diese Option ist hilfreich, um die Anzahl der Apps in den Zugriffsbereichen eines Benutzers zu reduzieren, sofern nur Office-Apps im Office-Portal anzeigen werden sollen. Die Einstellung befindet sich in den **Benutzereinstellungen** und lautet **Benutzer können Office 365-Apps nur im Office 365-Portal anzeigen**.

Weitere Informationen finden Sie unter [Ausblenden einer Anwendung auf der Benutzeroberfläche in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app).

---
 
### <a name="seamless-sign-into-apps-enabled-for-password-sso-directly-from-apps-url"></a>Nahtloses Anmelden bei Apps mit direkt über die App-URL aktiviertem Kennwort-SSO 

**Typ:** Neue Funktion  
**Dienstkategorie**: Meine Apps  
**Produktfunktion**: SSO

Die Browsererweiterung „Meine Apps“ ist jetzt über ein nützliches Tool verfügbar, mit dem Sie die Funktion zum einmaligen Anmelden „Meine Apps“ als Verknüpfung in Ihrem Browser verwenden können. Nach der Installation wird dem Benutzer ein Waffelsymbol im Browser angezeigt, das Schnellzugriff auf Apps bietet. Benutzer können ab sofort von folgenden Vorteilen profitieren:

- Die Möglichkeit, sich direkt bei Apps per Kennwort-SSO über die Anmeldeseite der App anzumelden
- Starten einer App mithilfe der Schnellsuchfunktion
- Verknüpfungen für zuletzt verwendete Apps über die Erweiterung
- Die Erweiterung ist für Microsoft Edge, Chrome und Firefox verfügbar.
 
Weitere Informationen finden Sie unter [Erweiterung zur sicheren Anmeldung bei „Meine Apps“](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction#my-apps-secure-sign-in-extension).

---

### <a name="azure-ad-administration-experience-in-azure-classic-portal-has-been-retired"></a>Die Azure AD-Verwaltungsoberfläche im klassischen Azure-Portal wurde eingestellt

**Typ**: Veraltet   
**Dienstkategorie**: Azure AD  
**Produktfunktion**: Verzeichnis

Die Azure AD-Administratoroberfläche im klassischen Azure-Portal ist seit dem 8. Januar 2018 eingestellt. Diese Änderung fand im Zuge der Einstellung des klassischen Azure-Portals selbst statt. Für sämtliche portalbasierte Verwaltungsaufgaben von Azure AD müssen Sie künftig das [Azure AD Admin Center](https://aad.portal.azure.com) verwenden.
 
---

### <a name="the-phonefactor-web-portal-has-been-retired"></a>Das PhoneFactor-Webportal wurde eingestellt

**Typ**: Veraltet  
**Dienstkategorie**: Azure AD  
**Produktfunktion**: Verzeichnis
 
Am 8. Januar 2018 wurde das PhoneFactor-Webportal eingestellt. Die Funktionen des Portals, nämlich die Verwaltung des MFA-Servers, wurden zum Azure-Portal unter portal.azure.com migriert. 

Die MFA-Konfiguration befindet sich unter **Azure Active Directory \> MFA-Server**.
 
---
 
### <a name="deprecate-azure-ad-reports"></a>Einstellung von Azure AD-Berichten

**Typ**: Veraltet  
**Dienstkategorie**: Berichterstellung  
**Produktfunktion**: Verwaltung des Identitätslebenszyklus  


Im Zuge der allgemeinen Verfügbarkeit der neuen Azure Active Directory-Verwaltungskonsole und der neuen APIs, die ab sofort für Aktivitäts- und Sicherheitsberichte verfügbar sind, wurden die Berichts-APIs auf dem Endpunkt unter „/reports“ am 31. Dezember 2017 eingestellt.

**Was ist verfügbar?**

Im Rahmen der Umstellung auf die neue Verwaltungskonsole haben wir zwei neue APIs zum Abrufen von Azure AD-Aktivitätsprotokollen zur Verfügung gestellt. Die neuen APIs bieten umfangreichere Filter- und Sortierfunktionen sowie mehr Überprüfungs- und Anmeldeaktivitäten. Die Daten, die bisher in den Sicherheitsberichten verfügbar waren, können jetzt über die API für Identity Protection-Risikoereignisse in Microsoft Graph abgerufen werden.

Weitere Informationen finden Sie unter

- [Erste Schritte mit der Berichterstellungs-API von Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal)

- [Erste Schritte mit Azure Active Directory Identity Protection und Microsoft Graph](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-graph-getting-started)

---

## <a name="december-2017"></a>Dezember 2017

### <a name="terms-of-use-in-the-access-panel"></a>Nutzungsbedingungen im Zugriffsbereich

**Typ:** Neue Funktion  
**Dienstkategorie**: Nutzungsbedingungen  
**Produktfunktion:** Konformität
 
Ab sofort können Sie die von Ihnen zuvor akzeptierten Nutzungsbedingungen im Zugriffsbereich einsehen.

Folgen Sie diesen Schritten:

1. Navigieren Sie zum [MyApps-Portal](https://myapps.microsoft.com), und melden Sie sich an.

2. Wählen Sie oben rechts Ihren Namen und dann in der Liste **Profil** aus. 

3. Klicken Sie in Ihrem **Profil** auf **Nutzungsbedingungen lesen**. 

4. Nun können Sie sich die akzeptierten Nutzungsbedingungen ansehen. 

Weitere Informationen finden Sie unter [Nutzungsbedingungsfeature (Vorschauversion) für Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-tou).
 
---
 
### <a name="new-azure-ad-sign-in-experience"></a>Neuer Anmeldevorgang für Azure AD

**Typ:** Neue Funktion  
**Dienstkategorie**: Azure AD  
**Produktfunktion**: Benutzerauthentifizierung
 
Die Benutzeroberfläche des Azure AD- und Microsoft-Kontoidentitätssystems wurden hinsichtlich eines konsistenten Aussehens und Verhaltens neu gestaltet. Darüber hinaus erfasst die Azure AD-Anmeldeseite zuerst den Benutzernamen, gefolgt von den Anmeldeinformationen auf einem zweiten Bildschirm.

Weitere Informationen finden Sie unter [The new Azure AD Signin Experience is now in Public Preview](https://cloudblogs.microsoft.com/enterprisemobility/2017/08/02/the-new-azure-ad-signin-experience-is-now-in-public-preview/) (Die neue Umgebung für die Azure AD-Anmeldung ist jetzt als Public Preview verfügbar).
 
---
 
### <a name="fewer-sign-in-prompts-a-new-keep-me-signed-in-experience-for-azure-ad-sign-in"></a>Weniger Anmeldeeingabeaufforderungen: eine neue „Angemeldet bleiben“-Umgebung für die Azure AD-Anmeldung

**Typ:** Neue Funktion  
**Dienstkategorie**: Azure AD  
**Produktfunktion**: Benutzerauthentifizierung
 
Das Kontrollkästchen **Angemeldet bleiben** auf der Azure AD-Anmeldeseite wurde durch eine neue Eingabeaufforderung ersetzt, die nach erfolgreicher Authentifizierung angezeigt wird. 

Wenn Sie für diese Eingabeaufforderung **Ja** auswählen, erhalten Sie vom Dienst ein beständiges Aktualisierungstoken. Dies ist das gleiche Verhalten wie beim Aktivieren des Kontrollkästchens **Angemeldet bleiben** in der alten Umgebung. Bei Verbundmandanten wird diese Eingabeaufforderung angezeigt, nachdem Sie sich erfolgreich bei dem Verbunddienst authentifiziert haben.

Weitere Informationen finden Sie unter [Fewer login prompts: The new “Keep me signed in” experience for Azure AD is in preview](https://cloudblogs.microsoft.com/enterprisemobility/2017/09/19/fewer-login-prompts-the-new-keep-me-signed-in-experience-for-azure-ad-is-in-preview/) (Weniger Anmeldeeingabeaufforderungen: Die neue „Angemeldet bleiben“-Umgebung für Azure AD befindet sich in der Vorschauphase). 

---

### <a name="add-configuration-to-require-the-terms-of-use-to-be-expanded-prior-to-accepting"></a>Hinzufügen einer Konfiguration, um vor dem Akzeptieren das Erweitern der Nutzungsbedingungen zu erfordern

**Typ:** Neue Funktion  
**Dienstkategorie**: Nutzungsbedingungen  
**Produktfunktion:** Konformität
 
Eine Option für Administratoren wurde hinzugefügt, mit der Benutzer aufgefordert werden können, die Nutzungsbedingungen vor dem Akzeptieren zu erweitern.

Wählen Sie **Ein** oder **Aus** aus, um die Erweiterung der Nutzungsbedingungen durch den Benutzer zu erfordern. Bei der Einstellung **Ein** wird vorausgesetzt, dass sich Benutzer diese vor dem Akzeptieren der Nutzungsbedingungen ansehen.

Weitere Informationen finden Sie unter [Nutzungsbedingungsfeature (Vorschauversion) für Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-tou).
 
---

### <a name="scoped-activation-for-eligible-role-assignments"></a>Bereichsbezogene Aktivierung für berechtigte Rollenzuweisungen

**Typ:** Neue Funktion  
**Dienstkategorie**: Privileged Identity Management  
**Produktfunktion**: Privileged Identity Management
 
Mit der bereichsbezogenen Aktivierung können Sie berechtigte Rollenzuweisungen für Azure-Ressourcen mit weniger Autonomie aktivieren, als die ursprünglichen Standardwerte für die Zuweisung vorsehen. Beispielsweise wird Ihnen „Besitzer“ für ein Abonnement in Ihrem Mandanten zugewiesen. Mit der bereichsbezogenen Aktivierung können Sie die Besitzerrolle für bis zu fünf innerhalb des Abonnements enthaltene Ressourcen aktivieren (z.B. für Ressourcengruppen und virtuelle Computer). Durch Festlegen des Gültigkeitsbereichs der Aktivierung kann die Wahrscheinlichkeit verringert werden, dass unerwünschte Änderungen an wichtigen Azure-Ressourcen vorgenommen werden.

Weitere Informationen finden Sie unter [Was ist Azure AD Privileged Identity Management?](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure).
 
---
 
### <a name="new-federated-apps-in-the-azure-ad-app-gallery"></a>Neue Verbund-Apps im Azure AD-App-Katalog

**Typ:** Neue Funktion  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion**: Integration von Drittanbieteranwendungen

Im Dezember 2017 haben wir diese neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Accredible](https://go.microsoft.com/fwlink/?linkid=863523), Adobe Experience Manager, [EFI Digital StoreFront](https://go.microsoft.com/fwlink/?linkid=861685), [Communifire](https://go.microsoft.com/fwlink/?linkid=861676) CybSafe, [FactSet](https://go.microsoft.com/fwlink/?linkid=863525), [IMAGE WORKS](https://go.microsoft.com/fwlink/?linkid=863517), [MOBI](https://go.microsoft.com/fwlink/?linkid=863521), [MobileIron Azure AD-Integration](https://go.microsoft.com/fwlink/?linkid=858027), [Reflektive](https://go.microsoft.com/fwlink/?linkid=863518), [SAML SSO for Bamboo by resolution GmbH](https://go.microsoft.com/fwlink/?linkid=863520), [SAML SSO for Bitbucket by resolution GmbH](https://go.microsoft.com/fwlink/?linkid=863519), [Vodeclic](https://go.microsoft.com/fwlink/?linkid=863522), WebHR, Zenegy Azure AD-Integration.

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial).

Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 
 
---

### <a name="approval-workflows-for-azure-ad-directory-roles"></a>Genehmigungsworkflows für Azure AD-Verzeichnisrollen

**Typ:** Geänderte Funktion  
**Dienstkategorie**: Privileged Identity Management  
**Produktfunktion**: Privileged Identity Management
 
Ein Genehmigungsworkflow für Azure AD-Verzeichnisrollen ist allgemein verfügbar.

Mit dem Genehmigungsworkflow können Administratoren für privilegierte Rollen festlegen, dass berechtigte Rollenmitglieder die Rollenaktivierung anfordern müssen, bevor sie die privilegierte Rolle verwenden können. Es können Genehmigungsverantwortlichkeiten an verschiedene Benutzer und Gruppen delegiert werden. Berechtigte Rollenmitglieder erhalten eine Benachrichtigung, wenn die Genehmigung abgeschlossen und ihre Rolle aktiv ist.

---
 
### <a name="pass-through-authentication-skype-for-business-support"></a>Pass-Through-Authentifizierung: Unterstützung von Skype for Business

**Typ:** Geänderte Funktion  
**Dienstkategorie**: Authentifizierungen (Anmeldungen)  
**Produktfunktion**: Benutzerauthentifizierung

Die Pass-Through-Authentifizierung unterstützt jetzt Benutzeranmeldungen bei Skype for Business-Clientanwendungen, die die moderne Authentifizierung, einschließlich Online- und Hybridtopologien, unterstützen. 

Weitere Informationen finden Sie unter [Mit moderner Authentifizierung unterstützte Skype for Business-Topologien](https://technet.microsoft.com/library/mt803262.aspx).
 
---

### <a name="updates-to-azure-ad-privileged-identity-management-for-azure-rbac-preview"></a>Updates für die Azure RBAC in Azure AD Privileged Identity Management (Vorschauversion)

**Typ:** Geänderte Funktion  
**Dienstkategorie**: Privileged Identity Management  
**Produktfunktion**: Privileged Identity Management
 
Mit der aktualisierten Public Preview von Azure AD Privileged Identity Management (PIM) für die rollenbasierte Zugriffssteuerung in Azure (RBAC) haben Sie nun folgende Möglichkeiten:

* Nicht mehr Verwaltung als nötig
* Erfordern der Genehmigung zum Aktivieren von Ressourcenrollen
* Planen einer zukünftigen Rollenaktivierung, die eine Genehmigung für Azure AD- und Azure RBAC-Rollen erfordert
 
Weitere Informationen finden Sie unter [PIM für Azure-Ressourcen (Vorschauversion)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

---
 
## <a name="november-2017"></a>November 2017
 
### <a name="access-control-service-retirement"></a>Einstellung des Access Control Service

**Typ:** Plan für Änderung  
**Dienstkategorie**: Access Control Service  
**Produktfunktion**: Access Control Service 

Der Azure Active Directory Access Control (auch als Access Control Service bezeichnet) wird Ende 2018 eingestellt. Weitere Informationen, u.a. ein ausführlicher Zeitplan und allgemeine Migrationsanweisungen, werden in den nächsten Wochen bereitgestellt. Auf dieser Seite können Sie uns Ihre Kommentare oder Fragen zum Access Control Service mitteilen. Ein Teammitglied wird diese dann beantworten.

---

### <a name="restrict-browser-access-to-the-intune-managed-browser"></a>Beschränkung des Browserzugriffs auf Intune Managed Browser 

**Typ:** Plan für Änderung  
**Dienstkategorie**: Bedingter Zugriff  
**Produktfunktion**: Identitätssicherheit und -schutz

Sie können den Browserzugriff auf Office 365 und weitere mit Azure AD verbundene Cloud-Apps mithilfe von Intune Managed Browser als genehmigte App einschränken. 

Ab sofort können Sie folgende Bedingung für den anwendungsbasierten bedingten Zugriff konfigurieren:

**Client-Apps:** Browser

**Welche Auswirkung hat die Änderung?**

Der Zugriff wird blockiert, wenn Sie diese Bedingung verwenden. Sobald die Vorschauversion verfügbar ist, ist für den gesamten Zugriff die Verwendung der Managed Browser-Anwendung erforderlich. 

Achten Sie in kommenden Blogbeiträgen und Anmerkungen zu dieser Version auf Informationen zu dieser Funktionalität. 

Weitere Informationen finden Sie unter [Bedingter Zugriff in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).
 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Neue genehmigte Client-Apps für den App-basierten bedingten Zugriff mit Azure AD

**Typ:** Plan für Änderung  
**Dienstkategorie**: Bedingter Zugriff  
**Produktfunktion**: Identitätssicherheit und -schutz

Die folgenden Apps wurden der Liste der [genehmigten Client-Apps](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement) hinzugefügt:

- [Microsoft Kaizala](https://www.microsoft.com/garage/profiles/kaizala/)
- [Microsoft StaffHub](https://staffhub.office.com/what-it-is)

Weitere Informationen finden Sie unter

- [Genehmigte Client-App als Voraussetzung](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [App-basierter bedingter Zugriff mit Azure AD](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="terms-of-use-support-for-multiple-languages"></a>Unterstützung von Nutzungsbedingungen für mehrere Sprachen

**Typ:** Neue Funktion    
**Dienstkategorie**: Nutzungsbedingungen  
**Produktfunktion:** Konformität

Administratoren können jetzt neue Nutzungsbedingungen erstellen, die mehrere PDF-Dokumente enthalten. Diese PDF-Dateien können mit einer zugehörigen Sprache gekennzeichnet werden. Benutzern wird dann die PDF-Datei angezeigt, die der Sprache in den Einstellungen entspricht. Falls keine Übereinstimmung vorliegt, wird die Standardsprache angezeigt.

---
 
### <a name="real-time-password-writeback-client-status"></a>Echtzeitclientstatus beim Kennwortrückschreiben

**Typ:** Neue Funktion  
**Dienstkategorie**: Self-Service-Kennwortzurücksetzung  
**Produktfunktion**: Benutzerauthentifizierung

Nun können Sie den Status Ihres lokalen Clients für das Kennwortrückschreiben anzeigen. Diese Option steht im Abschnitt **Lokale Integration** auf der Seite [Kennwortzurücksetzung](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset) zur Verfügung. 

Wenn Verbindungsprobleme bei Ihrem lokalen Client für das Kennwortrückschreiben vorliegen, wird eine Fehlermeldung mit folgenden Informationen angezeigt:

- Informationen dazu, warum keine Verbindung mit dem lokalen Client für das Kennwortrückschreiben möglich ist
- Ein Link zur Dokumentation, die Sie bei der Problembehandlung unterstützt 

Weitere Informationen finden Sie unter [Lokale Integration](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-how-it-works#on-premises-integration).

---

### <a name="azure-ad-app-based-conditional-access"></a>App-basierter bedingter Zugriff mit Azure AD 
 
**Typ:** Neue Funktion  
**Dienstkategorie**: Azure AD  
**Produktfunktion**: Identitätssicherheit und -schutz

Sie können jetzt über den [App-basierten bedingten Zugriff mit Azure AD](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access) den Zugriff auf Office 365 und weitere mit Azure AD verbundene Cloud-Apps auf [genehmigte Client-Apps](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement) beschränken, die Intune-App-Schutzrichtlinien unterstützen. Intune-App-Schutzrichtlinien werden verwendet, um Unternehmensdaten in diesen Clientanwendungen zu konfigurieren und zu schützen.

Durch das Kombinieren von [App-basierten](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access) und [gerätebasierten](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-policy-connected-applications) Richtlinien für den bedingten Zugriff können Sie Daten für private Geräte und Unternehmensgeräte flexibel schützen.

Für den App-basierten bedingten Zugriff stehen ab sofort die folgenden Bedingungen und Steuerelemente zur Verfügung:

**Bedingung für unterstützte Plattform**

- iOS
- Android

**Client-Apps-Bedingung**

- Mobile Apps und Desktop-Apps

**Zugriffssteuerung**

- Genehmigte Client-App erforderlich

Weitere Informationen finden Sie unter [App-basierter bedingter Zugriff mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access).
 
---

### <a name="manage-azure-ad-devices-in-the-azure-portal"></a>Verwalten von Azure AD-Geräten im Azure-Portal

**Typ:** Neue Funktion  
**Dienstkategorie**: Geräteregistrierung und -verwaltung  
**Produktfunktion**: Identitätssicherheit und -schutz

Ab jetzt finden Sie alle mit Azure AD verbundenen Geräte sowie die gerätebezogenen Aktivitäten an einem Ort. Es steht eine neue Verwaltungsoberfläche zur Verfügung, mit der Sie all ihre Geräteidentitäten und -einstellungen im Azure-Portal verwalten können. Dieses Release ermöglicht Folgendes:

- Anzeigen all Ihrer Geräte, die für den bedingten Zugriff in Azure AD zur Verfügung stehen
- Anzeigen von Eigenschaften, einschließlich in Hybrid-Azure AD eingebundenen Geräten
- Auffinden von BitLocker-Schlüsseln für Ihre in Azure AD eingebundenen Geräte, Verwalten Ihrer Geräte mit Intune und vieles mehr
- Verwalten von gerätebezogenen Azure AD-Einstellungen

Weitere Informationen finden Sie unter [Verwalten von Geräten mit dem Azure-Portal](https://docs.microsoft.com/azure/active-directory/device-management-azure-portal).

---

### <a name="support-for-macos-as-a-device-platform-for-azure-ad-conditional-access"></a>Unterstützung der macOS-Geräteplattform für den bedingten Azure AD-Zugriff 

**Typ:** Neue Funktion    
**Dienstkategorie**: Bedingter Zugriff  
**Produktfunktion**: Identitätssicherheit und -schutz 

Ab sofort können Sie macOS als Geräteplattformbedingung in Ihre Richtlinie für den bedingten Azure AD-Zugriff einschließen (oder ausschließen). Durch das Hinzufügen von macOS als unterstützte Geräteplattform haben Sie folgende Möglichkeiten:

- **Registrieren und Verwalten von macOS-Geräten mit Intune**: Ähnlich wie für andere Plattformen (z.B. iOS und Android) steht für macOS eine Unternehmensportalanwendung bereit, um einheitliche Registrierungen durchzuführen. Sie können ein Gerät mithilfe der neuen Unternehmensportal-App für macOS bei Intune und Azure AD registrieren.
- **Sicherstellen, dass macOS-Geräte die in Intune definierten Konformitätsrichtlinien Ihrer Organisation einhalten**: In Intune im Azure-Portal können Sie nun Konformitätsrichtlinien für macOS-Geräte einrichten. 
- **Beschränken des Zugriffs auf Anwendungen in Azure AD auf ausschließlich konforme macOS-Geräte**: Für die Erstellung bedingter Zugriffsrichtlinien ist macOS als separate Geräteplattformoption festgelegt. Nun können Sie macOS-spezifische Richtlinien für den bedingten Zugriff für die festgelegte Zielanwendung in Azure erstellen.

Weitere Informationen finden Sie unter

- [Erstellen einer Gerätekonformitätsrichtlinie für macOS-Geräte mit Intune](https://aka.ms/macoscompliancepolicy)
- [Bedingter Zugriff in Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
 
---

### <a name="network-policy-server-extension-for-azure-multi-factor-authentication"></a>Erweiterung „Netzwerkrichtlinienserver“ für Azure Multi-Factor Authentication 

**Typ:** Neue Funktion    
**Dienstkategorie:** Multi-Factor Authentication  
**Produktfunktion**: Benutzerauthentifizierung

Die Erweiterung „Netzwerkrichtlinienserver“ für Azure Multi-Factor Authentication fügt Ihrer Authentifizierungsinfrastruktur unter Verwendung Ihrer vorhandenen Server cloudbasierte Funktionen zur mehrstufigen Authentifizierung hinzu. Mit der Erweiterung „Netzwerkrichtlinienserver“ können Sie zu Ihrem bestehenden Authentifizierungsablauf eine Überprüfung per Telefonanruf, SMS oder Telefon-App hinzufügen. Dabei müssen keine neuen Server installiert, konfiguriert und verwaltet werden. 

Diese Erweiterung wurde für Organisationen entwickelt, die VPN-Verbindungen schützen möchten, ohne den Azure Multi-Factor Authentication-Server bereitzustellen. Die Erweiterung „Netzwerkrichtlinienserver“ fungiert als Adapter zwischen RADIUS und cloudbasierter Azure Multi-Factor Authentication, um eine zweite Stufe der Authentifizierung für Verbund- oder synchronisierte Benutzer bereitzustellen.

Weitere Informationen finden Sie unter [Integrieren Ihrer vorhandenen NPS-Infrastruktur in Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-nps-extension).
 
---

### <a name="restore-or-permanently-remove-deleted-users"></a>Wiederherstellen oder dauerhaftes Entfernen gelöschter Benutzer

**Typ:** Neue Funktion    
**Dienstkategorie**: Benutzerverwaltung  
**Produktfunktion**: Verzeichnis 

Im Azure AD Admin Center haben Sie jetzt folgende Möglichkeiten:

- Wiederherstellen eines gelöschten Benutzers 
- Dauerhaftes Löschen eines Benutzers

**So können Sie es ausprobieren**

1. Wählen Sie im Azure AD Admin Center im Abschnitt **Verwalten** die Option [Alle Benutzer](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All) aus. 

2. Wählen Sie in der Liste **Anzeigen** die Option **Kürzlich gelöschte Benutzer** aus. 

3. Wählen Sie mindestens einen kürzlich gelöschten Benutzer aus, und stellen Sie den Benutzer dann entweder wieder her, oder löschen Sie ihn dauerhaft.
 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Neue genehmigte Client-Apps für den App-basierten bedingten Zugriff mit Azure AD
 
**Typ:** Geänderte Funktion  
**Dienstkategorie**: Bedingter Zugriff  
**Produktfunktion**: Identitätssicherheit und -schutz

Die folgenden Apps wurden der Liste der [genehmigten Client-Apps](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement) hinzugefügt:

- Microsoft Planner
- Azure Information Protection 

Weitere Informationen finden Sie unter

- [Genehmigte Client-App als Voraussetzung](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [App-basierter bedingter Zugriff mit Azure AD](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="use-or-between-controls-in-a-conditional-access-policy"></a>Einfügen von „OR“ zwischen Steuerelementen in einer Richtlinie für den bedingten Zugriff 

**Typ:** Geänderte Funktion    
**Dienstkategorie**: Bedingter Zugriff  
**Produktfunktion**: Identitätssicherheit und -schutz
 
Ab sofort können Sie „OR“ (eines der ausgewählten Steuerelemente ist erforderlich) zwischen Steuerelementen für den bedingten Zugriff einfügen. Durch dieses Feature können Sie Richtlinien mit einem „OR“ zwischen Steuerelementen für den Zugriff erstellen. Beispielsweise können Sie mit diesem Feature eine Richtlinie erstellen, sodass der Benutzer sich entweder über die Multi-Factor Authentication anmelden „OR“ (oder) ein konformes Gerät verwenden muss.

Weitere Informationen finden Sie unter [Steuerelemente beim bedingten Zugriff mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-controls).
 
---

### <a name="aggregation-of-real-time-risk-events"></a>Aggregation von Echtzeitrisikoereignissen

**Typ:** Geänderte Funktion    
**Dienstkategorie**: Identitätsschutz  
**Produktfunktion**: Identitätssicherheit und -schutz

Ab sofort werden jetzt in Azure AD Identity Protection alle Echtzeitrisikoereignisse, die von derselben IP-Adresse stammen, für einen vorgegebenen Tag für jeden Risikoereignistyp aggregiert. Durch diese Änderung werden weniger Risikoereignisse ohne Änderung der Benutzersicherheit angezeigt.

Die zugrunde liegende Echtzeiterkennung erfolgt bei jeder Benutzeranmeldung. Wenn Sie eine Sicherheitsrichtlinie für das Anmelderisiko auf Multi-Factor Authentication oder eine Blockierung des Zugriffs festgelegt haben, wird diese bei jeder riskanten Anmeldung weiterhin ausgelöst.
 
---
 
## <a name="october-2017"></a>Oktober 2017

### <a name="deprecate-azure-ad-reports"></a>Einstellung von Azure AD-Berichten

**Typ:** Plan für Änderung  
**Dienstkategorie**: Berichterstellung  
**Produktfunktion**: Verwaltung des Identitätslebenszyklus  

Azure-Portal bietet Ihnen Folgendes:

- Eine neue Azure AD-Verwaltungskonsole
- Neue APIs für Aktivitäts- und Sicherheitsberichte
 
Aufgrund dieser neuen Funktionen werden die Berichts-APIs auf dem Endpunkt unter „/reports“ am 10. Dezember 2017 eingestellt. 

---

### <a name="automatic-sign-in-field-detection"></a>Automatische Erkennung von Anmeldefeldern

**Typ:** Korrigiert   
**Dienstkategorie**: Meine Apps  
**Produktfunktion:** Einmaliges Anmelden  

Azure AD unterstützt die automatische Erkennung von Anmeldefeldern für Anwendungen, die ein HTML-Benutzernamen- und Kennwortfeld rendern. Diese Schritte sind unter [So erfassen Sie Anmeldefelder für eine Anwendung automatisch](https://docs.microsoft.com/azure/active-directory/application-config-sso-problem-configure-password-sso-non-gallery#how-to-manually-capture-sign-in-fields-for-an-application) dokumentiert. Sie finden diese Funktion, indem Sie im [Azure-Portal](https://aad.portal.azure.com) auf der Seite **Unternehmensanwendungen** eine *Nicht-Katalog*-Anwendung hinzufügen. Zusätzlich können Sie den Modus **Einmaliges Anmelden** in dieser neuen Anwendung auf **Kennwortbasiertes einmaliges Anmelden** festlegen, indem Sie eine Web-URL eingeben und die Seite anschließend speichern.
 
Aufgrund eines Dienstproblems war diese Funktion vorübergehend deaktiviert. Das Problem wurde behoben, und die automatische Erkennung des Anmeldefelds ist nun wieder verfügbar.

---

### <a name="new-multi-factor-authentication-features"></a>Neue Multi-Factor Authentication-Features

**Typ:** Neue Funktion  
**Dienstkategorie**: Multi-Factor Authentication  
**Produktfunktion**: Identitätssicherheit und -schutz  

Die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) ist ein wesentlicher Bestandteil des Schutzes Ihrer Organisation. Um die Anmeldeinformationen anpassungsfähiger und das Benutzererlebnis nahtloser zu gestalten, wurden die folgenden Features hinzugefügt: 

- Die Captcha-Ergebnisse bei der mehrstufigen Authentifizierung sind unmittelbar in den Azure AD-Anmeldebericht integriert, einschließlich des programmgesteuerten Zugriffs auf MFA-Ergebnisse.
- Die MFA-Konfiguration wird stärker in die zentrale Azure AD-Konfigurationsumgebung im Azure-Portal integriert.

Mit dieser öffentlichen Vorschau werden MFA-Verwaltung und -Berichterstellung zu einem integrierten Bestandteil der zentralen Azure AD-Konfigurationsumgebung. Nun können Sie die MFA-Verwaltungsportalfunktionalität in der Azure AD-Oberfläche verwalten.

Weitere Informationen finden Sie in der [Referenz zur Berichterstellung für die mehrstufige Authentifizierung im Azure-Portal](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins-mfa). 

---

### <a name="terms-of-use"></a>Nutzungsbedingungen

**Typ:** Neue Funktion  
**Dienstkategorie**: Nutzungsbedingungen  
**Produktfunktion:** Konformität  

Mithilfe der Azure AD-Nutzungsbedingungen können Sie Benutzern Informationen wie relevante Haftungsausschlüsse zur Erfüllung gesetzlicher oder Complianceanforderungen zur Verfügung stellen.

Sie können Azure AD-Nutzungsbedingungen in den folgenden Szenarien verwenden:

- Allgemeine Nutzungsbedingungen für alle Benutzer in Ihrer Organisation
- Spezifische Nutzungsbedingungen basierend auf Benutzerattributen (z.B. Ärzte im Vergleich zu Krankenschwestern oder einheimische Mitarbeiter im Vergleich zu ausländischen Mitarbeitern) von dynamischen Gruppen
- Spezifische Nutzungsbedingungen für den Zugriff auf bedeutende Geschäftsanwendungen wie Salesforce

Weitere Informationen finden Sie im [Azure AD B2C](https://docs.microsoft.com/azure/active-directory/active-directory-tou).

---

### <a name="enhancements-to-privileged-identity-management"></a>Verbesserungen an Privileged Identity Management

**Typ:** Neue Funktion  
**Dienstkategorie**: Privileged Identity Management  
**Produktfunktion**: Privileged Identity Management  

Mit Azure AD Privileged Identity Management können Sie den Zugriff auf Azure-Ressourcen (Vorschauversion) für folgende Elemente innerhalb Ihrer Organisation verwalten, steuern und überwachen:

- Abonnements
- Ressourcengruppen
- Virtuelle Computer 

Alle Ressourcen im Azure-Portal, die die Azure RBAC-Funktion nutzen, können von sämtlichen Sicherheits- und Lebenszyklusverwaltungsfunktionen profitieren, die Azure AD Privileged Identity Management bereitstellt.

Weitere Informationen finden Sie unter [Privileged Identity Management für Azure-Ressourcen](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

---

### <a name="access-reviews"></a>Zugriffsüberprüfungen

**Typ:** Neue Funktion  
**Dienstkategorie** : Zugriffsüberprüfungen  
**Produktfunktion:** Konformität  

Organisationen können mithilfe von Zugriffsüberprüfungen (in der Vorschauversion) effizient Gruppenmitgliedschaften und den Zugriff auf Unternehmensanwendungen verwalten: 

- Sie können den Gastbenutzerzugriff erneut bestätigen, indem Sie den Zugriff auf Anwendungen und Gruppenmitgliedschaften im Rahmen einer Zugriffsüberprüfung überprüfen. Anhand der Erkenntnisse aus den Zugriffsüberprüfungen können Prüfer effizient entscheiden, ob Gäste weiterhin Zugriff haben sollen.
- Mithilfe von Zugriffsüberprüfungen können Sie den Anwendungszugriff von Mitarbeitern sowie deren Gruppenmitgliedschaften neu zertifizieren.

Die Kontrollen der Zugriffsüberprüfung können in unternehmensrelevanten Programmen zusammengefasst werden, um Überprüfungen für compliance- oder risikokritische Anwendungen nachzuverfolgen.

Weitere Informationen finden Sie unter [Azure AD-Zugriffsüberprüfungen](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview).

---

### <a name="hide-third-party-applications-from-my-apps-and-the-office-365-app-launcher"></a>Möglichkeit zum Ausblenden von Drittanbieteranwendungen in „Meine Apps“ und im Office 365-App-Startprogramm

**Typ:** Neue Funktion  
**Dienstkategorie**: Meine Apps  
**Produktfunktion:** Einmaliges Anmelden  

Mit der neuen Eigenschaft **App ausblenden** können Sie nun Apps, die in Ihren Benutzerportalen angezeigt werden, besser verwalten. Sie können Apps ausblenden für den Fall, dass App-Kacheln für Back-End-Dienste angezeigt werden oder Kacheln doppelt vorhanden sind und die App-Startprogramme von Benutzern überladen. Die Umschaltfläche befindet sich im Abschnitt **Eigenschaften** der Drittanbieter-App und hat die Bezeichnung **Für Benutzer sichtbar?**. Sie können eine App auch programmgesteuert über PowerShell ausblenden. 

Weitere Informationen finden Sie unter [Ausblenden einer Anwendung auf der Benutzeroberfläche in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app). 


**Was ist verfügbar?**

 Im Rahmen der Umstellung auf die neue Verwaltungskonsole stehen zwei neue APIs zum Abrufen von Azure AD-Aktivitätsprotokollen zur Verfügung. Die neuen APIs bieten umfangreichere Filter- und Sortierfunktionen sowie mehr Überprüfungs- und Anmeldeaktivitäten. Die Daten, die bisher in den Sicherheitsberichten verfügbar waren, können jetzt über die API für Identity Protection-Risikoereignisse in Microsoft Graph abgerufen werden.


## <a name="september-2017"></a>September 2017

### <a name="hotfix-for-identity-manager"></a>Hotfix für Identity Manager

**Typ:** Geänderte Funktion  
**Dienstkategorie**: Identity Manager  
**Produktfunktion**: Verwaltung des Identitätslebenszyklus  

Ein Hotfixrollup-Paket (Build 4.4.1642.0) ist ab dem 25. September 2017 für Identity Manager 2016 Service Pack 1 verfügbar. Dieser Rolluppaket enthält Folgendes:

- Problembehebungen und Verbesserungen
- Ein kumulatives Update, das alle Identity Manager 2016 Service Pack 1-Updates bis Build 4.4.1459.0 für Identity Manager 2016 ersetzt 
- Identity Manager 2016 Build 4.4.1302.0 wird vorausgesetzt. 

Weitere Informationen finden Sie unter [Hotfixrollup-Paket (Build 4.4.1642.0) ist für Identity Manager 2016 Service Pack 1 verfügbar](https://support.microsoft.com/help/4021562). 

---