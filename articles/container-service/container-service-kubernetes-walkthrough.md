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
ms.date: 04/05/2017
ms.author: anhowe
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 538f282b28e5f43f43bf6ef28af20a4d8daea369
ms.openlocfilehash: 5c529ae41b42d276d37e6103305e33ed04694e18
ms.lasthandoff: 04/07/2017

---

# <a name="get-started-with-a-kubernetes-cluster-in-container-service"></a>Erste Schritte mit einem Kubernetes-Cluster in Container Service


In dieser exemplarischen Vorgehensweise wird veranschaulicht, wie Sie die Befehle von Azure CLI 2.0 verwenden, um in Azure Container Service einen Kubernetes-Cluster zu erstellen. Anschließend können Sie das Befehlszeilentool `kubectl` verwenden, um mit der Nutzung der Container im Cluster zu beginnen.

Die folgende Abbildung zeigt die Architektur eines Container Service-Clusters mit einem Master und zwei Agents. Der Master dient als Kubernetes-REST-API. Die Agentknoten werden in einer Azure-Verfügbarkeitsgruppe gruppiert und dienen zum Ausführen Ihrer Container. Alle VMs befinden sich in demselben privaten virtuellen Netzwerk, und untereinander besteht Vollzugriff.

![Abbildung: Kubernetes-Cluster in Azure](media/container-service-kubernetes-walkthrough/kubernetes.png)

## <a name="prerequisites"></a>Voraussetzungen
Bei dieser exemplarischen Vorgehensweise wird davon ausgegangen, dass Sie [Azure CLI 2.0](/cli/azure/install-az-cli2) installiert und eingerichtet haben. 

In den Befehlsbeispielen wird vorausgesetzt, dass Sie die Azure-CLI in einer Bash-Shell ausführen, die unter Linux und Mac OS verwendet wird. Bei Ausführung der Azure-CLI auf einem Windows-Client können einige Skriptelemente und die Dateisyntax je nach Befehlsshell ggf. leicht abweichen. 

## <a name="create-your-kubernetes-cluster"></a>Erstellen Ihres Kubernetes-Clusters

Hier sind kurze Shellbefehle angegeben, in denen Azure CLI 2.0 zum Erstellen des Clusters verwendet wird. 

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Zum Erstellen Ihres Clusters müssen Sie zuerst eine Ressourcengruppe an einem bestimmten geeigneten Standort erstellen. Führen Sie Befehle aus, die den folgenden Befehlen ähneln:

```azurecli
RESOURCE_GROUP=my-resource-group
LOCATION=westus
az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="create-a-cluster"></a>Erstellen eines Clusters
Nachdem Sie eine Ressourcengruppe erstellt haben, können Sie darin einen Cluster erstellen. Im folgenden Beispiel wird die Option `--generate-ssh-keys` verwendet, mit der die benötigten Dateien mit den öffentlichen und privaten SSH-Schlüsseln für die Bereitstellung generiert werden, falls sie im Standardverzeichnis `~/.ssh/` nicht bereits vorhanden sind. 

Außerdem wird mit diesem Befehl automatisch der [Azure Active Directory-Dienstprinzipal](container-service-kubernetes-service-principal.md) generiert, der von einem Kubernetes-Cluster in Azure verwendet wird.

```azurecli
DNS_PREFIX=some-unique-value
CLUSTER_NAME=any-acs-cluster-name
az acs create --orchestrator-type=kubernetes --resource-group $RESOURCE_GROUP --name=$CLUSTER_NAME --dns-prefix=$DNS_PREFIX --generate-ssh-keys
```


Nach einigen Minuten ist der Befehl abgeschlossen, und Sie sollten über einen funktionierenden Kubernetes-Cluster verfügen.

### <a name="connect-to-the-cluster"></a>Verbinden mit dem Cluster

Zum Herstellen der Verbindung mit dem Kubernetes-Cluster auf Ihrem Clientcomputer verwenden Sie [`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/), also den Kubernetes-Befehlszeilenclient. 

Falls Sie die Installation von `kubectl` noch nicht durchgeführt haben, können Sie wie folgt vorgehen:

```azurecli
sudo az acs kubernetes install-cli
```
> [!TIP]
> Standardmäßig wird mit diesem Befehl die `kubectl`-Binärdatei auf einem Linux- oder Mac OS-System unter `/usr/local/bin/kubectl` und auf einem Windows-System unter `C:\Program Files (x86)\kubectl.exe` installiert. Um einen anderen Installationspfad anzugeben, können Sie den Parameter `--install-location` verwenden.
>

Stellen Sie nach der Installation von `kubectl` sicher, dass das dazugehörige Verzeichnis in Ihrem Systempfad enthalten ist, oder fügen Sie es dem Pfad hinzu. 


Führen Sie anschließend den folgenden Befehl aus, um die Masterkonfiguration des Kubernetes-Clusters in die Datei `~/.kube/config` herunterzuladen:

```azurecli
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$CLUSTER_NAME
```

Weitere Optionen für die Installation und Konfiguration von `kubectl` finden Sie unter [Verbinden mit einem Azure Container Service-Cluster](container-service-connect.md).

An diesem Punkt sollte alles bereit sein, um von Ihrem Computer auf den Cluster zuzugreifen. Probieren Sie Folgendes aus:

```bash
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

```bash
kubectl run nginx --image nginx
```

Mit diesem Befehl wird der Nginx-Docker-Container in einem Pod auf einem der Knoten gestartet.

Führen Sie Folgendes aus, um den ausgeführten Container anzuzeigen:

```bash
kubectl get pods
```

### <a name="expose-the-service-to-the-world"></a>Globales Verfügbarmachen des Diensts
Erstellen Sie einen Kubernetes-`Service` vom Typ `LoadBalancer`, um den Dienst global verfügbar zu machen:

```bash
kubectl expose deployments nginx --port=80 --type=LoadBalancer
```

Dieser Befehl bewirkt, dass von Kubernetes eine Azure-Lastenausgleichsregel mit einer öffentlichen IP-Adresse erstellt wird. Die Weitergabe der Änderung an den Lastenausgleich dauert einige Minuten. Weitere Informationen finden Sie unter [Lastenausgleich für Container in einem Kubernetes-Cluster in Azure Container Service](container-service-kubernetes-load-balancing.md).

Führen Sie den folgenden Befehl aus, um verfolgen zu können, wie der Dienst vom Status `pending` zur Anzeige einer externen IP-Adresse wechselt:

```bash
watch 'kubectl get svc'
```

  ![Abbildung: Verfolgen des Übergangs von „pending“ zur externen IP-Adresse](media/container-service-kubernetes-walkthrough/kubernetes-nginx3.png)

Wenn die externe IP-Adresse angezeigt wird, können Sie im Browser dorthin navigieren:

  ![Abbildung: Navigieren zu Nginx](media/container-service-kubernetes-walkthrough/kubernetes-nginx4.png)  


### <a name="browse-the-kubernetes-ui"></a>Durchsuchen der Kubernetes-Benutzeroberfläche
Zum Anzeigen der Kubernetes-Weboberfläche können Sie Folgendes verwenden:

```bash
kubectl proxy
```
Mit diesem Befehl wird ein einfacher authentifizierter Proxy auf „localhost“ ausgeführt, den Sie zum Anzeigen der Kubernetes-Webbenutzeroberfläche unter [http://localhost:8001/ui](http://localhost:8001/ui) verwenden können. Weitere Informationen finden Sie unter [Verwenden der Webbenutzeroberfläche von Kubernetes mit Azure Container Service](container-service-kubernetes-ui.md).

![Abbildung des Kubernetes-Dashboards](media/container-service-kubernetes-walkthrough/kubernetes-dashboard.png)

### <a name="remote-sessions-inside-your-containers"></a>Remotesitzungen in Ihren Containern
Mit Kubernetes können Sie Befehle in einem Docker-Remotecontainer Ihres Clusters ausführen.

```bash
# Get the name of your nginx pods
kubectl get pods
```

Mit dem Pod-Namen können Sie einen Remotebefehl auf dem Pod ausführen.  Beispiel:

```bash
kubectl exec <pod name> date
```

Außerdem erhalten Sie mit den `-it`-Flags eine vollständig interaktive Sitzung:

```bash
kubectl exec <pod name> -it bash
```

![Remotesitzung in einem Container](media/container-service-kubernetes-walkthrough/kubernetes-remote.png)



## <a name="next-steps"></a>Nächste Schritte

Unter den folgenden Ressourcen finden Sie Informationen dazu, welche weiteren Möglichkeiten Sie mit einem Kubernetes-Cluster haben:

* [Kubernetes Bootcamp](https://katacoda.com/embed/kubernetes-bootcamp/1/): Es wird veranschaulicht, wie Sie Anwendungen in Containern bereitstellen, skalieren, aktualisieren und debuggen.
* [Kubernetes User Guide](http://kubernetes.io/docs/user-guide/) (Kubernetes-Benutzerhandbuch): Enthält Informationen zum Ausführen von Programmen in einem vorhandenen Kubernetes-Cluster.
* [Kubernetes Examples](https://github.com/kubernetes/kubernetes/tree/master/examples) (Kubernetes-Beispiele): Enthält Beispiele für die Ausführung von echten Anwendungen mit Kubernetes.

