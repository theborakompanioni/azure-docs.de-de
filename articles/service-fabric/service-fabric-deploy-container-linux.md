---
title: Service Fabric und Bereitstellung von Containern in Linux | Microsoft Docs
description: "Es wird beschrieben, wie Sie Service Fabric und Docker-Container zur Bereitstellung von Microserviceanwendungen nutzen. In diesem Artikel werden die Funktionen, die in Service Fabric für Container enthalten sind, und die Bereitstellung von Docker-Container-Images in einem Cluster beschrieben."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 4ba99103-6064-429d-ba17-82861b6ddb11
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/24/2016
ms.author: mfussell
translationtype: Human Translation
ms.sourcegitcommit: f31c8ab3d2b4fd871c92ac0e7d04bc8d5ab86830
ms.openlocfilehash: 963ca79f83d9ae4b3c5a0e5da6c5fb7985e9ca77


---
# <a name="deploy-a-docker-container-to-service-fabric"></a>Bereitstellen eines Docker-Containers in Service Fabric
> [!div class="op_single_selector"]
> * [Bereitstellen von Windows Containern](service-fabric-deploy-container.md)
> * [Bereitstellen von Docker-Containern](service-fabric-deploy-container-linux.md)
>
>

Dieser Artikel zeigt Ihnen, wie Sie unter Linux Dienste in Docker-Containern erstellen.

Service Fabric verfügt über mehrere Containerfunktionen für die Erstellung von Anwendungen, die sich aus Microservices in Containern zusammensetzen. Diese Dienste werden als Dienste in Containern bezeichnet.

Die Funktionen sind:

* Bereitstellung und Aktivierung von Containerimages
* Ressourcenkontrolle
* Repositoryauthentifizierung
* Containerport zum Hosten der Portzuordnung
* Container-zu-Container-Ermittlung und -Kommunikation
* Möglichkeit zum Konfigurieren und Festlegen von Umgebungsvariablen

## <a name="packaging-a-docker-container-with-yeoman"></a>Packen von Docker-Containern mit Yeoman
Beim Packen eines Containers unter Linux können Sie wählen, ob Sie eine Yeoman-Vorlage verwenden oder das [Anwendungspaket manuell erstellen](service-fabric-deploy-container.md#manually).

Eine Service Fabric-Anwendung kann einen oder mehrere Container enthalten, die jeweils eine bestimmte Rolle bei der Bereitstellung von Funktionen der Anwendung haben. Das Service Fabric SDK für Linux enthält einen [Yeoman](http://yeoman.io/)-Generator, mit dem Sie problemlos die Anwendung erstellen und ein Containerimage hinzufügen können. Lassen Sie uns mit Yeoman eine neue Anwendung mit einem einzelnen Docker-Container erstellen, deren Name *SimpleContainerApp* lautet. Sie können später weitere Dienste hinzufügen, indem Sie die generierten Manifestdateien bearbeiten.

## <a name="create-the-application"></a>Erstellen der Anwendung
1. Geben Sie in einem Terminal die Zeichenfolge **yo azuresfguest** ein.
2. Wählen Sie für das Framework **Container** aus.
3. Nennen Sie die Anwendung z.B. „SimpleContainerApp“.
4. Geben Sie die URL für das Containerimage aus einem DockerHub-Repository an. Der image-Parameter hat das Format [Repository]/[Imagename].

![Service Fabric-Yeoman-Generator für Container][sf-yeoman]

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
5. Verwenden Sie das in der Vorlage bereitgestellte Deinstallationsskript, um die Anwendungsinstanz zu löschen und die Registrierung des Anwendungstyps aufzuheben.

    ```bash
    ./uninstall.sh
    ```

## <a name="adding-more-services-to-an-existing-application"></a>Hinzufügen weiterer Dienste zu einer vorhandenen Anwendung

Führen Sie zum Hinzufügen eines weiteren Containerdiensts zu einer Anwendung, die bereits mit `yo` erstellt wurde, die folgenden Schritte aus: 
1. Legen Sie das Verzeichnis auf den Stamm der vorhandenen Anwendung fest.  Beispiel: `cd ~/YeomanSamples/MyApplication`, wenn `MyApplication` die von Yeoman erstellte Anwendung ist.
2. Führen Sie `yo azuresfguest:AddService` aus.



## <a name="next-steps"></a>Nächste Schritte
* [Übersicht über Service Fabric und Container](service-fabric-containers-overview.md)
* [Interagieren mit einem Service Fabric-Cluster mithilfe der Azure-Befehlszeilenschnittstelle](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-deploy-container-linux/sf-container-yeoman.png



<!--HONumber=Dec16_HO1-->


