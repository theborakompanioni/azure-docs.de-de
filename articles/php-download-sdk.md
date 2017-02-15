---
title: "Herunterladen des Azure SDK für PHP"
description: "Erfahren Sie mehr über das Herunterladen und Installieren von Azure SDK für PHP."
documentationcenter: php
services: app-service\web
author: allclark
manager: douge
editor: 
ms.assetid: bac355ac-4c25-42f4-8273-c5112eafa8d4
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 06/01/2016
ms.author: allclark;yaqiyang
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 7bd6eddcdb20e3ea49cd29d46bb4f98d73ee9cc8


---
# <a name="download-the-azure-sdk-for-php"></a>Herunterladen des Azure SDK für PHP
## <a name="overview"></a>Übersicht
Das Azure SDK für PHP enthält Komponenten, mit deren Hilfe Sie PHP-Anwendungen für Azure entwickeln, bereitstellen und verwalten können. Insbesondere enthält das Azure SDK für PHP Folgendes:

* **Die PHP-Clientbibliotheken für Azure**. Diese Basisklassenbibliotheken bieten eine Schnittstelle für den Zugriff auf Azure-Funktionen wie Datenverwaltungs- und Clouddienste.  
* **Azure-Befehlszeilenschnittstelle für Mac, Linux und Windows (Azure CLI)**. Dies ist eine Gruppe von Befehlen zum Bereitstellen und Verwalten von Azure-Diensten wie Azure-Websites und virtuellen Azure-Computern. Die Azure-Befehlszeilenschnittstelle funktioniert auf allen Plattformen, z. B. Mac, Linux und Windows.
* **Azure PowerShell (nur Windows)**. Dies ist eine Gruppe von PowerShell-Cmdlets zum Bereitstellen und Verwalten von Azure-Diensten wie Clouddiensten und virtuellen Computern.
* **Die Azure-Emulatoren (nur Windows)**. Die Rechen- und Speicheremulatoren sind lokale Emulatoren von Datenverwaltungs- und Clouddiensten, mit denen Sie eine Anwendung lokal testen können. Die Azure-Emulatoren laufen nur unter Windows.

In den Abschnitten unten wird erläutert, wie Sie die oben beschriebenen Komponenten herunterladen und installieren.

Für die Anweisungen in diesem Thema wird vorausgesetzt, dass Sie [PHP][install-php] installiert haben.

> [!NOTE]
> Sie müssen PHP 5.5 oder höher besitzen, um die PHP-Clientbibliotheken für Azure verwenden zu können.
> 
> 

## <a name="php-client-libraries-for-azure"></a>PHP-Clientbibliotheken für Azure
Die PHP Clientbibliotheken für Azure bieten eine Schnittstelle zum Zugriff auf Azure-Funktionen wie Datenverwaltungs- und Clouddienste auf jedem Betriebssystem. Diese Bibliotheken können über Composer installiert werden.

Informationen über die Verwendung der PHP-Clientbibliotheken für Azure finden Sie unter [Verwenden des Blob-Diensts][blob-service], [Verwenden des Tabellendiensts][table-service] und [Verwenden des Warteschlangendiensts][queue-service].

### <a name="install-via-composer"></a>Installation mithilfe von Composer
1. [Installieren Sie Git][install-git].

    > [AZURE.NOTE] Unter Windows muss die ausführbare Git-Datei zu Ihrer PATH-Umgebungsvariable hinzugefügt werden.

1. Erstellen Sie im Stammverzeichnis Ihres Projekts eine Datei namens **composer.json** , und fügen Sie zu dieser den folgenden Code hinzu:
   
        {
            "require": {
                "microsoft/windowsazure": "^0.4"
            }
        }
2. Laden Sie **[composer.phar][composer-phar]** in Ihr Projektverzeichnis herunter.
3. Öffnen Sie eine Eingabeaufforderung, und führen Sie in Ihrem Projektverzeichnis folgenden Befehl aus
   
        php composer.phar install

## <a name="azure-powershell-and-azure-emulators"></a>Azure PowerShell und Azure-Emulatoren
Azure PowerShell ist eine Gruppe von PowerShell-Cmdlets zum Bereitstellen und Verwalten von Azure-Diensten (wie Clouddiensten und virtuellen Computern). Die Azure-Emulatoren sind Emulatoren von Clouddiensten und Datenverwaltungsdiensten, mit denen Sie eine Anwendung lokal testen können. Diese Komponenten werden nur unter Windows unterstützt.

Die empfohlene Methode zur Installation von Azure PowerShell sowie der Azure-Emulatoren ist die Verwendung des [Microsoft-Webplattform-Installers][download-wpi]. Bitte beachten Sie, dass Sie auch noch weitere Entwicklungskomponenten wie PHP, SQL Server, die Microsoft-Treiber für SQL Server für PHP und WebMatrix installieren können.

Informationen zur Verwendung von Azure finden Sie unter [Verwenden von Azure PowerShell][powershell-tools].

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle
Die Azure-Befehlszeilenschnittstelle ist eine Gruppe von Befehlen zum Bereitstellen und Verwalten von Azure-Diensten wie Azure-Websites und virtuellen Azure-Computern. Informationen zum Installieren der Azure-Befehlszeilenschnittstelle finden Sie unter [Installieren der Azure-Befehlszeilenschnittstelle](xplat-cli-install.md).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie im [PHP Developer Center](/develop/php/).

[install-php]: http://www.php.net/manual/en/install.php
[composer-github]: https://github.com/composer/composer
[composer-phar]: http://getcomposer.org/composer.phar
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[download-wpi]: http://go.microsoft.com/fwlink/?LinkId=253447
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[blob-service]: http://go.microsoft.com/fwlink/?LinkId=252714
[table-service]: http://go.microsoft.com/fwlink/?LinkId=252715
[queue-service]: http://go.microsoft.com/fwlink/?LinkId=252716
[azure cli]: http://go.microsoft.com/fwlink/?LinkId=252717
[powershell-tools]: http://go.microsoft.com/fwlink/?LinkId=252718
[php-sdk-github]: http://go.microsoft.com/fwlink/?LinkId=252719
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git



<!--HONumber=Nov16_HO3-->


