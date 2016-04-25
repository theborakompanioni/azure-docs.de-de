<properties
   pageTitle="Einführung in Azure Container Service | Microsoft Azure"
   description="Azure Container Service bietet eine Möglichkeit zur Vereinfachung der Erstellung, Konfiguration und Verwaltung eines Clusters virtueller Computer, die für die Ausführung von Anwendungen in Containern vorkonfiguriert sind."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, Container, Microservices, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/16/2016"
   ms.author="rogardle"/>

# Einführung in Azure Container Service

Azure Container Service (ACS) vereinfacht Ihnen das Erstellen, Konfigurieren und Verwalten eines Clusters virtueller Computer, die für die Ausführung von Anwendungen in Containern vorkonfiguriert sind. Der Dienst verwendet eine optimierte Konfiguration gängiger Tools für Open-Source-Planung und Orchestrierung. So können Sie Ihre vorhandenen Kenntnisse nutzen, bzw. auf einen großen und wachsenden Pool von Communityfachkenntnissen zur Bereitstellung und Verwaltung von containerbasierten Anwendungen in Microsoft Azure zurückgreifen.

<br /> ![Azure Container Service bietet eine Möglichkeit, Anwendungen, die in Containern ausgeführt werden, auf mehreren Hosts in Azure zu verwalten.](./media/acs-intro/acs-cluster.png) <br /><br />

Azure Container Service nutzt das Docker-Containerformat, um sicherzustellen, dass Ihre Anwendungscontainer vollständig portierbar sind. Der Dienst unterstützt außerdem Marathon und Apache Mesos oder Docker Swarm, sodass Sie diese Anwendungen auf Tausende und sogar Zehntausende von Containern skalieren können.

Mit dem Azure Container Service können Sie die Unternehmensfunktionen von Azure nutzen, während gleichzeitig die Anwendungsportierbarkeit erhalten bleibt, auch auf den Orchestrierungsebenen.

Verwenden von Azure Container Service
-----------------------------

Mit dem Azure Container Service verfolgen wir das Ziel, mit Open Source-Tools und -Technologien, die heutzutage bei unseren Kunden beliebt sind, eine Umgebung für das Containerhosting bereitzustellen. Zu diesem Zweck machen wir die Standard-API-Endpunkte für den von Ihnen ausgewählten Orchestrator verfügbar. Mithilfe dieser Endpunkte können Sie jede Software nutzen, die mit diesen Endpunkten kommunizieren kann. Im Fall des Docker Swarm-Endpunkts können Sie z. B. die Docker-Befehlszeilenschnittstelle (Command-Line Interface, CLI) verwenden. Für Apache Mesos könnten Sie die DCOS-CLI verwenden.

Erstellen eines Docker-Clusters mit Azure Container Service
-------------------------------------------------------

Um mit der Nutzung von Azure Container Service (ACS) zu beginnen, können Sie einen Azure Container Service-Cluster mithilfe einer Azure Resource Manager-Vorlage bereitstellen. Sie können diese Bereitstellung mit Apache Mesos oder Docker Swarm mit verschiedenen Größen- und Verfügbarkeitsoptionen konfigurieren. Azure Resource Manager-Vorlagen können Sie über das Azure-Portal mittels Azure-Befehlszeilenschnittstelle oder PowerShell bereitstellen. Die Vorlagen können auch so geändert werden, dass sie eine zusätzliche oder erweiterte Azure-Konfiguration enthalten. Weitere Informationen zur Bereitstellung eines Azure Container Service-Clusters finden Sie unter [Bereitstellen eines Azure Container Service-Clusters](./container-service-deployment.md).

Bereitstellen einer Anwendung
------------------------

Während der Vorschauphase bieten wir eine Auswahl zwischen Docker Swarm und Apache Mesos (mit DCOS Marathon- und DCOS Chronos-Frameworks) als Orchestrierung.

### Verwenden von Apache Mesos

Apache Mesos ist ein Open Source-Projekt, das bei der Apache Software Foundation beheimatet ist. Es führt einige der [größten Namen der IT-Branche](http://mesos.apache.org/documentation/latest/powered-by-mesos/) als Benutzer und Mitwirkende auf.

![Für Swarm konfigurierter Azure Container Service mit angezeigten Agents und Mastern.](media/acs-intro/acs-mesos.png)

Mesos zeichnet sich durch einen beeindruckenden Funktionsumfang aus.

-   Skalierbarkeit auf Zehntausende von Knoten

-   Fehlertolerant replizierte Master und Slaves mit Apache ZooKeeper

-   Unterstützung von Containern im Docker-Format

-   Native Isolierung zwischen Aufgaben mit Linux-Containern

-   Zeitplanung von mehreren Ressourcen (Arbeitsspeicher, CPU, Datenträger und Ports)

-   Java-, Python- und C++-APIs zur Entwicklung neuer paralleler Anwendungen

-   Eine Webbenutzeroberfläche zum Anzeigen von Clusterzuständen

Mesos bietet Unterstützung für eine große Anzahl von [Frameworks](http://mesos.apache.org/documentation/latest/frameworks/), die Sie für die Planung von Workloads im Azure Container Service verwenden können. Standardmäßig enthält Azure Container Service die Marathon- und Chronos-Frameworks.

#### Verwenden von Marathon und Chronos

Marathon ist ein clusterweites Initialisierungs- und Steuerungssystem für Dienste in cgroups – oder, im Fall von Azure Container Service, in Containern im Docker-Format. Es ist ein idealer Partner für Chronos, eines fehlertoleranten Auftragsplaners für Mesos, der Abhängigkeiten und zeitbasierte Pläne verarbeitet.

Marathon und Chronos bieten eine Webbenutzeroberfläche, über die Sie Ihre Anwendungen bereitstellen können. Sie greifen über eine URL darauf zu, die etwa wie `http://DNS\_PREFIX.REGION.cloudapp.azure.com` aussieht, wobei sowohl DNS\_PREFIX als auch REGION zur Zeit der Bereitstellung definiert werden. Natürlich können Sie auch Ihren eigenen DNS-Namen angeben. Weitere Informationen zum Ausführen eines Containers auf der Marathon-Webbenutzeroberfläche finden Sie unter [Containerverwaltung über die Webbenutzeroberfläche](./container-service-mesos-marathon-ui.md).

Sie können auch die REST-APIs für die Kommunikation mit Marathon und Chronos verwenden. Es gibt eine Reihe von Clientbibliotheken, die für jedes Tool verfügbar sind. Sie decken zahlreiche Sprachen ab – und natürlich können Sie das HTTP-Protokoll in einer beliebigen Sprache verwenden. Darüber hinaus bieten viele beliebte DevOps-Tools Unterstützung für diese Planer. Dies bietet maximale Flexibilität für Ihr Arbeitsteam, wenn es in einem Azure Container Service-Cluster arbeitet. Weitere Informationen zum Ausführen eines Containers mithilfe der Marathon-REST-API finden Sie unter [Containerverwaltung über die REST-API](./container-service-mesos-marathon-rest.md).

### Verwenden von Docker Swarm

Docker Swarm bietet eine systemeigene Clusterfunktion für Docker. Da Docker die Standard-API von Docker Swarm bedient, kann jedes Tool, das bereits mit einem Docker-Daemon kommuniziert, Swarm verwenden, um transparent auf mehreren Hosts in Azure Container Service zu skalieren.

![Für die Verwendung von Apache Mesos konfigurierter Azure Container Service – Anzeige von Jumpbox, Agents und Mastern.](media/acs-intro/acs-swarm.png)

Unterstützte Tools für die Verwaltung von Containern auf einem Swarm-Cluster sind u. a. folgende:

-   Dokku

-   Docker CLI und Docker Compose

-   Krane

-   Jenkins

Videos
------
AzureCon-Ankündigung:

> [AZURE.VIDEO azurecon-2015-deep-dive-on-the-azure-container-service-with-mesos]  

Erste Schritte mit dem Azure Container Service:

> [AZURE.VIDEO connect-2015-getting-started-developing-with-docker-and-azure-container-service]

<!---HONumber=AcomDC_0413_2016-->