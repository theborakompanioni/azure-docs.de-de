---
title: Sichern und Wiederherstellen einer Oracle Database 12c-Datenbank auf einem virtuellen Azure Linux-Computer | Microsoft-Dokumentation
description: "In diesem Artikel wird erläutert, wie Sie eine Oracle Database 12c-Datenbank in der Azure-Umgebung sichern und wiederherstellen."
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
ms.date: 5/17/2017
ms.author: rclaus
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: 9a2293f13b90e9a4cb11b4169fad969dd622a9a6
ms.contentlocale: de-de
ms.lasthandoff: 07/08/2017

---

# <a name="back-up-and-recover-an-oracle-database-12c-database-on-an-azure-linux-virtual-machine"></a>Sichern und Wiederherstellen einer Oracle Database 12c-Datenbank auf einem virtuellen Azure Linux-Computer

Sie können über die Azure-Befehlszeilenschnittstelle (Command-Line Interface, CLI) oder über Skripts Azure-Ressourcen erstellen und verwalten. In diesem Artikel verwenden Sie Skripts in der Azure-Befehlszeilenschnittstelle, um eine Oracle Database 12c-Datenbank aus einem Azure Marketplace-Katalogimage bereitzustellen.

Stellen Sie sicher, dass die Azure CLI installiert ist, bevor Sie beginnen. Weitere Informationen finden Sie im [Azure CLI-Installationshandbuch](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="prepare-the-environment"></a>Vorbereiten der Umgebung

### <a name="step-1-prerequisites"></a>Schritt 1: Voraussetzungen

*   Sie müssen für die Sicherung und Wiederherstellung zuerst einen virtuellen Linux-Computer erstellen, auf dem eine Instanz von Oracle Database 12c installiert ist. Verwenden Sie zum Erstellen des virtuellen Computers das Marketplace-Image *Oracle:Oracle-Database-Ee:12.1.0.2:latest*.

    Wie Sie eine Oracle-Datenbank erstellen, erfahren Sie im [Schnellstart zum Erstellen einer Oracle-Datenbank](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/oracle-database-quick-create).


### <a name="step-2-connect-to-the-vm"></a>Schritt 2: Herstellen der Verbindung zum virtuellen Computer

*   Erstellen Sie mit dem folgenden Befehl eine SSH-Sitzung (Secure Shell) mit dem virtuellen Computer. Ersetzen Sie die Kombination aus IP-Adresse und Hostname durch den `publicIpAddress`-Wert für Ihren virtuellen Computer.

    ```bash 
    ssh <publicIpAddress>
    ```

### <a name="step-3-prepare-the-database"></a>Schritt 3: Vorbereiten der Datenbank

1.  In diesem Schritt wird davon ausgegangen, dass Sie über eine Oracle-Instanz (cdb1) verfügen, die auf dem virtuellen Computer *myVM* ausgeführt wird.

    Führen Sie den *Oracle*-Superuser-Stamm aus, und initialisieren Sie anschließend den Listener:

    ```bash
    $ sudo su - oracle
    $ lsnrctl start
    Copyright (c) 1991, 2014, Oracle.  All rights reserved.

    Starting /u01/app/oracle/product/12.1.0/dbhome_1/bin/tnslsnr: please wait...

    TNSLSNR for Linux: Version 12.1.0.2.0 - Production
    Log messages written to /u01/app/oracle/diag/tnslsnr/myVM/listener/alert/log.xml
    Listening on: (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=myVM.twltkue3xvsujaz1bvlrhfuiwf.dx.internal.cloudapp.net)(PORT=1521)))

    Connecting to (ADDRESS=(PROTOCOL=tcp)(HOST=)(PORT=1521))
    STATUS of the LISTENER
    ------------------------
    Alias                     LISTENER
    Version                   TNSLSNR for Linux: Version 12.1.0.2.0 - Production
    Start Date                23-MAR-2017 15:32:08
    Uptime                    0 days 0 hr. 0 min. 0 sec
    Trace Level               off
    Security                  ON: Local OS Authentication
    SNMP                      OFF
    Listener Log File         /u01/app/oracle/diag/tnslsnr/myVM/listener/alert/log.xml
    Listening Endpoints Summary...
    (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=myVM.twltkue3xvsujaz1bvlrhfuiwf.dx.internal.cloudapp.net)(PORT=1521)))
    The listener supports no services
    The command completed successfully
    ```

2.  (Optional) Stellen Sie sicher, dass sich die Datenbank im Archivierungsprotokoll-Modus befindet:

    ```bash
    $ sqlplus / as sysdba
    SQL> SELECT log_mode FROM v$database;

    LOG_MODE
    ------------
    NOARCHIVELOG

    SQL> SHUTDOWN IMMEDIATE;
    SQL> STARTUP MOUNT;
    SQL> ALTER DATABASE ARCHIVELOG;
    SQL> ALTER DATABASE OPEN;
    SQL> ALTER SYSTEM SWITCH LOGFILE;
    ```
3.  (Optional) Erstellen Sie eine Tabelle zum Testen des Commits:

    ```bash
    SQL> alter session set "_ORACLE_SCRIPT"=true ;
    Session altered.
    SQL> create user scott identified by tiger;
    User created.
    SQL> grant create session to scott;
    Grant succeeded.
    SQL> grant create table to scott;
    Grant succeeded.
    SQL> alter user scott quota 100M on users;
    User altered.
    SQL> connect scott/tiger
    SQL> create table scott_table(col1 number, col2 varchar2(50));
    Table created.
    SQL> insert into scott_Table VALUES(1,'Line 1');
    1 row created.
    SQL> commit;
    Commit complete.
    ```
4.  Überprüfen oder ändern Sie den Speicherort und die Größe der Sicherungsdatei:

    ```bash
    $ sqlplus / as sysdba
    SQL> show parameter db_recovery
    NAME                                 TYPE        VALUE
    ------------------------------------ ----------- ------------------------------
    db_recovery_file_dest                string      /u01/app/oracle/fast_recovery_area
    db_recovery_file_dest_size           big integer 4560M
    ```
5. Verwenden Sie Oracle Recovery Manager (RMAN), um die Datenbank zu sichern:

    ```bash
    $ rman target /
    RMAN> backup database plus archivelog;
    ```

### <a name="step-4-application-consistent-backup-for-linux-vms"></a>Schritt 4: Anwendungskonsistente Sicherung für virtuelle Linux-Computer

Anwendungskonsistente Sicherungen sind eine neue Funktion in Azure Backup. Sie können Skripts erstellen und auswählen, die vor und nach der VM-Momentaufnahme ausgeführt werden.

1. Laden Sie die JSON-Datei herunter.

    Laden Sie „VMSnapshotScriptPluginConfig.json von https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig“ herunter. Die Inhalte der Datei sehen in etwa wie folgt aus:

    ```azurecli
    {
        "pluginName" : "ScriptRunner",
        "preScriptLocation" : "",
        "postScriptLocation" : "",
        "preScriptParams" : ["", ""],
        "postScriptParams" : ["", ""],
        "preScriptNoOfRetries" : 0,
        "postScriptNoOfRetries" : 0,
        "timeoutInSeconds" : 30,
        "continueBackupOnFailure" : true,
        "fsFreezeEnabled" : true
    }
    ```

2. Erstellen Sie den Ordner „/etc/azure“ auf dem virtuellen Computer:

    ```bash
    $ sudo su -
    # mkdir /etc/azure
    # cd /etc/azure
    ```

3. Kopieren Sie die JSON-Datei.

    Kopieren Sie die Datei „VMSnapshotScriptPluginConfig.json“ in den Ordner „/etc/azure“.

4. Bearbeiten Sie die JSON-Datei.

    Bearbeiten Sie die Datei „VMSnapshotScriptPluginConfig.json“ so, dass Sie die Parameter `PreScriptLocation` und `PostScriptlocation` enthält. Beispiel:

    ```azurecli
    {
        "pluginName" : "ScriptRunner",
        "preScriptLocation" : "/etc/azure/pre_script.sh",
        "postScriptLocation" : "/etc/azure/post_script.sh",
        "preScriptParams" : ["", ""],
        "postScriptParams" : ["", ""],
        "preScriptNoOfRetries" : 0,
        "postScriptNoOfRetries" : 0,
        "timeoutInSeconds" : 30,
        "continueBackupOnFailure" : true,
        "fsFreezeEnabled" : true
    }
    ```

5. Erstellen Sie die Skriptdateien vor und nach der Momentaufnahme.

    Hier ist ein Beispiel für Skripts vor und nach der Momentaufnahme für ein Cold Backup (eine Offlinesicherung mit Herunterfahren und Neustart):

    Bei /etc/azure/pre_script.sh:

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "$ORA_HOME/bin/dbshut $ORA_HOME" > /etc/azure/pre_script_$v_date.log
    ```

    Bei /etc/azure/post_script.sh:

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "$ORA_HOME/bin/dbstart $ORA_HOME" > /etc/azure/post_script_$v_date.log
    ```

    Hier ist ein Beispiel für Skripts vor und nach der Momentaufnahme für ein Hot Backup (eine Offlinesicherung):

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "sqlplus / as sysdba @/etc/azure/pre_script.sql" > /etc/azure/pre_script_$v_date.log
    ```

    Bei /etc/azure/post_script.sh:

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "sqlplus / as sysdba @/etc/azure/post_script.sql" > /etc/azure/pre_script_$v_date.log
    ```

    Ändern Sie bei /etc/azure/pre_script.sql die Inhalte der Datei Ihren Anforderungen entsprechend:

    ```bash
    alter tablespace SYSTEM begin backup;
    ...
    ...
    alter system switch logfile;
    alter system archive log stop;
    ```

    Ändern Sie bei /etc/azure/post_script.sql die Inhalte der Datei Ihren Anforderungen entsprechend:

    ```bash
    alter tablespace SYSTEM end backup;
    ...
    ...
    alter system archive log start;
    ```

6. Ändern Sie die Dateiberechtigungen:

    ```bash
    # chmod 600 /etc/azure/VMSnapshotScriptPluginConfig.json
    # chmod 700 /etc/azure/pre_script.sh
    # chmod 700 /etc/azure/post_script.sh
    ```

7. Testen Sie die Skripts.

    Melden Sie sich zum Testen der Skripts zuerst als Stammbenutzer an. Stellen Sie dann sicher, dass keine Fehler auftreten:

    ```bash
    # /etc/azure/pre_script.sh
    # /etc/azure/post_script.sh
    ```

Weitere Informationen finden Sie unter [Application-consistent backup for Linux VMs (Anwendungskonsistente Sicherung für virtuelle Linux-Computer)](https://azure.microsoft.com/en-us/blog/announcing-application-consistent-backup-for-linux-vms-using-azure-backup/).


### <a name="step-5-use-azure-recovery-services-vaults-to-back-up-the-vm"></a>Schritt 5: Verwenden von Azure Recovery Services-Tresoren zum Sichern des virtuellen Computers

1.  Suchen Sie im Azure-Portal nach **Recovery Services-Tresore**.

    ![Seite mit Recovery Services-Tresoren](./media/oracle-backup-recovery/recovery_service_01.png)

2.  Klicken Sie auf dem Blatt **Recovery Services-Tresore** auf **Hinzufügen**, um einen neuen Tresor hinzuzufügen.

    ![Seite zum Hinzufügen von Recovery Services-Tresoren](./media/oracle-backup-recovery/recovery_service_02.png)

3.  Klicken Sie auf **myVault**, um fortzufahren.

    ![Seite mit Details zu Recovery Services-Tresoren](./media/oracle-backup-recovery/recovery_service_03.png)

4.  Klicken Sie auf dem Blatt **myVault** auf **Sicherung**.

    ![Seite zur Sicherung der Recovery Services-Tresore](./media/oracle-backup-recovery/recovery_service_04.png)

5.  Verwenden Sie auf dem Blatt **Sicherungsziel** die Standardwerte für **Azure** und **Virtueller Computer**. Klicken Sie auf **OK**.

    ![Seite mit Details zu Recovery Services-Tresoren](./media/oracle-backup-recovery/recovery_service_05.png)

6.  Verwenden Sie für **Sicherungsrichtlinie** die **DefaultPolicy**, oder wählen Sie **Neue Richtlinie erstellen** aus. Klicken Sie auf **OK**.

    ![Seite mit Details zur Sicherungsrichtlinie für Recovery Services-Tresore](./media/oracle-backup-recovery/recovery_service_06.png)

7.  Aktivieren Sie auf dem Blatt **Virtuelle Computer auswählen** das Kontrollkästchen bei **myVM1**, und klicken Sie dann auf **OK**. Klicken Sie auf die Schaltfläche **Sicherung aktivieren**.

    ![Seite mit Details zu den zu sichernden Recovery Services-Tresorelementen](./media/oracle-backup-recovery/recovery_service_07.png)

    > [!IMPORTANT]
    > Nachdem Sie auf **Sicherung aktivieren** geklickt haben, wird der Sicherungsprozess erst gestartet, wenn die geplante Zeit abgelaufen ist. Führen Sie den nächsten Schritt aus, um eine sofortige Sicherung einzurichten.

8.  Wählen Sie auf dem Blatt **myVault – Sicherungselemente** unter **Anzahl von Sicherungselementen** die Anzahl der Sicherungselemente aus.

    ![Seite mit myVault-Details zu Recovery Services-Tresoren](./media/oracle-backup-recovery/recovery_service_08.png)

9.  Klicken Sie auf dem Blatt **Sicherungselemente (Virtueller Azure-Computer)** rechts auf die Schaltfläche mit den Auslassungspunkten (**...**) und anschließend auf **Jetzt sichern**.

    ![Befehl „Jetzt sichern“ für Recovery Services-Tresore](./media/oracle-backup-recovery/recovery_service_09.png)

10. Klicken Sie auf die Schaltfläche **Sicherung**. Warten Sie, bis der Sicherungsvorgang abgeschlossen ist. Gehen Sie dann zu [Schritt 6: Entfernen der Datenbankdateien](#step-6-remove-the-database-files).

    Klicken Sie auf **Aufträge**, um den Status des Sicherungsauftrags anzuzeigen.

    ![Seite mit Aufträgen zu Recovery Services-Tresoren](./media/oracle-backup-recovery/recovery_service_10.png)

    Der Status des Sicherungsauftrags wird im folgenden Bild dargestellt:

    ![Seite mit Aufträgen zu Recovery Services-Tresoren mit Status](./media/oracle-backup-recovery/recovery_service_11.png)

11. Geben Sie für eine anwendungskonsistente Sicherung alle Fehler in der Protokolldatei an. Die Protokolldatei befindet sich unter /var/log/azure/Microsoft.Azure.RecoveryServices.VMSnapshotLinux/1.0.9114.0.

### <a name="step-6-remove-the-database-files"></a>Schritt 6: Entfernen der Datenbankdateien 
Weiter unten in diesem Artikel erfahren Sie, wie der Wiederherstellungsprozess getestet werden kann. Bevor Sie den Wiederherstellungsprozess testen können, müssen Sie die Datenbankdateien entfernen.

1.  Entfernen Sie den Tabellenbereich und die Sicherungsdateien:

    ```bash
    $ sudo su - oracle
    $ cd /u01/app/oracle/oradata/cdb1
    $ rm -f *.dbf
    $ cd /u01/app/oracle/fast_recovery_area/CDB1/backupset
    $ rm -rf *
    ```
    
2.  (Optional) Fahren Sie die Oracle-Instanz herunter:

    ```bash
    $ sqlplus / as sysdba
    SQL> shutdown abort
    ORACLE instance shut down.
    ```

## <a name="restore-the-deleted-files-from-the-recovery-services-vaults"></a>Wiederherstellen der gelöschten Dateien aus den Recovery Services-Tresoren
Gehen Sie wie folgt vor, um die gelöschten Dateien wiederherzustellen:

1. Suchen Sie im Azure-Portal nach dem Recovery Services-Tresorelement *myVault*. Wählen Sie in der **Übersicht** unter **Sicherungselemente** die Anzahl der Elemente aus.

    ![myVault-Sicherungselemente für Recovery Services-Tresore](./media/oracle-backup-recovery/recovery_service_12.png)

2. Wählen Sie unter **Anzahl von Sicherungselementen** die Anzahl der Elemente aus.

    ![Anzahl von Sicherungselementen auf virtuellem Azure-Computer für Recovery Services-Tresore](./media/oracle-backup-recovery/recovery_service_13.png)

3. Klicken Sie auf dem Blatt **myvm1** auf **Dateiwiederherstellung (Vorschauversion)**.

    ![Screenshot der Seite zur Dateiwiederherstellung für Recovery Services-Tresore](./media/oracle-backup-recovery/recovery_service_14.png)

4. Klicken Sie im Bereich **Dateiwiederherstellung (Vorschauversion)** auf **Skript herunterladen**. Speichern Sie dann die Downloaddatei (.sh) in einem Ordner auf dem Clientcomputer.

    ![Speichern von Optionen durch Herunterladen der Skriptdatei](./media/oracle-backup-recovery/recovery_service_15.png)

5. Kopieren Sie die .sh-Datei auf den virtuellen Computer.

    Im folgenden Beispiel wird dargestellt, wie Sie die Datei mit einem SCP-Befehl (Secure Copy) auf den virtuellen Computer verschieben. Sie können die Inhalte auch in die Zwischenablage kopieren und sie anschließend in eine neue Datei einfügen, die auf dem virtuellen Computer eingerichtet ist.

    > [!IMPORTANT]
    > Stellen Sie im folgenden Beispiel sicher, dass Sie die IP-Adresse und die Ordnerwerte aktualisiert haben. Die Werte müssen dem Ordner zugeordnet werden können, in dem die Datei gespeichert ist.

    ```bash
    $ scp Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh <publicIpAddress>:/<folder>
    ```
6. Ändern Sie die Datei so, dass sie im Besitz des Stamms ist.

    Ändern Sie die Datei im folgenden Beispiel so, dass sie im Besitz des Stamms ist. Ändern Sie dann die Berechtigungen.

    ```bash 
    $ ssh <publicIpAddress>
    $ sudo su -
    # chown root:root /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    # chmod 755 /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    # /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    ```
    Im folgenden Beispiel wird dargestellt, was Ihnen nach der Ausführung des vorherigen Skripts angezeigt werden sollte. Geben Sie **Y** ein, wenn Sie zum Fortsetzen des Vorgangs aufgefordert werden.

    ```bash
    Microsoft Azure VM Backup - File Recovery
    ______________________________________________
    The script requires 'open-iscsi' and 'lshw' to run.
    Do you want us to install 'open-iscsi' and 'lshw' on this machine?
    Please press 'Y' to continue with installation, 'N' to abort the operation. : Y
    Installing 'open-iscsi'....
    Installing 'lshw'....

    Connecting to recovery point using ISCSI service...

    Connection succeeded!

    Please wait while we attach volumes of the recovery point to this machine...

    ************ Volumes of the recovery point and their mount paths on this machine ************

    Sr.No.  |  Disk  |  Volume  |  MountPath

    1)  | /dev/sde  |  /dev/sde1  |  /root/myVM-20170517093913/Volume1

    2)  | /dev/sde  |  /dev/sde2  |  /root/myVM-20170517093913/Volume2

    ************ Open File Explorer to browse for files. ************

    After recovery, to remove the disks and close the connection to the recovery point, please click 'Unmount Disks' in step 3 of the portal.

    Please enter 'q/Q' to exit...
    ```

7. Der Zugriff auf die bereitgestellten Volumes wurde bestätigt.

    Geben Sie zum Beenden **q** ein, und suchen Sie anschließend nach den bereitgestellten Volumes. Geben Sie **df -k** ein, um bei einer Eingabeaufforderung eine Liste mit den hinzugefügten Volumes zu erstellen.

    ![Befehl „df-k“](./media/oracle-backup-recovery/recovery_service_16.png)

8. Verwenden Sie das folgende Skript, um die fehlenden Dateien wieder in die Ordner zu kopieren:

    ```bash
    # cd /root/myVM-2017XXXXXXX/Volume2/u01/app/oracle/fast_recovery_area/CDB1/backupset/2017_xx_xx
    # cp *.bkp /u01/app/oracle/fast_recovery_area/CDB1/backupset/2017_xx_xx
    # cd /u01/app/oracle/fast_recovery_area/CDB1/backupset/2017_xx_xx
    # chown oracle:oinstall *.bkp
    # cd /root/myVM-2017XXXXXXX/Volume2/u01/app/oracle/oradata/cdb1
    # cp *.dbf /u01/app/oracle/oradata/cdb1
    # cd /u01/app/oracle/oradata/cdb1
    # chown oracle:oinstall *.dbf
    ```
9. Verwenden Sie im folgenden Skript RMAN, um die Datenbank wiederherzustellen:

    ```bash
    # sudo su - oracle
    $ rman target /
    RMAN> startup mount;
    RMAN> restore database;
    RMAN> recover database;
    RMAN> alter database open resetlogs;
    RMAN> SELECT * FROM scott.scott_table;
    ```
    
10. Heben Sie die Bereitstellung des Datenträgers auf.

    Klicken Sie im Azure-Portal auf dem Blatt **Dateiwiederherstellung (Vorschauversion)** auf **Bereitstellung der Datenträger aufheben**.

    ![Befehl zum Aufheben der Bereitstellung von Datenträgern](./media/oracle-backup-recovery/recovery_service_17.png)

## <a name="restore-the-entire-vm"></a>Wiederherstellen des gesamten virtuellen Computers

Statt die aus den Recovery Services-Tresoren gelöschten Dateien wiederherzustellen, können Sie den gesamten virtuellen Computer wiederherstellen.

### <a name="step-1-delete-myvm"></a>Schritt 1: Löschen von myVM

*   Rufen Sie im Azure-Portal den **myVM1**-Tresor auf, und wählen Sie dann **Löschen** aus.

    ![Befehl „Löschen“ für Tresore](./media/oracle-backup-recovery/recover_vm_01.png)

### <a name="step-2-recover-the-vm"></a>Schritt 2: Wiederherstellen des virtuellen Computers

1.  Wechseln Sie zu **Recovery Services-Tresore**, und wählen Sie anschließend **myVault** aus.

    ![myVault-Eintrag](./media/oracle-backup-recovery/recover_vm_02.png)

2.  Wählen Sie in der **Übersicht** unter **Sicherungselemente** die Anzahl der Elemente aus.

    ![myVault-Sicherungselemente](./media/oracle-backup-recovery/recover_vm_03.png)

3.  Wählen Sie auf dem Blatt **Sicherungselemente (Virtueller Azure-Computer)** **myvm1** aus.

    ![Seite zur Wiederherstellung des virtuellen Computers](./media/oracle-backup-recovery/recover_vm_04.png)

4.  Klicken Sie auf dem Blatt **myvm1** auf die Schaltfläche mit den Auslassungspunkten (**...**) und dann auf **Virtuellen Computer wiederherstellen**.

    ![Befehl „Virtuellen Computer wiederherstellen“](./media/oracle-backup-recovery/recover_vm_05.png)

5.  Wählen Sie auf dem Blatt **Wiederherstellungspunkt auswählen** das Element aus, das Sie wiederherstellen möchten, und klicken Sie anschließend auf **OK**.

    ![Wählen Sie den Wiederherstellungspunkt aus.](./media/oracle-backup-recovery/recover_vm_06.png)

    Wenn Sie die anwendungskonsistente Sicherung aktiviert haben, wird eine vertikale blaue Leiste angezeigt.

6.  Wählen Sie auf dem Blatt **Wiederherstellungskonfiguration** den Namen des virtuellen Computers und die Ressourcengruppe aus, und klicken Sie dann auf **OK**.

    ![Werte der Wiederherstellungskonfiguration](./media/oracle-backup-recovery/recover_vm_07.png)

7.  Klicken Sie auf die Schaltfläche **Wiederherstellen**, um die VM wiederherzustellen.

8.  Klicken Sie auf **Aufträge** und dann auf **Sicherungsaufträge**, um den Status des Wiederherstellungsprozesses anzuzeigen.

    ![Befehl „Status des Sicherungsauftrags“](./media/oracle-backup-recovery/recover_vm_08.png)

    Die folgende Abbildung zeigt den Status des Wiederherstellungsvorgangs:

    ![Status des Wiederherstellungsprozesses](./media/oracle-backup-recovery/recover_vm_09.png)

### <a name="step-3-set-the-public-ip-address"></a>Schritt 3: Einrichten der öffentlichen IP-Adresse
Nachdem der virtuelle Computer wiederhergestellt wurde, können Sie die öffentliche IP-Adresse einrichten.

1.  Geben Sie in das Suchfeld **öffentliche IP-Adresse** ein.

    ![Liste der öffentlichen IP-Adressen](./media/oracle-backup-recovery/create_ip_00.png)

2.  Klicken Sie auf dem Blatt **Öffentliche IP-Adressen** auf **Hinzufügen**. Wählen Sie auf dem Blatt **Öffentliche IP-Adresse erstellen** als **Name** den öffentlichen IP-Namen aus. Wählen Sie für **Ressourcengruppe** die Option **Vorhandene verwenden** aus. Klicken Sie dann auf **Erstellen**.

    ![Erstellen der IP-Adresse](./media/oracle-backup-recovery/create_ip_01.png)

3.  Suchen Sie nach **myVMip** und wählen Sie dies aus, um die öffentliche IP-Adresse der Netzwerkschnittstelle des virtuellen Computers zuzuordnen. Klicken Sie dann auf **Zuordnen**.

    ![Zuordnen einer IP-Adresse](./media/oracle-backup-recovery/create_ip_02.png)

4.  Wählen Sie als **Ressourcentyp** die Option **Netzwerkschnittstelle** aus. Wählen Sie die Netzwerkschnittstelle aus, die von der myVM-Instanz verwendet wird, und klicken Sie dann auf **OK**.

    ![Auswählen von Ressourcentyp und Netzschnittstellenwerten](./media/oracle-backup-recovery/create_ip_03.png)

5.  Suchen Sie nach der Instanz von myVM, die aus dem Portal portiert wird, und öffnen Sie sie. Die dem virtuellen Computer zugeordnete IP-Adresse wird auf dem Blatt **Übersicht** von myVM angezeigt.

    ![Wert der IP-Adresse](./media/oracle-backup-recovery/create_ip_04.png)

### <a name="step-4-connect-to-the-vm"></a>Schritt 4: Herstellen der Verbindung zum virtuellen Computer

*   Verwenden Sie folgendes Skript, um eine Verbindung zum virtuellen Computer herzustellen:

    ```bash 
    ssh <publicIpAddress>
    ```

### <a name="step-5-test-whether-the-database-is-accessible"></a>Schritt 5: Testen, ob auf die Datenbank zugegriffen werden kann
*   Verwenden Sie folgendes Skript, um den Zugriff zu testen:

    ```bash 
    $ sudo su - oracle
    $ sqlplus / as sysdba
    SQL> startup
    ```

    > [!IMPORTANT]
    > Wenn der Datenbankbefehl **Start** einen Fehler generiert, finden Sie unter [Schritt 6: Verwenden von RMAN zum Wiederherstellen der Datenbank](#step-6-optional-use-rman-to-recover-the-database) Informationen zum Wiederherstellen der Datenbank.

### <a name="step-6-optional-use-rman-to-recover-the-database"></a>Schritt 6: (Optional) Verwenden von RMAN zum Wiederherstellen der Datenbank
*   Verwenden Sie das folgende Skript, um die Datenbank wiederherzustellen:

    ```bash
    # sudo su - oracle
    $ rman target /
    RMAN> startup mount;
    RMAN> restore database;
    RMAN> recover database;
    RMAN> alter database open resetlogs;
    RMAN> SELECT * FROM scott.scott_table;
    ```

Die Sicherung und Wiederherstellung der Oracle Database 12c-Datenbank auf einem virtuellen Azure Linux-Computer ist nun abgeschlossen.

## <a name="delete-the-vm"></a>Löschen der virtuellen Computer

Wenn Sie den virtuellen Computer nicht mehr benötigen, können Sie mit dem folgenden Befehl die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen entfernen:

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

[Tutorial: Erstellen eines hoch verfügbaren virtuellen Computers](../../linux/create-cli-complete.md)

[Erkunden der Azure-CLI-Beispiele für die Bereitstellung virtueller Computer](../../linux/cli-samples.md)




