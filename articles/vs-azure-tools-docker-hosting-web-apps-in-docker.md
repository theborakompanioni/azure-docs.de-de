---
title: Bereitstellen eines ASP.NET Core-Linux-Docker-Containers auf einem Docker-Remotehost | Microsoft Docs
description: "Erfahren Sie, wie Sie Visual Studio-Tools für Docker zum Bereitstellen einer ASP.NET Core-Web-App in einem Docker-Container verwenden, der auf einem virtuellen Azure-Docker-Hostcomputer unter Linux ausgeführt wird."
services: azure-container-service
documentationcenter: .net
author: mlearned
manager: douge
editor: 
ms.assetid: e5e81c5e-dd18-4d5a-a24d-a932036e78b9
ms.service: azure-container-service
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/08/2016
ms.author: mlearned
ms.translationtype: Human Translation
ms.sourcegitcommit: be747170a0d8a7a6defd790a3f8a122c4d397671
ms.openlocfilehash: 4a87ee69f23779bf4f6f5db40bc05edbcfc7668d
ms.contentlocale: de-de
ms.lasthandoff: 05/23/2017


---
# <a name="deploy-an-aspnet-container-to-a-remote-docker-host"></a>Bereitstellen eines ASP.NET-Containers auf einem Docker-Remotehost
## <a name="overview"></a>Übersicht
Docker ist ein einfaches Containermodul, in gewisser Weise mit einem virtuellen Computer vergleichbar, das Sie zum Hosten von Anwendungen und Diensten verwenden können.
Dieses Tutorial führt Sie durch die Verwendung der Erweiterung [Visual Studio-Tools für Docker](https://docs.microsoft.com/en-us/dotnet/articles/core/docker/visual-studio-tools-for-docker), um mithilfe von PowerShell eine ASP.NET Core-App auf einem Docker-Host in Azure bereitzustellen.

## <a name="prerequisites"></a>Voraussetzungen
Zum Abschließen dieses Tutorials müssen folgende Voraussetzungen erfüllt sein:

* Erstellen eines virtuellen Azure-Docker-Hostcomputers gemäß der Beschreibung in [Verwenden von „docker-machine“ mit Azure](virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Installieren der neuesten Version von [Visual Studio](https://www.visualstudio.com/downloads/)
* Herunterladen des [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)
* Installieren von [Docker für Windows](https://docs.docker.com/docker-for-windows/install/)

## <a name="1-create-an-aspnet-core-web-app"></a>1. Erstellen einer ASP.NET Core-Web-App
Die folgenden Schritte führen Sie durch die Erstellung einer einfachen ASP.NET Core-App, die in diesem Tutorial verwendet wird.

[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. Hinzufügen der Docker-Unterstützung
[!INCLUDE [create-aspnet5-app](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-use-the-dockertaskps1-powershell-script"></a>3. Verwenden des PowerShell-Skripts „DockerTask.ps1“
1. Öffnen Sie eine PowerShell-Eingabeaufforderung für das Stammverzeichnis des Projekts. 
   
   ```
   PS C:\Src\WebApplication1>
   ```
2. Vergewissern Sie sich, dass der Remotehost ausgeführt wird. Der Status „Wird ausgeführt“ sollte angezeigt werden. 
   
   ```
   docker-machine ls
   NAME         ACTIVE   DRIVER   STATE     URL                        SWARM   DOCKER    ERRORS
   MyDockerHost -        azure    Running   tcp://xxx.xxx.xxx.xxx:2376         v1.10.3
   ```
   
3. Erstellen Sie die App mit dem -Build-Parameter.
   
   ```
   PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release -Machine mydockerhost
   ```  

   > ```
   > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release 
   > ```  
   > 
   > 
4. Führen Sie die App mit dem -Run-Parameter aus.
   
   ```
   PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release -Machine mydockerhost
   ```
   
   > ```
   > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release 
   > ```
   > 
   > 
   
   Nach Abschluss von Docker sollten etwa folgende Ergebnisse angezeigt werden:
   
   ![Ihre App anzeigen][3]

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/docker-props-in-solution-explorer.png
[1]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/change-docker-machine-name.png
[2]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/launch-application.png
[3]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/view-application.png

