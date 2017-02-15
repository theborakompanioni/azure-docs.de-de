---
title: Failback von virtuellen VMware-Computern und physischen Servern zum lokalen Standort | Microsoft Docs
description: "Hier erfahren Sie, wie nach einem Failover von VMware-VMs und physischen Servern zu Azure ein Failback zum lokalen Standort ausgeführt wird."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: mkjain
editor: 
ms.assetid: 7ca86e21-7a5d-45ab-8f4b-e42da90f199a
ms.service: site-recovery
ms.devlang: na
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: required
ms.date: 08/22/2016
ms.author: ruturajd
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: aa1754af79df3b78d402866a2cd4ff09a2f4057b


---
# <a name="fail-back-vmware-virtual-machines-and-physical-servers-to-the-on-premises-site"></a>Failback von virtuellen VMware-Computern und physischen Servern zum lokalen Standort
> [!div class="op_single_selector"]
> * [Azure-Portal](site-recovery-failback-azure-to-vmware.md)
> * [Klassisches Azure-Portal](site-recovery-failback-azure-to-vmware-classic.md)
> * [Klassisches Azure-Portal (Vorgängerversion)](site-recovery-failback-azure-to-vmware-classic-legacy.md)
> 
> 

In diesem Artikel wird beschrieben, wie Sie für virtuelle Azure-Computer ein Failback zum lokalen Standort durchführen. Befolgen Sie die Anweisungen in diesem Artikel, wenn Sie bereit sind, Ihre virtuellen VMware-Computer oder Ihre physischen Windows-/Linux-Server nach einem Failover vom lokalen Standort auf Azure (beschrieben in diesem [Tutorial](site-recovery-vmware-to-azure-classic.md)) wieder per Failback auf den lokalen Standort zurückzuführen.

## <a name="overview"></a>Übersicht
Dieses Diagramm zeigt Aufbau und Ablauf des Failbacks in diesem Szenario.

Verwenden Sie diese Architektur, wenn der Prozessserver lokal angeordnet ist und Sie eine ExpressRoute-Verbindung verwenden.

![](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)

Verwenden Sie diese Architektur, wenn der Prozessserver in Azure angeordnet ist und wenn Sie entweder ein VPN oder eine ExpressRoute-Verbindung verwenden.

![](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.png)

Eine vollständige Liste der Ports und das Failbackarchitekturdiagramm sehen Sie in der folgenden Abbildung.

![](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

Das Failback funktioniert wie folgt:

* Nach einem Failover zu Azure erfolgt die Rückführung zum lokalen Standort per Failback in mehreren Phasen:
  * **Phase 1**: Sie schützen die virtuellen Azure-Computer erneut, damit diese zurück auf die virtuellen VMware-Computer am lokalen Standort repliziert werden. Beim erneuten Aktivieren des Schutzes wird der virtuelle Computer (Virtual Machine, VM) in eine Failback-Schutzgruppe verschoben, die beim ursprünglichen Erstellen der Failover-Schutzgruppe automatisch erstellt wurde. Wenn Sie Ihre Failover-Schutzgruppe einem Wiederherstellungsplan hinzugefügt haben, wurde auch die Failback-Schutzgruppe automatisch diesem Plan hinzugefügt.  Beim Ausführen des erneuten Schutzes legen Sie die Planung für das Failback fest.
  * **Phase 2**: Nach der Replikation Ihrer virtuellen Azure-Computer auf den lokalen Standort führen Sie ein Failover aus, um das Failback von Azure durchzuführen.
  * **Phase 3**: Nachdem Ihre Daten per Failback zurückgeführt wurden, schützen Sie die lokalen virtuellen Computer, auf die Sie das Failback durchgeführt haben, erneut, damit diese mit der Replikation nach Azure beginnen.

> [!VIDEO https://channel9.msdn.com/Blogs/Windows-Azure/Enhanced-VMware-to-Azure-Failback/player]
> 
> 

### <a name="failback-to-the-original-or-alternate-location"></a>Failback zum ursprünglichen oder zu einem anderen Standort
Wenn Sie ein Failover für einen virtuellen VMware-Computer durchgeführt haben, können Sie das Failback zur gleichen Quell-VM ausführen, sofern sie lokal noch vorhanden ist. In diesem Szenario werden beim Failback nur die geänderten Daten (Deltaänderungen) zurückgeführt. Beachten Sie Folgendes:

* Wenn Sie ein Failover für physische Server durchgeführt haben, erfolgt das Failback immer zu einem neuen virtuellen VMware-Computer.
  * Beachten Sie vor dem Ausführen eines Failbacks an einem physischen Computer, dass
  * der geschützte physische Computer beim Failover zurück von Azure zu VMware als virtueller Computer zurückkehrt
  * Sie sicherstellen, dass Sie mindestens einen Masterzielserver zusammen mit den erforderlichen ESX/ESXi-Hosts ermitteln, zu denen Sie ein Failback ausführen müssen.
* Wenn Sie ein Failback zum ursprünglichen virtuellen Computer durchführen, ist Folgendes erforderlich: 
  
  * Wenn der virtuelle Computer von einem vCenter-Server verwaltet wird, muss der ESX-Host des Masterziels Zugriff auf den Datenspeicher des virtuellen Computers haben.
  * Wenn sich der virtuelle Computer auf einem ESX-Host befindet, aber nicht von vCenter verwaltet wird, muss sich die Festplatte des virtuellen Computers in einem Datenspeicher befinden, auf den der Host des Masterziels Zugriff hat.
  * Wenn sich Ihr virtueller Computer auf einem ESX-Host befindet und nicht vCenter verwendet, müssen Sie für den ESX-Host des Masterziels eine Ermittlung ausführen, bevor Sie den erneuten Schutz ausführen. Dasselbe gilt auch bei einem Failback für physische Server.
  * Als weitere Option können Sie den lokalen virtuellen Computer (sofern er noch vorhanden ist) vor dem Ausführen des Failbacks löschen. Beim Failback wird dann ein neuer virtueller Computer auf dem gleichen Host erstellt, der als Masterziel-ESX-Host fungiert.
* Wenn Sie das Failback zu einem anderen Speicherort durchführen, werden die Daten in dem gleichen Datenspeicher und in dem gleichen ESX-Host wiederhergestellt, der vom lokalen Masterzielserver verwendet wird. 

## <a name="prerequisites"></a>Voraussetzungen
* Sie benötigen eine VMware-Umgebung, um ein Failback für virtuelle VMware-Computer und physische Server durchführen zu können. Ein Failback auf einen physischen Server wird nicht unterstützt.
* Ein Failback ist nur möglich, wenn Sie beim ursprünglichen Einrichten des Schutzes ein Azure-Netzwerk erstellt haben. Für das Failback wird eine VPN-Verbindung oder eine ExpressRoute-Verbindung von dem Azure-Netzwerk, in dem sich die virtuellen Azure-Computer befinden, mit ihrem lokalen Standort benötigt.
* Wenn die virtuellen Computer, für die Sie das Failback ausführen möchten, von einem vCenter-Server verwaltet werden, müssen Sie sicherstellen, dass auf den vCenter-Servern die erforderlichen Berechtigungen für die Ermittlung von virtuellen Computern (VMs) vorhanden sind. [Weitere Informationen](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access)
* Wenn auf einem virtuellen Computer Momentaufnahmen vorhanden sind, tritt beim Ausführen des erneuten Schutzes ein Fehler auf. Sie können die Momentaufnahmen oder die Datenträger löschen. 
* Vor dem Ausführen des Failbacks müssen Sie einige Komponenten erstellen.
  * **Erstellen Sie einen Prozessserver in Azure**. Dies ist ein virtueller Azure-Computer, den Sie erstellen und während des Failbacks fortlaufend ausführen müssen. Sie können diesen virtuellen Computer nach Abschluss des Failbacks löschen.
  * **Erstellen Sie einen Masterzielserver:** Der Masterzielserver sendet und empfängt Failbackdaten. Auf dem von Ihnen lokal erstellten Verwaltungsserver ist standardmäßig ein Masterzielserver installiert. Je nach Datenverkehrsvolumen beim Failback müssen Sie jedoch u. U. einen separaten Masterzielserver für das Failback erstellen.
  * Wenn Sie einen zusätzlichen Masterzielserver erstellen möchten, der unter Linux ausgeführt wird, müssen Sie vor dem Installieren des Masterzielservers die Linux-VM einrichten. Eine Beschreibung hierzu finden Sie weiter unten.
* Der Konfigurationsserver ist lokal erforderlich, wenn Sie ein Failback durchführen. Während des Failbacks muss der virtuelle Computer in der Konfigurationsserverdatenbank vorhanden sein, andernfalls ist das Failback nicht erfolgreich. Daher stellen Sie sicher, dass Sie die regelmäßigen geplanten Sicherungen des Servers ausführen. Im Notfall müssen Sie ihn mit der gleichen IP-Adresse wiederherstellen, damit das Failback funktioniert.

## <a name="set-up-the-process-server-in-azure"></a>Einrichten des Prozessservers in Azure
Sie müssen einen Prozessserver in Azure installieren, damit die virtuellen Azure-Computer die Daten zurück an einem lokalen Masterzielserver senden können. 

1. Wählen Sie im Site Recovery-Portal unter **Konfigurationsserver** die Option zum Hinzufügen eines neuen Prozessservers aus.
   
   ![](./media/site-recovery-failback-azure-to-vmware-classic/ps1.png)
2. Geben Sie einen Prozessservernamen sowie einen Namen und ein Kennwort an, um eine Verbindung mit dem virtuellen Azure-Computer als Administrator herzustellen. Wählen Sie in **Konfigurationsserver** den lokalen Verwaltungsserver aus, und geben Sie das Azure-Netzwerk an, in dem der Prozessserver bereitgestellt werden soll. Dies muss das Netzwerk sein, in dem sich die virtuellen Azure-Computer befinden. Geben Sie eine eindeutige IP-Adresse aus dem ausgewählten Subnetz an, und starten Sie die Bereitstellung.
   
   ![](./media/site-recovery-failback-azure-to-vmware-classic/ps2.png)
   
   Es wird ein Auftrag zum Bereitstellen des Prozessservers ausgelöst.
   
   ![](./media/site-recovery-failback-azure-to-vmware-classic/ps3.png)
   
   Nachdem der Prozessserver in Azure bereitgestellt wurde, können Sie sich mit den angegebenen Anmeldeinformationen dort anmelden. Bei der ersten Anmeldung wird ein Prozessserver-Dialogfeld angezeigt. Geben Sie die IP-Adresse des lokalen Verwaltungsservers und die zugehörige Passphrase ein. Belassen Sie die Standardeinstellung für den Port bei „443“. Sie können auch den Standardport 9443 für die Datenreplikation beibehalten, sofern Sie diese Einstellung beim Einrichten des lokalen Verwaltungsservers nicht bewusst geändert haben. 
   
   > [!NOTE]
   > Der Server wird nicht unter **VM-Eigenschaften**angezeigt. Er wird nur auf der Registerkarte **Server** des Verwaltungsservers angezeigt, bei dem er registriert wurde. Es kann etwa 10–15 Minuten dauern, bis der Prozessserver angezeigt wird.
   > 
   > 

## <a name="set-up-the-master-target-server-on-premises"></a>Einrichten des lokalen Masterzielservers
Der Masterzielserver empfängt die Failbackdaten. Auf dem lokalen Verwaltungsserver wird automatisch ein Masterzielserver installiert. Wenn Sie jedoch große Datenmengen per Failback zurückführen, müssen Sie möglicherweise einen zusätzlichen Masterzielserver einrichten. Gehen Sie hierzu wie folgt vor:

> [!NOTE]
> Wenn Sie einen Masterzielserver unter Linux installieren möchten, befolgen Sie die Anweisungen im nächsten Abschnitt.
> 
> 

1. Wenn Sie den Masterzielserver unter Windows installieren, öffnen Sie auf dem virtuellen Computer, auf dem Sie den Masterzielserver installieren, die Seite „Schnellstart“. Laden Sie anschließend die Installationsdatei für den Azure Site Recovery-Assistenten für einheitliches Setup herunter.
2. Führen Sie das Setup aus, und wählen Sie in **Vorbereitung** die Option **Add additional process servers to scale out deployment** (Weitere Prozessserver zum horizontalen Hochskalieren der Bereitstellung hinzufügen) aus.
3. Beenden Sie den Assistenten genauso wie beim [Einrichten des Verwaltungsservers](site-recovery-vmware-to-azure-classic.md#step-5-install-the-management-server). Geben Sie auf der Seite **Konfigurationsserverdetails** die IP-Adresse dieses Masterzielservers sowie eine Passphrase für den Zugriff auf den virtuellen Computer an.

### <a name="set-up-a-linux-vm-as-the-master-target-server"></a>Einrichten eines virtuellen Linux-Computers als Masterzielserver
Um den Verwaltungsserver einzurichten, auf dem der Masterzielserver als virtueller Linux-Computer ausgeführt wird, müssen Sie das Minimalbetriebssystem CentOS 6.6 installieren, die SCSI-IDs für jede SCSI-Festplatte abrufen, einige zusätzliche Pakete installieren und einige benutzerdefinierte Änderungen vornehmen.

#### <a name="install-centos-66"></a>Installieren von CentOS 6.6
1. Installieren Sie das Minimalbetriebssystem CentOS 6.6 auf der Verwaltungsserver-VM. Behalten Sie das ISO-Image im DVD-Laufwerk, und starten Sie das System. Überspringen Sie den Medientest, wählen Sie als Sprache US-Englisch aus, wählen Sie **Basic Storage Devices** (Grundlegende Speichergeräte) aus, vergewissern Sie sich, dass sich auf der Festplatte keine wichtigen Daten befinden, und klicken Sie auf **Yes** (Ja), sodass alle Daten gelöscht werden. Geben Sie den Hostnamen des Verwaltungsservers ein, und wählen Sie die Netzwerkkarte des Servers aus.  Wählen Sie im Dialogfeld **Editing System** (Bearbeitungssystem) die Option **Connect automatically** (Automatisch verbinden) aus, und fügen Sie eine statische IP-Adresse, ein Netzwerk und DNS-Einstellungen hinzu. Geben Sie eine Zeitzone und ein Stammkennwort zum Zugriff auf den Verwaltungsserver ein. 
2. Wenn Sie nach dem gewünschten Installationstyp gefragt werden, wählen Sie für die Partition **Create Custom Layout** aus. Klicken Sie anschließend auf **Next** select **Free** aus, und klicken Sie auf „Create“. Erstellen Sie die Partitionen **/**, **/var/crash** und **/home** mit **FS Type:** **ext4**. Erstellen Sie die Partition „swap“ als **FS Type: swap**.
3. Wenn bereits vorhandene Geräte gefunden werden, wird eine Warnung angezeigt. Klicken Sie auf **Format** , um das Laufwerk mit den Partitionseinstellungen zu formatieren. Klicken Sie auf **Write change to disk** , um die Partitionsänderungen zu übernehmen.
4. Wählen Sie **Install boot loader** > **Next** (Startladeprogramm installieren > Weiter) aus, um das Startladeprogramm auf der Stammpartition zu installieren.
5. Klicken Sie nach Abschluss der Installation auf **Reboot**.

#### <a name="retrieve-the-scsi-ids"></a>Abrufen der SCSI-IDs
1. Rufen Sie nach der Installation die SCSI-IDs für alle SCSI-Festplatten im virtuellen Computer ab. Fahren Sie zu diesem Zweck die Verwaltungsserver-VM herunter, klicken Sie in VMware in den VM-Eigenschaften mit der rechten Maustaste auf den VM-Eintrag, und klicken Sie anschließend auf **Edit Settings** > **Options** (Einstellungen bearbeiten > Optionen).
2. Wählen Sie **Advanced** > **General item** (Erweitert > Allgemeine Elemente) aus, und klicken Sie auf **Configuration Parameters** (Konfigurationsparameter). Diese Option ist deaktiviert, wenn der virtuelle Computer ausgeführt wird. Um sie zu aktivieren, muss der virtuelle Computer heruntergefahren werden.
3. Wenn die Zeile **disk.EnableUUID** vorhanden ist, stellen Sie sicher, dass der Wert auf **True** festgelegt ist (Groß-/Kleinschreibung wird beachtet). Wenn dies der Fall ist, können Sie den Vorgang abbrechen und den SCSI-Befehl in einem Gastbetriebssystem testen, nachdem es hochgefahren wurde. 
4. Wenn die Zeile nicht vorhanden ist, klicken Sie auf **Zeile hinzufügen**, und fügen Sie sie mit dem Wert **True** hinzu. Verwenden Sie keine doppelten Anführungszeichen.

#### <a name="install-additional-packages"></a>Installieren von zusätzlichen Paketen
Sie müssen einige zusätzliche Pakete herunterladen und installieren. 

1. Stellen Sie sicher, dass der Masterzielserver mit dem Internet verbunden ist.
2. Führen Sie den folgenden Befehl aus, um 15 Pakete aus dem CentOS-Repository herunterzuladen und zu installieren: **# yum install –y xfsprogs perl lsscsi rsync wget kexec-tools**.
3. Wenn Linux auf den zu schützenden Quellcomputern mit dem Dateisystem Reiser oder XFS für das Stamm- oder Startgerät ausgeführt wird, müssen Sie die diese zusätzlichen Pakete wie folgt herunterladen und installieren:
   
   * # <a name="cd-usrlocal"></a>cd /usr/local
   * # <a name="wget-httpelrepoorglinuxelrepoel6x8664rpmskmod-reiserfs-00-1el6elrepox8664rpmhttpelrepoorglinuxelrepoel6x8664rpmskmod-reiserfs-00-1el6elrepox8664rpm"></a>wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm)
   * # <a name="wget-httpelrepoorglinuxelrepoel6x8664rpmsreiserfs-utils-3621-1el6elrepox8664rpmhttpelrepoorglinuxelrepoel6x8664rpmsreiserfs-utils-3621-1el6elrepox8664rpm"></a>wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm)
   * # <a name="rpm-ivh-kmod-reiserfs-00-1el6elrepox8664rpm-reiserfs-utils-3621-1el6elrepox8664rpm"></a>rpm –ivh kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm
   * # <a name="wget-httpmirrorcentosorgcentos66osx8664packagesxfsprogs-311-16el6x8665rpmhttpmirrorcentosorgcentos66osx8664packagesxfsprogs-311-16el6x8665rpm"></a>wget [http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm](http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm)
   * # <a name="rpm-ivh-xfsprogs-311-16el6x8664rpm"></a>rpm –ivh xfsprogs-3.1.1-16.el6.x86_64.rpm

#### <a name="apply-custom-changes"></a>Anwenden von benutzerdefinierten Änderungen
Nachdem Sie die nach der Installation nötigen Schritte ausgeführt und die Pakete installiert haben, führen Sie folgende benutzerdefinierte Änderungen aus:

1. Kopieren Sie die Binärdatei „RHEL 6-64 Unified Agent“ auf den virtuellen Computer. Führen Sie den folgenden Befehl aus, um das tar-Archiv der Binärdatei zu entpacken: **tar -zxvf <file name>**.
2. Führen Sie den folgenden Befehl aus, um Berechtigungen zu erteilen: **# chmod 755 ./ApplyCustomChanges.sh**
3. Führen Sie das folgende Skript aus: **# ./ApplyCustomChanges.sh**. Das Skript darf nur einmal ausgeführt werden. Starten Sie den Server neu, nachdem das Skript erfolgreich ausgeführt wurde.

## <a name="run-the-failback"></a>Ausführen des Failbacks
### <a name="reprotect-the-azure-vms"></a>Erneutes Schützen der virtuellen Azure-Computer
1. Wählen Sie im Site Recovery-Portal auf der Registerkarte **Computer** den virtuellen Computer aus, für den das Failover durchgeführt wurde, und klicken Sie auf **Erneut schützen**.
2. Wählen Sie bei **Masterzielserver** und **Prozessserver** den lokalen Masterzielserver und den Azure-VM-Prozessserver aus.
3. Wählen Sie das Konto aus, das Sie für die Verbindung mit dem virtuellen Computer konfiguriert haben.
4. Wählen Sie die Failbackversion der Schutzgruppe aus. Beispiel: Wenn der virtuelle Computer in der Schutzgruppe „PG_1“ geschützt ist, müssen Sie „PG1_Failback“ auswählen.
5. Wenn die Wiederherstellung an einem anderen Speicherort erfolgen soll, wählen Sie das Aufbewahrungslaufwerk und den Datenspeicher aus, wie sie für den Masterzielserver konfiguriert wurden. Beim Ausführen des Failbacks zum lokalen Standort verwenden die virtuellen VMware-Computer im Failback-Schutzplan den gleichen Datenspeicher wie der Masterzielserver. Wenn Sie den virtuellen Azure-Replikatcomputer auf dem gleichen lokalen virtuellen Computer wiederherstellen möchten, muss sich der lokale virtuelle Computer bereits im selben Datenspeicher befinden wie der Masterzielserver. Wenn lokal kein virtueller Computer vorhanden ist, wird er beim Ausführen des erneuten Schutzes erstellt.
   
   ![](./media/site-recovery-failback-azure-to-vmware-classic/failback1.png)
6. Nachdem Sie auf **OK** geklickt haben, um den erneuten Schutz zu starten, beginnt ein Auftrag mit der Replikation des virtuellen Computers von Azure am lokalen Standort. Sie können den Fortschritt auf der Registerkarte **Aufträge** nachverfolgen.
   
   ![](./media/site-recovery-failback-azure-to-vmware-classic/failback2.png)

### <a name="run-a-failover-to-the-on-premises-site"></a>Ausführen eines Failovers zum lokalen Standort
Nach dem Ausführen des erneuten Schutzes wird der virtuelle Computer zur Failbackversion seiner Schutzgruppe verschoben und automatisch dem Wiederherstellungsplan hinzugefügt, den Sie für das Failover zu Azure genutzt haben (sofern ein solcher vorhanden ist).

1. Wählen Sie auf der Seite **Wiederherstellungspläne** den Wiederherstellungsplan aus, der die Failbackgruppe enthält, und klicken Sie auf **Failover** > **Ungeplantes Failover**.
2. Überprüfen Sie unter **Failover bestätigen** die Failoverrichtung (zu Azure), und wählen Sie den Wiederherstellungspunkt aus, der für das Failover verwendet werden soll (den aktuellsten). Wenn Sie beim Konfigurieren der Replikationseigenschaften **Multi-VM** aktiviert haben, können Sie zum letzten anwendungs- oder absturzkonsistenten Wiederherstellungspunkt wiederherstellen. Klicken Sie auf das Häkchen, um das Failover zu starten.
3. Während des Failovers fährt Site Recovery die virtuellen Azure-Computer herunter. Vergewissern Sie sich, dass das Failback wie erwartet abgeschlossen wurde, und überprüfen Sie anschließend, ob die virtuellen Azure-Computer wie erwartet heruntergefahren wurden.

### <a name="reprotect-the--on-premises-site"></a>Erneutes Schützen des lokalen Standorts
Nach Abschluss des Failbacks befinden sich Ihre Daten wieder am lokalen Standort, aber sie sind nicht geschützt. Führen Sie daher folgende Schritte aus, um die Replikation zu Azure erneut zu starten:

1. Wählen Sie im Site Recovery-Portal auf der Registerkarte **Computer** die virtuellen Computer aus, für die das Failback durchgeführt wurde, und klicken Sie auf **Erneut schützen**. 
2. Vergewissern Sie sich, dass die Replikation zu Azure erwartungsgemäß funktioniert. Wenn dies der Fall ist, können Sie die (derzeit nicht ausgeführten) virtuellen Azure-Computer löschen, für die das Failback ausgeführt wurde.

### <a name="common-issues-in-failback"></a>Häufige Probleme beim Failback
1. Wenn Sie die vCenter-Ermittlung schreibgeschützter Benutzer ausführen und virtuelle Computer schützen, funktioniert das Failover. Zum Zeitpunkt des erneuten Schützens schlägt dies fehl, da die Datenspeicher nicht ermittelt werden können. Symptom dafür ist, dass Sie die Datenspeicher beim erneuten Schützen nicht sehen. Zum Beheben dieses Problems können Sie die vCenter-Anmeldeinformationen mit einem entsprechenden Konto aktualisieren, das die Berechtigungen besitzt, und den Auftrag wiederholen. [Weitere Informationen](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access)
2. Beim Failback einer Linux-VM und ihrer lokalen Ausführung sehen Sie, dass das Netzwerk-Manager-Paket auf dem Computer deinstalliert wird. Dies liegt daran, dass beim Wiederherstellen des virtuellen Computers in Azure das Netzwerk-Manager-Paket entfernt wird.
3. Wenn ein virtueller Computer mit statischer IP-Adresse konfiguriert ist und ein Failover zu Azure ausgeführt wird, wird die IP-Adresse über DHCP abgerufen. Nach dem Failover zurück zum lokalen Standort verwendet der virtuelle Computer weiterhin DHCP zum Abrufen der IP-Adresse. Sie müssen sich manuell bei dem Computer anmelden und die IP-Adresse auf die statische Adresse zurücksetzen, falls erforderlich.
4. Wenn Sie die kostenlose Edition ESXi 5.5 oder die kostenlose Edition vSphere 6 Hypervisor verwenden, wäre das Failover erfolgreich, das Failback jedoch nicht. Sie müssen auf die Evaluierungslizenzen aktualisieren, um das Failback zu aktivieren.

## <a name="failing-back-with-expressroute"></a>Ausführen eines Failbacks mit ExpressRoute
Ein Failback kann über eine VPN-Verbindung oder über Azure ExpressRoute ausgeführt werden. Wenn Sie ExpressRoute verwenden möchten, beachten Sie Folgendes:

* ExpressRoute muss in dem virtuellen Azure-Netzwerk eingerichtet werden, zu dem das Failover der Quellcomputer durchgeführt wird und in dem sich die virtuellen Azure-Computer nach dem Failover befinden.
* Die Daten werden zu einem Azure-Speicherkonto auf einem öffentlichen Endpunkt repliziert. Sie müssen ein öffentliches Peering in ExpressRoute einrichten, bei dem das Zielrechenzentrum für die Site Recovery-Replikation ExpressRoute verwendet.




<!--HONumber=Nov16_HO3-->


