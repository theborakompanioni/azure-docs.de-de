<properties
	pageTitle="Plattformgestützte Migration von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager | Microsoft Azure"
	description="In diesem Artikel wird die plattformgestützte Migration von Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager erläutert."
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

# Plattformgestützte Migration von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager

Es ist schon fast ein Jahr her, seit wir die Unterstützung für virtuelle Computer unter Azure Resource Manager angekündigt haben. Hier finden Sie weitere Informationen zu den Weiterentwicklungen und zusätzlichen Funktionen, die unterstützt werden. Außerdem wird beschrieben, wie Sie am besten eine Verbindung herstellen und Ressourcen aus den beiden Bereitstellungsmodellen im Abonnement zusammen nutzen können, indem Sie Site-to-Site-Gateways für virtuelle Netzwerke verwenden. In diesem Artikel wird veranschaulicht, wie die Migration von IaaS-Ressourcen (Infrastructure-as-a-Service) vom klassischen Bereitstellungsmodell zu Resource Manager ermöglicht wird.

## Ziel der Migration

Mit der Veröffentlichung des neuen Modells können Sie zusammengehörige Dienste in einer Ressourcengruppe bereitstellen, verwalten und überwachen Resource Manager ermöglicht die Bereitstellung komplexer Anwendungen über Vorlagen, konfiguriert virtuelle Computer mit VM-Erweiterungen und bietet Möglichkeiten zur Zugriffsverwaltung und Markierung. Außerdem kann die skalierbare, parallele Bereitstellung für virtuelle Computer in Verfügbarkeitsgruppen genutzt werden. Darüber hinaus bietet das neue Modell die unabhängige Lebenszyklusverwaltung der Bereiche Compute, Netzwerk und Speicher. Zudem liegt ein Schwerpunkt auf der standardmäßigen Sicherstellung der Sicherheit, indem virtuelle Computer in einem virtuellen Netzwerk verwendet werden.

Nahezu alle Funktionen des klassischen Bereitstellungsmodells werden für die Bereiche Compute, Netzwerk und Speicher unter Azure Resource Manager unterstützt. Aufgrund dieser neuen Funktion und der wachsenden Bereitstellungsbasis in Azure Resource Manager möchten wir, dass Kunden vorhandene Bereitstellungen im klassischen Bereitstellungsmodell migrieren können.

>[AZURE.NOTE] Während der öffentlichen Vorschau des Migrationsdiensts empfehlen wir, ausschließlich Nicht-Produktions-Workloads in Ihrem Azure-Abonnement zu migrieren.

## Änderungen an der Automatisierung und an den Tools nach der Migration

Im Rahmen der Migration Ihrer Ressourcen vom klassischen Modell zum Resource Manager-Modell müssen Sie Ihre bestehende Automatisierung oder die bestehenden Tools aktualisieren, um sicherzustellen, dass sie nach der Migration weiterhin funktionieren.

## Die Bedeutung der Migration von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Resource Manager

Bevor wir näher auf die Details eingehen, möchten wir kurz den Unterschied zwischen Datenebenen- und Verwaltungsebenenvorgängen für IaaS-Ressourcen erklären. Es ist sehr wichtig, dass Sie diesen Unterschied verstehen, weil dadurch deutlich wird, wie wir die Unterstützung der Migration planen.

- *Verwaltungsebene*: Beschreibt die Aufrufe, die auf der Verwaltungsebene oder in der API zur Änderung von Ressourcen eingehen. Beispielsweise werden ausgeführte Ressourcen durch Vorgänge verwaltet wie das Erstellen eines virtuellen Computers, das Neustarten eines virtuellen Computers und das Aktualisieren eines virtuellen Netzwerks mit einem neuen Subnetz. Die Verbindung zu den Instanzen ist davon nicht direkt betroffen.
- *Datenebene* (Anwendung): Beschreibt die Laufzeit der Anwendung selbst und umfasst die Interaktion mit Instanzen, die nicht über die Azure-API verlaufen. Das Zugreifen auf Ihre Website oder das Abrufen von Daten von einer ausgeführten SQL Server-Instanz oder einem MongoDB-Server wird als Interaktion mit der Datenebene bzw. der Anwendung angesehen. Das Kopieren eines Blobs aus einem Speicherkonto und das Zugreifen auf eine öffentliche IP-Adresse, um per RDP oder SSH eine Verbindung mit dem virtuellen Computer herzustellen, sind ebenfalls Vorgänge auf der Datenebene. Mit diesen Vorgängen wird die Ausführung der Anwendung für die Bereiche Compute, Netzwerk und Speicher sichergestellt.

>[AZURE.NOTE] In einigen Migrationsszenarien werden wir Ihre virtuellen Computer beenden, die Zuordnung aufheben und sie wieder neu starten. Dadurch wird eine kurze Downtime auf der Datenebene entstehen.

## Unterstützte Migrationsbereiche

Während der öffentlichen Vorschau bieten wir zwei Migrationsbereiche an, bei denen es hauptsächlich um Compute und Netzwerk geht. Um eine nahtlose Migration zu ermöglichen, haben wir ermöglicht, dass die klassischen Speicherkonten Datenträger für Resource Manager-VMs enthalten.

### Migration virtueller Computer (nicht in einem virtuellen Netzwerk)

Beim Resource Manager-Bereitstellungsmodell wird die Sicherheit Ihrer Anwendungen standardmäßig erzwungen. Beim Resource Manager-Modell müssen sich alle virtuellen Computer in einem virtuellen Netzwerk befinden. Daher werden die virtuellen Computer im Rahmen der Migration neu gestartet (`Stop`, `Deallocate` und `Start`). Für die virtuellen Netzwerke haben Sie zwei Optionen:
- Sie können für die Plattform die Erstellung eines neuen virtuellen Netzwerks anfordern und den virtuellen Computer zum neuen virtuellen Netzwerk migrieren.
- Sie können den virtuellen Computer zu einem vorhandenen virtuellen Netzwerk in Resource Manager migrieren.

>[AZURE.NOTE] In diesem Migrationsbereich sind die Vorgänge auf Verwaltungsebene und Datenebene für einen Zeitraum während der Migration unter Umständen nicht zulässig.

### Migration virtueller Computer (in einem virtuellen Netzwerk)

In diesem Bereich migrieren wir für die meisten VM-Konfigurationen nur die Metadaten zwischen dem klassischen Bereitstellungsmodell und dem Resource Manager-Bereitstellungsmodell. Die zugrunde liegenden virtuellen Computer werden auf derselben Hardware, in demselben Netzwerk und mit demselben Speicher ausgeführt. Wenn es um die Migration der Metadaten aus dem klassischen Bereitstellungsmodell zu Resource Manager geht, sind die Vorgänge der Verwaltungsebene also unter Umständen für einen bestimmten Zeitraum während der Migration nicht zulässig. Die Datenebene funktioniert jedoch weiterhin. Dies bedeutet, dass es für Ihre Anwendungen, die auf virtuellen Computern ausgeführt werden (klassisch), während der Migration nicht zur Downtime kommt.

Die folgenden Konfigurationen werden derzeit nicht unterstützt. Wenn wir diese Unterstützung später hinzufügen, kann es für einige virtuelle Computer bei dieser Konfiguration zur Downtime kommen (VM-Vorgänge zum Beenden, Aufheben der Zuordnung und Neustart werden durchlaufen).

-	Verwendung mehrerer Verfügbarkeitsgruppen in einem Clouddienst.
-	Verwendung von mindestens einer Verfügbarkeitsgruppe und von virtuellen Computern, die keiner Verfügbarkeitsgruppe angehören, in einem einzigen Clouddienst.

>[AZURE.NOTE] In diesem Migrationsbereich ist die Verwaltungsebene für einen Zeitraum während der Migration unter Umständen nicht zulässig. Für bestimmte Konfigurationen kann es wie zuvor beschrieben zu einer Downtime der Datenebene kommen.

### Speicherkonten und Migration

Die Speicherkontomigration wird für die öffentliche Vorschau nicht unterstützt.

Um eine nahtlose Migration zu ermöglichen, haben wir dafür gesorgt, dass Resource Manager-VMs in einem klassischen Speicherkonto bereitgestellt werden können. Mit dieser Funktion können und sollen Compute- und Netzwerkressourcen unabhängig von Speicherkonten migriert werden.

## Nicht unterstützte Features und Konfigurationen

Derzeit werden einige Features und Konfigurationen nicht unterstützt. In den folgenden Abschnitten wird beschrieben, was wir diesbezüglich empfehlen.

### Nicht unterstützte Funktionen

Die folgenden Features werden für die öffentliche Vorschau nicht unterstützt. Optional können Sie diese Einstellungen entfernen, die virtuellen Computer migrieren und die Einstellungen dann im Resource Manager-Bereitstellungsmodell wieder aktivieren.

Ressourcenanbieter | Funktion
---------- | ------------
Compute | Nicht zugeordnete VM-Datenträger.
Compute | VM-Images.
Netzwerk | Nicht zugeordnete reservierte IP-Adressen (falls nicht einem virtuellen Computer zugeordnet). Reservierte IP-Adressen, die virtuellen Computern zugeordnet sind, werden unterstützt.
Netzwerk | Nicht zugeordnete Netzwerksicherheitsgruppen (falls nicht einem virtuellen Netzwerk oder einer Netzwerkschnittstelle zugeordnet). NSGs, auf die von virtuellen Netzwerken verwiesen wird, werden unterstützt.
Netzwerk | Endpunkt-ACLs.
Netzwerk | Gateways des virtuellen Netzwerks (Standort-zu-Standort, Azure ExpressRoute, Punkt-zu-Standort).

### Nicht unterstützte Konfigurationen

Die folgenden Konfigurationen werden nicht für die öffentliche Vorschau unterstützt.

Dienst | Konfiguration | Empfehlungen
---------- | ------------ | ------------
Ressourcen-Manager | Rollenbasierte Zugriffssteuerung (RBAC) für klassische Ressourcen | Da der URI der Ressourcen nach der Migration geändert wird, empfehlen wir Ihnen, die RBAC-Richtlinienaktualisierungen zu planen, die nach der Migration ausgeführt werden müssen.
Compute | Mehrere Subnetze, die einem virtuellen Computer zugeordnet sind | Aktualisieren Sie die Subnetzkonfiguration so, dass nur auf Subnetze verwiesen wird.
Compute | Virtuelle Computer, die zu einem virtuellen Netzwerk gehören, aber denen kein explizites Subnetz zugewiesen ist | Sie können die VM optional löschen.
Compute | Virtuelle Computer mit Warnungen, Richtlinien für automatische Skalierung | Derzeit ist diese Migration erfolgreich, und diese Einstellungen werden verworfen. Wir empfehlen Ihnen dringend, vor der Migration Ihre Umgebung auszuwerten. Alternativ hierzu können Sie die Warnungseinstellungen nach Abschluss der Migration neu konfigurieren.
Compute | XML-VM-Erweiterungen (Visual Studio Debugger, Web Deploy und Remotedebuggen) | Dies wird nicht unterstützt. Wir empfehlen, diese Erweiterungen vom virtuellen Computer zu entfernen, um die Migration fortzusetzen.
Compute | Clouddienste, die Web-/Workerrollen enthalten | Dies wird derzeit nicht unterstützt.
Netzwerk | Virtuelle Netzwerke, die virtuelle Computer und Web-/Workerrollen enthalten | Dies wird derzeit nicht unterstützt.
Azure App Service | Virtuelle Netzwerke, die App Service-Umgebungen enthalten | Dies wird derzeit nicht unterstützt.
Azure HDInsight | Virtuelle Netzwerke, die HDInsight-Dienste enthalten | Dies wird derzeit nicht unterstützt.
Microsoft Dynamics Lifecycle Services | Virtuelle Netzwerke, die virtuelle Computer enthalten, die von Dynamics Lifecycle Services verwaltet werden | Dies wird derzeit nicht unterstützt.

## Migrationsvorgang

Wir empfehlen Ihnen Folgendes, bevor Sie mit dem Migrationsvorgang beginnen:

- Stellen Sie sicher, dass die Ressourcen, die Sie migrieren möchten, keine nicht unterstützten Features oder Konfigurationen verwenden. In den meisten Fällen erkennt die Plattform Probleme dieser Art und löst einen Fehler aus.
- Wenn Sie über virtuelle Computer verfügen, die nicht in einem virtuellen Netzwerk enthalten sind, werden diese während der Vorbereitung beendet, und die Zuordnung wird aufgehoben. Wenn Sie die öffentliche IP-Adresse nicht verlieren möchten, ist es hilfreich, sich die Informationen zum Reservieren der IP-Adresse anzusehen, bevor Sie den Vorbereitungsvorgang beginnen. Wenn sich die virtuellen Computer aber in einem virtuellen Netzwerk befinden, werden sie nicht beendet, und die Zuordnung wird nicht aufgehoben.
- Versuchen Sie zu diesem Zeitpunkt keine Produktionsressourcen zu migrieren.
- Planen Sie die Migration für einen Zeitraum außerhalb der Geschäftszeiten, um Raum für unerwartete Fehler zu lassen, die während der Migration unter Umständen auftreten können.
- Laden Sie die aktuelle Konfiguration Ihrer virtuellen Computer mit PowerShell, CLI-Befehlen (Befehlszeilenschnittstelle) oder REST-APIs herunter, um die Überprüfung zu vereinfachen, nachdem der Vorbereitungsschritt abgeschlossen ist.
- Aktualisieren Sie Ihre Skripts für die Automatisierung und Operationalisierung, um sie an das Resource Manager-Bereitstellungsmodell anzupassen, bevor Sie die Migration starten. Sie können optional GET-Vorgänge durchführen, wenn sich die Ressourcen im Zustand „Vorbereitet“ befinden.
- Werten Sie die RBAC-Richtlinien aus, die für die klassischen IaaS-Ressourcen konfiguriert sind, und stellen Sie einen Plan für die Vorgehensweise nach dem Abschluss der Migration auf.

Der Migrationsworkflow sieht wie folgt aus. Mit der Ankündigung der öffentlichen Vorschau haben wir Unterstützung für das Auslösen der Migration über REST-APIs, PowerShell und die Azure-Befehlszeilenschnittstelle hinzugefügt.

![Screenshot zum Migrationsworkflow](./media/virtual-machines-windows-migration-classic-resource-manager/migration-workflow.png)

>[AZURE.NOTE] Alle in den folgenden Abschnitten beschriebenen Vorgänge sind idempotent. Sollte ein Problem auftreten, das nicht auf ein nicht unterstütztes Feature oder auf einen Konfigurationsfehler zurückzuführen ist, wiederholen Sie den Vorbereitungs-, Abbruch- oder Commitvorgang. Die Plattform versucht dann erneut, die Aktion auszuführen.

### Vorbereiten

Der Vorbereitungsvorgang ist der erste Schritt im Migrationsprozess. Das Ziel dieses Schritts besteht darin, die Transformation der IaaS-Ressourcen von Ressourcen des klassischen Bereitstellungsmodells zu Resource Manager-Ressourcen zu simulieren und diese zu Visualisierungszwecken nebeneinander darzustellen.

Sie wählen das virtuelle Netzwerk oder den gehosteten Dienst aus (sofern es kein virtuelles Netzwerk ist), den Sie für die Migration vorbereiten möchten.

Zuerst analysiert die Plattform im Hintergrund immer die Daten für die zu migrierenden Ressourcen und gibt durch „Erfolgreich“/„Fehler“ an, ob die Ressourcen für die Migration bereit sind.

* Wenn die Migration für die Ressource nicht möglich ist, werden über die Plattform die Gründe dafür angegeben, warum die Migration nicht unterstützt wird.
* Wenn die Migration für die Ressource durchgeführt werden kann, sperrt die Plattform zuerst die Vorgänge der Verwaltungsebene für die zu migrierenden Ressourcen. Beispielsweise ist es nicht möglich, einem in der Migration befindlichen virtuellen Computer einen Datenträger hinzuzufügen.

Die Plattform startet dann für die zu migrierenden Ressourcen die Migration der Metadaten vom klassischen Bereitstellungsmodell zu Resource Manager.

Nach Abschluss der Vorbereitung haben Sie die Option, die Ressourcen sowohl im klassischen Bereitstellungsmodell als auch im Resource Manager-Bereitstellungsmodell zu visualisieren. Für jeden Clouddienst im klassischen Bereitstellungsmodell erstellen wir einen Ressourcengruppennamen nach dem Muster `cloud-service-name>-migrated`.

### Überprüfung (manuell oder per Skript)

Im Überprüfungsschritt können Sie bei Bedarf die Konfiguration verwenden, die Sie zuvor heruntergeladen haben, um zu überprüfen, ob die Migration korrekt aussieht. Alternativ dazu können Sie sich auch beim Portal anmelden und Stichproben der Eigenschaften und Ressourcen durchführen, um sicherzustellen, dass die Migration der Metadaten in Ordnung ist.

Wenn Sie ein virtuelles Netzwerk migrieren, wird der Großteil der Konfiguration für virtuelle Computer nicht neu gestartet. Für die Anwendungen auf diesen VMs können Sie überprüfen, ob die Anwendung noch betriebsbereit ist und ausgeführt wird.

Sie können Ihre Skripts für Überwachung/Automatisierung und den Betrieb testen, um zu ermitteln, ob die VMs wie erwartet ausgeführt werden und ob die aktualisierten Skripts richtig funktionieren. Beachten Sie, dass GET-Vorgänge nur dann unterstützt werden, wenn sich die Ressourcen im Status „Vorbereitet“ befinden.

Es gibt kein festes Zeitfenster, vor dem Sie für die Migration ein Commit durchführen müssen. Sie können sich in diesem Zustand so viel Zeit wie nötig lassen. Beachten Sie aber, dass die Verwaltungsebene für diese Ressourcen gesperrt ist, bis Sie entweder einen Abbruch (abort) oder ein Commit (commit) durchführen.

Falls Probleme auftreten, können Sie die Migration immer abbrechen und zurück zum klassischen Bereitstellungsmodell wechseln. Nach dem Wechsel zurück werden die Vorgänge der Verwaltungsebene für die Ressourcen geöffnet, damit Sie den normalen Betrieb für diese virtuellen Computer im klassischen Bereitstellungsmodell wieder aufnehmen können.

### Abbruch

Der Abbruch ist ein optionaler Schritt, mit dem Sie Ihre Änderungen auf das klassische Bereitstellungsmodell zurücksetzen und die Migration beenden können. Beachten Sie, dass dieser Vorgang nicht mehr ausgeführt werden kann, nachdem Sie den Commitvorgang ausgelöst haben.

### Commit

Nach Abschluss der Überprüfung können Sie einen Commit für die Migration durchführen. Die Ressourcen werden nicht mehr im klassischen Bereitstellungsmodell angezeigt und stehen nur noch im Resource Manager-Bereitstellungsmodell zur Verfügung. Dies bedeutet auch, dass die migrierten Ressourcen nur im neuen Portal verwaltet werden können.

Wenn dieser Vorgang nicht erfolgreich ist, raten wir Ihnen, es einige Male erneut zu versuchen. Sollten weiterhin Fehler auftreten, erstellen Sie ein Supportticket, oder erstellen Sie einen Eintrag in unserem [VM-Forum](https://social.msdn.microsoft.com/Forums/azure/de-DE/home?forum=WAVirtualMachinesforWindows) mit dem Tag „ClassicIaaSMigration“.

## Häufig gestellte Fragen

**Wirkt sich dieser Migrationsplan auf meine vorhandenen Dienste oder Anwendungen aus, die auf virtuellen Azure-Computern ausgeführt werden?**

Nein. Die virtuellen Computer (klassisch) sind vollständig unterstützte Dienste mit allgemeiner Verfügbarkeit. Sie können diese Ressourcen weiterhin verwenden, um Ihre Nutzung von Microsoft Azure zu erweitern.

**Was passiert mit meinen VMs, wenn ich für die nahe Zukunft keine Migration plane?**

Die vorhandenen klassischen APIs und das klassische Ressourcenmodell werden nicht eingestellt. In Anbetracht der erweiterten Features, die im Resource Manager-Bereitstellungsmodell zur Verfügung stehen, möchten wir die Migration möglichst einfach gestalten. Wir empfehlen Ihnen dringend, sich über [einige Weiterentwicklungen](virtual-machines-windows-compare-deployment-models.md) zu informieren, die Teil von IaaS unter Resource Manager sind.

**Was bedeutet dieser Migrationsplan für meine vorhandenen Tools?**

Die Aktualisierung Ihrer Tools auf das Resource Manager-Bereitstellungsmodell ist eine der wichtigsten Änderungen, die Sie in Ihren Migrationsplänen berücksichtigen sollten.

**Wie lange dauert die Downtime der Verwaltungsebene?**

Dies hängt davon ab, wie viele Ressourcen migriert werden. Bei kleineren Bereitstellungen (einige Dutzend virtueller Computer) sollte der gesamte Migrationsprozess weniger als eine Stunde dauern. Bei größeren Bereitstellungen (Hunderte virtueller Computer) kann die Migration einige Stunden dauern. Da sich der Dienst in der öffentlichen Vorschauphase befindet, empfehlen wir Ihnen dringend, ihn unter Ihrem Entwicklungs- oder Testabonnement auszuführen, um die Auswirkungen auszuwerten.

**Kann ich einen Rollback durchführen, nachdem für meine migrierten Ressourcen in Resource Manager ein Commit durchgeführt wurde?**

Sie können die Migration abbrechen, solange sich die Ressourcen im Zustand „Vorbereitet“ befinden. Der Rollback wird nicht unterstützt, nachdem die Ressourcen per Commitvorgang erfolgreich migriert wurden.

**Kann ich für meine Migration einen Rollback durchführen, wenn beim Commitvorgang ein Fehler auftritt?**

Sie können die Migration nicht abbrechen, wenn für den Commitvorgang ein Fehler auftritt. Alle Migrationsvorgänge, einschließlich des Commitvorgangs, sind idempotent. Daher wird empfohlen, den Vorgang nach einer kurzen Wartezeit zu wiederholen. Sollte weiterhin ein Fehler auftreten, erstellen Sie ein Supportticket, oder erstellen Sie einen Eintrag in unserem [VM-Forum](https://social.msdn.microsoft.com/Forums/azure/de-DE/home?forum=WAVirtualMachinesforWindows) mit dem Tag „ClassicIaaSMigration“.

**Muss ich eine weitere ExpressRoute-Verbindung erwerben, wenn ich IaaS unter Resource Manager verwenden muss?**

Nein. Wir haben vor Kurzem die [Koexistenz einer ExpressRoute-Verbindung für das klassische Bereitstellungsmodell und Resource Manager](../expressroute/expressroute-howto-coexist-resource-manager.md) ermöglicht. Sie müssen keine neue ExpressRoute-Verbindung erwerben, wenn Sie bereits eine besitzen.

**Was passiert, wenn ich für meine klassischen IaaS-Ressourcen Richtlinien für die rollenbasierte Zugriffssteuerung konfiguriert habe?**

Während der Migration wird für die Ressourcen die Transformation vom klassischen Bereitstellungsmodell zu Resource Manager durchgeführt. Es ist also ratsam, die Aktualisierungen der RBAC-Richtlinien zu planen, die nach der Migration durchgeführt werden müssen.

**Was passiert, wenn ich gegenwärtig Azure Site Recovery oder Azure Backup verwende?**

Die Unterstützung für Azure Site Recovery und Backup für virtuelle Computer unter Resource Manager wurde vor Kurzem hinzugefügt. Wir arbeiten an der Möglichkeit, auch die Migration virtueller Computer zu Resource Manager zu unterstützen. Derzeit empfehlen wir Ihnen, keine Migration durchzuführen, wenn Sie diese Funktionen nutzen.

**Kann ich mein Abonnement oder meine Ressourcen überprüfen, um zu ermitteln, ob sie für die Migration geeignet sind?**

Derzeit wird während des Vorbereitungsvorgangs eine implizite Überprüfung für die Ressourcen durchgeführt, die für die Migration vorbereitet werden. Bei der Option für die plattformgestützte Migration besteht der erste Schritt zum Vorbereiten der Migration darin, zu überprüfen, ob die Ressourcen für die Migration geeignet sind. Wenn die Überprüfung nicht erfolgreich ist, werden die Ressourcen nicht „angefasst“.

**Was passiert, wenn ein Kontingentfehler auftritt, während ich die IaaS-Ressourcen für die Migration vorbereite?**

Wir empfehlen Ihnen, die Migration abzubrechen und anschließend eine Supportanfrage zu erstellen, um die Kontingente in der Region zu erhöhen, zu der Sie die virtuellen Computer migrieren. Nachdem die Kontingentanfrage genehmigt wurde, können Sie wieder mit dem Ausführen der Migrationsschritte beginnen.

**Wie melde ich ein Problem?**

Posten Sie Ihre Probleme und Fragen zur Migration in unserem [VM-Forum](https://social.msdn.microsoft.com/Forums/azure/de-DE/home?forum=WAVirtualMachinesforWindows) unter dem Schlüsselwort „ClassicIaaSMigration“. Wir empfehlen, all Ihre Fragen in diesem Forum zu posten. Wenn Sie einen Supportvertrag haben, können Sie auch gerne ein Supportticket erstellen.

**Was passiert, wenn mir die Namen der Ressourcen nicht gefallen, die von der Plattform während der Migration ausgewählt wurden?**

Für alle Ressourcen, für die Sie unter dem klassischen Bereitstellungsmodell explizit Namen angeben, werden diese während der Migration beibehalten. In einigen Fällen werden neue Ressourcen erstellt. Beispiel: Für jeden virtuellen Computer wird eine Netzwerkschnittstelle erstellt. Derzeit wird es nicht unterstützt, die Namen dieser neuen Ressourcen, die während der Migration erstellt werden, zu steuern. Besuchen Sie das [Azure-Feedbackforum](http://feedback.azure.com), um für dieses Feature abzustimmen.


## Nächste Schritte
Nachdem Sie nun eine Vorstellung von der Migration klassischer IaaS-Ressourcen zu Resource Manager haben, können Sie damit beginnen, die Ressourcen zu migrieren.

- [Ausführliche technische Informationen zur plattformgestützten Migration vom klassischen Bereitstellungsmodell zu Azure Resource Manager](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
- [Migrieren von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe von PowerShell](virtual-machines-windows-ps-migration-classic-resource-manager.md)
- [Migrieren von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe der Befehlszeilenschnittstelle](virtual-machines-linux-cli-migration-classic-resource-manager.md)
- [Klonen eines klassischen virtuellen Computers nach Azure Resource Manager mithilfe von PowerShell-Skripts aus der Community](virtual-machines-windows-migration-scripts.md)

<!---HONumber=AcomDC_0615_2016-->