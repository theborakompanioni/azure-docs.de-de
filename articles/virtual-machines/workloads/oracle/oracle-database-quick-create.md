---
title: Erstellen einer Oracle Database-12c-Datenbank auf einem virtuellen Azure-Computer | Microsoft-Dokumentation
description: Richten Sie in Ihrer Azure-Umgebung schnell eine Oracle Database-12c-Datenbank ein.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: tonyguid
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/26/2017
ms.author: rclaus
ms.translationtype: Human Translation
ms.sourcegitcommit: e72275ffc91559a30720a2b125fbd3d7703484f0
ms.openlocfilehash: 38cb5c4c1beb0e50c8a6395afb118c63b2750210
ms.contentlocale: de-de
ms.lasthandoff: 05/05/2017

---

# <a name="create-an-oracle-database-12c-database-in-an-azure-virtual-machine"></a>Erstellen einer Oracle Database-12c-Datenbank auf einem virtuellen Azure-Computer

Sie können die Azure-Befehlszeilenschnittstelle (CLI) zum Erstellen und Verwalten von Azure-Ressourcen über eine Eingabeaufforderung oder mit Skripts verwenden. In diesem Artikel verwenden Sie Skripts in der Azure-CLI, um eine Oracle Database-12c-Datenbank aus einem Azure Marketplace-Katalogimage bereitzustellen.

Stellen Sie sicher, dass die Azure-CLI installiert ist, bevor Sie beginnen. Weitere Informationen finden Sie im [Azure CLI-Installationshandbuch](https://docs.microsoft.com/cli/azure/install-azure-cli). 

## <a name="sign-in-to-azure"></a>Anmelden bei Azure 

Verwenden Sie in der Azure CLI zum Anmelden bei Ihrem Azure-Abonnement den Befehl [az login](/cli/azure/#login). Befolgen Sie dann die Anweisungen auf dem Bildschirm.

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Verwenden Sie zum Erstellen einer Ressourcengruppe den Befehl [az group create](/cli/azure/group#create). Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. 

Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myResourceGroup` am Standort `westus`:

```azurecli
az group create --name myResourceGroup --location westus
```

## <a name="create-a-vm"></a>Erstellen einer VM

Verwenden Sie zum Erstellen eines virtuellen Computers (VM) den Befehl [az vm create](/cli/azure/vm#create). 

Im folgenden Beispiel wird ein virtueller Computer namens `myVM` erstellt. Darüber hinaus werden SSH-Schlüssel erstellt, falls sie nicht bereits an einem Standardschlüsselspeicherort vorhanden sind. Um einen bestimmten Satz von Schlüsseln zu verwenden, nutzen Sie die Option `--ssh-key-value`.  

```azurecli
az vm create --resource-group myResourceGroup \
    --name myVM \
    --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
    --size Standard_DS2_v2 \
    --generate-ssh-keys
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

## <a name="connect-to-the-vm"></a>Herstellen der Verbindung zur VM

Erstellen Sie mit dem folgenden Befehl eine SSH-Sitzung mit dem virtuellen Computer. Ersetzen Sie die IP-Adresse durch den Wert für `publicIpAddress` Ihres virtuellen Computers.

```bash 
ssh <publicIpAddress>
```

## <a name="create-the-database"></a>Erstellen der Datenbank

Die Oracle-Software ist bereits im Marketplace-Image installiert. Installieren Sie als Nächstes die Datenbank. 

1.  Führen Sie den *Oracle*-Superuser aus, und initialisieren Sie den Listener für die Protokollierung:

    ```bash
    sudo su - oracle
    [oracle@myVM /]$ lsnrctl start
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

2.  Erstellen Sie die Datenbank:

    ```bash
    [oracle@myVM /]$ dbca -silent \
    -createDatabase \
    -templateName General_Purpose.dbc \
    -gdbname cdb1 \
    -sid cdb1 \
    -responseFile NO_VALUE \
    -characterSet AL32UTF8 \
    -sysPassword OraPasswd1 \
    -systemPassword OraPasswd1 \
    -createAsContainerDatabase true \
    -numberOfPDBs 1 \
    -pdbName pdb1 \
    -pdbAdminPassword OraPasswd1 \
    -databaseType MULTIPURPOSE \
    -automaticMemoryManagement false \
    -storageType FS \
    -ignorePreReqs

    Copying database files
    1% complete
    2% complete
    8% complete
    13% complete
    19% complete
    27% complete
    Creating and starting Oracle instance
    29% complete
    32% complete
    33% complete
    34% complete
    38% complete
    42% complete
    43% complete
    45% complete
    Completing Database Creation
    48% complete
    51% complete
    53% complete
    62% complete
    70% complete
    72% complete
    Creating Pluggable Databases
    78% complete
    100% complete
    Look at the log file "/u01/app/oracle/cfgtoollogs/dbca/cdb1/cdb1.log" for further details.
    ```

## <a name="prepare-for-connectivity"></a>Vorbereiten auf die Konnektivität 
Testen Sie auf lokale Konnektivität, um sicherzustellen, dass die Datenbank richtig initialisiert wurde. Die einfachste Möglichkeit hierzu besteht darin, eine Verbindung mit `sqlplus` herzustellen.  

Bevor Sie eine Verbindung herstellen, müssen Sie zwei Umgebungsvariablen festlegen: ORACLE_HOME und ORACLE_SID.

```bash
ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME

ORACLE_SID=cdb1; export ORACLE_SID
```

Sie können ORACLE_HOME und ORACLE_SID auch zur Datei .bashrc hinzufügen. Dies speichert die Umgebungsvariablen für zukünftige Anmeldungen.

```
# Add ORACLE_HOME.
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1

# Add ORACLE_SID.
export ORACLE_SID=cdb1

```

## <a name="oracle-em-express-connectivity"></a>Konnektivität mit Oracle EM Express

Damit ein GUI-Verwaltungstool verwendet werden kann, um die Datenbank zu untersuchen, müssen Sie Oracle EM Express einrichten. Um eine Verbindung mit Oracle EM Express herstellen zu können, muss zuerst der Port in Oracle konfigurieren werden:

```bash
$ sudo su - oracle

sqlplus / as sysdba

SQL*Plus: Release 12.1.0.2.0 Production on Fri Apr 7 13:16:30 2017

Copyright (c) 1982, 2014, Oracle.  All rights reserved.


Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With the Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL> select con_id, name, open_mode from v$pdbs;

    CON_ID NAME                           OPEN_MODE
---------- ------------------------------ ----------
         2 PDB$SEED                       READ ONLY
         3 PDB1                           MOUNT

SQL> alter session set container=pdb1;

Session altered.

SQL> alter database open;

database opened.

SQL> alter session set container=pdb1;

Session altered.

SQL> exec DBMS_XDB_CONFIG.SETHTTPSPORT(5502);

PL/SQL procedure successfully completed.
```

## <a name="automate-database-startup-and-shutdown"></a>Automatisieren des Startens und Beendens der Datenbank

Die Oracle-Datenbank wird standardmäßig nicht automatisch gestartet, wenn Sie den virtuellen Computer starten. Sie müssen sich als Root-Benutzer anmelden, um festzulegen, dass die Oracle-Datenbank gestartet wird, wenn Sie den virtuellen Computer starten. Erstellen und aktualisieren Sie anschließend einige Systemdateien.

1.  Melden Sie sich als Root-Benutzer an:

    ```bash
    # sudo su -
    ```

2.  Ändern Sie die Datei „/etc/oratab“ von der Standardeinstellung `N` in `Y`:

    ```
    cdb1:/u01/app/oracle/product/12.1.0/dbhome_1:Y
    ```

3.  Erstellen Sie die Datei „/etc/init.d/dbora“:

    ```bash
    #!/bin/sh
    # chkconfig: 345 99 10
    # Description: Oracle auto start-stop script.
    #
    # Set ORA_HOME to be equivalent to $ORACLE_HOME.
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle

    case "$1" in
    'start')
        # Start the Oracle databases:
        # The following command assumes that the Oracle sign-in
        # will not prompt the user for any values.
        # Remove "&" if you don't want startup as a background process.
        su - $ORA_OWNER -c "$ORA_HOME/bin/dbstart $ORA_HOME" &
        touch /var/lock/subsys/dbora
        ;;

    'stop')
        # Stop the Oracle databases:
        # The following command assumes that the Oracle sign-in
        # will not prompt the user for any values.
        su - $ORA_OWNER -c "$ORA_HOME/bin/dbshut $ORA_HOME" &
        rm -f /var/lock/subsys/dbora
        ;;
    esac
    ```

4.  Ändern Sie die Berechtigungen:

    ```bash
    # chgrp dba /etc/init.d/dbora
    # chmod 750 /etc/init.d/dbora
    ```

5.  Erstellen Sie symbolische Verknüpfungen für das Starten und Beenden:

    ```bash
    # ln -s /etc/init.d/dbora /etc/rc.d/rc0.d/K01dbora
    # ln -s /etc/init.d/dbora /etc/rc.d/rc3.d/S99dbora
    # ln -s /etc/init.d/dbora /etc/rc.d/rc5.d/S99dbora
    ```

6.  Starten Sie den virtuellen Computer neu, um die Änderungen zu testen:

    ```bash
    # reboot
    ```

## <a name="open-ports-for-connectivity"></a>Öffnen der Ports für Konnektivität

Abschließend müssen einige externe Endpunkte konfiguriert werden. Sie müssen zuerst die SSH-Sitzung auf dem virtuellen Computer beenden, um die Azure-Netzwerksicherheitsgruppe einzurichten, die den virtuellen Computer schützt. 

1.  Führen Sie folgenden Befehl aus, um den Endpunkt zu öffnen, den Sie für den Remote-Zugriff auf die Oracle-Datenbank verwenden: 

    ```azurecli
    az network nsg rule create --resource-group myResourceGroup\
        --nsg-name myVmNSG --name allow-oracle\
        --protocol tcp --direction inbound --priority 999 \
        --source-address-prefix '*' --source-port-range '*' \
        --destination-address-prefix '*' --destination-port-range 1521 --access allow
    ```

    Das Ergebnis sollte dem folgenden Beispiel ähneln:

    ```
    {
    "access": "Allow",
    "description": null,
    "destinationAddressPrefix": "*",
    "destinationPortRange": "1521",
    "direction": "Inbound",
    "etag": "W/\"bd77dcae-e5fd-4bd6-a632-26045b646414\"",
    "id": "/subscriptions/<subscription-id>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myVmNSG/securityRules/allow-oracle",
    "name": "allow-oracle",
    "priority": 999,
    "protocol": "Tcp",
    "provisioningState": "Succeeded",
    "resourceGroup": "myResourceGroup",
    "sourceAddressPrefix": "*",
    "sourcePortRange": "*"
    }
    ```

2.  Führen Sie folgenden Befehl aus, um den Endpunkt zu öffnen, den Sie für den Remote-Zugriff auf Oracle EM Express verwenden:

    ```azurecli
    az network nsg rule create --resource-group myResourceGroup\
        --nsg-name myVmNSG --name allow-oracle-EM\
        --protocol tcp --direction inbound --priority 1001 \
        --source-address-prefix '*' --source-port-range '*' \
        --destination-address-prefix '*' --destination-port-range 5502 --access allow
    ```

    Das Ergebnis sollte dem folgenden Beispiel ähneln:

    ```azurecli
    {
    "access": "Allow",
    "description": null,
    "destinationAddressPrefix": "*",
    "destinationPortRange": "5502",
    "direction": "Inbound",
    "etag": "W/\"06c68b5e-1b3f-4ae0-bcf6-59b3b981d685\"",
    "id": "/subscriptions/2dad32d6-b188-49e6-9437-ca1d51cec4dd/resourceGroups/kennyRG/providers/Microsoft.Network/networkSecurityGroups/kennyVM1NSG/securityRules/allow-oracle-EM",
    "name": "allow-oracle-EM",
    "priority": 1001,
    "protocol": "Tcp",
    "provisioningState": "Succeeded",
    "resourceGroup": "myResourceGroup",
    "sourceAddressPrefix": "*",
    "sourcePortRange": "*"
    }
    ```

3.  Stellen Sie eine Verbindung mit EM Express aus Ihrem Browser her: 

    ```
    https://<VM hostname>:5502/em
    ```

Sie können sich über das SYS-Konto mit dem während der Installation festgelegten Kennwort anmelden.


## <a name="delete-the-vm"></a>Löschen des virtuellen Computers

Wenn Sie den virtuellen Computer nicht mehr benötigen, können Sie mit dem folgenden Befehl die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen entfernen:

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

[Tutorial: Erstellen eines hoch verfügbaren virtuellen Computers](../../linux/create-cli-complete.md)

[Erkunden der Azure-CLI-Beispiele für die Bereitstellung virtueller Computer](../../linux/cli-samples.md)

