---
title: "Schnellstartanleitung: Azure Docker CE-Cluster für Linux | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie in Azure Container Service mithilfe der Azure-Befehlszeilenschnittstelle einen Docker CE-Cluster für Linux-Container erstellen."
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
ms.date: 08/25/2017
ms.author: nepeters
ms.custom: 
ms.translationtype: HT
ms.sourcegitcommit: 5b6c261c3439e33f4d16750e73618c72db4bcd7d
ms.openlocfilehash: 7b8336e3865e7032e3ee0d5e4ee712bcb95aa4b5
ms.contentlocale: de-de
ms.lasthandoff: 08/28/2017

---

# <a name="deploy-docker-ce-cluster"></a>Bereitstellen eines Docker CE-Clusters

In dieser Schnellstartanleitung wird ein Docker CE-Cluster mithilfe der Azure-Befehlszeilenschnittstelle bereitgestellt. Anschließend wird in dem Cluster eine Anwendung mit mehreren Containern bereitgestellt und ausgeführt, die ein Web-Front-End und eine Redis-Instanz umfasst. Nach Abschluss des Vorgangs kann auf die Anwendung über das Internet zugegriffen werden.

Docker CE in Azure Container Service befindet sich in der Vorschauphase und **darf nicht für Produktionsworkloads verwendet werden**.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Schnellstart die Azure CLI-Version 2.0.4 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#create) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist eine logische Gruppe, in der Azure-Ressourcen bereitgestellt und verwaltet werden.

Im folgenden Beispiel wird eine Ressourcengruppe namens *myResourceGroup* am Standort *ukwest* erstellt.

```azurecli-interactive
az group create --name myResourceGroup --location ukwest
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

Erstellen Sie mit dem Befehl [az acs create](/cli/azure/acs#create) einen Docker CE-Cluster in Azure Container Service. 

Im folgenden Beispiel wird ein Cluster namens *mySwarmCluster* mit einem Linux-Masterknoten und drei Linux-Agent-Knoten erstellt.

```azurecli-interactive
az acs create --name mySwarmCluster --orchestrator-type dockerce --resource-group myResourceGroup --generate-ssh-keys
```

Nach einigen Minuten ist die Ausführung des Befehls abgeschlossen, und es werden Informationen zum Cluster im JSON-Format zurückgegeben.

## <a name="connect-to-the-cluster"></a>Verbinden mit dem Cluster

In dieser Schnellstartanleitung benötigen Sie den vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) des Docker Swarm-Masters und des Docker-Agent-Pools. Führen Sie den folgenden Befehl aus, um die FQDNs für Master und Agent zurückgegeben.


```bash
az acs list --resource-group myResourceGroup --query '[*].{Master:masterProfile.fqdn,Agent:agentPoolProfiles[0].fqdn}' -o table
```

Ausgabe:

```bash
Master                                                               Agent
-------------------------------------------------------------------  --------------------------------------------------------------------
myswarmcluster-myresourcegroup-d5b9d4mgmt.ukwest.cloudapp.azure.com  myswarmcluster-myresourcegroup-d5b9d4agent.ukwest.cloudapp.azure.com
```

Erstellen Sie einen SSH-Tunnel zum Swarm-Master. Ersetzen Sie `MasterFQDN` durch die FQDN-Adresse des Swarm-Masters.

```bash
ssh -p 2200 -fNL localhost:2374:/var/run/docker.sock azureuser@MasterFQDN
```

Legen Sie die Umgebungsvariable `DOCKER_HOST` fest. Dadurch können Sie Docker-Befehle für Docker Swarm ausführen, ohne den Namen des Hosts anzugeben.

```bash
export DOCKER_HOST=localhost:2374
```

Nun können Sie Docker-Dienste für Docker Swarm ausführen.


## <a name="run-the-application"></a>Ausführen der Anwendung

Erstellen Sie eine Datei namens `azure-vote.yaml`, und fügen Sie folgenden Inhalt ein.


```yaml
version: '3'
services:
  azure-vote-back:
    image: redis
    ports:
        - "6379:6379"

  azure-vote-front:
    image: microsoft/azure-vote-front:redis-v1
    environment:
      REDIS: azure-vote-back
    ports:
        - "80:80"
```

Führen Sie den Befehl [docker stack deploy](https://docs.docker.com/engine/reference/commandline/stack_deploy/) aus, um den Azure Vote-Dienst zu erstellen.

```bash
docker stack deploy azure-vote --compose-file azure-vote.yaml
```

Ausgabe:

```bash
Creating network azure-vote_default
Creating service azure-vote_azure-vote-back
Creating service azure-vote_azure-vote-front
```

Geben Sie mithilfe des Befehls [docker stack ps](https://docs.docker.com/engine/reference/commandline/stack_ps/) den Bereitstellungsstatus der Anwendung zurück.

```bash
docker stack ps azure-vote
```

Sobald `CURRENT STATE` für jeden Dienst `Running` lautet, ist die Anwendung bereit.

```bash
ID                  NAME                            IMAGE                                 NODE                               DESIRED STATE       CURRENT STATE                ERROR               PORTS
tnklkv3ogu3i        azure-vote_azure-vote-front.1   microsoft/azure-vote-front:redis-v1   swarmm-agentpool0-66066781000004   Running             Running 5 seconds ago                            
lg99i4hy68r9        azure-vote_azure-vote-back.1    redis:latest                          swarmm-agentpool0-66066781000002   Running             Running about a minute ago
```

## <a name="test-the-application"></a>Testen der Anwendung

Navigieren Sie zum FQDN des Swarm-Agent-Pools, um die Azure Vote-Anwendung zu testen.

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
