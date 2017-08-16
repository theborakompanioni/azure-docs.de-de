---
title: Erstellen einer .NET Service Fabric-Anwendung in Azure | Microsoft-Dokumentation
description: "Erstellen Sie eine .NET-Anwendung für Azure, indem Sie das Beispiel des Service Fabric-Schnellstarts verwenden."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: mikhegn
ms.translationtype: HT
ms.sourcegitcommit: 14915593f7bfce70d7bf692a15d11f02d107706b
ms.openlocfilehash: 530749275b720caefd7e7e57291b4bc0d313faf0
ms.contentlocale: de-de
ms.lasthandoff: 08/10/2017

---

# <a name="create-a-net-service-fabric-application-in-azure"></a>Erstellen einer .NET Service Fabric-Anwendung in Azure
Azure Service Fabric ist eine Plattform für verteilte Systeme zum Bereitstellen und Verwalten von skalierbaren und zuverlässigen Microservices und Containern. 

In diesem Schnellstart wird gezeigt, wie Sie Ihre erste .NET-Anwendung für Service Fabric bereitstellen. Am Ende verfügen Sie über eine Abstimmungsanwendung mit einem ASP.NET Core-Web-Front-End, mit der Abstimmungsergebnisse im Cluster in einem zustandsbehafteten Back-End-Dienst gespeichert werden.

![Screenshot der Anwendung](./media/service-fabric-quickstart-dotnet/application-screenshot.png)

Mithilfe dieser Anwendung erfahren Sie Folgendes:
> [!div class="checklist"]
> * Erstellen einer Anwendung mit .NET und Service Fabric
> * Verwenden von ASP.NET Core als Web-Front-End
> * Speichern von Anwendungsdaten in einem zustandsbehafteten Dienst
> * Lokales Debuggen Ihrer Anwendung
> * Bereitstellen der Anwendung in einem Cluster in Azure
> * Horizontales Hochskalieren der Anwendung über mehrere Knoten hinweg
> * Durchführen eines parallelen Anwendungsupgrades

## <a name="prerequisites"></a>Voraussetzungen
So führen Sie diesen Schnellstart durch:
1. [Installieren Sie Visual Studio 2017](https://www.visualstudio.com/) mit den Workloads **Azure-Entwicklung** und **ASP.NET und Webentwicklung**.
2. [Installieren Sie Git.](https://git-scm.com/)
3. [Installieren Sie das Microsoft Azure Service Fabric-SDK](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK).
4. Führen Sie den folgenden Befehl aus, um für Visual Studio die Bereitstellung im lokalen Service Fabric-Cluster zu ermöglichen:
    ```powershell
    Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
    ```

## <a name="download-the-sample"></a>Herunterladen des Beispiels
Führen Sie in einem Befehlsfenster den folgenden Befehl aus, um das Beispiel-App-Repository auf Ihren lokalen Computer zu klonen.
```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="run-the-application-locally"></a>Lokales Ausführen der Anwendung
Klicken Sie im Menü „Start“ mit der rechten Maustaste auf das Visual Studio-Symbol, und wählen Sie die Option **Als Administrator ausführen**. Um den Debugger an Ihre Dienste anzufügen, müssen Sie Visual Studio als Administrator ausführen.

Öffnen Sie die Visual Studio-Lösung **Voting.sln** aus dem geklonten Repository.

Drücken Sie **F5**, um die Anwendung bereitzustellen.

> [!NOTE]
> Wenn Sie die Anwendung zum ersten Mal ausführen und bereitstellen, erstellt Visual Studio einen lokalen Cluster für das Debuggen. Dieser Vorgang kann einige Zeit dauern. Der Status der Clustererstellung wird im Ausgabefenster von Visual Studio angezeigt.

Starten Sie nach Abschluss der Bereitstellung einen Browser, und öffnen Sie diese Seite: `http://localhost:8080` (Web-Front-End der Anwendung).

![Front-End der Anwendung](./media/service-fabric-quickstart-dotnet/application-screenshot-new.png)

Sie können jetzt einen Satz mit Abstimmungsoptionen hinzufügen und die Abstimmung freigeben. Die Anwendung führt alle Daten in Ihrem Service Fabric-Cluster aus und speichert sie dort, ohne dass eine separate Datenbank verwendet werden muss.

## <a name="walk-through-the-voting-sample-application"></a>Durchlaufen der Beispielanwendung für die Abstimmung
Die Abstimmungsanwendung besteht aus zwei Diensten:
- Web-Front-End-Dienst (VotingWeb): Ein ASP.NET Core-Web-Front-End-Dienst, der die Webseite bereitstellt und Web-APIs für die Kommunikation mit dem Back-End-Dienst verfügbar macht.
- Back-End-Dienst (VotingData): Ein ASP.NET Core-Web-Dienst, der eine API verfügbar macht, um die Abstimmungsergebnisse in einem zuverlässigen Wörterbuch zu speichern, das dauerhaft auf dem Datenträger vorhanden ist.

![Anwendungsdiagramm](./media/service-fabric-quickstart-dotnet/application-diagram.png)

Beim Abstimmen in der Anwendung treten die folgenden Ereignisse ein:
1. Ein JavaScript sendet die Abstimmungsanforderung als HTTP PUT-Anforderung an die Web-API im Web-Front-End-Dienst.

2. Der Web-Front-End-Dienst nutzt einen Proxy, um eine HTTP PUT-Anforderung zu lokalisieren und an den Back-End-Dienst weiterzuleiten.

3. Der Back-End-Dienst erhält die eingehende Anforderung und speichert das aktualisierte Ergebnis in einem zuverlässigen Wörterbuch, das auf mehreren Knoten im Cluster repliziert und dauerhaft auf dem Datenträger gespeichert wird. Alle Daten der Anwendung werden im Cluster gespeichert, sodass keine Datenbank erforderlich ist.

## <a name="debug-in-visual-studio"></a>Debuggen in Visual Studio
Beim Debuggen der Anwendung in Visual Studio verwenden Sie einen lokalen Service Fabric-Entwicklungscluster. Sie haben die Möglichkeit, Ihre Oberfläche für das Debuggen an Ihr Szenario anzupassen. In dieser Anwendung speichern wir Daten in unserem Back-End-Dienst, indem wir ein zuverlässiges Wörterbuch verwenden. Standardmäßig wird die Anwendung von Visual Studio entfernt, wenn Sie den Debugger beenden. Die Entfernung der Anwendung führt dazu, dass auch die Daten im Back-End-Dienst entfernt werden. Um die Daten zwischen den Debugsitzungen beizubehalten, können Sie den **Debugmodus für die Anwendung** als Eigenschaft im Projekt **Voting** in Visual Studio ändern.

Führen Sie die folgenden Schritte aus, um zu ermitteln, was im Code passiert:
1. Öffnen Sie die Datei **VotesController.cs**, und legen Sie in der **Put**-Methode der Web-API (Zeile 47) einen Breakpoint fest. Sie können in Visual Studio im Projektmappen-Explorer nach der Datei suchen.

2. Öffnen Sie die Datei **VoteDataController.cs**, und legen Sie in der **Put**-Methode dieser Web-API (Zeile 50) einen Breakpoint fest.

3. Wechseln Sie zurück in den Browser, und klicken Sie auf eine Abstimmungsoption, oder fügen Sie eine neue Abstimmungsoption hinzu. Sie stoßen im API-Controller des Web-Front-Ends auf den ersten Breakpoint.
    - An diesem Punkt sendet das JavaScript im Browser eine Anforderung an den Web-API-Controller im Front-End-Dienst.
    
    ![Front-End-Dienst „Stimme hinzufügen“](./media/service-fabric-quickstart-dotnet/addvote-frontend.png)

    - Zunächst erstellen wir die URL zum Reverseproxy für unseren Back-End-Dienst **(1)**.
    - Anschließend senden wir die HTTP-Anforderung PUT an den Reverseproxy **(2)**.
    - Zum Schluss geben wir die Antwort vom Back-End-Dienst an den Client zurück **(3)**.

4. Drücken Sie **F5**, um fortzufahren.
    - Sie befinden sich jetzt am Breakpoint im Back-End-Dienst.
    
    ![Back-End-Dienst „Stimme hinzufügen“](./media/service-fabric-quickstart-dotnet/addvote-backend.png)

    - In der ersten Zeile der Methode **(1)** verwenden wir das `StateManager`-Element, um ein zuverlässiges Wörterbuch mit dem Namen `counts` abzurufen bzw. hinzuzufügen.
    - Für alle Interaktionen mit Werten in einem zuverlässigen Wörterbuch ist eine Transaktion erforderlich. Diese Transaktion wird mithilfe der Anweisung **(2)** erstellt.
    - In der Transaktion aktualisieren wir dann den Wert des relevanten Schlüssels für die Abstimmungsoption und committen den Vorgang **(3)**. Nachdem die Rückgabe für die Commit-Methode durchgeführt wurde, werden die Daten im Wörterbuch aktualisiert und auf anderen Knoten im Cluster repliziert. Die Daten sind jetzt sicher im Cluster gespeichert, und der Back-End-Dienst kann das Failover auf andere Knoten durchführen, während die Daten weiterhin verfügbar sind.
5. Drücken Sie **F5**, um fortzufahren.

Drücken Sie **UMSCHALT+F5**, um die Debugsitzung zu beenden.

## <a name="deploy-the-application-to-azure"></a>Bereitstellen der Anwendung für Azure
Zum Bereitstellen der Anwendung in einem Cluster in Azure können Sie entweder Ihren eigenen Cluster erstellen oder einen Partycluster verwenden.

Partycluster sind kostenlose, zeitlich begrenzte Service Fabric-Cluster, die in Azure gehostet und vom Service Fabric-Team ausgeführt werden und in denen jeder Benutzer Anwendungen bereitstellen und mehr über die Plattform erfahren kann. [Befolgen Sie die Anweisungen](http://aka.ms/tryservicefabric), um Zugriff auf einen Partycluster zu erhalten. 

Informationen zum Erstellen Ihres eigenen Clusters finden Sie unter [Erstellen Ihres ersten Service Fabric-Clusters in Azure](service-fabric-get-started-azure-cluster.md).

> [!Note]
> Der Web-Front-End-Dienst ist für das Lauschen auf eingehenden Datenverkehr über Port 8080 konfiguriert. Stellen Sie sicher, dass der Port in Ihrem Cluster geöffnet ist. Wenn Sie den Partycluster verwenden, ist dieser Port geöffnet.
>

### <a name="deploy-the-application-using-visual-studio"></a>Bereitstellen der Anwendung mit Visual Studio
Nachdem die Anwendung nun bereit ist, können Sie sie direkt aus Visual Studio in einem Cluster bereitstellen.

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **Voting**, und wählen Sie **Veröffentlichen**. Das Dialogfeld „Veröffentlichen“ wird angezeigt.

    ![Dialogfeld „Veröffentlichen“](./media/service-fabric-quickstart-dotnet/publish-app.png)

2. Tragen Sie den Verbindungsendpunkt des Clusters in das Feld **Verbindungsendpunkt** ein, und klicken Sie auf **Veröffentlichen**. Wenn Sie sich für den Partycluster registrieren, wird der Verbindungsendpunkt im Browser angegeben. Beispiel: `winh1x87d1d.westus.cloudapp.azure.com:19000`.

3. Öffnen Sie einen Browser, und geben Sie die Clusteradresse ein, z.B. `http://winh1x87d1d.westus.cloudapp.azure.com`. Sie sollten jetzt sehen, dass die Anwendung im Cluster in Azure ausgeführt wird.

![Front-End der Anwendung](./media/service-fabric-quickstart-dotnet/application-screenshot-new-azure.png)

## <a name="scale-applications-and-services-in-a-cluster"></a>Skalieren von Anwendungen und Diensten in einem Cluster
Service Fabric-Dienste können für einen Cluster auf einfache Weise skaliert werden, um eine Änderung der Last für die Dienste auszugleichen. Sie skalieren einen Dienst, indem Sie die Anzahl von Instanzen ändern, die im Cluster ausgeführt werden. Sie haben mehrere Möglichkeiten, Ihre Dienste zu skalieren. Sie können Skripts oder Befehle aus PowerShell oder Azure CLI 2.0 verwenden. In diesem Beispiel verwenden wir Service Fabric Explorer.

Service Fabric Explorer wird in allen Service Fabric-Clustern ausgeführt und ist über einen Browser zugänglich, indem auf den HTTP-Verwaltungsport (19080) der Cluster zugegriffen wird, z.B. `http://winh1x87d1d.westus.cloudapp.azure.com:19080`.

Führen Sie die folgenden Schritte aus, um den Web-Front-End-Dienst zu skalieren:

1. Öffnen Sie Service Fabric Explorer in Ihrem Cluster, z.B. `http://winh1x87d1d.westus.cloudapp.azure.com:19080`.
2. Klicken Sie auf das Auslassungszeichen (drei Punkte) neben dem Knoten **fabric:/Voting/VotingWeb** in der Strukturansicht, und wählen Sie **Scale Service** (Dienst skalieren).

    ![Service Fabric Explorer](./media/service-fabric-quickstart-dotnet/service-fabric-explorer-scale.png)

    Sie können jetzt angeben, dass die Anzahl von Instanzen des Web-Front-End-Diensts skaliert werden soll.

3. Ändern Sie die Anzahl in **2**, und klicken Sie auf **Scale Service** (Dienst skalieren).
4. Klicken Sie in der Strukturansicht auf den Knoten **fabric:/Voting/VotingWeb**, und erweitern Sie den Partitionsknoten (durch eine GUID dargestellt).

    ![Service Fabric Explorer – Dienst skalieren](./media/service-fabric-quickstart-dotnet/service-fabric-explorer-scaled-service.png)

    Sie sehen nun, dass der Dienst über zwei Instanzen verfügt, und in der Strukturansicht ist zu erkennen, auf welchen Knoten die Instanzen ausgeführt werden.

Mit dieser einfachen Verwaltungsaufgabe haben wir die Ressourcen verdoppelt, die für unseren Front-End-Dienst zum Verarbeiten der Benutzerauslastung verfügbar sind. Es ist wichtig zu verstehen, dass Sie nicht mehrere Instanzen eines Diensts benötigen, damit er zuverlässig ausgeführt wird. Wenn ein Dienst ausfällt, wird von Service Fabric sichergestellt, dass im Cluster eine neue Dienstinstanz ausgeführt wird.

## <a name="perform-a-rolling-application-upgrade"></a>Durchführen eines parallelen Anwendungsupgrades
Beim Bereitstellen von neuen Updates für Ihre Anwendung führt Service Fabric den Rollout des Updates auf sichere Weise durch. Bei parallelen Upgrades kommt es während des Vorgangs nicht zu Ausfallzeiten, und bei Fehlern wird ein automatischer Rollback durchgeführt.

Gehen Sie zum Aktualisieren der Anwendung wie folgt vor:

1. Öffnen Sie in Visual Studio die Datei **Index.cshtml**. Sie können in Visual Studio im Projektmappen-Explorer nach der Datei suchen.
2. Ändern Sie die Überschrift auf der Seite, indem Sie Text hinzufügen. Beispiel:
    ```html
        <div class="col-xs-8 col-xs-offset-2 text-center">
            <h2>Service Fabric Voting Sample v2</h2>
        </div>
    ```
3. Speichern Sie die Datei.
4. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **Voting**, und wählen Sie **Veröffentlichen**. Das Dialogfeld „Veröffentlichen“ wird angezeigt.
5. Klicken Sie auf die Schaltfläche **Manifestversion**, um die Version des Diensts und der Anwendung zu ändern.
6. Ändern Sie die Version des **Code**-Elements unter **VotingWebPkg** beispielsweise in „2.0.0“, und klicken Sie auf **Speichern**.

    ![Dialogfeld zum Ändern der Version](./media/service-fabric-quickstart-dotnet/change-version.png)
7. Aktivieren Sie im Dialogfeld **Service Fabric-Anwendung veröffentlichen** das Kontrollkästchen „Upgrade der Anwendung ausführen“, und klicken Sie auf **Veröffentlichen**.

    ![Dialogfeld „Veröffentlichen“ – Einstellung für Upgrade](./media/service-fabric-quickstart-dotnet/upgrade-app.png)
8. Öffnen Sie Ihren Browser, und navigieren Sie zu der Clusteradresse an Port 19080, z.B. `http://winh1x87d1d.westus.cloudapp.azure.com:19080`.
9. Klicken Sie in der Strukturansicht auf den Knoten **Anwendungen** und dann im rechten Bereich auf **Upgrades in Progress** (Laufende Upgrades). Sie sehen, wie das Upgrade die Upgradedomänen in Ihrem Cluster durchläuft und wie sichergestellt wird, dass jede Domäne fehlerfrei ist, bevor mit der nächsten Domäne fortgefahren wird.
    ![Upgradeansicht im Service Fabric Explorer](./media/service-fabric-quickstart-dotnet/upgrading.png)

    Mit Service Fabric werden Upgrades sicher gemacht, indem nach dem Aktualisieren des Diensts auf einem Knoten im Cluster jeweils zwei Minuten gewartet wird. Rechnen Sie damit, dass der gesamte Updatevorgang ca. acht Minuten dauert.

10. Während das Upgrade durchgeführt wird, können Sie die Anwendung weiterverwenden. Da zwei Instanzen des Diensts im Cluster ausgeführt werden, erhalten einige Anforderungen unter Umständen eine aktualisierte Version der Anwendung, während für andere weiterhin die alte Version verwendet wird.

## <a name="next-steps"></a>Nächste Schritte
In diesem Schnellstart haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen einer Anwendung mit .NET und Service Fabric
> * Verwenden von ASP.NET Core als Web-Front-End
> * Speichern von Anwendungsdaten in einem zustandsbehafteten Dienst
> * Lokales Debuggen Ihrer Anwendung
> * Bereitstellen der Anwendung in einem Cluster in Azure
> * Horizontales Hochskalieren der Anwendung über mehrere Knoten hinweg
> * Durchführen eines parallelen Anwendungsupgrades

Weitere Informationen zu Service Fabric und .NET finden Sie im Tutorial zum folgenden Thema:
> [!div class="nextstepaction"]
> [.NET-Anwendung in Service Fabric](service-fabric-tutorial-create-dotnet-app.md)
