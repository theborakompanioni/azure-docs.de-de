<properties 
   pageTitle="Failback von virtuellen VMware-Computern und physischen Servern zum lokalen Standort | Microsoft Azure"
   description="Hier erfahren Sie, wie nach einem Failover von VMware-VMs und physischen Servern zu Azure ein Failback zum lokalen Standort ausgeführt wird." 
   services="site-recovery" 
   documentationCenter="" 
   authors="ruturaj" 
   manager="mkjain" 
   editor=""/>

<tags
   ms.service="site-recovery"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.workload="required" 
   ms.date="07/08/2016"
   ms.author="ruturajd"/>

# Failback von virtuellen VMware-Computern und physischen Servern zum lokalen Standort

> [AZURE.SELECTOR]
- [Azure-Portal](site-recovery-failback-azure-to-vmware.md)
- [Klassisches Azure-Portal](site-recovery-failback-azure-to-vmware-classic.md)
- [Klassisches Azure-Portal (Vorgängerversion)](site-recovery-failback-azure-to-vmware-classic-legacy.md)


In diesem Artikel wird beschrieben, wie Sie für virtuelle Azure-Computer ein Failback zum lokalen Standort durchführen. Befolgen Sie die Anweisungen in diesem Artikel, wenn Sie bereit sind, Ihre virtuellen VMware-Computer oder Ihre physischen Windows-/Linux-Server nach einem Failover vom lokalen Standort auf Azure (beschrieben in diesem [Tutorial](site-recovery-vmware-to-azure-classic.md)) wieder per Failback auf den lokalen Standort zurückzuführen.



## Übersicht

Dieses Diagramm zeigt Aufbau und Ablauf des Failbacks in diesem Szenario.

Verwenden Sie diese Architektur, wenn der Prozessserver lokal angeordnet ist und Sie eine ExpressRoute-Verbindung verwenden.

![Architekturdiagramm für ExpressRoute](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)

Verwenden Sie diese Architektur, wenn der Prozessserver in Azure angeordnet ist und wenn Sie entweder ein VPN oder eine ExpressRoute-Verbindung verwenden.

![Architekturdiagramm für VPN](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.PNG)

Eine vollständige Liste der Ports und das Failbackarchitekturdiagramm sehen Sie in der folgenden Abbildung.

![Failover-Failback alle Ports](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

Das Failback funktioniert wie folgt:

- Nach einem Failover zu Azure erfolgt die Rückführung zum lokalen Standort per Failback in mehreren Phasen:
	- **Phase 1**: Sie schützen die virtuellen Azure-Computer erneut, damit diese zurück auf die virtuellen VMware-Computer am lokalen Standort repliziert werden.
	- **Phase 2**: Nach der Replikation Ihrer virtuellen Azure-Computer auf den lokalen Standort führen Sie ein Failover aus, um das Failback von Azure durchzuführen.
	- **Phase 3**: Nachdem Ihre Daten per Failback zurückgeführt wurden, schützen Sie die lokalen virtuellen Computer, auf die Sie das Failback durchgeführt haben, erneut, damit diese mit der Replikation nach Azure beginnen.


### Failback zum ursprünglichen oder zu einem anderen Standort

Wenn Sie ein Failover für einen virtuellen VMware-Computer durchgeführt haben, können Sie das Failback zur gleichen Quell-VM ausführen, sofern sie lokal noch vorhanden ist. In diesem Szenario werden beim Failback nur die geänderten Daten (Deltaänderungen) zurückgeführt. Beachten Sie Folgendes:

- Wenn Sie ein Failover für physische Server durchgeführt haben, erfolgt das Failback immer zu einem neuen virtuellen VMware-Computer.
	- Beachten Sie vor dem Ausführen eines Failbacks an einem physischen Computer Folgendes:
		- Der geschützte physische Computer wird beim Failback von Azure zu VMware zu einem virtuellen Computer.
		- Stellen Sie sicher, dass Sie mindestens einen Masterzielserver zusammen mit den erforderlichen ESX/ESXi-Hosts ermitteln, auf die Sie ein Failback ausführen müssen.
- Wenn Sie ein Failback zum ursprünglichen virtuellen Computer durchführen, ist Folgendes erforderlich:
	- Wenn der virtuelle Computer von einem vCenter-Server verwaltet wird, muss der ESX-Host des Masterziels Zugriff auf den Datenspeicher des virtuellen Computers haben.
	- Wenn sich der virtuelle Computer auf einem ESX-Host befindet, aber nicht von vCenter verwaltet wird, muss sich die Festplatte des virtuellen Computers in einem Datenspeicher befinden, auf den der Host des Masterziels Zugriff hat.
	- Wenn sich Ihr virtueller Computer auf einem ESX-Host befindet und nicht vCenter verwendet, müssen Sie für den ESX-Host des Masterziels eine Ermittlung ausführen, bevor Sie den erneuten Schutz ausführen. Dasselbe gilt auch bei einem Failback für physische Server.
	- Als weitere Option können Sie den lokalen virtuellen Computer (sofern er noch vorhanden ist) vor dem Ausführen des Failbacks löschen. Beim Failback wird dann ein neuer virtueller Computer auf dem gleichen Host erstellt, der als Masterziel-ESX-Host fungiert.
	
- Wenn Sie das Failback zu einem anderen Speicherort durchführen, werden die Daten in dem gleichen Datenspeicher und in dem gleichen ESX-Host wiederhergestellt, der vom lokalen Masterzielserver verwendet wird.


## Voraussetzungen

- Sie benötigen eine VMware-Umgebung, um ein Failback für virtuelle VMware-Computer und physische Server durchführen zu können. Ein Failback auf einen physischen Server wird nicht unterstützt.
- Ein Failback ist nur möglich, wenn Sie beim ursprünglichen Einrichten des Schutzes ein Azure-Netzwerk erstellt haben. Für das Failback wird eine VPN-Verbindung oder eine ExpressRoute-Verbindung von dem Azure-Netzwerk, in dem sich die virtuellen Azure-Computer befinden, mit ihrem lokalen Standort benötigt.
- Wenn die virtuellen Computer, für die Sie das Failback ausführen möchten, von einem vCenter-Server verwaltet werden, müssen Sie sicherstellen, dass auf den vCenter-Servern die erforderlichen Berechtigungen für die Ermittlung von virtuellen Computern vorhanden sind. [Weitere Informationen](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access)
- Wenn auf einem virtuellen Computer Momentaufnahmen vorhanden sind, tritt beim Ausführen des erneuten Schutzes ein Fehler auf. Sie können die Momentaufnahmen oder die Datenträger löschen.
- Vor dem Ausführen des Failbacks müssen Sie einige Komponenten erstellen.
	- **Erstellen Sie einen Prozessserver in Azure**. Dies ist ein virtueller Azure-Computer, den Sie erstellen und während des Failbacks fortlaufend ausführen müssen. Sie können diesen virtuellen Computer nach Abschluss des Failbacks löschen.
	- **Erstellen Sie einen Masterzielserver**: Der Masterzielserver sendet und empfängt Failbackdaten. Auf dem von Ihnen lokal erstellten Verwaltungsserver ist standardmäßig ein Masterzielserver installiert. Je nach Datenverkehrsvolumen beim Failback müssen Sie jedoch u. U. einen separaten Masterzielserver für das Failback erstellen.
	- Wenn Sie einen zusätzlichen Masterzielserver erstellen möchten, der unter Linux ausgeführt wird, müssen Sie vor dem Installieren des Masterzielservers die Linux-VM einrichten. Eine Beschreibung hierzu finden Sie weiter unten.
- Der Konfigurationsserver ist lokal erforderlich, wenn Sie ein Failback durchführen. Während des Failbacks muss der virtuelle Computer in der Konfigurationsserverdatenbank vorhanden sein, andernfalls ist das Failback nicht erfolgreich. Daher stellen Sie sicher, dass Sie die regelmäßigen geplanten Sicherungen des Servers ausführen. Im Notfall müssen Sie ihn mit der gleichen IP-Adresse wiederherstellen, damit das Failback funktioniert.
- Stellen Sie sicher, dass Sie die Einstellung „disk.enableUUID=true“ in den Konfigurationsparametern der Masterziel-VM in VMware festlegen. Wenn diese Zeile nicht vorhanden ist, fügen Sie sie hinzu. Dies ist erforderlich, um eine konsistente UUID der VMDK-Datei bereitzustellen, damit sie ordnungsgemäß bereitgestellt wird.
- **Für den Masterzielserver kann kein speicherbezogener vMotion-Vorgang ausgeführt werden**. Dadurch kann ein Fehler beim Failback auftreten. Der virtuelle Computer wird nicht gestartet, da die Datenträger nicht für ihn verfügbar gemacht werden.

## Failbackrichtlinie
Sie werden eine Failbackrichtlinie benötigen, um zurück auf den lokalen Standort zu replizieren. Diese Richtlinie wird automatisch erstellt, wenn Sie eine vorwärts gerichtete Richtlinie erstellen. Beachten Sie Folgendes:

1. Diese Richtlinie wird während der Erstellung automatisch dem Konfigurationsserver zugeordnet.
2. Diese Richtlinie kann nicht bearbeitet werden.
3. Die festgelegten Werte der Richtlinie sind: RPO-Schwellenwert = 15 Min., Aufbewahrungszeitraum des Wiederherstellungspunkts = 24 Stunden, Momentaufnahmehäufigkeit für Anwendungskonsistenz = 60 Min. ![](./media/site-recovery-failback-azure-to-vmware-new/failback-policy.png)

## Einrichten des Prozessservers in Azure

Sie müssen einen Prozessserver in Azure installieren, damit die virtuellen Azure-Computer die Daten zurück an einem lokalen Masterzielserver senden können.

Wenn Ihre Computer als klassische Ressourcen geschützt sind (wenn es sich also bei der in Azure wiederhergestellten VM um eine klassische VM handelt), benötigen Sie einen klassischen Prozessserver in Azure. Wenn Sie die Computer mit Resource Manager als Bereitstellungstyp wiederhergestellt haben, benötigen Sie einen Prozessserver eines Resource Manager-Bereitstellungstyps. Der Typ wird von dem virtuellen Azure-Netzwerk ausgewählt, in dem Sie den Prozessserver bereitstellen.

1.  Wählen Sie im Tresor unter „Einstellungen“ > „Site Recovery-Infrastruktur“ (unter der Überschrift „Verwalten“) > **Konfigurationsserver** (unter der Überschrift „Für VMware und physische Computer“) den Konfigurationsserver aus. Klicken Sie auf „+ Prozessserver“ (im unten gezeigten Screenshot gelb hervorgehoben).

	![](./media/site-recovery-failback-azure-to-vmware-classic/add-processserver.png)

2. Wählen Sie zum Bereitstellen des Prozessservers „Bereitstellen eines Failbackprozessservers in Azure“ aus.

3. Wählen Sie das Abonnement aus, in dem Sie die Computer wiederhergestellt haben.

4. Wählen Sie als Nächstes das Azure-Netzwerk aus, in dem sich die wiederhergestellten Computer befinden. Der Prozessserver muss sich im selben Netzwerk befinden, damit die wiederhergestellten virtuellen Computer und der Prozessserver kommunizieren können.

5. Wenn Sie ein Netzwerk mit *klassischer Bereitstellung* ausgewählt haben, werden Sie dazu aufgefordert, über den Azure-Katalog einen neuen virtuellen Computer zu erstellen und den neuen Prozessserver darin zu installieren.

	![](./media/site-recovery-failback-azure-to-vmware-classic/add-classic.png)
	
	1. Der Name des Images ist *Microsoft Azure Site Recovery Process Server V2*. Stellen Sie sicher, dass Sie *Klassisch* als Bereitstellungsmodell auswählen.
	
		![](./media/site-recovery-failback-azure-to-vmware-classic/templatename.png)
	
	2. Installieren Sie den Prozessserver gemäß den [hier angegebenen](./site-recovery-vmware-to-azure-classicz.md#step-5-install-the-management-server) Schritten.
	
6. Wenn Sie das *Resource Manager*-Azure-Netzwerk auswählen, müssen Sie zum Bereitstellen des Servers die folgenden Informationen eingeben.

    1. Die Ressourcengruppe, in der Sie den Server bereitstellen möchten
	
	2. Benennen Sie den Server.
	
	3. Geben Sie dem Server einen Benutzernamen und ein Kennwort, damit Sie sich anmelden können.
	
	4. Wählen Sie das Speicherkonto, in dem Sie den Server bereitstellen möchten.
	
	5. Wählen Sie das angegebene Subnetz und die Netzwerkschnittstelle, mit der Sie eine Verbindung zu ihm herstellen. Hinweis: Sie müssen selbst eine [Netzwerkschnittstelle](../virtual-network/virtual-networks-multiple-nics.md) erstellen und diese während der Bereitstellung auswählen.
	
		![](./media/site-recovery-failback-azure-to-vmware-classic/psinputsadd.png)
	
	6. Klicken Sie auf OK. Dadurch wird ein Auftrag ausgelöst, der einen virtuellen Computer mit dem Bereitstellungstyp „Resource Manager“ mit dem Prozessserversetup erstellt. Sie müssen das Setup auf dem virtuellen Computer ausführen, um den Server auf dem Konfigurationsserver zu registrieren. Dazu müssen Sie [diese Schritte](./site-recovery-vmware-to-azure-classic.md#step-5-install-the-management-server) ausführen.

	7. Es wird ein Auftrag zum Bereitstellen des Prozessservers ausgelöst.

7. Am Ende sollte der Prozessserver auf der Seite „Konfigurationsserver“ im Abschnitt „Zugeordnete Server“ auf der Registerkarte „Prozessserver“ aufgeführt werden. ![](./media/site-recovery-failback-azure-to-vmware-new/pslistingincs.png)

		
	>[AZURE.NOTE] Der Server wird nicht unter **VM-Eigenschaften** angezeigt. Er wird nur auf der Registerkarte **Server** des Verwaltungsservers angezeigt, bei dem er registriert wurde. Es kann etwa 10–15 Minuten dauern, bis der Prozessserver angezeigt wird.


## Einrichten des lokalen Masterzielservers

Der Masterzielserver empfängt die Failbackdaten. Auf dem lokalen Verwaltungsserver wird automatisch ein Masterzielserver installiert. Wenn Sie jedoch große Datenmengen per Failback zurückführen, müssen Sie möglicherweise einen zusätzlichen Masterzielserver einrichten. Gehen Sie hierzu wie folgt vor:

>[AZURE.NOTE] Wenn Sie einen Masterzielserver unter Linux installieren möchten, befolgen Sie die Anweisungen im nächsten Abschnitt.

1. Wenn Sie den Masterzielserver unter Windows installieren, öffnen Sie auf dem virtuellen Computer, auf dem Sie den Masterzielserver installieren, die Seite „Schnellstart“. Laden Sie anschließend die Installationsdatei für den Azure Site Recovery-Assistenten für einheitliches Setup herunter.
2. Führen Sie das Setup aus, und wählen Sie in **Vorbereitung** die Option **Weitere Prozessserver zum horizontalen Hochskalieren der Bereitstellung hinzufügen** aus.
3. Beenden Sie den Assistenten genauso wie beim [Einrichten des Verwaltungsservers](site-recovery-vmware-to-azure-classic.md#step-5-install-the-management-server). Geben Sie auf der Seite **Konfigurationsserverdetails** die IP-Adresse dieses Masterzielservers sowie eine Passphrase für den Zugriff auf den virtuellen Computer an.

### Einrichten eines virtuellen Linux-Computers als Masterzielserver
Zum Einrichten des Verwaltungsservers, auf dem der Masterzielserver als virtueller Linux-Computer ausgeführt wird, müssen Sie das Minimalbetriebssystem CentOS 6.6 installieren, die SCSI-IDs für jede SCSI-Festplatte abrufen, einige zusätzliche Pakete installieren und einige benutzerdefinierte Änderungen vornehmen.

#### Installieren von CentOS 6.6

1.	Installieren Sie das Minimalbetriebssystem CentOS 6.6 auf der Verwaltungsserver-VM. Behalten Sie das ISO-Image im DVD-Laufwerk, und starten Sie das System. Überspringen Sie den Medientest, wählen Sie als Sprache US-Englisch aus, wählen Sie **Basic Storage Devices** aus, vergewissern Sie sich, dass sich auf der Festplatte keine wichtigen Daten befinden, und klicken Sie auf **Yes**, wodurch alle Daten gelöscht werden. Geben Sie den Hostnamen des Verwaltungsservers ein, und wählen Sie die Netzwerkkarte des Servers aus. Wählen Sie im Dialogfeld **Editing System** die Option **Connect automatically** aus, und fügen Sie eine statische IP-Adresse, ein Netzwerk und DNS-Einstellungen hinzu. Geben Sie eine Zeitzone und ein Stammkennwort zum Zugriff auf den Verwaltungsserver ein.
2.	Wenn Sie nach dem gewünschten Installationstyp gefragt werden, wählen Sie für die Partition **Create Custom Layout** aus. Klicken Sie anschließend auf **Next**, wählen Sie **Free** aus, und klicken Sie auf „Create“. Erstellen Sie die Partitionen **/**, **/var/crash** und **/home** mit **FS Type:** **ext4**. Erstellen Sie die Partition „swap“ als **FS Type: swap**.
3.	Wenn bereits vorhandene Geräte gefunden werden, wird eine Warnung angezeigt. Klicken Sie auf **Format**, um das Laufwerk mit den Partitionseinstellungen zu formatieren. Klicken Sie auf **Write change to disk**, um die Partitionsänderungen zu übernehmen.
4.	Wählen Sie **Install boot loader** > **Next**, um das Startladeprogramm auf der Stammpartition zu installieren.
5.	Klicken Sie nach Abschluss der Installation auf **Reboot**.


#### Abrufen der SCSI-IDs

1. Rufen Sie nach der Installation die SCSI-IDs für alle SCSI-Festplatten im virtuellen Computer ab. Fahren Sie zu diesem Zweck die Verwaltungsserver-VM herunter, klicken Sie in VMware in den VM-Eigenschaften mit der rechten Maustaste auf den VM-Eintrag, und klicken Sie anschließend auf **Einstellungen bearbeiten** > **Optionen**.
2. Wählen Sie **Advanced** > **General item** aus, und klicken Sie auf **Configuration Parameters**. Diese Option ist deaktiviert, wenn der virtuelle Computer ausgeführt wird. Um sie zu aktivieren, muss der virtuelle Computer heruntergefahren werden.
3. Wenn die Zeile **disk.EnableUUID** vorhanden ist, sollten Sie sicherstellen, dass der Wert auf **True** festgelegt ist (Groß-/Kleinschreibung wird beachtet). Wenn dies der Fall ist, können Sie den Vorgang abbrechen und den SCSI-Befehl in einem Gastbetriebssystem testen, nachdem es hochgefahren wurde.
4. Wenn die Zeile nicht vorhanden ist, klicken Sie auf **Add Row**, und fügen Sie sie mit dem Wert **True** hinzu. Verwenden Sie keine doppelten Anführungszeichen.

#### Installieren von zusätzlichen Paketen

Sie müssen einige zusätzliche Pakete herunterladen und installieren.

1.	Stellen Sie sicher, dass der Masterzielserver mit dem Internet verbunden ist.
2.	Führen Sie den folgenden Befehl aus, um 15 Pakete aus dem CentOS-Repository herunterzuladen und zu installieren: **# yum install –y xfsprogs perl lsscsi rsync wget kexec-tools**.
3.	Wenn Linux auf den zu schützenden Quellcomputern mit dem Dateisystem Reiser oder XFS für das Stamm- oder Startgerät ausgeführt wird, müssen Sie diese zusätzlichen Pakete herunterladen und wie folgt installieren:

	- # cd /usr/local
	- # wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm)
	- # wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm)
	- # rpm –ivh kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm
	- # wget [http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm](http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm)
	- # rpm –ivh xfsprogs-3.1.1-16.el6.x86\_64.rpm

#### Anwenden von benutzerdefinierten Änderungen

Nachdem Sie die nach der Installation nötigen Schritte ausgeführt und die Pakete installiert haben, führen Sie folgende benutzerdefinierte Änderungen aus:

1.	Kopieren Sie die Binärdatei „RHEL 6-64 Unified Agent“ auf den virtuellen Computer. Führen Sie für die Binärdatei den folgenden Untar-Befehl aus: **tar -zxvf <Dateiname>**
2.	Führen Sie den folgenden Befehl aus, um Berechtigungen zu erteilen: **# chmod 755 ./ApplyCustomChanges.sh**
3.	Führen Sie das folgende Skript aus: **# ./ApplyCustomChanges.sh**. Das Skript darf nur einmal ausgeführt werden. Starten Sie den Server neu, nachdem das Skript erfolgreich ausgeführt wurde.


## Ausführen des Failbacks

### Erneutes Schützen der virtuellen Azure-Computer

1.	Wählen Sie im Tresor unter den replizierten Elementen den virtuellen Computer aus, für den ein Failover ausgeführt wurde, und klicken Sie mit der rechten Maustaste auf **Erneut schützen**. Sie können auch auf den Computer klicken und „reprotect“ (Erneut schützen) aus den Befehlsschaltflächen auswählen.
2.	Auf dem Blatt können Sie sehen, dass die Schutzrichtung „Azure auf lokal“ bereits ausgewählt ist.
3.  Wählen Sie bei **Masterzielserver** und **Prozessserver** den lokalen Masterzielserver und den Azure-VM-Prozessserver aus.
4.  Wählen Sie den **Datenspeicher** aus, in dem die Datenträger lokal wiederhergestellt werden sollen. Diese Option wird verwendet, wenn der lokale virtuelle Computer gelöscht wird und neue Datenträger erstellt werden müssen. Diese Option wird ignoriert, wenn die Datenträger bereits vorhanden sind. Sie müssen jedoch immer noch einen Wert angeben.
5.	Das Aufbewahrungslaufwerk wird verwendet, um die Zeitpunkte festzuhalten, wenn von der VM zurück auf den lokalen Standort repliziert wird. Einige der Kriterien eines Aufbewahrungslaufwerks sind nachstehend beschrieben. Ohne diese wird das Laufwerk nicht für den Masterzielserver aufgelistet. a. Das Volume darf nicht für andere Zwecke (Ziel für die Replikation usw.) verwendet werden. b. Das Volume darf sich nicht im Sperrmodus befinden. c. Bei dem Volume darf es sich nicht um ein Cachevolume handeln. (Die Masterzielserver-Installation darf auf diesem Volume nicht vorhanden sein. Das Volume für die benutzerdefinierte Prozessserver- und Masterzielserver-Installation kann für das Aufbewahrungsvolume nicht ausgewählt werden. Das hier installierte Prozessserver- und Masterzielserver-Volume ist ein Cachevolume des Masterzielservers) d. Das Dateisystem des Volumes darf nicht FAT und FAT32 sein. e. Die Volumekapazität muss größer als 0 sein. e. Das Standardaufbewahrungsvolume für Windows ist das R-Volume. f. Das Standardaufbewahrungsvolume für Linux ist „/mnt/retention“.

6.  Die Failbackrichtlinie wird automatisch ausgewählt.

7.	Nachdem Sie auf **OK** geklickt haben, um den erneuten Schutz zu starten, beginnt ein Auftrag mit der Replikation des virtuellen Computers von Azure zum lokalen Standort. Sie können den Fortschritt auf der Registerkarte **Aufträge** nachverfolgen.

Wenn die Wiederherstellung an einem anderen Speicherort erfolgen soll, wählen Sie das Aufbewahrungslaufwerk und den Datenspeicher aus, wie sie für den Masterzielserver konfiguriert wurden. Beim Ausführen des Failbacks zum lokalen Standort verwenden die virtuellen VMware-Computer im Failback-Schutzplan den gleichen Datenspeicher wie der Masterzielserver. Wenn Sie den virtuellen Azure-Replikatcomputer auf dem gleichen lokalen virtuellen Computer wiederherstellen möchten, muss sich der lokale virtuelle Computer bereits im selben Datenspeicher befinden wie der Masterzielserver. Wenn lokal kein virtueller Computer vorhanden ist, wird beim Ausführen des erneuten Schutzes ein neuer Computer erstellt. ![](./media/site-recovery-failback-azure-to-vmware-new/reprotectinputs.png)



Sie können den erneuten Schutz auch auf einer Wiederherstellungsplanebene ausführen. Wenn Sie über eine Replikationsgruppe verfügen, können Sie diese nur mithilfe eines Wiederherstellungsplans erneut schützen. Beim erneuten Schützen über einen Wiederherstellungsplan müssen Sie die obigen Werte für jeden geschützten Computer angeben.

>[AZURE.NOTE] Eine Replikationsgruppe muss mithilfe desselben Masterzielservers erneut geschützt werden. Replikationsgruppen können nicht gleichzeitig erneut geschützt werden, wenn Sie dafür verschiedene Masterzielserver verwenden.

### Ausführen eines Failovers zum lokalen Standort

Nach dem erneuten Schützen der VM können Sie ein Failover von Azure auf den lokalen Standort initiieren.

1.	Wählen Sie auf der Seite mit den replizierten Elementen den virtuellen Computer aus, und klicken Sie mit der rechten Maustaste auf **Ungeplantes Failover**.
2.	Überprüfen Sie unter **Failover bestätigen** die Failoverrichtung (von Azure), und wählen Sie den Wiederherstellungspunkt aus, der für das Failover verwendet werden soll (den neuesten oder den neuesten anwendungskonsistenten Wiederherstellungspunkt). Der anwendungskonsistente Punkt würde vor dem aktuellsten Zeitpunkt liegen und einigen Datenverlust verursachen.
3.	Während des Failovers fährt Site Recovery die virtuellen Azure-Computer herunter. Vergewissern Sie sich, dass das Failback wie erwartet abgeschlossen wurde, und überprüfen Sie anschließend, ob die virtuellen Azure-Computer wie erwartet heruntergefahren wurden.

### Erneutes Schützen des lokalen Standorts

Nachdem das Failback abgeschlossen ist, müssen Sie den virtuellen Computer committen, um sicherzustellen, dass die in Azure wiederhergestellten VMs gelöscht wurden.

1. Klicken Sie mit der rechten Maustaste auf das geschützte Element, und klicken Sie dann auf „Commit“ (Committen). Ein Auftrag wird ausgelöst, der die vorher in Azure wiederhergestellten virtuellen Computer entfernt.

Nach dem Committen befinden sich Ihre Daten wieder am lokalen Standort, sie sind jedoch nicht geschützt. Führen Sie daher folgende Schritte aus, um die Replikation zu Azure erneut zu starten:

1.	Wählen Sie im Tresor unter „Einstellung“ > „Replizierte Elemente“ die virtuellen Computer aus, für die das Failback durchgeführt wurde, und klicken Sie auf **Erneut schützen**.
2.  Geben Sie den Wert des Prozessservers an, der zum Zurücksenden von Daten an Azure verwendet werden muss.
3.	Klicken Sie auf „OK“, um den Auftrag zum erneuten Schützen zu starten.

Nachdem das erneute Schützen abgeschlossen ist, wird die VM zurück nach Azure repliziert, und Sie können ein Failover ausführen.


### Häufige Probleme beim Failback

1. Wenn Sie die vCenter-Ermittlung schreibgeschützter Benutzer ausführen und virtuelle Computer schützen, funktioniert das Failover. Zum Zeitpunkt des erneuten Schützens schlägt dies fehl, da die Datenspeicher nicht ermittelt werden können. Symptom dafür ist, dass Sie die Datenspeicher beim erneuten Schützen nicht sehen. Zum Beheben dieses Problems können Sie die vCenter-Anmeldeinformationen mit einem entsprechenden Konto aktualisieren, das die Berechtigungen besitzt, und den Auftrag wiederholen. [Weitere Informationen](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access)
2. Wenn Sie für eine Linux-VM ein Failback ausführen und sie lokal ausführen, sehen Sie, dass das Netzwerk-Manager-Paket auf dem Computer deinstalliert wird. Dies liegt daran, dass beim Wiederherstellen des virtuellen Computers in Azure das Netzwerk-Manager-Paket entfernt wird.
3. Wenn ein virtueller Computer mit statischer IP-Adresse konfiguriert ist und ein Failover zu Azure ausgeführt wird, wird die IP-Adresse über DHCP abgerufen. Nach dem Failover zurück zum lokalen Standort verwendet der virtuelle Computer weiterhin DHCP zum Abrufen der IP-Adresse. Sie müssen sich manuell bei dem Computer anmelden und die IP-Adresse auf die statische Adresse zurücksetzen, falls erforderlich.
4. Wenn Sie die kostenlose Edition ESXi 5.5 oder die kostenlose Edition vSphere 6 Hypervisor verwenden, wäre das Failover erfolgreich, das Failback jedoch nicht. Sie müssen auf eine der Evaluierungslizenzen aktualisieren, um das Failback zu aktivieren.

## Ausführen eines Failbacks mit ExpressRoute

Ein Failback kann über eine VPN-Verbindung oder über Azure ExpressRoute ausgeführt werden. Wenn Sie ExpressRoute verwenden möchten, beachten Sie Folgendes:

- ExpressRoute muss in dem virtuellen Azure-Netzwerk eingerichtet werden, zu dem das Failover der Quellcomputer durchgeführt wird und in dem sich die virtuellen Azure-Computer nach dem Failover befinden.
- Die Daten werden zu einem Azure-Speicherkonto auf einem öffentlichen Endpunkt repliziert. Sie müssen ein öffentliches Peering in ExpressRoute einrichten, bei dem das Zieldatencenter für die Site Recovery-Replikation ExpressRoute verwendet.

<!---HONumber=AcomDC_0817_2016-->