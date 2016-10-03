<properties
	pageTitle="Replizieren von virtuellen Hyper-V-Computern (ohne VMM) nach Azure per Azure Site Recovery über das Azure-Portal | Microsoft Azure"
	description="Es wird beschrieben, wie Sie Azure Site Recovery bereitstellen, um Replikation, Failover und Wiederherstellung von lokalen Hyper-V-VMs, die nicht von VMM verwaltet werden, auf Azure mit dem Azure-Portal zu orchestrieren."
	services="site-recovery"
	documentationCenter=""
	authors="rayne-wiselman"
	manager="jwhit"
	editor=""/>

<tags
	ms.service="site-recovery"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.workload="storage-backup-recovery"
	ms.date="09/19/2016"
	ms.author="raynew"/>


# Replizieren von virtuellen Hyper-V-Computern (ohne VMM) nach Azure per Azure Site Recovery über das Azure-Portal | Microsoft Azure

> [AZURE.SELECTOR]
- [Azure-Portal](site-recovery-hyper-v-site-to-azure.md)
- [Azure – klassisch](site-recovery-hyper-v-site-to-azure-classic.md)
- [PowerShell ARM](site-recovery-deploy-with-powershell-resource-manager.md)



Willkommen bei Azure Site Recovery! Verwenden Sie die Informationen in diesem Artikel, wenn Sie lokale virtuelle Hyper-V-Computer nach Azure replizieren möchten, die **nicht** in System Center Virtual Machine Manager-Clouds (VMM) verwaltet werden. In diesem Artikel wird beschrieben, wie die Replikation mithilfe von Azure Site Recovery im Azure-Portal eingerichtet werden kann.

> [AZURE.NOTE] Azure verfügt über zwei unterschiedliche [Bereitstellungsmodelle](../resource-manager-deployment-model.md) für das Erstellen und Verwenden von Ressourcen: die Azure Resource Manager-Bereitstellung (ARM) und die klassische Bereitstellung. Azure verfügt auch über zwei Portale: das klassische Azure-Portal mit Unterstützung des klassischen Bereitstellungsmodells und das Azure-Portal mit Unterstützung für beide Bereitstellungsmodelle.


Azure Site Recovery im Azure-Portal verfügt über eine Reihe neuer Features:

- Im Azure-Portal wurden die Dienste Azure Backup und Azure Site Recovery in einem Recovery Services-Tresor kombiniert, damit Sie die Geschäftskontinuität und Notfallwiederherstellung zentral verwalten können. Dank eines einheitlichen Dashboards können Sie alle Vorgänge an Ihren lokalen Standorten und in der öffentlichen Azure-Cloud überwachen und verwalten.
- Benutzer mit Azure-Abonnements, die über das Programm für Cloudlösungsanbieter (Cloud Solution Provider, CSP) bereitgestellt wurden, können Site Recovery-Vorgänge jetzt im Azure-Portal verwalten.
- Mit Site Recovery im Azure-Portal können Computer in ARM-Speicherkonten repliziert werden. Bei einem Failover erstellt Site Recovery ARM-basierte VMs in Azure.
- Für Site Recovery wird die Replikation auf klassische Speicherkonten und das Failover auf VMs mithilfe des klassischen Modells weiterhin unterstützt.


Nachdem Sie diesen Artikel gelesen haben, können Sie unten im Disqus-Kommentarbereich Ihre Kommentare eingeben. Im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) können Sie technische Fragen stellen.


## Übersicht


Organisationen benötigen eine Strategie für die Geschäftskontinuität und Notfallwiederherstellung (BCDR), mit der bestimmt wird, wie Apps, Workloads und Daten bei geplanten und ungeplanten Ausfällen verfügbar bleiben und die normalen Arbeitsbedingungen so schnell wie möglich wiederhergestellt werden können. Ihre BCDR-Strategie sorgt dafür, dass Unternehmensdaten geschützt sind und wiederhergestellt werden können. Sie stellt außerdem sicher, dass Workloads auch nach dem Eintreten eines Notfalls ständig verfügbar sind.

Site Recovery ist ein Azure-Dienst, der einen Beitrag zu Ihrer BCDR-Strategie leistet, indem die Replikation von lokalen physischen Servern und virtuellen Maschinen in die Cloud (Azure) oder in ein sekundäres Rechenzentrum orchestriert wird. Wenn es an Ihrem primären Standort zu Ausfällen kommt, wird ein Failover zum sekundären Standort durchgeführt, um die Verfügbarkeit von Apps und Workloads zu erhalten. Wenn wieder Normalbetrieb herrscht, führen Sie das Failback zum primären Standort durch. Weitere Informationen finden Sie unter [Was ist Azure Site Recovery?](site-recovery-overview.md).

Dieser Artikel enthält alle Informationen, die Sie zum Replizieren von Hyper-V-VMs nach Azure benötigen, die nicht in VMM-Clouds verwaltet werden. Er enthält eine Übersicht über die Architektur, Informationen zur Planung und Bereitstellungsschritte zum Konfigurieren von lokalen Servern, Azure, einer Replikationsrichtlinie und der Kapazitätsplanung. Nachdem Sie die Infrastruktur eingerichtet haben, können Sie die Replikation auf den zu schützenden Computern aktivieren und das Failover testen.

## Vorteile für Unternehmen

- Ermöglicht das externe Failover für geschäftliche Workloads und Anwendungen, die auf Hyper-V-VMs ausgeführt werden.
- Repliziert von Hyper-V-VMs mit Azure Site Recovery, ohne dass ein VMM-Server benötigt wird.
- Stellt eine einzelne Recovery Services-Konsole für das einfache Einrichten und Verwalten von Replikations-, Failover- und Wiederherstellungsprozessen bereit.
- Ermöglicht Ihnen das einfache Ausführen von Failovern von Ihrer lokalen Infrastruktur auf Azure und das Durchführen eines Failback (einer Wiederherstellung) von Azure auf den lokalen Standort.
- Sie können Wiederherstellungspläne mit mehreren Computern konfigurieren, damit Failover für Workloads von Anwendungen mit mehreren Ebenen zusammen erfolgen.

## Szenarioarchitektur

Dies sind die Szenariokomponenten:

- **Hyper-V-Host oder -Cluster**: lokale Hyper-V-Hostserver oder -Cluster. Die Hyper-V-Hosts, die virtuelle Computer ausführen, die Sie schützen möchten, werden während der Bereitstellung von Site Recovery in logischen Hyper-V-Standorten gesammelt.
- **Azure Site Recovery-Anbieter und Recovery Services-Agent**: Während der Bereitstellung installieren Sie den Azure Site Recovery-Anbieter und den Microsoft Azure Recovery Services-Agent auf Hyper-V-Hostservern. Der Anbieter kommuniziert mit Azure Site Recovery über HTTPS 443, um die Orchestrierung zu replizieren. Standardmäßig repliziert der Agent Daten auf dem Hyper-V-Hostserver über HTTPS 443 in den Azure-Speicher.
- **Azure**: Sie benötigen ein Azure-Abonnement, ein Azure-Speicherkonto zum Speichern replizierter Daten und ein virtuelles Azure-Netzwerk, damit Azure-VMs nach dem Failover über eine Netzwerkverbindung verfügen.

![Hyper-V-Standort-Architektur](./media/site-recovery-hyper-v-site-to-azure/architecture.png)
 


## Voraussetzungen für Azure

In Azure benötigen Sie für die Bereitstellung dieses Szenarios Folgendes:

**Voraussetzung** | **Details**
--- | ---
**Azure-Konto**| Sie benötigen ein [Microsoft Azure](http://azure.microsoft.com/)-Konto. Für den Einstieg steht eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/) zur Verfügung. Weitere Informationen zu den Preisen für Site Recovery erhalten Sie [hier](https://azure.microsoft.com/pricing/details/site-recovery/). 
**Azure-Speicher** | Sie benötigen ein Standardspeicherkonto. Hierfür können Sie ein LRS- oder GRS-Speicherkonto verwenden. Wir empfehlen Ihnen die Verwendung von GRS, damit Resilienz für die Daten besteht, wenn es zu einem regionalen Ausfall kommt oder wenn die primäre Region nicht wiederhergestellt werden kann. [Weitere Informationen](../storage/storage-redundancy.md). Das Konto muss sich in derselben Region wie der Recovery Services-Tresor befinden.<br/><br/> Storage Premium wird nicht unterstützt.<br/><br/> Replizierte Daten werden in Azure Storage gespeichert, und virtuelle Azure-Computer werden erstellt, wenn ein Failover durchgeführt wird.<br/><br/> [Erfahren Sie mehr](../storage/storage-introduction.md) über Azure Storage.
**Azure-Netzwerk** | Sie benötigen ein virtuelles Azure-Netzwerk, mit dem die virtuellen Azure-Computer eine Verbindung herstellen, wenn ein Failover stattfindet. Das virtuelle Azure-Netzwerk muss sich in derselben Region wie der Recovery Services-Tresor befinden. 

## Lokale Voraussetzungen

Lokal benötigen Sie Folgendes:

**Voraussetzung** | **Details**
--- | ---
**Hyper-V**| Ein oder mehrere lokale Server, auf denen Windows Server 2012 R2 ausgeführt wird, mit den aktuellsten Updates und der Hyper-V-Rolle.<br/><br/>Der Hyper-V-Server sollte einen oder mehrere virtuelle Computer enthalten.<br/><br/>Hyper-V-Server sollten mit dem Internet verbunden sein, entweder direkt oder über einen Proxy.<br/><br/>Auf Hyper-V-Servern sollten die in [KB2961977](https://support.microsoft.com/de-DE/kb/2961977 "KB2961977") erwähnten Fixes installiert sein.
**Anbieter und Agent** | Während der Bereitstellung von Azure Site Recovery installieren Sie den Azure Site Recovery-Anbieter. Die Anbieterinstallation installiert den Azure Recovery Services-Agent außerdem auf jedem Hyper-V-Server, der über zu schützende virtuelle Computer verfügt. Alle Hyper-V-Server in einem Site Recovery-Tresor sollten über die gleichen Anbieter- und Agent-Versionen verfügen.<br/><br/>Der Anbieter muss über das Internet eine Verbindung mit Azure Site Recovery herstellen. Datenverkehr kann direkt oder über einen Proxy gesendet werden. Beachten Sie, dass HTTPS-basierte Proxys nicht unterstützt werden. Der Proxyserver sollte den Zugriff auf Folgendes zulassen: <br/><br/> *.hypervrecoverymanager.windowsazure.com <br/><br/> *.accesscontrol.windows.net <br/><br/> *.backup.windowsazure.com <br/><br/> *.blog.core.windows.net <br/><br/> *store.core.windows.net <br/><br/> https://www.msftncsi.com/ncsi.txt<br/><br/>Wenn Sie auf dem Server über Firewallregeln verfügen, die auf IP-Adressen basieren, sollten Sie sicherstellen, dass die Regeln die Kommunikation mit Azure zulassen. Sie müssen die [IP-Adressbereiche des Azure-Rechenzentrums](https://www.microsoft.com/download/confirmation.aspx?id=41653) und das HTTPS-Protokoll (433) zulassen.<br/><br/>Lassen Sie IP-Adressbereiche für die Azure-Region Ihres Abonnements und für die Region „USA, Westen“ zu.

## Voraussetzungen für geschützte Computer


**Voraussetzung** | **Details**
--- | ---
**Geschützte VMs** | Bevor Sie für eine VM das Failover durchführen, müssen Sie sicherstellen, dass der Name, der dem virtuellen Azure-Computer zugewiesen wird, die [Voraussetzungen für Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements) erfüllt. Sie können den Namen ändern, nachdem Sie die Replikation für die VM aktiviert haben.<br/><br/> Die individuelle Datenträgerkapazität auf geschützten Computern sollte nicht mehr als 1023 GB betragen. Ein virtueller Computer kann bis zu 64 Datenträger haben (also bis zu 64 TB).<br/><br/> Gastcluster mit freigegebenen Datenträgern werden nicht unterstützt.<br/><br/> Wenn auf der Quell-VM ein NIC-Teamvorgang genutzt wird, wird dieser nach dem Failover zu Azure in eine einzelne Netzwerkschnittstellenkarte (Network Interface Card, NIC) konvertiert.<br/><br/>Das Schützen von VMs, auf denen Linux mit einer statischen IP-Adresse ausgeführt wird, wird nicht unterstützt.

## Vorbereiten der Bereitstellung

Für die Vorbereitung der Bereitstellung benötigen Sie Folgendes:

1. [Richten Sie ein Azure-Netzwerk ein](#set-up-an-azure-network), in dem sich Azure-VMs befinden, wenn sie nach dem Failover erstellt werden.
2. [Richten Sie ein Azure-Speicherkonto ein](#set-up-an-azure-storage-account), das für replizierte Daten verwendet werden kann.
3. [Bereiten Sie die Hyper-V-Hosts vor](#prepare-the-hyper-v-hosts), um sicherzustellen, dass diese auf die erforderlichen URLs zugreifen können.

### Einrichten eines Azure-Netzwerks

Richten Sie ein Azure-Netzwerk ein. Sie benötigen dies, damit die nach dem Failover erstellten Azure-VMs mit einem Netzwerk verbunden sind.

- Das Netzwerk sollte in derselben Region wie das Netzwerk angeordnet sein, in dem Sie den Recovery Services-Tresor bereitstellen.
- Richten Sie das Azure-Netzwerk im [ARM-Modus](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) oder im [klassischen Modus](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) ein (je nachdem, welches Ressourcenmodell Sie für virtuelle Azure-Computer nach dem Failover verwenden möchten).
- Wir empfehlen Ihnen, ein Netzwerk einzurichten, bevor Sie beginnen. Falls Sie es nicht tun, müssen Sie diesen Schritt während der Site Recovery-Bereitstellung ausführen.

> [AZURE.NOTE] [Migration of networks](../resource-group-move-resources.md) zwischen Ressourcengruppen im gleichen Abonnement oder zwischen verschiedenen Abonnements wird für Netzwerke nicht unterstützt, die für die Site Recovery-Bereitstellung verwendet werden.

### Einrichten eines Azure-Speicherkontos

- Sie benötigen ein standardmäßiges Azure-Speicherkonto für Daten, die in Azure repliziert werden.
- Je nach Ressourcenmodell, das Sie für Azure-VMs nach dem Failover verwenden möchten, richten Sie ein Konto im [ARM-Modus](../storage/storage-create-storage-account.md) oder im [klassischen Modus](../storage/storage-create-storage-account-classic-portal.md) ein.
- Es wird empfohlen, ein Speicherkonto einzurichten, bevor Sie beginnen. Falls Sie es nicht tun, müssen Sie diesen Schritt während der Site Recovery-Bereitstellung ausführen. Die Konten müssen sich in derselben Region wie der Recovery Services-Tresor befinden.

> [AZURE.NOTE] [Migration of storage accounts](../resource-group-move-resources.md) zwischen Ressourcengruppen im gleichen Abonnement oder zwischen verschiedenen Abonnements wird für Speicherkonten nicht unterstützt, die für die Site Recovery-Bereitstellung verwendet werden.

### Vorbereiten der Hyper-V-Hosts

- Stellen Sie sicher, dass die Hyper-V-Hosts die [Voraussetzungen](#on-premises-prerequisites) erfüllen.

### Erstellen eines Recovery Services-Tresors

1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf **Neu** > **Verwaltung** > **Sicherung und Site Recovery (OMS)**. Alternativ können Sie auf **Durchsuchen** > **Recovery Services-Tresore** > **Hinzufügen** klicken.

	![Neuer Tresor](./media/site-recovery-hyper-v-site-to-azure/new-vault3.png)

3. Geben Sie unter **Name** einen Anzeigenamen an, mit dem der Tresor identifiziert wird. Wenn Sie mehrere Abonnements haben, müssen Sie ein Abonnement auswählen.
4. [Erstellen Sie eine neue Ressourcengruppe](../resource-group-template-deploy-portal.md), oder wählen Sie eine vorhandene Ressourcengruppe aus, und geben Sie eine Azure-Region an. Computer werden in dieser Region repliziert. Sie finden eine Liste der unterstützten Regionen unter Geografische Verfügbarkeit auf der Seite [Azure Site Recovery – Preisübersicht](https://azure.microsoft.com/pricing/details/site-recovery/)
4. Wenn Sie schnell über das Dashboard auf den Tresor zugreifen möchten, klicken Sie auf **An Dashboard anheften** und anschließend auf **Tresor erstellen**.

	![Neuer Tresor](./media/site-recovery-hyper-v-site-to-azure/new-vault-settings.png)

Der neue Tresor wird unter **Dashboard** > **Alle Ressourcen** und auf dem Hauptblatt **Recovery Services-Tresore** angezeigt.

## Erste Schritte

Site Recovery verfügt über eine Benutzeroberfläche für die ersten Schritte, damit Sie die Bereitstellung so schnell wie möglich durchführen können. Im Rahmen der ersten Schritte werden die Voraussetzungen überprüft, und Sie werden durch die Schritte der Site Recovery-Bereitstellung in der richtigen Reihenfolge geführt.

Sie wählen den Typ der Computer aus, die Sie replizieren möchten, und geben an, wo sie repliziert werden sollen. Sie richten lokale Server, Azure-Speicherkonten und Netzwerke ein. Sie erstellen Replikationsrichtlinien und führen die Kapazitätsplanung durch. Nachdem Sie Ihre Infrastruktur eingerichtet haben, aktivieren Sie die Replikation für VMs. Sie können Failover für bestimmte Computer ausführen oder Wiederherstellungspläne erstellen, um das Failover für mehrere Computer durchzuführen.

Beginnen Sie mit den ersten Schritten, indem Sie auswählen, wie Sie Site Recovery bereitstellen möchten. Der Ablauf der ersten Schritte kann je nach Replikationsanforderungen leicht variieren.



## Schritt 1: Auswählen der Schutzziele

Wählen Sie aus, was Sie replizieren möchten und wohin die Daten repliziert werden sollen.

1. Wählen Sie auf dem Blatt **Recovery Services-Tresore** Ihren Tresor aus, und klicken Sie auf **Einstellungen**.
2. Klicken Sie unter **Einstellungen** > **Erste Schritte** auf **Site Recovery** > **Schritt 1: Bereiten Sie die Infrastruktur vor** > **Schutzziel**.

	![Ziele wählen](./media/site-recovery-hyper-v-site-to-azure/choose-goals.png)

3. Wählen Sie unter **Schutzziel** die Option **To Azure** (Zu Azure) und dann **Yes, with Hyper-V** (Ja, mit Hyper-V) aus. Wählen Sie **Nein** aus, um zu bestätigen, dass Sie VMM nicht verwenden. Klicken Sie dann auf **OK**.

	![Ziele wählen](./media/site-recovery-hyper-v-site-to-azure/choose-goals2.png)


## Schritt 2: Einrichten der Quellumgebung

Richten Sie den Hyper-V-Standort ein, installieren Sie den Azure Site Recovery-Anbieter und den Azure Recovery Services-Agent auf Hyper-V-Hosts, und registrieren Sie die Hosts im Tresor.


1. Klicken Sie auf **Schritt 2: Infrastruktur vorbereiten** > **Quelle**. Klicken Sie auf **+Hyper-V-Standort**, um einen neuen Hyper-V-Standort als Container für Ihre Hyper-V-Hosts oder -Cluster hinzuzufügen.

	![Quelle einrichten](./media/site-recovery-hyper-v-site-to-azure/set-source1.png)

2. Geben Sie auf dem Blatt **Hyper-V-Standort erstellen** einen Namen für den Standort an. Klicken Sie dann auf **OK**. Wählen Sie den Standort aus, den Sie gerade erstellt haben.

	![Quelle einrichten](./media/site-recovery-hyper-v-site-to-azure/set-source2.png)

3. Klicken Sie auf **+Hyper-V Server**, um dem Standort einen Server hinzuzufügen.
4. Vergewissern Sie sich, dass unter **Server hinzufügen** > **Servertyp** als Typ **Hyper-V Server** angezeigt wird. Stellen Sie sicher, dass der Hyper-V-Server, den Sie hinzufügen möchten, die [Voraussetzungen](#on-premises-prerequisites) erfüllt und auf die angegebenen URLs zugreifen kann.
4. Laden Sie die Installationsdatei für den Azure Site Recovery-Anbieter herunter. Führen Sie diese Datei aus, um sowohl den Anbieter als auch den Recovery Services-Agent auf jedem Hyper-V-Host zu installieren.
5. Laden Sie den Registrierungsschlüssel herunter. Sie benötigen diese Angaben beim Ausführen des Setups. Der Schlüssel ist nach der Erstellung fünf Tage lang gültig.

	![Quelle einrichten](./media/site-recovery-hyper-v-site-to-azure/set-source3.png)

6. Führen Sie die Setupdatei für den Anbieter auf jedem Host aus, den Sie zum Hyper-V-Standort hinzugefügt haben. Wenn Sie auf einem Hyper-V-Cluster installieren, führen Sie das Setup auf jedem Clusterknoten aus. Durch das Installieren und Registrieren jedes Hyper-V-Clusterknotens wird sichergestellt, dass virtuelle Computer geschützt bleiben, selbst wenn sie über Knoten hinweg migrieren.

### Installieren Sie den Anbieter und den Agent.

1. Führen Sie die Setupdatei für den Anbieter aus.
2. Unter **Microsoft Update** können Sie Updates aktivieren, damit Updates für den Anbieter gemäß Ihrer Microsoft Update-Richtlinie installiert werden.
3. Akzeptieren oder ändern Sie unter **Installation** den Standardspeicherort der Anbieterinstallation, und klicken Sie auf **Installieren**.
4. Klicken Sie auf der Seite **Tresoreinstellungen** auf **Durchsuchen**, um die Tresorschlüsseldatei auszuwählen, die Sie heruntergeladen haben. Geben Sie das Azure Site Recovery-Abonnement, den Tresornamen und den Hyper-V-Standort an, zu dem der Hyper-V-Server gehört.

	![Serverregistrierung](./media/site-recovery-hyper-v-site-to-azure/provider3.png)

5\. Geben Sie unter **Proxyeinstellungen** an, wie der Anbieter, der auf dem Server installiert wird, eine Internetverbindung mit Azure Site Recovery herstellt.

- Wenn der Anbieter eine direkte Verbindung herstellt, wählen Sie **Ohne Proxy direkt verbinden** aus.
- Wenn die Verbindung über einen derzeit auf dem Server eingerichteten Proxy hergestellt werden soll, wählen Sie **Mit vorhandenen Proxyeinstellungen verbinden** aus.
- Wenn für den vorhandenen Proxy eine Authentifizierung erforderlich ist oder Sie für die Anbieterverbindung einen benutzerdefinierten Proxy verwenden möchten, wählen Sie **Mit benutzerdefinierten Proxyeinstellungen verbinden** aus.
- Bei einem benutzerdefinierten Proxy müssen Sie die Adresse, den Port und Anmeldeinformationen eingeben.
- Stellen Sie sicher, dass die in den [Voraussetzungen](#on-premises-prerequisites) beschriebenen URLs vom Proxy zugelassen werden, falls Sie einen verwenden.

	![Internet](./media/site-recovery-hyper-v-site-to-azure/provider7.PNG)

6\. Klicken Sie nach Abschluss der Installation auf **Registrieren**, um den Server im Tresor zu registrieren. ![Installationspfad](./media/site-recovery-hyper-v-site-to-azure/provider2.png)

7\. Nach der Registrierung ruft Azure Site Recovery Metadaten vom Hyper-V-Server ab, und der Server wird auf dem Blatt **Einstellungen** > **Site Recovery-Infrastruktur** > **Hyper-V-Hosts** angezeigt.


### Installation über die Befehlszeile

Der Azure Site Recovery-Anbieter und der Azure Site Recovery-Agent können auch mithilfe der folgenden Befehlszeile installiert werden. Mit dieser Methode kann der Anbieter in Server Core für Windows Server 2012 R2 installiert werden.

1. Laden Sie die Installationsdatei und den Registrierungsschlüssel des Anbieters in einen Ordner herunter. Beispiel: C:\\ASR.
2. Extrahieren Sie über eine Eingabeaufforderung mit erhöhten Rechten das Installationsprogramm des Anbieters, indem Sie diese Befehle ausführen:

	    	C:\Windows\System32> CD C:\ASR
	    	C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Führen Sie diesen Befehl aus, um die Komponenten zu installieren:

			C:\ASR> setupdr.exe /i

4. Führen Sie anschließend diese Befehle aus, um den Server im Tresor zu registrieren: CD C:\\Programme\\Microsoft Azure Site Recovery Provider\\ C:\\Programme\\Microsoft Azure Site Recovery Provider> DRConfigurator.exe /r /Friendlyname <Anzeigename des Servers> /Credentials <Pfad der Anmeldeinformationendatei>, wobei:

- **/Credentials**: erforderlicher Parameter zum Angeben des Speicherorts der Registrierungsschlüsseldatei.
- **/FriendlyName**: erforderlicher Parameter für den Namen des Hyper-V-Hostservers, der im Azure Site Recovery-Portal angezeigt wird.
- **/proxyAddress**: optionaler Parameter, der die Adresse des Proxyservers angibt.
- **/proxyport**: optionaler Parameter, der den Port des Proxyservers angibt.
- **/proxyUsername**: optionaler Parameter, der den Proxybenutzernamen angibt (sofern der Proxy eine Authentifizierung erfordert).
- **/proxyPassword**: optionaler Parameter, der das Kennwort für die Authentifizierung mit dem Proxyserver angibt (sofern der Proxy eine Authentifizierung erfordert).


## Schritt 3: Einrichten der Zielumgebung

Geben Sie das Azure-Speicherkonto, das für die Replikation verwendet werden soll, und das Azure-Netzwerk an, mit dem Azure-VMs nach dem Failover eine Verbindung herstellen.

1.	Klicken Sie auf **Infrastruktur vorbereiten** > **Ziel**, und wählen Sie das gewünschte Azure-Abonnement aus.
2.	Geben Sie das Bereitstellungsmodell an, das Sie nach einem Failover für VMs verwenden möchten.
3.	Site Recovery prüft, ob Sie über ein oder mehrere kompatible Azure-Speicherkonten und -Netzwerke verfügen.

	![Speicher](./media/site-recovery-hyper-v-site-to-azure/select-target.png)

4.	Wenn Sie noch kein Speicherkonto erstellt haben und dies mit ARM nachholen möchten, klicken Sie auf **+Speicherkonto**, um diesen Vorgang gleich durchzuführen. Geben Sie auf dem Blatt **Speicherkonto erstellen** einen Kontonamen, einen Typ, ein Abonnement und einen Standort an. Das Konto sollte sich an demselben Standort wie der Recovery Services-Tresor befinden.

	![Speicher](./media/site-recovery-hyper-v-site-to-azure/gs-createstorage.png)

	Wenn Sie ein Speicherkonto mit dem klassischen Modell erstellen möchten, verwenden Sie hierfür das [Azure-Portal](../storage/storage-create-storage-account-classic-portal.md).
	
5.	Wenn Sie noch kein Azure-Netzwerk erstellt haben und dies mit ARM nachholen möchten, klicken Sie auf **+Netzwerk**, um diesen Vorgang gleich durchzuführen. Geben Sie auf dem Blatt **Virtuelles Netzwerk erstellen** einen Netzwerknamen, einen Adressbereich, Subnetzdetails, ein Abonnement und einen Standort an. Das Netzwerk sollte sich an demselben Standort wie der Recovery Services-Tresor befinden.

	![Netzwerk](./media/site-recovery-hyper-v-site-to-azure/gs-createnetwork.png)

	Wenn Sie ein Netzwerk mit dem klassischen Modell erstellen möchten, verwenden Sie hierfür das [Azure-Portal](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).
	

## Schritt 4: Einrichten der Replikationseinstellungen

1. Klicken Sie zum Erstellen einer neuen Replikationsrichtlinie auf **Infrastruktur vorbereiten** > **Replikationseinstellungen** > **+Erstellen und zuordnen**.

	![Netzwerk](./media/site-recovery-hyper-v-site-to-azure/gs-replication.png)

2. Geben Sie unter **Richtlinie erstellen und zuordnen** einen Richtliniennamen an.
3. Geben Sie unter **Kopierhäufigkeit** an, wie oft Sie Deltadaten nach der ersten Replikation replizieren möchten (alle 30 Sekunden, nach 5 Minuten oder nach 15 Minuten).
4. Geben Sie unter **Aufbewahrungszeitraum des Wiederherstellungspunkts** das Aufbewahrungszeitfenster für die einzelnen Wiederherstellungspunkte in Stunden an. Geschützte Computer können innerhalb eines Zeitfensters an einem beliebigen Punkt wiederhergestellt werden.
6. Geben Sie unter **App-konsistente Momentaufnahmehäufigkeit** an, wie häufig (1 bis 12 Stunden) Wiederherstellungspunkte erstellt werden sollen, die anwendungskonsistente Momentaufnahmen enthalten. Hyper-V verwendet zwei Momentaufnahmen: eine Standard-Momentaufnahme, die eine inkrementelle Momentaufnahme des gesamten virtuellen Computers bereitstellt, und eine anwendungskonsistente Momentaufnahme, die eine Zeitpunkt-Momentaufnahme der Anwendungsdaten innerhalb des virtuellen Computers erfasst. Anwendungskonsistente Momentaufnahmen verwenden den Volumeschattenkopie-Dienst (Volume Shadow Copy Service, VSS), um sicherzustellen, dass Anwendungen sich bei der Erstellung der Momentaufnahme in einem konsistenten Zustand befinden. Beachten Sie, dass die Leistung von Anwendungen auf virtuellen Quellcomputern durch die Aktivierung anwendungskonsistenter Momentaufnahmen beeinträchtigt wird. Stellen Sie sicher, dass der festgelegte Wert kleiner als die konfigurierte Anzahl der zusätzlichen Wiederherstellungspunkte ist.
3. Geben Sie unter **Startzeit der ersten Replikation** an, wann die erste Replikation starten soll. Da die Replikation über Ihre Internetbandbreite durchgeführt wird, ist es ratsam, sie außerhalb der Zeiten mit der höchsten Arbeitsbelastung einzuplanen. Klicken Sie dann auf **OK**.

	![Replikationsrichtlinie](./media/site-recovery-hyper-v-site-to-azure/gs-replication2.png)

Wenn Sie eine neue Richtlinie erstellen, wird sie dem Hyper-V-Standort automatisch zugeordnet. Klicken Sie auf **OK**. Sie können den Hyper-V-Standort (und die darin enthaltenen VMs) über **Einstellungen** > **Replikation** > „Richtlinienname“ > **Hyper-V-Standort zuordnen** mehreren Replikationsrichtlinien zuordnen.

## Schritt 5: Kapazitätsplanung

Nachdem Sie nun Ihre grundlegende Infrastruktur eingerichtet haben, können Sie sich mit der Kapazitätsplanung beschäftigen und ermitteln, ob Sie zusätzliche Ressourcen benötigen.

Site Recovery verfügt über einen Capacity Planner, der Sie dabei unterstützt, die richtigen Ressourcen für Ihre Quellumgebung, die Site Recovery-Komponenten, das Netzwerk und den Speicher zuzuordnen. Sie können den Planer im Schnellmodus für Schätzungen ausführen, die auf einer durchschnittlichen Anzahl von VMs, Datenträgern und Speicher basieren, oder im ausführlichen Modus, in dem Sie Zahlen auf Workloadebene eingeben. Führen Sie Folgendes aus, bevor Sie beginnen:

- Sammeln Sie Informationen zu Ihrer Replikationsumgebung, z.B. VMs, Datenträger pro VM und Speicher pro Datenträger.
- Schätzen Sie die tägliche Änderungsrate für replizierte Daten. Zur Unterstützung können Sie den [Capacity Planner für Hyper-V-Replikat](https://www.microsoft.com/download/details.aspx?id=39057) verwenden.

1.	Klicken Sie auf **Herunterladen**, um das Tool herunterzuladen, und führen Sie es anschließend aus. Lesen Sie sich den [entsprechenden Artikel](site-recovery-capacity-planner.md) durch.
2.	Wählen Sie anschließend unter **Haben Sie den Capacity Planner ausgeführt?** die Option **Ja** aus.

	![Kapazitätsplanung](./media/site-recovery-hyper-v-site-to-azure/gs-capacity-planning.png)

### Aspekte der Netzwerkbandbreite

Sie können den Capacity Planner verwenden, um die Bandbreite zu berechnen, die Sie für die Replikation benötigen (erste Replikation und dann Deltareplikation). Sie haben verschiedene Möglichkeiten, um die verwendete Bandbreite für die Replikation zu steuern:

- **Bandbreite drosseln**: Hyper-V-Datenverkehr, der nach Azure repliziert wird, wird über einen speziellen Hyper-V-Host geleitet. Sie können die Bandbreite auf dem Hostserver drosseln.
- **Bandbreite optimieren**: Sie können beeinflussen, wie viel Bandbreite für die Replikation verwendet wird, indem Sie einige Registrierungsschlüssel nutzen.

#### Bandbreite einschränken

1. Öffnen Sie das Microsoft Azure Backup-MMC-Snap-In auf dem Hyper-V-Hostserver. Standardmäßig ist auf dem Desktop oder unter „C:\\Programme\\Microsoft Azure Recovery Services Agent\\bin\\wabadmin“ eine Verknüpfung für Microsoft Azure Backup verfügbar.
2. Klicken Sie im Snap-In auf **Eigenschaften ändern**.
3. Wählen Sie auf der Registerkarte **Drosselung** die Option **Internet-Bandbreiteneinschränkung für Sicherungsvorgänge aktivieren**, und legen Sie die Grenzwerte für Geschäftszeiten und arbeitsfreie Zeiten fest. Der gültige Bereich reicht von 512 KBit/s bis 102 MBit/s.

	![Bandbreite einschränken](./media/site-recovery-hyper-v-site-to-azure/throttle2.png)

Sie können auch das Cmdlet [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) verwenden, um die Drosselung festzulegen. Hier ein Beispiel:

    $mon = [System.DayOfWeek]::Monday 
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** gibt an, dass keine Drosselung erforderlich ist.


#### Netzwerkbandbreite beeinflussen

1. Navigieren Sie in der Registrierung zu **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows Azure Backup\\Replication**.
	- Um die Bandbreite für den Datenverkehr auf einem replizierenden Datenträger zu beeinflussen, ändern Sie den Wert für **UploadThreadsPerVM**, oder erstellen Sie den Schlüssel, falls dieser nicht vorhanden ist.
	- Um die Bandbreite für den Failbackdatenverkehr von Azure zu beeinflussen, ändern Sie den Wert für **DownloadThreadsPerVM**.
2. Der Standardwert ist 4. In einem absichtlich mit großen Reserven ausgestatteten Netzwerk müssen die Standardwerte dieser Registrierungsschlüssel geändert werden. Der maximale Wert beträgt 32. Überwachen Sie den Datenverkehr, um den Wert zu optimieren.

## Schritt 6: Aktivieren der Replikation

Aktivieren Sie die Replikation jetzt wie folgt:

1. Klicken Sie auf **Schritt 2: Anwendung replizieren** > **Quelle**. Klicken Sie nach der erstmaligen Aktivierung der Replikation im Tresor auf **+Replizieren**, um die Replikation für weitere Computer zu aktivieren.

	![Replikation aktivieren](./media/site-recovery-hyper-v-site-to-azure/enable-replication.png)

2. Wählen Sie auf dem Blatt **Quelle** den Hyper-V-Standort aus. Klicken Sie dann auf **OK**.
3. Wählen Sie unter **Ziel** das Tresorabonnement und das Failovermodell aus, das Sie in Azure (klassisches oder Resource Manager-Modell) nach einem Failover verwenden möchten.
4. Wählen Sie das Speicherkonto aus, das Sie verwenden möchten. Wenn Sie ein anderes Speicherkonto als Ihre bereits vorhandenen Speicherkonten verwenden möchten, können Sie ein [Speicherkonto erstellen](#set-up-an-azure-storage-account). Klicken Sie zum Erstellen eines Speicherkontos mit dem ARM-Modell auf **Neu erstellen**. Wenn Sie ein Speicherkonto mit dem klassischen Modell erstellen möchten, verwenden Sie hierfür das [Azure-Portal](../storage/storage-create-storage-account-classic-portal.md). Klicken Sie dann auf **OK**.
5.  Wählen Sie das Azure-Netzwerk und das Subnetz aus, mit dem Azure-VMs eine Verbindung herstellen, wenn sie nach einem Failover erstellt werden. Wählen Sie die Option **Jetzt für die ausgewählten Computer konfigurieren** aus, um die Netzwerkeinstellung auf alle Computer anzuwenden, die geschützt werden sollen. Wählen Sie **Später konfigurieren** aus, um das Azure-Netzwerk pro Computer auszuwählen. Wenn Sie ein anderes Netzwerk als Ihre bereits vorhandenen Netzwerke verwenden möchten, können Sie [ein Netzwerk erstellen](#set-up-an-azure-network). Klicken Sie zum Erstellen eines Netzwerks mit dem ARM-Modell auf **Neu erstellen**. Wenn Sie ein Netzwerk mit dem klassischen Modell erstellen möchten, verwenden Sie das [Azure-Portal](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Wählen Sie, falls zutreffend, ein Subnetz aus. Klicken Sie dann auf **OK**.

	![Replikation aktivieren](./media/site-recovery-hyper-v-site-to-azure/enable-replication11.png)

6. Klicken Sie auf **Virtuelle Computer** > **Virtuelle Computer auswählen**, und wählen Sie die Computer aus, die Sie replizieren möchten. Sie können nur Computer auswählen, für die die Replikation aktiviert werden kann. Klicken Sie dann auf **OK**.

	![Replikation aktivieren](./media/site-recovery-hyper-v-site-to-azure/enable-replication5.png)

11. Wählen Sie unter **Eigenschaften** > **Eigenschaften konfigurieren** das Betriebssystem für die ausgewählten VMs und den Betriebssystem-Datenträger aus. Stellen Sie sicher, dass der Name des virtuellen Azure-Computers (Zielname) den [Anforderungen für virtuelle Azure-Computer](site-recovery-best-practices.md#azure-virtual-machine-requirements) entspricht, und ändern Sie diesen bei Bedarf. Klicken Sie dann auf **OK**. Sie können später weitere Eigenschaften festlegen.

	![Replikation aktivieren](./media/site-recovery-hyper-v-site-to-azure/enable-replication6.png)

12. Wählen Sie unter **Replikationseinstellungen** > **Replikationseinstellungen konfigurieren** die Replikationsrichtlinie aus, die Sie für die geschützten VMs anwenden möchten. Klicken Sie dann auf **OK**. Sie können die Replikationsrichtlinie unter **Einstellungen** > **Replikationsrichtlinien** > „Richtlinienname“ > **Einstellungen bearbeiten** ändern. Von Ihnen vorgenommene Änderungen werden für Computer, die bereits repliziert werden, und für neue Computer verwendet.

	![Replikation aktivieren](./media/site-recovery-hyper-v-site-to-azure/enable-replication7.png)

Sie können den Fortschritt des Auftrags **Schutz aktivieren** unter **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** verfolgen. Nachdem der Auftrag Schutz abschließen ausgeführt wurde, ist der Computer bereit für das Failover.

### Anzeigen und Verwalten von VM-Eigenschaften

Es wird empfohlen, dass Sie die Eigenschaften des Quellcomputers überprüfen.

1. Klicken Sie auf **Einstellungen** > **Geschützte Elemente** > **Replizierte Elemente**, und wählen Sie den Computer aus.

	![Replikation aktivieren](./media/site-recovery-hyper-v-site-to-azure/test-failover1.png)

2. Unter **Eigenschaften** können Sie die Informationen zur Replikation und zum Failover für den virtuellen Computer anzeigen.

	![Replikation aktivieren](./media/site-recovery-hyper-v-site-to-azure/test-failover2.png)

3. Unter **Compute und Netzwerk** > **Compute-Eigenschaften** können Sie den Namen und die Zielgröße des virtuellen Azure-Computers angeben. Ändern Sie ggf. den Namen, damit er die Azure-Anforderungen erfüllt. Sie können auch Informationen zum Zielnetzwerk, zum Subnetz sowie zur IP-Adresse anzeigen und ändern, die der Azure-VM zugewiesen wird. Beachten Sie Folgendes:

	- Sie können die Ziel-IP-Adresse festlegen. Wenn Sie keine Adresse angeben, wird für den Computer, für den das Failover durchgeführt wurde, DHCP verwendet. Wenn Sie eine Adresse festlegen, die beim Failover nicht verfügbar ist, tritt beim Failover ein Fehler auf. Dieselbe Ziel-IP-Adresse kann für das Testfailover verwendet werden, wenn die Adresse im Testfailover-Netzwerk verfügbar ist.
	- Die Anzahl der Netzwerkkarten hängt von der Größe ab, die Sie für den virtuellen Zielcomputer angeben. Hierbei gilt Folgendes:

		- Wenn die Anzahl der Netzwerkkarten des Quellcomputers maximal der Anzahl der Netzwerkkarten entspricht, die für die Größe des Zielcomputers zulässig ist, hat der Zielcomputer die gleiche Anzahl von Netzwerkkarten wie der Quellcomputer.
		- Wenn die Anzahl der Netzwerkadapter für den virtuellen Quellcomputer die maximal zulässige Anzahl für die Größe des Zielcomputers übersteigt, wird die Anzahl verwendet, die maximal für die Größe des Zielcomputers zulässig ist.
		- Ein Beispiel: Wenn ein Quellcomputer zwei Netzwerkkarten besitzt und der Zielcomputer aufgrund seiner Größe vier Netzwerkkarten unterstützt, erhält der Zielcomputer zwei Netzwerkkarten. Wenn der Quellcomputer dagegen zwei Netzwerkadapter besitzt und der Zielcomputer aufgrund seiner Größe nur einen Adapter unterstützt, erhält der Zielcomputer nur einen Adapter.
		- Wenn die VM über mehrere Netzwerkkarten verfügt, werden alle mit dem gleichen Netzwerk verbunden.

	![Replikation aktivieren](./media/site-recovery-hyper-v-site-to-azure/test-failover4.png)

5.	Unter **Datenträger** werden das Betriebssystem und die Datenträger auf der VM angezeigt, die repliziert wird.


## Schritt 7: Testen der Bereitstellung

Zum Testen der Bereitstellung können Sie ein Testfailover für einen einzelnen virtuellen Computer durchführen. Alternativ können Sie einen Wiederherstellungsplan erstellen, der mehrere virtuelle Computer enthält.


### Vorbereiten des Testfailovers

- Zum Ausführen eines Testfailovers empfehlen wir Folgendes: Erstellen Sie ein neues Azure-Zielnetzwerk, das von Ihrem Azure-Produktionsnetzwerk isoliert ist (Standardverhalten bei der Erstellung eines neuen Netzwerks in Azure). [Erfahren Sie mehr](site-recovery-failover.md#run-a-test-failover) über die Ausführung von Testfailovervorgängen.
- Installieren Sie den Azure-Agent auf dem geschützten Computer, um für das Failover zu Azure die beste Leistung zu erzielen. Hierdurch wird der Startvorgang beschleunigt und die Problembehandlung vereinfacht. Installieren Sie den Agent für [Linux](https://github.com/Azure/WALinuxAgent) oder [Windows](http://go.microsoft.com/fwlink/?LinkID=394789).
- Zum vollständigen Testen der Bereitstellung benötigen Sie eine entsprechende Infrastruktur, damit der replizierte Computer wie erwartet funktioniert. Wenn Sie Active Directory und DNS testen möchten, können Sie einen virtuellen Computer als Domänencontroller mit DNS erstellen und per Azure Site Recovery zu Azure replizieren. Weitere Informationen finden Sie unter [Überlegungen zum Testfailover für Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
- Beachten Sie Folgendes, wenn Sie anstelle eines Testfailovers ein ungeplantes Failover durchführen möchten:

	- Falls möglich, sollten Sie primäre Computer herunterfahren, bevor Sie ein ungeplantes Failover ausführen. Dadurch wird sichergestellt, dass die Quell- und Replikatcomputer nicht gleichzeitig ausgeführt werden.
	- Beim Durchführen eines ungeplanten Failovers wird die Datenreplikation der primären Computer beendet. Das heißt, dass nach dem Beginn des Failovers keine Datenänderungen mehr übertragen werden. Außerdem wird ein ungeplantes Failover, das auf einem Wiederherstellungsplan basiert, auch dann bis zum Ende durchgeführt, wenn ein Fehler auftritt.
	
### Vorbereiten der Verbindungsherstellung mit Azure-VMs nach dem Failover

Gehen Sie wie folgt vor, wenn Sie die Verbindung mit Azure-VMs nach dem Failover per RDP herstellen möchten:

**Auf dem lokalen Computer vor dem Failover**:

- Aktivieren Sie für den Zugriff über das Internet RDP. Stellen Sie sicher, dass TCP- und UDP-Regeln für **Öffentlich** hinzugefügt werden, und stellen Sie sicher, dass RDP unter **Windows-Firewall** > **Zugelassene Apps und Features** für alle Profile zugelassen ist.
- Aktivieren Sie für den Zugriff über eine Standort-zu-Standort-Verbindung RDP auf dem Computer, und stellen Sie sicher, dass RDP unter **Windows-Firewall** > **Zugelassene Apps und Features** für die Netzwerke vom Typ **Domäne** und **Privat** zugelassen ist.
- Installieren Sie den [Azure-VM-Agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) auf dem lokalen Computer.
- Achten Sie darauf, dass die SAN-Richtlinie des Betriebssystems auf OnlineAll festgelegt ist. [Weitere Informationen](https://support.microsoft.com/kb/3031135)
- Deaktivieren Sie den IPSec-Dienst, bevor Sie das Failover durchführen.

**Auf der Azure-VM nach einem Failover**:

- Fügen Sie der Netzwerkschnittstellenkarte, die dem virtuellen Azure-Computer zugeordnet ist, eine öffentliche IP-Adresse hinzu, um RDP zuzulassen.
- Stellen Sie sicher, dass keine Domänenrichtlinien vorhanden sind, die das Verbinden mit einem virtuellen Computer über eine öffentliche Adresse verhindern.
- Versuchen Sie, eine Verbindung herzustellen. Wenn Sie keine Verbindung herstellen können, sollten Sie überprüfen, ob der virtuelle Computer ausgeführt wird. Weitere Tipps zur Problembehandlung finden Sie in [diesem Artikel](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

Gehen Sie wie folgt vor, wenn Sie nach dem Failover auf eine Azure-VM mit Linux über einen Secure Shell-Client (ssh) zugreifen möchten:

**Auf dem lokalen Computer vor dem Failover**:

- Stellen Sie sicher, dass der Secure Shell-Dienst auf der Azure-VM so festgelegt ist, dass er beim Systemstart automatisch gestartet wird.
- Überprüfen Sie, ob die Firewallregeln eine SSH-Verbindung damit zulassen.

**Auf der Azure-VM nach einem Failover**:

- In den Netzwerksicherheitsgruppen-Regeln auf der VM nach dem Failover und für das Azure-Subnetz, mit dem die VM verbunden ist, müssen eingehende Verbindungen zum SSH-Port zulässig sein.
- Es sollte ein öffentlicher Endpunkt erstellt werden, um eingehende Verbindungen für den SSH-Port (standardmäßig TCP-Port 22) zuzulassen.
- Wenn auf die VM über eine VPN-Verbindung (Express Route- oder Standort-zu-Standort-VPN-Verbindung) zugegriffen wird, kann der Client verwendet werden, um per SSH eine direkte Verbindung mit der VM herzustellen.

### Ausführen eines Testfailovers

Gehen Sie wie folgt vor, um das Testfailover durchzuführen:

1. Klicken Sie zum Durchführen eines Failovers für einen einzelnen virtuellen Computer unter **Einstellungen** > **Replizierte Elemente** auf den virtuellen Computer und dann auf **+Testfailover**.

	![Testfailover](./media/site-recovery-hyper-v-site-to-azure/run-failover1.png)

2. Klicken Sie zum Durchführen eines Failovers für einen Wiederherstellungsplan unter **Einstellungen** > **Wiederherstellungspläne** mit der rechten Maustaste auf den Plan, und klicken Sie dann auf **Testfailover**. [Befolgen Sie diese Anweisungen](site-recovery-create-recovery-plans.md) zum Erstellen eines Wiederherstellungsplans.

3. Wählen Sie unter **Testfailover** das Azure-Netzwerk aus, mit dem Azure-VMs nach dem Failover verbunden werden.

	![Testfailover](./media/site-recovery-hyper-v-site-to-azure/run-failover2.png)

4. Klicken Sie auf **OK**, um den Failovervorgang zu starten. Sie können den Fortschritt verfolgen, indem Sie auf den virtuellen Computer klicken, um die Eigenschaften zu öffnen. Alternativ dazu können Sie unter **Einstellungen** > **Site Recovery-Aufträge** auf den Auftrag **Testfailover** klicken.
5. Gehen Sie wie folgt vor, wenn das Failover die Phase **Test abschließen** erreicht hat:
	1. Zeigen Sie den virtuellen Replikatcomputer im Azure-Portal an. Prüfen Sie, ob der virtuelle Computer erfolgreich startet.
	2. Wenn Sie den Zugriff auf virtuelle Computer aus Ihrem lokalen Netzwerk eingerichtet haben, können Sie eine Remotedesktopverbindung mit dem virtuellen Computer herstellen.
	3. Klicken Sie auf **Test abschließen**, um den Test abzuschließen.
	4. Klicken Sie auf **Notizen**, um alle Beobachtungen im Zusammenhang mit dem Test-Failover aufzuzeichnen und zu speichern.
	5. Klicken Sie auf **Das Testfailover ist abgeschlossen**. Bereinigen Sie die Testumgebung, um den virtuellen Testcomputer automatisch auszuschalten und ihn zu löschen.
	6. An diesem Punkt werden alle Elemente oder VMs gelöscht, die von Site Recovery während des Testfailovers automatisch erstellt wurden. Alle weiteren Elemente, die Sie für das Testfailover erstellt haben, werden nicht gelöscht.
	
	> [AZURE.NOTE] Sollte ein Testfailover länger als zwei Wochen ausgeführt werden, wird sein Abschluss erzwungen.

6. Nach Abschluss des Failovers sollte der Azure-Replikatcomputer im Azure-Portal unter **Virtuelle Computer** angezeigt werden. Stellen Sie sicher, dass die VM die richtige Größe hat, mit dem richtigen Netzwerk verbunden ist und ausgeführt wird.
7. Wenn Sie die [Vorbereitung für Verbindungen nach dem Failover](#prepare-to-connect-to-Azure-VMs-after-failover) durchgeführt haben, sollten Sie die Verbindung mit der Azure-VM herstellen können.


## Überwachen der Bereitstellung

Hier wird beschrieben, wie Sie die Konfigurationseinstellungen, den Status und die Integrität für die Site Recovery-Bereitstellung überwachen können:

1. Klicken Sie auf den Tresornamen, um auf das Dashboard **Zusammenfassung** zuzugreifen. In diesem Dashboard werden Site Recovery-Aufträge, Replikationsstatus, Wiederherstellungspläne, Serverzustand und Ereignisse angezeigt. Sie können die Zusammenfassung so anpassen, dass die nützlichsten Kacheln und Layouts angezeigt werden, z.B. auch der Status anderer Site Recovery- und Backup-Tresore.

	![Zusammenfassung](./media/site-recovery-hyper-v-site-to-azure/essentials.png)

2. Auf der Kachel **Integrität** können Sie Standortserver, bei denen Probleme auftreten, sowie die von Site Recovery in den letzten 24 Stunden ausgelösten Ereignisse überwachen.
3. Sie können die Replikation über die Kacheln **Replizierte Elemente**, **Wiederherstellungspläne** und **Site Recovery-Aufträge** verwalten und überwachen. Genauere Details zu Aufträgen können Sie unter **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** anzeigen.





## Nächste Schritte

Nachdem die Bereitstellung eingerichtet wurde und ausgeführt wird, können Sie sich über die unterschiedlichen Failoverarten [informieren](site-recovery-failover.md).

<!---HONumber=AcomDC_0921_2016-->