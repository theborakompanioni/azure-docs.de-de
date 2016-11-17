---
title: Einrichten Ihrer Entwicklungsumgebung unter Linux | Microsoft Docs
description: "Installieren Sie die Laufzeit und das SDK, und erstellen Sie einen lokalen Entwicklungscluster unter Linux. Nach Abschluss des Setups können Sie mit der Erstellung von Clientanwendungen beginnen."
services: service-fabric
documentationcenter: .net
author: seanmck
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/26/2016
ms.author: seanmck
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 567a998102558626df73878865b317b830ba1faa


---
# <a name="prepare-your-development-environment-on-linux"></a>Vorbereiten Ihrer Entwicklungsumgebung unter Linux
> [!div class="op_single_selector"]
> -[ Windows](service-fabric-get-started.md)
> 
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
> 
> 

 Zur Bereitstellung und Ausführung von [Azure Service Fabric-Anwendungen](service-fabric-application-model.md) auf Ihrem Linux-Entwicklungscomputer müssen Sie die Laufzeit und das allgemeine SDK installieren. Darüber hinaus können Sie auch optionale SDKs für Java und .NET Core installieren.

## <a name="prerequisites"></a>Voraussetzungen
### <a name="supported-operating-system-versions"></a>Unterstützte Betriebssystemversionen
Die folgenden Betriebssystemversionen werden bei der Entwicklung unterstützt:

* Ubuntu 16.04 (Xenial Xerus)

## <a name="update-your-apt-sources"></a>Aktualisieren Ihrer apt-Quellen
Um das SDK und das dazugehörige Laufzeitpaket über „apt-get“ installieren zu können, müssen Sie zunächst Ihre apt-Datenquellen aktualisieren.

1. Öffnen Sie ein Terminal.
2. Fügen Sie der Quellenliste das Service Fabric-Repository hinzu.
   
    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ trusty main" > /etc/apt/sources.list.d/servicefabric.list'
    ```
3. Fügen Sie Ihrem apt-Schlüsselbund den neuen GPG-Schlüssel hinzu.
   
    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    ```
4. Aktualisieren Sie Ihre Paketlisten auf der Grundlage der neu hinzugefügten Repositorys.
   
    ```bash
    sudo apt-get update
    ```

## <a name="install-and-set-up-the-sdk"></a>Installieren und Einrichten des SDKs
Nach der Aktualisierung Ihrer Datenquellen können Sie das SDK installieren.

1. Installieren Sie das Service Fabric-SDK-Paket. Sie werden aufgefordert, die Installation zu bestätigen und einem Lizenzvertrag zuzustimmen.
   
    ```bash
    sudo apt-get install servicefabricsdkcommon
    ```
2. Führen Sie das SDK-Setupskript aus.
   
    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/sdkcommonsetup.sh
    ```

## <a name="set-up-the-azure-crossplatform-cli"></a>Einrichten der plattformübergreifenden Azure-Befehlszeilenschnittstelle
Die [plattformübergreifende Azure-Befehlszeilenschnittstelle][azure-xplat-cli-github] enthält Befehle für die Interaktion mit Service Fabric-Entitäten (wie etwa Cluster und Anwendungen). Sie basiert auf Node.js. [Vergewissern Sie sich daher, dass Sie Node installiert haben][install-node], bevor Sie mit den weiteren Schritten fortfahren.

1. Klonen Sie das GitHub-Repository auf Ihren Entwicklungscomputer.
   
    ```bash
    git clone https://github.com/Azure/azure-xplat-cli.git
    ```
2. Wechseln Sie in das geklonte Repository, und installieren Sie die Abhängigkeiten der Befehlszeilenschnittstelle mithilfe des Knotenpaket-Managers (Node Package Manager, npm).
   
    ```bash
    cd azure-xplat-cli
    npm install
    ```
3. Erstellen Sie einen Symlink zwischen dem Ordner „bin/azure“ des geklonten Repositorys und „/usr/bin/azure“, damit er Ihrem Pfad hinzugefügt wird und Befehle in jedem Verzeichnis zur Verfügung stehen.
   
    ```bash
    sudo ln -s $(pwd)/bin/azure /usr/bin/azure
    ```
4. Aktivieren Sie schließlich die automatische Vervollständigung von Service Fabric-Befehlen.
   
    ```bash
    azure --completion >> ~/azure.completion.sh
    echo 'source ~/azure.completion.sh' >> ~/.bash_profile
    source ~/azure.completion.sh
    ```

## <a name="set-up-a-local-cluster"></a>Einrichten eines lokalen Clusters
Wenn alles erfolgreich installiert wurde, können Sie einen lokalen Cluster starten.

1. Führen Sie das Clustersetupskript aus.
   
    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```
2. Öffnen Sie einen Webbrowser, und navigieren Sie zu http://localhost:19080/Explorer. Der gestartete Cluster wird auf dem Dashboard von Service Fabric Explorer angezeigt.
   
    ![Service Fabric Explorer unter Linux][sfx-linux]

Nun können Sie vorgefertigte Service Fabric-Anwendungspakete oder neue, auf Gastcontainern oder ausführbaren Gastdateien basierende Anwendungspakete bereitstellen. Wenn Sie neue Dienste mit den Java- oder .NET Core-SDKs erstellen möchten, führen Sie die folgenden optionalen Einrichtungsschritte aus.

## <a name="install-the-java-sdk-and-eclipse-neon-plugin-optional"></a>Installieren von Java SDK und Eclipse Neon-Plug-In (optional)
Das Java SDK stellt die Bibliotheken und Vorlagen bereit, die zum Erstellen von Service Fabric-Diensten mit Java benötigt werden.

1. Installieren Sie das Java SDK-Paket.
   
    ```bash
    sudo apt-get install servicefabricsdkjava
    ```
2. Führen Sie das SDK-Setupskript aus.
   
    ```bash
    sudo /opt/microsoft/sdk/servicefabric/java/sdkjavasetup.sh
    ```

Das Eclipse-Plug-In für Service Fabric können Sie über die Eclipse Neon-IDE installieren.

1. Vergewissern Sie sich in Eclipse, dass mindestens die Buildship-Version 1.0.17 installiert ist. Die Version der installierten Komponenten können Sie unter **Hilfe > Installationsdetails** ermitteln. Eine Aktualisierungsanleitung für Buildship finden Sie [hier][buildship-update].
2. Wählen Sie zum Installieren des Service Fabric-Plug-Ins **Hilfe > Neue Software installieren...** aus.
3. Geben Sie in das Textfeld „Arbeiten mit“ ein: http://dl.windowsazure.com/eclipse/servicefabric
4. Klicken Sie auf "Hinzufügen".
   
    ![Eclipse-Plug-In][sf-eclipse-plugin]
5. Wählen Sie das Service Fabric-Plug-In aus, und klicken Sie auf „Weiter“.
6. Fahren Sie mit der Installation fort, und akzeptieren Sie den Endbenutzer-Lizenzvertrag.

## <a name="install-the-net-core-sdk-optional"></a>Installieren des .NET Core SDKs (optional)
Das .NET Core SDK stellt die Bibliotheken und Vorlagen bereit, die zum Erstellen von Service Fabric-Diensten mit plattformübergreifendem .NET Core benötigt werden.

1. Installieren Sie das .NET Core SDK-Paket.
   
    ```bash
    sudo apt-get install servicefabricsdkcsharp
    ```
2. Führen Sie das SDK-Setupskript aus.
   
    ```bash
    sudo /opt/microsoft/sdk/servicefabric/csharp/sdkcsharpsetup.sh
    ```

## <a name="next-steps"></a>Nächste Schritte
* [Create your first Java application on Linux](service-fabric-create-your-first-linux-application-with-java.md)
* [Prepare your development environment on OSX (Vorbereiten Ihrer Entwicklungsumgebung unter OSX)](service-fabric-get-started-mac.md)

<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png



<!---HONumber=Nov16_HO2-->


