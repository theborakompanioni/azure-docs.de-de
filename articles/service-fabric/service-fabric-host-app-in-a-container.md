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
ms.date: 05/19/2017
ms.author: mikhegn
ms.translationtype: Human Translation
ms.sourcegitcommit: d9ae8e8948d82b9695d7d144d458fe8180294084
ms.openlocfilehash: 56788914452c0f3a7072d6b2805866072b9b7fea
ms.contentlocale: de-de
ms.lasthandoff: 05/23/2017

---

# <a name="deploy-a-net-app-in-a-container-to-azure-service-fabric"></a>Bereitstellen einer .NET-App in einem Container in Azure Service Fabric

In diesem Tutorial wird gezeigt, wie Sie eine vorhandene ASP.NET-Anwendung in einem Windows-Container mit Visual Studio 2017 Update 3 Preview bereitstellen. Anschließend wird beschrieben, wie Sie den Container in Azure mithilfe von Visual Studio Team Services bereitstellen und den Container in einem Service Fabric-Cluster hosten.

## <a name="prerequisites"></a>Voraussetzungen

1. Installieren Sie [Docker CE für Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description), damit Sie Container unter Windows 10 ausführen können.
2. Machen Sie sich mit den [Schnellstart für Windows 10-Container][link-container-quickstart] vertraut.
3. In diesem Tutorial verwenden wir **Fabrikam Fiber CallCenter**, eine Beispiel-App, die Sie [hier][link-fabrikam-github] herunterladen können.
4. [Azure PowerShell][link-azure-powershell-install]
5. [Erweiterung für die Continuous Delivery-Tools für Visual Studio 2017][link-visualstudio-cd-extension]
6. Ein [Azure-Abonnement][link-azure-subscription] und ein [Visual Studio Team Services-Konto][link-vsts-account]. Für dieses Tutorial können Sie kostenlose Tarife aller Dienste verwenden.

>[!NOTE]
>Wenn Sie Windows-Containerimages zum ersten Mal auf Ihrem Computer ausführen, müssen die Basisimages für Ihre Container von Docker CE per Pull abgerufen werden. Die in diesem Tutorial verwendeten Images haben eine Größe von 14 GB. Fahren Sie fort, und führen Sie den folgenden Befehl in Powershell aus, um die Basisimages per Pull abzurufen:
>1. docker pull microsoft/mssql-server-windows-developer
>2. docker pull microsoft/aspnet:4.6.2

## <a name="containerize-the-application"></a>Packen der Anwendung in Container

Um die Ausführung unserer Anwendung in einem Container zu starten, müssen wir **Docker-Unterstützung** zum Projekt in Visual Studio hinzufügen. Wenn Sie **Docker-Unterstützung** zur Anwendung hinzufügen, geschieht Folgendes: Erstens wird dem Projekt eine Datei mit dem Namen _docker_ hinzugefügt. Diese neue Datei beschreibt, wie das Containerimage erstellt werden soll. Zweitens wird ein neues _docker-compose_-Projekt zur Projektmappe hinzugefügt. Das neue Projekt enthält einige docker-compose-Dateien. Mit docker-compose-Dateien kann beschrieben werden, wie der Container ausgeführt wird.

Hier finden Sie weitere Informationen zum Arbeiten mit [Visual Studio-Containertools][link-visualstudio-container-tools].

### <a name="add-docker-support"></a>Hinzufügen der Docker-Unterstützung

1. Öffnen Sie die Datei **FabrikamFiber.CallCenter.sln** in Visual Studio.

2. Klicken Sie mit der rechten Maustaste auf das Projekt **FabrikamFiber.Web**, und klicken Sie dann auf **Hinzufügen** > **Docker-Unterstützung**.

### <a name="add-support-for-sql"></a>Hinzufügen von Unterstützung für SQL

Diese Anwendung verwendet SQL als Datenanbieter, daher ist eine SQL Server-Instanz zum Ausführen der Anwendung erforderlich. In diesem Tutorial verwenden wir die SQL Server-Instanz in einem Container für das lokale Debuggen.
Zum Ausführen einer SQL Server-Instanz in einem Container beim Debuggen können wir auf ein SQL Server-Containerimage in der Datei „docker-compose.override.yml“ verweisen. 

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

   >[!WARNING]
   >Wenn SQL Server in einem Container ausgeführt wird, wird das dauerhafte Speichern von Daten beim Beenden des Containers nicht unterstützt. Verwenden Sie diese Konfiguration nicht für die Produktion.

4. Ändern Sie den `fabrikamfiber.web`-Knoten, und fügen Sie einen neuen untergeordneten Knoten mit dem Namen `depends_on:` hizu. Dadurch wird sichergestellt, dass der `db`-Dienst (der SQL Server-Container) vor unserer Webanwendung (fabrikamfiber.web) gestartet wird.

   ```yml
     fabrikamfiber.web:
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

Für den Rest dieses Tutorials verwenden Sie Visual Studio Team Services, um den Container in Service Fabric unter Azure bereitzustellen.

## <a name="create-a-service-fabric-cluster"></a>Erstellen von Service Fabric-Clustern

Wenn Sie bereits über einen Service Fabric-Cluster zum Bereitstellen der Anwendung verfügen, können Sie diesen Schritt überspringen. Andernfalls fahren Sie mit dem Erstellen eines Service Fabric-Clusters fort.

>[!NOTE]
>Mit dem folgenden Verfahren wird ein durch ein selbstsigniertes Zertifikat gesicherter Service Fabric-Einzelknoten-Vorschaucluster (aus einem einzelnen virtuellen Computer bestehend) erstellt, der in Key Vault platziert wird.
>Einzelknotencluster können nicht über einen virtuellen Computer hinaus skaliert werden, und Vorschaucluster können nicht auf neuere Versionen aktualisiert werden.
>Zum Berechnen der Kosten, die durch Ausführen eines Service Fabric-Clusters in Azure anfallen, verwenden Sie den [Azure-Preisrechner][link-azure-pricing-calculator].
>Weitere Informationen zum Erstellen von Service Fabric-Clustern finden Sie im Artikel [Erstellen eines Service Fabric-Clusters mithilfe von Azure Resource Manager][link-servicefabric-create-secure-clusters].

1. Laden Sie eine lokale Kopie der Azure Resource Manager-Vorlage und die Parameterdatei aus folgendem GitHub-Repository herunter:
    * [Azure Resource Manager-Vorlage für Service Fabric][link-sf-clustertemplate]
       - **azuredeploy.json:** die Azure Resource Manager-Vorlage, die einen Service Fabric-Cluster definiert.
       - **azuredeploy.parameters.json:** eine Parameterdatei, mit der Sie die Bereitstellung des Clusters anpassen können.
2. Passen Sie die folgenden Parameter in der Parameterdatei an:
  
   | Parameter       | Beschreibung | Empfohlener Wert |
   | --------------- | ----------- | --------------- |
   | clusterLocation | Die Azure-Region, in der der Cluster bereitgestellt werden soll. | *Beispielsweise westeurope, eastasia, eastus* |
   | clusterName     | Der Name Ihres Clusters. | *Beispielsweise bobs-sfpreviewcluster* |
   | adminUserName   | Das lokale Administratorkonto auf dem virtuellen Clustercomputer. | *Alle gültigen Windows Server-Benutzernamen* |
   | adminPassword   | Das Kennwort des lokalen Administratorkontos auf dem virtuellen Clustercomputer. | *Alle gültigen Windows Server-Kennwörter* |
   | clusterCodeVersion | Die auszuführende Service Fabric-Version. (255.255.X.255 sind Vorschauversionen.) | **255.255.5713.255** |
   | vmInstanceCount | Die Anzahl der virtuellen Computer in Ihrem Cluster (kann 1 oder 3-99 sein). | **1** |

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
          -TemplateFile C:\Users\me\Desktop\azuredeploy.json `
          -ParameterFile C:\Users\me\Desktop\azuredeploy.parameters.json `
          -CertificateOutputFolder C:\Users\me\Desktop\ `
          -CertificatePassword $pwd `
          -CertificateSubjectName "mycluster.westeurope.cloudapp.azure.com" `
          -ResourceGroupName myclusterRG
   ```

   >[!NOTE]
   >Der Parameter `-CertificateSubjectName` sollte auf den in der Parameterdatei angegebenen clusterName-Parameter und die mit der Azure-Region verknüpfte Domäne abgestimmt sein, z.B.: `clustername.eastus.cloudapp.azure.com`.
   
    Sobald die Konfiguration abgeschlossen ist, werden Informationen über den in Azure erstellten Cluster asusgegeben, und das Zertifikat wird in das -CertificateOutputFolder-Verzeichnis kopiert.

8. **Doppelklicken** Sie auf das Zertifikat, um den Zertifikatimport-Assistenten zu öffnen.

9. Verwenden Sie die Standardeinstellungen, aktivieren Sie jedoch das Kontrollkästchen **Schlüssel als exportierbar markieren.** im Schritt **Schutz für den privaten Schlüssel**. Bei der Konfiguration von Azure Container Registry für die Authentifizierung des Service Fabric-Clusters im weiteren Verlauf des Tutorials muss das Zertifikat in Visual Studio exportiert werden.

10. Sie können nun Service Fabric Explorer in einem Browser öffnen. Die URL ist der **ManagementEndpoint** in der Ausgabe des PowerShell-Cmdlets, z.B. *https://mycluster.westeurope.cloudapp.azure.com:19080*. 

>[!NOTE]
>Beim Öffnen von Service Fabric Explorer wird ein Zertifikatfehler angezeigt, da Sie ein selbstsigniertes Zertifikat verwenden. In Edge müssen Sie auf *Details* und dann auf den Link *Webseite trotzdem laden* klicken. In Chrome müssen Sie auf *Advanced* (Erweitert) und dann auf den Link *proceed* (Fortfahren) klicken.

>[!NOTE]
>Wenn bei der Clustererstellung ein Fehler auftritt, können Sie den Befehl, mit dem die bereits bereitgestellten Ressourcen aktualisiert werden, erneut ausführen. Wenn ein Zertifikat als Teil der fehlerhaften Bereitstellung erstellt wurde, wird ein neues generiert. Informationen zum Beheben von Fehlern bei der Clustererstellung finden Sie im Artikel [Erstellen eines Service Fabric-Clusters in Azure mithilfe von Azure Resource Manager][link-servicefabric-create-secure-clusters].

## <a name="getting-the-application-ready-for-the-cloud"></a>Vorbereiten der Anwendung für die Cloud

Zum Vorbereiten der Anwendung für die Ausführung in Service Fabric in Azure müssen zwei Schritte ausgeführt werden:

1. Verfügbarmachen des Ports, über den die Webanwendung im Service Fabric-Cluster erreichbar sein soll
2. Angeben einer produktionsbereiten SQL-Datenbank für die Anwendung

### <a name="1-exposing-the-web-application-in-service-fabric"></a>1. Verfügbarmachen der Webanwendung in Service Fabric
Für den konfigurierten Service Fabric-Cluster ist standardmäßig Port 80 im Azure Load Balancer geöffnet, der einen Lastenausgleich des eingehenden Datenverkehrs an den Cluster bewirkt. Der Container kann über die Datei „docker-compose.yml“ über diesen Port verfügbar gemacht werden.

1. Öffnen Sie den **Projektmappen-Explorer**.

2. Öffnen Sie **docker-compose** > **docker-compose.yml**.

3. Ändern Sie den Knoten `fabrikamfiber.web`, und fügen Sie den neuen untergeordneten Knoten `ports:` und die Zeichenfolge `- "80:80"` hinzu. Die vollständige Datei „docker-compose.yml“ sollte wie folgt aussehen:

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

### <a name="2-provide-a-production-ready-sql-database-for-our-application"></a>2. Angeben einer produktionsbereiten SQL-Datenbank für die Anwendung
Beim Erstellen einer Containerversion dieser Anwendung und Aktivieren des lokalen Debuggens haben wir die Anwendung so eingerichtet, dass SQL Server in einem Container ausgeführt wird. Dieser Ansatz ist eine gute Lösung beim lokalen Debuggen der Anwendung, da die Daten in der Datenbank nicht dauerhaft gespeichert werden müssen. Bei der Ausführung in der Produktion müssen die Daten jedoch dauerhaft in der Datenbank gespeichert werden. Derzeit besteht keine Möglichkeit, die dauerhafte Speicherung von Daten in einem Container sicherzustellen. Daher können Sie Produktionsdaten in SQL Server nicht in einem Container speichern.

Wenn für Ihren Dienst also eine persistente SQL-Datenbank erforderlich ist, empfiehlt sich die Verwendung einer Azure SQL-Datenbank. Informationen zum Einrichten und Ausführen einer verwalteten SQL Server-Instanz in Azure finden Sie im Artikel [Schnellstarts: Azure SQL-Datenbank][[link-azure-sql]].

>[!NOTE]
>Denken Sie daran, die Verbindungszeichenfolgen in der Datei „web.release.config“ im Projekt „FabrikamFiber.Web“ in die SQL Server-Instanz zu ändern.
>Diese Anwendung schlägt ordnungsgemäß fehl, wenn keine SQL-Datenbank erreichbar ist. Sie können fortfahren und die Anwendung ohne eine SQL Server-Instanz bereitstellen.

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
   
   Nachdem die Konfiguration abgeschlossen ist, wird der Container in Service Fabric bereitgestellt. Immer wenn Sie Updates per Push an das Repository übertragen, werden ein neues Build und eine neue Release ausgeführt.
   
   >[!NOTE]
   >Die Erstellung der Containerimages dauert ca. 15 Minuten.
   >Bei der ersten Bereitstellung im Service Fabric-Cluster werden die grundlegenden Windows Server Core-Containerimages heruntergeladen. Der Download dauert weitere 5 bis 10 Minuten.

7. Browsen Sie über die URL des Clusters zu der Anwendung Fabrikam Call Center, z.B. *http://mycluster.westeurope.cloudapp.azure.com*.

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
[link-azure-pricing-calculator]: https://azure.microsoft.com/en-us/pricing/calculator/
[link-azure-subscription]: https://azure.microsoft.com/en-us/free/
[link-vsts-account]: https://www.visualstudio.com/team-services/pricing/
[link-azure-sql]: /sql-database

[image-web-preview]: media/service-fabric-host-app-in-a-container/fabrikam-web-sample.png
[image-source-control]: media/service-fabric-host-app-in-a-container/add-to-source-control.png
[image-publish-repo]: media/service-fabric-host-app-in-a-container/publish-repo.png
[image-setup-ci]: media/service-fabric-host-app-in-a-container/configure-continuous-integration.png
