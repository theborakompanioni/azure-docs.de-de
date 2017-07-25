---
title: Erstellen einer Azure Service Fabric Reliable Actors-Java-Anwendung unter Linux | Microsoft-Dokumentation
description: "Es wird beschrieben, wie Sie in fünf Minuten eine Service Fabric Reliable Actors-Anwendung erstellen und bereitstellen."
services: service-fabric
documentationcenter: java
author: rwike77
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: ryanwi
ms.translationtype: HT
ms.sourcegitcommit: 9afd12380926d4e16b7384ff07d229735ca94aaa
ms.openlocfilehash: 254f38a600ea4026120bc411368eeb01310e56b2
ms.contentlocale: de-de
ms.lasthandoff: 07/15/2017

---
# <a name="create-your-first-java-service-fabric-reliable-actors-application-on-linux"></a>Erstellen Ihrer ersten Service Fabric Reliable Actors-Java-Anwendung unter Linux

Dieser Schnellstart hilft Ihnen beim Erstellen Ihrer ersten Azure Service Fabric-Java-Anwendung in einer Linux-Entwicklungsumgebung in wenigen Minuten.  Wenn Sie fertig sind, verfügen Sie über eine einfache Java-Einzeldienstanwendung, die im lokalen Entwicklungscluster ausgeführt wird.  

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie beginnen, installieren Sie das Service Fabric-SDK und die Azure-Befehlszeilenschnittstelle und richten einen Entwicklungscluster in Ihrer [Linux-Entwicklungsumgebung](service-fabric-get-started-linux.md) ein. Bei Verwendung von Mac OS X können Sie [mithilfe von Vagrant eine Linux-Entwicklungsumgebung auf einem virtuellen Computer einrichten](service-fabric-get-started-mac.md).

Außerdem empfiehlt es sich, die [Azure CLI 2.0](service-fabric-azure-cli-2-0.md) (empfohlen) oder [XPlat-CLI](service-fabric-azure-cli.md) für die Bereitstellung Ihrer Anwendung zu konfigurieren.

## <a name="create-the-application"></a>Erstellen der Anwendung
Eine Service Fabric-Anwendung enthält einen oder mehrere Dienste, die jeweils eine bestimmte Rolle bei der Bereitstellung von Funktionen der Anwendung haben. Das Service Fabric SDK für Linux enthält ein [Yeoman](http://yeoman.io/) -Generator, mit dem Sie problemlos Ihren ersten Dienst erstellen und später weitere Dienste hinzufügen können.  Sie können auch Service Fabric-Java-Anwendungen mithilfe eines Plug-Ins für Eclipse entwickeln, erstellen und bereitstellen. Weitere Informationen finden Sie unter [Erstellen und Bereitstellen Ihrer ersten Java-Anwendung mithilfe von Eclipse](service-fabric-get-started-eclipse.md). In diesem Schnellstart verwenden Sie Yeoman, um eine Anwendung mit einem einzigen Dienst zu erstellen, der einen Zählerwert speichert und abruft.

1. Geben Sie in einem Terminal ``yo azuresfjava`` ein.
2. Benennen Sie Ihre Anwendung. 
3. Wählen Sie die Art Ihres ersten Diensts aus, und benennen Sie ihn. Im Rahmen dieses Tutorials wählen Sie einen Reliable Actor-Dienst aus. Weitere Informationen zu anderen Diensttypen finden Sie unter [Übersicht über die Service Fabric-Programmiermodelle](service-fabric-choose-framework.md).
   ![Service Fabric-Yeoman-Generator für Java][sf-yeoman]

## <a name="build-the-application"></a>Erstellen der Anwendung
Die Yeoman-Vorlagen von Service Fabric enthalten ein Buildskript für [Gradle](https://gradle.org/). Damit können Sie die Anwendung über das Terminal erstellen. Führen Sie Folgendes aus, um die Anwendung zu erstellen und zu packen:

  ```bash
  cd myapp
  gradle
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
Akteure führen selbst keine Aktionen durch. Sie benötigen einen anderen Dienst oder Client, der ihnen Nachrichten sendet. Die Actor-Vorlage enthält ein einfaches Testskript, das Sie für die Interaktion mit dem Actor-Dienst verwenden können.

1. Führen Sie das Skript mithilfe des watch-Hilfsprogramms aus, um die Ausgabe des Actor-Diensts zu erhalten.  Das Testskript ruft die `setCountAsync()`-Methode für den Akteur auf, um einen Zähler zu erhöhen, und die `getCountAsync()`-Methode für den Akteur, um den neuen Zählerwert abzurufen. Anschließend wird der Wert an der Konsole angezeigt.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. Suchen Sie in Service Fabric Explorer den Knoten, der das primäre Replikat für den Akteurdienst hostet. Im folgenden Screenshot ist das „Node_3“: Das primäre Dienstreplikat verarbeitet die Lese- und Schreibvorgänge.  Änderungen am Dienststatus werden dann an die sekundären Replikate, die auf den Knoten 0 und 1 im Screenshot unten ausgeführt werden, repliziert.

    ![Suchen des primären Replikats in Service Fabric Explorer][sfx-primary]

3. Klicken Sie in **Knoten** auf den Knoten, den Sie im vorherigen Schritt ermittelt haben, und wählen Sie im Aktionsmenü die Option **Deaktivieren (neu starten)** aus. Mit dieser Aktion wird der Knoten, auf dem das primäre Dienstreplikat ausgeführt wird, neu gestartet und ein Failover auf eines der sekundären Replikate erzwungen, das auf einem anderen Knoten ausgeführt wird.  Dieses sekundäre Replikat wird zum primären Replikat heraufgestuft. Ein weiteres sekundäres Replikat wird auf einem anderen Knoten erstellt, und das primäre Replikat beginnt damit, Lese- und Schreibvorgänge anzunehmen. Behalten Sie beim Neustart des Knotens die Ausgabe des Testclients im Auge, und beachten Sie, dass sich der Zähler trotz des Failovers weiter erhöht.

## <a name="remove-the-application"></a>Entfernen der Anwendung
Verwenden Sie das in der Vorlage bereitgestellte Deinstallationsskript, um die Anwendungsinstanz zu löschen, die Registrierung des Anwendungspakets aufzuheben und das Anwendungspaket aus dem Imagespeicher des Clusters zu entfernen.

```bash
./uninstall.sh
```

Im Service Fabric Explorer werden die Anwendung und der Anwendungstyp anschließend nicht mehr im Knoten **Anwendungen** angezeigt.

## <a name="next-steps"></a>Nächste Schritte
* [Erstellen Ihrer ersten Service Fabric-Java-Anwendung unter Linux mit Eclipse](service-fabric-get-started-eclipse.md)
* [Erfahren Sie mehr über Reliable Actors.](service-fabric-reliable-actors-introduction.md)
* [Interagieren mit Service Fabric-Clustern mithilfe der Azure-Befehlszeilenschnittstelle](service-fabric-azure-cli.md)
* [Problembehandlung bei der Bereitstellung](service-fabric-azure-cli.md#troubleshooting)
* Informieren Sie sich über [Service Fabric-Supportoptionen](service-fabric-support.md).

## <a name="related-articles"></a>Verwandte Artikel

* [Getting started with Service Fabric and Azure CLI 2.0](service-fabric-azure-cli-2-0.md) (Erste Schritte mit Service Fabric und der Azure CLI 2.0)
* [Interagieren mit einem Service Fabric-Cluster mithilfe der Azure-Befehlszeilenschnittstelle](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png

