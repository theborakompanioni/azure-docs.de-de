---
title: "Azure Container Service – häufig gestellte Fragen | Microsoft-Dokumentation"
description: "Hier finden Sie Antworten auf häufig gestellte Fragen zu Azure Container Service – einem Dienst zur Vereinfachung der Erstellung, Konfiguration und Verwaltung eines Clusters virtueller Computer für die Ausführung von Docker-Container-Apps."
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Container, Microservices, Mesos, Azure, Kubernetes
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/03/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: d9dad6cff80c1f6ac206e7fa3184ce037900fc6b
ms.openlocfilehash: c11adbe77131d4e0337ccd926da03041042c3703
ms.lasthandoff: 03/06/2017


---
# <a name="container-service-frequently-asked-questions"></a>Häufig gestellte Fragen zu Container Service


## <a name="orchestrators"></a>Orchestratoren

### <a name="which-container-orchestrators-do-you-support-on-azure-container-service"></a>Welche Containerorchestratoren werden für Azure Container Service unterstützt? 

Unterstützt werden DC/OS (Open Source), Docker Swarm und Kubernetes. Weitere Informationen finden Sie in der [Übersicht](container-service-intro.md).
 
### <a name="do-you-support-docker-swarm-mode"></a>Wird der Docker Swarm-Modus unterstützt? 

Der Swarm-Modus wird nicht unterstützt, die Unterstützung ist aber bereits in Planung. 

### <a name="does-azure-container-service-support-windows-containers"></a>Unterstützt Azure Container Service Windows-Container?  

Derzeit werden Linux-Container für alle Orchestratoren unterstützt. Die Unterstützung für Windows-Container mit Kubernetes befindet sich in der Vorschauphase.

### <a name="do-you-recommend-a-specific-orchestrator-in-azure-container-service"></a>Wird ein bestimmter Orchestrator in Azure Container Service empfohlen? 
Wir empfehlen im Allgemeinen keinen bestimmten Orchestrator. Falls Sie bereits mit einem der unterstützten Orchestratoren vertraut sind, können Sie sich Ihre Erfahrung in Azure Container Service zunutze machen. Datentrends legen jedoch nahe, dass DC/OS sich in der Produktion für Big Data- und IoT-Workloads bewährt hat, Kubernetes sich für native Cloudworkloads eignet und Docker Swarm sich durch seine Integration in Docker-Tools und seine einfache Lernkurve auszeichnet.

Abhängig von Ihrem Szenario können Sie auch benutzerdefinierte Containerlösungen mit anderen Azure-Diensten erstellen und verwalten. Zu diesen Diensten zählen [Virtual Machines](../virtual-machines/virtual-machines-linux-azure-overview.md), [Service Fabric](../service-fabric/service-fabric-overview.md), [Web-Apps](../app-service-web/app-service-web-overview.md) und [Batch](../batch/batch-technical-overview.md).  

### <a name="what-is-the-difference-between-azure-container-service-and-acs-engine"></a>Was ist der Unterschied zwischen Azure Container Service und ACS Engine? 
Azure Container Service ist ein durch eine SLA abgedeckter Azure-Dienst mit Features im Azure-Portal, Azure-Befehlszeilentools und Azure-APIs. Der Dienst ermöglicht die schnelle Implementierung und Verwaltung von Clustern unter Verwendung standardmäßiger Containerorchestrierungstools mit einer vergleichsweise geringen Anzahl von Konfigurationsoptionen. 

[ACS Engine](http://github.com/Azure/acs-engine) ist ein Open-Source-Projekt, mit dem Poweruser sämtliche Ebenen der Clusterkonfiguration anpassen können. Aufgrund der Möglichkeit, sowohl die Infrastruktur- als auch die Softwarekonfiguration zu ändern, bieten wir für ACS Engine keine SLA an. Der Support wird über das Open-Source-Projekt auf GitHub und nicht über offizielle Microsoft-Kanäle bereitgestellt. 

## <a name="cluster-management"></a>Clusterverwaltung

### <a name="how-do-i-create-ssh-keys-for-my-cluster"></a>Wie erstelle ich SSH-Schlüssel für meine Cluster?

Für die Authentifizierung bei den virtuellen Linux-Computern Ihres Clusters können Sie mithilfe der Standardtools Ihres Betriebssystems ein Paar aus einem öffentlichen und einem privaten SSH-RSA-Schlüssel erstellen. Eine entsprechende Anleitung finden Sie im Leitfaden für [OS X und Linux](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) bzw. im Leitfaden für [Windows](../virtual-machines/virtual-machines-linux-ssh-from-windows.md). 

Wenn Sie einen Containerdienstcluster mithilfe von [Azure CLI 2.0-Befehlen](container-service-create-acs-cluster-cli.md) bereitstellen, können SSH-Schlüssel automatisch für Ihren Cluster generiert werden.

### <a name="how-do-i-create-a-service-principal-for-my-kubernetes-cluster"></a>Wie erstelle ich einen Dienstprinzipal für meinen Kubernetes-Cluster?

Eine Azure Active Directory-Dienstprinzipal-ID und ein Kennwort werden auch für die Erstellung eines Kubernetes-Clusters in Azure Container Service benötigt. Weitere Informationen finden Sie im Artikel zum [Dienstprinzipal für einen Kubernetes-Cluster](container-service-kubernetes-service-principal.md).


Wenn Sie einen Kubernetes-Cluster mithilfe von [Azure CLI 2.0-Befehlen](container-service-create-acs-cluster-cli.md) bereitstellen, können Dienstprinzipal-Anmeldeinformationen automatisch für Ihren Cluster generiert werden.


### <a name="how-do-i-increase-the-number-of-masters-after-a-cluster-is-created"></a>Wie kann ich nach der Clustererstellung die Masteranzahl erhöhen? 
Nach der Erstellung des Clusters ist die Masteranzahl unveränderlich. Bei der Clustererstellung sollten Sie im Idealfall mehrere Master auswählen, um eine hohe Verfügbarkeit sicherzustellen.


### <a name="how-do-i-increase-the-number-of-agents-after-a-cluster-is-created"></a>Wie kann ich nach der Clustererstellung die Agent-Anzahl erhöhen? 
Die Agent-Anzahl im Cluster kann über das Azure-Portal oder mithilfe von Befehlszeilentools skaliert werden. Informationen hierzu finden Sie unter [Skalieren eines Azure Container Service-Clusters](container-service-scale.md).


### <a name="what-are-the-urls-of-my-masters-and-agents"></a>Wie lauten die URLs meiner Master und Agents? 
Die URLs von Clusterressourcen in Azure Container Service basieren auf dem von Ihnen angegebenen DNS-Namenspräfix sowie auf dem Namen der Azure-Region, die Sie für die Bereitstellung ausgewählt haben. Der vollständig qualifizierte Domänennamen (Fully Qualified Domain Name, FQDN) des Masterknotens hat beispielsweise das folgende Format:

``` 
DNSnamePrefix.AzureRegion.cloudapp.azure.net
```

Häufig verwendete URLs für Ihren Cluster finden Sie im Azure-Portal, im Azure-Ressourcen-Explorer und in anderen Azure-Tools.

### <a name="how-do-i-tell-which-orchestrator-version-is-running-in-my-cluster"></a>Woher weiß ich, welche Orchestratorversion in meinem Cluster ausgeführt wird?

* DC/OS: Siehe [Mesosphere-Dokumentation](https://support.mesosphere.com/hc/en-us/articles/207719793-How-to-get-the-DCOS-version-from-the-command-line-)
* Docker Swarm: Führen Sie `docker version` aus.
* Kubernetes: Führen Sie `kubectl version` aus.


### <a name="how-do-i-upgrade-the-orchestrator-after-deployment"></a>Wie aktualisiere ich den Orchestrator nach der Bereitstellung?

Derzeit stellt Azure Container Service keine Tools zum Aktualisieren der in Ihrem Cluster bereitgestellten Orchestratorversion zur Verfügung. Wenn Container Service eine höhere Version unterstützt, können Sie einen neuen Cluster bereitstellen. Eine andere Möglichkeit besteht darin, einen Cluster direkt mithilfe orchestratorspezifischer Tools (sofern verfügbar) zu aktualisieren. Ein Beispiel finden Sie im Artikel zu [DC/OS-Upgrades](https://dcos.io/docs/1.8/administration/upgrading/).
 
### <a name="where-do-i-find-the-ssh-connection-string-to-my-cluster"></a>Wo finde ich die SSH-Verbindungszeichenfolge für meinen Cluster?

Die Verbindungszeichenfolge können Sie im Azure-Portal oder mithilfe von Azure-Befehlszeilentools ermitteln. 

1. Navigieren Sie im Portal zur Ressourcengruppe für die Clusterbereitstellung.  

2. Klicken Sie auf **Übersicht** und anschließend unter **Zusammenfassung** auf den Link für **Bereitstellungen**. 

3. Klicken Sie auf dem Blatt **Bereitstellungsverlauf** auf die Bereitstellung, deren Name sich aus **microsoft-acs** und einem Bereitstellungsdatum zusammensetzt. Beispiel: microsoft-acs-201701310000.  

4. Auf der Seite **Zusammenfassung** werden unter **Ausgaben** mehrere Clusterlinks bereitgestellt<provided></provided>. **SSHMaster0** stellt eine SSH-Verbindungszeichenfolge für den ersten Master in Ihrem Containerdienstcluster bereit. 

Wie bereits erwähnt kann der FQDN des Masters auch mithilfe von Azure-Tools ermittelt werden. Stellen Sie eine SSH-Verbindung mit dem Master her, und verwenden Sie dabei den FQDN des Masters sowie den Benutzernamen, den Sie bei der Clustererstellung angegeben haben. Beispiel:

```bash
ssh userName@masterFQDN –A –p 22 
```

Weitere Informationen finden Sie unter [Verbinden mit einem Azure Container Service-Cluster](container-service-connect.md).




## <a name="next-steps"></a>Nächste Schritte

* [Informieren Sie sich ausführlicher](container-service-intro.md) über Azure Container Service.
* Stellen Sie einen Azure Container Service-Cluster über das [Portal](container-service-deployment.md) oder mithilfe von [Azure CLI 2.0](container-service-create-acs-cluster-cli.md) bereit.
