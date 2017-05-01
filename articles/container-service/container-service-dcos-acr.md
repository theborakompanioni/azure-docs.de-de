---
title: Verwenden von ACR mit einem Azure DC/OS-Cluster | Microsoft-Dokumentation
description: Verwenden von Azure Container Registry mit einem DC/OS-Cluster in Azure Container Service
services: container-service
documentationcenter: 
author: julienstroheker
manager: dcaro
editor: 
tags: acs, azure-container-service, acr, azure-container-registry
keywords: Docker, Container, Microservices, Mesos, Azure, FileShare, cifs
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/23/2017
ms.author: juliens
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: a8a3716f8d03b596285026426c7514b7b642cb25
ms.lasthandoff: 03/31/2017


---
# <a name="use-acr-with-a-dcos-cluster-to-deploy-your-application"></a>Verwenden von ACR mit einem DC/OS-Cluster zum Bereitstellen Ihrer Anwendung

In diesem Artikel wird erläutert, wie Sie eine private Containerregistrierung wie ACR (Azure Container Registry) mit einem DC/OS-Cluster verwenden. Mithilfe von ACR können Sie Images privat speichern und behalten die Kontrolle, z.B. über die verschiedenen Versionen und Updates.

Vor dem Durcharbeiten dieses Beispiels müssen folgende Voraussetzungen erfüllt sein: 
* Sie benötigen eine DC/OS-Cluster, der in Azure Container Service konfiguriert ist. Weitere Informationen dazu finden Sie unter [Bereitstellen eines Azure Container Service-Clusters](container-service-deployment.md).
* Eine Azure Container Service-Instanz muss bereitgestellt sein. Informationen finden Sie unter [Erstellen einer privaten Docker-Containerregistrierung mit dem Azure-Portal](https://docs.microsoft.com/azure/container-registry/container-registry-get-started-portal) und [Erstellen einer privaten Docker-Containerregistrierung mit der Azure CLI 2.0](https://docs.microsoft.com/azure/container-registry/container-registry-get-started-azure-cli).
* Eine Dateifreigabe muss im DC/OS-Cluster konfiguriert sein. Informationen finden Sie unter [Bereitstellen und Einbinden einer Dateifreigabe für einen DC/OS-Cluster](container-service-dcos-fileshare.md).
* Sie müssen wissen, wie Sie ein Docker-Image über die [Webbenutzeroberfläche](container-service-mesos-marathon-ui.md) oder die [REST-API](container-service-mesos-marathon-rest.md) in einem DC/OS-Cluster bereitstellen.

## <a name="manage-the-authentication-inside-your-cluster"></a>Verwalten der Authentifizierung in Ihrem Cluster

Bei der herkömmlichen Art, Images per Push und Pull in einer privaten Registrierung zu verwalten, muss zunächst eine Authentifizierung für die Registrierung erfolgen. Zu diesem Zweck müssen Sie die Befehlszeile `docker login` in jedem Docker-Clientprozess verwenden, der Ihre private Registrierung nutzen muss.
In Produktionsszenarien – in unserem Fall DC/OS – müssen Sie sicherstellen, dass Sie Images aus jedem Knoten per Pull abrufen können. Dies bedeutet, dass Sie den Authentifizierungsprozess automatisieren müssen, anstatt die Befehlszeile auf jedem einzelnen Computer auszuführen, denn die Authentifizierung kann – wie Sie sich sicher vorstellen können – je nach Größe Ihres Clusters ein schwieriger und ressourcenintensiver Vorgang sein. 

Wir nehmen an, dass Sie bereits [eine Dateifreigabe in Ihrem DC/OS-Cluster eingerichtet haben](container-service-dcos-fileshare.md), und nutzen diese folgendermaßen:

### <a name="from-any-client-machine-recommended-method"></a>Auf einem beliebigen Clientcomputer [empfohlenen Methode]

Die folgenden Befehle können in jeder Umgebung ausgeführt werden (Windows/Mac/Linux):

1. Stellen Sie sicher, dass folgende Voraussetzungen erfüllt sind:
  * TAR-Tool
    * [Windows](http://gnuwin32.sourceforge.net/packages/gtar.htm)
  * Docker 
    * [Windows](https://www.docker.com/docker-windows)
    * [MAC](https://www.docker.com/docker-mac)
    * [Ubuntu](https://www.docker.com/docker-ubuntu)
    * [Andere](https://www.docker.com/get-docker)
  * Dateifreigabe in Ihrem Cluster [mit folgender Methode](container-service-dcos-fileshare.md) eingebunden:

2. Initiieren Sie die Authentifizierung bei Ihrem ACR-Dienst, indem Sie folgenden Befehl auf Ihrem bevorzugten Terminal verwenden: `sudo docker login --username=<USERNAME> --password=<PASSWORD> <ACR-REGISTRY-NAME>.azurecr.io`. Sie müssen die Variablen `USERNAME`, `PASSWORD` und `ACR-REGISTRY-NAME` durch die Werte ersetzen, die in Ihrem Azure-Portal bereitgestellt wurden.

3. Interessanter Hinweis: Wenn Sie einen `docker login`-Vorgang ausführen, werden die Werte lokal in Ihrem Basisordner auf dem Computer gespeichert (`cd ~/.docker` unter Mac und Linux sowie `cd %HOMEPATH%` unter Windows). Wir komprimieren die Inhalte dieses Ordners mit dem Befehl `tar czf`.

4. Der letzte Schritt besteht darin, die gerade erstellte TAR-Datei in die Dateifreigabe zu kopieren, [die Sie als Voraussetzung erstellt haben](container-service-dcos-fileshare.md). Dazu können Sie folgenden Befehl an der Azure-Befehlszeilenschnittstelle verwenden: `az storage file upload -s <shareName> --account-name <storageAccountName> --account-key <storageAccountKey> -source <pathToTheTarFile>`

Hier finden Sie ein Beispiel mit dem folgenden Setup (in einer Windows-Umgebung):
* ACR-Name: **`demodcos`**
* Benutzername: **`demodcos`**
* Kennwort: **`+js+/=I1=L+D=+eRpU+/=wI/AjvDo=J0`**
* Speicherkontoname: **`anystorageaccountname`**
* Speicherkontoschlüssel: **`aYGl6Nys4De5J3VPldT1rXxz2+VjgO7dgWytnoWClurZ/l8iO5c5N8xXNS6mpJhSc9xh+7zkT7Mr+xIT4OIVMg==`**
* Im Speicherkonto erstellter Freigabename: **`share`**
* Pfad des hochzuladenden TAR-Archivs: **`%HOMEPATH%/.docker/docker.tar.gz`**

```bash
# Changing directory to the home folder of the default user
cd %HOMEPATH%

# Authentication into my ACR
docker login --username=demodcos --password=+js+/=I1=L+D=+eRpU+/=wI/AjvDo=J0 demodcos.azurecr.io

# Tar the contains of the .docker folder
tar czf docker.tar.gz .docker

# Upload the tar archive in the fileshare
az storage file upload -s share --account-name anystorageaccountname --account-key aYGl6Nys4De5J3VPldT1rXxz2+VjgO7dgWytnoWClurZ/l8iO5c5N8xXNS6mpJhSc9xh+7zkT7Mr+xIT4OIVMg== --source %HOMEPATH%/docker.tar.gz
```

### <a name="from-the-master-not-recommended-method"></a>Über den Master [keine empfohlene Methode]

Die Ausführung des Vorgangs über den Master wird nicht empfohlen, um Fehler und negative Auswirkungen auf die gesamte Umgebung zu vermeiden.

1. Stellen Sie zunächst eine SSH-Verbindung mit dem Master (oder dem ersten Master) Ihres DC/OS-basierten Clusters her. Beispielsweise `ssh userName@masterFQDN –A –p 22`, wobei der masterFQDN der vollqualifizierte Domänenname des virtuellen Mastercomputers ist. [Klicken Sie hier, um weitere Informationen zu erhalten](https://docs.microsoft.com/azure/container-service/container-service-connect#connect-to-a-dcos-or-swarm-cluster)

2. Initiieren Sie die Authentifizierung bei Ihrem ACR-Dienst, indem Sie folgenden Befehl verwenden: `sudo docker login --username=<USERNAME> --password=<PASSWORD> <ACR-REGISTRY-NAME>.azurecr.io`. Sie müssen die Variablen `USERNAME`, `PASSWORD` und `ACR-REGISTRY-NAME` durch die Werte ersetzen, die in Ihrem Azure-Portal bereitgestellt wurden.

3. Interessanter Hinweis: Wenn Sie einen `docker login`-Vorgang ausführen, werden die Werte lokal in Ihrem Basisordner (`~/.docker`) auf dem Computer gespeichert. Wir komprimieren die Inhalte dieses Ordners mit dem Befehl `tar czf`.

4. Der letzte Schritt besteht darin, die gerade erstellte TAR-Datei in die Dateifreigabe zu kopieren. Mit diesem Vorgang ermöglichen Sie die Verwendung dieser Anmeldeinformationen auf allen virtuellen Computern in Ihrem Cluster zur Authentifizierung bei Ihrer Azure Container Registry.

Hier finden Sie ein Beispiel mit dem folgenden Setup:
* ACR-Name: **`demodcos`**
* Benutzername: **`demodcos`**
* Kennwort: **`+js+/=I1=L+D=+eRpU+/=wI/AjvDo=J0`**
* Bereitstellungspunkt innerhalb des Clusters: **`/mnt/share`**

```bash
# Changing directory to the home folder of the default user
cd ~

# Authentication into my ACR
sudo docker login --username=demodcos --password=+js+/=I1=L+D=+eRpU+/=wI/AjvDo=J0 demodcos.azurecr.io

# Tar the contains of the .docker folder
sudo tar czf docker.tar.gz .docker

# Copy of the tar file in the file share of my cluster
sudo cp docker.tar.gz /mnt/share
```


## <a name="deploy-an-image-from-acr-with-marathon"></a>Bereitstellen eines Images aus ACR mit Marathon

In diesem Abschnitt wird angenommen, dass Sie die Images, die Sie bereitstellen möchten, bereits per Push in Ihre Containerregistrierung übertragen haben. Informationen dazu finden Sie unter [Pushübertragung des ersten Images an eine private Containerregistrierung mit der Docker CLI](https://docs.microsoft.com/azure/container-registry/container-registry-get-started-docker-cli).

Wenn wir das Image **simple-web** mit dem Tag **2.1** aus unserer in Azure ACR gehosteten privaten Registrierung bereitstellen möchten, verwenden wir die folgende Konfiguration:

```json
{
  "id": "myapp",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "demodcos.azurecr.io/simple-web:2.1",
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
  },
  "uris":  [
       "file:///mnt/share/docker.tar.gz"
   ]
}
```

> [!NOTE] 
> Wie Sie sehen, verwenden wir die Option **uris**, um anzugeben, wo die Anmeldeinformationen gespeichert sind.
>

## <a name="next-steps"></a>Nächste Schritte
* Erhalten Sie weitere Informationen zum [Verwalten Ihrer DC/OS-Container](container-service-mesos-marathon-ui.md).
* DC/OS-Containerverwaltung über die [Marathon-REST-API](container-service-mesos-marathon-rest.md).
