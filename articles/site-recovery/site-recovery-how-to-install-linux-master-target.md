---
title: Installieren des Linux-Masterzielservers | Microsoft-Dokumentation
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
ms.date: 12/20/2016
ms.author: ruturajd
translationtype: Human Translation
ms.sourcegitcommit: d1a7ed7e182530f81a426a4383297a49505f65ea
ms.openlocfilehash: d76ea0fb27ecece4e8dcd06a2dde9a0794071884


---
# <a name="how-to-install-linux-master-target-server"></a>Installieren des Linux-Masterzielservers
Der Dienst Azure Site Recovery unterstützt Ihre Strategie für Geschäftskontinuität und Notfallwiederherstellung, indem Replikation, Failover und Wiederherstellung virtueller Computer und physischer Server aufeinander abgestimmt werden. Computer können in Azure oder in einem sekundären lokalen Rechenzentrum repliziert werden. Eine kurze Übersicht über das Gesamtthema finden Sie unter [Was ist Azure Site Recovery?](site-recovery-overview.md)

## <a name="overview"></a>Übersicht
Dieser Artikel enthält Informationen und Anweisungen zur Wiederherstellung (Failover und Failback) virtueller Computer und physischer Server, die mit Site Recovery geschützt sind.

Kommentare oder Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)veröffentlichen.

## <a name="pre-requisites"></a>Voraussetzungen

1. Um den richtigen Host für die Bereitstellung des Masterziels auszuwählen, müssen Sie Folgendes ermitteln: ob das Failback auf einer lokal vorhandenen VM oder einer neuen VM durchgeführt wird (da die lokale VM gelöscht wurde). 
    * Für eine vorhandene VM sollte der Host des Masterziels Zugriff auf die Datenspeicher der VM haben.
    * Wenn die VM nicht lokal vorhanden ist, wird die Failback-VM auf demselben Host wie das Masterziel erstellt.
2. Das Masterziel sollte sich in einem Netzwerk befinden, das mit dem Prozessserver und dem Konfigurationsserver kommunizieren kann.
3. Die Versionsnummer des Masterziels sollte kleiner oder gleich der Versionsnummer des Prozessservers und Konfigurationsservers sein. (Beispiel: Wenn der Konfigurationsserver die Version 9.4 hat, können für das Masterziel die Versionen 9.4 und 9.3 verwendet werden, aber nicht 9.5.)
4. Das Masterziel kann nur eine VMware-VM sein, keine physische VM.


## <a name="steps-to-deploy-the-master-target-server"></a>Schritte zum Bereitstellen des Masterzielservers

### <a name="install-centos-66-minimal"></a>Installieren von CentOS 6.6 Minimal

Führen Sie die unten angegebenen Schritte aus, um das Betriebssystem CentOS 6.6 – 64 Bit zu installieren.

**Schritt 1:** Wählen Sie in den folgenden Links die nächstgelegene Spiegelung aus, um eine ISO-Datei mit CentOS 6.6 Minimal 64 Bit herunterzuladen.

<http://archive.kernel.org/centos-vault/6.6/isos/x86_64/CentOS-6.6-x86_64-minimal.iso>

<http://mirror.symnds.com/distributions/CentOS-vault/6.6/isos/x86_64/CentOS-6.6-x86_64-minimal.iso>

<http://bay.uchicago.edu/centos-vault/6.6/isos/x86_64/CentOS-6.6-x86_64-minimal.iso>

<http://mirror.nsc.liu.se/centos-store/6.6/isos/x86_64/CentOS-6.6-x86_64-minimal.iso>

Legen Sie den Datenträger mit der ISO-Datei mit CentOS 6.6 Minimal 64 Bit in das DVD-Laufwerk ein, und starten Sie das System.

![](./media/site-recovery-how-to-install-linux-master-target/media/image1.png)

**Schritt 2:** Wählen Sie **Skip** (Überspringen), um den Prozess zum Testen der Medien zu ignorieren.

![](./media/site-recovery-how-to-install-linux-master-target/media/image2.png)

**Schritt 3:** Der Begrüßungsbildschirm der Installation wird angezeigt. Klicken Sie auf die Schaltfläche **Next** (Weiter).

![](./media/site-recovery-how-to-install-linux-master-target/media/image3.png)

**Schritt 4:** Wählen Sie **English** (Englisch) als Sprache aus, und klicken Sie auf **Next** (Weiter), um fortzufahren.

![](./media/site-recovery-how-to-install-linux-master-target/media/image4.png)

**Schritt 5:** Wählen Sie als Tastaturlayout **US English** (Englisch USA) aus. Klicken Sie auf **Next** (Weiter), um die Installation fortzusetzen.

![](./media/site-recovery-how-to-install-linux-master-target/media/image5.png)

**Schritt 6:** Wählen Sie den Gerätetyp für die Installation. Wählen Sie **Basic storage Devices** (Grundlegende Speichergeräte).

Klicken Sie auf **Next** (Weiter), um die Installation fortzusetzen.

![](./media/site-recovery-how-to-install-linux-master-target/media/image6.png)

**Schritt 7:** Eine Warnmeldung mit dem Hinweis, dass die auf der Festplatte vorhandenen Daten gelöscht werden, wird angezeigt. Stellen Sie sicher, dass die Festplatte keine wichtigen Daten enthält, und klicken Sie auf **Yes, discard any data** (Ja, alle Daten verwerfen).

![](./media/site-recovery-how-to-install-linux-master-target/media/image7.png)

**Schritt 8:** Geben Sie den Hostnamen für Ihren Server im Textfeld **Hostname** ein.
Klicken Sie auf **Configure Network** (Netzwerk konfigurieren).

Wählen Sie im Fenster **Network Connection** (Netzwerkverbindung) Ihre Netzwerkschnittstelle aus. Klicken Sie auf die Schaltfläche **Edit** (Bearbeiten), um die IPV4-Einstellungen zu konfigurieren.

![](./media/site-recovery-how-to-install-linux-master-target/media/image8.png)

**Schritt 9:** Das folgende Fenster **Editing System eth0** (Bearbeitungssystem eth0) wird angezeigt. Aktivieren Sie das Kontrollkästchen **Connect automatically** (Automatisch verbinden). Wählen Sie auf der Registerkarte „IPv4 Settings“ (IPv4-Einstellungen) die Methode **Manual** (Manuell), und klicken Sie dann auf die Schaltfläche **Add** (Hinzufügen). Geben Sie die Details zur statischen IP-Adresse, zum Gateway und zum DNS-Server an. Klicken Sie auf **Apply** (Übernehmen), um die Details zu speichern.

![](./media/site-recovery-how-to-install-linux-master-target/media/image9.png)

**Schritt 10:** Wählen Sie im Kombinationsfeld Ihre Zeitzone aus, und klicken Sie auf **Next** (Weiter), um fortzufahren.

![](./media/site-recovery-how-to-install-linux-master-target/media/image10.png)

**Schritt 11:** Geben Sie das **Root password** (Stammkennwort) ein, und bestätigen Sie das Kennwort. Klicken Sie anschließend auf **Next** (Weiter).

![](./media/site-recovery-how-to-install-linux-master-target/media/image11.png)

**Schritt 12:** Wählen Sie als Partitionsmodus die Option **Create Custom Layout** (Benutzerdefiniertes Layout) aus, und klicken Sie auf **Next** (Weiter), um fortzufahren.

![](./media/site-recovery-how-to-install-linux-master-target/media/image12.png)

**Schritt 13:** Wählen Sie die Partition **Free** aus, und klicken Sie auf **Create** (Erstellen), um die Partitionen **/**, **/var/crash** und **/home** mit **ext4** als Dateisystemtyp zu erstellen. Erstellen Sie die Partition **Swap** mit **swap** als Dateisystemtyp. Verwenden Sie zum Zuordnen der Partitionsgröße die Formel für die Größenzuordnung, die in der Tabelle unten angegeben ist.

HINWEIS: Bei dem System mit dem Linux-basierten Masterziel (Master Target, MT) darf für den Stamm- oder Aufbewahrungspeicherplatz nicht der LVM verwendet werden. Das Linux-MT ist standardmäßig so konfiguriert, dass LVM-Partitionen/Datenträger nicht erkannt werden.

![](./media/site-recovery-how-to-install-linux-master-target/media/image13.png)

**Schritt 14:** Klicken Sie nach dem Erstellen der Partition auf **Next** (Weiter), um fortzufahren.

![](./media/site-recovery-how-to-install-linux-master-target/media/image14.png)

**Schritt 15:** Falls bereits vorhandene Geräte gefunden werden, wird eine Warnmeldung zur Formatierung angezeigt.

Klicken Sie auf **Format** (Formatieren), um für die Festplatte die Formatierung mit der aktuellen Partitionstabelle durchzuführen.

![](./media/site-recovery-how-to-install-linux-master-target/media/image15.png)



**Schritt 16:** Klicken Sie auf **Write changes to disk** (Änderungen auf Datenträger schreiben), um die Partitionsänderungen auf den Datenträger anzuwenden.

![](./media/site-recovery-how-to-install-linux-master-target/media/image16.png)

**Schritt 17:** Aktivieren Sie die Option **Install boot loader** (Startladeprogramm installieren), und klicken Sie auf **Next** (Weiter), um das Startladeprogramm auf der Stammpartition zu installieren.

![](./media/site-recovery-how-to-install-linux-master-target/media/image17.png)



**Schritt 18:** Der Installationsprozess wird gestartet. Sie können den Status der Installation verfolgen.

![](./media/site-recovery-how-to-install-linux-master-target/media/image18.png)

**Schritt 19:** Nachdem der Installationsvorgang erfolgreich abgeschlossen wurde, wird der folgende Bildschirm angezeigt. Klicken Sie auf **Reboot** (Neu starten).

![](./media/site-recovery-how-to-install-linux-master-target/media/image19.png)


### <a name="post-installation-steps"></a>Schritte nach der Installation

Aktivieren Sie den Parameter „disk.EnableUUID = TRUE“, um SCSI-IDs für die einzelnen SCSI-Festplatten eines virtuellen Linux-Computers zu erhalten.

Gehen Sie hierzu wie folgt vor:

a. Fahren Sie den virtuellen Computer herunter.

b. Klicken Sie im linken Bereich mit der rechten Maustaste auf den VM-Eintrag, und wählen Sie **Edit Settings**.

c. Klicken Sie auf die Registerkarte **Options** .

d. Klicken Sie links auf **Advanced&gt;General** und anschließend rechts auf **Configuration Parameters**.

![](./media/site-recovery-how-to-install-linux-master-target/media/image20.png)

Wenn der Computer ausgeführt wird, ist die Option für die Konfigurationsparameter deaktiviert. Fahren Sie den Computer herunter, um die Registerkarte zu aktivieren.

e. Prüfen Sie, ob bereits eine Zeile mit **disk.EnableUUID** vorhanden ist.

  Falls die Zeile vorhanden und der Wert auf „False“ festgelegt ist, überschreiben Sie den Wert mit „True“. Die Groß- und Kleinschreibung spielt dabei keine Rolle.

  Falls die Zeile vorhanden und der Wert auf „True“ festgelegt ist, klicken Sie auf „Cancel“, und testen Sie nach dem Start des Gastbetriebssystems den SCSI-Befehl.

f. Ist die Zeile nicht vorhanden, klicken Sie auf **Add Row**.

  Fügen Sie in der Spalte „Name“ die Zeichenfolge „disk.EnableUUID“ ein.

  Legen Sie den Wert auf „TRUE“ fest.

HINWEIS: Fügen Sie die oben angegebenen Werte nicht mit Anführungszeichen ein.

![](./media/site-recovery-how-to-install-linux-master-target/media/image21.png)

#### <a name="download-and-install-the-additional-packages"></a>Herunterladen und Installieren zusätzlicher Pakete

HINWEIS: Vergewissern Sie sich vor dem Herunterladen und Installieren zusätzlicher Pakete, dass das System über eine Internetverbindung verfügt.

```
# yum install -y xfsprogs perl lsscsi rsync wget kexec-tools
```

Der obige Befehl lädt die folgenden 15 Pakete aus dem CentOS 6.6-Repository herunter und installiert sie:


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

HINWEIS: Wenn für Computer mit Quellenschutz für das Stamm- oder Startgerät das Reiser- oder das XFS-Dateisystem verwendet wird, müssen vor dem Schutzvorgang die folgenden zusätzlichen Pakete heruntergeladen und auf dem Linux-Masterziel installiert werden.

***ReiserFS (Suse11SP3. ReiserFS ist in Suse11SP3 aber nicht das Standarddateisystem.)***

```
# cd /usr/local

# wget
<http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm>

# wget
<http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm>

# rpm -ivh kmod-reiserfs-0.0-1.el6.elrepo.x86\_64.rpm
reiserfs-utils-3.6.21-1.el6.elrepo.x86\_64.rpm
```

***XFS (RHEL, CentOS 7 oder höher)***

```
# cd /usr/local

# wget
<http://archive.kernel.org/centos-vault/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_64.rpm>

# rpm -ivh xfsprogs-3.1.1-16.el6.x86\_64.rpm

# yum install device-mapper-multipath 
```
Ist erforderlich, um die Verwendung von Multipfadpaketen auf dem Masterzielserver zu ermöglichen.

### <a name="download-the-mt-installation-packages"></a>Herunterladen der Pakete für die Installation des Masterziels

Laden Sie die aktuellen Daten für die Installation des Linux-Masterziels hier herunter: [https://aka.ms/latestlinuxmobsvc](https://aka.ms/latestlinuxmobsvc).

Geben Sie Folgendes ein, um den Download über Linux durchzuführen: 

```
    # wget https://aka.ms/latestlinuxmobsvc
```

Stellen Sie sicher, dass Sie das Installationsprogramm herunterladen und nur im Basisverzeichnis entzippen. Wenn Sie es in „/usr/Local“ entzippen, schlägt die Installation fehl.

### <a name="apply-custom-configuration-changes"></a>Anwenden benutzerdefinierter Konfigurationsänderungen

Schließen Sie zunächst die Schritte nach der Installation ab, bevor Sie benutzerdefinierte Konfigurationsänderungen anwenden.

Gehen Sie zum Anwenden benutzerdefinierter Konfigurationsänderungen wie folgt vor:

1. Kopieren Sie die Binärdatei für „RHEL&6;-64 Unified Agent“ in das neu erstellte Betriebssystem.

2. Führen Sie für die Binärdatei den folgenden Untar-Befehl aus:
    ```
    tar -zxvf <File name>
    ```

    ![](./media/site-recovery-how-to-install-linux-master-target/image16.png)
    
3. Führen Sie den folgenden Befehl aus, um die erforderliche Berechtigung zu erteilen:
    ```
    # chmod 755 ./ApplyCustomChanges.sh
    ```

4. Führen Sie den folgenden Befehl aus, um das Skript auszuführen:
    ```
    # ./ApplyCustomChanges.sh
    ```
HINWEIS: Führen Sie das Skript nur einmal auf dem Server aus. ****Starten Sie den Server nach erfolgreicher Ausführung des obigen Skripts neu.

### <a name="add-retention-disk-to-linux-mt-vm"></a>Hinzufügen des Aufbewahrungsdatenträgers zur Linux-Masterziel-VM 

Führen Sie die unten angegebenen Schritte aus, um einen Aufbewahrungsdatenträger zu erstellen.

**Schritt 1:** Fügen Sie an die Linux-Masterziel-VM einen neuen Datenträger mit einer Größe von **1 TB** an, und ermitteln Sie die Multipfad-ID.

Rufen Sie den Befehl **multipath -ll** auf, um die Multipfad-ID des Aufbewahrungsdatenträgers zu ermitteln.

![](./media/site-recovery-how-to-install-linux-master-target/media/image22.png)

**Schritt 2:** Rufen Sie den Befehl **mkfs.ext4 /dev/mapper/<Multipfad-ID des Aufbewahrungsdatenträgers>** auf, um auf dem Aufbewahrungsdatenträger ein Dateisystem zu erstellen.

![](./media/site-recovery-how-to-install-linux-master-target/media/image23.png)

**Schritt 3:** Rufen Sie nach Abschluss der Dateisystemerstellung die unten angegebenen Befehle auf, um den Aufbewahrungsdatenträger bereitzustellen.

![](./media/site-recovery-how-to-install-linux-master-target/media/image24.png)

**Schritt 4:** Erstellen Sie zuletzt den fstab-Eintrag:
```
vi /etc/fstab 
```
Fügen Sie folgende Zeile an:

** /dev/mapper/36000c2989daa2fe6dddcde67f2079afe /mnt/retention ext4 rw 0 0 **

### <a name="install-master-target"></a>Installieren des Masterziels


> [!NOTE]
> Die Versionsnummer des Masterzielservers sollte kleiner oder gleich der Versionsnummer des Prozessservers und Konfigurationsservers sein. Wenn die Versionsnummer des Masterziels höher ist, ist der Vorgang zum erneuten Schützen erfolgreich, aber die Replikation schlägt fehl.
> 

> [!NOTE]
> Vergewissern Sie sich vor dem Installieren des Masterzielservers, dass die Datei „/etc/hosts“ auf der VM Einträge enthält, mit denen der Name des lokalen Hosts den IP-Adressen der einzelnen Netzwerkkarten zugeordnet wird.
> 


1. Führen Sie den unten angegebenen Befehl aus, um das Masterziel zu installieren. Wählen Sie als Agent-Rolle die Option „Master Target“ (Masterziel).
```
# ./install
```

2. Wählen Sie den Standardspeicherort für die Installation aus.
    
    ![](./media/site-recovery-how-to-install-linux-master-target/image17.png)
    

3. Wählen Sie die zu konfigurierenden globalen Einstellungen aus.

    ![](./media/site-recovery-how-to-install-linux-master-target/image18.png)
    
4. Geben Sie die IP-Adressen für den CS-Server an.

5. Geben Sie den Port für den CX-Server an (normalerweise 9443).
    
    ![](./media/site-recovery-how-to-install-linux-master-target/image19.png)
    
6. Kopieren Sie die CS-Passphrase unter „C:\ProgramData\Microsoft Azure Site Recovery\private\connection.passphrase“ auf dem Konfigurationsserver.

7. Warten Sie, bis der Installations- und Registrierungsvorgang abgeschlossen ist.
### <a name="install-vmware-tools-on-the-master-target-server"></a>Installieren von VMware-Tools auf dem Masterzielserver

VMware-Tools müssen auf dem Masterziel installiert werden, damit die Datenspeicher ermittelt werden können. Wenn die Tools nicht installiert werden, werden die Datenspeicher nicht auf dem Bildschirm für das erneute Schützen angezeigt.

## <a name="next-steps"></a>Nächste Schritte
 

## <a name="common-issues"></a>Häufige Probleme



<!--HONumber=Feb17_HO3-->


