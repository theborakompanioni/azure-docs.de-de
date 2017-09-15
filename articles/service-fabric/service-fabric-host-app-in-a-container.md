---
title: Bereitstellen von .NET-Apps in einem Container in Azure Service Fabric | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie eine .NET-App in Visual Studio in einem Docker-Container packen. Diese neue „Container“-App wird dann in einem Service Fabric-Cluster bereitgestellt."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/19/2017
ms.author: mikhegn
ms.translationtype: HT
ms.sourcegitcommit: 0425da20f3f0abcfa3ed5c04cec32184210546bb
ms.openlocfilehash: 484db494e7975df950543d19bf841a4df7cdd139
ms.contentlocale: de-de
ms.lasthandoff: 07/20/2017

---

# <a name="deploy-a-net-application-in-a-windows-container-to-azure-service-fabric"></a>Bereitstellen einer .NET-App in einem Windows-Container in Azure Service Fabric

In diesem Tutorial wird gezeigt, wie Sie eine vorhandene ASP.NET-Anwendung in einem Windows-Container in Azure bereitstellen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Docker-Projekts in Visual Studio
> * Packen einer vorhandenen Anwendung in Container
> * Einrichten der Continuous Integration mit Visual Studio und VSTS

## <a name="prerequisites"></a>Voraussetzungen

1. Installieren Sie [Docker CE für Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description), damit Sie Container unter Windows 10 ausführen können.
2. Machen Sie sich mit den [Schnellstart für Windows 10-Container][link-container-quickstart] vertraut.
3. Laden Sie die Beispielanwendung [Fabrikam Fiber CallCenter][link-fabrikam-github] herunter.
4. Installieren Sie [Azure PowerShell][link-azure-powershell-install].
5. Installieren Sie die [Erweiterung für die Continuous Delivery-Tools für Visual Studio 2017][link-visualstudio-cd-extension].
6. Erstellen Sie ein [Azure-Abonnement][link-azure-subscription] und ein [Visual Studio Team Services-Konto][link-vsts-account]. 
7. [Erstellen Sie einen Cluster in Azure.](service-fabric-tutorial-create-cluster-azure-ps.md)

## <a name="containerize-the-application"></a>Packen der Anwendung in Container

Sie besitzen einen [in Azure ausgeführten Service Fabric-Cluster](service-fabric-tutorial-create-cluster-azure-ps.md) und können nun eine Containeranwendung erstellen und bereitstellen. Um die Ausführung unserer Anwendung in einem Container zu starten, müssen wir **Docker-Unterstützung** zum Projekt in Visual Studio hinzufügen. Wenn Sie **Docker-Unterstützung** zur Anwendung hinzufügen, geschieht Folgendes: Zuerst wird dem Projekt eine _Dockerfile_-Datei hinzugefügt. Diese neue Datei beschreibt, wie das Containerimage erstellt werden soll. Zweitens wird ein neues _docker-compose_-Projekt zur Projektmappe hinzugefügt. Das neue Projekt enthält einige docker-compose-Dateien. Mit docker-compose-Dateien kann beschrieben werden, wie der Container ausgeführt wird.

Hier finden Sie weitere Informationen zum Arbeiten mit [Visual Studio-Containertools][link-visualstudio-container-tools].

>[!NOTE]
>Wenn Sie Windows-Containerimages zum ersten Mal auf Ihrem Computer ausführen, müssen die Basisimages für Ihre Container von Docker CE per Pull abgerufen werden. Die in diesem Tutorial verwendeten Images haben eine Größe von 14 GB. Fahren Sie fort, und führen Sie den folgenden Terminalbefehl aus, um die Basisimages per Pull abzurufen:
>```cmd
>docker pull microsoft/mssql-server-windows-developer
>docker pull microsoft/aspnet:4.6.2
>```

### <a name="add-docker-support"></a>Hinzufügen der Docker-Unterstützung

Öffnen Sie die Datei [FabrikamFiber.CallCenter.sln][link-fabrikam-github] in Visual Studio.

Klicken Sie mit der rechten Maustaste auf das Projekt **FabrikamFiber.Web**, und klicken Sie dann auf **Hinzufügen** > **Docker-Unterstützung**.

### <a name="add-support-for-sql"></a>Hinzufügen von Unterstützung für SQL

Diese Anwendung verwendet SQL als Datenanbieter, daher ist eine SQL Server-Instanz zum Ausführen der Anwendung erforderlich. Verweisen Sie in der Datei „docker-compose.override.yml“ auf ein SQL Server-Containerimage.

Öffnen Sie in Visual Studio den **Projektmappen-Explorer**, navigieren Sie zu **docker-compose**, und öffnen Sie die Datei **docker-compose.override.yml**.

Navigieren Sie zum Knoten `services:`, und fügen Sie einen Knoten namens `db:` hinzu, der den SQL Server-Eintrag für den Container definiert.

```yml
  db:
    image: microsoft/mssql-server-windows-developer
    environment:
      sa_password: "Password1"
      ACCEPT_EULA: "Y"
    ports:
      - "1433"
    healthcheck:
      test: [ "CMD", "sqlcmd", "-U", "sa", "-P", "Password1", "-Q", "select 1" ]
      interval: 1s
      retries: 20
```

>[!NOTE]
>Sie können für lokales Debuggen jede beliebigen SQL Server-Instanz verwenden, solange sie von Ihrem Host aus erreichbar ist. Allerdings unterstützt **localdb** keine `container -> host`-Kommunikation.

>[!WARNING]
>Wenn SQL Server in einem Container ausgeführt wird, wird das dauerhafte Speichern von Daten nicht unterstützt. Wenn der Container beendet wird, werden Ihre Daten gelöscht. Verwenden Sie diese Konfiguration nicht für die Produktion.

Navigieren Sie zum Knoten `fabrikamfiber.web:`, und fügen Sie einen untergeordneten Knoten mit dem Namen `depends_on:` hinzu. Dadurch wird sichergestellt, dass der `db`-Dienst (der SQL Server-Container) vor unserer Webanwendung (fabrikamfiber.web) gestartet wird.

```yml
  fabrikamfiber.web:
    depends_on:
      - db
```

### <a name="update-the-web-config"></a>Aktualisieren der Webkonfiguration

Aktualisieren Sie im Projekt **FabrikamFiber.Web** die Verbindungszeichenfolge in der Datei **web.config** so, dass sie auf die SQL Server-Instanz im Container verweist.

```xml
<add name="FabrikamFiber-Express" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />

<add name="FabrikamFiber-DataWarehouse" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />
```

>[!NOTE]
>Wenn Sie eine andere SQL Server-Instanz beim Erstellen eines Releasebuilds Ihrer Webanwendung verwenden möchten, fügen Sie eine weitere Verbindungszeichenfolge zu Ihrer web.release.config-Datei hinzu.

### <a name="test-your-container"></a>Testen des Containers

Drücken Sie **F5**, um die Anwendung in Ihrem Container auszuführen und zu debuggen.

Edge öffnet die definierte Startseite Ihrer Anwendung unter Verwendung der IP-Adresse des Containers im internen NAT-Netzwerk (in der Regel 172.x.x.x). Weitere Informationen zum Debuggen von Anwendungen in Containern mit Visual Studio 2017 finden Sie [in diesem Artikel][link-debug-container].

![Beispiel für Fabrikam in einem Container][image-web-preview]

Der Container kann jetzt erstellt und in einer Service Fabric-Anwendung verpackt werden. Nachdem Sie das Containerimage auf dem Computer erstellt haben, können Sie es per Push an eine beliebige Containerregistrierung weiterleiten, und per Pull auf jeden beliebigen Host zur Ausführung abrufen.

## <a name="get-the-application-ready-for-the-cloud"></a>Vorbereiten der Anwendung für die Cloud

Zum Vorbereiten der Anwendung für die Ausführung in Service Fabric in Azure müssen zwei Schritte ausgeführt werden:

1. Verfügbarmachen des Ports, über den die Webanwendung im Service Fabric-Cluster erreichbar sein soll
2. Angeben einer produktionsbereiten SQL-Datenbank für die Anwendung

### <a name="expose-the-port-for-the-app"></a>Verfügbarmachen des Ports für die App
Für den konfigurierten Service Fabric-Cluster ist standardmäßig Port *80* im Azure Load Balancer geöffnet, der einen Lastenausgleich des eingehenden Datenverkehrs an den Cluster bewirkt. Der Container kann über die Datei „docker-compose.yml“ über diesen Port verfügbar gemacht werden.

Öffnen Sie in Visual Studio den **Projektmappen-Explorer**, navigieren Sie zu **docker-compose**, und öffnen Sie die Datei **docker-compose.override.yml**.

Ändern Sie den Knoten `fabrikamfiber.web:`, und fügen Sie einen untergeordneten Knoten mit dem Namen `ports:` hinzu.

Fügen Sie den Zeichenfolgeneintrag `- "80:80"` hinzu.

```yml
  version: '3'

  services:
    fabrikamfiber.web:
      image: fabrikamfiber.web
      build:
        context: .\FabrikamFiber.Web
        dockerfile: Dockerfile
      ports:
        - "80:80"
```

### <a name="use-a-production-sql-database"></a>Verwenden von SQL-Datenbank für die Produktion
Bei der Ausführung in der Produktion müssen die Daten dauerhaft in der Datenbank gespeichert werden. Derzeit besteht keine Möglichkeit, die dauerhafte Speicherung von Daten in einem Container sicherzustellen. Daher können Sie Produktionsdaten in SQL Server nicht in einem Container speichern.

Es wird empfohlen, Azure SQL-Datenbank zu nutzen. Informationen zum Einrichten und Ausführen einer verwalteten SQL Server-Instanz in Azure finden Sie im Artikel [Dokumentation zu Azure SQL-Datenbank][link-azure-sql].

>[!NOTE]
>Denken Sie daran, die Verbindungszeichenfolgen in der Datei **web.release.config** im Projekt **FabrikamFiber.Web** in die SQL Server-Instanz zu ändern.
>
>Diese Anwendung schlägt ordnungsgemäß fehl, wenn keine SQL-Datenbank erreichbar ist. Sie können fortfahren und die Anwendung ohne eine SQL Server-Instanz bereitstellen.

## <a name="deploy-with-visual-studio-team-services"></a>Bereitstellen mit Visual Studio Team Services

Um die Bereitstellung mithilfe von Visual Studio Team Services einzurichten, müssen Sie die [Erweiterung für die Continuous Delivery-Tools für Visual Studio 2017][link-visualstudio-cd-extension] installieren. Diese Erweiterung vereinfacht die Bereitstellung in Azure durch Konfigurieren von Visual Studio Team Services und Bereitstellen der App in Ihrem Service Fabric-Cluster.

Zunächst muss Ihr Code in der Quellcodeverwaltung gehostet werden. Im restlichen Teil dieses Abschnitts wird davon ausgegangen, dass **Git** verwendet wird.

### <a name="set-up-a-vsts-repo"></a>Einrichten eines VSTS-Repositorys
Klicken Sie unten rechts in Visual Studio auf **Zur Quellcodeverwaltung hinzufügen** > **Git** (bzw. die bevorzugte Option).

![Schaltfläche der Quellcodeverwaltung][image-source-control]

Klicken Sie im _Team Explorer_-Bereich auf **Git-Repository veröffentlichen**.

Wählen Sie den Namen des VSTS-Repositorys aus, und klicken Sie auf **Repository**.

![Repository in VSTS veröffentlichen][image-publish-repo]

Nachdem Ihr Code nun mit einem VSTS-Quellrepository synchronisiert wurde, können Sie Continuous Integration und Continuous Delivery konfigurieren.

### <a name="setup-continuous-delivery"></a>Einrichten der Continuous Delivery

Klicken Sie im _Projektmappen-Explorer_ mit der rechten Maustaste auf **Projektmappe** > **Continuous Delivery konfigurieren**.

Wählen Sie das Azure-Abonnement aus.

Legen Sie **Hosttyp** auf **Service Fabric-Cluster** fest.

Legen Sie **Zielhost** auf den Service Fabric-Cluster fest, den Sie im vorherigen Abschnitt erstellt haben.

Wählen Sie eine **Containerregistrierung** aus, in der Sie den Container veröffentlichen.

>[!TIP]
>Verwenden Sie die Schaltfläche **Bearbeiten**, um eine Containerregistrierung zu erstellen.

Klicken Sie auf **OK**.

![Continuous Integration für Service Fabric einrichten][image-setup-ci]
   
   Nachdem die Konfiguration abgeschlossen ist, wird der Container in Service Fabric bereitgestellt. Immer wenn Sie Updates per Push an das Repository übertragen, werden ein neues Build und eine neue Release ausgeführt.
   
   >[!NOTE]
   >Die Erstellung der Containerimages dauert ca. 15 Minuten.
   >Bei der ersten Bereitstellung im Service Fabric-Cluster werden die grundlegenden Windows Server Core-Containerimages heruntergeladen. Der Download dauert weitere 5 bis 10 Minuten.

Browsen Sie über die URL des Clusters zu der Anwendung Fabrikam Call Center, z.B. *http://mycluster.westeurope.cloudapp.azure.com*.

Nachdem Sie nun die Anwendung in Container gepackt und die Fabrikam Call Center-Projektmappe bereitgestellt haben, können Sie das [Azure-Portal][link-azure-portal] öffnen und die Ausführung der Anwenung in Service Fabric anzeigen. Um die Anwendung zu testen, öffnen Sie einen Webbrowser, und navigieren Sie zur URL des Service Fabric-Clusters.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines Docker-Projekts in Visual Studio
> * Packen einer vorhandenen Anwendung in Container
> * Einrichten der Continuous Integration mit Visual Studio und VSTS

<!--   NOTE SURE WHAT WE SHOULD DO YET HERE

Advance to the next tutorial to learn how to bind a custom SSL certificate to it.

> [!div class="nextstepaction"]
> [Bind an existing custom SSL certificate to Azure Web Apps](app-service-web-tutorial-custom-ssl.md)

## Next steps

- [Container Tooling in Visual Studio][link-visualstudio-container-tools]
- [Get started with containers in Service Fabric][link-servicefabric-containers]
- [Creating Service Fabric applications][link-servicefabric-createapp]
-->

[link-debug-container]: /dotnet/articles/core/docker/visual-studio-tools-for-docker
[link-fabrikam-github]: https://aka.ms/fabrikamcontainer
[link-container-quickstart]: /virtualization/windowscontainers/quick-start/quick-start-windows-10
[link-visualstudio-container-tools]: /dotnet/articles/core/docker/visual-studio-tools-for-docker
[link-azure-powershell-install]: /powershell/azure/install-azurerm-ps
[link-servicefabric-create-secure-clusters]: service-fabric-cluster-creation-via-arm.md
[link-visualstudio-cd-extension]: https://aka.ms/cd4vs
[link-servicefabric-containers]: service-fabric-get-started-containers.md
[link-servicefabric-createapp]: service-fabric-create-your-first-application-in-visual-studio.md
[link-azure-portal]: https://portal.azure.com
[link-sf-clustertemplate]: https://aka.ms/securepreviewonelineclustertemplate
[link-azure-pricing-calculator]: https://azure.microsoft.com/en-us/pricing/calculator/
[link-azure-subscription]: https://azure.microsoft.com/en-us/free/
[link-vsts-account]: https://www.visualstudio.com/team-services/pricing/
[link-azure-sql]: /azure/sql-database/

[image-web-preview]: media/service-fabric-host-app-in-a-container/fabrikam-web-sample.png
[image-source-control]: media/service-fabric-host-app-in-a-container/add-to-source-control.png
[image-publish-repo]: media/service-fabric-host-app-in-a-container/publish-repo.png
[image-setup-ci]: media/service-fabric-host-app-in-a-container/configure-continuous-integration.png

