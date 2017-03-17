---
title: "Installieren des Linux-Masterzielservers für ein Failover von Azure auf lokale Instanzen | Microsoft-Dokumentation"
description: "Bevor Sie die Schritte zum erneuten Schützen einer Linux-VM ausführen können, benötigen Sie einen Linux-Masterzielserver. Hier wird beschrieben, wie Sie die Installation durchführen."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 02/13/2017
ms.author: ruturajd
translationtype: Human Translation
ms.sourcegitcommit: c4fb25880584add0832464d9049dc6c78059c48b
ms.openlocfilehash: 8ac18526e6781f189444233a191aa1d6c5b863ff
ms.lasthandoff: 02/22/2017


---
# <a name="how-to-install-linux-master-target-server"></a>Installieren des Linux-Masterzielservers
Nach dem Failover Ihrer virtuellen Computer können Sie ein Failback für diese zurück auf die lokalen Instanzen durchführen. Für ein Failback müssen Sie den virtuellen Computer zuerst in den geschützten Zustand versetzen, indem Sie ihn von Azure bis zur lokalen Instanz erneut schützen. Dazu benötigen Sie einen Masterzielserver, um den Datenverkehr lokal zu empfangen. Falls es sich bei Ihrem geschützten virtuellen Computer um einen virtuellen Windows-Computer handelt, benötigen Sie ein Windows-Masterziel. Für einen virtuellen Linux-Computer benötigen Sie ein Linux-Masterziel zum erneuten Schützen. Lesen Sie die Schritte unten, um ein Linux-Masterziel zu erstellen und zu installieren.

## <a name="overview"></a>Übersicht
Dieser Artikel bietet Informationen und Anweisungen zum Installieren eines Linux-Masterziels.

Kommentare oder Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)veröffentlichen.

## <a name="pre-requisites"></a>Voraussetzungen

* Um den richtigen Host für die Bereitstellung des Masterziels auszuwählen, müssen Sie ermitteln, ob das Failback auf eine vorhandene lokale VM oder eine neue VM ausgeführt wird (da die lokale VM gelöscht wurde). 
    * Bei einem bestehenden virtuellen Computer benötigt der Masterzielserver Zugriff auf die Datenspeicher des virtuellen Computers.
    * Wenn der virtuelle Computer nicht lokal vorhanden ist, wird der virtuelle Failbackcomputer auf demselben Host wie das Masterziel erstellt. Sie können einen beliebigen ESXi-Host zum Installieren des Masterziels verwenden.
* Das Masterziel sollte sich in einem Netzwerk befinden, das mit dem Prozessserver und dem Konfigurationsserver kommunizieren kann.
* Die Versionsnummer des Masterziels sollte kleiner oder gleich der Versionsnummer des Prozessservers und Konfigurationsservers sein. (Beispiel: Wenn der Konfigurationsserver die Version 9.4 hat, können für das Masterziel die Versionen 9.4 und 9.3 verwendet werden, aber nicht 9.5.)
* Das Masterziel kann nur eine VMware-VM sein, keine physische VM.


## <a name="steps-to-deploy-the-master-target-server"></a>Schritte zum Bereitstellen des Masterzielservers

### <a name="install-centos-66-minimal"></a>Installieren von CentOS 6.6 Minimal

Führen Sie die unten angegebenen Schritte aus, um das Betriebssystem CentOS 6.6 – 64 Bit zu installieren.

1. Wählen Sie in den folgenden Links die nächstgelegene Spiegelung aus, um eine ISO-Datei mit CentOS 6.6 und mindestens 64 Bit herunterzuladen.

    <http://archive.kernel.org/centos-vault/6.6/isos/x86_64/CentOS-6.6-x86_64-minimal.iso>

    <http://mirror.symnds.com/distributions/CentOS-vault/6.6/isos/x86_64/CentOS-6.6-x86_64-minimal.iso>

    <http://bay.uchicago.edu/centos-vault/6.6/isos/x86_64/CentOS-6.6-x86_64-minimal.iso>

    <http://mirror.nsc.liu.se/centos-store/6.6/isos/x86_64/CentOS-6.6-x86_64-minimal.iso>

    Legen Sie den Datenträger mit der ISO-Datei mit CentOS 6.6 Minimal 64 Bit in das DVD-Laufwerk ein, und starten Sie das System.

    ![](./media/site-recovery-how-to-install-linux-master-target/media/image1.png)

2. Wählen Sie **Skip** (Überspringen), um den Prozess zum Testen der Medien zu ignorieren.

    ![](./media/site-recovery-how-to-install-linux-master-target/media/image2.png)

3. Der Begrüßungsbildschirm der Installation wird angezeigt. Klicken Sie auf die Schaltfläche **Next** (Weiter).

    ![](./media/site-recovery-how-to-install-linux-master-target/media/image3.png)

4. Wählen Sie **English** (Englisch) als Ihre bevorzugte Sprache, und klicken Sie auf **Next** (Weiter), um den Vorgang fortzusetzen.

    ![](./media/site-recovery-how-to-install-linux-master-target/media/image4.png)

5. Wählen Sie **US English** (Englisch (USA)) als Tastaturlayout aus. Klicken Sie auf **Next** (Weiter), um die Installation fortzusetzen.

    ![](./media/site-recovery-how-to-install-linux-master-target/media/image5.png)

6. Wählen Sie den Gerätetyp für die Installation aus. Wählen Sie **Basic storage Devices** (Grundlegende Speichergeräte). Klicken Sie auf **Next** (Weiter), um die Installation fortzusetzen.

    ![](./media/site-recovery-how-to-install-linux-master-target/media/image6.png)

7. Eine Warnmeldung mit dem Hinweis, dass die auf der Festplatte vorhandenen Daten gelöscht werden, wird angezeigt. Stellen Sie sicher, dass die Festplatte keine wichtigen Daten enthält, und klicken Sie auf **Yes, discard any data** (Ja, alle Daten verwerfen).

    ![](./media/site-recovery-how-to-install-linux-master-target/media/image7.png)

8. Geben Sie den Hostnamen für Ihren Server im Textfeld **Hostname** ein. Klicken Sie auf **Configure Network** (Netzwerk konfigurieren), und wählen Sie im Fenster **Network Connection** (Netzwerkverbindung) Ihre Netzwerkschnittstelle aus. Klicken Sie auf die Schaltfläche **Edit** (Bearbeiten), um die IPV4-Einstellungen zu konfigurieren.

    ![](./media/site-recovery-how-to-install-linux-master-target/media/image8.png)

9. Das folgende Fenster **Editing System eth0** (Bearbeitungssystem eth0) wird angezeigt. Aktivieren Sie das Kontrollkästchen **Connect automatically** (Automatisch verbinden). Wählen Sie auf der Registerkarte „IPv4 Settings“ (IPv4-Einstellungen) die Methode **Manual** (Manuell), und klicken Sie dann auf die Schaltfläche **Add** (Hinzufügen). Geben Sie die Details zur statischen IP-Adresse, zum Gateway und zum DNS-Server an. Klicken Sie auf **Apply** (Übernehmen), um die Details zu speichern.

    ![](./media/site-recovery-how-to-install-linux-master-target/media/image9.png)

10. Wählen Sie im Kombinationsfeld Ihre Zeitzone aus, und klicken Sie auf **Next** (Weiter), um fortzufahren.

    ![](./media/site-recovery-how-to-install-linux-master-target/media/image10.png)

11. Geben Sie das **Root password** (Stammkennwort) ein, und bestätigen Sie das Kennwort. Klicken Sie anschließend auf **Next** (Weiter).

    ![](./media/site-recovery-how-to-install-linux-master-target/media/image11.png)

12. Wählen Sie als Partitionsmodus die Option **Create Custom Layout** (Benutzerdefiniertes Layout erstellen) aus, und klicken Sie auf **Next** (Weiter), um fortzufahren.

    ![](./media/site-recovery-how-to-install-linux-master-target/media/image12.png)

13. Wählen Sie die Partition **Free** (Frei) aus, und klicken Sie auf **Create** (Erstellen), um die Partitionen **/**, **/var/crash** und **/home** mit **ext4** als Dateisystemtyp zu erstellen. Erstellen Sie die Partition **Swap** mit **swap** als Dateisystemtyp. Verwenden Sie zum Zuordnen der Partitionsgröße die Formel für die Größenzuordnung, die in der Tabelle unten angegeben ist.

    > [!NOTE]
    > Bei dem System mit dem Linux-basierten Masterziel (Master Target, MT) darf für den Stamm- oder Aufbewahrungspeicherplatz nicht der LVM verwendet werden. Das Linux-MT ist standardmäßig so konfiguriert, dass LVM-Partitionen/Datenträger nicht erkannt werden.

    ![](./media/site-recovery-how-to-install-linux-master-target/media/image13.png)

14. Klicken Sie nach dem Erstellen der Partition auf **Next** (Weiter), um fortzufahren.

    ![](./media/site-recovery-how-to-install-linux-master-target/media/image14.png)

15. Falls bereits vorhandene Geräte gefunden werden, wird eine Warnmeldung zur Formatierung angezeigt. Klicken Sie auf **Format** (Formatieren), um für die Festplatte die Formatierung mit der aktuellen Partitionstabelle durchzuführen.

    ![](./media/site-recovery-how-to-install-linux-master-target/media/image15.png)

16. Klicken Sie auf **Write changes to disk** (Änderungen auf Datenträger schreiben), um die Partitionsänderungen auf den Datenträger anzuwenden.

    ![](./media/site-recovery-how-to-install-linux-master-target/media/image16.png)

17. Aktivieren Sie die Option **Install boot loader** (Startladeprogramm installieren), und klicken Sie auf **Next** (Weiter), um das Startladeprogramm auf der Stammpartition zu installieren.

    ![](./media/site-recovery-how-to-install-linux-master-target/media/image17.png)


18. Der Installationsprozess wird gestartet. Sie können den Status der Installation verfolgen.

    ![](./media/site-recovery-how-to-install-linux-master-target/media/image18.png)

19. Nachdem der Installationsvorgang erfolgreich abgeschlossen wurde, wird der folgende Bildschirm angezeigt. Klicken Sie auf **Reboot** (Neu starten).

    ![](./media/site-recovery-how-to-install-linux-master-target/media/image19.png)


### <a name="post-installation-steps"></a>Schritte nach der Installation
Als Nächstes bereiten wir den Computer auf die Konfiguration als Masterzielserver vor.

Aktivieren Sie den Parameter „disk.EnableUUID = TRUE“, um SCSI-IDs für die einzelnen SCSI-Festplatten eines virtuellen Linux-Computers zu erhalten.

Gehen Sie hierzu wie folgt vor:

a. Fahren Sie den virtuellen Computer herunter.

b. Klicken Sie im linken Bereich mit der rechten Maustaste auf den VM-Eintrag, und wählen Sie **Edit Settings**.

c. Klicken Sie auf die Registerkarte **Options** .

d. Klicken Sie links auf **Advanced&gt;General** und anschließend rechts auf **Configuration Parameters**.

![](./media/site-recovery-how-to-install-linux-master-target/media/image20.png)

Wenn der Computer ausgeführt wird, ist die Option für die Konfigurationsparameter deaktiviert. Fahren Sie den Computer herunter, um die Registerkarte zu aktivieren.

e. Prüfen Sie, ob bereits eine Zeile mit **disk.EnableUUID** vorhanden ist.

f. Falls der Wert vorhanden und auf „False“ festgelegt ist, überschreiben Sie den Wert mit „True“. Die Groß- und Kleinschreibung spielt dabei keine Rolle.

g. Falls der Wert vorhanden und auf „True“ festgelegt ist, klicken Sie auf „Cancel“ (Abbrechen).
    
h. Ist der Wert nicht vorhanden, klicken Sie auf **Add Row** (Zeile hinzufügen).

i. Fügen Sie „disk.EnableUUID“ in der Spalte „Name“ hinzu, und setzen Sie den Wert auf „TRUE“.

   * Falls die Zeile vorhanden und der Wert auf „False“ festgelegt ist, überschreiben Sie den Wert mit „True“. Die Groß- und Kleinschreibung spielt dabei keine Rolle.

   * Falls die Zeile vorhanden und der Wert auf „True“ festgelegt ist, klicken Sie auf „Cancel“, und testen Sie nach dem Start des Gastbetriebssystems den SCSI-Befehl.

f. Ist die Zeile nicht vorhanden, klicken Sie auf **Add Row**.

  Fügen Sie in der Spalte „Name“ die Zeichenfolge „disk.EnableUUID“ ein.

  Legen Sie den Wert auf „TRUE“ fest.



![](./media/site-recovery-how-to-install-linux-master-target/media/image21.png)

#### <a name="download-and-install-the-additional-packages"></a>Herunterladen und Installieren zusätzlicher Pakete

> [!NOTE]
> Stellen Sie sicher, dass das System über eine Internetverbindung verfügt, bevor Sie zusätzliche Pakete herunterladen und installieren. Anderenfalls müssen Sie das RPM-Paket manuell suchen und installieren.

```
yum install -y xfsprogs perl lsscsi rsync wget kexec-tools
```

Der obige Befehl lädt die folgenden 15 Pakete aus dem CentOS 6.6-Repository herunter und installiert sie: Wenn Sie über keinen Internetzugriff verfügen, müssen Sie die folgenden RPM herunterladen:


bc-1.06.95-1.el6.x86\_64.rpm

busybox-1.15.1-20.el6.x86\_64.rpm

elfutils-libs-0.158-3.2.el6.x86\_64.rpm

kexec-tools-2.0.0-280.el6.x86\_64.rpm

lsscsi-0.23-2.el6.x86\_64.rpm

lzo-2.03-3.1.el6\_5.1.x86\_64.rpm

perl-5.10.1-136.el6\_6.1.x86\_64.rpm

perl-Module-Pluggable-3.90-136.el6\_6.1.x86\_64.rpm

perl-Pod-Escapes-1.04-136.el6\_6.1.x86\_64.rpm

perl-Pod-Simple-3.13-136.el6\_6.1.x86\_64.rpm

perl-libs-5.10.1-136.el6\_6.1.x86\_64.rpm

perl-version-0.77-136.el6\_6.1.x86\_64.rpm

rsync-3.0.6-12.el6.x86\_64.rpm

snappy-1.1.0-1.el6.x86\_64.rpm

wget-1.12-5.el6\_6.1.x86\_64.rpm


#### <a name="install-additional-packages-for-specific-operating-systems"></a>Installieren zusätzlicher Pakete für bestimmte Betriebssysteme

> [!NOTE]
> Wenn für Computer mit Quellschutz für das Stamm- oder Startgerät das Reiser- oder das XFS-Dateisystem verwendet wird, müssen vor dem Schutzvorgang die folgenden zusätzlichen Pakete heruntergeladen und auf dem Linux-Masterziel installiert werden.
 

***ReiserFS (bei Verwendung in Suse11SP3. ReiserFS ist in Suse11SP3 allerdings nicht das Standarddateisystem.)***

```
cd /usr/local

wget
<http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm>

wget
<http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm>

rpm -ivh kmod-reiserfs-0.0-1.el6.elrepo.x86\_64.rpm
reiserfs-utils-3.6.21-1.el6.elrepo.x86\_64.rpm
```

***XFS (RHEL, CentOS 7 oder höher)***

```
cd /usr/local

wget
<http://archive.kernel.org/centos-vault/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_64.rpm>

rpm -ivh xfsprogs-3.1.1-16.el6.x86\_64.rpm

yum install device-mapper-multipath 
```
Ist erforderlich, um die Verwendung von Multipfadpaketen auf dem Masterzielserver zu ermöglichen.

### <a name="get-the-installer-for-setup"></a>Herunterladen des Installationsprogramms zur Einrichtung

Wenn Sie auf Ihrem Masterzielserver über Internetzugriff verfügen, können Sie das Installationsprogramm anhand der unten aufgeführten Schritte herunterladen. Anderenfalls kopieren Sie das Installationsprogramm vom Prozessserver und installieren es.

#### <a name="download-the-mt-installation-packages"></a>Herunterladen der Pakete für die Installation des Masterziels

Laden Sie die aktuellen Daten für die Installation des Linux-Masterziels hier herunter: [https://aka.ms/latestlinuxmobsvc](https://aka.ms/latestlinuxmobsvc).

Geben Sie Folgendes ein, um den Download über Linux durchzuführen: 

```
wget https://aka.ms/latestlinuxmobsvc -O latestlinuxmobsvc.tar.gz
```

Stellen Sie sicher, dass Sie das Installationsprogramm herunterladen und nur im Basisverzeichnis entzippen. Wenn Sie es in „/usr/Local“ entzippen, schlägt die Installation fehl.


#### <a name="access-the-installer-from-the-process-server"></a>Zugriff auf das Installationsprogramm über den Prozessserver

Navigieren Sie auf dem Prozessserver zum Verzeichnis „C:\Program Files (x86)\Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository“.

Kopieren Sie die erforderliche Installationsdatei vom Prozessserver, und speichern Sie sie unter „latestlinuxmobsvc.tar.gz“ in Ihrem Basisverzeichnis.


### <a name="apply-custom-configuration-changes"></a>Anwenden benutzerdefinierter Konfigurationsänderungen

Gehen Sie zum Anwenden benutzerdefinierter Konfigurationsänderungen wie folgt vor:


1. Führen Sie für die Binärdatei den folgenden Untar-Befehl aus:
    ```
    tar -zxvf latestlinuxmobsvc.tar.gz
    ```

    ![](./media/site-recovery-how-to-install-linux-master-target/image16.png)
    
2. Führen Sie den folgenden Befehl aus, um die erforderliche Berechtigung zu erteilen:
    ```
    chmod 755 ./ApplyCustomChanges.sh
    ```

3. Führen Sie den folgenden Befehl aus, um das Skript auszuführen:
    ```
    ./ApplyCustomChanges.sh
    ```
> [!NOTE]
> Führen Sie das Skript nur einmal auf dem Server aus. Fahren Sie den Server herunter. Starten Sie den Server nach dem Hinzufügen eines Datenträgers wie in den nächsten Schritten angegeben neu.

### <a name="add-retention-disk-to-linux-mt-vm"></a>Hinzufügen des Aufbewahrungsdatenträgers zum virtuellen Linux-Masterzielcomputer 

Führen Sie die unten angegebenen Schritte aus, um einen Aufbewahrungsdatenträger zu erstellen.

1. Fügen Sie einen neuen **1-TB**-Datenträger zum virtuellen Linux-Masterzielcomputer hinzu, und **starten** Sie den Computer.

2. Rufen Sie den Befehl **multipath -ll** auf, um die Multipfad-ID des Aufbewahrungsdatenträgers zu ermitteln.

    ```
    multipath -ll
    ```

    ![](./media/site-recovery-how-to-install-linux-master-target/media/image22.png)

3. Formatieren Sie das Laufwerk, und erstellen Sie ein Dateisystem auf dem neuen Laufwerk. 
    
    ```
    mkfs.ext4 /dev/mapper/<Retention disk's multipath id>
    ```
    ![](./media/site-recovery-how-to-install-linux-master-target/media/image23.png)

4. Stellen Sie nach dem Erstellen des Dateisystems den Aufbewahrungsdatenträger bereit.
    ```
    mkdir /mnt/retention
    mount /dev/mapper/<Retention disk's multipath id> /mnt/retention
    ```

    ![](./media/site-recovery-how-to-install-linux-master-target/media/image24.png)

5. Erstellen Sie schließlich den fstab-Eintrag, um bei jedem Neustart einen Aufbewahrungsdatenträger bereitzustellen.
    ```
    vi /etc/fstab 
    ```
    Drücken Sie die EINFG-TASTE, um mit der Bearbeitung der Datei zu beginnen. Erstellen Sie eine neue Zeile, und fügen Sie den folgenden Text ein. Bearbeiten Sie die Multipfad-ID des Datenträgers auf Basis der hervorgehobenen Multipfad-ID aus dem vorherigen Befehl.

    **/dev/mapper/<Retention disks multipath id> /mnt/retention ext4 rw 0 0**

    Drücken Sie die ESC-TASTE, und geben Sie „:wq“ (Schreiben und Beenden) ein, um das Editor-Fenster zu schließen.

### <a name="install-master-target"></a>Installieren des Masterziels

> [!NOTE]
> Die Versionsnummer des Masterzielservers sollte kleiner oder gleich der Versionsnummer des Prozessservers und Konfigurationsservers sein. Wenn die Versionsnummer des Masterziels höher ist, ist der Vorgang zum erneuten Schützen erfolgreich, aber die Replikation schlägt fehl.
 

> [!NOTE]
> Vergewissern Sie sich vor dem Installieren des Masterzielservers, dass die Datei „/etc/hosts“ auf der VM Einträge enthält, mit denen der Name des lokalen Hosts den IP-Adressen der einzelnen Netzwerkkarten zugeordnet wird.
 
1. Kopieren Sie die Passphrase aus **C:\ProgramData\Microsoft Azure Site Recovery\private\connection.passphrase** auf dem Konfigurationsserver, und speichern Sie sie dann in „passphrase.txt“ im selben lokalen Verzeichnis, indem Sie folgenden Befehl ausführen.

    ```
    echo <passphrase> >passphrase.txt
    ```
    Beispiel: echo itUx70I47uxDuUVY >passphrase.txt

2. Notieren Sie die IP-Adresse des Konfigurationsservers. Wir benötigen sie im nächsten Schritt.

3. Führen Sie den folgenden Befehl aus, um den Masterzielserver zu installieren und den Server auf dem Konfigurationsserver zu registrieren.
    
    ```
    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i <Configuration Server IP Address> -p 443 -s y -c https -P passphrase.txt
    ```

    Beispiel: ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

    Warten Sie, bis die Ausführung des Skripts abgeschlossen ist. Wenn das Masterziel erfolgreich registriert wurde, ist es auf der Site Recovery-Infrastrukturseite im Portal aufgeführt.

#### <a name="installing-master-target-using-interactive-install"></a>Installieren des Masterziels mithilfe der interaktiven Installation

1. Führen Sie den folgenden Befehl aus, um das Masterziel zu installieren. Wählen Sie als Agent-Rolle die Option „Master Target“ (Masterziel).

    ```
    ./install
    ```

2. Wählen Sie den standardmäßigen Speicherort für die Installation, und drücken Sie die EINGABETASTE, um fortzufahren.
    
    ![](./media/site-recovery-how-to-install-linux-master-target/image17.png)
    

3. Wählen Sie die zu konfigurierenden globalen Einstellungen aus.

    ![](./media/site-recovery-how-to-install-linux-master-target/image18.png)
    
4. Geben Sie die IP-Adressen für den CS-Server an.

5. Legen Sie 443 als CS-Serverport fest.
    
    ![](./media/site-recovery-how-to-install-linux-master-target/image19.png)
    
6. Kopieren Sie die CS-Passphrase unter „C:\ProgramData\Microsoft Azure Site Recovery\private\connection.passphrase“ auf dem Konfigurationsserver, und fügen Sie sie ins Feld „Passphrase“ ein. Es ist auch nach dem Kopieren und Einfügen leer.

7. Wechseln Sie in das Menü [Quit] (Beenden).

8. Warten Sie, bis der Installations- und Registrierungsvorgang abgeschlossen ist.

### <a name="install-vmware-tools-on-the-master-target-server"></a>Installieren von VMware-Tools auf dem Masterzielserver

VMware-Tools müssen auf dem Masterziel installiert werden, damit die Datenspeicher ermittelt werden können. Wenn die Tools nicht installiert werden, werden die Datenspeicher nicht auf dem Bildschirm für das erneute Schützen angezeigt. Sie müssen nach der Installation von VMware-Tools einen Neustart durchführen.

## <a name="next-steps"></a>Nächste Schritte
Sobald die Installation und Registrierung des Masterziels abgeschlossen ist, erscheint das Masterziel im Abschnitt „Masterziel“ in der Site Recovery-Infrastruktur in der Übersicht der Konfigurationsserver.

Sie können jetzt mit [Reprotection](site-recovery-how-to-reprotect.md) (Erneuter Schutz) und Failback fortfahren.

## <a name="common-issues"></a>Häufige Probleme

* Stellen Sie sicher, dass Sie Storage vMotion nicht auf Verwaltungskomponenten wie dem Masterziel aktivieren. Wenn das Masterziel nach erfolgreichem erneutem Schutz verschoben wird, können die VMDK-Instanzen nicht getrennt werden, und das Failback schlägt fehl.
* Der Masterzielcomputer darf keine Momentaufnahmen auf dem virtuellen Computer aufweisen. Wenn Momentaufnahmen vorhanden sind, schlägt das Failback fehl.
* Aufgrund von benutzerdefinierten NIC-Konfigurationen bei einigen Kunden ist die Netzwerkschnittstelle während des Systemstarts deaktiviert, und der Masterziel-Agent kann nicht initialisiert werden. Stellen Sie sicher, dass die folgenden Eigenschaften richtig festgelegt sind.
    * Überprüfen Sie die beiden Eigenschaften in den Dateien der Ethernet-Karte „/etc/sysconfig/network-scripts/ifcfg-eth*“:
        * BOOTPROTO=dhcp 
        * ONBOOT=yes


