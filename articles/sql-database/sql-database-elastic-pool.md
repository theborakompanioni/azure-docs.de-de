---
title: "Was sind Pools für elastische Datenbanken? Verwalten mehrerer SQL-Datenbanken – Azure | Microsoft-Dokumentation"
description: "Verwalten und skalieren Sie mehrere SQL-Datenbanken – Hunderte und Tausende - mit elastischen Pools. Ein Preis für Ressourcen, die Sie nach Bedarf verteilen können."
keywords: mehrere Datenbanken, Datenbankressourcen, Datenbankleistung
services: sql-database
documentationcenter: 
author: ddove
manager: jhubbard
editor: 
ms.assetid: b46e7fdc-2238-4b3b-a944-8ab36c5bdb8e
ms.service: sql-database
ms.custom: multiple databases
ms.devlang: NA
ms.date: 05/15/2017
ms.author: ddove
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.translationtype: Human Translation
ms.sourcegitcommit: 95b8c100246815f72570d898b4a5555e6196a1a0
ms.openlocfilehash: de0c9969ec28e7b6e57bc384ac3ec47191096a9b
ms.contentlocale: de-de
ms.lasthandoff: 05/18/2017


---

# <a name="elastic-pools-help-you-manage-and-scale-multiple-sql-databases"></a>Elastische Pools als Hilfe beim Verwalten und Skalieren mehrerer SQL-Datenbanken

Pools für elastische SQL-Datenbanken sind eine einfache, kostengünstige Lösung zum Verwalten und Skalieren mehrerer Datenbanken mit variierenden und unvorhersehbaren Anforderungen. Die Datenbanken in einem elastischen Pool befinden sich auf einem einzelnen Azure SQL-Datenbank-Server und nutzen gemeinsam eine festgelegte Anzahl von Ressourcen ([elastischen Datenbanktransaktionseinheiten [elastic Database Transaction Units, eDTUs]](sql-database-what-is-a-dtu.md)) zu einem festen Preis. Mit Pools für elastische Datenbanken in Azure SQL-Datenbanken sind SaaS-Entwickler in der Lage, das Preis-Leistungs-Verhältnis einer Gruppe von Datenbanken im Rahmen eines vorgegebenen Budgets zu optimieren und gleichzeitig eine flexible Leistung für jede Datenbank sicherzustellen.   

> [!NOTE]
> Elastische Pools sind in allen Azure-Regionen allgemein verfügbar, mit Ausnahme von „Indien, Westen“. Dort befinden sie sich derzeit in der Vorschauphase.  Die allgemeine Verfügbarkeit von elastischen Pools in dieser Region wird so bald wie möglich bereitgestellt.
>

## <a name="overview-of-elastic-pools"></a>Übersicht zu elastischen Pools 

SaaS-Entwickler erstellen Anwendungen, die auf der obersten von umfangreichen Datenebenen aufsetzen, die wiederum aus zahlreichen Datenbanken bestehen können. In einem üblichen Anwendungsmuster wird jedem Kunden eine eigene Datenbank zur Verfügung gestellt. Verschiedene Kunden weisen jedoch oft unterschiedliche und unvorhersehbare Verwendungsmuster auf, und es ist schwierig, den Ressourcenbedarf jedes einzelnen Datenbankbenutzers vorherzusagen. Bisher hatten Sie zwei Optionen: 

- Überdimensionierte Bereitstellung der Ressourcen, die sich an der Nutzung zu Spitzenzeiten orientiert (samt der damit verbundenen Kosten) oder
- Unterdimensionierte Bereitstellung, um Kosten zu Lasten von Leistung und Kundenzufriedenheit während Spitzenzeiten einzusparen. 

Elastische Pools lösen dieses Problem, indem sie sicherstellen, dass die Datenbanken die Leistungsressourcen erhalten, die sie benötigen, und zwar genau zu dem Zeitpunkt, zu dem sie sie benötigen. Sie stellen einen einfachen Ressourcenzuordnungsmechanismus innerhalb eines vorhersagbaren Budgets bereit. Weitere Informationen zu Entwurfsmustern für SaaS-Anwendungen, für die elastische Pools verwendet werden, finden Sie unter [Entwurfsmuster für SaaS-Anwendungen mit mehreren Mandanten und Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md).

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Elastic-databases-helps-SaaS-developers-tame-explosive-growth/player]
>

Mit elastischen Pools können Entwickler [elastische Datenbanktransaktionseinheiten](sql-database-what-is-a-dtu.md) (elastic Database Transaction Units, eDTUs) für einen Pool erwerben, der von mehreren Datenbanken genutzt wird, um unvorhersehbare Auslastungszeiten von einzelnen Datenbanken abzufedern. Die eDTU-Anforderungen eines Pools werden anhand der zusammengefassten Auslastung der hierin befindlichen Datenbanken ermittelt. Die Anzahl der für den Pool verfügbaren eDTUs wird vom Budget des Entwicklers gesteuert. Der Entwickler fügt dem Pool einfach Datenbanken hinzu, legt die minimale und maximale Anzahl der eDTUs für die Datenbanken fest und bestimmt dann basierend auf dem vorhandenen Budget die eDTUs des Pools. Mithilfe von Pools können Entwickler ihre Dienste problemlos und kontinuierlich ausbauen, von schlanken Startups bis hin zu etablierten Unternehmen.

Im Pool können einzelne Datenbanken die automatische Skalierung innerhalb der angegebenen Parameter flexibel automatisch skalieren. Bei hoher Auslastung kann eine Datenbank mehr eDTUs nutzen, um die Anforderungen zu erfüllen. Datenbanken verbrauchen bei geringerer Auslastung weniger Ressourcen und ohne Auslastung gar keine eDTUs. Durch die Bereitstellung von Ressourcen für den gesamten Pool und nicht nur für einzelne Datenbanken vereinfachen Sie Ihre Verwaltungsaufgaben. Außerdem verfügen Sie über ein vorhersagbares Budget für den Pool. Einem vorhandenen Pool können zusätzliche eDTUs hinzugefügt werden, ohne dass es zu Datenbankausfällen kommt. Es gilt jedoch die Ausnahme, dass die Datenbanken unter Umständen verschoben werden müssen, um die zusätzlichen Computeressourcen für die neue eDTU-Reserverierung bereitzustellen. Ebenso können zusätzliche eDTUs, die nicht mehr benötigt werden, zu jedem beliebigen Zeitpunkt aus einem vorhandenen Pool entfernt werden. Und Sie können dem Pool Datenbanken hinzufügen oder Datenbanken aus dem Pool entfernen. Wenn die Ressourcen für eine Datenbank voraussichtlich nicht ausgeschöpft werden, sollten Sie sie entfernen.

Sie können einen elastischen Pool über das [Azure-Portal](sql-database-elastic-pool-manage-portal.md), mit [PowerShell](sql-database-elastic-pool-manage-powershell.md), [Transact-SQL](sql-database-elastic-pool-manage-tsql.md), [C#](sql-database-elastic-pool-manage-csharp.md) oder der REST-API erstellen und verwalten. 

## <a name="when-to-consider-a-pool"></a>Wann sollten Sie einen Pool in Betracht ziehen?
Pools eignen sich hervorragend für eine große Anzahl an Datenbanken mit spezifischen Nutzungsmustern. Im Hinblick auf eine einzelne Datenbank wird dieses Muster durch eine geringe durchschnittliche Auslastung mit relativ wenigen Nutzungslastspitzen gekennzeichnet.

Je mehr Datenbanken Sie einem Pool hinzufügen können, desto größer werden Ihre Ersparnisse. Abhängig vom Auslastungsmuster Ihrer Anwendung können Sie bereits mit nur zwei S3-Datenbanken Ersparnisse erzielen.  

Die folgenden Abschnitte helfen Ihnen einzuschätzen, ob Ihre Datenbanksammlung von einem Pool profitieren kann. In den Beispielen wird von Standardpools ausgegangen. Die Grundlagen gelten jedoch auch für Basic- und Premiumpools.

### <a name="assessing-database-utilization-patterns"></a>Bewerten der Datenbankauslastungsmuster
In der folgenden Abbildung finden Sie ein Beispiel für eine Datenbank, die sich in der Regel im Leerlauf befindet, bei der es allerdings auch regelmäßig zu Auslastungsspitzen kommt. Dieses Auslastungsmuster eignet sich für einen Pool:

   ![eine einzelne, für einen Pool geeignete Datenbank](./media/sql-database-elastic-pool/one-database.png)

Im dargestellten Zeitraum von fünf Minuten schnellt DB1 auf bis zu 90 DTUs. Die mittlere Auslastung liegt jedoch bei unter 5 DTUs. Zum Ausführen dieser Workload in einer einzelnen Datenbank ist eine S3-Leistungsstufe erforderlich. Allerdings bewirkt dies, dass die meisten Ressourcen in Zeiten geringer Aktivität nicht verwendet werden.

Mit einem Pool können diese ungenutzten DTUs auf mehrere Datenbanken verteilt werden, wodurch sich die erforderlichen DTUs und damit auch die Gesamtkosten verringern.

Ausgehend vom vorherigen Beispiel nehmen wir einmal an, dass es weitere Datenbanken mit ähnlichen Auslastungsmustern wie DB1 gibt. In den beiden folgenden Abbildungen wird die Auslastung von einmal vier, und von weiteren 20 Datenbanken in jeweils einem Graphen dargestellt. Dies soll veranschaulichen, dass sich die Auslastung der Datenbanken zeitlich nicht überlappt:

   ![vier Datenbanken mit einem für einen Pool geeigneten Auslastungsmuster](./media/sql-database-elastic-pool/four-databases.png)

  ![20 Datenbanken mit einem für einen Pool geeigneten Auslastungsmuster](./media/sql-database-elastic-pool/twenty-databases.png)

Die zusammengefasste DTU-Auslastung aller 20 Datenbanken wird in der vorstehenden Abbildung durch die schwarze Linie dargestellt. Diese zeigt, dass die zusammengefasste DTU-Auslastung niemals 100 DTUs übersteigt, und gibt an, dass die 20 Datenbanken in diesem Zeitraum 100 eDTUs gemeinsam nutzen können. Dies führt zu einer 20-fachen Verringerung im Hinblick auf die DTUs und einer 13-fachen Preisreduzierung im Vergleich zur Positionierung jeder Datenbank in S3-Leistungsstufen für Einzeldatenbanken.

Dieses Beispiel ist aus den folgenden Gründen ideal:

* Es gibt große Unterschiede zwischen der Spitzenauslastung und der mittleren Auslastung pro Datenbank.  
* Die Spitzenauslastung jeder Datenbank ergibt sich zu jeweils unterschiedlichen Zeitpunkten.
* eDTUs werden von mehreren Datenbanken gemeinsam genutzt.

Der Preis eines Pools ergibt sich aus den eDTUs des Pools. Während der Preis pro eDTU für einen Pool 1,5 Mal höher als der Preis pro DTU für eine Einzeldatenbank ist, können hierbei **Pool-eDTUs von vielen Datenbanken gemeinsam genutzt werden, daher liegt der Preis unter der Gesamtanzahl der benötigten eDTUs**. Die Unterschiede bei der Preisgestaltung und die gemeinsame Nutzung von eDTUs sind die Basis des Einsparungspotenzials, das von Pools geboten wird.  

Die folgenden Faustregeln hinsichtlich der Anzahl der Datenbanken und ihrer Auslastung sollen Ihnen helfen, sicherzustellen, dass Pools kosteneffizienter als Leistungsebenen für einzelne Datenbanken sind.


### <a name="minimum-number-of-databases"></a>Minimale Anzahl Datenbanken
Wenn die Summe der DTUs für Leistungsstufen für einzelne Datenbanken mehr als das 1,5-fache der für den Pool benötigten eDTUs entspricht, ist ein elastischer Pool kostengünstiger. Informationen zu den verfügbaren Größen finden Sie unter [eDTUs und Speicherbeschränkungen für elastische Pools und elastische Datenbanken](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools).

***Beispiel***<br>
Es werden mindestens zwei S3-Datenbanken oder mindestens 15 S0-Datenbanken benötigt, damit ein Pool mit 100 eDTUs kosteneffizienter als die Verwendung von Leistungsebenen für einzelne Datenbanken ist.

### <a name="maximum-number-of-concurrently-peaking-databases"></a>Maximale Anzahl von gleichzeitig unter Spitzenlast laufenden Datenbanken
Bei der gemeinsamen Nutzung von eDTUs können nicht alle Datenbanken in einem Pool gleichzeitig eDTUs bis zum verfügbaren Grenzwert verwenden, wenn Leistungsstufen für einzelne Datenbanken verwendet werden. Je weniger Datenbanken gleichzeitig auf Spitzenlast genutzt werden, desto niedriger kann die Pool-eDTU festgelegt werden und desto kosteneffizienter wird der Pool. Im Allgemeinen sollten nicht mehr als zwei Drittel der Datenbanken (67 %) im Pool gleichzeitig den eDTU-Grenzwert erreichen.

***Beispiel***<br>
Um die Kosten für drei S3-Datenbanken in einem Pool mit 200 eDTUs zu senken, können höchstens zwei dieser Datenbanken gleichzeitig mit Spitzenauslastung ausgeführt werden. Andernfalls müsste der Pool auf mehr als 200 eDTUs ausgelegt werden, wenn mehr als zwei dieser vier S3-Datenbanken gleichzeitig mit Spitzenauslastung ausgeführt werden. Wenn die Größe des Pools auf mehr als 200 eDTUs geändert wird, müssten weitere S3-Datenbanken zum Pool hinzugefügt werden, damit die Kosten unterhalb der Leistungsstufe für einzelne Datenbanken bleiben.

Beachten Sie, dass in diesem Beispiel die Auslastung anderer Datenbanken im Pool nicht berücksichtigt wird. Wenn alle Datenbanken zu einem gegebenen Zeitpunkt eine Spitzenauslastung aufweisen, können weniger als zwei Drittel (67 %) der Datenbanken gleichzeitig eine Spitzenauslastung aufweisen.

### <a name="dtu-utilization-per-database"></a>DTU-Auslastung pro Datenbank
Ein großer Unterschied zwischen der mittleren und der Spitzenauslastung einer Datenbank weist darauf hin, dass es längere Zeiträume mit geringer Auslastung und kurze Zeiträume mit hoher Auslastung gibt. Dieses Auslastungsmuster eignet sich ideal für die gemeinsame, datenbankübergreifende Nutzung von Ressourcen. Eine Datenbank eignet sich für die Aufnahme in einen Pool, wenn die Spitzenauslastung etwa um das 1,5-fache höher als die mittlere Auslastung ist.

***Beispiel***<br>
Eine S3-Datenbank, die zu Spitzenzeiten 100 DTUs und im Durchschnitt 67 DTUs oder weniger benötigt, ist ein guter Kandidat für die gemeinsame Nutzung von eDTUs in einem Pool. Alternativ ist auch eine S1-Datenbank, die zu Spitzenzeiten 20 DTUs und im Durchschnitt 13 DTUs oder weniger benötigt, ein guter Kandidat für einen Pool.

## <a name="sizing-an-elastic-pool"></a>Größenanpassung eines elastischen Pools
Die optimale Größe eines Pools hängt von den zusammengefassten eDTUs und den Speicherressourcen ab, die für alle Datenbanken im Pool benötigt werden. Dies setzt voraus, dass Sie die größere der beiden folgenden Mengen ermitteln:

* Maximale Anzahl DTUs, die von allen Datenbanken im Pool verwendet werden.
* Maximale Speicherbytes, die von allen Datenbanken im Pool verwendet werden.

Informationen zu den verfügbaren Größen finden Sie unter [eDTUs und Speicherbeschränkungen für elastische Pools und elastische Datenbanken](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools).

Die SQL-Datenbank ermittelt automatisch den Verlauf der Ressourcennutzung von Datenbanken auf einem vorhandenen SQL-Datenbankserver und empfiehlt eine geeignete Poolkonfiguration im Azure-Portal. Zusätzlich zu den Empfehlungen enthält die Datenbank eine integrierte Funktion, mit der die eDTU-Nutzung einer benutzerdefinierten Gruppe von Datenbanken auf dem Server geschätzt wird. Dadurch können Sie eine Was-wäre-wenn-Analyse durchführen, indem Sie interaktiv Datenbanken zum Pool hinzufügen und entfernen. So können Sie die Ressourcennutzung ermitteln und eine Orientierungshilfe für die Größenanpassung erhalten, bevor Sie Ihre Veränderungen committen. Eine Anleitung finden Sie unter [Überwachen, Verwalten und Skalieren eines Pools für elastische Datenbanken](sql-database-elastic-pool-manage-portal.md).

Wenn Sie keine Tools verwenden können, kann Ihnen die folgende Anleitung dabei helfen, einzuschätzen, ob ein Pool kostengünstiger als eine einzelne Datenbank ist:

1. Schätzen Sie die für den Pool benötigten eDTUs nach der folgenden Methode:

   MAX(<*Gesamtanzahl Datenbanken* x *durchschnittliche DTU-Nutzung pro Datenbank*>,<br>
   <*Anzahl gleichzeitiger Datenbanken mit Spitzenauslastung* x *DTU-Spitzenauslastung pro Datenbank*)
2. Schätzen Sie den für den Pool benötigten Speicherplatz, indem Sie die Menge der für alle Datenbanken im Pool benötigten Bytes addieren. Ermitteln Sie dann die eDTU-Poolgröße, die diese Menge an Speicher bietet. Informationen zu den durch die eDTU-Poolgröße bedingten Speicherbegrenzungen des Pools finden Sie unter [eDTUs und Speicherbeschränkungen für elastische Pools und elastische Datenbanken](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools).
3. Nehmen Sie die größere der eDTU-Schätzungen aus Schritt 1 und Schritt 2.
4. Auf der [SQL-Datenbank Preisseite](https://azure.microsoft.com/pricing/details/sql-database/) finden Sie die kleinste eDTU-Poolgröße, die größer ist als die Schätzung aus Schritt 3.
5. Vergleichen Sie den Poolpreis aus Schritt 5 mit dem Preis der geeigneten Leistungsstufen für einzelne Datenbanken.

## <a name="edtu-and-storage-limits-for-elastic-pools"></a>eDTUs und Speicherbeschränkungen für elastische Pools

Die folgenden Tabellen beschreiben die Ressourcengrenzwerte des elastischen Pools.  Beachten Sie, dass die Ressourcengrenzwerte einzelner Datenbanken in elastischen Pools im Allgemeinen denen einzelner Datenbanken außerhalb von Pools basierend auf DTU-Anzahl und Dienstebene entsprechen.  Auf eine S2-Datenbank können z.B. max. 120 Mitarbeiter gleichzeitig zugreifen.  Also können auch 120 Mitarbeiter gleichzeitig auf eine Datenbank in einem Standard-Pool zugreifen, wenn max. 50 DTUs pro Datenbank im Pool zulässig sind (entspricht S2).

[!INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-pools.md)]

Wenn alle DTUs eines elastischen Pools verwendet werden, erhält jede Datenbank im Pool gleich viel Ressourcen zum Verarbeiten von Abfragen.  Der SQL-Datenbank-Dienst bietet eine faire gemeinsame Nutzung von Ressourcen durch Datenbanken, indem gleiche Slices an Computezeit zugesichert werden. Diese faire gemeinsame Nutzung in elastischen Pools ergänzt jegliche Ressourcen, die jeder Datenbank auf andere Weise garantiert werden, wenn das DTU-Minimum pro Datenbank auf einen Wert ungleich null festgelegt ist.

## <a name="database-properties-for-pooled-databases"></a>Eigenschaften von Datenbanken in einem Pool

Die folgende Tabelle beschreibt die Eigenschaften von Datenbanken in einem Pool.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Max. Anz. von eDTUs pro Datenbank |Die maximale Anzahl von eDTUs, die jede Datenbank im Pool verwenden kann, sofern basierend auf der Nutzung durch andere Datenbanken im Pool verfügbar.  Die maximale Anzahl der eDTUs pro Datenbank ist keine Ressourcengarantie für eine Datenbank.  Dies ist eine globale Einstellung, die für alle Datenbanken im Pool gilt. Legen Sie die maximale Anzahl der eDTUs pro Datenbank hoch genug fest, sodass Spitzen bei der Datenbanknutzung verarbeitet werden können. Sie sollten ein gewisses Maß an Mehrlast einplanen, da für den Pool im Allgemeinen von Nutzungsmustern starker und schwacher Auslastung ausgegangen wird, bei der aber nicht alle Datenbanken gleichzeitig stark ausgelastet sind. Angenommen, die Spitzenauslastung pro Datenbank beträgt 20 eDTUs und betrifft nur 20 % der 100 Datenbanken im Pool.  Wenn die eDTU-Höchstanzahl pro Datenbank auf 20 eDTUs festgelegt ist, ist es sinnvoll, die fünffache Mehrlast für den Pool einzuplanen und die eDTUs pro Pool auf 400 festzulegen. |
| Min. Anz. von eDTUs pro Datenbank |Die minimale Anzahl der eDTUs, die für jede Datenbank im Pool garantiert werden können.  Dies ist eine globale Einstellung, die für alle Datenbanken im Pool gilt. Die Mindestanzahl der eDTUs pro Datenbank kann auf 0 festgelegt werden. Dies ist auch der Standardwert. Diese Eigenschaft ist auf einen Wert zwischen 0 und der durchschnittlichen eDTU-Nutzung pro Datenbank festgelegt. Das Produkt aus der Anzahl von Datenbanken im Pool und der Mindestzahl von eDTUs pro Datenbank darf die tatsächliche Anzahl der eDTUs pro Pool nicht übersteigen.  Wenn ein Pool beispielsweise 20 Datenbanken umfasst und die Mindestanzahl der eDTUs pro Datenbank auf 10 eDTUs festgelegt hat, müssen mindestens 200 eDTUs pro Pool festgelegt sein. |
| Max. Datenspeicher pro Datenbank |Der maximale Speicherplatz für eine Datenbank in einem Pool. In einem Pool zusammengefasste Datenbanken nutzen den Poolspeicher gemeinsam, daher ist der Datenbankspeicher auf den jeweils kleineren Wert des verbleibenden Poolspeichers oder des maximalen Speicherplatzes pro Datenbank beschränkt. Der maximale Speicherplatz pro Datenbank bezieht sich auf die maximale Größe der Datendateien und beinhaltet nicht den von Protokolldateien belegten Speicherplatz. |
|||

## <a name="elastic-jobs"></a>Elastische Aufträge
Mit einem Pool werden die Verwaltungsaufgaben vereinfacht, indem Skripts in **[elastischen Aufträgen](sql-database-elastic-jobs-overview.md)** ausgeführt werden. Durch einen elastischen Auftrag werden die meisten aufwändigen Schritte beseitigt, die bei der Verwendung einer großen Anzahl von Datenbanken anfallen. Lesen Sie sich [Erste Schritte mit Aufträgen für die elastische Datenbank](sql-database-elastic-jobs-getting-started.md) durch, bevor Sie beginnen.

Weitere Informationen zu anderen Datenbanktools für die Verwendung mit mehreren Datenbanken finden Sie unter [Horizontales Hochskalieren mit Azure SQL-Datenbank](sql-database-elastic-scale-introduction.md).

## <a name="business-continuity-features-for-databases-in-a-pool"></a>Funktionen der Geschäftskontinuität für Datenbanken in einem Pool
In einem Pool zusammengefasste Datenbanken unterstützen in der Regel die gleichen [Geschäftskontinuitätsfeatures](sql-database-business-continuity.md), die auch bei Einzeldatenbanken zur Verfügung stehen.

### <a name="point-in-time-restore"></a>Point-in-Time-Wiederherstellung
Bei der Point-in-Time-Wiederherstellung werden automatische Datenbanksicherungen verwendet, um den Status einer Datenbank in einem Pool zu einem bestimmten Zeitpunkt wiederherzustellen. Siehe [Point-in-Time-Wiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore)

### <a name="geo-restore"></a>Geografische Wiederherstellung
Die geografische Wiederherstellung ist die Standardoption für die Wiederherstellung, wenn eine Datenbank aufgrund eines Vorfalls in der Region, in der die Datenbank gehostet wird, nicht verfügbar ist. Siehe [Wiederherstellen einer Azure SQL-Datenbank oder Failover auf eine sekundäre Datenbank](sql-database-disaster-recovery.md)

### <a name="active-geo-replication"></a>Aktive Georeplikation
Für Anwendungen, für die umfangreichere Wiederherstellungsanforderungen erforderlich sind, als die Geowiederherstellung bieten kann, konfigurieren Sie [aktive Georeplikation](sql-database-geo-replication-overview.md).

## <a name="next-steps"></a>Nächste Schritte

* Sie können einen elastischen Pool über das [Azure-Portal](sql-database-elastic-pool-manage-portal.md), mit [PowerShell](sql-database-elastic-pool-manage-powershell.md), [Transact-SQL](sql-database-elastic-pool-manage-tsql.md), [C#](sql-database-elastic-pool-manage-csharp.md) oder der REST-API erstellen und verwalten.
* Ein Video finden Sie unter [Microsoft Virtual Academy video course on Azure SQL Database elastic capabilities](https://mva.microsoft.com/training-courses/elastic-database-capabilities-with-azure-sql-db-16554) (Microsoft Virtual Academy-Videokurs zu Funktionen elastischer Datenbanken in Azure SQL-Datenbank).
* Weitere Informationen zu Entwurfsmustern für SaaS-Anwendungen, für die elastische Pools verwendet werden, finden Sie unter [Entwurfsmuster für SaaS-Anwendungen mit mehreren Mandanten und Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md).

