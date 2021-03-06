---
title: Verwalten der Berechtigungen für Datenbanken in Azure-Daten-Explorer
description: In diesem Artikel wird die rollenbasierte Zugriffssteuerung für Datenbanken und Tabellen in Azure-Daten-Explorer beschrieben.
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 287b95b59b0ec2b308d3e455c4f6ffce4baf4ff7
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2018
ms.locfileid: "50212878"
---
# <a name="manage-azure-data-explorer-database-permissions"></a>Verwalten der Berechtigungen für Datenbanken in Azure-Daten-Explorer

Der Azure-Daten-Explorer ermöglicht Ihnen über das Modell für die *rollenbasierte Zugriffssteuerung* die Steuerung des Zugriffs auf Datenbanken und Tabellen. Bei diesem Modell werden *Prinzipale* (Benutzer, Gruppen und Apps) *Rollen* zugewiesen. Prinzipale können gemäß den zugewiesenen Rollen auf Ressourcen zugreifen.

Dieser Artikel beschreibt die verfügbaren Rollen und erläutert, wie diesen Rollen über das Azure-Portal und Verwaltungsbefehle des Azure-Daten-Explorers Prinzipale zugewiesen werden.

## <a name="roles-and-permissions"></a>Rollen und Berechtigungen

Der Azure-Daten-Explorer verfügt über die folgenden Rollen:

|Rolle                       |Berechtigungen                                                                        |
|---------------------------|-----------------------------------------------------------------------------------|
|Database admin (Datenbankadministrator)             |Kann alle Aktionen im Bereich einer bestimmten Datenbank ausführen.|
|Datenbank-Benutzer              |Kann alle Daten und Metadaten in der Datenbank lesen. Darüber hinaus kann er Tabellen und Funktionen in der Datenbank erstellen und wird dabei zum Tabellenadministrator für die jeweiligen Tabellen.|
|Database viewer (Anzeigender Datenbankbenutzer)            |Kann alle Daten und Metadaten in der Datenbank lesen.|
|Database ingestor (Datenbankerfasser)          |Kann Daten in allen vorhandenen Tabellen in der Datenbank erfassen, die Daten aber nicht abfragen.|
|Database monitor (Datenbankmonitor)           |Kann Befehle vom Typ „.show ... im Kontext der Datenbank und ihrer untergeordneten Entitäten ausführen.|
|Table admin (Tabellenadministrator)                |Kann alle Aktionen im Bereich einer bestimmten Tabelle ausführen. |
|Table ingestor (Tabellenerfasser)             |Kann Daten im Bereich einer bestimmten Tabelle erfassen, die Daten aber nicht abfragen.|

## <a name="manage-permissions-in-the-azure-portal"></a>Verwalten von Berechtigungen im Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Navigieren Sie zum Azure-Daten-Explorer-Cluster.

1. Wählen Sie im Abschnitt **Übersicht** die Datenbank aus, für die Sie Berechtigungen verwalten möchten.

    ![Datenbank auswählen](media/manage-database-permissions/select-database.png)

1. Wählen Sie **Berechtigungen** und dann **Hinzufügen** aus.

    ![Datenbankberechtigungen](media/manage-database-permissions/database-permissions.png)

1. Wählen Sie unter **Add database permissions** (Datenbankberechtigungen hinzufügen) die Rolle, der Sie den Prinzipal hinzufügen möchten, und dann **Select principals** (Prinzipale auswählen) aus.

    ![Hinzufügen von Datenbankberechtigungen](media/manage-database-permissions/add-permission.png)

1. Suchen Sie nach dem Prinzipal, und wählen Sie ihn und dann **Auswählen** aus.

    ![Verwalten von Berechtigungen im Azure-Portal](media/manage-database-permissions/new-principals.png)

1. Wählen Sie **Speichern**aus.

    ![Verwalten von Berechtigungen im Azure-Portal](media/manage-database-permissions/save-permission.png)

## <a name="manage-permissions-with-management-commands"></a>Verwalten von Berechtigungen mit Verwaltungsbefehlen

1. Melden Sie sich unter [https://dataexplorer.azure.com](https://dataexplorer.azure.com) an, und fügen Sie Ihren Cluster hinzu, sofern er noch nicht verfügbar ist.

1. Wählen Sie im linken Bereich die entsprechende Datenbank aus.

1. Weisen Sie mit dem Befehl `.add` Rollen Prinzipale hinzu: `.add database databasename rolename ('aaduser | aadgroup=user@domain.com')`. Führen Sie zum Hinzufügen eines Benutzers zur Rolle „Datenbank-Benutzer“ den folgenden Befehl aus. Verwenden Sie dabei Ihren eigenen Datenbanknamen und -benutzer.

    ```Kusto
    .add database <TestDatabase> users ('aaduser=<user@contoso.com>')
    ```

    Die Ausgabe des Befehls enthält die Liste der vorhandenen Benutzer und der Rollen, denen sie in der Datenbank zugewiesen sind.

## <a name="next-steps"></a>Nächste Schritte

[Write queries](write-queries.md) (Schreiben von Abfragen)
