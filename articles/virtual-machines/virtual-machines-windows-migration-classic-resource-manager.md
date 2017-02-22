---
title: "Migrieren klassischer Ressourcen zu Azure Resource Manager – Übersicht | Microsoft-Dokumentation"
description: "In diesem Artikel wird die plattformgestützte Migration von Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager erläutert."
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 78492a2c-2694-4023-a7b8-c97d3708dcb7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: kasing
translationtype: Human Translation
ms.sourcegitcommit: 2c96a3ca5fd72a4a3c992206aeb93f201342dd6a
ms.openlocfilehash: aafaacea59c2c7fc463fb84207417d2c4e1d81ff


---
# <a name="platform-supported-migration-of-iaas-resources-from-classic-to-azure-resource-manager"></a>Plattformgestützte Migration von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager
In diesem Artikel erfahren Sie, wie wir die Migration von IaaS-Ressourcen (Infrastructure-as-a-Service) vom klassischen zum Resource Manager-Bereitstellungsmodell ermöglichen. Informieren Sie sich weiter über [Features und Vorteile von Azure Resource Manager](../azure-resource-manager/resource-group-overview.md). Es wird ausführlich beschrieben, wie Sie mit den beiden in Ihrem Abonnement parallel vorhandenen Bereitstellungsmodellen Verbindungen mit Ressourcen herstellen, indem Sie Standort-zu-Standort-Gateways für virtuelle Netzwerke verwenden.

## <a name="goal-for-migration"></a>Ziel der Migration
Resource Manager ermöglicht die Bereitstellung komplexer Anwendungen über Vorlagen, konfiguriert virtuelle Computer mit VM-Erweiterungen und bietet Möglichkeiten zur Zugriffsverwaltung und Markierung. Azure Resource Manager umfasst die skalierbare, parallele Bereitstellung für virtuelle Computer in Verfügbarkeitsgruppen. Das neue Bereitstellungsmodell bietet auch die unabhängige Lebenszyklusverwaltung der Bereiche Compute, Netzwerk und Speicher. Zudem liegt ein Schwerpunkt auf der standardmäßigen Sicherstellung der Sicherheit, indem virtuelle Computer in einem virtuellen Netzwerk verwendet werden.

Nahezu alle Funktionen des klassischen Bereitstellungsmodells werden für die Bereiche Compute, Netzwerk und Speicher unter Azure Resource Manager unterstützt. Um die neuen Funktionen in Azure Resource Manager zu nutzen, können Sie vorhandene Bereitstellungen aus dem klassischen Bereitstellungsmodell migrieren.

## <a name="changes-to-your-automation-and-tooling-after-migration"></a>Änderungen an der Automatisierung und an den Tools nach der Migration
Im Rahmen der Migration Ihrer Ressourcen vom klassischen zum Resource Manager-Bereitstellungsmodell müssen Sie Ihre bestehende Automatisierung oder die bestehenden Tools aktualisieren, um sicherzustellen, dass sie nach der Migration weiterhin funktionieren.

## <a name="meaning-of-migration-of-iaas-resources-from-classic-to-resource-manager"></a>Die Bedeutung der Migration von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Resource Manager
Bevor wir näher auf die Details eingehen, betrachten wir den Unterschied zwischen Datenebenen- und Verwaltungsebenenvorgängen bei IaaS-Ressourcen.

* *Verwaltungsebene* : Beschreibt die Aufrufe, die auf der Verwaltungsebene oder in der API zur Änderung von Ressourcen eingehen. Beispielsweise werden ausgeführte Ressourcen durch Vorgänge verwaltet wie das Erstellen eines virtuellen Computers, das Neustarten eines virtuellen Computers und das Aktualisieren eines virtuellen Netzwerks mit einem neuen Subnetz. Die Verbindung zu den Instanzen ist davon nicht direkt betroffen.
* *Datenebene* (Anwendung): Beschreibt die Laufzeit der Anwendung selbst und umfasst die Interaktion mit Instanzen, die nicht über die Azure-API verlaufen. Das Zugreifen auf Ihre Website oder das Abrufen von Daten von einer ausgeführten SQL Server-Instanz oder einem MongoDB-Server wird als Interaktion mit der Datenebene bzw. der Anwendung angesehen. Das Kopieren eines Blobs aus einem Speicherkonto und das Zugreifen auf eine öffentliche IP-Adresse, um per RDP oder SSH eine Verbindung mit dem virtuellen Computer herzustellen, sind ebenfalls Vorgänge auf der Datenebene. Mit diesen Vorgängen wird die Ausführung der Anwendung für die Bereiche Compute, Netzwerk und Speicher sichergestellt.

> [!NOTE]
> In einigen Migrationsszenarien hält die Azure-Plattform die virtuellen Computer an, hebt ihre Zuordnung auf und startet sie neu. Dies führt zu einer kurzen Downtime auf der Datenebene.
>
>

## <a name="supported-scopes-of-migration"></a>Unterstützte Migrationsbereiche
Es gibt drei Migrationsbereiche, bei denen es hauptsächlich um Compute-, Netzwerk- und Speicherressourcen geht.

### <a name="migration-of-virtual-machines-not-in-a-virtual-network"></a>Migration virtueller Computer (nicht in einem virtuellen Netzwerk)
Beim Resource Manager-Bereitstellungsmodell wird die Sicherheit Ihrer Anwendungen standardmäßig erzwungen. Beim Resource Manager-Modell müssen sich alle virtuellen Computer in einem virtuellen Netzwerk befinden. Die Azure-Plattform startet die virtuellen Computer im Rahmen der Migration neu (`Stop`, `Deallocate` und `Start`). Für die virtuellen Netzwerke haben Sie zwei Optionen:

* Sie können für die Plattform die Erstellung eines neuen virtuellen Netzwerks anfordern und den virtuellen Computer zum neuen virtuellen Netzwerk migrieren.
* Sie können den virtuellen Computer zu einem vorhandenen virtuellen Netzwerk in Resource Manager migrieren.

> [!NOTE]
> In diesem Migrationsbereich sind die Vorgänge auf Verwaltungsebene und Datenebene für einen Zeitraum während der Migration unter Umständen nicht zulässig.
>
>

### <a name="migration-of-virtual-machines-in-a-virtual-network"></a>Migration virtueller Computer (in einem virtuellen Netzwerk)
Bei den meisten VM-Konfigurationen werden nur die Metadaten zwischen dem klassischen und dem Resource Manager-Bereitstellungsmodell migriert. Die zugrunde liegenden virtuellen Computer werden auf derselben Hardware, in demselben Netzwerk und mit demselben Speicher ausgeführt. Vorgänge auf Verwaltungsebene sind während der Migration unter Umständen für einen bestimmten Zeitraum nicht zulässig. Die Datenebene funktioniert jedoch weiterhin. Dies bedeutet, dass für Ihre Anwendungen, die auf virtuellen Computern ausgeführt werden (klassisch), während der Migration keine Downtime auftritt.

Die folgenden Konfigurationen werden derzeit nicht unterstützt: Wenn die Unterstützung später hinzukommt, kann für einige virtuelle Computer in dieser Konfiguration eine Downtime auftreten (VM-Vorgänge zum Beenden, Aufheben der Zuordnung und Neustart werden durchlaufen).

* Verwendung mehrerer Verfügbarkeitsgruppen in einem Clouddienst.
* Verwendung von mindestens einer Verfügbarkeitsgruppe und von virtuellen Computern, die keiner Verfügbarkeitsgruppe angehören, in einem einzigen Clouddienst.

> [!NOTE]
> In diesem Migrationsbereich ist die Verwaltungsebene für einen Zeitraum während der Migration unter Umständen nicht zulässig. Für bestimmte Konfigurationen kann, wie zuvor beschrieben, eine Downtime der Datenebene auftreten.
>
>

### <a name="storage-accounts-migration"></a>Migration von Speicherkonten
Um eine nahtlose Migration zu ermöglichen, können Sie Resource Manager-VMs in einem klassischen Speicherkonto bereitstellen. Mit dieser Funktion können und sollen Compute- und Netzwerkressourcen unabhängig von Speicherkonten migriert werden. Sobald Sie Ihre virtuellen Computer und das virtuelle Netzwerk migrieren, müssen Sie Ihre Speicherkonten migrieren, um den Migrationsprozess abzuschließen.

> [!NOTE]
> Beim Resource Manager-Bereitstellungsmodell gilt das Konzept von klassischen Images und Datenträgern nicht. Wenn das Speicherkonto migriert wird, sind klassische Images und Datenträger im Resource Manager-Stapel nicht sichtbar, die Sicherungs-VHDs bleiben jedoch im Speicherkonto.
>
>

## <a name="unsupported-features-and-configurations"></a>Nicht unterstützte Features und Konfigurationen
Derzeit werden einige Features und Konfigurationen nicht unterstützt. In den folgenden Abschnitten wird beschrieben, was wir diesbezüglich empfehlen.

### <a name="unsupported-features"></a>Nicht unterstützte Funktionen
Die folgenden Funktionen werden derzeit nicht unterstützt: Optional können Sie diese Einstellungen entfernen, die virtuellen Computer migrieren und die Einstellungen dann im Resource Manager-Bereitstellungsmodell wieder aktivieren.

| Ressourcenanbieter | Feature |
| --- | --- |
| Compute |Nicht zugeordnete VM-Datenträger. |
| Compute |VM-Images. |
| Netzwerk |Endpunkt-ACLs. |
| Netzwerk |Gateways des virtuellen Netzwerks (Azure ExpressRoute-Gateways, Anwendungsgateway). |
| Netzwerk |Virtuelle Netzwerke mit VNet-Peering. (Migrieren von VNet zu ARM, dann Peering.) Erfahren Sie mehr über das [VNet-Peering](../virtual-network/virtual-network-peering-overview.md). |
| Netzwerk |Traffic Manager-Profile. |

### <a name="unsupported-configurations"></a>Nicht unterstützte Konfigurationen
Die folgenden Konfigurationen werden derzeit nicht unterstützt:

| Dienst | Konfiguration | Empfehlung |
| --- | --- | --- |
| Ressourcen-Manager |Rollenbasierte Zugriffssteuerung (RBAC) für klassische Ressourcen |Da der URI der Ressourcen nach der Migration geändert wird, sollten Sie die RBAC-Richtlinienaktualisierungen planen, die nach der Migration ausgeführt werden müssen. |
| Compute |Mehrere Subnetze, die einem virtuellen Computer zugeordnet sind |Aktualisieren Sie die Subnetzkonfiguration so, dass nur auf Subnetze verwiesen wird. |
| Compute |Virtuelle Computer, die zu einem virtuellen Netzwerk gehören, aber denen kein explizites Subnetz zugewiesen ist |Sie können die VM optional löschen. |
| Compute |Virtuelle Computer mit Warnungen, Richtlinien für automatische Skalierung |Die Migration wird durchlaufen, und diese Einstellungen werden gelöscht. Wir empfehlen Ihnen dringend, vor der Migration Ihre Umgebung auszuwerten. Alternativ hierzu können Sie die Warnungseinstellungen nach Abschluss der Migration neu konfigurieren. |
| Compute |XML-VM-Erweiterungen (BGInfo 1.*, Visual Studio Debugger, Web Deploy und Remotedebuggen) |Dies wird nicht unterstützt. Es wird empfohlen, dass Sie diese Erweiterungen vom virtuellen Computer entfernen, um die Migration fortzusetzen. Andernfalls werden sie während der Migration automatisch gelöscht. |
| Compute |Startdiagnose mit Storage Premium |Deaktivieren Sie die Funktion „Startdiagnose“ für die virtuellen Computer, bevor Sie mit der Migration fortfahren. Sobald die Migration abgeschlossen ist, können Sie die Startdiagnose im Resource Manager-Stapel erneut aktivieren. Darüber hinaus sollten für Screenshots und serielle Protokolle verwendete Blobs gelöscht werden, damit diese Blobs nicht mehr berechnet werden. |
| Compute |Clouddienste, die Web-/Workerrollen enthalten |Dies wird derzeit nicht unterstützt. |
| Netzwerk |Virtuelle Netzwerke, die virtuelle Computer und Web-/Workerrollen enthalten |Dies wird derzeit nicht unterstützt. |
| Azure App Service |Virtuelle Netzwerke, die App Service-Umgebungen enthalten |Dies wird derzeit nicht unterstützt. |
| Azure HDInsight |Virtuelle Netzwerke, die HDInsight-Dienste enthalten |Dies wird derzeit nicht unterstützt. |
| Microsoft Dynamics Lifecycle Services |Virtuelle Netzwerke, die virtuelle Computer enthalten, die von Dynamics Lifecycle Services verwaltet werden |Dies wird derzeit nicht unterstützt. |
| Azure AD Domain Services |Virtuelle Netzwerke, die Azure AD Domain Services enthalten |Dies wird derzeit nicht unterstützt. |
| Compute |Azure Security Center-Erweiterungen mit einem VNET, das ein VPN-Gateway mit Transitkonnektivität oder ein ExpressRoute-Gateway mit lokalem DNS-Server aufweist |Azure Security Center installiert die Erweiterungen automatisch auf Ihren virtuellen Computern, um deren Sicherheit zu überwachen und Warnungen auszulösen. Diese Erweiterungen werden in der Regel automatisch installiert, wenn die Azure Security Center-Richtlinie für das Abonnement aktiviert ist. Eine Migration von ExpressRoute-Gateways wird aktuell nicht unterstützt, und VPN-Gateways mit Transitkonnektivität verlieren den lokalen Zugriff. Durch das Löschen eines ExpressRoute-Gateways oder das Migrieren eines VPN-Gateways mit Transitkonnektivität geht der Internetzugriff für das VM-Speicherkonto verloren, wenn ein Commit für die Migration ausgeführt wird. Die Migration wird nicht fortgesetzt, wenn dies geschieht, da das Statusblob des Gast-Agents nicht aufgefüllt werden kann. Es wird empfohlen, die Azure Security Center-Richtlinie für das Abonnement für 3 Stunden zu deaktivieren, bevor Sie mit der Migration fortfahren. |

## <a name="the-migration-experience"></a>Migrationsvorgang
Wir empfehlen Ihnen Folgendes, bevor Sie mit dem Migrationsvorgang beginnen:

* Stellen Sie sicher, dass die Ressourcen, die Sie migrieren möchten, keine nicht unterstützten Features oder Konfigurationen verwenden. Gewöhnlich erkennt die Plattform diese Probleme und generiert einen Fehler.
* Wenn Sie über virtuelle Computer verfügen, die nicht in einem virtuellen Netzwerk enthalten sind, werden diese während der Vorbereitung beendet, und die Zuordnung wird aufgehoben. Wenn Sie die öffentliche IP-Adresse nicht verlieren möchten, sollten Sie sich die Informationen zum Reservieren der IP-Adresse ansehen, bevor Sie den Vorbereitungsvorgang beginnen. Wenn sich die virtuellen Computer aber in einem virtuellen Netzwerk befinden, werden sie nicht beendet, und die Zuordnung wird nicht aufgehoben.
* Planen Sie die Migration für einen Zeitraum außerhalb der Geschäftszeiten, um Raum für unerwartete Fehler zu lassen, die während der Migration unter Umständen auftreten können.
* Laden Sie die aktuelle Konfiguration Ihrer virtuellen Computer mit PowerShell, CLI-Befehlen (Befehlszeilenschnittstelle) oder REST-APIs herunter, um die Überprüfung zu vereinfachen, nachdem der Vorbereitungsschritt abgeschlossen ist.
* Aktualisieren Sie Ihre Skripts für die Automatisierung und Operationalisierung, um sie an das Resource Manager-Bereitstellungsmodell anzupassen, bevor Sie die Migration starten. Sie können optional GET-Vorgänge durchführen, wenn sich die Ressourcen im Zustand „Vorbereitet“ befinden.
* Werten Sie die RBAC-Richtlinien aus, die für die klassischen IaaS-Ressourcen konfiguriert sind, und stellen Sie einen Plan für die Vorgehensweise nach dem Abschluss der Migration auf.

Der Migrationsworkflow sieht wie folgt aus:

![Screenshot zum Migrationsworkflow](./media/virtual-machines-windows-migration-classic-resource-manager/migration-workflow.png)

> [!NOTE]
> Alle in den folgenden Abschnitten beschriebenen Vorgänge sind idempotent. Sollte ein Problem auftreten, das nicht auf ein nicht unterstütztes Feature oder auf einen Konfigurationsfehler zurückzuführen ist, wiederholen Sie den Vorbereitungs-, Abbruch- oder Commitvorgang. Die Azure-Plattform versucht erneut, die Aktion auszuführen.
>
>

### <a name="validate"></a>Überprüfen
Der Überprüfungsvorgang ist der erste Schritt im Migrationsprozess. Das Ziel dieses Schritts ist, im Hintergrund die Daten für die zu migrierenden Ressourcen zu analysieren und durch „Erfolgreich“/„Fehler“ anzugeben, ob die Ressourcen für die Migration bereit sind.

Sie wählen das virtuelle Netzwerk oder den gehosteten Dienst aus (sofern es kein virtuelles Netzwerk ist), das bzw. den Sie für die Migration überprüfen möchten.

* Wenn die Migration für die Ressource nicht möglich ist, werden auf der Azure-Plattform alle Gründe angegeben, aus denen die Migration nicht unterstützt wird.

Beim Überprüfen von Speicherdiensten befindet sich das migrierte Konto in einer Ressourcengruppe mit dem gleichen Namen wie Ihr Speicherkonto (mit dem Zusatz „-Migrated“).  Beispiel: Wenn Ihr Speicherkonto „mystorage“ heißt, befindet sich die mit ARM aktivierte Ressource in einer Ressourcengruppe namens „mystorage-Migrated“ und enthält ein Speicherkonto mit dem Namen „mystorage“.

### <a name="prepare"></a>Vorbereiten
Der Vorbereitungsvorgang ist der zweite Schritt im Migrationsprozess. Das Ziel dieses Schritts besteht darin, die Transformation der IaaS-Ressourcen von Ressourcen des klassischen Bereitstellungsmodells zu Resource Manager-Ressourcen zu simulieren und diese zu Visualisierungszwecken nebeneinander darzustellen.

Sie wählen das virtuelle Netzwerk oder den gehosteten Dienst aus (sofern es kein virtuelles Netzwerk ist), das bzw. den Sie für die Migration vorbereiten möchten.

* Wenn die Migration für die Ressource nicht möglich ist, beendet die Azure-Plattform den Migrationsprozesses und gibt den Grund an, aus dem der Vorbereitungsvorgang nicht erfolgreich war.
* Wenn die Migration für die Ressource durchgeführt werden kann, sperrt die Azure-Plattform zuerst die Vorgänge der Verwaltungsebene für die zu migrierenden Ressourcen. Beispielsweise können Sie einem in der Migration befindlichen virtuellen Computer keinen Datenträger hinzuzufügen.

Die Azure-Plattform startet dann für die zu migrierenden Ressourcen die Migration der Metadaten vom klassischen Bereitstellungsmodell zu Resource Manager.

Nach Abschluss der Vorbereitung können Sie die Ressourcen sowohl im klassischen Bereitstellungsmodell als auch im Resource Manager-Bereitstellungsmodell visualisieren. Für jeden Clouddienst im klassischen Bereitstellungsmodell erstellt die Azure-Plattform einen Ressourcengruppennamen nach dem Muster `cloud-service-name>-migrated`.

> [!NOTE]
> Virtuelle Computer, die sich nicht in einem klassischen virtuellen Netzwerk befinden, erhalten in dieser Phase der Migration den Status „Beendet (Zuordnung aufgehoben)“.
>
>

### <a name="check-manual-or-scripted"></a>Überprüfung (manuell oder per Skript)
Im Überprüfungsschritt können Sie bei Bedarf die Konfiguration verwenden, die Sie zuvor heruntergeladen haben, um zu überprüfen, ob die Migration korrekt aussieht. Alternativ dazu können Sie sich auch beim Portal anmelden und Stichproben der Eigenschaften und Ressourcen durchführen, um sicherzustellen, dass die Migration der Metadaten in Ordnung ist.

Wenn Sie ein virtuelles Netzwerk migrieren, wird der Großteil der Konfiguration für virtuelle Computer nicht neu gestartet. Für die Anwendungen auf diesen VMs können Sie überprüfen, ob die Anwendung noch betriebsbereit ist und ausgeführt wird.

Sie können Ihre Skripts für Überwachung/Automatisierung und den Betrieb testen, um zu ermitteln, ob die VMs wie erwartet ausgeführt werden und ob die aktualisierten Skripts richtig funktionieren. GET-Vorgänge werden nur dann unterstützt, wenn sich die Ressourcen im Status „Vorbereitet“ befinden.

Es gibt kein festes Zeitfenster, vor dem Sie für die Migration ein Commit durchführen müssen. Sie können sich in diesem Zustand so viel Zeit wie nötig lassen. Die Verwaltungsebene ist aber für diese Ressourcen gesperrt, bis Sie entweder einen Abbruch (abort) oder ein Commit (commit) durchführen.

Falls Probleme auftreten, können Sie die Migration immer abbrechen und zurück zum klassischen Bereitstellungsmodell wechseln. Nach dem Wechsel zurück öffnet die Azure-Plattform die Vorgänge der Verwaltungsebene für die Ressourcen , damit Sie den normalen Betrieb für diese virtuellen Computer im klassischen Bereitstellungsmodell wieder aufnehmen können.

### <a name="abort"></a>Abbruch
Der Abbruch ist ein optionaler Schritt, mit dem Sie Ihre Änderungen auf das klassische Bereitstellungsmodell zurücksetzen und die Migration beenden können.

> [!NOTE]
> Dieser Vorgang kann nicht mehr ausgeführt werden, nachdem Sie den Commitvorgang ausgelöst haben.     
>
>

### <a name="commit"></a>Commit
Nach Abschluss der Überprüfung können Sie einen Commit für die Migration durchführen. Die Ressourcen werden nicht mehr im klassischen Bereitstellungsmodell angezeigt und stehen nur noch im Resource Manager-Bereitstellungsmodell zur Verfügung. Die migrierten Ressourcen können nur im neuen Portal verwaltet werden.

> [!NOTE]
> Dies ist ein idempotenter Vorgang. Wenn hierbei ein Fehler auftritt, sollten Sie den Vorgang wiederholen. Sollten weiterhin Fehler auftreten, erstellen Sie ein Supportticket, oder posten Sie unter Verwendung des Tags ClassicIaaSMigration in unserem [VM-Forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows).
>
>

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen
**Wirkt sich dieser Migrationsplan auf meine vorhandenen Dienste oder Anwendungen aus, die auf virtuellen Azure-Computern ausgeführt werden?**

Nein. Die virtuellen Computer (klassisch) sind vollständig unterstützte Dienste mit allgemeiner Verfügbarkeit. Sie können diese Ressourcen weiterhin verwenden, um Ihre Nutzung von Microsoft Azure zu erweitern.

**Was passiert mit meinen VMs, wenn ich für die nahe Zukunft keine Migration plane?**

Die vorhandenen klassischen APIs und das klassische Ressourcenmodell werden nicht eingestellt. In Anbetracht der erweiterten Features, die im Resource Manager-Bereitstellungsmodell zur Verfügung stehen, möchten wir die Migration möglichst einfach gestalten. Wir empfehlen Ihnen dringend, sich über [einige Weiterentwicklungen](../azure-resource-manager/resource-manager-deployment-model.md) zu informieren, die Teil von IaaS unter Resource Manager sind.

**Was bedeutet dieser Migrationsplan für meine vorhandenen Tools?**

Die Aktualisierung Ihrer Tools auf das Resource Manager-Bereitstellungsmodell ist eine der wichtigsten Änderungen, die Sie in Ihren Migrationsplänen berücksichtigen sollten.

**Wie lange dauert die Downtime der Verwaltungsebene?**

Dies hängt davon ab, wie viele Ressourcen migriert werden. Bei kleineren Bereitstellungen (einige Dutzend virtueller Computer) sollte der gesamte Migrationsprozess weniger als eine Stunde dauern. Bei größeren Bereitstellungen (Hunderte virtueller Computer) kann die Migration einige Stunden dauern.

**Kann ich einen Rollback durchführen, nachdem für meine migrierten Ressourcen in Resource Manager ein Commit durchgeführt wurde?**

Sie können die Migration abbrechen, solange sich die Ressourcen im Zustand „Vorbereitet“ befinden. Der Rollback wird nicht unterstützt, nachdem die Ressourcen per Commitvorgang erfolgreich migriert wurden.

**Kann ich für meine Migration einen Rollback durchführen, wenn beim Commitvorgang ein Fehler auftritt?**

Sie können die Migration nicht abbrechen, wenn für den Commitvorgang ein Fehler auftritt. Alle Migrationsvorgänge, einschließlich des Commitvorgangs, sind idempotent. Daher wird empfohlen, den Vorgang nach einer kurzen Wartezeit zu wiederholen. Sollten weiterhin Fehler auftreten, erstellen Sie ein Supportticket, oder posten Sie unter Verwendung des Tags ClassicIaaSMigration in unserem [VM-Forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows).

**Muss ich eine weitere ExpressRoute-Verbindung erwerben, wenn ich IaaS unter Resource Manager verwenden muss?**

Nein. Vor Kurzem haben wir das [Umstellen von ExpressRoute-Verbindungen vom klassischen Bereitstellungsmodell auf das Resource Manager-Bereitstellungsmodell](../expressroute/expressroute-move.md)ermöglicht. Sie müssen keine neue ExpressRoute-Verbindung erwerben, wenn Sie bereits eine besitzen.

**Was passiert, wenn ich für meine klassischen IaaS-Ressourcen Richtlinien für die rollenbasierte Zugriffssteuerung konfiguriert habe?**

Während der Migration wird für die Ressourcen die Transformation vom klassischen Bereitstellungsmodell zu Resource Manager durchgeführt. Es ist also ratsam, die Aktualisierungen der RBAC-Richtlinien zu planen, die nach der Migration durchgeführt werden müssen.

**Was passiert, wenn ich gegenwärtig Azure Site Recovery oder Azure Backup verwende?**

Informationen zum Migrieren des virtuellen Computers, den Sie für die Sicherung aktiviert haben, finden Sie unter [Ich habe meine klassischen virtuellen Computer in einem Sicherungstresor gesichert. Nun möchten ich meine virtuellen Computer vom klassischen Modus zum Resource Manager-Modus migrieren. Wie kann ich meine VMs im Recovery Services-Tresor sichern?](../backup/backup-azure-backup-ibiza-faq.md)Ich habe meine klassischen VMs im Sichern gesichert. Nun möchten ich meine virtuellen Computer vom klassischen Modus zum Resource Manager-Modus migrieren.  Wie kann ich sie in einem Recovery Services-Tresor sichern?

**Kann ich mein Abonnement oder meine Ressourcen überprüfen, um zu ermitteln, ob sie für die Migration geeignet sind?**

Ja. Bei der Option für die plattformgestützte Migration besteht der erste Schritt zum Vorbereiten der Migration darin, zu überprüfen, ob die Ressourcen für die Migration geeignet sind. Falls beim Überprüfungsvorgang ein Fehler auftritt, erhalten Sie alle Meldungen über alle Gründe, aus denen die Migration nicht abgeschlossen werden kann.

**Was passiert, wenn ein Kontingentfehler auftritt, während ich die IaaS-Ressourcen für die Migration vorbereite?**

Wir empfehlen Ihnen, die Migration abzubrechen und anschließend eine Supportanfrage zu erstellen, um die Kontingente in der Region zu erhöhen, zu der Sie die virtuellen Computer migrieren. Nachdem die Kontingentanfrage genehmigt wurde, können Sie wieder mit dem Ausführen der Migrationsschritte beginnen.

**Wie melde ich ein Problem?**

Posten Sie Ihre Probleme und Fragen zur Migration mit dem Schlüsselwort ClassicIaaSMigration in unserem [VM-Forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows). Wir empfehlen, all Ihre Fragen in diesem Forum zu posten. Wenn Sie einen Supportvertrag haben, können Sie auch gerne ein Supportticket erstellen.

**Was passiert, wenn mir die Namen der Ressourcen nicht gefallen, die von der Plattform während der Migration ausgewählt wurden?**

Alle Ressourcen, für die Sie unter dem klassischen Bereitstellungsmodell explizit Namen angeben, werden während der Migration beibehalten. In einigen Fällen werden neue Ressourcen erstellt. Beispiel: Für jeden virtuellen Computer wird eine Netzwerkschnittstelle erstellt. Derzeit wird das Steuern der Namen dieser neuen Ressourcen, die während der Migration erstellt werden, nicht unterstützt. Besuchen Sie das [Azure-Feedbackforum](http://feedback.azure.com), um für dieses Feature abzustimmen.

**Ich erhalte die Fehlermeldung *Die VM meldet den Gesamtstatus des Agents als ,Nicht bereit‘. Daher kann die VM nicht migriert werden. Stellen Sie sicher, dass der VM-Agent den Gesamtstatus des Agents als ,Bereit‘ meldet“* oder *„Die VM enthält eine Erweiterung, deren Status nicht von der VM gemeldet wird. Daher kann diese VM nicht migriert werden.“***

Diese Meldung wird empfangen, wenn der virtuelle Computer keine ausgehende Verbindung mit dem Internet aufweist. Der VM-Agent verwendet ausgehende Verbindungen, um das Azure-Speicherkonto für die Aktualisierung des Agent-Status alle fünf Minuten zu erreichen.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun eine Vorstellung von der Migration klassischer IaaS-Ressourcen zu Resource Manager haben, können Sie damit beginnen, die Ressourcen zu migrieren.

* [Ausführliche technische Informationen zur plattformgestützten Migration vom klassischen Bereitstellungsmodell zu Azure Resource Manager](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
* [Migrieren von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe von PowerShell](virtual-machines-windows-ps-migration-classic-resource-manager.md)
* [Migrieren von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe der Befehlszeilenschnittstelle](virtual-machines-linux-cli-migration-classic-resource-manager.md)
* [Klonen eines klassischen virtuellen Computers nach Azure Resource Manager mithilfe von PowerShell-Skripts aus der Community](virtual-machines-windows-migration-scripts.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Überprüfen der häufigsten Fehler bei der Migration](virtual-machines-migration-errors.md)



<!--HONumber=Jan17_HO4-->


