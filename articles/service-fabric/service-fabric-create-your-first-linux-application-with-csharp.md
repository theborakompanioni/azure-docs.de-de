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
ms.date: 7/27/2017
ms.author: subramar
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: 4baf144cc28eeff0ab8f8b60e837f8a2bad903af
ms.contentlocale: de-de
ms.lasthandoff: 07/01/2017

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

Außerdem empfiehlt es sich, die [Azure CLI 2.0](service-fabric-azure-cli-2-0.md) (empfohlen) oder [XPlat-CLI](service-fabric-azure-cli.md) für die Bereitstellung Ihrer Anwendung zu konfigurieren.

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

Die erstellte Anwendung kann im lokalen Cluster bereitgestellt werden.

### <a name="using-xplat-cli"></a>Verwendung der XPlat-CLI

1. Stellen Sie eine Verbindung mit dem lokalen Service Fabric-Cluster her.

    ```bash
    azure servicefabric cluster connect
    ```

2. Führen Sie das in der Vorlage bereitgestellte Installationsskript aus, um das Anwendungspaket in den Imagespeicher des Clusters zu kopieren, den Anwendungstyp zu registrieren und eine Instanz der Anwendung zu erstellen.

    ```bash
    ./install.sh
    ```

### <a name="using-azure-cli-20"></a>Mithilfe von Azure-CLI 2.0

Bei der Bereitstellung der erstellten Anwendung wird genau wie bei anderen Service Fabric-Anwendungen vorgegangen. Ausführliche Anweisungen finden Sie in der Dokumentation zum [Verwalten einer Service Fabric-Anwendung mit der Azure CLI](service-fabric-application-lifecycle-azure-cli-2-0.md).

Parameter für diesen Befehl finden Sie in den erstellten Manifesten im Anwendungspaket.

Öffnen Sie nach der Bereitstellung der Anwendung einen Browser, und navigieren Sie zu [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) unter [http://localhost:19080/Explorer](http://localhost:19080/Explorer).
Erweitern Sie dann den Knoten **Anwendungen**. Hier finden Sie nun einen Eintrag für Ihren Anwendungstyp und einen weiteren für die erste Instanz dieses Typs.

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

## <a name="migrating-from-projectjson-to-csproj"></a>Migrieren von project.json zu .csproj
1. Wenn Sie „dotnet migrate“ im Stammverzeichnis des Projekts ausführen, wird „project.json“ ins CSPROJ-Format migriert.
2. Aktualisieren Sie die Projektverweise auf CSPROJ-Dateien in den Projektdateien entsprechend.
3. Aktualisieren Sie die Projektdateinamen in „build.sh“ in CSPROJ-Dateien.

## <a name="next-steps"></a>Nächste Schritte
* [Erfahren Sie mehr über Reliable Actors.](service-fabric-reliable-actors-introduction.md)
* [Interagieren mit einem Service Fabric-Cluster mithilfe der Azure-Befehlszeilenschnittstelle](service-fabric-azure-cli.md)
* Informieren Sie sich über [Service Fabric-Supportoptionen](service-fabric-support.md).

## <a name="related-articles"></a>Verwandte Artikel

* [Getting started with Service Fabric and Azure CLI 2.0](service-fabric-azure-cli-2-0.md) (Erste Schritte mit Service Fabric und der Azure CLI 2.0)
* [Interagieren mit einem Service Fabric-Cluster mithilfe der Azure-Befehlszeilenschnittstelle](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-csharp/yeoman-csharp.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-csharp/sfx-primary.png

