---
title: "Öffentliche und private DC/OS-Agent-Pools – ACS | Microsoft-Dokumentation"
description: "Funktionsweise öffentlicher und privater Agent-Pools mit einem Azure Container Service-Cluster"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Container, Microservices, Mesos, Azure
ms.assetid: 36d657c9-8845-4bf7-bed2-088323b67406
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2016
ms.author: danlep
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: a6d9ab6d95de936e4f1d28d47d4e1d74c080bdae


---
# <a name="dcos-agent-pools-for-azure-container-service"></a>DC/OS-Agent-Pools für Azure Container Service
Azure Container Service für DC/OS unterteilt Agents in öffentliche und private Pools. Eine Bereitstellung ist in jedem Pool möglich, der Pool hat jedoch Auswirkungen auf den Zugriff zwischen Computern in Ihrem Containerdienst. Die Computer können über Internetzugriff (öffentlich) verfügen oder nur intern (privat) verwendet werden. Dieser Artikel geht kurz darauf ein, warum ein öffentlicher und ein privater Pool zur Verfügung stehen.

### <a name="private-agents"></a>Private Agents
Private Agentknoten werden über ein nicht routingfähiges Netzwerk ausgeführt. Über dieses Netzwerk kann nur von der Administratorzone oder über den Edgerouter der öffentlichen Zone zugegriffen werden. Standardmäßig startet DC/OS Apps auf privaten Agentknoten. Weitere Informationen zur Netzwerksicherheit finden Sie in der [DC/OS-Dokumentation](https://dcos.io/docs/1.7/administration/securing-your-cluster/) .

### <a name="public-agents"></a>Öffentliche Agents
Öffentliche Agent-Knoten führen DC/OS-Apps und -Dienste über ein öffentlich zugängliches Netzwerk aus. Weitere Informationen zur Netzwerksicherheit finden Sie in der [DC/OS-Dokumentation](https://dcos.io/docs/1.7/administration/securing-your-cluster/) .

## <a name="using-agent-pools"></a>Verwenden von Agent-Pools
Standardmäßig stellt **Marathon** jede neue Anwendung auf den *privaten* Agentknoten bereit. Sie müssen die Anwendung bei der Anwendungserstellung explizit auf dem *öffentlichen* Knoten bereitstellen. Geben Sie dazu auf der Registerkarte **Optional** für den Wert **Accepted Resource Roles** den Text **slave_public** ein. Dieser Prozess ist [hier](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) und in der [DC/OS](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/)-Dokumentation beschrieben.

## <a name="next-steps"></a>Nächste Schritte
Erhalten Sie weitere Informationen zum [Verwalten Ihrer DC/OS-Container](container-service-mesos-marathon-ui.md).

Erfahren Sie, wie Sie [die von Azure bereitgestellte Firewall öffnen](container-service-enable-public-access.md) , um den Zugriff auf Ihre DC/OS-Container zu ermöglichen.




<!--HONumber=Nov16_HO3-->


