---
title: Verwenden der Azure Docker-VM-Erweiterung | Microsoft Docs
description: Erfahren Sie, wie die Docker-VM-Erweiterung verwendet wird, um schnell und sicher eine Docker-Umgebung in Azure mit Resource Manager-Vorlagen und Azure CLI 2.0 bereitzustellen.
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
ms.date: 02/23/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 508c8433620a1ba3feabc1b35ca2ad461680f37c
ms.openlocfilehash: 0d1146b4e1d707f914fcb535210b71897efd0b29
ms.lasthandoff: 03/01/2017


---
# <a name="create-a-docker-environment-in-azure-using-the-docker-vm-extension"></a>Erstellen einer Docker-Umgebung in Azure mit der Docker-VM-Erweiterung
Docker ist eine beliebte Plattform für die Containerverwaltung und Imageerstellung, die es Ihnen ermöglicht, schnell mit Containern unter Linux zu arbeiten. In Azure gibt es verschiedene Möglichkeiten, wie Sie Docker gemäß Ihren Anforderungen bereitstellen können. In diesem Artikel geht es um die Verwendung der Docker-VM-Erweiterung und der Azure Resource Manager-Vorlagen mithilfe von Azure CLI 2.0. Sie können diese Schritte auch mit [Azure CLI 1.0](virtual-machines-linux-dockerextension-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ausführen.

## <a name="azure-docker-vm-extension-overview"></a>Übersicht über die Azure Docker-VM-Erweiterung
Die Azure Docker-VM-Erweiterung installiert und konfiguriert den Docker-Daemon, Docker-Client und Docker Compose auf dem virtuellen Linux-Computer (VM). Durch die Verwendung der Azure Docker-VM-Erweiterung haben Sie eine bessere Kontrolle und bessere Features als bei der alleinigen Nutzung von Docker Machine oder bei der selbstständigen Erstellung des Docker-Hosts. Mit diesen zusätzlichen Features, z.B. [Docker Compose](https://docs.docker.com/compose/overview/), ist die Azure Docker-VM-Erweiterung für robustere Entwickler- oder Produktionsumgebungen geeignet.

Weitere Informationen zu den verschiedenen Bereitstellungsmethoden, z.B. der Verwendung von Docker Machine und Azure Container Service, finden Sie in den folgenden Artikeln:

* Zur schnellen Erstellung eines Prototyps für eine App können Sie mit [Docker Machine](virtual-machines-linux-docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) einen einzelnen Docker-Host erstellen.
* Zum Erstellen von produktionsreifen, skalierbaren Umgebungen, die zusätzliche Planungs- und Verwaltungstools bereitstellen, können Sie einen [Docker Swarm-Cluster in Azure Container Services](../container-service/container-service-deployment.md) bereitstellen.

## <a name="deploy-a-template-with-the-azure-docker-vm-extension"></a>Bereitstellen einer Vorlage mit der Azure Docker-VM-Erweiterung
Wir verwenden eine vorhandene Schnellstartvorlage zum Erstellen einer Ubuntu-VM, bei der die Azure Docker-VM-Erweiterung zum Installieren und Konfigurieren des Docker-Hosts verwendet wird. Die Vorlage finden Sie hier: [Simple deployment of an Ubuntu VM with Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu)(Einfache Bereitstellung eines virtuellen Ubuntu-Computers mit Docker). Die neueste Version von [Azure CLI 2.0](/cli/azure/install-az-cli2) muss installiert sein, und Sie müssen mithilfe von [az login](/cli/azure/#login) bei einem Azure-Konto angemeldet sein.

Erstellen Sie zunächst mit [az group create](/cli/azure/group#create) eine Ressourcengruppe. Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myResourceGroup` am Standort `West US`:

```azurecli
 az group create --name myResourceGroup --location westus
```

Stellen Sie als Nächstes mit [az group deployment create](/cli/azure/group/deployment#create) einen virtuellen Computer bereit, der die Azure Docker-VM-Erweiterung aus [dieser Azure Resource Manager-Vorlage auf GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu) enthält. Geben Sie wie folgt eigene Werte für `newStorageAccountName`, `adminUsername`, `adminPassword` und `dnsNameForPublicIP` an:

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"newStorageAccountName": {"value": "mystorageaccount"},
    "adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "dnsNameForPublicIP": {"value": "mypublicdns"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Es dauert einige Minuten, bis die Bereitstellung abgeschlossen ist. Wenn die Bereitstellung abgeschlossen ist, [fahren Sie mit dem nächsten Schritt fort](#deploy-your-first-nginx-container), um eine SSH-Verbindung mit Ihrem virtuellen Computer herzustellen. 

Optional können Sie dem vorherigen Befehl das `--no-wait`-Flag hinzufügen, um stattdessen die Steuerung an die Eingabeaufforderung zurückzugeben und die Bereitstellung im Hintergrund auszuführen. Auf diese Weise können Sie die Befehlszeilenschnittstelle für andere Aufgaben nutzen, während die Bereitstellung ausgeführt wird. Sie können dann mit dem Befehl [az vm show](/cli/azure/vm#show) Details zum Docker-Hoststatus anzeigen. Im folgenden Beispiel wird der Status des virtuellen Computers `myDockerVM` (der Standardname aus der Vorlage – ändern Sie diesen Namen nicht) in der Ressourcengruppe `myResourceGroup` überprüft:

```azurecli
az vm show --resource-group myResourceGroup --name myDockerVM \
  --query [provisioningState] --output tsv
```

Wenn diese Befehl `Succeeded` zurückgibt, ist die Bereitstellung abgeschlossen, und Sie können im nächsten Schritt eine SSH-Verbindung mit dem virtuellen Computer herstellen.

## <a name="deploy-your-first-nginx-container"></a>Bereitstellen des ersten nginx-Containers
Verwenden Sie `az vm show -g myResourceGroup -n myDockerVM -d --query [fqdns] -o tsv`, um die Details Ihrer VM anzuzeigen, einschließlich DNS-Name. Wechseln Sie wie folgt von Ihrem lokalen Computer per SSH auf Ihren neuen Docker-Host:

```bash
ssh azureuser@mypublicdns.westus.cloudapp.azure.com
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

Um den Container in Aktion zu sehen, öffnen Sie einen Webbrowser und geben den DNS-Namen Ihres Docker-Hosts ein:

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


