---
title: 
description: 
services: site-recovery
documentationcenter: 
author: 
manager: 
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 
ms.author: 
translationtype: Human Translation
ms.sourcegitcommit: 4871e5959483058aa957e3373ddc5121b398e1d9
ms.openlocfilehash: 1d6d595bf788fc3fdba0bbc98887614e17c791dc
ms.lasthandoff: 02/17/2017


---



### <a name="install-the-mobility-service"></a>Installieren des Mobilitätsdiensts
Der erste Schritt beim Aktivieren des Schutzes für virtuelle Computer und physische Server besteht im Installieren des Mobilitätsdiensts. Dazu haben Sie mehrere Möglichkeiten:

* **Serverpushvorgänge verarbeiten**: Wenn Sie die Replikation auf einem Computer aktivieren, übertragen Sie die Mobilitätsdienstkomponente mittels Push vom Prozessserver, und installieren Sie sie. 
*Beachten Sie*, dass die Pushinstallation nicht auftritt, wenn auf den Computern bereits eine aktuelle Version der Komponente ausgeführt wird.
* **Pushvorgang im Unternehmen**: Installieren Sie die Komponente automatisch mithilfe des in Ihrem Unternehmen verwendeten Pushprozesses, z.B. WSUS (Windows Server Update Services) oder System Center Configuration Manager oder [Azure Automation und Desired State Configuration](site-recovery-automate-mobility-service-install.md). Richten Sie zuvor den Konfigurationsserver ein.
* **Manuelle Installation**: Installieren Sie die Komponente manuell auf jedem Computer, den Sie replizieren möchten. Richten Sie zuvor den Konfigurationsserver ein.

#### <a name="prepare-for-automatic-push-on-windows-machines"></a>Vorbereiten des automatischen Pushs auf Windows-Computern
Im Folgenden wird beschrieben, wie Sie Windows-Computer vorbereiten, damit der Prozessserver automatisch den Mobilitätsdienst darauf installieren kann.

1. Erstellen Sie ein Konto, mit dem der Prozessserver auf den Computer zugreifen kann. Das Konto muss über Administratorrechte (lokal oder für die Domäne) verfügen und es wird nur für die Pushinstallation verwendet.

   > [!NOTE]
   > Wenn Sie kein Domänenkonto verwenden, müssen Sie die Remote-Benutzerzugriffssteuerung auf dem lokalen Computer deaktivieren. Zu diesem Zweck fügen Sie in der Registrierung unter „HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System“ den DWORD-Eintrag „LocalAccountTokenFilterPolicy“ mit dem Wert 1 hinzu. Um den Registrierungseintrag von einer Befehlszeilenschnittstelle aus hinzuzufügen, geben Sie **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**ein.
   >
   >
2. Wählen Sie in der Windows-Firewall des Computers, den Sie schützen möchten, **Eine App oder ein Feature durch die Windows-Firewall zulassen**aus. Aktivieren Sie **Datei- und Druckerfreigabe** und **Windows-Verwaltungsinstrumentation**. Für Computer, die einer Domäne angehören, können Sie die Firewalleinstellungen mit einem Gruppenrichtlinienobjekt konfigurieren.

   ![Firewalleinstellungen](./media/site-recovery-vmware-to-azure/mobility1.png)
3. Fügen Sie das von Ihnen erstellte Konto hinzu:

   * Öffnen Sie **cspsconfigtool**. Es steht als Verknüpfung auf dem Desktop zur Verfügung und befindet sich im Ordner [INSTALLATIONSORT]\home\svsystems\bin.
   * Klicken Sie auf der Registerkarte **Konten verwalten** auf **Konto hinzufügen**.
   * Fügen Sie das von Ihnen erstellte Konto hinzu. Nach dem Hinzufügen des Kontos müssen Sie die Anmeldeinformationen angeben, wenn Sie die Replikation für einen Computer aktivieren.

#### <a name="prepare-for-automatic-push-on-linux-servers"></a>Vorbereiten des automatischen Pushs auf Linux-Servern
1. Vergewissern Sie sich, dass der zu schützende Linux-Computer unterstützt wird, wie unter [Voraussetzungen für geschützte Computer](#protected-machine-prerequisites)beschrieben. Stellen Sie sicher, dass zwischen dem Linux-Computer und dem Prozessserver eine Netzwerkkonnektivität besteht.
2. Erstellen Sie ein Konto, mit dem der Prozessserver auf den Computer zugreifen kann. Das Konto sollte ein Root-Benutzer auf dem Linux-Quellserver sein und wird nur für die Pushinstallation verwendet.

   * Öffnen Sie **cspsconfigtool**. Es steht als Verknüpfung auf dem Desktop zur Verfügung und befindet sich im Ordner [INSTALLATIONSORT]\home\svsystems\bin.
   * Klicken Sie auf der Registerkarte **Konten verwalten** auf **Konto hinzufügen**.
   * Fügen Sie das von Ihnen erstellte Konto hinzu. Nach dem Hinzufügen des Kontos müssen Sie die Anmeldeinformationen angeben, wenn Sie die Replikation für einen Computer aktivieren.
3. Vergewissern Sie sich, dass die Datei „/etc/hosts“ auf dem Linux-Quellserver Einträge enthält, die den Namen des lokalen Hosts den IP-Adressen zuordnen, die allen Netzwerkkarten zugewiesen sind.
4. Installieren Sie die neuesten „openssh“-, „openssh-server“- und „openssl“-Pakete auf dem Computer, den Sie replizieren möchten.
5. Stellen Sie sicher, dass SSH auf Port 22 aktiviert ist und ausgeführt wird.
6. Aktivieren Sie das SFTP-Subsystem und die Kennwortauthentifizierung in der Datei „sshd_config“ wie folgt:

   * Melden Sie sich als Root-Benutzer an.
   * Suchen Sie in der Datei „/etc/ssh/sshd_config“ die Zeile, die mit **PasswordAuthentication** beginnt.
   * Heben Sie die Auskommentierung der Zeile auf, und ändern Sie den Wert von **no** in **yes**.
   * Suchen Sie die Zeile, die mit **Subsystem** beginnt, und heben Sie die Auskommentierung der Zeile auf.

     ![Linux](./media/site-recovery-vmware-to-azure/mobility2.png)

### <a name="install-the-mobility-service-manually"></a>Manuelles Installieren des Mobilitätsdiensts
Die Installationsprogramme stehen auf dem Konfigurationsserver unter **C:\Programme (x86)\Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository** zur Verfügung.

| Quellbetriebssystem | Installationsdatei für den Mobilitätsdienst |
| --- | --- |
| Windows Server (nur&64; Bit) |Microsoft-ASR_UA_9.*.0.0_Windows_* release.exe |
| CentOS 6.4, 6.5, 6.6 (nur 64 Bit) |Microsoft-ASR_UA_9.*.0.0_RHEL6-64_*release.tar.gz |
| SUSE Linux Enterprise Server 11 SP3 (nur 64 Bit) |Microsoft-ASR_UA_9.*.0.0_SLES11-SP3-64_*release.tar.gz |
| Oracle Enterprise Linux 6.4, 6.5 (nur 64 Bit) |Microsoft-ASR_UA_9.*.0.0_OL6-64_*release.tar.gz |

#### <a name="install-mobility-service-on-a-windows-server"></a>Installieren des Mobilitätsdiensts auf einem Windows Server
1. Laden Sie das entsprechende Installationsprogramm herunter, und führen Sie es aus.
2. Wählen Sie unter **Before you begin** (Vorbereitung) die Option **Mobility service** (Mobilitätsdienst) aus.

    ![Mobilitätsdienst](./media/site-recovery-vmware-to-azure/mobility3.png)
3. Geben Sie unter **Configuration Server Details** (Konfigurationsserverdetails) die IP-Adresse des Konfigurationsservers und die Passphrase ein, die beim Ausführen des einheitlichen Setups generiert wurden. Sie können die Passphrase abrufen, indem Sie auf dem Konfigurationsserver **<SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe –v** ausführen.

    ![Mobilitätsdienst](./media/site-recovery-vmware-to-azure/mobility6.png)
4. Behalten Sie unter **Install Location** (Installationsort) die Standardeinstellung bei und klicken Sie auf **Next** (Weiter), um mit der Installation zu beginnen.
5. Überwachen Sie die Installation unter **Installation Progress** (Installationsstatus), und starten Sie den Computer neu, wenn Sie dazu aufgefordert werden. Nach der Installation des Diensts kann es etwa 15 Minuten dauern, bis der Status im Portal aktualisiert wird.

#### <a name="install-mobility-service-on-a-windows-server-using-the-command-prompt"></a>Installieren des Mobilitätsdiensts auf einem Windows-Server über die Eingabeaufforderung
1. Kopieren Sie das Installationsprogramm in einen lokalen Ordner (z.B. C:\Temp) auf dem Server, den Sie schützen möchten. Das Installationsprogramm befindet sich auf dem Konfigurationsserver unter **[Installationsort]\home\svsystems\pushinstallsvc\repository**. Das Paket für Windows-Betriebssysteme verfügt über einen Namen, der etwa „Microsoft-ASR_UA_9.3.0.0_Windows_GA_17thAug2016_release.exe“ lautet.
2. Benennen Sie diese Datei in „MobilitySvcInstaller.exe“ um.
3. Führen Sie den folgenden Befehl aus, um das MSI-Installationsprogramm zu extrahieren:

    ``C:\> cd C:\Tempww
    ``C:\Temp > MobilitySvcInstaller.exe/q / /xC:\Temp\Extracted``
    ``C:\Temp > cd extrahiert``
    ``C:\Temp\Extracted > UnifiedAgent.exe/Role "Agent" /CSEndpoint "IP-Adresse des Konfigurationsservers"/PassphraseFilePath <Full path to the passphrase file>``

##### <a name="full-command-line-syntax"></a>Vollständige Befehlszeilensyntax
    UnifiedAgent.exe [/Role <Agent/MasterTarget>] [/InstallLocation <Installation Directory>] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>] [/LogFilePath <Log File Path>]<br/>

**Parameter**

* **/Role:** Obligatorisch. Gibt an, ob der Mobilitätsdienst installiert werden soll. Eingabewerte Agent|MasterTarget
* **/InstallLocation:** Obligatorisch. Gibt an, wo der Dienst installiert werden soll.
* **/PassphraseFilePath:** Obligatorisch. Die Passphrase des Konfigurationsservers.
* **/LogFilePath:** Obligatorisch. Der Speicherort, an dem die Dateien des Installationsprotokolls erstellt werden sollen.

#### <a name="uninstall-the-mobility-service-manually"></a>Manuelles Deinstallieren des Mobilitätsdiensts
Der Mobilitätsdienst kann über „Software“ in der Systemsteuerung oder mithilfe der Befehlszeilenanweisung MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} deinstalliert werden.

#### <a name="install-the-mobility-service-on-a-linux-server"></a>Installieren des Mobilitätsdiensts auf einem Linux-Server
1. Kopieren Sie anhand der obigen Tabelle das geeignete TAR-Archiv auf den zu replizierenden Linux-Computer.
2. Öffnen Sie ein Shellprogramm, und extrahieren Sie das gezippte TAR-Archiv in einen lokalen Pfad, indem Sie Folgendes ausführen: `tar -xvzf Microsoft-ASR_UA_8.5.0.0*`
3. Erstellen Sie in dem lokalen Verzeichnis, in das Sie den Inhalt des TAR-Archivs extrahiert haben, die Datei „passphrase.txt“. Hierzu kopieren Sie die Passphrase aus „C:\ProgramData\Microsoft Azure Site Recovery\private\connection.passphrase“ auf dem Konfigurationsserver, und speichern Sie sie dann in „passphrase.txt“, indem Sie *`echo <passphrase> >passphrase.txt`* in der Shell ausführen.
4. Installieren Sie den Mobilitätsdienst, indem Sie *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*ausführen.
5. Geben Sie die interne IP-Adresse des Konfigurationsservers an, und stellen Sie sicher, dass der Port 443 ausgewählt ist. Nach der Installation des Diensts kann es etwa 15 Minuten dauern, bis der Status im Portal aktualisiert wird.

**Sie können die Installation auch über die Befehlszeile ausführen**:

Kopieren Sie die Passphrase aus „C:\Programme (x86)\InMage Systems\private\connection“ auf dem Konfigurationsserver, und speichern Sie sie in „passphrase.txt“ auf dem Konfigurationsserver. Führen Sie anschließend die aufgeführten Befehle aus. In unserem Beispiel wird für den Konfigurationsserver die IP-Adresse 104.40.75.37 verwendet, und der HTTPS-Port sollte 443 sein:


So installieren Sie den Dienst auf einem Produktionsserver:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

So installieren Sie den Dienst auf dem Masterzielserver:

    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt




