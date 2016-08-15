<properties
	pageTitle="Replizieren von virtuellen Hyper-V-Computern in VMM-Clouds an einen sekundären VMM-Standort mit dem Azure-Portal | Microsoft Azure"
	description="Es wird beschrieben, wie Sie Azure Site Recovery bereitstellen, um Replikation, Failover und Wiederherstellung von Hyper-V-VMs in VMM-Clouds an einen sekundären VMM-Standort mit dem Azure-Portal zu orchestrieren."
	services="site-recovery"
	documentationCenter=""
	authors="rayne-wiselman"
	manager="jwhit"
	editor=""/>

<tags
	ms.service="site-recovery"
	ms.workload="backup-recovery"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/26/2016"
	ms.author="raynew"/>

# Replizieren von virtuellen Hyper-V-Computern in VMM-Clouds an einen sekundären VMM-Standort mithilfe des Azure-Portals

> [AZURE.SELECTOR]
- [Azure-Portal](site-recovery-vmm-to-vmm.md)
- [Klassisches Portal](site-recovery-vmm-to-vmm-classic.md)
- [PowerShell – Resource Manager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)

Willkommen bei Azure Site Recovery! Verwenden Sie die Informationen in diesem Artikel, wenn Sie lokale virtuelle Hyper-V-Computer an einen sekundären Standort replizieren möchten, die in System Center Virtual Machine Manager-Clouds (VMM) verwaltet werden. In diesem Artikel wird beschrieben, wie die Replikation mithilfe von Azure Site Recovery im Azure-Portal eingerichtet werden kann.

> [AZURE.NOTE] Azure verfügt über zwei verschiedene [Bereitstellungsmodelle](../resource-manager-deployment-model.md) für das Erstellen und Verwenden von Ressourcen: Azure Resource Manager- und klassische Bereitstellung. Azure verfügt auch über zwei Portale: das klassische Azure-Portal mit Unterstützung des klassischen Bereitstellungsmodells und das Azure-Portal mit Unterstützung für beide Bereitstellungsmodelle.


Azure Site Recovery im Azure-Portal verfügt über eine Reihe neuer Features:

- Im Azure-Portal wurden die Dienste Azure Backup und Azure Site Recovery in einem Recovery Services-Tresor kombiniert, damit Sie die Geschäftskontinuität und Notfallwiederherstellung zentral verwalten können. Dank eines einheitlichen Dashboards können Sie alle Vorgänge an Ihren lokalen Standorten und in der öffentlichen Azure-Cloud überwachen und verwalten.
- Benutzer mit Azure-Abonnements, die über das Programm für Cloudlösungsanbieter (Cloud Solution Provider, CSP) bereitgestellt wurden, können Site Recovery-Vorgänge jetzt im Azure-Portal verwalten.


Nachdem Sie diesen Artikel gelesen haben, können Sie unten im Disqus-Kommentarbereich Ihre Kommentare eingeben. Im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) können Sie technische Fragen stellen.


## Übersicht

Organisationen benötigen eine Strategie für die Geschäftskontinuität und Notfallwiederherstellung (BCDR), mit der bestimmt wird, wie Apps, Workloads und Daten bei geplanten und ungeplanten Ausfällen verfügbar bleiben und die normalen Arbeitsbedingungen so schnell wie möglich wiederhergestellt werden können. Bei Ihrer BCDR-Strategie sollten Sie darauf achten, dass Unternehmensdaten geschützt sind und wiederhergestellt werden können. Stellen Sie außerdem sicher, dass Workloads auch nach dem Eintreten eines Notfalls ständig verfügbar sind.

Site Recovery ist ein Azure-Dienst, der einen Beitrag zu Ihrer BCDR-Strategie leistet, indem die Replikation von lokalen physischen Servern und virtuellen Maschinen in die Cloud (Azure) oder in ein sekundäres Rechenzentrum orchestriert wird. Wenn es an Ihrem primären Standort zu Ausfällen kommt, wird ein Failover zum sekundären Standort durchgeführt, um die Verfügbarkeit von Apps und Workloads zu erhalten. Wenn wieder Normalbetrieb herrscht, führen Sie das Failback zum primären Standort durch. Weitere Informationen finden Sie unter [Was ist Azure Site Recovery?](site-recovery-overview.md).

Dieser Artikel enthält alle Informationen, die Sie zum Replizieren von lokalen Hyper-V-VMs in VMM-Clouds an einen sekundären VMM-Standort benötigen. Er enthält eine Übersicht über die Architektur, Informationen zur Planung und Bereitstellungsschritte zum Konfigurieren von lokalen Servern, Replikationseinstellungen und der Kapazitätsplanung. Nachdem Sie die Infrastruktur eingerichtet haben, können Sie die Replikation auf den zu schützenden Computern aktivieren und überprüfen, ob das Failover funktioniert.

## Vorteile für Unternehmen

- Site Recovery bietet Schutz von geschäftlichen Workloads und Anwendungen, die auf Hyper-V-VMs ausgeführt werden, indem sie nach sekundären Hyper-V-Servern repliziert werden.
- Das Recovery Services-Portal ist ein zentraler Ort zum Einrichten, Verwalten und Überwachen von Replikation, Failover und Wiederherstellung.
- Sie können problemlos von Ihrer primären lokalen Infrastruktur aus Failovers zum sekundären Standort und Failbacks (Wiederherstellungen) vom sekundären an den primären Standort ausführen.
- Sie können Wiederherstellungspläne mit mehreren Computern konfigurieren, damit Failover für Workloads von Anwendungen mit mehreren Ebenen zusammen erfolgen.

## Szenarioarchitektur

Dies sind die Szenariokomponenten:

- **Primärer Standort**: Am primären Standort führen ein oder mehrere Hyper-V-Hostserver Quell-VMs aus, die Sie replizieren möchten. Diese primären Hostserver befinden sich in einer privaten VMM-Cloud.
- **Sekundärer Standort**: Am sekundären Standort führen ein oder mehrere Hyper-V-Hostserver Ziel-VMs aus, an die Sie die primären virtuellen Computer replizieren möchten. Diese Hostserver befinden sich in einer privaten VMM-Cloud. Die Cloud kann sich auf dem primären Server (wenn Sie nur einen einzelnen VMM-Server besitzen) oder auf einem sekundären VMM-Server befinden.
- **Anbieter**: Während der Site Recovery-Bereitstellung installieren Sie den Azure Site Recovery-Anbieter auf den VMM-Servern und registrieren diese Server bei einem Recovery Services-Tresor. Der Anbieter auf dem VMM-Server kommuniziert mit Site Recovery über HTTPS 443, um die Orchestrierung zu replizieren. Die Datenreplikation erfolgt zwischen primären und sekundären Hyper-V-Hostservern. Replizierte Daten bleiben an lokalen Standorten und Netzwerken und werden nicht an Azure gesendet. Hier finden Sie weitere Informationen zum [Datenschutz](#privacy-information-for-site-recovery).

![E2E-Topologie](./media/site-recovery-vmm-to-vmm/architecture.png)

### Übersicht zum Datenschutz

In dieser Tabelle ist zusammengefasst, wie Daten in diesem Szenario gespeichert werden:
****
Aktion | **Details** | **Gesammelte Daten** | **Verwenden Sie** | **Erforderlich** 
--- | --- | --- | --- | ---
**Registrierung** | Sie erstellen und registrieren einen VMM-Server in einem Recovery Services-Tresor. Wenn Sie die Registrierung eines Servers später aufheben möchten, können Sie dafür die Serverinformationen aus dem Azure-Portal löschen. | Nach der Registrierung eines VMM-Servers erfasst Site Recovery Metadaten über den VMM-Server und die Namen der von Site Recovery erkannten VMM-Clouds, verarbeitet und überträgt sie. | Die Daten dienen zum Identifizieren und zum Kommunizieren mit dem entsprechenden VMM-Server sowie zum Konfigurieren der Einstellungen für die entsprechenden VMM-Clouds. | Dieses Feature ist erforderlich. Wenn Sie diese Informationen nicht an Site Recovery senden möchten, sollten Sie den Site Recovery-Dienst nicht verwenden. 
**Replikation aktivieren** | Der auf dem VMM-Server installierte Azure Site Recovery-Anbieter ist für die Kommunikation mit dem Site Recovery-Dienst zuständig. Der Anbieter ist eine Dynamic Link Library (DLL), die im VMM-Prozess gehostet wird. Nachdem der Anbieter installiert ist, wird die Funktion „Datacenter Recovery“ in der VMM-Administratorkonsole aktiviert. Neue und vorhandene virtuelle Computer können dieses Feature aktivieren, um den Schutz für einen virtuellen Computer zu aktivieren. | Wenn diese Eigenschaft festgelegt ist, sendet der Anbieter den Namen und die ID des virtuellen Computers an Site Recovery. Die Replikation wird vom Windows Server 2012- oder Windows Server 2012 R2-Hyper-V-Replikat aktiviert. Die Daten des virtuellen Computers werden von einem Hyper-V-Host auf einen anderen repliziert (der sich in der Regel in einem anderen "Recovery"-Rechenzentrum befindet). | Site Recovery verwendet die Metadaten zum Auffüllen der VM-Informationen im Azure-Portal. | Dieses Feature ist ein wesentlicher Bestandteil des Diensts und kann nicht deaktiviert werden. Wenn Sie diese Informationen nicht senden möchten, aktivieren Sie den Site Recovery-Schutz für virtuelle Computer nicht. Beachten Sie, dass alle Daten über HTTPS vom Anbieter an Site Recovery gesendet werden.
**Plan für die Wiederherstellung** | Pläne für die Wiederherstellung unterstützen Sie beim Erstellen eines Orchestrierungsplans für das Wiederherstellungs-Rechenzentrum. Sie können die Reihenfolge definieren, in der VMs oder eine Gruppe virtueller Computer am Wiederherstellungsstandort gestartet werden sollen. Außerdem können Sie die Ausführung beliebiger automatisierter Skripts oder manueller Aktionen angeben, die zum Zeitpunkt der Wiederherstellung für jede VM ausgeführt werden. Ein Failover wird in der Regel auf der Ebene des Wiederherstellungsplans ausgelöst, um eine koordinierte Wiederherstellung einzuleiten. | Site Recovery sammelt, verarbeitet und überträgt Metadaten für den Wiederherstellungsplan, einschließlich der Metadaten für virtuelle Computer sowie der Metadaten von Automatisierungsskripts und Hinweisen für manuelle Aktionen. | Die Metadaten werden verwendet, um den Wiederherstellungsplan im Azure-Portal zu erstellen. | Dieses Feature ist ein wesentlicher Bestandteil des Diensts und kann nicht deaktiviert werden. Wenn Sie diese Informationen nicht an Site Recovery senden möchten, erstellen Sie keine Wiederherstellungspläne.
**Netzwerkzuordnung** | Netzwerkinformationen aus dem primären Rechenzentrum werden dem Rechenzentrum für die Wiederherstellung zugeordnet. Wenn VMs am Wiederherstellungsstandort wiederhergestellt werden, hilft die Netzwerkzuordnung beim Herstellen der Netzwerkkonnektivität. | Site Recovery erfasst, verarbeitet und überträgt die Metadaten der logischen Netzwerke für die einzelnen Standorte (primär und Rechenzentrum). | Die Metadaten werden verwendet, um Netzwerkeinstellungen aufzufüllen, damit Sie die Netzwerkinformationen zuordnen können. | Dieses Feature ist ein wesentlicher Bestandteil des Diensts und kann nicht deaktiviert werden. Wenn Sie diese Informationen nicht an Site Recovery senden möchten, verwenden Sie die Netzwerkzuordnung nicht.
**Failover (geplant/ungeplant/Test)** | Bei einem Failover werden VMs in einem VMM-verwalteten Rechenzentrum offline genommen und dafür in einem anderen online geschaltet. Die Failoveraktion wird im Azure-Portal manuell ausgelöst. | Der Anbieter auf dem VMM-Server wird von Site Recovery über das Failoverereignis benachrichtigt und führt über VMM-Schnittstellen eine Failoveraktion auf dem Hyper-V-Host aus. Das tatsächliche Failover einer VM wird von einem Hyper-V-Host zu einem anderen ausgeführt und von Windows Server 2012 oder Windows Server 2012 R2 Hyper-V-Replikat abgewickelt. Nachdem das Failover abgeschlossen ist, sendet der Anbieter auf dem VMM-Server im Wiederherstellungs-Rechenzentrum Erfolgsinformationen an Site Recovery. | Site Recovery füllt anhand der gesendeten Informationen den Status der Informationen zur Failoveraktion im Azure-Portal auf. | Dieses Feature ist ein wesentlicher Bestandteil des Diensts und kann nicht deaktiviert werden. Wenn Sie diese Informationen nicht an Site Recovery senden möchten, verwenden Sie das Failover nicht.


## Voraussetzungen für Azure

In Azure benötigen Sie für die Bereitstellung dieses Szenarios Folgendes:

**Voraussetzungen** | **Details** 
--- | ---
**Azure**| Sie benötigen ein [Microsoft Azure](http://azure.microsoft.com/)-Konto. Für den Einstieg steht eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/) zur Verfügung. Erfahren Sie mehr über die [Preise für Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/). 


## Lokale Voraussetzungen

Folgendes benötigen Sie am primären und sekundären lokalen Standort, um dieses Szenario bereitzustellen:

**Voraussetzungen** | **Details** 
--- | ---
**VMM** | Wir empfehlen Ihnen, am primären und am sekundären Standort je einen VMM-Server bereitzustellen.<br/><br/> Eine [Replikation zwischen Clouds auf einem einzelnen VMM-Server](site-recovery-single-vmm.md) ist auch möglich. Zu diesem Zweck benötigen Sie mindestens zwei auf dem VMM-Server konfigurierte Clouds.<br/><br/> Auf den VMM-Servern sollte mindestens System Center 2012 SP1 mit den neuesten Updates ausgeführt werden.<br/><br/> Für jeden VMM-Server muss mindestens eine Cloud konfiguriert sein, und für alle Clouds muss das Hyper-V-Kapazitätsprofil festgelegt sein. <br/><br/>Clouds müssen eine oder mehrere VMM-Hostgruppen enthalten.<br/><br/>Weitere Informationen zum Einrichten von VMM-Clouds finden Sie in [Vorbereiten der Azure Site Recovery-Bereitstellung](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric) und [Walkthrough: Creating Private Clouds with System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx) (Exemplarische Vorgehensweise: Erstellen von privaten Clouds mit System Center 2012 SP1 VMM).<br/><br/> VMM-Server benötigen Zugriff auf das Internet. 
**Hyper-V** | Auf den Hyper-V-Servern muss mindestens Windows Server 2012 mit der Hyper-V-Rolle ausgeführt werden, und die neuesten Updates müssen installiert sein.<br/><br/> Ein Hyper-V-Server muss mindestens eine VM enthalten.<br/><br/> Hyper-V-Hostserver sollten in Hostgruppen in der primären sowie in der sekundären VMM-Cloud angeordnet sein.<br/><br/> Wenn Sie Hyper-V unter Windows Server 2012 R2 in einem Cluster ausführen, sollten Sie [Update 2961977](https://support.microsoft.com/kb/2961977) installieren.<br/><br/> Wenn Sie Hyper-V unter Windows Server 2012 in einem Cluster ausführen, beachten Sie, dass der Clusterbroker nicht automatisch erstellt wird, falls Sie einen auf statischen IP-Adressen basierenden Cluster haben. Sie müssen den Clusterbroker manuell konfigurieren. [Weitere Informationen](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx)
**Anbieter** | Während der Site Recovery-Bereitstellung installieren Sie den Azure Site Recovery-Anbieter auf VMM-Servern. Der Anbieter kommuniziert mit Azure Site Recovery über HTTPS 443, um die Replikation zu orchestrieren. Die Datenreplikation erfolgt über das LAN oder eine VPN-Verbindung zwischen dem primären und sekundären Hyper-V-Server.<br/><br/> Der auf dem VMM-Server ausgeführte Anbieter benötigt Zugriff auf folgende URLs: *.hypervrecoverymanager.windowsazure.com; *.accesscontrol.windows.net; *.backup.windowsazure.com; *.blob.core.windows.net; *.store.core.windows.net.<br/><br/> Ermöglichen Sie darüber hinaus die Firewallkommunikation der VMM-Server mit den [IP-Bereichen des Azure-Rechenzentrums](https://www.microsoft.com/download/confirmation.aspx?id=41653), und lassen Sie das HTTPS-Protokoll (443) zu.

## Vorbereiten der Bereitstellung

Für die Vorbereitung der Bereitstellung benötigen Sie Folgendes:

1. [Bereiten Sie den VMM-Server vor](#prepare-the-vmm-server), um die Site Recovery-Bereitstellung durchführen zu können.
2. [Bereiten Sie die Netzwerkzuordnung vor](#prepare-for-network-mapping). Richten Sie Netzwerke ein, damit Sie die Netzwerkzuordnung konfigurieren können.

### Vorbereiten des VMM-Servers

Vergewissern Sie sich, dass der VMM-Server die [Voraussetzungen](#on-premises-prerequisites) erfüllt und auf die angegebenen URLs zugreifen kann.


### Vorbereiten der Netzwerkzuordnung

Die Netzwerkzuordnung zwischen VMM-VM-Netzwerken auf dem primären und sekundären VMM-Server dient folgenden Zwecken:

- Optimales Platzieren von Replikat-VMs auf sekundären Hyper-V-Hosts nach dem Failover.
- Verbinden von Replikat-VMs mit entsprechenden VM-Netzwerken.
- Wenn Sie die Netzwerkzuordnung nicht konfigurieren, werden Replikat-VMs nach einem Failover nicht mit einem Netzwerk verbunden.
- Wenn Sie die Netzwerkzuordnung während der Site Recovery-Bereitstellung einrichten möchten, benötigen Sie Folgendes:

	- Stellen Sie sicher, dass VMs auf dem Hyper-V-Quellhostserver mit einem VMM-VM-Netzwerk verbunden sind. Dieses Netzwerk sollte mit einem logischen Netzwerk verbunden sein, das der Cloud zugeordnet ist.
	- Stellen Sie sicher, dass für die sekundäre Cloud, die Sie für die Wiederherstellung verwenden, ein entsprechendes VM-Netzwerk konfiguriert ist. Dieses VM-Netzwerk sollte mit einem logischen Netzwerk verbunden sein, das der sekundären Cloud zugeordnet ist.

- Weitere Informationen zur Funktionsweise der Netzwerkzuordnung finden Sie [hier](site-recovery-network-mapping.md).

## Vorbereiten der Bereitstellung mit einem einzelnen VMM-Server

Wenn Sie nur über einen einzelnen VMM-Server verfügen, können Sie virtuelle Computer in Hyper-V-Hosts in der VMM-Cloud nach [Azure](site-recovery-vmm-to-azure.md) oder in eine sekundäre VMM-Cloud replizieren. Sie sollten die erste Option wählen, weil die Replikation zwischen Clouds nicht nahtlos ist, aber wenn Sie dies trotzdem tun müssen, gehen Sie folgendermaßen vor:

1. **Richten Sie VMM auf einem virtuellen Hyper-V-Computer ein**. Sie sollten auf der gleichen VM die von VMM verwendete SQL Server-Instanz installieren. Dies spart Zeit, da nur ein virtueller Computer erstellt werden muss. Wenn Sie eine Remoteinstanz von SQL Server verwenden möchten und ein Ausfall auftritt, müssen Sie diese Instanz vor dem Wiederherstellen von VMM wiederherstellen.
2. **Stellen Sie sicher, dass für den VMM-Server mindestens zwei Clouds konfiguriert sind**. Eine Cloud enthält die virtuellen Computer, die repliziert werden sollen, und die andere Cloud dient als sekundärer Standort. Die Cloud mit den virtuellen Computern, die Sie schützen möchten, muss bestimmte [Voraussetzungen](#on-premises-prerequisites) erfüllen.
3. Richten Sie Site Recovery wie in diesem Artikel beschrieben ein. Erstellen und registrieren Sie den VMM-Server im Tresor, richten Sie eine Replikationsrichtlinie ein, und aktivieren Sie die Replikation. Sie sollten festlegen, dass die anfängliche Replikation über das Netzwerk erfolgt.
4. Beim Einrichten der Netzwerkzuordnung ordnen Sie das VM-Netzwerk für die primäre Cloud dem VM-Netzwerk für die sekundäre Cloud zu.
5. Aktivieren Sie in der Hyper-V-Manager-Konsole Hyper-V-Replikat auf dem Hyper-V-Host, der die VMM-VM enthält, und aktivieren Sie die Replikation auf dem virtuellen Computer. Fügen Sie die VMM-VM nicht zu Clouds hinzu, die von Site Recovery geschützt werden, um sicherzustellen, dass Einstellungen von Hyper-V Replikat nicht durch Site Recovery außer Kraft gesetzt werden.
6. Wenn Sie Wiederherstellungspläne für das Failover erstellen, verwenden Sie den gleichen VMM-Server als Quelle und Ziel.
7. Sie führen Failover und Wiederherstellung wie folgt aus:

	- Führen Sie unter Verwendung von Hyper-V-Manager mit einem geplanten Failover ein manuelles Failover der VMM-VM zum sekundären Standort aus.
	- Führen Sie ein Failover der virtuellen Computer aus.
	- Melden Sie sich nach der Wiederherstellung der primären VMM-VM beim Azure-Portal > Recovery Services-Tresor an, und führen Sie ein nicht geplantes Failover der virtuellen Computer vom sekundären zum primären Standort aus.
	- Nach Abschluss des nicht geplanten Failovers können die Benutzer wieder vom primären Standort aus auf sämtliche Ressourcen zugreifen.


### Erstellen eines Recovery Services-Tresors

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf **Neu** > **Verwaltung** > **Recovery Services**. Alternativ können Sie auf **Durchsuchen** > **Recovery Services-Tresore** > **Hinzufügen** klicken.

	![Neuer Tresor](./media/site-recovery-vmm-to-vmm/new-vault3.png)

3. Geben Sie unter **Name** einen Anzeigenamen an, mit dem der Tresor identifiziert wird. Wenn Sie mehrere Abonnements haben, müssen Sie ein Abonnement auswählen.
4. [Erstellen Sie eine neue Ressourcengruppe](../resource-group-template-deploy-portal.md), oder wählen Sie eine vorhandene Ressourcengruppe aus, und geben Sie eine Azure-Region an. Computer werden in dieser Region repliziert. Sie finden eine Liste der unterstützten Regionen unter Geografische Verfügbarkeit auf der Seite [Azure Site Recovery – Preisübersicht](https://azure.microsoft.com/pricing/details/site-recovery/)
4. Wenn Sie schnell über das Dashboard auf den Tresor zugreifen möchten, klicken Sie auf **An Dashboard anheften** > **Tresor erstellen**.

	![Neuer Tresor](./media/site-recovery-vmm-to-vmm/new-vault-settings.png)

Der neue Tresor wird unter **Dashboard** > **Alle Ressourcen** und auf dem Hauptblatt **Recovery Services-Tresore** angezeigt.




## Erste Schritte

Site Recovery verfügt über eine Benutzeroberfläche für die ersten Schritte, damit Sie die Bereitstellung so schnell wie möglich durchführen können. Im Rahmen der ersten Schritte werden die Voraussetzungen überprüft, und Sie werden durch die Schritte der Site Recovery-Bereitstellung in der richtigen Reihenfolge geführt.

Sie wählen den Typ der Computer aus, die Sie replizieren möchten, und geben an, wo sie repliziert werden sollen. Sie richten lokale Server ein, erstellen Replikationsrichtlinien und führen die Kapazitätsplanung durch. Nachdem Sie Ihre Infrastruktur eingerichtet haben, aktivieren Sie die Replikation für VMs. Sie können Failover für bestimmte Computer ausführen oder Wiederherstellungspläne erstellen, um das Failover für mehrere Computer durchzuführen.

Beginnen Sie mit den ersten Schritten, indem Sie auswählen, wie Sie Site Recovery bereitstellen möchten. Der Ablauf der ersten Schritte kann je nach Replikationsanforderungen leicht variieren.

## Schritt 1: Auswählen der Schutzziele

Wählen Sie aus, was Sie replizieren möchten und wohin die Daten repliziert werden sollen.

1. Wählen Sie auf dem Blatt **Recovery Services-Tresore** Ihren Tresor aus, und klicken Sie auf **Einstellungen**.
2. Klicken Sie unter **Einstellungen** > **Erste Schritte** auf **Site Recovery** > **Schritt 1: Infrastruktur vorbereiten** > **Schutzziel**.
3. Wählen Sie unter **Schutzziel** die Option **Wiederherstellungsstandort** und dann **Ja, mit Hyper-V** aus.
4. Wählen Sie **Ja** aus, um anzugeben, dass Sie VMM verwenden, um die Hyper-V-Hosts zu verwalten, und wählen Sie **Ja** aus, wenn Sie über einen sekundären VMM-Server verfügen. Wenn Sie die Replikation zwischen Clouds auf einem einzelnen VMM-Server bereitstellen, klicken Sie auf **Nein**. Klicken Sie dann auf **OK**.

	![Ziele wählen](./media/site-recovery-vmm-to-vmm/choose-goals.png)


## Schritt 2: Einrichten der Quellumgebung

Installieren Sie den Azure Site Recovery-Anbieter auf den VMM-Servern, und registrieren Sie die Server im Tresor.

1. Klicken Sie auf **Schritt 2: Infrastruktur vorbereiten** > **Quelle**.

	![Quelle einrichten](./media/site-recovery-vmm-to-vmm/goals-source.png)

2. Klicken Sie unter **Quelle vorbereiten** auf **+ VMM**, um einen VMM-Server hinzuzufügen.

	![Quelle einrichten](./media/site-recovery-vmm-to-vmm/set-source1.png)

2. Vergewissern Sie sich auf dem Blatt **Server hinzufügen**, dass unter **Servertyp** der Eintrag **System Center-VMM-Server** angezeigt wird und dass der VMM-Server die [Voraussetzungen und URL-Anforderungen](#on-premises-prerequisites) erfüllt.
4. Laden Sie die Installationsdatei für den Azure Site Recovery-Anbieter herunter.
5. Laden Sie den Registrierungsschlüssel herunter. Sie benötigen diese Angaben beim Ausführen des Setups. Der Schlüssel ist nach der Erstellung fünf Tage lang gültig.

	![Quelle einrichten](./media/site-recovery-vmm-to-vmm/set-source3.png)

6. Installieren Sie den Azure Site Recovery-Anbieter auf dem VMM-Server.

> [AZURE.NOTE] Sie müssen nicht explizit etwas auf Hyper-V-Hostservern installieren.


### Einrichten des Azure Site Recovery-Anbieters

1. Führen Sie die Anbietersetupdatei auf jedem VMM-Server aus. Wenn VMM in einem Cluster bereitgestellt wird und Sie den Anbieter erstmals installieren, führen Sie die Installation auf einem aktiven Knoten durch und schließen Sie sie ab, um den VMM-Server im Tresor zu registrieren. Installieren Sie den Anbieter anschließend auf den anderen Knoten. Auf allen Clusterknoten sollte die gleiche Version des Anbieters ausgeführt werden.
2. Das Setup führt einige Voraussetzungsüberprüfungen durch und fordert die Zustimmung zum Beenden des VMM-Diensts an. Der VMM-Dienst wird nach Abschluss des Setups automatisch neu gestartet. Bei der Installation auf einem VMM-Cluster werden Sie aufgefordert, die Clusterrolle zu beenden.

2.  Unter **Microsoft Update** können Sie Updates aktivieren, damit Updates für den Anbieter gemäß Ihrer Microsoft Update-Richtlinie installiert werden.
3. Akzeptieren oder ändern Sie unter **Installation** den Standardspeicherort der Anbieterinstallation, und klicken Sie auf **Installieren**.

	![Installationspfad](./media/site-recovery-vmm-to-vmm/provider-location.png)

3. Klicken Sie nach Abschluss der Installation auf **Registrieren**, um den Server im Tresor zu registrieren.

	![Installationspfad](./media/site-recovery-vmm-to-vmm/provider-register.png)

9. Prüfen Sie unter **Tresorname** den Namen des Tresors, in dem der Server registriert wird. Klicken Sie auf *Weiter*.

	![Serverregistrierung](./media/site-recovery-vmm-to-vmm-classic/vaultcred.PNG)

7. Geben Sie auf der Seite **Internetverbindung** an, wie sich der Anbieter auf dem VMM-Server mit dem Internet verbinden soll. Wählen Sie **Mit vorhandenen Proxyeinstellungen verbinden** aus, um die Standard-Internetverbindungseinstellungen auf dem Server zu verwenden.

	![Interneteinstellungen](./media/site-recovery-vmm-to-vmm-classic/proxydetails.PNG)

	- Wenn Sie einen benutzerdefinierten Proxy verwenden möchten, sollten Sie diesen vor der Installation des Anbieters einrichten. Wenn Sie benutzerdefinierte Proxyeinstellungen konfigurieren, wird ein Test ausgeführt, um die Proxyverbindung zu überprüfen.
	- Wenn Sie einen benutzerdefinierten Proxy verwenden oder Ihr Standardproxy eine Authentifizierung verlangt, müssen Sie die Proxydetails einschließlich der Proxyadresse und des Ports eingeben.
	- Der VMM-Server und die Hyper-V-Hosts sollten auf die folgenden URLs Zugriff haben:
		- *.hypervrecoverymanager.windowsazure.com
		- *.accesscontrol.windows.net
		- *.backup.windowsazure.com
		- *.blob.core.windows.net
		- *.store.core.windows.net
	- Lassen Sie die unter [IP-Bereiche des Azure-Rechenzentrums](https://www.microsoft.com/download/confirmation.aspx?id=41653) angegebenen IP-Adressen sowie das HTTPS-Protokoll (443) zu. Fügen Sie die IP-Adressbereiche der zu verwendenden Azure-Region sowie die IP-Adressbereiche der westlichen USA einer Positivliste hinzu.
	- Wenn Sie einen benutzerdefinierten Proxy verwenden, wird ein ausführendes VMM-Konto (DRAProxyAccount) automatisch mit den angegebenen Proxyanmeldeinformationen erstellt. Konfigurieren Sie den Proxyserver so, dass dieses Konto erfolgreich authentifiziert werden kann. In der VMM-Konsole können die Einstellungen des ausführenden VMM-Kontos geändert werden. Zu diesem Zweck öffnen Sie den Arbeitsbereich **Einstellungen**, erweitern Sie **Sicherheit**, klicken Sie auf **Ausführende Konten**, und ändern Sie das Kennwort für DRAProxyAccount. Sie müssen den VMM-Dienst neu starten, damit diese Einstellung wirksam wird.


8. Wählen Sie unter **Registrierungsschlüssel** den Schlüssel aus, den Sie von Azure Site Recovery heruntergeladen und auf den VMM-Server kopiert haben.


10.  Die Verschlüsselungseinstellung wird nur verwendet, wenn Sie Hyper-V-VMs in VMM-Clouds in Azure replizieren. Bei der Replikation an einem sekundären Standort wird sie nicht verwendet.

11.  Geben Sie unter **Servername** einen Anzeigenamen ein, um den VMM-Server im Tresor zu identifizieren. Geben Sie in einer Clusterkonfiguration den Rollennamen des VMM-Clusters an.
12.  Wählen Sie unter **Cloudmetadaten synchronisieren** aus, ob Sie Metadaten für alle Clouds auf dem VMM-Server mit dem Tresor synchronisieren möchten. Diese Aktion muss für jeden VMM-Server nur einmal ausgeführt werden. Wenn Sie nicht alle Clouds synchronisieren möchten, können Sie diese Einstellung deaktiviert lassen und in den Cloudeigenschaften in der VMM-Konsole jede Cloud einzeln synchronisieren.

13.  Klicken Sie auf **Weiter**, um den Prozess abzuschließen. Nach der Registrierung werden die Metadaten vom VMM-Server von Azure Site Recovery abgerufen. Der Server wird im Tresor auf der Registerkarte **VMM-Server** der Seite **Server** angezeigt.
 	
	![Lastpage](./media/site-recovery-vmm-to-vmm-classic/provider13.PNG)

11. Sobald der Server in der Site Recovery-Konsole verfügbar ist, wählen Sie unter **Quelle** > **Quelle vorbereiten** den VMM-Server sowie die Cloud aus, in der sich der Hyper-V-Host befindet. Klicken Sie dann auf **OK**.

#### Installation über die Befehlszeile

Der Azure Site Recovery-Anbieter kann über die Befehlszeile installiert werden. Mit dieser Methode kann der Anbieter in Server Core für Windows Server 2012 R2 installiert werden.

1. Laden Sie die Installationsdatei und den Registrierungsschlüssel des Anbieters in einen Ordner herunter. Beispiel: C:\\ASR.
2. Beenden Sie den System Center Virtual Machine Manager-Dienst.
3. Extrahieren Sie über eine Eingabeaufforderung mit erhöhten Rechten das Installationsprogramm des Anbieters, indem Sie diese Befehle ausführen:

    	C:\Windows\System32> CD C:\ASR
    	C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q

4. Führen Sie diesen Befehl aus, um den Anbieter zu installieren:

    	C:\ASR> setupdr.exe /i

5. Führen Sie anschließend diese Befehle aus, um den Server im Tresor zu registrieren:

    	CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
    	C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>     

Die Parameter lauten:

 - **/Credentials**: erforderlicher Parameter zum Angeben des Speicherorts der Registrierungsschlüsseldatei.
 - **/FriendlyName**: erforderlicher Parameter für den Namen des Hyper-V-Hostservers, der im Azure Site Recovery-Portal angezeigt wird.
 - **/EncryptionEnabled**: Optionaler Parameter, den Sie nur bei der Replikation von VMM nach Azure verwenden.
 - **/proxyAddress**: optionaler Parameter, der die Adresse des Proxyservers angibt.
 - **/proxyport**: optionaler Parameter, der den Port des Proxyservers angibt.
 - **/proxyUsername**: optionaler Parameter, der den Proxybenutzernamen angibt (sofern der Proxy eine Authentifizierung erfordert).
 - **/proxyPassword**: optionaler Parameter, der das Kennwort für die Authentifizierung mit dem Proxyserver angibt (sofern der Proxy eine Authentifizierung erfordert).

## Schritt 3: Einrichten der Zielumgebung

Wählen Sie VMM-Server und Cloud aus.

1. Klicken Sie auf **Infrastruktur vorbereiten** > **Ziel**, und wählen Sie den gewünschten VMM-Zielserver aus.
2.	Clouds auf dem Server, die mit Site Recovery synchronisiert werden, werden angezeigt. Wählen Sie die Zielcloud aus.

	![Ziel](./media/site-recovery-vmm-to-vmm/target-vmm.png)

## Schritt 4: Einrichten der Replikationseinstellungen

1. Klicken Sie zum Erstellen einer neuen Replikationsrichtlinie auf **Infrastruktur vorbereiten** > **Replikationseinstellungen** > **+Erstellen und zuordnen**.

	![Netzwerk](./media/site-recovery-vmm-to-vmm/gs-replication.png)

2. Geben Sie unter **Richtlinie erstellen und zuordnen** einen Richtliniennamen an. Quell- und Zieltyp müssen jeweils **Hyper-V** lauten.
3. Wählen Sie unter **Hyper-V-Hostversion** aus, welches Betriebssystem auf dem Host ausgeführt wird.

	> [AZURE.NOTE] Die VMM-Cloud kann Hyper-V-Hosts enthalten, auf denen verschiedene (unterstützte) Versionen von Windows Server ausgeführt werden, doch eine Replikationsrichtlinie wird auf Hosts angewendet, die das gleiche Betriebssystem ausführen. Wenn Sie über Hosts verfügen, auf denen mehrere Betriebssystemversionen ausgeführt werden, erstellen Sie separate Replikationsrichtlinien.

4. Geben Sie unter **Authentifizierungstyp** und **Authentifizierungsport** an, wie der Datenverkehr zwischen dem primären und dem Hyper-V-Wiederherstellungs-Hostserver authentifiziert wird. Wählen Sie **Zertifikat** aus, sofern Sie nicht über eine funktionsfähige Kerberos-Umgebung verfügen. Azure Site Recovery konfiguriert automatisch Zertifikate für die HTTPS-Authentifizierung. Sie müssen gar nichts manuell durchführen. Standardmäßig werden die Ports 8083 und 8084 (für Zertifikate) in der Windows-Firewall auf den Hyper-V-Hostservern geöffnet. Wenn Sie **Kerberos** auswählen, wird ein Kerberos-Ticket für die gegenseitige Authentifizierung der Hostserver verwendet. Bitte beachten Sie, dass diese Einstellung nur für Hyper-V-Hostserver relevant ist, die unter Windows Server 2012 R2 laufen.
3. Geben Sie unter **Kopierhäufigkeit** an, wie oft Sie Deltadaten nach der ersten Replikation replizieren möchten (alle 30 Sekunden, nach 5 Minuten oder nach 15 Minuten).
4. Geben Sie unter **Aufbewahrungszeitraum des Wiederherstellungspunkts** das Aufbewahrungszeitfenster für die einzelnen Wiederherstellungspunkte in Stunden an. Geschützte Computer können innerhalb eines Zeitfensters an einem beliebigen Punkt wiederhergestellt werden.
6. Geben Sie unter **App-konsistente Momentaufnahmehäufigkeit** an, wie häufig (1 bis 12 Stunden) Wiederherstellungspunkte erstellt werden sollen, die anwendungskonsistente Momentaufnahmen enthalten. Hyper-V verwendet zwei Momentaufnahmen: eine Standard-Momentaufnahme, die eine inkrementelle Momentaufnahme des gesamten virtuellen Computers bereitstellt, und eine anwendungskonsistente Momentaufnahme, die eine Zeitpunkt-Momentaufnahme der Anwendungsdaten innerhalb des virtuellen Computers erfasst. Anwendungskonsistente Momentaufnahmen verwenden den Volumeschattenkopie-Dienst (Volume Shadow Copy Service, VSS), um sicherzustellen, dass Anwendungen sich bei der Erstellung der Momentaufnahme in einem konsistenten Zustand befinden. Beachten Sie, dass die Leistung von Anwendungen auf virtuellen Quellcomputern durch die Aktivierung anwendungskonsistenter Momentaufnahmen beeinträchtigt wird. Stellen Sie sicher, dass der festgelegte Wert kleiner als die konfigurierte Anzahl der zusätzlichen Wiederherstellungspunkte ist.
7. Geben Sie unter **Datenübertragungskomprimierung** an, ob replizierte Daten, die übertragen werden, komprimiert werden sollen.
8. Wählen Sie **Replikat-VM löschen** aus, um anzugeben, dass die Replikat-VM gelöscht werden sollte, wenn Sie den Schutz für die Quell-VM deaktivieren. Wenn Sie diese Einstellung aktivieren, und Sie deaktivieren den Schutz für die Quell-VMs, wird der virtuelle Computer aus der Site Recovery-Konsole entfernt, die Site Recovery-Einstellungen für die VMM werden aus der VMM-Konsole entfernt, und das Replikat wird gelöscht.
3. Wenn Sie über das Netzwerk replizieren, geben Sie unter **Methode für anfängliche Replikation** an, ob die erste Replikation sofort gestartet werden soll oder ob Sie einen Zeitplan festlegen möchten. Um Netzwerkbandbreite zu sparen, sollte der Zeitraum außerhalb Ihrer Spitzenzeiten liegen. Klicken Sie dann auf **OK**.

	![Replikationsrichtlinie](./media/site-recovery-vmm-to-vmm/gs-replication2.png)

6. Wenn Sie eine neue Richtlinie erstellen, wird sie der VMM-Cloud automatisch zugeordnet. Klicken Sie unter **Replikationsrichtlinie** auf **OK**. Sie können unter **Einstellungen** > **Replikation** > „<Richtlinienname>“ > **VMM-Cloud zuordnen** zusätzliche VMM-Clouds (und die darin enthaltenen VMs) zu dieser Replikationsrichtlinie zuordnen.

	![Replikationsrichtlinie](./media/site-recovery-vmm-to-vmm/policy-associate.png)

### Vorbereiten der ersten Offlinereplikation

Sie können eine Offlinereplikation für die erstmalige Datenkopie durchführen. Sie können dies wie folgt vorbereiten:

- Geben Sie auf dem Quellserver einen Pfad an, von dem der Datenexport stattfinden soll. Weisen Sie Vollzugriff für NTFS und Freigabeberechtigungen für den VMM-Dienst auf dem Exportpfad zu. Auf dem Zielserver geben Sie einen Pfad an, von dem der Datenimport stattfinden soll. Weisen Sie für diesen Importpfad die gleichen Berechtigungen zu.
- Wenn der Import- oder Exportpfad freigegeben ist, weisen Sie Mitgliedschaften in den Gruppen "Administratoren", "Hauptbenutzer", "Druck-Operatoren" oder "Server-Operatoren" für das VMM-Dienstkonto auf dem Remotecomputer zu, auf dem sich die Freigabe befindet.
- Wenn Sie zum Hinzufügen von Hosts ausführende Konten verwenden, weisen Sie auf dem Import- und Exportpfad Lese- und Schreibberechtigungen für die ausführenden Konten in VMM zu.
- Die Import- und Exportfreigaben sollten sich nicht auf Computern befinden, die als Hyper-V-Hostserver eingesetzt werden, da die Loopbackkonfiguration von Hyper-V nicht unterstützt wird.
- Aktivieren und konfigurieren Sie die eingeschränkte Delegierung in Active Directory auf jedem Hyper-V-Hostserver mit virtuellen Computern, die Sie schützen möchten, um den Remotecomputern zu vertrauen, auf denen sich der Import- und der Exportpfad befinden. Gehen Sie dabei wie folgt vor:
	1. Öffnen Sie auf dem Domänencontroller **Active Directory-Benutzer und-Computer**.
	2. Klicken Sie in der Konsolenstruktur auf **Domänenname** > **Computer**.
	3. Klicken Sie mit der rechten Maustaste auf den Namen des Hyper-V-Hostservers > **Eigenschaften**.
	4. Klicken Sie auf der Registerkarte **Delegierung** auf **Computer bei Delegierungen angegebener Dienste vertrauen**.
	5. Klicken Sie auf **Beliebiges Authentifizierungsprotokoll verwenden**.
	6. Klicken Sie auf **Hinzufügen** > **Benutzer und Computer**.
	7. Geben Sie den Namen des Computers ein, der den Exportpfad hostet > **OK**. Halten Sie die STRG-Taste gedrückt, und klicken Sie in der Liste der verfügbaren Dienste auf **CIFS** > **OK**. Wiederholen Sie diesen Vorgang für den Namen des Computers, der den Importpfad hostet. Wiederholen Sie den Vorgang bei Bedarf für zusätzliche Hyper-V-Hostserver.


### Konfigurieren der Netzwerkzuordnung

Richten Sie eine Netzwerkzuordnung zwischen Quell- und Zielnetzwerken ein.

- Lesen Sie sich eine [kurze Übersicht](#prepare-for-network-mapping) über die Abläufe bei der Netzwerkzuordnung durch. Ausführlichere Informationen finden Sie [hier](site-recovery-network-mapping.md).
- Stellen Sie sicher, dass die virtuellen Computer auf den VMM-Servern mit einem VM-Netzwerk verbunden sind.

Konfigurieren Sie die Zuordnung wie folgt:

1. Klicken Sie unter **Einstellungen** > **Site Recovery-Infrastruktur** > **Netzwerkzuordnung** > **Netzwerkzuordnungen** auf **+Netzwerkzuordnung**.

	![Netzwerkzuordnung](./media/site-recovery-vmm-to-azure/network-mapping1.png)

2. Wählen Sie auf der Registerkarte **Netzwerkzuordnung hinzufügen** die Quell- und Ziel-VMM-Server aus. Die VM-Netzwerke, die den VMM-Servern zugeordnet sind, werden abgerufen.
3. Wählen Sie unter **Quellnetzwerk** in der Liste mit den VM-Netzwerken, die dem primären VMM-Server zugeordnet sind, das gewünschte Netzwerk aus.
6. Wählen Sie unter **Zielnetzwerk** das Netzwerk aus, das Sie auf dem sekundären VMM-Server verwenden möchten. Klicken Sie dann auf **OK**.

	![Netzwerkzuordnung](./media/site-recovery-vmm-to-vmm/network-mapping2.png)

Wenn die Netzwerkzuordnung beginnt, passiert Folgendes:

- Vorhandene virtuelle Replikatcomputer, die dem Quell-VM-Netzwerk entsprechen, werden mit dem Ziel-VM-Netzwerk verbunden.
- Neue virtuelle Computer, die mit dem Quell-VM-Netzwerk verbunden sind, werden nach der Replikation mit dem zugeordneten Zielnetzwerk verbunden.
- Wenn Sie eine vorhandene Zuordnung mit einem neuen Netzwerk ändern, werden virtuelle Replikatcomputer anhand der neuen Einstellungen verbunden.
- Wenn das Zielnetzwerk mehrere Subnetze enthält und eines dieser Subnetze den gleichen Namen besitzt wie das Subnetz des virtuellen Quellcomputers, wird der virtuelle Replikatcomputer nach dem Failover mit diesem Zielsubnetz verbunden. Gibt es kein Zielsubnetz mit einem übereinstimmenden Namen, wird der virtuelle Computer mit dem ersten Subnetz im Netzwerk verbunden.

## Schritt 5: Kapazitätsplanung

Nachdem Sie nun Ihre grundlegende Infrastruktur eingerichtet haben, können Sie sich mit der Kapazitätsplanung beschäftigen und ermitteln, ob Sie zusätzliche Ressourcen benötigen.

Site Recovery verfügt über ein Kapazitätsplanungstool auf Excel-Basis, der Sie dabei unterstützt, die richtigen Ressourcen für Ihre Quellumgebung, die Site Recovery-Komponenten, das Netzwerk und den Speicher zuzuordnen. Sie können den Planer im Schnellmodus für Schätzungen ausführen, die auf einer durchschnittlichen Anzahl von VMs, Datenträgern und Speicher basieren, oder im ausführlichen Modus, in dem Sie Zahlen auf Workloadebene eingeben. Führen Sie Folgendes aus, bevor Sie beginnen:

- Sammeln Sie Informationen zu Ihrer Replikationsumgebung, z.B. VMs, Datenträger pro VM und Speicher pro Datenträger.
- Schätzen Sie die tägliche Änderungsrate für replizierte Deltadaten. Zur Unterstützung können Sie den [Capacity Planner für Hyper-V-Replikat](https://www.microsoft.com/download/details.aspx?id=39057) verwenden.

1.	Klicken Sie auf **Herunterladen**, um das Tool herunterzuladen, und führen Sie es anschließend aus. Lesen Sie sich den [entsprechenden Artikel](site-recovery-capacity-planner.md) durch.
2.	Wählen Sie anschließend unter **Haben Sie den Capacity Planner ausgeführt?** die Option **Ja** aus.

	![Kapazitätsplanung](./media/site-recovery-vmm-to-azure/gs-capacity-planning.png)

### Steuern der Bandbreite

Nachdem Sie die Echtzeitinformationen zur Deltareplikation mithilfe des Capacity Planners für das Hyper-V-Replikat gesammelt haben, unterstützt Sie das Excel-basierte Tool Capacity Planner bei der Berechnung der Bandbreite, die Sie für die Replikation (von anfänglichen und Deltadaten) benötigen. Um den Umfang der für die Replikation verwendeten Bandbreite zu steuern, können Sie die NetQos-Richtlinie mit der Gruppenrichtlinie oder Windows PowerShell konfigurieren. Hierzu können Sie zwischen mehreren Methoden wählen:

**PowerShell** | **Details**
--- | ---
**New-NetQosPolicy „QoS zu Zielsubnetz“** | Drosselung des Datenverkehrs von einem Hyper-V-Host unter Windows Server 2012 R2 zu einem sekundären Subnetz. Verwenden Sie sie, wenn die primären und sekundären Subnetze unterschiedlich sind.
**New-NetQosPolicy „QoS zu Zielport“** | Drosselung des Datenverkehrs von einem Hyper-V-Host unter Windows Server 2012 R2 zu einem Zielport.
**New-NetQosPolicy „Drosselung des Datenverkehrs von VMM“** | Drosselung des Datenverkehrs von „vmms.exe“. Hiermit wird der Hyper-V- und Livemigrations-Datenverkehr gedrosselt. Sie können die Einstellung durch Anpassung von IP-Protokollen und Ports verfeinern.

Sie können Bandbreitengewichtungs-Einstellungen verwenden oder den Datenverkehr nach Bits pro Sekunde beschränken. Wenn Sie einen Cluster verwenden, müssen Sie dies auf allen Clusterknoten durchführen. Weitere Informationen finden Sie unter:


- Blog von Thomas Maurer: [Throttling Hyper-V Replica Traffic](http://www.thomasmaurer.ch/2013/12/throttling-hyper-v-replica-traffic/) (Drosselung von Hyper-V-Replikat-Datenverkehr)
- Weitere Informationen zum [Cmdlet New-NetQosPolicy](https://technet.microsoft.com/library/hh967468.aspx)


## Schritt 6: Aktivieren der Replikation 

Aktivieren Sie die Replikation jetzt wie folgt:

1. Klicken Sie auf **Schritt 2: Anwendung replizieren** > **Quelle**. Klicken Sie nach der erstmaligen Aktivierung der Replikation im Tresor auf **+Replizieren**, um die Replikation für weitere Computer zu aktivieren.

	![Replikation aktivieren](./media/site-recovery-vmm-to-vmm/enable-replication1.png)

2. Wählen Sie auf dem Blatt **Quelle** den VMM-Server und die Cloud aus, in der sich die Hyper-V-Hosts befinden, die Sie replizieren möchten. Klicken Sie dann auf **OK**.

	![Replikation aktivieren](./media/site-recovery-vmm-to-vmm/enable-replication2.png)

3. Überprüfen Sie auf dem Blatt **Ziel** den sekundären VMM-Server und die Cloud.
4. Wählen Sie unter **Virtuelle Computer** in der Liste die VMs aus, die Sie schützen möchten.

	![Aktivieren des Schutzes für virtuelle Computer](./media/site-recovery-vmm-to-vmm/enable-replication5.png)

Sie können den Fortschritt der Aktion **Schutz aktivieren** unter „Einstellungen“ > **Aufträge** > **Site Recovery-Aufträge** verfolgen. Nachdem der Auftrag **Schutz abschließen** ausgeführt wurde, ist der virtuelle Computer bereit zum Failover.


>[AZURE.NOTE] Sie können auch Schutz für virtuelle Maschinen in der VMM-Konsole aktivieren. Klicken Sie unter „Eigenschaften des virtuellen Computers“ auf der Registerkarte **Azure Site Recovery** auf der Symbolleiste auf **Schutz aktivieren**.

Nachdem Sie die Replikation aktiviert haben, können Sie unter **Einstellungen** > **Replizierte Elemente** > „Name des virtuellen Computers“ Eigenschaften für den virtuellen Computer anzeigen. Das Dashboard **Zusammenfassung** enthält Informationen zur Replikationsrichtlinie für die VM und deren Status. Klicken Sie auf **Eigenschaften**, um weitere Details anzuzeigen.

### Integrieren vorhandener virtueller Computer

Wenn in VMM virtuelle Computer vorhanden sind, die über Hyper-V-Replikate repliziert werden, können Sie diese für den Schutz durch Azure Site Recovery wie folgt integrieren:

1. Stellen Sie sicher, dass der Hyper-V-Hostserver für den vorhandenen virtuellen Computer sich in der primären Cloud befindet, und dass der Hyper-V-Hostserver für den virtuellen Replikatcomputer sich in der sekundären Cloud befindet.
2. Stellen Sie sicher, dass eine Replikationsrichtlinie für die primäre VMM-Cloud konfiguriert ist.
2. Aktivieren Sie die Replikation für den primären virtuellen Computer. Azure Site Recovery und VMM stellen sicher, dass der gleiche Replikathost und virtuelle Computer erkannt werden. Azure Site Recovery verwendet die Replikation wieder und stellt sie anhand der festgelegten Einstellungen erneut her.


## Schritt 7: Testen der Bereitstellung

Zum Testen der Bereitstellung können Sie ein Testfailover für einen einzelnen virtuellen Computer durchführen oder einen Wiederherstellungsplan erstellen, der einen oder mehrere virtuelle Computer enthält.

### Vorbereiten des Failovers

- Zum vollständigen Testen der Bereitstellung benötigen Sie eine entsprechende Infrastruktur, damit der replizierte Computer wie erwartet funktioniert. Wenn Sie Active Directory und DNS testen möchten, können Sie einen virtuellen Computer als Domänencontroller mit DNS erstellen und per Azure Site Recovery zu Azure replizieren. Weitere Informationen finden Sie unter [Überlegungen zum Testfailover für Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
- Die Anweisungen in diesem Artikel beschreiben die Ausführung eines Testfailovers ohne Netzwerk. Diese Option testet, ob ein Failover des virtuellen Computers durchgeführt wird, jedoch nicht die Netzwerkeinstellungen für den virtuellen Computer. Weitere Informationen zu anderen Optionen finden Sie [hier](site-recovery-failover.md#run-a-test-failover).
- Beachten Sie Folgendes, wenn Sie anstelle eines Testfailovers ein ungeplantes Failover durchführen möchten:

	- Falls möglich, sollten Sie primäre Computer herunterfahren, bevor Sie ein ungeplantes Failover ausführen. Dadurch wird sichergestellt, dass die Quell- und Replikatcomputer nicht gleichzeitig ausgeführt werden.
	- Beim Durchführen eines ungeplanten Failovers wird die Datenreplikation der primären Computer beendet. Das heißt, dass nach dem Beginn des Failovers keine Datenänderungen mehr übertragen werden. Außerdem wird ein ungeplantes Failover, das auf einem Wiederherstellungsplan basiert, auch dann bis zum Ende durchgeführt, wenn ein Fehler auftritt.




### Ausführen eines Testfailovers

1. Klicken Sie zum Durchführen eines Failovers für einen einzelnen virtuellen Computer unter **Einstellungen** > **Replizierte Elemente** auf den virtuellen Computer und dann auf **+Testfailover**.

	![Testfailover](./media/site-recovery-vmm-to-vmm/test-failover.png)

2. Klicken Sie zum Durchführen eines Failovers für einen Wiederherstellungsplan unter **Einstellungen** > **Wiederherstellungspläne** mit der rechten Maustaste auf den Plan, und klicken Sie dann auf **Testfailover**. [Befolgen Sie diese Anweisungen](site-recovery-create-recovery-plans.md) zum Erstellen eines Wiederherstellungsplans.
2. Wählen Sie unter **Testfailover** die Option **Kein** aus. Mit dieser Option testen Sie, ob ein Failover des virtuellen Computers wie erwartet durchgeführt wird, doch der replizierte virtuelle Computer wird nicht mit einem Netzwerk verbunden.

	![Testfailover](./media/site-recovery-vmm-to-vmm/test-failover1.png)

3. Klicken Sie auf **OK**, um den Failovervorgang zu starten. Sie können den Fortschritt verfolgen, indem Sie auf den virtuellen Computer klicken, um die Eigenschaften zu öffnen. Alternativ dazu können Sie unter **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** auf den Auftrag **Testfailover** klicken.
4. Gehen Sie wie folgt vor, wenn der Failoverauftrag die Phase **Test abschließen** erreicht hat:

	-  Zeigen Sie die Replikat-VM in der sekundären VMM-Cloud an.
	-  Klicken Sie auf **Test abschließen**, um das Testfailover abzuschließen.
	-  Klicken Sie auf **Notizen**, um alle Beobachtungen im Zusammenhang mit dem Test-Failover aufzuzeichnen und zu speichern.

5. Der virtuelle Computer für den Test wird auf dem Host erstellt, auf dem auch der replizierte virtuelle Computer vorhanden ist. Er wird der gleichen Cloud hinzugefügt, in der sich der replizierte virtuelle Computer befindet.
6. Nachdem Sie sich vergewissert haben, dass die virtuellen Computer erfolgreich starten, klicken Sie auf **Das Testfailover ist abgeschlossen**. An diesem Punkt werden alle Elemente gelöscht, die von Site Recovery während des Testfailovers automatisch erstellt wurden.

	> [AZURE.NOTE] Sollte ein Testfailover länger als zwei Wochen ausgeführt werden, wird sein Abschluss erzwungen.



### DNS-Aktualisierung mit der Replikat-VM-IP-Adresse

Nach dem Failover hat die Replikat-VM möglicherweise nicht die gleiche IP-Adresse wie der primäre virtuelle Computer.

- Virtuelle Computer aktualisieren den DNS-Server, den sie nach dem Start verwenden.
- Sie können den DNS-Server auch wie folgt manuell aktualisieren:

#### Abrufen der IP-Adresse

Führen Sie diese Beispielskript aus, um die IP-Adresse abzurufen.

    	$vm = Get-SCVirtualMachine -Name <VM_NAME>
		$na = $vm[0].VirtualNetworkAdapters>
		$ip = Get-SCIPAddress -GrantToObjectID $na[0].id
		$ip.address  

#### DNS-Aktualisierung

Führen Sie dieses Beispielskript aus, um DNS zu aktualisieren, wobei Sie die IP-Adresse angeben, die Sie mithilfe des vorherigen Beispielskripts abgerufen haben.

		string]$Zone,
		[string]$name,
		[string]$IP
		)
		$Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
		$newrecord = $record.clone()
		$newrecord.RecordData[0].IPv4Address  =  $IP
		Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord

## Nächste Schritte

Nachdem die Bereitstellung eingerichtet wurde und ausgeführt wird, [informieren](site-recovery-failover.md) Sie sich über die unterschiedlichen Failoverarten.

<!---HONumber=AcomDC_0803_2016-->