<properties
	pageTitle="Plattformgestützte Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager"
	description="In diesem Artikel werden die plattformgestützten Migrationsdienstfunktionen der Dienstverwaltung für die Migration zu Azure Resource Manager beschrieben."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="mahthi"
	manager="drewm"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/04/2016"
	ms.author="mahthi"/>

# Plattformgestützte Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager

Es ist schon fast ein Jahr her, dass wir die Unterstützung für virtuelle Computer unter Azure Resource Manager angekündigt haben. Hier finden Sie weitere Informationen zu den Weiterentwicklungen und zusätzlichen Funktionen, die unterstützt werden. Außerdem wird beschrieben, wie Sie Ressourcen aus den beiden Bereitstellungsmodellen am besten verbinden und im Abonnement zusammen nutzen können, indem Sie Standort-zu-Standort-Gateways für virtuelle Netzwerke verwenden. In diesem Artikel soll veranschaulicht werden, wie die Migration von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Resource Manager ermöglicht wird.

## Was ist unser Ziel bei der Migration?

Wir freuen uns sehr über die Leistungsfähigkeit und die Funktionalität der neuen Oberfläche und APIs der virtuellen Computer. Wir sind der Meinung, dass dies wirklich viele neue Möglichkeiten zur Nutzung der Cloud schafft. Dank der Veröffentlichung des neuen Modells können Sie die jeweiligen Dienste in einer Ressourcengruppe bereitstellen, verwalten und überwachen. Resource Manager ermöglicht die Bereitstellung komplexer Anwendungen mit Vorlagen, konfiguriert virtuelle Computer mit VM-Erweiterungen und bietet eine Zugriffsverwaltung und das Taggen. Außerdem kann die skalierbare, parallele Bereitstellung für virtuelle Computer in Verfügbarkeitsgruppen genutzt werden. Darüber hinaus bietet das neue Modell die unabhängige Lebenszyklusverwaltung der Bereiche Compute, Netzwerk und Speicher. Der Schwerpunkt liegt zudem auf der standardmäßigen Sicherstellung der Sicherheit, indem virtuelle Computer in einem virtuellen Netzwerk verwendet werden.

Aus Sicht der Features werden unter Azure Resource Manager fast alle Features für die Bereiche Compute, Netzwerk und Speicher unterstützt. Es gibt noch ein paar Ausnahmen, an denen wir in den nächsten Monaten mit Hochdruck arbeiten werden. Wir optimieren die Benutzeroberfläche ständig und fügen dem Azure-Portal weitere Features hinzu, um die Umgebungen noch besser zu verknüpfen.

Aufgrund dieser neuen Funktion und der wachsenden Bereitstellungsbasis im neuen Azure Resource Manager möchten wir erreichen, dass Kunden vorhandene Bereitstellungen unter dem klassischen Bereitstellungsmodell migrieren können.

>[AZURE.NOTE] Mit dieser Ankündigung starten wir die öffentliche Vorschauversion des Migrationsdiensts. Während der öffentlichen Vorschauphase empfehlen wir nur die Migration von Entwicklungs- oder Testworkloads unter Ihrem Azure-Abonnement.

## Änderungen an der Automatisierung und an den Tools nach der Migration

Beachten Sie Folgendes: Eine der wichtigen Änderungen, die Sie im Rahmen der Migration Ihrer Ressourcen aus dem klassischen Bereitstellungsmodell zum Resource Manager-Modell vornehmen müssen, sind die Aktualisierungen der vorhandenen Automatisierung und der Tools. So stellen Sie sicher, dass auch nach dem Migrieren der Ressourcen alles noch funktioniert.

## Was bedeutet die Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Resource Manager?

Bevor wir näher auf die Details eingehen, möchten wir kurz den Unterschied zwischen Datenebenen- und Verwaltungsebenenvorgängen für IaaS-Ressourcen erklären. Es ist sehr wichtig, dass Sie diesen Unterschied verstehen, weil hierbei deutlich wird, wie wir die Unterstützung der Migration planen.

- Verwaltungsebene: Beschreibt die Aufrufe, die auf der Verwaltungsebene oder in der API zur Änderung von Ressourcen eingehen. Beispiel: Erstellen einer VM, Neustarten einer VM, Aktualisieren eines virtuelles Netzwerks mit einem neuen Subnetz usw. Bei all diesen Vorgängen werden die ausgeführten Ressourcen verwaltet, aber es ergeben sich keine direkten Auswirkungen auf die Verbindungsherstellung mit den Instanzen.
- Datenebene (Anwendung): Beschreibt die „Laufzeit“ der Anwendung selbst und umfasst die Interaktion mit Instanzen, die nicht über die Azure-API verlaufen. Das Zugreifen auf Ihre Website oder das Abrufen von Daten von einer ausgeführten SQL Server-Instanz oder einem mongoDB-Server wird als Interaktion mit der Datenebene bzw. der Anwendung angesehen. Das Kopieren eines Blobs aus einem Speicherkonto und das Zugreifen auf eine öffentliche IP-Adresse, um per RDP oder SSH eine Verbindung mit dem virtuellen Computer herzustellen, sind auch Vorgänge auf der Datenebene. Mit diesen Vorgängen wird die Ausführung der Anwendung für die Bereiche Compute, Netzwerk und Speicher sichergestellt.

>[AZURE.NOTE] Bei einigen Migrationsszenarien (weitere Details im Abschnitt zu den nicht unterstützten Konfigurationen) werden die virtuellen Computer beendet und nach dem Aufheben der Zuordnung neu gestartet, was zu einem kurzen Ausfall der Datenebene führt.

## Wie lauten die unterstützten Migrationsbereiche?

Während der öffentlichen Vorschauphase bieten wir zwei Migrationsbereiche an, bei denen es hauptsächlich um Compute und Netzwerk geht. Die Unterstützung für die Migration von Speicherkonten ist geplant und wird in Kürze veröffentlicht. Um eine nahtlose Migration zu ermöglichen, haben wir dafür gesorgt, dass die klassischen Speicherkonten Datenträger für Resource Manager-VMs enthalten. Unten finden Sie hierzu weitere Details.

### Migration von virtuellen Computern (nicht in einem virtuellen Netzwerk)

Beim Resource Manager-Bereitstellungsmodell wird die Sicherheit Ihrer Anwendungen standardmäßig erzwungen. Daher müssen sich beim Resource Manager-Modell alle VMs in einem virtuellen Netzwerk befinden. Im Rahmen der Migration werden die VMs deshalb neu gestartet (beenden, Zuordnung aufheben und starten). In Bezug auf virtuelle Netzwerke haben Sie folgende Wahlmöglichkeiten:
- Sie können für die Plattform die Erstellung eines neuen virtuellen Netzwerks anfordern und den virtuellen Computer in das neue virtuelle Netzwerk migrieren. (oder)
- Sie können den virtuellen Computer zu einem vorhandenen virtuellen Netzwerk in Resource Manager migrieren.

>[AZURE.NOTE] In diesem Migrationsbereich sind die Vorgänge auf „Verwaltungsebene“ und „Datenebene“ für einen bestimmten Zeitraum während der Migration unter Umständen nicht zulässig.

### Migration von virtuellen Computern (in einem virtuellen Netzwerk)

In diesem Bereich migrieren wir für die meisten VM-Konfigurationen nur die Metadaten zwischen dem klassischen Bereitstellungsmodell und Resource Manager. Die zugrunde liegenden virtuellen Computer werden auf derselben Hardware, in demselben Netzwerk und mit demselben Speicher ausgeführt. Wenn es um die Migration der Metadaten aus dem klassischen Bereitstellungsmodell zu Resource Manager geht, sind die Vorgänge der „Verwaltungsebene“ also unter Umständen für einen bestimmten Zeitraum während der Migration nicht zulässig. Die Datenebene funktioniert aber weiterhin. Die bedeutet, dass es für Ihre Anwendungen, die auf virtuellen Computern ausgeführt werden (klassisch), während der Migration nicht zu Ausfallzeiten kommt.

Die folgenden Konfigurationen werden derzeit nicht unterstützt. Wenn wir diese Unterstützung später hinzufügen, können für folgende VMs bei dieser Konfiguration aber Ausfallzeiten auftreten (beenden, Zuordnung aufheben und neu starten):

-	Verwendung von mehr als einer Verfügbarkeitsgruppe in einem Clouddienst
-	Verwendung von mindestens einer Verfügbarkeitsgruppe und von VMs, die keiner Verfügbarkeitsgruppe angehören, in einem Clouddienst

>[AZURE.NOTE] In diesem Migrationsbereich ist die Verwaltungsebene für einen bestimmten Zeitraum während der Migration unter Umständen nicht zulässig. Für einige spezielle Konfigurationen kann es wie oben beschrieben zu einem Ausfall der Datenebene kommen.

### Speicherkonten und Migration

Die Speicherkontomigration wird derzeit für die öffentliche Vorschauphase nicht unterstützt. Die Unterstützung für die Speicherkontomigration ist aber geplant und soll in Kürze veröffentlicht werden. Es gibt zwei wichtige Aspekte in Bezug auf die Migration und das damit verbundene Speicherkontoverhalten.

- Ermöglichen der Bereitstellung von Resource Manager-VMs in einem klassischen Speicherkonto: Um die nahtlose Migration zu ermöglichen, haben wir die Funktion zum Bereitstellen von Resource Manager-VMs in einem klassischen Speicherkonto eingebaut. Mit dieser Funktion können Compute- und Netzwerkressourcen unabhängig von Speicherkonten migriert werden.
- Bewährte Methode für Speicherkontomigration: Wenn die Speicherkontomigration unterstützt wird, setzt die Plattform die Compute- und Netzwerkressourcen separat von den Speicherkonten durch, um für einen nahtlosen Vorgang zu sorgen.

## Nicht unterstützte Features und Konfigurationen

Derzeit werden bestimmte Features und Konfigurationen nicht unterstützt. Wir arbeiten daran, die entsprechende Unterstützung hinzuzufügen. Im folgenden Abschnitt sind unsere Empfehlungen und Pläne dazu aufgeführt.

### Nicht unterstützte Funktionen

Die folgende Liste enthält Funktionen, die für die öffentliche Vorschauversion nicht unterstützt werden. Optional können Sie diese Einstellungen entfernen, die VMs migrieren und sie dann unter dem Resource Manager-Bereitstellungsmodell wieder aktivieren. Die Unterstützung für die meisten dieser Funktionen ist geplant und wird veröffentlicht, sobald sie verfügbar ist.

Ressourcenanbieter | Funktion
---------- | ------------
Compute | Startdiagnose
Compute | Nicht zugeordnete VM-Datenträger
Compute | VM-Images
Netzwerk | Nicht zugeordnete reservierte IP-Adressen [falls nicht an einen virtuellen Computer angefügt]. Reservierte IP-Adressen, die VMs zugeordnet sind, werden unterstützt.
Netzwerk | Nicht zugeordnete Netzwerksicherheitsgruppen [falls nicht einem virtuellen Netzwerk oder einer Netzwerkschnittstelle zugeordnet]. NSGs, auf die von VNETs verwiesen wird, werden unterstützt.
Netzwerk | Endpunkt-ACLs
Netzwerk | Gateways des virtuellen Netzwerks (Site-to-Site, ExpressRoute, Point-to-Site)
Speicher | Speicherkonten

### Nicht unterstützte Konfigurationen

Die folgende Liste enthält Konfigurationen, die für die öffentliche Vorschauversion nicht unterstützt werden. Unten sind unsere Empfehlungen angegeben. Die Unterstützung für einige dieser Konfigurationen ist geplant und wird veröffentlicht, sobald sie verfügbar ist.

Dienst | Konfiguration | Empfehlungen
---------- | ------------ | ------------
Ressourcen-Manager | Rollenbasierte Zugriffssteuerung für klassische Ressourcen | Da die URIs der Ressourcen nach der Migration geändert werden, ist es ratsam, im Voraus die Aktualisierungen der RBAC-Richtlinien zu planen, die nach der Migration durchgeführt werden müssen.
Compute | Mehrere Subnetze, die einem virtuellen Computer zugeordnet sind | Aktualisieren Sie die Subnetzkonfiguration so, dass nur auf das Subnetz verwiesen wird.
Compute | Virtuelle Computer, die zu einem virtuellen Netzwerk gehören, aber denen kein explizites Subnetz zugewiesen ist | Sie können die VM optional löschen. Die Unterstützung dieser Funktion ist derzeit geplant.
Compute | VMs mit Warnungen, Richtlinien für automatische Skalierung | Derzeit ist diese Migration erfolgreich, und diese Einstellungen werden verworfen. Wir empfehlen Ihnen dringend, vor der Migration Ihre Umgebung auszuwerten. Alternativ hierzu können Sie die Warnungseinstellungen auch nach Abschluss der Migration neu konfigurieren.
Compute | XML-VM-Erweiterungen [VS Debugger, WebDeploy und RemoteDebug] | Wird nicht unterstützt. Es wird empfohlen, diese Erweiterungen vom virtuellen Computer zu entfernen, bevor Sie die Migration fortsetzen.
Compute | Clouddienste, die Web-/Workerrollen enthalten | Dies wird derzeit nicht unterstützt und befindet sich um Planungsstadium.
Netzwerk | Virtuelle Netzwerke, die virtuelle Computer und Web-/Workerrollen enthalten | Dies wird derzeit nicht unterstützt und befindet sich um Planungsstadium.
Netzwerk | Subnetze, die Leerzeichen im Namen enthalten | Die Unterstützung dieser Funktion ist geplant.
App-Dienste | Virtuelles Netzwerk, das App Service-Umgebungen enthält | Dies wird derzeit nicht unterstützt und befindet sich um Planungsstadium.
HDInsight-Dienste | Virtuelles Netzwerk, das HDInsight-Dienste enthält | Dies wird derzeit nicht unterstützt und befindet sich um Planungsstadium.
Dynamics Lifecycle Services | Virtuelles Netzwerk, das virtuelle Computer mit Verwaltung per Dynamics Lifecycle Services enthält | Dies wird derzeit nicht unterstützt und befindet sich um Planungsstadium.

## Migrationsvorgang

Wir empfehlen Ihnen, einige Schritte auszuführen, bevor Sie mit dem Migrationsvorgang beginnen.

1. Stellen Sie sicher, dass für die Ressourcen, die für die Migration eingeplant sind, keine nicht unterstützten Features oder Konfigurationen genutzt werden. In den meisten Fällen erkennt die Plattform Probleme dieser Art und löst einen Fehler aus.
2. Wenn Sie über VMs verfügen, die nicht in einem virtuellen Netzwerk enthalten sind, werden sie während der Vorbereitung beendet, und die Zuordnung wird aufgehoben. Wenn Sie die öffentliche IP-Adresse also nicht verlieren möchten, ist es hilfreich, sich die Informationen zum Reservieren der IP-Adresse anzusehen, bevor Sie den Vorbereitungsprozess auslösen. Wenn sich die VMs aber in einem virtuellen Netzwerk befinden, werden sie nicht beendet, und die Zuordnung wird nicht aufgehoben.
3. Für Azure ist es ratsam, dass Sie eine Testumgebung mit einem ähnlichen Setup einrichten und diese Ressourcen migrieren, bevor Sie Produktionsbereitstellungen migrieren.
4. Planen Sie die Migration für einen Zeitraum außerhalb der Geschäftszeiten, um Raum für unerwartete Fehler zu lassen, die während der Migration unter Umständen auftreten können.
5. Laden Sie die aktuelle Konfiguration Ihrer virtuellen Computer mit PowerShell, CLI-Befehlen oder REST-APIs herunter, um die Überprüfung zu vereinfachen, nachdem der Vorbereitungsschritt abgeschlossen ist.
6. Aktualisieren Sie Ihre Skripts für die Automatisierung und Operationalisierung, um sie an das Resource Manager-Bereitstellungsmodell anzupassen, bevor Sie die Migration starten. Außerdem können Sie optional GET-Vorgänge durchführen, wenn sich die Ressourcen im Zustand „Vorbereitet“ befinden.
7. Werten Sie die RBAC-Richtlinien aus, die für die klassischen IaaS-Ressourcen konfiguriert sind, und stellen Sie einen Plan für die Vorgehensweise nach dem Abschluss der Migration auf.

Mit der Ankündigung der öffentlichen Vorschau haben wir Unterstützung für das Auslösen der Migration über REST-APIs, PowerShell und die Azure-Befehlszeilenschnittstelle hinzugefügt. Die Azure-Portal-Unterstützung für die Migration, damit Sie den Migrationsablauf vom klassischen Bereitstellungsmodell zur ARM-Migration visualisieren und durchlaufen können, ist derzeit geplant.

![Screenshot zum Migrationsworkflow](./media/virtual-machines-windows-migration-classic-resource-manager/migration-workflow.png)

1.	Vorbereiten
	* Dies ist der erste Schritt im Migrationsprozess. Das Ziel dieses Schritts besteht darin, die Transformation der IaaS-Ressourcen von Ressourcen des klassischen Bereitstellungsmodells zu Resource Manager-Ressourcen zu simulieren und zu Visualisierungszwecken nebeneinander darzustellen. Der Ablauf der Aktionen ist unten ausführlich beschrieben.
  * Sie wählen das virtuelle Netzwerk oder den gehosteten Dienst aus (sofern es kein VNET ist), den Sie für die Migration vorbereiten möchten.
  *	Zuerst führt die Plattform im Hintergrund immer eine Datenanalyse für die zu migrierenden Ressourcen durch und gibt „Erfolg“ (bzw. „Fehler“) zurück, wenn die Ressourcen für die Migration bereit sind.
	*	Wenn die Migration für die Ressource nicht möglich ist, werden die Gründe dafür angegeben, warum die Migration nicht unterstützt wird.
	* Wenn die Migration für die Ressource durchgeführt werden kann, sperrt die Plattform zuerst die Vorgänge der Verwaltungsebene für die zu migrierenden Ressourcen. Beispiel: Es ist nicht möglich, einer in der Migration befindlichen VM einen Datenträger hinzuzufügen.
  *	Die Plattform startet für die zu migrierenden Ressourcen dann die Migration der Metadaten vom klassischen Bereitstellungsmodell zu Resource Manager.  
  *	Nach Abschluss der Vorbereitung können Sie die Ressourcen für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell visualisieren. Für jeden Clouddienst des klassischen Bereitstellungsmodells erstellen wir einen Ressourcengruppennamen nach dem Muster `cloud-service-name>-migrated`.

2.	Überprüfung – manuell oder per Skript
  * In diesem Schritt können Sie bei Bedarf die Konfiguration verwenden, die Sie zuvor heruntergeladen haben, um zu überprüfen, ob die Migration korrekt aussieht. Alternativ dazu können Sie sich auch am Portal anmelden und Stichproben der Eigenschaften und Ressourcen durchführen, um sicherzustellen, dass für die Migration der Metadaten alles richtig vorbereitet ist.
	* Wenn Sie ein virtuelles Netzwerk migrieren, wird der Großteil der Konfigurationen virtueller Computer nicht neu gestartet. Für die Anwendungen auf diesen VMs können Sie überprüfen, ob die Anwendung noch betriebsbereit ist und ausgeführt wird.
	* Sie können Ihre Skripts für Überwachung/Automatisierung und den Betrieb testen, um zu ermitteln, ob die VMs wie erwartet ausgeführt werden und ob die aktualisierten Skripts richtig funktionieren. Beachten Sie, dass nur GET-Vorgänge unterstützt werden, wenn sich die Ressourcen im Status „Vorbereitet“ befinden.
  * Es gibt kein festes Zeitfenster, vor dem Sie für die Migration ein „Commit“ durchführen müssen. Sie können sich in diesem Zustand so viel Zeit wie nötig lassen. Beachten Sie aber, dass die Verwaltungsebene für diese Ressourcen gesperrt ist, bis Sie entweder einen Abbruch (abort) oder ein Commit (commit) durchführen.
  * Falls Probleme auftreten, können Sie die Migration immer abbrechen und zurück zum klassischen Bereitstellungsmodell wechseln. Nach dem Wechsel zurück werden die Vorgänge der Verwaltungsebene für die Ressourcen geöffnet, damit Sie den normalen Betrieb dieser VMs unter dem klassischen Bereitstellungsmodell wieder aufnehmen können.

3. Abbruch
  * Dies ist ein optionaler Schritt, mit dem Sie Ihre Änderungen auf das klassische Bereitstellungsmodell zurücksetzen und die Migration abbrechen können. Beachten Sie, dass dieser Vorgang nicht mehr ausgeführt werden kann, nachdem Sie den Vorgang „Commit“ ausgelöst haben. 	

4.	Commit
  * Nachdem Sie die Überprüfung abgeschlossen haben, können Sie ein „Commit“ für die Migration durchführen. Die Ressourcen werden unter dem klassischen Bereitstellungsmodell nicht mehr angezeigt, sondern sind nur noch unter dem Resource Manager-Bereitstellungsmodell verfügbar. Dies bedeutet auch, dass die migrierten Ressourcen nur im neuen Portal verwaltet werden können.
	* Wenn dieser Vorgang nicht erfolgreich ist, raten wir Ihnen, es einige Male erneut zu versuchen. Falls sich kein Erfolg einstellt, sollten Sie ein Supportticket oder einen Eintrag im Forum mit dem Tag ClassicIaaSMigration erstellen. Dies ist [hier](https://social.msdn.microsoft.com/Forums/azure/de-DE/home?forum=WAVirtualMachinesforWindows) möglich.

>[AZURE.NOTE] Beachten Sie Folgendes: Alle im Anschluss beschriebenen Vorgänge sind idempotent. Sollte ein Fehler auftreten, der nicht auf ein nicht unterstütztes Feature oder auf eine nicht unterstützte Konfiguration zurückzuführen ist, wiederholen Sie den prepare-, abort- oder commit-Vorgang. Die Plattform versucht dann erneut, die Aktion auszuführen.

## Nächste Schritte
Nachdem Sie nun eine Vorstellung von der Migration klassischer IaaS-Ressourcen zu Resource Manager haben, können Sie damit beginnen, die Ressourcen zu migrieren.

- [Ausführliche technische Informationen zur plattformgestützten Migration vom klassischen Bereitstellungsmodell zu Azure Resource Manager](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
- [Migrieren von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe von PowerShell](virtual-machines-windows-ps-migration-classic-resource-manager.md)
- [Migrieren von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe der Befehlszeilenschnittstelle](virtual-machines-linux-cli-migration-classic-resource-manager.md)
- [Klonen eines klassischen virtuellen Computers für Azure Resource Manager mithilfe von PowerShell-Skripts aus der Community](virtual-machines-windows-migration-scripts.md)

## Häufig gestellte Fragen

**Wirkt sich dieser Migrationsplan auf meine vorhandenen Dienste oder Anwendungen aus, die auf Azure Virtual Machines ausgeführt werden?**

Nein. Bei den virtuellen Computern (klassisch) handelt es sich um vollständig unterstützte Dienste mit allgemeiner Verfügbarkeit. Sie können diese Ressourcen weiterhin nutzen, um Ihren „Fußabdruck“ in der Microsoft Azure Cloud zu erweitern.

**Was passiert mit meinen VMs, wenn ich für die nahe Zukunft keine Migration plane?**

Die vorhandenen klassischen APIs und das Ressourcenmodell werden nicht als veraltet ausgesondert. Wir möchten die Migration extrem einfach gestalten, indem wir die erweiterten Funktionen für das Resource Manager-Bereitstellungsmodell zur Verfügung stellen. Daher empfehlen wir Ihnen dringend, sich [hier](virtual-machines-windows-compare-deployment-models.md) über einige Weiterentwicklungen zu informieren, die im Rahmen des IaaS-Vorgangs unter Resource Manager vorgenommen wurden.

**Was bedeutet dieser Migrationsplan für meine vorhandenen Tools?**

Die Aktualisierung Ihrer Tools auf das Resource Manager-Bereitstellungsmodell ist eine der wichtigsten Änderungen, die Sie in Ihren Migrationsplänen berücksichtigen sollten.

**Wie lange dauert der Ausfall der Verwaltungsebene?**

Dies hängt davon ab, wie viele Ressourcen migriert werden. Bei kleineren Bereitstellungen (einige Dutzend VMs) sollte der gesamte Migrationsprozess weniger als eine Stunde dauern. Für umfangreiche Bereitstellungen (Hunderte von VMs) kann der Vorgang aber auch mehrere Stunden dauern. Wenn sich der Dienst in der öffentlichen Vorschauphase befindet, empfehlen wir Ihnen dringend, ihn unter Ihrem Entwicklungs- oder Testabonnement auszuführen, um die Auswirkungen auszuwerten.

**Kann ich ein Rollback durchführen, nachdem für meine migrierten Ressourcen in Resource Manager ein Commit durchgeführt wurde?**

Sie können die Migration abbrechen, solange sich die Ressourcen im Zustand „Vorbereitet“ befinden. Das Rollback wird nicht unterstützt, nachdem die Ressourcen per Commit-Vorgang erfolgreich migriert wurden.

**Kann ich für meine Migration ein Rollback durchführen, wenn beim Commit-Vorgang ein Fehler auftritt?**

Sie können die Migration nicht abbrechen, wenn für den Commit-Vorgang ein Fehler auftritt. Alle Migrationsvorgänge, einschließlich des Commit-Vorgangs, sind idempotent. Daher wird empfohlen, dass Sie versuchen, den Vorgang nach einer kurzen Wartezeit zu wiederholen. Falls der Fehler weiterhin auftritt, sollten Sie ein Supportticket oder einen Eintrag im Forum mit dem Tag ClassicIaaSMigration erstellen. Dies ist [hier](https://social.msdn.microsoft.com/Forums/azure/de-DE/home?forum=WAVirtualMachinesforWindows) möglich.

**Muss ich eine weitere ExpressRoute-Verbindung kaufen, wenn ich IaaS unter dem Resource Manager verwenden muss?**

Nein. Wir haben vor Kurzem die [Koexistenz einer ExpressRoute-Verbindung für das klassische Bereitstellungsmodell und Resource Manager](../expressroute/expressroute-howto-coexist-resource-manager.md) ermöglicht. Sie müssen keine neue ExpressRoute-Verbindung erwerben, wenn Sie bereits eine besitzen.

**Gibt es eine Roadmap dafür, zu welchem Zeitpunkt die nicht unterstützten Szenarien der Migrationsliste hinzugefügt werden?**

Derzeit ist für die Veröffentlichung der ersten Gruppe der Szenarien die erste Hälfte des Kalenderjahrs 2016 geplant. Nach der ersten Veröffentlichung werden wir damit fortfahren, die Unterstützung für die bisher nicht unterstützten Funktionen hinzuzufügen.

**Was passiert, wenn ich für meine klassischen IaaS-Ressourcen Richtlinien für die rollenbasierte Zugriffssteuerung konfiguriert habe?**

Während der Migration wird für die Ressourcen die Transformation vom klassischen Bereitstellungsmodell zu Resource Manager durchgeführt. Es ist also ratsam, im Voraus die Aktualisierungen der RBAC-Richtlinien zu planen, die nach der Migration durchgeführt werden müssen.

**Was passiert, wenn ich gegenwärtig Azure Site Recovery- oder Backup-Dienste unter Azure verwende?**

Die Unterstützung für Azure Site Recovery und Backup für virtuelle Computer unter Resource Manager wurde vor Kurzem hinzugefügt. Wir arbeiten eng mit den zuständigen Teams zusammen, um eine Funktion zu ermöglichen, mit der auch die Migration von VMs zu Resource Manager durchgeführt werden kann. Derzeit empfehlen wir Ihnen, keine Migration durchzuführen, wenn Sie diese Funktionen nutzen.

**Kann ich mein Abonnement oder meine Ressourcen überprüfen, um zu ermitteln, ob sie für die Migration geeignet sind?**

Derzeit wird während des Vorbereitungsvorgangs eine implizite Überprüfung für die Ressourcen durchgeführt, die für die Migration vorbereitet werden. Bei der Option für die plattformgestützte Migration besteht der erste Schritt zum Vorbereiten der Migration darin zu überprüfen, ob die Ressourcen für die Migration geeignet sind. Wenn die Überprüfung nicht erfolgreich ist, werden die Ressourcen nicht „angefasst“. Wir planen aber auch die Veröffentlichung einer neuen Überprüfungsaktion, mit der der Vorgang erheblich vereinfacht wird.

**Was passiert, wenn ein Kontingentfehler auftritt, während ich die IaaS-Ressourcen für die Migration vorbereite?**

Wir empfehlen Ihnen, die Migration abzubrechen. Erstellen Sie anschließend eine Supportanfrage, um die Kontingente in der Region zu erhöhen, zu der Sie die VMs migrieren. Nachdem die Kontingentanfrage genehmigt wurde, können Sie wieder mit dem Ausführen der Migrationsschritte beginnen.

**Wie kann ich ein Problem melden, wenn ich keinen Supportvertrag habe?**

Bitte posten Sie Ihre Probleme und Fragen zur Migration [hier](https://social.msdn.microsoft.com/Forums/azure/de-DE/home?forum=WAVirtualMachinesforWindows) in unserem Forum zu Virtual Machines unter dem Schlüsselwort ClassicIaaSMigration. Wir empfehlen Ihnen, alle Fragen in diesem Forum zu stellen. Falls Sie über einen Supportvertrag verfügen, können Sie auch gern zusätzlich ein Supportticket erstellen.

**Was passiert, wenn mir die Namen nicht gefallen, die von der Plattform während der Migration für meine Ressourcengruppen ausgewählt wurden?**

In Kürze wird die Unterstützung für das Verschieben von Ressourcen zwischen Ressourcengruppen angekündigt. Sobald diese Funktion unterstützt wird, können Sie die Ressourcen in die Ressourcengruppe Ihrer Wahl verschieben.

**Was passiert, wenn mir die Namen der Ressourcen nicht gefallen, die von der Plattform während der Migration ausgewählt wurden?**

Für alle Ressourcen, für die Sie unter dem klassischen Bereitstellungsmodell explizit Namen angeben, werden diese während der Migration beibehalten. In einigen Fällen werden neue Ressourcen erstellt. Beispiel: Für jede VM wird eine Netzwerkschnittstelle erstellt. Derzeit wird es nicht unterstützt, die Namen dieser neuen Ressourcen, die während der Migration erstellt werden, zu steuern. Sie können [hier](http://feedback.azure.com) Ihre Stimme für diese Funktion abgeben.

<!---HONumber=AcomDC_0511_2016-->