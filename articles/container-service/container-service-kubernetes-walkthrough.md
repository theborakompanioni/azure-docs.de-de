---
title: "Schnellstart – Azure Kubernetes-Cluster für Linux | Microsoft-Dokumentation"
description: "Hier lernen Sie schnell, einen Kubernetes-Cluster für Linux-Container in Azure Container Service mithilfe der Azure-Befehlszeilenschnittstelle zu erstellen."
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 8da267e8-2aeb-4c24-9a7a-65bdca3a82d6
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 26c07d30f9166e0e52cb396cdd0576530939e442
ms.openlocfilehash: 3be2079d205d6bfd4c796e5f6abcd7ac5fe595a2
ms.contentlocale: de-de
ms.lasthandoff: 07/19/2017

---

# <a name="deploy-kubernetes-cluster-for-linux-containers"></a>Bereitstellen eines Kubernetes-Clusters für Linux-Container

Die Azure CLI dient zum Erstellen und Verwalten von Azure-Ressourcen über die Befehlszeile oder mit Skripts. In dieser Anleitung wird die Bereitstellung eines [Kubernetes](https://kubernetes.io/docs/home/)-Clusters in [Azure Container Service](container-service-intro.md) über die Azure-Befehlszeilenschnittstelle beschrieben. Nach dem Bereitstellen des Clusters verbinden Sie diesen mit dem Kubernetes-Befehlszeilentool `kubectl` und stellen dann den ersten Linux-Container bereit.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Schnellstart die Azure CLI-Version 2.0.4 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#create) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist eine logische Gruppe, in der Azure-Ressourcen bereitgestellt und verwaltet werden. 

Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-kubernetes-cluster"></a>Erstellen eines Kubernetes-Clusters
Erstellen Sie mit dem Befehl [az acs create](/cli/azure/acs#create) einen Kubernetes-Cluster in Azure Container Service. 

Im folgenden Beispiel wird ein Cluster mit dem Namen *myK8sCluster* mit einem Linux-Masterknoten und zwei Linux-Agentknoten erstellt. In diesem Beispiel werden SSH-Schlüssel erstellt, wenn diese in den Standardspeicherorten noch nicht vorhanden sind. Um einen bestimmten Satz von Schlüsseln zu verwenden, nutzen Sie die Option `--ssh-key-value`. Ändern Sie den Namen des Clusters in einen für Ihre Umgebung geeigneten Namen. 



```azurecli-interactive 
az acs create --orchestrator-type=kubernetes \
    --resource-group myResourceGroup \
    --name=myK8sCluster \
    --generate-ssh-keys 
```

Nach einigen Minuten ist der Befehl abgeschlossen, und es werden Informationen zu der Bereitstellung angezeigt.

## <a name="install-kubectl"></a>Installieren von kubectl

Zum Herstellen der Verbindung mit dem Kubernetes-Cluster auf Ihrem Clientcomputer verwenden Sie [`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/) (den Kubernetes-Befehlszeilenclient). 

Bei Verwendung von Azure Cloud Shell ist `kubectl` bereits installiert. Wenn Sie ihn lokal installieren möchten, verwenden Sie den Befehl [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli).

Im folgenden Azure CLI-Beispiel wird `kubectl` in Ihrem System installiert. Wenn Sie die Azure CLI unter macOS oder Linux ausführen, müssen Sie den Befehl möglicherweise mit `sudo` ausführen.

```azurecli-interactive 
az acs kubernetes install-cli 
```

## <a name="connect-with-kubectl"></a>Verbinden mit kubectl

Führen Sie den Befehl [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials) aus, um `kubectl` für die Verbindung mit dem Kubernetes-Cluster zu konfigurieren. Im folgenden Beispiel wird die Clusterkonfiguration für den Kubernetes-Cluster heruntergeladen.

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group=myResourceGroup --name=myK8sCluster
```

Um die Verbindung zwischen dem Cluster und Ihrem Computer zu überprüfen, führen Sie folgenden Befehl aus:

```azurecli-interactive
kubectl get nodes
```

`kubectl` listet den Master- und die Agentknoten auf.

```azurecli-interactive
NAME                    STATUS                     AGE       VERSION
k8s-agent-98dc3136-0    Ready                      5m        v1.5.3
k8s-agent-98dc3136-1    Ready                      5m        v1.5.3
k8s-master-98dc3136-0   Ready,SchedulingDisabled   5m        v1.5.3

```


## <a name="deploy-an-nginx-container"></a>Bereitstellen eines NGINX-Containers

Sie können einen Docker-Container in einem Kubernetes-*Pod* ausführen, der einen oder mehrere Container enthält. 

Mit dem folgenden Befehl wird der NGINX-Docker-Container in einem Kubernetes-Pod auf einem der Knoten gestartet. In diesem Fall führt der Container den von einem Image in [Docker Hub](https://hub.docker.com/_/nginx/) abgerufenen NGINX-Webserver aus.

```azurecli-interactive
kubectl run nginx --image nginx
```
Überprüfen Sie mit dem folgenden Befehl, ob der Container ausgeführt wird:

```azurecli-interactive
kubectl get pods
```

## <a name="view-the-nginx-welcome-page"></a>Anzeigen der NGINX-Willkommensseite
Geben Sie den folgenden Befehl ein, um den NGINX-Server mit einer öffentlichen IP-Adresse global verfügbar zu machen:

```azurecli-interactive
kubectl expose deployments nginx --port=80 --type=LoadBalancer
```

Dieser Befehl bewirkt, dass Kubernetes einen Dienst und eine [Azure Load Balancer-Regel](container-service-kubernetes-load-balancing.md) mit einer öffentlichen IP-Adresse für den Dienst erstellt. 

Führen Sie den folgenden Befehl aus, um den Status des Diensts anzuzeigen.

```azurecli-interactive
kubectl get svc
```

Zunächst wird die IP-Adresse als `pending` angezeigt. Nach ein paar Minuten ist die externe IP-Adresse des Diensts festgelegt:
  
```azurecli-interactive
NAME         CLUSTER-IP     EXTERNAL-IP     PORT(S)        AGE       
kubernetes   10.0.0.1       <none>          443/TCP        21h       
nginx        10.0.111.25    52.179.3.96     80/TCP         22m
```

Sie können die NGINX-Standardwillkommensseite unter der externen IP-Adresse in einem Webbrowser Ihrer Wahl anzeigen:

![Abbildung: Navigieren zu Nginx](media/container-service-kubernetes-walkthrough/kubernetes-nginx4.png)  


## <a name="delete-cluster"></a>Löschen von Clustern
Wenn der Cluster nicht mehr benötigt wird, können Sie mit dem Befehl [az group delete](/cli/azure/group#delete) die Ressourcengruppe, den Containerdienst und alle zugehörigen Ressourcen entfernen.

```azurecli-interactive 
az group delete --name myResourceGroup
```


## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie einen Kubernetes-Cluster bereitgestellt, mit `kubectl` verbunden und einen Pod mit einem NGINX-Container bereitgestellt. Weitere Informationen zu Azure Container Service erhalten Sie, wenn Sie mit dem Tutorial zum Kubernetes-Cluster fortfahren.

> [!div class="nextstepaction"]
> [Verwalten eines ACS Kubernetes-Clusters](./container-service-tutorial-kubernetes-prepare-app.md)

