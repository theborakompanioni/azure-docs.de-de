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
ms.sourcegitcommit: 7948c99b7b60d77a927743c7869d74147634ddbf
ms.openlocfilehash: 39f6acd0def9fa5c2de470268cda6666aa572e5d
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017

---

# <a name="set-up-oracle-asm-on-an-azure-linux-virtual-machine"></a>Einrichten von Oracle ASM auf einem virtuellen Azure Linux-Computer  

In diesem Artikel erfahren Sie, wie Sie Oracle Automatic Storage Management (Oracle ASM) auf einem virtuellen Oracle Linux-Computer in Azure installieren und einrichten.

Stellen Sie sicher, dass die Azure CLI installiert ist, bevor Sie beginnen. Weitere Informationen finden Sie im [Azure CLI-Installationshandbuch](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="prepare-the-environment"></a>Vorbereiten der Umgebung

### <a name="sign-in-to-azure"></a>Anmelden bei Azure 

Verwenden Sie in der Azure CLI zum Anmelden bei Ihrem Azure-Abonnement den Befehl [az login](/cli/azure/#login). Befolgen Sie dann die Anweisungen auf dem Bildschirm.

```azurecli
az login
```

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Verwenden Sie zum Erstellen einer Ressourcengruppe den Befehl [az group create](/cli/azure/group#create). Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. 

Das folgende Beispiel erstellt am Standort „westus“ eine Ressourcengruppe mit dem Namen „myResourceGroup“.

```azurecli
az group create --name myResourceGroup --location westus
```

### <a name="create-a-vm"></a>Erstellen einer VM

Führen Sie zum Erstellen eines virtuellen Computers mit Oracle ASM die folgenden Schritte aus:

#### <a name="1--to-create-a-virtual-machine-use-the-az-vm-createcliazurevmcreate-command"></a>1.  Verwenden Sie zum Erstellen eines virtuellen Computers den Befehl [az vm create](/cli/azure/vm#create). 

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

#### <a name="2--add-disks-to-use-for-your-oracle-asm-configuration"></a>2.  Fügen Sie Datenträger hinzu, die für die Oracle ASM-Konfiguration verwendet werden:

  ```azurecli
  az vm disk attach -g myResourceGroup --vm-name myVM --disk myDataDisk --new --size-gb 50
  az vm disk attach -g myResourceGroup --vm-name myVM --disk myDataDisk2 --new --size-gb 50
  az vm disk attach -g myResourceGroup --vm-name myVM --disk myDataDisk3 --new --size-gb 50
  az vm disk attach -g myResourceGroup --vm-name myVM --disk myDataDisk4 --new --size-gb 50
  ```

### <a name="connect-to-the-vm"></a>Herstellen der Verbindung zur VM

Erstellen Sie mit dem folgenden Befehl eine SSH-Sitzung mit dem virtuellen Computer. Ersetzen Sie die IP-Adresse durch den Wert für `publicIpAddress` Ihres virtuellen Computers.

```bash 
ssh <publicIpAddress>
```

## <a name="install-oracle-asm"></a>Installieren von Oracle ASM

Führen Sie zum Installieren von Oracle ASM die folgenden Schritte aus. 

Weitere Informationen zum Installieren von Oracle ASM finden Sie unter [Oracle ASMLib Downloads für Oracle Linux 6](http://www.oracle.com/technetwork/server-storage/linux/asmlib/ol6-1709075.html) (Oracle ASMLib-Downloads für Oracle Linux 6).  

### <a name="1-run-yum-list"></a>1. Führen Sie `yum list` aus:

  ```bash
  $ sudo su -
  # yum list
  ```
  Bei der ersten Ausführung kann es einige Minuten dauern, bis `yum list` geladen wird.

### <a name="2--run-these-additional-commands"></a>2.  Führen Sie diese zusätzlichen Befehle aus:

  ```bash
  # yum list | grep oracleasm
  # yum -y install kmod-oracleasm.x86_64
  # yum -y install oracleasm-support.x86_64
  # wget http://download.oracle.com/otn_software/asmlib/oracleasmlib-2.0.12-1.el6.x86_64.rpm
  # yum -y install oracleasmlib-2.0.12-1.el6.x86_64.rpm
  # rm -f oracleasmlib-2.0.12-1.el6.x86_64.rpm
  ```

### <a name="3--verify-that-oracle-asm-is-installed"></a>3.  Stellen Sie sicher, dass Oracle ASM installiert ist:

  ```bash
  # rpm -qa |grep oracleasm
  oracleasm-support-2.1.10-4.el6.x86_64
  kmod-oracleasm-2.0.8-15.el6_9.x86_64
  oracleasmlib-2.0.12-1.el6.x86_64
  ```

### <a name="4--add-users-and-groups"></a>4.  Fügen Sie Benutzer und Gruppen hinzu:

  ```bash
  # groupadd -g 54345 asmadmin
  # groupadd -g 54346 asmdba
  # groupadd -g 54347 asmoper
  # useradd -u 3000 -g oinstall -G dba,asmadmin,asmdba,asmoper grid
  # usermod -g oinstall -G dba,asmdba,asmadmin oracle
  ```

### <a name="5--verify-users-and-groups"></a>5.  Überprüfen Sie Benutzer und Gruppen:

  ```bash
  # id grid
  uid=3000(grid) gid=54321(oinstall) groups=54321(oinstall),54322(dba),54345(asmadmin),54346(asmdba),54347(asmoper)
  ```

### <a name="6--create-a-folder-and-change-owner"></a>6.  Erstellen Sie einen Ordner, und ändern Sie den Besitzer:

  ```bash
  # mkdir /u01/app/grid
  # chown grid:oinstall /u01/app/grid
  ```

## <a name="set-up-oracle-asm"></a>Einrichten von Oracle ASM

In diesem Tutorial wird als Standardbenutzer *grid* und als Standardgruppe *asmadmin* verwendet. Stellen Sie sicher, dass der *oracle*-Benutzer Mitglied der Gruppe „asmadmin“ ist. Führen Sie zum Einrichten der Oracle ASM-Installation die folgenden Schritte aus:

### <a name="1--set-the-oracle-asm-library-driver"></a>1.  Legen Sie den Treiber für die Oracle ASM-Bibliothek fest:

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

### <a name="2--view-the-disk-configuration"></a>2.  Zeigen Sie die Datenträgerkonfiguration an:
  ```bash
  # cat /proc/partitions
  ```

### <a name="3--format-the-disk"></a>3.  Formatieren Sie den Datenträger:

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

### <a name="4--repeat-the-preceding-step-for-devsdd-devsde-and-devsdf"></a>4.  Wiederholen Sie den vorherigen Schritt für „/dev/sdd“, „/dev/sde“ und „/dev/sdf“.

### <a name="5--check-the-disk-configuration"></a>5.  Überprüfen Sie die Datenträgerkonfiguration:

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

### <a name="6--check-the-oracle-asm-service-status"></a>6.  Überprüfen Sie den Oracle ASM-Dienststatus:

```bash
# service oracleasm status
Checking if ASM is loaded: no
Checking if /dev/oracleasm is mounted: no
```

### <a name="7--start-the-oracle-asm-service"></a>7.  Starten Sie den Oracle ASM-Dienst:

```bash
# service oracleasm start
Initializing the Oracle ASMLib driver:                     [  OK  ]
Scanning the system for Oracle ASMLib disks:               [  OK  ]
```

### <a name="8--create-oracle-asm-disks"></a>8.  Erstellen Sie die Oracle ASM-Datenträger:

```bash
# service oracleasm createdisk ASMSP /dev/sdc1
Marking disk "ASMSP" as an ASM disk:                       [  OK  ]

# service oracleasm createdisk DATA /dev/sdd1
Marking disk "DATA" as an ASM disk:                        [  OK  ]

# service oracleasm createdisk DATA1 /dev/sde1
Marking disk "DATA1" as an ASM disk:                       [  OK  ]

# service oracleasm createdisk FRA /dev/sdf1
Marking disk "FRA" as an ASM disk:                         [  OK  ]
```

### <a name="9--list-oracle-asm-disks"></a>9.  Listen Sie die Oracle ASM-Datenträger auf:

```bash
# service oracleasm listdisks
ASMSP
DATA
DATA1
FRA
```

### <a name="10-change-the-passwords-for-the-root-oracle-and-grid-users-you-use-the-passwords-later-during-installation"></a>10. Ändern Sie die Kennwörter für die root-, oracle und grid-Benutzer (Sie verwenden die Kennwörter später während der Installation):

```bash
# passwd oracle
# passwd grid
# passwd root
```

### <a name="11-change-the-folder-permission"></a>11. Ändern Sie die Ordnerberechtigung:

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

### <a name="1--download-oracle-grid-infrastructure-from-the-oracle-asm-download-pagehttpwwworaclecomtechnetworkdatabaseenterprise-editiondownloadsdatabase12c-linux-download-2240591html"></a>1.  Laden Sie Oracle Grid Infrastructure von der [Oracle ASM-Downloadseite](http://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-linux-download-2240591.html) herunter. 

  Unter dem Downloadtitel **Oracle Database 12c Release 1 Grid Infrastructure (12.1.0.2.0) for Linux x86-64** sollten zwei ZIP-Dateien zum Download zur Verfügung stehen.

### <a name="2--after-you-download-the-zip-files-to-your-client-computer-you-can-use-secure-copy-protocol-scp-to-copy-the-files-to-your-vm"></a>2.  Nachdem Sie die ZIP-Dateien auf den Clientcomputer heruntergeladen haben, können Sie mit Secure Copy Protocol (SCP) die Dateien auf den virtuellen Computer kopieren.

```bash
scp *.zip <publicIpAddress>:<folder>
```

### <a name="3--move-the-zip-files-to-the-opt-folder-then-change-the-owner-of-the-files"></a>3.  Verschieben Sie die ZIP-Dateien in den Ordner „/opt“. Ändern Sie dann den Besitzer der Dateien:

```bash
# mv <folder>/*.zip /opt
# cd /opt
# chown grid:oinstall linuxamd64_12102_grid_1of2.zip
# chown grid:oinstall linuxamd64_12102_grid_2of2.zip
```
### <a name="4--unzip-the-files-install-the-linux-unzip-utility-if-its-not-already-installed"></a>4.  Entzippen Sie die Dateien (installieren Sie das Linux-Hilfsprogramm zum Entpacken, sofern es noch nicht installiert ist):
```bash
# yum install unzip
# unzip linuxamd64_12102_grid_1of2.zip
# unzip linuxamd64_12102_grid_2of2.zip
```
### <a name="5--change-permission"></a>5.  Ändern Sie die Berechtigung:
```bash
# chown -R grid:oinstall /opt/grid
```
### <a name="6--turn-off-the-firewall"></a>6.  Deaktivieren Sie die Firewall:
```bash
# service iptables status
# service iptables stop
```

### <a name="7--check-available-swap-space-you-need-at-lease-68-gb-of-swap-space-to-install-grid-by-default-linux-azure-vms-will-not-have-swap-enabled-and-configured-by-default"></a>7.  Überprüfen Sie den verfügbaren Auslagerungsbereich. Um Grid zu installieren, muss Ihr Auslagerungsbereich mindestens 6,8 GB groß sein. Standardmäßig ist die Auslagerung für virtuelle Azure Linux-Computer weder aktiviert noch konfiguriert.

Es wird dringend empfohlen, mit dem Waagent einen Auslagerungsbereich zu konfigurieren, damit dieser auf dem kurzlebigen Datenträger (temporären Datenträger) immer erstellt wird. Weitere Informationen zu diesen Schritten finden Sie unter folgendem Link: 

* [How to add a swap file in Linux Azure virtual machines (Hinzufügen einer Auslagerungsdatei auf virtuellen Linux-Azure-Computern)](https://support.microsoft.com/en-us/help/4010058/how-to-add-a-swap-file-in-linux-azure-virtual-machines)

## <a name="prepare-the-client-and-vm-to-run-x11-for-windows-clients-only"></a>Vorbereiten des Clients und des virtuellen Computers für die Ausführung von x11 (nur für Windows-Clients)
Dies ist ein optionaler Schritt, den Sie überspringen können, wenn Sie einen Linux-Client verwenden oder x11 bereits verwenden.

### <a name="1--download-putty-and-xming-to-your-windows-computer"></a>1.  Laden Sie PuTTY und Xming auf Ihren Windows-Computer herunter:

  * Laden Sie [PuTTY](http://www.putty.org/) herunter.
  * Laden Sie [Xming](https://xming.en.softonic.com/) herunter.

### <a name="2--after-you-install-putty-in-the-putty-folder-for-example-cprogram-filesputty-run-puttygenexe-putty-key-generator"></a>2.  Führen Sie nach der Installation von PuTTY im PuTTY-Order (z.B. „C:\Programme\PuTTY“) „puttygen.exe“ (PuTTY-Schlüsselgenerator) aus.

### <a name="3--in-putty-key-generator"></a>3.  Im PuTTY-Schlüsselgenerator:

- Um einen Schlüssel zu generieren, wählen Sie die Schaltfläche **Generate** (Generieren).
- Kopieren Sie den Inhalt des Schlüssels (STRG+C).
- Wählen Sie die Schaltfläche **Save private key** (Privaten Schlüssel speichern).
- Ignorieren Sie die angezeigte Warnung, und wählen Sie dann **OK**.

  ![Screenshot der PuTTY-Schlüsselgeneratorseite](./media/oracle-asm/puttykeygen.png)

### <a name="4--in-your-vm-run-these-commands"></a>4.  Führen Sie auf dem virtuellen Computer die folgenden Befehle aus:

```bash
# sudo su - grid
$ mkdir .ssh (if not already created)
$ cd .ssh
```

### <a name="5--create-a-file-named-authorizedkeys-paste-the-contents-of-the-key-in-this-file-and-then-save-the-file"></a>5.  Erstellen Sie eine Datei namens „authorized_keys“. Fügen Sie den Inhalt des Schlüssels in dieser Datei ein, und speichern Sie die Datei.

> [!NOTE]
> Der Schlüssel muss die Zeichenfolge `ssh-rsa` enthalten. Zudem muss der Inhalt des Schlüssels als einzelne Textzeile eingefügt werden.
>  

### <a name="6--start-putty-in-the-category-pane-go-to-connection--ssh--auth-in-the-private-key-file-for-authentication-box-browse-to-the-key-that-you-generated-earlier"></a>6.  Starten Sie PuTTY. Navigieren Sie im Bereich **Category** (Kategorie) zu **Connection** > **SSH** > **Auth** (Verbindung > SSH > Authentifizierung). Navigieren Sie im Feld **Private key file for authentication** (Datei mit privatem Schlüssel für die Authentifizierung) zu dem zuvor erstellten Schlüssel.

  ![Screenshot der Seite zum Festlegen des privaten Schlüssels](./media/oracle-asm/setprivatekey.png)

### <a name="7--in-the-category-pane-go-to-connection--ssh--x11-select-the-enable-x11-forwarding-box"></a>7.  Navigieren Sie im Bereich **Category** (Kategorie) zu **Connection** > **SSH** > **X11** (Verbindung > SSH > X11). Aktivieren Sie das Kästchen **Enable X11 forwarding** (X11-Weiterleitung aktivieren).

  ![Screenshot der Seite für die X11-Aktivierung](./media/oracle-asm/enablex11.png)

### <a name="8-in-the-category-pane-go-to-session-enter-the-host-information-and-then-select-open"></a>8. Navigieren Sie im Bereich **Category** (Kategorie) zu **Session** (Sitzung). Geben Sie die Hostinformationen ein, und wählen Sie dann **Open** (Öffnen).

  ![Screenshot der Seite mit der Sitzung](./media/oracle-asm/puttysession.png)

## <a name="install-oracle-grid-infrastructure"></a>Installieren von Oracle Grid Infrastructure

Führen Sie zum Installieren von Oracle Grid Infrastructure die folgenden Schritte aus:

### <a name="1-sign-in-as-grid-you-should-be-able-to-sign-in-without-being-prompted-for-a-password"></a>1. Melden Sie sich als grid-Benutzer an. (Sie sollten sich ohne Aufforderung zur Kennworteingabe anmelden können.) 

> [!NOTE]
> Stellen Sie sicher, dass Xming ausgeführt wird, bevor Sie die Installation starten.

```bash
$ cd /opt/grid
$ ./runInstaller
```

  Das Installationsprogramm für Oracle Grid Infrastructure 12c Release 1 wird geöffnet. (Es kann einen Moment, bis das Installationsprogramm gestartet wird.)

### <a name="2-on-the-select-installation-option-page-select-install-and-configure-oracle-grid-infrastructure-for-a-standalone-server"></a>2. Wählen Sie auf der Seite **Select Installation Option** (Installationsoption auswählen) die Option **Install and Configure Oracle Grid Infrastructure for a Standalone Server** (Oracle Grid Infrastructure für einen eigenständigen Server installieren und konfigurieren).

  ![Screenshot der Seite „Select Installation Option“ (Installationsoption auswählen) des Installationsprogramms](./media/oracle-asm/install01.png)

### <a name="3-on-the-select-product-languages-page-select-english-or-the-language-that-you-want"></a>3. Wählen Sie auf der Seite **Select Product Languages** (Produktsprache auswählen) die Sprache **English** (Englisch) oder die gewünschte Sprache.

  ![Screenshot der Seite „Select Product Languages“ (Produktsprache auswählen) des Installationsprogramms](./media/oracle-asm/install02.png)

### <a name="4-on-the-create-asm-disk-group-page"></a>4. Auf der Seite **Create ASM Disk Group** (ASM-Datenträgergruppe erstellen):
- Geben Sie einen Namen für die Datenträgergruppe ein.
- Wählen Sie unter **Redundancy** (Redundanz) die Option **External** (Extern).
- Wählen Sie unter **Allocation Unit Size** (Größe der Zuordnungseinheit) die Größe **4**.
- Wählen Sie unter **Add Disks** (Datenträger hinzufügen) die Option **ORCLASMSP**.

  ![Screenshot der Seite „Create ASM Disk Group“ (ASM-Datenträgergruppe erstellen) des Installationsprogramms](./media/oracle-asm/install03.png)

### <a name="5-on-the-specify-asm-password-page-select-the-use-same-passwords-for-these-accounts-option-and-enter-a-password"></a>5. Wählen Sie auf der Seite **Specify ASM Password** (ASM-Kennwort angeben) die Option **Use same passwords for these accounts** (Gleiche Kennwörter für diese Konten verwenden), und geben Sie ein Kennwort ein.

  ![Screenshot der Seite „Specify ASM Password“ (ASM-Kennwort angeben) des Installationsprogramms](./media/oracle-asm/install04.png)

### <a name="6-optional-on-the-specify-management-options-page-select-the-register-with-enterprise-manager-em-cloud-control-box"></a>6. (Optional) Aktivieren Sie auf der Seite **Specify Management Options** (Verwaltungsoptionen angeben) das Kästchen **Register with Enterprise Manager (EM) Cloud Control** (Bei Enterprise Manager (EM) Cloud Control registrieren).

  ![Screenshot der Seite „Specify Management Options“ (Verwaltungsoptionen angeben) des Installationsprogramms](./media/oracle-asm/install05.png)

### <a name="7-on-the-privileged-operating-system-groups-page-use-the-default-settings"></a>7. Verwenden Sie auf der Seite **Privileged Operating System Groups** (Privilegierte Betriebssystemgruppen) die Standardeinstellungen.

  ![Screenshot der Seite „Privileged Operating System Groups“ (Privilegierte Betriebssystemgruppen) des Installationsprogramms](./media/oracle-asm/install06.png)

### <a name="8-on-the-specify-installation-location-page-use-default-settings"></a>8. Verwenden Sie auf der Seite **Specify Installation Location** (Installationsort angeben) die Standardeinstellungen.

  ![Screenshot der Seite „Specify Installation Location“ (Installationsort angeben) des Installationsprogramms](./media/oracle-asm/install07.png)

### <a name="9-on-the-create-inventory-page-enter-or-browse-to-the-folder-location"></a>9. Geben Sie auf der Seite **Create Inventory** (Bestand erstellen) den Speicherort des Ordners an, oder navigieren Sie zu diesem.

  ![Screenshot der Seite „Create Inventory“ (Bestand erstellen) des Installationsprogramms](./media/oracle-asm/install08.png)

### <a name="10-on-the-root-script-execution-configuration-page-select-the-automatically-run-configuration-scripts-box-then-select-the-use-root-user-credential-option-and-enter-the-root-user-password"></a>10. Aktivieren Sie auf der Seite **Root script execution configuration** (Konfiguration der root-Skriptausführung) das Kästchen **Automatically run configuration scripts** (Konfigurationsskripts automatisch ausführen). Wählen Sie dann die Option **Use "root" user credential** (Anmeldeinformationen des root-Benutzers verwenden), und geben Sie das Kennwort des root-Benutzers ein.

  ![Screenshot der Seite „Root script execution configuration“ (Konfiguration der root-Skriptausführung) des Installationsprogramms](./media/oracle-asm/install09.png)

### <a name="11-on-the-perform-prerequisite-checks-page-select-fix--check-again"></a>11. Wählen Sie auf der Seite **Perform Prerequisite Checks** (Überprüfungen auf erforderliche Komponenten ausführen) die Option **Fix & Check Again** (Beheben und erneut überprüfen).

  ![Screenshot der Seite „Perform Prerequisite Checks“ (Überprüfungen auf erforderliche Komponenten ausführen) des Installationsprogramms](./media/oracle-asm/install10.png)

### <a name="12-on-the-fixup-script-page-select-ok"></a>12. Wählen Sie auf der Seite **Fixup Script** (Fehlerbehebungsskript) die Schaltfläche **OK**.

  ![Screenshot der Seite „Fixup Script“ (Fehlerbehebungsskript) des Installationsprogramms](./media/oracle-asm/install11.png)

### <a name="13-on-the-summary-page-review-your-settings-selections-and-then-select-install"></a>13. Überprüfen Sie auf der Seite **Summary** (Zusammenfassung) Ihre Einstellungen, und wählen Sie dann **Install** (Installieren).

  ![Screenshot der Seite „Summary“ (Zusammenfassung) des Installationsprogramms](./media/oracle-asm/install12.png)

### <a name="14-a-warning-dialog-box-appears-select-yes-to-continue"></a>14. Ein Dialogfeld mit einer Warnung wird angezeigt. Wählen Sie **Yes** (Ja), um fortzufahren.

  ![Screenshot des Dialogfelds mit der Warnung](./media/oracle-asm/install14.png)

### <a name="15-on-the-finish-page-select-close-to-finish-the-installation"></a>15. Wählen Sie auf der Seite **Finish** (Fertig stellen) die Schaltfläche **Close** (Schließen), um die Installation abzuschließen.

  ![Screenshot der Seite „Finish“ (Fertig stellen) des Installationsprogramms](./media/oracle-asm/install16.png)

## <a name="set-up-your-oracle-asm-installation"></a>Einrichten der Oracle ASM-Installation

Führen Sie zum Einrichten der Oracle ASM-Installation die folgenden Schritte aus:

### <a name="1--sign-in-as-grid-from-your-x11-session"></a>1.  Melden Sie sich in Ihrer X11-Sitzung als grid-Benutzer an:

  ```bash
  $ cd /u01/app/grid/product/12.1.0/grid/bin
  $ ./asmca
  ```

  Oracle ASM Configuration Assistant wird geöffnet.

### <a name="2--on-the-configure-asm-disk-groups-page-select-the-create-button-and-then-select-show-advanced-options"></a>2.  Wählen Sie auf der Seite **Configure ASM: Disk Groups** (ASM konfigurieren: Datenträgergruppen) die Schaltfläche **Create** (Erstellen), und wählen Sie dann **Show Advanced Options** (Erweiterte Optionen anzeigen).

  ![Screenshot der Seite „Configure ASM: Disk Groups“ (ASM konfigurieren: Datenträgergruppen)](./media/oracle-asm/asm01.png)

### <a name="3--on-the-create-disk-group-page"></a>3.  Auf der Seite **Create Disk Group** (Datenträgergruppe erstellen):

- Geben Sie den Namen der Datenträgergruppe ein.
- Wählen Sie unter **Select Member Disks** (Mitgliedsdatenträger auswählen) die Optionen **ORCL_DATA** und **ORCL_DATA1**.
- Wählen Sie unter **Allocation Unit Size** (Größe der Zuordnungseinheit) die Größe **4**.

  ![Screenshot der Seite „Create Disk Group“ (Datenträgergruppe erstellen)](./media/oracle-asm/asm02.png)

### <a name="4--on-the-configure-asm-disk-groups-page-select-the-create-button-and-then-select-show-advanced-options"></a>4.  Wählen Sie auf der Seite **Configure ASM: Disk Groups** (ASM konfigurieren: Datenträgergruppen) die Schaltfläche **Create** (Erstellen), und wählen Sie dann **Show Advanced Options** (Erweiterte Optionen anzeigen).

  ![Screenshot der Seite „Configure ASM: Disk Groups“ (ASM konfigurieren: Datenträgergruppen)](./media/oracle-asm/asm03.png)

### <a name="5--on-the-create-disk-group-page"></a>5.  Auf der Seite **Create Disk Group** (Datenträgergruppe erstellen):

- Geben Sie den Namen der Datenträgergruppe ein.
- Wählen Sie unter **Redundancy** (Redundanz) die Option **External** (Extern).
- Wählen Sie unter **Select Member Disks** (Mitgliedsdatenträger auswählen) die Option **FRA**.
- Wählen Sie unter **Allocation Unit Size** (Größe der Zuordnungseinheit) die Größe **4**.

  ![Screenshot der Seite „Create Disk Group“ (Datenträgergruppe erstellen)](./media/oracle-asm/asm04.png)

### <a name="6--select-exit-to-close-asm-configuration-assistant"></a>6.  Wählen Sie **Exit** (Beenden), um ASM Configuration Assistant zu schließen.

  ![Screenshot der Seite „Configure ASM: Disk Groups“ (ASM konfigurieren: Datenträgergruppen) mit der Schaltfläche „Exit“ (Beenden)](./media/oracle-asm/asm05.png)

## <a name="create-the-database"></a>Erstellen der Datenbank

Die Oracle-Software ist bereits im Azure Marketplace-Image installiert. Führen Sie zum Installieren der Datenbank die folgenden Schritte aus:

### <a name="1--switch-users-to-the-oracle-superuser-and-then-initialize-the-listener-for-logging"></a>1.  Wechseln Sie zum Oracle-Superuser, und initialisieren Sie den Listener für die Protokollierung:

  ```bash
  $ su - oracle
  Password:
  $ cd /u01/app/oracle/product/12.1.0/dbhome_1/bin
  $ ./dbca
   ```
   Database Configuration Assistant wird geöffnet.

### <a name="2--on-the-database-operation-page-select-create-database"></a>2.  Wählen Sie auf der Seite **Database Operation** (Datenbankvorgang) die Option **Create Database** (Datenbank erstellen).

  ![Screenshot der Seite „Database Operation“ (Datenbankvorgang)](./media/oracle-asm/createdb01.png)
  
### <a name="3-on-the-creation-mode-page"></a>3. Auf der Seite **Creation Mode** (Erstellungsmodus):

- Geben Sie einen Namen für die Datenbank ein.
- Wählen Sie für **Storage Type** (Speichertyp) die Option **Automatic Storage Management (ASM)** (Automatische Speicherverwaltung).
- Navigieren Sie unter **Database Files Location** (Speicherort der Datenbankdateien) zum gewünschten Ordner.
- Navigieren Sie unter **Fast Recovery Area** (Bereich für die schnelle Wiederherstellung) zum gewünschten Ordner.

  ![Screenshot der Seite „Creation Mode“ (Erstellungsmodus)](./media/oracle-asm/createdb02.png)

### <a name="4--on-the-summary-page-review-your-settings-selections-and-then-select-finish-to-create-the-database"></a>4.  Überprüfen Sie auf der Seite **Summary** (Zusammenfassung) Ihre Einstellungen, und wählen Sie dann **Finish** (Fertig stellen), um die Datenbank zu erstellen.

  ![Screenshot der Seite „Summary“ (Zusammenfassung)](./media/oracle-asm/createdb03.png)

### <a name="5---optional-on-the-finish-page-to-change-the-passwords-select-password-management"></a>5.   (Optional) Wählen Sie auf der Seite **Finish** (Fertig stellen) zum Ändern des Kennworts **Password Management** (Kennwortverwaltung).

  ![Screenshot der Seite „Finish“ (Fertig stellen)](./media/oracle-asm/createdb04.png)


## <a name="delete-the-vm"></a>Löschen der virtuellen Computer

Wenn Sie den virtuellen Computer nicht mehr benötigen, können Sie mit dem folgenden Befehl die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen entfernen:

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

[Tutorial: Erstellen hoch verfügbarer virtueller Computer0](../../linux/create-cli-complete.md)

[Erkunden der Azure CLI-Beispiele für die Bereitstellung virtueller Computer](../../linux/cli-samples.md)

