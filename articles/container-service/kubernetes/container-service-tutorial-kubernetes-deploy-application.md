---
title: "Tutorial für Azure Container Service – Bereitstellen einer Anwendung | Microsoft-Dokumentation"
description: "Tutorial für Azure Container Service – Bereitstellen einer Anwendung"
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
ms.date: 07/25/2017
ms.author: nepeters
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: bfd49ea68c597b109a2c6823b7a8115608fa26c3
ms.openlocfilehash: 81a6a3d5364642b2da75faf875d64d2f4a1939d4
ms.contentlocale: de-de
ms.lasthandoff: 07/25/2017

---

# <a name="run-applications-in-kubernetes"></a>Ausführen von Anwendungen in Kubernetes

In diesem Tutorial – Teil 4 von 7 – wird eine Beispielanwendung in einem Kubernetes-Cluster bereitgestellt. Folgende Schritte werden ausgeführt:

> [!div class="checklist"]
> * Herunterladen von Kubernetes-Manifestdateien
> * Ausführen einer Anwendung in Kubernetes
> * Testen der Anwendung

In den nachfolgenden Tutorials wird diese Anwendung horizontal hochskaliert und aktualisiert. Zudem wird die Operations Management Suite für die Überwachung des Kubernetes-Clusters konfiguriert.

Dieses Tutorial setzt voraus, dass Sie grundlegend mit den Konzepten von Kubernetes vertraut sind. Ausführliche Informationen zu Kubernetes finden Sie in der [Kubernetes-Dokumentation](https://kubernetes.io/docs/home/).

## <a name="before-you-begin"></a>Voraussetzungen

In vorherigen Tutorials wurde eine Anwendung in ein Containerimage gepackt, das Image wurde in Azure Container Registry hochgeladen, und es wurde ein Kubernetes-Cluster erstellt. Wenn Sie diese Schritte nicht ausgeführt haben und dies jetzt nachholen möchten, kehren Sie zum [Tutorial 1 – Erstellen von Containerimages](./container-service-tutorial-kubernetes-prepare-app.md) zurück. 

Für dieses Tutorial wird mindestens ein Kubernetes-Cluster benötigt.

## <a name="get-manifest-file"></a>Abrufen der Manifestdatei

In diesem Tutorial werden [Kubernetes-Objekte](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/) mithilfe eines Kubernetes-Manifests bereitgestellt. Ein Kubernetes-Manifest ist eine YAML- oder JSON-formatierte Datei, die die Kubernetes-Objektbereitstellung und Konfigurationsanweisungen enthält.

Die Anwendungsmanifestdatei für dieses Tutorial steht im Repository der Azure Voting-Anwendung zur Verfügung, das in einem vorherigen Tutorial geklont wurde. Wenn nicht bereits geschehen, klonen Sie das Repository mit folgendem Befehl: 

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

Die Manifestdatei befindet sich im folgenden Verzeichnis des geklonten Repositorys.

```bash
/azure-voting-app-redis/kubernetes-manifests/azure-vote-all-in-one-redis.yaml
```

## <a name="update-manifest-file"></a>Aktualisieren der Manifestdatei

Wenn Sie Azure Container Registry zum Speichern der Containerimages verwenden, muss das Manifest mit dem ACR-loginServer-Namen aktualisiert werden.

Rufen Sie den ACR-Anmeldeservernamen mit dem [az acr list](/cli/azure/acr#list)-Befehl auf.

```azurecli-interactive
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Das Beispielmanifest wurde vorab mit dem Repositorynamen *microsoft* erstellt. Öffnen Sie die Datei in einem beliebigen Text-Editor, und ersetzen Sie den Wert *microsoft* durch den Namen des Anmeldeservers Ihrer ACR-Instanz.

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:redis-v1
```

## <a name="deploy-application"></a>Bereitstellen der Anwendung

Führen Sie die Anwendung mithilfe des Befehls [kubectl create](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#create) aus. Dieser Befehl analysiert die Manifestdatei und erstellt die definierten Kubernetes-Objekte.

```azurecli-interactive
kubectl create -f ./azure-voting-app-redis/kubernetes-manifests/azure-vote-all-in-one-redis.yaml
```

Ausgabe:

```bash
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-application"></a>Testen der Anwendung

Es wird ein [Kubernetes-Dienst](https://kubernetes.io/docs/concepts/services-networking/service/) erstellt, der die Anwendung für das Internet verfügbar macht. Dieser Vorgang kann einige Minuten dauern. 

Verwenden Sie zum Überwachen des Fortschritts den Befehl [kubectl get service](https://review.docs.microsoft.com/en-us/azure/container-service/container-service-kubernetes-walkthrough?branch=pr-en-us-17681) mit dem Argument `--watch`.

```azurecli-interactive
kubectl get service azure-vote-front --watch
```

Der Wert **EXTENAL-IP** für den Dienst *azure-vote-front* wird zunächst als *ausstehend* angezeigt. Sobald die externe IP-Adresse nicht mehr *ausstehend* ist, sondern eine *IP-Adresse* angezeigt wird, verwenden Sie `CTRL-C`, um die kubectl-Überwachung zu beenden.

```bash
NAME               CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   10.0.42.158   <pending>     80:31873/TCP   1m
azure-vote-front   10.0.42.158   52.179.23.131 80:31873/TCP   2m
```

Um die Anwendung anzuzeigen, navigieren Sie zu der externen IP-Adresse.

![Abbildung: Kubernetes-Cluster in Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde die Azure Vote-Anwendung in einem Azure Container Service-Kubernetes-Cluster bereitgestellt. Folgende Aufgaben wurden ausgeführt:  

> [!div class="checklist"]
> * Herunterladen von Kubernetes-Manifestdateien
> * Ausführen der Anwendung in Kubernetes
> * Testen der Anwendung

Wechseln Sie zum nächsten Tutorial, wo Sie erfahren, wie Sie eine Kubernetes-Anwendung und die zugrunde liegende Kubernetes-Infrastruktur skalieren. 

> [!div class="nextstepaction"]
> [Scale Kubernetes application and infrastructure](./container-service-tutorial-kubernetes-scale.md) (Skalieren einer Kubernetes-Anwendung und -Infrastruktur)
