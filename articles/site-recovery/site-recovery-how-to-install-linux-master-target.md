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
ms.date: 08/11/2017
ms.author: ruturajd
ms.translationtype: HT
ms.sourcegitcommit: a9cfd6052b58fe7a800f1b58113aec47a74095e3
ms.openlocfilehash: 5341e3e56e0c366079958dd9a885f6ee3e8436cb
ms.contentlocale: de-de
ms.lasthandoff: 08/12/2017

---
# <a name="install-a-linux-master-target-server"></a>Installieren eines Linux-Masterzielservers
Nach dem Failover Ihrer virtuellen Computer können Sie für die virtuellen Computer ein Failback zum lokalen Standort ausführen. Für ein Failback müssen Sie den virtuellen Computer von Azure zum lokalen Standort erneut schützen. Für diesen Prozess benötigen Sie einen lokalen Masterzielserver, der den Datenverkehr empfängt. 

Falls es sich bei Ihrem geschützten virtuellen Computer um einen virtuellen Windows-Computer handelt, benötigen Sie ein Windows-Masterziel. Bei einem virtuellen Linux-Computer benötigen Sie ein Linux-Masterziel. Lesen Sie die folgenden Schritte, um Informationen zum Erstellen und Installieren eines Linux-Masterziels zu erhalten.

> [!IMPORTANT]
> Ab Version 9.10.0 des Masterzielservers kann der neueste Masterzielserver nur auf einem Ubuntu 16.04 Server installiert werden. Neue Installationen sind auf CentOS6.6-Servern nicht zugelassen. Sie können Ihre alten Masterzielserver jedoch weiterhin mithilfe der Version 9.10.0 aktualisieren.

## <a name="overview"></a>Übersicht
Dieser Artikel enthält Anleitungen zum Installieren eines Linux-Masterziels.

Kommentare oder Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.

## <a name="prerequisites"></a>Voraussetzungen

* Zur Auswahl des Hosts, auf dem Sie das Masterziel bereitstellen, ermitteln Sie, ob das Failback zu einem vorhandenen, lokalen virtuellen Computer oder zu einem neuen virtuellen Computer führt. 
    * Bei einem vorhandenen virtuellen Computer muss der Host des Masterziels über Zugriff auf die Datenspeicher des virtuellen Computers verfügen.
    * Falls kein lokaler virtueller Computer vorhanden ist, wird das Failback des virtuellen Computers auf demselben Host wie das Masterziel erstellt. Sie können einen beliebigen ESXi-Host zum Installieren des Masterziels verwenden.
* Das Masterziel sollte sich in einem Netzwerk befinden, das mit dem Prozessserver und dem Konfigurationsserver kommunizieren kann.
* Die Version des Masterziels darf nicht höher als die des Prozess- und des Konfigurationsservers sein. Beispiel: Bei Version 9.4 des Konfigurationsservers kann die Version des Masterziels 9.4 oder 9.3, aber nicht 9.5 sein.
* Das Masterziel kann nur ein virtueller VMware-Computer und kein physischer Server sein.

## <a name="create-the-master-target-according-to-the-sizing-guidelines"></a>Erstellen des Masterziels gemäß den Richtlinien zum Festlegen der Größe

Erstellen Sie das Masterziel gemäß den folgenden Richtlinien zum Festlegen der Größe:
- **RAM:** 6 GB oder mehr
- **Größe des Betriebssystemdatenträgers:** 100 GB oder mehr (für die CentOS6.6-Installation)
- **Zusätzliche Datenträgergröße für Aufbewahrungslaufwerk:** 1 TB
- **CPU-Kerne:** mindestens 4 Kerne

Folgende Ubuntu-Kernels werden unterstützt:


|Kernelserie  |Unterstützung bis  |
|---------|---------|
|4.4      |4.4.0-81-generic         |
|4.8      |4.8.0-56-generic         |
|4.10     |4.10.0-24-generic        |


## <a name="deploy-the-master-target-server"></a>Bereitstellen des Masterzielservers

### <a name="install-ubuntu-16042-minimal"></a>Installieren Sie Ubuntu 16.04.2 Minimal

Führen Sie die folgenden Schritte aus, um das 64-Bit-Betriebssystem Ubuntu 16.04.2 zu installieren:

**Schritt 1:** Rufen Sie den [Downloadlink](https://www.ubuntu.com/download/server/thank-you?version=16.04.2&architecture=amd64) auf, und wählen Sie den nächstgelegenen Spiegel aus, von dem Sie ein ISO-Image mit Ubuntu 16.04.2 minimal 64 Bit herunterladen.

Legen Sie den Datenträger mit dem ISO-Image für Ubuntu 16.04.2 minimal 64 Bit in das DVD-Laufwerk ein, und starten Sie das System.

**Schritt 2:** Wählen Sie **Englisch** als bevorzugte Sprache aus, und drücken Sie die **EINGABETASTE**.

![Sprache auswählen](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image1.png)

**Schritt 3:** Wählen Sie **Ubuntu Server installieren**, und drücken Sie die **EINGABETASTE**.

![„Ubuntu Server installieren“ auswählen](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image2.png)

**Schritt 4:** Wählen Sie **Englisch** als bevorzugte Sprache aus, und drücken Sie die **EINGABETASTE**.

![Englisch als bevorzugte Sprache auswählen](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image3.png)

**Schritt 5:** Wählen Sie die entsprechende Option aus den Optionen **Zeitzone** aus, und drücken Sie **EINGABETASTE**.

![Richtige Zeitzone auswählen](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image4.png)

**Schritt 6:** Wählen Sie **Nein** (die Standardoption) aus, und drücken Sie die **EINGABETASTE**.


![Tastatur konfigurieren](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image5.png)

**Schritt 7:** Wählen Sie **Englisch (USA)** als Ursprungsland für die Tastatur aus, und drücken Sie die **EINGABETASTE**.

![USA als Ursprungsland auswählen](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image6.png)

**Schritt 8:** Wählen Sie **Englisch (USA)** als Tastaturlayout aus, und drücken Sie die **EINGABETASTE**.

![Englisch (USA) als Tastaturlayout auswählen](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image7.png)

**Schritt 9:** Geben Sie im Feld **Hostname** den Hostnamen für Ihren Server ein, und wählen Sie dann **Weiter** aus.

![Hostnamen für Ihren Server eingeben](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image8.png)

**Schritt 10:** Erstellen Sie ein Benutzerkonto, indem Sie den Benutzernamen eingeben, und wählen Sie dann **Weiter** aus.

![Erstellen eines Benutzerkontos](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image9.png)

**Schritt 11:** Geben Sie das Kennwort für das neue Benutzerkonto ein, und wählen Sie dann **Weiter** aus.

![Kennwort eingeben](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image10.png)

**Schritt 12:** Bestätigen Sie das Kennwort für den neuen Benutzer, und wählen Sie dann **Weiter** aus.

![Kennwörter bestätigen](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image11.png)

**Schritt 13:** Wählen Sie **Nein** (die Standardoption) aus, und drücken Sie die **EINGABETASTE**.

![Benutzer und Kennwörter einrichten](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image12.png)

**Schritt 14:** Wenn die richtige Zeitzone angezeigt wird, wählen Sie **Ja** (die Standardoption) aus, und drücken Sie dann die **EINGABETASTE**.

Wenn Sie die Zeitzone ändern möchten, wählen Sie **Nein** aus.

![Uhrzeit konfigurieren](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image13.png)

**Schritt 15:** Wählen Sie aus den Optionen zum Partitionierungsverfahren **Guided - use entire disk** (Geführt – gesamten Datenträger verwenden) aus, und drücken Sie die **EINGABETASTE**.

![Option für Partitionierungsverfahren auswählen](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image14.png)

**Schritt 16:** Wählen Sie den geeigneten Datenträger aus den Optionen **Select disk to partition** (Zu partitionierenden Datenträger auswählen) aus, und drücken Sie die **EINGABETASTE**.


![Datenträger auswählen](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image15.png)

**Schritt 17:** Wählen Sie **Ja** aus, um die Änderungen auf den Datenträger zu schreiben, und drücken Sie die **EINGABETASTE**.

![Änderungen auf Datenträger schreiben](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image16.png)

**Schritt 18:** Wählen Sie die Standardoption und dann **Weiter** aus, und drücken Sie die **EINGABETASTE**.

![Standardoption auswählen](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image17.png)

**Schritt 19:** Wählen Sie die entsprechende Option zum Verwalten von Upgrades auf Ihrem System aus, und drücken Sie die **EINGABETASTE**.

![Option zum Verwalten von Upgrades auswählen](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image18.png)

> [!WARNING]
> Da der Azure Site Recovery-Masterzielserver eine ganz bestimmte Version von Ubuntu erfordert, müssen Sie sicherstellen, dass die Kernelupgrades für den virtuellen Computer deaktiviert sind. Wenn sie aktiviert sind, verursacht jedes reguläre Upgrade Fehlfunktionen auf dem Masterzielserver. Stellen Sie sicher, dass die Option **Keine automatischen Updates** ausgewählt ist.


**Schritt 20:** Wählen Sie die Standardoption aus. Wenn Sie OpenSSH für eine SSH-Verbindung verwenden möchten, wählen Sie die Option **OpenSSH Server** aus, und wählen Sie dann **Weiter** aus.

![Software auswählen](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image19.png)

**Schritt 21:** Wählen Sie **Ja** aus, und drücken Sie dann die **EINGABETASTE**.

![GRUB-Startladeprogramm installieren](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image20.png)

**Schritt 22:** Wählen Sie das entsprechende Gerät für die Installation des Startladeprogramms (vorzugsweise **/dev/sda**) aus, und drücken Sie dann die **EINGABETASTE**.

![Gerät für die Installation des Startladeprogramms auswählen](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image21.png)

**Schritt 23:** Wählen Sie **Weiter** aus, und drücken Sie die **EINGABETASTE**, um die Installation abzuschließen.

![Installation abschließen](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image22.png)

Nach Abschluss der Installation melden Sie sich mit den neuen Benutzeranmeldeinformationen beim virtuellen Computer an. (Weitere Informationen finden Sie in **Schritt 10**.)

Führen Sie die im folgenden Screenshot dargestellten Schritte aus, um das ROOT-Benutzerkennwort festzulegen. Melden Sie sich dann als ROOT-Benutzer an.

![ROOT-Benutzerkennwort festlegen](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image23.png)


### <a name="prepare-the-machine-for-configuration-as-a-master-target-server"></a>Vorbereiten des Computers auf die Konfiguration als Masterzielserver
Als Nächstes bereiten Sie den Computer auf die Konfiguration als Masterzielserver vor.

Aktivieren Sie den Parameter **disk.EnableUUID = TRUE**, um die ID für jede SCSI-Festplatte eines virtuellen Linux-Computers abzurufen.

Um diesen Parameter zu aktivieren, führen Sie die folgenden Schritte aus:

1. Fahren Sie den virtuellen Computer herunter.

2. Klicken Sie mit der rechten Maustaste auf den Eintrag des virtuellen Computers im linken Bereich, und wählen Sie dann **Edit Settings** (Einstellungen bearbeiten) aus.

3. Wählen Sie die Registerkarte **Options** (Optionen) aus.

4. Wählen Sie im linken Bereich **Advanced** (Erweitert)  > **General** (Allgemein) aus, und klicken Sie rechts unten in der Anzeige auf die Schaltfläche **Configuration Parameters** (Konfigurationsparameter).

    ![Registerkarte „Options“ (Optionen)](./media/site-recovery-how-to-install-linux-master-target/media/image20.png)

    Die Option **Configuration Parameters** (Konfigurationsparameter) ist nicht verfügbar, wenn der Computer ausgeführt wird. Um diese Registerkarte zu aktivieren, fahren Sie den virtuellen Computer herunter.

5. Prüfen Sie, ob bereits eine Zeile mit **disk.EnableUUID** vorhanden ist.

    - Wenn der Wert vorhanden ist und auf **False** festgelegt ist, ändern Sie den Wert in **True**. (Bei den Werten wird nicht zwischen Groß- und Kleinschreibung unterschieden.)

    - Wenn der Wert vorhanden und auf **True** festgelegt ist, wählen Sie **Cancel** (Abbrechen) aus.

    - Ist der Wert nicht vorhanden, wählen Sie **Add Row** (Zeile hinzufügen) aus.

    - Fügen Sie in der Spalte „Name“ **disk.EnableUUID** hinzu, und legen Sie den Wert auf **TRUE** fest.

    ![Überprüfen, ob disk.EnableUUID bereits vorhanden ist](./media/site-recovery-how-to-install-linux-master-target/media/image21.png)

#### <a name="disable-kernel-upgrades"></a>Deaktivieren des Kernelupgrades

Der Azure Site Recovery-Masterzielserver erfordert eine ganz bestimmte Version von Ubuntu. Stellen Sie daher sicher, dass die Kernelupgrades für den virtuellen Computer deaktiviert sind.

Wenn Kernelupgrades aktiviert sind, verursacht jedes reguläre Upgrade Fehlfunktionen auf dem Masterzielserver.

#### <a name="download-and-install-additional-packages"></a>Herunterladen und Installieren zusätzlicher Pakete

> [!NOTE]
> Stellen Sie sicher, dass Sie über eine Internetverbindung verfügen, um zusätzliche Pakete herunterzuladen und zu installieren. Wenn keine Internetverbindung vorhanden ist, müssen Sie diese RPM-Pakete manuell suchen und installieren.

```
apt-get install -y multipath-tools lsscsi python-pyasn1 lvm2 kpartx
```

### <a name="get-the-installer-for-setup"></a>Herunterladen des Installationsprogramms zur Einrichtung

Wenn Ihr Masterziel über eine Internetverbindung verfügt, können Sie die folgenden Schritte durchführen, um das Installationsprogramm herunterzuladen. Andernfalls können Sie das Installationsprogramm vom Prozessserver kopieren und anschließend installieren.

#### <a name="download-the-master-target-installation-packages"></a>Herunterladen der Pakete für die Installation des Masterziels

[Laden Sie die neuesten Linux-Masterziel-Installationsbits herunter](https://aka.ms/latestlinuxmobsvc).

Geben Sie zum Herunterladen mit Linux Folgendes ein:

```
wget https://aka.ms/latestlinuxmobsvc -O latestlinuxmobsvc.tar.gz
```

Stellen Sie sicher, dass Sie das Installationsprogramm herunterladen und im Basisverzeichnis entzippen. Wenn Sie es in **/usr/Local** entzippen, schlägt die Installation fehl.


#### <a name="access-the-installer-from-the-process-server"></a>Zugriff auf das Installationsprogramm über den Prozessserver

1. Navigieren Sie auf dem Prozessserver zu **C:\Programme (x86)\Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**.

2. Kopieren Sie die erforderliche Installationsdatei vom Prozessserver, und speichern Sie sie unter **latestlinuxmobsvc.tar.gz** in Ihrem Basisverzeichnis.


### <a name="apply-custom-configuration-changes"></a>Anwenden benutzerdefinierter Konfigurationsänderungen

Gehen Sie zum Vornehmen von Änderungen an der benutzerdefinierten Konfiguration folgendermaßen vor:


1. Führen Sie den folgenden Befehl aus, um die Binärdatei zu entpacken.
    ```
    tar -zxvf latestlinuxmobsvc.tar.gz
    ```
    ![Screenshot des auszuführenden Befehls](./media/site-recovery-how-to-install-linux-master-target/image16.png)

2. Führen Sie den folgenden Befehl aus, um die Berechtigung zu erteilen.
    ```
    chmod 755 ./ApplyCustomChanges.sh
    ```

3. Führen Sie den folgenden Befehl aus, um das Skript auszuführen.
    ```
    ./ApplyCustomChanges.sh
    ```
> [!NOTE]
> Führen Sie das Skript nur einmal auf dem Server aus. Fahren Sie den Server herunter. Starten Sie den Server anschließenden neu, nachdem Sie einen Datenträger hinzufügen, wie in den nächsten Schritten beschrieben.

### <a name="add-a-retention-disk-to-the-linux-master-target-virtual-machine"></a>Hinzufügen eines Aufbewahrungsdatenträgers mit dem virtuellen Linux-Masterzielcomputer

Führen Sie die folgenden Schritte aus, um einen Aufbewahrungsdatenträger zu erstellen:

1. Fügen Sie einen neuen 1 TB-Datenträger auf dem virtuellen Linux-Masterzielcomputer hinzu, und starten Sie den Computer anschließend.

2. Ermitteln Sie mit dem Befehl **multipath -ll** die Multipfad-ID des Aufbewahrungsdatenträgers.

    ```
    multipath -ll
    ```
    ![Die Multipfad-ID des Aufbewahrungsdatenträgers](./media/site-recovery-how-to-install-linux-master-target/media/image22.png)

3. Formatieren Sie das Laufwerk, und erstellen Sie dann ein Dateisystem auf dem neuen Laufwerk.

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

5. Erstellen Sie den **fstab**-Eintrag, um den Aufbewahrungsdatenträger bei jedem Systemstart bereitzustellen.
    ```
    vi /etc/fstab
    ```
    Drücken Sie **EINFG**, um mit der Bearbeitung der Datei zu beginnen. Erstellen Sie eine neue Zeile, und fügen Sie den folgenden Text ein. Bearbeiten Sie die Multipfad-ID des Datenträgers auf Basis der hervorgehobenen Multipfad-ID aus dem vorherigen Befehl.

    **/dev/mapper/<Retention disks multipath id> /mnt/retention ext4 rw 0 0**

    Drücken Sie **ESC**, und geben Sie **:wq** (Schreiben und Beenden) ein, um das Editor-Fenster zu schließen.

### <a name="install-the-master-target"></a>Installieren des Masterziels

> [!IMPORTANT]
> Die Version des Masterzielservers darf nicht höher als die des Prozess- und des Konfigurationsservers sein. Wenn diese Bedingung nicht erfüllt ist, funktioniert nur das erneute Schützen, die Replikation schlägt jedoch fehl.


> [!NOTE]
> Vergewissern Sie sich vor dem Installieren des Masterzielservers, dass die Datei **/etc/hosts** auf dem virtuellen Computer Einträge enthält, mit denen der lokale Hostname den IP-Adressen der einzelnen Netzwerkkarten zugeordnet wird.

1. Kopieren Sie die Passphrase unter **C:\ProgramData\Microsoft Azure Site Recovery\private\connection.passphrase** auf dem Konfigurationsserver. Speichern Sie diese dann im gleichen Verzeichnis als **passphrase.txt**, indem Sie den folgenden Befehl ausführen:

    ```
    echo <passphrase> >passphrase.txt
    ```
    Beispiel: 
    
    ```
    echo itUx70I47uxDuUVY >passphrase.txt
    ```

2. Notieren Sie die IP-Adresse des Konfigurationsservers. Sie benötigen sie im nächsten Schritt.

3. Führen Sie den folgenden Befehl aus, um den Masterzielserver zu installieren und den Server auf dem Konfigurationsserver zu registrieren.

    ```
    ./install -q -d /usr/local/ASR -r MT -v VmWare
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```

    Beispiel: 
    
    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

    Warten Sie, bis das Skript abgeschlossen wurde. Wenn das Masterziel erfolgreich registriert wurde, ist es auf der **Site Recovery-Infrastrukturseite** im Portal aufgeführt.


#### <a name="install-the-master-target-by-using-interactive-installation"></a>Installieren des Masterziels mithilfe der interaktiven Installation

1. Führen Sie den folgenden Befehl aus, um das Masterziel zu installieren. Wählen Sie als Agent-Rolle die Option **Masterziel** aus.

    ```
    ./install
    ```

2. Wählen Sie den Standardspeicherort für die Installation, und drücken Sie die **EINGABETASTE**, um fortzufahren.

    ![Ein Standardverzeichnis für die Installation des Masterziels auswählen](./media/site-recovery-how-to-install-linux-master-target/image17.png)

Nachdem die Installation abgeschlossen ist, registrieren Sie den Konfigurationsserver über die Befehlszeile.

1. Notieren Sie die IP-Adresse des Konfigurationsservers. Sie benötigen sie im nächsten Schritt.

2. Führen Sie den folgenden Befehl aus, um den Masterzielserver zu installieren und den Server auf dem Konfigurationsserver zu registrieren.

    ```
    ./install -q -d /usr/local/ASR -r MT -v VmWare
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```
    Beispiel: 

    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

   Warten Sie, bis das Skript abgeschlossen wurde. Wenn das Masterziel erfolgreich registriert wurde, wird es auf der **Site Recovery-Infrastrukturseite** im Portal aufgeführt.


### <a name="upgrade-the-master-target"></a>Aktualisieren des Masterziels

Führen Sie das Installationsprogramm aus. Es erkennt automatisch, dass der Agent auf dem Masterzielserver installiert ist. Wählen Sie zum Aktualisieren **Y** aus.  Nachdem das Setup abgeschlossen wurde, überprüfen Sie die Version des installierten Masterziels mithilfe des folgenden Befehls.

    ```
    cat /usr/local/.vx_version
    ```

Wie Sie sehen, ist im Feld **Version** die Versionsnummer des Masterziels angegeben.

### <a name="install-vmware-tools-on-the-master-target-server"></a>Installieren von VMware-Tools auf dem Masterzielserver

Installieren Sie VMware-Tools auf dem Masterziel, damit es die Datenspeicher erkennen kann. Wenn die Tools nicht installiert werden, werden die Datenspeicher nicht auf dem Bildschirm für das erneute Schützen angezeigt. Nach der Installation von VMware-Tools müssen Sie einen Neustart durchführen.

## <a name="next-steps"></a>Nächste Schritte
Nach Abschluss der Installation und Registrierung des Masterziels erscheint das Masterziel im Abschnitt **Masterziel** in der **Site Recovery-Infrastruktur** in der Übersicht der Konfigurationsserver.

Sie können jetzt mit [Reprotection](site-recovery-how-to-reprotect.md) (Erneuter Schutz) und Failback fortfahren.

## <a name="common-issues"></a>Häufige Probleme

* Stellen Sie sicher, dass Sie Storage vMotion nicht auf Verwaltungskomponenten wie dem Masterziel aktivieren. Wenn das Masterziel nach erfolgreichem erneuten Schützen verschoben wird, können die Datenträger des virtuellen Computers nicht getrennt werden. In diesem Fall tritt beim Failback ein Fehler auf.

* Das Masterziel darf keine Momentaufnahmen auf dem virtuellen Computer aufweisen. Wenn Momentaufnahmen vorhanden sind, schlägt das Failback fehl.

* Aufgrund von benutzerdefinierten NIC-Konfigurationen bei einigen Kunden ist die Netzwerkschnittstelle während des Systemstarts deaktiviert, und der Masterziel-Agent kann nicht initialisiert werden. Stellen Sie sicher, dass die folgenden Eigenschaften richtig festgelegt sind. Überprüfen Sie diese Eigenschaften in den Dateien der Ethernet-Karte: „/etc/sysconfig/network-scripts/ifcfg-eth*“.
    * BOOTPROTO=dhcp
    * ONBOOT=yes

