---
title: Bereitstellen von LAMP auf einem virtuellen Linux-Computer mithilfe von Azure CLI 1.0 | Microsoft Docs
description: Erfahren Sie, wie Sie LAMP-Stapel auf einem virtuellen Linux-Computer in Azure installieren.
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
ms.devlang: NA
ms.topic: article
ms.date: 2/21/2017
ms.author: juluk
translationtype: Human Translation
ms.sourcegitcommit: 59af3469a5b2d5cca68bf18dca1aa1e3ab684adb
ms.openlocfilehash: 0675b6471e37e89e426df85e2fb696fcff2927fd
ms.lasthandoff: 02/27/2017


---
# <a name="deploy-lamp-stack-with-the-azure-cli-10"></a>Bereitstellen von LAMP-Stapeln mit Azure CLI 1.0
In diesem Artikel werden Sie durch die Bereitstellung eines Apache-Webservers und von MySQL und PHP (LAMP-Stapel) in Azure geführt. Sie benötigen ein Azure-Konto ([kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)) und die [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md), die [mit Ihrem Azure-Konto verbunden](../xplat-cli-connect.md) ist.

## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe
Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

- [Azure CLI 1.0:] Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell (in diesem Artikel)
- [Azure CLI 2.0:](virtual-machines-linux-create-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Unsere CLI der nächsten Generation für das Resource Manager-Bereitstellungsmodell

```
# One command to create a resource group holding a VM with LAMP already on it
$ azure group create -n uniqueResourceGroup -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
```

* Bereitstellen von LAMP auf einem vorhandenen virtuellen Computer

```
# Two commands: one updates packages, the other installs Apache, MySQL, and PHP
user@ubuntu$ sudo apt-get update
user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a>Bereitstellen von LAMP auf einer neuen VM – Exemplarische Vorgehensweise
Sie können beginnen, indem Sie eine [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) erstellen, die den neuen virtuellen Computer enthält:

    $ azure group create uniqueResourceGroup westus
    info:    Executing command group create
    info:    Getting resource group uniqueResourceGroup
    info:    Creating resource group uniqueResourceGroup
    info:    Created resource group uniqueResourceGroup
    data:    Id:                  /subscriptions/########-####-####-####-############/resourceGroups/uniqueResourceGroup
    data:    Name:                uniqueResourceGroup
    data:    Location:            westus
    data:    Provisioning State:  Succeeded
    data:    Tags: null
    data:
    info:    group create command OK

Zum Erstellen der eigentlichen VM können Sie eine bereits geschriebene Azure Resource Manager-Vorlage verwenden, die [hier bei GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app)erhältlich ist.

    $ azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json uniqueResourceGroup uniqueLampName

Eine Antwort mit Aufforderungen zur weiteren Eingabe wird angezeigt:

    info:    Executing command group deployment create
    info:    Supply values for the following parameters
    storageAccountNamePrefix: lampprefix
    location: westus
    adminUsername: someUsername
    adminPassword: somePassword
    mySqlPassword: somePassword
    dnsLabelPrefix: azlamptest
    info:    Initializing template configurations and parameters
    info:    Creating a deployment
    info:    Created template deployment "uniqueLampName"
    info:    Waiting for deployment to complete
    data:    DeploymentName     : uniqueLampName
    data:    ResourceGroupName  : uniqueResourceGroup
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          :
    data:    Mode               : Incremental
    data:    CorrelationId      : d51bbf3c-88f1-4cf3-a8b3-942c6925f381
    data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
    data:    ContentVersion     : 1.0.0.0
    data:    DeploymentParameters :
    data:    Name                      Type          Value
    data:    ------------------------  ------------  -----------
    data:    storageAccountNamePrefix  String        lampprefix
    data:    location                  String        westus
    data:    adminUsername             String        someUsername
    data:    adminPassword             SecureString  undefined
    data:    mySqlPassword             SecureString  undefined
    data:    dnsLabelPrefix            String        azlamptest
    data:    ubuntuOSVersion           String        14.04.2-LTS
    info:    group deployment create command OK

Sie haben nun eine Linux-VM erstellt, auf der LAMP bereits installiert ist. Bei Bedarf können Sie die Installation überprüfen, indem Sie nach unten zu [Überprüfen der erfolgreichen LAMP-Installation](#verify-lamp-successfully-installed) wechseln.

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a>Bereitstellen von LAMP auf einer vorhandenen VM – Exemplarische Vorgehensweise
Falls Sie bei der Erstellung einer Linux-VM Hilfe benötigen, lesen Sie diese [Informationen zum Erstellen einer Linux-VM](virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Als Nächstes müssen Sie per SSH auf die Linux-VM wechseln. Falls Sie bei der Erstellung eines SSH-Schlüssels Hilfe benötigen, lesen Sie diese [Informationen zum Erstellen eines SSH-Schlüssels unter Linux/Mac](virtual-machines-linux-mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
Wenn Sie bereits über einen SSH-Schlüssel verfügen, können Sie mit `ssh exampleUsername@exampleDNS` per SSH auf die Linux-VM wechseln.

Da Sie sich jetzt auf der Linux-VM befinden, folgt nun die exemplarische Vorgehensweise zur Installation des LAMP-Stapels auf Debian-basierten Distributionen. Die Befehle können für andere Linux-Distributionen jeweils variieren.

#### <a name="installing-on-debianubuntu"></a>Installation unter Debian/Ubuntu
Die folgenden Pakete müssen installiert sein: `apache2`, `mysql-server`, `php5` und `php5-mysql`. Sie können diese Pakete installieren, indem Sie direkt darauf zugreifen oder Tasksel verwenden. Die Anleitungen für beide Optionen sind unten aufgeführt.
Vor der Installation müssen Sie die Paketlisten herunterladen und aktualisieren.

    user@ubuntu$ sudo apt-get update

##### <a name="individual-packages"></a>Einzelne Pakete
Verwenden von apt-get:

    user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql

##### <a name="using-tasksel"></a>Verwenden von Tasksel:
Alternativ dazu können Sie Tasksel herunterladen. Dabei handelt es sich um ein Debian/Ubuntu-Tool, das mehrere in Beziehung stehende Pakete als koordinierten „Task“ auf Ihrem System installiert.

    user@ubuntu$ sudo apt-get install tasksel
    user@ubuntu$ sudo tasksel install lamp-server

Nach der Ausführung einer der oben genannten Optionen werden Sie aufgefordert, diese Pakete und eine Reihe weiterer Abhängigkeiten zu installieren. Drücken Sie "y" und dann die EINGABETASTE, um den Vorgang fortzusetzen, und befolgen Sie weitere Aufforderungen zum Festlegen eines Administratorkennworts für MySQL. Daraufhin werden die PHP-Erweiterungen installiert, die mindestens für die Verwendung von PHP mit MySQL erforderlich sind. 

![][1]

Führen Sie folgenden Befehl aus, um weitere PHP-Erweiterungen anzuzeigen, die als Pakete verfügbar sind:

    user@ubuntu$ apt-cache search php5


#### <a name="create-infophp-document"></a>Erstellen des Dokuments „info.php“
Sie sollten jetzt in der Befehlszeile überprüfen können, welche Version von Apache, MySQL und PHP Sie verwenden, indem Sie `apache2 -v`, `mysql -v` oder `php -v` eingeben.

Falls Sie weitere Tests durchführen möchten, können Sie schnell eine PHP-Infoseite zum Anzeigen in einem Browser erstellen. Erstellen Sie im Text-Editor Nano mit diesem Befehl eine neue Datei:

    user@ubuntu$ sudo nano /var/www/html/info.php

Fügen Sie im GNU Nano-Text-Editor die folgenden Zeilen hinzu:

    <?php
    phpinfo();
    ?>

Speichern und beenden Sie anschließend den Text-Editor.

Starten Sie Apache mit diesem Befehl neu, damit alle neuen Installationen wirksam werden.

    user@ubuntu$ sudo service apache2 restart

## <a name="verify-lamp-successfully-installed"></a>Überprüfen der erfolgreichen LAMP-Installation
Sie können jetzt die von Ihnen erstellte PHP-Informationsseite überprüfen, indem Sie einen Browser öffnen und zu http://IhrDNS/info.php navigieren. Es sollte in etwa wie in der folgenden Abbildung aussehen.

![][2]

Sie können Ihre Apache-Installation überprüfen, indem Sie unter http://youruniqueDNS/info.php die Apache2 Ubuntu-Standardseite anzeigen. Die Ausgabe sollte wie in der folgenden Abbildung aussehen.

![][3]

Glückwunsch! Sie haben einen LAMP-Stapel auf Ihrer Azure-VM eingerichtet.

## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich die Ubuntu-Dokumentation zum LAMP-Stapel an:

* [https://help.ubuntu.com/community/ApacheMySQLPHP](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/virtual-machines-linux-deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/virtual-machines-linux-deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/virtual-machines-linux-deploy-lamp-stack/apachesuccesspage.png
