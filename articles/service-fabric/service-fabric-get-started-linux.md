---
title: Einrichten Ihrer Entwicklungsumgebung unter Linux | Microsoft Docs
description: "Installieren Sie die Laufzeit und das SDK, und erstellen Sie einen lokalen Entwicklungscluster unter Linux. Nach Abschluss des Setups können Sie mit der Erstellung von Clientanwendungen beginnen."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 7/27/2017
ms.author: subramar
ms.translationtype: HT
ms.sourcegitcommit: 54454e98a2c37736407bdac953fdfe74e9e24d37
ms.openlocfilehash: da6a8b4824d7215eb1db131680856ac04003f5aa
ms.contentlocale: de-de
ms.lasthandoff: 07/13/2017

---
# <a name="prepare-your-development-environment-on-linux"></a>Vorbereiten Ihrer Entwicklungsumgebung unter Linux
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md)
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>  

Zur Bereitstellung und Ausführung von [Azure Service Fabric-Anwendungen](service-fabric-application-model.md) auf Ihrem Linux-Entwicklungscomputer müssen Sie die Laufzeit und das allgemeine SDK installieren. Darüber hinaus können Sie auch optionale SDKs für Java und .NET Core installieren.

## <a name="prerequisites"></a>Voraussetzungen

Die folgenden Betriebssystemversionen werden bei der Entwicklung unterstützt:

* Ubuntu 16.04 (`Xenial Xerus`)

## <a name="update-your-apt-sources"></a>Aktualisieren Ihrer APT-Quellen
Um das SDK und das dazugehörige Laufzeitpaket über das Befehlszeilenprogramm „apt-get“ installieren zu können, müssen Sie zunächst Ihre APT-Datenquellen (Advanced Packaging Tool) aktualisieren.

1. Öffnen Sie ein Terminal.
2. Fügen Sie der Quellenliste das Service Fabric-Repository hinzu.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ trusty main" > /etc/apt/sources.list.d/servicefabric.list'
    ```

3. Fügen Sie das `dotnet`-Repository Ihrer Quellenliste hinzu.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
    ```

4. Fügen Sie Ihrem APT-Schlüsselbund den neuen GnuPG- bzw. GPG-Schlüssel (Gnu Privacy Guard) hinzu.

    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
    ```

5. Fügen Sie Ihrem APT-Schlüsselbund den offiziellen Docker-GPG-Schlüssel hinzu.

    ```bash
    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    ```

6. Richten Sie das Docker-Repository ein.

    ```bash
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    ```

7. Aktualisieren Sie Ihre Paketlisten auf der Grundlage der neu hinzugefügten Repositorys.

    ```bash
    sudo apt-get update
    ```

## <a name="install-and-set-up-the-sdk-for-containers-and-guest-executables"></a>Installieren und Einrichten des SDK für Container und ausführbare Gastanwendungsdateien

Nach der Aktualisierung Ihrer Quellen können Sie das SDK installieren.

1. Installieren Sie das Service Fabric-SDK-Paket, bestätigen Sie die Installation, und stimmen Sie dem Lizenzvertrag zu.

    ```bash
    sudo apt-get install servicefabricsdkcommon
    ```

    >   [!TIP]
    >   Mit den folgenden Befehlen wird das Akzeptieren der Lizenz für Service Fabric-Pakete automatisiert:
    >   ```bash
    >   echo "servicefabric servicefabric/accepted-eula-v1 select true" | sudo debconf-set-selections
    >   echo "servicefabricsdkcommon servicefabricsdkcommon/accepted-eula-v1 select true" | sudo debconf-set-selections
    >   ```
    
2. Führen Sie das SDK-Setupskript aus.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/sdkcommonsetup.sh
    ```

Nachdem Sie das allgemeine SDK-Paket installiert haben, können Sie durch Ausführen von `yo azuresfguest` oder `yo azuresfcontainer` Apps mit ausführbaren Gastdateien oder Containerdiensten erstellen. Unter Umständen müssen Sie die Umgebungsvariable „$NODE_PATH“ auf den Speicherort der Knotenmodule festlegen. 


```bash
    export NODE_PATH=$NODE_PATH:$HOME/.node/lib/node_modules 
```

Wenn Sie die Umgebung als Stamm (Root) verwenden, müssen Sie die Variable ggf. mit dem folgenden Befehl festlegen:

```bash
    export NODE_PATH=$NODE_PATH:/root/.node/lib/node_modules 
```


> [!TIP]
> Es kann hilfreich sein, diese Befehle der Datei „~/.bashrc“ hinzuzufügen, damit Sie die Umgebungsvariable nicht bei jeder Anmeldung festlegen müssen.
>

## <a name="set-up-the-xplat-service-fabric-cli"></a>Einrichten der XPlat-CLI für Service Fabric
Die [XPlat-CLI][azure-xplat-cli-github] enthält Befehle für die Interaktion mit Service Fabric-Entitäten, etwa Clustern und Anwendungen. Sie basiert auf Node.js. [Vergewissern Sie sich daher, dass Sie Node installiert haben][install-node], bevor Sie mit den folgenden Anweisungen fortfahren:

1. Klonen Sie das GitHub-Repository auf Ihren Entwicklungscomputer.

    ```bash
    git clone https://github.com/Azure/azure-xplat-cli.git
    ```

2. Wechseln Sie in das geklonte Repository, und installieren Sie die CLI-Abhängigkeiten mithilfe des Knotenpaket-Managers (Node Package Manager, npm).

    ```bash
    cd azure-xplat-cli
    npm install
    ```

3. Erstellen Sie einen Symlink, der aus dem Ordner `bin/azure` des geklonten Repositorys auf `/usr/bin/azure` verweist.

    ```bash
    sudo ln -s $(pwd)/bin/azure /usr/bin/azure
    ```

4. Aktivieren Sie schließlich die automatische Vervollständigung von Service Fabric-Befehlen.

    ```bash
    azure --completion >> ~/azure.completion.sh
    echo 'source ~/azure.completion.sh' >> ~/.bash_profile
    source ~/azure.completion.sh
    ```

### <a name="set-up-azure-cli-20"></a>Einrichten von Azure CLI 2.0

Als Alternative zur XPlat-CLI ist jetzt ein Service Fabric-Befehlsmodul in der Azure-Befehlszeilenschnittstelle enthalten.

Weitere Informationen zum Installieren von Azure CLI 2.0 und zur Verwendung der Service Fabric-Befehle finden Sie unter [Service Fabric und Azure CLI 2.0](service-fabric-azure-cli-2-0.md).

## <a name="set-up-a-local-cluster"></a>Einrichten eines lokalen Clusters
Nach erfolgreicher Installation können Sie einen lokalen Cluster starten.

1. Führen Sie das Clustersetupskript aus.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```

2. Öffnen Sie einen Webbrowser, und wechseln Sie zu [Service Fabric Explorer](http://localhost:19080/Explorer). Der gestartete Cluster wird auf dem Dashboard von Service Fabric Explorer angezeigt.

    ![Service Fabric Explorer unter Linux][sfx-linux]

Nun können Sie vorgefertigte Service Fabric-Anwendungspakete oder neue, auf Gastcontainern oder ausführbaren Gastdateien basierende Anwendungspakete bereitstellen. Wenn Sie neue Dienste mit den Java- oder .NET Core-SDKs erstellen möchten, führen Sie die optionalen Einrichtungsschritte aus den nachfolgenden Abschnitten aus.


> [!NOTE]
> Eigenständige Cluster werden unter Linux nicht unterstützt. Die Vorschau unterstützt nur One-Box-Cluster und Azure-Linux-Cluster mit mehreren Computern.
>

## <a name="install-the-java-sdk-optional-if-you-want-to-use-the-java-programming-models"></a>Installieren des Java SDKs (optional, falls Sie Java-Programmiermodelle nutzen möchten)
Das Java SDK stellt die Bibliotheken und Vorlagen bereit, die zum Erstellen von Service Fabric-Diensten mit Java benötigt werden.

1. Installieren Sie das Java SDK-Paket.

    ```bash
    sudo apt-get install servicefabricsdkjava
    ```

2. Führen Sie das SDK-Setupskript aus.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/java/sdkjavasetup.sh
    ```

## <a name="install-the-eclipse-neon-plug-in-optional"></a>Installieren des Eclipse Neon-Plug-Ins (optional)

Das Eclipse-Plug-In für Service Fabric können Sie über die **Eclipse-IDE für Java-Entwickler** installieren. Sie können Eclipse verwenden, um zusätzlich zu Service Fabric-Java-Anwendungen Anwendungen mit ausführbarer Gastanwendungsdatei und Containeranwendungen für Service Fabric zu erstellen.

> [!NOTE]
> Das Java SDK ist eine Voraussetzung für die Verwendung des Eclipse-Plug-Ins. Dies gilt auch, wenn Sie es nur für ausführbare Gastanwendungsdateien und Containeranwendungen verwenden.
>

1. Stellen Sie in Eclipse sicher, dass Sie die aktuellen Versionen von Eclipse Neon und Buildship (1.0.17 oder höher) installiert haben. Die Version der installierten Komponenten können Sie unter **Hilfe** > **Installationsdetails** ermitteln. Anweisungen zum Aktualisieren von Buildship finden Sie unter [Eclipse Buildship: Eclipse Plug-ins for Gradle][buildship-update] (Eclipse Buildship: Eclipse-Plug-Ins für Gradle).

2. Wählen Sie zum Installieren des Service Fabric-Plug-Ins **Hilfe** > **Neue Software installieren...** aus.

3. Geben Sie im Feld **Work with** (Verwenden von) die URL **http://dl.microsoft.com/eclipse** ein.

4. Klicken Sie auf **Hinzufügen**.

    ![Die Seite „Verfügbare Software“][sf-eclipse-plugin]

5. Wählen Sie das Plug-In **Service Fabric** aus, und klicken Sie auf **Weiter**.

6. Führen Sie die Installationsschritte aus, und akzeptieren Sie die Microsoft-Software-Lizenzbedingungen.

Falls Sie das Service Fabric-Plug-In für Eclipse bereits installiert haben, vergewissern Sie sich, dass Sie über die aktuelle Version verfügen. Wählen Sie hierzu **Hilfe** > **Installationsdetails** aus, und suchen Sie anschließend in der Liste mit den installierten Plug-Ins nach Service Fabric. Ist eine neuere Version verfügbar, wählen Sie **Aktualisieren** aus.

Weitere Informationen finden Sie unter [Service Fabric-Plug-In für die Entwicklung von Eclipse-Java-Anwendungen](service-fabric-get-started-eclipse.md).


## <a name="install-the-net-core-sdk-optional-if-you-want-to-use-the-net-core-programming-models"></a>Installieren des .NET Core SDKs (optional, falls Sie die .NET Core-Programmiermodelle verwenden möchten)
Das .NET Core SDK stellt die Bibliotheken und Vorlagen bereit, die zum Erstellen von Service Fabric-Diensten mit .NET Core benötigt werden.

1. Installieren Sie das .NET Core SDK-Paket.

   ```bash
   sudo apt-get install servicefabricsdkcsharp
   ```

2. Führen Sie das SDK-Setupskript aus.

   ```bash
   sudo /opt/microsoft/sdk/servicefabric/csharp/sdkcsharpsetup.sh
   ```

## <a name="update-the-sdk-and-runtime"></a>Aktualisieren des SDKs und der Laufzeit

Führen Sie die folgenden Befehle aus (deaktivieren Sie die nicht gewünschten SDKs), um das Update auf die aktuelle Version von SDK und Runtime durchzuführen:

```bash
sudo apt-get update
sudo apt-get install servicefabric servicefabricsdkcommon servicefabricsdkcsharp servicefabricsdkjava
```


> [!NOTE]
> Die Aktualisierung der Pakete kann die Beendigung Ihres lokalen Entwicklungsclusters zur Folge haben. Starten Sie den lokalen Cluster nach einem Upgrade anhand der Anweisungen auf dieser Seite neu.

## <a name="next-steps"></a>Nächste Schritte
* [Erstellen und Bereitstellen Ihrer ersten Service Fabric-Java-Anwendung unter Linux mithilfe von Yeoman](service-fabric-create-your-first-linux-application-with-java.md)
* [Erstellen und Bereitstellen Ihrer ersten Service Fabric-Java-Anwendung unter Linux mithilfe des Service Fabric-Plug-Ins für Eclipse](service-fabric-get-started-eclipse.md)
* [Erstellen der ersten Java-Anwendung unter Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
* [Prepare your development environment on OSX (Vorbereiten Ihrer Entwicklungsumgebung unter OSX)](service-fabric-get-started-mac.md)
* [Verwalten von Service Fabric-Anwendungen mit der XPlat-CLI](service-fabric-azure-cli.md)
* [Unterschiede zwischen Service Fabric unter Linux (Vorschau) und Windows (allgemein verfügbar)](service-fabric-linux-windows-differences.md)

## <a name="related-articles"></a>Verwandte Artikel

* [Service Fabric und Azure CLI 2.0](service-fabric-azure-cli-2-0.md)
* [Interagieren mit einem Service Fabric-Cluster mithilfe der Azure-Befehlszeilenschnittstelle](service-fabric-azure-cli.md)

<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png

