---
title: "Was sind Pools für elastische Datenbanken? Verwalten mehrerer SQL-Datenbanken – Azure | Microsoft-Dokumentation"
description: "Verwalten und skalieren Sie mehrere SQL-Datenbanken – Hunderte und Tausende - mit elastischen Pools. Ein Preis für Ressourcen, die Sie nach Bedarf verteilen können."
keywords: mehrere Datenbanken, Datenbankressourcen, Datenbankleistung
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: b46e7fdc-2238-4b3b-a944-8ab36c5bdb8e
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: NA
ms.date: 07/31/2017
ms.author: carlrab
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.translationtype: HT
ms.sourcegitcommit: 818f7756189ed4ceefdac9114a0b89ef9ee8fb7a
ms.openlocfilehash: 2e0d9067cd942fccf9eeb2750cff1d1f3b478eba
ms.contentlocale: de-de
ms.lasthandoff: 07/14/2017

---

# <a name="elastic-pools-help-you-manage-and-scale-multiple-sql-databases"></a>Elastische Pools als Hilfe beim Verwalten und Skalieren mehrerer SQL-Datenbanken

Pools für elastische SQL-Datenbanken sind eine einfache, kostengünstige Lösung zum Verwalten und Skalieren mehrerer Datenbanken mit variierenden und unvorhersehbaren Anforderungen. Die Datenbanken in einem elastischen Pool befinden sich auf einem einzelnen Azure SQL-Datenbank-Server und nutzen gemeinsam eine festgelegte Anzahl von Ressourcen ([elastischen Datenbanktransaktionseinheiten [elastic Database Transaction Units, eDTUs]](sql-database-what-is-a-dtu.md)) zu einem festen Preis. Mit Pools für elastische Datenbanken in Azure SQL-Datenbanken sind SaaS-Entwickler in der Lage, das Preis-Leistungs-Verhältnis einer Gruppe von Datenbanken im Rahmen eines vorgegebenen Budgets zu optimieren und gleichzeitig eine flexible Leistung für jede Datenbank sicherzustellen.   

> [!NOTE]
> Elastische Pools sind in allen Azure-Regionen allgemein verfügbar, mit Ausnahme von „Indien, Westen“. Dort befinden sie sich derzeit in der Vorschauphase.  Die allgemeine Verfügbarkeit von elastischen Pools in dieser Region wird so bald wie möglich bereitgestellt.
>

## <a name="what-are-sql-elastic-pools"></a>Was sind Pools für elastische SQL-Datenbanken? 

SaaS-Entwickler erstellen Anwendungen, die auf der obersten von umfangreichen Datenebenen aufsetzen, die wiederum aus zahlreichen Datenbanken bestehen können. In einem üblichen Anwendungsmuster wird jedem Kunden eine eigene Datenbank zur Verfügung gestellt. Verschiedene Kunden weisen jedoch oft unterschiedliche und unvorhersehbare Verwendungsmuster auf, und es ist schwierig, den Ressourcenbedarf jedes einzelnen Datenbankbenutzers vorherzusagen. Bisher hatten Sie zwei Optionen: 

- Überdimensionierte Bereitstellung der Ressourcen, die sich an der Nutzung zu Spitzenzeiten orientiert (samt der damit verbundenen Kosten) oder
- Unterdimensionierte Bereitstellung, um Kosten zu Lasten von Leistung und Kundenzufriedenheit während Spitzenzeiten einzusparen. 

Elastische Pools lösen dieses Problem, indem sie sicherstellen, dass die Datenbanken die Leistungsressourcen erhalten, die sie benötigen, und zwar genau zu dem Zeitpunkt, zu dem sie sie benötigen. Sie stellen einen einfachen Ressourcenzuordnungsmechanismus innerhalb eines vorhersagbaren Budgets bereit. Weitere Informationen zu Entwurfsmustern für SaaS-Anwendungen, für die elastische Pools verwendet werden, finden Sie unter [Entwurfsmuster für SaaS-Anwendungen mit mehreren Mandanten und Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md).

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Elastic-databases-helps-SaaS-developers-tame-explosive-growth/player]
>

Mit elastischen Pools können Entwickler [elastische Datenbanktransaktionseinheiten](sql-database-what-is-a-dtu.md) (elastic Database Transaction Units, eDTUs) für einen Pool erwerben, der von mehreren Datenbanken genutzt wird, um unvorhersehbare Auslastungszeiten von einzelnen Datenbanken abzufedern. Die eDTU-Anforderungen eines Pools werden anhand der zusammengefassten Auslastung der hierin befindlichen Datenbanken ermittelt. Die Anzahl der für den Pool verfügbaren eDTUs wird vom Budget des Entwicklers gesteuert. Der Entwickler fügt dem Pool einfach Datenbanken hinzu, legt die minimale und maximale Anzahl der eDTUs für die Datenbanken fest und bestimmt dann basierend auf dem vorhandenen Budget die eDTUs des Pools. Mithilfe von Pools können Entwickler ihre Dienste problemlos und kontinuierlich ausbauen, von schlanken Startups bis hin zu etablierten Unternehmen.

Im Pool können einzelne Datenbanken die automatische Skalierung innerhalb der angegebenen Parameter flexibel automatisch skalieren. Bei hoher Auslastung kann eine Datenbank mehr eDTUs nutzen, um die Anforderungen zu erfüllen. Datenbanken verbrauchen bei geringerer Auslastung weniger Ressourcen und ohne Auslastung gar keine eDTUs. Durch die Bereitstellung von Ressourcen für den gesamten Pool und nicht nur für einzelne Datenbanken vereinfachen Sie Ihre Verwaltungsaufgaben. Außerdem verfügen Sie über ein vorhersagbares Budget für den Pool. Einem vorhandenen Pool können zusätzliche eDTUs hinzugefügt werden, ohne dass es zu Datenbankausfällen kommt. Es gilt jedoch die Ausnahme, dass die Datenbanken unter Umständen verschoben werden müssen, um die zusätzlichen Computeressourcen für die neue eDTU-Reserverierung bereitzustellen. Ebenso können zusätzliche eDTUs, die nicht mehr benötigt werden, zu jedem beliebigen Zeitpunkt aus einem vorhandenen Pool entfernt werden. Und Sie können dem Pool Datenbanken hinzufügen oder Datenbanken aus dem Pool entfernen. Wenn die Ressourcen für eine Datenbank voraussichtlich nicht ausgeschöpft werden, sollten Sie sie entfernen.

Sie können einen elastischen Pool über das [Azure-Portal](sql-database-elastic-pool-manage-portal.md), mit [PowerShell](sql-database-elastic-pool-manage-powershell.md), [Transact-SQL](sql-database-elastic-pool-manage-tsql.md), [C#](sql-database-elastic-pool-manage-csharp.md) oder der REST-API erstellen und verwalten. 

## <a name="when-should-you-consider-a-sql-database-elastic-pool"></a>Wann sollten Sie einen Pool für elastische SQL-Datenbanken erwägen?

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

## <a name="how-do-i-choose-the-correct-pool-size"></a>Wie wähle ich die richtige Poolgröße?

Die optimale Größe eines Pools hängt von den zusammengefassten eDTUs und den Speicherressourcen ab, die für alle Datenbanken im Pool benötigt werden. Dies setzt voraus, dass Sie die größere der beiden folgenden Mengen ermitteln:

* Maximale Anzahl DTUs, die von allen Datenbanken im Pool verwendet werden.
* Maximale Speicherbytes, die von allen Datenbanken im Pool verwendet werden.

Informationen zu den verfügbaren Größen finden Sie unter [eDTUs und Speicherbeschränkungen für elastische Pools und elastische Datenbanken](#what-are-the-resource-limits-for-elastic-pools).

Die SQL-Datenbank ermittelt automatisch den Verlauf der Ressourcennutzung von Datenbanken auf einem vorhandenen SQL-Datenbankserver und empfiehlt eine geeignete Poolkonfiguration im Azure-Portal. Zusätzlich zu den Empfehlungen enthält die Datenbank eine integrierte Funktion, mit der die eDTU-Nutzung einer benutzerdefinierten Gruppe von Datenbanken auf dem Server geschätzt wird. Dadurch können Sie eine Was-wäre-wenn-Analyse durchführen, indem Sie interaktiv Datenbanken zum Pool hinzufügen und entfernen. So können Sie die Ressourcennutzung ermitteln und eine Orientierungshilfe für die Größenanpassung erhalten, bevor Sie Ihre Veränderungen committen. Eine Anleitung finden Sie unter [Überwachen, Verwalten und Skalieren eines Pools für elastische Datenbanken](sql-database-elastic-pool-manage-portal.md).

Wenn Sie keine Tools verwenden können, kann Ihnen die folgende Anleitung dabei helfen, einzuschätzen, ob ein Pool kostengünstiger als eine einzelne Datenbank ist:

1. Schätzen Sie die für den Pool benötigten eDTUs nach der folgenden Methode:

   MAX(<*Gesamtanzahl Datenbanken* x *durchschnittliche DTU-Nutzung pro Datenbank*>,<br>
   <*Anzahl gleichzeitiger Datenbanken mit Spitzenauslastung* x *DTU-Spitzenauslastung pro Datenbank*)
2. Schätzen Sie den für den Pool benötigten Speicherplatz, indem Sie die Menge der für alle Datenbanken im Pool benötigten Bytes addieren. Ermitteln Sie dann die eDTU-Poolgröße, die diese Menge an Speicher bietet. Informationen zu den durch die eDTU-Poolgröße bedingten Speicherbegrenzungen des Pools finden Sie unter [eDTUs und Speicherbeschränkungen für elastische Pools und elastische Datenbanken](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools).
3. Nehmen Sie die größere der eDTU-Schätzungen aus Schritt 1 und Schritt 2.
4. Auf der [SQL-Datenbank Preisseite](https://azure.microsoft.com/pricing/details/sql-database/) finden Sie die kleinste eDTU-Poolgröße, die größer ist als die Schätzung aus Schritt 3.
5. Vergleichen Sie den Poolpreis aus Schritt 5 mit dem Preis der geeigneten Leistungsstufen für einzelne Datenbanken.

### <a name="changing-elastic-pool-resources"></a>Ändern der Ressourcen elastischer Pools

Sie können die Ressourcen, die in einem elastischen Pool verringern sind, basierend auf den Anforderungen erhöhen oder verringern.

* Änderungen der minimalen oder maximalen Anzahl der eDTUs pro Datenbank werden in der Regel in maximal 5 Minuten durchgeführt.
* Änderungen der eDTUs pro Pool hängen von der Gesamtmenge des Speicherplatzes aller Datenbanken im Pool ab. Änderungen dauern durchschnittlich 90 Minuten oder weniger pro 100 GB. Wenn beispielsweise der gesamte, von allen Datenbanken im Pool verwendete Speicherplatz 200 GB beträgt, ist für die Änderung der eDTUs pro Pool eine Latenzzeit von drei Stunden oder weniger zu erwarten.

## <a name="what-are-the-resource-limits-for-elastic-pools"></a>Welche Ressourcengrenzwerte gelten für elastische Pools?

Die folgenden Tabellen beschreiben die Ressourcengrenzwerte des elastischen Pools.  Beachten Sie, dass die Ressourcengrenzwerte einzelner Datenbanken in elastischen Pools im Allgemeinen denen einzelner Datenbanken außerhalb von Pools basierend auf DTU-Anzahl und Dienstebene entsprechen.  Auf eine S2-Datenbank können z.B. max. 120 Mitarbeiter gleichzeitig zugreifen.  Also können auch 120 Mitarbeiter gleichzeitig auf eine Datenbank in einem Standard-Pool zugreifen, wenn max. 50 DTUs pro Datenbank im Pool zulässig sind (entspricht S2).

[!INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-pools.md)]

Wenn alle DTUs eines elastischen Pools verwendet werden, erhält jede Datenbank im Pool gleich viel Ressourcen zum Verarbeiten von Abfragen.  Der SQL-Datenbank-Dienst bietet eine faire gemeinsame Nutzung von Ressourcen durch Datenbanken, indem gleiche Slices an Computezeit zugesichert werden. Diese faire gemeinsame Nutzung in elastischen Pools ergänzt jegliche Ressourcen, die jeder Datenbank auf andere Weise garantiert werden, wenn das DTU-Minimum pro Datenbank auf einen Wert ungleich null festgelegt ist.

### <a name="database-properties-for-pooled-databases"></a>Eigenschaften von Datenbanken in einem Pool

Die folgende Tabelle beschreibt die Eigenschaften von Datenbanken in einem Pool.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Max. Anz. von eDTUs pro Datenbank |Die maximale Anzahl von eDTUs, die jede Datenbank im Pool verwenden kann, sofern basierend auf der Nutzung durch andere Datenbanken im Pool verfügbar.  Die maximale Anzahl der eDTUs pro Datenbank ist keine Ressourcengarantie für eine Datenbank.  Dies ist eine globale Einstellung, die für alle Datenbanken im Pool gilt. Legen Sie die maximale Anzahl der eDTUs pro Datenbank hoch genug fest, sodass Spitzen bei der Datenbanknutzung verarbeitet werden können. Sie sollten ein gewisses Maß an Mehrlast einplanen, da für den Pool im Allgemeinen von Nutzungsmustern starker und schwacher Auslastung ausgegangen wird, bei der aber nicht alle Datenbanken gleichzeitig stark ausgelastet sind. Angenommen, die Spitzenauslastung pro Datenbank beträgt 20 eDTUs und betrifft nur 20 % der 100 Datenbanken im Pool.  Wenn die eDTU-Höchstanzahl pro Datenbank auf 20 eDTUs festgelegt ist, ist es sinnvoll, die fünffache Mehrlast für den Pool einzuplanen und die eDTUs pro Pool auf 400 festzulegen. |
| Min. Anz. von eDTUs pro Datenbank |Die minimale Anzahl der eDTUs, die für jede Datenbank im Pool garantiert werden können.  Dies ist eine globale Einstellung, die für alle Datenbanken im Pool gilt. Die Mindestanzahl der eDTUs pro Datenbank kann auf 0 festgelegt werden. Dies ist auch der Standardwert. Diese Eigenschaft ist auf einen Wert zwischen 0 und der durchschnittlichen eDTU-Nutzung pro Datenbank festgelegt. Das Produkt aus der Anzahl von Datenbanken im Pool und der Mindestzahl von eDTUs pro Datenbank darf die tatsächliche Anzahl der eDTUs pro Pool nicht übersteigen.  Wenn ein Pool beispielsweise 20 Datenbanken umfasst und die Mindestanzahl der eDTUs pro Datenbank auf 10 eDTUs festgelegt hat, müssen mindestens 200 eDTUs pro Pool festgelegt sein. |
| Max. Datenspeicher pro Datenbank |Der maximale Speicherplatz für eine Datenbank in einem Pool. In einem Pool zusammengefasste Datenbanken nutzen den Poolspeicher gemeinsam, daher ist der Datenbankspeicher auf den jeweils kleineren Wert des verbleibenden Poolspeichers oder des maximalen Speicherplatzes pro Datenbank beschränkt. Der maximale Speicherplatz pro Datenbank bezieht sich auf die maximale Größe der Datendateien und beinhaltet nicht den von Protokolldateien belegten Speicherplatz. |
|||

## <a name="using-other-sql-database-features-with-elastic-pools"></a>Verwenden anderer SQL Datenbank-Funktionen mit elastischen Pools

### <a name="elastic-jobs-and-elastic-pools"></a>Elastische Aufträge und elastische Pools

Mit einem Pool werden die Verwaltungsaufgaben vereinfacht, indem Skripts in **[elastischen Aufträgen](sql-database-elastic-jobs-overview.md)** ausgeführt werden. Durch einen elastischen Auftrag werden die meisten aufwändigen Schritte beseitigt, die bei der Verwendung einer großen Anzahl von Datenbanken anfallen. Lesen Sie sich [Erste Schritte mit Aufträgen für die elastische Datenbank](sql-database-elastic-jobs-getting-started.md) durch, bevor Sie beginnen.

Weitere Informationen zu anderen Datenbanktools für die Verwendung mit mehreren Datenbanken finden Sie unter [Horizontales Hochskalieren mit Azure SQL-Datenbank](sql-database-elastic-scale-introduction.md).

### <a name="business-continuity-options-for-databases-in-an-elastic-pool"></a>Optionen für Geschäftskontinuität für Datenbanken in einem elastischen Pool
In einem Pool zusammengefasste Datenbanken unterstützen in der Regel die gleichen [Geschäftskontinuitätsfeatures](sql-database-business-continuity.md), die auch bei Einzeldatenbanken zur Verfügung stehen.

- **Point-in-Time-Wiederherstellung**: Bei der Point-in-Time-Wiederherstellung werden automatische Datenbanksicherungen verwendet, um den Status einer Datenbank in einem Pool zu einem bestimmten Zeitpunkt wiederherzustellen. Siehe [Point-in-Time-Wiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore)

- **Geografische Wiederherstellung**: Die geografische Wiederherstellung ist die Standardoption für die Wiederherstellung, wenn eine Datenbank aufgrund eines Vorfalls in der Region, in der die Datenbank gehostet wird, nicht verfügbar ist. Siehe [Wiederherstellen einer Azure SQL-Datenbank oder Failover auf eine sekundäre Datenbank](sql-database-disaster-recovery.md)

- **Aktive Georeplikation**: Für Anwendungen, für die umfangreichere Wiederherstellungsanforderungen erforderlich sind, als die Geowiederherstellung bieten kann, konfigurieren Sie [aktive Georeplikation](sql-database-geo-replication-overview.md).

## <a name="manage-sql-database-elastic-pools-using-the-azure-portal"></a>Verwalten von Pools für elastische SQL-Datenbanken im Azure-Portal

### <a name="creating-a-new-sql-database-elastic-pool-using-the-azure-portal"></a>Erstellen eines neuen Pools für elastische SQL-Datenbanken im Azure-Portal

Zur Erstellung eines Pools für elastische Datenbanken im Azure-Portal gibt es zwei Möglichkeiten. Sie können einen Pool von Grund auf neu erstellen, wenn Sie genau wissen, wie der Pool aussehen soll. Sie können aber auch mit einer Empfehlung des Diensts beginnen. SQL-Datenbank verfügt über integrierte Intelligenz, die auf der Grundlage der Telemetriedaten Ihrer bisherigen Datenbanknutzung ein Setup für einen Pool für elastische Datenbanken empfiehlt, falls dieses für Sie kostengünstiger ist. 

Am einfachsten verschieben Sie vorhandene Datenbanken in einen Pool für elastische Datenbanken, indem Sie im Portal auf der Grundlage eines bereits vorhandenen **Serverblatts** einen Pool für elastische Datenbanken erstellen. Sie können einen Pool für elastische Datenbanken aber auch erstellen, indem Sie im **Marketplace** nach **Elastischer SQL-Pool** suchen oder auf dem Navigationsblatt **Elastische SQL-Pools** auf **+Hinzufügen** klicken. Im Rahmen dieses Poolbereitstellungsworkflows können Sie einen neuen oder einen bereits vorhandenen Server angeben.

> [!NOTE]
> Auf einem Server können mehrere Pools erstellt werden, es ist jedoch nicht möglich, Datenbanken von verschiedenen Servern im gleichen Pool zusammenzufassen.
>  

Der Tarif für den Pool bestimmt die Features für die elastischen Datenbanken im Pool, die maximale Anzahl von eDTUs (eDTU MAX) und den für jede Datenbank verfügbaren Speicher (GB). Weitere Informationen finden Sie unter [Tarife](#edtu-and-storage-limits-for-elastic-pools).

Klicken Sie auf **Tarif**, wählen sie den gewünschten Tarif aus, und klicken Sie anschließend auf **Auswählen**, um den Tarif für den Pools zu ändern.

> [!IMPORTANT]
> Nachdem Sie den Tarif ausgewählt und Ihre Änderungen übernommen haben, indem Sie im letzten Schritt auf **OK** geklickt haben, können Sie den Tarif des Pools nicht mehr ändern. Wenn Sie den Tarif eines vorhandenen Pools für elastische Datenbanken ändern möchten, müssen Sie einen Pool für elastische Datenbanken unter dem gewünschten Tarif erstellen und die Datenbanken dann in diesen neuen Pool migrieren.
>

Wenn die von Ihnen verwendeten Datenbanken über ausreichend Telemetriedaten zur bisherigen Nutzung verfügen, werden der Graph **Estimated eDTU and GB usage** (Geschätzte eDTU- und GB-Nutzung) und das Balkendiagramm **Actual eDTU usage** (Tatsächliche eDTU-Nutzung) aktualisiert. Diese helfen Ihnen, Konfigurationsentscheidungen zu treffen. Außerdem erhalten Sie vom Dienst unter Umständen eine Empfehlungsnachricht zur richtigen Dimensionierung des Pools.

Der SQL-Datenbankdienst bewertet den Nutzungsverlauf und empfiehlt einen oder mehrere Pools, wenn dies kostengünstiger als die Verwendung von Einzeldatenbanken ist. Jede Empfehlung ist mit einer eindeutigen Teilmenge der Datenbanken des Servers konfiguriert, die optimal zum Pool passen.

![Empfohlener Pool](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)  

Die Poolempfehlung umfasst Folgendes:

- Tarif für den Pool (Basic, Standard, Premium oder Premium RS)
- Passende **POOL-eDTUs** (auch als maximale eDTUs pro Pool bezeichnet)
- **eDTU maximal** und **eDTU minimal** pro Datenbank
- Liste der empfohlenen Datenbanken für den Pool

> [!IMPORTANT]
> Der Dienst berücksichtigt bei der Empfehlung von Pools die Telemetrie der letzten 30 Tage. Damit eine Datenbank für einen Pool für elastische Datenbanken infrage kommt, muss sie seit mindestens sieben Tagen vorhanden sein. Datenbanken, die sich bereits in einem elastischen Pool befinden, werden nicht als Kandidaten für elastische Pools empfohlen.
>

Der Dienst bewertet den Ressourcenbedarf und die Kosteneffizienz beim Verschieben der Einzeldatenbanken auf jeder Dienstebene in Pools auf der gleichen Ebene. Beispielsweise werden alle Datenbanken der Standard-Edition auf einem Server entsprechend ihrer Eignung für einen elastischen Pool der Standard-Edition bewertet. Dies bedeutet, dass der Dienst keine tarifübergreifenden Empfehlungen trifft, also etwa eine Standard-Datenbank nicht in einen Premium-Pool verschiebt.

Nach dem Hinzufügen von Datenbanken zum Pool werden auf der Grundlage des Nutzungsverlaufs der von Ihnen ausgewählten Datenbanken dynamisch Empfehlungen generiert. Diese Empfehlungen werden im Diagramm zur eDTU- und GB-Nutzung sowie im Empfehlungsbanner oben auf dem Blatt **Pool konfigurieren** angezeigt. Die Empfehlungen unterstützen Sie bei der Erstellung eines Pools für elastische Datenbanken, der speziell für Ihre Datenbanken optimiert ist.

![Dynamische Empfehlungen](./media/sql-database-elastic-pool-create-portal/dynamic-recommendation.png)

### <a name="manage-and-monitor-an-elastic-pool"></a>Verwalten und Überwachen eines Pools für elastische Datenbanken

Im Azure-Portal können Sie die Verwendung eines elastischen Pools sowie der darin enthaltenen Datenbanken überwachen. Außerdem können Sie einen Änderungssatz für Ihren elastischen Pool erstellen und alle Änderungen gleichzeitig übermitteln. So können Sie etwa Datenbanken hinzufügen oder entfernen, die Einstellungen des elastischen Pools ändern oder Ihre Datenbankeinstellungen anpassen.

Die Abbildung weiter unten zeigt ein Beispiel für einen Pool für elastische Datenbanken. Die Ansicht enthält Folgendes:

*  Diagramme zur Überwachung der Ressourcenverwendung durch den elastischen Pool und die im Pool enthaltenen Datenbanken.
*  Die Schaltfläche **Pool konfigurieren** zum Vornehmen von Änderungen an dem elastischen Pool.
*  Die Schaltfläche **Datenbank erstellen** zum Erstellen einer Datenbank und Hinzufügen der Datenbank zum aktuellen Pool für elastische Datenbanken.
*  Elastische Aufträge zum Verwalten einer großen Anzahl von Datenbanken durch Ausführen von Transact-SQL-Skripts für alle Datenbanken in einer Liste.

![Poolansicht](./media/sql-database-elastic-pool-manage-portal/basic.png)

Navigieren Sie zu einem bestimmten Pool, um dessen Ressourcenverwendung anzuzeigen. In der Standardkonfiguration wird für den Pool die Speicher- und eDTU-Verwendung der letzten Stunde angezeigt. Das Diagramm kann mit anderen Metriken und Zeitfenstern konfiguriert werden. Klicken Sie unter **Überwachung des elastischen Pools** auf das Diagramm **Ressourcenverwendung**, um eine detaillierte Ansicht der angegebenen Metriken im angegebenen Zeitfenster anzuzeigen.

![Überwachung des elastischen Pools](./media/sql-database-elastic-pool-manage-portal/basic-2.png)

![Blatt "Metrik"](./media/sql-database-elastic-pool-manage-portal/metric.png)

### <a name="to-customize-the-chart-display"></a>So passen Sie die Darstellung des Diagramms an

Sie können das Diagramm und das Metrikblatt bearbeiten, um andere Metriken wie etwa die prozentuale Verwendung von CPU, Daten-E/A und Protokoll-E/A anzuzeigen.

![Klicken auf „Bearbeiten“](./media/sql-database-elastic-pool-manage-portal/edit-metric.png)

Wählen Sie auf dem Formular **Diagramm bearbeiten** einen Zeitbereich („Letzte Stunde“, „Heute“ oder „Letzte Woche“) aus, oder klicken Sie auf **Benutzerdefiniert**, um einen beliebigen Datumsbereich innerhalb der letzten beiden Wochen auszuwählen. Sie können zwischen einem Balken- oder Liniendiagramm wählen und dann die zu überwachenden Ressourcen auswählen.

> [!Note]
> Hinweis: Nur Metriken mit der gleichen Maßeinheit können gleichzeitig im Diagramm angezeigt werden. Wenn Sie also beispielsweise die Option für den eDTU-Prozentsatz auswählen, können nur andere prozentbasierte Metriken ausgewählt werden.
>

[Klicken Sie auf „Bearbeiten“](./media/sql-database-elastic-pool-manage-portal/edit-chart.png)

### <a name="manage-and-monitor-databases-in-an-elastic-pool"></a>Verwalten und Überwachen von Datenbanken in einem Pool für elastische Datenbanken

Einzelne Datenbanken können auch auf potenzielle Probleme überwacht werden. Unter **Überwachung der elastischen Datenbank**befindet sich ein Diagramm mit Metriken für fünf Datenbanken. Standardmäßig zeigt das Diagramm die fünf wichtigsten Datenbanken des Pools (gemessen an der durchschnittlichen eDTU-Verwendung innerhalb der letzten Stunde). 

![Überwachung des elastischen Pools](./media/sql-database-elastic-pool-manage-portal/basic-3.png)

Klicken Sie unter **Überwachung der elastischen Datenbank** auf **eDTU-Nutzung von Datenbanken während der letzten Stunde**. Daraufhin wird die Ansicht **Ressourcenverwendung der Datenbank** mit Details zur Datenbanknutzung im Pool angezeigt. Über das Raster im unteren Teil des Blatts können Sie bis zu fünf beliebige Datenbanken im Pool auswählen und deren Verwendung im Diagramm anzeigen. Sie können auch auf **Diagramm bearbeiten** klicken, um die Metriken und das Zeitfenster für das Diagramm anzupassen.

![Blatt mit der Ressourcenverwendung der Datenbank](./media/sql-database-elastic-pool-manage-portal/db-utilization.png)

### <a name="to-customize-the-view"></a>So passen Sie die Ansicht an

Sie können das Diagramm bearbeiten und einen Zeitbereich („Letzte Stunde“ oder „Letzte 24 Stunden“) auswählen oder auf **Benutzerdefiniert** klicken, um einen anderen Tag innerhalb der letzten beiden Wochen auszuwählen.

![Klicken auf „Diagramm bearbeiten“](./media/sql-database-elastic-pool-manage-portal/db-utilization-blade.png)

![Klicken auf „Benutzerdefiniert“](./media/sql-database-elastic-pool-manage-portal/editchart-date-time.png)

Klicken Sie auf das Dropdownfeld **Vergleich von Datenbanken nach**, um eine andere Metrik auszuwählen.

![Bearbeiten des Diagramms](./media/sql-database-elastic-pool-manage-portal/edit-comparison-metric.png)

### <a name="to-select-databases-to-monitor"></a>So wählen Sie die zu überwachenden Datenbanken aus

In der Datenbankliste auf dem Blatt mit der Ressourcenverwendung der Datenbank **** können Sie nach bestimmten Datenbanken suchen, indem Sie die einzelnen Listenseiten durchgehen oder den Namen einer Datenbank eingeben. Aktivieren Sie das Kontrollkästchen, um die Datenbank auszuwählen.

![Suchen nach zu überwachenden Datenbanken](./media/sql-database-elastic-pool-manage-portal/select-dbs.png)


### <a name="add-an-alert-to-an-elastic-pool-resource"></a>Hinzufügen einer Warnung zu einer Poolressource für elastische Datenbanken

Sie können einem Pool für elastische Datenbanken Regeln hinzufügen, um E-Mails an Personen oder Warnzeichenfolgen an URL-Endpunkte zu senden, wenn der Pool für elastische Datenbanken einen bestimmten Verwendungsschwellenwert erreicht.

**So fügen Sie eine Warnung zu einer Ressource hinzu:**

1. Klicken Sie auf das Diagramm **Ressourcennutzung**, um das Blatt **Metrik** zu öffnen. Klicken Sie auf **Warnung hinzufügen**, und füllen Sie dann die Informationen auf dem Blatt **Warnungsregel hinzufügen** aus (**Ressource** wird automatisch auf den Pool festgelegt, mit dem Sie arbeiten).
2. Geben Sie einen aussagekräftigen **Namen** und eine aussagekräftige **Beschreibung** für die Warnung ein.
3. Wählen Sie in der Liste eine **Metrik** aus, für die die Warnung ausgegeben werden soll.

    Das Diagramm zeigt dynamisch die Ressourcenverwendung für diese Metrik an, um Ihnen die Festlegung eines Schwellenwerts zu erleichtern.

4. Wählen Sie eine **Bedingung** (größer als, kleiner als usw.) und einen **Schwellenwert** aus.
5. Wählen Sie einen **Zeitraum** für die Metrikregel aus, der erfüllt sein muss, ehe die Warnung ausgelöst wird.
6. Klicken Sie auf **OK**.

Weitere Informationen finden Sie unter [Erstellen von SQL-Datenbankwarnungen im Azure-Portal](sql-database-insights-alerts-portal.md).

### <a name="move-a-database-into-an-elastic-pool"></a>Verschieben einer Datenbank in einen elastischen Pool

Sie können Datenbanken in einem vorhandenen Pool hinzufügen oder entfernen. Datenbanken können sich in verschiedenen Pools befinden. Sie können jedoch nur Datenbanken hinzufügen, die sich auf demselben logischen Server befinden.

 ![Klicken auf „Pool konfigurieren“](./media/sql-database-elastic-pool-manage-portal/configure-pool.png)

![Klicken auf „Zu Pool hinzufügen“](./media/sql-database-elastic-pool-manage-portal/add-to-pool.png)

![Auswählen hinzuzufügender Datenbanken](./media/sql-database-elastic-pool-manage-portal/add-databases-pool.png)

![Ausstehende Hinzufügungen zum Pool](./media/sql-database-elastic-pool-manage-portal/pending-additions.png)

![Klicken Sie auf Speichern.](./media/sql-database-elastic-pool-manage-portal/click-save.png)

### <a name="move-a-database-out-of-an-elastic-pool"></a>Verschieben einer Datenbank aus einem elastischen Pool

![Auflisten von Datenbanken](./media/sql-database-elastic-pool-manage-portal/select-pools-removal.png)

![Auflisten von Datenbanken](./media/sql-database-elastic-pool-manage-portal/click-remove.png)

![Vorschau-Datenbank hinzufügen und entfernen](./media/sql-database-elastic-pool-manage-portal/pending-removal.png)

![Klicken Sie auf Speichern.](./media/sql-database-elastic-pool-manage-portal/click-save.png)

### <a name="change-performance-settings-of-an-elastic-pool"></a>Ändern der Leistungseinstellungen eines Pools für elastische Datenbanken

Im Zuge der Überwachung der Ressourcenverwendung eines Pools für elastische Datenbanken stellen Sie unter Umständen fest, dass gewisse Anpassungen erforderlich sind. Möglicherweise muss die Leistung oder Speicherbegrenzung des Pools geändert werden. Gegebenenfalls möchten Sie auch die Datenbankeinstellungen im Pool ändern. Sie können das Setup des Pools jederzeit ändern, um die beste Leistungsverteilung und Kosteneffizienz zu erzielen. Weitere Informationen finden Sie unter [Wann sollte ein elastischer Pool verwendet werden?](sql-database-elastic-pool.md).

So ändern Sie die eDTUs oder Speicherbegrenzungen pro Pool und die eDTUs pro Datenbank:

![Ressourcenverwendung eines elastischen Pools](./media/sql-database-elastic-pool-manage-portal/resize-pool.png)

![Aktualisieren eines Pools für elastische Datenbanken und neue monatliche Kosten](./media/sql-database-elastic-pool-manage-portal/pool-change-edtu.png)

## <a name="manage-sql-database-elastic-pools-using-powershell"></a>Verwalten von Pools für elastische SQL-Datenbanken mit PowerShell

Verwenden Sie zum Erstellen und Verwalten von elastischen Pools für SQL-Datenbanken mithilfe von Azure PowerShell die folgenden PowerShell-Cmdlets. Wenn Sie PowerShell installieren oder upgraden müssen, helfen Ihnen die Informationen unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) weiter. Informationen zum Erstellen und Verwalten von Datenbanken, Servern und Firewallregeln finden Sie unter [Erstellen und Verwalten von Azure SQL-Datenbankservern und -datenbanken mit PowerShell](sql-database-servers-databases.md#manage-azure-sql-servers-databases-and-firewalls-using-powershell). 

> [!TIP]
> PowerShell-Beispielskripts finden Sie unter [Erstellen von Pools für elastische Datenbanken und Verschieben von Datenbanken zwischen Pools und aus einem Pool heraus mit PowerShell](scripts/sql-database-move-database-between-pools-powershell.md) und [Verwenden von PowerShell zum Überwachen und Skalieren eines Pools für elastische SQL-Datenbanken in der Azure SQL-Datenbank](scripts/sql-database-monitor-and-scale-pool-powershell.md).
>

| Cmdlet | Beschreibung |
| --- | --- |
|[New-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool)|Erstellt einen Pool für elastische Datenbanken für eine logische SQL Server-Instanz.|
|[Get-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/get-azurermsqlelasticpool)|Ruft elastische Pools und ihre Eigenschaftswerte für eine logische SQL Server-Instanz ab.|
|[Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool)|Ändert Eigenschaften eines Pools für elastische Datenbanken für eine logische SQL Server-Instanz.|
|[Remove-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/remove-azurermsqlelasticpool)|Löscht einen Pool für elastische Datenbanken aus einer logische SQL Server-Instanz.|
|[Get-AzureRmSqlElasticPoolActivity](/powershell/module/azurerm.sql/get-azurermsqlelasticpoolactivity)|Ruft den Status von Vorgängen für einen elastischen Pool für eine logische SQL Server-Instanz ab.|
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Erstellt eine neue Datenbank in einem vorhandenen Pool oder als einzelne Datenbank. |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Ruft mindestens eine Datenbank ab.|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Legt Eigenschaften für eine Datenbank fest oder verschiebt eine vorhandene Datenbank in, aus oder zwischen Pools für elastische Datenbanken.|
|[Remove-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|Entfernt eine Datenbank.|

> [!TIP]
> Das Erstellen zahlreicher Datenbanken in einem Pool für elastische Datenbanken kann eine Weile dauern, wenn dies über das Portal oder über PowerShell-Cmdlets erfolgt, die jeweils nur eine Einzeldatenbank erstellen. Informationen zum Automatisieren der Erstellung in einem Pool für elastische Datenbanken finden Sie unter [CreateOrUpdateElasticPoolAndPopulate](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).
>

## <a name="manage-sql-database-elastic-pools-using-the-azure-cli"></a>Verwalten von Pools für elastische SQL-Datenbanken mit der Azure CLI

Verwenden Sie zum Erstellen und Verwalten von Pools für elastische SQL-Datenbanken mithilfe der [Azure CLI](/cli/azure/overview) die folgenden [Azure CLI-SQL-Datenbank](/cli/azure/sql/db)-Befehle. Führen Sie die CLI mithilfe von [Cloud Shell](/azure/cloud-shell/overview) in Ihrem Browser aus, oder [installieren](/cli/azure/install-azure-cli) Sie sie unter macOS, Linux oder Windows. 

> [!TIP]
> Azure CLI-Beispielskripts finden Sie unter [Verwenden der Azure CLI zum Verschieben einer Azure SQL-Datenbank in einen Pool für elastische SQL-Datenbanken ](scripts/sql-database-move-database-between-pools-cli.md) und [Verwenden der Azure CLI zum Skalieren eines Pools für elastische SQL-Datenbanken in Azure SQL-Datenbank](scripts/sql-database-scale-pool-cli.md).
>

| Cmdlet | Beschreibung |
| --- | --- |
|[az sql elastic-pool create](/cli/azure/sql/elastic-pool#create)|Erstellt einen elastischen Pool.|
|[az sql elastic-pool list](/cli/azure/sql/elastic-pool#list)|Gibt eine Liste elastischer Pools auf einem Server zurück.|
|[az sql elastic-pool list-dbs](/cli/azure/sql/elastic-pool#list-dbs)|Gibt eine Liste der Datenbanken in einem elastischen Pool zurück.|
|[az sql elastic-pool list-editions](/cli/azure/sql/elastic-pool#list-editions)|Umfasst auch verfügbare DTU-Einstellungen, Speicherlimits und datenbankbezogene Einstellungen für den Pool. Zum Verringern der Ausführlichkeit sind zusätzliche Speicherlimits und datenbankbezogene Einstellungen standardmäßig ausgeblendet.|
|[az sql elastic-pool update](/cli/azure/sql/elastic-pool#update)|Aktualisiert einen elastischen Pool.|
|[az sql elastic-pool delete](/cli/azure/sql/elastic-pool#delete)|Löscht den elastischen Pool.|

## <a name="manage-sql-database-elastic-pools-using-transact-sql"></a>Verwalten von Pools für elastische SQL-Datenbanken mit Transact-SQL

Verwenden Sie die folgenden T-SQL-Befehle zum Erstellen und Verschieben von Datenbanken in vorhandenen elastischen Pools oder zum Zurückgeben von Informationen zu einem elastischen Pool von SQL-Datenbanken mit Transact-SQL. Sie können diese Befehle mit dem Azure-Portal, [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs) oder einem beliebigen anderen Programm ausführen, mit dem eine Verbindung mit einem Azure SQL-Datenbankserver hergestellt und Transact-SQL-Befehle übergeben werden können. Informationen zum Erstellen und Verwalten von Datenbanken, Servern und Firewallregeln finden Sie unter [Erstellen und Verwalten von Azure SQL-Datenbankservern und -datenbanken mit Transact-SQL](sql-database-servers-databases.md#manage-azure-sql-servers-databases-and-firewalls-using-transact-sql).

> [!IMPORTANT]
> Mit Transact-SQL können Sie einen Pool für elastische Azure SQL-Datenbanken nicht erstellen, aktualisieren oder löschen. Sie können Datenbanken einem elastischen Pool hinzufügen oder daraus entfernen und mithilfe von DMVs Informationen zu vorhandenen elastischen Pools zurückgeben.
>

| Befehl | Beschreibung |
| --- | --- |
|[CREATE DATABASE (Azure SQL-Datenbank)](/sql/t-sql/statements/create-database-azure-sql-database)|Erstellt eine neue Datenbank in einem vorhandenen Pool oder als einzelne Datenbank. Sie müssen über eine Verbindung mit der Masterdatenbank verfügen, um eine neue Datenbank erstellen zu können.|
| [ALTER DATABASE (Azure SQL-Datenbank)](/sql/t-sql/statements/alter-database-azure-sql-database) |Dient zum Verschieben einer Datenbank in, aus oder zwischen elastischen Pools.|
|[DROP DATABASE (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Löscht eine Datenbank.|
|[sys.elastic_pool_resource_stats (Azure SQL-Datenbank)](/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database)|Gibt Ressourcenverwendungsstatistiken für alle Pools für elastische Datenbanken in einem logischen Server zurück. Für jeden Pool für elastische Datenbanken gibt es für jedes 15-sekündige Berichtszeitfenster eine Zeile (vier Zeilen pro Minute). Dies umfasst CPU-, E/A-, Protokoll-, Speicher- und gleichzeitige Anforderungs-/Sitzungsauslastung durch alle Datenbanken im Pool.|
|[sys.database_service_objectives (Azure SQL-Datenbank)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Gibt die Edition (Dienstebene), das Dienstziel (Tarif) und den Namen des Pools für elastische Datenbanken, falls vorhanden, für eine Azure SQL-Datenbank oder ein Azure SQL Data Warehouse zurück. Wenn eine Anmeldung an der Masterdatenbank in einem Azure SQL-Datenbankserver besteht, werden Informationen zu allen Datenbanken zurückgegeben. Für Azure SQL Data Warehouse müssen Sie über eine Verbindung mit der Masterdatenbank verfügen.|

## <a name="manage-sql-database-elastic-pools-using-the-rest-api"></a>Verwalten von Pools für elastische SQL-Datenbanken mit der REST-API

Informationen zum Erstellen und Verwalten von Pools für elastische SQL-Datenbanken mithilfe der REST-API finden Sie unter [Azure SQL Database REST API (Azure SQL-Datenbank – REST-API)](/rest/api/sql/).

## <a name="next-steps"></a>Nächste Schritte

* Ein Video finden Sie unter [Microsoft Virtual Academy video course on Azure SQL Database elastic capabilities](https://mva.microsoft.com/training-courses/elastic-database-capabilities-with-azure-sql-db-16554) (Microsoft Virtual Academy-Videokurs zu Funktionen elastischer Datenbanken in Azure SQL-Datenbank).
* Weitere Informationen zu Entwurfsmustern für SaaS-Anwendungen, für die elastische Pools verwendet werden, finden Sie unter [Entwurfsmuster für SaaS-Anwendungen mit mehreren Mandanten und Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md).
* Ein SaaS-Tutorial, in dem elastische Pools verwendet werden, finden Sie in der [Einführung in die SaaS-Anwendung Wingtip](sql-database-wtp-overview.md).

