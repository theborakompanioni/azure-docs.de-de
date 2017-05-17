---
title: "Entwerfen von Cloudlösungen für die Notfallwiederherstellung mithilfe der SQL-Datenbank-Georeplikation | Microsoft Docs"
description: "Erfahren Sie, wie Sie durch Wahl des richtigen Failovermusters Ihre Cloud-Lösung für die Notfallwiederherstellung entwerfen können."
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: 2db99057-0c79-4fb0-a7f1-d1c057ec787f
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/07/2017
ms.author: sashan;carlrab
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 4abcfa777c08cec25770dc92f38e530f1ddb1d89
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="disaster-recovery-strategies-for-applications-using-sql-database-elastic-pools"></a>Strategien für die Notfallwiederherstellung für Anwendungen mit elastischem SQL-Datenbankpool
Im Laufe der Jahre haben wir gelernt, dass Clouddienste nicht narrensicher sind und dass es zu schwerwiegenden Vorfällen kommen kann. SQL-Datenbank verfügt über eine Reihe von Funktionen, mit denen für die geschäftliche Kontinuität Ihrer Anwendung gesorgt werden kann, wenn Vorfälle dieser Art auftreten. [Elastische Pools](sql-database-elastic-pool.md) und einzelne Datenbanken unterstützen die gleichen Funktionen für die Notfallwiederherstellung. In diesem Artikel werden mehrere Notfallwiederherstellungsstrategien für elastische Pools beschrieben, bei denen diese SQL-Datenbankfunktionen zur Sicherstellung der geschäftlichen Kontinuität verwendet werden.

Im Rahmen dieses Artikels verwenden wir das kanonische SaaS-ISV-Anwendungsmuster:

<i>Bei einer modernen cloudbasierten Webanwendung wird eine SQL-Datenbank für jeden Endbenutzer bereitgestellt. Der ISV verfügt über eine große Zahl von Kunden und setzt aus diesem Grund viele Datenbanken ein, die als Mandantendatenbanken bezeichnet werden. Da die Mandantendatenbanken in der Regel unvorhersagbare Aktivitätsmuster aufweisen, nutzt der ISV einen elastischen Pool, um die Datenbankkosten für längere Zeiträume eindeutig vorhersagbar zu machen. Mit dem elastischen Pool wird außerdem die Leistungsverwaltung bei Spitzen der Benutzeraktivität vereinfacht. Zusätzlich zu den Mandantendatenbanken werden für die Anwendung auch mehrere Datenbanken genutzt, um Benutzerprofile und die Sicherheit zu verwalten, Verwendungsmuster zu erfassen usw. Die Verfügbarkeit der einzelnen Mandanten wirkt sich nicht auf die Gesamtverfügbarkeit der Anwendung aus. Die Verfügbarkeit und Leistung von Verwaltungsdatenbanken ist für die Funktion der Anwendung von entscheidender Bedeutung, und wenn die Verwaltungsdatenbanken offline sind, ist auch die gesamte Anwendung offline.</i>  

Im weiteren Verlauf des Artikels werden Strategien für die Notfallwiederherstellung anhand mehrerer Szenarios besprochen, angefangen bei kostenbewussten Anwendungen bis hin zu Anwendungen mit strengen Verfügbarkeitsanforderungen.  

## <a name="scenario-1-cost-sensitive-startup"></a>Szenario 1: Kostenbewusste Anwendung für Startup-Unternehmen
<i>Wir sind ein Startup-Unternehmen, bei dem stark auf die Kosten geachtet wird.  Wir möchten die Bereitstellung und Verwaltung der Anwendung vereinfachen und einen eingeschränkten Servicelevel (SLA) für einzelne Kunden verwenden. Es soll aber sichergestellt sein, dass die Anwendung als Ganzes niemals offline ist.</i>

Zur Erfüllung der Anforderung zur Vereinfachung sollten Sie alle Mandantendatenbanken in einem elastischen Pool in der Azure-Region Ihrer Wahl bereitstellen und die Verwaltungsdatenbank(en) als georeplizierte einzelne Datenbank(en) bereitstellen. Verwenden Sie für die Notfallwiederherstellung von Mandanten die Geowiederherstellung, für die keine zusätzlichen Kosten anfallen. Um die Verfügbarkeit der Verwaltungsdatenbanken sicherzustellen, sollten sie mithilfe einer Failovergruppe in eine andere Region georepliziert werden (Schritt 1). Die laufenden Kosten der Notfallwiederherstellungskonfiguration in diesem Szenario entsprechen den Gesamtkosten der sekundären Datenbanken. Diese Konfiguration ist im nächsten Diagramm dargestellt.

![Abbildung 1](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-1.png)

Bei einem Ausfall in der primären Region können Sie die Wiederherstellungsschritte ausführen, die im nächsten Diagramm angegeben sind, um die Anwendung wieder in den Onlinezustand zu versetzen.

* Die Failovergruppe initiiert das automatische Failover der Verwaltungsdatenbank in die DR-Region. Die Anwendung stellt die Verbindung mit der neuen primären Datenbank und allen neuen Konten automatisch wieder her, und in der DR-Region werden Mandantendatenbanken erstellt. Für die vorhandenen Kunden sind die Daten vorübergehend nicht verfügbar.
* Erstellen Sie den elastischen Pool mit der gleichen Konfiguration wie für den ursprünglichen Pool (2).
* Verwenden Sie die Geowiederherstellung, um Kopien der Mandantendatenbanken zu erstellen (3). Sie können erwägen, die einzelnen Wiederherstellungen über die Endbenutzerverbindungen auszulösen, oder Sie können ein anderes anwendungsspezifisches Prioritätsschema verwenden.


An diesem Punkt befindet sich die Anwendung in der Region für die Notfallwiederherstellung wieder im Onlinezustand, aber bei einigen Kunden kommt es beim Zugreifen auf ihre Daten zu Verzögerungen.

![Abbildung 2](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-2.png)

Wenn der Ausfall temporärer Art ist, ist es möglich, dass die primäre Region von Azure wiederhergestellt wird, bevor in der Region für die Notfallwiederherstellung alle Wiederherstellungen abgeschlossen sind. In diesem Fall sollten Sie orchestrieren, dass die Anwendung zurück in die primäre Region verschoben wird. Während des Vorgangs werden die Schritte ausgeführt, die im nächsten Diagramm dargestellt sind.

* Brechen Sie alle ausstehenden Anforderungen zur Geowiederherstellung ab.   
* Führen Sie für die Verwaltungsdatenbanken ein Failover in die primäre Region durch (5). Hinweis: Nach der Wiederherstellung der Region sind aus den alten primären Replikaten automatisch sekundäre Replikate geworden. Nun werden die Rollen wieder gewechselt. 
* Ändern Sie die Verbindungszeichenfolge der Anwendung so, dass sie wieder auf die primäre Region verweist. Alle neuen Konten und Mandantendatenbanken werden jetzt in der primären Region erstellt. Für einige vorhandene Kunden sind die Daten vorübergehend nicht verfügbar.   
* Legen Sie alle Datenbanken im Pool für die Notfallwiederherstellung auf „Schreibgeschützt“ fest, um sicherzustellen, dass sie in der Region für die Notfallwiederherstellung nicht geändert werden können (6). 
* Benennen Sie für jede Datenbank im Pool für die Notfallwiederherstellung, die sich seit der Wiederherstellung geändert hat, die entsprechenden Datenbanken im primären Pool um, oder löschen Sie sie (7). 
* Kopieren Sie die aktualisierten Datenbanken aus dem Pool für die Notfallwiederherstellung in den primären Pool (8). 
* Löschen Sie den Pool für die Notfallwiederherstellung (9).

An diesem Punkt ist die Anwendung in der primären Region online, und alle Mandantendatenbanken sind im primären Pool verfügbar.

![Abbildung 3](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-3.png)

Der **Hauptvorteil** dieser Strategie sind die geringen laufenden Kosten für die Datenebenenredundanz. Sicherungen werden automatisch über den SQL-Datenbankdienst ohne Umschreiben der Anwendung und zusätzliche Kosten durchgeführt.  Kosten fallen nur an, wenn die elastischen Datenbanken wiederhergestellt werden. Der **Nachteil** besteht darin, dass die vollständige Wiederherstellung aller Mandantendatenbanken sehr lange dauert. Die Dauer hängt von der Anzahl von Wiederherstellungen ab, die Sie in der Region für die Notfallwiederherstellung initiieren, sowie von der Gesamtgröße der Mandantendatenbanken. Auch wenn Sie den Wiederherstellungen einiger Mandanten Vorrang einräumen, entsteht ein Wettbewerb mit allen anderen Wiederherstellungen, die in derselben Region initiiert werden. Der Dienst führt Vermittlungen und Drosselungen durch, um die allgemeinen Auswirkungen auf die Datenbanken der vorhandenen Kunden gering zu halten. Außerdem kann die Wiederherstellung der Mandantendatenbanken erst beginnen, nachdem der neue elastische Pool in der Region für die Notfallwiederherstellung erstellt wurde.

## <a name="scenario-2-mature-application-with-tiered-service"></a>Szenario 2: Ausgereifte Anwendung mit Diensten auf mehreren Ebenen
<i>Wir verwenden eine ausgereifte SaaS-Anwendung mit Dienstangeboten auf mehreren Ebenen und unterschiedlichen SLAs für Testkunden und zahlende Kunden. Für die Testkunden sollen die Kosten so weit wie möglich reduziert werden. Für Testkunden kann es zu Ausfallzeiten kommen, aber die Wahrscheinlichkeit dafür soll verringert werden. In Bezug auf die zahlenden Kunden ist jede Ausfallzeit mit einem Abwanderungsrisiko verbunden. Daher soll sichergestellt sein, dass zahlende Kunden immer auf ihre Daten zugreifen können.</i> 

Zur Unterstützung dieses Szenarios sollten Sie die Testmandanten von den zahlenden Mandanten trennen, indem Sie sie in separaten elastischen Pools anordnen. Für die Testkunden gilt dann ein niedrigerer eDTU-Wert pro Mandant und ein SLA mit einer längeren Wiederherstellungszeit. Die zahlenden Kunden sind in einem Pool mit einem höheren eDTU-Wert pro Mandant und einem höheren SLA angeordnet. Um die kürzeste Wiederherstellungsdauer zu gewährleisten, sollten die Mandantendatenbanken der zahlenden Kunden georepliziert werden. Diese Konfiguration ist im nächsten Diagramm dargestellt. 

![Abbildung 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-4.png)

Wie beim ersten Szenario auch, sind die Verwaltungsdatenbanken ziemlich aktiv, sodass Sie dafür eine einzelne georeplizierte Datenbank verwenden sollten (1). So wird die vorhersagbare Leistung für neue Kundenabonnements, Profilupdates und andere Verwaltungsvorgänge sichergestellt. Die Region, in der sich die primären Replikate der Verwaltungsdatenbanken befinden, ist die primäre Region, und die Region, in der sich die sekundären Replikate der Verwaltungsdatenbanken befinden, ist die Region für die Notfallwiederherstellung.

Die Mandantendatenbanken der zahlenden Kunden verfügen über aktive Datenbanken im „bezahlten“ Pool, der in der primären Region bereitgestellt wird. Sie sollten in der Region für die Notfallwiederherstellung einen sekundären Pool gleichen Namens bereitstellen. Jeder Mandant wird dann in den sekundären Pool georepliziert (2). Dies ermöglicht eine schnelle Wiederherstellung aller Mandantendatenbanken per Failover. 

Bei einem Ausfall in der primären Region können Sie die Wiederherstellungsschritte ausführen, die im nächsten Diagramm angegeben sind, um die Anwendung wieder in den Onlinezustand zu versetzen:

![Abbildung 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-5.png)

* Führen Sie ein sofortiges Failover der Verwaltungsdatenbanken in die Region für die Notfallwiederherstellung durch (3).
* Ändern Sie die Verbindungszeichenfolge der Anwendung so, dass sie auf die Region für die Notfallwiederherstellung verweist. Alle neuen Konten und Mandantendatenbanken werden jetzt in der Region für die Notfallwiederherstellung erstellt. Für die vorhandenen Testkunden sind die Daten vorübergehend nicht verfügbar.
* Führen Sie für die Datenbanken der bezahlten Mandanten ein Failover in den Pool in der Region für die Notfallwiederherstellung durch, um die Verfügbarkeit sofort wiederherzustellen (4). Da es sich beim Failover um eine schnelle Änderung der Metadatenebene handelt, können Sie eine Optimierung erwägen, bei der die einzelnen Failover bei Bedarf von den Endbenutzerverbindungen ausgelöst werden. 
* Falls die eDTU-Größe des sekundären Pools niedriger als die Größe des primären Pools war, weil für die sekundären Datenbanken die Kapazität zum Verarbeiten der Änderungsprotokolle nur erforderlich war, als es sich um sekundäre Replikate gehandelt hat, sollten Sie die Poolkapazität jetzt sofort erhöhen. Dies ist nötig, um die gesamte Workload aller Mandanten abdecken zu können (5). 
* Erstellen Sie den neuen elastischen Pool mit dem gleichen Namen und der gleichen Konfiguration in der Notfallwiederherstellungsregion für die Datenbanken der Testkunden (6). 
* Nachdem der Pool für die Testkunden erstellt wurde, verwenden Sie die Geowiederherstellung, um die einzelnen Testmandanten-Datenbanken im neuen Pool wiederherzustellen (7). Sie können erwägen, die einzelnen Wiederherstellungen über die Endbenutzerverbindungen auszulösen, oder Sie können ein anderes anwendungsspezifisches Prioritätsschema verwenden.

An diesem Punkt ist Ihre Anwendung in der Region für die Notfallwiederherstellung wieder online. Alle zahlenden Kunden haben Zugriff auf ihre Daten, während es für Testkunden beim Zugreifen auf die Daten zu Verzögerungen kommt.

Wenn die primäre Region von Azure wiederhergestellt wurde, *nachdem* Sie die Anwendung in der Region für die Notfallwiederherstellung wiederhergestellt haben, können Sie die Ausführung der Anwendung in dieser Region fortsetzen, oder Sie können ein Failback zurück zur primären Region durchführen. Falls die primäre Region wiederhergestellt wird, *bevor* der Failoverprozess abgeschlossen ist, sollten Sie erwägen, sofort ein Failback durchzuführen. Beim Failback werden die Schritte ausgeführt, die im nächsten Diagramm dargestellt sind: 

![Abbildung 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-6.png)

* Brechen Sie alle ausstehenden Anforderungen zur Geowiederherstellung ab.   
* Führen Sie das Failover für die Verwaltungsdatenbanken durch (8). Nach der Wiederherstellung der Region werden die alten primären Replikate automatisch zu den sekundären Replikaten. Nun werden sie wieder zu primären Replikaten.  
* Führen Sie das Failover der bezahlten Mandantendatenbanken durch (9). Entsprechend werden die alten primären Replikate nach der Wiederherstellung der Region automatisch zu den sekundären Replikaten. Nun werden sie wieder zu primären Replikaten. 
* Legen Sie die wiederhergestellten Testdatenbanken, die sich in der Region für die Notfallwiederherstellung geändert haben, auf „Schreibgeschützt“ fest (10).
* Benennen Sie für jede Datenbank im Notfallwiederherstellungspool für die Testkunden, die sich seit der Wiederherstellung geändert hat, die entsprechende Datenbank im primären Pool für Testkunden um, oder löschen Sie sie (11). 
* Kopieren Sie die aktualisierten Datenbanken aus dem Pool für die Notfallwiederherstellung in den primären Pool (12). 
* Löschen Sie den Pool für die Notfallwiederherstellung (13). 

> [!NOTE]
> Der Failovervorgang ist asynchron. Um die Wiederherstellungszeit zu minimieren, ist es wichtig, dass Sie den Failoverbefehl der Mandantendatenbanken für Batches von mindestens 20 Datenbanken ausführen. 
> 
> 

Der **Hauptvorteil** dieser Strategie besteht darin, dass für die zahlenden Kunden der höchste Servicelevel (SLA) erzielt wird. Außerdem ist sichergestellt, dass die Blockierung der neuen Testzugriffe sofort aufgehoben wird, sobald der Notfallwiederherstellungspool für Testkunden erstellt wurde. Der **Nachteil** ist, dass sich bei dieser Einrichtung die Gesamtkosten der Mandantendatenbanken um die Kosten für den sekundären Notfallwiederherstellungspool für zahlende Kunden erhöhen. Wenn der sekundäre Pool eine andere Größe hat, verringert sich für zahlende Kunden außerdem die Leistung nach einem Failover, bis das Poolupgrade in der Region für die Notfallwiederherstellung abgeschlossen ist. 

## <a name="scenario-3-geographically-distributed-application-with-tiered-service"></a>Szenario 3: Geografisch verteilte Anwendung mit Diensten auf mehreren Ebenen
<i>Wir verwenden eine ausgereifte SaaS-Anwendung mit Dienstangeboten auf mehreren Ebenen. Wir möchten zahlenden Kunden einen sehr aggressiven Servicelevel (SLA) anbieten und die Auswirkungen bei Ausfällen gering halten, da auch kurze Unterbrechungen bei Kunden zu Unzufriedenheit führen können. Es ist sehr wichtig, dass die zahlenden Kunden immer Zugriff auf ihre Daten haben. Die Testversionen sind kostenlos, und während des Testzeitraums wird keine SLA angeboten.</i> 

Für dieses Szenario sollten Sie drei separate elastische Pools verwenden. Zwei Pools gleicher Größe mit hohen eDTUs pro Datenbank sollten in zwei unterschiedlichen Regionen bereitgestellt werden, um die Mandantendatenbanken der zahlenden Kunden aufnehmen zu können. Der dritte Pool mit den Testmandanten verfügt hierbei über niedrigere eDTUs pro Datenbank und wird in einer der beiden Regionen bereitgestellt.

Um bei Ausfällen die kürzesten Wiederherstellungsdauern garantieren zu können, sollten die Mandantendatenbanken der zahlenden Kunden mit 50% der primären Datenbanken in beiden Regionen georepliziert werden. Außerdem verfügt jede Region über 50% der sekundären Datenbanken. Wenn eine Region offline ist, wären hierbei also nur 50% der Datenbanken zahlender Kunden betroffen und müssten einem Failover unterzogen werden. Die restlichen Datenbanken würden intakt bleiben. Diese Konfiguration ist im folgenden Diagramm dargestellt:

![Abbildung 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-7.png)

Wie in den vorherigen Szenarien auch, sind die Verwaltungsdatenbanken sehr aktiv, sodass Sie sie als einzelne georeplizierte Datenbanken konfigurieren sollten (1). Auf diese Weise wird die vorhersagbare Leistung der neuen Kundenabonnements, Profilupdates und anderen Verwaltungsvorgänge sichergestellt. Region A ist die primäre Region für die Verwaltungsdatenbanken, und Region B wird für die Wiederherstellung der Verwaltungsdatenbanken verwendet.

Die Mandantendatenbanken der zahlenden Kunden werden auch georepliziert, aber die primären und sekundären Replikate werden auf Region A und Region B aufgeteilt (2). So kann für die primären Mandantendatenbanken, die vom Ausfall betroffen sind, ein Failover in die andere Region durchgeführt werden, damit sie verfügbar sind. Die andere Hälfte der Mandantendatenbanken ist davon nicht betroffen. 

Im nächsten Diagramm sind die Wiederherstellungsschritte dargestellt, die ausgeführt werden müssen, wenn es in Region A zu einem Ausfall kommt.

![Abbildung 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-8.png)

* Führen Sie für die Verwaltungsdatenbanken sofort ein Failover in Region B durch (3).
* Ändern Sie die Verbindungszeichenfolge der Anwendung so, dass sie auf die Verwaltungsdatenbanken in Region B verweist. Ändern Sie die Verwaltungsdatenbanken, um sicherzustellen, dass die neuen Konten und Mandantendatenbanken in Region B erstellt werden und die vorhandenen Mandantendatenbanken ebenfalls dort angeordnet sind. Für die vorhandenen Testkunden sind die Daten vorübergehend nicht verfügbar.
* Führen Sie für die Datenbanken der bezahlten Mandanten ein Failover in Pool 2 von Region B durch, um die Verfügbarkeit sofort wiederherzustellen (4). Da es sich beim Failover um eine schnelle Änderung der Metadatenebene handelt, können Sie eine Optimierung erwägen, bei der die einzelnen Failover bei Bedarf von den Endbenutzerverbindungen ausgelöst werden. 
* Da Pool 2 jetzt nur primäre Datenbanken enthält, erhöht sich die gesamte Workload im Pool, sodass Sie die eDTU-Größe sofort erhöhen sollten (5). 
* Erstellen Sie den neuen elastischen Pool mit dem gleichen Namen und der gleichen Konfiguration in Region B für die Datenbanken der Testkunden (6). 
* Verwenden Sie nach der Erstellung des Pools die Geowiederherstellung, um die individuelle Testmandanten-Datenbank im Pool wiederherzustellen (7). Sie können erwägen, die einzelnen Wiederherstellungen über die Endbenutzerverbindungen auszulösen, oder Sie können ein anderes anwendungsspezifisches Prioritätsschema verwenden.

> [!NOTE]
> Der Failovervorgang ist asynchron. Um die Wiederherstellungszeit zu minimieren, ist es wichtig, dass Sie den Failoverbefehl der Mandantendatenbanken für Batches von mindestens 20 Datenbanken ausführen. 
> 
> 

An diesem Punkt ist die Anwendung in Region B wieder online. Alle zahlenden Kunden haben Zugriff auf ihre Daten, während es für Testkunden beim Zugreifen auf die Daten zu Verzögerungen kommt.

Nach dem Wiederherstellen von Region A müssen Sie entscheiden, ob Sie Region B für Testkunden verwenden oder ein Failback zur Verwendung des Testkundenpools in Region A durchführen möchten. Ein Kriterium kann der Prozentsatz der Testmandanten-Datenbanken sein, die seit der Wiederherstellung geändert wurden. Unabhängig von dieser Entscheidung müssen Sie die kostenpflichtigen Mandanten zwischen zwei Pools neu verteilen. Das nächste Diagramm veranschaulicht den Prozess, bei dem für die Mandantendatenbanken für Testkunden ein Failback auf Region A ausgeführt wird.  

![Abbildung 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-9.png)

* Brechen Sie alle ausstehenden Anforderungen der Geowiederherstellung an den Notfallwiederherstellungspool für Testkunden ab.   
* Führen Sie das Failover für die Verwaltungsdatenbank durch (8). Nach der Wiederherstellung der Region werden die alten primären Replikate automatisch zu den sekundären Replikaten. Nun werden sie wieder zu primären Replikaten.  
* Wählen Sie aus, für welche Datenbanken für bezahlte Mandaten ein Failback zu Pool 1 durchgeführt werden soll, und initiieren Sie das Failover zu den sekundären Replikaten (9). Nach der Wiederherstellung der Region werden alle Datenbanken in Pool 1 automatisch zu sekundären Replikaten. Nun werden 50% davon wieder zu primären Replikaten. 
* Reduzieren Sie die Größe von Pool 2 auf den eDTU-Originalwert (10).
* Legen Sie alle wiederhergestellten Testdatenbanken in Region B auf „Schreibgeschützt“ fest (11).
* Benennen Sie für jede Datenbank im Notfallwiederherstellungspool für Testkunden, die sich seit der Wiederherstellung geändert hat, die entsprechende Datenbank im primären Testpool um, oder löschen Sie sie (12). 
* Kopieren Sie die aktualisierten Datenbanken aus dem Pool für die Notfallwiederherstellung in den primären Pool (13). 
* Löschen Sie den Pool für die Notfallwiederherstellung (14). 

Die **Hauptvorteile** dieser Strategie lauten:

* Es wird ein aggressiver Servicelevel (SLA) für die zahlenden Kunden unterstützt, da sichergestellt ist, dass bei einem Ausfall nicht mehr als 50% der Mandantendatenbanken betroffen sind. 
* Es ist sichergestellt, dass die Blockierung der neuen Testzugriffe sofort aufgehoben wird, sobald der Notfallwiederherstellungspool für Testkunden bei der Wiederherstellung erstellt wurde. 
* Ermöglicht eine effizientere Nutzung der Poolkapazität, da 50% der sekundären Datenbanken in Pool 1 und Pool 2 garantiert weniger aktiv als die primären Datenbanken sind.

Die **Hauptnachteile** lauten:

* Die CRUD-Vorgänge für die Verwaltungsdatenbanken weisen für die Endbenutzer, die mit Region A verbunden sind, eine geringere Latenz als für die Endbenutzer auf, die mit Region B verbunden sind. Der Grund ist, dass sie für das primäre Replikat der Verwaltungsdatenbanken ausgeführt werden.
* Hierfür ist ein komplexeres Design der Verwaltungsdatenbank erforderlich. Beispielsweise muss jeder Mandantendatensatz über eine Standortkennzeichnung verfügen, die beim Failover und Failback geändert werden muss.  
* Für die zahlenden Kunden kann sich die Leistung ggf. verschlechtern, bis das Poolupgrade in Region B abgeschlossen ist. 

## <a name="summary"></a>Zusammenfassung
In diesem Artikel geht es um die Notfallwiederherstellungsstrategien für die Datenbankebene, die für eine mehrinstanzenfähige SaaS-ISV-Anwendung verwendet werden. Die gewählte Strategie sollte auf den Anforderungen der Anwendung basieren, z.B. dem Geschäftsmodell, dem SLA, das Sie Ihren Kunden anbieten möchten, Budgetbeschränkungen usw. Für jede Strategie werden die Vor- und Nachteile beschrieben, damit Sie eine fundierte Entscheidung treffen können. Ihre Anwendung umfasst wahrscheinlich noch weitere Azure-Komponenten. Es ist ratsam, die damit verbundenen Hinweise zur Geschäftskontinuität zu beachten und die Wiederherstellung der Datenbankebene mit ihnen zu orchestrieren. Weitere Informationen zum Verwalten der Wiederherstellung von Datenbankanwendungen in Azure finden Sie unter [Entwerfen von Cloudlösungen für die Notfallwiederherstellung](sql-database-designing-cloud-solutions-for-disaster-recovery.md).  

## <a name="next-steps"></a>Nächste Schritte
* Informationen über automatisierte Sicherungen von Azure SQL-Datenbanken finden Sie unter [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md)
* Eine Übersicht und verschiedene Szenarien zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
* Informationen zum Verwenden automatisierter Sicherungen für die Wiederherstellung finden Sie unter [Wiederherstellen einer Datenbank aus vom Dienst initiierten Sicherungen](sql-database-recovery-using-backups.md)
* Informationen über schnellere Wiederherstellungsoptionen finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md)  
* Informationen zum Verwenden automatisierter Sicherungen für die Archivierung finden Sie unter [Datenbankkopie](sql-database-copy.md)


