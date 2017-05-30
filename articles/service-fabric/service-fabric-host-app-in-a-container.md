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
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/10/2017
ms.author: mikhegn
ms.translationtype: Human Translation
ms.sourcegitcommit: 5e92b1b234e4ceea5e0dd5d09ab3203c4a86f633
ms.openlocfilehash: a965fcb9dd5caf9656af5ae381b25baaaa01ec6d
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---

# <a name="deploy-a-net-app-in-a-container-to-azure-service-fabric"></a>Bereitstellen einer .NET-App in einem Container in Azure Service Fabric

In diesem Tutorial wird gezeigt, wie Sie eine vorhandene ASP.NET-Anwendung in einem Windows-Container mit Visual Studio 2017 Update 3 Preview bereitstellen. Anschließend wird beschrieben, wie Sie den Container in Azure mithilfe von Visual Studio Team Services bereitstellen und den Container in einem Service Fabric-Cluster hosten.

## <a name="prerequisites"></a>Voraussetzungen

1. Installieren Sie [Docker CE für Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description), damit Sie Container unter Windows 10 ausführen können.
2. Machen Sie sich mit den [Schnellstart für Windows 10-Container][link-container-quickstart] vertraut.
3. In diesem Artikel verwenden wir **Fabrikam Fiber**, eine Beispiel-App, die Sie [hier][link-fabrikam-github] herunterladen können.
4. [Azure PowerShell][link-azure-powershell-install]
5. [Erweiterung für die Continuous Delivery-Tools für Visual Studio 2017][link-visualstudio-cd-extension]

>[!NOTE]
>Wenn Sie Windows-Containerimages zum ersten Mal auf Ihrem Computer ausführen, müssen die Basisimages für Ihre Container von Docker CE per Pull abgerufen werden. Die in diesem Tutorial verwendeten Images sind 14 GB groß. Fahren Sie fort, und führen Sie den folgenden Befehl in Powershell aus, um die Basisimages per Pull abzurufen:
>
>```cmd
>docker pull microsoft/mssql-server-windows-developer
>docker pull microsoft/aspnet:4.6.2
>```

## <a name="containerize-the-application"></a>Packen der Anwendung in Container

Um die Ausführung unserer Anwendung in einem Container zu starten, müssen wir **Docker-Unterstützung** zum Projekt in Visual Studio hinzufügen. Wenn Sie **Docker-Unterstützung** zur Anwendung hinzufügen, geschieht Folgendes: Erstens wird dem Projekt eine Datei mit dem Namen _docker_ hinzugefügt. Diese neue Datei beschreibt, wie das Containerimage erstellt werden soll. Zweitens wird ein neues _docker-compose_-Projekt zur Projektmappe hinzugefügt. Dieses neue Projekt enthält einige docker-compose-Dateien, mit denen beschrieben werden kann, wie der Container ausgeführt wird.

Hier finden Sie weitere Informationen zum Arbeiten mit [Visual Studio-Containertools][link-visualstudio-container-tools].

### <a name="add-docker-support"></a>Hinzufügen der Docker-Unterstützung

1. Öffnen Sie die Datei **FabrikamFiber.CallCenter.sln** in Visual Studio.

2. Klicken Sie mit der rechten Maustaste auf das Projekt **FabrikamFiber.Web**, und klicken Sie dann auf **Hinzufügen** > **Docker-Unterstützung**.

### <a name="add-support-for-sql"></a>Hinzufügen von Unterstützung für SQL

Diese Anwendung verwendet SQL als Datenanbieter, daher ist eine SQL Server-Instanz zum Ausführen der Anwendung erforderlich. Wir verwenden in diesem Tutorial die SQL Server-Instanz in einem Container.
Um Docker anzuweisen, dass eine SQL-Server-Instanz in einem Container ausgeführt werden soll, können wir auf ein SQL Server-Containerimage in unserer „docker-compose.override.yml“-Datei im docker-compose-Projekt verweisen. Auf diese Weise wird die im Container ausgeführte SQL Server-Instanz beim Debuggen der Anwendung in Visual Studio verwendet.

1. Öffnen Sie den **Projektmappen-Explorer**.

2. Öffnen Sie **docker-compose** > **docker-compose.yml** > **docker-compose.override.yml**.

3. Fügen Sie unter dem `services:`-Knoten einen neuen Knoten mit dem Namen `db:` hinzu. Dieser Knoten übernimmt die Deklaration zum Ausführen einer SQL Server-Instanz in einem Container.

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

   >[!NOTE]
   >Wenn Sie SQL Server immer in einem Container ausführen möchten, können Sie auch die oben stehenden Angaben zur docker-compose.yml-Datei statt zur docker-compose.override.yml-Datei hinzufügen.


4. Ändern Sie den `fabrikamfiber.web`-Knoten, und fügen Sie einen neuen untergeordneten Knoten mit dem Namen `depends_on:` hizu. Dadurch wird sichergestellt, dass der `db`-Dienst (der SQL Server-Container) vor unserer Webanwendung (fabrikamfiber.web) gestartet wird.

   ```yml
     fabrikamfiber.web:
       ports:
         - "80"
       depends_on:
         - db
   ```

5. Aktualisieren Sie im Projekt **FabrikamFiber.Web** die Verbindungszeichenfolge in der Datei **web.config** so, dass sie auf die SQL Server-Instanz im Container verweist.

   ```xml
   <add name="FabrikamFiber-Express" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />
   
   <add name="FabrikamFiber-DataWarehouse" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />
   ```

   >[!NOTE]
   >Wenn Sie eine andere SQL Server-Instanz beim Erstellen eines Releasebuilds Ihrer Webanwendung verwenden möchten, fügen Sie eine weitere Verbindungszeichenfolge zu Ihrer web.release.config-Datei hinzu.

6. Drücken Sie **F5**, um die Anwendung in Ihrem Container auszuführen und zu debuggen.

   Edge öffnet die definierte Startseite Ihrer Anwendung unter Verwendung der IP-Adresse des Containers im internen NAT-Netzwerk (in der Regel 172.x.x.x). Weitere Informationen zum Debuggen von Anwendungen in Containern mit Visual Studio 2017 finden Sie [in diesem Artikel][link-debug-container].

   ![Beispiel für Fabrikam in einem Container][image-web-preview]

Die Anwendung kann nun erstellt und in einen Container gepackt werden. Nachdem Sie das Containerimage auf dem Computer erstellt haben, können Sie es per Push an eine beliebige Containerregistrierung weiterleiten, und per Pull auf jeden beliebigen Host zur Ausführung abrufen.

Für den Rest dieses Tutorials verwenden Sie Visual Studio Team Services, um den Container zu erstellen und bereitzustellen, per Push an eine Azure Container Registry zu senden und in Service Fabric unter Azure bereitzustellen.

## <a name="create-a-service-fabric-cluster"></a>Erstellen von Service Fabric-Clustern

Wenn Sie bereits über einen Service Fabric-Cluster zum Bereitstellen der Anwendung verfügen, können Sie diesen Schritt überspringen. Andernfalls fahren Sie mit dem Erstellen eines Service Fabric-Clusters fort.

>[!NOTE]
>Die folgende Prozedur erstellt einen, durch ein selbstsigniertes Zertifikat gesicherten Service Fabric-Cluster, der in einen im Rahmen der Bereitstellung erstellten Schlüsseltresor (KeyVault) platziert wird. Weitere Informationen zur Verwendung der Azure Active Directory-Authentifizierung finden Sie im Artikel [Erstellen eines Service Fabric-Clusters mithilfe von Azure Resource Manager][link-servicefabric-create-secure-clusters].

1. Laden Sie eine lokale Kopie der im Folgenden aufgeführten Azure-Vorlage und Parameterdateien herunter.
    * [Azure Resource Manager-Vorlage für Service Fabric][link-sf-clustertemplate]: Die Resource Manager-Vorlage, die einen Service Fabric-Cluster definiert.
    * [Vorlagenparameterdatei][link-sf-clustertemplate-parameters]: Eine Parameterdatei, mit der Sie die Bereitstellung des Clusters anpassen können.
2. Passen Sie die folgenden Parameter in der Parameterdatei an:
  
   | Parameter       | Beschreibung |
   | --------------- | ----------- |
   | clusterName     | Der Name Ihres Clusters. |
   | adminUserName   | Das lokale Administratorkonto auf dem virtuellen Clustercomputer. |
   | adminPassword   | Das Kennwort des lokalen Administratorkontos auf dem virtuellen Clustercomputer. |
   | clusterLocation | Die Azure-Region, in der der Cluster bereitgestellt werden soll. |
   | vmInstanceCount | Die Anzahl der Knoten im Cluster (kann 1 sein) |

3. Öffnen Sie **PowerShell**.
4. **Melden Sie sich bei Azure an**.

   ```powershell
   Login-AzureRmAccount
   ```

5. Wählen Sie das **Abonnement:** aus, in dem der Cluster bereitgestellt werden soll.

   ```powershell
   Select-AzureRmSubscription -SubscriptionId <subscription-id>
   ```

6. Erstellen und **Verschlüsseln Sie ein Kennwort** für das von Service Fabric verwendete Zertifikat.

   ```powershell
   $pwd = "<your password>" | ConvertTo-SecureString -AsPlainText -Force
   ```

7. **Erstellen Sie den Cluster**, indem Sie den folgenden Befehl ausführen:

   ```powershell
   New-AzureRmServiceFabricCluster 
       -TemplateFile C:\users\me\downloads\PreviewSecureClusters.json `
       -ParameterFile C:\users\me\downloads\myCluster.parameters.json `
       -CertificateOutputFolder C:\users\me\desktop\ `
       -CertificatePassword $pwd `
       -CertificateSubjectName "mycluster.westeurope.cloudapp.azure.com" `
       -ResourceGroupName myclusterRG
   ```

   >[!NOTE]
   >Der Parameter `-CertificateSubjectName` sollte auf den in der Parameterdatei angegebenen clusterName-Parameter und die mit der Azure-Region verknüpfte Domäne abgestimmt sein, z.B.: `clustername.eastus.cloudapp.azure.com`.
   
    Sobald die Konfiguration abgeschlossen ist, werden Informationen über den in Azure erstellten Cluster asusgegeben, und das Zertifikat wird in das -CertificateOutputFolder-Verzeichnis kopiert.

8. **Doppelklicken** Sie auf das Zertifikat, um es auf dem lokalen Computer zu installieren.

## <a name="deploy-with-visual-studio"></a>Bereitstellen mit Visual Studio 2013

Um die Bereitstellung mithilfe von Visual Studio Team Services einzurichten, müssen Sie die [Erweiterung für die Continuous Delivery-Tools für Visual Studio 2017][link-visualstudio-cd-extension] installieren. Diese Erweiterung vereinfacht die Bereitstellung in Azure durch Konfigurieren von Visual Studio Team Services und Bereitstellen der App in Ihrem Service Fabric-Cluster.

Zunächst muss Ihr Code in der Quellcodeverwaltung gehostet werden. Im restlichen Teil dieses Abschnitts wird davon ausgegangen, dass **Git** verwendet wird.

1. Klicken Sie unten rechts in Visual Studio auf **Zur Quellcodeverwaltung hinzufügen** > **Git** (bzw. die bevorzugte Option).

   ![Schaltfläche der Quellcodeverwaltung][image-source-control]

2. Klicken Sie im _Team Explorer_-Bereich auf **Git-Repository veröffentlichen**.

3. Wählen Sie den Namen des VSTS-Repositorys aus, und klicken Sie auf **Repository**.

   ![Repository in VSTS veröffentlichen][image-publish-repo]

Nachdem Ihr Code nun mit einem VSTS-Quellrepository synchronisiert wurde, können Sie Continuous Integration und Continuous Delivery konfigurieren.

1. Klicken Sie im _Projektmappen-Explorer_ mit der rechten Maustaste auf **Projektmappe** > **Continuous Delivery konfigurieren**.

2. Wählen Sie das Azure-Abonnement aus.

3. Legen Sie **Hosttyp** auf **Service Fabric-Cluster** fest.

   >[!NOTE]
   >Je nach Containertyp, den Sie erstellen, werden weitere Optionen zum Hosten der Anwendung in Containern in Azure hinzugefügen. 

4. Legen Sie **Zielhost** auf den Service Fabric-Cluster fest, den Sie im vorherigen Abschnitt erstellt haben.

5. Wählen Sie eine **Containerregistrierung** aus, in der Sie den Container veröffentlichen.

   >[!TIP]
   >Verwenden Sie die Schaltfläche **Bearbeiten**, um eine Containerregistrierung zu erstellen.
    
6. Klicken Sie auf **OK**.

   ![Continuous Integration für Service Fabric einrichten][image-setup-ci]
   
   Sobald die Konfiguration abgeschlossen ist, wird Ihr Container jedes Mal in Service Fabric bereitgestellt, wenn Sie Updates per Push an das Repository übertragen.

7. **Starten Sie einen Build** mit **Team Explorer**, und sehen Sie, wie Ihre Containeranwendung in Service Fabric ausgeführt wird.

Nachdem Sie nun die Anwendung in Container gepackt und die Fabrikam Call Center-Projektmappe bereitgestellt haben, können Sie das [Azure-Portal][link-azure-portal] öffnen und die Ausführung der Anwenung in Service Fabric anzeigen. Um die Anwendung zu testen, öffnen Sie einen Webbrowser, und navigieren Sie zur URL des Service Fabric-Clusters.

## <a name="next-steps"></a>Nächste Schritte

- [Containertools in Visual Studio][link-visualstudio-container-tools]
- [Erste Schritte mit Containern in Service Fabric][link-servicefabric-containers]
- [Erstellen von Service Fabric-Anwendungen][link-servicefabric-createapp]

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
[link-sf-clustertemplate-parameters]: https://aka.ms/securepreviewonelineclusterparameters

[image-web-preview]: media/service-fabric-host-app-in-a-container/fabrikam-web-sample.png
[image-source-control]: media/service-fabric-host-app-in-a-container/add-to-source-control.png
[image-publish-repo]: media/service-fabric-host-app-in-a-container/publish-repo.png
[image-setup-ci]: media/service-fabric-host-app-in-a-container/configure-continuous-integration.png

