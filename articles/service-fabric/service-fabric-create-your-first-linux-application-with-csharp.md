---
title: Erstellen Ihrer ersten Azure-Microservices-App unter Linux mithilfe von C# | Microsoft-Dokumentation
description: Erstellen und Bereitstellen einer Service Fabric-Anwendung mithilfe von C#
services: service-fabric
documentationcenter: csharp
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 5a96d21d-fa4a-4dc2-abe8-a830a3482fb1
ms.service: service-fabric
ms.devlang: csharp
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/02/2017
ms.author: subramar
translationtype: Human Translation
ms.sourcegitcommit: 7033955fa9c18b2fa1a28d488ad5268d598de287
ms.openlocfilehash: c810f3e86ba582943e88f3085f6d9cff2496031c
ms.lasthandoff: 01/24/2017


---
# <a name="create-your-first-azure-service-fabric-application"></a>Erstellen Ihrer ersten Azure Service Fabric-Anwendung
> [!div class="op_single_selector"]
> * [C# – Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java – Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# – Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
> 
> 

Service Fabric bietet SDKs, mit denen sich Dienste unter Linux sowohl in .NET Core als auch in Java erstellen lassen. In diesem Tutorial erfahren Sie, wie Sie eine Anwendung für Linux und einen Dienst unter Verwendung von C# (.NET Core) erstellen.

## <a name="prerequisites"></a>Voraussetzungen
Vor Beginn des Tutorials müssen Sie zunächst [Ihre Linux-Entwicklungsumgebung einrichten](service-fabric-get-started-linux.md). Bei Verwendung von Mac OS X können Sie [mithilfe von Vagrant eine Linux-One-Box-Umgebung auf einem virtuellen Computer einrichten](service-fabric-get-started-mac.md).

## <a name="create-the-application"></a>Erstellen der Anwendung
Eine Service Fabric-Anwendung kann einen oder mehrere Dienste enthalten, die jeweils eine bestimmte Rolle bei der Bereitstellung von Funktionen der Anwendung haben. Das Service Fabric SDK für Linux enthält ein [Yeoman](http://yeoman.io/) -Generator, mit dem Sie problemlos Ihren ersten Dienst erstellen und später weitere Dienste hinzufügen können. Im nächsten Schritt erstellen wir mithilfe von Yeoman eine Anwendung mit einem einzelnen Dienst.

1. Geben Sie an einem Terminal den folgenden Befehl ein, um mit dem Erstellen des Gerüsts zu beginnen: `yo azuresfcsharp`
2. Benennen Sie Ihre Anwendung.
3. Wählen Sie die Art Ihres ersten Diensts aus, und benennen Sie ihn. Im Rahmen dieses Tutorials wählen wir einen Reliable Actor-Dienst aus.
   
   ![Service Fabric-Yeoman-Generator für C#][sf-yeoman]

> [!NOTE]
> Weitere Informationen zu den Optionen finden Sie unter [Übersicht über die Service Fabric-Programmiermodelle](service-fabric-choose-framework.md).
> 
> 

## <a name="build-the-application"></a>Erstellen der Anwendung
Die Yeoman-Vorlagen von Service Fabric enthalten ein Buildskript. Damit können Sie (nach Navigation zum Anwendungsordner) die App über das Terminal erstellen.

  ```sh
 cd myapp 
 ./build.sh 
  ```

## <a name="deploy-the-application"></a>Bereitstellen der Anwendung
Die erstellte Anwendung kann mithilfe der Azure-Befehlszeilenschnittstelle im lokalen Cluster bereitgestellt werden.

1. Stellen Sie eine Verbindung mit dem lokalen Service Fabric-Cluster her.
   
    ```sh
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
3. Klicken Sie auf den Knoten, den Sie im vorherigen Schritt ermittelt haben, und wählen Sie im Aktionsmenü die Option **Deaktivieren (neu starten)** aus. Mit dieser Aktion wird ein Knoten in Ihrem lokalen Cluster neu gestartet und ein Failover auf ein sekundäres Replikat erzwungen, das auf einem anderen Knoten ausgeführt wird. Behalten Sie bei dieser Aktion die Ausgabe des Testclients im Auge, und beachten Sie, dass sich der Zähler trotz des Failovers weiter erhöht.

## <a name="adding-more-services-to-an-existing-application"></a>Hinzufügen weiterer Dienste zu einer vorhandenen Anwendung

Führen Sie zum Hinzufügen eines weiteren Diensts zu einer Anwendung, die bereits mit `yo` erstellt wurde, die folgenden Schritte aus: 
1. Legen Sie das Verzeichnis auf den Stamm der vorhandenen Anwendung fest.  Beispiel: `cd ~/YeomanSamples/MyApplication`, wenn `MyApplication` die von Yeoman erstellte Anwendung ist.
2. Führen Sie `yo azuresfcsharp:AddService` aus.

## <a name="next-steps"></a>Nächste Schritte
* [Erfahren Sie mehr über Reliable Actors.](service-fabric-reliable-actors-introduction.md)
* [Interagieren mit einem Service Fabric-Cluster mithilfe der Azure-Befehlszeilenschnittstelle](service-fabric-azure-cli.md)
* Informieren Sie sich über [Service Fabric-Supportoptionen](service-fabric-support.md).

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-csharp/yeoman-csharp.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-csharp/sfx-primary.png

