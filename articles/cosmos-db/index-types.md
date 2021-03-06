---
title: Indextypen in Azure Cosmos DB
description: Übersicht über Indextypen in Azure Cosmos DB
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/5/2018
ms.author: rimman
ms.openlocfilehash: f3c5d7bc1907e94ff2e590fe77cc531ac4b01f4c
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51628560"
---
# <a name="index-types-in-azure-cosmos-db"></a>Indextypen in Azure Cosmos DB

Beim Konfigurieren der Indizierungsrichtlinie für einen Pfad stehen Ihnen mehrere Optionen zur Verfügung. Für jeden Pfad können Sie eine oder mehrere Indizierungsdefinitionen angeben:

- **Datentyp**: „String“, „Number“, „Point“, „Polygon“ oder „LineString“ (kann nur einen Eintrag pro Datentyp pro Pfad enthalten).

- **Indexart**: „Hash“ (Gleichheitsabfragen), „Range“ (Gleichheits-, Bereichs- und ORDER BY-Abfragen) oder „Spatial“ (räumliche Abfragen).

- **Genauigkeit**: Beim Hashindex variiert dies von 1 bis 8 für Zeichenfolgen und Zahlen und der Standardwert ist 3. Für einen Bereichsindex beträgt der Wert der maximalen Genauigkeit „-1“. Für Zeichenfolgen- oder numerische Werte kann dieser zwischen 1 und 100 (maximale Genauigkeit) variieren.

## <a name="index-kind"></a>Indexart

Azure Cosmos DB unterstützt die Indexarten „Hash“ und „Range“ für alle Pfade, die für die Datentypen „String“ und „Number“ konfiguriert werden können.

- **Index „Hash“** unterstützt effiziente Gleichheits- und JOIN-Abfragen. In den meisten Fällen benötigen Hashindizes keine höhere Genauigkeit als den Standardwert von 3 Byte. Der Datentyp kann „String“ oder „Number“ sein.

- **Index „Range“** unterstützt effiziente Gleichheitsabfragen, Bereichsabfragen (mit >, <, >=, <=, !=) und ORDER By-Abfragen. Bei ORDER BY-Abfragen muss standardmäßig auch die maximale Indexgenauigkeit (-1) angegeben werden. Der Datentyp kann „String“ oder „Number“ sein.

- **Index „Spatial“** unterstützt effiziente räumliche Abfragen zur Entfernung und zu enthaltenen Elementen. Der Datentyp kann „Point“, „Polygon“ oder „LineString“ sein. Azure Cosmos DB unterstützt auch die Indexart „Spatial“ für alle Pfade, die für den Datentyp „Point“, „Polygon“ oder „LineString“ angegeben werden können. Der Wert im angegebenen Pfad muss ein gültiges GeoJSON-Fragment wie {"type": "Point", "coordinates": [0.0, 10.0]} sein. Azure Cosmos DB unterstützt die automatische Indizierung der Datentypen „Point“, „Polygon“ und „LineString“.

Im Folgenden finden Sie einige Beispiele, wie die Indizes „Hash“, „Range“ und „Spatial“ für die Verarbeitung genutzt werden können:

| **Indexart** | **Beschreibung/Anwendungsfall** |
| ---------- | ---------------- |
| Hash  | Hash über /prop/? (oder /) kann verwendet werden, um die folgenden Abfragen effizient zu bedienen:<br><br>SELECT FROM collection c WHERE c.prop = "value"<br><br>Hash über /props/[]/? (oder / oder /props/) kann verwendet werden, um die folgenden Abfragen effizient zu bedienen:<br><br>SELECT tag FROM collection c JOIN tag IN c.props WHERE tag = 5  |
| Range  | Bereich über /prop/? (oder /) kann verwendet werden, um die folgenden Abfragen effizient zu bedienen:<br><br>SELECT FROM collection c WHERE c.prop = "value"<br><br>SELECT FROM collection c WHERE c.prop > 5<br><br>SELECT FROM collection c ORDER BY c.prop   |
| Spatial     | Bereich über /prop/? (oder /) kann verwendet werden, um die folgenden Abfragen effizient zu bedienen:<br><br>SELECT FROM collection c<br><br>WHERE ST_DISTANCE(c.prop, {"type": "Point", "coordinates": [0.0, 10.0]}) < 40<br><br>SELECT FROM collection c WHERE ST_WITHIN(c.prop, {"type": "Polygon", ... }) --mit aktivierter Indizierung nach Punkten<br><br>SELECT FROM collection c WHERE ST_WITHIN({„type": "Point", ... }, c.prop) --mit aktivierter Indizierung nach Polygonen.     |

## <a name="default-behavior-of-index-kinds"></a>Standardverhalten von Indexarten

- Standardmäßig wird ein Fehler für Abfragen mit Bereichsoperatoren wie „>=“ zurückgegeben, wenn kein Bereichsindex (mit Genauigkeit) vorhanden ist, um zu signalisieren, dass möglicherweise eine Überprüfung erforderlich ist, um die Abfrage zu verarbeiten.

- Bereichsabfragen können ohne Bereichsindex mit dem Header "x-ms-documentdb-enable-scan" in der REST-API oder mithilfe der Anforderungsoption "EnableScanInQuery" des .NET SDK ausgeführt werden. Wenn die Abfrage weitere Filter enthält, die Azure Cosmos DB für den Index verwenden kann, wird kein Fehler zurückgegeben.

- Standardmäßig wird ein Fehler für räumliche Abfragen zurückgegeben, wenn kein räumlicher Index oder andere Filter vorhanden sind, für die die Verarbeitung über den Index möglich ist. Solche Abfragen können als Scan mit "x-ms-documentdb-enable-scan" oder "EnableScanInQuery" ausgeführt werden.

## <a name="index-precision"></a>Indexgenauigkeit

- Mit der Indexgenauigkeit können Sie Abstimmungen zwischen dem Indexspeicheraufwand und der Abfrageleistung vornehmen. Bei Zahlen wird die Verwendung der Standardkonfiguration für die Genauigkeit „-1“ (maximale Genauigkeit) empfohlen. Da Zahlen in JSON eine Größe von 8 Byte aufweisen, entspricht dies einer Konfiguration von 8 Byte. Die Auswahl eines niedrigeren Werts für die Genauigkeit, z. B. 1 bis 7, bedeutet, dass Werte in einigen Bereichen dem gleichen Indexeintrag zugeordnet werden. Daher reduziert sich der Indexspeicherplatz, bei der Abfrageausführung müssen jedoch möglicherweise mehr Elemente verarbeitet werden. Folglich erhöht sich der Durchsatz in Anforderungseinheiten.

- Die Indexgenauigkeit ist eher im Zusammenhang mit Zeichenfolgenbereichen hilfreich. Da Zeichenfolgen eine beliebige Länge aufweisen können, kann sich die Wahl der Indexgenauigkeit auf die Leistung von Abfragen von Zeichenfolgenbereichen auswirken. Außerdem kann sich dies auch auf den benötigten Indexspeicherplatz auswirken. Zeichenfolgenbereich-Indizes können mit einer Indexgenauigkeit zwischen 1 und 100 oder -1 (Maximum) konfiguriert werden. Wenn Sie ORDER BY-Abfragen für Zeichenfolgeneigenschaften ausführen möchten, müssen Sie eine Genauigkeit von „-1“ für die entsprechenden Pfade angeben.

- Räumliche Indizes verwenden stets die Standardindexgenauigkeit für alle Typen („Point“, „LineString“ und „Polygon“). Die Standardindexgenauigkeit für räumliche Indizes kann nicht überschrieben werden.

Azure Cosmos DB gibt einen Fehler zurück, wenn in einer Abfrage ORDER BY verwendet wird, aber kein Bereichsindex für den abgefragten Pfad mit maximaler Genauigkeit vorhanden ist.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Indizierung in Azure Cosmos DB finden Sie in den folgenden Artikeln:

- [Übersicht über die Indizierung](index-overview.md)
- [Indizierungsrichtlinie](indexing-policies.md)
- [Indexpfade](index-paths.md)

