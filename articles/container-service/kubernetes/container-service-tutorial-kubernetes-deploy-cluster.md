---
title: "Tutorial für Azure Container Service – Bereitstellen eines Clusters | Microsoft-Dokumentation"
description: "Tutorial für Azure Container Service – Bereitstellen eines Clusters"
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
ms.date: 08/21/2017
ms.author: nepeters
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 0aae2acfbf30a77f57ddfbaabdb17f51b6938fd6
ms.openlocfilehash: 16070499b7befca26d55259d845d1dbc14110f2a
ms.contentlocale: de-de
ms.lasthandoff: 08/09/2017

---

# <a name="deploy-a-kubernetes-cluster-in-azure-container-service"></a>Bereitstellen eines Kubernetes-Clusters in Azure Container Service

Kubernetes bietet eine verteilte Plattform für containerbasierte Anwendungen. Mit Azure Container Service ist die Bereitstellung eines produktionsbereiten Kubernetes-Clusters schnell und einfach. In diesem Tutorial (Teil 3 von 7) wird ein Azure Container Service-Kubernetes-Clusters bereitgestellt. Folgende Schritte werden ausgeführt:

> [!div class="checklist"]
> * Bereitstellen eines Kubernetes-ACS-Clusters
> * Installation der Kubernetes-Befehlszeilenschnittstelle (kubectl)
> * Konfiguration von kubectl

In den nachfolgenden Tutorials wird die Azure Vote-Anwendung im Cluster bereitgestellt, skaliert und aktualisiert. Außerdem wird die Operations Management Suite für die Überwachung des Kubernetes-Clusters konfiguriert.

## <a name="before-you-begin"></a>Voraussetzungen

In vorherigen Tutorials wurde ein Containerimage erstellt und in eine Azure Container Registry-Instanz hochgeladen. Wenn Sie diese Schritte nicht ausgeführt haben und dies jetzt nachholen möchten, kehren Sie zum [Tutorial 1 – Erstellen von Containerimages](./container-service-tutorial-kubernetes-prepare-app.md) zurück.

## <a name="create-kubernetes-cluster"></a>Erstellen eines Kubernetes-Clusters

Im [vorherigen Tutorial](./container-service-tutorial-kubernetes-prepare-acr.md) wurde eine Ressourcengruppe namens *myResourceGroup* erstellt. Wenn dies noch nicht erfolgt ist, erstellen Sie diese Ressourcengruppe jetzt.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

Erstellen Sie mit dem Befehl [az acs create](/cli/azure/acs#create) einen Kubernetes-Cluster in Azure Container Service. 

Im folgenden Beispiel wird ein Cluster namens *myK8sCluster* mit einem Linux-Masterknoten und drei Linux-Agent-Knoten erstellt.

```azurecli-interactive 
az acs create --orchestrator-type=kubernetes --resource-group myResourceGroup --name=myK8SCluster --generate-ssh-keys 
```

Nach einigen Minuten ist die Ausführung des Befehls abgeschlossen, und es werden Informationen zur ACS-Bereitstellung im JSON-Format zurückgegeben.

## <a name="install-the-kubectl-cli"></a>Installieren der Befehlszeilenschnittstelle „kubectl“

Zum Herstellen der Verbindung mit dem Kubernetes-Cluster auf Ihrem Clientcomputer verwenden Sie den Kubernetes-Befehlszeilenclient [kubectl](https://kubernetes.io/docs/user-guide/kubectl/). 

Bei Verwendung von Azure Cloud Shell ist `kubectl` bereits installiert. Wenn Sie ihn lokal installieren möchten, verwenden Sie den Befehl [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli).

Bei Ausführung unter Linux oder macOS müssen Sie möglicherweise sudo verwenden. Stellen Sie unter Windows sicher, dass die Shell als Administrator ausgeführt wird.

```azurecli-interactive 
az acs kubernetes install-cli 
```

Unter Windows ist *C:\Programme (x86)\kubectl.exe* die standardmäßige Installationsdatei. Sie müssen diese Datei möglicherweise zu diesen Windows-Verzeichnis hinzufügen. 

## <a name="connect-with-kubectl"></a>Verbinden mit kubectl

Führen Sie den Befehl [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials) aus, um `kubectl` für die Verbindung mit dem Kubernetes-Cluster zu konfigurieren.

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group=myResourceGroup --name=myK8SCluster
```

Um die Verbindung mit dem Cluster zu überprüfen, führen Sie den Befehl [kubectl get nodes](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) aus.

```azurecli-interactive
kubectl get nodes
```

Ausgabe:

```bash
NAME                    STATUS                     AGE       VERSION
k8s-agent-98dc3136-0    Ready                      5m        v1.6.2
k8s-agent-98dc3136-1    Ready                      5m        v1.6.2
k8s-agent-98dc3136-2    Ready                      5m        v1.6.2
k8s-master-98dc3136-0   Ready,SchedulingDisabled   5m        v1.6.2
```

Nach Abschluss des Tutorials verfügen Sie über einen ACS-Kubernetes-Cluster, der für die Verarbeitung von Workloads bereit ist. In nachfolgenden Tutorials wird in diesem Cluster eine Anwendung mit mehreren Containern bereitgestellt, horizontal hochskaliert, aktualisiert und überwacht.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde ein Azure Container Service-Kubernetes-Clusters bereitgestellt. Die folgenden Schritte wurden ausgeführt:

> [!div class="checklist"]
> * Bereitstellen eines Kubernetes-ACS-Clusters
> * Installieren der Kubernetes-Befehlszeilenschnittstelle (kubectl)
> * Konfigurieren von kubectl

Fahren Sie mit dem nächsten Tutorial fort, um mehr über die Ausführung von Anwendungen im Cluster zu erfahren.

> [!div class="nextstepaction"]
> [Bereitstellen einer Anwendung in Kubernetes](./container-service-tutorial-kubernetes-deploy-application.md)

