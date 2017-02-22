---
title: "Verwalten eines Azure Kubernetes-Clusters mit der Webbenutzeroberfläche | Microsoft-Dokumentation"
description: "Verwenden der Webbenutzeroberfläche von Kubernetes in Azure Container Service"
services: container-service
documentationcenter: 
author: bburns
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/09/2016
ms.author: bburns
translationtype: Human Translation
ms.sourcegitcommit: 0aa9b3ae14f586fc79e6ebee898e794d526c19bd
ms.openlocfilehash: 4192369f2e7758200131aa85c60d07436f7cbbdc


---

# <a name="using-the-kubernetes-web-ui-with-azure-container-service"></a>Verwenden der Webbenutzeroberfläche von Kubernetes mit Azure Container Service

## <a name="prerequisites"></a>Voraussetzungen
In dieser exemplarischen Vorgehensweise wird davon ausgegangen, dass Sie mit [Azure Container Service einen Kubernetes-Cluster erstellt haben](container-service-kubernetes-walkthrough.md).

> [!NOTE]
> Die Kubernetes-Unterstützung in Azure Container Service befindet sich derzeit in der Vorschauphase.
>

Ferner wird angenommen, dass die Azure CLI und Kubectl-Tools installiert sind.

Führen Sie Folgendes aus, um zu prüfen, ob das Tool `az` installiert ist:

```console
$ az --version
```

Wenn das Tool `az` nicht installiert ist, finden Sie [hier](https://github.com/azure/azure-cli#installation) Anweisungen.

Führen Sie Folgendes aus, um zu prüfen, ob das Tool `kubectl` installiert ist:

```console
$ kubectl version
```

Wenn `kubectl` nicht installiert ist, können Sie folgenden Befehl ausführen:

```console
$ az acs kubernetes install-cli
```

## <a name="overview"></a>Übersicht

### <a name="connect-to-the-web-ui"></a>Herstellen einer Verbindung mit der Webbenutzeroberfläche
Sie können die Kubernetes-Webbenutzeroberfläche starten, indem Sie Folgendes ausführen:

```console
$ az acs kubernetes browse
```

Ein Webbrowser sollte geöffnet werden, der für die Kommunikation mit einem sicheren Proxy konfiguriert ist, der Ihren lokalen Computer mit der Kubernetes-Webbenutzeroberfläche verbindet.

### <a name="create-and-expose-a-service"></a>Erstellen und Bereitstellen eines Diensts
Auf der Webbenutzeroberfläche von Kubernetes befindet sich im Fenster rechts oben die Schaltfläche „Create“.

![Kubernetes-Benutzeroberfläche mit Schaltfläche „Erstellen“](media/k8s/create.png)

Hierüber wird ein Dialogfeld geöffnet, in dem Sie mit dem Erstellen Ihrer Anwendung beginnen können.
Geben Sie ihr den Namen `hello-nginx`. Verwenden Sie den [`nginx`-Container aus Docker](https://hub.docker.com/_/nginx/), und stellen Sie drei Replikate für diesen Webdienst bereit.

![Kubernetes-Pod-Dialogfeld „Create“](media/k8s/nginx.png)

Erstellen Sie einen Kubernetes-Dienst des Typs „External“ für einen Lastenausgleich des Datenverkehrs Ihrer drei Replikate.  Wählen Sie „External“ aus, und geben Sie für „Port“ 80 ein.

![Kubernetes-Dialogfeld für die Diensterstellung](media/k8s/service.png)

Klicken Sie abschließend auf die Schaltfläche „Deploy“, um diese Container und Dienste bereitzustellen.

![Kubernetes-Schaltfläche „Deploy“](media/k8s/deploy.png)

### <a name="view-your-containers"></a>Anzeigen Ihrer Container
Nach Klicken auf „Deploy“ wird auf der Benutzeroberfläche eine Ansicht Ihres bereitgestellten Diensts gezeigt:

![Kubernetes-Status](media/k8s/status.png)

Sie können den Status der einzelnen Kubernetes-Objekte links auf der Benutzeroberfläche anhand des Kreises erkennen. Wen ein Kreis nur teilweise vollständig ist, wird das Objekt noch bereitgestellt. Wenn ein Objekt vollständig bereitgestellt ist, wird ein grünes Häkchen angezeigt:

![Anzeige des Kubernetes-Bereitstellungsstatus](media/k8s/deployed.png)

Sobald alles ausgeführt wird, können Sie auf einen Ihrer Pods klicken, um Einzelheiten zum ausgeführten Webdienst anzuzeigen.

![Kubernetes-Pods](media/k8s/pods.png)

In der Ansicht eines Pods werden Informationen zu den Containern im Pod sowie zu den von diesen Containern verwendeten CPU- und Arbeitsspeicherressourcen gezeigt:

![Kubernetes-Ressourcen](media/k8s/resources.png)

Wenn die Ressourcen nicht angezeigt werden, müssen Sie möglicherweise einige Minuten warten, bis die Überwachungsdaten weitergegeben wurden.

Sie können auch auf den Link „Logs“ klicken, um die Protokolle für Ihren Container anzuzeigen:

![Kubernetes-Protokolle](media/k8s/logs.png)

### <a name="viewing-your-service"></a>Anzeigen Ihres Diensts
Zusätzlich zur Ausführung Ihrer Container hat die Kubernetes-Benutzeroberfläche einen externen `Service` erstellt, der einen Lastenausgleich für den Datenverkehr zu den Containern in Ihrem Cluster bereitstellt.

Klicken Sie im linken Navigationsbereich auf „Services“, um alle Dienste anzuzeigen (es sollte nur einer vorhanden sein).

![Kubernetes-Dienste](media/k8s/service-deployed.png)

In dieser Ansicht sollte eine externe IP-Adresse angezeigt werden, die Ihrem Dienst zugeordnet ist.
Wenn Sie auf diese IP-Adresse klicken, sollte Ihr nginx-Container angezeigt werden, der hinter dem Lastenausgleich ausgeführt wird.

![nginx-Ansicht](media/k8s/nginx-page.png)

### <a name="resizing-your-service"></a>Ändern der Größe Ihres Diensts
Zusätzlich zum Anzeigen der Objekte auf der Benutzeroberfläche können Sie die Kubernetes-API-Objekte auch bearbeiten und aktualisieren.

Wechseln Sie zunächst zur `Deployment` Ihres Diensts, indem Sie im linken Navigationsbereich auf „Deployments“ klicken.

Sobald Sie in dieser Ansicht sind, klicken Sie auf die Replikatgruppe und dann auf der oberen Navigationsleiste auf die Schaltfläche „Edit“:

![Kubernetes-Schaltfläche „Edit“](media/k8s/edit.png)

Ändern Sie das Feld `spec.replicas` in „2“, und klicken Sie auf „Update“.

Dadurch sinkt die Anzahl der Replikate um zwei, indem einer Ihrer Pods gelöscht wird.

 




<!--HONumber=Jan17_HO4-->


