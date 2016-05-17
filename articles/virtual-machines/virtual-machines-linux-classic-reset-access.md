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
	ms.date="04/20/2016"
	ms.author="cynthn"/>

# Zurücksetzen des Zugriffs, Verwalten von Benutzern und Überprüfen von Datenträgern mit der Azure-VMAccess-Erweiterung für Linux


Wenn Sie aufgrund eines vergessenen Kennworts, eines falschen SSH-Schlüssels (Secure Shell) oder eines Problems bei der SSH-Konfiguration keine Verbindung mit einem virtuellen Linux-Computer herstellen können, haben Sie die Möglichkeit, das Kennwort oder den SSH-Schlüssel zurückzusetzen, SSH-Konfigurationsprobleme zu beheben und die Datenträgerkonsistenz zu überprüfen. Verwenden Sie dazu die VMAccessForLinux-Erweiterung mit der Azure-Befehlszeilenschnittstelle.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] [Resource Manager model](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).




## Azure-Befehlszeilenschnittstelle

Sie benötigen Folgendes:

- Azure-Befehlszeilenschnittstelle (CLI). Sie müssen [die Azure-Befehlszeilenschnittstelle installieren](../xplat-cli-install.md) und [eine Verbindung mit Ihrem Abonnement herstellen](../xplat-cli-connect.md), um die mit Ihrem Konto verknüpften Azure-Ressourcen zu verwenden.
- Ein neues Kennwort oder neue SSH-Schlüssel, wenn Sie diese zurücksetzen möchten. Dies ist nicht erforderlich, wenn Sie die SSH-Konfiguration zurücksetzen möchten.


## Verwenden des Befehls „azure vm extension set“

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

### <a name="pwresetcli"></a>Zurücksetzen des Kennworts

Schritt 1: Erstellen Sie eine Datei namens PrivateConf.json mit den folgenden Inhalten, und ersetzen Sie dabei die Platzhalter-Werte.

	{
	"username":"currentusername",
	"password":"newpassword",
	"expiration":"2016-01-01"
	}

Schritt 2: Führen Sie diesen Befehl aus, und ersetzen Sie dabei "vmname" durch den Namen des virtuellen Computers.

	azure vm extension set vmname VMAccessForLinux Microsoft.OSTCExtensions 1.* –-private-config-path PrivateConf.json

### <a name="sshkeyresetcli"></a>Zurücksetzen des SSH-Schlüssels

Schritt 1: Erstellen Sie eine Datei namens PrivateConf.json mit den folgenden Inhalten, und ersetzen Sie dabei die Platzhalter-Werte.

	{
	"username":"currentusername",
	"ssh_key":"contentofsshkey"
	}

Schritt 2: Führen Sie diesen Befehl aus, und ersetzen Sie dabei "vmname" durch den Namen des virtuellen Computers.

	azure vm extension set vmname VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

### <a name="resetbothcli"></a>Zurücksetzen des Kennworts und des SSH-Schlüssels

Schritt 1: Erstellen Sie eine Datei namens PrivateConf.json mit den folgenden Inhalten, und ersetzen Sie dabei die Platzhalter-Werte.

	{
	"username":"currentusername",
	"ssh_key":"contentofsshkey",
	"password":"newpassword"
	}

Schritt 2: Führen Sie diesen Befehl aus, und ersetzen Sie dabei "vmname" durch den Namen des virtuellen Computers.

	azure vm extension set vmname VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

### <a name="createnewsudocli"></a>Erstellen eines neuen Benutzerkontos in sudo

Wenn Sie Ihren Benutzernamen vergessen haben, können Sie mit VMAccess einen neuen mit Berechtigung für sudo erstellen. Der vorhandene Benutzername und das Kennwort werden in diesem Fall nicht geändert.

Um einen neuen sudo-Benutzer mit Kennwortzugriff zu erstellen, verwenden Sie das Skript in [Zurücksetzen des Kennworts](#pwresetcli) und geben den neuen Benutzernamen an.

Um einen neuen sudo-Benutzer mit SSH-Schlüsselzugriff zu erstellen, verwenden Sie das Skript in [Zurücksetzen des SSH-Schlüssels](#sshkeyresetcli) und geben den neuen Benutzernamen an.

Sie können auch [Zurücksetzen des Kennworts und des SSH-Schlüssels](#resetbothcli) verwenden, um einen neuen Benutzer mit Kennwort- und SSH-Schlüsselzugriff zu erstellen.

### <a name="sshconfigresetcli"></a>Zurücksetzen der SSH-Konfiguration

Wenn sich die SSH-Konfiguration in einen unerwünschten Zustand befindet, verlieren Sie möglicherweise auch den Zugriff auf den virtuellen Computer. Mit der VMAccess-Erweiterung können Sie die Konfiguration auf den Standardzustand zurücksetzen. Hierzu legen Sie einfach nur den Schlüssel "reset\_ssh" auf "True" fest. Die Erweiterung startet den SSH-Server neu, öffnet den SSH-Port auf Ihrem virtuellen Computer und setzt die SSH-Konfiguration auf die Standardeinstellungen zurück. Das Benutzerkonto (Name, Kennwort oder SSH-Schlüssel) wird nicht geändert.

> [AZURE.NOTE] Die SSH-Konfigurationsdatei befindet sich unter "/etc/ssh/sshd\_config".

Schritt 1: Erstellen Sie eine Datei namens PrivateConf.json mit diesem Inhalt.

	{
	"reset_ssh":"True"
	}

Schritt 2: Führen Sie diesen Befehl aus, und ersetzen Sie dabei "vmname" durch den Namen des virtuellen Computers.

	azure vm extension set vmname VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

### <a name="deletecli"></a>Löschen eines Benutzers

Wenn Sie ein Benutzerkonto löschen möchten, ohne sich direkt auf dem virtuellen Computer anzumelden, können Sie dieses Skript verwenden.

Schritt 1: Erstellen Sie eine Datei namens PrivateConf.json mit folgendem Inhalte, und ersetzen Sie dabei die Platzhalter-Werte.

	{
	"remove_user":"usernametoremove"
	}

Schritt 2: Führen Sie diesen Befehl aus, und ersetzen Sie dabei "vmname" durch den Namen des virtuellen Computers.

	azure vm extension set vmname VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

### <a name="statuscli"></a>Anzeigen des Status der VMAccess-Erweiterung

Führen Sie zum Anzeigen des Status der VMAccess-Erweiterung diesen Befehl aus.

	azure vm extension get

### <a name='checkdisk'<</a>Überprüfen der Konsistenz von hinzugefügten Datenträgern

Gehen Sie wie folgt vor, um fsck auf allen Datenträgern Ihrer virtuellen Linux-Maschine auszuführen:

Schritt 1: Erstellen Sie eine Datei namens „PublicConf.json“ mit diesem Inhalt. Die Datenträgerüberprüfung ermittelt anhand eines booleschen Werts, ob an die virtuelle Maschine angefügte Datenträger überprüft werden sollen.

    {   
    "check_disk": "true"
    }

Schritt 2: Führen Sie den folgenden Befehl aus, und ersetzen Sie dabei die Platzhalterwerte.

   azure vm extension set vm-name VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json

### <a name='repairdisk'></a>Reparieren von hinzugefügten Datenträgern auf Ihrem virtuellen Linux-Computer

Verwenden Sie zum Reparieren von Datenträgern, die nicht bereitgestellt werden können oder für die Fehler bei der Bereitstellungskonfiguration angezeigt werden, die VMAccess-Erweiterung, um die Bereitstellungskonfiguration ihrer virtuellen Linux-Maschine zurückzusetzen.

Schritt 1: Erstellen Sie eine Datei namens „PublicConf.json“ mit diesem Inhalt.

    {
    "repair_disk":"true",
    "disk_name":"yourdisk"
    }

Schritt 2: Führen Sie den folgenden Befehl aus, und ersetzen Sie dabei die Platzhalterwerte.

    azure vm extension set vm-name VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json



## Zusätzliche Ressourcen

* Wenn Sie Azure PowerShell-Cmdlets oder Azure Resource Manager-Vorlagen verwenden möchten, um das Kennwort oder den SSH-Schlüssel zurückzusetzen, SSH-Konfigurationsprobleme zu beheben und die Datenträgerkonsistenz zu überprüfen, lesen Sie die [Dokumentation zur VMAccess-Erweiterung auf GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess). 

* Sie können auch das [Azure-Portal](https://portal.azure.com) nutzen, um das Kennwort oder den SSH-Schlüssel für einen virtuellen Linux-Computers zurückzusetzen, der im klassischen Bereitstellungsmodell bereitgestellt wurde. Derzeit können Sie das Portal nicht verwenden, um dies für einen virtuellen Linux-Computer durchzuführen, der im Resource Manager-Bereitstellungsmodell bereitgestellt wurde.

* Weitere Informationen zur Verwendung von VM-Erweiterungen für virtuelle Azure-Computer finden Sie unter [Informationen zu Erweiterungen und Features für virtuelle Computer](virtual-machines-linux-extensions-features.md).

<!---HONumber=AcomDC_0504_2016-->