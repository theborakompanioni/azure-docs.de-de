---
title: Einrichten Ihrer Entwicklungsumgebung unter Mac OS X | Microsoft Docs
description: "Installieren Sie Laufzeit, SDK und Tools, und erstellen Sie einen lokalen Entwicklungscluster. Nach Abschluss des Setups können Sie mit der Erstellung von Clientanwendungen unter Mac OS X beginnen."
services: service-fabric
documentationcenter: java
author: saysa
manager: raunakp
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/27/2016
ms.author: saysa
translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: e5d14eb0a656d67030f4c0d3d510aec0e9cafae7
ms.lasthandoff: 03/28/2017


---
# <a name="set-up-your-development-environment-on-mac-os-x"></a>Einrichten Ihrer Entwicklungsumgebung unter Mac OS X
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md)
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>  

Sie können Service Fabric-Anwendungen für die Ausführung in Linux-Clustern unter Mac OS X erstellen. In diesem Artikel erfahren Sie, wie Sie Ihren Mac für die Entwicklung einrichten.

## <a name="prerequisites"></a>Voraussetzungen
Service Fabric wird nicht nativ unter OS X ausgeführt. Für die Verwendung eines lokalen Service Fabric-Clusters stellen wir einen vorkonfigurierten virtuellen Ubuntu-Computer mit Vagrant und VirtualBox bereit. Bevor Sie beginnen, benötigen Sie Folgendes:

* [Vagrant (mindestens Version 1.8.4)](http://www.vagrantup.com/downloads.html)
* [VirtualBox](http://www.virtualbox.org/wiki/Downloads)

>[!NOTE]
> Sie müssen Versionen von Vagrant und VirtualBox verwenden, die sich gegenseitig unterstützen. Vagrant kann mit einer nicht unterstützten Version von VirtualBox möglicherweise nicht einwandfrei ausgeführt werden.
>

## <a name="create-the-local-vm"></a>Erstellen des lokalen virtuellen Computers
Führen Sie die folgenden Schritte aus, um den lokalen virtuellen Computer mit einem Service Fabric-Cluster mit fünf Knoten zu erstellen:

1. Klonen des Repositorys `Vagrantfile`

    ```bash
    git clone https://github.com/azure/service-fabric-linux-vagrant-onebox.git
    ```
    Mit diesen Schritten wird die Datei `Vagrantfile` mit der VM-Konfiguration und dem Downloadort der VM bereitgestellt.

   
2. Navigieren Sie zum lokalen Klon des Repositorys:

    ```bash
    cd service-fabric-linux-vagrant-onebox
    ```
3. Optional: Ändern Sie die Standardeinstellungen des virtuellen Computers

    Der lokale virtuelle Computer ist standardmäßig wie folgt konfiguriert:

   * 3 GB zugeordneter Arbeitsspeicher
   * Privates Hostnetzwerk (konfiguriert mit der IP-Adresse 192.168.50.50, um Pass-Through-Datenverkehr des Mac-Hosts zu ermöglichen)

     Sie können diese Einstellungen ändern oder die Konfiguration des virtuellen Computers über `Vagrantfile` ergänzen. Eine Liste mit allen Konfigurationsoptionen finden Sie in der [Vagrant-Dokumentation](http://www.vagrantup.com/docs) .
4. Erstellen des virtuellen Computers

    ```bash
    vagrant up
    ```

   In diesem Schritt wird das vorkonfigurierte VM-Image heruntergeladen und lokal gestartet. Anschließend wird auf dem virtuellen Computer ein lokaler Service Fabric-Cluster eingerichtet. Dieser Vorgang dauert ein paar Minuten. Wenn die Einrichtung erfolgreich war, erscheint in der Ausgabe eine Meldung mit dem Hinweis, dass der Cluster gestartet wird.

    ![Start der Clustereinrichtung nach Bereitstellung des virtuellen Computers][cluster-setup-script]

>[!TIP]
> Falls der Download der VM sehr lange dauert, können Sie sie per wget oder curl oder über einen Browser herunterladen, indem Sie zu dem Link navigieren, der unter **config.vm.box_url** in der Datei `Vagrantfile` angegeben ist. Nachdem Sie den Download an den lokalen Speicherort durchgeführt haben, können Sie die Datei `Vagrantfile` so bearbeiten, dass darin auf den lokalen Pfad verwiesen wird, an den Sie das Image heruntergeladen haben. Wenn Sie das Image beispielsweise nach „/home/users/test/azureservicefabric.tp8.box“ heruntergeladen haben, legen Sie **config.vm.box_url** auf diesen Pfad fest.
>

5. Vergewissern Sie sich, dass der Cluster ordnungsgemäß eingerichtet wurde, indem Sie unter http://192.168.50.50:19080/Explorer zu Service Fabric Explorer navigieren (vorausgesetzt, Sie haben die standardmäßige IP-Adresse des privaten Netzwerks beibehalten).

    ![Service Fabric Explorer auf dem Host-Mac][sfx-mac]

## <a name="install-the-service-fabric-plugin-for-eclipse-neon"></a>Installieren des Service Fabric-Plug-Ins für Eclipse Neon

Service Fabric umfasst ein Plug-In für die **Eclipse Neon-IDE für Java**, das die Erstellung und Bereitstellung von Java-Diensten vereinfachen kann. Sie können die Installationsschritte in dieser allgemeinen [Dokumentation](service-fabric-get-started-eclipse.md#install-or-update-the-service-fabric-plug-in-in-eclipse-neon) zur Installation und Aktualisierung des Service Fabric-Plug-Ins für Eclipse ausführen.

## <a name="using-service-fabric-eclipse-plugin-on-mac"></a>Verwenden des Service Fabric-Plug-Ins für Eclipse unter Mac

Vergewissern Sie sich, dass Sie die Schritte in der [Dokumentation zum Service Fabric-Plug-Ins für Eclipse](service-fabric-get-started-eclipse.md) ausgeführt haben. Die Schritte zum Entwickeln, Erstellen und Bereitstellen einer Service Fabric-Java-Anwendung mithilfe eines Vagrant-Gastcontainers auf einem Mac-Host ist größtenteils identisch mit der allgemeinen-Dokumentation, abgesehen von den folgenden Punkten:

* Da die Service Fabric-Bibliotheken für Ihre Service Fabric-Java-Anwendung erforderlich sind, muss das Eclipse-Projekt unter einem freigegebenen Pfad erstellt werden. Der Inhalt unter dem Pfad auf dem Host, auf dem sich die ``Vagrantfile`` befindet, wird standardmäßig für den ``/vagrant``-Pfad auf dem Gast freigegeben.
* Wenn sich die ``Vagrantfile`` unter einem Pfad befindet, z.B. ``~/home/john/allprojects/``, müssen Sie Ihr Service Fabric-Projekt ``MyActor`` am Speicherort ``~/home/john/allprojects/MyActor`` erstellen, und der Pfad zu Ihrem Eclipse-Arbeitsbereich wäre ``~/home/john/allprojects``.

## <a name="next-steps"></a>Nächste Schritte
<!-- Links -->
* [Erstellen und Bereitstellen Ihrer ersten Service Fabric-Java-Anwendung unter Linux mithilfe von Yeoman](service-fabric-create-your-first-linux-application-with-java.md)
* [Erstellen und Bereitstellen Ihrer ersten Service Fabric-Java-Anwendung unter Linux mithilfe des Service Fabric-Plug-Ins für Eclipse](service-fabric-get-started-eclipse.md)
* [Erstellen eines Service Fabric-Clusters in Azure über das Azure-Portal](service-fabric-cluster-creation-via-portal.md)
* [Erstellen eines Service Fabric-Clusters in Azure mithilfe von Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
* [Modellieren von Anwendungen in Service Fabric](service-fabric-application-model.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

