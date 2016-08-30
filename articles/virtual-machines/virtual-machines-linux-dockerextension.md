<properties
   pageTitle="Informationen zur Docker-VM-Erweiterung in Azure | Microsoft Azure"
   description="Erfahren Sie, wie die Docker-VM-Erweiterung verwendet wird, um schnell und sicher eine Docker-Umgebung in Azure bereitzustellen"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="07/20/2016"
   ms.author="iainfou"/>

# Verwenden der Docker-VM-Erweiterung zum Bereitstellen Ihrer Umgebung

Docker ist eine beliebte Plattform für Containerverwaltung und Imageerstellung, die es Ihnen ermöglicht, schnell mit Containern unter Linux (und auch Windows) zu arbeiten. Mit Azure haben Sie die Flexibilität, Docker abhängig von Ihrem Bedarf auf unterschiedliche Arten bereitzustellen:

- Wenn Sie schnell einen Prototyp einer App erstellen möchten, können Sie [den Azure-Treiber für Docker Machine verwenden](./virtual-machines-linux-docker-machine.md), um Docker-Hosts in Azure bereitzustellen.
- Die Docker-VM-Erweiterung für virtuelle Azure-Computer wird für vorlagenbasierte Bereitstellungen verwendet. Dieser Ansatz kann in Azure Resource Manager-Vorlagenbereitstellungen integriert werden und umfasst alle zugehörigen Vorteile, wie z.B. rollenbasierter Zugriff, Diagnose und Konfiguration nach der Bereitstellung.
- Die Docker-VM-Erweiterung unterstützt auch Docker Compose. Docker Compose verwendet eine deklarative YAML-Datei, um aus einer auf einem Entwicklermodell basierenden Anwendung umgebungsübergreifend eine einheitliche Bereitstellung zu generieren.
- Sie können auch [einen vollständigen Docker Swarm-Cluster in Azure Container Services bereitstellen](../container-service/container-service-deployment.md), um produktionsbereite, skalierbare Bereitstellungen zu erhalten, die die zusätzlichen Tools für die Planung und Verwaltung von Swarm nutzen.

Dieser Artikel befasst sich mit der Verwendung von Resource Manager-Vorlagen, um die Docker-VM-Erweiterung in einer von Ihnen definierten angepassten, produktionsbereiten Umgebung bereitzustellen.

## Docker-VM-Erweiterung von Azure für Vorlagenbereitstellungen

Die Azure Docker-VM-Erweiterung installiert und konfiguriert den Docker-Daemon, Docker-Client und Docker Compose auf dem virtuellen Linux-Computer. Die Erweiterung wird auch verwendet, um Containeranwendungen mit Docker Compose zu definieren und bereitzustellen. Sie verfügen über zusätzliche Steuerelemente zur Verwendung von Docker Machine oder zur Erstellung des Docker-Hosts, somit eignet sich diese Lösung besser für stabilere Entwickler- oder Produktionsumgebungen.

Über Azure Resource Manager können Sie Vorlagen erstellen und bereitstellen, die die gesamte Struktur Ihrer Umgebung definieren. Mit Vorlagen können Sie Docker-Hosts, Speicher, rollenbasierte Zugriffssteuerung (RBAC), Diagnosen usw. definieren. [Informieren Sie sich über Resource Manager](../resource-group-overview.md) und Vorlagen, um einige der Vorteile besser zu verstehen. Mit Resource Manager-Vorlagen können Sie auch Bereitstellungen nach Bedarf in der Zukunft reproduzieren.

## Bereitstellen einer Vorlage mit der Docker-VM-Erweiterung:

Wir verwenden eine vorhandene Vorlage für den Schnellstart, um zu veranschaulichen, wie ein virtueller Ubuntu-Computer bereitgestellt wird, auf dem die Docker-VM-Erweiterung installiert ist. Die Vorlage finden Sie hier: [Simple deployment of an Ubuntu VM with Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu) (Einfache Bereitstellung eines virtuellen Ubuntu-Computers mit Docker). Sie benötigen auch die [neueste Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) im Resource Manager-Modus (`azure config mode arm`).

Stellen Sie die Vorlage mithilfe der Azure-Befehlszeilenschnittstelle bereit. Geben Sie dabei einen Namen für die neue Ressourcengruppe (hier `myDockerResourceGroup`) und den Vorlagen-URI an:

```
azure group create --name myDockerResourceGroup --location "West US" \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Beantworten Sie die Eingabeaufforderungen für den Namen des Speicherkontos, den DNS-Namen, den Benutzernamen usw., und warten Sie dann ein paar Minuten, bis die Bereitstellung abgeschlossen ist. Eine Ausgabe ähnlich der folgenden sollte angezeigt werden:

```
info:    Executing command group create
+ Getting resource group myDockerResourceGroup
+ Updating resource group myDockerResourceGroup
info:    Updated resource group myDockerResourceGroup
info:    Supply values for the following parameters
newStorageAccountName: mydockerstorage
adminUsername: ops
adminPassword: P@ssword!
dnsNameForPublicIP: mydockergroup
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/guid/resourceGroups/myDockerResourceGroup
data:    Name:                myDockerResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK

```

## Bereitstellen des ersten nginx-Containers
Stellen Sie nach Abschluss der Bereitstellung eine SSH-Verbindung mit dem neuen Docker-Hosts her. Verwenden Sie dazu den DNS-Namen, den Sie während der Bereitstellung angegeben haben. Versuchen wir, einen nginx-Container auszuführen:

```
sudo docker run -d -p 80:80 nginx
```

Eine Ausgabe ähnlich der folgenden sollte angezeigt werden:

```
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

Überprüfen Sie den auf dem Host ausgeführten Container mit `sudo docker ps`:

```
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

Öffnen Sie einen Webbrowser, und geben Sie den DNS-Namen ein, den Sie während der Bereitstellung angegeben haben, um den Container in Aktion zu sehen:

![Ausführen des ngnix-Containers](./media/virtual-machines-linux-dockerextension/nginxrunning.png)

Sie möchten möglicherweise mithilfe von Docker Compose den Docker-Daemon-TCP-Port und Sicherheit konfigurieren oder Container bereitstellen. Weitere Informationen finden Sie im [GitHub-Projekt für die Azure-VM-Erweiterung für Docker](https://github.com/Azure/azure-docker-extension/).

## JSON-Vorlagenreferenz für die Docker-VM-Erweiterung

In diesem Beispiel wurde eine Vorlage für den Schnellstart verwendet. Um die Docker-VM-Erweiterung in Azure mit Ihren eigenen Resource Manager-Vorlagen bereitzustellen, fügen Sie Folgendes hinzu:

```
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

Eine ausführlichere exemplarische Vorgehensweise zur Verwendung von Resource Manager-Vorlagen finden Sie unter [Übersicht über Azure Resource Manager](../resource-group-overview.md).

## Nächste Schritte

Informieren Sie sich über die genaueren Schritte für die verschiedenen Bereitstellungsoptionen:

1. [Verwenden eines Docker-Computers mit dem Azure-Treiber](./virtual-machines-linux-docker-machine.md)
2. [Verwenden der Docker-VM-Erweiterung aus der Azure-Befehlszeilenschnittstelle (Azure-CLI)](./virtual-machines-linux-classic-cli-use-docker.md)
3. [Erste Schritte mit Docker und Compose zum Definieren und Ausführen einer Anwendung mit mehreren Containern auf einem virtuellen Azure-Computer](virtual-machines-linux-docker-compose-quickstart.md)
3. [Bereitstellen eines Azure Container Service-Clusters](../container-service/container-service-deployment.md)

<!---HONumber=AcomDC_0817_2016-->