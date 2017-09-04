---
title: "Tutorial für Azure Containerdienst – DC/OS verwalten | Microsoft-Dokumentation"
description: "Tutorial für Azure Containerdienst – DC/OS verwalten"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Container, Microservices, Kubernetes, DC/OS, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/17/2017
ms.author: nepeters
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 7456da29aa07372156f2b9c08ab83626dab7cc45
ms.openlocfilehash: e93f782c26c32f97749e817ec59ee3c2ecb7e119
ms.contentlocale: de-de
ms.lasthandoff: 08/28/2017

---

# <a name="azure-container-service-tutorial---manage-dcos"></a>Tutorial für Azure Containerdienst – DC/OS verwalten

DC/OS bietet eine verteilte Plattform für das Ausführen moderner und containerbasierter Anwendungen. Mit dem Azure Container Service ist die Bereitstellung eines produktionsbereiten DC/OS-Clusters schnell und einfach. In diesem Schnellstart werden die notwendigen grundlegenden Schritte zum Bereitstellen eines DC/OS-Clusters und zur Ausführung eines einfachen Workloads aufgeführt.

> [!div class="checklist"]
> * Erstellen eines ACS-DC/OS-Clusters
> * Verbinden mit dem Cluster
> * Installieren der DC/OS-CLI
> * Übermitteln einer Anwendung an den Cluster
> * Skalieren einer Anwendung auf dem Cluster
> * Skalieren der DC/OS-Clusterknoten
> * Grundlegende DC/OS-Verwaltung
> * Löschen des DC/OS-Clusters

Für dieses Tutorial ist mindestens Version 2.0.4 der Azure CLI erforderlich. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

## <a name="create-dcos-cluster"></a>Erstellen eines DC/OS-Clusters

Erstellen Sie zunächst mit dem Befehl [az group create](/cli/azure/group#create) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. 

Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *westeurope*.

```azurecli
az group create --name myResourceGroup --location westeurope
```

Erstellen Sie als Nächstes einen DC/OS-Cluster mit dem Befehl [az acs create](/cli/azure/acs#create).

Das folgende Beispiel erstellt einen DC/OS-Cluster mit dem Namen *MyDCOSCluster* und SSH-Schlüssel, falls diese nicht bereits vorhanden sind. Um einen bestimmten Satz von Schlüsseln zu verwenden, nutzen Sie die Option `--ssh-key-value`.  

```azurecli
az acs create \
  --orchestrator-type dcos \
  --resource-group myResourceGroup \
  --name myDCOSCluster \
  --generate-ssh-keys
```

Nach einigen Minuten ist der Befehl abgeschlossen, und gibt Informationen zu der Bereitstellung zurück.

## <a name="connect-to-dcos-cluster"></a>Herstellen einer Verbindung mit dem Cluster DC/OS

Sobald ein DC/OS-Cluster erstellt wurde, kann darauf über einen SSH-Tunnel zugegriffen werden. Führen Sie den folgenden Befehl aus, um die öffentliche IP-Adresse des DC/OS-Masters zurückzugeben. Diese IP-Adresse wird in einer Variablen gespeichert und im nächsten Schritt verwendet.

```azurecli
ip=$(az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-master')].[ipAddress]" -o tsv)
```

Um den SSH-Tunnel zu erstellen, führen Sie den folgenden Befehl aus, und befolgen Sie die Anweisungen auf dem Bildschirm. Wenn Port 80 bereits verwendet wird, löst der Befehl einen Fehler aus. Aktualisieren Sie den getunnelten Port auf einen aktuell nicht verwendeten Container, wie z.B. `85:localhost:80`. 

```azurecli
sudo ssh -i ~/.ssh/id_rsa -fNL 80:localhost:80 -p 2200 azureuser@$ip
```

## <a name="install-dcos-cli"></a>Installieren der DC/OS-CLI

Installieren Sie die DC/OS-CLI mithilfe des Befehls [az acs dcos install-cli](/azure/acs/dcos#install-cli). Wenn Sie Azure CloudShell verwenden, ist die DC/OS-CLI bereits installiert. Wenn Sie die Azure CLI unter macOS oder Linux ausführen, müssen Sie den Befehl möglicherweise mit „sudo“ ausführen.

```azurecli
az acs dcos install-cli
```

Bevor die CLI mit dem Cluster verwendet werden kann, muss sie für die Verwendung des SSH-Tunnels konfiguriert werden. Führen Sie hierzu den folgenden Befehl aus, und passen Sie gegebenenfalls den Port an.

```azurecli
dcos config set core.dcos_url http://localhost
```

## <a name="run-an-application"></a>Ausführen einer Anwendung

Der Standardplanungsmechanismus für einen Cluster mit ACS-DC/OS-Cluster ist Marathon. Marathon wird verwendet, um eine Anwendung zu starten und den Status der Anwendung auf dem DC/OS-Cluster zu verwalten. Um eine Anwendung über Marathon zu planen, erstellen Sie eine Datei mit dem Namen **marathon-app.json**, und kopieren Sie den folgenden Inhalt hinein. 

```json
{
  "id": "demo-app-private",
  "cmd": null,
  "cpus": 1,
  "mem": 32,
  "disk": 0,
  "instances": 1,
  "container": {
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ]
    },
    "type": "DOCKER"
  }
}
```

Führen Sie den folgenden Befehl aus, um die Ausführung der Anwendung auf dem DC/OS-Cluster zu planen.

```azurecli
dcos marathon app add marathon-app.json
```

Um den Bereitstellungsstatus für die App anzuzeigen, führen Sie den folgenden Befehl aus.

```azurecli
dcos marathon app list
```

Wenn der Spaltenwert **TASKS** von *0/1* auf *1/1* wechselt, wurde die Anwendungsbereitstellung abgeschlossen.

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     0/1    ---       ---      False      DOCKER   None
```

## <a name="scale-marathon-application"></a>Skalieren der Marathon-Anwendung

Im vorherigen Beispiel wurde eine Einzelinstanzanwendung erstellt. Um diese Bereitstellung zu aktualisieren, sodass drei Instanzen der Anwendung verfügbar sind, öffnen Sie die Datei **marathon-app.json**, und aktualisieren Sie die Instanzeigenschaft auf 3.

```json
{
  "id": "demo-app-private",
  "cmd": null,
  "cpus": 1,
  "mem": 32,
  "disk": 0,
  "instances": 3,
  "container": {
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ]
    },
    "type": "DOCKER"
  }
}
```

Aktualisieren Sie die Anwendung mit dem Befehl `dcos marathon app update`.

```azurecli
dcos marathon app update demo-app-private < marathon-app.json
```

Um den Bereitstellungsstatus für die App anzuzeigen, führen Sie den folgenden Befehl aus.

```azurecli
dcos marathon app list
```

Wenn der Spaltenwert **TASKS** von *1/3* auf *3/1* wechselt, wurde die Anwendungsbereitstellung abgeschlossen.

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     1/3    ---       ---      False      DOCKER   None
```

## <a name="run-internet-accessible-app"></a>Ausführen einer App, auf die über das Internet zugegriffen werden kann

Der ACS-DC/OS-Cluster besteht aus zwei NodeSets: ein öffentliches, auf das über das Internet zugegriffen werden kann, und ein privates, auf das nicht über das Internet zugegriffen werden kann. Das Standardset sind die privaten Knoten, die im letzten Beispiel verwendet wurden.

Um eine Anwendung über das Internet abrufbar zu machen, können Sie diese auf dem öffentlichen NodeSet bereitstellen. Geben Sie hierzu dem `acceptedResourceRoles`-Objekt einen Wert von `slave_public`.

Erstellen Sie eine Datei mit dem Namen **nginx-public.json** und kopieren Sie den folgenden Inhalt hinein.

```json
{
  "id": "demo-app",
  "cmd": null,
  "cpus": 1,
  "mem": 32,
  "disk": 0,
  "instances": 1,
  "container": {
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "hostPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ]
    },
    "type": "DOCKER"
  },
  "acceptedResourceRoles": [
    "slave_public"
  ]
}
```

Führen Sie den folgenden Befehl aus, um die Ausführung der Anwendung auf dem DC/OS-Cluster zu planen.

```azurecli 
dcos marathon app add nginx-public.json
```

Rufen Sie die öffentliche IP-Adresse der öffentlichen DC/OS Cluster-Agents ab.

```azurecli 
az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-agent')].[ipAddress]" -o tsv
```

Wenn Sie zu dieser Adresse navigieren, wird die Standard-NGINX-Website zurückgegeben.

![NGINX](./media/container-service-dcos-manage-tutorial/nginx.png)

## <a name="scale-dcos-cluster"></a>Skalierung des DC/OS-Clusters

In den vorherigen Beispielen wurde eine Anwendung auf mehreren Instanzen skaliert. Die DC/OS-Infrastruktur kann auch für die Bereitstellung von mehr oder weniger Rechenkapazität skaliert werden. Dies erfolgt mit dem Befehl [az acs scale](). 

Um die aktuelle Anzahl von DC/OS-Agents anzuzeigen, verwenden Sie den Befehl [az acs show](/cli/azure/acs#show).

```azurecli
az acs show --resource-group myResourceGroup --name myDCOSCluster --query "agentPoolProfiles[0].count"
```

Um die Anzahl auf 5 zu erhöhen, verwenden Sie den Befehl [az acs scale](/cli/azure/acs#scale). 

```azurecli
az acs scale --resource-group myResourceGroup --name myDCOSCluster --new-agent-count 5
```

## <a name="delete-dcos-cluster"></a>Löschen eines DC/OS-Clusters

Mit dem Befehl [az group delete](/cli/azure/group#delete) können Sie die Ressourcengruppe, das DC/OS-Cluster und alle dazugehörigen Ressourcen entfernen, wenn sie nicht mehr benötigt werden.

```azurecli 
az group delete --name myResourceGroup --no-wait
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie u.a. folgende grundlegende DC/OS-Verwaltungstasks kennengelernt. 

> [!div class="checklist"]
> * Erstellen eines ACS-DC/OS-Clusters
> * Verbinden mit dem Cluster
> * Installieren der DC/OS-CLI
> * Übermitteln einer Anwendung an den Cluster
> * Skalieren einer Anwendung auf dem Cluster
> * Skalieren der DC/OS-Clusterknoten
> * Löschen des DC/OS-Clusters

Fahren Sie mit dem nächsten Tutorial fort, um den Lastausgleich für Anwendungen in DC/OS in Azure kennenzulernen. 

> [!div class="nextstepaction"]
> [Lastausgleich für Anwendungen](container-service-load-balancing.md)
