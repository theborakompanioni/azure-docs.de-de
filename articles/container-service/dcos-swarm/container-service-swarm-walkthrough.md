---
title: "Schnellstartanleitung: Docker Swarm-Cluster in Azure für Linux | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie in Azure Container Service mithilfe der Azure-Befehlszeilenschnittstelle einen Docker Swarm-Cluster für Linux-Container erstellen."
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service, Docker, Swarm
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/14/2017
ms.author: nepeters
ms.custom: 
ms.translationtype: HT
ms.sourcegitcommit: 1e6fb68d239ee3a66899f520a91702419461c02b
ms.openlocfilehash: 1d10c347795227ed056a95d1bcd4aff82af7b876
ms.contentlocale: de-de
ms.lasthandoff: 08/16/2017

---

# <a name="deploy-docker-swarm-cluster"></a>Bereitstellen eines Docker Swarm-Clusters

In dieser Schnellstartanleitung wird mithilfe der Azure-Befehlszeilenschnittstelle ein Docker Swarm-Cluster bereitgestellt. Anschließend wird in dem Cluster eine Anwendung mit mehreren Containern bereitgestellt und ausgeführt, die ein Web-Front-End und eine Redis-Instanz umfasst. Nach Abschluss des Vorgangs kann auf die Anwendung über das Internet zugegriffen werden.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

Für diese Schnellstartanleitung müssen Sie mindestens Version 2.0.4 der Azure-Befehlszeilenschnittstelle ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#create) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist eine logische Gruppe, in der Azure-Ressourcen bereitgestellt und verwaltet werden.

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *westus* erstellt.

```azurecli-interactive
az group create --name myResourceGroup --location westus
```

Ausgabe:

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup",
  "location": "westcentralus",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-docker-swarm-cluster"></a>Erstellen eines Docker Swarm-Clusters

Erstellen Sie in Azure Container Service mit dem Befehl [az acs create](/cli/azure/acs#create) einen Docker Swarm-Cluster. 

Im folgenden Beispiel wird ein Cluster namens *mySwarmCluster* mit einem Linux-Masterknoten und drei Linux-Agent-Knoten erstellt.

```azurecli-interactive
az acs create --name mySwarmCluster --orchestrator-type Swarm --resource-group myResourceGroup --generate-ssh-keys
```

Nach einigen Minuten ist die Ausführung des Befehls abgeschlossen, und es werden Informationen zum Cluster im JSON-Format zurückgegeben.

## <a name="connect-to-the-cluster"></a>Verbinden mit dem Cluster

In dieser Schnellstartanleitung benötigen Sie die IP-Adresse des Docker Swarm-Masters und des Docker-Agent-Pools. Führen Sie den folgenden Befehl aus, um beide IP-Adressen zurückzugeben.


```bash
az network public-ip list --resource-group myResourceGroup --query "[*].{Name:name,IPAddress:ipAddress}" -o table
```

Ausgabe:

```bash
Name                                                                 IPAddress
-------------------------------------------------------------------  -------------
swarmm-agent-ip-myswarmcluster-myresourcegroup-d5b9d4agent-66066781  52.179.23.131
swarmm-master-ip-myswarmcluster-myresourcegroup-d5b9d4mgmt-66066781  52.141.37.199
```

Erstellen Sie einen SSH-Tunnel zum Swarm-Master. Ersetzen Sie `IPAddress` durch die IP-Adresse des Swarm-Masters.

```bash
ssh -p 2200 -fNL 2375:localhost:2375 azureuser@IPAddress
```

Legen Sie die Umgebungsvariable `DOCKER_HOST` fest. Dadurch können Sie Docker-Befehle für Docker Swarm ausführen, ohne den Namen des Hosts anzugeben.

```bash
export DOCKER_HOST=:2375
```

Nun können Sie Docker-Dienste für Docker Swarm ausführen.


## <a name="run-the-application"></a>Ausführen der Anwendung

Erstellen Sie eine Datei namens `docker-compose.yaml`, und fügen Sie folgenden Inhalt ein.

```yaml
version: '3'
services:
  azure-vote-back:
    image: redis
    container_name: azure-vote-back
    ports:
        - "6379:6379"

  azure-vote-front:
    image: microsoft/azure-vote-front:redis-v1
    container_name: azure-vote-front
    environment:
      REDIS: azure-vote-back
    ports:
        - "80:80"
```

Führen Sie den folgenden Befehl aus, um den Azure Vote-Dienst zu erstellen:

```bash
docker-compose up -d
```

Ausgabe:

```bash
Creating network "user_default" with the default driver
Pulling azure-vote-front (microsoft/azure-vote-front:redis-v1)...
swarm-agent-EE873B23000005: Pulling microsoft/azure-vote-front:redis-v1...
swarm-agent-EE873B23000004: Pulling microsoft/azure-vote-front:redis-v1... : downloaded
Pulling azure-vote-back (redis:latest)...
swarm-agent-EE873B23000004: Pulling redis:latest... : downloaded
Creating azure-vote-front ... 
Creating azure-vote-back ... 
Creating azure-vote-front
Creating azure-vote-back ...
```

## <a name="test-the-application"></a>Testen der Anwendung

Navigieren Sie zur IP-Adresse des Swarm-Agent-Pools, um die Azure Vote-Anwendung zu testen.

![Abbildung der Navigation zu Azure Vote](media/container-service-docker-swarm-mode-walkthrough/azure-vote.png)

## <a name="delete-cluster"></a>Löschen von Clustern
Wenn der Cluster nicht mehr benötigt wird, können Sie mit dem Befehl [az group delete](/cli/azure/group#delete) die Ressourcengruppe, den Containerdienst und alle zugehörigen Ressourcen entfernen.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="get-the-code"></a>Abrufen des Codes

In dieser Schnellstartanleitung wurden vorab erstellte Containerimages verwendet, um einen Docker-Dienst zu erstellen. Der dazugehörige Anwendungscode, die Dockerfile-Datei und die Compose-Datei sind auf GitHub verfügbar.

[https://github.com/Azure-Samples/azure-voting-app-redis](https://github.com/Azure-Samples/azure-voting-app-redis.git)

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen Docker Swarm-Cluster und eine Anwendung mit mehreren Containern dafür bereitgestellt.

Informationen zur Integration von Docker Swarm in Visual Studio Team Services finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [CD/CI mit Docker Swarm und VSTS](./container-service-docker-swarm-setup-ci-cd.md)
