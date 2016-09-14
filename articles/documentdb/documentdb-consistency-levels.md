<properties
	pageTitle="Konsistenzebenen in DocumentDB | Microsoft Azure"
	description="DocumentDB bietet vier Konsistenzebenen, um für vorhersehbare Kompromisse zwischen Konsistenz, Verfügbarkeit und Latenz sorgen zu können."
	keywords="Eventual Consistency, DocumentDB, Azure, Microsoft Azure"
	services="documentdb"
	authors="mimig1"
	manager="jhubbard"
	editor="cgronlun"
	documentationCenter=""/>

<tags
	ms.service="documentdb"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/24/2016"
	ms.author="mimig"/>

# Konsistenzebenen in DocumentDB

Azure DocumentDB ist von Grund auf im Hinblick auf eine globale Verteilung konzipiert. Ziel ist das Bieten vorhersehbarer Garantien niedriger Latenz, einer SLA mit einer Verfügbarkeit von 99,99% und mehrerer überlegt definierter gelockerter Konsistenzmodelle. DocumentDB bietet derzeit vier Konsistenzebenen: STARK, BEGRENZTE VERALTUNG, SITZUNG und LETZTLICH. Neben den Konsistenzmodellen **STARK** und **LETZTLICH**, die meist von anderen NoSQL-Datenbanken geboten werden, bietet DocumentDB auch zwei sorgfältig programmierte und operationalisierte Modelle, **BEGRENZTE VERALTUNG** und **SITZUNG**, die ihren Nutzen in praktischen Anwendungsfällen unter Beweis gestellt haben. Diese vier Konsistenzebenen ermöglichen Ihnen, fundierte Kompromisse zwischen Konsistenz, Verfügbarkeit und Latenz zu finden.

## Umfang der Konsistenz

Die Granularität der Konsistenz wird auf eine einzelne Benutzeranforderung beschränkt. Eine Schreibanforderung kann einer Transaktion des Typs „Einfügen“, „Ersetzen“, „Einfügen/Aktualisieren (Upsert)“ oder „Löschen“ (mit oder ohne Ausführung eines zugehörigen vor- oder nachgelagerten Triggers) entsprechen. Eine Schreibanforderung kann auch eine Transaktionsausführung einer gespeicherten JavaScript-Prozedur sein, die auf mehrere Dokumente in einer Partition angewendet wird. Wie Schreibvorgänge ist eine Lese-/Abfragetransaktion auch auf eine einzelne Benutzeranforderung beschränkt. Der Benutzer muss ggf. ein großes Resultset paginieren, das sich über mehrere Partitionen erstreckt, doch jede Lesetransaktion ist auf eine einzelne Seite beschränkt und erfolgt innerhalb einer einzelnen Partition.

## Konsistenzebenen

Sie können eine Standardkonsistenzebene für Ihr Datenbankkonto konfigurieren, die für alle Sammlungen (in allen Datenbanken) in diesem Datenbankkonto gilt. Standardmäßig wird für alle Lesevorgänge und Abfragen für benutzerdefinierte Ressourcen die Standardkonsistenzebene verwendet, die für das Datenbankkonto festgelegt ist. Die Konsistenzebene einer bestimmten Lese-/Abfrageanforderung kann jedoch durch Angabe des Anforderungsheaders [[x-ms-consistency-level]](https://msdn.microsoft.com/library/azure/mt632096.aspx) gelockert werden. Vom DocumentDB-Replikationsprotokoll werden vier Arten von Konsistenzebenen unterstützt, die wie nachfolgend beschrieben einen klaren Kompromiss zwischen bestimmten Konsistenzgarantien und Leistung bieten.

![DocumentDB bietet mehrere überlegt definierte (gelockerte) Konsistenzmodelle.][1]

**STARK (Strong)**:

- STARK bietet garantierte [Linearisierbarkeit](https://aphyr.com/posts/313-strong-consistency-models), was heißt, dass die Lesevorgänge auf jeden Fall die neueste Version eines Dokuments zurückgeben.
- Mit der Konsistenzebene STARK wird gewährleistet, dass ein Schreibvorgang erst sichtbar ist, nachdem er dauerhaft vom Mehrheitsquorum der Replikate bestätigt wurde. Ein Schreibvorgang wird entweder synchron dauerhaft sowohl vom primären Replikat als auch vom Quorum der sekundären Replikate bestätigt oder abgebrochen. Ein Lesevorgang wird immer von dem Mehrheitslesequorum bestätigt. Ein Client kann niemals einen unbestätigten oder unvollständigen Schreibvorgang sehen, wodurch gewährleistet wird, dass er immer auf die neuesten bestätigten Schreibvorgänge zugreift.
- DocumentDB-Konten, die mit dem Konsistenzmodell STARK konfiguriert sind, kann nur eine Azure-Region zugeordnet werden.
- Die Kosten eines Lesevorgangs (im Sinne genutzter [Anforderungseinheiten](documentdb-request-units.md)) mit der Konsistenzebene STARK sind höher als bei SITZUNG und LETZTLICH, jedoch identisch mit BEGRENZTE VERALTUNG.
 

**Begrenzte Veraltung (Bounded staleness)**:

- Die Konsistenzebene BEGRENZTE VERALTUNG garantiert, dass Lesevorgänge hinter Schreibvorgängen höchstens *K* Versionen oder Präfixe eines Dokuments oder mit dem Zeitintervall *t* zurückbleiben.
- Bei Wahl von BEGRENZTE VERALTUNG kann „Veraltung“ deshalb auf zwei Weisen konfiguriert werden:
    - Anzahl der *K* Versionen des Dokuments, um die die Lesevorgänge hinter den Schreibvorgängen zurückbleiben
    - Zeitintervall *t*
- BEGRENZTE VERALTUNG bietet eine vollständige globale Reihenfolge außer innerhalb des „Veraltungsfensters“. Beachten Sie, dass die monotonen Lesegarantien innerhalb einer Region sowohl innerhalb als auch außerhalb des „Veraltungsfensters“ vorliegen.
- BEGRENZTE VERALTUNG bietet eine stärkere Konsistenzgarantie als die Konsistenzebenen SITZUNG und LETZTLICH. Für global verteilte Anwendungen wird empfohlen, BEGRENZTE VERALTUNG in Szenarien zu nutzen, in denen Sie eine hohe Konsistenz, aber auch eine Verfügbarkeit von 99,99% und niedrige Latenz wünschen.
- DocumentDB-Konten, die mit der Konsistenzebene BEGRENZTE VERALTUNG konfiguriert sind, kann eine beliebige Anzahl von Azure-Regionen zugeordnet werden.
- Die Kosten eines Lesevorgangs (im Sinne genutzter Anforderungseinheiten) mit der Konsistenzebene BEGRENZTE VERALTUNG sind höher als bei SITZUNG und LETZTLICH, jedoch identisch mit STARKE KONSISTENZ.

**Sitzung (Session)**:

- Anders als die globalen Konsistenzmodelle, die von den Konsistenzebenen STARK und BEGRENZTE VERALTUNG geboten werden, ist die Konsistenzebene SITZUNG auf eine bestimmte Clientsitzung beschränkt.
- Die Konsistenzebene SITZUNG ist ideal für alle Szenarien, an denen eine Geräte- oder Benutzersitzung beteiligt ist, da sie monotone Lese- und Schreibvorgänge garantiert und RYW-Garantien bietet (Read Your Own Writes, eigene Schreibvorgänge lesen).
- Die Konsistenzebene SITZUNG bietet vorhersagbare Konsistenz für eine Sitzung, einen maximalen Lesedurchsatz und Lese- und Schreibvorgänge mit niedrigster Latenz.
- DocumentDB-Konten, die mit der Konsistenzebene SITZUNG konfiguriert sind, kann eine beliebige Anzahl von Azure-Regionen zugeordnet werden.
- Die Kosten für einen Lesevorgang (hinsichtlich genutzter Anforderungseinheiten) mit der Konsistenzebene SITZUNG sind niedriger als bei STARK und BEGRENZTE VERALTUNG, aber höher als bei LETZTLICH.
 

**Letztlich (Eventual)**:

- Die Konsistenzebene LETZTLICH garantiert bei Fehlen weiterer Schreibvorgänge, dass es bei den Replikaten innerhalb der Gruppe letztendlich zur Konvergenz kommt.
- Die Konsistenzebene LETZTLICH stellt die schwächste Form von Konsistenz dar, bei der ein Client ggf. ältere Werte als die ihm zuvor angezeigten abruft.
- Die Konsistenzebene LETZTLICH bietet die schwächste Lesekonsistenz, jedoch die niedrigste Latenz für Lese- und Schreibvorgänge.
- DocumentDB-Konten, die mit der Konsistenzebene LETZTLICH konfiguriert sind, kann eine beliebige Anzahl von Azure-Regionen zugeordnet werden.
- Die Kosten für einen Lesevorgang (hinsichtlich genutzter Anforderungseinheiten) mit der Konsistenzebene LETZTLICH sind die niedrigsten aller Konsistenzebenen von DocumentDB.


## Konsistenzgarantien

In der folgenden Tabelle sind die verschiedenen Konsistenzgarantien entsprechend den vier Konsistenzebenen beschrieben.

| Garantie | Stark (Strong) | Begreznte Veraltung (Bounded Staleness) | Sitzung (Session) | Letztlich (Eventual) |
|----------------------------------------------------------|-------------------------------------------------|------------------------------------------------------------------------------------------------|--------------------------------------------------|--------------------------------------------------|
| **Vollständige globale Reihenfolge** | Ja | Ja, außerhalb des „Veraltungsfensters“ | Nein, teilweise Sitzungsreihenfolge | Nein |
| **Konsistente Präfixgarantie** | Ja | Ja | Ja | Ja |
| **Monotone Lesevorgänge** | Ja | Ja, in Regionen außerhalb des „Veraltungsfensters“ und durchweg innerhalb einer Region. | Ja, für die angegebene Sitzung | Nein |
| **Monotone Schreibvorgänge** | Ja | Ja | Ja | Ja |
| **Lesen der eigenen Schreibvorgänge** | Ja | Ja | Ja (in der Region des Schreibvorgangs) | Nein |


## Konfigurieren der Standardkonsistenzebene

1.  Klicken Sie im [Azure-Portal](https://portal.azure.com/) in der Navigationsleiste auf **DocumentDB (NoSQL)**.

2. Wählen Sie auf dem Blatt **DocumentDB (NoSQL)** das zu ändernde Datenbankkonto aus.

3. Klicken Sie auf dem Kontoblatt auf **Standardkonsistenz**.


4. Wählen Sie im Blatt **Standardkonsistenz** die neue Konsistenzebene aus und klicken Sie auf **Speichern**.

	![Screenshot mit dem Symbol „Einstellungen“ und dem Eintrag „Standardkonsistenz“](./media/documentdb-consistency-levels/database-consistency-level-1.png)

## Konsistenzebenen für Abfragen

Bei benutzerdefinierten Ressourcen entspricht die Konsistenzebene für Abfragen standardmäßig der Konsistenzebene für Lesevorgänge. Der Index wird bei jedem Einfügen, Ersetzen oder Löschen eines Dokuments der Sammlung standardmäßig synchron aktualisiert. Auf diese Weise können die Abfragen dieselbe Konsistenzebene wie die von Dokumentlesevorgängen berücksichtigen. Obwohl DocumentDB für Schreibvorgänge optimiert ist und beständige Mengen an Dokumentschreibvorgängen, die synchrone Indexwartung und Bereitstellung konsistenter Abfragen unterstützt, können Sie bestimmte Sammlungen so konfigurieren, dass ihr Index verzögert aktualisiert wird. Die verzögerte Indizierung steigert die Schreibleistung noch weiter und ist ideal für Sammelerfassungsszenarien mit einer starken Lesearbeitsauslastung geeignet.

Indizierungsmodus|	Lesevorgänge|	Abfragen  
-------------|-------|---------
Konsistent (Standard)|	Wählen Sie STARK, BEGRENZTE VERALTUNG, SITZUNG oder LETZTLICH.|	Wählen Sie STARK, BEGRENZTE VERALTUNG, SITZUNG oder LETZTLICH.|
Verzögert|	Wählen Sie STARK, BEGRENZTE VERALTUNG, SITZUNG oder LETZTLICH.|	Letztlich (Eventual)  

Wie bei Leseanforderungen kann die Konsistenzebene einer bestimmten Abfrageanforderung durch Angabe des Anforderungsheaders [x-ms-consistency-level](https://msdn.microsoft.com/library/azure/mt632096.aspx) heruntergestuft werden.

## Nächste Schritte

Wenn Sie weitere Informationen zu Konsistenzebenen und deren Vor- und Nachteile möchten, empfehlen wir die folgenden Ressourcen:

-	Doug Terry. Replicated Data Consistency explained through baseball (Video) [https://www.youtube.com/watch?v=gluIh8zd26I](https://www.youtube.com/watch?v=gluIh8zd26I) (Konsistenz replizierter Daten – erläutert am Beispiel Baseball)
-	Doug Terry. Replicated Data Consistency explained through baseball. [http://research.microsoft.com/pubs/157411/ConsistencyAndBaseballReport.pdf](http://research.microsoft.com/pubs/157411/ConsistencyAndBaseballReport.pdf)
-	Doug Terry. Sitzungsgarantien für schwach konsistente replizierte Daten. [http://dl.acm.org/citation.cfm?id=383631](http://dl.acm.org/citation.cfm?id=383631)
-	Daniel Abadi. Konsistenzkompromisse im Design moderner verteilter Datenbanksysteme: CAP ist nur ein Teil der Wahrheit. [http://computer.org/csdl/mags/co/2012/02/mco2012020037-abs.html](http://computer.org/csdl/mags/co/2012/02/mco2012020037-abs.html)
-	Peter Bailis, Shivaram Venkataraman, Michael J. Franklin, Joseph M. Hellerstein, Ion Stoica. Probabilistic Bounded Staleness (PBS) for Practical Partial Quorums. [http://vldb.org/pvldb/vol5/p776\_peterbailis\_vldb2012.pdf](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
-	Werner Vogels. Eventual Consistent - Revisited. [http://allthingsdistributed.com/2008/12/eventually\_consistent.html](http://allthingsdistributed.com/2008/12/eventually_consistent.html)


[1]: ./media/documentdb-consistency-levels/consistency-tradeoffs.png

<!---HONumber=AcomDC_0831_2016-->