---
title: Verwenden der Azure Docker-VM-Erweiterung | Microsoft Docs
description: Erfahren Sie, wie die Docker-VM-Erweiterung verwendet wird, um schnell und sicher eine Docker-Umgebung in Azure mit Resource Manager-Vorlagen bereitzustellen.
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 936d67d7-6921-4275-bf11-1e0115e66b7f
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/09/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 63cf1a5476a205da2f804fb2f408f4d35860835f
ms.openlocfilehash: e97981118cb8f2c4c2821ed9db50095f2385342c


---
# <a name="create-a-docker-environment-in-azure-using-the-docker-vm-extension"></a>Erstellen einer Docker-Umgebung in Azure mit der Docker-VM-Erweiterung
Docker ist eine beliebte Plattform für Containerverwaltung und Imageerstellung, die es Ihnen ermöglicht, schnell mit Containern unter Linux (und auch Windows) zu arbeiten. In Azure gibt es verschiedene Möglichkeiten, wie Sie Docker gemäß Ihren Anforderungen bereitstellen können. In diesem Artikel geht es um die Verwendung der Docker-VM-Erweiterung und Azure Resource Manager-Vorlagen. 

Weitere Informationen zu den verschiedenen Bereitstellungsmethoden, z.B. der Verwendung von Docker Machine und Azure Container Service, finden Sie in den folgenden Artikeln:

* Zur schnellen Erstellung eines Prototyps für eine App können Sie mit [Docker Machine](virtual-machines-linux-docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) einen einzelnen Docker-Host erstellen.
* Für größere und stabilere Umgebungen können Sie die Azure Docker-VM-Erweiterung verwenden, die auch [Docker Compose](https://docs.docker.com/compose/overview/) unterstützt, um einheitliche Containerbereitstellungen zu generieren. In diesem Artikel wird die Verwendung der Azure Docker-VM-Erweiterung beschrieben.
* Zum Erstellen von produktionsreifen, skalierbaren Umgebungen, die zusätzliche Planungs- und Verwaltungstools bereitstellen, können Sie einen [Docker Swarm-Cluster in Azure Container Services](../container-service/container-service-deployment.md) bereitstellen.

## <a name="azure-docker-vm-extension-overview"></a>Übersicht über die Azure Docker-VM-Erweiterung
Die Azure Docker-VM-Erweiterung installiert und konfiguriert den Docker-Daemon, Docker-Client und Docker Compose auf dem virtuellen Linux-Computer (VM). Durch die Verwendung der Azure Docker-VM-Erweiterung haben Sie eine bessere Kontrolle und bessere Features als bei der alleinigen Nutzung von Docker Machine oder bei der selbstständigen Erstellung des Docker-Hosts. Mit diesen zusätzlichen Features, z.B. [Docker Compose](https://docs.docker.com/compose/overview/), ist die Azure Docker-VM-Erweiterung für robustere Entwickler- oder Produktionsumgebungen geeignet.

Mit Azure Resource Manager-Vorlagen wird die gesamte Struktur Ihrer Umgebung definiert. Mit Vorlagen können Sie Ressourcen erstellen und konfigurieren, z.B. die Docker-Host-VMs, Speicher, rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) und Diagnose. Sie können diese Vorlagen wiederverwenden, um auf einheitliche Weise zusätzliche Bereitstellungen zu erstellen. Weitere Informationen zu Azure Resource Manager und Vorlagen finden Sie unter [Übersicht über den Resource Manager](../azure-resource-manager/resource-group-overview.md). 

## <a name="deploy-a-template-with-the-azure-docker-vm-extension"></a>Bereitstellen einer Vorlage mit der Azure Docker-VM-Erweiterung
Wir verwenden eine vorhandene Schnellstartvorlage zum Erstellen einer Ubuntu-VM, bei der die Azure Docker-VM-Erweiterung zum Installieren und Konfigurieren des Docker-Hosts verwendet wird. Die Vorlage finden Sie hier: [Simple deployment of an Ubuntu VM with Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu)(Einfache Bereitstellung eines virtuellen Ubuntu-Computers mit Docker). 

Installieren Sie die [neueste Version der Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md), und melden Sie sich wie folgt im Resource Manager-Modus an:

```azurecli
azure config mode arm
```

Stellen Sie die Vorlage mit der Azure-Befehlszeilenschnittstelle (CLI) bereit, und geben Sie den Vorlagen-URI an. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen `myResourceGroup` am Standort `WestUS` erstellt. Verwenden Sie den Namen und Standort Ihrer eigenen Ressourcengruppe wie folgt:

```azurecli
azure group create --name myResourceGroup --location "West US" \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Befolgen Sie die Aufforderungen zum Benennen des Speicherkontos, Angeben eines Benutzernamens und Kennworts und Angeben eines DNS-Namens. Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

```azurecli
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Updating resource group myResourceGroup
info:    Updated resource group myResourceGroup
info:    Supply values for the following parameters
newStorageAccountName: mystorageaccount
adminUsername: ops
adminPassword: P@ssword!
dnsNameForPublicIP: mypublicip
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

In der Azure-Befehlszeilenschnittstelle gelangen Sie nach wenigen Sekunden zurück zur Eingabeaufforderung, aber der Docker-Host wird mit der Azure Docker-VM-Erweiterung erstellt und konfiguriert. Es dauert einige Minuten, bis die Bereitstellung abgeschlossen ist. Sie können Details zum Docker-Hoststatus mit dem Befehl `azure vm show` anzeigen.

Im folgenden Beispiel wird der Status der VM mit dem Namen `myDockerVM` (Standardname aus der Vorlage, nicht ändern) in der Ressourcengruppe mit dem Namen `myResourceGroup` überprüft. Geben Sie den Namen der Ressourcengruppe ein, die Sie im vorherigen Schritt erstellt haben:

```azurecli
azure vm show -g myResourceGroup -n myDockerVM
```

Die Ausgabe des Befehls `azure vm show` ähnelt dem folgenden Beispiel:

```azurecli
info:    Executing command vm show
+ Looking up the VM "myDockerVM"
+ Looking up the NIC "myVMNicD"
+ Looking up the public ip "myPublicIPD"
data:    Id                              :/subscriptions/guid/resourceGroups/myresourcegroup/providers/Microsoft.Compute/virtualMachines/MyDockerVM
data:    ProvisioningState               :Succeeded
data:    Name                            :MyDockerVM
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
[...]
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-D3-95
data:          Provisioning State        :Succeeded
data:          Name                      :myVMNicD
data:          Location                  :westus
data:            Public IP address       :13.91.107.235
data:            FQDN                    :mypublicip.westus.cloudapp.azure.com]
data:
data:    Diagnostics Instance View:
info:    vm show command OK
```

Im oberen Bereich der Ausgabe finden Sie den `ProvisioningState` -Wert für den virtuellen Computer. Wenn hier `Succeeded`angezeigt wird, ist die Bereitstellung abgeschlossen, und Sie können sich per SSH beim virtuellen Computer anmelden.

Zum Ende der Ausgabe wird mit `FQDN` der vollqualifizierte Domänenname Ihres Docker-Hosts angezeigt. In den weiteren Schritten verwenden Sie den vollqualifizierten Domänennamen, um sich per SSH beim Docker-Host anzumelden.

## <a name="deploy-your-first-nginx-container"></a>Bereitstellen des ersten nginx-Containers
Nachdem die Bereitstellung abgeschlossen ist, können Sie von Ihrem lokalen Computer per SSH auf Ihren neuen Docker-Host zugreifen. Geben Sie Ihren eigenen Benutzernamen und FQDN wie folgt ein:

```bash
ssh ops@mypublicip.westus.cloudapp.azure.com
```

Nach der Anmeldung am Docker-Host führen wir einen nginx-Container aus:

```bash
sudo docker run -d -p 80:80 nginx
```

Die Ausgabe ähnelt dem folgenden Beispiel, wenn das nginx-Image heruntergeladen und ein Container gestartet wird:

```bash
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
a48df1751a97: Pull complete
8ddc2d7beb91: Pull complete
Digest: sha256:2ca2638e55319b7bc0c7d028209ea69b1368e95b01383e66dfe7e4f43780926d
Status: Downloaded newer image for nginx:latest
b6ed109fb743a762ff21a4606dd38d3e5d35aff43fa7f12e8d4ed1d920b0cd74
```

Überprüfen Sie den Status des Containers, der auf Ihrem Docker-Host ausgeführt wird, wie folgt:

```bash
sudo docker ps
```

Die Ausgabe ähnelt dem folgenden Beispiel. Sie sehen, dass der nginx-Container ausgeführt wird und die Weiterleitung über die TCP-Ports 80 und 443 erfolgt:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

Um den Container in Aktion zu sehen, öffnen Sie einen Webbrowser und geben den FQDN Ihres Docker-Hosts ein:

![Ausführen des ngnix-Containers](./media/virtual-machines-linux-dockerextension/nginxrunning.png)

## <a name="azure-docker-vm-extension-template-reference"></a>Vorlagenreferenz für die Azure Docker-VM-Erweiterung
Im vorherigen Beispiel wird eine vorhandene Schnellstartvorlage verwendet. Sie können die Azure Docker-VM-Erweiterung auch mit eigenen Resource Manager-Vorlagen bereitstellen. Fügen Sie Ihren Resource Manager-Vorlagen hierzu Folgendes hinzu, und definieren Sie den `vmName` Ihrer VM entsprechend:

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "[concat(variables('vmName'), '/DockerExtension'))]",
  "apiVersion": "2015-05-01-preview",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
  ],
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "DockerExtension",
    "typeHandlerVersion": "1.1",
    "autoUpgradeMinorVersion": true,
    "settings": {},
    "protectedSettings": {}
  }
}
```

Eine ausführlichere exemplarische Vorgehensweise zur Verwendung von Resource Manager-Vorlagen finden Sie unter [Übersicht über Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

## <a name="next-steps"></a>Nächste Schritte
Unter Umständen möchten Sie mithilfe von [Docker Compose](https://docs.docker.com/compose/overview/) den [Docker-Daemon-TCP-Port konfigurieren](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), sich mit der [Docker-Sicherheit](https://docs.docker.com/engine/security/security/) vertraut machen oder Container bereitstellen. Weitere Informationen zur Azure Docker-VM-Erweiterung selbst finden Sie im [GitHub-Projekt](https://github.com/Azure/azure-docker-extension/).

Weitere Informationen zu den zusätzlichen Docker-Bereitstellungsoptionen in Azure finden Sie unter:

* [Verwenden eines Docker-Computers mit dem Azure-Treiber](virtual-machines-linux-docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)  
* [Erste Schritte mit Docker und Compose zum Definieren und Ausführen einer Anwendung mit mehreren Containern auf einem virtuellen Azure-Computer](virtual-machines-linux-docker-compose-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Bereitstellen eines Azure Container Service-Clusters](../container-service/container-service-deployment.md)




<!--HONumber=Nov16_HO3-->


