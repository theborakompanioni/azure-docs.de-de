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
ms.sourcegitcommit: db18dd24a1d10a836d07c3ab1925a8e59371051f
ms.openlocfilehash: 7a53ca814170a6651fc499c41395fe088a6f869a
ms.contentlocale: de-de
ms.lasthandoff: 06/15/2017

---

# <a name="back-up-and-recover-an-oracle-database-12c-database-on-an-azure-linux-virtual-machine"></a>Sichern und Wiederherstellen einer Oracle Database 12c-Datenbank auf einem virtuellen Azure Linux-Computer

Sie können über die Azure-Befehlszeilenschnittstelle (Command-Line Interface, CLI) oder über Skripts Azure-Ressourcen erstellen und verwalten. In diesem Artikel verwenden Sie Skripts in der Azure-Befehlszeilenschnittstelle, um eine Oracle Database 12c-Datenbank aus einem Azure Marketplace-Katalogimage bereitzustellen.

Stellen Sie sicher, dass die Azure-CLI installiert ist, bevor Sie beginnen. Weitere Informationen finden Sie im [Azure CLI-Installationshandbuch](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="prepare-the-environment"></a>Vorbereiten der Umgebung
### <a name="step-1-assumptions"></a>Schritt 1: Annahmen

*   Sie müssen für die Sicherung und Wiederherstellung mit einer installierten Instanz von Oracle Database 12c einen virtuellen Linux-Computer erstellen. Verwenden Sie zum Erstellen der virtuellen Computer das Marketplace-Image *Oracle:Oracle-Database-Ee:12.1.0.2:latest*.

    Anweisungen zur Vorgehensweise beim Erstellen einer Oracle-Datenbank finden Sie im [Oracle Database Quick Create Guide (Handbuch zur Schnellerfassung von Oracle-Datenbanken)](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/oracle-database-quick-create).


### <a name="step-2-connect-to-the-vm"></a>Schritt 2: Herstellen der Verbindung zum virtuellen Computer

*   Verwenden Sie folgenden Befehl, um eine SSH-Sitzung (SSH = Secure Shell) mit dem virtuellen Computer zu erstellen: Ersetzen Sie die Kombination aus IP-Adresse und Hostname durch den Wert `publicIpAddress` für Ihren virtuellen Computer.

    ```bash 
    ssh <publicIpAddress>
    ```

### <a name="step-3-prepare-the-database"></a>Schritt 3: Vorbereiten der Datenbank

1.  In diesem Schritt wird davon ausgegangen, dass Sie über eine Oracle-Instanz (cdb1) verfügen, die auf dem virtuellen Computer *MyVM* ausgeführt wird.

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

2.  Stellen Sie sicher, dass sich die Datenbank im Archivierungsprotokoll-Modus (optionaler Schritt) befindet:

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
3.  Erstellen Sie eine Tabelle zum Testen des Commits (optionaler Schritt):

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
5. Verwenden Sie den Oracle Recovery Manager (RMAN), um die Datenbank zu sichern:

    ```bash
    $ rman target /
    RMAN> backup database plus archivelog;
    ```

### <a name="step-4-application-consistent-backup-for-linux-vms"></a>Schritt 4: Anwendungskonsistente Sicherung für virtuelle Linux-Computer

Dies ist ein neues Feature von Azure Backup, mit dem Benutzer Skripts (pre oder post) angeben können, die vor und nach dem Erstellen der Momentaufnahme des virtuellen Computers ausgeführt werden sollen.

#### <a name="1-download-vmsnapshotscriptpluginconfigjson-from-httpsgithubcommicrosoftazurebackupvmsnapshotpluginconfig-content-should-be-similar-to-following"></a>1. Laden Sie „VMSnapshotScriptPluginConfig.json von https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig“ herunter. Der Inhalt sollte in etwa wie folgt aussehen.

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

#### <a name="2-create-etcazure-folder-on-vm"></a>2. Erstellen Sie den Ordner „/etc/azure“ auf dem virtuellen Computer.

```bash
$ sudo su -
# mkdir /etc/azure
# cd /etc/azure
```

#### <a name="3-copy-vmsnapshotscriptpluginconfigjson-file-to-folder-etcazure"></a>3. Kopieren Sie die Datei „VMSnapshotScriptPluginConfig.json in den Ordner /etc/azure“.

#### <a name="4-edit-the-vmsnapshotscriptpluginconfigjson-file-to-included-the-prescriptlocation-and-postscriptlocation-parameters-for-example"></a>4. Bearbeiten Sie die Datei „VMSnapshotScriptPluginConfig.json“, um die Parameter „PreScriptLocation“ und „PostScriptlocation“ einzuschließen. Beispiel:
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
#### <a name="5-create-the-pre-and-post-script-files"></a>5. Erstellen der Pre- und Post-Skriptdateien

Im Folgenden finden Sie ein Beispiel für Pre- und Post-Skripts bei einem Cold Backup (Herunterfahren und Neustart)

Bei /etc/azure/pre_script.sh

```bash
v_date=`date +%Y%m%d%H%M`
ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
ORA_OWNER=oracle
su - $ORA_OWNER -c "$ORA_HOME/bin/dbshut $ORA_HOME" > /etc/azure/pre_script_$v_date.log
```

Bei /etc/azure/post_script.sh

```bash
v_date=`date +%Y%m%d%H%M`
ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
ORA_OWNER=oracle
su - $ORA_OWNER -c "$ORA_HOME/bin/dbstart $ORA_HOME" > /etc/azure/post_script_$v_date.log
```

Im Folgenden finden Sie ein Beispiel für Pre- und Post-Skripts bei einem Hot Backup

```bash
v_date=`date +%Y%m%d%H%M`
ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
ORA_OWNER=oracle
su - $ORA_OWNER -c "sqlplus / as sysdba @/etc/azure/pre_script.sql" > /etc/azure/pre_script_$v_date.log
```

Bei /etc/azure/post_script.sh

```bash
v_date=`date +%Y%m%d%H%M`
ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
ORA_OWNER=oracle
su - $ORA_OWNER -c "sqlplus / as sysdba @/etc/azure/post_script.sql" > /etc/azure/pre_script_$v_date.log
```

Bei /etc/azure/pre_script.sql müssen Sie den Inhalt der Datei Ihren Anforderungen entsprechend ändern.

```bash
alter tablespace SYSTEM begin backup;
...
...
alter system switch logfile;
alter system archive log stop;
```

Bei /etc/azure/post_script.sql müssen Sie den Inhalt der Datei Ihren Anforderungen entsprechend ändern.

```bash
alter tablespace SYSTEM end backup;
...
...
alter system archive log start;
```

#### <a name="6-change-file-permissions"></a>6. Ändern Sie die Dateiberechtigungen

```bash
# chmod 600 /etc/azure/VMSnapshotScriptPluginConfig.json
# chmod 700 /etc/azure/pre_script.sh
# chmod 700 /etc/azure/post_script.sh
```

#### <a name="7-test-the-scripts-sign-in-as-root-make-sure-no-errors"></a>7. Testen Sie die Skripts (melden Sie sich als Root an), und stellen Sie sicher, dass keine Fehler vorhanden sind

```bash
# /etc/azure/pre_script.sh
# /etc/azure/post_script.sh
```

Weitere Informationen finden Sie unter [Anwendungskonsistente Sicherung virtueller Linux-Computer](https://azure.microsoft.com/en-us/blog/announcing-application-consistent-backup-for-linux-vms-using-azure-backup/).


### <a name="step-5-use-azure-recovery-services-vaults-to-back-up-the-vm"></a>Schritt 5: Verwenden von Azure Recovery Services-Tresoren zum Sichern des virtuellen Computers

1.  Melden Sie sich beim Azure-Portal an, und suchen Sie anschließend nach einer Instanz der Recovery Services-Tresore.
![Seite mit Recovery Services-Tresoren](./media/oracle-backup-recovery/recovery_service_01.png)

2.  Klicken Sie auf die Schaltfläche **Hinzufügen**, um einen neuen Tresor hinzuzufügen.
![Seite zum Hinzufügen von Recovery Services-Tresoren](./media/oracle-backup-recovery/recovery_service_02.png)

3.  Klicken Sie auf **myVault**, um fortzufahren. Die Seite „Details“ wird angezeigt.
![Seite mit Details zu Recovery Services-Tresoren](./media/oracle-backup-recovery/recovery_service_03.png)

4.  Klicken Sie auf die Schaltfläche **Sicherung**. Fügen Sie als Nächstes das Sicherungsziel, die Richtlinie und Sicherungselemente hinzu.
![Seite zur Sicherung der Recovery Services-Tresore](./media/oracle-backup-recovery/recovery_service_04.png)

5.  Verwenden Sie bei **Sicherungsziel** die Standardwerte für **Azure** und **Virtueller Computer**. Klicken Sie auf **OK**, um fortzufahren.
![Seite mit Details zu Recovery Services-Tresoren](./media/oracle-backup-recovery/recovery_service_05.png)

6.  Verwenden Sie für **Sicherungsrichtlinie** die **DefaultPolicy** oder **Neue Richtlinie erstellen**. Klicken Sie auf **OK**, um fortzufahren.
![Seite mit Details zur Sicherungsrichtlinie für Recovery Services-Tresore](./media/oracle-backup-recovery/recovery_service_06.png)

7.  Aktivieren Sie das Kontrollkästchen **myVM1**, klicken Sie auf **OK**, und klicken Sie anschließend auf **Sicherung aktivieren**.
![Seite mit Details zu den zu sichernden Recovery Services-Tresorelementen](./media/oracle-backup-recovery/recovery_service_07.png)

    > [!IMPORTANT]
    > Nachdem Sie auf **Sicherung aktivieren** geklickt haben, wird der Sicherungsprozess erst gestartet, wenn die geplante Zeit abgelaufen ist. Führen Sie den nächsten Schritt aus, um eine sofortige Sicherung einzurichten.

8.  Klicken Sie auf **Elemente sichern** und anschließend unter **Anzahl von Sicherungselementen** auf die Anzahl der Sicherungselemente.

    ![Seite mit myVault-Details zu Recovery Services-Tresoren](./media/oracle-backup-recovery/recovery_service_08.png)

9.  Klicken Sie rechts auf der Seite auf die Schaltfläche mit den Auslassungspunkten (**...** ) und anschließend auf **Jetzt sichern**.

    ![Befehl „Jetzt sichern“ für Recovery Services-Tresore](./media/oracle-backup-recovery/recovery_service_09.png)

10. Klicken Sie auf die Schaltfläche **Sichern**, warten Sie, bis der Sicherungsprozess abgeschlossen wurde, und wechseln Sie anschließend zu „Schritt 5: Entfernen der Datenbankdateien“.
Klicken Sie auf **Aufträge**, um den Status des Sicherungsauftrags anzuzeigen.
![Seite mit Aufträgen zu Recovery Services-Tresoren](./media/oracle-backup-recovery/recovery_service_10.png)

    Der Status des Sicherungsauftrags wird im nächsten Bild dargestellt.

    ![Seite mit Aufträgen zu Recovery Services-Tresoren mit Status](./media/oracle-backup-recovery/recovery_service_11.png)

11. Für eine anwendungskonsistente Sicherung. Behandeln Sie alle Fehler in der Protokolldatei „/var/log/azure/Microsoft.Azure.RecoveryServices.VMSnapshotLinux/1.0.9114.0“.

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
    
2.  Fahren Sie die Oracle-Instanz herunter (optionaler Schritt):

    ```bash
    $ sqlplus / as sysdba
    SQL> shutdown abort
    ORACLE instance shut down.
    ```

## <a name="restore-the-deleted-files-from-recovery-services-vaults"></a>Wiederherstellen der gelöschten Dateien aus Recovery Services-Tresoren
Gehen Sie wie folgt vor, um die gelöschten Dateien wiederherzustellen:

1. Melden Sie sich beim Azure-Portal an, und suchen Sie anschließend nach dem Recovery Services-Tresorelement *MyVault*. Klicken Sie in der rechten oberen Ecke unter **Elemente sichern** auf die Anzahl der Elemente.
![myVault-Sicherungselemente für Recovery Services-Tresore](./media/oracle-backup-recovery/recovery_service_12.png)

2. Klicken Sie unter **Anzahl von Sicherungselementen** auf die Anzahl der Elemente.
![Anzahl von Sicherungselementen auf virtuellem Azure-Computer für Recovery Services-Tresore](./media/oracle-backup-recovery/recovery_service_13.png)

3. Klicken Sie auf **Dateiwiederherstellung (Vorschau)**.
![Screenshot der Seite zur Dateiwiederherstellung für Recovery Services-Tresore](./media/oracle-backup-recovery/recovery_service_14.png)

4. Klicken Sie auf **Skript herunterladen**, und speichern Sie die heruntergeladene Datei (.sh) anschließend in einem Ordner auf dem Clientcomputer.
![Speichern von Optionen durch Herunterladen der Skriptdatei](./media/oracle-backup-recovery/recovery_service_15.png)

5. Kopieren Sie die .sh-Datei auf den virtuellen Computer.

    Im nächsten Beispiel wird dargestellt, wie die Datei mit einem SCP-Befehl (SCP = Secure Copy) auf den virtuellen Computer verschoben wird. Sie können den Inhalt auch in die Zwischenablage kopieren und den Inhalt anschließend in eine neue Datei einfügen, die auf dem virtuellen Computer eingerichtet ist.

    > [!IMPORTANT]
    > Im nächsten Beispiel stellen Sie sicher, dass Sie die IP-Adresse und die Ordnerwerte aktualisiert haben. Die Werte müssen dem Ordner zugeordnet werden, in dem die Datei gespeichert ist.

    ```bash
    $ scp Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh <publicIpAddress>:/<folder>
    ```
6. Ändern Sie die Datei so, dass sie im Besitz des Stamms ist.

    Im nächsten Beispiel ändern Sie die Datei so, dass sie im Besitz des Stamms ist. Anschließend ändern Sie die Berechtigung.

    ```bash 
    $ ssh <publicIpAddress>
    $ sudo su -
    # chown root:root /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    # chmod 755 /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    # /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    ```
    Im nächsten Beispiel wird dargestellt, was Ihnen nach der Ausführung des vorherigen Skripts angezeigt werden sollte. Geben Sie **Y** ein, wenn Sie zum Fortsetzen des Vorgangs aufgefordert werden.

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

    ![Befehl df -k](./media/oracle-backup-recovery/recovery_service_16.png)

8. Verwenden Sie das folgende Skript, um die fehlenden Dateien wieder in die Ordner zu kopieren.

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

    Klicken Sie im Azure-Portal auf **Bereitstellung von Datenträgern aufheben**.

    ![Befehl zum Aufheben der Bereitstellung von Datenträgern](./media/oracle-backup-recovery/recovery_service_17.png)

## <a name="restore-the-entire-vm"></a>Wiederherstellen des gesamten virtuellen Computers

Statt die aus Recovery Services-Tresoren gelöschten Dateien wiederherzustellen, können Sie den gesamten virtuellen Computer wiederherstellen.

### <a name="step-1-drop-the-myvm"></a>Schritt 1: Löschen von myVM

*   Melden Sie sich beim Azure-Portal an, rufen Sie den *myVM*-Tresor auf, und wählen Sie anschließend **Löschen** aus.

    ![Befehl „Löschen“](./media/oracle-backup-recovery/recover_vm_01.png)

### <a name="step-2-recover-the-vm"></a>Schritt 2: Wiederherstellen des virtuellen Computers

1.  Wechseln Sie zu **Recovery Services-Tresore**, und wählen Sie anschließend **myVault** aus.
![Eintrag „myVault“, der in der Benutzeroberfläche angezeigt wird](./media/oracle-backup-recovery/recover_vm_02.png)

2.  Klicken Sie in der rechten oberen Ecke unter **Elemente sichern** auf die Anzahl der Elemente.
![myVault-Sicherungselemente](./media/oracle-backup-recovery/recover_vm_03.png)

3.  Klicken Sie unter **Anzahl von Sicherungselementen** auf die Anzahl der Elemente.
![Seite zur Wiederherstellung des virtuellen Computers](./media/oracle-backup-recovery/recover_vm_04.png)

4.  Klicken Sie rechts auf der Seite auf die Schaltfläche mit den Auslassungspunkten (**...** ) und anschließend auf **Virtuellen Computer wiederherstellen**.
![Befehl „Virtuellen Computer wiederherstellen“](./media/oracle-backup-recovery/recover_vm_05.png)

5.  Wählen Sie das Element, das Sie wiederherstellen möchten, und klicken Sie anschließend auf **OK**.
![Wählen Sie den Wiederherstellungspunkt](./media/oracle-backup-recovery/recover_vm_06.png) aus. Sie müssten einen senkrechten blauen Strich sehen, wenn Sie die anwendungskonsistente Sicherung aktiviert haben.

6.  Wählen Sie den **Namen des virtuellen Computers** und die **Ressourcengruppe** aus, und klicken Sie anschließend auf **OK**.
![Wiederherstellen von Konfigurationswerten](./media/oracle-backup-recovery/recover_vm_07.png)

7.  Klicken Sie zum Wiederherstellen des virtuellen Computers in der linken unteren Ecke der Benutzeroberfläche (siehe vorherige Abbildung) auf **Wiederherstellen**.

8.  Klicken Sie auf **Aufträge**, um den Status des Wiederherstellungsprozesses anzuzeigen.
![Befehl „Status des Sicherungsauftrags“](./media/oracle-backup-recovery/recover_vm_08.png)

    In der nächsten Abbildung wird der Status des Wiederherstellungsprozesses dargestellt.

    ![Status des Wiederherstellungsprozesses](./media/oracle-backup-recovery/recover_vm_09.png)

### <a name="step-3-set-the-public-ip-address"></a>Schritt 3: Einrichten der öffentlichen IP-Adresse
Nachdem der virtuelle Computer wiederhergestellt wurde, können Sie die öffentliche IP-Adresse einrichten.

1.  Suchen Sie über das Suchfeld nach **Öffentlichen IP-Adressen**.

    ![Liste der öffentlichen IP-Adressen](./media/oracle-backup-recovery/create_ip_00.png)

2.  Klicken Sie in der linken oberen Ecke auf **Hinzufügen**. Wählen Sie für **Name** den Namen der öffentlichen IP und für **Ressourcengruppe** den Eintrag **Vorhandene verwenden** aus. Klicken Sie auf **Erstellen**.

    ![Erstellen der IP-Adresse](./media/oracle-backup-recovery/create_ip_01.png)

3.  Suchen Sie nach **myVMip**, und klicken Sie anschließend auf **Zuordnen**, um die öffentliche IP-Adresse der Netzwerkschnittstelle des virtuellen Computers zuzuordnen.

    ![Zuordnen einer IP-Adresse](./media/oracle-backup-recovery/create_ip_02.png)

4.  Wählen Sie für den **Ressourcentyp** den Eintrag **Netzwerkschnittstelle** aus. Wählen Sie die Netzschnittstelle aus, die von der myVM-Instanz verwendet wird, und klicken Sie anschließend auf **OK**.

    ![Auswählen von Ressourcentyp und Netzschnittstellenwerten](./media/oracle-backup-recovery/create_ip_03.png)

5.  Suchen Sie nach der myVM-Instanz, die aus dem Portal portiert wird, und öffnen Sie sie. Die dem virtuellen Computer zugeordnete IP-Adresse wird in der rechten oberen Ecke angezeigt.

    ![Wert der IP-Adresse](./media/oracle-backup-recovery/create_ip_04.png)

### <a name="step-4-connect-to-the-vm"></a>Schritt 4: Herstellen der Verbindung zum virtuellen Computer

*   Verwenden Sie hierzu folgendes Skript:

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
> Wenn der Datenbankbefehl **Start** einen Fehler generiert, finden Sie unter „Schritt 6: Verwenden von RMAN zum Wiederherstellen der Datenbank“ Informationen zum Wiederherstellen der Datenbank.

### <a name="step-6-use-rman-to-recover-the-database-optional-step"></a>Schritt 6: Verwenden von RMAN zum Wiederherstellen der Datenbank (optionaler Schritt)
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

Die Sicherung und Wiederherstellung der Oracle 12c-Datenbank auf einem virtuellen Azure Linux-Computer ist nun abgeschlossen.
## <a name="delete-the-vm"></a>Löschen des virtuellen Computers

Wenn Sie den virtuellen Computer nicht mehr benötigen, können Sie mit dem folgenden Befehl die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen entfernen:

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

[Tutorial: Erstellen eines hoch verfügbaren virtuellen Computers](../../linux/create-cli-complete.md)

[Erkunden der Azure-CLI-Beispiele für die Bereitstellung virtueller Computer](../../linux/cli-samples.md)

