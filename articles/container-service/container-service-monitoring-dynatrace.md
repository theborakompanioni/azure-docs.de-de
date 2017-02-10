---
title: "Überwachen eines Azure Container Service-DC/OS-Clusters mit Dynatrace | Microsoft-Dokumentation"
description: "Überwachen eines Azure Container Service-DC/OS-Clusters mit Dynatrace. Bereitstellen von Dynatrace OneAgent mithilfe des DC/OS-Dashboards."
services: container-service
documentationcenter: 
author: MartinGoodwell
manager: 
editor: 
tags: acs, azure-container-service
keywords: Container, DC/OS, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/13/2016
ms.author: rogardle
translationtype: Human Translation
ms.sourcegitcommit: 96d35fbc4a8dfb620a2cfe67df449134d2f1e542
ms.openlocfilehash: 48bb16972f2e846997e081e702ffbce57736c484


---
# <a name="monitor-an-azure-container-service-cluster-with-dynatrace-saasmanaged"></a>Überwachen eines Azure Container Service-Clusters mit Dynatrace SaaS/Managed
In diesem Artikel erfahren Sie, wie Sie [Dynatrace](https://www.dynatrace.com/) OneAgent zum Überwachen aller Agentknoten in Ihrem Azure Container Service-Cluster bereitstellen. Für diese Konfiguration benötigen Sie ein Konto mit Dynatrace SaaS/Managed. 

## <a name="dynatrace-saasmanaged"></a>Dynatrace SaaS/Managed
Dynatrace ist eine cloudnative Überwachungslösung für hochdynamische Container- und Clusterumgebungen. Sie ermöglicht es Ihnen, Ihre Containerbereitstellungen und Speicherbelegungen mithilfe von Echtzeitnutzungsdaten zusätzlich zu optimieren. Die Lösung kann durch automatisierte(s) Baselining, Problemkorrelation und Erkennung der Grundursache Anwendungs- und Infrastrukturprobleme automatisch ermitteln.

Die folgende Abbildung zeigt die Benutzeroberfläche von Dynatrace:

![Benutzeroberfläche von Dynatrace](./media/container-service-monitoring-dynatrace/dynatrace.png)

## <a name="prerequisites"></a>Voraussetzungen 
Sie müssen einen per Azure Container Service konfigurierten Cluster [bereitstellen](./container-service-deployment.md) und eine [Verbindung damit herstellen](./container-service-connect.md). Lernen Sie die [Marathon-Benutzeroberfläche](./container-service-mesos-marathon-ui.md)kennen. Wechseln Sie zu [https://www.dynatrace.com/trial/](https://www.dynatrace.com/trial/), um ein Dynatrace SaaS-Konto einzurichten.  

## <a name="configure-a-dynatrace-deployment-with-marathon"></a>Konfigurieren einer Dynatrace-Bereitstellung mit Marathon
Im Folgenden wird erläutert, wie Sie Dynatrace-Anwendungen für Ihren Cluster mit Marathon konfigurieren und bereitstellen.

1. Greifen Sie über [http://localhost:80/](http://localhost:80/) auf die DC/OS-Benutzeroberfläche zu. Navigieren Sie in der DC/OS-Benutzeroberfläche zur Registerkarte **Universe**, und suchen Sie nach **Dynatrace**.

    ![Dynatrace in DC/OS Universe](./media/container-service-monitoring-dynatrace/dynatrace-universe.png)

2. Zum Durchführen der Konfiguration benötigen Sie ein Dynatrace SaaS-Konto oder ein kostenloses Testkonto. Melden Sie sich beim Dynatrace-Dashboard an, und wählen Sie **Deploy Dynatrace** (Dynatrace bereitstellen) aus.

    ![Einrichten der PaaS-Integration in Dynatrace](./media/container-service-monitoring-dynatrace/setup-paas.png)

3. Wählen Sie auf der Seite die Option **Set up PaaS integration** (PaaS-Integration einrichten) aus. 

    ![Dynatrace-API-Token](./media/container-service-monitoring-dynatrace/api-token.png) 

4. Geben Sie Ihr API-Token in die Dynatrace OneAgent-Konfiguration in DC/OS Universe ein. 

    ![Dynatrace OneAgent-Konfiguration in DC/OS Universe](./media/container-service-monitoring-dynatrace/dynatrace-config.png)

5. Legen Sie die Instanzen auf die Anzahl von Knoten fest, die Sie ausführen möchten. Sie können auch eine höhere Zahl festlegen, DC/OS sucht jedoch weiter nach neuen Instanzen, bis diese Zahl tatsächlich erreicht wird. Falls Sie möchten, können Sie auch einen Wert wie 1000000 festlegen. Wenn dem Cluster ein neuer Knoten hinzugefügt wird, stellt Dynatrace in diesem Fall automatisch einen Agent auf dem neuen Knoten bereit. Der Nachteil hierbei ist jedoch, dass DC/OS ständig versucht, weitere Instanzen bereitzustellen.

    ![Dynatrace-Konfiguration in den DC/OS Universe-Instanzen](./media/container-service-monitoring-dynatrace/dynatrace-config2.png)

## <a name="next-steps"></a>Nächste Schritte

Kehren Sie zum Dynatrace-Dashboard zurück, nachdem Sie das Paket installiert haben. Sie können nun die verschiedenen Nutzungsmetriken für die Container in Ihrem Cluster erkunden. 


<!--HONumber=Dec16_HO3-->


