---
title: Verwenden von PostgreSQL-Erweiterungen in Azure Database for PostgreSQL
description: Beschreibung der Möglichkeit zum Erweitern der Funktionalität von Datenbanken mithilfe von Erweiterungen in Azure-Datenbank für PostgreSQL
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 11/12/2018
ms.openlocfilehash: 195a54ab65573a24911169b7dc42de7637ad60f9
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2018
ms.locfileid: "51823808"
---
# <a name="postgresql-extensions-in-azure-database-for-postgresql"></a>PostgreSQL-Erweiterungen in Azure Database for PostgreSQL
PostgreSQL bietet die Möglichkeit, die Funktionalität Ihrer Datenbank mithilfe von Erweiterungen zu erweitern. Durch Erweiterungen können mehrere SQL-bezogene Objekte zu einem einzigen Paket gebündelt und über einen einzigen Befehl in die Datenbank geladen oder aus dieser entfernt werden. Nach dem Laden in die Datenbank können Erweiterungen ebenso wie integrierte Features funktionieren. Weitere Informationen zu PostgreSQL-Erweiterungen finden Sie unter  [Packen von zugehörigen Objekten in einer Erweiterung](https://www.postgresql.org/docs/9.6/static/extend-extensions.html).

## <a name="how-to-use-postgresql-extensions"></a>Wie werden PostgreSQL-Erweiterungen verwendet?
Bevor Sie PostgreSQL-Erweiterungen verwenden können, müssen diese in Ihrer Datenbank installiert werden. Um eine bestimmte Erweiterung zu installieren, führen Sie zum Laden der gepackten Objekte in Ihrer Datenbank den Befehl  [CREATE EXTENSION](https://www.postgresql.org/docs/9.6/static/sql-createextension.html)  über das psql-Tool aus.

Azure Database for PostgreSQL unterstützt eine Teilmenge der wichtigsten unten aufgeführten Erweiterungen. Andere Erweiterungen außer den hier aufgeführten werden nicht unterstützt. Mit dem Azure Database for PostgreSQL-Dienst können Sie keine eigenen Erweiterungen erstellen.

## <a name="extensions-supported-by-azure-database-for-postgresql"></a>Unterstützte Erweiterungen in Azure-Datenbank für PostgreSQL
In den folgenden Tabellen werden die standardmäßigen PostgreSQL-Erweiterungen aufgeführt, die derzeit von Azure-Datenbank für PostgreSQL unterstützt werden. Sie können diese Informationen auch abrufen, indem Sie `SELECT * FROM pg_available_extensions;` ausführen.

### <a name="data-types-extensions"></a>Datentypenerweiterungen

> [!div class="mx-tableFixed"]
| **Erweiterung** | **Beschreibung** |
|---|---|
| [chkpass](https://www.postgresql.org/docs/9.6/static/chkpass.html) | Stellt einen Datentyp für automatisch verschlüsselte Kennwörter bereit. |
| [citext](https://www.postgresql.org/docs/9.6/static/citext.html) | Gibt einen Zeichenfolgentyp an, bei dem Groß-/Kleinschreibung beachtet werden muss. |
| [cube](https://www.postgresql.org/docs/9.6/static/cube.html) | Stellt einen Datentyp für mehrdimensionale Cubes bereit. |
| [hstore](https://www.postgresql.org/docs/9.6/static/hstore.html) | Gibt den Datentyp zum Speichern von Schlüssel-/Wertpaaren an. |
| [isn](https://www.postgresql.org/docs/9.6/static/isn.html) | Stellt Datentypen für internationale Produktnummerierungsnormen bereit. |
| [ltree](https://www.postgresql.org/docs/9.6/static/ltree.html) | Stellt einen Datentyp für hierarchische baumähnliche Strukturen bereit. |

### <a name="functions-extensions"></a>Funktionserweiterungen

> [!div class="mx-tableFixed"]
| **Erweiterung** | **Beschreibung** |
|---|---|
| [earthdistance](https://www.postgresql.org/docs/9.6/static/earthdistance.html) | Bietet eine Möglichkeit, Großkreisentferungen auf der Erdoberfläche zu berechnen. |
| [fuzzystrmatch](https://www.postgresql.org/docs/9.6/static/fuzzystrmatch.html) | Stellt mehrere Funktionen bereit, um Ähnlichkeiten und den Abstand zwischen Zeichenfolgen zu ermitteln |
| [intarray](https://www.postgresql.org/docs/9.6/static/intarray.html) | Stellt Funktionen und Operatoren bereit, um Arrays mit Ganzzahlen ohne Nullen zu bearbeiten |
| [pgcrypto](https://www.postgresql.org/docs/9.6/static/pgcrypto.html) | Stellt kryptografische Funktionen bereit |
| [pg\_partman](https://pgxn.org/dist/pg_partman/doc/pg_partman.html) | Verwaltet partitionierte Tabellen nach Zeit oder ID |
| [pg\_trgm](https://www.postgresql.org/docs/9.6/static/pgtrgm.html) | Stellt Funktionen und Operatoren bereit, um die Ähnlichkeit von alphanumerischen Texten basierend auf übereinstimmenden Trigrammen zu ermitteln |
| [tablefunc](https://www.postgresql.org/docs/9.6/static/tablefunc.html) | Stellt Funktionen bereit, um ganze Tabellen einschließlich einer Kreuztabelle zu bearbeiten. |
| [uuid-ossp](https://www.postgresql.org/docs/9.6/static/uuid-ossp.html) | Generiert universell eindeutige Bezeichner (UUIDs) |

### <a name="full-text-search-extensions"></a>Erweiterungen für die Volltextsuche

> [!div class="mx-tableFixed"]
| **Erweiterung** | **Beschreibung** |
|---|---|
| [dict\_int](https://www.postgresql.org/docs/9.6/static/dict-int.html) | Bietet eine Textsuch-Wörterbuchvorlage für Integerwerte. |
| [unaccent](https://www.postgresql.org/docs/9.6/static/unaccent.html) | Ein Wörterbuch für die Textsuche, das Akzente (diakritische Zeichen) aus Lexemen entfernt |

### <a name="index-types-extensions"></a>Indextypenerweiterungen

> [!div class="mx-tableFixed"]
| **Erweiterung** | **Beschreibung** |
|---|---|
| [btree\_gin](https://www.postgresql.org/docs/9.6/static/btree-gin.html) | Gibt Beispielklassen für den GIN-Operator an, der das B-Struktur-ähnliche Verhalten für bestimmte Datentypen implementiert |
| [btree\_gist](https://www.postgresql.org/docs/9.6/static/btree-gist.html) | Gibt Operatorklassen für den GiST-Index an, der die B-Struktur implementiert |

### <a name="language-extensions"></a>Spracherweiterungen

> [!div class="mx-tableFixed"]
| **Erweiterung** | **Beschreibung** |
|---|---|
| [plpgsql](https://www.postgresql.org/docs/9.6/static/plpgsql.html) | Ladbare prozedurale Sprache, PL/pgSQL. |
| [plv8](https://plv8.github.io/) | Eine Javascript-Spracherweiterung für PostgreSQL, die für gespeicherte Prozeduren, Trigger usw. verwendet werden kann |

### <a name="miscellaneous-extensions"></a>Verschiedene Erweiterungen

> [!div class="mx-tableFixed"]
| **Erweiterung** | **Beschreibung** |
|---|---|
| [pg\_buffercache](https://www.postgresql.org/docs/9.6/static/pgbuffercache.html) | Bietet eine Möglichkeit, um die Vorgänge im freigegebenen Puffercache in Echtzeit mitzuverfolgen |
| [pg\_prewarm](https://www.postgresql.org/docs/9.6/static/pgprewarm.html) | Bietet eine Möglichkeit, um Relationsdaten in den Puffercache zu laden |
| [pg\_stat\_statements](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) | Bietet eine Möglichkeit, um Ausführungsstatistiken zu allen SQL-Anweisungen nachzuverfolgen, die von einem Server ausgeführt werden (Eine Notiz zu dieser Erweiterung finden Sie unten.) |
| [pgrowlocks](https://www.postgresql.org/docs/9.6/static/pgrowlocks.html) | Bietet eine Möglichkeit zum Anzeigen von Sperrinformationen auf Zeilenebene. |
| [pgstattuple](https://www.postgresql.org/docs/9.6/static/pgstattuple.html) | Bietet eine Möglichkeit zum Anzeigen von Statistiken auf Tupelebene. |
| [postgres\_fdw](https://www.postgresql.org/docs/9.6/static/postgres-fdw.html) | Wrapper für programmfremde Daten, um auf in externen PostgreSQL-Servern gespeicherten Daten zuzugreifen |
| [hypopg](https://hypopg.readthedocs.io/en/latest/) | Stellt eine Möglichkeit zum Erstellen hypothetischer Indizes bereit, die weder CPU noch Speicherplatz benötigen. |
| [dblink](https://www.postgresql.org/docs/current/dblink.html) | Ein Modul, das Verbindungen mit anderen PostgreSQL-Datenbanken innerhalb einer Datenbanksitzung unterstützt |


### <a name="postgis-extensions"></a>PostGIS-Erweiterungen

> [!div class="mx-tableFixed"]
| **Erweiterung** | **Beschreibung** |
|---|---|
| [PostGIS](http://www.postgis.net/), postgis\_topology, postgis\_tiger\_geocoder, postgis\_sfcgal | Räumliche und geografische Objekte für PostgreSQL |
| address\_standardizer, address\_standardizer\_data\_us | Wird verwendet, um eine Adresse in dessen zugehörige Elemente zu analysieren. Wird als Unterstützung für den Normalisierungsschritt zur Geocodierung von Adressen verwendet. |
| [pgrouting](https://pgrouting.org/) | Erweitert die PostGIS/PostgreSQL-Geodatenbank um die Bereitstellung von Funktionen zum räumlichen Routing |


### <a name="using-pgstatstatements"></a>Verwenden von pg_stat_statements
Die [pg\_stat\_statements-Erweiterung](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) wird auf jedem Azure Database for PostgreSQL-Server vorab geladen,um Ihnen eine Möglichkeit zur Nachverfolgung von Ausführungsstatistiken von SQL-Anweisungen bereitzustellen.
Die Einstellung `pg_stat_statements.track`, die steuert, welche Anweisungen von der Erweiterung gezählt werden, ist standardmäßig auf `top` festgelegt, was bedeutet, dass alle Anweisungen, die direkt von Clients ausgestellt werden, nachverfolgt werden. Die beiden anderen Nachverfolgungsebenen sind `none` und `all`. Diese Einstellung kann als Serverparameter über das [Azure-Portal](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-portal) oder die [Azure CLI](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-cli) konfiguriert werden.

Zwischen den von pg_stat_statements bereitgestellten Abfrageausführungsinformationen und der Auswirkung auf die Serverleistung besteht ein Kompromiss, da jede SQL-Anweisung protokolliert wird. Wenn Sie die pg_stat_statements-Erweiterung nicht aktiv verwenden, empfiehlt es sich, `pg_stat_statements.track` auf `none` festzulegen. Beachten Sie, dass sich einige Überwachungsdienste von Drittanbietern auf pg_stat_statements beziehen können, um Statistiken zur Abfrageleistung zu liefern. Bestätigen Sie also, ob dieser Fall auf Sie zutrifft oder nicht.


## <a name="next-steps"></a>Nächste Schritte
Wenn Sie eine Erweiterung fehlt, die Sie verwenden möchten, lassen Sie es uns wissen. Stimmen Sie in unserem [Kundenfeedback-Forum](https://feedback.azure.com/forums/597976-azure-database-for-postgresql) über vorhandene Anfragen ab, oder teilen Sie uns weiteres Feedback und Wünsche mit.
