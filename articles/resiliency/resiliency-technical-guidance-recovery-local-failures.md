<properties
   pageTitle="Wiederherstellung nach lokalen Ausfällen in Azure – technischer Leitfaden | Microsoft Azure"
   description="Artikel über die Grundlagen und den Entwurf von robusten, hoch verfügbaren, fehlertoleranten Anwendungen sowie die Planung der Notfallwiederherstellung nach lokalen Fehlern in Azure."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="hongfeig"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/13/2016"
   ms.author="patw;jroth;aglick"/>

#Technischer Leitfaden zur Resilienz in Azure – Wiederherstellung nach lokalen Ausfällen in Azure

Die Anwendungsverfügbarkeit wird durch zwei wesentliche Faktoren gefährdet: den Ausfall von Geräten (z.B. Laufwerken und Servern) und die Erschöpfung kritischer Ressourcen, beispielsweise von Computeressourcen unter Spitzenlastbedingungen. Um unter solchen Umständen für hohe Verfügbarkeit zu sorgen, bietet Azure eine Kombination aus Ressourcenverwaltung, Elastizität, Lastenausgleich und Partitionierung. Einige dieser Funktionen werden für alle Clouddienste automatisch ausgeführt; in einigen Fällen muss der Anwendungsentwickler jedoch einige zusätzliche Aufgaben erledigen, um von den Funktionen zu profitieren.

##Cloud Services
Alle in Azure gehosteten Clouddienste sind Sammlungen aus einer oder mehreren Web- oder Workerrollen. Ein oder mehrere Instanzen einer bestimmten Rolle können gleichzeitig ausgeführt werden. Die Anzahl der Instanzen wird durch Konfiguration bestimmt. Rolleninstanzen werden mithilfe einer Komponente überwacht und verwaltet, die als Fabric Controller (FC) bezeichnet wird. Der FC erkennt Software- und Hardwarefehler automatisch und reagiert darauf.

  * Jede Rolleninstanz wird in einem eigenen virtuellen Computer ausgeführt und kommuniziert über einen Gast-Agent (GA) mit dem FC. Der GA sammelt Ressourcen- und Knotenmetriken, einschließlich VM-Nutzung, Status, Protokollen, Ressourcennutzung, Ausnahmen und Fehlerbedingungen. Der FC fragt den GA in konfigurierbaren Intervallen ab und startet den virtuellen Computer neu, falls der GA nicht reagiert.
  * Bei einem Hardwarefehler verschiebt der zugehörige FC alle betroffenen Rolleninstanzen in einen neuen Hardwareknoten und konfiguriert das Netzwerk neu, um den Datenverkehr an diesen Knoten zu leiten.

Um von diesen Funktionen profitieren zu können, sollten Entwickler sicherstellen, dass Dienstrollen keinen Statusinformationen in den Rolleninstanzen speichern. Stattdessen sollte der Zugriff auf alle persistenten Daten über einen permanenten Speicher erfolgen, beispielsweise über Azure-Speicherdienste oder Azure SQL-Datenbank. Auf diese Weise können Anforderungen von allen Rollen verarbeitet werden. Das bedeutet auch, dass Rolleninstanzen jederzeit ausfallen können, ohne Inkonsistenzen im vorübergehenden oder permanenten Status des Diensts zu verursachen.

Die Anforderung, den Status außerhalb der Rollen zu speichern, hat verschiedene Auswirkungen. Es sollten z.B. alle zugehörigen Änderungen an einem Azure-Tabellenspeicher nach Möglichkeit in einer einzigen Entitätsgruppentransaktion ausgeführt werden. Es ist allerdings nicht immer möglich, alle Änderungen in einer einzigen Transaktion auszuführen. Es muss sorgfältig darauf geachtet werden, dass Rolleninstanzfehler keine Probleme verursachen, wenn sie langfristige Vorgänge unterbrechen, die zwei oder mehr Updates am persistenten Status des Diensts umfassen. Wenn eine andere Rolle versucht, einen solchen Vorgang erneut auszuführen, sollte sie den Fall voraussehen, dass die Arbeit nur teilweise abgeschlossen wurde, und entsprechend reagieren.

Ein Beispiel: Wenn in einem Dienst, der Daten über mehrere Speicher hinweg partitioniert, während des Verschiebens eines Shards eine Workerrolle ausfällt, wird der Shard möglicherweise nicht vollständig verschoben. Möglicherweise wird der Verschiebevorgang von einer anderen Workerrolle erneut begonnen, wodurch verwaiste oder beschädigte Daten entstehen können. Um solche Probleme zu vermeiden, müssen langfristige Vorgänge idempotent (d.h. ohne Nebeneffekt wiederholbar) sein und/oder inkrementell neu gestartet (d.h. vom letzten Ausfallpunkt aus fortgesetzt) werden können.

  * Um idempotent zu sein, sollte ein langfristiger Vorgang immer die gleiche Auswirkung haben, egal, wie oft er ausgeführt wird und ob er unterbrochen wird.
  * Für inkrementelle Neustarts sollte ein langfristiger Vorgang aus einer Reihe kleinerer, unteilbarer Vorgänge bestehen und seinen Fortschritt in einem permanenten Speicher erfassen, damit alle nachfolgenden Aufrufe dort fortfahren, wo der vorherige Vorgang angehalten wurde.

Alle langfristigen Vorgänge sollten wiederholt aufgerufen werden, bis sie erfolgreich abgeschlossen werden. Ein Bereitstellungsvorgang kann beispielsweise in einer Azure-Warteschlange platziert und von einer Workerrolle erst nach erfolgreicher Durchführung aus der Warteschlange entfernt werden. Zum Bereinigen von Daten, die durch unterbrochene Vorgänge erstellt wurden, ist möglicherweise eine Garbage Collection erforderlich.

###Elastizität
Die anfängliche Anzahl von Instanzen, die für jede Rolle ausgeführt werden, ist in der Konfiguration jeder Rolle festgelegt. Administratoren sollten zu Beginn jede Rolle so konfigurieren, dass sie je nach erwarteter Auslastung mit mindestens zwei Instanzen ausgeführt wird. Rolleninstanzen lassen sich problemlos nach oben oder unten skalieren, wenn sich Nutzungsmuster ändern. Die Skalierung kann manuell im Azure-Portal oder automatisiert über Windows PowerShell, die Dienstverwaltungs-API oder Drittanbietertools erfolgen. Informationen dazu finden Sie unter [Automatisches Skalieren eines Clouddiensts](../cloud-services/cloud-services-how-to-scale.md).

###Partitionierung
Der Fabric Controller von Azure verwendet zwei Arten von Partitionen: Updatedomänen und Fehlerdomänen.

  * Eine Updatedomäne wird zum Aktualisieren der Rolleninstanzen eines Diensts in Gruppen verwendet. Azure stellt Dienstinstanzen in mehreren Updatedomänen bereit. Für ein direktes Update fährt der FC alle Instanzen in einer Updatedomäne herunter, aktualisiert sie und startet sie neu. Anschließend fährt der FC mit der nächsten Updatedomäne fort. Dieser Ansatz verhindert, dass während des Aktualisierungsvorgangs der komplette Dienst nicht verfügbar ist.
  * Eine Fehlerdomäne definiert potenzielle Hardware- oder Netzwerkfehlerstellen. Bei Rollen mit mehr als einer Instanz stellt der FC sicher, dass die Instanzen auf mehrere Fehlerdomänen verteilt werden, um zu verhindern, dass isolierte Hardwarefehler zu Dienstunterbrechungen führen. Sämtliche Risiken hinsichtlich Server- und Clusterfehlern in Azure werden durch Fehlerdomänen abgedeckt.

Gemäß der [Azure-SLA](https://azure.microsoft.com/support/legal/sla/) garantiert Microsoft eine externe Konnektivität von mindestens 99,95 %, wenn zwei oder mehr Webrolleninstanzen in verschiedenen Fehler- und Upgradedomänen bereitgestellt werden. Im Gegensatz zu Updatedomänen gibt es keine Möglichkeit, die Anzahl von Fehlerdomänen zu steuern. Azure weist Fehlerdomänen automatisch zu und verteilt Rolleninstanzen auf diese. Mindestens die ersten beiden Instanzen jeder Rolle werden in verschiedenen Fehler- und Upgradedomänen platziert, um sicherzustellen, dass jede Rolle mit mindestens zwei Instanzen die SLA erfüllt. Dies ist im folgenden Diagramm dargestellt:

![Fehlerdomänenisolation (vereinfachte Darstellung)](./media/resiliency-technical-guidance-recovery-local-failures/partitioning-1.png "Fehlerdomänenisolation – vereinfachte Darstellung")

###Lastenausgleich
Jeglicher eingehender Datenverkehr einer Webrolle wird durch einen zustandslosen Lastenausgleich geleitet, der Clientanforderungen zwischen den Rolleninstanzen verteilt. Einzelne Rolleninstanzen besitzen keine öffentliche IP-Adressen und sind nicht direkt aus dem Internet erreichbar. Webrollen sind zustandslos, daher können alle Clientanforderungen zu jeder Rolleninstanz weitergeleitet werden. Alle 15 Sekunden wird ein [StatusCheck](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.statuscheck.aspx)-Ereignis ausgelöst. Dieses kann verwendet werden, um anzugeben, ob die Rolle für den Empfang von Datenverkehr bereit ist oder ob sie beschäftigt ist und aus der Lastenausgleichsrotation herausgenommen werden sollte.

##Virtuelle Computer
Virtuelle Azure-Computer unterscheiden sich von PaaS-Computerollen in Zusammenhang mit einer hohen Verfügbarkeit in mehreren Punkten. In einigen Fällen müssen Sie zusätzliche Aufgaben ausführen, um eine hohe Verfügbarkeit sicherzustellen.

###Dauerhaftigkeit von Datenträgern
Im Gegensatz zu PaaS-Rolleninstanzen sind in virtuellen Computern gespeicherte Daten persistent, auch wenn der virtuelle Computer verschoben wird. Virtuelle Azure-Computer verwenden VM-Datenträger, die in Azure Storage als Blobs vorhanden sind. Aufgrund der Verfügbarkeitsmerkmale von Azure Storage sind die Daten, die auf Laufwerken virtueller Computer gespeichert werden, ebenfalls hoch verfügbar. Beachten Sie, dass das Laufwerk D: eine Ausnahme von dieser Regel darstellt. Das Laufwerk D: ist eigentlich ein physischer Speicher auf dem Rackserver, der den virtuellen Computer hostet, und die Daten dieses Laufwerks gehen verloren, wenn der virtuelle Computer recycelt wird. Das Laufwerk D: ist nur für die temporäre Speicherung vorgesehen.

###Partitionierung
Azure bietet native Unterstützung für die Ebenen einer PaaS-Anwendung (Webrolle und Workerrolle) und kann diese daher ordnungsgemäß auf Fehler- und Updatedomänen verteilen. Im Gegensatz dazu müssen die Ebenen in IaaS-Anwendungen mithilfe von Verfügbarkeitsgruppen manuell definiert werden. Für eine IaaS-SLA sind Verfügbarkeitsgruppen erforderlich.

![Verfügbarkeitsgruppen für Microsoft Azure Virtual Machines](./media/resiliency-technical-guidance-recovery-local-failures/partitioning-2.png "Verfügbarkeitsgruppen für Microsoft Azure Virtual Machines")

Im obigen Diagramm sind die IIS-Ebene (Internet Information Server), die als Web-App-Ebene fungiert, und die SQL-Ebene, die als Datenebene fungiert, verschiedenen Verfügbarkeitsgruppen zugewiesen. Indem die Instanzen auf verschiedene Fehlerdomänen verteilt werden, wird sichergestellt, dass alle Instanzen jeder Ebene über Hardwareredundanz verfügen und während einer Aktualisierung nicht heruntergefahren werden.

###Lastenausgleich
Wenn der Datenverkehr auf die virtuellen Computer verteilt werden soll, müssen Sie die virtuellen Computer in einen Clouddienst gruppieren und den Lastenausgleich über einen bestimmten TCP- oder UDP-Endpunkt ausführen. Weitere Informationen finden Sie unter [Lastenausgleich zwischen virtuellen Computern](../virtual-machines/virtual-machines-linux-load-balance.md). Wenn die virtuellen Computer Eingaben aus einer anderen Quelle erhalten (beispielsweise einem Warteschlangenmechanismus), ist kein Lastenausgleich erforderlich. Der Lastenausgleich verwendet eine grundlegende Integritätsprüfung, um zu ermitteln, ob der Datenverkehr an den Knoten gesendet werden soll. Sie können auch selbst Tests erstellen, um anwendungsspezifische Integritätsmetriken zu implementieren, die bestimmen, ob der virtuelle Computer Datenverkehr empfangen soll.

##Speicher
Azure Storage ist der grundlegende permanente Datendienst für Azure und stellt Blob-, Tabellen-, Warteschlangen- und VM-Datenträgerspeicher bereit. Storage verwendet eine Kombination aus Replikation und Ressourcenverwaltung, um innerhalb eines einzelnen Rechenzentrums hohe Verfügbarkeit bereitzustellen. Die Verfügbarkeits-SLA von Azure Storage garantiert, dass ordnungsgemäß formatierte Anforderungen zum Hinzufügen, Aktualisieren, Lesen und Löschen von Daten mindestens 99,9 % der Zeit erfolgreich und ordnungsgemäß verarbeitet werden und dass Speicherkonten über eine Verbindung zum Internetgateway verfügen.

###Replikation
Die Dauerhaftigkeit von Daten für Azure Storage wird ermöglicht, indem mehrere Kopien aller Daten auf verschiedenen Laufwerken gespeichert werden, die sich auf vollständig voneinander unabhängigen physischen Speichersubsystemen innerhalb der Region befinden. Daten werden synchron repliziert, und für alle Kopien wird ein Commit ausgeführt, bevor ein Schreibvorgang bestätigt wird. Azure Storage weist eine hohe Konsistenz auf, was bedeutet, dass Lesevorgänge garantiert die neuesten Schreibvorgänge widerspiegeln. Darüber hinaus werden Kopien der Daten fortlaufend überprüft, um Datenverfall zu erkennen und zu beheben – eine häufig übersehene Bedrohung der Integrität gespeicherter Daten. Dienste profitieren bereits durch die reine Verwendung von Azure Storage von der Replikation. Dienstentwickler müssen keine zusätzlichen Aufgaben ausführen, um die Wiederherstellung nach einem lokalen Fehler einzurichten.

###Ressourcenverwaltung
Speicherkonten, die nach dem 7. Juni 2012 erstellt wurden, können auf bis zu 200 TB skaliert werden (zuvor lag die Höchstgrenze bei 100 TB). Wenn zusätzlicher Speicherplatz erforderlich ist, müssen Anwendungen so entworfen werden, dass sie mehrere Speicherkonten verwenden.

###VM-Datenträger
Ein VM-Datenträger wird als Seitenblob in Azure Storage gespeichert und erhält damit die gleichen Dauerhaftigkeits- und Skalierbarkeitseigenschaften wie Blobspeicher. Aufgrund dieses Designs sind die Daten auf einem Datenträger eines virtuellen Computers persistent, selbst wenn der Server ausfällt, auf dem der virtuelle Computer ausgeführt wird, und der virtuelle Computer auf einem anderen Server neu gestartet werden muss.

##Datenbank

###SQL-Datenbank
Microsoft Azure SQL-Datenbank bietet DaaS-Funktionen (Database as a Service). So können Anwendungen schnell bereitgestellt, Daten in Anwendungen eingefügt und relationale Datenbanken abgefragt werden. Azure SQL-Datenbank stellt viele der vertrauten SQL Server-Features und -Funktionen bereit und entbindet Sie gleichzeitig vom Aufwand für Hardware, Konfiguration, Patching und Resilienz.

>[AZURE.NOTE]Azure SQL-Datenbank bietet keine 1:1-Funktionsparität mit SQL Server und wurde für andere Anforderungen entwickelt, die nur auf Cloudanwendungen zutreffen (elastische Skalierung, Database as a Service zur Senkung von Wartungskosten usw.). Weitere Informationen finden Sie unter [Wählen Sie eine SQL Server-Cloudoption: Azure SQL-Datenbank (PaaS) oder SQL Server auf Azure-VMs (IaaS)](../sql-database/data-management-azure-sql-database-and-sql-server-iaas.md).

####Replikation
Azure SQL-Datenbank stellt integrierte Resilienz gegenüber Fehlern auf Knotenebene bereit. Alle Schreibvorgänge in eine Datenbank werden automatisch mithilfe eines Commitverfahrens per Quorum auf mindestens zwei Hintergrundknoten repliziert (der primäre und mindestens ein sekundärer Knoten müssen bestätigen, dass die Aktivität in das Transaktionsprotokoll geschrieben wird, bevor die Transaktion als erfolgreich betrachtet und zurückgegeben wird). Im Fall eines Knotenfehlers führt die Datenbank automatisch ein Failover auf eins der sekundären Replikate durch. Dies verursacht eine vorübergehende Unterbrechung der Verbindung für Clientanwendungen. Aus diesem Grund müssen alle Microsoft Azure SQL-Datenbank-Clients die Verarbeitung vorübergehender Verbindungen in irgendeiner Form implementieren. Weitere Informationen finden Sie unter [Using the Transient Fault Handling Application Block with SQL Azure](https://msdn.microsoft.com/library/hh680899.aspx) (Verwenden des Anwendungsblocks zur Verarbeitung von vorübergehenden Fehlern mit SQL Azure).

####Ressourcenverwaltung
Jede Datenbank wird bei der Erstellung mit einer Größenbeschränkung konfiguriert. Die derzeit verfügbare maximale Größe beträgt 150 GB. Wenn eine Datenbank den oberen Grenzwert erreicht, werden weitere INSERT- oder UPDATE-Befehle zurückgewiesen (das Abfragen und Löschen von Daten ist weiterhin möglich).

Innerhalb einer Datenbank verwendet Microsoft Azure SQL-Datenbank ein Fabric zur Verwaltung von Ressourcen. Anstelle eines Fabric Controllers wird jedoch eine Ringtopologie eingesetzt, um Fehler zu erkennen. Jedes Replikat in einem Cluster besitzt zwei Nachbarn und ist dafür zuständig zu erkennen, wenn diese ausfallen. Wenn ein Replikat ausfällt, lösen seine Nachbarn einen Reconfiguration Agent (RA) aus, um das Replikat auf einem anderen Computer neu zu erstellen. Mithilfe einer Moduleinschränkung wird sichergestellt, dass ein logischer Server nicht zu viele Ressourcen auf einem Computer verwendet oder die physischen Grenzwerte des Computers überschreitet.

###Elastizität
Wenn die Anwendung mehr Speicherplatz als das von der Datenbank vorgegebene Limit von 150 GB erfordert, muss ein Verfahren zur horizontalen Hochskalierung implementiert werden. Die horizontale Hochskalierung mit Microsoft Azure SQL-Datenbank erfolgt durch manuelle Partitionierung – auch als Sharding bezeichnet – von Daten über mehrere Azure SQL-Datenbank-Instanzen hinweg. Ein solcher Ansatz ermöglicht es, mithilfe der Skalierung einen nahezu linearen Kostenanstieg zu erreichen. Durch elastisches Wachstum bzw. bedarfsgesteuerte Kapazität können Systeme mit inkrementellem Kostenanstieg wachsen, da Datenbanken basierend auf der pro Tag tatsächlich durchschnittlich genutzten Größe berechnet werden, nicht basierend auf der möglichen Maximalgröße.

##SQL Server auf virtuellen Computern
Durch Installieren von SQL Server auf virtuellen Azure-Computern (Version 2014 oder höher) können Sie die herkömmlichen Verfügbarkeitsfunktionen von SQL Server nutzen, wie beispielsweise AlwaysOn-Verfügbarkeitsgruppen oder Datenbankspiegelung. Beachten Sie, dass für virtuelle Computer, Speicher und Netzwerkressourcen von Azure andere Betriebsmerkmale gelten als für eine lokale, nicht virtualisierte IT-Infrastruktur. Für eine erfolgreiche Implementierung einer SQL Server-Lösung für Hochverfügbarkeit und Notfallwiederherstellung in Azure sollten Sie diese Unterschiede kennen und Ihre Lösung daran anpassen.

###Hochverfügbarkeitsknoten in einer Verfügbarkeitsgruppe
Wenn Sie eine Hochverfügbarkeitslösung in Azure implementieren, können Sie mithilfe einer Verfügbarkeitsgruppe in Azure Hochverfügbarkeitsknoten in separaten Fehler- und Upgradedomänen platzieren. Bei einer Verfügbarkeitsgruppe handelt es sich um ein Azure-Konzept. Sie sollten diese bewährte Methode befolgen, um sicherzustellen, dass Ihre Datenbanken tatsächlich hoch verfügbar sind – unabhängig davon, ob Sie AlwaysOn-Verfügbarkeitsgruppen, Datenbankspiegelung oder andere Verfahren verwenden. Wenn Sie diese bewährte Methode nicht befolgen, könnten Sie fälschlicherweise davon ausgehen, dass Ihr System hoch verfügbar ist, in Wahrheit können Ihre Knoten jedoch alle gleichzeitig ausfallen, weil sie zufällig in der gleichen Fehlerdomäne im Azure-Rechenzentrum platziert wurden. Diese Empfehlung gilt nicht im gleichen Ausmaß für den Protokollversand, da es sich hierbei um eine Funktion der Notfallwiederherstellung handelt und Sie sicherstellen sollten, dass die Server in separaten Azure-Rechenzentrums-Standorten (Regionen) ausgeführt werden. Definitionsgemäß handelt es sich bei diesen Rechenzentrums-Standorten um separate Fehlerdomänen.

Damit virtuelle Azure-Computer derselben Verfügbarkeitsgruppe hinzugefügt werden, müssen Sie sie im gleichen Clouddienst bereitstellen. Nur Knoten in demselben Clouddienst können Mitglieder derselben Verfügbarkeitsgruppe sein. Darüber hinaus sollten sich die virtuellen Computer im gleichen VNet befinden, um sicherzustellen, dass sie auch einer Dienstreparatur ihre IPs beibehalten, und so DNS-Aktualisierungen zu vermeiden.

###Nur in Azure: Lösungen mit hoher Verfügbarkeit
Sie können eine Lösung mit hoher Verfügbarkeit für SQL Server-Datenbanken in Azure mit AlwaysOn-Verfügbarkeitsgruppen oder per Datenbankspiegelung umsetzen.

Das folgende Diagramm veranschaulicht die Architektur von AlwaysOn-Verfügbarkeitsgruppen, die in Azure Virtual Machines ausgeführt werden. Das Diagramm stammt aus dem detaillierten Artikel zu diesem Thema: [Hohe Verfügbarkeit und Notfallwiederherstellung für SQL Server auf virtuellen Azure-Computern](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

![AlwaysOn-Verfügbarkeitsgruppen in Microsoft Azure](./media/resiliency-technical-guidance-recovery-local-failures/high_availability_solutions-1.png "AlwaysOn-Verfügbarkeitsgruppen in Microsoft Azure")

Sie können eine AlwaysOn-Verfügbarkeitsgruppe auch als automatische End-to-End-Bereitstellung auf virtuellen Azure-Computern implementieren, indem Sie die AlwaysOn-Vorlage im Microsoft Azure-Portal verwenden. Weitere Informationen finden Sie unter [SQL Server AlwaysOn Offering in Microsoft Azure Portal Gallery](https://blogs.technet.microsoft.com/dataplatforminsider/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery/) (in englischer Sprache).

Das folgende Diagramm veranschaulicht die Verwendung der Datenbankspiegelung auf Azure Virtual Machines. Auch dieses Diagramm stammt aus dem detaillierten Artikel [Hohe Verfügbarkeit und Notfallwiederherstellung für SQL Server auf virtuellen Azure-Computern](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

![Datenbankspiegelung in Microsoft Azure](./media/resiliency-technical-guidance-recovery-local-failures/high_availability_solutions-2.png "Datenbankspiegelung in Microsoft Azure")

>[AZURE.NOTE]In beiden Architekturen ist ein Domänencontroller erforderlich. Bei der Datenbankspiegelung können jedoch Serverzertifikate verwendet werden, sodass kein Domänencontroller benötigt wird.

##Weitere Azure Platform-Dienste
Azure Cloud Services basieren auf Azure, profitieren also von den bereits beschriebenen Plattformfunktionen zur Wiederherstellung nach lokalen Fehlern. In einigen Fällen können Sie bestimmte Aktionen ausführen, um die Verfügbarkeit für Ihr spezielles Szenario zu erhöhen.

###Service Bus
Um die Risiken durch einen vorübergehenden Ausfall von Azure Service Bus zu minimieren, können Sie eine dauerhafte clientseitige Warteschlange erstellen. Diese Warteschlange verwendet vorübergehend einen alternativen, lokalen Speichermechanismus, um Nachrichten zu speichern, die der Service Bus-Warteschlange nicht hinzugefügt werden können. Die Anwendung kann entscheiden, wie die temporär gespeicherten Nachrichten verarbeitet werden sollen, nachdem der Dienst wiederhergestellt wurde. Weitere Informationen finden Sie unter [Bewährte Methoden für Leistungsoptimierungen mithilfe von Service Bus-Brokermessaging](../service-bus/service-bus-performance-improvements.md). Weitere Informationen finden Sie ebenfalls unter [Service Bus (Notfallwiederherstellung)](./resiliency-technical-guidance-recovery-loss-azure-region.md#service-bus).

###Mobile Services
Es gibt zwei Überlegungen zur Verfügbarkeit für Azure Mobile Services. Erstens: Sichern Sie regelmäßig die Azure SQL-Datenbank-Instanz, die mit Ihrem mobilen Dienst verknüpft ist. Sichern Sie auch die Skripts für den mobilen Dienst. Weitere Informationen finden Sie unter [Notfallwiederherstellung mobiler Dienste](../mobile-services/mobile-services-disaster-recovery.md). Bei einem temporären Ausfall von Mobile Services müssen Sie möglicherweise vorübergehend ein alternatives Azure-Rechenzentrum verwenden. Weitere Informationen finden Sie unter [Mobile Services (Notfallwiederherstellung)](./resiliency-technical-guidance-recovery-loss-azure-region.md#mobile-services).

###HDInsight
Die mit HDInsight verknüpften Daten werden standardmäßig in Azure Blob Storage gespeichert, der die von Azure Storage festgelegten Eigenschaften für Hochverfügbarkeit und Dauerhaftigkeit besitzt. Die Verarbeitung über mehrere Knoten hinweg, die für Hadoop MapReduce-Aufträge erforderlich ist, erfolgt auf einem vorübergehenden Hadoop Distributed File System (HDFS), das bei Bedarf von HDInsight bereitgestellt wird. Die Ergebnisse eines MapReduce-Auftrags werden standardmäßig auch in Azure Blob Storage gespeichert, sodass die verarbeiteten Daten dauerhaft und hoch verfügbar sind, nachdem die Bereitstellung des Hadoop-Clusters aufgehoben wurde. Weitere Informationen finden Sie unter [HDInsight (Notfallwiederherstellung)](./resiliency-technical-guidance-recovery-loss-azure-region.md#hdinsight).

##Prüflisten: lokale Fehler
 
##Prüfliste – Cloud Services
  1. Lesen Sie den Abschnitt [Cloud Services](#cloud-services) in diesem Dokument.
  2. Konfigurieren Sie mindestens zwei Instanzen für jede Rolle.
  3. Speichern Sie den Status in einem permanenten Speicher, nicht in Rolleninstanzen.
  4. Verarbeiten Sie das StatusCheck-Ereignis ordnungsgemäß.
  5. Fassen Sie aufeinander bezogene Änderungen in Transaktionen zusammen, sofern möglich.
  6. Überprüfen Sie, ob die Workerrollenaufgaben idempotent sind und neu gestartet werden können.
  7. Rufen Sie Vorgänge so lange auf, bis sie erfolgreich sind.
  8. Erwägen Sie Strategien für eine automatische Skalierung.

##Prüfliste – Virtual Machines
  1. Lesen Sie den Abschnitt [Virtual Machines](#virtual-machines) in diesem Dokument.
  2. Verwenden Sie das Laufwerk D: nicht als dauerhaften Speicher.
  3. Gruppieren Sie Computer in einer Dienstebene in eine Verfügbarkeitsgruppe.
  4. Konfigurieren Sie den Lastenausgleich und optionale Tests.
 
##Prüfliste – Speicher
  1. Lesen Sie den Abschnitt [Speicher](#storage) in diesem Dokument.
  2. Verwenden Sie mehrere Speicherkonten, wenn Daten oder Bandbreite das Kontingent überschreiten.

##Prüfliste – SQL-Datenbank
  1. Lesen Sie den Abschnitt [SQL-Datenbank](#sql-database) in diesem Dokument.
  2. Implementieren Sie eine Wiederholungsrichtlinie zum Verarbeiten von vorübergehenden Fehlern.
  3. Verwenden Sie Partitionierung/Sharding als Strategie zum horizontalen Hochskalieren.
  
##Prüfliste – SQL Server auf Virtual Machines
  1. Lesen Sie den Abschnitt [SQL Server auf Virtual Machines](#sql-server-on-virtual-machines) in diesem Dokument.
  2. Befolgen Sie die vorherigen Empfehlungen für Virtual Machines.
  3. Verwenden Sie die Hochverfügbarkeitsfunktionen von SQL Server wie z.B. AlwaysOn.
  
##Prüfliste – Service Bus
  1. Lesen Sie den Abschnitt [Service Bus](#service-bus) in diesem Dokument.
  2. Erstellen Sie ggf. eine dauerhafte clientseitige Warteschlange.

##Prüfliste – HDInsight
  1. Lesen Sie den Abschnitt [HDInsight](#hdinsight) in diesem Dokument.
  2. Es sind keine zusätzlichen Schritte erforderlich, um die Verfügbarkeit bei lokalen Fehlern sicherzustellen.
 
##Nächste Schritte
Dieser Artikel gehört zu einer Reihe von Artikeln, die als [Technischer Leitfaden zur Resilienz in Azure](./resiliency-technical-guidance.md) dienen. Im nächsten Artikel dieser Reihe geht es um die [Wiederherstellung nach einer regionsweiten Dienstunterbrechung](./resiliency-technical-guidance-recovery-loss-azure-region.md).

<!---HONumber=AcomDC_0525_2016-->