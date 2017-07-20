---
title: Konsistenzebenen in Azure Cosmos DB | Microsoft-Dokumentation
description: "Azure Cosmos DB bietet fünf Konsistenzebenen, um für vorhersehbare Kompromisse zwischen Konsistenz, Verfügbarkeit und Latenz sorgen zu können."
keywords: Letztliche Konsistenz, Azure Cosmos DB, Azure, Microsoft Azure
services: cosmos-db
author: mimig1
manager: jhubbard
editor: cgronlun
documentationcenter: 
ms.assetid: 3fe51cfa-a889-4a4a-b320-16bf871fe74c
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: mimig
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: abca1eff9d0b79420e70da5a4c551eceda478491
ms.contentlocale: de-de
ms.lasthandoff: 06/17/2017


---
# <a name="tunable-data-consistency-levels-in-azure-cosmos-db"></a>Einstellbare Datenkonsistenzebenen in Azure Cosmos DB
Azure Cosmos DB ist für jedes Datenmodell von Grund auf im Hinblick auf eine globale Verteilung konzipiert. Ziel ist das Bieten vorhersehbarer Garantien niedriger Latenz, einer SLA mit einer Verfügbarkeit von 99,99% und mehrerer überlegt definierter gelockerter Konsistenzmodelle. Azure Cosmos DB bietet derzeit fünf Konsistenzebenen: Stark, Begrenzte Veraltung, Sitzung, Konsistentes Präfix und Letztlich. 

Neben den Modellen **Starke Konsistenz** und **Letztliche Konsistenz**, die üblicherweise von anderen verteilten Datenbanken geboten werden, bietet Azure Cosmos DB drei weitere sorgfältig programmierte und operationalisierte Modelle, die ihren Nutzen in praktischen Anwendungsfällen unter Beweis gestellt haben. Dies sind die Konsistenzebenen **Begrenzte Veraltung**, **Sitzung** und **Präfixkonsistenz**. Diese fünf Konsistenzebenen ermöglichen Ihnen, fundierte Kompromisse zwischen Konsistenz, Verfügbarkeit und Latenz zu finden. 

## <a name="distributed-databases-and-consistency"></a>Verteilte Datenbanken und Konsistenz
Kommerziell verteilte Datenbanken werden in zwei Kategorien unterteilt: Datenbanken, die keinerlei klar definierte, belegbare Konsistenzoptionen bieten, und Datenbanken, die zwei gegensätzliche Programmierbarkeitsoptionen (starke Konsistenz oder letztliche Konsistenz) bieten. 

Erstere liefern Anwendungsentwicklern minuziöse Replikationsprotokolle und bürden ihnen die schwierige Aufgabe auf, zwischen Konsistenz, Verfügbarkeit, Wartezeit und Durchsatz abzuwägen. Letztere setzen die Anwendungsentwickler unter Druck, eine der beiden Extreme auszuwählen. Trotz der Fülle der Untersuchungen und Vorschläge für mehr als 50 Konsistenzmodelle war die verteilte Datenbankcommunity nicht in der Lage, Konsistenzebenen über starke und letztliche Konsistenz kommerzialisieren. Cosmos DB bietet Entwicklern die Auswahl zwischen fünf klar definierten Konsistenzmodellen aus dem gesamten Konsistenzspektrum: Starke Konsistenz, Begrenzte Veraltung, [Sitzungskonsistenz](http://dl.acm.org/citation.cfm?id=383631), Präfixkonsistenz und Letztliche Konsistenz. 

![Azure Cosmos DB bietet mehrere überlegt definierte (gelockerte) Konsistenzmodelle.](./media/consistency-levels/five-consistency-levels.png)

Die folgende Tabelle zeigt die speziellen Garantien der einzelnen Konsistenzebenen.
 
**Konsistenzebenen und Garantien**

| Konsistenzebene | Garantien |
| --- | --- |
| STARK (Strong) | Linearisierbarkeit |
| Begrenzte Veraltung (Bounded staleness) | Präfixkonsistenz. Lesevorgänge bleiben hinter Schreibvorgängen um Präfix k oder Intervall t zurück |
| Sitzung   | Präfixkonsistenz. Monotone Lesevorgänge, monotone Schreibvorgänge, Lesen der eigenen Schreibvorgänge, Schreibvorgänge folgen Lesevorgängen |
| Präfixkonsistenz | Die zurückgegebenen Updates sind ein bestimmtes Präfix aller Updates ohne Lücken |
| Letztlich (Eventual)  | Lesevorgänge in falscher Reihenfolge |

Sie können die Standardkonsistenzebene für Ihr Cosmos DB-Konto konfigurieren (und die Konsistenz später für eine bestimmte Leseanforderung außer Kraft setzen). Intern gilt die Standardkonsistenzebene für Daten in den Partitionssätzen, die sich auf mehrere Regionen erstrecken können. Ca. 73 % unsere Mandanten arbeiten mit Sitzungskonsistenz, und 20 % bevorzugen begrenzte Veraltung. Wir beobachten, dass ca. 3 % unserer Kunden anfänglich mit verschiedenen Konsistenzebenen experimentieren, ehe sie für ihre Anwendung eine bestimmte Konsistenzoption wählen. Wir beobachten auch, dass nur 2 % unserer Mandanten Konsistenzebenen anforderungsbezogen überschreiben. 

In Cosmos DB sind Lesevorgänge mit Sitzungs-, Präfix- und letztlicher Konsistenz zweimal günstiger als Lesevorgänge mit starker Konsistenz oder begrenzter Veraltung. Cosmos DB bietet branchenführende umfassende SLAs für Verfügbarkeit (99,99 %), Durchsatz und Wartezeit sowie Konsistenzgarantien. Wir setzen eine [Linearisierbarkeitsprüfung](http://dl.acm.org/citation.cfm?id=1806634) ein, die fortlaufend auf unsere Diensttelemetriedaten angewendet wird und Ihnen sämtliche Konsistenzverstöße offen meldet. Für die Konsistenzebene „Begrenzte Veraltung“ überwachen und melden wir sämtliche Verstöße gegen die Grenzen „k“ und „t“. Für alle fünf gelockerten Konsistenzebenen melden wir Ihnen direkt die [Metrik „Probabilistisch begrenzte Veraltung“](http://dl.acm.org/citation.cfm?id=2212359).  

## <a name="scope-of-consistency"></a>Umfang der Konsistenz
Die Granularität der Konsistenz wird auf eine einzelne Benutzeranforderung beschränkt. Eine Schreibanforderung kann einer Transaktion des Typs „Einfügen“, „Ersetzen“, „Einfügen/Aktualisieren (Upsert)“ oder „Löschen“ entsprechen. Wie bei Schreibvorgängen ist eine Lese-/Abfragetransaktion auch auf eine einzelne Benutzeranforderung beschränkt. Der Benutzer muss ggf. ein großes Resultset paginieren, das sich über mehrere Partitionen erstreckt, doch jede Lesetransaktion ist auf eine einzelne Seite beschränkt und erfolgt innerhalb einer einzelnen Partition.

## <a name="consistency-levels"></a>Konsistenzebenen
Sie können eine Standardkonsistenzebene für Ihr Datenbankkonto konfigurieren, die für alle Sammlungen (und Datenbanken) in Ihrem Cosmos DB-Konto gilt. Standardmäßig wird für alle Lesevorgänge und Abfragen für benutzerdefinierte Ressourcen die Standardkonsistenzebene verwendet, die für das Datenbankkonto festgelegt ist. Sie können die Konsistenzebene einer bestimmten Lese-/Abfrageanforderung mithilfe der einzelnen unterstützten APIs lockern. Vom Azure Cosmos DB-Replikationsprotokoll werden fünf Arten von Konsistenzebenen unterstützt, die (wie in diesem Abschnitt beschrieben) einen klaren Kompromiss zwischen bestimmten Konsistenzgarantien und Leistung bieten.

**Starke Konsistenz**: 

* „Starke Konsistenz“ bietet garantierte [Linearisierbarkeit](https://aphyr.com/posts/313-strong-consistency-models), was heißt, dass die Lesevorgänge auf jeden Fall die neueste Version eines Elements zurückgeben. 
* Mit der Konsistenzebene STARK wird gewährleistet, dass ein Schreibvorgang erst sichtbar ist, nachdem er dauerhaft vom Mehrheitsquorum der Replikate bestätigt wurde. Ein Schreibvorgang wird entweder synchron dauerhaft sowohl vom primären Replikat als auch vom Quorum der sekundären Replikate bestätigt oder abgebrochen. Ein Lesevorgang wird immer von dem Mehrheitslesequorum bestätigt. Ein Client kann niemals einen unbestätigten oder unvollständigen Schreibvorgang sehen, wodurch gewährleistet wird, dass er immer auf die neuesten bestätigten Schreibvorgänge zugreift. 
* Azure Cosmos DB-Konten, die mit dem Modell „Starke Konsistenz“ konfiguriert sind, kann nur eine Azure-Region zugeordnet werden. 
* Die Kosten eines Lesevorgangs (im Sinne genutzter [Anforderungseinheiten](request-units.md) ) mit der Konsistenzebene STARK sind höher als bei SITZUNG und LETZTLICH, jedoch identisch mit BEGRENZTE VERALTUNG.

**Begrenzte Veraltung (Bounded staleness)**: 

* Die Konsistenzebene „Begrenzte Veraltung“ garantiert, dass Lesevorgänge hinter Schreibvorgängen höchstens *K* Versionen oder Präfixe eines Elements oder mit dem Zeitintervall *t* zurückbleiben. 
* Bei Wahl von „Begrenzte Veraltung“ kann die „Veraltung“ auf zwei Weisen konfiguriert werden: Anzahl der *K*-Versionen des Elements, um das die Lesevorgänge den Schreibvorgängen hinterherhinken, und das Zeitintervall *t*. 
* Begrenzte Veraltung bietet eine vollständige globale Reihenfolge außer innerhalb des „Veraltungsfensters“. Die monotonen Lesegarantien liegen innerhalb einer Region sowohl innerhalb als auch außerhalb des „Veraltungsfensters“ vor. 
* BEGRENZTE VERALTUNG bietet eine stärkere Konsistenzgarantie als die Konsistenzebenen SITZUNG und LETZTLICH. Für global verteilte Anwendungen wird empfohlen, BEGRENZTE VERALTUNG in Szenarien zu nutzen, in denen Sie eine hohe Konsistenz, aber auch eine Verfügbarkeit von 99,99% und niedrige Latenz wünschen. 
* Azure Cosmos DB-Konten, die mit der Konsistenz „Begrenzte Veraltung“ konfiguriert sind, kann eine beliebige Anzahl von Azure-Regionen zugeordnet werden. 
* Die Kosten eines Lesevorgangs (im Sinne genutzter Anforderungseinheiten) mit der Konsistenzebene BEGRENZTE VERALTUNG sind höher als bei SITZUNG und LETZTLICH, jedoch identisch mit STARKE KONSISTENZ.

**Sitzung (Session)**: 

* Anders als die globalen Konsistenzmodelle, die von den Konsistenzebenen STARK und BEGRENZTE VERALTUNG geboten werden, ist die Konsistenzebene SITZUNG auf eine bestimmte Clientsitzung beschränkt. 
* Die Konsistenzebene SITZUNG ist ideal für alle Szenarien, an denen eine Geräte- oder Benutzersitzung beteiligt ist, da sie monotone Lese- und Schreibvorgänge garantiert und RYW-Garantien bietet (Read Your Own Writes, eigene Schreibvorgänge lesen). 
* Die Konsistenzebene SITZUNG bietet vorhersagbare Konsistenz für eine Sitzung, einen maximalen Lesedurchsatz und Lese- und Schreibvorgänge mit niedrigster Latenz. 
* Azure Cosmos DB-Konten, die mit „Sitzungskonsistenz“ konfiguriert sind, kann eine beliebige Anzahl von Azure-Regionen zugeordnet werden. 
* Die Kosten für einen Lesevorgang (hinsichtlich genutzter Anforderungseinheiten) mit der Konsistenzebene SITZUNG sind niedriger als bei STARK und BEGRENZTE VERALTUNG, aber höher als bei LETZTLICH.

<a id="consistent-prefix"></a>
**Präfixkonsistenz**: 

* Die Präfixkonsistenz garantiert bei Fehlen weiterer Schreibvorgänge, dass es bei den Replikaten innerhalb der Gruppe letztendlich zur Konvergenz kommt. 
* Präfixkonsistenz stellt sicher, dass Lesevorgänge keine Schreibvorgänge außerhalb der Reihenfolge angezeigt werden. Wenn Schreibvorgänge in der Reihenfolge `A, B, C` erfolgen, wird einem Client entweder `A`, `A,B` oder `A,B,C`, aber niemals eine falsche Reihenfolge wie `A,C` oder `B,A,C` angezeigt.
* Azure Cosmos DB-Konten, die mit Präfixkonsistenz konfiguriert sind, kann eine beliebige Anzahl von Azure-Regionen zugeordnet werden. 

**Letztlich (Eventual)**: 

* „Letztliche Konsistenz“ garantiert bei Fehlen weiterer Schreibvorgänge, dass es bei den Replikaten innerhalb der Gruppe letztendlich zur Konvergenz kommt. 
* Die Konsistenzebene LETZTLICH stellt die schwächste Form von Konsistenz dar, bei der ein Client ggf. ältere Werte als die ihm zuvor angezeigten abruft.
* Die Konsistenzebene LETZTLICH bietet die schwächste Lesekonsistenz, jedoch die niedrigste Latenz für Lese- und Schreibvorgänge.
* Azure Cosmos DB-Konten, die mit „Letztliche Konsistenz“ konfiguriert sind, kann eine beliebige Anzahl von Azure-Regionen zugeordnet werden. 
* Die Kosten eines Lesevorgangs (hinsichtlich genutzter Anforderungseinheiten) mit der Ebene „Letztliche Konsistenz“ sind die niedrigsten aller Konsistenzebenen von Azure Cosmos DB.

## <a name="configuring-the-default-consistency-level"></a>Konfigurieren der Standardkonsistenzebene
1. Klicken Sie im [Azure-Portal](https://portal.azure.com/) auf der linken Navigationsleiste auf **Azure Cosmos DB**.
2. Wählen Sie auf dem Blatt **Azure Cosmos DB** das zu ändernde Datenbankkonto aus.
3. Klicken Sie auf dem Kontoblatt auf **Standardkonsistenz**.
4. Wählen Sie im Blatt **Standardkonsistenz** die neue Konsistenzebene aus und klicken Sie auf **Speichern**.
   
    ![Screenshot mit dem Symbol „Einstellungen“ und dem Eintrag „Standardkonsistenz“](./media/consistency-levels/database-consistency-level-1.png)

## <a name="consistency-levels-for-queries"></a>Konsistenzebenen für Abfragen
Bei benutzerdefinierten Ressourcen entspricht die Konsistenzebene für Abfragen standardmäßig der Konsistenzebene für Lesevorgänge. Der Index wird bei jedem Einfügen, Ersetzen oder Löschen eines Dokuments eines Elements im Cosmos DB-Container standardmäßig synchron aktualisiert. Auf diese Weise können die Abfragen dieselbe Konsistenzebene wie die von Lesevorgängen von Datenpunkten berücksichtigen. Obwohl Azure Cosmos DB für Schreibvorgänge optimiert ist und beständige Mengen von Schreibvorgängen, die synchrone Indexwartung und Bereitstellung konsistenter Abfragen unterstützt, können Sie bestimmte Sammlungen so konfigurieren, dass ihr Index verzögert aktualisiert wird. Die verzögerte Indizierung steigert die Schreibleistung noch weiter und ist ideal für Sammelerfassungsszenarien mit einer starken Lesearbeitsauslastung geeignet.  

| Indizierungsmodus | Lesevorgänge | Abfragen |
| --- | --- | --- |
| Konsistent (Standard) |Wählen Sie „Stark“, „Begrenzte Veraltung“, „Sitzung“, „Präfixkonsistenz“ oder „Letztlich“. |Wählen Sie STARK, BEGRENZTE VERALTUNG, SITZUNG oder LETZTLICH. |
| Verzögert |Wählen Sie „Stark“, „Begrenzte Veraltung“, „Sitzung“, „Präfixkonsistenz“ oder „Letztlich“. |Letztlich (Eventual) |
| Keine |Wählen Sie „Stark“, „Begrenzte Veraltung“, „Sitzung“, „Präfixkonsistenz“ oder „Letztlich“. |Nicht zutreffend |

Wie bei Leseanforderungen kann die Konsistenzebene einer bestimmten Abfrageanforderung in jeder API heruntergestuft werden.

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie weitere Informationen zu Konsistenzebenen und deren Vor- und Nachteile möchten, empfehlen wir die folgenden Ressourcen:

* Doug Terry. Erläuterung der Konsistenz replizierter Daten anhand von Baseball (Video).   
  [https://www.youtube.com/watch?v=gluIh8zd26I](https://www.youtube.com/watch?v=gluIh8zd26I)
* Doug Terry. Replicated Data Consistency explained through baseball.   
  [http://research.microsoft.com/pubs/157411/ConsistencyAndBaseballReport.pdf](http://research.microsoft.com/pubs/157411/ConsistencyAndBaseballReport.pdf)
* Doug Terry. Sitzungsgarantien für schwach konsistente replizierte Daten.   
  [http://dl.acm.org/citation.cfm?id=383631](http://dl.acm.org/citation.cfm?id=383631)
* Daniel Abadi. Konsistenzkompromisse im Design moderner verteilter Datenbanksysteme: CAP ist nur ein Teil der Wahrheit.   
  [http://computer.org/csdl/mags/co/2012/02/mco2012020037-abs.html](http://computer.org/csdl/mags/co/2012/02/mco2012020037-abs.html)
* Peter Bailis, Shivaram Venkataraman, Michael J. Franklin, Joseph M. Hellerstein, Ion Stoica. Probabilistic Bounded Staleness (PBS) für praktische teilweise Quoren.   
  [http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
* Werner Vogels. Eventual Consistent - Revisited.    
  [http://allthingsdistributed.com/2008/12/eventually_consistent.html](http://allthingsdistributed.com/2008/12/eventually_consistent.html)
* Moni Naor, Avishai Wool: The Load, Capacity, and Availability of Quorum Systems, SIAM Journal on Computing, v.27 n.2, p.423-447, April 1998.
  [http://epubs.siam.org/doi/abs/10.1137/S0097539795281232](http://epubs.siam.org/doi/abs/10.1137/S0097539795281232)
* Sebastian Burckhardt, Chris Dern, Macanal Musuvathi, Roy Tan, Line-up: a complete and automatic linearizability checker, Proceedings of the 2010 ACM SIGPLAN conference on Programming language design and implementation, June 05-10, 2010, Toronto, Ontario, Canada [doi>10.1145/1806596.1806634] [http://dl.acm.org/citation.cfm?id=1806634](http://dl.acm.org/citation.cfm?id=1806634)
* Peter Bailis, Shivaram Venkataraman, Michael J. Franklin, Joseph M. Hellerstein, Ion Stoica: Probabilistically bounded staleness for practical partial quorums, Proceedings of the VLDB Endowment, v.5 n.8, p.776-787, April 2012 [http://dl.acm.org/citation.cfm?id=2212359](http://dl.acm.org/citation.cfm?id=2212359)

