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
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/14/2017
ms.author: nepeters
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: d24c6777cc6922d5d0d9519e720962e1026b1096
ms.openlocfilehash: 2c7c8e241010e86bf9ffe5b70921da71b8ace9da
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---

# <a name="create-container-images-to-be-used-with-azure-container-service"></a>Containerimages zur Verwendung mit Azure Container Service erstellen

In diesem Tutorial – Teil 1 von 7 – wird eine Anwendung mit mehreren Containern für die Verwendung in Kubernetes vorbereitet. Folgende Schritte werden ausgeführt:  

> [!div class="checklist"]
> * Klonen der Anwendungsquelle von GitHub  
> * Erstellen eines Containerimages aus der Anwendungsquelle
> * Testen der Anwendung in einer lokalen Docker-Umgebung

Nach Abschluss kann in Ihrer lokalen Entwicklungsumgebung auf die folgende Anwendung zugegriffen werden.

![Abbildung: Kubernetes-Cluster in Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

In den nachfolgenden Tutorials wird das Containerimage in eine Azure Container Registry-Instanz hochgeladen und anschließend in einem in Azure gehosteten Kubernetes-Cluster ausgeführt.

## <a name="before-you-begin"></a>Voraussetzungen

In diesem Tutorial wird vorausgesetzt, dass zentrale Docker-Begriffe wie Container und Containerimages sowie grundlegende Docker-Befehle bekannt sind. Eine Einführung in die Grundlagen der Container finden Sie bei Bedarf unter [Get started with Docker (Erste Schritte mit Docker)]( https://docs.docker.com/get-started/). 

Für dieses Tutorial ist eine Docker-Entwicklungsumgebung erforderlich. Für Docker sind Pakete erhältlich, mit denen Docker problemlos auf einem [Mac](https://docs.docker.com/docker-for-mac/)-, [Windows](https://docs.docker.com/docker-for-windows/)- oder [Linux](https://docs.docker.com/engine/installation/#supported-platforms)-System konfiguriert werden kann.

## <a name="get-application-code"></a>Abrufen von Anwendungscode

Die in diesem Tutorial verwendete Beispielanwendung ist eine einfache Abstimmungs-App. Die Anwendung besteht aus einer Front-End-Webkomponente und einer Back-End-Redis-Instanz. Die Webkomponente wird in ein benutzerdefiniertes Containerimage gepackt. Die Redis-Instanz verwendet ein nicht modifiziertes Image aus Docker Hub.  

Verwenden Sie Git, um eine Kopie der Anwendung in Ihre Entwicklungsumgebung herunterzuladen.

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

Wechseln Sie in das Verzeichnis, sodass Sie im geklonten Verzeichnis arbeiten.

```
cd azure-voting-app-redis
```

Im Verzeichnis befinden sich der Anwendungsquellcode, eine vorab erstellte Docker Compose-Datei und eine Kubernetes-Manifestdatei. Diese Dateien werden während des Tutorials verwendet. 

## <a name="create-container-images"></a>Erstellen von Containerimages

[Docker Compose](https://docs.docker.com/compose/) kann verwendet werden, um die Erstellung von Containerimages und die Bereitstellung von Anwendungen mit mehreren Containern zu automatisieren.

Führen Sie die Datei `docker-compose.yml` aus, um das Containerimage zu erstellen, das Redis-Image herunterzuladen und die Anwendung zu starten.

```bash
docker-compose up -d
```

Verwenden Sie anschließend den Befehl [docker-images](https://docs.docker.com/engine/reference/commandline/images/), um die erstellten Images anzuzeigen.

```bash
docker images
```

Beachten Sie, dass drei Images heruntergeladen oder erstellt wurden. Das `azure-vote-front`-Image enthält die Anwendung und verwendet das `nginx-flask`-Image als Grundlage. Das `redis`-Image wird zum Starten einer Redis-Instanz verwendet.

```bash
REPOSITORY                   TAG        IMAGE ID            CREATED             SIZE
azure-vote-front             latest     9cc914e25834        40 seconds ago      694MB
redis                        latest     a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask      788ca94b2313        9 months ago        694MB
```

Führen Sie den Befehl [docker ps](https://docs.docker.com/engine/reference/commandline/ps/) aus, um die ausgeführten Container anzuzeigen.

```bash
docker ps
```

Ausgabe:

```bash
CONTAINER ID        IMAGE             COMMAND                  CREATED             STATUS              PORTS                           NAMES
82411933e8f9        azure-vote-front  "/usr/bin/supervisord"   57 seconds ago      Up 30 seconds       443/tcp, 0.0.0.0:8080->80/tcp   azure-vote-front
b68fed4b66b6        redis             "docker-entrypoint..."   57 seconds ago      Up 30 seconds       0.0.0.0:6379->6379/tcp          azure-vote-back
```

## <a name="test-application-locally"></a>Lokales Testen der Anwendung

Navigieren Sie zu http://localhost:8080, um die ausgeführte Anwendung anzuzeigen.

![Abbildung: Kubernetes-Cluster in Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Nach der Überprüfung der Funktionsfähigkeit der Anwendung können die ausgeführten Container beendet und entfernt werden. Löschen Sie die Containerimages nicht. Das `azure-vote-front`-Image wird im nächsten Tutorial in eine Azure Container Registry-Instanz hochgeladen.

Führen Sie folgenden Befehl aus, um die ausgeführten Container zu beenden.

```bash
docker-compose stop
```

Löschen Sie die beendeten Container und Ressourcen mit dem folgenden Befehl:

```bash
docker-compose down
```

Nach Abschluss des Vorgangs verfügen Sie über ein Containerimage, das die Azure Voting-Anwendung enthält.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde eine Anwendung getestet und es wurden Containerimages für die Anwendung erstellt. Die folgenden Schritte wurden durchgeführt:

> [!div class="checklist"]
> * Klonen der Anwendungsquelle von GitHub  
> * Erstellen eines Containerimages aus der Anwendungsquelle
> * Testen der Anwendung in einer lokalen Docker-Umgebung

Wechseln Sie zum nächsten Tutorial, und erfahren Sie, wie Containerimages in einer Azure Container Registry gespeichert werden.

> [!div class="nextstepaction"]
> [Übertragen von Images zu Azure Container Registry mithilfe von Push](./container-service-tutorial-kubernetes-prepare-acr.md)

