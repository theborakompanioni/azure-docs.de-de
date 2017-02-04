---
title: "Azure Container Service-Containerverwaltung über die REST-API | Microsoft Docs"
description: "Stellen Sie mithilfe der Marathon-REST-API Container für einen Azure Container Service-Mesos-Cluster bereit."
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Container, Microservices, Mesos, Azure
ms.assetid: c7175446-4507-4a33-a7a2-63583e5996e3
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/13/2016
ms.author: timlt
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: e8f1ad596d2b64380876a501ebcf127afdda9ccf


---
# <a name="container-management-through-the-rest-api"></a>Containerverwaltung über die REST-API
DC/OS stellt eine Umgebung für die Bereitstellung und Skalierung geclusterter Workloads bereit und abstrahiert die zugrunde liegende Hardware. Zusätzlich zu DC/OS ist auch ein Framework vorhanden, mit dem die Planung und Ausführung von Computeworkloads verwaltet wird.

Es sind zwar Frameworks für viele gängige Workloads verfügbar, aber in diesem Dokument wird beschrieben, wie Sie Containerbereitstellungen mit Marathon erstellen und skalieren. Bevor Sie diese Beispiele durcharbeiten, benötigen Sie einen DC/OS-Cluster, der im Azure Container Service konfiguriert ist. Sie müssen auch über Remoteverbindungen mit diesem Cluster verfügen. Weitere Informationen zu diesen Elementen finden Sie in den folgenden Artikeln:

* [Bereitstellen eines Azure Container Service-Clusters](container-service-deployment.md)
* [Verbinden mit einem Azure Container Service-Cluster](container-service-connect.md)

Nachdem die Verbindung mit dem Azure Container Service-Cluster hergestellt wurde, können Sie auf DC/OS und die verwandten REST-APIs über „http://localhost:local-port“ zugreifen. Bei den Beispielen in diesem Dokument wird davon ausgegangen, dass das Tunneling über Port 80 erfolgt. Der Marathon-Endpunkt kann beispielsweise unter `http://localhost/marathon/v2/`erreicht werden. Weitere Informationen zu den verschiedenen APIs finden Sie in der Mesosphere-Dokumentation für die [Marathon-API](https://mesosphere.github.io/marathon/docs/rest-api.html) und die [Chronos-API](https://mesos.github.io/chronos/docs/api.html) sowie in der Apache-Dokumentation für die [Mesos Scheduler-API](http://mesos.apache.org/documentation/latest/scheduler-http-api/).

## <a name="gather-information-from-dcos-and-marathon"></a>Erfassen von Informationen von DC/OS und Marathon
Erfassen Sie vor dem Bereitstellen von Containern im DC/OS-Cluster einige Informationen zum DC/OS-Cluster, z.B. die Namen und den aktuellen Status der DC/OS-Agents. Fragen Sie dazu den `master/slaves`-Endpunkt der DC/OS-REST-API ab. Wenn alles gut geht, wird eine Liste der DC/OS-Agents mit jeweils einigen Eigenschaften angezeigt.

```bash
curl http://localhost/mesos/master/slaves
```

Verwenden Sie nun den Marathon-`/apps`-Endpunkt, um aktuelle Anwendungsbereitstellungen im DC/OS-Cluster zu überprüfen. Wenn es sich um einen neuen Cluster handelt, wird ein leeres Array für Apps angezeigt.

```
curl localhost/marathon/v2/apps

{"apps":[]}
```

## <a name="deploy-a-docker-formatted-container"></a>Bereitstellen eines Containers im Docker-Format
Sie stellen Container im Docker-Format mit Marathon über eine JSON-Datei bereit, die die vorgesehene Bereitstellung beschreibt. Im folgenden Beispiel wird der Nginx-Container bereitgestellt. Dabei wird Port 80 des DC/OS-Agents an Port 80 des Containers gebunden. Beachten Sie außerdem, dass die Eigenschaft „acceptedResourceRoles“ auf „slave_public“ festgelegt ist. Damit wird der Container für einen Agent in der öffentlich zugänglichen Agent-Skalierungsgruppe bereitgestellt.

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

Erstellen Sie zum Bereitstellen eines Containers im Docker-Format Ihre eigene JSON-Datei, oder verwenden Sie das Beispiel auf der [Seite mit der Azure Container Service-Demonstration](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json). Speichern Sie die Datei an einem zugänglichen Speicherort. Führen Sie als Nächstes den folgenden Befehl aus, um den Container bereitzustellen. Geben Sie den Namen der JSON-Datei an.

```
curl -X POST http://localhost/marathon/v2/apps -d @marathon.json -H "Content-type: application/json"
```

Die Ausgabe sieht etwa wie folgt aus:

```json
{"version":"2015-11-20T18:59:00.494Z","deploymentId":"b12f8a73-f56a-4eb1-9375-4ac026d6cdec"}
```

Wenn Sie jetzt Marathon auf Anwendungen abfragen, wird diese neue Anwendung in der Ausgabe angezeigt.

```
curl localhost/marathon/v2/apps
```

## <a name="scale-your-containers"></a>Skalieren der Container
Sie können auch die Marathon-API verwenden, um Anwendungsbereitstellungen horizontal hoch- oder herunterzuskalieren. Im vorherigen Beispiel haben Sie eine Instanz einer Anwendung bereitgestellt. Wir führen hierfür jetzt das horizontale Hochskalieren auf drei Instanzen einer Anwendung durch. Hierzu erstellen Sie eine JSON-Datei, indem Sie den folgenden JSON-Text verwenden und an einem zugänglichen Speicherort speichern.

```json
{ "instances": 3 }
```

Führen Sie den folgenden Befehl aus, um die Anwendung horizontal hochzuskalieren.

> [!NOTE]
> Der URI ist „http://localhost/marathon/v2/apps/“ gefolgt von der ID der zu skalierenden Anwendung. Wenn Sie das hier bereitgestellte Nginx-Beispiel verwenden, lautet der URI „http://localhost/marathon/v2/apps/nginx“.
> 
> 

```json
curl http://localhost/marathon/v2/apps/nginx -H "Content-type: application/json" -X PUT -d @scale.json
```

Fragen Sie den Marathon-Endpunkt zum Schluss auf Anwendungen ab. Sie sehen, dass jetzt drei Nginx-Container vorhanden sind.

```
curl localhost/marathon/v2/apps
```

## <a name="use-powershell-for-this-exercise-marathon-rest-api-interaction-with-powershell"></a>Verwenden Sie PowerShell für diese Übung: Marathon-REST-API-Interaktion mit PowerShell
Sie können diese Aktionen auch durchführen, indem Sie PowerShell-Befehle in einem Windows-System verwenden.

Führen Sie den folgenden Befehl aus, um Informationen zum DC/OS-Cluster zu erfassen, z.B. Agent-Namen und -Status.

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

Erstellen Sie Ihre eigene JSON-Datei, oder verwenden Sie das Beispiel auf der [Seite mit dem Azure Container Service-Demo](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json). Speichern Sie die Datei an einem zugänglichen Speicherort. Führen Sie als Nächstes den folgenden Befehl aus, um den Container bereitzustellen. Geben Sie den Namen der JSON-Datei an.

```powershell
Invoke-WebRequest -Method Post -Uri http://localhost/marathon/v2/apps -ContentType application/json -InFile 'c:\marathon.json'
```

Sie können auch die Marathon-API verwenden, um Anwendungsbereitstellungen horizontal hoch- oder herunterzuskalieren. Im vorherigen Beispiel haben Sie eine Instanz einer Anwendung bereitgestellt. Wir führen hierfür jetzt das horizontale Hochskalieren auf drei Instanzen einer Anwendung durch. Hierzu erstellen Sie eine JSON-Datei, indem Sie den folgenden JSON-Text verwenden und an einem zugänglichen Speicherort speichern.

```json
{ "instances": 3 }
```

Führen Sie den folgenden Befehl aus, um die Anwendung horizontal hochzuskalieren.

> [!NOTE]
> Der URI ist „http://localhost/marathon/v2/apps/“ gefolgt von der ID der zu skalierenden Anwendung. Wenn Sie das hier bereitgestellte Nginx-Beispiel verwenden, lautet der URI „http://localhost/marathon/v2/apps/nginx“.
> 
> 

```powershell
Invoke-WebRequest -Method Put -Uri http://localhost/marathon/v2/apps/nginx -ContentType application/json -InFile 'c:\scale.json'
```

## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich unter Apache Mesos weiter über [HTTP-Endpunkte](http://mesos.apache.org/documentation/latest/endpoints/).
* [Weitere Informationen zur Marathon-REST-API](https://mesosphere.github.io/marathon/docs/rest-api.html)




<!--HONumber=Dec16_HO2-->


