---
title: Wann sollte ein elastischer Pool verwendet werden?
description: "Ein elastischer Pool ist eine Sammlung verfügbarer Ressourcen, die von einer Gruppe von elastischen Datenbanken gemeinsam genutzt werden. Dieses Dokument bietet einen Leitfaden, der Ihnen helfen soll, die Eignung eines elastischen Pools für eine Gruppe von Datenbanken zu bewerten."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 3d3941d5-276c-4fd2-9cc1-9fe8b1e4c96c
ms.service: sql-database
ms.custom: multiple databases
ms.devlang: NA
ms.date: 08/08/2016
ms.author: sstein
ms.workload: data-management
ms.topic: get-started-article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: 145cdc5b686692b44d2c3593a128689a56812610
ms.openlocfilehash: 591272fa55d4e3989c95323f3ef65c4508e2822f


---
# <a name="when-should-an-elastic-pool-be-used"></a>Wann sollte ein elastischer Pool verwendet werden?
Bewerten Sie auf Grundlage der Datenbankverwendungsmuster und der Preisunterschiede zwischen einem elastischen Pool und eigenständigen Datenbanken, ob die Verwendung eines elastischen Pools kosteneffizient ist. Darüber hinaus sind weitere Anleitungen enthalten, die Sie bei der Ermittlung der aktuellen Poolgröße für eine vorhandene Reihe von SQL-Datenbanken unterstützen.  

* Eine Übersicht zu Pools für SQL-Datenbanken finden Sie unter [Was ist ein elastischer Azure-Pool?](sql-database-elastic-pool.md).

> [!NOTE]
> Elastische Pools sind in allen Azure-Regionen allgemein verfügbar, mit Ausnahme von „Indien, Westen“. Dort befinden sie sich derzeit in der Vorschauphase.  Die allgemeine Verfügbarkeit von elastischen Pools in dieser Region wird so bald wie möglich bereitgestellt.
> 
> 

## <a name="elastic-pools"></a>Elastische Pools
SaaS-Entwickler erstellen Anwendungen, die auf der obersten von umfangreichen Datenebenen aufsetzen, die wiederum aus zahlreichen Datenbanken bestehen können. Das gängige Anwendungsmuster besteht darin, für jeden Kunden eine eigenständige Datenbank bereitzustellen. Verschiedene Kunden weisen jedoch oft unterschiedliche und unvorhersehbare Verwendungsmuster auf, und es ist schwierig, den Ressourcenbedarf jedes einzelnen Datenbankbenutzers vorherzusagen. Daher stellt der Entwickler möglicherweise zu viele Ressourcen bereit, um den bestmöglichen Datendurchsatz und die optimale Antwortzeit für alle Datenbanken zu gewährleisten. Das ist jedoch mit erheblichen Kosten verbunden. Alternativ stellt der Entwickler weniger Ressourcen bereit und riskiert eine schlechtere Leistung für seine Kunden. Weitere Informationen zu Entwurfsmustern für SaaS-Anwendungen, für die elastische Pools verwendet werden, finden Sie unter [Entwurfsmuster für SaaS-Anwendungen mit mehreren Mandanten und Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md).

Mit Pools für elastische Datenbanken in Azure SQL-Datenbanken sind SaaS-Entwickler in der Lage, das Preis-Leistungs-Verhältnis einer Gruppe von Datenbanken im Rahmen eines vorgegebenen Budgets zu optimieren und gleichzeitig eine flexible Leistung für jede Datenbank sicherzustellen. Mit Pools können Entwickler elastische Datenbanktransaktionseinheiten (Elastic Database Transaction Units, eDTUs) für einen Pool erwerben, der von mehreren Datenbanken genutzt wird, um unvorhersehbare Auslastungszeiten von einzelnen Datenbanken abzufedern. Die eDTU-Anforderungen eines Pools werden anhand der zusammengefassten Auslastung der hierin befindlichen Datenbanken ermittelt. Die Anzahl der für den Pool verfügbaren eDTUs wird vom Budget des Entwicklers gesteuert. Pools vereinfachen es dem Entwickler, die Auswirkungen des Budgets auf die Leistung des Pools und umgekehrt abzuwägen. Der Entwickler fügt dem Pool einfach Datenbanken hinzu, legt die minimale und maximale Anzahl der eDTUs für die Datenbanken fest und bestimmt dann basierend auf dem vorhandenen Budget die eDTUs des Pools. Mithilfe von Pools können Entwickler ihre Dienste problemlos und kontinuierlich ausbauen – vom schlanken Startup bis hin zu etablierten Unternehmen.  

## <a name="when-to-consider-a-pool"></a>Wann sollten Sie einen Pool in Betracht ziehen?
Pools eignen sich hervorragend für eine große Anzahl an Datenbanken mit spezifischen Nutzungsmustern. Im Hinblick auf eine einzelne Datenbank wird dieses Muster durch eine geringe durchschnittliche Auslastung mit relativ wenigen Nutzungslastspitzen gekennzeichnet.

Je mehr Datenbanken Sie einem Pool hinzufügen können, desto größer werden Ihre Ersparnisse. Abhängig vom Auslastungsmuster Ihrer Anwendung können Sie bereits mit nur zwei S3-Datenbanken Ersparnisse erzielen.  

Die folgenden Abschnitte helfen Ihnen einzuschätzen, ob Ihre Datenbanksammlung von einem Pool profitieren kann. In den Beispielen wird von Standardpools ausgegangen. Die Grundlagen gelten jedoch auch für Basic- und Premiumpools.

### <a name="assessing-database-utilization-patterns"></a>Bewerten der Datenbankauslastungsmuster
In der folgenden Abbildung finden Sie ein Beispiel für eine Datenbank, die sich in der Regel im Leerlauf befindet, bei der es allerdings auch regelmäßig zu Auslastungsspitzen kommt. Dieses Auslastungsmuster eignet sich hervorragend für einen Pool:

   ![Eigenständige, für einen Pool geeignete Datenbank](./media/sql-database-elastic-pool-guidance/one-database.png)

In dem vorstehend dargestellten Zeitraum von fünf Minuten schnellt DB1 auf bis zu 90 DTUs. Die mittlere Auslastung liegt jedoch bei unter 5 DTUs. Zum Ausführen dieser Workload in einer eigenständigen Datenbank ist eine S3-Leistungsstufe erforderlich. Allerdings bewirkt dies, dass die meisten Ressourcen in Zeiten geringer Aktivität nicht verwendet werden.

Mit einem Pool können diese ungenutzten DTUs auf mehrere Datenbanken verteilt werden, wodurch sich die Gesamtmenge der erforderlichen DTUs und damit auch die Gesamtkosten verringern.

Ausgehend vom vorherigen Beispiel nehmen wir einmal an, dass es weitere Datenbanken mit ähnlichen Auslastungsmustern wie DB1 gibt. In den beiden folgenden Abbildungen wird die Auslastung von einmal vier, und von weiteren 20 Datenbanken in jeweils einem Graphen dargestellt. Dies soll veranschaulichen, dass sich die Auslastung der Datenbanken zeitlich nicht überlappt:

   ![vier Datenbanken mit einem für einen Pool geeigneten Auslastungsmuster](./media/sql-database-elastic-pool-guidance/four-databases.png)

   ![20 Datenbanken mit einem für einen Pool geeigneten Auslastungsmuster](./media/sql-database-elastic-pool-guidance/twenty-databases.png)

Die zusammengefasste DTU-Auslastung aller 20 Datenbanken wird in der vorstehenden Abbildung durch die schwarze Linie dargestellt. Diese zeigt, dass die zusammengefasste DTU-Auslastung niemals 100 DTUs übersteigt, und gibt an, dass die 20 Datenbanken in diesem Zeitraum 100 eDTUs gemeinsam nutzen können. Dies führt zu einer 20-fachen Verringerung im Hinblick auf die DTUs und einer 13-fachen Preisreduzierung im Vergleich zur Positionierung jeder Datenbank in S3-Leistungsstufen für eigenständige Datenbanken.

Dieses Beispiel ist aus den folgenden Gründen ideal:

* Es gibt große Unterschiede zwischen der Spitzenauslastung und der mittleren Auslastung pro Datenbank.  
* Die Spitzenauslastung jeder Datenbank ergibt sich zu jeweils unterschiedlichen Zeitpunkten.
* Die eDTUs werden von einer großen Anzahl Datenbanken gemeinsam genutzt.

Der Preis eines Pools ergibt sich aus den eDTUs des Pools. Während der Preis pro eDTU für einen Pool 1,5 Mal höher als der Preis pro DTU für eine eigenständige Datenbank ist, können hierbei **Pool-eDTUs von vielen Datenbanken gemeinsam genutzt werden, daher liegt der Preis in vielen Fällen unter der Gesamtanzahl benötigter eDTUs**. Die Unterschiede bei der Preisgestaltung und die gemeinsame Nutzung von eDTUs sind die Basis des Einsparungspotenzials, das von Pools geboten wird.  

Mithilfe der folgenden Faustregeln hinsichtlich der Anzahl von Datenbanken und ihrer Auslastung können Sie sicherstellen, dass Pools kosteneffizienter als Leistungsstufen für eigenständige Datenbanken sind.

### <a name="minimum-number-of-databases"></a>Minimale Anzahl Datenbanken
Wenn die Summe der DTUs für Leistungsstufen für eigenständige Datenbanken mehr als dem 1,5-Fachen der für den Pool benötigten eDTUs entspricht, ist ein elastischer Pool kostengünstiger. Informationen zu den verfügbaren Größen finden Sie unter [eDTUs und Speicherbeschränkungen für elastische Pools und elastische Datenbanken](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools-and-elastic-databases).

***Beispiel***<br>
Es sind mindestens zwei S3-Datenbanken oder mindestens 15 S0-Datenbanken erforderlich, damit ein Pool mit 100 eDTUs kosteneffizienter ist als die Verwendung von Leistungsstufen für eigenständige Datenbanken.

### <a name="maximum-number-of-concurrently-peaking-databases"></a>Maximale Anzahl von gleichzeitig unter Spitzenlast laufenden Datenbanken
Bei der gemeinsamen Nutzung von eDTUs können nicht alle Datenbanken in einem Pool gleichzeitig eDTUs bis zum verfügbaren Grenzwert verwenden, wenn Leistungsstufen für eigenständige Datenbanken verwendet werden. Je weniger Datenbanken gleichzeitig mit Spitzenlast genutzt werden, desto niedriger kann die Pool-eDTU festgelegt werden und desto kosteneffizienter wird der Pool. Im Allgemeinen sollten nicht mehr als zwei Drittel der Datenbanken (67 %) im Pool gleichzeitig den eDTU-Grenzwert erreichen.

***Beispiel***<br>
 Um die Kosten für drei S3-Datenbanken in einem Pool mit 200 eDTUs zu senken, können höchstens zwei dieser Datenbanken gleichzeitig mit Spitzenauslastung ausgeführt werden.  Andernfalls müsste der Pool auf mehr als 200 eDTUs ausgelegt werden, wenn mehr als zwei dieser vier S3-Datenbanken gleichzeitig mit Spitzenauslastung ausgeführt werden.  Wenn die Größe des Pools in mehr als 200 eDTUs geändert wird, müssten dem Pool weitere S3-Datenbanken hinzugefügt werden, damit die Kosten unterhalb der Leistungsstufen für eigenständige Datenbanken bleiben.  

Beachten Sie, dass in diesem Beispiel die Auslastung anderer Datenbanken im Pool nicht berücksichtigt wird. Wenn alle Datenbanken zu einem gegebenen Zeitpunkt eine Spitzenauslastung aufweisen, können weniger als zwei Drittel (67 %) der Datenbanken gleichzeitig eine Spitzenauslastung aufweisen.

### <a name="dtu-utilization-per-database"></a>DTU-Auslastung pro Datenbank
Ein großer Unterschied zwischen der mittleren und der Spitzenauslastung einer Datenbank weist darauf hin, dass es längere Zeiträume mit geringer Auslastung und kurze Zeiträume mit hoher Auslastung gibt. Dieses Auslastungsmuster eignet sich ideal für die gemeinsame, datenbankübergreifende Nutzung von Ressourcen. Eine Datenbank eignet sich für die Aufnahme in einen Pool, wenn die Spitzenauslastung etwa um das 1,5-fache höher als die mittlere Auslastung ist.

***Beispiel***<br>
 Eine S3-Datenbank, die zu Spitzenzeiten 100 DTUs und im Durchschnitt 67 DTUs oder weniger benötigt, ist ein guter Kandidat für die gemeinsame Nutzung von eDTUs in einem Pool.  Alternativ ist auch eine S1-Datenbank, die zu Spitzenzeiten 20 DTUs und im Durchschnitt 13 DTUs oder weniger benötigt, ein guter Kandidat für einen Pool.

## <a name="sizing-an-elastic-pool"></a>Größenanpassung eines elastischen Pools
Die optimale Größe eines Pools hängt von den zusammengefassten eDTUs und den Speicherressourcen ab, die für alle Datenbanken im Pool benötigt werden. Dies setzt voraus, dass Sie die größere der beiden folgenden Mengen ermitteln:

* Maximale Anzahl DTUs, die von allen Datenbanken im Pool verwendet werden.
* Maximale Speicherbytes, die von allen Datenbanken im Pool verwendet werden.

Informationen zu den verfügbaren Größen finden Sie unter [eDTUs und Speicherbeschränkungen für elastische Pools und elastische Datenbanken](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools-and-elastic-databases).

Die SQL-Datenbank ermittelt automatisch den Verlauf der Ressourcennutzung von Datenbanken auf einem vorhandenen SQL-Datenbankserver und empfiehlt eine geeignete Poolkonfiguration im Azure-Portal. Zusätzlich zu den Empfehlungen enthält die Datenbank eine integrierte Funktion, mit der die eDTU-Nutzung einer benutzerdefinierten Gruppe von Datenbanken auf dem Server geschätzt wird. Dadurch können Sie eine Was-wäre-wenn-Analyse durchführen, indem Sie interaktiv Datenbanken zum Pool hinzufügen und entfernen. So können Sie die Ressourcennutzung ermitteln und eine Orientierungshilfe für die Größenanpassung erhalten, bevor Sie Ihre Veränderungen committen. Eine Anleitung finden Sie unter [Überwachen, Verwalten und Skalieren eines Pools für elastische Datenbanken](sql-database-elastic-pool-manage-portal.md).

Tipps für flexiblere Bewertungen der Ressourcennutzung, die Ad-hoc-Schätzungen der Servergröße für V12-Vorgängerversionen ermöglichen, sowie Tipps für die Größeneinschätzung von Datenbanken auf verschiedenen Servern finden Sie unter [PowerShell-Skript zum Ermitteln der für einen Pool für elastische Datenbanken geeigneten Datenbanken](sql-database-elastic-pool-database-assessment-powershell.md).

| Funktion | Portalfunktion | PowerShell-Skript |
|:--- |:--- |:--- |
| Granularität |15 Sekunden |15 Sekunden |
| Berücksichtigt Preisunterschiede zwischen einem Pool und Leistungsstufen für eigenständige Datenbanken |Ja |Nein |
| Ermöglicht die Anpassung der Liste der analysierten Datenbanken |Ja |Ja |
| Ermöglicht die Anpassung des Zeitraums der Analyse |Nein |Ja |
| Ermöglicht die Anpassung der Liste der Datenbanken, die auf verschiedenen Servern analysiert wurden |Nein |Ja |
| Ermöglicht die Anpassung der Liste der Datenbanken, die auf V11-Servern analysiert wurden |Nein |Ja |

Falls Sie keine Tools verwenden können, können Sie anhand der folgenden detaillierten Anweisungen einschätzen, ob ein Pool kostengünstiger ist als eine eigenständige Datenbank:

1. Schätzen Sie die für den Pool benötigten eDTUs nach der folgenden Methode:
   
   MAX(<*Gesamtanzahl Datenbanken* x *durchschnittliche DTU-Nutzung pro Datenbank*>,<br>
   <*Anzahl gleichzeitiger Datenbanken mit Spitzenauslastung* x *DTU-Spitzenauslastung pro Datenbank*)
2. Schätzen Sie den für den Pool benötigten Speicherplatz, indem Sie die Menge der für alle Datenbanken im Pool benötigten Bytes addieren.  Ermitteln Sie dann die eDTU-Poolgröße, die diese Menge an Speicher bietet.  Informationen zu den durch die eDTU-Poolgröße bedingten Speicherbegrenzungen des Pools finden Sie unter [eDTUs und Speicherbeschränkungen für elastische Pools und elastische Datenbanken](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools-and-elastic-databases).
3. Nehmen Sie die größere der eDTU-Schätzungen aus Schritt 1 und Schritt 2.
4. Auf der [SQL-Datenbank Preisseite](https://azure.microsoft.com/pricing/details/sql-database/) finden Sie die kleinste eDTU-Poolgröße, die größer ist als die Schätzung aus Schritt 3.
5. Vergleichen Sie den Poolpreis<< aus Schritt 5 mit dem Preis der entsprechenden Leistungsstufen für eigenständige Datenbanken.

## <a name="summary"></a>Zusammenfassung
Nicht alle eigenständigen Datenbanken eignen sich gut für Pools. Datenbanken mit Auslastungsmustern, die sich durch geringe durchschnittliche Auslastung und relativ seltene Auslastungsspitzen auszeichnen, sind herausragende Kandidaten. Die Auslastungsmuster von Anwendungen sind dynamisch. Nutzen Sie also die Informationen und Tools aus diesem Artikel, um grob einzuschätzen, ob ein Pool für einige oder alle Ihre Datenbanken die richtige Wahl ist. Dieser Artikel ist lediglich ein erster Schritt, um Sie bei der Entscheidung, inwiefern ein elastischer Pool eine geeignete Lösung ist, zu unterstützen. Denken Sie daran, den Verlauf der Ressourcennutzung kontinuierlich zu überwachen, und dass auch die Leistungsebenen aller Ihrer Datenbanken stetig neu bewertet werden müssen. Vergessen Sie nicht, dass Sie Datenbanken ganz einfach in oder aus elastischen Pools verschieben können, und wenn Sie über eine sehr große Anzahl von Datenbanken verfügen, können Sie auch unterschiedliche Pools mit unterschiedlichen Größen erstellen, auf die Sie Ihre Datenbanken verteilen.

## <a name="next-steps"></a>Nächste Schritte
* [Erstellen eines elastischen Pools](sql-database-elastic-pool-create-portal.md)
* [Überwachen und Verwalten eines elastischen Pools](sql-database-elastic-pool-manage-portal.md)
* [SQL-Datenbankoptionen und -leistung: Grundlegendes zum Angebot in den einzelnen Tarifen](sql-database-service-tiers.md)
* [PowerShell-Skript zum Ermitteln der für einen Pool für elastische Datenbanken geeigneten Datenbanken](sql-database-elastic-pool-database-assessment-powershell.md)




<!--HONumber=Dec16_HO2-->


