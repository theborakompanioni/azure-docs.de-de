<properties
   pageTitle="Durchführen des Lastenausgleichs für einen Azure Container Service-Cluster | Microsoft Azure"
   description="Es wird beschrieben, wie Sie den Lastenausgleich für einen Azure Container Service-Cluster durchführen."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Container, Microservices, DC/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/18/2016"
   ms.author="rogardle"/>

# Durchführen des Lastenausgleichs für einen Azure Container Service-Cluster

In diesem Artikel richten wir ein Web-Front-End ein, das zentral hochskaliert werden kann, um Dienste im Hintergrund des Azure-Lastenausgleichs bereitzustellen.


## Voraussetzungen

[Stellen Sie eine Instanz von Azure Container Service mit dem Orchestratortyp DCOS bereit](container-service-deployment.md), [stellen Sie sicher, dass der Client eine Verbindung mit Ihrem Cluster herstellen kann](container-service-connect.md), und[AZURE.INCLUDE [installieren Sie die DC/OS-CLI](../../includes/container-service-install-dcos-cli-include.md)].


## Lastenausgleich

In einem Container Service-Cluster gibt es zwei Lastenausgleichsebenen: Azure LB für die öffentlichen Einstiegspunkte (für Endbenutzer), und die zugrunde liegende Komponente „marathon-lb“, mit der eingehende Anforderungen zur Verarbeitung an Containerinstanzen weitergeleitet werden. Wenn wir die Container skalieren, mit denen der Dienst bereitgestellt wird, wird „marathon-lb“ dynamisch angepasst.

## Marathon LB 

Die Marathon LB-Lösung wird basierend auf den Containern, die Sie bereitgestellt haben, dynamisch neu konfiguriert. Außerdem kann sie den Verlust eines Containers oder Agents überstehen. Wenn dies passiert, wird der Container von Mesos einfach an einem anderen Ort neu gestartet, und Marathon LB wird neu konfiguriert.

Führen Sie zum Installieren von Marathon LB den folgenden Befehl auf Ihrem Clientcomputer aus:

```bash
dcos package install marathon-lb 
``` 

Da wir jetzt über das Paket „marathon-lb“ verfügen, können wir einen einfachen Webserver bereitstellen, indem wir die folgende Konfiguration verwenden:


```json
{ 
  "id": "web", 
  "container": { 
    "type": "DOCKER", 
    "docker": { 
      "image": "tutum/hello-world", 
      "network": "BRIDGE", 
      "portMappings": [ 
        { "hostPort": 0, "containerPort": 80, "servicePort": 10000 } 
      ], 
      "forcePullImage":true 
    } 
  }, 
  "instances": 3, 
  "cpus": 0.1, 
  "mem": 65, 
  "healthChecks": [{ 
      "protocol": "HTTP", 
      "path": "/", 
      "portIndex": 0, 
      "timeoutSeconds": 10, 
      "gracePeriodSeconds": 10, 
      "intervalSeconds": 2, 
      "maxConsecutiveFailures": 10 
  }], 
  "labels":{ 
    "HAPROXY_GROUP":"external",
    "HAPROXY_0_VHOST":"YOUR FQDN",
    "HAPROXY_0_MODE":"http" 
  } 
}

```

Wichtigste Bestandteile:
  * Legen Sie den Wert von HAProxy\_0\_VHOST auf den FQDN des Lastenausgleichs für Ihre Agents fest. Er hat folgendes Format: `<acsName>agents.<region>.cloudapp.azure.com`. Wenn ich beispielsweise einen Container Service-Cluster mit dem Namen `myacs` in der Region `West US` erstelle, lautet der FQDN wie folgt: `myacsagents.westus.cloudapp.azure.com`. Sie können hierauf auch zugreifen, indem Sie nach dem Lastenausgleichsmodul mit „agent“ im Namen suchen, wenn Sie sich die Ressourcen in der Ressourcengruppe ansehen, die Sie für den Containerdienst im [Azure-Portal](https://portal.azure.com) erstellt haben.
  * Legen Sie servicePort auf einen Port fest, für den Folgendes gilt: >= 10.000. So wird der Dienst identifiziert, der in diesem Container ausgeführt wird. Von marathon-lb wird dieses Verfahren verwendet, um Dienste zu identifizieren, für die der Ausgleich durchgeführt werden soll.
  * Legen Sie die Bezeichnung HAPROXY\_GROUP auf „external“ fest.
  * Legen Sie hostPort auf 0 fest. Dies bedeutet, dass marathon willkürlich einen verfügbaren Port zuordnet.

Kopieren Sie diesen JSON-Code in eine Datei mit dem Namen `hello-web.json`, und verwenden Sie sie, um einen Container bereitzustellen:

```bash
dcos marathon app add hello-web.json 
``` 

## Azure LB 

Standardmäßig werden von Azure LB die Ports 80, 8080 und 443 verfügbar gemacht. Bei Verwendung von einem dieser drei Ports (wie im obigen Beispiel) müssen Sie nichts weiter unternehmen: Sie sollten den FQDN Ihres Agent-Lastenausgleichs „treffen“ können, und bei jeder von Ihnen durchgeführten Aktualisierung ist jeweils einer Ihrer drei Webserver das Ziel (Roundrobin-Verfahren). Wenn Sie einen anderen Port verwenden, müssen Sie dagegen eine Roundrobin-Regel hinzufügen und für den jeweiligen Port einen Test für den Azure LB hinzufügen. Dies ist über die [Azure XPLAT-CLI](../xplat-cli-azure-resource-manager.md) mit den Befehlen `azure lb rule create` und `azure lb probe create` möglich.


## Zusätzliche Szenarien

Es kann vorkommen, dass Sie unterschiedliche Domänen nutzen, um unterschiedliche Dienste verfügbar zu machen. Zum Beispiel:

mydomain1.com -> Azure LB:80 -> marathon-lb:10001 -> mycontainer1:33292 mydomain2.com -> Azure LB:80 -> marathon-lb:10002 -> mycontainer2:22321

Informieren Sie sich hierfür über [virtuelle Hosts](https://mesosphere.com/blog/2015/12/04/dcos-marathon-lb/). Sie stellen eine Möglichkeit dar, Domänen bestimmten marathon-lb-Pfaden zuzuordnen.

Alternativ dazu können Sie verschiedene Ports verfügbar machen und im Hintergrund von marathon-lb neu dem richtigen Dienst zuordnen. Beispiel:

Azure lb:80 -> marathon-lb:10001 -> mycontainer:233423 Azure lb:8080 -> marathon-lb:1002 -> mycontainer2:33432
 

## Nächste Schritte

Informieren Sie sich unter [diesem Blogbeitrag](https://mesosphere.com/blog/2015/12/04/dcos-marathon-lb/) weiter über Marathon LB.

<!---HONumber=AcomDC_0525_2016-->