---
title: Bereitstellen der App in Azure App Service mithilfe von FTP/S | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihre App mithilfe von FTP oder FTPS in Azure App Service bereitstellen.
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: ae78b410-1bc0-4d72-8fc4-ac69801247ae
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2016
ms.author: cephalin;dariac
translationtype: Human Translation
ms.sourcegitcommit: 816113f7635a003e22a5172113e5039dbcc1ceac
ms.openlocfilehash: 5dc546849bd02ccf4d02f3e6363a3e2fc3898259


---
# <a name="deploy-your-app-to-azure-app-service-using-ftps"></a>Bereitstellen der App in Azure App Service mithilfe von FTP/S
In diesem Artikel erfahren Sie, wie Sie eine Web-App, das mobile App-Back-End oder eine API-App mithilfe von FTP oder FTPS in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) bereitstellen.

Der FTP/S-Endpunkt für Ihre App ist bereits aktiv. Zum Aktivieren der FTP/S-Bereitstellung ist keine Konfiguration erforderlich. 

<a name="step1"></a>
## <a name="step-1-set-deployment-credentials"></a>Schritt 1: Festlegen von Anmeldeinformationen für die Bereitstellung

Für den Zugriff auf den FTP-Server für Ihre App benötigen Sie zuerst Anmeldeinformationen für die Bereitstellung. 

Informationen zum Festlegen oder Zurücksetzen Ihrer Anmeldeinformationen für die Bereitstellung finden Sie unter [Anmeldeinformationen für die Azure App Service-Bereitstellung](app-service-deployment-credentials.md). Dieses Tutorial veranschaulicht die Verwendung von Anmeldeinformationen auf Benutzerebene.

## <a name="step-2-get-ftp-connection-information"></a>Schritt 2: Abrufen der FTP-Verbindungsinformationen

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com) das Blatt [Ressourcen](../azure-resource-manager/resource-group-portal.md#manage-resources) Ihrer App.
2. Wählen Sie im linken Menü **Übersicht**, und notieren Sie dann die Werte für **FTP/Bereitstellungsbenutzer**, **FTP-Hostname** und **FTPS-Hostname**. 

    ![FTP-Verbindungsinformationen](./media/web-sites-deploy/FTP-Connection-Info.PNG)

    > [!NOTE]
    > Der Benutzerwert für **FTP/Bereitstellungsbenutzer**, wie im Azure-Portal angezeigt, einschließlich des App-Namens, um den richtigen Kontext für den FTP-Server bereitzustellen.
    > Die gleichen Informationen finden Sie, wenn Sie im linken Menü **Eigenschaften** auswählen. 
    >
    > Das Kennwort für die Bereitstellung wird nie angezeigt. Wenn Sie Ihr Kennwort für die Bereitstellung vergessen haben, wechseln Sie zurück zu [Schritt1](#step1), und setzen Sie es zurück.
    >
    >

## <a name="step-3-deploy-files-to-azure"></a>Schritt 3: Bereitstellen von Dateien in Azure

1. Verwenden Sie in Ihrem FTP-Client (z.B. [Visual Studio](https://www.visualstudio.com/vs/community/), [FileZilla](https://filezilla-project.org/download.php?type=client) die gesammelten Verbindungsinformationen, um eine Verbindung mit Ihrer App herzustellen.
3. Kopieren Sie Ihre Dateien und die entsprechende Verzeichnisstruktur in das Verzeichnis [**/site/wwwroot**](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) in Azure (bzw. für WebJobs in das Verzeichnis **/site/wwwroot/App_Data/Jobs/**).
4. Navigieren Sie zur URL Ihrer App, um sicherzustellen, dass die Anwendung richtig ausgeführt wird. 

> [!NOTE] 
> Im Gegensatz zu [Git-basierten Bereitstellungen](app-service-deploy-local-git.md) unterstützt die FTP-Bereitstellung keine der folgenden Bereitstellungsautomatisierungen: 
>
> - Wiederherstellen der Abhängigkeit (z.B. NuGet-, NPM-, PIP- und Composer-Automatisierungen)
> - Kompilierung von .NET-Binärdateien
> - Generierung von „web.config“ (hier sehen Sie ein [Node.js-Beispiel](https://github.com/projectkudu/kudu/wiki/Using-a-custom-web.config-for-Node-apps))
> 
> Diese erforderlichen Dateien müssen Sie auf Ihrem lokalen Computer manuell wiederherstellen, erstellen, generieren und zusammen mit Ihrer App bereitstellen.
>
>

## <a name="next-steps"></a>Nächste Schritte

Für komplexere Bereitstellungsszenarien versuchen Sie die [Bereitstellung in Azure mit Git](app-service-deploy-local-git.md). Die Git-basierte Bereitstellung in Azure ermöglicht Versionskontrolle, Paketwiederherstellung, MSBuild und mehr.

## <a name="more-resources"></a>Weitere Ressourcen

* [Erstellen einer PHP-MySQL-Web-App und Bereitstellen über FTP](web-sites-php-mysql-deploy-use-ftp.md).
* [Anmeldeinformationen für die Azure App Service-Bereitstellung](app-service-deploy-ftp.md)



<!--HONumber=Jan17_HO4-->


