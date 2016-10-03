<properties
   pageTitle="Bereitstellen eines ASP.NET Core-Linux-Docker-Containers auf einem Docker-Remotehost | Microsoft Azure"
   description="Erfahren Sie, wie Sie Visual Studio-Tools für Docker zum Bereitstellen einer ASP.NET Core-Web-App in einem Docker-Container verwenden, der auf einem virtuellen Azure-Docker-Hostcomputer unter Linux ausgeführt wird."   
   services="azure-container-service"
   documentationCenter=".net"
   authors="mlearned"
   manager="douge"
   editor=""/>

<tags
   ms.service="azure-container-service"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/08/2016"
   ms.author="mlearned"/>

# Bereitstellen eines ASP.NET-Containers auf einem Docker-Remotehost

## Übersicht
Docker ist ein einfaches Containermodul, in gewisser Weise mit einem virtuellen Computer vergleichbar, das Sie zum Hosten von Anwendungen und Diensten verwenden können. Dieses Tutorial führt Sie durch die Verwendung der Erweiterung [Visual Studio 2015-Tools für Docker](http://aka.ms/DockerToolsForVS), um mithilfe von PowerShell eine ASP.NET Core-App auf einem Docker-Host in Azure bereitzustellen.

## Voraussetzungen
Folgendes ist zum Durchführen dieses Tutorials erforderlich:

- Erstellen eines virtuellen Azure-Docker-Hostcomputers gemäß der Beschreibung in [Verwenden von „docker-machine“ mit Azure](./virtual-machines/virtual-machines-linux-docker-machine.md)
- Installieren von [Visual Studio 2015 Update 3](https://go.microsoft.com/fwlink/?LinkId=691129)
- [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)
- Installieren von [Visual Studio 2015-Tools für Docker – Preview](http://aka.ms/DockerToolsForVS)

## 1\. Erstellen einer ASP.NET Core-Web-App
Die folgenden Schritte führen Sie durch die Erstellung einer einfachen ASP.NET Core-App, die in diesem Tutorial verwendet wird.

[AZURE.INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## 2\. Hinzufügen der Docker-Unterstützung

[AZURE.INCLUDE [create-aspnet5-app](../includes/vs-azure-tools-docker-add-docker-support.md)]

## 3\. Verwenden des PowerShell-Skripts „DockerTask.ps1“ 

1.  Öffnen Sie eine PowerShell-Eingabeaufforderung für das Stammverzeichnis des Projekts.

    ```
    PS C:\Src\WebApplication1>
    ```

1.  Vergewissern Sie sich, dass der Remotehost ausgeführt wird. Der Status „Wird ausgeführt“ sollte angezeigt werden.

    ```
    docker-machine ls
    NAME         ACTIVE   DRIVER   STATE     URL                        SWARM   DOCKER    ERRORS
    MyDockerHost -        azure    Running   tcp://xxx.xxx.xxx.xxx:2376         v1.10.3
    ```

    > [AZURE.NOTE] Wenn Sie die Betaversion von Docker verwenden, ist Ihr Host hier nicht aufgeführt.

1.  Erstellen Sie die App mit dem -Build-Parameter.

    ```
    PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release -Machine mydockerhost
    ```  

    > [AZURE.NOTE] Wenn Sie die Betaversion von Docker verwenden, lassen Sie das -Machine-Argument aus.
    > 
    > ```
    > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release 
    > ```  


1.  Führen Sie die App mit dem -Run-Parameter aus.

    ```
    PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release -Machine mydockerhost
    ```

    > [AZURE.NOTE] Wenn Sie die Betaversion von Docker verwenden, lassen Sie das -Machine-Argument aus.
    > 
    > ```
    > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release 
    > ```

	Nach Abschluss von Docker sollten etwa folgende Ergebnisse angezeigt werden:

    ![Ihre App anzeigen][3]

[0]: ./media/vs-azure-tools-docker-hosting-web-apps-in-docker/docker-props-in-solution-explorer.png
[1]: ./media/vs-azure-tools-docker-hosting-web-apps-in-docker/change-docker-machine-name.png
[2]: ./media/vs-azure-tools-docker-hosting-web-apps-in-docker/launch-application.png
[3]: ./media/vs-azure-tools-docker-hosting-web-apps-in-docker/view-application.png

<!---HONumber=AcomDC_0921_2016-->