---
title: "Verwenden der Docker-VM-Erweiterung für Linux | Microsoft Docs"
description: "Beschreibt die Docker- und Azure Virtual Machines-Erweiterungen und zeigt die Erstellung von virtuellen Azure-Computern, die als Docker-Hosts verwendet werden, über die Azure-Befehlszeilenschnittstelle im klassischen Bereitstellungsmodell."
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 19cf64e8-f92c-43ad-a120-8976cd9102ac
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/27/2016
ms.author: rasquill
translationtype: Human Translation
ms.sourcegitcommit: f6537e4ebac76b9f3328223ee30647885ee15d3e
ms.openlocfilehash: a1742808be5df2a57e53bb6918f89df4953d0694


---
# <a name="using-the-docker-vm-extension-with-the-azure-classic-portal"></a>Verwenden der Docker-VM-Erweiterung mit dem klassischen Azure-Portal
> [!IMPORTANT] 
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells.

[Docker](https://www.docker.com/) ist einer der beliebtesten Virtualisierungsansätze, der für das Isolieren von Daten und Computing auf gemeinsamen Ressourcen [Linux-Container](http://en.wikipedia.org/wiki/LXC) statt virtueller Computer verwendet. Verwenden Sie die Docker-VM-Erweiterung des [Azure Linux-Agents] , um einen virtuellen Docker-Computer zu erstellen, der eine beliebige Anzahl von Containern für Ihre Anwendungen in Azure hostet.

> [!NOTE]
> Dieses Thema beschreibt das Erstellen eines virtuellen Docker-Computers im klassischen Azure-Portal. Informationen zum Erstellen eines virtuellen Docker-Computers an der Befehlszeile finden Sie unter [Verwenden der Docker-VM-Erweiterung über die Azure-Befehlszeilenschnittstelle (Azure-CLI)]. Eine allgemeine Diskussion über die Container und ihre Vorteile finden Sie unter [Docker High Level Whiteboard](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard)(Whiteboard auf hoher Ebene zu Docker) (in englischer Sprache).
> 
> 

## <a name="create-a-new-vm-from-the-image-gallery"></a>Erstellen eines neuen virtuellen Computers über den Image-Katalog
Für den ersten Schritt ist ein virtueller Azure-Computer von einem Linux-Image erforderlich, das die Docker-VM-Erweiterung unterstützt, mit einem Ubuntu 14.04 LTS-Image aus der Image-Galerie als einen Beispielserver und Ubuntu 14.04 Desktop als einen Client. Klicken Sie im Portal in der unteren linken Ecke auf **+ Neu** zum Erstellen einer neuen Instanz für einen virtuellen Computer, und wählen Sie ein Ubuntu 14.04 LTS-Image aus den verfügbaren Optionen oder analog zur folgenden Darstellung aus der vollständigen Image-Galerie aus.

> [!NOTE]
> Zurzeit unterstützen nur Ubuntu 14.04 LTS-Abbilder nach Juli 2014 die Docker-VM-Erweiterung.
> 
> 

![Erstellen eines neuen Ubuntu-Images](./media/virtual-machines-linux-classic-portal-use-docker/ChooseUbuntu.png)

## <a name="create-docker-certificates"></a>Erstellen von Docker-Zertifikaten
Stellen Sie nach dem Erstellen des virtuellen Computers sicher, dass der Docker auf Ihrem Clientcomputer installiert ist. (Einzelheiten finden Sie in den [Installationsanweisungen für Docker](https://docs.docker.com/installation/#installation).)

Erstellen Sie das Zertifikat und Schlüsseldateien für die Docker-Kommunikation gemäß [Running Docker with https] (Ausführen von Docker mit HTTPS, in englischer Sprache), und speichern Sie sie im Verzeichnis **`~/.docker`** auf Ihrem Clientcomputer.

> [!NOTE]
> Für die Docker-VM-Erweiterung im Portal sind zurzeit base64-codierte Anmeldeinformationen erforderlich.
> 
> 

Verwenden Sie an der Befehlszeile **`base64`** oder ein anderes bevorzugtes Codierungstool zum Erstellen base64-codierter Themen. Wenn Sie dies mit ein paar einfachen Zertifikaten und Schlüsseldateien vornehmen, sieht das möglicherweise in etwa so aus:

```
 ~/.docker$ ls
 ca-key.pem  ca.pem  cert.pem  key.pem  server-cert.pem  server-key.pem
 ~/.docker$ base64 ca.pem > ca64.pem
 ~/.docker$ base64 server-cert.pem > server-cert64.pem
 ~/.docker$ base64 server-key.pem > server-key64.pem
 ~/.docker$ ls
 ca64.pem    ca.pem    key.pem            server-cert.pem   server-key.pem
 ca-key.pem  cert.pem  server-cert64.pem  server-key64.pem
```

## <a name="add-the-docker-vm-extension"></a>Hinzufügen der Docker-VM-Erweiterung
Suchen Sie zum Hinzufügen der Docker-VM-Erweiterung die von Ihnen erstellte VM-Instanz, führen Sie einen Bildlauf zu **Erweiterungen** durch, um „VM-Erweiterungen“ analog zur unteren Beschreibung anzuzeigen.

> [!NOTE]
> Diese Funktionalität wird nur im Vorschauportal unterstützt: https://portal.azure.com/.
> 
> 

![](./media/virtual-machines-linux-classic-portal-use-docker/ClickExtensions.png)

### <a name="add-an-extension"></a>Hinzufügen einer Erweiterung
Klicken Sie auf **+ Hinzufügen** , um die möglichen VM-Erweiterungen anzuzeigen, die Sie zu diesem virtuellen Computer hinzufügen können.

![](./media/virtual-machines-linux-classic-portal-use-docker/ClickAdd.png)

### <a name="select-the-docker-vm-extension"></a>Auswählen der Docker-VM-Erweiterung
Wählen Sie die Docker-VM-Erweiterung aus, wodurch die Docker-Beschreibung und wichtige Links angezeigt werden, und klicken Sie dann unten auf **Erstellen** , um die Installationsprozedur zu starten.

![](./media/virtual-machines-linux-classic-portal-use-docker/ChooseDockerExtension.png)

![](./media/virtual-machines-linux-classic-portal-use-docker/CreateButtonFocus.png)

### <a name="add-your-certificate-and-key-files"></a>Fügen Sie Ihr Zertifikat und Schlüsseldateien hinzu:
Geben Sie in den Formularfeldern die base64-codierten Versionen Ihres ZS-Zertifikats, Ihr Serverzertifikat und Ihren Serverschlüssel ein, wie dies in der folgenden Grafik gezeigt wird.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddExtensionFormFilled.png)

> [!NOTE]
> Beachten Sie (analog zur vorherigen Abbildung), dass 2376 standardmäßig aufgefüllt wird. Sie können hier einen beliebigen Endpunkt eingeben. Der nächste Schritt ist jedoch für den übereinstimmenden Endpunkt gedacht. Wenn Sie den Standardwert ändern, müssen Sie die Erschließung des übereinstimmenden Endpunkts im nächsten Schritt sicherstellen.
> 
> 

## <a name="add-the-docker-communication-endpoint"></a>Hinzufügen des Docker-Kommunikationsendpunkts
Wählen Sie beim Anzeigen der von Ihnen erstellten Ressourcengruppe die Netzwerksicherheitsgruppe aus, die Ihrem virtuellen Computer zugeordnet ist. Klicken Sie dann auf **Eingangssicherheitsregeln**, um die Regeln anzuzeigen (siehe Abbildung).

![](./media/virtual-machines-linux-classic-portal-use-docker/AddingEndpoint.png)

Klicken Sie zum Hinzufügen einer weiteren Regel auf **+ Hinzufügen**, und geben Sie im Standardfall einen Namen für den Endpunkt ein (hier: **Docker**). Geben Sie für den Zielportbereich den Wert „2376“ ein. Legen Sie den Wert für das Protokoll auf **TCP** fest, und klicken Sie auf **OK**, um die Regel zu erstellen.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddEndpointFormFilledOut.png)

## <a name="test-your-docker-client-and-azure-docker-host"></a>Testen des Docker-Clients und des Azure Docker-Hosts
Suchen und kopieren Sie den Namen der Domäne Ihres virtuellen Computers. Geben Sie zudem an der Befehlszeile Ihres Clientcomputers `docker --tls -H tcp://`*dockerextension*`.cloudapp.net:2376 info` ein (wobei *dockerextension* durch die Unterdomäne für Ihren virtuellen Computer ersetzt wird).

Das Ergebnis sollte in etwa so aussehen:

```
$ docker --tls -H tcp://dockerextension.cloudapp.net:2376 info
Containers: 0
Images: 0
Storage Driver: devicemapper
 Pool Name: docker-8:1-131214-pool
 Pool Blocksize: 65.54 kB
 Data file: /var/lib/docker/devicemapper/devicemapper/data
 Metadata file: /var/lib/docker/devicemapper/devicemapper/metadata
 Data Space Used: 305.7 MB
 Data Space Total: 107.4 GB
 Metadata Space Used: 729.1 kB
 Metadata Space Total: 2.147 GB
 Library Version: 1.02.82-git (2013-10-04)
Execution Driver: native-0.2
Kernel Version: 3.13.0-36-generic
WARNING: No swap limit support
```

Nachdem Sie die oben genannten Schritte abgeschlossen haben, verfügen Sie nun über einen voll funktionsfähigen Docker-Host auf einer Azure-VM, der so konfiguriert ist, dass eine Remoteverbindung von anderen Clients hergestellt werden kann.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte
Sie sind nun bereit, das [Docker-Benutzerhandbuch] und Ihren virtuellen Docker-Computer zu verwenden. Informationen zum Automatisieren der Erstellung von Docker-Hosts auf Azure-VMs über die Befehlszeilenschnittstelle finden Sie unter [Verwenden der Docker-VM-Erweiterung über die Azure-Befehlszeilenschnittstelle (Azure-CLI)]

<!--Anchors-->
[Create a new VM from the Image Gallery]: #createvm
[Create Docker Certificates]: #dockercerts
[Add the Docker VM Extension]: #adddockerextension
[Test Docker Client and Azure Docker Host]: #testclientandserver
[Next steps]: #next-steps

<!--Image references-->
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[Verwenden der Docker-VM-Erweiterung über die Azure-Befehlszeilenschnittstelle (Azure-CLI)]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-xplat-cli/
[Azure Linux-Agents]: virtual-machines-linux-agent-user-guide.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md

[Running Docker with https]: http://docs.docker.com/articles/https/
[Docker-Benutzerhandbuch]: https://docs.docker.com/userguide/



<!--HONumber=Feb17_HO2-->


