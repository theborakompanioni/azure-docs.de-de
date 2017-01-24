---
title: Verbinden mit einem Azure Container Service-Cluster | Microsoft Docs
description: "Erfahren Sie, wie Sie die Verbindung mit einem Azure Container Service-Cluster über einen SSH-Tunnel herstellen."
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Container, Microservices, DC/OS, Azure
ms.assetid: ff8d9e32-20d2-4658-829f-590dec89603d
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/13/2016
ms.author: rogardle
translationtype: Human Translation
ms.sourcegitcommit: bcc2d3468c8a560105aa2c2feb0d969ec3cccdcb
ms.openlocfilehash: 5296586b9266f432042f847f4dff9e6ff62ebc8b


---
# <a name="connect-to-an-azure-container-service-cluster"></a>Verbinden mit einem Azure Container Service-Cluster
Für alle DC/OS-, Kubernetes- und Docker Swarm-Cluster, die von Azure Container Service bereitgestellt werden, werden REST-Endpunkte verfügbar gemacht.  Dieser Endpunkt von Kubernetes wird sicher im Internet verfügbar gemacht, und Sie können direkt von jedem Computer aus auf ihn zugreifen, der mit dem Internet verbunden ist. Für DC/OS und Docker Swarm müssen Sie einen SSH-Tunnel erstellen, um eine sichere Verbindung zum REST-Endpunkt herzustellen. Jede dieser Verbindungen ist unten beschrieben.

> [!NOTE]
> Die Kubernetes-Unterstützung in Azure Container Service befindet sich derzeit in der Vorschauphase.
>

## <a name="connecting-to-a-kubernetes-cluster"></a>Herstellen einer Verbindung zu einem Kubernetes-Cluster
Sie müssen das `kubectl`-Befehlszeilentool installieren, um eine Verbindung mit einem Kubernetes-Cluster herstellen zu können.  Die einfachste Möglichkeit zum Installieren dieses Tools ist die Verwendung des Azure 2.0 `az`-Befehlszeilentools.

```console
az acs kubernetes install cli [--install-location=/some/directory]
```

Alternativ können Sie den Client direkt über die [Übersicht der verschiedenen Releases](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md#downloads-for-v146) herunterladen.

Nachdem `kubectl` installiert wurde, müssen Sie die Clusteranmeldeinformationen auf Ihren Computer kopieren.  Am einfachsten geht das, indem Sie das `az`-Befehlszeilentool erneut verwenden.

```console
az acs kubernetes get-credentials --dns-prefix=<some-prefix> --location=<some-location>
```

Dadurch werden die Clusteranmeldeinformationen in `$HOME/.kube/config` heruntergeladen, und `kubectl` erwartet, dass sie sich dort befinden.

Sie können alternativ auch `scp` verwenden, um die Datei sicher von `$HOME/.kube/config` auf der Master-VM auf Ihren lokalen Computer zu kopieren.

```console
mkdir $HOME/.kube/config
scp azureuser@<master-dns-name>:.kube/config $HOME/.kube/config
```

Wenn Sie mit Windows arbeiten, müssen Sie „Bash on Ubuntu on Windows“ oder das PuTTY „pscp“-Tool verwenden.

Nachdem Sie `kubectl` konfiguriert haben, können Sie dies durch Auflisten der Knoten im Cluster testen:

```console
kubectl get nodes
```

Abschließend können Sie das Kubernetes-Dashboard anzeigen. Führen Sie zuerst Folgendes aus:

```console
kubectl proxy
```

Die Kubernetes UI ist jetzt unter „http://localhost:8001/ui“ verfügbar.

Weitere Anweisungen finden Sie in der [Schnellstartanleitung von Kubernetes (in englischer Sprache)](http://kubernetes.io/docs/user-guide/quick-start/).

## <a name="connecting-to-a-dcos-or-swarm-cluster"></a>Herstellen einer Verbindung mit einem DC/OS- oder Swarm-Cluster

Für die DC/OS- und Docker Swarm-Cluster, die vom Azure Container Service bereitgestellt werden, werden REST-Endpunkte verfügbar gemacht. Diese Endpunkte sind aber nicht öffentlich zugänglich. Zum Verwalten dieser Endpunkte müssen Sie einen SSH-Tunnel (Secure Shell) erstellen. Sobald ein SSH-Tunnel eingerichtet wurde, können Sie Befehle auf die Cluster-Endpunkte anwenden und die Cluster-Benutzeroberfläche über einen Browser auf Ihrem System anzeigen. In diesem Dokument wird die Erstellung eines SSH-Tunnels in Linux, OS X und Windows schrittweise erläutert.

> [!NOTE]
> Sie können eine SSH-Sitzung mit einem Clusterverwaltungssystem erstellen. Dies ist aber nicht zu empfehlen. Wenn Sie direkt auf einem Verwaltungssystem arbeiten, besteht das Risiko unbeabsichtigter Konfigurationsänderungen.   
> 
> 

## <a name="create-an-ssh-tunnel-on-linux-or-os-x"></a>Erstellen eines SSH-Tunnels unter Linux oder OS X
Als Erstes ermitteln Sie beim Erstellen eines SSH-Tunnels unter Linux oder OS X den öffentlichen DNS-Namen von Masterelementen mit Lastenausgleich. Erweitern Sie hierzu die Ressourcengruppe, damit jede Ressource angezeigt wird. Suchen Sie die öffentliche IP-Adresse des Masters, und wählen Sie sie aus. Es wird ein Blatt mit Informationen zur öffentlichen IP-Adresse geöffnet, das den DNS-Namen enthält. Speichern Sie diesen Namen für die spätere Verwendung. <br />

![Öffentlicher DNS-Name](media/pubdns.png)

Öffnen Sie nun eine Shell, und führen Sie den folgenden Befehl aus, wobei Folgendes gilt:

**PORT** ist der Port des Endpunkts, den Sie verfügbar machen möchten. Für Swarm ist dies 2375. Verwenden Sie für DC/OS Port 80.  
**BENUTZERNAME** ist der Benutzername, der bei der Bereitstellung des Clusters angegeben wurde.  
**DNSPREFIX** ist das DNS-Präfix, das bei der Bereitstellung des Clusters angegeben wurde.  
**REGION** ist die Region, in der sich die Ressourcengruppe befindet.  
**PATH_TO_PRIVATE_KEY** [OPTIONAL] ist der Pfad zum privaten Schlüssel, der zu dem öffentlichen Schlüssel passt, den Sie beim Erstellen des Container Service-Clusters angegeben haben. Verwenden Sie diese Option mit dem Flag „-i“.

```bash
ssh -L PORT:localhost:PORT -f -N [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com -p 2200
```
> Für die SSH-Verbindung wird Port 2200 und nicht der Standardport 22 verwendet.
> 
> 

## <a name="dcos-tunnel"></a>DC/OS-Tunnel
Öffnen Sie einen Tunnel zu den zu DC/OS gehörigen Endpunkten, indem Sie einen Befehl wie den folgenden ausführen:

```bash
sudo ssh -L 80:localhost:80 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Nun können Sie wie folgt auf die zu DC/OS gehörigen Endpunkte zugreifen:

* DC/OS: `http://localhost/`
* Marathon: `http://localhost/marathon`
* Mesos: `http://localhost/mesos`

Sie erreichen die REST-APIs für jede Anwendung über diesen Tunnel auf ähnliche Weise:

## <a name="swarm-tunnel"></a>Swarm-Tunnel
Führen Sie zum Öffnen eines Tunnels zum Swarm-Endpunkt einen Befehl wie diesen aus:

```bash
ssh -L 2375:localhost:2375 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Sie können die DOCKER_HOST-Umgebungsvariable jetzt wie folgt festlegen: Sie können Ihre Docker-Befehlszeilenschnittstelle (CLI) wie gewohnt weiterverwenden.

```bash
export DOCKER_HOST=:2375
```

## <a name="create-an-ssh-tunnel-on-windows"></a>Erstellen eines SSH-Tunnels unter Windows
Es gibt mehrere Möglichkeiten, wie Sie SSH-Tunnel unter Windows erstellen können. In diesem Dokument wird beschrieben, wie Sie PuTTY verwenden, um dies zu erreichen.

Laden Sie PuTTY auf Ihr Windows-System herunter, und führen Sie die Anwendung aus.

Geben Sie einen Hostnamen ein, der aus dem Administratorbenutzernamen für den Cluster und dem öffentlichen DNS-Namen für den ersten Master im Cluster besteht. Der **Hostname** sieht wie folgt aus: `adminuser@PublicDNS`. Geben Sie 2200 als **Port**ein.

![PuTTY-Konfiguration 1](media/putty1.png)

Wählen Sie **SSH** und **Authentifizierung** aus. Fügen Sie Ihre private Schlüsseldatei für die Authentifizierung hinzu.

![PuTTY-Konfiguration 2](media/putty2.png)

Wählen Sie **Tunnel** , und konfigurieren Sie die folgenden weitergeleiteten Ports:

* **Quellport**: Ihr Wert – verwenden Sie 80 für DC/OS bzw. 2375 für Swarm.
* **Ziel**: Verwenden Sie „localhost:80“ für DC/OS bzw. „localhost:2375“ für Swarm.

Das folgende Beispiel ist für DC/OS konfiguriert, sieht für Docker Swarm aber ähnlich aus.

> [!NOTE]
> Port 80 darf beim Erstellen dieses Tunnels nicht verwendet werden.
> 
> 

![PuTTY-Konfiguration 3](media/putty3.png)

Speichern Sie nach Abschluss des Vorgangs die Verbindungskonfiguration, und stellen Sie die Verbindung mit der PuTTY-Sitzung her. Beim Herstellen der Verbindung können Sie die Portkonfiguration im PuTTY-Ereignisprotokoll anzeigen.

![PuTTY-Ereignisprotokoll](media/putty4.png)

Wenn Sie den Tunnel für DC/OS konfiguriert haben, können Sie wie folgt auf den zugehörigen Endpunkt zugreifen:

* DC/OS: `http://localhost/`
* Marathon: `http://localhost/marathon`
* Mesos: `http://localhost/mesos`

Wenn Sie den Tunnel für Docker Swarm konfiguriert haben, können Sie über die Docker-Befehlszeilenschnittstelle auf den Swarm-Cluster zugreifen. Sie müssen zunächst eine Windows-Umgebungsvariable mit dem Namen `DOCKER_HOST` und dem Wert ` :2375` konfigurieren.

## <a name="next-steps"></a>Nächste Schritte
Bereitstellen und Verwalten von Containern mit DC/OS oder Swarm:

* [Verwenden von Azure Container Service und DC/OS](container-service-mesos-marathon-rest.md)
* [Verwenden von Azure Container Service und Docker Swarm](container-service-docker-swarm.md)




<!--HONumber=Dec16_HO3-->


