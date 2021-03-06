---
title: Was ist Azure AD Privileged Identity Management? | Microsoft-Dokumentation
description: Hier finden Sie eine Übersicht über Azure Active Directory Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: pim
ms.topic: overview
ms.date: 03/07/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 6720294fe9a3e166d0d6ef8f141e53212ef4b194
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52496797"
---
# <a name="what-is-azure-ad-privileged-identity-management"></a>Was ist Azure AD Privileged Identity Management?

Mit Azure Active Directory Privileged Identity Management können Sie den Zugriff innerhalb Ihrer Organisation verwalten, steuern und überwachen. Dazu zählt der Zugriff auf Ressourcen in Azure AD, in Azure-Ressourcen und anderen Microsoft-Onlinediensten wie Office 365 oder Microsoft Intune.

> [!NOTE]
> Wenn Sie Privileged Identity Management für Ihren Mandanten aktivieren, ist für jeden Benutzer, der mit dem Dienst interagiert oder einen Nutzen daraus zieht, eine gültige Lizenz vom Typ Azure AD Premium P2 oder Enterprise Mobility + Security E5 (kostenpflichtig oder Testversion) erforderlich. Beispiele sind Benutzer bzw. Benutzer in einer Gruppe, für die Folgendes gilt:
>
>- Zuweisung der Rolle „Administrator für privilegierte Rollen“ 
>- Zuweisung als für andere Verzeichnisrollen verfügbar, die per PIM verwaltet werden können 
>- Möglichkeit zum Genehmigen/Ablehnen von Anforderungen in PIM 
>- Zuweisung einer Azure-Ressourcenrolle mit Just-in-Time- oder direkten (zeitbasierten) Zuweisungen  
>- Zuweisung zu einer Zugriffsüberprüfung
>
>Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](../fundamentals/active-directory-whatis.md).

Organisationen möchten die Anzahl von Personen mit Zugriff auf sichere Informationen oder Ressourcen möglichst gering halten, da sich dadurch das Risiko verringert, dass ein böswilliger Benutzer Zugriff darauf erhält oder dass ein autorisierter Benutzer versehentlich eine sensible Ressource kompromittiert.  Benutzer müssen jedoch in Azure AD, Azure, Office 365 oder SaaS-Apps weiterhin privilegierte Vorgänge ausführen. Organisationen können Benutzern privilegierten Zugriff auf Azure-Ressourcen wie Abonnements und Azure AD gewähren. Dabei muss jedoch überwacht werden, wofür diese Benutzer ihre Administratorrechte nutzen. Azure AD Privileged Identity Management trägt dazu bei, das Risiko durch unverhältnismäßige oder unnötige Zugriffsrechte bzw. durch deren Missbrauch zu verringern.

Azure AD Privileged Identity Management ermöglicht Ihrer Organisation Folgendes:

- Ermitteln, welchen Benutzern privilegierte Rollen für die Verwaltung von Azure-Ressourcen und welchen Benutzern Administratorrollen in Azure AD zugewiesen sind
- Aktivieren von bedarfsgesteuertem Just-In-Time-Administratorzugriff auf Microsoft-Onlinedienste wie Office 365 und Intune sowie auf Azure-Ressourcen von Abonnements, Ressourcengruppen und Einzelressourcen wie virtuellen Computern 
- Anzeigen eines Administratoraktivierungsverlaufs, der unter anderem Aufschluss über die von Administratoren vorgenommenen Änderungen an Azure-Ressourcen gibt
- Erhalten von Benachrichtigungen bei geänderten Administratorzuweisungen
- Erzwingen einer Genehmigung für die Aktivierung privilegierter Azure AD-Administratorrollen
- Überprüfen der Mitgliedschaft von Administratorrollen und Erzwingen der Angabe einer Begründung bei längerer Mitgliedschaft

In Azure AD kann Azure AD Privileged Identity Management die Benutzer verwalten, die den integrierten Azure AD-Organisationsrollen zugewiesen sind (beispielsweise „Globaler Administrator“). In Azure kann Azure AD Privileged Identity Management die Benutzer und Gruppen verwalten, die über Azure RBAC-Rollen zugewiesen werden (einschließlich „Besitzer“ oder „Mitwirkender“).

## <a name="just-in-time-administrator-access"></a>Bedarfsabhängiger Administratorzugriff

In der Vergangenheit konnten Sie einen Benutzer über das Azure-Portal, über andere Portale von Microsoft-Onlinediensten oder über die Azure AD-Cmdlets in Windows PowerShell einer Administratorrolle zuweisen. Dadurch wird der Benutzer zum **permanenten Administrator**, der in seiner zugewiesenen Rolle stets aktiv bleibt. Mit Azure AD Privileged Identity Management wird das Konzept **berechtigter Administratoren** eingeführt. Berechtigte Administratoren sollten Benutzer sein, die von Zeit zu Zeit (aber nicht ständig und jeden Tag) Zugriff mit erhöhten Rechten benötigen. Die Rolle ist inaktiv, bis der Benutzer Zugriff benötigt. Dann wird eine Aktivierung ausgeführt, und der Benutzer wird für einen zuvor festgelegten Zeitraum zu einem aktiven Administrator. Immer mehr Unternehmen entscheiden sich für diesen Ansatz, um den ständigen Administratorzugriff auf privilegierte Rollen zu verringern oder ganz zu beseitigen.


## <a name="terminology"></a>Begriff

*Benutzer mit berechtigter Rolle:* Ein Benutzer mit berechtigter Rolle ist ein Benutzer in Ihrer Organisation, der einer Rolle in Azure AD als berechtigt zugewiesen ist (Rolle erfordert Aktivierung).

*Delegierte genehmigende Person:* Einzelne oder mehrere Personen oder Gruppen in Ihrem Azure AD, die für die Genehmigung von Anforderungen der Rollenaktivierung verantwortlich sind, sind genehmigende Personen.

## <a name="scenarios"></a>Szenarien

Privileged Identity Management unterstützt folgende Szenarien:

**Als Administrator für privilegierte Rollen haben Sie folgende Möglichkeiten:**

- Aktivieren der Genehmigung für bestimmte Rollen
- Festlegen von Benutzern und/oder Gruppen als genehmigende Personen für die Genehmigung von Anforderungen
- Anzeigen des Anforderungs- und Genehmigungsverlaufs für alle privilegierten Rollen

**Als festgelegte genehmigende Person können Sie:**

- Anzeigen ausstehender Genehmigungen (Anforderungen)
- Genehmigen oder Ablehnen von Anforderungen von Rechteerweiterungen für Rollen (einzeln und/oder mehrere)
- Abgeben einer Begründung für die Genehmigung/Ablehnung 

**Als Benutzer mit berechtigter Rolle haben Sie folgende Möglichkeiten:**

- Anfordern der Aktivierung einer Rolle, die genehmigt werden muss
- Anzeigen des Status Ihrer Aktivierungsanforderung
- Fertigstellen Ihrer Aufgabe in Azure AD, wenn die Aktivierung genehmigt wurde

## <a name="who-can-do-what-in-pim"></a>Berechtigungen und Rollen

Wenn Sie die erste Person sind, die PIM verwendet, werden Ihnen automatisch die Rollen [Sicherheitsadministrator](../users-groups-roles/directory-assign-admin-roles.md#security-administrator) und [Administrator für privilegierte Rollen](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) im Verzeichnis zugewiesen.

Für Azure AD-Rollen gilt: Nur ein Benutzer mit der Rolle eines Administrators für privilegierte Rollen kann Zuweisungen für andere Administratoren in PIM verwalten. Sie können [Zugriff für andere Administratoren zum Verwalten von PIM gewähren](pim-how-to-give-access-to-pim.md). Globale Administratoren, Sicherheitsadministratoren und Benutzer mit Leseberechtigung für Sicherheitsfunktionen können Zuweisungen zu Azure AD-Rollen in PIM anzeigen.

Bei Azure-Ressourcenrollen können nur Abonnementadministratoren, Ressourcenbesitzer und Ressourcen-Benutzerzugriffsadministratoren Zuweisungen für andere Administratoren in PIM verwalten. Administratoren für privilegierte Rollen, Sicherheitsadministratoren oder Benutzer mit Leseberechtigung für Sicherheitsfunktionen haben nicht standardmäßig Zugriff auf Zuweisungen zu Azure-Ressourcenrollen in PIM.

## <a name="privileged-identity-management-overview-entry-point"></a>Übersicht über Privileged Identity Management (Einstiegspunkt)

Azure AD Privileged Identity Management unterstützt die Verwaltung von Azure AD-Verzeichnisrollen sowie von Rollen für Azure-Ressourcen. Die Funktion von Rollen für Azure-Ressourcen unterscheidet sich von Administratorrollen in Azure AD. Azure-Ressourcenrollen bieten präzise Berechtigungen für die Ressource, für die sie zugewiesen sind, sowie für alle untergeordneten Ressourcen in der Ressourcenhierarchie (Vererbung). Weitere Informationen zu RBAC, Ressourcenhierarchie und Vererbung finden Sie [hier](../../role-based-access-control/role-assignments-portal.md). PIM für Azure AD-Verzeichnisrollen und Azure-Ressourcen kann über den entsprechenden Link im Abschnitt „Verwalten“ verwaltet werden. Zu diesem Abschnitt gelangen Sie über das linke Navigationsmenü in der PIM-Übersicht.

Mit PIM können Sie über den Aufgabenabschnitt des linken Navigationsmenüs komfortabel Rollen aktivieren sowie ausstehende Aktivierungen/Anforderungen, ausstehende Genehmigungen (für Azure AD-Verzeichnisrollen) und Überprüfungen mit ausstehender Antwort anzeigen.

Wenn Sie von der Übersicht aus auf eines der Elemente im Aufgabenmenü zugreifen, enthält die daraufhin angezeigte Ansicht Ergebnisse sowohl für Azure AD-Verzeichnisrollen als auch für Azure-Ressourcenrollen.

![Schnellstart](./media/pim-configure/quick-start.png)

Unter „My roles“ (Meine Rollen) befindet sich eine Liste mit aktiven und berechtigten Rollenzuweisungen für Azure AD-Verzeichnisrollen und Azure-Ressourcenrollen. Weitere Informationen zum Aktivieren berechtigter Rollenzuweisungen finden Sie [hier](pim-how-to-activate-role.md).

Zur Aktivierung von Rollen für Azure-Ressourcen wird eine neue Umgebung eingeführt, in der berechtigte Mitglieder einer Rolle die Aktivierung für einen späteren Zeitpunkt planen und eine bestimmte Aktivierungsdauer auswählen können, die innerhalb des von Administratoren festgelegten Maximalbereichs liegt.

![](./media/pim-configure/activations.png)

Falls eine geplante Aktivierung nicht mehr benötigt wird, können Benutzer die ausstehende Anforderung abbrechen, indem sie über das linke Navigationsmenü zu den ausstehenden Anforderungen navigieren und für die entsprechende Anforderung auf die Schaltfläche „Abbrechen“ klicken.

![Ausstehende Anforderungen](./media/pim-configure/pending-requests.png)

## <a name="privileged-identity-management-admin-dashboard"></a>Privileged Identity Management-Administator-Dashboard

Azure AD Privileged Identity Manager bietet ein Administratoren-Dashboard, das wichtige Informationen enthält, wie etwa:

* Warnungen, die auf Möglichkeiten zur Erhöhung der Sicherheit hinweisen
* Anzahl der Benutzer, die jeder privilegierten Rolle zugewiesen sind  
* Anzahl berechtigter und permanenter Administratoren
* Ein Diagramm der Aktivierungen privilegierter Rollen in Ihrem Verzeichnis
* Die Anzahl von Just-In-Time-Zuweisungen, zeitgebundenen Zuweisungen und permanenten Zuweisungen für Azure-Ressourcenrollen
* Benutzer und Gruppen mit neuen Rollenzuweisungen in den letzten 30 Tagen (Azure-Ressourcenrollen)


![PIM-Dashboard – Screenshot](./media/pim-configure/PIM_Admin_Overview.png)

## <a name="privileged-role-management"></a>Verwaltung privilegierter Rollen

Mit Azure AD Privileged Identity Management können Sie die Administratoren verwalten, indem Sie den einzelnen Rollen permanente oder berechtigte Administratoren für Azure AD-Verzeichnisrollen hinzufügen oder selbige entfernen. Mit PIM für Azure-Ressourcen können Besitzer, Benutzerzugriffsadministratoren und globale Administratoren, die die Verwaltung von Abonnements in ihrem Mandanten ermöglichen, Azure-Ressourcenrollen Benutzer oder Gruppen als berechtigt (Just-In-Time-Zugriff) oder zeitgebunden (keine Aktivierung erforderlich) zuweisen – entweder mit einem Start- und Endzeitpunkt (Datum und Uhrzeit) oder permanent (sofern in den Rolleneinstellungen aktiviert).

![Hinzufügen/Entfernen von PIM-Administratoren – Screenshot](./media/pim-configure/PIM_AddRemove.png)

## <a name="configure-the-role-activation-settings"></a>Konfigurieren der Rollenaktivierungseinstellungen

Mithilfe der [Rolleneinstellungen](pim-how-to-change-default-settings.md) können Sie die Eigenschaften der Aktivierung der berechtigten Rolle für Azure AD-Verzeichnisrollen konfigurieren:

* Dauer des Rollenaktivierungszeitraums
* Benachrichtigung zur Rollenaktivierung
* Informationen, die ein Benutzers während des Rollenaktivierungsprozesses bereitstellen muss
* Dienstticket oder Vorfallnummer
* [Anforderungen an den Genehmigungsworkflow](./azure-ad-pim-approval-workflow.md)

![PIM-Einstellungen – Administratoraktivierung – Screenshot](./media/pim-configure/PIM_Settings_w_Approval_Disabled.png)

Beachten Sie, dass in der Abbildung die Schaltflächen für **Multi-Factor Authentication** deaktiviert sind. Für bestimmte, sehr privilegierte Rollen ist MFA für erhöhten Schutz erforderlich.

Mit Rolleneinstellungen für Azure-Ressourcenrollen können Administratoren Einstellungen für Just-In-Time- und Direktzuweisungen konfigurieren:

- Die Möglichkeit, Benutzer oder Gruppen Rollen ohne Enddatum/-uhrzeit zuzuweisen (permanente Zuweisung)
- Die Standarddauer einer Zuweisung (sofern nicht permanent)
- Die maximale Aktivierungsdauer (Aktivierung eines berechtigten Rollenmitglieds)
- Die Informationen, die ein Benutzer im Rahmen der Rollenaktivierung (Just-In-Time-Zuweisungen) oder im Rahmen des Zuweisungsprozesses (Direktzuweisungen) angeben muss

![](./media/pim-configure/role-settings-details.png)

## <a name="role-activation"></a>Rollenaktivierung

Um eine [Rolle zu aktivieren](pim-how-to-activate-role.md), fordert ein berechtigter Administrator eine zeitgebundene „Aktivierung“ für die Rolle an. Die Aktivierung kann über die Option **Meine Rolle aktivieren** in Azure AD Privileged Identity Management angefordert werden.

Ein Administrator, der eine Rolle aktivieren möchte, muss Azure AD Privileged Identity Management im Azure-Portal initialisieren.

Die Rollenaktivierung ist anpassbar. In den PIM-Einstellungen können Sie die Dauer der Aktivierung sowie die Informationen festlegen, die der Administrator angeben muss, um die Rolle zu aktivieren.

![Anforderung der Rollenaktivierung durch PIM-Administrator – Screenshot](./media/pim-configure/PIM_RequestActivation.png)

## <a name="review-role-activity"></a>Überprüfen der Rollenaktivität

Ihnen stehen zwei Möglichkeiten zur Verfügung, um nachzuverfolgen, wie Ihre Mitarbeiter und Administratoren privilegierte Rollen nutzen. Die erste Option verwendet den [Verlauf der Verzeichnisrollenüberwachung](pim-how-to-use-audit-log.md). Im Überwachungsverlauf werden Änderungen an privilegierten Rollenzuweisungen, der Rollenaktivierungsverlauf sowie Änderungen an Einstellungen für Azure-Ressourcenrollen nachverfolgt. 

![PIM-Aktivierungsverlauf – Screenshot](./media/pim-configure/PIM_ActivationHistory.png)

Die zweite Option besteht darin, regelmäßige [Zugriffsüberprüfungen](pim-how-to-start-security-review.md)einzurichten. Diese Zugriffsüberprüfungen können von einem zugewiesenen Prüfer (wie etwa einem Teamleiter) durchgeführt werden, oder die Mitarbeiter führen die Überprüfung selbst durch. Dies ist die beste Möglichkeit, um zu prüfen, wer noch Zugriff benötigt und wer nicht mehr.

## <a name="azure-ad-pim-at-subscription-expiration"></a>Azure AD PIM bei Abonnementablauf

Zur Verwendung von Azure AD PIM muss ein Mandant in seinem Mandanten über ein Testabonnement oder kostenpflichtiges Abonnement vom Typ „Azure AD Premium P2“ (oder EMS E5) verfügen.  Darüber hinaus müssen den Administratoren des Mandanten Lizenzen zugewiesen werden.  Das gilt insbesondere für Administratoren in Azure AD-Rollen, die über Azure AD PIM verwaltet werden, für Administratoren in Azure RBAC-Rollen, die über Azure AD PIM verwaltet werden, und für Benutzer ohne Administratorrechte, die Zugriffsüberprüfungen durchführen.
Wenn Ihre Organisation Azure AD Premium P2 nicht verlängert oder der Testzeitraum abläuft, stehen die Azure AD PIM-Features in Ihrem Mandanten nicht mehr zur Verfügung, berechtigte Rollenzuweisungen werden entfernt, und Benutzer können keine Rollen mehr aktivieren. Weitere Informationen finden Sie in den [Abonnementanforderungen von Azure AD PIM](./subscription-requirements.md).

## <a name="next-steps"></a>Nächste Schritte

- [Einstieg in die Verwendung von PIM](pim-getting-started.md)
- [Abonnementanforderungen für die Verwendung von PIM](subscription-requirements.md)
- [Schützen des privilegierten Zugriffs für hybride und Cloudbereitstellungen in Azure AD](../users-groups-roles/directory-admin-roles-secure.md?toc=%2fazure%2factive-directory%2fprivileged-identity-management%2ftoc.json)
