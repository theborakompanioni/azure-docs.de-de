---
title: "Tutorial für Azure Container Service – Vorbereiten von ACR | Microsoft-Dokumentation"
description: "Tutorial für Azure Container Service –Vorbereiten von ACR"
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
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/06/2017
ms.author: nepeters
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: adc00cb52d5805e152ca742f4cd6b9fab3ed3788
ms.contentlocale: de-de
ms.lasthandoff: 06/28/2017

---

# <a name="deploy-and-use-azure-container-registry"></a>Bereitstellen und Verwenden von Azure Container Registry

Azure Container Registry (ACR) ist eine Azure-basierte, private Registrierung für Docker-Containerimages. In diesem Tutorial werden die Schritte für die Bereitstellung einer Azure Container Registry-Instanz und die Übertragung von Containerimages in diese Instanz per Push erläutert. Folgende Schritte werden ausgeführt:

> [!div class="checklist"]
> * Bereitstellen einer Azure Container Registry-Instanz
> * Kennzeichnen von Containerimages für ACR
> * Hochladen von Images in ACR

In den nachfolgenden Tutorials wird diese ACR-Instanz in einen Azure Container Service-Kubernetes-Cluster integriert, sodass Containerimages sicher ausgeführt werden können. 

## <a name="before-you-begin"></a>Voraussetzungen

Im [vorherigen Tutorial](./container-service-tutorial-kubernetes-prepare-app.md) wurden Containerimages für eine einfache Azure Voting App erstellt. In diesem Tutorial werden diese Images per Push in eine Azure Container Registry-Instanz übertragen. Wenn Sie die Images der Azure Voting App noch nicht erstellt haben, kehren Sie zum [Tutorial 1 – Erstellen von Containerimages](./container-service-tutorial-kubernetes-prepare-app.md) zurück. Alternativ können die hier beschriebenen Schritte für jegliche Containerimages ausgeführt werden.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial die Azure CLI-Version 2.0.4 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

## <a name="deploy-azure-container-registry"></a>Bereitstellen von Azure Container Registry

Für die Bereitstellung einer Azure Container Registry-Instanz benötigen Sie zunächst eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#create) eine Ressourcengruppe. In diesem Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* in der Region *eastus* erstellt.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Erstellen Sie mit dem Befehl [az acr create](/cli/azure/acr#create) eine Azure Container Registry-Instanz. Der Name einer Containerregistrierung **muss eindeutig sein**. Verwenden Sie das folgende Beispiel, und aktualisieren Sie den Namen mit zufällig gewählten Zeichen.

```azurecli-interactive
az acr create --resource-group myResourceGroup --name myContainerRegistry007 --sku Basic --admin-enabled true
```

## <a name="get-acr-information"></a>Abrufen von ACR-Informationen 

Nach dem Erstellen der ACR-Instanz werden der Name, der Name des Anmeldeservers und das Authentifizierungskennwort benötigt. Mit dem folgenden Code werden alle diese Werte zurückgegeben. Notieren Sie sich diese Werte, auf sie wird im gesamten Tutorial Bezug genommen.  

ACR-Name und -Anmeldeserver:

```azurecli-interactive
az acr list --resource-group myResourceGroup --query "[].{acrName:name,acrLoginServer:loginServer}" --output table
```

ACR-Kennwort – muss mit dem ACR-Namen aktualisiert werden.

```azurecli-interactive
az acr credential show --name <acrName> --query passwords[0].value -o tsv
```

## <a name="container-registry-login"></a>Anmeldung bei der Containerregistrierung

Sie müssen sich bei der ACR-Instanz anmelden, damit Sie Images per Push in sie übertragen können. Verwenden Sie den Befehl [docker login](https://docs.docker.com/engine/reference/commandline/login/), um den Vorgang abzuschließen. Wenn Sie den Befehl „docker login“ ausführen, müssen Sie den Namen des ACR-Anmeldeservers und die ACR-Anmeldeinformationen angeben.

```bash
docker login --username=<acrName> --password=<acrPassword> <acrLoginServer>
```

Nach Abschluss des Vorgangs wird eine Erfolgsmeldung zurückgegeben.

## <a name="tag-container-images"></a>Kennzeichnen von Containerimages

Jedes Containerimage muss mit dem `loginServer`-Namen der Registrierung gekennzeichnet werden. Dieses Tag wird beim Übertragen von Containerimages per Push in eine Imageregistrierung für das Routing verwendet.

Verwenden Sie den Befehl `docker images`, um eine Liste der aktuellen Images anzuzeigen.

```bash
docker images
```

Ausgabe:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
azure-vote-front             latest              c13c4f50ede1        39 seconds ago       716 MB
azure-vote-back              latest              33fe5afc1885        About a minute ago   407 MB
mysql                        latest              e799c7f9ae9c        4 weeks ago          407 MB
tiangolo/uwsgi-nginx-flask   flask               788ca94b2313        8 months ago         694 MB
```

Kennzeichnen Sie das Image *azure-vote-front* mit dem loginServer-Namen der Containerregistrierung. Fügen Sie zudem `:v1` am Ende des Imagenamens hinzu. Dieses Tag gibt die Imageversionsnummer an.

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:v1
```

Führen Sie den Befehl auch für das Image *azure-vote-back* aus.

```bash
docker tag azure-vote-back <acrLoginServer>/azure-vote-back:v1
```

Führen Sie nach der Kennzeichnung den Befehl `docker images` aus, um den Vorgang zu überprüfen.

```bash
docker images
```

Ausgabe:

```bash
REPOSITORY                                           TAG                 IMAGE ID            CREATED             SIZE
azure-vote-back                                      latest              a9dace4e1a17        7 minutes ago       407 MB
mycontainerregistry082.azurecr.io/azure-vote-back    v1                  a9dace4e1a17        7 minutes ago       407 MB
azure-vote-front                                     latest              eaf2b9c57e5e        8 minutes ago       716 MB
mycontainerregistry082.azurecr.io/azure-vote-front   v1                  eaf2b9c57e5e        8 minutes ago       716 MB
mysql                                                latest              e799c7f9ae9c        6 weeks ago         407 MB
tiangolo/uwsgi-nginx-flask                           flask               788ca94b2313        8 months ago        694 MB
```

## <a name="push-images-to-acr"></a>Übertragen von Images in ACR per Push

Übertragen Sie das Image *azure-vote-front* per Push in die Registrierung. 

Verwenden Sie das folgende Beispiel, und ersetzen Sie den ACR-loginServer-Namen durch den loginServer-Namen aus Ihrer Umgebung. Dieser Vorgang nimmt einige Minuten in Anspruch.

```bash
docker push <acrLoginServer>/azure-vote-front:v1
```

Wiederholen Sie den Vorgang für das Image *azure-vote-back*.

```bash
docker push <acrLoginServer>/azure-vote-back:v1
```

## <a name="list-images-in-acr"></a>Auflisten von Images in ACR 

Führen Sie den Befehl [az acr repository list](/cli/azure/acr/repository#list) aus, um eine Liste der Images zurückzugeben, die per Push in Ihre Azure Container Registry-Instanz übertragen wurden. Aktualisieren Sie den Befehl mit dem Namen der ACR-Instanz.

```azurecli-interactive
az acr repository list --name <acrName> --username <acrName> --password <acrPassword> --output table
```

Ausgabe:

```azurecli
Result
----------------
azure-vote-back
azure-vote-front
```

Verwenden Sie dann den Befehl [az acr repository show-tags](/cli/azure/acr/repository#show-tags), um die Tags für ein bestimmtes Image anzuzeigen.

```azurecli-interactive
az acr repository show-tags --name <acrName> --username <acrName> --password <acrPassword> --repository azure-vote-front --output table
```

Ausgabe:

```azurecli
Result
--------
v1
```

Nach Abschluss des Tutorials sind die beiden Containerimages in einer privaten Azure Container Registry-Instanz gespeichert. Diese Images werden in den nachfolgenden Tutorials aus ACR in einem Kubernetes-Cluster bereitgestellt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde eine Azure Container Registry-Instanz für die Verwendung in einem ACR-Kubernetes-Cluster vorbereitet. Die folgenden Schritte wurden ausgeführt:

> [!div class="checklist"]
> * Bereitstellen einer Azure Container Registry-Instanz
> * Kennzeichnen von Containerimages für ACR
> * Hochladen von Images in ACR

Im nächsten Tutorial erfahren Sie, wie Sie einen Kubernetes-Cluster in Azure bereitstellen.

> [!div class="nextstepaction"]
> [Bereitstellen von Kubernetes-Clustern](./container-service-tutorial-kubernetes-deploy-cluster.md)
