---
title: Erstellen Ihrer ersten Azure-Microservices-App unter Linux mithilfe von Java | Microsoft-Dokumentation
description: Erstellen und Bereitstellen einer Service Fabric-Anwendung mithilfe von Java
services: service-fabric
documentationcenter: java
author: seanmck
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/05/2017
ms.author: seanmck
translationtype: Human Translation
ms.sourcegitcommit: 9553c9ed02fa198d210fcb64f4657f84ef3df801
ms.openlocfilehash: eedddf7a40acfba7513efd810d115f1afe2f224d
ms.lasthandoff: 03/23/2017


---
# <a name="create-your-first-azure-service-fabric-application"></a>Erstellen Ihrer ersten Azure Service Fabric-Anwendung
> [!div class="op_single_selector"]
> * [C# – Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java – Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# – Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

Service Fabric bietet SDKs, mit denen sich Dienste unter Linux sowohl in .NET Core als auch in Java erstellen lassen. In diesem Tutorial erfahren Sie, wie Sie unter Verwendung von Java eine Anwendung für Linux und einen Dienst erstellen.  

> [!NOTE]
> Java als erstklassige integrierte Programmiersprache wird nur für die Linux-Vorschauversion unterstützt (Windows-Unterstützung ist geplant). Allerdings können alle Anwendungen, einschließlich Java-Anwendungen, als ausführbare Gastdateien oder in Containern unter Windows oder Linux ausgeführt werden. Weitere Informationen finden Sie unter [Bereitstellen einer ausführbaren Gastanwendungsdatei in Service Fabric](service-fabric-deploy-existing-app.md) und [Vorschau: Bereitstellen eines Containers in Service Fabric](service-fabric-deploy-container.md).
>

## <a name="video-tutorial"></a>Videotutorial

Im folgenden Microsoft Virtual Academy-Video wird die Vorgehensweise zum Erstellen einer Java-App unter Linux veranschaulicht:  
<center><a target="\_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=DOX8K86yC_206218965">  
<img src="./media/service-fabric-create-your-first-linux-application-with-java/LinuxVid.png" WIDTH="360" HEIGHT="244">  
</a></center>


## <a name="prerequisites"></a>Voraussetzungen
Vor Beginn des Tutorials müssen Sie zunächst [Ihre Linux-Entwicklungsumgebung einrichten](service-fabric-get-started-linux.md). Bei Verwendung von Mac OS X können Sie [mithilfe von Vagrant eine Linux-One-Box-Umgebung auf einem virtuellen Computer einrichten](service-fabric-get-started-mac.md).

## <a name="create-the-application"></a>Erstellen der Anwendung
Eine Service Fabric-Anwendung kann einen oder mehrere Dienste enthalten, die jeweils eine bestimmte Rolle bei der Bereitstellung von Funktionen der Anwendung haben. Das Service Fabric SDK für Linux enthält ein [Yeoman](http://yeoman.io/) -Generator, mit dem Sie problemlos Ihren ersten Dienst erstellen und später weitere Dienste hinzufügen können. Im nächsten Schritt erstellen wir mithilfe von Yeoman eine Anwendung mit einem einzelnen Dienst.

1. Geben Sie in einem Terminal ``yo azuresfjava`` ein.
2. Benennen Sie Ihre Anwendung.
3. Wählen Sie die Art Ihres ersten Diensts aus, und benennen Sie ihn. Im Rahmen dieses Tutorials wählen wir einen Reliable Actor-Dienst aus.

   ![Service Fabric-Yeoman-Generator für Java][sf-yeoman]

> [!NOTE]
> Weitere Informationen zu den Optionen finden Sie unter [Übersicht über die Service Fabric-Programmiermodelle](service-fabric-choose-framework.md).
>

## <a name="build-the-application"></a>Erstellen der Anwendung
Die Yeoman-Vorlagen von Service Fabric enthalten ein Buildskript für [Gradle](https://gradle.org/). Damit können Sie die App über das Terminal erstellen.

  ```bash
  cd myapp
  gradle
  ```

## <a name="deploy-the-application"></a>Bereitstellen der Anwendung
Die erstellte Anwendung kann mithilfe der Azure-Befehlszeilenschnittstelle im lokalen Cluster bereitgestellt werden.

1. Stellen Sie eine Verbindung mit dem lokalen Service Fabric-Cluster her.

    ```bash
    azure servicefabric cluster connect
    ```

2. Verwenden Sie das in der Vorlage bereitgestellte Installationsskript, um das Anwendungspaket in den Imagespeicher des Clusters zu kopieren, den Anwendungstyp zu registrieren und eine Instanz der Anwendung zu erstellen.

    ```bash
    ./install.sh
    ```

3. Navigieren Sie in einem Browser zu Service Fabric Explorer (http://localhost:19080/Explorer). Falls Sie Vagrant unter Mac OS X verwenden, ersetzen Sie „localhost“ durch die private IP-Adresse des virtuellen Computers.

4. Erweitern Sie den Knoten „Anwendungen“. Hier finden Sie nun einen Eintrag für Ihren Anwendungstyp und einen weiteren für die erste Instanz dieses Typs.

## <a name="start-the-test-client-and-perform-a-failover"></a>Starten des Testclients und Ausführen eines Failovers
Actor-Projekte führen keine eigenständigen Aktionen durch. Sie benötigen einen anderen Dienst oder Client, der ihnen Nachrichten sendet. Die Actor-Vorlage enthält ein einfaches Testskript, das Sie für die Interaktion mit dem Actor-Dienst verwenden können.

1. Führen Sie das Skript mithilfe des watch-Hilfsprogramms aus, um die Ausgabe des Actor-Diensts zu erhalten.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. Suchen Sie in Service Fabric Explorer den Knoten, der das primäre Replikat für den Actor-Dienst hostet. Im folgenden Screenshot ist das „Node_3“:

    ![Suchen des primären Replikats in Service Fabric Explorer][sfx-primary]

3. Klicken Sie auf den Knoten, den Sie im vorherigen Schritt ermittelt haben, und wählen Sie im Aktionsmenü die Option **Deaktivieren (neu starten)** aus. Mit dieser Aktion wird einer der fünf Knoten in Ihrem lokalen Cluster neu gestartet und ein Failover auf eines der sekundären Replikate erzwungen, das auf einem anderen Knoten ausgeführt wird. Behalten Sie bei dieser Aktion die Ausgabe des Testclients im Auge, und beachten Sie, dass sich der Zähler trotz des Failovers weiter erhöht.

## <a name="create-and-deploy-an-application-with-the-eclipse-neon-plugin"></a>Erstellen und Bereitstellen einer Anwendung mit dem Eclipse Neon-Plug-In

Service Fabric gibt Ihnen außerdem die Möglichkeit zum Erstellen und Bereitstellen von Service Fabric-Java-Anwendungen mithilfe von Eclipse. Wählen Sie bei der Installation von Eclipse die **Eclipse-IDE für Java-Entwickler**. Außerdem unterstützt Service Fabric derzeit das Plug-In für Eclipse **Neon**. Lesen Sie die ausführliche Dokumentation [Erstellen und Bereitstellen Ihrer ersten Service Fabric-Java-Anwendung mithilfe des Service Fabric-Plug-Ins für Eclipse unter Linux](service-fabric-get-started-eclipse.md).

## <a name="adding-more-services-to-an-existing-application"></a>Hinzufügen weiterer Dienste zu einer vorhandenen Anwendung

### <a name="using-command-line-utility"></a>Verwenden des Befehlszeilenprogramms
Führen Sie zum Hinzufügen eines weiteren Diensts zu einer Anwendung, die bereits mit `yo` erstellt wurde, die folgenden Schritte aus:
1. Legen Sie das Verzeichnis auf den Stamm der vorhandenen Anwendung fest.  Beispiel: `cd ~/YeomanSamples/MyApplication`, wenn `MyApplication` die von Yeoman erstellte Anwendung ist.
2. Führen Sie `yo azuresfjava:AddService` aus.

### <a name="using-service-fabric-eclipse-plugin-for-java-on-linux"></a>Verwenden des Service Fabric Eclipse-Plug-Ins für Java unter Linux
Informationen zum Hinzufügen von Diensten zu einer vorhandenen Anwendung, die mit dem Eclipse-Plug-In für Service Fabric erstellt wurde, finden Sie in [dieser](service-fabric-get-started-eclipse.md#add-a-service-fabric-service-to-your-service-fabric-application) Dokumentation.

## <a name="next-steps"></a>Nächste Schritte
* [Erstellen und Bereitstellen Ihrer ersten Service Fabric-Java-Anwendung mithilfe des Service Fabric-Plug-Ins für Eclipse unter Linux](service-fabric-get-started-eclipse.md)
* [Erfahren Sie mehr über Reliable Actors.](service-fabric-reliable-actors-introduction.md)
* [Interagieren mit einem Service Fabric-Cluster mithilfe der Azure-Befehlszeilenschnittstelle](service-fabric-azure-cli.md)
* [Problembehandlung bei der Bereitstellung](service-fabric-azure-cli.md#troubleshooting)
* Informieren Sie sich über [Service Fabric-Supportoptionen](service-fabric-support.md).

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png

