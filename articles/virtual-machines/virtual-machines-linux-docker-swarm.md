---
title: "Verwenden von Docker mit Swarm auf Azure – Erste Schritte"
description: Beschreibt das Erstellen einer Gruppe von VMs mit der Docker-VM-Erweiterung sowie die Verwendung von Swarm zum Erstellen eines Docker-Clusters.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: squillace
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: d529b1f5-864e-4163-9b34-b52d48ceedb1
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 01/04/2016
ms.author: rasquill
translationtype: Human Translation
ms.sourcegitcommit: f6537e4ebac76b9f3328223ee30647885ee15d3e
ms.openlocfilehash: 834bb8a60c3dcb2d0b50809ffb0e4780579b2cda


---
# <a name="how-to-use-docker-with-swarm"></a>Verwenden von Docker mit Swarm
> [!IMPORTANT] 
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells. Informationen zu Resource Manager-Vorlagen für die Bereitstellung von Docker Swarm finden Sie [hier](https://azure.microsoft.com/documentation/templates/docker-swarm-cluster/).

Dieses Thema zeigt eine sehr einfache Möglichkeit, mithilfe von [Docker](https://www.docker.com/) und [Swarm](https://github.com/docker/swarm) einen über Swarm verwalteten Cluster in Azure zu erstellen. Hierbei werden vier virtuelle Computer in Azure erstellt, von denen einer als Swarm-Manager fungiert und drei als Bestandteil des Clusters aus Docker-Hosts verwendet werden. Wenn Sie fertig sind, können Sie mithilfe von Swarm den Cluster anzeigen und mit der Verwendung von Docker beginnen. Darüber hinaus werden die Azure-CLI-Aufrufe in diesem Thema im ASM-Modus (Azure Service Management) ausgeführt. 

> [!NOTE]
> In diesem Thema werden Docker mit Swarm und die Azure-Befehlszeilenschnittstelle *ohne* **docker-machine** verwendet, um zu zeigen, wie die verschiedenen Tools zusammen funktionieren, aber dennoch unabhängig bleiben. **docker-machine** enthält **--swarm**-Optionen, mit denen Sie **docker-machine** zum direkten Hinzufügen von Knoten zu einem Schwarm verwenden können. Ein Beispiel finden Sie in der Dokumentation zu [docker-machine](https://github.com/docker/machine). Informationen zur Ausführung von **docker-machine** für Azure-VMs finden Sie unter [Verwenden von „docker-machine“ mit Azure](virtual-machines-linux-docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

## <a name="create-docker-hosts-with-azure-virtual-machines"></a>Erstellen von Docker-Hosts mit Azure-VMs
In diesem Thema werden vier VMs erstellt, aber Sie können eine beliebige Anzahl von VMs erstellen. Führen Sie den folgenden Aufruf aus, und ersetzen Sie dabei *&lt;password&gt;* durch das von Ihnen gewählte Kennwort.

    azure vm docker create swarm-master -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-1 -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-2 -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-3 -l "East US" -e 22 $imagename ops <password>

Im Anschluss sollten Sie in der Lage sein, über **azure vm list** Ihre Azure-VMs anzuzeigen:

    $ azure vm list | grep "swarm-[mn]"
    data:    swarm-master     ReadyRole           East US       swarm-master.cloudapp.net                               100.78.186.65
    data:    swarm-node-1     ReadyRole           East US       swarm-node-1.cloudapp.net                               100.66.72.126
    data:    swarm-node-2     ReadyRole           East US       swarm-node-2.cloudapp.net                               100.72.18.47  
    data:    swarm-node-3     ReadyRole           East US       swarm-node-3.cloudapp.net                               100.78.24.68  

## <a name="installing-swarm-on-the-swarm-master-vm"></a>Installieren von Swarm auf der Swarm-Master-VM
Dieses Thema verwendet das [Containermodell der Installation aus der Docker-Swarm-Dokumentation](https://github.com/docker/swarm#1---docker-image) – Sie können jedoch auch eine SSH-Verbindung mit **swarm-master** herstellen. In diesem Modell wird **swarm** als Docker-Container heruntergeladen, der Swarm ausführt. Nachfolgend wird dieser Schritt *remote über den Laptop unter Verwendung von Docker* ausgeführt, um eine Verbindung mit der **swarm-master**-VM herzustellen und diese anzuweisen, den Befehl **swarm create** zur Cluster-ID-Erstellung zu verwenden. Über die Cluster-ID ermittelt **swarm** die Mitglieder der Schwarmgruppe. (Sie können das Repository auch klonen und die Erstellung selbst durchführen, so haben Sie vollständige Kontrolle und können das Debugging aktivieren.)

    $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run --rm swarm create
    Unable to find image 'swarm:latest' locally
    511136ea3c5a: Pull complete
    a8bbe4db330c: Pull complete
    9dfb95669acc: Pull complete
    0b3950daf974: Pull complete
    633f3d9a9685: Pull complete
    bba5f98a0414: Pull complete
    defbc1ab4462: Pull complete
    92d78d321ff2: Pull complete
    swarm:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Status: Downloaded newer image for swarm:latest
    36731c17189fd8f450c395db8437befd

Die letzte Zeile ist die Cluster-ID. Kopieren Sie diese an einen beliebigen Ort, da Sie sie beim Beitritt der Knoten-VM zum Swarm-Master erneut benötigen, um den "swarm" zu erstellen. In diesem Beispiel lautet die Cluster-ID **36731c17189fd8f450c395db8437befd**.

> [!NOTE]
> Beachten Sie, dass zum Verbinden mit der **swarm-master**-VM in Azure die lokale Docker-Installation verwendet wird. Dabei wird **swarm-master** angewiesen, den **create**-Befehl herunterzuladen, zu installieren und auszuführen. Dieser Befehl gibt die Cluster-ID zurück, die später zu Ermittlungszwecken verwendet wird.
> <!-- -->
> Führen Sie zur Bestätigung `docker -H tcp://`*&lt;hostname&gt;*` images` aus, um die Containerprozesse im **swarm-master**-Computer aufzulisten. Wiederholen Sie die Ausführung zum Vergleich auf einem anderen Knoten (da wir den vorherigen swarm-Befehl mit der Option **--rm** ausgeführt haben, wurde der Container nach Fertigstellung entfernt, deshalb wird durch **docker ps -a** nichts zurückgegeben):
> 
> 

        $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 images
        REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
        swarm               latest              92d78d321ff2        11 days ago         7.19 MB
        $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 images
        REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
        $
<P />

> Wenn Sie mit **Docker** vertraut sind, wissen Sie, dass die anderen Knoten keine Einträge aufweisen, da noch keine Images heruntergeladen und ausgeführt wurden.
> 
> 

## <a name="join-the-node-vms-to-our-docker-cluster"></a>Hinzufügen der Knoten-VMs zum Docker-Cluster
Listen Sie für jeden Knoten mit der Azure-CLI die Endpunktinformationen auf. Nachstehend wird dieser Schritt für den Docker-Host **swarm-node-1** ausgeführt, um den Docker-Port des Knotens zu ermitteln.

    $ azure vm endpoint list swarm-node-1
    info:    Executing command vm endpoint list
    + Getting virtual machines
    data:    Name    Protocol  Public Port  Private Port  Virtual IP      EnableDirectServerReturn  Load Balanced
    data:    ------  --------  -----------  ------------  --------------  ------------------------  -------------
    data:    docker  tcp       2376         2376          138.91.112.194  false                     No
    data:    ssh     tcp       22           22            138.91.112.194  false                     No
    info:    vm endpoint list command OK


Verwenden Sie **Docker** und die Option `-H`, um den Docker-Client auf Ihre Knoten-VM zu verweisen. Fügen Sie diesen Knoten dem zu erstellenden Schwarm hinzu, indem Sie die Cluster-ID und den Docker-Port des Knotens (letzteren mit **--addr**) übergeben:

    $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 run -d swarm join --addr=138.91.112.194:2376 token://36731c17189fd8f450c395db8437befd
    Unable to find image 'swarm:latest' locally
    511136ea3c5a: Pull complete
    a8bbe4db330c: Pull complete
    9dfb95669acc: Pull complete
    0b3950daf974: Pull complete
    633f3d9a9685: Pull complete
    bba5f98a0414: Pull complete
    defbc1ab4462: Pull complete
    92d78d321ff2: Pull complete
    swarm:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Status: Downloaded newer image for swarm:latest
    bbf88f61300bf876c6202d4cf886874b363cd7e2899345ac34dc8ab10c7ae924

Das sieht gut aus. Geben Sie Folgendes ein, um zu bestätigen, dass **swarm** auf **swarm-node-1** ausgeführt wird:

    $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 ps -a
        CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS               NAMES
        bbf88f61300b        swarm:latest        "/swarm join --addr=   13 seconds ago      Up 12 seconds       2375/tcp            angry_mclean

Wiederholen Sie diesen Vorgang für alle Knoten im Cluster. Im vorliegenden Fall sind dies **swarm-node-2** und **swarm-node-3**.

## <a name="begin-managing-the-swarm-cluster"></a>Beginnen Sie mit der Verwaltung des Swarm-Clusters,
    $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run -d -p 2375:2375 swarm manage token://36731c17189fd8f450c395db8437befd
    d7e87c2c147ade438cb4b663bda0ee20981d4818770958f5d317d6aebdcaedd5

damit Sie anschließend Ihre Knoten im Cluster auflisten können:

    ralph@local:~$ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run --rm swarm list token://73f8bc512e94195210fad6e9cd58986f
    54.149.104.203:2375
    54.187.164.89:2375
    92.222.76.190:2375

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte
Führen Sie Aufgaben im Swarm aus. Anregungen finden Sie unter [https://github.com/docker/swarm/](https://github.com/docker/swarm/) oder in diesem [Video](https://www.youtube.com/watch?v=EC25ARhZ5bI).

<!-- links -->

[docker-machine-azure]: virtual-machines-linux-docker-machine.md




<!--HONumber=Dec16_HO1-->


