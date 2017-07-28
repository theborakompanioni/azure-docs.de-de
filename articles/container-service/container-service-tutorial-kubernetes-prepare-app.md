---
title: "Tutorial für Azure Container Service - App vorbereiten | Microsoft-Dokumentation"
description: "Tutorial für Azure Container Service - App vorbereiten"
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
ms.date: 06/26/2017
ms.author: nepeters
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: 49d727de69c700af0ae6402ad54b5880010ebb3b
ms.contentlocale: de-de
ms.lasthandoff: 06/28/2017

---

# <a name="create-container-images-to-be-used-with-azure-container-service"></a>Containerimages zur Verwendung mit Azure Container Service erstellen

In diesem Tutorial wird eine Anwendung für Kubernetes vorbereitet. Folgende Schritte werden durchgeführt:  

> [!div class="checklist"]
> * Klonen der Anwendungsquelle von GitHub  
> * Erstellen von Containerimages aus der Anwendungsquelle
> * Testen der Images in einer lokalen Docker-Umgebung

In den nachfolgenden Tutorials werden diese Containerimages in eine Azure Container Registry hochgeladen und anschließend in einem mit Azure gehosteten Kubernetes-Cluster ausgeführt.

## <a name="before-you-begin"></a>Voraussetzungen

In diesem Tutorial wird vorausgesetzt, dass zentrale Docker-Begriffe wie Container und Containerimages sowie grundlegende Docker-Befehle bekannt sind. Eine Einführung in die Grundlagen der Container finden Sie bei Bedarf unter [Get started with Docker (Erste Schritte mit Docker)]( https://docs.docker.com/get-started/). 

Für dieses Tutorial ist eine Docker-Entwicklungsumgebung erforderlich. Für Docker sind Pakete erhältlich, mit denen Docker problemlos auf einem [Mac](https://docs.docker.com/docker-for-mac/)-, [Windows](https://docs.docker.com/docker-for-windows/)- oder [Linux](https://docs.docker.com/engine/installation/#supported-platforms)-System konfiguriert werden kann.

## <a name="get-application-code"></a>Abrufen von Anwendungscode

Die in diesem Tutorial verwendete Beispielanwendung ist eine einfache Abstimmungs-App. Die Anwendung besteht aus einer Front-End-Webkomponente und eine Back-End-Datenbank. 

Verwenden Sie Git, um eine Kopie der Anwendung in Ihre Entwicklungsumgebung herunterzuladen.

```bash
git clone https://github.com/Azure-Samples/azure-voting-app.git
```

Im Anwendungsverzeichnis finden Sie vorab erstellte Dockerfiles und Kubernetes-Manifestdateien. Mit diesen Dateien werden während des Tutorials Ressourcen erstellt. 

## <a name="create-container-images"></a>Erstellen von Containerimages

Verwenden Sie zum Erstellen eines Containerimages für das Anwendungs-Front-End den Befehl [docker build](https://docs.docker.com/engine/reference/commandline/build/).

```bash
docker build ./azure-voting-app/azure-vote -t azure-vote-front
```

Wiederholen Sie den Befehl für das Back-End-Containerimage.

```bash
docker build ./azure-voting-app/azure-vote-mysql -t azure-vote-back
```

Verwenden Sie anschließend den Befehl `docker images`, um die erstellten Images anzuzeigen. 

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

## <a name="test-application"></a>Testen der Anwendung

Testen Sie die beiden Containerimages nach dem Erstellen, und testen Sie sie in Ihrer lokalen Entwicklungsumgebung. 

Erstellen Sie zunächst ein Docker-Netzwerk. Dieses Netzwerk wird für die Kommunikation zwischen den Containern verwendet.  

```bash
docker network create azure-vote
```

Führen Sie eine Instanz des Back-End-Containerimages mithilfe des Befehls `docker run` aus.

In diesem Beispiel wird die Datenbankdatei „mysql“ im Container gespeichert. Sobald diese Anwendung in die Kubernete-Cluster verschoben wird, wird ein externes Datenvolumen zum Speichern der Datenbankdatei verwendet. Darüber hinaus werden Umgebungsvariablen zum Festlegen von MySQL-Anmeldeinformationen verwendet.

```bash
docker run -p 3306:3306 --name azure-vote-back -d --network azure-vote -e MYSQL_ROOT_PASSWORD=Password12 -e MYSQL_USER=dbuser -e MYSQL_PASSWORD=Password12 -e MYSQL_DATABASE=azurevote azure-vote-back 
```

Führen Sie eine Instanz des Front-End-Containerimages aus.

Zum Konfigurieren der Datenbank-Verbindungsinformationen werden Umgebungsvariablen verwendet.

```bash
docker run -d -p 8080:80 --name azure-vote-front --network=azure-vote -e MYSQL_USER=dbuser -e MYSQL_PASSWORD=Password12 -e MYSQL_DATABASE=azurevote -e MYSQL_HOST=azure-vote-back azure-vote-front
```

Führen Sie nach Abschluss des Vorgangs `docker ps` aus, um die ausgeführten Container anzuzeigen.  

```bash
docker ps
```

Ausgabe:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                           NAMES
3aa02e8ae965        azure-vote-front     "/usr/bin/supervisord"   59 seconds ago      Up 57 seconds       443/tcp, 0.0.0.0:8080->80/tcp   flaskmysqlvote_azure-vote-front_1
5ae60b3ba181        azure-vote-backend   "docker-entrypoint..."   59 seconds ago      Up 58 seconds       0.0.0.0:3306->3306/tcp          azure-vote-back
```

Navigieren Sie zu `http://localhost:8080`, um die ausgeführte Anwendung anzuzeigen. Die Anwendung braucht einige Sekunden, um zu initialisieren. Wenn ein Fehler auftritt, versuchen Sie es erneut.

![Abbildung: Kubernetes-Cluster in Azure](media/container-service-kubernetes-tutorials/vote-app.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Nach der Überprüfung der Funktionsfähigkeit der Anwendung können die ausgeführten Container beendet und entfernt werden. Löschen Sie die Containerimages nicht. Diese Images werden im nächsten Tutorial in eine Azure Container Registry-Instanz hochgeladen.

Beenden und löschen Sie den Front-End-Container mit dem Befehl [docker rm](https://docs.docker.com/engine/reference/commandline/rm/). 

```bash
docker rm -f azure-vote-front
```

Beenden und löschen Sie den Back-End-Container mit dem Befehl [docker rm](https://docs.docker.com/engine/reference/commandline/rm/). 

```bash
docker rm -f azure-vote-back
```

Löschen Sie das Netzwerk mit dem Befehl [docker network rm](https://docs.docker.com/engine/reference/commandline/network_rm/).

```bash
docker network rm azure-vote
```

Nach Abschluss des Vorgangs verfügen Sie über zwei Containerimages, aus denen die Anwendung Azure Vote besteht.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde eine Anwendung getestet und es wurden Containerimages für die Anwendung erstellt. Die folgenden Schritte wurden durchgeführt:

> [!div class="checklist"]
> * Klonen der Anwendungsquelle von GitHub  
> * Erstellen von Containerimages aus der Anwendungsquelle
> * Testen der Images in einer lokalen Docker-Umgebung

Wechseln Sie zum nächsten Tutorial, und erfahren Sie, wie Containerimages in einer Azure Container Registry gespeichert werden.

> [!div class="nextstepaction"]
> [Übertragen von Images zu Azure Container Registry mithilfe von Push](./container-service-tutorial-kubernetes-prepare-acr.md)
