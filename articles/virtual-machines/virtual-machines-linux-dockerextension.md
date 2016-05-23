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
   ms.date="05/04/2016"
   ms.author="iainfou"/>

# Verwenden der Docker-VM-Erweiterung zum Bereitstellen Ihrer Umgebung

Docker ist eine beliebte Plattform für Containerverwaltung und Imageerstellung, die es Ihnen ermöglicht, schnell mit Containern unter Linux (und auch Windows) zu arbeiten. Mit Azure haben Sie die Flexibilität, Docker abhängig von Ihrem Bedarf auf unterschiedliche Arten bereitzustellen:

- Wenn Sie schnell einen Prototyp einer App erstellen möchten oder wenn Sie Docker Machine bereits kennen und verwenden, können Sie [den Azure-Treiber für Docker Machine verwenden](./virtual-machines-linux-docker-machine.md), um Docker-Hosts in Azure bereitzustellen.
- Für eine vorlagenbasierte Bereitstellung kann die Docker-VM-Erweiterung für virtuelle Azure-Computer verwendet werden. Dieser Ansatz kann in Azure Resource Manager-Vorlagenbereitstellungen integriert werden und umfasst alle zugehörigen Vorteile, wie z. B. rollenbasierter Zugriff, Diagnose und Konfiguration nach der Bereitstellung.
- Sie können auch [einen vollständigen Docker Swarm-Cluster in Azure Container Services bereitstellen](../container-service/container-service-deployment.md), um produktionsbereite, skalierbare Bereitstellungen zu erhalten, die die zusätzlichen Tools für Planung und Verwaltung von Swarm nutzen.

Dieser Artikel befasst sich mit der Verwendung von Resource Manager-Vorlagen, um die Docker-VM-Erweiterung in einer von Ihnen definierten angepassten, produktionsbereiten Umgebung bereitzustellen.

## Docker-VM-Erweiterung von Azure für Vorlagenbereitstellungen

Der Azure Docker-VM-Erweiterung installiert und konfiguriert den Docker-Daemon, Docker-Client und Docker Compose auf dem virtuellen Linux-Computer. Die Erweiterung kann auch verwendet werden, um Containeranwendungen mit Docker Compose zu definieren und bereitzustellen. Mithilfe von Resource Manager-Vorlagen kann die Umgebung dann auf konsistente Weise erneut bereitgestellt werden. Die Verwendung der Docker-VM-Erweiterung von Azure eignet sich gut für stabilere Entwickler- oder Produktionsumgebungen, da Sie im Vergleich mit der einfachen Docker Machine-Nutzung oder der eigenen Erstellung des Docker-Hosts über eine zusätzliche Kontrolle verfügen.

Mithilfe von Azure Resource Manager können Sie Vorlagen erstellen und bereitstellen, mit denen die gesamte Struktur der Umgebung definiert wird, z.B. Docker-Hosts, Speicher, rollenbasierte Zugriffssteuerung (RBAC), Diagnose usw. [Informieren Sie sich über Resource Manager](../resource-group-overview.md) und Vorlagen, um einige der Vorteile besser zu verstehen. Der Vorteil der Verwendung von Resource Manager-Vorlagen gegenüber der einfachen Verwendung von Docker Machine ist, dass Sie zusätzliche Docker-Hosts, Speicher, Zugriffssteuerung usw. definieren können und in der Lage sind, die Bereitstellungen bei Bedarf in der Zukunft zu reproduzieren.

## Bereitstellen einer Vorlage mit der Docker-VM-Erweiterung:

Wir verwenden eine vorhandene Vorlage für den Schnellstart, um zu veranschaulichen, wie ein virtueller Ubuntu-Computer bereitgestellt wird, auf dem die Docker-VM-Erweiterung installiert ist. Sie können die Vorlage hier anzeigen: [Simple deployment of an Ubuntu VM with Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu) (Einfache Bereitstellung eines virtuellen Ubuntu-Computers mit Docker)

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

Stellen Sie nach Abschluss der Bereitstellung eine SSH-Verbindung mit dem neuen Docker-Hosts her. Verwenden Sie dazu den DNS-Namen, den Sie während der Bereitstellung angegeben haben. Die Docker-Tools sind bereits installiert. Versuchen wir also, einen nginx-Container auszuführen:

```
docker run -d -p 80:80 nginx
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

Überprüfen Sie den Container, der auf dem Host ausgeführt wird, mit `docker ps`:

```
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   nostalgic_murdock
```

Öffnen Sie einen Webbrowser, und geben Sie den DNS-Namen ein, den Sie während der Bereitstellung angegeben haben, um den Container in Aktion zu sehen:

![Ausführen des ngnix-Containers](./media/virtual-machines-linux-dockerextension/nginxrunning.png)

Weitere Informationen zur Docker-VM-Erweiterung, z. B. zum Konfigurieren des Docker-Daemon-TCP-Ports, zum Konfigurieren der Sicherheit und zum Bereitstellen von Containern mithilfe von Docker Compose, finden Sie im Thema zur [Azure-VM-Erweiterung für Docker-GitHub-Projekt](https://github.com/Azure/azure-docker-extension/).

## JSON-Vorlagenreferenz für die Docker-VM-Erweiterung

In diesem Beispiel wurde eine Vorlage für den Schnellstart verwendet. Sie können Ihre vorhandenen Resource Manager-Vorlagen verwenden, um die Docker-VM-Erweiterung auf virtuellen Computern zu installieren, die in der Vorlage definiert sind, indem Sie Folgendes in der JSON-Definitionsdatei hinzufügen:

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

Eine ausführlichere exemplarische Vorgehensweise zum Verwenden von Resource Manager-Vorlagen finden Sie unter [Übersicht über Azure Resource Manager](../resource-group-overview.md).

## Nächste Schritte

Informieren Sie sich über die genaueren Schritte für die verschiedenen Bereitstellungsoptionen:

1. [Verwenden eines Docker-Computers mit dem Azure-Treiber](./virtual-machines-linux-docker-machine.md)  
2. [Verwenden der Docker-VM-Erweiterung aus der Azure-Befehlszeilenschnittstelle (Azure-CLI)](./virtual-machines-linux-classic-cli-use-docker.md)  
3. [Bereitstellen eines Azure Container Service-Clusters](../container-service/container-service-deployment.md)

<!---HONumber=AcomDC_0511_2016-->