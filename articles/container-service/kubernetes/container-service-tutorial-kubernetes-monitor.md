---
title: "Tutorial für Azure Container Service – Überwachen von Kubernetes | Microsoft-Dokumentation"
description: "Tutorial für Azure Container Service – Überwachen von Kubernetes mit Microsoft Operations Management Suite (OMS)"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Container, Microservices, Kubernetes, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: aurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: danlep
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: d24c6777cc6922d5d0d9519e720962e1026b1096
ms.openlocfilehash: 65556049d33c84848f015a095acf9c1f69348c6d
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---

# <a name="monitor-a-kubernetes-cluster-with-operations-management-suite"></a>Überwachen eines Kubernetes-Clusters mit der Operations Management Suite

Die Überwachung des Kubernetes-Clusters und der Kubernetes-Container ist wichtig, vor allem wenn Sie einen umfangreichen Produktionscluster mit mehreren Apps verwalten. 

Sie können verschiedene Kubernetes-Überwachungslösungen nutzen, entweder von Microsoft oder von anderen Anbietern. In diesem Tutorial überwachen Sie den Kubernetes-Cluster mithilfe der Containerlösung in der [Operations Management Suite](../../operations-management-suite/operations-management-suite-overview.md), der cloudbasierten IT-Verwaltungslösung von Microsoft. (Die OMS-Containerlösung befindet sich in der Vorschauversion.)

Dieses Tutorial – Teil 7 von 7 – behandelt folgende Aufgaben:

> [!div class="checklist"]
> * Abrufen von OMS-Arbeitsbereichseinstellungen
> * Einrichten von OMS-Agents auf den Kubernetes-Knoten
> * Zugreifen auf Überwachungsinformationen im OMS-Portal oder Azure-Portal

## <a name="before-you-begin"></a>Voraussetzungen

In vorherigen Tutorials wurden eine Anwendung in Containerimages gepackt, diese Images in Azure Container Registry hochgeladen und ein Kubernetes-Cluster erstellt. 

Wenn Sie diese Schritte nicht ausgeführt haben und dies jetzt nachholen möchten, kehren Sie zum [Tutorial 1 – Erstellen von Containerimages](./container-service-tutorial-kubernetes-prepare-app.md) zurück. 

## <a name="get-workspace-settings"></a>Abrufen von Arbeitsbereichseinstellungen

Wenn Sie Zugriff auf das [OMS-Portal](https://mms.microsoft.com) haben, wechseln Sie zu **Einstellungen** > **Verbundene Quellen** > **Linux-Server**. Dort finden Sie die *Arbeitsbereichs-ID* und einen primären oder sekundären *Arbeitsbereichsschlüssel*. Notieren Sie sich diese Werte, Sie benötigen sie zum Einrichten von OMS-Agents im Cluster.

## <a name="set-up-oms-agents"></a>Einrichten von OMS-Agents

Hier sehen Sie ein Beispiel für eine YAML-Datei zum Einrichten von OMS-Agents in den Linux-Clusterknoten. Mit der Datei wird ein Kubernetes-[DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) erstellt, das einen einzelnen identischen Pod auf jedem Clusterknoten ausführt. Die DaemonSet-Ressource eignet sich ideal für das Bereitstellen eines Überwachungs-Agents. 

Speichern Sie den folgenden Text in einer Datei mit dem Namen `oms-daemonset.yaml`, und ersetzen Sie die Platzhalterwerte für *myWorkspaceID* und *myWorkspaceKey* durch Ihre OMS-Arbeitsbereichs-ID und den Arbeitsbereichsschlüssel. (In der Produktion können Sie diese Werte als Geheimnisse codieren.)

```YAML
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
 name: omsagent
spec:
 template:
  metadata:
   labels:
    app: omsagent
    agentVersion: v1.3.4-127
    dockerProviderVersion: 10.0.0-25
  spec:
   containers:
     - name: omsagent 
       image: "microsoft/oms"
       imagePullPolicy: Always
       env:
       - name: WSID
         value: myWorkspaceID
       - name: KEY 
         value: myWorkspaceKey
       - name: DOMAIN
         value: opinsights.azure.com
       securityContext:
         privileged: true
       ports:
       - containerPort: 25225
         protocol: TCP 
       - containerPort: 25224
         protocol: UDP
       volumeMounts:
        - mountPath: /var/run/docker.sock
          name: docker-sock
        - mountPath: /var/log 
          name: host-log
       livenessProbe:
        exec:
         command:
         - /bin/bash
         - -c
         - ps -ef | grep omsagent | grep -v "grep"
        initialDelaySeconds: 60
        periodSeconds: 60
   volumes:
    - name: docker-sock 
      hostPath:
       path: /var/run/docker.sock
    - name: host-log
      hostPath:
       path: /var/log
```

Erstellen Sie das DaemonSet mit dem folgenden Befehl:

```azurecli-interactive
kubectl create -f oms-daemonset.yaml
```

Führen Sie den folgenden Befehl aus, um zu überprüfen, ob das DaemonSet erstellt wurde:

```azurecli-interactive
kubectl get daemonset
```

Die Ausgabe sieht in etwa wie folgt aus:

```azurecli-interactive
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE-SELECTOR   AGE
omsagent   3         3         3         0            3           <none>          5m
```

Nachdem die Agents ausgeführt werden, dauert es einige Minuten, bis die Daten in der OMS erfasst und verarbeitet werden.

## <a name="access-monitoring-data"></a>Zugreifen auf Überwachungsdaten

Sie können die Überwachungsdaten der OMS-Container mit der [Containerlösung](../../log-analytics/log-analytics-containers.md) im OMS-Portal oder im Azure-Portal anzeigen und analysieren. 

Um die Containerlösung über das [OMS-Portal](https://mms.microsoft.com) zu installieren, wechseln Sie zu **Lösungskatalog**. Fügen Sie dann **Containerlösung** hinzu. Alternativ können Sie die Containerlösung über den [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft.containersoms?tab=Overview) hinzufügen.

Suchen Sie im OMS-Portal die Zusammenfassungskachel **Container** auf dem OMS-Dashboard. Klicken Sie auf die Kachel, um Details anzuzeigen, z.B. Containerereignisse, Fehler, Status, Imageinventar sowie CPU- und Arbeitsspeicherauslastung. Detaillierte Informationen erhalten Sie, wenn Sie auf eine Zeile auf einer Kachel klicken oder eine [Protokollsuche](../../log-analytics/log-analytics-log-searches.md) durchführen.

![Containerdashboard im OMS-Portal](./media/container-service-tutorial-kubernetes-monitor/oms-containers-dashboard.png)

Im Azure-Portal navigieren Sie zu **Log Analytics** und wählen den Namen Ihres Arbeitsbereichs aus. Klicken Sie zum Anzeigen der Zusammenfassungskachel **Container** auf **Lösungen** > **Container**. Klicken Sie auf die Kachel, um Details anzuzeigen.

Ausführliche Anweisungen zum Abfragen und Analysieren von Überwachungsdaten finden Sie in der [Dokumentation zu Azure Log Analytics](../../log-analytics/index.md).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Ihren Kubernetes-Cluster mit der OMS überwacht. Sie haben folgende Aufgaben ausgeführt:

> [!div class="checklist"]
> * Abrufen von OMS-Arbeitsbereichseinstellungen
> * Einrichten von OMS-Agents auf den Kubernetes-Knoten
> * Zugreifen auf Überwachungsinformationen im OMS-Portal oder Azure-Portal


Klicken Sie auf diesen Link, um vordefinierte Skriptbeispiele für Container Service anzuzeigen.

> [!div class="nextstepaction"]
> [Skriptbeispiele für Azure Container Service](cli-samples.md)

