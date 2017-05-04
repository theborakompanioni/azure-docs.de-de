---
title: "Failback für virtuelle VMware-Computer von Azure zur lokalen Infrastruktur | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie nach einem Failover von VMware-VMs und physischen Servern zu Azure ein Failback zum lokalen Standort ausgeführt wird."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: mkjain
editor: 
ms.assetid: 5a47337f-89a9-43e8-8fdc-7da373c0fb0f
ms.service: site-recovery
ms.devlang: na
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: required
ms.date: 03/27/2017
ms.author: ruturajd
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 3bd182a775377f912914c0c7a63fe41811146e1a
ms.lasthandoff: 04/27/2017


---
# <a name="fail-back-vmware-virtual-machines-and-physical-servers-to-the-on-premises-site"></a>Failback von virtuellen VMware-Computern und physischen Servern zum lokalen Standort
> [!div class="op_single_selector"]
> * [VMware-/physische Computer von Azure](site-recovery-how-to-failback-azure-to-vmware.md)
> * [Hyper-V-VMs von Azure](site-recovery-failback-from-azure-to-hyper-v.md)

In diesem Artikel wird beschrieben, wie Sie für virtuelle Azure-Computer ein Failback auf den lokalen Standort durchführen. Befolgen Sie die Anweisungen hier, wenn Sie bereit sind, Ihre virtuellen VMware-Computer oder Ihre physischen Windows-/Linux-Server wieder per Failback zurückzuführen, nachdem Sie Ihre Computer mithilfe dieses [Verweises](site-recovery-how-to-reprotect.md) erneut geschützt haben.

>[!NOTE]
>Wenn Sie das klassische Azure-Portal verwenden, nutzen Sie die [hier](site-recovery-failback-azure-to-vmware-classic.md) genannten Anweisungen für verbesserte VMware-zu-Azure-Architektur und die [hier](site-recovery-failback-azure-to-vmware-classic-legacy.md) genannten Anweisungen für die ältere Architektur.

## <a name="overview"></a>Übersicht
Die Diagramme in diesem Abschnitt zeigen Aufbau und Ablauf des Failbacks in diesem Szenario.

Verwenden Sie diese Architektur, wenn der Prozessserver lokal ausgeführt wird und Sie eine Azure ExpressRoute-Verbindung verwenden:

![Architekturdiagramm für ExpressRoute](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)

Verwenden Sie diese Architektur, wenn sich der Prozessserver in Azure befindet und Sie entweder ein VPN oder eine ExpressRoute-Verbindung verwenden:

![Architekturdiagramm für VPN](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.png)

Eine vollständige Liste der Ports und das Failbackarchitekturdiagramm sehen Sie auf der folgenden Abbildung:

![Failover-Failback-Liste aller Ports](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

Nach einem Failover zu Azure erfolgt die Rückführung zum lokalen Standort per Failback in drei Phasen:

* **Phase 1**: Sie schützen die virtuellen Azure-Computer erneut, damit diese zurück auf die virtuellen VMware-Computer am lokalen Standort repliziert werden.
* **Phase 2**: Nach der Replikation Ihrer virtuellen Azure-Computer auf den lokalen Standort führen Sie ein Failover aus, um das Failback von Azure durchzuführen.
* **Phase 3**: Nachdem Ihre Daten per Failback zurückgeführt wurden, schützen Sie die lokalen virtuellen Computer, auf die Sie das Failback durchgeführt haben, erneut, damit diese mit der Replikation nach Azure beginnen.

### <a name="fail-back-to-the-original-location-or-an-alternate-location"></a>Failback zum ursprünglichen oder zu einem anderen Standort
Nachdem Sie ein Failover für einen virtuellen VMware-Computer durchgeführt haben, können Sie das Failback zum gleichen virtuellen Quellcomputer ausführen, sofern er lokal noch vorhanden ist. In diesem Szenario werden beim Failback nur die Deltas zurückgeführt.

Wenn Sie ein Failover für physische Server durchgeführt haben, erfolgt das Failback immer zu einem neuen virtuellen VMware-Computer. Beachten Sie vor dem Ausführen eines Failbacks an einem physischen Computer Folgendes:
* Ein geschützter physischer Computer wird beim Failback von Azure zu VMware zu einem virtuellen Computer. Für einen geschützten physischen Computer unter Windows Server 2008 R2 SP1, für den ein Failover auf Azure ausgeführt wurde, kann kein Failback ausgeführt werden. Für einen Windows Server 2008 R2 SP1-Computer, der lokal als virtueller Computer ausgeführt wird, kann ein Failback ausgeführt werden.
* Stellen Sie sicher, dass Sie mindestens einen Masterzielserver zusammen mit den erforderlichen ESX/ESXi-Hosts ermitteln, auf die Sie ein Failback ausführen müssen.

Wenn Sie ein Failback zum ursprünglichen virtuellen Computer durchführen, ist Folgendes erforderlich:

* Wenn der virtuelle Computer von einem vCenter-Server verwaltet wird, muss der ESX-Host des Masterziels Zugriff auf den Datenspeicher des virtuellen Computers haben.
* Wenn sich der virtuelle Computer auf einem ESX-Host befindet, aber nicht von vCenter verwaltet wird, muss sich die Festplatte des virtuellen Computers in einem Datenspeicher befinden, auf den der Host des Masterziels Zugriff hat.
* Wenn sich Ihr virtueller Computer auf einem ESX-Host befindet und nicht vCenter verwendet, müssen Sie für den ESX-Host des Masterziels eine Ermittlung ausführen, bevor Sie den erneuten Schutz ausführen. Dasselbe gilt auch bei einem Failback für physische Server.
* Als weitere Option können Sie den lokalen virtuellen Computer (sofern er noch vorhanden ist) vor dem Ausführen des Failbacks löschen. Beim Failback wird dann ein neuer virtueller Computer auf dem gleichen Host erstellt, der als Masterziel-ESX-Host fungiert.

Wenn Sie das Failback zu einem anderen Speicherort durchführen, werden die Daten in dem gleichen Datenspeicher und in dem gleichen ESX-Host wiederhergestellt, der vom lokalen Masterzielserver verwendet wird.

## <a name="prerequisites"></a>Voraussetzungen
* Für das Durchführen von Failbacks für virtuelle VMware-Computer und physische Server benötigen Sie eine VMware-Umgebung. Ein Failback auf einen physischen Server wird nicht unterstützt.
* Ein Failback ist nur möglich, wenn Sie beim ursprünglichen Einrichten des Schutzes ein Azure-Netzwerk erstellt haben. Für das Failback wird eine VPN-Verbindung oder eine ExpressRoute-Verbindung von dem Azure-Netzwerk, in dem sich die virtuellen Azure-Computer befinden, mit ihrem lokalen Standort benötigt.
* Wenn die virtuellen Computer, für die Sie das Failback ausführen möchten, von einem vCenter-Server verwaltet werden, stellen Sie sicher, dass auf den vCenter-Servern die erforderlichen Berechtigungen für die Ermittlung von virtuellen Computern vorhanden sind. Weitere Informationen finden Sie unter [Replizieren von virtuellen VMware-Computern und physischen Servern in Azure mithilfe von Azure Site Recovery](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access).
* Wenn auf einem virtuellen Computer Momentaufnahmen vorhanden sind, tritt beim Ausführen des erneuten Schutzes ein Fehler auf. Sie können die Momentaufnahmen oder die Datenträger löschen.
* Vor dem Ausführen des Failbacks müssen Sie diese Komponenten erstellen:
  * **Erstellen Sie einen Prozessserver in Azure**. Diese Komponente ist ein virtueller Azure-Computer, den Sie erstellen und während des Failbacks fortlaufend ausführen. Sie können den virtuellen Computer nach Abschluss des Failbacks löschen.
  * **Erstellen Sie einen Masterzielserver:** Der Masterzielserver sendet und empfängt Failbackdaten. Auf dem von Ihnen lokal erstellten Verwaltungsserver ist standardmäßig ein Masterzielserver installiert. Je nach Datenverkehrsvolumen beim Failback müssen Sie jedoch u.U. einen separaten Masterzielserver für das Failback erstellen.
  * Zum Erstellen eines zusätzlichen Masterzielservers, der unter Linux ausgeführt wird, müssen Sie vor dem Installieren des Masterzielservers den virtuellen Linux-Computer einrichten. Eine Beschreibung hierzu finden Sie weiter unten.
* Ein Konfigurationsserver ist lokal erforderlich, wenn Sie ein Failback durchführen. Während des Failbacks muss der virtuelle Computer in der Konfigurationsserverdatenbank vorhanden sein. Wenn die Serverkonfigurationsdatenbank keinen virtuellen Computer enthält, ist kein erfolgreiches Failback möglich. Stellen Sie daher sicher, dass Sie die regelmäßigen geplanten Sicherungen des Servers durchführen. Im Notfall müssen Sie ihn mit der gleichen IP-Adresse wiederherstellen, damit das Failback funktioniert.
* Legen Sie in den **Konfigurationsparametern** des virtuellen Masterzielcomputers in VMware die Einstellung „disk.enableUUID=true“ fest. Wenn diese Zeile nicht vorhanden ist, fügen Sie sie hinzu. Dies ist erforderlich, um einen konsistenten Universally Unique Identifier (UUID) für die VMDK-Datei (Datenträger des virtuellen Computers) bereitzustellen, damit sie ordnungsgemäß bereitgestellt wird.
* Beachten Sie, dass für den Masterzielserver kein speicherbezogener vMotion-Vorgang ausgeführt werden kann und dadurch u.U. ein Fehler beim Failback auftritt. Der virtuelle Computer kann nicht gestartet werden, da die Datenträger nicht für ihn verfügbar gemacht werden.
* Fügen Sie auf dem Masterzielserver ein Laufwerk hinzu; dieses wird als „Aufbewahrungslaufwerk“ bezeichnet. Fügen Sie einen Datenträger hinzu, und formatieren Sie das Laufwerk.

## <a name="failback-policy"></a>Failbackrichtlinie
Sie benötigen eine Failbackrichtlinie, um zurück auf den lokalen Standort zu replizieren. Die Richtlinie wird automatisch erstellt, wenn Sie eine vorwärts gerichtete Richtlinie erstellen und wird dem Konfigurationsserver automatisch zugeordnet. Sie kann nicht bearbeitet werden. Für die Richtlinie gelten die folgenden Replikationseinstellungen:

* RPO-Schwellenwert = 15 Minuten
* Aufbewahrungszeitraum des Wiederherstellungspunkts = 24 Stunden
* Häufigkeit von Momentaufnahmen für App-Konsistenz = 60 Minuten

 ![Replikationseinstellungen für die Failbackrichtlinie](./media/site-recovery-failback-azure-to-vmware-new/failback-policy.png)

## <a name="set-up-a-process-server-in-azure"></a>Einrichten eines Prozessservers in Azure
Installieren Sie einen Prozessserver in Azure, damit die virtuellen Azure-Computer die Daten zurück an dem lokalen Masterzielserver senden können.

Wenn Ihre virtuellen Computer als klassische Ressourcen geschützt sind (d.h. wenn der in Azure wiederhergestellte virtuelle Computer mit dem klassischen Bereitstellungsmodell erstellt wurde), benötigen Sie einen Prozessserver in Azure. Wenn Sie die virtuellen Computer mit Azure Resource Manager als Bereitstellungstyp wiederhergestellt haben, muss auch der Prozessserver vom Bereitstellungstyp „Resource Manager“ sein. Der Bereitstellungstyp wird vom virtuellen Azure-Netzwerk ausgewählt, in dem Sie den Prozessserver bereitstellen.

1. Wählen Sie in **Tresor** > **Einstellungen** > **Site Recovery-Infrastruktur** (unter **Verwalten**) > **Konfigurationsserver** (unter **Für VMware und physische Computer**) den Konfigurationsserver aus.
2. Klicken Sie auf **Prozessserver**.

  ![Schaltfläche „Prozessserver“](./media/site-recovery-failback-azure-to-vmware-classic/add-processserver.png)
3. Wählen Sie zum Bereitstellen des Prozessservers **Bereitstellen eines Failbackprozessservers in Azure** aus.
4. Wählen Sie das Abonnement aus, in dem Sie die virtuellen Computer wiederhergestellt haben.
5. Wählen Sie das Azure-Netzwerk aus, in dem Sie die virtuellen Computer wiederhergestellt haben. Der Prozessserver muss sich im selben Netzwerk befinden, damit die wiederhergestellten virtuellen Computer und der Prozessserver kommunizieren können.
6. Wenn Sie ein Netzwerk mit dem *klassischen Bereitstellungsmodell* ausgewählt haben, erstellen Sie über Azure Marketplace einen virtuellen Computer und installieren dann den Prozessserver darin.

 ![Das Fenster „Prozessserver hinzufügen“](./media/site-recovery-failback-azure-to-vmware-classic/add-classic.png)

 Achten Sie beim Erstellen des Prozessservers auf Folgendes:
 * Der Name des Images ist *Microsoft Azure Site Recovery Process Server V2*. Wählen Sie **Klassisch** als Bereitstellungsmodell aus.

       ![Select "Classic" as the Process Server deployment model](./media/site-recovery-failback-azure-to-vmware-classic/templatename.png)
 * Installieren Sie den Prozessserver gemäß den Anweisungen in [Replizieren von virtuellen VMware-Computern und physischen Servern in Azure mithilfe von Azure Site Recovery](site-recovery-vmware-to-azure-classic.md#step-5-install-the-management-server).
7. Wenn Sie *Resource Manager* als Modell für das Azure-Netzwerk auswählen, geben Sie beim Bereitstellen des Prozessservers die folgenden Informationen an:

  * Name der Ressourcengruppe, in der Sie den Server bereitstellen möchten
  * Name des Servers
  * Einen Benutzernamen und ein Kennwort für die Anmeldung am Server
  * Speicherkonto, in dem Sie den Server bereitstellen möchten
  * Subnetz und Netzwerkschnittstelle, die eine Verbindung damit herstellen soll
   >[!NOTE]
   >Sie müssen selbst eine [Netzwerkschnittstelle](../virtual-network/virtual-networks-multiple-nics.md) (NIC) erstellen und diese während der Bereitstellung des Prozessservers auswählen.

    ![Eingeben von Informationen im Dialogfeld „Prozessserver hinzufügen“](./media/site-recovery-failback-azure-to-vmware-classic/psinputsadd.png)

8. Klicken Sie auf **OK**. Dadurch wird ein Auftrag ausgelöst, der während der Einrichtung des Prozessservers einen virtuellen Computer mit dem Bereitstellungstyp „Resource Manager“ erstellt. Zum Registrieren des Servers auf dem Konfigurationsserver führen Sie das Setup auf dem virtuellen Computer aus. Folgen Sie dabei den Anweisungen in [Replizieren von virtuellen VMware-Computern und physischen Servern in Azure mithilfe von Azure Site Recovery](site-recovery-vmware-to-azure-classic.md#step-5-install-the-management-server). Ein Auftrag zum Bereitstellen des Prozessservers wird ausgelöst.

  Der Prozessserver ist auf der Registerkarte  **Konfigurationsserver** > **Zugeordnete Server** > **Prozessserver**.

    ![](./media/site-recovery-failback-azure-to-vmware-new/pslistingincs.png)

    > [!NOTE]
    > Unter den **Eigenschaften des virtuellen Computers** wird der Prozessserver nicht angezeigt. Er wird nur auf der Registerkarte **Server** des Verwaltungsservers angezeigt, bei dem er registriert ist. Es kann 10 bis 15 Minuten dauern, bis der Prozessserver angezeigt wird.


## <a name="set-up-the-master-target-server-on-premises"></a>Einrichten des lokalen Masterzielservers
Der Masterzielserver empfängt die Failbackdaten. Der Server wird automatisch auf dem lokalen Verwaltungsserver installiert. Wenn Sie jedoch zu viele Daten per Failback zurückführen, müssen Sie möglicherweise einen zusätzlichen Masterzielserver einrichten. Gehen Sie wie folgt vor, um einen lokalen Masterzielserver einzurichten:

> [!NOTE]
> Wenn Sie einen Masterzielserver unter Linux installieren möchten, befolgen Sie die Anweisungen im nächsten Verfahren. Verwenden Sie nur das Minimalbetriebssystem CentOS 6.6 als Betriebssystem des Masterzielservers.

1. Wenn Sie den Masterzielserver unter Windows einrichten, öffnen Sie auf dem virtuellen Computer, auf dem Sie den Masterzielserver installieren, die Seite „Schnellstart“.
2. Laden Sie dann die Installationsdatei für den Azure Site Recovery-Assistenten für einheitliches Setup herunter.
3. Führen Sie das Setup aus, und wählen Sie in **Vorbereitung** die Option **Add additional process servers to scale out deployment** (Weitere Prozessserver zum horizontalen Hochskalieren der Bereitstellung hinzufügen) aus.
4. Beenden Sie den Assistenten genauso wie beim [Einrichten des Verwaltungsservers](site-recovery-vmware-to-azure-classic.md#step-5-install-the-management-server). Geben Sie auf der Seite **Konfigurationsserverdetails** die IP-Adresse des Masterzielservers sowie eine Passphrase für den Zugriff auf den virtuellen Computer an.

### <a name="set-up-a-linux-vm-as-the-master-target-server"></a>Einrichten eines virtuellen Linux-Computers als Masterzielserver
Zum Einrichten des Verwaltungsservers, auf dem der Masterzielserver als virtueller Linux-Computer ausgeführt wird, installieren Sie das Minimalbetriebssystem CentOS 6.6. Anschließend rufen Sie die SCSI-IDs für jede SCSI-Festplatte ab, installieren einige zusätzliche Pakete und nehmen einige benutzerdefinierte Änderungen vor.

#### <a name="install-centos-66"></a>Installieren von CentOS 6.6

1. Installieren Sie das Minimalbetriebssystem CentOS 6.6 auf der Verwaltungsserver-VM. Behalten Sie das ISO-Image im DVD-Laufwerk, und starten Sie das System. Überspringen Sie den Medientest. Wählen Sie als Sprache **US-Englisch** aus, wählen Sie **Basic Storage Devices** (Grundlegende Speichergeräte) aus, vergewissern Sie sich, dass sich auf der Festplatte keine wichtigen Daten befinden, und klicken Sie auf **Yes** (Ja), sodass alle Daten gelöscht werden. Geben Sie den Hostnamen des Verwaltungsservers ein, und wählen Sie die Netzwerkkarte des Servers aus.  Wählen Sie im Dialogfeld **Editing System** (Bearbeitungssystem) die Option **Connect automatically** (Verbindung automatisch herstellen) aus, und fügen Sie dann eine statische IP-Adresse, ein Netzwerk und DNS-Einstellungen hinzu. Legen Sie eine Zeitzone fest. Geben Sie das Stammkennwort für den Zugriff auf den Verwaltungsserver ein.
2. Wenn Sie nach dem gewünschten Installationstyp gefragt werden, wählen Sie **Create Custom Layout** (Benutzerdefiniertes Layout erstellen) als Partition aus. Klicken Sie auf **Weiter**. Wählen Sie **Kostenlos**, und klicken Sie dann auf **Erstellen**. Erstellen Sie die Partitionen **/**, **/var/crash** und **/home** mit **FS Type:** **ext4**. Erstellen Sie die Partition „swap“ als **FS Type: swap**.
3. Wenn bereits vorhandene Geräte gefunden werden, wird eine Warnung angezeigt. Klicken Sie auf **Format** , um das Laufwerk mit den Partitionseinstellungen zu formatieren. Klicken Sie auf **Write change to disk** , um die Partitionsänderungen zu übernehmen.
4. Wählen Sie **Install boot loader** > **Next** (Startladeprogramm installieren > Weiter) aus, um das Startladeprogramm auf der Stammpartition zu installieren.
5. Klicken Sie nach Abschluss der Installation auf **Neu starten**.

#### <a name="retrieve-the-scsi-ids"></a>Abrufen der SCSI-IDs

1. Rufen Sie nach der Installation die SCSI-IDs für alle SCSI-Festplatten im virtuellen Computer ab. Fahren Sie dazu den virtuellen Verwaltungsservercomputer herunter. Klicken Sie in VMware in den Eigenschaften des virtuellen Computers mit der rechten Maustaste auf den VM-Eintrag **Einstellungen bearbeiten** > **Optionen**.
2. Wählen Sie **Advanced** > **General item** (Erweitert > Allgemeine Elemente) aus, und klicken Sie auf **Configuration Parameters** (Konfigurationsparameter). Diese Option ist nicht verfügbar, wenn der virtuelle Computer ausgeführt wird. Der Computer muss heruntergefahren werden, um sie zu aktivieren.
3. Führen Sie einen der folgenden Schritte aus:
 * Wenn die Zeile **disk.EnableUUID** vorhanden ist, stellen Sie sicher, dass der Wert auf **True** festgelegt ist (Groß-/Kleinschreibung wird beachtet). Wenn dies der Fall ist, können Sie den Vorgang abbrechen und den SCSI-Befehl in einem Gastbetriebssystem testen, nachdem es hochgefahren wurde.
 * Wenn die Zeile **disk.EnableUUID** nicht vorhanden ist, klicken Sie auf **Add Row** (Zeile hinzufügen), und fügen Sie sie dann mit dem Wert **True** hinzu. Verwenden Sie keine doppelten Anführungszeichen.

#### <a name="install-additional-packages"></a>Installieren von zusätzlichen Paketen
Sie müssen einige zusätzliche Pakete herunterladen und installieren.

1. Stellen Sie sicher, dass der Masterzielserver mit dem Internet verbunden ist.
2. Führen Sie den folgenden Befehl aus, um 15 Pakete aus dem CentOS-Repository herunterzuladen und zu installieren: `# yum install –y xfsprogs perl lsscsi rsync wget kexec-tools`.
3. Wenn Linux auf den zu schützenden Quellcomputern mit dem Dateisystem Reiser oder XFS für das Stamm- oder Startgerät ausgeführt wird, laden Sie diese zusätzlichen Pakete herunter und installieren sie wie folgt:

   * \# cd /usr/local
   * \# wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm)
   * \# wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm)
   * \# rpm –ivh kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm
   * \# wget [http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm](http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm)
   * \# rpm –ivh xfsprogs-3.1.1-16.el6.x86_64.rpm
   * \# yum install device-mapper-multipath (erforderlich für das Aktivieren von Multipfadpaketen auf dem Masterzielserver)

#### <a name="apply-custom-changes"></a>Anwenden von benutzerdefinierten Änderungen
Nachdem Sie die nach der Installation nötigen Schritte ausgeführt und die Pakete installiert haben, führen Sie folgende benutzerdefinierte Änderungen aus:

1. Kopieren Sie die Binärdatei „RHEL 6-64 Unified Agent“ auf den virtuellen Computer. Führen Sie den folgenden Befehl aus, um das tar-Archiv der Binärdatei zu entpacken: `tar –zxvf <file name>`.
2. Führen Sie den folgenden Befehl aus, um Berechtigungen zu erteilen: `# chmod 755 ./ApplyCustomChanges.sh`.
3. Führen Sie das folgende Skript aus: `# ./ApplyCustomChanges.sh`. Führen Sie es nur einmal aus. Starten Sie den Server neu, nachdem das Skript erfolgreich ausgeführt wurde.

## <a name="run-the-failback"></a>Ausführen des Failbacks
### <a name="reprotect-the-azure-vms"></a>Erneutes Schützen der virtuellen Azure-Computer
1. Klicken Sie im **Tresor** in den **replizierten Elementen** mit der rechten Maustaste auf den virtuellen Computer, für den ein Failover ausgeführt wurde, und wählen Sie dann **Erneut schützen** aus.
2. Auf dem Blatt können Sie sehen, dass die Schutzrichtung **Azure auf lokal** bereits ausgewählt ist.
3. Wählen Sie bei **Masterzielserver** und **Prozessserver** den lokalen Masterzielserver und den virtuellen Azure-Prozessservercomputer aus.
4. Wählen Sie den Datenspeicher aus, in dem die Datenträger lokal wiederhergestellt werden sollen. Verwenden Sie diese Option, wenn der lokale virtuelle Computer gelöscht wurde und neue Datenträger erstellt werden müssen. Ignorieren Sie die Option, wenn die Datenträger bereits vorhanden sind. Einen Wert müssen Sie dennoch angeben.
5. Verwenden Sie ein Aufbewahrungslaufwerk, um die Zeitpunkte festzuhalten, wenn der virtuelle Computer zurück auf den lokalen Standort repliziert wird. Einige der Kriterien eines Aufbewahrungslaufwerks sind hier aufgeführt. Ohne diese wird das Laufwerk nicht für den Masterzielserver aufgelistet.

  * Das Volume darf nicht für andere Zwecke (Ziel für die Replikation usw.) verwendet werden.
  * Das Volume darf sich nicht im Sperrmodus befinden.
  * Bei dem Volume darf es sich nicht um ein Cachevolume handeln. (Die Masterzielserver-Installation darf auf diesem Volume nicht vorhanden sein. Das Volume für die benutzerdefinierte Prozessserver- und Masterzielserver-Installation kann für das Aufbewahrungsvolume nicht ausgewählt werden. Das hier installierte Prozessserver- und Masterzielserver-Volume ist ein Cachevolume des Masterzielservers.)
  * Der Dateisystemtyp für das Volume darf nicht FAT oder FAT32 lauten.
  * Die Volumekapazität muss größer als 0 sein.
  * Das Standardaufbewahrungsvolume für Windows ist das R-Volume.
  * Das Standardaufbewahrungsvolume für Linux ist „/mnt/retention“.

6. Die Failbackrichtlinie wird automatisch ausgewählt.
7. Nachdem Sie auf **OK** geklickt haben, um den erneuten Schutz zu starten, beginnt ein Auftrag mit der Replikation des virtuellen Computers von Azure am lokalen Standort. Sie können den Fortschritt auf der Registerkarte **Aufträge** nachverfolgen.

Wenn die Wiederherstellung an einem anderen Speicherort erfolgen soll, wählen Sie das Aufbewahrungslaufwerk und den Datenspeicher aus, die für den Masterzielserver konfiguriert wurden. Beim Ausführen des Failbacks zum lokalen Standort verwenden die virtuellen VMware-Computer im Failback-Schutzplan den gleichen Datenspeicher wie der Masterzielserver. Wenn Sie den virtuellen Azure-Replikatcomputer auf dem gleichen lokalen virtuellen Computer wiederherstellen möchten, muss sich der lokale virtuelle Computer bereits im selben Datenspeicher befinden wie der Masterzielserver. Wenn lokal kein virtueller Computer vorhanden ist, wird er beim Ausführen des erneuten Schutzes erstellt.

![Auswählen von „Azure auf lokale Instanz“ im Dropdownmenü](./media/site-recovery-failback-azure-to-vmware-new/reprotectinputs.png)

Sie können den erneuten Schutz auch auf einer Wiederherstellungsplanebene ausführen. Wenn Sie über eine Replikationsgruppe verfügen, können Sie diese nur mithilfe eines Wiederherstellungsplans erneut schützen. Verwenden Sie beim erneuten Schützen über einen Wiederherstellungsplan für jeden geschützten Computer die obigen Werte.

> [!NOTE]
> Replikationsgruppen müssen mit demselben Masterzielserver erneut geschützt werden. Sie können nicht gleichzeitig erneut geschützt werden, wenn verschiedene Masterzielserver dafür verwendet werden.

### <a name="run-a-failover-to-the-on-premises-site"></a>Ausführen eines Failovers zum lokalen Standort
Nach dem erneuten Schützen des virtuellen Computers können Sie ein Failover von Azure auf den lokalen Standort initiieren.

1. Klicken Sie auf der Seite mit den replizierten Elementen mit der rechten Maustaste auf den virtuellen Computer, und wählen Sie dann **Ungeplantes Failover** aus.
2. Überprüfen Sie unter **Failover bestätigen** die Failoverrichtung (von Azure), und wählen Sie dann den Wiederherstellungspunkt aus, der für das Failover verwendet werden soll (den neuesten oder den neuesten anwendungskonsistenten Wiederherstellungspunkt). Ein anwendungskonsistenter Punkt liegt vor dem aktuellsten Zeitpunkt und verursacht einigen Datenverlust.
3. Während des Failovers fährt Site Recovery die virtuellen Azure-Computer herunter. Nachdem Sie sich vergewissert haben, dass das Failback wie erwartet abgeschlossen wurde, können Sie überprüfen, ob die virtuellen Azure-Computer wie erwartet heruntergefahren wurden.

### <a name="reprotect-the-on-premises-site"></a>Erneutes Schützen des lokalen Standorts
Nachdem das Failback abgeschlossen wurde, führen Sie einen Commit für den virtuellen Computer aus, um sicherzustellen, dass die in Azure wiederhergestellten virtuellen Computer gelöscht wurden. Klicken Sie dazu mit der rechten Maustaste auf das geschützte Element, und klicken Sie dann auf **Commit**. Dadurch wird ein Auftrag wird ausgelöst, der die zuvor in Azure wiederhergestellten virtuellen Computer entfernt.

Nach dem Commit sollten sich Ihre Daten wieder am lokalen Standort befinden, sind jedoch nicht geschützt. Führen Sie daher folgende Schritte aus, um die Replikation zu Azure erneut zu starten:

1. Wählen Sie im **Tresor** unter **Einstellung** > **Replizierte Elemente** die virtuellen Computer aus, für die das Failback durchgeführt wurde, und klicken Sie dann auf **Erneut schützen**.
2. Geben Sie den Wert des Prozessservers an, der zum Zurücksenden von Daten an Azure verwendet werden muss.
3. Klicken Sie auf **OK**.

Nachdem das erneute Schützen abgeschlossen ist, wird der virtuelle Computer zurück nach Azure repliziert, und Sie können ein Failover ausführen.

### <a name="resolve-common-failback-issues"></a>Beheben häufiger Probleme beim Failback
* Wenn Sie die vCenter-Ermittlung schreibgeschützter Benutzer ausführen und virtuelle Computer schützen, funktioniert das Failover. Während des erneuten Schützens schlägt dies fehl, da die Datenspeicher nicht ermittelt werden können. Das Symptom dafür ist, dass Sie die Datenspeicher beim während des erneuten Schützens nicht sehen. Zum Beheben dieses Problems können Sie die vCenter-Anmeldeinformationen mit einem entsprechenden Konto aktualisieren, das über die erforderlichen Berechtigungen verfügt, und den Auftrag wiederholen. Weitere Informationen finden Sie unter [Replizieren von virtuellen VMware-Computern und physischen Servern in Azure mithilfe von Azure Site Recovery](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access).
* Wenn Sie ein Failback für einen virtuellen Linux-Computer durchführen und ihn lokal ausführen, sehen Sie, dass das Netzwerk-Manager-Paket auf dem Computer deinstalliert wurde. Der Grund für die Deinstallation ist, dass beim Wiederherstellen des virtuellen Computers in Azure das Netzwerk-Manager-Paket entfernt wird.
* Wenn ein virtueller Computer mit einer statischen IP-Adresse konfiguriert ist und ein Failover zu Azure ausgeführt wird, wird die IP-Adresse über DHCP abgerufen. Nach dem Failover zurück zum lokalen Standort verwendet der virtuelle Computer weiterhin DHCP zum Abrufen der IP-Adresse. Melden Sie sich ggf. manuell am Computer an, und setzen Sie die IP-Adresse auf die statische Adresse zurück.
* Wenn Sie die kostenlose Edition ESXi 5.5 oder die kostenlose Edition vSphere 6 Hypervisor verwenden, wäre das Failover erfolgreich, das Failback jedoch nicht. Sie müssen auf die Evaluierungslizenz eines der Programme aktualisieren, um das Failback zu aktivieren.
* Wenn der Konfigurationsserver auf dem Prozessserver nicht erreichbar ist, können Sie Konnektivität mit dem Konfigurationsserver mit dem Telnet-Befehl an den Konfigurationsservercomputer an Port 443 überprüfen. Sie können auch versuchen, den Konfigurationsserver auf dem Prozessservercomputer zu pingen. Ein Prozessserver sollte auch einen Takt aufweisen, wenn er mit dem Konfigurationsserver verbunden ist.
* Wenn Sie versuchen, ein Failback auf einen alternativen vCenter-Computer durchzuführen, stellen Sie sicher, dass sowohl Ihr neuer vCenter-Computer als auch der Masterzielserver erkannt werden. Als typisches Symptom sind die Datenspeicher im Dialogfeld **Erneut schützen** nicht zugänglich oder sichtbar.
* Für einen als physischen lokalen Computer geschützten WS 2008 R2 SP1-Computer kann kein Failback von Azure auf lokale Standorte ausgeführt werden.

## <a name="fail-back-with-expressroute"></a>Ausführen eines Failbacks mit ExpressRoute
Ein Failback kann über eine VPN- oder eine ExpressRoute-Verbindung ausgeführt werden. Wenn Sie eine ExpressRoute-Verbindung verwenden möchten, beachten Sie Folgendes:

* Die ExpressRoute-Verbindung muss in dem virtuellen Azure-Netzwerk eingerichtet werden, zu dem das Failover der Quellcomputer durchgeführt wird und in dem sich die virtuellen Azure-Computer nach dem Failover befinden.
* Die Daten werden zu einem Azure-Speicherkonto auf einem öffentlichen Endpunkt repliziert. Zu Verwendung einer ExpressRoute-Verbindung richten Sie in ExpressRoute ein öffentliches Peering mit dem Zieldatencenter für die Site Recovery-Replikation ein.

