---
title: "Überwachen eines Azure-Kubernetes-Clusters – Operations Management | Microsoft-Dokumentation"
description: "Überwachen eines Kubernetes-Clusters in Azure Container Service mithilfe von Microsoft Operations Management Suite"
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
ms.sourcegitcommit: 4e4a4f4e299dc2747eb48bbd2e064cd80783211c
ms.openlocfilehash: 46240f3dc99a8c8a103a1e7919ad4f5e7a8ea62a
ms.lasthandoff: 04/04/2017


---

# <a name="monitor-an-azure-container-service-cluster-with-microsoft-operations-management-suite-oms"></a>Überwachen eines Azure Container Service-Clusters mit Microsoft Operations Management Suite (OMS)

## <a name="prerequisites"></a>Voraussetzungen
In dieser exemplarischen Vorgehensweise wird davon ausgegangen, dass Sie mit [Azure Container Service einen Kubernetes-Cluster erstellt haben](container-service-kubernetes-walkthrough.md).

Außerdem wird angenommen, dass die Tools `az` (Azure-Befehlszeilenschnittstelle) und `kubectl` installiert sind.

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

## <a name="monitoring-containers-with-operations-management-suite-oms"></a>Überwachen von Containern mit Operations Management Suite (OMS)

Microsoft Operations Management (OMS) ist die cloudbasierte IT-Verwaltungslösung von Microsoft, die Sie beim Verwalten und Schützen Ihrer Infrastruktur lokal und in der Cloud unterstützt. Die Containerlösung ist eine Lösung in OMS Log Analytics, mit der Sie das Inventar, die Leistung und die Protokolle von Containern an einer zentralen Stelle anzeigen können. Sie können Container überwachen und Problembehebungen durchführen, indem Sie die Protokolle an einer zentralen Stelle anzeigen und Container mit zu hoher Aktivität und zu hohem Ressourcenverbrauch auf einem Host suchen.

![](media/container-service-monitoring-oms/image1.png)

Weitere Informationen zur Containerlösung finden Sie unter [Log Analytics-Containerlösung](../log-analytics/log-analytics-containers.md).

## <a name="installing-oms-on-kubernetes"></a>Installieren von OMS für Kubernetes

### <a name="obtain-your-workspace-id-and-key"></a>Abrufen von Arbeitsbereichs-ID und -Schlüssel
Damit der OMS-Agent mit dem Dienst kommunizieren kann, muss er mit einer Arbeitsbereichs-ID und einem Arbeitsbereichsschlüssel konfiguriert werden. Um die Arbeitsbereichs-ID und den Schlüssel zu erhalten, müssen Sie unter <https://mms.microsoft.com> ein OMS-Konto erstellen.
Führen Sie die entsprechenden Schritte aus, um ein Konto zu erstellen. Nachdem Sie das Konto erstellt haben, müssen Sie Ihre ID und Ihren Schlüssel abrufen. Klicken Sie hierzu wie unten gezeigt auf **Einstellungen** > **Verbundene Quellen** > **Linux-Server**.

 ![](media/container-service-monitoring-oms/image5.png)

### <a name="install-the-oms-agent-using-a-daemonset"></a>Installieren des OMS-Agents mithilfe eines DaemonSets
DaemonSets werden von Kubernetes verwendet, um auf jedem Host im Cluster eine einzelne Instanz eines Containers auszuführen.
Sie eignen sich perfekt zum Ausführen von Überwachungs-Agents.

Im Anschluss sehen Sie die [DaemonSet-YAML-Datei](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes). Speichern Sie sie in einer Datei namens `oms-daemonset.yaml`, und ersetzen Sie die Platzhalterwerte für `WSID` und `KEY` durch Ihre Arbeitsbereichs-ID und den dazugehörigen Schlüssel.

Nachdem Sie der DaemonSet-Konfiguration Ihre Arbeitsbereichs-ID und den Schlüssel hinzugefügt haben, können Sie den OMS-Agent mithilfe des `kubectl`-Befehlszeilentools in Ihrem Cluster installieren:

```console
$ kubectl create -f oms-daemonset.yaml
```

### <a name="conclusion"></a>Zusammenfassung
Das ist alles! Nach wenigen Minuten sollten Daten bei Ihrem OMS-Dashboard eingehen.

