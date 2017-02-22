---
title: Verwalten eines Azure-DC/OS-Clusters mit der Marathon-REST-API | Microsoft-Dokumentation
description: "Stellen Sie mithilfe der Marathon-REST-API Container für einen Azure Container Service-DC/OS-Cluster bereit."
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Container, Microservices, Mesos, Azure
ms.assetid: c7175446-4507-4a33-a7a2-63583e5996e3
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/01/2017
ms.author: danlep
translationtype: Human Translation
ms.sourcegitcommit: b2b969500d20d0c840f201ed2cf13a6f2ab38ee5
ms.openlocfilehash: 719f1ea6a6f51d4a787f0465a4bbadb1a6057a8b


---
# <a name="dcos-container-management-through-the-marathon-rest-api"></a>DC/OS-Containerverwaltung über die Marathon-REST-API
DC/OS stellt eine Umgebung für die Bereitstellung und Skalierung geclusterter Workloads bereit und abstrahiert die zugrunde liegende Hardware. Zusätzlich zu DC/OS ist auch ein Framework vorhanden, mit dem die Planung und Ausführung von Computeworkloads verwaltet wird. Es sind zwar Frameworks für viele gängige Workloads verfügbar, aber in diesem Dokument wird beschrieben, wie Sie Containerbereitstellungen mit Marathon erstellen und skalieren. 

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie diese Beispiele durcharbeiten, benötigen Sie einen DC/OS-Cluster, der im Azure Container Service konfiguriert ist. Sie müssen auch über Remoteverbindungen mit diesem Cluster verfügen. Weitere Informationen zu diesen Elementen finden Sie in den folgenden Artikeln:

* [Bereitstellen eines Azure Container Service-Clusters](container-service-deployment.md)
* [Verbinden mit einem Azure Container Service-Cluster](container-service-connect.md)

## <a name="access-the-dcos-apis"></a>Zugriff auf die DC/OS-APIs
Nachdem die Verbindung mit dem Azure Container Service-Cluster hergestellt wurde, können Sie auf DC/OS und die verwandten REST-APIs über „http://localhost:local-port“ zugreifen. Bei den Beispielen in diesem Dokument wird davon ausgegangen, dass das Tunneling über Port 80 erfolgt. Der Marathon-Endpunkt kann beispielsweise unter URIs erreicht werden, die mit `http://localhost/marathon/v2/` beginnen. 

Weitere Informationen zu den verschiedenen APIs finden Sie in der Mesosphere-Dokumentation für die [Marathon-API](https://mesosphere.github.io/marathon/docs/rest-api.html) und die [Chronos-API](https://mesos.github.io/chronos/docs/api.html) sowie in der Apache-Dokumentation für die [Mesos Scheduler-API](http://mesos.apache.org/documentation/latest/scheduler-http-api/).

## <a name="gather-information-from-dcos-and-marathon"></a>Erfassen von Informationen von DC/OS und Marathon
Erfassen Sie vor dem Bereitstellen von Containern im DC/OS-Cluster einige Informationen zum DC/OS-Cluster, z.B. die Namen und den aktuellen Status der DC/OS-Agents. Fragen Sie dazu den `master/slaves`-Endpunkt der DC/OS-REST-API ab. Wenn alles geklappt hat, gibt die Abfrage eine Liste mit DC/OS-Agents und jeweils einigen Eigenschaften zurück.

```bash
curl http://localhost/mesos/master/slaves
```

Verwenden Sie nun den Marathon-`/apps`-Endpunkt, um aktuelle Anwendungsbereitstellungen im DC/OS-Cluster zu überprüfen. Im Falle eines neuen Clusters wird ein leeres Array für Apps angezeigt.

```bash
curl localhost/marathon/v2/apps

{"apps":[]}
```

## <a name="deploy-a-docker-formatted-container"></a>Bereitstellen eines Containers im Docker-Format
Sie stellen Container im Docker-Format mit Marathon über eine JSON-Datei bereit, die die vorgesehene Bereitstellung beschreibt. Im folgenden Beispiel wird der Nginx-Container bereitgestellt. Dabei wird Port 80 des DC/OS-Agents an Port 80 des Containers gebunden. Beachten Sie, dass die Eigenschaft `acceptedResourceRoles` auf `slave_public` festgelegt ist. Damit wird der Container für einen Agent in der öffentlich zugänglichen Agent-Skalierungsgruppe bereitgestellt.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
    "acceptedResourceRoles": [
    "slave_public"
  ],
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Speichern Sie zum Bereitstellen eines Containers im Docker-Format die JSON-Datei an einem zugänglichen Speicherort. Führen Sie als Nächstes den folgenden Befehl aus, um den Container bereitzustellen. Geben Sie den Namen der JSON-Datei an (in diesem Beispiel: `marathon.json`).

```bash
curl -X POST http://localhost/marathon/v2/apps -d @marathon.json -H "Content-type: application/json"
```

Die Ausgabe sieht in etwa wie folgt aus:

```json
{"version":"2015-11-20T18:59:00.494Z","deploymentId":"b12f8a73-f56a-4eb1-9375-4ac026d6cdec"}
```

Wenn Sie jetzt Marathon auf Anwendungen abfragen, erscheint diese neue Anwendung in der Ausgabe.

```bash
curl localhost/marathon/v2/apps
```

Sie können überprüfen, ob Nginx ausgeführt wird, indem Sie eine HTTP-Anforderung an den vollqualifizierten Domänennamen des Agent-Pools senden (unter `http://<containerServiceName>agents.<region>.cloudapp.azure.com`).

## <a name="scale-your-containers"></a>Skalieren der Container
Sie können die Marathon-API verwenden, um Anwendungsbereitstellungen horizontal hoch- oder herunterzuskalieren. Im vorherigen Beispiel haben Sie eine Instanz einer Anwendung bereitgestellt. Wir führen hierfür jetzt das horizontale Hochskalieren auf drei Instanzen einer Anwendung durch. Hierzu erstellen Sie eine JSON-Datei, indem Sie den folgenden JSON-Text verwenden und an einem zugänglichen Speicherort speichern.

```json
{ "instances": 3 }
```

Führen Sie den folgenden Befehl aus, um die Anwendung horizontal hochzuskalieren.

> [!NOTE]
> Der URI lautet „http://localhost/marathon/v2/apps/“, gefolgt von der ID der zu skalierenden Anwendung. Wenn Sie das hier bereitgestellte Nginx-Beispiel verwenden, lautet der URI „http://localhost/marathon/v2/apps/nginx“.
> 
> 

```bash
curl http://localhost/marathon/v2/apps/nginx -H "Content-type: application/json" -X PUT -d @scale.json
```

Fragen Sie den Marathon-Endpunkt zum Schluss auf Anwendungen ab. Wie Sie sehen, sind nun drei Nginx-Container vorhanden.

```bash
curl localhost/marathon/v2/apps
```

## <a name="equivalent-powershell-commands"></a>Entsprechende PowerShell-Befehle
Sie können diese Aktionen auch durchführen, indem Sie PowerShell-Befehle in einem Windows-System verwenden.

Führen Sie den folgenden Befehl aus, um Informationen zum DC/OS-Cluster zu erfassen, z.B. Agent-Namen und -Status:

```powershell
Invoke-WebRequest -Uri http://localhost/mesos/master/slaves
```

Sie stellen Container im Docker-Format mit Marathon über eine JSON-Datei bereit, die die vorgesehene Bereitstellung beschreibt. Im folgenden Beispiel wird der Nginx-Container bereitgestellt. Dabei wird Port 80 des DC/OS-Agents an Port 80 des Containers gebunden.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Speichern Sie zum Bereitstellen eines Containers im Docker-Format die JSON-Datei an einem zugänglichen Speicherort. Führen Sie als Nächstes den folgenden Befehl aus, um den Container bereitzustellen. Geben Sie den Namen der JSON-Datei an (in diesem Beispiel: `marathon.json`).

```powershell
Invoke-WebRequest -Method Post -Uri http://localhost/marathon/v2/apps -ContentType application/json -InFile 'c:\marathon.json'
```

Sie können auch die Marathon-API verwenden, um Anwendungsbereitstellungen horizontal hoch- oder herunterzuskalieren. Im vorherigen Beispiel haben Sie eine Instanz einer Anwendung bereitgestellt. Wir führen hierfür jetzt das horizontale Hochskalieren auf drei Instanzen einer Anwendung durch. Hierzu erstellen Sie eine JSON-Datei, indem Sie den folgenden JSON-Text verwenden und an einem zugänglichen Speicherort speichern.

```json
{ "instances": 3 }
```

Führen Sie den folgenden Befehl aus, um die Anwendung horizontal hochzuskalieren:

> [!NOTE]
> Der URI lautet „http://localhost/marathon/v2/apps/“, gefolgt von der ID der zu skalierenden Anwendung. Wenn Sie das hier bereitgestellte Nginx-Beispiel verwenden, lautet der URI „http://localhost/marathon/v2/apps/nginx“.
> 
> 

```powershell
Invoke-WebRequest -Method Put -Uri http://localhost/marathon/v2/apps/nginx -ContentType application/json -InFile 'c:\scale.json'
```

## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich auf der Apache Mesos-Website ausführlicher über [HTTP-Endpunkte](http://mesos.apache.org/documentation/latest/endpoints/).
* Weitere Informationen zur [Marathon-REST-API](https://mesosphere.github.io/marathon/docs/rest-api.html)




<!--HONumber=Feb17_HO1-->


