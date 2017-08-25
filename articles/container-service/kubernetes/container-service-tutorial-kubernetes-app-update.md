---
title: "Tutorial für Azure Container Service – Aktualisieren einer Anwendung | Microsoft-Dokumentation"
description: "Tutorial für Azure Container Service – Aktualisieren einer Anwendung"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Container, Microservices, Kubernetes, DC/OS, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: aurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: nepeters
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: bfd49ea68c597b109a2c6823b7a8115608fa26c3
ms.openlocfilehash: 72cdbfe2fe65e5152ca748cbb3989e3ef980ee50
ms.contentlocale: de-de
ms.lasthandoff: 07/25/2017

---

# <a name="update-an-application-in-kubernetes"></a>Aktualisieren einer Anwendung in Kubernetes

Nach der Bereitstellung einer Anwendung in Kubernetes kann diese durch Angeben eines neuen Containerimages oder einer neuen Imageversion aktualisiert werden. Wenn Sie eine Anwendung aktualisieren, wird der Updaterollout bereitgestellt, sodass jeweils nur ein Teil der Bereitstellung aktualisiert wird. Durch dieses bereitgestellte Update kann die Anwendung während des Updates weiter ausgeführt werden. Zudem wird ein Rollbackmechanismus bereitgestellt, wenn bei der Bereitstellung ein Fehler auftritt. 

In diesem Tutorial – Teil 6 von 7 – wird die Azure Voting-Beispiel-App aktualisiert. Sie führen folgende Aufgaben aus:

> [!div class="checklist"]
> * Aktualisieren des Front-End-Anwendungscodes
> * Erstellen eines aktualisierten Containerimages
> * Übertragen des Containerimages in Azure Container Registry per Push
> * Bereitstellen des aktualisierten Containerimages

In nachfolgenden Tutorials wird die Operations Management Suite für die Überwachung des Kubernetes-Clusters konfiguriert.

## <a name="before-you-begin"></a>Voraussetzungen

In vorherigen Tutorials wurde eine Anwendung in ein Containerimage gepackt, das Image wurde in Azure Container Registry hochgeladen, und es wurde ein Kubernetes-Cluster erstellt. Anschließend wurde die Anwendung im Kubernetes-Cluster ausgeführt. 

Wenn Sie diese Schritte nicht ausgeführt haben und dies jetzt nachholen möchten, kehren Sie zum [Tutorial 1 – Erstellen von Containerimages](./container-service-tutorial-kubernetes-prepare-app.md) zurück. 

## <a name="update-application"></a>Aktualisieren der Anwendung

Um die Schritte in diesem Tutorial ausführen zu können, müssen Sie eine Kopie der Azure Voting App geklont haben. Erstellen Sie diese geklonte Kopie gegebenenfalls mit dem folgenden Befehl:

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

Öffnen Sie die Datei `config_file.cfg` mit einem Code-Editor oder Text-Editor. Diese Datei befindet sich unter dem folgenden Verzeichnis des geklonten Repositorys.

```bash
 /azure-voting-app-redis/azure-vote/azure-vote/config_file.cfg
```

Ändern Sie die Werte für `VOTE1VALUE` und `VOTE2VALUE`, und speichern Sie dann die Datei.

```bash
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

Verwenden Sie [docker-compose](https://docs.docker.com/compose/), um das Front-End-Image neu zu erstellen und die aktualisierte Anwendung auszuführen.

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml up --build -d
```

## <a name="test-application-locally"></a>Lokales Testen der Anwendung

Wechseln Sie zu `http://localhost:8080`, um die aktualisierte Anwendung anzuzeigen.

![Abbildung: Kubernetes-Cluster in Azure](media/container-service-kubernetes-tutorials/vote-app-updated.png)

## <a name="tag-and-push-images"></a>Kennzeichnen und Übertragen von Images per Push

Kennzeichnen Sie das Image *azure-vote-front* mit dem loginServer-Namen der Containerregistrierung.

Wenn Sie Azure Container Registry verwenden, rufen Sie den Namen des Anmeldeservers mit dem Befehl [az acr list](/cli/azure/acr#list) ab.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Verwenden Sie [docker tag](https://docs.docker.com/engine/reference/commandline/tag/), um das Image zu kennzeichnen. Ersetzen Sie `<acrLoginServer>` durch den Namen des Azure Container Registry-Anmeldeservers oder den Hostnamen der öffentlichen Registrierung.

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v2
```

Verwenden Sie [docker push](https://docs.docker.com/engine/reference/commandline/push/), um das Image in Ihre Registrierung hochzuladen. Ersetzen Sie `<acrLoginServer>` durch den Namen des Azure Container Registry-Anmeldeservers oder den Hostnamen der öffentlichen Registrierung.

```bash
docker push <acrLoginServer>/azure-vote-front:redis-v2
```

## <a name="deploy-update-application"></a>Bereitstellen der aktualisierten Anwendung

Zur Gewährleistung der maximalen Betriebszeit müssen mehrere Instanzen des Anwendungs-Pods ausgeführt werden. Überprüfen Sie diese Konfiguration mit dem Befehl [kubectl get pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get).

```bash
kubectl get pod
```

Ausgabe:

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-217588096-5w632    1/1       Running   0          10m
azure-vote-front-233282510-b5pkz   1/1       Running   0          10m
azure-vote-front-233282510-dhrtr   1/1       Running   0          10m
azure-vote-front-233282510-pqbfk   1/1       Running   0          10m
```

Wenn das Image „azure-vote-front“ nicht auf mehreren Pods ausgeführt wird, skalieren Sie die Bereitstellung von *azure-vote-front*.


```azurecli-interactive
kubectl scale --replicas=3 deployment/azure-vote-front
```

Um die Anwendung zu aktualisieren, verwenden Sie den Befehl [kubectl set](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#set). Aktualisieren Sie `<acrLoginServer>` mit dem Anmeldeserver oder dem Hostnamen der Containerregistrierung.

```azurecli-interactive
kubectl set image deployment azure-vote-front azure-vote-front=<acrLoginServer>/azure-vote-front:redis-v2
```

Verwenden Sie zum Überwachen der Bereitstellung den Befehl [kubectl get pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get). Nach Bereitstellung der aktualisierten Anwendung werden die Pods beendet und mit dem neuen Containerimage neu erstellt.

```azurecli-interactive
kubectl get pod
```

Ausgabe:

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2978095810-gq9g0   1/1       Running   0          5m
azure-vote-front-1297194256-tpjlg   1/1       Running   0         1m
azure-vote-front-1297194256-tptnx   1/1       Running   0         5m
azure-vote-front-1297194256-zktw9   1/1       Terminating   0         1m
```

## <a name="test-updated-application"></a>Testen der aktualisierten Anwendung

Rufen Sie die externe IP-Adresse des Diensts *azure-vote-front* ab.

```azurecli-interactive
kubectl get service azure-vote-front
```

Wechseln Sie zu der IP-Adresse, um die aktualisierte Anwendung anzuzeigen.

![Abbildung: Kubernetes-Cluster in Azure](media/container-service-kubernetes-tutorials/vote-app-updated-external.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Anwendung aktualisiert und diese Aktualisierung in einem Kubernetes-Cluster bereitgestellt. Die folgenden Aufgaben wurden ausgeführt:

> [!div class="checklist"]
> * Aktualisieren des Front-End-Anwendungscodes
> * Erstellen eines aktualisierten Containerimages
> * Übertragen des Containerimages in Azure Container Registry per Push
> * Bereitstellen der aktualisierten Anwendung

Im nächsten Tutorial erfahren Sie, wie Sie Kubernetes mit der Operations Management Suite überwachen.

> [!div class="nextstepaction"]
> [Überwachen von Kubernetes mit OMS](./container-service-tutorial-kubernetes-monitor.md)
