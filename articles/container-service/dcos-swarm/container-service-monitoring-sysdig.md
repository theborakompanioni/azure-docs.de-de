---
title: "Überwachen eines Azure Container Service-Clusters mit Sysdig | Microsoft Docs"
description: "Es wird beschrieben, wie Sie einen Azure Container Service-Cluster mit Sysdig überwachen."
services: container-service
documentationcenter: 
author: sauryadas
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Container, DC/OS, Azure
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2016
ms.author: saudas
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: bfd49ea68c597b109a2c6823b7a8115608fa26c3
ms.openlocfilehash: 9ee460b29deba598e648e72958b08e77015a5505
ms.contentlocale: de-de
ms.lasthandoff: 07/25/2017

---
# <a name="monitor-an-azure-container-service-cluster-with-sysdig"></a>Überwachen eines Azure Container Service-Clusters mit Sysdig
In diesem Artikel stellen wir Sysdig-Agents für alle Agent-Knoten im Azure Container Service-Cluster bereit. Für diese Konfiguration benötigen Sie ein Konto mit Sysdig. 

## <a name="prerequisites"></a>Voraussetzungen
Sie müssen einen per Azure Container Service konfigurierten Cluster [bereitstellen](container-service-deployment.md) und eine [Verbindung damit herstellen](../container-service-connect.md). Lernen Sie die [Marathon-Benutzeroberfläche](container-service-mesos-marathon-ui.md)kennen. Navigieren Sie zu [http://app.sysdigcloud.com](http://app.sysdigcloud.com) , um ein Sysdig-Cloudkonto einzurichten. 

## <a name="sysdig"></a>Sysdig
Sysdig ist ein Überwachungsdienst, mit dem Sie Container im Cluster überwachen können. Sysdig ist als Unterstützung bei der Problembehandlung bekannt, verfügt aber auch über grundlegende Überwachungsmetriken für CPU, Netzwerk, Arbeitsspeicher und E/A. Mit Sysdig können Sie leicht erkennen, welche Container am stärksten ausgelastet sind bzw. die größte Menge an Arbeitsspeicher und CPU verbrauchen. Diese Ansicht ist im Abschnitt „Übersicht“ enthalten, der sich derzeit im Betastatus befindet. 

![Sysdig-Benutzeroberfläche](./media/container-service-monitoring-sysdig/sysdig6.png) 

## <a name="configure-a-sysdig-deployment-with-marathon"></a>Konfigurieren einer Sysdig-Bereitstellung mit Marathon
Diese Schritte verdeutlichen, wie Sie Sysdig-Anwendungen für Ihren Cluster mit Marathon konfigurieren und bereitstellen. 

Greifen Sie über [http://localhost:80/](http://localhost:80/) auf Ihre DC/OS-Benutzeroberfläche zu, und navigieren Sie zur Option „Universe“ unten links. Suchen Sie darin nach „Sysdig“.

![Sysdig in DC/OS Universe](./media/container-service-monitoring-sysdig/sysdig1.png)

Zum Durchführen der Konfiguration benötigen Sie nun ein Sysdig-Cloudkonto oder ein kostenloses Testkonto. Klicken Sie nach dem Anmelden an der Sysdig-Cloudwebsite auf Ihren Benutzernamen. Auf der Seite sollte Ihr „Zugriffsschlüssel“ angezeigt werden. 

![Sysdig-API-Schlüssel](./media/container-service-monitoring-sysdig/sysdig2.png) 

Geben Sie als Nächstes den Zugriffsschlüssel in die Sysdig-Konfiguration in DC/OS Universe ein. 

![Sysdig-Konfiguration in DC/OS Universe](./media/container-service-monitoring-sysdig/sysdig3.png)

Legen Sie die Instanzen auf „10000000“ fest, damit Sysdig für jeden neu hinzugefügten Knoten des Clusters automatisch einen Agent bereitstellt. Dies ist eine provisorische Lösung, um sicherzustellen, dass Sysdig für alle neuen Agents im Cluster bereitgestellt wird. 

![Sysdig-Konfiguration in DC/OS Universe-Instanzen](./media/container-service-monitoring-sysdig/sysdig4.png)

Navigieren Sie nach dem Installieren des Pakets zurück zur Sysdig-Benutzeroberfläche. Sie können darin die unterschiedlichen Nutzungsmetriken für die Container im Cluster untersuchen. 

Sie können auch Dashboards für Mesos und Marathon installieren, indem Sie den [neuen Dashboard-Assistenten](https://app.sysdigcloud.com/#/dashboards/new) verwenden.

