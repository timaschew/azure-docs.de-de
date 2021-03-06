---
title: Einrichten der Azure SQL-Datensynchronisierung | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie die Azure SQL-Datensynchronisierung einrichten.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: allenwux
ms.author: xiwu
ms.reviewer: douglasl
manager: craigg
ms.date: 11/07/2018
ms.openlocfilehash: 9175ed0b4f362a40e1d29a20a8378854b5f4cc81
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53310385"
---
# <a name="tutorial-set-up-sql-data-sync-to-sync-data-between-azure-sql-database-and-sql-server-on-premises"></a>Tutorial: Einrichten der SQL-Datensynchronisierung zum Synchronisieren von Daten zwischen Azure SQL-Datenbank und lokalem SQL Server

In diesem Tutorial erfahren Sie, wie sie Azure SQL-Datensynchronisierung einrichten können, indem Sie eine hybride Synchronisierungsgruppe erstellen, die Azure SQL-Datenbank und SQL Server-Instanzen enthält. Die neue Synchronisierungsgruppe ist vollständig konfiguriert und synchronisiert mit dem von Ihnen festgelegten Zeitplan.

In diesem Tutorial wird davon ausgegangen, dass Sie über eine gewisse Erfahrung mit SQL-Datenbank und SQL Server verfügen.

Eine Übersicht über die SQL-Datensynchronisierung finden Sie unter [Synchronisieren von Daten über mehrere Cloud- und lokale Datenbanken mit SQL-Datensynchronisierung](sql-database-sync-data.md).

Vollständige PowerShell-Beispiele, die die Konfiguration der SQL-Datensynchronisierung veranschaulichen, finden Sie in den folgenden Artikeln:

- [Verwenden von PowerShell zum Synchronisieren zwischen mehreren Azure SQL-Datenbanken](scripts/sql-database-sync-data-between-sql-databases.md)
- [Verwenden von PowerShell zum Synchronisieren zwischen einer Azure SQL-Datenbank und einer lokalen SQL Server-Datenbank](scripts/sql-database-sync-data-between-azure-onprem.md)

## <a name="step-1---create-sync-group"></a>Schritt 1: Erstellen einer Synchronisierungsgruppe

### <a name="locate-the-data-sync-settings"></a>Suchen Sie die Einstellungen zur Datensynchronisierung

1. Navigieren Sie in Ihrem Browser zum Azure-Portal.

2. Machen Sie im Portal Ihre SQL-Datenbank über das Dashboard oder über das Symbol für SQL-Datenbanken im der Symbolleiste ausfindig.

    ![Liste der Azure SQL-Datenbanken](media/sql-database-get-started-sql-data-sync/datasync-preview-sqldbs.png)

3. Wählen Sie auf der Seite **SQL-Datenbanken** die vorhandene SQL-Datenbank, die Sie als die Hub-Datenbank für die Datensynchronisierung verwenden möchten. Die Seite für die SQL-Datenbank wird geöffnet.

    Die Hubdatenbank ist der zentrale Endpunkt der Synchronisierungstopologie, bei der eine Synchronisierungsgruppe mehrere Datenbankendpunkte enthält. Alle anderen Datenbankendpunkte in der gleichen Synchronisierungsgruppe, d.h. alle Mitgliedsdatenbanken, werden mit der Hubdatenbank synchronisiert.

4. Wählen Sie auf der Seite „SQL-Datenbank“ der ausgewählten Datenbank **Sync to other databases** (Mit anderen Datenbanken synchronisieren). Die Seite „Datensynchronisierung“ wird geöffnet.

    ![Option „Sync to other databases“ (Mit anderen Datenbanken synchronisieren)](media/sql-database-get-started-sql-data-sync/datasync-preview-newsyncgroup.png)

### <a name="create-a-new-sync-group"></a>Erstellen einer neuen Synchronisierungsgruppe

1. Wählen Sie auf der Seite „Datensynchronisierung“ die Option **Neue Synchronisierungsgruppe** aus. Die Seite **Neue Synchronisierungsgruppe** wird geöffnet. Schritt 1, **Synchronisierungsgruppe erstellen**, ist markiert. Die Seite **Datensynchronisierungsgruppe erstellen** wird ebenfalls geöffnet.

2. Führen Sie auf der Seite **Datensynchronisierungsgruppe erstellen** folgende Schritte aus:

   1. Geben Sie im Feld **Name der Synchronisierungsgruppe** einen Namen für die neue Synchronisierungsgruppe ein.

   2. Wählen Sie im Abschnitt **Datenbank für Synchronisierungsmetadaten** aus, ob Sie eine neue Datenbank erstellen (empfohlen) oder eine vorhandene verwenden möchten.

        > [!NOTE]
        > Es wird empfohlen, dass Sie eine neue, leere Datenbank als Datenbank für Synchronisierungsmetadaten erstellen. Durch die Datensynchronisierung werden Tabellen in Datenbanken erstellt und eine häufige Workload ausgeführt. Diese Datenbank wird automatisch als Datenbank für Synchronisierungsmetadaten für alle Synchronisierungsgruppen in der ausgewählten Region freigegeben. Sie können die Datenbank für Synchronisierungsmetadaten oder deren Namen nicht ändern, ohne alle Synchronisierungsgruppen und Synchronisierungs-Agents in der Region zu entfernen.

        Wenn Sie auf **Neue Datenbank** geklickt haben, klicken Sie auf **Neue Datenbank erstellen**. Die Seite **SQL-Datenbank** wird geöffnet. Benennen und konfigurieren Sie die neue Datenbank auf der Seite **SQL-Datenbank**. Wählen Sie dann **OK**aus.

        Wenn Sie auf **Use existing database** (Bestehende Datenbank verwenden) geklickt haben, wählen Sie die Datenbank aus der Liste.

   3. Wählen Sie im Abschnitt **Automatisch synchronisieren** zunächst **Ein** oder **Aus**.

        Wenn Sie **Ein** ausgewählt haben, geben Sie im Abschnitt **Synchronisierungshäufigkeit** eine Zahl ein: Geben Sie Sekunden, Minuten, Stunden und Tage an.

        ![Synchronisierungshäufigkeit festlegen](media/sql-database-get-started-sql-data-sync/datasync-preview-syncfreq.png)

   4. Wählen Sie im Abschnitt **Konfliktlösung** „Hub gewinnt“ oder „Member gewinnt“ aus.

        „Hub gewinnt“ bedeutet, dass bei einem Konflikt die Daten in der Hubdatenbank die in Konflikt stehenden Daten in der Mitgliedsdatenbank überschreiben. „Mitglied gewinnt“ bedeutet, dass bei einem Konflikt die Daten in der Mitgliedsdatenbank die in Konflikt stehenden Daten in der Hubdatenbank überschreiben.

       ![Konfliktlösung bestimmen](media/sql-database-get-started-sql-data-sync/datasync-preview-conflictres.png)

   5. Klicken Sie auf **OK**, und warten Sie, bis die neue Synchronisierungsgruppe erstellt und bereitgestellt wird.

## <a name="step-2---add-sync-members"></a>Schritt 2: Hinzufügen von Synchronisierungsmitgliedern

Nachdem die neue Synchronisierungsgruppe erstellt und bereitgestellt wurde, ist Schritt 2, **Synchronisierungsmitglieder hinzufügen**, auf der Seite **Neue Synchronisierungsgruppe** markiert.

Geben Sie im Abschnitt **Hub-Datenbank** die vorhandenen Anmeldeinformationen für den SQL-Datenbankserver ein, auf dem die Hub-Datenbank gespeichert ist. Geben Sie in diesem Abschnitt *keine* neuen Anmeldeinformationen ein.

![Die Hub-Datenbank wurde in die Synchronisierungsgruppe hinzugefügt.](media/sql-database-get-started-sql-data-sync/datasync-preview-hubadded.png)

### <a name="add-an-azure-sql-database"></a>Hinzufügen einer Azure SQL-Datenbank

Im Abschnitt **Mitgliedsdatenbank** können Sie optional eine Azure SQL-Datenbank zur Synchronisierungsgruppe hinzufügen, indem Sie auf **Azure-Datenbank hinzufügen** klicken. Die Seite **Azure-Datenbank konfigurieren** wird geöffnet.

Führen Sie auf der Seite **Azure-Datenbank konfigurieren** die folgenden Schritte aus:

1. Geben Sie im Feld **Name des Synchronisierungsmitglieds** einen Namen für das neue Synchronisierungsmitglied ein. Dieser Name unterscheidet sich vom Namen der Datenbank.

2. Wählen Sie im Feld **Abonnement** das verknüpfte Azure-Abonnement für die Rechnungsstellung aus.

3. Wählen Sie im Feld **Azure SQL-Server** den vorhandenen SQL-Datenbankserver aus.

4. Wählen Sie im Feld **Azure SQL-Server** die vorhandene SQL-Datenbank aus.

5. Wählen Sie im Feld **Synchronisierungsrichtungen** „bidirektionale Synchronisierung“, „Zum Hub“ oder „Vom Hub“ aus.

    ![Hinzufügen eines neuen Synchronisierungsmitglieds von SQL-Datenbank](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadding.png)

6. Geben Sie in den Felder **Benutzername** und **Kennwort** die vorhandenen Anmeldeinformationen für den SQL-Datenbankserver ein, auf dem sich die Mitgliedsdatenbank befindet. Geben Sie in diesem Abschnitt *keine* neuen Anmeldeinformationen ein.

7. Klicken Sie auf **OK**, und warten Sie, bis die neue Synchronisierungsmitglied erstellt und bereitgestellt wird.

    ![Das neue Synchronisierungsmitglied von SQL-Datenbank wurde hinzugefügt.](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadded.png)

### <a name="add-on-prem"></a> Hinzufügen einer lokalen SQL Server-Datenbank

Im Abschnitt **Mitgliedsdatenbank** können Sie optional einen lokalen SQL-Server zur Synchronisierungsgruppe hinzufügen, indem Sie auf **Lokale Datenbank hinzufügen** klicken. Die Seite **Lokale Konfiguration** wird geöffnet.

Führen Sie auf der Seite **Lokale Konfiguration** die folgenden Schritte aus:

1. Klicken Sie auf **Synchronisierungs-Agent-Gateway auswählen**. Die Seite **Synchronisierungs-Agent auswählen** wird geöffnet.

    ![Wählen Sie das Synchronisierungs-Agent-Gateway aus.](media/sql-database-get-started-sql-data-sync/datasync-preview-choosegateway.png)

2. Wählen Sie auf der Seite **Synchronisierungs-Agent-Gateway auswählen** aus, ob Sie einen vorhandenen Agent verwenden oder einen neuen erstellen möchten.

    Wenn Sie **Vorhandene Agents** ausgewählt haben, wählen Sie den vorhandenen Agent aus der Liste aus.

    Wenn Sie **Neuen Agent erstellen**, führen Sie folgende Schritte aus:

   1. Laden Sie die Software für den Clientsynchronisierungs-Agent unter dem bereitgestellten Link herunter, und installieren Sie diese auf dem Computer, auf dem sich der SQL-Server befindet. Sie können den Datensynchronisierungs-Agent auch direkt unter [SQL Azure Data Sync Agent](https://www.microsoft.com/download/details.aspx?id=27693) herunterladen.

        > [!IMPORTANT]
        > Sie müssen den ausgehenden TCP-Port 1433 in der Firewall öffnen, damit der Client-Agent mit dem Server kommunizieren kann.

   2. Geben Sie einen Namen für den Agent ein.

   3. Klicken Sie auf **Schlüssel erstellen und generieren**.

   4. Kopieren Sie den Schlüssel des Agents in die Zwischenablage.

        ![Erstellen einen neuen Synchronisierungs-Agents](media/sql-database-get-started-sql-data-sync/datasync-preview-selectsyncagent.png)

   5. Klicken Sie auf **OK**, um die Seite **Synchronisierungs-Agent auswählen** zu schließen.

   6. Suchen Sie auf den SQL-Servercomputer nach der Anwendung des Clientsynchronisierungs-Agents, und führen Sie diese aus.

        ![Die Anwendung des Clientsynchronisierungs-Agents](media/sql-database-get-started-sql-data-sync/datasync-preview-clientagent.png)

   7. Klicken Sie in der Synchronisierungs-Agent-Anwendung auf **Submit Agent Key** (Agent-Schlüssel übermitteln). Das Dialogfeld **Sync Metadata Database Configuration** (Konfiguration der Datenbank für Synchronisierungsmetadaten) wird geöffnet.

   8. Fügen Sie im Dialogfeld **Konfiguration der Datenbank für Synchronisierungsmetadaten** den Agent-Schlüssel ein, den Sie aus dem Azure-Portal kopiert haben. Geben Sie auch die vorhandenen Anmeldeinformationen für den Azure SQL-Datenbankserver ein, auf dem sich die Metadatendatenbank befindet. (Wenn Sie eine neue Metadatendatenbank erstellt haben, befindet sich diese Datenbank auf demselben Server wie die Hub-Datenbank.) Klicken Sie auf **OK**, und warten Sie, bis die Konfiguration abgeschlossen ist.

        ![Eingabe des Agent-Schlüssels und der Anmeldeinformationen des Servers](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-enterkey.png)

        > [!NOTE]
        > Wenn Sie jetzt eine Fehlermeldung der Firewall erhalten, müssen Sie eine Firewallregel in Azure aufstellen, um eingehenden Datenverkehr vom SQL-Servercomputer zuzulassen. Sie können die Regel manuell im Verwaltungsportal erstellen, aber möglicherweise ist es für Sie einfacher, diese in SSMS (SQL Server Management Studio) zu erstellen. Versuchen Sie in SSMS eine Verbindung mit der Hub-Datenbank in Azure herzustellen. Geben Sie den Namen im Format „<Name der Hub-Datenbank>.database.windows.net“ ein. Führen Sie zum Konfigurieren der Azure-Firewallregel die Schritte im Dialogfeld aus. Kehren Sie dann zur Anwendung des Clientsynchronisierungs-Agents zurück.

   9. Klicken Sie in der Anwendung des Clientsynchronisierungs-Agents auf **Register** (Registrieren), um eine SQL-Serverdatenbank beim Agent zu registrieren. Das Dialogfeld **SQL Server-Konfiguration** wird geöffnet.

        ![Hinzufügen und Konfigurieren einer SQL Server-Datenbank](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-adddb.png)

   10. Wählen Sie im Dialogfeld **SQL Server-Konfiguration** aus, ob die Verbindung über die SQL Server-Authentifizierung oder die Windows-Authentifizierung erfolgen soll. Wenn Sie die SQL Server-Authentifizierung ausgewählt haben, geben Sie die vorhandenen Anmeldeinformationen ein. Geben Sie den SQL Server-Namen und den Namen der Datenbank an, die Sie synchronisieren möchten. Klicken Sie auf **Verbindung testen**, um Ihre Einstellung zu testen. Klicken Sie dann auf **Speichern**. Die registrierte Datenbank wird in der Liste angezeigt.

        ![Die SQL Server-Datenbank ist nun registriert.](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-dbadded.png)

   11. Sie können jetzt die Anwendung des Clientsynchronisierungs-Agents schließen.

   12. Klicken Sie im Portal auf der Seite **Lokale Konfiguration** auf **Datenbank auswählen**. Die Seite **Datenbank auswählen** wird geöffnet.

   13. Geben Sie auf der Seite **Datenbank auswählen** im Feld **Name des Synchronisierungsmitglieds** einen Namen für das neue Synchronisierungsmitglied an. Dieser Name unterscheidet sich vom Namen der Datenbank. Wählen Sie die Datenbank aus der Liste aus. Wählen Sie im Feld **Synchronisierungsrichtungen** „bidirektionale Synchronisierung“, „Zum Hub“ oder „Vom Hub“ aus.

        ![Wählen Sie die lokale Datenbank](media/sql-database-get-started-sql-data-sync/datasync-preview-selectdb.png)

   14. Klicken Sie auf **OK**, um die Seite **Datenbank auswählen** zu schließen. Klicken Sie dann auf **OK**, um die Seite **Lokale Konfiguration** zu schließen, und warten Sie, bis das neue Synchronisierungsmitglied erstellt und bereitgestellt wurde. Klicken Sie abschließend auf **OK**, um die Seite **Synchronisierungsmitglieder auswählen** zu schließen.

        ![Lokale Datenbank zur Synchronisierungsgruppe hinzugefügt](media/sql-database-get-started-sql-data-sync/datasync-preview-onpremadded.png)

3. Um eine Verbindung mit der SQL-Datensynchronisierung und dem lokalen Agent herzustellen, fügen Sie Ihren Benutzername der Rolle `DataSync_Executor` hinzu. Die Datensynchronisierung erstellt diese Rolle auf der SQL Server-Instanz.

## <a name="step-3---configure-sync-group"></a>Schritt 3: Konfigurieren der Synchronisierungsgruppe

Nachdem die neuen Mitglieder der Synchronisierungsgruppe erstellt und bereitgestellt wurden, ist Schritt 3, **Synchronisierung konfigurieren**, auf der Seite **Neue Synchronisierungsgruppe** markiert.

1. Wählen Sie auf der Seite **Tabellen** eine Datenbank aus der Liste der Synchronisierungsgruppenmitglieder aus, und klicken Sie anschließend auf **Schema aktualisieren**.

2. Wählen Sie aus der Liste der verfügbaren Tabellen die Tabellen aus, die Sie synchronisieren möchten.

    ![Auswählen zu synchronisierender Tabellen](media/sql-database-get-started-sql-data-sync/datasync-preview-tables.png)

3. Standardmäßig werden alle Spalten in der Tabelle ausgewählt. Wenn Sie nicht alle Spalten synchronisieren möchten, entfernen Sie das Häkchen im Kontrollkästchen für die Spalten, die Sie nicht synchronisieren möchten. Achten Sie darauf, dass die Primärschlüsselspalte ausgewählt bleibt.

    ![Auswählen zu synchronisierender Felder](media/sql-database-get-started-sql-data-sync/datasync-preview-tables2.png)

4. Klicken Sie abschließend auf **Speichern**.

## <a name="faq-about-setup-and-configuration"></a>Häufig gestellte Fragen zur Einrichtung und Konfiguration

### <a name="how-frequently-can-data-sync-synchronize-my-data"></a>Wie häufig kann die Datensynchronisierung für meine Daten erfolgen?

Die Mindestdauer zwischen der Auslösung von Synchronisierungen beträgt fünf Minuten.

### <a name="does-sql-data-sync-fully-create-and-provision-tables"></a>Werden von der SQL-Datensynchronisierung Tabellen vollständig erstellt und bereitgestellt?

Wenn die Synchronisierungsschematabellen in der Zieldatenbank noch nicht erstellt wurden, werden sie von der SQL-Datensynchronisierung mit den von Ihnen ausgewählten Spalten erstellt. Allerdings wird dabei aus den folgenden Gründen kein vollständig identisches Schema erstellt:

- In der Zieltabelle werden nur die von Ihnen ausgewählten Spalten erstellt. Falls also einige Spalten in der Quelltabelle nicht Teil der Synchronisierungsgruppe sind, werden sie nicht in den Zieltabellen bereitgestellt.
- Indizes werden nur für die ausgewählten Spalten erstellt. Enthält der Quelltabellenindex Spalten, die nicht Teil der Synchronisierungsgruppe sind, werden diese Indizes nicht in den Zieltabellen bereitgestellt.
- Indizes für XML-Spalten werden nicht bereitgestellt.
- CHECK-Einschränkungen werden nicht bereitgestellt.
- Vorhandene Trigger für die Quelltabellen werden nicht bereitgestellt.
- Sichten und gespeicherte Prozeduren werden in der Zieldatenbank nicht erstellt.

Aufgrund dieser Einschränkungen wird Folgendes empfohlen:

- Stellen Sie für Produktionsumgebungen das vollständig identische Schema selbst bereit.
- Zum Testen des Diensts ist die Funktion für die automatische Bereitstellung der SQL-Datensynchronisierung gut geeignet.

### <a name="why-do-i-see-tables-that-i-did-not-create"></a>Warum sehe ich Tabellen, die ich nicht erstellt habe?

Bei der Datensynchronisierung werden Nebentabellen für die Änderungsnachverfolgung erstellt. Löschen Sie diese nicht, da ansonsten die Datensynchronisierung nicht mehr funktioniert.

### <a name="is-my-data-convergent-after-a-sync"></a>Sind meine Daten nach einer Synchronisierung konvergent?

Nicht unbedingt. In einer Synchronisierungsgruppe mit einem Hub und drei Spokes (A, B und C) erfolgen die Synchronisierungen zwischen dem Hub und A, dem Hub und B und dem Hub und C. Wenn die Datenbank A *nach* der Synchronisierung des Hubs mit A geändert wird, wird diese Änderung erst bei der nächsten Synchronisierungsaufgabe in Datenbank B und Datenbank C geschrieben.

### <a name="how-do-i-get-schema-changes-into-a-sync-group"></a>Wie übertrage ich Schemaänderungen in eine Synchronisierungsgruppe?

Alle Schemaänderungen müssen manuell vorgenommen und verteilt werden.

1. Replizieren Sie die Schemaänderungen manuell auf dem Hub und auf allen Synchronisierungsmitgliedern.
2. Aktualisieren Sie das Synchronisierungsschema.

**Hinzufügen neuer Tabellen und Spalten:**

Neue Tabellen und Spalten haben keine Auswirkungen auf die aktuelle Synchronisierung. Die Datensynchronisierung berücksichtigt neue Tabellen und Spalten erst, nachdem Sie sie dem Synchronisierungsschema hinzugefügt haben. Gehen Sie beim Hinzufügen neuer Datenbankobjekte am besten wie folgt vor:

1. Fügen Sie die neuen Tabellen oder Spalten dem Hub und allen Synchronisierungsmitgliedern hinzu.
2. Fügen Sie die neuen Tabellen oder Spalten dem Synchronisierungsschema hinzu.
3. Fügen Sie Werte in die neuen Tabellen und Spalten ein.

**Ändern des Datentyps einer Spalte:**

Wenn Sie den Datentyp einer vorhandenen Spalte ändern, funktioniert die Datensynchronisierung weiterhin – vorausgesetzt, die neuen Werte sind mit dem ursprünglich im Synchronisierungsschema definierten Datentyp kompatibel. Wenn Sie also beispielsweise in der Quelldatenbank den Typ von **int** in **bigint** ändern, funktioniert die Datensynchronisierung weiterhin, solange Sie keinen Wert einfügen, der für den Datentyp **int** zu groß ist. Replizieren Sie die Schemaänderung manuell auf dem Hub sowie auf allen Synchronisierungsmitgliedern, und aktualisieren Sie anschließend das Synchronisierungsschema, um die Änderung abzuschließen.

### <a name="how-can-i-export-and-import-a-database-with-data-sync"></a>Wie kann ich eine Datenbank durch Datensynchronisierung exportieren und importieren?

Nachdem Sie eine Datenbank als `.bacpac`-Datei exportiert und die Datei zum Erstellen einer neuen Datenbank importiert haben, müssen Sie die folgenden zwei Schritte ausführen, um die Datensynchronisierung in der neuen Datenbank zu verwenden:

1. Bereinigen Sie in der **neuen Datenbank** mithilfe [dieses Skripts](https://github.com/vitomaz-msft/DataSyncMetadataCleanup/blob/master/Data%20Sync%20complete%20cleanup.sql) die Datensynchronisierungsobjekte und Seitentabellen. Dieses Skript löscht alle erforderlichen Datensynchronisierungsobjekte aus der Datenbank.
2. Erstellen Sie die Synchronisierungsgruppe mit der neuen Datenbank neu. Wenn Sie die alte Synchronisierungsgruppe nicht mehr benötigen, löschen Sie sie.

## <a name="faq-about-the-client-agent"></a>Häufig gestellte Fragen zum Client-Agent

Häufig gestellte Fragen zum Client-Agent finden Sie unter [Agent – Häufig gestellte Fragen](sql-database-data-sync-agent.md#agent-faq).

## <a name="next-steps"></a>Nächste Schritte

Herzlichen Glückwunsch. Sie haben nun eine Synchronisierungsgruppe erstellt, die sowohl eins SQL-Datenbank-Instanz als auch eine SQL Server-Datenbank enthält.

Weitere Informationen zur SQL-Datensynchronisierung finden Sie unter:

-   Übersicht: [Synchronisieren von Daten über mehrere Cloud- und lokale Datenbanken mit SQL-Datensynchronisierung](sql-database-sync-data.md)
-   Einrichten der Datensynchronisierung
    - Mit PowerShell
        -  [Verwenden von PowerShell zum Synchronisieren zwischen mehreren Azure SQL-Datenbanken](scripts/sql-database-sync-data-between-sql-databases.md)
        -  [Verwenden von PowerShell zum Synchronisieren zwischen einer Azure SQL-Datenbank und einer lokalen SQL Server-Datenbank](scripts/sql-database-sync-data-between-azure-onprem.md)
-   Datensynchronisierungs-Agent: [Datensynchronisierungs-Agent für die Azure SQL-Datensynchronisierung](sql-database-data-sync-agent.md)
-   Bewährte Methoden: [Bewährte Methoden für die Azure SQL-Datensynchronisierung](sql-database-best-practices-data-sync.md)
-   Überwachung: [Überwachen der SQL-Datensynchronisierung mit Log Analytics](sql-database-sync-monitor-oms.md)
-   Problembehandlung: [Behandeln von Problemen mit der Azure SQL-Datensynchronisierung](sql-database-troubleshoot-data-sync.md)
-   Aktualisieren des Synchronisierungsschemas
    -   Mit Transact-SQL: [Automatisieren der Replikation von Schemaänderungen in der Azure SQL-Datensynchronisierung](sql-database-update-sync-schema.md)
    -   Mit PowerShell: [Verwenden von PowerShell zum Aktualisieren des Synchronisierungsschemas in einer bestehenden Synchronisierungsgruppe](scripts/sql-database-sync-update-schema.md)

Weitere Informationen zu SQL-Datenbank finden Sie unter:

- [Übersicht über die SQL-Datenbank](sql-database-technical-overview.md)
- [Datenbank-Lebenszyklusverwaltung](https://msdn.microsoft.com/library/jj907294.aspx)
