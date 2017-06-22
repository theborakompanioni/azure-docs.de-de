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
ms.date: 05/08/2017
ms.author: anhowe
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: a643f139be40b9b11f865d528622bafbe7dec939
ms.openlocfilehash: 0604a85192ed632b621113b98cc44172c584ea01
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017

---

# <a name="get-started-with-a-kubernetes-cluster-in-container-service"></a>Erste Schritte mit einem Kubernetes-Cluster in Container Service


In dieser exemplarischen Vorgehensweise wird veranschaulicht, wie Sie die Befehle von Azure CLI 2.0 verwenden, um in Azure Container Service einen Kubernetes-Cluster zu erstellen. Anschließend können Sie das Befehlszeilentool `kubectl` verwenden, um mit der Nutzung der Container im Cluster zu beginnen.

Die folgende Abbildung zeigt die Architektur eines Container Service-Clusters mit einem Linux-Master und zwei Linux-Agents. Der Master dient als Kubernetes-REST-API. Die Agentknoten werden in einer Azure-Verfügbarkeitsgruppe gruppiert und dienen zum Ausführen Ihrer Container. Alle VMs befinden sich in demselben privaten virtuellen Netzwerk, und untereinander besteht Vollzugriff.

![Abbildung: Kubernetes-Cluster in Azure](media/container-service-kubernetes-walkthrough/kubernetes.png)

Weitere Hintergrundinformationen finden Sie in der [Einführung in Docker-Containerhostinglösungen](container-service-intro.md) und der [Kubernetes-Dokumentation](https://kubernetes.io/docs/home/).

## <a name="prerequisites"></a>Voraussetzungen
Zum Erstellen eines Azure Container Service-Clusters mit Azure CLI 2.0 ist Folgendes erforderlich:
* Azure-Konto ([kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/))
* Installation und Einrichtung von [Azure CLI 2.0](/cli/azure/install-az-cli2)

Außerdem benötigen Sie Folgendes (oder Sie können die Azure-CLI zum automatischen Generieren während der Clusterbereitstellung verwenden):

* **Öffentlicher SSH-RSA-Schlüssel**: Informationen zum vorherigen Erstellen von SSH-RSA-Schlüsseln (Secure Shell) finden Sie in den Anleitungen für [MacOS und Linux](../virtual-machines/linux/mac-create-ssh-keys.md) oder für [Windows](../virtual-machines/linux/ssh-from-windows.md). 

* **Client-ID und Clientgeheimnis des Dienstprinzipals**: Eine Anleitung zum Erstellen eines Azure Active Directory-Dienstprinzipals und weitere Informationen finden Sie unter [Informationen zum Dienstprinzipal für einen Kubernetes-Cluster](container-service-kubernetes-service-principal.md).

 Im Beispielbefehl in diesem Artikel werden die SSH-Schlüssel und der Dienstprinzipal automatisch erstellt.

## <a name="create-your-kubernetes-cluster"></a>Erstellen Ihres Kubernetes-Clusters

Hier sind kurze Bash-Shellbefehle angegeben, in denen Azure CLI 2.0 zum Erstellen des Clusters verwendet wird. 

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Zum Erstellen Ihres Clusters müssen Sie zuerst eine Ressourcengruppe an einem Standort erstellen, an dem der Azure Container Service [verfügbar](https://azure.microsoft.com/regions/services/) ist. Führen Sie Befehle aus, die den folgenden Befehlen ähneln:

```azurecli
RESOURCE_GROUP=my-resource-group
LOCATION=westus
az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="create-a-cluster"></a>Erstellen eines Clusters
Erstellen Sie einen Kubernetes-Cluster in Ihrer Ressourcengruppe mithilfe des Befehls `az acs create` mit `--orchestrator-type=kubernetes`. Informationen zur Befehlssyntax finden Sie in der `az acs create`-[Hilfe](/cli/azure/acs#create).

Bei dieser Version des Befehls werden die SSH-RSA-Schlüssel und der Dienstprinzipal für den Kubernetes-Cluster automatisch erstellt.



```azurecli
DNS_PREFIX=some-unique-value
CLUSTER_NAME=any-acs-cluster-name
az acs create --orchestrator-type=kubernetes --resource-group $RESOURCE_GROUP --name=$CLUSTER_NAME --dns-prefix=$DNS_PREFIX --generate-ssh-keys
```

Nach einigen Minuten ist der Befehl abgeschlossen, und Sie sollten über einen funktionierenden Kubernetes-Cluster verfügen.

> [!IMPORTANT]
> Wenn Ihr Konto nicht über die Berechtigungen zum Erstellen des Azure AD-Dienstprinzipals verfügt, generiert der Befehl einen Fehler ähnlich diesem: **Nicht genügend Berechtigungen zum Abschließen des Vorgangs**. Weitere Informationen finden Sie im Artikel zum [Dienstprinzipal für einen Kubernetes-Cluster](container-service-kubernetes-service-principal.md).
> 



### <a name="connect-to-the-cluster"></a>Verbinden mit dem Cluster

Zum Herstellen der Verbindung mit dem Kubernetes-Cluster auf Ihrem Clientcomputer verwenden Sie [`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/), also den Kubernetes-Befehlszeilenclient. 

Falls Sie die Installation von `kubectl` noch nicht durchgeführt haben, können Sie `az acs kubernetes install-cli` zum Installieren verwenden. (Sie können es auch von der [Kubernetes-Website](https://kubernetes.io/docs/tasks/kubectl/install/) herunterladen.)

```azurecli
sudo az acs kubernetes install-cli
```

> [!TIP]
> Standardmäßig wird mit diesem Befehl die `kubectl`-Binärdatei auf einem Linux- oder Mac OS-System unter `/usr/local/bin/kubectl` und auf einem Windows-System unter `C:\Program Files (x86)\kubectl.exe` installiert. Um einen anderen Installationspfad anzugeben, können Sie den Parameter `--install-location` verwenden.
>
> Stellen Sie nach der Installation von `kubectl` sicher, dass das dazugehörige Verzeichnis in Ihrem Systempfad enthalten ist, oder fügen Sie es dem Pfad hinzu. 
>


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

### <a name="start-a-container"></a>Starten eines Containers
Sie können einen Container (in diesem Fall der Nginx-Webserver) wie folgt ausführen:

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
Mit diesem Befehl wird ein authentifizierter Proxy auf „localhost“ ausgeführt, den Sie zum Anzeigen der Kubernetes-Webbenutzeroberfläche unter [http://localhost:8001/ui](http://localhost:8001/ui) verwenden können. Weitere Informationen finden Sie unter [Verwenden der Webbenutzeroberfläche von Kubernetes mit Azure Container Service](container-service-kubernetes-ui.md).

![Abbildung des Kubernetes-Dashboards](media/container-service-kubernetes-walkthrough/kubernetes-dashboard.png)

### <a name="remote-sessions-inside-your-containers"></a>Remotesitzungen in Ihren Containern
Mit Kubernetes können Sie Befehle in einem Docker-Remotecontainer Ihres Clusters ausführen.

```bash
# Get the name of your nginx pods
kubectl get pods
```

Mit dem Pod-Namen können Sie einen Remotebefehl auf dem Pod ausführen. Beispiel:

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

