<properties
   pageTitle="Öffentliche und private Agent-Pools – ACS | Microsoft Azure"
   description="Funktionsweise öffentlicher und privater Agent-Pools mit einem Azure Container Service-Cluster"
   services="container-service"
   documentationCenter=""
   authors="Thraka"
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
   ms.date="08/16/2016"
   ms.author="adegeo"/>

# Agent-Pools

Azure Container Service unterteilt Agents in mehrere Pools. Während jeder virtuelle Computer in einem Pool identisch ist, hat jeder Pool sein eigenes Subnetz. Daher kann jeder Pool entweder als öffentlich oder privat gekennzeichnet werden, was den Zugriff zwischen den Computern in Ihrem Containerdienst beeinflusst.

### Private Agents

Private Agentknoten werden über ein nicht routingfähiges Netzwerk ausgeführt. Über dieses Netzwerk kann nur von der Administratorzone oder über den Edgerouter der öffentlichen Zone zugegriffen werden. Standardmäßig startet DC/OS Apps auf privaten Agentknoten. Weitere Informationen zur Netzwerksicherheit finden Sie in der [DC/OS-Dokumentation](https://dcos.io/docs/1.7/administration/securing-your-cluster/).

### Öffentliche Agents

Öffentliche Agent-Knoten führen DC/OS-Apps und -Dienste über ein öffentlich zugängliches Netzwerk aus. Weitere Informationen zur Netzwerksicherheit finden Sie in der [DC/OS-Dokumentation](https://dcos.io/docs/1.7/administration/securing-your-cluster/).

## Verwenden von Agent-Pools

Standardmäßig stellt **Marathon** jede neue Anwendung auf den *privaten* Agentknoten bereit. Sie müssen die Anwendung explizit auf den *öffentlichen* Knoten bereitstellen. Leider ist das Bereitstellen auf dem öffentlichen Knoten nicht ganz so unkompliziert wie z.B. das Aktivieren eines Kontrollkästchens. Wechseln Sie beim Erstellen einer neuen Anwendung zur Registerkarte **Optional**, und geben Sie unter **Accepted Resource Roles** (Akzeptierte Ressourcenrollen) den Text **slave\_public** ein. Dieser Prozess ist [hier](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) und in der [DC/OS-Dokumentation](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/) beschrieben.

## Nächste Schritte

Erhalten Sie weitere Informationen zum [Verwalten Ihrer DC/OS-Container](container-service-mesos-marathon-ui.md).

<!---HONumber=AcomDC_0824_2016-->