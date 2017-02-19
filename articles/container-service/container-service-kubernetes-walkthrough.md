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
ms.date: 11/15/2016
ms.author: anhowe
translationtype: Human Translation
ms.sourcegitcommit: 7ed8fb75f057d5a7cfde5436e72e8fec52d07156
ms.openlocfilehash: f3b2fc301bf7083f192c0ec872c4e032472eef97


---

# <a name="microsoft-azure-container-service-engine---kubernetes-walkthrough"></a>Microsoft Azure Container Service-Modul – Exemplarische Vorgehensweise für Kubernetes

## <a name="prerequisites"></a>Voraussetzungen
In dieser exemplarischen Vorgehensweise wird davon ausgegangen, dass Sie das [Befehlszeilentool „azure-cli“](https://github.com/azure/azure-cli#installation) installiert und unter `~/.ssh/id_rsa.pub` einen [öffentlichen SSH-Schlüssel](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) erstellt haben.

## <a name="overview"></a>Übersicht

Mit der folgenden Anleitung können Sie einen Kubernetes-Cluster mit einem Master und zwei Workerknoten erstellen.
Der Master dient als Kubernetes-REST-API.  Die Workerknoten werden in einer Azure-Verfügbarkeitsgruppe gruppiert und dienen zum Ausführen Ihrer Container. Alle VMs befinden sich in demselben privaten VNET, und untereinander besteht Vollzugriff.

> [!NOTE]
> Die Kubernetes-Unterstützung in Azure Container Service befindet sich derzeit in der Vorschauphase.
>

Die folgende Abbildung zeigt die Architektur eines Container Service-Clusters mit einem Master und zwei Agents:

![Abbildung: Kubernetes-Cluster in Azure](media/container-service-kubernetes-walkthrough/kubernetes.png)

## <a name="creating-your-kubernetes-cluster"></a>Erstellen Ihres Kubernetes-Clusters

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Zum Erstellen Ihres Clusters müssen Sie zuerst eine Ressourcengruppe an einem bestimmten geeigneten Standort erstellen:
```console
RESOURCE_GROUP=my-resource-group
LOCATION=westus
az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="create-a-cluster"></a>Erstellen eines Clusters
Nachdem Sie eine Ressourcengruppe erstellt haben, können Sie darin wie folgt einen Cluster erstellen:
```console
DNS_PREFIX=some-unique-value
SERVICE_NAME=any-acs-service-name
az acs create --orchestrator-type=kubernetes --resource-group $RESOURCE_GROUP --name=$SERVICE_NAME --dns-prefix=$DNS_PREFIX
```

> [!NOTE]
> Mit azure-cli wird `~/.ssh/id_rsa.pub` auf die Linux-VMs hochgeladen.
>

Nachdem dieser Befehl abgeschlossen ist, sollten Sie über einen funktionsfähigen Kubernetes-Cluster verfügen.

### <a name="configure-kubectl"></a>Konfigurieren von kubectl
`kubectl` ist der Kubernetes-Befehlszeilenclient.  Falls Sie die Installation noch nicht durchgeführt haben, können Sie wie folgt vorgehen:

```console
az acs kubernetes install-cli
```

Nach der Installation von `kubectl` wird durch die Ausführung des unten angegebenen Befehls die Masterkonfiguration des Kubernetes-Clusters in die Datei „~/.kube/config“ heruntergeladen.
```console
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$SERVICE_NAME
```

An diesem Punkt sollte alles bereit sein, um von Ihrem Computer auf den Cluster zuzugreifen. Probieren Sie Folgendes aus:
```console
kubectl get nodes
```

Vergewissern Sie sich auch, dass Sie die Computer in Ihrem Cluster sehen.

## <a name="create-your-first-kubernetes-service"></a>Erstellen des ersten Kubernetes-Diensts

Nach Abschluss dieser exemplarischen Vorgehensweise besitzen Sie folgende Kenntnisse:
 * Bereitstellen und globales Verfügbarmachen einer Docker-Anwendung
 * Verwenden von `kubectl exec` zum Ausführen von Befehlen in einem Container 
 * Zugreifen auf das Kubernetes-Dashboard

### <a name="start-a-simple-container"></a>Starten eines einfachen Containers
Sie können einen einfachen Container (in diesem Fall der Webserver `nginx`) wie folgt ausführen:

```console
kubectl run nginx --image nginx
```

Mit diesem Befehl wird der nginx-Docker-Container in einem Pod auf einem der Knoten gestartet.

Sie können Folgendes ausführen:
```console
kubectl get pods
```

Hiermit wird der ausgeführte Container angezeigt.

### <a name="expose-the-service-to-the-world"></a>Globales Verfügbarmachen des Diensts
Um den Dienst global verfügbar zu machen,  erstellen Sie einen Kubernetes-Dienst (`Service`) vom Typ `LoadBalancer`:

```console
kubectl expose deployments nginx --port=80 --type=LoadBalancer
```

Dies bewirkt, dass Kubernetes einen Azure Load Balancer mit einer öffentlichen IP erstellt. Die Weitergabe der Änderung an den Lastenausgleich dauert ca. 2 bis 3 Minuten.

Geben Sie Folgendes ein, um zu verfolgen, wie sich der Dienst von „pending“ (Ausstehend) in eine externe IP ändert:
```console
watch 'kubectl get svc'
```

  ![Abbildung: Verfolgen des Übergangs von „pending“ zur externen IP](media/container-service-kubernetes-walkthrough/kubernetes-nginx3.png)

Wenn die externe IP angezeigt wird, können Sie im Browser dorthin navigieren:

  ![Abbildung: Navigieren zu nginx](media/container-service-kubernetes-walkthrough/kubernetes-nginx4.png)  


### <a name="browse-the-kubernetes-ui"></a>Durchsuchen der Kubernetes-Benutzeroberfläche
Zum Anzeigen der Kubernetes-Weboberfläche können Sie Folgendes verwenden:

```console
kubectl proxy
```
Hiermit wird ein einfacher authentifizierter Proxy auf „localhost“ ausgeführt, den Sie zum Anzeigen der [Kubernetes-Benutzeroberfläche](http://localhost:8001/ui) verwenden können.

![Abbildung des Kubernetes-Dashboards](media/container-service-kubernetes-walkthrough/kubernetes-dashboard.png)

### <a name="remote-sessions-inside-your-containers"></a>Remotesitzungen in Ihren Containern
Mit Kubernetes können Sie Befehle in einem Docker-Remotecontainer Ihres Clusters ausführen.

```console
# Get the name of your nginx pod
kubectl get pods
```

Mit dem Pod-Namen können Sie einen Remotebefehl auf dem Pod ausführen.  Beispiel:
```console
kubectl exec nginx-701339712-retbj date
```

Außerdem erhalten Sie mit den `-it`-Flags eine vollständig interaktive Sitzung:

```console
kubectl exec nginx-701339712-retbj -it bash
```

![Abbildung: Curl für podIP](media/container-service-kubernetes-walkthrough/kubernetes-remote.png)


## <a name="details"></a>Details
### <a name="installing-the-kubectl-configuration-file"></a>Installieren der kubectl-Konfigurationsdatei
Beim Ausführen von `az acs kubernetes get-credentials` wurde die kube-Konfigurationsdatei für den Remotezugriff im Basisverzeichnis „~/.kube/config“ gespeichert.

Falls Sie einen direkten Download durchführen müssen, können Sie `ssh` unter Linux oder OS X und `Putty` unter Windows verwenden:

#### <a name="windows"></a>Windows
Hierfür wird pscp von [Putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) verwendet.  Stellen Sie sicher, dass Sie Ihr Zertifikat über [pageant](https://github.com/Azure/acs-engine/blob/master/docs/ssh.md#key-management-and-agent-forwarding-with-windows-pageant) verfügbar gemacht haben:
  ```
  # MASTERFQDN is obtained in step1
  pscp azureuser@MASTERFQDN:.kube/config .
  SET KUBECONFIG=%CD%\config
  kubectl get nodes
  ```

#### <a name="os-x-or-linux"></a>OS X oder Linux:
  ```
  # MASTERFQDN is obtained in step1
  scp azureuser@MASTERFQDN:.kube/config .
  export KUBECONFIG=`pwd`/config
  kubectl get nodes
  ```
## <a name="learning-more"></a>Weitere Informationen

### <a name="azure-container-service"></a>Azure Container Service

1. [Dokumentation zu Azure Container Service](https://azure.microsoft.com/en-us/documentation/services/container-service/)
2. [Azure Container Service-Open Source-Modul](https://github.com/azure/acs-engine)

### <a name="kubernetes-community-documentation"></a>Dokumentation zur Kubernetes-Community

1. [Kubernetes Bootcamp](https://katacoda.com/embed/kubernetes-bootcamp/1/): Es wird veranschaulicht, wie Sie Anwendungen in Containern bereitstellen, skalieren, aktualisieren und debuggen.
2. [Kubernetes User Guide](http://kubernetes.io/docs/user-guide/) (Kubernetes-Benutzerhandbuch): Enthält Informationen zum Ausführen von Programmen in einem vorhandenen Kubernetes-Cluster.
3. [Kubernetes Examples](https://github.com/kubernetes/kubernetes/tree/master/examples) (Kubernetes-Beispiele): Enthält Beispiele für die Ausführung von echten Anwendungen mit Kubernetes.



<!--HONumber=Jan17_HO4-->


