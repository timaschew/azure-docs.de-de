---
title: Hochverfügbarkeit in Azure Cosmos DB
description: Dieser Artikel beschreibt, wie Azure Cosmos DB Hochverfügbarkeit bietet.
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/15/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: dd018dca2de018733783605bfdb2802f91ebd76b
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51621172"
---
# <a name="high-availability-with-azure-cosmos-db"></a>Hochverfügbarkeit mit Azure Cosmos DB

Azure Cosmos DB repliziert Ihre Daten transparent in allen Azure-Regionen, die Ihrem Cosmos-Konto zugewiesen sind. Cosmos DB verwendet mehrere Redundanzebenen für Ihre Daten, wie in der folgenden Abbildung dargestellt:

![Ressourcenpartitionierung](./media/high-availability/figure1.png)

- Die Daten in Cosmos-Containern sind horizontal partitioniert.

- In jeder Region wird jede Partition durch einen Replikatsatz geschützt. Dabei werden alle Schreibvorgänge repliziert, und die Mehrheit der Replikate führt ein dauerhaftes Commit für sie aus. Replikate werden auf 10 bis 20 Fehlerdomänen verteilt.

- Jede Partition wird in allen Regionen repliziert. Jede Region enthält alle Datenpartitionen eines Cosmos-Containers und kann Schreibvorgänge zulassen sowie Lesevorgänge verarbeiten.  

Wenn Ihr Cosmos-Konto über n Azure-Regionen verteilt ist, gibt es mindestens n x 4 Kopien von allen Ihren Daten. Neben dem Zugriff mit geringer Latenz auf Ihre Daten und der Skalierung des Schreib- und Lesedurchsatzes in den Regionen, die Ihrem Cosmos-Konto zugeordnet sind, verbessert die Bereitstellung mehrerer Regionen (größer als N) die Verfügbarkeit.  

## <a name="slas-for-availability"></a>SLAs für Verfügbarkeit

Als global verteilte Datenbank bietet Cosmos DB umfangreiche SLAs, die Durchsatz, Latenz im 99. Perzentil, Konsistenz und Hochverfügbarkeit umfassen. In der Tabelle unten sind die Garantieangaben für die Hochverfügbarkeit aufgeführt, die von Cosmos DB für Konten mit einer oder mehreren Regionen gelten. Für Hochverfügbarkeit konfigurieren Sie Ihre Cosmos-Konten mit mehreren Schreibregionen.

|Vorgangsart  | Eine Region |Mehrere Regionen (eine Schreibregion)|Mehrere Regionen (mehrere Schreibregionen) |
|---------|---------|---------|-------|
|Schreibvorgänge    | 99,99    |99,99   |99,999|
|Lesevorgänge     | 99,99    |99,999  |99,999|

> [!NOTE]
> In der Praxis ist die tatsächliche Verfügbarkeit von Schreibvorgängen für die Modelle „Begrenzte Veraltung“, „Sitzung“, „Präfixkonsistenz“ und „Letztliche Konsistenz“ deutlich höher als die veröffentlichten SLAs. Die tatsächliche Verfügbarkeit von Lesevorgängen ist für alle Konsistenzebenen deutlich höher als die veröffentlichten SLAs.

## <a name="high-availability-with-cosmos-db-in-the-face-of-regional-outages"></a>Hochverfügbarkeit mit Cosmos DB bei regionalen Ausfällen

Regionale Ausfälle sind keine Seltenheit. Deswegen stellt Azure Cosmos DB sicher, dass Ihre Datenbank immer verfügbar ist. Im Folgenden wird das Verhalten von Cosmos DB – abhängig von der Konfiguration Ihres Cosmos-Kontos – bei einem Ausfall beschrieben:

- Bevor ein Schreibvorgang dem Client gegenüber bestätigt wird, wird bei Cosmos DB von einem Quorum von Replikaten in der Region, die die Schreibvorgänge akzeptiert, ein dauerhaftes Commit ausgeführt.

- Konten mit mehreren Regionen, die mit mehreren Schreibregionen konfiguriert sind, sind sowohl für Schreib- als auch für Lesevorgänge hoch verfügbar. Regionale Failover werden sofort ausgeführt und erfordern keine Änderungen von der Anwendung.

- Konten mit mehreren Regionen mit einer einzelnen Schreibregion bleiben bei Ausfall einer Schreibregion hochverfügbar für Lesevorgänge. Für Schreibvorgänge müssen Sie jedoch in Ihrem Cosmos-Konto die Option „Automatisches Failover aktivieren“ auswählen, damit ein Failover für die betreffende Region in eine andere zugeordnete Region ausgeführt werden kann. Das Failover erfolgt in der Reihenfolge der von Ihnen angegebenen Priorisierung der Regionen. Schließlich, wenn die betreffende Region wieder online ist, werden die nicht replizierten Daten, die bei Ausfall in der betreffenden Schreibregion waren, über den Konfliktfeed bereitgestellt. Anwendungen können den Konfliktfeed lesen, die Konflikte auf Grundlage anwendungsspezifischer Logik lösen und aktualisierte Daten nach Bedarf in den Cosmos-Container zurückschreiben. Wenn die vom Ausfall betroffene Schreibregion wiederhergestellt ist, steht sie automatisch als Leseregion zur Verfügung. Sie können ein manuelles Failover aufrufen und die betreffende Region als Schreibregion konfigurieren. Sie können mithilfe der [Azure-Befehlszeilenschnittstelle oder des Azure-Portals](how-to-manage-database-account.md#manual-failover) ein manuelles Failover ausführen.  

- Konten mit mehreren Regionen mit einer einzelnen Schreibregion bleiben bei Ausfall einer Leseregion hochverfügbar für Lese- und Schreibvorgänge. Die betreffende Region wird automatisch von der Schreibregion getrennt und als offline gekennzeichnet. Die Cosmos DB SDKs leiten Leseaufrufe an die nächste verfügbare Region in der Liste der bevorzugten Regionen weiter. Wenn keine der Regionen in der Liste verfügbar ist, wird für die Aufrufe automatisch ein Fallback zur aktuellen Schreibregion durchgeführt. Zur Verarbeitung des Ausfalls einer Leseregion sind keine Änderungen an Ihrem Anwendungscode erforderlich. Wenn die betreffende Region wieder online ist, wird die vom Ausfall betroffene Leseregion automatisch mit der aktuellen Schreibregion synchronisiert. Sie steht dann wieder für die Verarbeitung von Leseanforderungen zur Verfügung. Nachfolgende Lesevorgänge werden an die wiederhergestellte Region weitergeleitet, ohne dass Änderungen an Ihrem Anwendungscode erforderlich sind. Sowohl beim Failover als auch beim erneuten Verknüpfen einer zuvor ausgefallene Region gelten weiterhin die Lesekonsistenzgarantien von Cosmos DB.

- Konten mit einer Region sind nach einem regionalen Ausfall ggf. nicht mehr verfügbar. Es wird empfohlen, mindestens zwei Regionen (idealerweise mindestens zwei Schreibregionen) für Ihr Cosmos-Konto einzurichten, um die permanente Hochverfügbarkeit sicherzustellen.

- Auch in einem extrem seltenen und unglücklichen Fall, in dem die Azure-Region dauerhaft nicht mehr wiederhergestellt werden kann, kommt es nicht zu potenziellem Datenverlust, wenn für Ihr Cosmos-Konto mit mehreren Regionen die Standardkonsistenzebene „Sicher“ konfiguriert ist. Im Falle einer dauerhaft nicht mehr wiederherstellbaren Schreibregion ist das Fenster für potenzielle Datenverluste für die Cosmos-Konten mit mehreren Regionen, für die eine Konsistenz vom Typ „Begrenzte Veraltung“ konfiguriert ist, auf das Fenster der Veraltung begrenzt. Für die Konsistenzebenen „Sitzung“, „Präfixkonsistenz“ und „Letztlich“ ist das Fenster für potenzielle Datenverluste auf maximal fünf Sekunden begrenzt.

## <a name="building-highly-available-applications"></a>Erstellen hochverfügbarer Anwendungen

- Um Hochverfügbarkeit von Schreib- und Lesevorgängen zu garantieren, konfigurieren Sie Ihr Cosmos-Konto so, dass es mindestens zwei Regionen mit mehreren Schreibregionen umfasst. Diese Konfiguration sorgt für Verfügbarkeit, geringstmögliche Latenz und Skalierbarkeit für Lese- und Schreibvorgänge und wird durch SLAs unterstützt. Weitere Informationen finden Sie unter [Konfigurieren eines Cosmos-Kontos mit mehreren Schreibregionen](tutorial-global-distribution-sql-api.md).

- Aktivieren Sie für Cosmos-Konten mit mehreren Regionen, die mit einer einzelnen Schreibregion konfiguriert sind, [„Automatisches Failover“ über die Azure-Befehlszeilenschnittstelle oder das Azure-Portal](how-to-manage-database-account.md#automatic-failover). Wenn Sie das automatische Failover aktivieren, führt Cosmos DB bei einem regionalen Ausfall ein automatisches Failover für Ihr Konto aus.  

- Selbst wenn Ihr Cosmos-Konto hochverfügbar ist, ist Ihre Anwendung möglicherweise nicht richtig dafür konzipiert, hochverfügbar zu bleiben. Um die End-to-End-Hochverfügbarkeit für Ihre Anwendung zu testen, rufen Sie im Rahmen Ihrer Anwendungs- oder Notfallwiederherstellungstests regelmäßig über die [Azure-Befehlszeilenschnittstelle oder das Azure-Portal „Manuelles Failover“](how-to-manage-database-account.md#manual-failover) auf.

## <a name="next-steps"></a>Nächste Schritte

In den folgenden Artikeln erfahren Sie mehr über die Skalierung von Durchsatz:

* [Kompromisse in Bezug auf Verfügbarkeit und Leistung für verschiedene Konsistenzebenen](consistency-levels-tradeoffs.md)
* [Globales Skalieren von bereitgestelltem Durchsatz](scaling-throughput.md)
* [Globale Verteilung: Hintergrundinformationen](global-dist-under-the-hood.md)
* [Konsistenzebenen in Azure Cosmos DB](consistency-levels.md)