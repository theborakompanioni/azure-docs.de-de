<properties
   pageTitle="Durchführen des Lastenausgleichs für Container in einem Azure Container Service-Cluster | Microsoft Azure"
   description="Es wird beschrieben, wie Sie den Lastenausgleich für Container in einem Azure Container Service-Cluster durchführen."
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
   ms.date="07/11/2016"
   ms.author="rogardle"/>


# <a name="load-balance-containers-in-an-azure-container-service-cluster"></a>Durchführen des Lastenausgleichs für Container in einem Azure Container Service-Cluster

In diesem Artikel wird beschrieben, wie Sie einen internen Lastenausgleich in einem per DC/OS verwalteten Azure Container Service mit Marathon-LB erstellen. So wird das horizontale Skalieren Ihrer Anwendungen ermöglicht. Außerdem können Sie die öffentlichen und privaten Agent-Cluster nutzen, indem Sie Ihre Lastenausgleichsmodule im öffentlichen Cluster und Ihre Anwendungscontainer im privaten Cluster anordnen.

## <a name="prerequisites"></a>Voraussetzungen

[Stellen Sie eine Instanz von Azure Container Service mit dem Orchestratortyp DC/OS bereit](container-service-deployment.md), und [stellen Sie sicher, dass der Client eine Verbindung mit Ihrem Cluster herstellen kann](container-service-connect.md). 

## <a name="load-balancing"></a>Lastenausgleich

Der Containerdienstcluster, den wir erstellen, enthält zwei Ebenen für den Lastenausgleich: 

  1. Azure Load Balancer verfügt über öffentliche Einstiegspunkte (für Endbenutzer). Diese werden vom Azure-Containerdienst automatisch bereitgestellt und standardmäßig so konfiguriert, dass die Ports 80, 443 und 8080 verfügbar gemacht werden.
  2. Der Marathon Load Balancer (marathon-lb) leitet eingehende Anforderungen an Containerinstanzen weiter, die diese Anforderungen verarbeiten. Wenn wir die Container für unseren Webdienst skalieren, wird „marathon-lb“ dynamisch angepasst. Dieses Lastenausgleichsmodul wird nicht standardmäßig im Containerdienst bereitgestellt, aber es ist sehr einfach zu installieren.

## <a name="marathon-load-balancer"></a>Marathon Load Balancer

Marathon Load Balancer konfiguriert sich basierend auf den von Ihnen bereitgestellten Containern selbst dynamisch neu. Außerdem kann er den Verlust eines Containers oder Agents überstehen. Wenn dies passiert, wird der Container von Apache Mesos einfach an einem anderen Ort neu gestartet, und „marathon-lb“ wird angepasst.

Zum Installieren von Marathon Load Balancer können Sie entweder die DC/OS-Webbenutzeroberfläche oder die Befehlszeile verwenden.

### <a name="install-marathon-lb-using-dc/os-web-ui"></a>Installieren von Marathon-LB über die DC/OS-Webbenutzeroberfläche

  1. Klicken Sie auf „Universe“.
  2. Suchen Sie nach „Marathon-LB“.
  3. Klicken Sie auf „Install“ (Installieren).

![„marathon-lb“ über die DC/OS-Webschnittstelle installieren](./media/dcos/marathon-lb-install.png)

### <a name="install-marathon-lb-using-the-dc/os-cli"></a>Installieren von Marathon-LB mit der DC/OS-Befehlszeilenschnittstelle

Nachdem Sie die DC/OS-Befehlszeilenschnittstelle installiert und sichergestellt haben, dass Sie eine Verbindung mit Ihrem Cluster herstellen können, können Sie auf Ihrem Clientcomputer den folgenden Befehl ausführen:

```bash
dcos package install marathon-lb
```

Mit diesem Befehl wird der Lastenausgleich automatisch im öffentlichen Agent-Cluster installiert.

## <a name="deploy-a-load-balanced-web-application"></a>Bereitstellen einer Webanwendung mit Lastenausgleich

Da wir jetzt über das „marathon-lb“-Paket verfügen, können wir einen Anwendungscontainer bereitstellen, für den der Lastenausgleich durchgeführt werden soll. In diesem Beispiel stellen wir einen einfachen Webserver bereit, indem wir die folgende Konfiguration verwenden:

```json
{
  "id": "web",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "yeasy/simple-web",
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

  * Legen Sie den Wert von `HAProxy_0_VHOST` auf den FQDN des Lastenausgleichs für Ihre Agents fest. Dieser hat das folgende Format: `<acsName>agents.<region>.cloudapp.azure.com`. Wenn Sie beispielsweise einen Containerdienstcluster mit dem Namen `myacs` in der Region `West US` erstellen, lautet der FQDN wie folgt: `myacsagents.westus.cloudapp.azure.com`. Sie können hierauf auch zugreifen, indem Sie nach dem Lastenausgleichsmodul mit „agent“ im Namen suchen, wenn Sie sich die Ressourcen in der Ressourcengruppe ansehen, die Sie für den Containerdienst im [Azure-Portal](https://portal.azure.com)erstellt haben.
  * Legen Sie „servicePort“ auf einen Port mit einer Portnummer über 10.000 fest. So wird der Dienst identifiziert, der in diesem Container ausgeführt wird. Von „marathon-lb“ wird dieses Verfahren verwendet, um Dienste zu identifizieren, für die der Ausgleich durchgeführt werden soll.
  * Legen Sie die Bezeichnung `HAPROXY_GROUP` auf „external“ fest.
  * Legen Sie `hostPort` auf „0“ fest. Dies bedeutet, dass Marathon willkürlich einen verfügbaren Port zuordnet.
  * Legen Sie `instances` auf die Anzahl von Instanzen fest, die Sie erstellen möchten. Diese Anzahl können Sie später noch erhöhen oder verringern.

Beachten Sie, dass Marathon die Bereitstellung standardmäßig im privaten Cluster durchführt. Dies bedeutet, dass die obige Bereitstellung nur über den Lastenausgleich zugänglich ist. Dies ist in der Regel das erwünschte Verhalten.

### <a name="deploy-using-the-dc/os-web-ui"></a>Bereitstellen mit der DC/OS-Webbenutzeroberfläche

  1. Besuchen Sie die Marathon-Seite unter „http://localhost/marathon“ (nach dem Einrichten des [SSH-Tunnels](container-service-connect.md), und klicken Sie auf `Create Appliction`.
  2. Klicken Sie im Dialogfeld `New Application` oben rechts auf `JSON Mode`.
  3. Fügen Sie den obigen JSON-Code in den Editor ein.
  4. Klicken Sie auf `Create Appliction`.

### <a name="deploy-using-the-dc/os-cli"></a>Bereitstellen mit der DC/OS-Befehlszeilenschnittstelle

Um die Anwendung mit der DC/OS-Befehlszeilenschnittstelle bereitzustellen, kopieren Sie einfach den obigen JSON-Code in eine Datei mit dem Namen `hello-web.json` und führen Folgendes aus:

```bash
dcos marathon app add hello-web.json
```

## <a name="azure-load-balancer"></a>Azure-Lastenausgleich

Azure Load Balancer macht standardmäßig die Ports 80, 8080 und 443 verfügbar. Bei Verwendung von einem dieser drei Ports (wie im obigen Beispiel) müssen Sie nichts weiter unternehmen. Der FQDN Ihres Agent-Lastenausgleichs sollte auf Anhieb gefunden werden. Bei jeder von Ihnen durchgeführten Aktualisierung wird rollierend jeweils ein anderer Ihrer drei Webserver gefunden. Wenn Sie einen anderen Port verwenden, müssen Sie dagegen eine Roundrobin-Regel hinzufügen und für den jeweiligen Port einen Test für den Load Balancer hinzufügen. Hierfür können Sie die [Azure-Befehlszeilenschnittstelle](../xplat-cli-azure-resource-manager.md) mit den Befehlen `azure network lb rule create` und `azure network lb probe create` verwenden. Außerdem können Sie hierfür das Azure-Portal verwenden.


## <a name="additional-scenarios"></a>Zusätzliche Szenarien

Es kann vorkommen, dass Sie unterschiedliche Domänen nutzen, um unterschiedliche Dienste verfügbar zu machen. Beispiel:

mydomain1.com -> Azure LB:80 -> marathon-lb:10001 -> mycontainer1:33292  
mydomain2.com -> Azure LB:80 -> marathon-lb:10002 -> mycontainer2:22321

Informieren Sie sich hierfür über [virtuelle Hosts](https://mesosphere.com/blog/2015/12/04/dcos-marathon-lb/). Sie stellen eine Möglichkeit dar, Domänen bestimmten marathon-lb-Pfaden zuzuordnen.

Alternativ dazu können Sie verschiedene Ports verfügbar machen und im Hintergrund von „marathon-lb“ neu dem richtigen Dienst zuordnen. Beispiel:

Azure lb:80 -> marathon-lb:10001 -> mycontainer:233423  
Azure lb:8080 -> marathon-lb:1002 -> mycontainer2:33432


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [marathon-lb](https://dcos.io/docs/1.7/usage/service-discovery/marathon-lb/)finden Sie in der DC/OS-Dokumentation.



<!--HONumber=Oct16_HO2-->


