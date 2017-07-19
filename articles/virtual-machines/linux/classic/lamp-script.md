---
title: Verwenden der CustomScript-Erweiterung auf einem virtuellen Linux-Computer | Microsoft Docs
description: Erfahren Sie, wie die CustomScript-Erweiterung zum Bereitstellen von Anwendungen auf virtuellen Linux-Computern in Azure, die mit dem klassischen Bereitstellungsmodell erstellt wurden, verwendet wird.
editor: tysonn
manager: timlt
documentationcenter: 
services: virtual-machines-linux
author: gbowerman
tags: azure-service-management
ms.assetid: e535241d-feca-4412-b07a-67c936ba88a0
ms.service: virtual-machines-linux
ms.workload: multiple
ms.tgt_pltfrm: linux
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: guybo
ms.translationtype: Human Translation
ms.sourcegitcommit: 43aab8d52e854636f7ea2ff3aae50d7827735cc7
ms.openlocfilehash: 43175672c81373b5c9cc1bddc8ce27bcaaf5ec14
ms.contentlocale: de-de
ms.lasthandoff: 06/03/2017


---
# <a name="deploy-a-lamp-app-using-the-azure-customscript-extension-for-linux"></a>Bereitstellen einer LAMP-App mithilfe der Azure-CustomScript-Erweiterung für Linux
> [!IMPORTANT] 
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../../../resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells. Informationen zum Bereitstellen eines LAMP-Stacks mit dem Resource Manager-Modell finden Sie [hier](../create-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Die Microsoft Azure-CustomScript-Erweiterung für Linux bietet eine Möglichkeit, Ihre virtuellen Computer anzupassen, indem Sie beliebigen, in einer vom virtuellen Computer unterstützten Skriptsprache geschriebenen Code ausführen (z. B. Python, Bash usw.) Dies bietet eine sehr flexible Möglichkeit zum Automatisieren der Anwendungsbereitstellung auf verschiedenen Computern.

Sie können die CustomScript-Erweiterung mit dem Azure-Portal, mit Windows PowerShell oder mit der Azure-Befehlszeilenschnittstelle (Azure CLI) bereitstellen.

In diesem Artikel verwenden wir die Azure-Befehlszeilenschnittstelle zum Bereitstellen einer einfachen LAMP-Anwendung auf einem virtuellen Ubuntu-Computer, der mit dem klassischen Bereitstellungsmodell erstellt wurde.

## <a name="prerequisites"></a>Voraussetzungen
Erstellen Sie für dieses Beispiel zuerst zwei virtuelle Azure-Computer, auf denen Ubuntu 14.04 oder höher ausgeführt wird. Die virtuellen Computer heißen *script-vm* und *lamp-vm*. Verwenden Sie beim Erstellen der virtuellen Computer eindeutige Namen. Einer davon dient zur Ausführung der CLI-Befehle, auf dem anderen wird die LAMP-App bereitgestellt.

Darüber hinaus benötigen Sie ein Azure-Speicherkonto und einen Schlüssel für den Zugriff (Sie erhalten diesen über das Azure-Portal).

Weitere Informationen zum Erstellen von Linux-VMs auf Azure finden Sie unter [Erstellen eines virtuellen Linux-Computers](createportal.md).

Für die Installationsbefehle wird von Ubuntu ausgegangen, Sie können die Installation jedoch an alle unterstützten Linux-Distribution anpassen.

Auf dem virtuellen Computer „script-vm“ muss die Azure-CLI mit einer funktionierenden Verbindung zu Azure installiert sein. Hilfe zu diesem Thema finden Sie unter [Installieren und Konfigurieren der Azure-Befehlszeilenschnittstelle](../../../cli-install-nodejs.md).

## <a name="upload-a-script"></a>Hochladen von Skripts
Wir führen mit der CustomScript-Erweiterung ein Skript auf einem virtuellen Remotecomputer aus, um den LAMP-Stack zu installieren und eine PHP-Seite zu erstellen. Damit von einem beliebigen Standort aus auf das Skript zugegriffen werden kann, wird es als Azure-Blob hochgeladen.

### <a name="script-overview"></a>Skriptübersicht
Das nächste Skriptbeispiel installiert einen LAMP-Stack auf Ubuntu (einschließlich Einrichtung einer unbeaufsichtigten Installation von MySQL), schreibt eine einfache PHP-Datei und startet Apache.

    #!/bin/bash
    # set up a silent install of MySQL
    dbpass="mySQLPassw0rd"

    export DEBIAN_FRONTEND=noninteractive
    echo mysql-server-5.6 mysql-server/root_password password $dbpass | debconf-set-selections
    echo mysql-server-5.6 mysql-server/root_password_again password $dbpass | debconf-set-selections

    # install the LAMP stack
    apt-get -y install apache2 mysql-server php5 php5-mysql  

    # write some PHP
    echo \<center\>\<h1\>My Demo App\</h1\>\<br/\>\</center\> > /var/www/html/phpinfo.php
    echo \<\?php phpinfo\(\)\; \?\> >> /var/www/html/phpinfo.php

    # restart Apache
    apachectl restart

### <a name="upload-script"></a>Skript hochladen
Speichern Sie das Skript als Textdatei (z.B. *install_lamp.sh*), und laden Sie es dann in Azure Storage hoch. Diese Aufgabe kann auf einfache Weise mit der Azure-CLI ausgeführt werden. Im folgenden Beispiel wird die Datei in einen Container namens "scripts" hochgeladen. Wenn der Container nicht vorhanden ist, müssen Sie ihn zunächst erstellen.

    azure storage blob upload -a <yourStorageAccountName> -k <yourStorageKey> --container scripts ./install_lamp.sh

Erstellen Sie außerdem eine JSON-Datei, die beschreibt, wie das Skript aus Azure Storage heruntergeladen wird. Speichern Sie diese als *public_config.json* (ersetzen Sie „mystorage“ durch den Namen Ihres Speicherkontos):

    {"fileUris":["https://mystorage.blob.core.windows.net/scripts/install_lamp.sh"], "commandToExecute":"sh install_lamp.sh" }


## <a name="deploy-the-extension"></a>Bereitstellen der Erweiterung
Jetzt können wir mit dem nächsten Befehl die CustomScript-Erweiterung für Linux mit der Azure-CLI auf dem virtuellen Remotecomputer bereitstellen.

    azure vm extension set -c "./public_config.json" lamp-vm CustomScript Microsoft.Azure.Extensions 2.0

Mit dem vorhergehenden Befehl wird das Skript *install_lamp.sh* heruntergeladen und auf dem virtuellen Computer namens *lamp-vm* ausgeführt.

Da die App einen Webserver umfasst, müssen Sie daran denken, mithilfe des nächsten Befehls einen HTTP-Lauschport auf dem virtuellen Remotecomputer zu öffnen.

    azure vm endpoint create -n Apache -o tcp lamp-vm 80 80

## <a name="monitoring-and-troubleshooting"></a>Überwachung und Problembehandlung
Sie können den Fortschritt bei der Ausführung des benutzerdefinierten Skripts mithilfe der Protokolldatei auf dem virtuellen Computer überprüfen. Stellen Sie mit dem nächsten Befehl über SSH eine Verbindung mit *lamp-vm* her, und zeigen Sie mithilfe von "tail" die Protokolldatei an.

    cd /var/log/azure/customscript
    tail -f handler.log

Nach dem Ausführen der CustomScript-Erweiterung können Sie die von Ihnen erstellte PHP-Seite nach Informationen durchsuchen. Die PHP-Seite für das Beispiel in diesem Artikel ist *http://lamp-vm.cloudapp.net/phpinfo.php*.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
Sie können dieselben grundlegenden Schritte auch zur Bereitstellung komplexerer Apps verwenden. In diesem Beispiel wurde das Installationsskript als öffentliches Blob in Azure Storage gespeichert. Höhere Sicherheit erzielen Sie, indem Sie das Installationsskript mit [Secure Access Signature](https://msdn.microsoft.com/library/azure/ee395415.aspx) (SAS) als sicheres Blob speichern.

Nachfolgend finden Sie einige zusätzliche Ressourcen für die Azure-CLI, Linux und die CustomScript-Erweiterung.

[Automatisieren von Aufgaben zur Anpassung von Linux-VMs mit der CustomScript-Erweiterung](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/)

[Azure-Erweiterungen für Linux (GitHub)](https://github.com/Azure/azure-linux-extensions)

[Linux und Open-Source-Computing auf Azure](../opensource-links.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


