---
title: Einrichten Ihrer Entwicklungsumgebung unter Linux | Microsoft Docs
description: Installieren Sie die Laufzeit und das SDK, und erstellen Sie einen lokalen Entwicklungscluster unter Linux. Nach Abschluss des Setups können Sie mit der Erstellung von Clientanwendungen beginnen.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: '' 

ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/28/2017
ms.author: subramar
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: 5840932d79ef9fd99a94eb6ae4e587b0e616065e
ms.contentlocale: de-de
ms.lasthandoff: 07/01/2017


---
<a id="prepare-your-development-environment-on-linux" class="xliff"></a>

# Vorbereiten Ihrer Entwicklungsumgebung unter Linux
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md)
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>  

 Zur Bereitstellung und Ausführung von [Azure Service Fabric-Anwendungen](service-fabric-application-model.md) auf Ihrem Linux-Entwicklungscomputer müssen Sie die Laufzeit und das allgemeine SDK installieren. Darüber hinaus können Sie auch optionale SDKs für Java und .NET Core installieren.

<a id="prerequisites" class="xliff"></a>

## Voraussetzungen

<a id="supported-operating-system-versions" class="xliff"></a>

### Unterstützte Betriebssystemversionen
Die folgenden Betriebssystemversionen werden bei der Entwicklung unterstützt:

* Ubuntu 16.04 (`Xenial Xerus`)

<a id="update-your-apt-sources" class="xliff"></a>

## Aktualisieren Ihrer apt-Quellen
Um das SDK und das dazugehörige Laufzeitpaket über „apt-get“ installieren zu können, müssen Sie zunächst Ihre apt-Datenquellen aktualisieren.

1. Öffnen Sie ein Terminal.
2. Fügen Sie der Quellenliste das Service Fabric-Repository hinzu.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ trusty main" > /etc/apt/sources.list.d/servicefabric.list'
    ```

3. Fügen Sie das `dotnet`-Repository Ihrer Quellenliste hinzu.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
    ```

4. Fügen Sie Ihrem apt-Schlüsselbund den neuen GPG-Schlüssel hinzu.

    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
    ```

5. Fügen Sie Ihrem apt-Schlüsselbund den offiziellen GPG-Schlüssel von Docker hinzu.

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

<a id="install-and-set-up-the-sdk-for-containers-and-guest-executables" class="xliff"></a>

## Installieren und Einrichten des SDK für Container und ausführbare Gastanwendungsdateien

Nach der Aktualisierung Ihrer Datenquellen können Sie das SDK installieren.

1. Installieren Sie das Service Fabric-SDK-Paket. Sie werden aufgefordert, die Installation zu bestätigen und einem Lizenzvertrag zuzustimmen.

    ```bash
    sudo apt-get install servicefabricsdkcommon
    ```

    >   [!TIP]
    >   Mit den folgenden Befehlen wird das Akzeptieren der Lizenz für Service Fabric-Pakete automatisiert:
    >   ```bash
    >   echo "servicefabric servicefabric/accepted-eula-v1 select true" | debconf-set-selections
    >   echo "servicefabricsdkcommon servicefabricsdkcommon/accepted-eula-v1 select true" | debconf-set-selections
    >   ```
    
2. Führen Sie das SDK-Setupskript aus.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/sdkcommonsetup.sh
    ```

Nach dem Ausführen der Schritte zum Installieren des allgemeinen SDK-Pakets sollte die Erstellung von Apps mit ausführbaren Gastdateien oder Containerdiensten möglich sein, indem `yo azuresfguest` oder `yo azuresfcontainer` ausgeführt wird. Unter Umständen müssen Sie die Umgebungsvariable **$NODE_PATH** auf den Speicherort der Knotenmodule festlegen. 


```bash
    export NODE_PATH=$NODE_PATH:$HOME/.node/lib/node_modules 
```

Bei Verwendung der Umgebung als Stammumgebung (Root) müssen Sie die Variable ggf. mit dem folgenden Befehl festlegen:

```bash
    export NODE_PATH=$NODE_PATH:/root/.node/lib/node_modules 
```


> [!TIP]
> Es kann hilfreich sein, diese Befehle der Datei „~/.bashrc“ hinzuzufügen, damit Sie die Umgebungsvariable nicht bei jeder Anmeldung festlegen müssen.
>

<a id="setup-the-xplat-service-fabric-cli" class="xliff"></a>

## Einrichten der XPlat-CLI für Service Fabric
Die [XPlat-CLI][azure-xplat-cli-github] enthält Befehle für die Interaktion mit Service Fabric-Entitäten, etwa Clustern und Anwendungen. Sie basiert auf Node.js. [Vergewissern Sie sich daher, dass Sie Node installiert haben][install-node], bevor Sie mit den folgenden Anweisungen fortfahren:

1. Klonen Sie das GitHub-Repository auf Ihren Entwicklungscomputer.

    ```bash
    git clone https://github.com/Azure/azure-xplat-cli.git
    ```

2. Wechseln Sie in das geklonte Repository, und installieren Sie die Abhängigkeiten der Befehlszeilenschnittstelle mithilfe des Knotenpaket-Managers (Node Package Manager, npm).

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

<a id="setup-azure-cli-20" class="xliff"></a>

### Einrichten der Azure CLI 2.0

Als Alternative zur XPlat-CLI ist jetzt ein Service Fabric-Befehlsmodul in der Azure CLI enthalten.

Weitere Informationen zum Installieren der Azure CLI 2.0 und zum Verwenden der Service Fabric-Befehle finden Sie in der [Dokumentation zu den ersten Schritten](service-fabric-azure-cli-2-0.md).

<a id="set-up-a-local-cluster" class="xliff"></a>

## Einrichten eines lokalen Clusters
Wenn alles erfolgreich installiert wurde, können Sie einen lokalen Cluster starten.

1. Führen Sie das Clustersetupskript aus.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```

2. Öffnen Sie einen Webbrowser, und navigieren Sie zu http://localhost:19080/Explorer. Der gestartete Cluster wird auf dem Dashboard von Service Fabric Explorer angezeigt.

    ![Service Fabric Explorer unter Linux][sfx-linux]

Nun können Sie vorgefertigte Service Fabric-Anwendungspakete oder neue, auf Gastcontainern oder ausführbaren Gastdateien basierende Anwendungspakete bereitstellen. Wenn Sie neue Dienste mit den Java- oder .NET Core-SDKs erstellen möchten, führen Sie die folgenden optionalen Einrichtungsschritte in den nachfolgenden Abschnitten aus.


> [!NOTE]
> Eigenständige Cluster werden unter Linux nicht unterstützt. In der Vorschau werden lediglich One-Box-Cluster und Azure Linux-Cluster mit mehreren Computern unterstützt.
>

<a id="install-the-java-sdk-optional-if-you-wish-to-use-the-java-programming-models" class="xliff"></a>

## Installieren des Java SDK (optional, falls Sie Java-Programmiermodelle nutzen möchten)
Das Java SDK stellt die Bibliotheken und Vorlagen bereit, die zum Erstellen von Service Fabric-Diensten mit Java benötigt werden.

1. Installieren Sie das Java SDK-Paket.

    ```bash
    sudo apt-get install servicefabricsdkjava
    ```

2. Führen Sie das SDK-Setupskript aus.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/java/sdkjavasetup.sh
    ```

<a id="install-the-eclipse-neon-plugin-optional" class="xliff"></a>

## Installieren des Eclipse Neon-Plug-Ins (optional)

Das Eclipse-Plug-In für Service Fabric können Sie über die **Eclipse-IDE für Java-Entwickler** installieren. Sie können Eclipse verwenden, um zusätzlich zu Service Fabric-Java-Anwendungen Anwendungen mit ausführbarer Gastanwendungsdatei und Containeranwendungen für Service Fabric zu erstellen.

> [!NOTE]
> Das Java SDK ist eine Voraussetzung für die Verwendung des Eclipse-Plug-Ins. Dies gilt auch, wenn Sie es nur für ausführbare Gastanwendungsdateien und Containeranwendungen verwenden.
>

1. Stellen Sie in Eclipse sicher, dass Sie die aktuellen Versionen von Eclipse **Neon** und Buildship (1.0.17 oder höher) installiert haben. Die Version der installierten Komponenten können Sie unter **Hilfe > Installationsdetails** ermitteln. Eine Aktualisierungsanleitung für Buildship finden Sie [hier][buildship-update].
2. Wählen Sie zum Installieren des Service Fabric-Plug-Ins **Hilfe > Neue Software installieren...** aus.
3. Geben Sie im Textfeld „Work with“ (Verwenden von) Folgendes ein: „http://dl.microsoft.com/eclipse“.
4. Klicken Sie auf "Hinzufügen".

    ![Eclipse-Plug-In][sf-eclipse-plugin]

5. Wählen Sie das Service Fabric-Plug-In aus, und klicken Sie auf **Weiter**.
6. Fahren Sie mit der Installation fort, und akzeptieren Sie den Endbenutzer-Lizenzvertrag.

Falls Sie das Service Fabric-Eclipse-Plug-In bereits installiert haben, sollten Sie sicherstellen, dass es sich um die aktuelle Version handelt. Sie können dies überprüfen, indem Sie ``Help => Installation Details`` auswählen und in der Liste mit den installierten Plug-Ins nach Service Fabric suchen. Wählen Sie die Option zum Aktualisieren, falls eine neuere Version verfügbar ist.

Weitere Informationen finden Sie unter [Erste Schritte mit Eclipse für Service Fabric](service-fabric-get-started-eclipse.md).


<a id="install-the-net-core-sdk-optional-if-you-wish-to-use-the-net-core-programming-models" class="xliff"></a>

## Installieren des .NET Core SDK (optional, falls Sie die .NET Core-Programmiermodelle verwenden möchten)
Das .NET Core SDK stellt die Bibliotheken und Vorlagen bereit, die zum Erstellen von Service Fabric-Diensten mit .NET Core benötigt werden.

1. Installieren Sie das .NET Core SDK-Paket.

   ```bash
   sudo apt-get install servicefabricsdkcsharp
   ```

2. Führen Sie das SDK-Setupskript aus.

   ```bash
   sudo /opt/microsoft/sdk/servicefabric/csharp/sdkcsharpsetup.sh
   ```

<a id="updating-the-sdk-and-runtime" class="xliff"></a>

## Aktualisieren des SDK und der Laufzeit

Führen Sie die folgenden Befehle aus (deaktivieren Sie die nicht gewünschten SDKs), um das Update auf die aktuelle Version von SDK und Runtime durchzuführen:

```bash
sudo apt-get update
sudo apt-get install servicefabric servicefabricsdkcommon servicefabricsdkcsharp servicefabricsdkjava
```


> [!NOTE]
> Die Aktualisierung der Pakete kann dazu führen, dass der lokale Entwicklungscluster beendet wird. Starten Sie den lokalen Cluster nach einem Upgrade anhand der Anweisungen auf dieser Seite neu.

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
* [Erstellen und Bereitstellen Ihrer ersten Service Fabric-Java-Anwendung unter Linux mithilfe von Yeoman](service-fabric-create-your-first-linux-application-with-java.md)
* [Erstellen und Bereitstellen Ihrer ersten Service Fabric-Java-Anwendung unter Linux mithilfe des Service Fabric-Plug-Ins für Eclipse](service-fabric-get-started-eclipse.md)
* [Erstellen der ersten Java-Anwendung unter Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
* [Prepare your development environment on OSX (Vorbereiten Ihrer Entwicklungsumgebung unter OSX)](service-fabric-get-started-mac.md)
* [Verwalten von Service Fabric-Anwendungen mit der XPlat-CLI](service-fabric-azure-cli.md)
* [Unterschiede zwischen Service Fabric unter Linux (Vorschau) und Windows (allgemein verfügbar)](service-fabric-linux-windows-differences.md)

<a id="related-articles" class="xliff"></a>

## Verwandte Artikel

* [Getting started with Service Fabric and Azure CLI 2.0](service-fabric-azure-cli-2-0.md) (Erste Schritte mit Service Fabric und der Azure CLI 2.0)
* [Interagieren mit einem Service Fabric-Cluster mithilfe der Azure-Befehlszeilenschnittstelle](service-fabric-azure-cli.md)

<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png

