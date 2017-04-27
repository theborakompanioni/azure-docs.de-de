---
title: "Installieren eines Linux-Masterzielservers für ein Failover von Azure auf lokale Instanzen | Microsoft-Dokumentation"
description: "Bevor Sie die Schritte zum erneuten Schützen eines virtuellen Linux-Computers ausführen können, benötigen Sie einen Linux-Masterzielserver. Hier wird beschrieben, wie Sie die Installation durchführen."
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
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: c75a3a2477f113f17aab7a3e1969f15a4ec88a02
ms.lasthandoff: 03/31/2017


---
# <a name="how-to-install-a-linux-master-target-server"></a>Installieren eines Linux-Masterzielservers
Nach dem Failover Ihrer virtuellen Computer können Sie für die virtuellen Computer ein Failback zum lokalen Standort ausführen. Für ein Failback müssen Sie den virtuellen Computer von Azure zum lokalen Standort erneut schützen. Für diesen Prozess benötigen Sie einen lokalen Masterzielserver, der den Datenverkehr empfängt. Falls es sich bei Ihrem geschützten virtuellen Computer um einen virtuellen Windows-Computer handelt, benötigen Sie ein Windows-Masterziel. Bei einem virtuellen Linux-Computer benötigen Sie ein Linux-Masterziel. Lesen Sie die folgenden Schritte, um Informationen zum Erstellen und Installieren eines Linux-Masterziels zu erhalten.

## <a name="overview"></a>Übersicht
Dieser Artikel bietet Informationen und Anweisungen zum Installieren eines Linux-Masterziels.

Kommentare oder Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.

## <a name="prerequisites"></a>Voraussetzungen

* Zur korrekten Wahl des Hosts, auf dem Sie das Masterziel bereitstellen, ermitteln Sie, ob das Failback zu einem vorhandenen, lokalen virtuellen Computer oder zu einem neuen virtuellen Computer führt, weil der lokale virtuelle Computer gelöscht wurde.
    * Bei einem vorhandenen virtuellen Computer muss der Host des Masterziels über Zugriff auf die Datenspeicher des virtuellen Computers verfügen.
    * Falls kein lokaler virtueller Computer vorhanden ist, wird das Failback des virtuellen Computers auf demselben Host wie das Masterziel erstellt. Sie können einen beliebigen ESXi-Host zum Installieren des Masterziels verwenden.
* Das Masterziel sollte sich in einem Netzwerk befinden, das mit dem Prozessserver und dem Konfigurationsserver kommunizieren kann.
* Die Version des Masterziels darf nicht höher als die des Prozess- und des Konfigurationsservers sein. Beispiel: Bei Version 9.4 des Konfigurationsservers kann die Version des Masterziels 9.4 oder 9.3, aber nicht 9.5 sein.
* Das Masterziel kann nur ein virtueller VMware-Computer und kein physischer Server sein.
* Das Masterziel muss die unten stehende Größenrichtlinie erfüllen.
    * RAM: 6 GB oder mehr
    * Größe des Betriebssystemdatenträgers: 50 GB oder mehr (für die CentOS6.6-Installation)
    * Zusätzliche Datenträgergröße für Aufbewahrungslaufwerk: 1 TB
    * CPU-Kerne: mindestens 4 Kerne




## <a name="steps-to-deploy-the-master-target-server"></a>Schritte zum Bereitstellen des Masterzielservers

### <a name="install-centos-66-minimal"></a>Installieren von CentOS 6.6 Minimal

Verwenden Sie die folgenden Schritte, um das 64-Bit-Betriebssystem CentOS 6.6 zu installieren:

1. Wählen Sie in den folgenden Links die nächstgelegene Spiegelung aus, um eine ISO-Datei mit CentOS 6.6 und mindestens 64 Bit herunterzuladen.

    <http://archive.kernel.org/centos-vault/6.6/isos/x86_64/CentOS-6.6-x86_64-minimal.iso>

    <http://mirror.symnds.com/distributions/CentOS-vault/6.6/isos/x86_64/CentOS-6.6-x86_64-minimal.iso>

    <http://bay.uchicago.edu/centos-vault/6.6/isos/x86_64/CentOS-6.6-x86_64-minimal.iso>

    <http://mirror.nsc.liu.se/centos-store/6.6/isos/x86_64/CentOS-6.6-x86_64-minimal.iso>

    Legen Sie den Datenträger mit der ISO-Datei mit CentOS 6.6 Minimal 64 Bit in ein DVD-Laufwerk ein, und starten Sie das System.

    ![Willkommen bei CentoOS 6.6 (Dialogfeld)](./media/site-recovery-how-to-install-linux-master-target/media/image1.png)

2. Klicken Sie auf **Skip** (Überspringen), um den Prozess zum Testen der Medien zu ignorieren.

    ![Wählen Sie „Skip“ (Überspringen) aus, um den Prozess zum Testen der Medien zu ignorieren.](./media/site-recovery-how-to-install-linux-master-target/media/image2.png)

3. Klicken Sie auf dem Begrüßungsbildschirm der Installation auf die Schaltfläche **Next** (Weiter).

    ![Die Schaltfläche „Next“ (Weiter) auf dem Begrüßungsbildschirm der Installation](./media/site-recovery-how-to-install-linux-master-target/media/image3.png)

4. Wählen Sie **English** (Englisch) als Ihre bevorzugte Sprache, und klicken Sie auf **Next** (Weiter).

    ![Eine Sprache auswählen](./media/site-recovery-how-to-install-linux-master-target/media/image4.png)

5. Wählen Sie **US English** (Englisch, USA) als Tastaturlayout, und klicken Sie dann auf **Next** (Weiter).

    ![Das englische Tastaturlayout auswählen](./media/site-recovery-how-to-install-linux-master-target/media/image5.png)

6. Wählen Sie **Basic storage Devices** (Grundlegende Speichergeräte) aus, und klicken Sie dann auf **Next** (Weiter).

    ![Ein Speichergerät auswählen](./media/site-recovery-how-to-install-linux-master-target/media/image6.png)

7. Die angezeigte Warnmeldung gibt an, dass die vorhandenen Daten auf der Festplatte gelöscht werden. Stellen Sie sicher, dass die Festplatte keine wichtigen Daten enthält, und klicken Sie dann auf **Yes, discard any data** (Ja, alle Daten verwerfen).

    ![Warnung, dass Daten gelöscht werden, wenn Sie den Vorgang fortsetzen](./media/site-recovery-how-to-install-linux-master-target/media/image7.png)

8. Geben Sie den Hostnamen für den Server in das Feld **Hostname** ein, und klicken Sie dann auf **Configure Network** (Netzwerk konfigurieren). Wählen Sie im Dialogfeld **Network Connection** (Netzwerkverbindung) die Netzwerkschnittstelle aus, und klicken Sie dann auf die Schaltfläche **Edit** (Bearbeiten), um „IPV4Settings“ zu konfigurieren.

    ![Einen Hostnamen auswählen und IPV4 konfigurieren](./media/site-recovery-how-to-install-linux-master-target/media/image8.png)

9. Aktivieren Sie im Dialogfeld **Editing System eth0** (Bearbeitungssystem eth0) das Kontrollkästchen **Connect automatically** (Automatisch verbinden). Wählen Sie auf der Registerkarte **IPv4 Settings** (IPv4-Einstellungen) für **Method** (Methode) die Option **Manual** (Manuell), und klicken Sie dann auf die Schaltfläche **Add** (Hinzufügen). Geben Sie die Details zur **statischen IP-Adresse**, zur **Netzmaske**, zum **Gateway** und zum **DNS-Server** an. Klicken Sie auf **Apply** (Übernehmen), um die Details zu speichern.

    ![Einstellungen für die Netzwerkkonfiguration](./media/site-recovery-how-to-install-linux-master-target/media/image9.png)

10. Wählen Sie Ihre Zeitzone aus, und klicken Sie dann auf **Next** (Weiter).

    ![Eine Zeitzone auswählen](./media/site-recovery-how-to-install-linux-master-target/media/image10.png)

11. Geben Sie das **Root Password** (Stammkennwort) ein, bestätigen Sie das Kennwort, und klicken Sie anschließend auf **Next** (Weiter).

    ![Ein Kennwort hinzufügen](./media/site-recovery-how-to-install-linux-master-target/media/image11.png)

12. Wählen Sie **Create Custom Layout** (Benutzerdefiniertes Layout erstellen) aus, und klicken Sie dann auf **Next** (Weiter).

    ![Einen Installationstyp auswählen](./media/site-recovery-how-to-install-linux-master-target/media/image12.png)

13. Wählen Sie die Partition **Free** (Frei) aus, und klicken Sie auf **Create** (Erstellen), um die Partitionen **/**, **/var/crash** und **/home** mit **ext4** als Dateisystemtyp zu erstellen. Erstellen Sie die Partition **Swap** mit **swap** als Dateisystemtyp. Verwenden Sie zum Zuordnen der Partitionsgröße die Formel für die Größenzuordnung, die in der folgenden Tabelle angegeben ist.

    > [!NOTE]
    > Der Linux-Masterzielserver darf keine Logical Volume Manager (LVM) für Stamm- oder Aufbewahrungsspeicherplätze verwenden. Das Linux-Masterziel ist so konfiguriert, dass LVM-Partitionen und Datenträger standardmäßig nicht erkannt werden.

    ![Tabelle mit den Partitionsnamen, Partitionsgrößen und Systemdateitypen](./media/site-recovery-how-to-install-linux-master-target/media/image13.png)

14. Nach dem Erstellen der Partition klicken Sie auf **Next** (Weiter).

    ![Dialogfeld, das ausgewählte Werte für Partitionen anzeigt](./media/site-recovery-how-to-install-linux-master-target/media/image14.png)

15. Falls bereits vorhandene Geräte gefunden werden, wird eine Warnmeldung zur Formatierung angezeigt. Klicken Sie auf **Format** (Formatieren), um für die Festplatte die Formatierung mit der aktuellen Partitionstabelle durchzuführen.

    ![Klicken Sie auf die Schaltfläche „Format“ (Formatieren), um den Datenträger zu formatieren.](./media/site-recovery-how-to-install-linux-master-target/media/image15.png)

16. Klicken Sie auf **Write changes to disk** (Änderungen auf Datenträger schreiben), um die Partitionsänderungen auf den Datenträger anzuwenden.

    ![Klicken Sie auf „Write changes to disk“ (Änderungen auf Datenträger schreiben).](./media/site-recovery-how-to-install-linux-master-target/media/image16.png)

17. Aktivieren Sie die Option **Install boot loader** (Startladeprogramm installieren), und klicken Sie dann auf **Next** (Weiter), um das Startladeprogramm auf der Stammpartition zu installieren.

    ![Das Startladeprogramm auf der Stammpartition installieren](./media/site-recovery-how-to-install-linux-master-target/media/image17.png)


18. Der Installationsprozess wird gestartet. Sie können den Fortschritt überwachen.

    ![Dialogfeld, das den Installationsfortschritt anzeigt](./media/site-recovery-how-to-install-linux-master-target/media/image18.png)

19. Nachdem der Installationsvorgang erfolgreich abgeschlossen wurde, wird der folgende Bildschirm angezeigt. Klicken Sie auf **Reboot** (Neu starten).

    ![Installation erfolgreich](./media/site-recovery-how-to-install-linux-master-target/media/image19.png)


### <a name="post-installation-steps"></a>Schritte nach der Installation
Als Nächstes bereiten Sie den Computer auf die Konfiguration als Masterzielserver vor.

Aktivieren Sie den Parameter **disk.EnableUUID = TRUE**, um die ID für jede SCSI-Festplatte eines virtuellen Linux-Computers zu erhalten.

Um diesen Parameter zu aktivieren, verwenden Sie die folgenden Schritte:

1. Fahren Sie den virtuellen Computer herunter.

2. Klicken Sie mit der rechten Maustaste auf den Eintrag des virtuellen Computers im linken Bereich, und wählen Sie dann **Edit Settings** (Einstellungen bearbeiten) aus.

3. Klicken Sie auf die Registerkarte **Options** .

4. Wählen Sie im linken Bereich **Advanced (Erweitert) &gt; General (Allgemein)** aus, und klicken Sie dann rechts auf die Schaltfläche **Configuration Parameters** (Konfigurationsparameter).

    ![Registerkarte „Options“ (Optionen)](./media/site-recovery-how-to-install-linux-master-target/media/image20.png)

    Die Option **Configuration Parameters** (Konfigurationsparameter) ist nicht verfügbar, wenn der Computer ausgeführt wird. Um diese Registerkarte zu aktivieren, fahren Sie den virtuellen Computer herunter.

5. Prüfen Sie, ob bereits eine Zeile mit **disk.EnableUUID** vorhanden ist.

    - Wenn der Wert vorhanden ist und auf **False** festgelegt ist, ändern Sie den Wert in **True** (Bei den Werten wird die Groß-/Kleinschreibung nicht berücksichtigt).

    - Falls der Wert vorhanden und auf **True** festgelegt ist, klicken Sie auf **Cancel** (Abbrechen).

    - Ist der Wert nicht vorhanden, klicken Sie auf **Add Row** (Zeile hinzufügen).

    - Fügen Sie in der Spalte **Name** **disk.EnableUUID** hinzu, und setzen Sie den Wert auf **TRUE**.

    ![Überprüfen, ob disk.EnableUUID bereits vorhanden ist](./media/site-recovery-how-to-install-linux-master-target/media/image21.png)

#### <a name="download-and-install-additional-packages"></a>Herunterladen und Installieren zusätzlicher Pakete

> [!NOTE]
> Stellen Sie sicher, dass Sie über eine Internetverbindung verfügen, um zusätzliche Pakete herunterzuladen und zu installieren. Ohne Internetverbindung müssen Sie diese RPM-Pakete manuell suchen und installieren.

```
yum install -y xfsprogs perl lsscsi rsync wget kexec-tools
```

Mit dem vorherigen Befehl werden die folgenden 15 Pakete aus dem CentOS 6.6-Repository heruntergeladen und installiert. Wenn Sie über keinen Internetzugriff verfügen, müssen Sie die folgenden RPM-Pakete herunterladen:


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
> Wenn für Computer mit Quellschutz für das Stamm- oder Startgerät das ReiserFS- oder das XFS-Dateisystem verwendet wird, müssen Sie vor dem Schutzvorgang die folgenden zusätzlichen Pakete herunterladen und auf dem Linux-Masterziel installieren.


***ReiserFS (bei Verwendung in Suse11SP3. ReiserFS ist nicht das Standarddateisystem in Suse11SP3.)***

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
Ist erforderlich, um die Verwendung von Multipfadpaketen auf dem Masterziel zu ermöglichen.

### <a name="get-the-installer-for-setup"></a>Herunterladen des Installationsprogramms zur Einrichtung

Wenn Ihr Masterziel über eine Internetverbindung verfügt, können Sie die folgenden Schritte durchführen, um das Installationsprogramm herunterzuladen. Andernfalls können Sie das Installationsprogramm vom Prozessserver kopieren und installieren.

#### <a name="download-the-master-target-installation-packages"></a>Herunterladen der Pakete für die Installation des Masterziels

[Laden Sie die neuesten Linux-Masterziel-Installationsbits herunter](https://aka.ms/latestlinuxmobsvc).

Geben Sie zum Herunterladen mit Linux Folgendes ein:

```
wget https://aka.ms/latestlinuxmobsvc -O latestlinuxmobsvc.tar.gz
```

Stellen Sie sicher, dass Sie das Installationsprogramm herunterladen und im Basisverzeichnis entzippen. Wenn Sie es in „/usr/Local“ entzippen, schlägt die Installation fehl.


#### <a name="access-the-installer-from-the-process-server"></a>Zugriff auf das Installationsprogramm über den Prozessserver

1. Navigieren Sie auf dem Prozessserver zu „C:\Program Files (x86)\Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository“.

2. Kopieren Sie die erforderliche Installationsdatei vom Prozessserver, und speichern Sie sie unter „latestlinuxmobsvc.tar.gz“ in Ihrem Basisverzeichnis.


### <a name="apply-custom-configuration-changes"></a>Anwenden benutzerdefinierter Konfigurationsänderungen

Gehen Sie zum Vornehmen von Änderungen an der benutzerdefinierten Konfiguration folgendermaßen vor:


1. Führen Sie den folgenden Befehl aus, um die Binärdatei zu entpacken.
    ```
    tar -zxvf latestlinuxmobsvc.tar.gz
    ```
    ![Screenshot des ausgeführten Befehls](./media/site-recovery-how-to-install-linux-master-target/image16.png)

2. Führen Sie den folgenden Befehl aus, um die Berechtigung zu erteilen.
    ```
    chmod 755 ./ApplyCustomChanges.sh
    ```

3. Führen Sie den folgenden Befehl aus, um das Skript auszuführen.
    ```
    ./ApplyCustomChanges.sh
    ```
> [!NOTE]
> Führen Sie das Skript nur einmal auf dem Server aus. Fahren Sie den Server herunter. Starten Sie den Server neu, nachdem Sie einen Datenträger hinzufügen, wie in den nächsten Schritten beschrieben.

### <a name="add-a-retention-disk-to-the-linux-master-target-virtual-machine"></a>Hinzufügen eines Aufbewahrungsdatenträgers mit dem virtuellen Linux-Masterzielcomputer

Führen Sie die folgenden Schritte aus, um einen Aufbewahrungsdatenträger zu erstellen:

1. Fügen Sie einen neuen **1 TB**-Datenträger auf dem virtuellen Linux-Masterzielcomputer hinzu, und **starten** Sie den Computer.

2. Ermitteln Sie mit dem Befehl **multipath -ll** die Multipfad-ID des Aufbewahrungsdatenträgers.

    ```
    multipath -ll
    ```

    ![Die Multipfad-ID des Aufbewahrungsdatenträgers](./media/site-recovery-how-to-install-linux-master-target/media/image22.png)

3. Formatieren Sie das Laufwerk, und erstellen Sie ein Dateisystem auf dem neuen Laufwerk.

    ```
    mkfs.ext4 /dev/mapper/<Retention disk's multipath id>
    ```
    ![Erstellen eines Dateisystems auf dem Laufwerk](./media/site-recovery-how-to-install-linux-master-target/media/image23.png)

4. Nach dem Erstellen des Dateisystems stellen Sie den Aufbewahrungsdatenträger bereit.
    ```
    mkdir /mnt/retention
    mount /dev/mapper/<Retention disk's multipath id> /mnt/retention
    ```

    ![Bereitstellen des Aufbewahrungsdatenträgers](./media/site-recovery-how-to-install-linux-master-target/media/image24.png)

5. Erstellen Sie den **fstab**-Eintrag, um bei jedem Neustart einen Aufbewahrungsdatenträger bereitzustellen.
    ```
    vi /etc/fstab
    ```
    Drücken Sie die **EINFG-TASTE**, um mit der Bearbeitung der Datei zu beginnen. Erstellen Sie eine neue Zeile, und fügen Sie den folgenden Text ein. Bearbeiten Sie die Multipfad-ID des Datenträgers auf Basis der hervorgehobenen Multipfad-ID aus dem vorherigen Befehl.

    **/dev/mapper/<Retention disks multipath id> /mnt/retention ext4 rw 0 0**

    Drücken Sie die **ESC-TASTE**, und geben Sie **:wq** (Schreiben und Beenden) ein, um das Editor-Fenster zu schließen.

### <a name="install-the-master-target"></a>Installieren des Masterziels

> [!IMPORTANT]
> Die Version des Masterzielservers darf nicht höher als die des Prozess- und des Konfigurationsservers sein. Wenn diese Bedingung nicht erfüllt ist, funktioniert nur das erneute Schützen, die Replikation schlägt fehl.


> [!NOTE]
> Vergewissern Sie sich vor dem Installieren des Masterzielservers, dass die Datei „/etc/hosts“ auf dem virtuellen Computer Einträge enthält, mit denen der Name des lokalen Hosts den IP-Adressen der einzelnen Netzwerkkarten zugeordnet wird.

1. Kopieren Sie die Passphrase aus „C:\ProgramData\Microsoft Azure Site Recovery\private\connection.passphrase“ auf dem Konfigurationsserver, und speichern Sie sie dann in „passphrase.txt“ im selben lokalen Verzeichnis, indem Sie folgenden Befehl ausführen.

    ```
    echo <passphrase> >passphrase.txt
    ```
    Beispiel: echo itUx70I47uxDuUVY >passphrase.txt

2. Notieren Sie die IP-Adresse des Konfigurationsservers. Sie benötigen sie im nächsten Schritt.

3. Führen Sie den folgenden Befehl aus, um den Masterzielserver zu installieren und den Server auf dem Konfigurationsserver zu registrieren.

    ```
    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i <Configuration Server IP Address> -p 443 -s y -c https -P passphrase.txt
    ```

    Beispiel: ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

    Warten Sie, bis das Skript abgeschlossen wurde. Wenn das Masterziel erfolgreich registriert wurde, ist es auf der Site Recovery-Infrastrukturseite im Portal aufgeführt.

#### <a name="install-the-master-target-by-using-interactive-install"></a>Installieren des Masterziels mithilfe der interaktiven Installation

1. Führen Sie den folgenden Befehl aus, um das Masterziel zu installieren. Wählen Sie als Agent-Rolle die Option **Master Target** (Masterziel).

    ```
    ./install
    ```

2. Wählen Sie den standardmäßigen Speicherort für die Installation, und drücken Sie die **EINGABETASTE**, um fortzufahren.

    ![Ein Standardverzeichnis für die Installation des Masterziels auswählen](./media/site-recovery-how-to-install-linux-master-target/image17.png)


3. Wählen Sie die zu konfigurierenden **globalen** Einstellungen aus.

    ![Globale Einstellungen konfigurieren](./media/site-recovery-how-to-install-linux-master-target/image18.png)

4. Geben Sie die IP-Adressen des Konfigurationsservers an.

5. Geben Sie für den Port des Konfigurationsservers 443 an.

    ![Angeben von IP-Adresse und Port für den Konfigurationsserver](./media/site-recovery-how-to-install-linux-master-target/image19.png)

6. Kopieren Sie die Passphrase des Konfigurationsservers unter „C:\ProgramData\Microsoft Azure Site Recovery\private\connection.passphrase“ auf dem Konfigurationsserver, und fügen Sie sie ins Feld **Passphrase** ein. Das Feld ist leer, auch wenn Sie den Text eingefügt haben.

7. Wechseln Sie in das Menü **Quit** (Beenden).

8. Warten Sie, bis der Installations- und Registrierungsvorgang abgeschlossen ist.

### <a name="install-vmware-tools-on-the-master-target-server"></a>Installieren von VMware-Tools auf dem Masterzielserver

Installieren Sie VMware-Tools auf dem Masterziel, damit es die Datenspeicher erkennen kann. Wenn die Tools nicht installiert werden, werden die Datenspeicher nicht auf dem Bildschirm für das erneute Schützen angezeigt. Sie müssen nach der Installation von VMware-Tools einen Neustart durchführen.

## <a name="next-steps"></a>Nächste Schritte
Nach der Installation und Registrierung des Masterziels erscheint das Masterziel im Abschnitt **Masterziel** in der **Site Recovery-Infrastruktur** in der Übersicht der Konfigurationsserver.

Sie können jetzt mit [Reprotection](site-recovery-how-to-reprotect.md) (Erneuter Schutz) und Failback fortfahren.

## <a name="common-issues"></a>Häufige Probleme

* Stellen Sie sicher, dass Sie Storage vMotion nicht auf Verwaltungskomponenten wie dem Masterziel aktivieren. Wenn das Masterziel nach erfolgreichem erneuten Schützen verschoben wird, können die Datenträger des virtuellen Computers nicht getrennt werden, und das Failback schlägt fehl.
* Das Masterziel darf keine Momentaufnahmen auf dem virtuellen Computer aufweisen. Wenn Momentaufnahmen vorhanden sind, schlägt das Failback fehl.
* Aufgrund von benutzerdefinierten NIC-Konfigurationen bei einigen Kunden ist die Netzwerkschnittstelle während des Systemstarts deaktiviert, und der Masterziel-Agent kann nicht initialisiert werden. Stellen Sie sicher, dass die folgenden Eigenschaften richtig festgelegt sind. Überprüfen Sie diese Eigenschaften in den Dateien der Ethernet-Karte: „/etc/sysconfig/network-scripts/ifcfg-eth*“.
        * BOOTPROTO=dhcp * ONBOOT=yes

