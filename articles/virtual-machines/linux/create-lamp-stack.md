---
title: Bereitstellen von LAMP auf einem virtuellen Linux-Computer in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den LAMP-Stapel auf einem virtuellen Linux-Computer in Azure installieren.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: jluk
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 6c12603a-e391-4d3e-acce-442dd7ebb2fe
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 2/21/2017
ms.author: juluk
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 174490bec1aea20abf2c2bd465c7aa2c8590e3e2
ms.lasthandoff: 04/03/2017


---
# <a name="deploy-lamp-stack-on-azure"></a>Bereitstellen des LAMP-Stapels in Azure
In diesem Artikel werden Sie durch die Bereitstellung eines Apache-Webservers sowie von MySQL und PHP (LAMP-Stapel) in Azure geführt. Sie benötigen ein Azure-Konto ([kostenlose Testversion abrufen](https://azure.microsoft.com/pricing/free-trial/)) und die [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2). Sie können diese Schritte auch mit der [Azure CLI 1.0](create-lamp-stack-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ausführen.

## <a name="quick-command-summary"></a>Kurze Zusammenfassung der Befehle

1. Speichern Sie die Datei [azuredeploy.parameters.json](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) in einem frei wählbaren Verzeichnis auf Ihrem lokalen Computer.
2. Führen Sie die beiden folgenden Befehle aus, um eine Ressourcengruppe zu erstellen und dann Ihre Vorlage bereitzustellen:

```azurecli
az group create -l westus -n myResourceGroup
az group deployment create -g myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json \
    --parameters @filepathToParameters.json
```

### <a name="deploy-lamp-on-existing-vm"></a>Bereitstellen von LAMP auf einem vorhandenen virtuellen Computer
Die folgenden Befehle aktualisieren die Pakete und installieren dann Apache, MySQL und PHP:

```bash
sudo apt-get update
sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a>Bereitstellen von LAMP auf einem neuen virtuellen Computer – exemplarische Vorgehensweise

1. Erstellen Sie mit [az group create](/cli/azure/group#create) eine Ressourcengruppe, die den neuen virtuellen Computer enthalten soll:

```azurecli
az group create -l westus -n myResourceGroup
```
Zum Erstellen der eigentlichen VM können Sie eine bereits geschriebene Azure Resource Manager-Vorlage verwenden, die [hier bei GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app)erhältlich ist.

2. Speichern Sie die Datei [azuredeploy.parameters.json](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) auf Ihrem lokalen Computer.
3. Bearbeiten Sie die Datei **azuredeploy.parameters.json**, und geben Sie die von Ihnen bevorzugten Werte an.
4. Stellen Sie die Vorlage mit [az group deployment create] bereit, und verweisen Sie auf die heruntergeladene JSON-Datei:

```azurecli
az group deployment create -g myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json \
    --parameters @filepathToParameters.json
```

Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

```json
{
"id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroup/providers/Microsoft.Resources/deployments/azuredeploy",
"name": "azuredeploy",
"properties": {
    "correlationId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "debugSetting": null,
}
...
"provisioningState": "Succeeded",
"template": null,
"templateLink": {
    "contentVersion": "1.0.0.0",
    "uri": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json"
    },
    "timestamp": "2017-02-22T00:05:51.860411+00:00"
},
"resourceGroup": "myResourceGroup"
}
```

Sie haben nun eine Linux-VM erstellt, auf der LAMP bereits installiert ist. Bei Bedarf können Sie die Installation überprüfen, indem Sie die Schritte zum [Überprüfen der erfolgreichen LAMP-Installation](#verify-lamp-successfully-installed) unten in diesem Artikel ausführen.

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a>Bereitstellen von LAMP auf einem vorhandenen virtuellen Computer – exemplarische Vorgehensweise
Falls Sie beim Erstellen eines virtuellen Linux-Computers Hilfe benötigen, helfen Ihnen diese [Informationen zum Erstellen eines virtuellen Linux-Computers](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-quick-create-cli) weiter. Als Nächstes müssen Sie eine SSH-Verbindung mit dem virtuellen Linux-Computer herstellen. Falls Sie beim Erstellen eines SSH-Schlüssels Hilfe benötigen, lesen Sie diese [Informationen zum Erstellen eines SSH-Schlüssels unter Linux/Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
Wenn Sie bereits über einen SSH-Schlüssel verfügen, können Sie mit `ssh azureuser@mypublicdns.westus.cloudapp.azure.com` eine SSH-Verbindung mit dem virtuellen Linux-Computer herstellen.

Da Sie jetzt auf dem virtuellen Linux-Computer arbeiten, folgt nun die exemplarische Vorgehensweise zur Installation des LAMP-Stapels in Debian-basierten Distributionen. Die Befehle können für andere Linux-Distributionen jeweils variieren.

#### <a name="installing-on-debianubuntu"></a>Installation unter Debian/Ubuntu
Die folgenden Pakete müssen installiert sein: `apache2`, `mysql-server`, `php5` und `php5-mysql`. Sie können diese Pakete installieren, indem Sie direkt darauf zugreifen oder Tasksel verwenden.
Vor der Installation müssen Sie die Paketlisten herunterladen und aktualisieren.

```bash
sudo apt-get update
```

##### <a name="individual-packages"></a>Einzelne Pakete
Verwenden von apt-get:

```bash
sudo apt-get install apache2 mysql-server php5 php5-mysql
```

##### <a name="using-tasksel"></a>Verwenden von Tasksel
Alternativ dazu können Sie Tasksel herunterladen. Dabei handelt es sich um ein Debian/Ubuntu-Tool, das mehrere in Beziehung stehende Pakete als koordinierten „Task“ auf Ihrem System installiert.

```bash
sudo apt-get install tasksel
sudo tasksel install lamp-server
```

Nach der Ausführung einer der oben genannten Optionen werden Sie aufgefordert, diese Pakete und eine Reihe weiterer Abhängigkeiten zu installieren. Um ein Administratorkennwort für MySQL festzulegen, drücken Sie „y“ und dann die EINGABETASTE, und befolgen Sie alle weiteren Aufforderungen. Dieser Prozess installiert die PHP-Erweiterungen, die mindestens für die Verwendung von PHP mit MySQL erforderlich sind. 

![][1]

Führen Sie folgenden Befehl aus, um weitere PHP-Erweiterungen anzuzeigen, die als Pakete verfügbar sind:

```bash
apt-cache search php5
```

#### <a name="create-infophp-document"></a>Erstellen des Dokuments „info.php“
Sie sollten jetzt in der Befehlszeile überprüfen können, welche Version von Apache, MySQL und PHP Sie verwenden, indem Sie `apache2 -v`, `mysql -v` oder `php -v` eingeben.

Falls Sie weitere Tests durchführen möchten, können Sie schnell eine PHP-Infoseite zum Anzeigen in einem Browser erstellen. Erstellen Sie mit dem Nano-Text-Editor und diesem Befehl eine neue Datei:

```bash
sudo nano /var/www/html/info.php
```

Fügen Sie im GNU Nano-Text-Editor die folgenden Zeilen hinzu:

```php
<?php
phpinfo();
?>
```

Speichern und beenden Sie anschließend den Text-Editor.

Starten Sie Apache mit diesem Befehl neu, damit alle neuen Installationen wirksam werden.

```bash
sudo service apache2 restart
```

## <a name="verify-lamp-successfully-installed"></a>Überprüfen der erfolgreichen LAMP-Installation
Sie können jetzt die von Ihnen erstellte PHP-Informationsseite überprüfen, indem Sie einen Browser öffnen und zu „http://IhrDNS/info.php“ wechseln. Die Seite sollte ähnlich wie diese Abbildung aussehen.

![][2]

Sie können Ihre Apache-Installation überprüfen, indem Sie unter http://youruniqueDNS/info.php die Apache2 Ubuntu-Standardseite anzeigen. Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

![][3]

Glückwunsch! Sie haben einen LAMP-Stapel auf Ihrer Azure-VM eingerichtet.

## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich die Ubuntu-Dokumentation zum LAMP-Stapel an:

* [https://help.ubuntu.com/community/ApacheMySQLPHP](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/deploy-lamp-stack/apachesuccesspage.png

