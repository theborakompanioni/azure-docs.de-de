---
title: "Überwachen eines Azure-DC/OS-Clusters – Datadog | Microsoft-Dokumentation"
description: "Überwachen eines Azure-Containerdienstclusters mit Datadog. Verwenden Sie die DC/OS-Webbenutzeroberfläche, um die Datadog-Agents auf Ihrem Cluster bereitzustellen."
services: container-service
documentationcenter: 
author: sauryadas
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Container, DC/OS, Docker Swarm, Azure
ms.assetid: ed00635d-4569-4f87-ab63-e307b2690b42
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure
ms.date: 07/28/2016
ms.author: saudas
translationtype: Human Translation
ms.sourcegitcommit: 0aa9b3ae14f586fc79e6ebee898e794d526c19bd
ms.openlocfilehash: 8dfd53346b6198bf5ecab4b4b73bfed93fa46b18


---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-datadog"></a>Überwachen eines Azure Container Service-DC/OS-Clusters mit Datadog
In diesem Artikel stellen wir Datadog-Agents für alle Agent-Knoten in Ihrem Azure-Containerdienstcluster bereit. Für diese Konfiguration benötigen Sie ein Konto mit Datadog. 

## <a name="prerequisites"></a>Voraussetzungen
Sie müssen einen per Azure Container Service konfigurierten Cluster [bereitstellen](container-service-deployment.md) und eine [Verbindung damit herstellen](container-service-connect.md). Lernen Sie die [Marathon-Benutzeroberfläche](container-service-mesos-marathon-ui.md)kennen. Wechseln Sie zu [http://datadoghq.com](http://datadoghq.com) , um ein Datadog-Konto einzurichten. 

## <a name="datadog"></a>Datadog
Datadog ist ein Überwachungsdienst, der Überwachungsdaten von den Containern innerhalb Ihres Azure-Containerdienstclusters sammelt. Datadog hat ein „Docker Integration Dashboard“, das bestimmte Metriken in Ihrem Container anzeigt. Aus Ihren Containern gesammelte Metriken werden nach CPU, Arbeitsspeicher, Netzwerk und E/A organisiert. Datadog teilt Metriken in Container und Images auf. Ein Beispiel der Benutzeroberfläche für die CPU-Nutzung sehen Sie unten.

![Datadog-Benutzeroberfläche](./media/container-service-monitoring/datadog4.png)

## <a name="configure-a-datadog-deployment-with-marathon"></a>Konfigurieren einer Datadog-Bereitstellung mit Marathon
Diese Schritte verdeutlichen, wie Sie Datadog-Anwendungen für Ihren Cluster mit Marathon konfigurieren und bereitstellen. 

Greifen Sie über [http://localhost:80/](http://localhost:80/) auf die DC/OS-Benutzeroberfläche zu. Navigieren Sie in der DC/OS-Benutzeroberfläche zur Option „Universe“ unten links. Suchen Sie darin nach „Datadog“, und klicken Sie auf „Install“ (Installieren).

![Datadog-Paket innerhalb von DC/OS Universe](./media/container-service-monitoring/datadog1.png)

Zum Durchführen der Konfiguration benötigen Sie nun ein Datadog-Konto oder ein kostenloses Testkonto. Sobald Sie bei der Datadog-Website angemeldet sind, wechseln Sie auf der linken Seite zu „Integrations“ (Integrationen) > [APIs](https://app.datadoghq.com/account/settings#api). 

![Datadog-API-Schlüssel](./media/container-service-monitoring/datadog2.png)

Geben Sie als Nächstes Ihren API-Schlüssel in die Datadog-Konfiguration in DC/OS Universe ein. 

![Datadog-Konfiguration in DC/OS Universe](./media/container-service-monitoring/datadog3.png) 

In der obigen Konfiguration sind die Instanzen auf „10000000“ festgelegt, damit Datadog für jeden neu hinzugefügten Knoten des Clusters automatisch einen Agent bereitstellt. Dies ist eine vorläufige Lösung. Nachdem Sie das Paket installiert haben, gehen Sie zurück zur Website von Datadog, und suchen Sie [Dashboards](https://app.datadoghq.com/dash/list). Dort sehen Sie „Custom“ (benutzerdefiniert) und „Integration Dashboards“. Das [Docker-Dashboard](https://app.datadoghq.com/screen/integration/docker) weist alle Containermetriken auf, die Sie für die Überwachung des Clusters benötigen. 




<!--HONumber=Jan17_HO4-->


