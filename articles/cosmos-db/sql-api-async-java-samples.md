---
title: 'Azure Cosmos DB: Async Java-Beispiele für die SQL-API'
description: Async Java-Beispiele für allgemeine Aufgaben mit der SQL-API von Azure Cosmos DB (einschließlich CRUD-Vorgängen) finden Sie auf GitHub.
services: cosmos-db
author: SnehaGunda
documentationcenter: java
ms.service: cosmos-db
ms.devlang: java
ms.topic: sample
ms.date: 06/18/2018
ms.author: sngun
ms.openlocfilehash: 4c3f731695c21f85c1ceea908e0aee5cd340d3b2
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52880062"
---
# <a name="azure-cosmos-db-async-java-examples-for-the-sql-api"></a>Azure Cosmos DB: Async Java-Beispiele für die SQL-API

> [!div class="op_single_selector"]
> * [.NET-Beispiele](sql-api-dotnet-samples.md)
> * [Java-Beispiele](sql-api-java-samples.md)
> * [Async Java-Beispiele](sql-api-async-java-samples.md)
> * [Node.js-Beispiele](sql-api-nodejs-samples.md)
> * [Python-Beispiele](sql-api-python-samples.md)
> * [Katalog mit Azure-Codebeispielen](https://azure.microsoft.com/resources/samples/?sort=0&service=cosmos-db)
> 
> 

Die neuesten Beispielanwendungen, in denen CRUD-Vorgänge und andere gängige Vorgänge für Azure Cosmos DB-Ressourcen ausgeführt werden, finden Sie im GitHub-Repository [azure-cosmosdb-java](https://github.com/Azure/azure-cosmosdb-java). Dieser Artikel enthält Folgendes:

* Links zu den Aufgaben in den einzelnen Async Java-Beispielprojektdateien. 
* Links zum zugehörigen API-Referenzinhalt.

**Voraussetzungen**

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
  
- Sie können Ihre [Vorteile für Visual Studio-Abonnenten aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): Ihr Visual Studio-Abonnement beinhaltet ein monatliches Guthaben, das Sie für zahlungspflichtige Azure-Dienste nutzen können.

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

Zum Ausführen dieser Beispielanwendung benötigen Sie Folgendes:

* Java Development Kit 8
* Microsoft Azure Cosmos DB Java SDK

Optional können Sie Maven verwenden, um die neuesten Binärdateien des Microsoft Azure Cosmos DB Java SDK zur Verwendung in Ihrem Projekt abzurufen. Die neueste Version finden Sie [hier](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb). Maven fügt alle erforderlichen Abhängigkeiten automatisch hinzu. Andernfalls können Sie die in der Datei „pom.xml“ aufgelisteten Abhängigkeiten direkt herunterladen und Ihrem Buildpfad hinzufügen.

```bash
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-cosmosdb</artifactId>
    <version>${LATEST}</version>
</dependency>
```

**Ausführen der Beispielanwendungen**

Klonen des Beispielrepositorys:
```bash
$ git clone https://github.com/Azure/azure-cosmosdb-java.git

$ cd azure-cosmosdb-java
```

Sie können die Beispiele entweder mit Eclipse oder über die Befehlszeile mit Maven ausführen.

So führen Sie die Beispiele über Eclipse aus:
* Laden Sie die Datei „pom.xml“ des übergeordneten Hauptprojekts in Eclipse. „azure-cosmosdb-examples“ sollte automatisch geladen werden.
* Zum Ausführen der Beispiele benötigen Sie einen gültigen Azure Cosmos DB-Endpunkt. Die Endpunkte werden aus `src/test/java/com/microsoft/azure/cosmosdb/rx/examples/TestConfigurations.java` gelesen.
* Sie können Ihre Endpunktanmeldeinformationen als VM-Argumente in der Eclipse JUnit-Ausführungskonfiguration übergeben oder in „TestConfigurations.java“ einfügen.
    ```bash
    -DACCOUNT_HOST=<Fill your Azure Cosmos DB account name> -DACCOUNT_KEY=<Fill your Azure Cosmos DB primary key>
    ```
* Jetzt können Sie die Beispiele als JUnit-Tests in Eclipse ausführen.

So führen Sie die Beispiele über die Befehlszeile aus:
* Die zweite Möglichkeit zum Ausführen der Beispiele ist die Verwendung von Maven:
* Führen Sie Maven aus, und übergeben Sie Ihre Azure Cosmos DB-Endpunktanmeldeinformationen:
    ```bash
    mvn test -DACCOUNT_HOST=<Fill your Azure Cosmos DB account name> -DACCOUNT_KEY=<Fill your Azure Cosmos DB primary key>
    ```

   > [!NOTE]
   > Jedes Beispiel ist eigenständig mit eigener Einrichtung und Bereinigung. Die Beispiele richten mehrere Aufrufe an [DocumentClient.createCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.createcollection). Dabei wird Ihrem Abonnement jedes Mal 1 Stunde Nutzung gemäß der Leistungsstufe der erstellten Sammlung berechnet. 
   > 
   > 

## <a name="database-examples"></a>Datenbankbeispiele
Die Datei [DatabaseCrudAsyncAPITest](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DatabaseCRUDAsyncAPITest.java) zeigt, wie die folgenden Aufgaben ausgeführt werden:

| Aufgabe | API-Referenz |
| --- | --- |
| [Erstellen einer Datenbank](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DatabaseCRUDAsyncAPITest.java#L115-L132) | [AsyncDocumentClient.createDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.createdatabase) |
| [Fehler beim Erstellen einer bereits vorhandenen Datenbank](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DatabaseCRUDAsyncAPITest.java#L189-L204) | |
| [Erstellen und Lesen einer Datenbank](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DatabaseCRUDAsyncAPITest.java#L228-L249) | [AsyncDocumentClient.readDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.readdatabase) |
| [Erstellen und Löschen einer Datenbank](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DatabaseCRUDAsyncAPITest.java#L255-L276) | [AsyncDocumentClient.deleteDatabase](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.deletedatabase) |
| [Erstellen und Abfragen einer Datenbank](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DatabaseCRUDAsyncAPITest.java#L282-L312) | [AsyncDocumentClient.queryDatabases](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.querydatabases) |

## <a name="collection-examples"></a>Sammlungsbeispiele
Die Datei [CollectionCrudAsyncAPITest](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java) zeigt, wie die folgenden Aufgaben ausgeführt werden:

| Aufgabe | API-Referenz |
| --- | --- |
| [Erstellen einer Sammlung mit nur einer Partition](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java#L134-L153) | [AsyncDocumentClient.createCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.createcollection) |
| [Erstellen einer Sammlung mit mehreren Partitionen](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java#L164-L184) | [DocumentCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._document_collection)<br>[PartitionKeyDefinition](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._partition_key_definition)<br>[RequestOptions](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._request_options) |
| [Fehler beim Erstellen einer bereits vorhandenen Sammlung](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java#L241-L256) | |
| [Erstellen und Lesen einer Sammlung](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java#L281-L304) | [AsyncDocumentClient.readCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.readcollection) |
| [Erstellen und Löschen einer Sammlung](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java#L310-L333) | [AsyncDocumentClient.deleteCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.deletecollection) |
| [Erstellen und Abfragen einer Sammlung](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java#L339-L372) | [AsyncDocumentClient.queryCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.querycollections) |

## <a name="document-examples"></a>Dokumentbeispiele
Die Datei [DocumentCrudAsyncAPITest](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java) zeigt, wie die folgenden Aufgaben ausgeführt werden:

| Aufgabe | API-Referenz |
| --- | --- |
| [Erstellen eines Dokuments](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L137-L156) | [AsyncDocumentClient.createDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.createdocument) |
| [Erstellen eines Dokuments mit einer programmierbaren Dokumentdefinition](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L213-L242) | [Dokument](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._document)<br>[Resource.setId](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._resource.setid) |
| [Erstellen von Dokumenten und Ermitteln der RU-Gesamtkosten](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L248-L280) | [ResourceResponse.getRequestCharge](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._resource_response.getrequestcharge) |
| [Fehler beim Erstellen eines bereits vorhandenen Dokuments](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L316-L336) | |
| [Erstellen und Ersetzen eines Dokuments](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L342-L365) | [AsyncDocumentClient.replaceDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.replacedocument) |
| [Erstellen und Aktualisieren/Einfügen (Upsert) eines Dokuments](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L371-L393) | [AsyncDocumentClient.upsertDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.upsertdocument) |
| [Erstellen und Löschen eines Dokuments](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L399-L431) | [AsyncDocumentClient.deleteDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.deletedocument) |
| [Erstellen und Lesen eines Dokuments](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentCRUDAsyncAPITest.java#L437-L458) | [AsyncDocumentClient.readDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.readdocument) |

## <a name="indexing-examples"></a>Indizierungsbeispiele
Die Datei [CollectionCrudAsyncAPITest](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java) zeigt, wie die folgenden Aufgaben ausgeführt werden:

| Aufgabe | API-Referenz |
| --- | --- |
| [Erstellen eines Index und Festlegen der Indizierungsrichtlinie](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/CollectionCRUDAsyncAPITest.java#L394-L410) | [Index](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._index)<br>[IndexingPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._indexing_policy) |

Weitere Informationen zur Indizierung finden Sie unter [Indizierungsrichtlinien für Azure Cosmos DB](index-policy.md).

## <a name="query-examples"></a>Abfragebeispiele
Die Datei [DocumentQuerySamples](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentQueryAsyncAPITest.java) zeigt, wie die folgenden Aufgaben ausgeführt werden:

| Aufgabe | API-Referenz |
| --- | --- |
| [Ausführen einer einfachen Dokumentabfrage](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentQueryAsyncAPITest.java#L154-L190) | [AsyncDocumentClient.queryDocuments](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.querydocuments) |
| [Ausführen einer einfachen Dokumentabfrage und Ermitteln der RU-Gesamtkosten](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentQueryAsyncAPITest.java#L249-L268) | [FeedResponse.getRequestCharge](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._feed_response.getrequestcharge) |
| [Ausführen einer einfachen Dokumentabfrage, lesen einer einzelnen Seite und Kündigen des Abonnements für das zurückgegebene Observable-Element](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentQueryAsyncAPITest.java#L274-L312) | |
| [Ausführen einer einfachen Dokumentabfrage und Filtern der Ergebnisse](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentQueryAsyncAPITest.java#L318-L368) | |
| [Ausführen einer partitionsübergreifenden Dokumentabfrage mit Sortierung](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/DocumentQueryAsyncAPITest.java#L410-L457) | [FeedOptions.setEnableCrossPartitionQuery](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._feed_options.setenablecrosspartitionquery) |

Weitere Informationen zum Schreiben von Abfragen finden Sie unter [SQL-Abfragen für Azure Cosmos DB-DocumentDB-API](how-to-sql-query.md).

## <a name="offer-examples"></a>Angebotsbeispiele
Die Datei [OfferCRUDAsyncAPITest](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/OfferCRUDAsyncAPITest.java) zeigt, wie die folgenden Aufgaben ausgeführt werden:

| Aufgabe | API-Referenz |
| --- | --- |
| [Erstellen einer Sammlung und Festlegen des Durchsatzes](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/OfferCRUDAsyncAPITest.java#L106-L113) | [AsyncDocumentClient.createCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.createcollection)<br>[RequestOptions.setOfferThroughput](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._request_options.setofferthroughput) |
| [Lesen einer Sammlung, um das zugehörige Angebot zu finden](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/OfferCRUDAsyncAPITest.java#L118-L130) | [Offer.getContent](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._offer.getContent)<br>[DocumentClient.queryOffers](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.queryoffers) |
| [Aktualisieren des Durchsatzes einer Sammlung durch Ersetzen des Angebots](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/OfferCRUDAsyncAPITest.java#L101-L153) | [AsyncDocumentClient.replaceOffer](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.replaceoffer) |

## <a name="stored-procedure-examples"></a>Beispiele für gespeicherte Prozeduren
Die Datei [StoredProcedureAsyncAPITest](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/StoredProcedureAsyncAPITest.java) zeigt, wie die folgenden Aufgaben ausgeführt werden:

| Aufgabe | API-Referenz |
| --- | --- |
| [Erstellen und Ausführen einer gespeicherten Prozedur](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/StoredProcedureAsyncAPITest.java#L108-L155) | [StoredProcedure](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._stored_procedure)<br>[DocumentClient.createStoredProcedure](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.createstoredprocedure)<br>[AsyncDocumentClient.executeStoredProcedure](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.executestoredprocedure) |
| [Erstellen und Ausführen einer gespeicherten Prozedur mit Argumenten](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/StoredProcedureAsyncAPITest.java#L161-L195) | |
| [Erstellen und Ausführen einer gespeicherten Prozedur mit einem Objektargument](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/StoredProcedureAsyncAPITest.java#L201-L241) | |

## <a name="unique-key"></a>Eindeutiger Schlüssel
Die Datei [UniqueIndexAsyncAPITest](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/UniqueIndexAsyncAPITest.java) zeigt, wie die folgenden Aufgaben ausgeführt werden:

| Aufgabe | API-Referenz |
| --- | --- |
| [Erstellen einer Sammlung mit eindeutigem Schlüssel](https://github.com/Azure/azure-cosmosdb-java/blob/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples/UniqueIndexAsyncAPITest.java#L65-L97) | [UniqueKey](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._unique_key)<br>[UniqueKeyPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._unique_key_policy)<br>[DocumentCollection.setUniqueKeyPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._document_collection.setuniquekeypolicy) |
