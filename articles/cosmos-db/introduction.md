---
title: "Einführung in Azure Cosmos DB | Microsoft-Dokumentation"
description: "Hier finden Sie Informationen zu Azure Cosmos DB. Diese global verteilte Datenbank mit mehreren Modellen ist für geringe Wartezeiten, flexible Skalierbarkeit und hohe Verfügbarkeit konzipiert."
services: cosmosdb
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: a855183f-34d4-49cc-9609-1478e465c3b7
ms.service: cosmosdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/10/2017
ms.author: mimig
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 1591a7935b10e22164d26a09c4e923101b517227
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---

# <a name="welcome-to-azure-cosmos-db"></a>Willkommen bei Azure Cosmos DB

Azure Cosmos DB ist eine global verteilte Datenbank von Microsoft mit mehreren Modellen. Mit einem Klick ermöglicht Azure Cosmos DB Ihnen, Durchsatz und Speicher elastisch und unabhängig voneinander über eine beliebige Anzahl von geografischen Azure-Regionen hinweg zu skalieren. Azure Cosmos DB bietet Ihnen mit umfassenden [Vereinbarungen zum Servicelevel](https://aka.ms/acdbsla) (Service Level Agreements, SLAs) Durchsatz-, Wartezeit-, Verfügbarkeits- und Konsistenzgarantien wie kein anderer Datenbankdienst.

![Azure Cosmos DB ist ein weltweit verteilter Datenbankdienst von Microsoft mit elastischer horizontaler Hochskalierung, garantierter geringer Wartezeit, fünf Konsistenzmodellen und umfassenden SLA-Garantien.](./media/introduction/azure-cosmos-db.png)

Azure Cosmos DB enthält ein ressourcengesteuertes, vom Schema unabhängiges Datenbankmodul mit optimierten Schreibvorgängen, die mehrere Datenmodelle nativ unterstützt: Schlüssel-Wert-, Dokumenten-, Graph- und spaltenbasierte Datenmodelle. Azure Cosmos DB unterstützt zudem viele APIs für den Zugriff auf Daten, z.B. [MongoDB](../documentdb/documentdb-protocol-mongodb.md), [DocumentDB SQL](../documentdb/documentdb-introduction.md), [Gremlin](graph-introduction.md) (Vorschauversion) und [Azure-Tabellen](table-introduction.md) (Vorschauversion), und ist erweiterbar. 

Azure Cosmos DB wurde Ende 2010 eingeführt, um die Problembereiche zu beseitigen, mit denen sich Entwickler umfangreicher Anwendungen bei Microsoft konfrontiert sahen. Da die Erstellung weltweit verteilter Anwendungen nicht nur für Microsoft Probleme mit sich bringt, haben wir den Dienst extern allen Azure-Entwicklern in Form von Azure DocumentDB zur Verfügung gestellt. Mit Azure Cosmos DB ist der nächste große Schritt in der Entwicklung von DocumentDB geschafft, daher machen wir Azure Cosmos DB jetzt für Sie verfügbar. Im Rahmen dieser Version von Azure Cosmos DB werden DocumentDB-Kunden (mit ihren Daten) automatisch zu Azure Cosmos DB-Kunden. Der Übergang erfolgt reibungslos, und Sie erhalten nun Zugriff auf eine breitere Palette neuer Funktionen von Azure Cosmos DB. 

## <a name="capability-comparison"></a>Vergleich der Funktionen

Azure Cosmos DB kombiniert die besten Funktionen relationaler und nicht relationaler Datenbanken.

| Funktionen | Relationale Datenbanken    | Nicht relationale Datenbanken (NoSQL) |     Azure Cosmos DB |
| --- | --- | --- | --- |
| Globale Verteilung | x | x | ✓ Sofort einsatzbereit, mehr als 30 Regionen, Multihosting |
| Horizontale Skalierung | x | ✓  | ✓ Speicher und Durchsatz unabhängig voneinander skalieren | 
| Wartezeitgarantien | x | ✓  | ✓ < 10 ms für Lesevorgänge, < 15 ms für Schreibvorgänge bei p99 | 
| Hohe Verfügbarkeit | x | ✓  | ✓ AlwaysOn, PACELC-Kompromisse, automatisches und manuelles Failover |
| Datenmodell + API | Relational + SQL | API mit mehreren Modellen + OSS-API | API mit mehreren Modellen + SQL-API + OSS-API (weitere demnächst verfügbar) |
| SLAs | ✓  | x | ✓ Umfassende SLAs für Wartezeit, Durchsatz, Konsistenz, Verfügbarkeit |

## <a name="key-capabilities"></a>Wichtige Funktionen
Als global verteilter Datenbankdienst bietet Azure Cosmos DB die folgenden Funktionen, die Ihnen bei der Erstellung skalierbarer, global verteilter, extrem reaktionsschneller Anwendungen helfen:

* [**Globale, sofort einsatzbereite Verteilung**](#global-distribution)
    * Ihre Anwendung steht für Ihre Benutzer überall sofort zur Verfügung. Jetzt gilt dies auch für Ihre Daten.
    * Machen Sie sich keine Sorgen wegen der Hardware. Sie brauchen keine Knoten, VMs oder Kerne hinzuzufügen. Durch einfaches Zeigen und Klicken sind Ihre Daten verfügbar. 

* [**Mehrere Datenmodelle und beliebte APIs für den Zugriff auf und die Abfrage von Daten**](#data-models)
    * Unterstützung mehrerer Datenmodelle, einschließlich Schlüssel-Wert-, Dokumenten-, Graph- und spaltenbasierte Datenmodelle.
    * Erweiterbare APIs für Node.js, Java, .NET, .NET Core, Python und MongoDB.
    * SQL und Gremlin für Abfragen. 

* [**Elastische Skalierung von Durchsatz und Speicher, bedarfsorientiert und weltweit**](#horizontal-scale)
    * Skalieren Sie den Durchsatz einfach, [sekunden-](../documentdb/documentdb-request-units.md) und [minutengenau](https://aka.ms/acdbrupm), und nehmen Sie Änderungen vor, wann immer sie möchten. 
    * Skalieren Sie den Speicher [transparent und automatisch](partition-data.md), um Ihre Größenanforderungen jetzt und ständig zu erfüllen.

* [**Erstellen extrem reaktionsschneller unternehmenskritischer Anwendungen**](#low-latency) 
    * Sie erhalten Zugriff auf Ihre Daten mit Wartezeiten, die im Bereich einstelliger Millisekundenwerte im 99. Perzentil liegen, und zwar überall auf der ganzen Welt. 

* [**Sicherstellen der AlwaysOn-Verfügbarkeit**](#high-availability)
    * 99,99 % Verfügbarkeit innerhalb einer Region.
    * Nehmen Sie die Bereitstellung für höhere Verfügbarkeit in einer beliebigen Anzahl von [Azure-Regionen](https://azure.microsoft.com/regions) vor.
    * [Simulieren Sie den Ausfall](../documentdb/documentdb-regional-failovers.md) einer oder mehrerer Regionen garantiert ohne Datenverluste. 

* [**Schreiben global verteilter Anwendungen, fehlerfrei**](#consistency)
    * [Fünf Konsistenzmodelle](../documentdb/documentdb-consistency-levels.md) bieten starke SQL-ähnliche Konsistenz mit NoSQL-ähnlicher letztlicher Konsistenz und allen Stufen dazwischen. 
  
* [**Geld-zurück-Garantien**](#sla) 
    * Ihre Daten sind schnell verfügbar, oder Sie erhalten Ihr Geld zurück. 
    * [Vereinbarungen zum Servicelevel (Service Level Agreements, SLAs)](https://aka.ms/acdbsla) für Verfügbarkeit, Wartezeit, Durchsatz und Konsistenz. 

* [**Keine Datenbankschema-/-indexverwaltung**](#schema-free)
    * Hören Sie auf, sich Sorgen um die Synchronisierung Ihres Datenbankschemas und der Indizes mit dem Schema Ihrer Anwendung zu machen. Wir sind schemafrei. 

* [**Niedrige Betriebskosten**](#tco)
    * Fünf- bis zehnmal [kosteneffizienter](https://aka.ms/documentdb-tco-paper) als eine nicht verwaltete Lösung.
    * Dreimal günstiger als DynamoDB.

<a id="global-distribution"></a>

## <a name="global-distribution"></a>Globale Verteilung
Azure Cosmos DB-Container werden auf zwei Dimensionen verteilt: 

1. Innerhalb einer bestimmten Region werden alle Ressourcen mithilfe von Ressourcenpartitionen horizontal partitioniert (lokale Verteilung). 
2. Jede Ressourcenpartition wird außerdem über geografische Regionen repliziert (globale Verteilung). 

![Diagramm, das die globale Verteilung von Azure Cosmos DB verdeutlicht](./media/introduction/azure-cosmos-db-global-distribution.png) 

Wenn Speicher und Durchsatz skaliert werden müssen, führt Cosmos DB transparent Partitionsverwaltungsvorgänge in allen Regionen aus. Unabhängig von Skalierung, Verteilung oder Ausfällen stellt Cosmos DB weiterhin ein einzelnes Systemimage der global verteilten Ressourcen bereit. 

Die globale Verteilung von Ressourcen in Cosmos DB ist [sofort einsetzbar](../documentdb/documentdb-distribute-data-globally.md). Sie können Ihrem Datenbankkonto jederzeit mit wenigen Klicks (oder programmgesteuert mit einem einzigen API-Aufruf) eine beliebige Anzahl geografischer Regionen zuordnen. 

Unabhängig von der Datenmenge oder der Anzahl der Regionen garantiert Cosmos DB, dass jede neu zugeordnete Region innerhalb von weniger als einer Stunde im 99. Perzentil mit der Verarbeitung von Clientanforderungen beginnen kann. Dies erfolgt durch die Parallelisierung des Seedings und Kopieren der Daten aus allen Quellressourcenpartitionen in die neu zugeordnete Region. Kunden können eine vorhandene Region auch entfernen oder eine Region, die ihrem Datenbankkonto zugeordnet war, offline schalten.

<a id="data-models"></a>
## <a name="multi-model-multi-api-support"></a>Unterstützung mehrerer Modelle und mehrerer APIs
 Azure Cosmos DB unterstützt mehrere Datenmodelle, einschließlich Dokumenten-, Schlüssel-Wert-, Graph- und spaltenbasierte Datenmodelle. Das grundlegende Inhaltsmodell des Cosmos DB-Datenbankmoduls basiert auf Atom-Datensatz-Sequenz (Atom-Record-Sequence, ARS). Atome bestehen aus einem kleinen Satz von primitiven Typen, z.B. Zeichenfolge, boolesch und Zahl. Datensätze sind Strukturen, die aus diese Typen bestehen. Sequenzen sind Arrays, die aus Atomen, Datensätzen oder Sequenzen bestehen. 

![Azure Cosmos DB mit mehreren Modellen mehreren APIs](./media/introduction/azure-cosmos-db-multimodel.png) 
 
 Das Datenbankmodul kann verschiedene Datenmodelle effizient übersetzen und auf das ARS-basierte Datenmodell projizieren. Auf das Kerndatenmodell von Cosmos DB kann nativ über dynamisch geschriebene Programmiersprachen zugegriffen werden, und es kann in der vorliegenden Form als JSON verfügbar gemacht werden. 
 
 Der Dienst unterstützt zudem beliebte Datenbank-APIs für Datenzugriff und Abfragen. Das Cosmos DB-Datenbankmodul unterstützt derzeit [DocumentDB SQL](../documentdb/documentdb-introduction.md), [MongoDB](../documentdb/documentdb-protocol-mongodb.md), [Azure-Tabellen](table-introduction.md) (Vorschauversion) und [Gremlin](graph-introduction.md) (Vorschauversion). Sie können weiterhin Anwendungen mit gängigen OSS-APIs erstellen und alle Vorteile des kampferprobten und vollständig verwalteten, global verteilten Datenbankdiensts nutzen. 

<a id="horizontal-scale"></a>
## <a name="horizontal-scaling-of-storage-and-throughput"></a>Horizontale Skalierung von Speicher und Durchsatz
Alle Daten in einem Cosmos DB-Container (z.B. eine Dokumentensammlung, eine Tabelle oder ein Graph) werden horizontal partitioniert und transparent durch Ressourcenpartitionen verwaltet. Eine Ressourcenpartition ist ein konsistenter und hoch verfügbarer Container mit Daten, die durch einen [vom Kunden angegebenen Partitionsschlüssel](partition-data.md) partitioniert sind. Sie stellt ein einziges Systemimage für eine Auswahl seiner verwalteten Ressourcen bereit, und ist eine grundlegende Einheit für Skalierbarkeit und Verteilung. Cosmos DB wurde entwickelt, damit Sie den Durchsatz auf der Grundlage der Datenverkehrsmuster der Anwendung über verschiedene geografische Regionen hinweg elastisch skalieren können, um schwankende Workloads zu unterstützen, die sowohl hinsichtlich der Geographie als auch der Zeiten variieren. Der Dienst verwaltet die Partitionen transparent und ohne die Verfügbarkeit, die Konsistenz, die Wartezeit oder den Durchsatz eines Cosmos DB-Containers zu beeinträchtigen.  
 
![Azure Cosmos DB ist horizontal skalierbar](./media/introduction/azure-cosmos-db-partitioning.png) 

Sie können den Durchsatz eines Azure Cosmos DB-Containers elastisch skalieren, indem Sie programmgesteuert Durchsatz unter Verwendung von [Anforderungseinheiten pro Sekunde (RU/s)](../documentdb/documentdb-request-units.md) bereitstellen. Intern verwaltet der Dienst Ressourcenpartitionen transparent, um den Durchsatz für einen bestimmten Container zu erreichen. Cosmos DB stellt sicher, dass der Durchsatz in allen dem Container zugeordneten Regionen zur Verwendung verfügbar ist. Der neue Durchsatz wird nach der Änderung des konfigurierten Durchsatzwerts innerhalb von fünf Sekunden wirksam. 

Sie können Durchsatz für einen Cosmos DB-Container sowohl pro Sekunde als auch [pro Minute (RU/m)](request-units-per-minute.md) bereitstellen. Der bereitgestellte Durchsatz mit minutengenauer Granularität dient zur Verarbeitung unerwarteter Lastspitzen in der Workload, für die der Durchsatz mit sekundengenauer Granularität bereitgestellt wurde. 

<a id="low-latency"></a>
## <a name="low-latency-guarantees-at-the-99th-percentile"></a>Garantien geringer Wartezeit im 99. Perzentil
Als Teil der SLAs garantiert Cosmos DB seinen Kunden geringe End-to-End-Wartezeit im 99. Perzentil. Für ein typisches 1-KB-Element garantiert Cosmos DB eine End-to-End-Wartezeit für Lesevorgänge von weniger als 10 ms und für indizierte Schreibvorgänge von weniger als 15 ms im 99. Perzentil innerhalb derselben Azure-Region. Die mittlere Wartezeit liegt deutlich niedriger (unter 5 ms).  Durch eine obere Grenze der Anforderungsverarbeitung für jede Datenbanktransaktion erlaubt Cosmos DB es Clients, klar zwischen Transaktionen mit hoher Wartezeit und einer nicht verfügbaren Datenbank zu unterscheiden.

<a id="high-availability"></a>
## <a name="transparent-multi-homing-and-9999-high-availability"></a>Transparentes Multihosting und hohe Verfügbarkeit zu 99,99 %
Sie können den Regionen, die Ihrem Azure Cosmos DB-Datenbankkonto zugeordnet sind, dynamisch „Prioritäten“ zuweisen. Prioritäten werden verwendet, um die Anforderungen bei regionalen Ausfällen an bestimmte Regionen weiterzuleiten. Im unwahrscheinlichen Falle eines regionalen Notfalls führt Cosmos DB automatisch ein Failover in der Reihenfolge der Prioritäten aus.

Um die End-to-End-Verfügbarkeit der Anwendung zu testen, können Sie [manuell ein Failover auslösen](../documentdb/documentdb-regional-failovers.md) (begrenzt auf zwei Vorgänge innerhalb einer Stunde). Cosmos DB garantiert, dass während eines manuellen regionalen Failovers keine Daten verloren gehen. Für den Fall, dass ein regionaler Notfall auftritt, garantiert Cosmos DB eine obere Begrenzung des Datenverlusts während des vom System initiierten automatischen Failovers. Sie müssen die Anwendung nach einem regionalen Failover nicht erneut bereitstellen und die Verfügbarkeits-SLAs werden von Azure Cosmos DB eingehalten. 

In diesem Szenario ermöglicht Cosmos DB Ihnen die Interaktion mit Ressourcen über logische (regionsunabhängige) oder physische (regionsspezifische) Endpunkte. Ersteres ermöglicht transparentes Multihosting der Anwendung im Falle eines Failovers. Letzteres bietet präzise Steuerungsmöglichkeiten für die Anwendung, um Lese- und Schreibvorgänge an bestimmte Regionen umzuleiten. Cosmos DB garantiert eine 99,99 % Verfügbarkeits-SLA für jedes Datenbankkonto. Die Verfügbarkeitsgarantien sind unabhängig von Skalierung (bereitgestellter Durchsatz und Speicher), Anzahl der Regionen oder geografischer Entfernung zwischen Regionen, die einer bestimmten Datenbank zugeordnet sind. 

<a id="consistency"></a>
## <a name="multiple-well-defined-consistency-models"></a>Mehrere, klar definierte Konsistenzmodelle
Kommerziell verteilte Datenbanken werden in zwei Kategorien unterteilt: Datenbanken, die keinerlei klar definierte, belegbare Konsistenzoptionen bieten, und Datenbanken, die zwei gegensätzliche Programmierbarkeitsoptionen (starke Konsistenz oder letztliche Konsistenz) bieten. Erstere liefern Anwendungsentwicklern minuziöse Replikationsprotokolle und bürden ihnen die schwierige Aufgabe auf, zwischen Konsistenz, Verfügbarkeit, Wartezeit und Durchsatz abzuwägen. Letztere setzen die Anwendungsentwickler unter Druck, eine der beiden Extreme auszuwählen. Trotz der Fülle der Untersuchungen und Vorschläge für mehr als 50 Konsistenzmodelle war die verteilte Datenbankcommunity nicht in der Lage, Konsistenzebenen über starke und letztliche Konsistenz kommerzialisieren. 

Cosmos DB bietet Ihnen die Auswahl zwischen [fünf klar definierten Konsistenzmodellen](../documentdb/documentdb-consistency-levels.md) aus dem gesamten Konsistenzspektrum – starke Konsistenz, Konsistenz mit begrenzter Veraltung, [Sitzungskonsistenz](http://dl.acm.org/citation.cfm?id=383631), Präfixkonsistenz und letztliche Konsistenz. 

![Azure Cosmos DB bietet mehrere überlegt definierte (gelockerte) Konsistenzmodelle.](media/introduction/azure-cosmos-db-consistency-levels.png)

Die folgende Tabelle zeigt die speziellen Garantien der einzelnen Konsistenzebenen.
 
**Konsistenzebenen und Garantien**

| Konsistenzebene    | Garantien |
| --- | --- |
| STARK (Strong) | Linearisierbarkeit |
| Begrenzte Veraltung (Bounded staleness)    | Präfixkonsistenz. Lesevorgänge bleiben hinter Schreibvorgängen um Präfix k oder Intervall t zurück |
| Sitzung    | Präfixkonsistenz. Monotone Lesevorgänge, monotone Schreibvorgänge, Lesen der eigenen Schreibvorgänge, Schreibvorgänge folgen Lesevorgängen |
| Präfixkonsistenz    | Die zurückgegebenen Updates sind ein bestimmtes Präfix aller Updates ohne Lücken |
| Letztlich (Eventual)    | Lesevorgänge in falscher Reihenfolge |

Sie können die Standardkonsistenzebene für Ihr Cosmos DB-Konto konfigurieren (und die Konsistenz später für eine bestimmte Leseanforderung außer Kraft setzen). Intern gilt die Standardkonsistenzebene für Daten in den Partitionssätzen, die sich auf mehrere Regionen erstrecken können. 


<a id="sla"></a>
## <a name="guaranteed-service-level-agreements"></a>Garantierte Vereinbarungen zum Servicelevel (SLA)

Cosmos DB ist der erste verwaltete Datenbankdienst, der [SLA-Garantien](https://aka.ms/acdbsla) für Verfügbarkeit, Durchsatz, niedrige Wartezeit und Konsistenz mit 99,99 Prozent anbietet.
* Verfügbarkeit: Verfügbarkeits-SLA mit 99,99 Prozent Betriebszeit für jeden Vorgang auf Daten- und Steuerungsebene.
* Durchsatz: 99,99 % der Anforderungen werden erfolgreich abgeschlossen 
* Wartezeit: 99,99 % der < 10 ms-Wartezeiten im 99. Perzentil
* Konsistenz: 100 % der Leseanforderungen erfüllen die Konsistenzgarantie für die von Ihnen gewählte Konsistenzebene.


<a id="schema-free"></a>
## <a name="schema-free"></a>Schemafrei

Sowohl bei relationalen als auch NoSQL-Datenbanken sind Sie gezwungen, sich mit Schema- und Indexverwaltung, Versionsverwaltung und Migration auseinanderzusetzen. All dies ist in einem global verteilten Setup äußerst schwierig. Aber keine Sorge – Cosmos DB beseitigt dieses Problem. Bei Cosmos DB brauchen Sie keine Schemas und Indizes zu verwalten, sich nicht mit Schemaversionsverwaltung auseinanderzusetzen und sich keine Gedanken um Anwendungsausfallzeiten während der Migration von Schemas zu machen. Das Cosmos DB-Datenbankmodul ist komplett schemaunabhängig – alle erfassten Daten werden automatisch und ohne Schema oder Indizes erfasst, was extrem schnelle Abfragen ermöglicht. 

<a id="tco"></a>
## <a name="low-cost-of-ownership"></a>Niedrige Betriebskosten

 Wenn alle Aspekte der Gesamtbetriebskosten (TCO) berücksichtigt werden, können verwaltete Clouddienste wie Azure Cosmos DB fünf-bis zehnmal kosteneffektiver als die lokal oder auf virtuellen Computern ausgeführten OSS-Vergleichssysteme sein. Und Azure Cosmos DB ist bei großen Workloads zwei bis drei Mal günstiger als DynamoDB. Weitere Informationen finden Sie im [TCO-Whitepaper](https://aka.ms/documentdb-tco-paper). 

## <a name="next-steps"></a>Nächste Schritte
Erste Schritte mit Azure Cosmos DB mit einem unserer Schnellstarts:

* [Erste Schritte mit der DocumentDB-API von Azure Cosmos DB](create-documentdb-dotnet.md)
* [Erste Schritte mit der MongoDB-API von Azure Cosmos DB](create-mongodb-nodejs.md)
* [Erste Schritte mit der Graph-API von Azure Cosmos DB](create-graph-dotnet.md)
* [Erste Schritte mit der Table-API von Azure Cosmos DB](create-table-dotnet.md)

