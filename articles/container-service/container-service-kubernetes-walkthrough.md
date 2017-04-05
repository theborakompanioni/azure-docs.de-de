---
title: "Schnellstart für Kubernetes-Cluster in Azure | Microsoft-Dokumentation"
description: "Bereitstellung und erste Schritte für ein Kubernetes-Cluster in Azure Container Service"
services: container-service
documentationcenter: 
author: anhowe
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
ms.date: 03/01/2017
ms.author: anhowe
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: e4f47341554e2de514c8be2f5c85983d09bbb760
ms.lasthandoff: 04/03/2017

---

# <a name="get-started-with-a-kubernetes-cluster-in-container-service"></a>Erste Schritte mit einem Kubernetes-Cluster in Container Service


Mit den Anleitungen in diesem Artikel wird veranschaulicht, wie Sie die Azure CLI 2.0-Befehle verwenden, um einen Kubernetes-Cluster zu erstellen. Anschließend können Sie das Befehlszeilentool `kubectl` verwenden, um mit der Nutzung der Container im Cluster zu beginnen.

Die folgende Abbildung zeigt die Architektur eines Container Service-Clusters mit einem Master und zwei Agents. Der Master dient als Kubernetes-REST-API. Die Agentknoten werden in einer Azure-Verfügbarkeitsgruppe gruppiert und dienen zum Ausführen Ihrer Container. Alle VMs befinden sich in demselben privaten virtuellen Netzwerk, und untereinander besteht Vollzugriff.

![Abbildung: Kubernetes-Cluster in Azure](media/container-service-kubernetes-walkthrough/kubernetes.png)

## <a name="prerequisites"></a>Voraussetzungen
Bei dieser exemplarischen Vorgehensweise wird davon ausgegangen, dass Sie [Azure CLI 2.0](/cli/azure/install-az-cli2) installiert und eingerichtet haben. Außerdem muss unter `~/.ssh/id_rsa.pub` ein öffentlicher SSH-RSA-Schlüssel vorhanden sein. Falls der Schlüssel nicht vorhanden ist, helfen Ihnen die Schritte für [OS X und Linux](../virtual-machines/linux/mac-create-ssh-keys.md) bzw. [Windows](../virtual-machines/linux/ssh-from-windows.md) weiter.






## <a name="create-your-kubernetes-cluster"></a>Erstellen Ihres Kubernetes-Clusters

Hier sind kurze Shellbefehle angegeben, bei denen Azure CLI 2.0 zum Erstellen des Clusters verwendet wird. Weitere Informationen finden Sie unter [Erstellen eines Azure Container Service-Clusters mit Azure CLI 2.0](container-service-create-acs-cluster-cli.md).

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Zum Erstellen Ihres Clusters müssen Sie zuerst eine Ressourcengruppe an einem bestimmten geeigneten Standort erstellen. Führen Sie Befehle aus, die den folgenden Befehlen ähneln:

```console
RESOURCE_GROUP=my-resource-group
LOCATION=westus
az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="create-a-cluster"></a>Erstellen eines Clusters
Nachdem Sie eine Ressourcengruppe erstellt haben, können Sie darin einen Cluster erstellen:

```console
DNS_PREFIX=some-unique-value
CLUSTER_NAME=any-acs-cluster-name
az acs create --orchestrator-type=kubernetes --resource-group $RESOURCE_GROUP --name=$CLUSTER_NAME --dns-prefix=$DNS_PREFIX
```

> [!NOTE]
> Während der Bereitstellung wird `~/.ssh/id_rsa.pub` per CLI auf die Linux-VMs hochgeladen.
>

Nachdem dieser Befehl abgeschlossen ist, sollten Sie über einen funktionsfähigen Kubernetes-Cluster verfügen.

### <a name="connect-to-the-cluster"></a>Verbinden mit dem Cluster

Unten sind Azure-CLI-Befehle zum Herstellen der Verbindung mit dem Kubernetes-Cluster von Ihrem Clientcomputer per `kubectl` (Kubernetes-Befehlszeilenclient) angegeben. Weitere Informationen finden Sie unter [Verbinden mit einem Azure Container Service-Cluster](container-service-connect.md).

Falls Sie die Installation von `kubectl` noch nicht durchgeführt haben, können Sie wie folgt vorgehen:

```console
az acs kubernetes install-cli
```

Führen Sie nach der Installation von `kubectl` den folgenden Befehl aus, um die Masterkonfiguration des Kubernetes-Clusters in die Datei „~/.kube/config“ herunterzuladen:

```console
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$CLUSTER_NAME
```

An diesem Punkt sollte alles bereit sein, um von Ihrem Computer auf den Cluster zuzugreifen. Probieren Sie Folgendes aus:
```console
kubectl get nodes
```

Vergewissern Sie sich, dass eine Liste mit den Computern in Ihrem Cluster angezeigt wird.

## <a name="create-your-first-kubernetes-service"></a>Erstellen des ersten Kubernetes-Diensts

Nach Abschluss dieser exemplarischen Vorgehensweise besitzen Sie folgende Kenntnisse:
 * Bereitstellen und globales Verfügbarmachen einer Docker-Anwendung
 * Verwenden von `kubectl exec` zum Ausführen von Befehlen in einem Container 
 * Zugreifen auf das Kubernetes-Dashboard

### <a name="start-a-simple-container"></a>Starten eines einfachen Containers
Sie können einen einfachen Container (in diesem Fall der Nginx-Webserver) wie folgt ausführen:

```console
kubectl run nginx --image nginx
```

Mit diesem Befehl wird der Nginx-Docker-Container in einem Pod auf einem der Knoten gestartet.

Führen Sie Folgendes aus, um den ausgeführten Container anzuzeigen:

```console
kubectl get pods
```

### <a name="expose-the-service-to-the-world"></a>Globales Verfügbarmachen des Diensts
Erstellen Sie einen Kubernetes-`Service` vom Typ `LoadBalancer`, um den Dienst global verfügbar zu machen:

```console
kubectl expose deployments nginx --port=80 --type=LoadBalancer
```

Von Kubernetes wird dann eine Azure-Lastenausgleichsregel mit einer öffentlichen IP-Adresse erstellt. Die Weitergabe der Änderung an den Lastenausgleich dauert einige Minuten. Weitere Informationen finden Sie unter [Lastenausgleich für Container in einem Kubernetes-Cluster in Azure Container Service](container-service-kubernetes-load-balancing.md).

Führen Sie den folgenden Befehl aus, um verfolgen zu können, wie der Dienst vom Status `pending` zur Anzeige einer externen IP-Adresse wechselt:

```console
watch 'kubectl get svc'
```

  ![Abbildung: Verfolgen des Übergangs von „pending“ zur externen IP-Adresse](media/container-service-kubernetes-walkthrough/kubernetes-nginx3.png)

Wenn die externe IP-Adresse angezeigt wird, können Sie im Browser dorthin navigieren:

  ![Abbildung: Navigieren zu Nginx](media/container-service-kubernetes-walkthrough/kubernetes-nginx4.png)  


### <a name="browse-the-kubernetes-ui"></a>Durchsuchen der Kubernetes-Benutzeroberfläche
Zum Anzeigen der Kubernetes-Weboberfläche können Sie Folgendes verwenden:

```console
kubectl proxy
```
Hiermit wird ein einfacher authentifizierter Proxy auf „localhost“ ausgeführt, den Sie zum Anzeigen der Kubernetes-Webbenutzeroberfläche unter [http://localhost:8001/ui](http://localhost:8001/ui) verwenden können. Weitere Informationen finden Sie unter [Verwenden der Webbenutzeroberfläche von Kubernetes mit Azure Container Service](container-service-kubernetes-ui.md).

![Abbildung des Kubernetes-Dashboards](media/container-service-kubernetes-walkthrough/kubernetes-dashboard.png)

### <a name="remote-sessions-inside-your-containers"></a>Remotesitzungen in Ihren Containern
Mit Kubernetes können Sie Befehle in einem Docker-Remotecontainer Ihres Clusters ausführen.

```console
# Get the name of your nginx pods
kubectl get pods
```

Mit dem Pod-Namen können Sie einen Remotebefehl auf dem Pod ausführen.  Beispiel:

```console
kubectl exec <pod name> date
```

Außerdem erhalten Sie mit den `-it`-Flags eine vollständig interaktive Sitzung:

```console
kubectl exec <pod name> -it bash
```

![Remotesitzung in einem Container](media/container-service-kubernetes-walkthrough/kubernetes-remote.png)



## <a name="next-steps"></a>Nächste Schritte

Unter den folgenden Ressourcen finden Sie Informationen dazu, welche weiteren Möglichkeiten Sie mit einem Kubernetes-Cluster haben:

* [Kubernetes Bootcamp](https://katacoda.com/embed/kubernetes-bootcamp/1/): Es wird veranschaulicht, wie Sie Anwendungen in Containern bereitstellen, skalieren, aktualisieren und debuggen.
* [Kubernetes User Guide](http://kubernetes.io/docs/user-guide/) (Kubernetes-Benutzerhandbuch): Enthält Informationen zum Ausführen von Programmen in einem vorhandenen Kubernetes-Cluster.
* [Kubernetes Examples](https://github.com/kubernetes/kubernetes/tree/master/examples) (Kubernetes-Beispiele): Enthält Beispiele für die Ausführung von echten Anwendungen mit Kubernetes.

