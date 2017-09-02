---
title: Bereitstellen von LEMP auf einem virtuellen Linux-Computer in Azure | Microsoft-Dokumentation
description: "Tutorial – Installieren des LEMP-Stacks auf einem virtuellen Linux-Computer in Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 08/03/2017
ms.author: danlep
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 653af144eb12cacf955f96a5442efd73add38e88
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="install-a-lemp-web-server-on-an-azure-vm"></a>Installieren eines LEMP-Webservers auf einem virtuellen Azure-Computer
In diesem Artikel werden Sie durch die Bereitstellung eines NGINX-Webservers sowie von MySQL und PHP (LEMP-Stack) auf einem virtuellen Ubuntu-Computer in Azure geführt. Der LEMP-Stack ist eine Alternative zum gängigen [LAMP-Stack](tutorial-lamp-stack.md), der auch in Azure installiert werden kann. Um den LEMP-Server in Aktion zu sehen, können Sie optional eine WordPress-Website installieren und konfigurieren. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines virtuellen Ubuntu-Computers („L“ im LEMP-Stack)
> * Öffnen von Port 80 für Webdatenverkehr
> * Installieren von NGINX, MySQL und PHP
> * Überprüfen der Installation und Konfiguration
> * Installieren von WordPress auf dem LEMP-Server


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial die Azure CLI-Version 2.0.4 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

[!INCLUDE [virtual-machines-linux-tutorial-stack-intro.md](../../../includes/virtual-machines-linux-tutorial-stack-intro.md)]

## <a name="install-nginx-mysql-and-php"></a>Installieren von NGINX, MySQL und PHP

Führen Sie den folgenden Befehl aus, um die Ubuntu-Paketquellen zu aktualisieren und NGINX, MySQL und PHP zu installieren. 

```bash
sudo apt update && sudo apt install nginx mysql-server php-mysql php php-fpm
```

Sie werden aufgefordert, die Pakete und andere Abhängigkeiten zu installieren. Legen Sie bei entsprechender Aufforderung ein Stammkennwort für MySQL fest, und drücken Sie dann die [Eingabetaste], um den Vorgang fortzusetzen. Befolgen Sie die restlichen Anweisungen. Dieser Prozess installiert die PHP-Erweiterungen, die mindestens für die Verwendung von PHP mit MySQL erforderlich sind. 

![Seite für das MySQL-Stammkennwort][1]

## <a name="verify-installation-and-configuration"></a>Überprüfen der Installation und Konfiguration


### <a name="nginx"></a>NGINX

Überprüfen Sie die Version von NGINX mit dem folgenden Befehl:
```bash
nginx -v
```

Nachdem NGINX installiert und Port 80 für den virtuellen Computer geöffnet wurde, ist der Zugriff auf den Webserver über das Internet möglich. Öffnen Sie zum Anzeigen der NGINX-Startseite einen Webbrowser, und geben Sie die öffentliche IP-Adresse des virtuellen Computers ein. Verwenden Sie die öffentliche IP-Adresse, die Sie zum Herstellen einer SSH-Verbindung mit dem virtuellen Computer verwendet haben:

![NGINX-Standardseite][3]


### <a name="mysql"></a>MySQL

Überprüfen Sie die Version von MySQL mit dem folgenden Befehl (beachten Sie die Großschreibung des `V`-Parameters):

```bash
msql -V
```

Es wird empfohlen, das folgende Skript zum Sichern der Installation von MySQL auszuführen:

```bash
mysql_secure_installation
```

Geben Sie Ihr MySQL-Stammkennwort ein, und konfigurieren Sie die Sicherheitseinstellungen für Ihre Umgebung.

Wenn Sie eine MySQL-Datenbank erstellen, Benutzer hinzufügen oder Konfigurationseinstellungen ändern möchten, melden Sie sich bei MySQL an:

```bash
mysql -u root -p
```

Beenden Sie anschließend die MySQL-Eingabeaufforderung durch Eingabe von `\q`.

### <a name="php"></a>PHP

Überprüfen Sie die Version von PHP mit dem folgenden Befehl:

```bash
php -v
```

Konfigurieren Sie NGINX zur Verwendung von PHP FastCGI Process Manager (PHP-FPM). Führen Sie die folgenden Befehle zum Sichern der ursprünglichen Konfigurationsdatei des NGINX-Serverblocks aus, und bearbeiten Sie dann die ursprüngliche Datei in einem Editor Ihrer Wahl:

```bash
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default_backup

sudo sensible-editor /etc/nginx/sites-available/default
```

Ersetzen Sie im Editor den Inhalt von `/etc/nginx/sites-available/default` folgendermaßen. Siehe dazu die Kommentare als Erläuterung der Einstellungen. Ersetzen Sie *yourPublicIPAddress* durch die öffentliche IP-Adresse Ihres virtuellen Computers, und behalten Sie die übrigen Einstellungen bei. Speichern Sie dann die Datei.

```
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    # Homepage of website is index.php
    index index.php;

    server_name yourPublicIPAddress;

    location / {
        try_files $uri $uri/ =404;
    }

    # Include FastCGI configuration for NGINX
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.0-fpm.sock;
    }
}
```

Überprüfen Sie die NGINX-Konfiguration auf Syntaxfehler:

```bash
sudo nginx -t
```

Wenn die Syntax richtig ist, starten Sie NGINX mit dem folgenden Befehl neu:

```bash
sudo service nginx restart
```

Wenn Sie weitere Tests durchführen möchten, erstellen Sie schnell eine PHP-Infoseite zum Anzeigen in einem Browser. Die PHP-Infoseite wird mit dem folgenden Befehl erstellt:

```bash
sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'
```



Nun können Sie die erstellte PHP-Infoseite überprüfen. Öffnen Sie einen Browser, und wechseln Sie zu `http://yourPublicIPAddress/info.php`. Ersetzen Sie die öffentliche IP-Adresse Ihres virtuellen Computers. Die Seite sollte ähnlich wie diese Abbildung aussehen.

![PHP-Infoseite][2]


[!INCLUDE [virtual-machines-linux-tutorial-wordpress.md](../../../includes/virtual-machines-linux-tutorial-wordpress.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen LEMP-Server in Azure bereitgestellt. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Erstellen eines virtuellen Ubuntu-Computers
> * Öffnen von Port 80 für Webdatenverkehr
> * Installieren von NGINX, MySQL und PHP
> * Überprüfen der Installation und Konfiguration
> * Installieren von WordPress im LEMP-Stack

Im nächsten Tutorial erfahren Sie, wie Sie Webserver mit SSL-Zertifikaten sichern.

> [!div class="nextstepaction"]
> [Sichern von Webservern mit SSL](tutorial-secure-web-server.md)

[1]: ./media/tutorial-lemp-stack/configmysqlpassword-small.png
[2]: ./media/tutorial-lemp-stack/phpsuccesspage.png
[3]: ./media/tutorial-lemp-stack/nginx.png

