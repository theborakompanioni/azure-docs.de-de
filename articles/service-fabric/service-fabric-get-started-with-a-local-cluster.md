---
title: Lokales Bereitstellen und Aktualisieren von Azure-Microservices | Microsoft Dokumentation
description: Erfahren Sie, wie Sie einen lokalen Service Fabric-Cluster einrichten, eine vorhandene Anwendung im Cluster bereitstellen und die Anwendung aktualisieren.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 60a1f6a5-5478-46c0-80a8-18fe62da17a8
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/15/2017
ms.author: ryanwi;mikhegn
translationtype: Human Translation
ms.sourcegitcommit: eddca02c4fba88aee667216568beecc76ea65d7c
ms.openlocfilehash: d1320daaf4b0bd8c1a7b7c8e37fa8b81c4a53e64
ms.lasthandoff: 01/25/2017


---
# <a name="get-started-with-deploying-and-upgrading-applications-on-your-local-cluster"></a>Erste Schritte beim Bereitstellen und Aktualisieren von Anwendungen im lokalen Cluster
Das Azure Service Fabric-SDK umfasst eine vollständige lokale Entwicklungsumgebung, die Sie verwenden können, um schnell mit der Bereitstellung und Verwaltung von Anwendungen in einem lokalen Cluster zu beginnen. In diesem Artikel verwenden Sie Windows PowerShell, um einen lokalen Cluster zu erstellen, eine vorhandene Anwendung bereitzustellen und die Anwendung auf eine neue Version zu aktualisieren.

> [!NOTE]
> In diesem Artikel wird vorausgesetzt, dass Sie bereits [eine Entwicklungsumgebung eingerichtet haben](service-fabric-get-started.md).
> 
> 

## <a name="create-a-local-cluster"></a>Erstellen eines lokalen Clusters
Ein Service Fabric-Cluster ist ein Satz von Hardwareressourcen, auf dem Sie Anwendungen bereitstellen können. Normalerweise umfasst ein Cluster zwischen fünf und mehreren Tausend Computern. Das Service Fabric-SDK enthält aber eine Clusterkonfiguration, die auf einem einzelnen Computer ausgeführt werden kann.

Sie müssen dabei bedenken, dass der lokale Service Fabric-Cluster kein Emulator oder Simulator ist. Er führt den gleichen Plattformcode wie Cluster mit mehreren Computern aus. Der einzige Unterschied ist, dass die Plattformprozesse, die normalerweise auf fünf Computer verteilt sind, auf einem Computer ausgeführt werden.

Das SDK bietet zwei Möglichkeiten zum Einrichten eines lokalen Clusters: ein Windows PowerShell-Skript und die Taskleisten-App Local Cluster Manager. In diesem Tutorial wird das PowerShell-Skript verwendet.

> [!NOTE]
> Wenn Sie bereits einen lokalen Cluster durch die Bereitstellung einer Anwendung über Visual Studio erstellt haben, können Sie diesen Abschnitt überspringen.
> 
> 

1. Starten Sie als Administrator ein neues PowerShell-Fenster.
2. Führen Sie das Skript für die Clustereinrichtung aus dem SDK-Ordner aus:
   
    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"
    ```
   
    Die Clustereinrichtung dauert einen Moment. Nach Abschluss des Setups wird in etwa die folgende Ausgabe angezeigt:
   
    ![Ausgabe bei der Clustereinrichtung][cluster-setup-success]
   
    Sie können nun damit beginnen, eine Anwendung in Ihrem Cluster bereitzustellen.

## <a name="deploy-an-application"></a>Bereitstellen von Anwendungen
Das Service Fabric-SDK umfasst eine Vielzahl von Frameworks und Entwicklertools zum Erstellen von Anwendungen. Wenn Sie lernen möchten, wie Sie Anwendungen in Visual Studio erstellen können, lesen Sie [Erstellen Ihrer ersten Service Fabric-Anwendung in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).

In diesem Tutorial verwenden wir eine bereits vorhandene Beispielanwendung namens „WordCount“, damit Sie sich auf die Verwaltungsaspekte der Plattform (Bereitstellung, Überwachung und Upgrade) konzentrieren können.

1. Starten Sie als Administrator ein neues PowerShell-Fenster.
2. Importieren Sie das Service Fabric-SDK für das PowerShell-Modul.
   
    ```powershell
    Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
    ```
3. Erstellen Sie ein Verzeichnis zum Speichern der Anwendung, die Sie herunterladen und bereitstellen möchten, z.B. „C:\ServiceFabric“.
   
    ```powershell
    mkdir c:\ServiceFabric\
    cd c:\ServiceFabric\
    ```
4. [Laden Sie die WordCount-Anwendung](http://aka.ms/servicefabric-wordcountapp) an den Speicherort herunter, den Sie erstellt haben.  Hinweis: Der Microsoft Edge-Browser speichert die Datei mit der Erweiterung *.zip* .  Ändern Sie die Dateierweiterung in *.sfpkg*.
5. Stellen Sie eine Verbindung mit dem lokalen Cluster her:
   
    ```powershell
    Connect-ServiceFabricCluster localhost:19000
    ```
6. Erstellen Sie eine neue Anwendung, indem Sie den Bereitstellungsbefehl des SDK mit einem Namen und Pfad zum Anwendungspaket verwenden.
   
    ```powershell  
   Publish-NewServiceFabricApplication -ApplicationPackagePath c:\ServiceFabric\WordCountV1.sfpkg -ApplicationName "fabric:/WordCount"
    ```
   
    Wenn alles funktioniert, wird die folgende Ausgabe angezeigt:
   
    ![Bereitstellen einer Anwendung im lokalen Cluster][deploy-app-to-local-cluster]
7. Um die Anwendung in Aktion zu sehen, starten Sie den Browser und navigieren zu [http://localhost:8081/wordcount/index.html](http://localhost:8081/wordcount/index.html). Folgendes sollte angezeigt werden:
   
    ![Benutzeroberfläche der bereitgestellten Anwendung][deployed-app-ui]
   
    Die WordCount-Anwendung ist einfach gehalten. Sie enthält clientseitigen JavaScript-Code zum Generieren von zufälligen „Wörtern“ mit fünf Zeichen, die dann per ASP.NET-Web-API an die Anwendung weitergeleitet werden. Bei einem zustandsbehafteten Dienst wird die Anzahl von Wörtern nachverfolgt. Sie werden basierend auf dem ersten Buchstaben des Worts partitioniert. Den Quellcode für die WordCount-App finden Sie unter [Getting Started Samples](https://azure.microsoft.com/documentation/samples/service-fabric-dotnet-getting-started/)(Beispiele für erste Schritte).
   
    Die Anwendung, die wir bereitgestellt haben, enthält vier Partitionen. Wörter, die mit A bis G beginnen, werden in der ersten Partition gespeichert, Wörter mit H bis N in der zweiten Partition usw.

## <a name="view-application-details-and-status"></a>Anzeigen von Anwendungsdetails und des Anwendungsstatus
Nach dem Bereitstellen der Anwendung sehen wir uns nun die App-Details in PowerShell an.

1. Fragen Sie alle bereitgestellten Anwendungen im Cluster ab:
   
    ```powershell
    Get-ServiceFabricApplication
    ```
   
    Falls Sie nur die WordCount-App bereitgestellt haben, sehen Sie etwa Folgendes:
   
    ![Abfrage aller bereitgestellten Anwendungen in PowerShell][ps-getsfapp]
2. Machen Sie den nächsten Schritt, indem Sie die Dienste abfragen, die in der WordCount-Anwendung enthalten sind.
   
    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```
   
    ![Auflisten von Diensten für die Anwendung in PowerShell][ps-getsfsvc]
   
    Die Anwendung besteht aus zwei Diensten: dem Web-Front-End- und dem zustandsbehafteten Dienst, der die Wörter verwaltet.
3. Sehen Sie sich schließlich die Liste mit den Partitionen für WordCountService an:
   
    ```powershell
    Get-ServiceFabricPartition 'fabric:/WordCount/WordCountService'
    ```
   
    ![Anzeigen der Dienstpartitionen in PowerShell][ps-getsfpartitions]
   
    Die Befehle, die Sie verwendet haben, sind wie alle Service Fabric-PowerShell-Befehle für alle Cluster verfügbar, mit denen Sie eine Verbindung herstellen – lokal oder remote.
   
    Für eine eher visuelle Möglichkeit der Interaktion mit dem Cluster können Sie das webbasierte Tool Service Fabric-Explorer verwenden, indem Sie im Browser zu [http://localhost:19080/Explorer](http://localhost:19080/Explorer) navigieren.
   
    ![Anzeigen von Anwendungsdetails im Service Fabric-Explorer][sfx-service-overview]
   
   > [!NOTE]
   > Weitere Informationen zum Service Fabric Explorer finden Sie unter [Visualisieren des Clusters mit dem Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).
   > 
   > 

## <a name="upgrade-an-application"></a>Upgraden einer Anwendung
Service Fabric ermöglicht Upgrades ohne Ausfallzeit, indem die Integrität der Anwendung überwacht wird, während sie im Cluster bereitgestellt wird. Führen Sie ein Upgrade der WordCount-Anwendung durch.

Die neue Version der Anwendung zählt nur die Wörter, die mit einem Vokal beginnen. Während das Upgrade bereitgestellt wird, sehen wir zwei Änderungen im Verhalten der Anwendung. Erstens sollte sich die Rate verlangsamen, mit der die Anzahl zunimmt, da weniger Wörter gezählt werden. Da die erste Partition zwei Vokale enthält („A“ und „E“) und alle anderen Partitionen nur einen enthalten, sollte zweitens die Anzahl dieser Partition letztlich größer als die der anderen sein.

1. [Laden Sie das Paket für Version 2 von WordCount](http://aka.ms/servicefabric-wordcountappv2) an den Speicherort herunter, an den Sie auch das Paket für Version 1 heruntergeladen haben.
2. Kehren Sie zum PowerShell-Fenster zurück, und verwenden Sie den Upgradebefehl des SDK, um die neue Version des Clusters zu registrieren. Beginnen Sie dann mit dem Upgrade der Anwendung „fabric:/WordCount“.
   
    ```powershell
    Publish-UpgradedServiceFabricApplication -ApplicationPackagePath C:\ServiceFabric\WordCountV2.sfpkg -ApplicationName "fabric:/WordCount" -UpgradeParameters @{"FailureAction"="Rollback"; "UpgradeReplicaSetCheckTimeout"=1; "Monitored"=$true; "Force"=$true}
    ```
   
    Wenn das Upgrade beginnt, sehen Sie in PowerShell die folgende Ausgabe.
   
    ![Upgradestatus in PowerShell][ps-appupgradeprogress]
3. Möglicherweise ist es für Sie während des Upgrades einfacher, dessen Status über den Service Fabric-Explorer zu überwachen. Öffnen Sie ein Browserfenster, und navigieren Sie zu [http://localhost:19080/Explorer](http://localhost:19080/Explorer). Erweitern Sie in der Struktur links **Anwendungen**, und wählen Sie **WordCount** und anschließend **fabric:/WordCount**. Auf der Registerkarte „Zusammenfassung“ sehen Sie den Status des Upgrades, während es die Upgradedomänen des Clusters durchläuft.
   
    ![Upgradestatus im Service Fabric-Explorer][sfx-upgradeprogress]
   
    Während das Upgrade in den einzelnen Domänen durchgeführt wird, werden Integritätsprüfungen ausgeführt, um sicherzustellen, dass sich die Anwendung richtig verhält.
4. Wenn Sie die vorherige Abfrage für die Dienste der Anwendung „fabric:/WordCount“ erneut ausführen, passiert Folgendes: Die WordCountService-Version ändert sich, die WordCountWebService-Version hingegen nicht. Dies ist hier dargestellt:
   
    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```
   
    ![Abfragen der Anwendungsdienste nach dem Upgrade][ps-getsfsvc-postupgrade]
   
    Dieses Beispiel verdeutlicht, wie Anwendungsupgrades von Service Fabric verwaltet werden. Es ist nur die Gruppe der Dienste (bzw. der Code-/Konfigurationspakete in diesen Diensten) betroffen, die sich geändert haben, und der Upgradevorgang wird schneller und zuverlässiger.
5. Kehren Sie schließlich zum Browser zurück, um das Verhalten der neuen Anwendungsversion zu beobachten. Die Anzahl steigt wie erwartet langsamer an, und die erste Partition enthält eine etwas größere Zahl.
   
    ![Anzeigen der neuen Version der Anwendung im Browser][deployed-app-ui-v2]

## <a name="cleaning-up"></a>Bereinigen
Bevor Sie Ihre Arbeit abschließen, sollten Sie bedenken, dass der lokale Cluster real ist. Anwendungen werden im Hintergrund so lange weiter ausgeführt, bis Sie sie entfernen.  Je nach Art Ihrer Apps kann eine ausgeführte App erhebliche Ressourcen auf dem Computer beanspruchen. Sie haben mehrere Optionen zum Verwalten der Anwendungen und des Clusters:

1. Führen Sie den folgenden Befehl aus, um eine einzelne Anwendung und alle dazugehörigen Daten zu entfernen:
   
    ```powershell
    Unpublish-ServiceFabricApplication -ApplicationName "fabric:/WordCount"
    ```
   
    Alternativ können Sie die Anwendung über das Menü **AKTIONEN** von Service Fabric Explorer oder über das Kontextmenü in der Anwendungsliste auf der linken Seite löschen.
   
    ![Löschen einer Anwendung in Service Fabric-Explorer][sfe-delete-application]
2. Heben Sie nach dem Löschen der Anwendung aus dem Cluster die Registrierung der Versionen 1.0.0 und 2.0.0 des WordCount-Anwendungstyps auf. Beim Löschen werden die Anwendungspakete, einschließlich des Codes und der Konfiguration, aus dem Imagespeicher des Clusters entfernt.
   
    ```powershell
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 2.0.0
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 1.0.0
    ```
   
    Oder wählen Sie im Service Fabric Explorer für die Anwendung die Option **Unprovision Type** (Bereitstellung des Typs aufheben).
3. Zum Beenden des Clusters bei Beibehaltung der Anwendungsdaten und Ablaufverfolgungen klicken Sie in der Infobereichs-App auf **Lokalen Cluster beenden** .
4. Zum vollständigen Entfernen des Clusters klicken Sie in der Infobereichs-App auf **Lokalen Cluster entfernen** . Diese Option führt zu einer weiteren langsamen Bereitstellung, wenn Sie das nächste Mal in Visual Studio F5 drücken. Entfernen Sie den lokalen Cluster nur, wenn Sie nicht beabsichtigen, ihn einige Zeit zu verwenden, oder wenn Sie Ressourcen freigeben müssen.

## <a name="one-node-and-five-node-cluster-mode"></a>Clustermodus mit einem und fünf Knoten
Die Anwendungsentwicklung ist häufig geprägt von zahlreichen Iterationen der folgenden Schritte: Schreiben von Code, Debuggen, Ändern von Code, Debuggen. Zur Optimierung dieses Prozesses kann der lokale Cluster in zwei Modi ausgeführt werden: „Ein Knoten“ oder „Fünf Knoten“. Beide Clustermodi haben bestimmte Vorteile. Im Clustermodus mit fünf Knoten können Sie mit einem echten Cluster arbeiten. Sie können Failoverszenarien testen und mit mehr Instanzen und Replikaten Ihres Diensts arbeiten. Der Clustermodus mit einem einzelnen Knoten wurde für die schnelle Bereitstellung und Registrierung von Diensten optimiert, damit Sie mit der Service Fabric-Laufzeit schnell Code überprüfen können.

Bei den Clustermodi mit einem oder fünf Knoten handelt es sich nicht um einen Emulator oder Simulator. Der Cluster für die lokale Entwicklung führt den gleichen Plattformcode wie ein Cluster mit mehreren Computern aus.

> [!WARNING]
> Wenn Sie den Clustermodus ändern, wird der aktuelle Cluster aus Ihrem System entfernt und ein neuer Cluster erstellt. Die im Cluster gespeicherten Daten werden gelöscht, wenn Sie den Clustermodus ändern.
> 
> 

Um zum Clustermodus mit einem einzelnen Knoten zu wechseln, wählen Sie im Service Fabric-Manager für lokale Cluster die Option **Switch Cluster Mode** (Clustermodus ändern) aus.

![Wechseln des Clustermodus][switch-cluster-mode]

Alternativ können Sie den Clustermodus auch mithilfe von PowerShell ändern:

1. Starten Sie als Administrator ein neues PowerShell-Fenster.
2. Führen Sie das Skript für die Clustereinrichtung aus dem SDK-Ordner aus:
   
    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateOneNodeCluster
    ```
   
    Die Clustereinrichtung dauert einen Moment. Nach Abschluss des Setups wird in etwa die folgende Ausgabe angezeigt:
   
    ![Ausgabe bei der Clustereinrichtung][cluster-setup-success-1-node]

## <a name="next-steps"></a>Nächste Schritte
* Nachdem Sie nun einige vordefinierte Anwendungen bereitgestellt und aktualisiert haben, können Sie [selbst eine Anwendung in Visual Studio erstellen](service-fabric-create-your-first-application-in-visual-studio.md).
* Alle Aktionen, die für den lokalen Cluster in diesem Artikel ausgeführt werden, können auch in einem [Azure-Cluster](service-fabric-cluster-creation-via-portal.md) ausgeführt werden.
* Das Upgrade, das wir in diesem Artikel durchgeführt haben, ist einfach. In der [Dokumentation zu Upgrades](service-fabric-application-upgrade.md) erfahren Sie mehr über die Leistungsfähigkeit und Flexibilität von Service Fabric-Upgrades.

<!-- Images -->

[cluster-setup-success]: ./media/service-fabric-get-started-with-a-local-cluster/LocalClusterSetup.png
[extracted-app-package]: ./media/service-fabric-get-started-with-a-local-cluster/ExtractedAppPackage.png
[deploy-app-to-local-cluster]: ./media/service-fabric-get-started-with-a-local-cluster/DeployAppToLocalCluster.png
[deployed-app-ui]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-v1.png
[deployed-app-ui-v2]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-PostUpgrade.png
[sfx-app-instance]: ./media/service-fabric-get-started-with-a-local-cluster/SfxAppInstance.png
[sfx-two-app-instances-different-partitions]: ./media/service-fabric-get-started-with-a-local-cluster/SfxTwoAppInstances-DifferentPartitionCount.png
[ps-getsfapp]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFApp.png
[ps-getsfsvc]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc.png
[ps-getsfpartitions]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFPartitions.png
[ps-appupgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/PS-AppUpgradeProgress.png
[ps-getsfsvc-postupgrade]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc-PostUpgrade.png
[sfx-upgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/SfxUpgradeOverview.png
[sfx-service-overview]: ./media/service-fabric-get-started-with-a-local-cluster/sfx-service-overview.png
[sfe-delete-application]: ./media/service-fabric-get-started-with-a-local-cluster/sfe-delete-application.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
[switch-cluster-mode]: ./media/service-fabric-get-started-with-a-local-cluster/switch-cluster-mode.png

