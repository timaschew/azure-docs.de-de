---
title: 'Tutorial: Laden von Beispieldaten in eine Cassandra-API-Tabelle in Azure Cosmos DB mit einer Java-Anwendung'
description: In diesem Tutorial wird das Laden von Beispielbenutzerdaten in eine Cassandra-API-Tabelle in Azure Cosmos DB mithilfe einer Java-Anwendung gezeigt.
author: kanshiG
ms.service: cosmos-db
ms.component: cosmosdb-cassandra
ms.topic: tutorial
ms.date: 12/03/2018
ms.author: govindk
ms.reviewer: sngun
Customer intent: As a developer, I want to build a Java application to load data to a Cassandra API table in Azure Cosmos DB so that customers can store and manage the key/value data and utilize the global distribution, elastic scaling, multi-master, and other capabilities offered by Azure Cosmos DB.
ms.openlocfilehash: fad6d34a3d3712a036e0275717754f75d5314aae
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52863652"
---
# <a name="tutorial-load-sample-data-into-a-cassandra-api-table-in-azure-cosmos-db"></a>Tutorial: Laden von Beispieldaten in eine Cassandra-API-Tabelle in Azure Cosmos DB

Als Entwickler besitzen Sie möglicherweise Anwendungen, die Schlüssel/Wert-Paare verwenden. Sie können das Cassandra-API-Konto in Azure Cosmos DB zum Speichern und Verwalten von Schlüssel-Wert-Daten verwenden. In diesem Tutorial wird das Laden von Beispielbenutzerdaten in eine Tabelle in einem Cassandra-API-Konto in Azure Cosmos DB mithilfe einer Java-Anwendung gezeigt. Die Java-Anwendung verwendet den [Java-Treiber](https://github.com/datastax/java-driver) und lädt Benutzerdaten wie Benutzer-ID, Benutzername und Benutzerort. 

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Laden von Daten in eine Cassandra-Tabelle
> * Ausführen der App

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

* Dieser Artikel gehört zu einem mehrteiligen Tutorial. Bevor Sie mit diesem Dokument beginnen, müssen Sie [Cassandra-API-Konto, Keyspace und Tabelle erstellen](create-cassandra-api-account-java.md).   

## <a name="load-data-into-the-table"></a>Laden von Daten in die Tabelle

Führen Sie zum Laden von Daten in Ihre Cassandra-API-Tabelle die folgenden Schritte aus:

1. Öffnen Sie die Datei „UserRepository.java“ im Ordner „src\main\java\com\azure\cosmosdb\cassandra“, und fügen Sie den Code zum Einfügen der Felder „user_id“, „user_name“ und „user_bcity“ in die Tabelle hinzu:

   ```java
   /**
   * Insert a row into user table
   *
   * @param id   user_id
   * @param name user_name
   * @param city user_bcity
   */
   public void insertUser(PreparedStatement statement, int id, String name, String city) {
        BoundStatement boundStatement = new BoundStatement(statement);
        session.execute(boundStatement.bind(id, name, city));
   }

   /**
   * Create a PrepareStatement to insert a row to user table
   *
   * @return PreparedStatement
   */
   public PreparedStatement prepareInsertStatement() {
      final String insertStatement = "INSERT INTO  uprofile.user (user_id, user_name , user_bcity) VALUES (?,?,?)";
   return session.prepare(insertStatement);
   }
   ```
 
2. Öffnen Sie die Datei „UserProfile.java“ im Ordner „src\main\java\com\azure\cosmosdb\cassandra“. Diese Klasse enthält die main-Methode, die die Methoden createKeyspace und createTable aufruft, die Sie zuvor definiert haben. Fügen Sie nun den folgenden Code zum Einfügen einiger Beispieldaten in die Cassandra-API-Tabelle hinzu.

   ```java
   //Insert rows into user table
   PreparedStatement preparedStatement = repository.prepareInsertStatement();
     repository.insertUser(preparedStatement, 1, "JohnH", "Seattle");
     repository.insertUser(preparedStatement, 2, "EricK", "Spokane");
     repository.insertUser(preparedStatement, 3, "MatthewP", "Tacoma");
     repository.insertUser(preparedStatement, 4, "DavidA", "Renton");
     repository.insertUser(preparedStatement, 5, "PeterS", "Everett");
   ```

## <a name="run-the-app"></a>Ausführen der App

Öffnen Sie eine Eingabeaufforderung oder ein Terminalfenster, und ändern Sie den Ordnerpfad gemäß des Ordners, in dem Sie das Projekt erstellt haben. Führen Sie den Befehl „mvn clean install“ zum Generieren der Datei „cosmosdb-cassandra-examples.jar“ im Zielordner aus, und führen Sie die Anwendung aus. 

```bash
cd "cassandra-demo"

mvn clean install

java -cp target/cosmosdb-cassandra-examples.jar com.azure.cosmosdb.cassandra.examples.UserProfile
```

Sie können jetzt den Daten-Explorer im Azure-Portal öffnen, um zu bestätigen, dass die Benutzerinformationen der Tabelle hinzugefügt wurden.
    
## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, Beispieldaten in das Cassandra-API-Konto in Azure Cosmos DB zu laden. Sie können jetzt mit dem nächsten Artikel fortfahren:

> [!div class="nextstepaction"]
> [Abfragen von Daten aus dem Cassandra-API-Konto](cassandra-api-query-data.md)
