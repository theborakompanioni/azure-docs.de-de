---
title: Einrichten von Oracle ASM auf einem virtuellen Azure Linux-Computer | Microsoft-Dokumentation
description: Richten Sie in Ihrer Azure-Umgebung schnell Oracle ASM ein.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: v-shiuma
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/2/2017
ms.author: v-shiuma
ms.translationtype: Human Translation
ms.sourcegitcommit: c308183ffe6a01f4d4bf6f5817945629cbcedc92
ms.openlocfilehash: e3c98a3d4d7012f6cb6c7e1a1b8263e6ecdee57b
ms.contentlocale: de-de
ms.lasthandoff: 05/17/2017

---

# <a name="set-up-oracle-asm-on-an-azue-linux-virtual-machine"></a>Einrichten von Oracle ASM auf einem virtuellen Azure Linux-Computer 

In diesem Artikel erfahren Sie, wie Sie Oracle Automatic Storage Management (Oracle ASM) auf einem virtuellen Oracle Linux-Computer in Azure installieren und einrichten.

Stellen Sie sicher, dass die Azure CLI installiert ist, bevor Sie beginnen. Weitere Informationen finden Sie im [Azure CLI-Installationshandbuch](https://docs.microsoft.com/cli/azure/install-azure-cli). 

## <a name="sign-in-to-azure"></a>Anmelden bei Azure 

Verwenden Sie in der Azure CLI zum Anmelden bei Ihrem Azure-Abonnement den Befehl [az login](/cli/azure/#login). Befolgen Sie dann die Anweisungen auf dem Bildschirm.

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Verwenden Sie zum Erstellen einer Ressourcengruppe den Befehl [az group create](/cli/azure/group#create). Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. 

Das folgende Beispiel erstellt am Standort „westus“ eine Ressourcengruppe mit dem Namen „myResourceGroup“.

```azurecli
az group create --name myResourceGroup --location westus
```

## <a name="create-a-vm"></a>Erstellen einer VM

Führen Sie zum Erstellen eines virtuellen Computers mit Oracle ASM die folgenden Schritte aus:

1.  Verwenden Sie zum Erstellen eines virtuellen Computers den Befehl [az vm create](/cli/azure/vm#create). 

  Im folgenden Beispiel wird ein virtueller Computer namens „myVM“ erstellt. Darüber hinaus werden SSH-Schlüssel erstellt, falls sie nicht bereits an einem Standardschlüsselspeicherort vorhanden sind. Um einen bestimmten Satz von Schlüsseln zu verwenden, nutzen Sie die Option `--ssh-key-value`.  

  ```azurecli
  az vm create --resource-group myResourceGroup --name myVM --image Oracle:Oracle-Database-Ee:12.1.0.2:latest --size Standard_DS2_v2 --generate-ssh-keys
  ```

  Nach der Erstellung des virtuellen Computers zeigt die Azure CLI Informationen an, die den Informationen im folgenden Beispiel ähneln. Beachten Sie den Wert für `publicIpAddress`. Diese Adresse wird verwendet, um auf den virtuellen Computer zuzugreifen.

  ```azurecli
  {
    "fqdns": "",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
    "location": "westus",
    "macAddress": "00-0D-3A-36-2F-56",
    "powerState": "VM running",
    "privateIpAddress": "10.0.0.4",
    "publicIpAddress": "13.64.104.241",
    "resourceGroup": "myResourceGroup"
  }
  ```

2.  Fügen Sie Datenträger hinzu, die für die Oracle ASM-Konfiguration verwendet werden:

  ```azurecli
  az vm disk attach -g myResourceGroup --vm-name myVM --disk myDataDisk --new --size-gb 50
  az vm disk attach -g myResourceGroup --vm-name myVM --disk myDataDisk2 --new --size-gb 50
  az vm disk attach -g myResourceGroup --vm-name myVM --disk myDataDisk3 --new --size-gb 50
  az vm disk attach -g myResourceGroup --vm-name myVM --disk myDataDisk4 --new --size-gb 50
  ```

## <a name="connect-to-the-vm"></a>Herstellen der Verbindung zur VM

Erstellen Sie mit dem folgenden Befehl eine SSH-Sitzung mit dem virtuellen Computer. Ersetzen Sie die IP-Adresse durch den Wert für `publicIpAddress` Ihres virtuellen Computers.

```bash 
ssh <publicIpAddress>
```

## <a name="install-oracle-asm"></a>Installieren von Oracle ASM

Führen Sie zum Installieren von Oracle ASM die folgenden Schritte aus. 

Weitere Informationen zum Installieren von Oracle ASM finden Sie unter [Oracle ASMLib Downloads für Oracle Linux 6](http://www.oracle.com/technetwork/server-storage/linux/asmlib/ol6-1709075.html) (Oracle ASMLib-Downloads für Oracle Linux 6).  

1. Führen Sie `yum list` aus:

  ```bash
  $ sudo su -
  # yum list
  ```
  Bei der ersten Ausführung kann es einige Minuten dauern, bis `yum list` geladen wird.

2.  Führen Sie diese zusätzlichen Befehle aus:

  ```bash
  # yum list | grep oracleasm
  # yum -y install kmod-oracleasm.x86_64
  # yum -y install oracleasm-support.x86_64
  # wget http://download.oracle.com/otn_software/asmlib/oracleasmlib-2.0.12-1.el6.x86_64.rpm
  # yum -y install oracleasmlib-2.0.12-1.el6.x86_64.rpm
  # rm -f oracleasmlib-2.0.12-1.el6.x86_64.rpm
  ```

3.  Stellen Sie sicher, dass Oracle ASM installiert ist:

  ```bash
  # rpm -qa |grep oracleasm
  oracleasm-support-2.1.10-4.el6.x86_64
  kmod-oracleasm-2.0.8-15.el6_9.x86_64
  oracleasmlib-2.0.12-1.el6.x86_64
  ```

4.  Fügen Sie Benutzer und Gruppen hinzu:

  ```bash
  # groupadd -g 54345 asmadmin
  # groupadd -g 54346 asmdba
  # groupadd -g 54347 asmoper
  # useradd -u 3000 -g oinstall -G dba,asmadmin,asmdba,asmoper grid
  # usermod -g oinstall -G dba,asmdba,asmadmin oracle
  ```

5.  Überprüfen Sie Benutzer und Gruppen:

  ```bash
  # id grid
  uid=3000(grid) gid=54321(oinstall) groups=54321(oinstall),54322(dba),54345(asmadmin),54346(asmdba),54347(asmoper)
  ```

6.  Erstellen Sie einen Ordner, und ändern Sie den Besitzer:

  ```bash
  # mkdir /u01/app/grid
  # chown grid:oinstall /u01/app/grid
  ```

## <a name="set-up-oracle-asm"></a>Einrichten von Oracle ASM

In diesem Tutorial wird als Standardbenutzer *grid* und als Standardgruppe *asmadmin* verwendet. Stellen Sie sicher, dass der *oracle*-Benutzer Mitglied der Gruppe „asmadmin“ ist. Führen Sie zum Einrichten der Oracle ASM-Installation die folgenden Schritte aus:

1.  Legen Sie den Treiber für die Oracle ASM-Bibliothek fest:

  ```bash
  # /usr/sbin/oracleasm configure -i

  Configuring the Oracle ASM library driver.

  This will configure the on-boot properties of the Oracle ASM library
  driver. The following questions will determine whether the driver is
  loaded on boot and what permissions it will have. The current values
  will be shown in brackets ('[]'). Hitting <ENTER> without typing an
  answer will keep that current value. Ctrl-C will abort.

  Default user to own the driver interface []: grid
  Default group to own the driver interface []: asmadmin
  Start Oracle ASM library driver on boot (y/n) [n]: y
  Scan for Oracle ASM disks on boot (y/n) [y]: y
  Writing Oracle ASM library driver configuration: done
  ```

2.  Zeigen Sie die Datenträgerkonfiguration an:
  ```bash
  # cat /proc/partitions
  ```

3.  Formatieren Sie den Datenträger:

  ```bash
  # fdisk /dev/sdc
  Device contains not a valid DOS partition table, or Sun, SGI or OSF disklabel
  Building a new DOS disklabel with disk identifier 0xf865c6ca.
  Changes will remain in memory only, until you decide to write them.
  After that, of course, the previous content won't be recoverable.

  Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

  The device presents a logical sector size that is smaller than
  the physical sector size. Aligning to a physical sector (or optimal
  I/O) size boundary is recommended, or performance may be impacted.

  WARNING: DOS-compatible mode is deprecated. It's strongly recommended to
          switch off the mode (command 'c') and change display units to
          sectors (command 'u').

  Command (m for help): n
  Command action
    e   extended
    p   primary partition (1-4)
  p
  Partition number (1-4): 1
  First cylinder (1-6527, default 1):
  Using default value 1
  Last cylinder, +cylinders or +size{K,M,G} (1-6527, default 6527):
  Using default value 6527

  Command (m for help): w
  The partition table has been altered!

  Calling ioctl() to re-read partition table.
  Syncing disks.
  ```

4.  Wiederholen Sie den vorherigen Schritt für „/dev/sdd“, „/dev/sde“ und „/dev/sdf“.

5.  Überprüfen Sie die Datenträgerkonfiguration:

  ```bash
  # cat /proc/partitions
  major minor  #blocks  name

    8       16   14680064 sdb
    8       17   14678976 sdb1
    8       32   52428800 sdc
    8       33   52428096 sdc1
    8       48   52428800 sdd
    8       49   52428096 sdd1
    8       64   52428800 sde
    8       65   52428096 sde1
    8       80   52428800 sdf
    8       81   52428096 sdf1
    8        0   52428800 sda
    8        1     512000 sda1
    8        2   51915776 sda2
    11        0    1048575 sr0
  ```

6.  Überprüfen Sie den Oracle ASM-Dienststatus:

  ```bash
  # service oracleasm status
  Checking if ASM is loaded: no
  Checking if /dev/oracleasm is mounted: no
  ```

7.  Starten Sie den Oracle ASM-Dienst:

  ```bash
  # service oracleasm start
  Initializing the Oracle ASMLib driver:                     [  OK  ]
  Scanning the system for Oracle ASMLib disks:               [  OK  ]
  ```

8.  Erstellen Sie die Oracle ASM-Datenträger:

  ```bash
  # service oracleasm createdisk ASMSP /dev/sdc1
  Marking disk "ASMSP" as an ASM disk:                       [  OK  ]

  # service oracleasm createdisk DATA /dev/sdd1
  Marking disk "DATA" as an ASM disk:                        [  OK  ]

  # service oracleasm createdisk DATA1 /dev/sde1
  Marking disk "DATA1" as an ASM disk:                       [  OK  ]

  [root@myVM ~]# service oracleasm createdisk FRA /dev/sdf1
  Marking disk "FRA" as an ASM disk:                         [  OK  ]
  ```

9.  Listen Sie die Oracle ASM-Datenträger auf:

  ```bash
  # service oracleasm listdisks
  ASMSP
  DATA
  DATA1
  FRA
  ```

10. Ändern Sie die Kennwörter für die root-, oracle und grid-Benutzer (Sie verwenden die Kennwörter später während der Installation):

  ```bash
  # passwd oracle
  # passwd grid
  # passwd root
  ```

11. Ändern Sie die Ordnerberechtigung:

  ```bash
  # chmod -R 775 /opt
  # chown grid:oinstall /opt
  # chown oracle:oinstall /dev/sdc1
  # chown oracle:oinstall /dev/sdd1
  # chown oracle:oinstall /dev/sde1
  # chown oracle:oinstall /dev/sdf1
  # chmod 600 /dev/sdc1
  # chmod 600 /dev/sdd1
  # chmod 600 /dev/sde1
  # chmod 600 /dev/sdf1
  ```

## <a name="download-and-prepare-oracle-grid-infrastructure"></a>Herunterladen und Vorbereiten von Oracle Grid Infrastructure

Führen Sie zum Herunterladen der Oracle Grid Infrastructure-Software die folgenden Schritte aus:

1.  Laden Sie Oracle Grid Infrastructure von der [Oracle ASM-Downloadseite](http://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-linux-download-2240591.html) herunter. 

  Unter dem Downloadtitel **Oracle Database 12c Release 1 Grid Infrastructure (12.1.0.2.0) for Linux x86-64** sollten zwei ZIP-Dateien zum Download zur Verfügung stehen.

2.  Nachdem Sie die ZIP-Dateien auf den Clientcomputer heruntergeladen haben, können Sie mit Secure Copy Protocol (SCP) die Dateien auf den virtuellen Computer kopieren.

    ```bash
    scp *.zip <publicIpAddress>:<folder>
    ```

3.  Verschieben Sie die ZIP-Dateien in den Ordner „/opt“. Ändern Sie dann den Besitzer der Dateien:

    ```bash
    # mv <folder>/*.zip /opt
    # cd /opt
    # chown grid:oinstall linuxamd64_12102_grid_1of2.zip
    # chown grid:oinstall linuxamd64_12102_grid_2of2.zip
    ```

4.  Entzippen Sie die Dateien (installieren Sie das Linux-Hilfsprogramm zum Entpacken, sofern es noch nicht installiert ist):

    ```bash
    # yum install unzip
    # unzip linuxamd64_12102_grid_1of2.zip
    # unzip linuxamd64_12102_grid_2of2.zip
    ```

5.  Ändern Sie die Berechtigung:

    ```bash
    # chown -R grid:oinstall /opt/grid
    ```

6.  Deaktivieren Sie die Firewall:

    ```bash
    # service iptables status
    # service iptables stop
    ```

7.  Überprüfen Sie den verfügbaren Auslagerungsbereich. Sie benötigen einen Auslagerungsbereich von mindestens 6 GB, um Oracle Grid Infrastructure installieren zu können:

    ```bash
    # swapon -s
    ```

    Wenn als Auslagerungsbereich weniger als 6 GB verfügbar sind, können Sie mehr Platz hinzufügen, indem Sie die folgenden Befehle ausführen:

    ```bash
    # dd if=/dev/zero of=/extraswap bs=1M count=6144
    6144+0 records in
    6144+0 records out
    6442450944 bytes (6.4 GB) copied, 141.245 s, 45.6 MB/s

    # mkswap /mnt/resource/extraswap
    mkswap: /mnt/resource/extraswap: warning: don't erase bootbits sectors
            on whole disk. Use -f to force.
    Setting up swapspace version 1, size = 6291452 KiB
    no label, UUID=80bd7816-b3a2-4eec-a824-733209644fc5
    # swapon /mnt/resource/extraswap

    ```

## <a name="prepare-the-client-and-vm-to-run-x11-for-windows-clients-only"></a>Vorbereiten des Clients und des virtuellen Computers für die Ausführung von X11 (nur für Windows-Clients)

Führen Sie zum Vorbereiten des Clients und des virtuellen Computers für die Ausführung von X11 die folgenden Schritte aus: 

1.  Melden Sie sich als root-Benutzer an, und bearbeiten Sie die Datei „/etc/ssh/ssh_config“. Ändern Sie die Einstellung für „Forwardx11“ in `yes`:

  ```
  #   ForwardX11 no
  ForwardX11 yes

  ```

2.  Laden Sie PuTTY und Xming auf Ihren Windows-Computer herunter:

  * Laden Sie [PuTTY](http://www.putty.org/) herunter.
  * Laden Sie [Xming](https://xming.en.softonic.com/) herunter.

3.  Führen Sie nach der Installation von PuTTY im PuTTY-Order (z.B. „C:\Programme\PuTTY“) „puttygen.exe“ (PuTTY-Schlüsselgenerator) aus.

4.  Im PuTTY-Schlüsselgenerator:

  1.  Um einen Schlüssel zu generieren, wählen Sie die Schaltfläche **Generate** (Generieren). 
  2.  Kopieren Sie den Inhalt des Schlüssels (STRG+C).
  3.  Wählen Sie die Schaltfläche **Save private key** (Privaten Schlüssel speichern). 
  4.  Ignorieren Sie die angezeigte Warnung, und wählen Sie dann **OK**.

  ![Screenshot der PuTTY-Schlüsselgeneratorseite](./media/asm-configuration/puttykeygen.png)

5.  Führen Sie auf dem virtuellen Computer die folgenden Befehle aus:

  ```bash
  # sudo su - grid
  $ mkdir .ssh (if not already created)
  $ cd .ssh
  ```

6.  Erstellen Sie eine Datei namens „authorized_keys“. Fügen Sie den Inhalt des Schlüssels in dieser Datei ein, und speichern Sie die Datei.

  > [!NOTE]
  > Der Schlüssel muss die Zeichenfolge `ssh-rsa` enthalten. Zudem muss der Inhalt des Schlüssels als einzelne Textzeile eingefügt werden.
  >  

7.  Starten Sie PuTTY. Navigieren Sie im Bereich **Category** (Kategorie) zu **Connection** > **SSH** > **Auth** (Verbindung > SSH > Authentifizierung). Navigieren Sie im Feld **Private key file for authentication** (Datei des privaten Schlüssels für die Authentifizierung) zu dem zuvor erstellten Schlüssel.

  ![Screenshot der Seite zum Festlegen des privaten Schlüssels](./media/asm-configuration/setprivatekey.png)

9.  Navigieren Sie im Bereich **Category** (Kategorie) zu **Connection** > **SSH** > **X11** (Verbindung > SSH > X11). Aktivieren Sie das Kästchen **Enable X11 forwarding** (X11-Weiterleitung aktivieren).

  ![Screenshot der Seite für die X11-Aktivierung](./media/asm-configuration/enablex11.png)

10. Navigieren Sie im Bereich **Category** (Kategorie) zu **Session** (Sitzung). Geben Sie die Hostinformationen ein, und wählen Sie dann **Open** (Öffnen).

  ![Screenshot der Seite mit der Sitzung](./media/asm-configuration/puttysession.png)

## <a name="install-oracle-grid-infrastructure"></a>Installieren von Oracle Grid Infrastructure

Führen Sie zum Installieren von Oracle Grid Infrastructure die folgenden Schritte aus:

1. Melden Sie sich als grid-Benutzer an. (Sie sollten sich ohne Aufforderung zur Kennworteingabe anmelden können.) 

  > [!NOTE]
  > Stellen Sie sicher, dass Xming ausgeführt wird, bevor Sie die Installation starten.

    ```bash
    $ cd /opt/grid
    $ ./runInstaller
    ```

  Das Installationsprogramm für Oracle Grid Infrastructure 12c Release 1 wird geöffnet. (Es kann einen Moment, bis das Installationsprogramm gestartet wird.)

2. Wählen Sie auf der Seite **Select Installation Option** (Installationsoption auswählen) die Option **Install and Configure Oracle Grid Infrastructure for a Standalone Server** (Oracle Grid Infrastructure für einen eigenständigen Server installieren und konfigurieren).

  ![Screenshot der Seite „Select Installation Option“ (Installationsoption auswählen) des Installationsprogramms](./media/asm-configuration/install01.png)

3. Wählen Sie auf der Seite **Select Product Languages** (Produktsprache auswählen) die Sprache **English** (Englisch) oder die gewünschte Sprache.

  ![Screenshot der Seite „Select Product Languages“ (Produktsprache auswählen) des Installationsprogramms](./media/asm-configuration/install02.png)

4. Auf der Seite **Create ASM Disk Group** (ASM-Datenträgergruppe erstellen):
  1.  Geben Sie einen Namen für die Datenträgergruppe ein.
  2.  Wählen Sie unter **Redundancy** (Redundanz) die Option **External** (Extern).
  3.  Wählen Sie unter **Allocation Unit Size** (Größe der Zuordnungseinheit) die Größe **4**.
  4.  Wählen Sie unter **Add Disks** (Datenträger hinzufügen) die Option **ORCLASMSP**.

  ![Screenshot der Seite „Create ASM Disk Group“ (ASM-Datenträgergruppe erstellen) des Installationsprogramms](./media/asm-configuration/install03.png)

5. Wählen Sie auf der Seite **Specify ASM Password** (ASM-Kennwort angeben) die Option **Use same passwords for these accounts** (Gleiche Kennwörter für diese Konten verwenden), und geben Sie ein Kennwort ein.

  ![Screenshot der Seite „Specify ASM Password“ (ASM-Kennwort angeben) des Installationsprogramms](./media/asm-configuration/install04.png)

6. (Optional) Aktivieren Sie auf der Seite **Specify Management Options** (Verwaltungsoptionen angeben) das Kästchen **Register with Enterprise Manager (EM) Cloud Control** (Mit Enterprise Manager (EM) Cloud Control registrieren).

  ![Screenshot der Seite „Specify Management Options“ (Verwaltungsoptionen angeben) des Installationsprogramms](./media/asm-configuration/install05.png)

7. Verwenden Sie auf der Seite **Privileged Operating System Groups** (Privilegierte Betriebssystemgruppen) die Standardeinstellungen.

  ![Screenshot der Seite „Privileged Operating System Groups“ (Privilegierte Betriebssystemgruppen) des Installationsprogramms](./media/asm-configuration/install06.png)

8. Verwenden Sie auf der Seite **Specify Installation Location** (Installationsort angeben) die Standardeinstellungen.

  ![Screenshot der Seite „Specify Installation Location“ (Installationsort angeben) des Installationsprogramms](./media/asm-configuration/install07.png)

9. Geben Sie auf der Seite **Create Inventory** (Bestand erstellen) den Speicherort des Ordners an, oder navigieren Sie zu diesem.

  ![Screenshot der Seite „Create Inventory“ (Bestand erstellen) des Installationsprogramms](./media/asm-configuration/install08.png)

10. Aktivieren Sie auf der Seite **Root script execution configuration** (Konfiguration der root-Skriptausführung) das Kästchen **Automatically run configuration scripts** (Konfigurationsskripts automatisch ausführen). Wählen Sie dann die Option **Use "root" user credential** (Anmeldeinformationen des root-Benutzers verwenden), und geben Sie das Kennwort des root-Benutzers ein.

  ![Screenshot der Seite „Root script execution configuration“ (Konfiguration der root-Skriptausführung) des Installationsprogramms](./media/asm-configuration/install09.png)

11. Wählen Sie auf der Seite **Perform Prerequisite Checks** (Voraussetzungsüberprüfungen ausführen) die Option **Fix & Check Again** (Beheben und erneut überprüfen).

  ![Screenshot der Seite „Perform Prerequisite Checks“ (Voraussetzungsüberprüfungen ausführen) des Installationsprogramms](./media/asm-configuration/install10.png)

12. Wählen Sie auf der Seite **Fixup Script** (Fehlerbehebungsskript) die Schaltfläche **OK**.

  ![Screenshot der Seite „Fixup Script“ (Fehlerbehebungsskript) des Installationsprogramms](./media/asm-configuration/install11.png)

13. Überprüfen Sie auf der Seite **Summary** (Zusammenfassung) Ihre Einstellungen, und wählen Sie dann **Install** (Installieren).

  ![Screenshot der Seite „Summary“ (Zusammenfassung) des Installationsprogramms](./media/asm-configuration/install12.png)

14. Ein Dialogfeld mit einer Warnung wird angezeigt. Wählen Sie **Yes** (Ja), um fortzufahren.

  ![Screenshot des Dialogfelds mit der Warnung](./media/asm-configuration/install14.png)

15. Wählen Sie auf der Seite **Finish** (Fertig stellen) die Schaltfläche **Close** (Schließen), um die Installation abzuschließen.

  ![Screenshot der Seite „Finish“ (Fertig stellen) des Installationsprogramms](./media/asm-configuration/install16.png)

## <a name="set-up-your-oracle-asm-installation"></a>Einrichten der Oracle ASM-Installation

Führen Sie zum Einrichten der Oracle ASM-Installation die folgenden Schritte aus:

1.  Melden Sie sich in Ihrer X11-Sitzung als grid-Benutzer an:

  ```bash
  $ cd /u01/app/grid/product/12.1.0/grid/bin
  $ ./asmca
  ```

  Oracle ASM Configuration Assistant wird geöffnet.

2.  Wählen Sie auf der Seite **Configure ASM: Disk Groups** (ASM konfigurieren: Datenträgergruppen) die Schaltfläche **Create** (Erstellen), und wählen Sie dann **Show Advanced Options** (Erweiterte Optionen anzeigen).

  ![Screenshot der Seite „Configure ASM: Disk Groups“ (ASM konfigurieren: Datenträgergruppen)](./media/asm-configuration/asm01.png)

3.  Auf der Seite **Create Disk Group** (Datenträgergruppe erstellen):

  1.  Geben Sie den Namen der Datenträgergruppe ein.
  2.  Wählen Sie unter **Select Member Disks** (Mitgliedsdatenträger auswählen) die Optionen **ORCL_DATA** und **ORCL_DATA1**.
  3.  Wählen Sie unter **Allocation Unit Size** (Größe der Zuordnungseinheit) die Größe **4**. 

  ![Screenshot der Seite „Create Disk Group“ (Datenträgergruppe erstellen)](./media/asm-configuration/asm02.png)

4.  Wählen Sie auf der Seite **Configure ASM: Disk Groups** (ASM konfigurieren: Datenträgergruppen) die Schaltfläche **Create** (Erstellen), und wählen Sie dann **Show Advanced Options** (Erweiterte Optionen anzeigen).

  ![Screenshot der Seite „Configure ASM: Disk Groups“ (ASM konfigurieren: Datenträgergruppen)](./media/asm-configuration/asm03.png)

5.  Auf der Seite **Create Disk Group** (Datenträgergruppe erstellen):

  1.  Geben Sie den Namen der Datenträgergruppe ein.
  2.  Wählen Sie unter **Redundancy** (Redundanz) die Option **External** (Extern).
  3.  Wählen Sie unter **Select Member Disks** (Mitgliedsdatenträger auswählen) die Option **FRA**.
  4.  Wählen Sie unter **Allocation Unit Size** (Größe der Zuordnungseinheit) die Größe **4**.

  ![Screenshot der Seite „Create Disk Group“ (Datenträgergruppe erstellen)](./media/asm-configuration/asm04.png)

6.  Wählen Sie **Exit** (Beenden), um ASM Configuration Assistant zu schließen.

  ![Screenshot der Seite „Configure ASM: Disk Groups“ (ASM konfigurieren: Datenträgergruppen) mit der Schaltfläche „Exit“ (Beenden)](./media/asm-configuration/asm05.png)

## <a name="create-the-database"></a>Erstellen der Datenbank

Die Oracle-Software ist bereits im Azure Marketplace-Image installiert. Führen Sie zum Installieren der Datenbank die folgenden Schritte aus:

1.  Wechseln Sie zum Oracle-Superuser, und initialisieren Sie den Listener für die Protokollierung:

  ```bash
  $ su - oracle
  Password:
  $ cd /u01/app/oracle/product/12.1.0/dbhome_1/bin
  $ ./dbca
   ```
   Database Configuration Assistant wird geöffnet.

2.  Wählen Sie auf der Seite **Database Operation** (Datenbankvorgang) die Option **Create Database** (Datenbank erstellen).

  ![Screenshot der Seite „Database Operation“ (Datenbankvorgang)](./media/asm-configuration/createdb01.png)
  
3. Auf der Seite **Creation Mode** (Erstellungsmodus):

    1.  Geben Sie einen Namen für die Datenbank ein. 
    2.  Wählen Sie für **Storage Type** (Speichertyp) die Option **Automatic Storage Management (ASM)** (Automatische Speicherverwaltung).
    3.  Navigieren Sie unter **Database Files Location** (Speicherort der Datenbankdateien) zum gewünschten Ordner.
    4.  Navigieren Sie unter **Fast Recovery Area** (Bereich für die schnelle Wiederherstellung) zum gewünschten Ordner.

  ![Screenshot der Seite „Creation Mode“ (Erstellungsmodus)](./media/asm-configuration/createdb02.png)

4.  Überprüfen Sie auf der Seite **Summary** (Zusammenfassung) Ihre Einstellungen, und wählen Sie dann **Finish** (Fertig stellen), um die Datenbank zu erstellen.

  ![Screenshot der Seite „Summary“ (Zusammenfassung)](./media/asm-configuration/createdb03.png)

5.   (Optional) Wählen Sie auf der Seite **Finish** (Fertig stellen) zum Ändern des Kennworts **Password Management** (Kennwortverwaltung).

  ![Screenshot der Seite „Finish“ (Fertig stellen)](./media/asm-configuration/createdb04.png)


## <a name="delete-the-vm"></a>Löschen der virtuellen Computer

Wenn Sie den virtuellen Computer nicht mehr benötigen, können Sie mit dem folgenden Befehl die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen entfernen:

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

[Tutorial: Erstellen hoch verfügbarer virtueller Computer0](../../linux/create-cli-complete.md)

[Erkunden der Azure CLI-Beispiele für die Bereitstellung virtueller Computer](../../linux/cli-samples.md)

