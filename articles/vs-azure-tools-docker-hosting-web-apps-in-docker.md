<properties
   pageTitle="Bereitstellen eines ASP.NET-Containers auf einem Docker-Remotehost | Microsoft Azure"
   description="Erfahren Sie, wie Sie Visual Studio-Tools für Docker zum Veröffentlichen einer ASP.NET 5-Web-App in einem Docker-Container verwenden, der auf einem Azure-Docker-Hostcomputer ausgeführt wird."   
   services="visual-studio-online"
   documentationCenter=".net"
   authors="tomarcher"
   manager="douge"
   editor=""/>

<tags
   ms.service="visual-studio-online"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/08/2016"
   ms.author="tarcher"/>

# Bereitstellen eines ASP.NET-Containers auf einem Docker-Remotehost

## Übersicht
Docker ist ein einfaches Containermodul, in gewisser Weise mit einem virtuellen Computer vergleichbar, das Sie zum Hosten von Anwendungen und Diensten verwenden können. Visual Studio unterstützt Docker für Ubuntu, CoreOS und Windows. Dieses Tutorial führt Sie durch die Verwendung der Erweiterung [Visual Studio 2015-Tools für Docker](http://aka.ms/DockerToolsForVS), um eine ASP.NET 5-Anwendung auf einem Docker-Host in Azure zu veröffentlichen.

## 1\. Voraussetzungen
Folgendes ist zum Durchführen dieses Tutorials erforderlich:

- Erstellen eines virtuellen Azure-Docker-Hostcomputers gemäß der Beschreibung in [Verwenden von „docker-machine“ mit Azure](./virtual-machines/virtual-machines-linux-classic-docker-machine.md)
- Installieren von [Visual Studio 2015](https://www.visualstudio.com/de-DE/downloads/download-visual-studio-vs.aspx)
- Installieren von [Visual Studio 2015-Tools für Docker – Preview](http://aka.ms/DockerToolsForVS)

## 2\. Erstellen einer ASP.NET 5-Web-App
Die folgenden Schritte führen Sie durch die Erstellung einer einfachen ASP.NET 5-App, die in diesem Tutorial verwendet wird.

[AZURE.INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## 3\. Hinzufügen der Docker-Unterstützung

[AZURE.INCLUDE [create-aspnet5-app](../includes/vs-azure-tools-docker-add-docker-support.md)]

## 4\. Zeigen auf den Docker-Remotehost

1.  Suchen Sie im **Projektmappen-Explorer** von Visual Studio den Ordner **Eigenschaften** Ordner, und erweitern Sie ihn.
1.  Öffnen Sie die Datei *Docker.props*.

    ![Öffnen der Datei „Docker.props“][0]

1.  Ändern Sie den Wert von **DockerMachineName** in den Namen Ihres Docker-Remotehosts. Wenn Sie den Namen Ihres Docker-Remotehosts nicht kennen, führen Sie an der Windows PowerShell-Eingabeaufforderung ```docker-machine ls``` aus. Verwenden Sie den Wert unter der Spalte **Name** für den gewünschten Host.

    ![Ändern des Namens des Docker-Computers][1]

1.  Starten Sie Visual Studio neu.

## 5\. Konfigurieren des Azure-Docker-Host-Endpunkts
Fügen Sie vor der Bereitstellung Ihrer App von Visual Studio an Azure Ihrem virtuellen Docker-Hostcomputer den Endpunkt 80 hinzu, damit Sie Ihre App später im Browser anzeigen können. Dies ist entweder über das [klassische Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885) oder über Windows PowerShell möglich:

- **Konfiguration des Azure-Docker-Host-Endpunkts mithilfe des [klassischen Azure-Portals](http://go.microsoft.com/fwlink/?LinkID=213885)**

    1.  Navigieren Sie zum [klassischen Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885). 
    
    1.  Tippen Sie auf **VIRTUELLE COMPUTER**
    
    1.  Wählen Sie Ihren virtuellen Docker-Hostcomputer aus.
    
    1.  Tippen Sie auf die Registerkarte **ENDPUNKTE**.
    
    1.  Tippen Sie (unten auf der Seite) auf **HINZUFÜGEN**.
    
    1.  Führen Sie die Anweisungen aus, um Port 80 verfügbar zu machen, der standardmäßig vom Bereitstellungsskript verwendet wird.

- **Konfiguration des Azure-Docker-Host-Endpunkts mithilfe von Windows PowerShell**

    1. Öffnen Sie Windows PowerShell.
    1. Geben Sie den folgenden Befehl an der Windows PowerShell-Eingabeaufforderung (ändern Sie die Werte in spitzen Klammern in die entsprechenden Werte Ihrer Umgebung):  

        ```PowerShell
        C:\PS>Get-AzureVM -ServiceName "<your_cloud_service_name>" -Name "<your_vm_name>" | Add-AzureEndpoint -Name "<endpoint_name>" -Protocol "tcp" -PublicPort 80 -LocalPort 80 | Update-AzureVM
        ```

## 6\. Erstellen und Ausführen der App
Bei der Bereitstellung auf Remotehosts funktioniert das Volumezuordnungsfeature für die Entwicklung mithilfe des Features zum Bearbeiten und Aktualisieren nicht. Daher müssen Sie beim Erstellen der App die *Releasekonfiguration* verwenden, um die Konfiguration der Volumezuordnung zu vermeiden. Führen Sie diese Schritte aus, um Ihre App auszuführen.

1.  Wählen Sie auf der Symbolleiste von Visual Studio die **Releasekonfiguration** aus.

1.  Ändern Sie das Startziel in **Docker**.

1.  Tippen Sie auf das **Docker**-Symbol, um die App zu erstellen und auszuführen.

![App starten][2]

Die Ergebnisse sollten in etwa wie folgt aussehen:

![Ihre App anzeigen][3]

[0]: ./media/vs-azure-tools-docker-hosting-web-apps-in-docker/docker-props-in-solution-explorer.png
[1]: ./media/vs-azure-tools-docker-hosting-web-apps-in-docker/change-docker-machine-name.png
[2]: ./media/vs-azure-tools-docker-hosting-web-apps-in-docker/launch-application.png
[3]: ./media/vs-azure-tools-docker-hosting-web-apps-in-docker/view-application.png

<!---HONumber=AcomDC_0511_2016-->