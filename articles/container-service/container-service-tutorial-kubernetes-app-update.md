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
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/06/2017
ms.author: nepeters
ms.translationtype: Human Translation
ms.sourcegitcommit: bb794ba3b78881c967f0bb8687b1f70e5dd69c71
ms.openlocfilehash: ce4a88a7958116890ec17eb2405ba809487b9c0f
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017

---

# <a name="update-an-application-in-kubernetes"></a>Aktualisieren einer Anwendung in Kubernetes

Nach der Bereitstellung einer Anwendung in Kubernetes können Sie sie durch Angeben eines neuen Containerimages oder einer neuen Imageversion aktualisieren. Wenn Sie eine Anwendung aktualisieren, wird der Updaterollout bereitgestellt, sodass jeweils nur ein Teil der Bereitstellung aktualisiert wird. 

Durch dieses bereitgestellte Update kann die Anwendung während des Updates weiter ausgeführt werden. Zudem wird ein Rollbackmechanismus bereitgestellt, wenn bei der Bereitstellung ein Fehler auftritt. 

In diesem Tutorial wird die Beispiel-App Azure Voting aktualisiert. Sie führen folgende Aufgaben aus:

> [!div class="checklist"]
> * Aktualisieren des Front-End-Anwendungscodes
> * Erstellen eines aktualisierten Containerimages
> * Übertragen des Containerimages in Azure Container Registry per Push
> * Bereitstellen einer aktualisierten Anwendung

## <a name="before-you-begin"></a>Voraussetzungen

In vorherigen Tutorials haben wir eine Anwendung in Containerimages gepackt, die Images in Azure Container Registry hochgeladen und einen Kubernetes-Cluster erstellt. Anschließend haben wir die Anwendung im Kubernetes-Cluster ausgeführt. 

Wenn Sie diese Schritte nicht ausgeführt haben und dies jetzt nachholen möchten, kehren Sie zum [Tutorial 1 – Erstellen von Containerimages](./container-service-tutorial-kubernetes-prepare-app.md) zurück. 

Für dieses Tutorial wird mindestens ein Kubernetes-Cluster mit einer ausgeführten Anwendung benötigt.

## <a name="update-application"></a>Aktualisieren der Anwendung

Um die Schritte in diesem Tutorial ausführen zu können, müssen Sie eine Kopie der Azure Voting App geklont haben. Erstellen Sie diese geklonte Kopie gegebenenfalls mit dem folgenden Befehl:

```bash
git clone https://github.com/Azure-Samples/azure-voting-app.git
```

Öffnen Sie die Datei `config_file.cfg` mit einem Code-Editor oder Text-Editor. Diese Datei befindet sich unter dem folgenden Verzeichnis des geklonten Repositorys.

```bash
 /azure-voting-app/azure-vote/azure-vote/config_file.cfg
```

Ändern Sie die Werte für `VOTE1VALUE` und `VOTE2VALUE`, und speichern Sie dann die Datei.

```bash
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Half Full'
VOTE2VALUE = 'Half Empty'
SHOWHOST = 'false'
```

Verwenden Sie `docker build`, um das Front-End-Image neu zu erstellen.

```bash
docker build --no-cache ./azure-voting-app/azure-vote -t azure-vote-front:v2
```

## <a name="test-application"></a>Testen der Anwendung

Erstellen Sie ein Docker-Netzwerk. Dieses Netzwerk wird für die Kommunikation zwischen den Containern verwendet.  

```bash
docker network create azure-vote
```

Führen Sie eine Instanz des Back-End-Containerimages mithilfe des Befehls `docker run` aus.

```bash
docker run -p 3306:3306 --name azure-vote-back -d --network azure-vote -e MYSQL_ROOT_PASSWORD=Password12 -e MYSQL_USER=dbuser -e MYSQL_PASSWORD=Password12 -e MYSQL_DATABASE=azurevote azure-vote-back 
```

Führen Sie eine Instanz des Front-End-Containerimages aus.

```bash
docker run -d -p 8080:80 --name azure-vote-front --network=azure-vote -e MYSQL_USER=dbuser -e MYSQL_PASSWORD=Password12 -e MYSQL_DATABASE=azurevote -e MYSQL_HOST=azure-vote-back azure-vote-front:v2
```

Navigieren Sie zu `http://localhost:8080`, um die aktualisierte Anwendung anzuzeigen. Die Initialisierung der Anwendung dauert einige Sekunden. Wenn eine Fehlermeldung angezeigt wird, wiederholen Sie den Vorgang.

![Abbildung: Kubernetes-Cluster in Azure](media/container-service-kubernetes-tutorials/vote-app-updated.png)

## <a name="tag-and-push-images"></a>Kennzeichnen und Übertragen von Images per Push

Kennzeichnen Sie das Image *azure-vote-front* mit dem loginServer-Namen der Containerregistrierung.

Wenn Sie Azure Container Registry verwenden, rufen Sie den Namen des Anmeldeservers mit dem Befehl [az acr list](/cli/azure/acr#list) ab.

```azurecli-interactive
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Verwenden Sie [docker tag](https://docs.docker.com/engine/reference/commandline/tag/), um das Image zu kennzeichnen, und aktualisieren Sie den Befehl mit dem Azure Container Registry-Anmeldeserver oder dem Hostnamen der öffentlichen Registrierung.

```bash
docker tag azure-vote-front:v2 <acrLoginServer>/azure-vote-front:v2
```

Übertragen Sie das Image per Push in Ihre Registrierung. Ersetzen Sie `<acrLoginServer>` durch den Namen des Azure Container Registry-Anmeldeservers oder den Hostnamen der öffentlichen Registrierung.

```bash
docker push <acrLoginServer>/azure-vote-front:v2
```

## <a name="deploy-update-to-kubernetes"></a>Bereitstellen der Aktualisierung in Kubernetes

### <a name="verify-multiple-pod-replicas"></a>Überprüfen mehrerer POD-Replikate

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


```bash
kubectl scale --replicas=3 deployment/azure-vote-front
```

### <a name="update-application"></a>Aktualisieren der Anwendung

Führen Sie den folgenden Befehl aus, um die Anwendung zu aktualisieren. Aktualisieren Sie `<acrLoginServer>` mit dem Anmeldeserver oder dem Hostnamen der Containerregistrierung.

```bash
kubectl set image deployment azure-vote-front azure-vote-front=<acrLoginServer>/azure-vote-front:v2
```

Verwenden Sie zum Überwachen der Bereitstellung den Befehl [kubectl get pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get). Nach Bereitstellung der aktualisierten Anwendung werden die Pods beendet und mit dem neuen Containerimage neu erstellt.

```bash
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

```bash
kubectl get service
```

Rufen Sie die IP-Adresse auf, um die aktualisierte Anwendung anzuzeigen.

![Abbildung: Kubernetes-Cluster in Azure](media/container-service-kubernetes-tutorials/vote-app-updated-external.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde eine Anwendung aktualisiert und diese Aktualisierung in einem Kubernetes-Cluster bereitgestellt. Die folgenden Aufgaben wurden ausgeführt:  

> [!div class="checklist"]
> * Aktualisieren des Front-End-Anwendungscodes
> * Erstellen eines aktualisierten Containerimages
> * Übertragen des Containerimages in Azure Container Registry per Push
> * Bereitstellen der aktualisierten Anwendung

Im nächsten Tutorial erfahren Sie, wie Sie Kubernetes mit der Operations Management Suite überwachen.

> [!div class="nextstepaction"]
> [Überwachen von Kubernetes mit OMS](./container-service-tutorial-kubernetes-monitor.md)
