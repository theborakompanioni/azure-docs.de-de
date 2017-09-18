---
title: "Tutorial für Azure Container Service – Skalieren einer Anwendung | Microsoft-Dokumentation"
description: "Tutorial für Azure Container Service – Skalieren einer Anwendung"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Container, Microservices, Kubernetes, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: aurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/14/2017
ms.author: danlep
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: d24c6777cc6922d5d0d9519e720962e1026b1096
ms.openlocfilehash: bb528c424bc71f0309439e741c30e16d0d13c7d7
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---

# <a name="scale-kubernetes-pods-and-kubernetes-infrastructure"></a>Skalieren von Kubernetes-Pods und der Kubernetes-Infrastruktur

Wenn Sie die Tutorials durchgearbeitet haben, verfügen Sie über einen funktionsfähigen Kubernetes-Cluster in Azure Container Service und haben die Azure Vote-App bereitgestellt. 

In diesem Tutorial – Teil 5 von 7 – skalieren Sie die Pods in der App horizontal hoch und testen das Feature der automatischen Skalierung von Pods. Sie erfahren auch, wie Sie die Anzahl von Azure-VM-Agent-Knoten skalieren, um die Clusterkapazität für das Hosten von Workloads zu ändern. Folgende Aufgaben wurden ausgeführt:

> [!div class="checklist"]
> * Manuelles Skalieren von Kubernetes-Pods
> * Konfigurieren der automatischen Skalierung von Pods, die das Front-End einer App ausführen
> * Skalieren der Kubernetes-Azure-Agent-Knoten

In den nachfolgenden Tutorials wird die Azure Voting-Anwendung aktualisiert. Zudem wird die Operations Management Suite für die Überwachung des Kubernetes-Clusters konfiguriert.

## <a name="before-you-begin"></a>Voraussetzungen

In vorherigen Tutorials wurde eine Anwendung in ein Containerimage gepackt, dieses Image wurde in Azure Container Registry hochgeladen, und es wurde ein Kubernetes-Cluster erstellt. Anschließend wurde die Anwendung im Kubernetes-Cluster ausgeführt. 

Wenn Sie diese Schritte nicht ausgeführt haben und dies jetzt nachholen möchten, kehren Sie zum [Tutorial 1 – Erstellen von Containerimages](./container-service-tutorial-kubernetes-prepare-app.md) zurück. 

## <a name="manually-scale-pods"></a>Manuelles Skalieren von Pods

Bisher wurden die Front-End-Instanz und die Redis-Instanz der Azure Voting-App mit jeweils einem Replikat bereitgestellt. Führen Sie zum Überprüfen den Befehl [kubectl get](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) aus.

```azurecli-interactive
kubectl get pods
```

Ausgabe:

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

Ändern Sie die Anzahl von Pods in der `azure-vote-front`-Bereitstellung manuell mithilfe des Befehls [kubectl scale](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#scale). In diesem Beispiel wird die Anzahl auf 5 erhöht.

```azurecli-interactive
kubectl scale --replicas=5 deployment/azure-vote-front
```

Führen Sie [kubectl get pods](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) aus, um zu überprüfen, ob Kubernetes die Pods erstellt. Nach ca. einer Minute werden die zusätzlichen Pods ausgeführt:

```azurecli-interactive
kubectl get pods
```

Ausgabe:

```bash
NAME                                READY     STATUS    RESTARTS   AGE
azure-vote-back-2606967446-nmpcf    1/1       Running   0          15m
azure-vote-front-3309479140-2hfh0   1/1       Running   0          3m
azure-vote-front-3309479140-bzt05   1/1       Running   0          3m
azure-vote-front-3309479140-fvcvm   1/1       Running   0          3m
azure-vote-front-3309479140-hrbf2   1/1       Running   0          15m
azure-vote-front-3309479140-qphz8   1/1       Running   0          3m
```

## <a name="autoscale-pods"></a>Automatisches Skalieren von Pods

Kubernetes unterstützt [die automatische horizontale Skalierung von Pods](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/), um die Anzahl von Pods in einer Bereitstellung je nach CPU-Nutzung und anderen ausgewählten Metriken anzupassen. 

Um die automatische Skalierungsfunktion zu verwenden, müssen CPU-Anforderungen und -Grenzwerte Ihrer Pods definiert sein. In der `azure-vote-front`-Bereitstellung fordert der Front-End-Container 0,25 CPUs an, und es gilt ein Grenzwert von 0,5 CPUs. Die Einstellungen sehen folgendermaßen aus:

```YAML
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

Das folgende Beispiel verwendet den Befehl [kubectl autoscale](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#autoscale), um die Anzahl von Pods in der `azure-vote-front`-Bereitstellung automatisch zu skalieren. Wenn die CPU-Nutzung 50 % übersteigt, erhöht die automatische Skalierungsfunktion die Anzahl von Pods auf den Maximalwert 10.


```azurecli-interactive
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

Führen Sie den folgenden Befehl aus, um den Status der automatischen Skalierungsfunktion anzuzeigen:

```azurecli-interactive
kubectl get hpa
```

Ausgabe:

```bash
NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

Nach einigen Minuten mit minimaler Last in der Azure Voting-App sinkt die Anzahl von Podreplikaten automatisch auf 3.

## <a name="scale-the-agents"></a>Skalieren der Agents

Wenn Sie Ihren Kubernetes-Cluster mithilfe der Standardbefehle im vorherigen Tutorial erstellt haben, verfügt er über drei Agent-Knoten. Sie können die Anzahl von Agents manuell anpassen, wenn Sie größere oder kleinere Containerworkloads in Ihrem Cluster planen. Verwenden Sie den Befehl [az acs scale](/cli/azure/acs#scale), und geben Sie die Anzahl von Agents mit dem `--new-agent-count`-Parameter an.

Das folgende Beispiel erhöht die Anzahl von Agent-Knoten im Kubernetes-Cluster namens *myK8sCluster* auf vier. Diese Ausführung dieses Befehls dauert einige Minuten.

```azurecli-interactive
az acs scale --resource-group=myResourceGroup --name=myK8SCluster --new-agent-count 4
```

Die Befehlsausgabe zeigt die Anzahl von Agent-Knoten im Wert von `agentPoolProfiles:count` an:

```azurecli
{
  "agentPoolProfiles": [
    {
      "count": 4,
      "dnsPrefix": "myK8SCluster-myK8SCluster-e44f25-k8s-agents",
      "fqdn": "",
      "name": "agentpools",
      "vmSize": "Standard_D2_v2"
    }
  ],
...

```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie verschiedene Skalierungsfunktionen in Ihrem Kubernetes-Cluster verwendet. Sie haben folgende Aufgaben ausgeführt:

> [!div class="checklist"]
> * Manuelles Skalieren von Kubernetes-Pods
> * Konfigurieren der automatischen Skalierung von Pods, die das Front-End einer App ausführen
> * Skalieren der Kubernetes-Azure-Agent-Knoten

Fahren Sie mit dem nächsten Tutorial fort, um mehr über die Aktualisierung von Anwendungen im Cluster zu erfahren.

> [!div class="nextstepaction"]
> [Aktualisieren einer Anwendung in Kubernetes](./container-service-tutorial-kubernetes-app-update.md)


