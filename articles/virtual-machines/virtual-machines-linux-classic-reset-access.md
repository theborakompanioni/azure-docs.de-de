---
title: "Zurücksetzen des Kennworts und des SSH-Schlüssels für einen virtuellen Linux-Computer über die Befehlszeilenschnittstelle | Microsoft Docs"
description: "Informationen zum Verwenden der VMAccess-Erweiterung über die Azure-Befehlszeilenschnittstelle (CLI) zum Zurücksetzen eines Kennworts oder eines SSH-Schlüssels für einen virtuellen Linux-Computer, zum Beheben von SSH-Konfigurationsproblemen und zum Überprüfen der Datenträgerkonsistenz"
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: d975eb70-5ff1-40d1-a634-8dd2646dcd17
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/16/2016
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: ccd4dda3d7077c9884331c7bfa8021ade398ea42
ms.lasthandoff: 03/21/2017


---
# <a name="how-to-reset-a-linux-vm-password-or-ssh-key-fix-the-ssh-configuration-and-check-disk-consistency-using-the-vmaccess-extension"></a>Zurücksetzen eines Kennworts oder eines SSH-Schlüssels für einen virtuellen Linux-Computer, Beheben von SSH-Konfigurationsproblemen und Überprüfen der Datenträgerkonsistenz mithilfe der VMAccess-Erweiterung
Wenn Sie aufgrund eines vergessenen Kennworts, eines falschen SSH-Schlüssels (Secure Shell) oder eines Problems bei der SSH-Konfiguration keine Verbindung mit einem virtuellen Linux-Computer herstellen können, haben Sie die Möglichkeit, das Kennwort oder den SSH-Schlüssel zurückzusetzen, SSH-Konfigurationsprobleme zu beheben und die Datenträgerkonsistenz zu überprüfen. Verwenden Sie dazu die VMAccessForLinux-Erweiterung mit der Azure-Befehlszeilenschnittstelle. 

> [!IMPORTANT] 
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells. Erfahren Sie, wie Sie [diese Schritte mit dem Resource Manager-Modell ausführen](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).

Mit der Azure-Befehlszeilenschnittstelle verwenden Sie den Befehl **azure vm extension set** in Ihrer Befehlszeilenschnittstelle (Bash, Terminal, Eingabeaufforderung), um auf Befehle zuzugreifen. Informationen zur Nutzung der Erweiterung erhalten Sie durch Ausführen von **azure help vm extension set** .

Mit der Azure-CLI können Sie die folgenden Aufgaben ausführen:

* [Zurücksetzen des Kennworts](#pwresetcli)
* [Zurücksetzen des SSH-Schlüssels](#sshkeyresetcli)
* [Zurücksetzen des Kennworts und des SSH-Schlüssels](#resetbothcli)
* [Erstellen eines neuen Benutzerkontos in sudo](#createnewsudocli)
* [Zurücksetzen der SSH-Konfiguration](#sshconfigresetcli)
* [Löschen eines Benutzers](#deletecli)
* [Anzeigen des Status der VMAccess-Erweiterung](#statuscli)
* [Überprüfen der Konsistenz von hinzugefügten Datenträgern](#checkdisk)
* [Reparieren von hinzugefügten Datenträgern auf Ihrer Linux-VM](#repairdisk)

## <a name="prerequisites"></a>Voraussetzungen
Sie müssen die folgenden Schritte ausführen:

* Außerdem müssen Sie die [Azure-Befehlszeilenschnittstelle installieren](../cli-install-nodejs.md) und für die Verwendung der mit Ihrem Konto verknüpften Azure-Ressourcen eine [Verbindung mit Ihrem Abonnement herstellen](../xplat-cli-connect.md).
* Legen Sie den richtigen Modus für das klassische Bereitstellungsmodell fest, indem Sie an der Eingabeaufforderung Folgendes eingeben:
    ``` 
        azure config mode asm
    ```
* Stellen Sie ein neues Kennwort oder neue SSH-Schlüssel bereit, wenn Sie diese zurücksetzen möchten. Dies ist nicht erforderlich, wenn Sie die SSH-Konfiguration zurücksetzen möchten.

## <a name="pwresetcli"></a>Zurücksetzen des Kennworts
1. Erstellen Sie auf dem lokalen Computer eine Datei namens „PrivateConf.json“ mit diesen Zeilen. Ersetzen Sie **myUserName** und **myP@ssW0rd** durch Ihren eigenen Benutzernamen und Ihr Kennwort, und legen Sie ein eigenes Ablaufdatum fest.

    ```   
        {
        "username":"myUserName",
        "password":"myP@ssW0rd",
        "expiration":"2020-01-01"
        }
    ```
        
2. Führen Sie diesen Befehl aus, und ersetzen Sie dabei **myVM** durch den Namen des virtuellen Computers.

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* –-private-config-path PrivateConf.json
    ```

## <a name="sshkeyresetcli"></a>Zurücksetzen des SSH-Schlüssels
1. Erstellen Sie eine Datei namens „PrivateConf.json“ mit diesen Inhalten. Ersetzen Sie die Werte **myUserName** und **mySSHKey** durch Ihre eigenen Angaben.

    ```   
        {
        "username":"myUserName",
        "ssh_key":"mySSHKey"
        }
    ```
2. Führen Sie diesen Befehl aus, und ersetzen Sie dabei **myVM** durch den Namen des virtuellen Computers.
   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="resetbothcli"></a>Zurücksetzen des Kennworts und des SSH-Schlüssels
1. Erstellen Sie eine Datei namens „PrivateConf.json“ mit diesen Inhalten. Ersetzen Sie die Werte **myUserName**, **mySSHKey** und **myP@ssW0rd** durch Ihre eigenen Angaben.

    ``` 
        {
        "username":"myUserName",
        "ssh_key":"mySSHKey",
        "password":"myP@ssW0rd"
        }
    ```

2. Führen Sie diesen Befehl aus, und ersetzen Sie dabei **myVM** durch den Namen des virtuellen Computers.

    ```   
        azure vm extension set MyVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json
    ```

## <a name="createnewsudocli"></a>Erstellen eines neuen Benutzerkontos in sudo

Wenn Sie Ihren Benutzernamen vergessen haben, können Sie mit VMAccess einen neuen mit Berechtigung für sudo erstellen. Der vorhandene Benutzername und das Kennwort werden in diesem Fall nicht geändert.

Um einen neuen sudo-Benutzer mit Kennwortzugriff zu erstellen, verwenden Sie das Skript in [Zurücksetzen des Kennworts](#pwresetcli) und geben den neuen Benutzernamen an.

Um einen neuen sudo-Benutzer mit SSH-Schlüsselzugriff zu erstellen, verwenden Sie das Skript in [Zurücksetzen des SSH-Schlüssels](#sshkeyresetcli) und geben den neuen Benutzernamen an.

Sie können auch [Zurücksetzen des Kennworts und des SSH-Schlüssels](#resetbothcli) verwenden, um einen neuen Benutzer mit Kennwort- und SSH-Schlüsselzugriff zu erstellen.

## <a name="sshconfigresetcli"></a>Zurücksetzen der SSH-Konfiguration
Wenn sich die SSH-Konfiguration in einen unerwünschten Zustand befindet, verlieren Sie möglicherweise auch den Zugriff auf den virtuellen Computer. Mit der VMAccess-Erweiterung können Sie die Konfiguration auf den Standardzustand zurücksetzen. Hierzu legen Sie einfach nur den Schlüssel "reset_ssh" auf "True" fest. Die Erweiterung startet den SSH-Server neu, öffnet den SSH-Port auf Ihrem virtuellen Computer und setzt die SSH-Konfiguration auf die Standardeinstellungen zurück. Das Benutzerkonto (Name, Kennwort oder SSH-Schlüssel) wird nicht geändert.

> [!NOTE]
> Die SSH-Konfigurationsdatei befindet sich unter "/etc/ssh/sshd_config".
> 
> 

1. Erstellen Sie eine Datei namens „PrivateConf.json“ mit diesem Inhalt.

    ```   
        {
        "reset_ssh":"True"
        }
    ```

2. Führen Sie diesen Befehl aus, und ersetzen Sie dabei **myVM** durch den Namen des virtuellen Computers. 

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json
    ```

## <a name="deletecli"></a>Löschen eines Benutzers
Wenn Sie ein Benutzerkonto löschen möchten, ohne sich direkt auf dem virtuellen Computer anzumelden, können Sie dieses Skript verwenden.

1. Erstellen Sie eine Datei namens „PrivateConf.json“ mit diesem Inhalt, und ersetzen Sie dabei **removeUserName** durch den Benutzernamen, den Sie entfernen möchten. 

    ```   
        {
        "remove_user":"removeUserName"
        }
    ```

2. Führen Sie diesen Befehl aus, und ersetzen Sie dabei **myVM** durch den Namen des virtuellen Computers. 

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json
    ```

## <a name="statuscli"></a>Anzeigen des Status der VMAccess-Erweiterung
Führen Sie zum Anzeigen des Status der VMAccess-Erweiterung diesen Befehl aus.

```
        azure vm extension get
```

## <a name='checkdisk'></a>Überprüfen der Konsistenz von hinzugefügten Datenträgern
Gehen Sie wie folgt vor, um fsck auf allen Datenträgern Ihrer virtuellen Linux-Maschine auszuführen:

1. Erstellen Sie eine Datei namens „PublicConf.json“ mit diesem Inhalt. Die Datenträgerüberprüfung ermittelt anhand eines booleschen Werts, ob an die virtuelle Maschine angefügte Datenträger überprüft werden sollen. 

    ```   
        {   
        "check_disk": "true"
        }
    ```

2. Führen Sie diesen Befehl aus, und ersetzen Sie dabei **myVM** durch den Namen des virtuellen Computers.

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json 
    ```

## <a name='repairdisk'></a>Reparieren von Datenträgern
Verwenden Sie zum Reparieren von Datenträgern, die nicht bereitgestellt werden können oder für die Fehler bei der Bereitstellungskonfiguration angezeigt werden, die VMAccess-Erweiterung, um die Bereitstellungskonfiguration Ihres virtuellen Linux-Computers zurückzusetzen. Ersetzen Sie **myDisk** durch den Namen Ihres Datenträgers.

1. Erstellen Sie eine Datei namens „PublicConf.json“ mit diesem Inhalt. 

    ```   
        {
        "repair_disk":"true",
        "disk_name":"myDisk"
        }
    ```

2. Führen Sie diesen Befehl aus, und ersetzen Sie dabei **myVM** durch den Namen des virtuellen Computers.

    ```   
        azure vm extension set myVM VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json
    ```

## <a name="next-steps"></a>Nächste Schritte
* Wenn Sie Azure PowerShell-Cmdlets oder Azure Resource Manager-Vorlagen verwenden möchten, um das Kennwort oder den SSH-Schlüssel zurückzusetzen, SSH-Konfigurationsprobleme zu beheben und die Datenträgerkonsistenz zu überprüfen, lesen Sie die [Dokumentation zur VMAccess-Erweiterung auf GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess). 
* Sie können auch das [Azure-Portal](https://portal.azure.com) nutzen, um das Kennwort oder den SSH-Schlüssel eines virtuellen Linux-Computers zurückzusetzen, der im klassischen Bereitstellungsmodell bereitgestellt wurde. Derzeit können Sie das Portal nicht verwenden, um dies für einen virtuellen Linux-Computer durchzuführen, der im Resource Manager-Bereitstellungsmodell bereitgestellt wurde.
* Weitere Informationen zur Verwendung von VM-Erweiterungen für virtuelle Azure-Computer finden Sie unter [Informationen zu Erweiterungen und Features für virtuelle Computer](virtual-machines-linux-extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) .


