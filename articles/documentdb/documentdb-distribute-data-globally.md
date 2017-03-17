---
title: Globale Verteilung von Daten mit DocumentDB | Microsoft Docs
description: "Erfahren Sie mehr über weltweite Georeplikation, Failover und Datenwiederherstellungen mithilfe von globalen Datenbanken mit Azure DocumentDB, einem vollständig verwalteten NoSQL-Datenbankdienst."
services: documentdb
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: ba5ad0cc-aa1f-4f40-aee9-3364af070725
ms.service: documentdb
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/20/2017
ms.author: arramac
translationtype: Human Translation
ms.sourcegitcommit: 72d9c639a6747b0600c5ce3a1276f3c1d2da64b2
ms.openlocfilehash: 8c3ced706c26e09d709d7cfb4d81534c362e1628
ms.lasthandoff: 02/22/2017


---
# <a name="documentdb---a-globally-distributed-database-service-on-azure"></a>DocumentDB – ein global verteilter Datenbankdienst in Azure
Azure ist ubiquitär: Es ist in über 30 geografischen Regionen auf der ganzen Welt präsent und breitet sich kontinuierlich weiter aus. Die weltweite Präsenz von Azure ermöglicht den Entwicklern unter anderem die problemlose Erstellung, Bereitstellung und Verwaltung global verteilter Anwendungen. DocumentDB ist das mehrinstanzenfähige, global verteilte Datenbanksystem von Microsoft, mit dem Entwickler weltweit skalierbare Anwendungen erstellen können. DocumentDB ermöglicht die elastische Skalierung von Durchsatz und Speicher für eine beliebige Anzahl geografischer Regionen. Der Dienst zeichnet sich durch eine garantiert niedrige Latenz bei P99, eine hohe Verfügbarkeit von 99,99 Prozent, planbaren Durchsatz und [mehrere klar definierte Konsistenzmodelle](documentdb-consistency-levels.md) aus – jeweils abgedeckt durch umfassende SLAs. Dank des [schemaunabhängigen und für Schreibvorgänge optimierten Datenbankmoduls](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) kann DocumentDB standardmäßig alle erfassten Daten automatisch indizieren und in die Programmiersprachen [SQL](documentdb-sql-query.md), [MongoDB](documentdb-protocol-mongodb.md) und [JavaScript](documentdb-programming.md#javascript-language-integrated-query-api) integrierte Abfragen unabhängig von der Skalierung verarbeiten. Als Clouddienst wurde DocumentDB von Grund auf für Mehrinstanzenfähigkeit und globale Verteilung konzipiert.

**Eine einzelne DocumentDB-Sammlung, partitioniert und auf drei Azure-Regionen verteilt**

![Azure DocumentDB-Sammlung, partitioniert und auf drei Regionen verteilt](./media/documentdb-distribute-data-globally/documentdb-global-apps.png)

Bei der Entwicklung von DocumentDB haben wir gelernt, dass die globale Verteilung nicht als Anhängsel behandelt werden darf und nicht einfach auf ein Datenbanksystem mit einem einzelnen Standort aufgesetzt werden kann. Die Funktionen einer global verteilten Datenbank gehen über die herkömmliche geografische Notfallwiederherstellung (Geographical Disaster Recovery, Geo-DR) von Datenbanken mit nur einem Standort hinaus. Bei Datenbanken mit nur einem Standort und Geo-DR-Funktion handelt es sich um eine echte Untermenge weltweit verteilter Datenbanken. 

Dank der sofort einsatzbereiten globalen Verteilung von DocumentDB müssen Entwickler kein eigenes Replikationsgerüst erstellen – weder in Form des Lambda-Musters (Beispiel: [AWS DynamoDB-Replikation](https://github.com/awslabs/dynamodb-cross-region-library/blob/master/README.md)) über das Datenbankprotokoll noch durch doppelte Schreibvorgänge in mehreren Regionen. Diese Verfahren werden nicht empfohlen, da ihre Korrektheit nicht gewährleistet werden kann und keine vernünftigen SLAs bereitgestellt werden können. 

Dieser Artikel enthält eine Übersicht über die DocumentDB-Funktionen für die globale Verteilung. Außerdem beschreiben wir hier das einzigartige Konzept von DocumentDB für die Bereitstellung umfassender SLAs. 

## <a id="EnableGlobalDistribution"></a>Aktivieren der sofort einsatzbereiten globalen Verteilung
Folgende Funktionen von DocumentDB ermöglichen die problemlose Erstellung weltweit skalierbarer Anwendungen. Diese Funktionen stehen über die ressourcenanbieterbasierten [REST-APIs](https://docs.microsoft.com/rest/api/documentdbresourceprovider/) von DocumentDB sowie über das Azure-Portal zur Verfügung.

### <a id="RegionalPresence"></a>Ubiquitäre regionale Präsenz 
Es werden stetig neue Regionen online geschaltet, um die geografische Präsenz von Azure zu erweitern. DocumentDB ist standardmäßig in allen neuen Azure-Regionen verfügbar. Dadurch können Sie Ihrem DocumentDB-Datenbankkonto eine geografische Region zuordnen, sobald die neue Azure-Region den Betrieb aufnimmt.

**DocumentDB ist standardmäßig in allen Azure-Regionen verfügbar.**

![DocumentDB in allen Azure-Regionen verfügbar](./media/documentdb-distribute-data-globally/azure-regions.png)

### <a id="UnlimitedRegionsPerAccount"></a>Zuordnen einer unbegrenzten Anzahl von Regionen zu Ihrem DocumentDB-Datenbankkonto
Mit DocumentDB können Sie Ihrem DocumentDB-Datenbankkonto eine beliebige Anzahl von Azure-Regionen zuordnen. Mit Ausnahme von Geofencing-Einschränkungen (China, Deutschland...) gibt es hinsichtlich der Anzahl von Regionen, die Sie Ihrem DocumentDB-Datenbankkonto zuordnen können, keinerlei Einschränkungen. Die folgende Abbildung zeigt ein Datenbankkonto, das sich über 21 Azure-Regionen erstreckt.  

**DocumentDB-Datenbankkonto eines Mandanten, das sich über 21 Azure-Regionen erstreckt**

![DocumentDB-Datenbankkonto, das sich über 21 Azure-Regionen erstreckt](./media/documentdb-distribute-data-globally/documentdb-spanning-regions.png)

### <a id="PolicyBasedGeoFencing"></a>Richtlinienbasiertes Geofencing
DocumentDB verfügt über richtlinienbasierte Geofencing-Funktionen. Geofencing – eine wichtige Komponente zur Einhaltung von Einschränkungen in den Bereichen Datengovernance und Compliance – kann verhindern, dass Ihrem Konto eine bestimmte Region zugeordnet wird. Geofencing kann beispielsweise verwendet werden, um die globale Verteilung auf die Bereiche innerhalb einer unabhängigen Cloud (China, Deutschland...) oder innerhalb der Besteuerungsgrenzen einer Regierung (beispielsweise Australien) zu beschränken. Die Richtlinien werden mithilfe der Metadaten Ihres Azure-Abonnements gesteuert.

### <a id="DynamicallyAddRegions"></a>Dynamisches Hinzufügen und Entfernen von Regionen
Mit DocumentDB können Sie jederzeit Regionen zu Ihrem Datenbankkonto hinzufügen (zuordnen) oder daraus entfernen (Zuordnung aufheben), wie in der [vorherigen Abbildung](#UnlimitedRegionsPerAccount) zu sehen. Aufgrund der parallelen partitionsübergreifenden Datenreplizierung ist bis zu 100 TB gewährleistet, dass DocumentDB überall auf der Welt innerhalb von 30 Minuten verfügbar ist, wenn eine neue Region online geschaltet wird. 

### <a id="FailoverPriorities"></a>Failoverprioritäten
Mit DocumentDB können Sie für den Ausfall mehrerer Regionen die exakte Reihenfolge von regionalen Failovern steuern, indem Sie die Priorität mehreren, dem Datenbankkonto zugeordneten Regionen zuordnen, wie in der folgenden Abbildung zu sehen. DocumentDB stellt sicher, dass bei der automatischen Failoversequenz die von Ihnen angegebene Prioritätenreihenfolge eingehalten wird.

**Ein DocumentDB-Mandant kann die Failoverprioritätenreihenfolge (rechter Bereich) für Regionen konfigurieren, die einem Datenbankkonto zugeordnet sind.**

![Konfigurieren von Failoverprioritäten mit Azure DocumentDB](./media/documentdb-distribute-data-globally/documentdb-failover-priorities.png)

### <a id="OfflineRegions"></a>Dynamisches Offlineschalten einer Region
Mit DocumentDB können Sie Ihr Datenbankkonto in einer bestimmten Region offline und später wieder online schalten. Als offline gekennzeichnete Regionen sind nicht aktiv in die Replikation und nicht in die Failoversequenz eingebunden. Dadurch können Sie vor dem Rollout potenziell riskanter Upgrades für Ihre Anwendung das letzte als funktionierend bekannte Datenbankimage in einer der Leseregionen fixieren.

### <a id="ConsistencyLevels"></a>Mehrere klar definierte Konsistenzmodelle für global replizierte Datenbanken
DocumentDB macht [mehrere klar definierte Konsistenzebenen](documentdb-consistency-levels.md) mit SLA-Abdeckung verfügbar. In Abhängigkeit von der Workload/den Szenarien können Sie aus den verfügbaren Optionen ein bestimmtes Konsistenzmodell auswählen. 

### <a id="TunableConsistency"></a>Verschiedene Konsistenzeinstellungen für global replizierte Datenbanken
Mit DocumentDB können Sie die Standardkonsistenzauswahl programmgesteuert zur Laufzeit für einzelne Anforderungen außer Kraft setzen und lockern. 

### <a id="DynamicallyConfigurableReadWriteRegions"></a>Dynamisch konfigurierbare Lese- und Schreibregionen
Mit DocumentDB können Sie die (der Datenbank zugeordneten) Regionen für „Lesen“, „Schreiben“ oder „Lesen/Schreiben“ konfigurieren. 

### <a id="ElasticallyScaleThroughput"></a>Elastisches Skalieren des Durchsatzes für Azure-Regionen
Sie können eine DocumentDB-Sammlung durch programmgesteuertes Bereitstellen eines Durchsatzes elastisch skalieren. Der Durchsatz wird auf alle Regionen angewendet, in denen die Sammlung verteilt wird.

### <a id="GeoLocalReadsAndWrites"></a>Geolokale Lese- und Schreibvorgänge
Der Hauptvorteil einer global verteilten Datenbank besteht in der niedrigen Latenz beim weltweiten Datenzugriff. DocumentDB garantiert bei P99 eine niedrige Latenz für verschiedene Datenbankvorgänge. Dabei wird sichergestellt, dass alle Lesevorgänge an die nächstgelegene lokale Leseregion weitergeleitet werden. Bei der Verarbeitung einer Leseanforderung wird das lokale Quorum für die Region verwendet, in der der Lesevorgang initiiert wurde. Gleiches gilt für Schreibvorgänge. Ein Schreibvorgang wird erst bestätigt, wenn die Mehrheit der Replikate den Schreibvorgang dauerhaft lokal committet hat, sodass keine Abhängigkeit von der Bestätigung der Schreibvorgänge durch Remotereplikate besteht. Anders ausgedrückt: Für das Replikationsprotokoll von DocumentDB wird davon ausgegangen, dass das Lese- und Schreibquorum immer lokal für die Lese- und Schreibregionen gilt, in denen die Anforderung initiiert wird.

### <a id="ManualFailover"></a>Manuelles Initiieren eines regionalen Failovers
Mit DocumentDB können Sie das Failover des Datenbankkontos auslösen, um die Eigenschaften der *End-to-End-Verfügbarkeit* der gesamten Anwendung (außerhalb der Datenbank) zu überprüfen. Da sowohl die Sicherheits- und Verfügbarkeitseigenschaften der Fehlererkennung als auch die Wahl einer übergeordneten Instanz garantiert werden, garantiert DocumentDB, dass bei einem vom Mandanten manuell initiierten Failovervorgang *keine Daten verloren gehen*.

### <a id="AutomaticFailover"></a>Automatisches Failover
DocumentDB unterstützt automatisches Failover bei regionalen Ausfällen. Während eines regionalen Failovers erfüllt DocumentDB weiterhin die SLAs für Leselatenz, Verfügbarkeit, Konsistenz und Durchsatz. DocumentDB bietet eine Obergrenze für die Dauer eines automatischen Failovervorgangs. Hierbei handelt es sich um das Zeitfenster für potenzielle Datenverluste während eines regionalen Ausfalls.

### <a id="GranularFailover"></a>Konzipiert für verschiedene Failovergranularitäten
Aktuell werden die automatischen und manuellen Failoverfunktionen mit der Granularität des Datenbankkontos verfügbar gemacht. Intern ist DocumentDB allerdings für ein präziseres *automatisches* Failover einer Datenbank, Sammlung oder sogar Partition (eine Sammlung mit einer Reihe von Schlüsseln) konzipiert. 

### <a id="MultiHomingAPIs"></a>Multihosting-APIs
DocumentDB ermöglicht die Interaktion mit der Datenbank über logische (regionsunabhängige) oder physische (regionsspezifische) Endpunkte. Die Verwendung logischer Endpunkte ermöglicht transparentes Multihosting der Anwendung im Falle eines Failovers. Physische Endpunkte bieten präzise Steuerungsmöglichkeiten für die Anwendung, um Lese- und Schreibvorgänge an bestimmte Regionen umzuleiten.

### <a id="TransparentSchemaMigration"></a>Transparente und konsistente Migration von Datenbankschemas und Indizes 
DocumentDB ist vollkommen [schemaunabhängig](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf). Dank des einzigartigen Designs des Datenbankmoduls kann die Lösung automatisch und synchron sämtliche erfasste Daten indizieren, ohne dass Sie dafür ein Schema oder sekundäre Indizes bereitstellen müssen. Dadurch können Sie Ihre global verteilte Anwendung schnell durchlaufen, ohne sich Gedanken über die Migration von Datenbankschemas oder Indizes zu machen oder mehrstufige Anwendungsrollouts mit Schemaänderungen zu koordinieren. DocumentDB garantiert, dass Änderungen, die Sie explizit an Indizierungsrichtlinien vornehmen, keine Beeinträchtigung der Leistung oder Verfügbarkeit zur Folge haben.  

### <a id="ComprehensiveSLAs"></a>Umfassende, über bloße Hochverfügbarkeit hinausgehende SLAs
Als global verteilter Datenbankdienst bietet DocumentDB eine klar definierte SLA für **Datenverlust**, **Verfügbarkeit**, **Latenz bei P99**, **Durchsatz** und **Konsistenz** für die gesamte Datenbank – unabhängig davon, wie viele Regionen der Datenbank zugeordnet sind.  

## <a id="LatencyGuarantees"></a>Latenzgarantien
Der Hauptvorteil eines global verteilten Datenbankdiensts wie DocumentDB besteht in der niedrigen Latenz beim weltweiten Zugriff auf Ihre Daten. DocumentDB garantiert bei P99 eine niedrige Latenz für verschiedene Datenbankvorgänge. Das von DocumentDB verwendete Replikationsprotokoll sorgt dafür, dass die Datenbankvorgänge (im Idealfall sowohl Lese- als auch Schreibvorgänge) stets in der lokalen Region des Clients ausgeführt werden. Die Latenz-SLA von DocumentDB beinhaltet P99 für Lesevorgänge, (synchron) indizierte Schreibvorgänge und Abfragen für verschiedene Anforderungs- und Antwortgrößen. Die Latenzgarantien für Schreibvorgänge beinhalten permanente Mehrheitsquorum-Commits innerhalb des lokalen Datencenters.

### <a id="LatencyAndConsistency"></a>Zusammenhang zwischen Latenz und Konsistenz 
Damit ein global verteilter Dienst starke Konsistenz in einer global verteilten Umgebung bieten kann, müssen die Schreibvorgänge synchron repliziert oder synchrone regionsübergreifende Lesevorgänge ausgeführt werden. Aufgrund der hohen Geschwindigkeit sowie der Zuverlässigkeit des Fernnetzes geht starke Konsistenz immer mit hoher Latenz und geringer Verfügbarkeit von Datenbankvorgängen einher. Um eine niedrige Latenz bei P99 und eine Verfügbarkeit von 99,99 Prozent bieten zu können, muss der Dienst also eine asynchrone Replikation verwenden. Hierzu muss der Dienst wiederum [klar definierte, gelockerte Konsistenzoptionen](documentdb-consistency-levels.md) bieten. Diese müssen im Idealfall zwischen starker Konsistenz (für garantierte niedrige Latenz und hohe Verfügbarkeit) und letztlicher Konsistenz (für ein intuitives Programmiermodell) liegen.

DocumentDB stellt sicher, dass kein Lesevorgang erforderlich ist, um Replikate in mehreren Regionen zu kontaktieren und die spezifische garantierte Konsistenzebene zu erreichen. Analog dazu wird sichergestellt, dass ein Schreibvorgang beim regionsübergreifenden Replizieren der Daten nicht blockiert wird. (Mit anderen Worten: Schreibvorgänge werden synchron regionsübergreifend repliziert.) Für Datenbankkonten mit mehreren Regionen stehen mehrere gelockerte Konsistenzebenen zur Verfügung. 

### <a id="LatencyAndAvailability"></a>Zusammenhang zwischen Latenz und Verfügbarkeit 
Latenz und Verfügbarkeit sind zwei Seiten einer Medaille. Im stabilen Zustand sprechen wir von der Latenz eines Vorgangs, im Falle eines Ausfalls verwenden wir den Begriff Verfügbarkeit. Für die Anwendung macht es keinen Unterschied, ob ein Datenbankvorgang langsam ausgeführt wird oder ob die Datenbank nicht verfügbar ist. 

Zur Unterscheidung zwischen hoher Latenz und Nichtverfügbarkeit bietet DocumentDB eine absolute Obergrenze für die Latenz verschiedener Datenbankvorgänge. Übersteigt die Dauer des Datenbankvorgangs die Obergrenze, gibt DocumentDB einen Timeoutfehler zurück. Die Verfügbarkeits-SLA von DocumentDB gewährleistet, dass die Timeouts für die Verfügbarkeits-SLA berücksichtigt werden. 

### <a id="LatencyAndThroughput"></a>Zusammenhang zwischen Latenz und Durchsatz
Bei DocumentDB müssen Sie sich nicht zwischen Latenz und Durchsatz entscheiden. Die Lösung erfüllt die SLA für Latenz bei P99 und bietet gleichzeitig den von Ihnen bereitgestellten Durchsatz. 

## <a id="ConsistencyGuarantees"></a>Konsistenzgarantien
Das [starke Konsistenzmodell](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) ist zwar das Nonplusultra in Sachen Programmierbarkeit, es wird jedoch teuer durch hohe Latenz (im stabilen Zustand) bzw. durch den Verlust der Verfügbarkeit (im Falle eines Ausfalls) erkauft. 

DocumentDB bietet ein klar definiertes Programmiermodell, sodass Sie sich mit der Konsistenz der replizierten Daten befassen können. Um Ihnen die Erstellung von Anwendungen mit Multihosting zu ermöglichen, sind die von DocumentDB verfügbar gemachten Konsistenzmodelle regionsunabhängig, und es spielt auch keine Rolle, in welcher Region die Lese- und Schreibvorgänge verarbeitet werden. 

Die Konsistenz-SLA von DocumentDB garantiert, dass 100 Prozent der Leseanforderungen die Konsistenzgarantie für die von Ihnen angeforderte Konsistenzebene erfüllen. (Hierbei kann es sich entweder um die standardmäßige Konsistenzebene für das Datenbankkonto oder um den überschriebenen Wert der Anforderung handeln.) Bei einer Leseanforderung gilt die Konsistenz-SLA als erfüllt, wenn alle Konsistenzgarantien der Konsistenzebene erfüllt werden. Die folgende Tabelle enthält die Konsistenzgarantien für bestimmte Konsistenzebenen von DocumentDB.

**Konsistenzgarantien in Verbindung mit einer bestimmten Konsistenzebene in DocumentDB**

<table>
    <tr>
        <td><strong>Konsistenzebene</strong></td>
        <td><strong>Konsistenzmerkmale</strong></td>
        <td><strong>SLA</strong></td>
    </tr>
    <tr>
        <td rowspan="3">Sitzung</td>
        <td>Lesen eigener Schreibvorgänge</td>
        <td>100 %</td>
    </tr>
    <tr>
        <td>Monotones Lesen</td>
        <td>100 %</td>
    </tr>
    <tr>
        <td>Konsistentes Präfix</td>
        <td>100 %</td>
    </tr>
    <tr>
        <td rowspan="3">Bounded staleness</td>
        <td>Monotones Lesen (innerhalb einer Region)</td>
        <td>100 %</td>
    </tr>
    <tr>
        <td>Konsistentes Präfix</td>
        <td>100 %</td>
    </tr>
    <tr>
        <td>Begrenzte Veraltung &lt; K,T</td>
        <td>100 %</td>
    </tr>
    <tr>
        <td>Letztlich (Eventual)</td>
        <td>Konsistentes Präfix</td>
        <td>100 %</td>
    </tr>
    <tr>
        <td>STARK (Strong)</td>
        <td>Linearisierbar</td>
        <td>100 %</td>
    </tr>
</table>

### <a id="ConsistencyAndAvailability"></a>Zusammenhang zwischen Konsistenz und Verfügbarkeit
Das [Unmöglichkeitsergebnis](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf) des [CAP-Theorems](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf) beweist, dass das System im Falle eines Ausfalls nicht gleichzeitig verfügbar bleiben und linearisierbare Konsistenz bieten kann. Der Datenbankdienst kann entweder Konsistenz und Partitionen oder Verfügbarkeit und Partitionen bieten. Im ersten Fall wird die Verfügbarkeit zugunsten von linearisierbarer Konsistenz geopfert, im zweiten Fall wird die [linearisierbare Konsistenz](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) zugunsten der Verfügbarkeit aufgegeben. DocumentDB verstößt niemals gegen die angeforderte Konsistenzebene und ist damit formal betrachtet ein System mit Konsistenz und Partitionen. In der Praxis ist Konsistenz allerdings keine Alles-oder-Nichts-Option: Es gibt mehrere klar definierte Konsistenzmodelle entlang des Konsistenzspektrums zwischen linearisierbarer und letztlicher Konsistenz. In DocumentDB haben wir versucht, mehrere der gelockerten Konsistenzmodelle mit praktischer Anwendbarkeit und einem intuitiven Programmiermodell zu identifizieren. DocumentDB bietet als Kompromiss zwischen Konsistenz und Verfügbarkeit eine SLA mit einer Verfügbarkeit von 99,99 Prozent sowie [mehrere gelockerte, aber dennoch klar definierte Konsistenzebenen](documentdb-consistency-levels.md). 

### <a id="ConsistencyAndAvailability"></a>Zusammenhang zwischen Konsistenz und Latenz
Mit [PACELC](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf) wurde von Prof. Daniel Abadi eine umfassendere CAP-Variante vorgeschlagen, die auch Kompromisse zwischen Latenz und Konsistenz im stabilen Zustand berücksichtigt. Bei dieser Variante muss sich das Datenbanksystem im stabilen Zustand zwischen Konsistenz und Latenz entscheiden. Mit mehreren gelockerten Konsistenzmodellen (unterstützt durch asynchrone Replikation und Quoren für lokale Lese-/Schreibvorgänge) stellt DocumentDB sicher, dass es sich bei allen Lese- und Schreibvorgängen um lokale Vorgänge für die Lese- bzw. Schreibregion handelt.  Dadurch kann DocumentDB innerhalb der Region eine niedrige Latenz für die Konsistenzebenen garantieren.  

### <a id="ConsistencyAndThroughput"></a>Zusammenhang zwischen Konsistenz und Durchsatz
Da die Implementierung eines bestimmten Konsistenzmodells von der Wahl eines [Quorumstyps](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) abhängt, variiert auch der Durchsatz abhängig von der gewählten Konsistenz. So entspricht beispielsweise in DocumentDB der Durchsatz mit stark konsistenten Lesevorgängen ungefähr der Hälfte des Durchsatzes von Lesevorgängen mit letztlicher Konsistenz. 
 
**Zusammenhang zwischen Lesekapazitäten für eine bestimmte Konsistenzebene in DocumentDB**

![Zusammenhang zwischen Konsistenz und Durchsatz](./media/documentdb-distribute-data-globally/consistency-and-throughput.png)

## <a id="ThroughputGuarantees"></a>Durchsatzgarantien 
Mit DocumentDB lässt sich der Durchsatz (ebenso wie der Speicher) elastisch, regionsübergreifend und bedarfsgerecht skalieren. 

**Eine einzelne DocumentDB-Sammlung – partitioniert (mit drei Shards) und anschließend auf drei Azure-Regionen verteilt**

![Azure DocumentDB – verteilte und partitionierte Sammlungen](./media/documentdb-distribute-data-globally/documentdb-throughput.png)

Eine DocumentDB-Sammlung wird auf der Grundlage von zwei Dimensionen verteilt – erst innerhalb einer Region und anschließend regionsübergreifend. Das geht so: 

* Innerhalb einer einzelnen Region wird eine DocumentDB-Sammlung mithilfe von Ressourcenpartitionen horizontal hochskaliert. Jede Ressourcenpartition verwaltet einen Satz von Schlüsseln und ist dank Zustandsautomatreplikation zwischen einer Gruppe von Replikaten stark konsistent und hoch verfügbar. DocumentDB ist ein vollständig ressourcengesteuertes System, in dem eine Ressourcenpartition für die Bereitstellung ihres Durchsatzanteils am Budget der zugeteilten Systemressourcen verantwortlich ist. Die Skalierung der DocumentDB-Sammlung ist vollständig transparent: DocumentDB verwaltet die Ressourcenpartitionen und teilt sie bei Bedarf auf oder führt sie zusammen. 
* Anschließend werden die einzelnen Ressourcenpartitionen regionsübergreifend verteilt. Ressourcenpartitionen, die in verschiedenen Regionen den gleichen Satz von Schlüsseln besitzen, bilden eine Partitionsgruppe, wie in der [vorherigen Abbildung](#ThroughputGuarantees) zu sehen.  Ressourcenpartitionen innerhalb einer Partitionsgruppe werden mittels Zustandsautomatreplikation über mehrere Regionen hinweg koordiniert. Je nach konfigurierter Konsistenzebene werden die Ressourcenpartitionen innerhalb einer Partitionsgruppe dynamisch unter Verwendung verschiedener Topologien (Stern, Verkettung, Baumstruktur usw.) konfiguriert. 

Dank extrem reaktionsschneller Partitionsverwaltung, Lastenausgleich und strenger Ressourcengovernance können Sie den Durchsatz mit DocumentDB elastisch über mehrere Azure-Regionen hinweg für eine DocumentDB-Sammlung skalieren. Die Anpassung des Durchsatzes für eine Sammlung ist bei DocumentDB ein Laufzeitvorgang. Genau wie bei anderen Datenbankvorgängen garantiert DocumentDB auch hier die absolute Latenzobergrenze für Ihre Durchsatzänderungsanforderung. Als Beispiel zeigt die folgende Abbildung die Sammlung eines Kunden mit elastisch bereitgestelltem Durchsatz nach Bedarf (im Bereich zwischen einer Million und zehn Millionen Anforderungen/Sek. für zwei Regionen).
 
**Sammlung eines Kunden mit elastisch bereitgestelltem Durchsatz (zwischen einer Million und zehn Millionen Anforderungen/Sek.)**

![Azure DocumentDB – elastisch bereitgestellter Durchsatz](./media/documentdb-distribute-data-globally/documentdb-elastic-throughput.png)

### <a id="ThroughputAndConsistency"></a>Zusammenhang zwischen Durchsatz und Konsistenz 
Siehe [Zusammenhang zwischen Konsistenz und Durchsatz](#ConsistencyAndThroughput).

### <a id="ThroughputAndAvailability"></a>Zusammenhang zwischen Durchsatz und Verfügbarkeit
Bei Durchsatzänderungen hält DocumentDB seine Verfügbarkeit weiter aufrecht. DocumentDB bietet eine transparente Partitionsverwaltung (beispielsweise Aufteilungs-, Zusammenführungs- und Klonvorgänge) und stellt sicher, dass die Vorgänge weder die Leistung noch die Verfügbarkeit beeinträchtigen, wenn die Anwendung den Durchsatz elastisch erhöht oder verringert. 

## <a id="AvailabilityGuarantees"></a>Verfügbarkeitsgarantien
DocumentDB bietet für jeden Vorgang auf Daten- und Steuerungsebene eine Verfügbarkeits-SLA mit 99,99 Prozent. Wie bereits weiter oben beschrieben beinhalten die Verfügbarkeitsgarantien von DocumentDB eine absolute Latenzobergrenze für jeden Vorgang auf Daten- und Steuerungsebene. Die Verfügbarkeitsgarantien sind fest verankert und unabhängig von der Anzahl von Regionen und der geografischen Entfernung zwischen Regionen. Verfügbarkeitsgarantien gelten sowohl für manuelle als auch für automatische Failover. DocumentDB bietet transparente Multihosting-APIs, die sicherstellen, dass Ihre Anwendung logische Endpunkte verwenden und die Anforderungen im Falle eines Failovers transparent an die neue Region weiterleiten kann. Anders ausgedrückt: Ihre Anwendung muss bei einem regionalen Failover nicht erneut bereitgestellt werden, und die Verfügbarkeits-SLAs bleiben gewahrt.

### <a id="AvailabilityAndConsistency"></a>Zusammenhang zwischen Verfügbarkeit, Konsistenz, Latenz und Durchsatz
Der Zusammenhang zwischen Verfügbarkeit, Konsistenz, Latenz und Durchsatz wird unter [Zusammenhang zwischen Konsistenz und Verfügbarkeit](#ConsistencyAndAvailability), [Zusammenhang zwischen Latenz und Verfügbarkeit](#LatencyAndAvailability) und [Zusammenhang zwischen Durchsatz und Verfügbarkeit](#ThroughputAndAvailability) beschrieben. 

## <a id="GuaranteesAgainstDataLoss"></a>Garantien und Systemverhalten für Datenverlust
In DocumentDB wird jede Partition (einer Sammlung) durch eine Reihe von Replikaten in mindestens 10 bis 20 Fehlerdomänen hoch verfügbar gemacht. Alle Schreibvorgänge werden synchron und dauerhaft durch ein Mehrheitsquorum von Replikaten committet und erst dann gegenüber dem Client bestätigt. Asynchrone Replikation wird regionsübergreifend mit partitionsübergreifender Koordinierung angewendet. DocumentDB garantiert, dass bei einem manuellen, vom Mandanten initiierten Failover keine Daten verloren gehen. Bei einem automatischen Failover garantiert DocumentDB für das Datenverlust-Zeitfenster im Rahmen der SLA eine Obergrenze des konfigurierten Intervalls für begrenzte Veraltung.

## <a id="CustomerFacingSLAMetrics"></a>Kundenorientierte SLA-Metriken
DocumentDB macht die Durchsatz-, Latenz-, Konsistenz- und Verfügbarkeitsmetriken transparent verfügbar. Auf diese Metriken kann programmgesteuert und über das Azure-Portal zugegriffen werden, wie in der folgenden Abbildung zu sehen. Mit Azure Application Insights können Sie außerdem Warnungen für verschiedene Schwellenwerte einrichten.
 
**Konsistenz-, Latenz-, Durchsatz- und Verfügbarkeitsmetriken sind transparent für jeden Mandanten verfügbar.**

![Azure DocumentDB – für Kunden sichtbare SLA-Metriken](./media/documentdb-distribute-data-globally/documentdb-customer-slas.png)

## <a id="Next Steps"></a>Nächste Schritte
* Informationen zum Implementieren der globalen Replikation für Ihr DocumentDB-Konto über das Azure-Portal finden Sie unter [Ausführen einer globalen DocumentDB-Datenbankreplikation mit dem Azure-Portal](documentdb-portal-global-replication.md).
* Informationen zum Implementieren von Architekturen mit mehreren Mastern mit DocumentDB finden Sie unter [Multi-master database architectures with Azure DocumentDB](documentdb-multi-region-writers.md) (Architekturen mit mehreren Mastern mit Azure DocumentDB).
* Weitere Informationen zur Funktionsweise von automatischem und manuellem Failover in DocumentDB finden Sie unter [Regional Failovers in Azure DocumentDB](documentdb-regional-failovers.md) (Regionale Failover in Azure DocumentDB).

## <a id="References"></a>Referenzen
1. Eric Brewer. [Towards Robust Distributed Systems](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf)
2. Eric Brewer. [CAP Twelve Years Later – How the rules have changed](http://informatik.unibas.ch/fileadmin/Lectures/HS2012/CS341/workshops/reportsAndSlides/PresentationKevinUrban.pdf)
3. Gilbert, Lynch. [Brewer&#39;s Conjecture and Feasibility of Consistent, Available, Partition Tolerant Web Services](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf)
4. Daniel Abadi. [Consistency Tradeoffs in Modern Distributed Database Systems Design](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf)
5. Martin Kleppmann. [Please stop calling databases CP or AP](https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html)
6. Peter Bailis et al. [Probabilistic Bounded Staleness (PBS) for Practical Partial Quorums](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
7. Naor and Wool. [Load, Capacity and Availability in Quorum Systems](http://www.cs.utexas.edu/~lorenzo/corsi/cs395t/04S/notes/naor98load.pdf)
8. Herlihy and Wing. [Lineralizability: A correctness condition for concurrent objects](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf)
9. Azure DocumentDB-SLA (letzte Aktualisierung: Dezember 2016)
