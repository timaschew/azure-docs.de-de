---
title: Bedrohungserkennung – Azure SQL-Datenbank | Microsoft Docs
description: Die Bedrohungserkennung erkennt anormale Datenbankaktivitäten, die in einer einzelnen Datenbank oder in einem Pool für elastische Datenbanken auf potenzielle Sicherheitsrisiken für die Datenbank hindeuten.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto, carlrab
manager: craigg
ms.date: 10/25/2018
ms.openlocfilehash: 2882bd782359697cf714214e68166c9f997f52e4
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50093546"
---
# <a name="azure-sql-database-threat-detection"></a>Bedrohungserkennung von Azure SQL-Datenbank

Die Azure SQL-Bedrohungserkennung für [SQL-Datenbank](sql-database-technical-overview.md) und [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) identifiziert anomale Aktivitäten, die auf ungewöhnliche und möglicherweise schädliche Versuche hinweisen, auf Datenbanken zuzugreifen oder diese zu missbrauchen.

Die Bedrohungserkennung ist Teil des Angebots [SQL Advanced Threat Protection (ATP)](sql-advanced-threat-protection.md). Dabei handelt es sich um ein vereinheitlichtes Paket für erweiterte SQL-Sicherheitsfunktionen. Der Zugriff auf die Bedrohungserkennung und ihre Verwaltung sind über das zentrale SQL ATP-Portal möglich.


> [!NOTE] 
> Dieses Thema gilt für Azure SQL-Server sowie für Datenbanken von SQL-Datenbank und SQL Data Warehouse, die auf dem Azure SQL-Server erstellt werden. Der Einfachheit halber wird nur SQL-Datenbank verwendet, wenn sowohl SQL-Datenbank als auch SQL Data Warehouse gemeint sind.


## <a name="what-is-threat-detection"></a>Was ist Bedrohungserkennung?

Die SQL-Bedrohungserkennung bietet eine neue Sicherheitsebene, die es den Kunden ermöglicht, auf erkannte potenzielle Bedrohungen zu reagieren. Zu diesem Zweck werden Sicherheitshinweise zu anomalen Aktivitäten bereitgestellt. Benutzer erhalten Warnungen zu verdächtigen Datenbankaktivitäten, potenziellen Sicherheitsrisiken sowie Angriffen durch Einschleusung von SQL-Befehlen und anomalen Datenbankzugriffs- und -abfragemustern. Die SQL-Bedrohungserkennung integriert Warnungen in [Azure Security Center](https://azure.microsoft.com/services/security-center/). Dabei werden Detailinformationen zu verdächtigen Aktivitäten sowie Empfehlungen, wie die Bedrohung untersucht und abgewendet werden kann, bereitgestellt. Die SQL-Bedrohungserkennung vereinfacht den Umgang mit potenziellen Bedrohungen für die Datenbank, ohne das Fachwissen eines Sicherheitsexperten besitzen oder komplexe Sicherheitsüberwachungssysteme verwalten zu müssen. 

Zur vollständigen Untersuchung empfiehlt es sich, die [SQL-Datenbanküberwachung](sql-database-auditing.md) zu aktivieren, bei der Datenbankereignisse in ein Überwachungsprotokoll in Ihrem Azure Storage-Konto geschrieben werden.  

## <a name="set-up-threat-detection-for-your-database-in-the-azure-portal"></a>Einrichten der Bedrohungserkennung für Ihre Datenbank im Azure-Portal
1. Starten Sie das Azure-Portal unter [https://portal.azure.com](https://portal.azure.com).
2. Navigieren Sie zur Konfigurationsseite des Azure SQL-Datenbank-Servers, den Sie schützen möchten. Wählen Sie in den Sicherheitseinstellungen **Advanced Threat Protection** aus.
3. Gehen Sie auf der Konfigurationsseite **Advanced Threat Protection** folgendermaßen vor:

   - Aktivieren Sie Advanced Threat Protection auf dem Server.
   - Geben Sie unter **Einstellungen für Bedrohungserkennung** im Textfeld **Send alerts to** (Warnungen senden an) eine Liste von E-Mail-Adressen an, die Sicherheitswarnungen bei der Erkennung von anomalen Datenbankaktivitäten empfangen sollen.
  
   ![Einrichten der Bedrohungserkennung](./media/sql-database-threat-detection/set_up_threat_detection.png)

## <a name="set-up-threat-detection-using-powershell"></a>Einrichten der Bedrohungserkennung über PowerShell

Ein Skriptbeispiel finden Sie unter [Konfigurieren von Überwachung von SQL-Datenbank und Bedrohungserkennung mit PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).

## <a name="explore-anomalous-database-activities-upon-detection-of-a-suspicious-event"></a>Untersuchen anormaler Datenbankaktivitäten bei Erkennung eines verdächtigen Ereignisses

Bei Erkennung anormaler Datenbankaktivitäten erhalten Sie eine E-Mail-Benachrichtigung. Die E-Mail enthält Informationen zum verdächtigen Sicherheitsereignis (dazu gehören Art der anomalen Aktivitäten, Datenbankname, Servername, Anwendungsname und Zeit des Ereignisses). Darüber hinaus enthält die E-Mail Angaben zu möglichen Ursachen und empfohlenen Maßnahmen zur Untersuchung und Abwehr der potenziellen Bedrohung für die Datenbank.

![Bericht zu anomalen Aktivitäten](./media/sql-database-threat-detection/anomalous_activity_report.png)
     
1. Klicken Sie in der E-Mail auf den Link **View recent SQL alerts** (Aktuelle SQL-Warnungen anzeigen), um das Azure-Portal zu starten und die Azure Security Center-Seite für Warnungen zu öffnen, auf der eine Übersicht über die aktiven Bedrohungen angezeigt wird, die in der SQL-Datenbank erkannt wurden.

   ![Aktivitätsbedrohungen](./media/sql-database-threat-detection/active_threats.png)

2. Klicken Sie auf eine bestimmte Warnung, um weitere Details und Aktionen zum Untersuchen der entsprechenden Bedrohung und Abwehren zukünftiger Bedrohungen anzuzeigen.

   Beispielsweise ist die Einschleusung von SQL-Befehlen das häufigste Sicherheitsproblem für Webanwendungen im Internet, das für Angriffe auf datengesteuerte Anwendungen verwendet wird. Die Angreifer nutzen Sicherheitslücken der Anwendung, um böswillige SQL-Anweisungen in Eingabefelder der Anwendung einzuschleusen, sodass Daten in der Datenbank manipuliert oder verändert werden können. Bei Warnungen in Bezug auf die Einschleusung von SQL-Befehlen schließen die Details der Warnung die anfällige missbräuchlich genutzte SQL-Anweisung ein.

   ![Spezifische Warnung](./media/sql-database-threat-detection/specific_alert.png)

## <a name="explore-threat-detection-alerts-for-your-database-in-the-azure-portal"></a>Auswerten von Bedrohungserkennungswarnungen für Ihre Datenbank im Azure-Portal

Die Warnungen der Bedrohungserkennung von SQL-Datenbank sind in [Azure Security Center](https://azure.microsoft.com/services/security-center/) eingebunden. Auf einer Livekachel der SQL-Bedrohungserkennung auf den Blättern der Datenbank und von SQL ATP im Azure-Portal werden die Status von aktiven Bedrohungen nachverfolgt.

Klicken Sie auf **Threat detection alert** (Warnung der Bedrohungserkennung), um die Azure Security Center-Seite für Warnungen zu öffnen und eine Übersicht über die aktiven SQL-Bedrohungen zu erhalten, die in der Datenbank oder im Data Warehouse erkannt wurden.

   ![Warnung der Bedrohungserkennung](./media/sql-database-threat-detection/threat_detection_alert.png)
   
   ![Warnung der Bedrohungserkennung 2](./media/sql-database-threat-detection/threat_detection_alert_atp.png)

## <a name="azure-sql-database-threat-detection-alerts"></a>Warnungen der Bedrohungserkennung von Azure SQL-Datenbank 
Die Bedrohungserkennung für Azure SQL-Datenbank erkennt anomale Aktivitäten, die auf ungewöhnliche und möglicherweise schädliche Versuche hinweisen, bei denen auf Datenbanken zugegriffen oder diese missbraucht werden sollen. Hierbei können die folgenden Warnungen ausgelöst werden:
- **Anfälligkeit für die Einschleusung von SQL-Befehlen**: Diese Warnung wird ausgelöst, wenn eine Anwendung in der Datenbank eine fehlerhafte SQL-Anweisung generiert. Dies kann ein Hinweis auf ein mögliches Sicherheitsrisiko in Bezug auf Angriffe mit Einschleusung von SQL-Befehlen sein. Es gibt zwei mögliche Gründe für die Generierung einer fehlerhaften Anweisung:
   - Ein Fehler im Anwendungscode, der zur fehlerhaften SQL-Anweisung führt
   - Anwendungscode oder gespeicherte Prozeduren führen bei der Erstellung der fehlerhaften SQL-Anweisung keine Bereinigung der Benutzereingabe durch, und dies kann für eine Einschleusung von SQL-Befehlen ausgenutzt werden
- **Potenzielle Einschleusung von SQL-Befehlen**: Diese Warnung wird ausgelöst, wenn ein aktiver Exploit für ein identifiziertes Anwendungssicherheitsrisiko in Bezug auf die Einschleusung von SQL-Befehlen besteht. Dies bedeutet, dass der Angreifer versucht, schädliche SQL-Anweisungen einzuschleusen, indem er den anfälligen Anwendungscode bzw. die gespeicherten Prozeduren verwendet.
- **Access from unusual location** (Zugriff von einem ungewöhnlichen Ort): Diese Warnung wird ausgelöst, wenn eine Änderung des Zugriffsmusters für SQL Server erfolgt, bei der sich eine Person von einem ungewöhnlichen Ort aus an SQL Server angemeldet hat. In einigen Fällen erkennt die Warnung eine legitime Aktion (eine neue Anwendung oder Wartungsarbeiten von Entwicklern). In anderen Fällen erkennt die Warnung eine schädliche Aktion (ehemaliger Mitarbeiter, externer Angreifer).
- **Access from unusual Azure data center** (Zugriff aus einem ungewöhnlichen Azure-Rechenzentrum): Diese Warnung wird ausgelöst, wenn sich das Zugriffsmuster für SQL Server geändert hat, weil sich eine Person über ein ungewöhnliches Azure-Rechenzentrum an SQL Server angemeldet hat, das auf diesem Server während des letzten Zeitraums aufgetreten ist. In einigen Fällen erkennt die Warnung eine legitime Aktion (Ihre neue Anwendung in Azure, Power BI, Azure SQL-Abfrage-Editor). In anderen Fällen erkennt die Warnung ggf. eine schädliche Aktion einer Azure-Ressource bzw. eines -Diensts (ehemaliger Mitarbeiter, externer Angreifer).
- **Access from unfamiliar principal** (Zugriff über einen ungewöhnlichen Prinzipal): Diese Warnung wird ausgelöst, wenn eine Änderung des Zugriffsmusters für SQL Server erfolgt, bei der sich eine Person über einen ungewöhnlichen Prinzipal (SQL-Benutzer) an SQL Server angemeldet hat. In einigen Fällen erkennt die Warnung eine legitime Aktion (neue Anwendung, Wartungsarbeiten von Entwicklern). In anderen Fällen erkennt die Warnung eine schädliche Aktion (ehemaliger Mitarbeiter, externer Angreifer).
- **Access from a potentially harmful application** (Zugriff über eine potenziell schädliche Anwendung): Diese Warnung wird ausgelöst, wenn zum Zugreifen auf die Datenbank eine potenziell schädliche Anwendung verwendet wird. In einigen Fällen erkennt die Warnung aktive Eindringversuche. In anderen Fällen erkennt die Warnung einen Angriff mit allgemeinen Angriffstools.
- **Brute force SQL credentials** (Brute-Force-Angriff auf SQL-Anmeldeinformationen): Diese Warnung wird ausgelöst, wenn eine ungewöhnlich hohe Zahl von fehlgeschlagenen Anmeldungen mit unterschiedlichen Anmeldeinformationen vorliegt. In einigen Fällen erkennt die Warnung aktive Eindringversuche. In anderen Fällen erkennt die Warnung einen Brute-Force-Angriff.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [SQL Advanced Threat Protection](sql-advanced-threat-protection.md). 
* Weitere Informationen zu [Überwachung von Azure SQL-Datenbank](sql-database-auditing.md)
* Weitere Informationen zu [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
* Weitere Informationen zu den Preisen finden Sie unter [SQL-Datenbank – Preise](https://azure.microsoft.com/pricing/details/sql-database/).  
