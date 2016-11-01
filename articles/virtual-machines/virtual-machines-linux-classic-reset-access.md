<properties
        pageTitle="Zurücksetzen des Kennworts und des SSH-Schlüssels für einen virtuellen Linux-Computer über die Befehlszeilenschnittstelle | Microsoft Azure"
        description="Informationen zum Verwenden der VMAccess-Erweiterung über die Azure-Befehlszeilenschnittstelle (CLI) zum Zurücksetzen eines Kennworts oder eines SSH-Schlüssels für einen virtuellen Linux-Computer, zum Beheben von SSH-Konfigurationsproblemen und zum Überprüfen der Datenträgerkonsistenz"
        services="virtual-machines-linux"
        documentationCenter=""
        authors="cynthn"
        manager="timlt"
        editor=""
        tags="azure-service-management"/>

<tags
        ms.service="virtual-machines-linux"
        ms.workload="infrastructure-services"
        ms.tgt_pltfrm="vm-linux"
        ms.devlang="na"
        ms.topic="article"
        ms.date="06/14/2016"
        ms.author="cynthn"/>

# Zurücksetzen eines Kennworts oder eines SSH-Schlüssels für einen virtuellen Linux-Computer, Beheben von SSH-Konfigurationsproblemen und Überprüfen der Datenträgerkonsistenz mithilfe der VMAccess-Erweiterung


Wenn Sie aufgrund eines vergessenen Kennworts, eines falschen SSH-Schlüssels (Secure Shell) oder eines Problems bei der SSH-Konfiguration keine Verbindung mit einem virtuellen Linux-Computer herstellen können, haben Sie die Möglichkeit, das Kennwort oder den SSH-Schlüssel zurückzusetzen, SSH-Konfigurationsprobleme zu beheben und die Datenträgerkonsistenz zu überprüfen. Verwenden Sie dazu die VMAccessForLinux-Erweiterung mit der Azure-Befehlszeilenschnittstelle.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] Erfahren Sie, wie Sie [diese Schritte mit dem Resource Manager-Modell ausführen](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).

Mit der Azure-Befehlszeilenschnittstelle verwenden Sie den Befehl **azure vm extension set** in Ihrer Befehlszeilenschnittstelle (Bash, Terminal, Eingabeaufforderung), um auf Befehle zuzugreifen. Informationen zur Nutzung der Erweiterung erhalten Sie durch Ausführen von **azure help vm extension set**.

Mit der Azure-CLI können Sie die folgenden Aufgaben ausführen:

+ [Zurücksetzen des Kennworts](#pwresetcli)
+ [Zurücksetzen des SSH-Schlüssels](#sshkeyresetcli)
+ [Zurücksetzen des Kennworts und des SSH-Schlüssels](#resetbothcli)
+ [Erstellen eines neuen Benutzerkontos in sudo](#createnewsudocli)
+ [Zurücksetzen der SSH-Konfiguration](#sshconfigresetcli)
+ [Löschen eines Benutzers](#deletecli)
+ [Anzeigen des Status der VMAccess-Erweiterung](#statuscli)
+ [Überprüfen der Konsistenz von hinzugefügten Datenträgern](#checkdisk)
+ [Reparieren von hinzugefügten Datenträgern auf Ihrer Linux-VM](#repairdisk)


## Voraussetzungen

Sie müssen die folgenden Schritte ausführen:

- Sie müssen [die Azure-Befehlszeilenschnittstelle installieren](../xplat-cli-install.md) und [eine Verbindung mit Ihrem Abonnement herstellen](../xplat-cli-connect.md), um die mit Ihrem Konto verknüpften Azure-Ressourcen zu verwenden.
- Legen Sie den richtigen Modus für das klassische Bereitstellungsmodell fest, indem Sie an der Eingabeaufforderung Folgendes eingeben:
        
        azure config mode asm
        
- Stellen Sie ein neues Kennwort oder neue SSH-Schlüssel bereit, wenn Sie diese zurücksetzen möchten. Dies ist nicht erforderlich, wenn Sie die SSH-Konfiguration zurücksetzen möchten.


## <a name="pwresetcli"></a>Zurücksetzen des Kennworts

1. Erstellen Sie eine Datei namens „PrivateConf.json“ mit diesen Zeilen. Ersetzen Sie die Klammern und &#60;Platzhalterwerte&#62; durch Ihre eigenen Informationen.

        {
        "username":"<currentusername>",
        "password":"<newpassword>",
        "expiration":"<2016-01-01>"
        }

2. Führen Sie diesen Befehl aus, und ersetzen Sie dabei &#60;vm-name&#62; durch den Namen des virtuellen Computers.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* –-private-config-path PrivateConf.json

## <a name="sshkeyresetcli"></a>Zurücksetzen des SSH-Schlüssels

1. Erstellen Sie eine Datei namens „PrivateConf.json“ mit diesen Inhalten. Ersetzen Sie die Klammern und &#60;Platzhalterwerte&#62; durch Ihre eigenen Informationen.

        {
        "username":"<currentusername>",
        "ssh_key":"<contentofsshkey>"
        }

2. Führen Sie diesen Befehl aus, und ersetzen Sie dabei &#60;vm-name&#62; durch den Namen des virtuellen Computers.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="resetbothcli"></a>Zurücksetzen des Kennworts und des SSH-Schlüssels

1. Erstellen Sie eine Datei namens „PrivateConf.json“ mit diesen Inhalten. Ersetzen Sie die Klammern und &#60;Platzhalterwerte&#62; durch Ihre eigenen Informationen.

        {
        "username":"<currentusername>",
        "ssh_key":"<contentofsshkey>",
        "password":"<newpassword>"
        }

2. Führen Sie diesen Befehl aus, und ersetzen Sie dabei &#60;vm-name&#62; durch den Namen des virtuellen Computers.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="createnewsudocli"></a>Erstellen eines neuen Benutzerkontos in sudo

Wenn Sie Ihren Benutzernamen vergessen haben, können Sie mit VMAccess einen neuen mit Berechtigung für sudo erstellen. Der vorhandene Benutzername und das Kennwort werden in diesem Fall nicht geändert.

Um einen neuen sudo-Benutzer mit Kennwortzugriff zu erstellen, verwenden Sie das Skript in [Zurücksetzen des Kennworts](#pwresetcli) und geben den neuen Benutzernamen an.

Um einen neuen sudo-Benutzer mit SSH-Schlüsselzugriff zu erstellen, verwenden Sie das Skript in [Zurücksetzen des SSH-Schlüssels](#sshkeyresetcli) und geben den neuen Benutzernamen an.

Sie können auch [Zurücksetzen des Kennworts und des SSH-Schlüssels](#resetbothcli) verwenden, um einen neuen Benutzer mit Kennwort- und SSH-Schlüsselzugriff zu erstellen.

## <a name="sshconfigresetcli"></a>Zurücksetzen der SSH-Konfiguration

Wenn sich die SSH-Konfiguration in einen unerwünschten Zustand befindet, verlieren Sie möglicherweise auch den Zugriff auf den virtuellen Computer. Mit der VMAccess-Erweiterung können Sie die Konfiguration auf den Standardzustand zurücksetzen. Hierzu legen Sie einfach nur den Schlüssel "reset\_ssh" auf "True" fest. Die Erweiterung startet den SSH-Server neu, öffnet den SSH-Port auf Ihrem virtuellen Computer und setzt die SSH-Konfiguration auf die Standardeinstellungen zurück. Das Benutzerkonto (Name, Kennwort oder SSH-Schlüssel) wird nicht geändert.

> [AZURE.NOTE] Die SSH-Konfigurationsdatei befindet sich unter "/etc/ssh/sshd\_config".

1. Erstellen Sie eine Datei namens „PrivateConf.json“ mit diesem Inhalt.

        {
        "reset_ssh":"True"
        }

2. Führen Sie diesen Befehl aus, und ersetzen Sie dabei &#60;vm-name&#62; durch den Namen des virtuellen Computers.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="deletecli"></a>Löschen eines Benutzers

Wenn Sie ein Benutzerkonto löschen möchten, ohne sich direkt auf dem virtuellen Computer anzumelden, können Sie dieses Skript verwenden.

1. Erstellen Sie eine Datei namens „PrivateConf.json“ mit diesem Inhalt, und ersetzen Sie &#60;usernametoremove&#62; durch den Benutzernamen, den Sie entfernen möchten.

        {
        "remove_user":"<usernametoremove>"
        }

2. Führen Sie diesen Befehl aus, und ersetzen Sie dabei &#60;vm-name&#62; durch den Namen des virtuellen Computers.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="statuscli"></a>Anzeigen des Status der VMAccess-Erweiterung

Führen Sie zum Anzeigen des Status der VMAccess-Erweiterung diesen Befehl aus.

        azure vm extension get

## <a name='checkdisk'<</a>Überprüfen der Konsistenz von hinzugefügten Datenträgern

Gehen Sie wie folgt vor, um fsck auf allen Datenträgern Ihrer virtuellen Linux-Maschine auszuführen:

1. Erstellen Sie eine Datei namens „PublicConf.json“ mit diesem Inhalt. Die Datenträgerüberprüfung ermittelt anhand eines booleschen Werts, ob an die virtuelle Maschine angefügte Datenträger überprüft werden sollen.

        {   
        "check_disk": "true"
        }

2. Führen Sie diesen Befehl aus, und ersetzen Sie dabei &#60;vm-name&#62; durch den Namen des virtuellen Computers.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json 

## <a name='repairdisk'></a>Reparieren von Datenträgern 

Verwenden Sie zum Reparieren von Datenträgern, die nicht bereitgestellt werden können oder für die Fehler bei der Bereitstellungskonfiguration angezeigt werden, die VMAccess-Erweiterung, um die Bereitstellungskonfiguration Ihres virtuellen Linux-Computers zurückzusetzen. Ersetzen Sie &#60;yourdisk&#62; durch den Namen Ihres Datenträgers.

1. Erstellen Sie eine Datei namens „PublicConf.json“ mit diesem Inhalt.

        {
        "repair_disk":"true",
        "disk_name":"<yourdisk>"
        }

2. Führen Sie diesen Befehl aus, und ersetzen Sie dabei &#60;vm-name&#62; durch den Namen des virtuellen Computers.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json



## Nächste Schritte

* Wenn Sie Azure PowerShell-Cmdlets oder Azure Resource Manager-Vorlagen verwenden möchten, um das Kennwort oder den SSH-Schlüssel zurückzusetzen, SSH-Konfigurationsprobleme zu beheben und die Datenträgerkonsistenz zu überprüfen, lesen Sie die [Dokumentation zur VMAccess-Erweiterung auf GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).

* Sie können auch das [Azure-Portal](https://portal.azure.com) nutzen, um das Kennwort oder den SSH-Schlüssel eines virtuellen Linux-Computers zurückzusetzen, der im klassischen Bereitstellungsmodell bereitgestellt wurde. Derzeit können Sie das Portal nicht verwenden, um dies für einen virtuellen Linux-Computer durchzuführen, der im Resource Manager-Bereitstellungsmodell bereitgestellt wurde.

* Weitere Informationen zur Verwendung von VM-Erweiterungen für virtuelle Azure-Computer finden Sie unter [Informationen zu Erweiterungen und Features für virtuelle Computer](virtual-machines-linux-extensions-features.md).

<!---HONumber=AcomDC_0629_2016-->