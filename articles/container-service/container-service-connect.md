<properties
   pageTitle="Verbinden mit einem Azure Container Service-Cluster | Microsoft Azure"
   description="Erfahren Sie, wie Sie die Verbindung mit einem Azure Container Service-Cluster über einen SSH-Tunnel herstellen."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, Container, Microservices, DC/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/12/2016"
   ms.author="rogardle"/>


# Verbinden mit einem Azure Container Service-Cluster

Für die DC/OS- und Docker Swarm-Cluster, die vom Azure Container Service bereitgestellt werden, werden REST-Endpunkte verfügbar gemacht. Diese Endpunkte sind aber nicht öffentlich zugänglich. Zum Verwalten dieser Endpunkte müssen Sie einen SSH-Tunnel (Secure Shell) erstellen. Sobald ein SSH-Tunnel eingerichtet wurde, können Sie Befehle auf die Cluster-Endpunkte anwenden und die Cluster-Benutzeroberfläche über einen Browser auf Ihrem System anzeigen. In diesem Dokument wird die Erstellung eines SSH-Tunnels in Linux, OS X und Windows schrittweise erläutert.

>[AZURE.NOTE] Sie können eine SSH-Sitzung mit einem Clusterverwaltungssystem erstellen. Dies ist aber nicht zu empfehlen. Wenn Sie direkt auf einem Verwaltungssystem arbeiten, besteht das Risiko unbeabsichtigter Konfigurationsänderungen.

## Erstellen eines SSH-Tunnels unter Linux oder OS X

Als Erstes ermitteln Sie beim Erstellen eines SSH-Tunnels unter Linux oder OS X den öffentlichen DNS-Namen von Masterelementen mit Lastenausgleich. Erweitern Sie hierzu die Ressourcengruppe, damit jede Ressource angezeigt wird. Suchen Sie die öffentliche IP-Adresse des Masters, und wählen Sie sie aus. Es wird ein Blatt mit Informationen zur öffentlichen IP-Adresse geöffnet, das den DNS-Namen enthält. Speichern Sie diesen Namen für die spätere Verwendung. <br />


![Öffentlicher DNS-Name](media/pubdns.png)

Öffnen Sie nun eine Shell, und führen Sie den folgenden Befehl aus, wobei Folgendes gilt:

**PORT** ist der Port des Endpunkts, den Sie verfügbar machen möchten. Für Swarm ist dies 2375. Verwenden Sie für DC/OS Port 80. **BENUTZERNAME** ist der Benutzername, der bei der Bereitstellung des Clusters angegeben wurde. **DNSPREFIX** ist das DNS-Präfix, das bei der Bereitstellung des Clusters angegeben wurde. **REGION** ist die Region, in der sich die Ressourcengruppe befindet. **PATH\_TO\_PRIVATE\_KEY** [OPTIONAL] ist der Pfad zum privaten Schlüssel, der zu dem öffentlichen Schlüssel passt, den Sie beim Erstellen des Container Service-Clusters angegeben haben. Verwenden Sie diese Option mit dem Flag „-i“.

```bash
# ssh sample

ssh -L PORT:localhost:PORT -f -N [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com -p 2200
```
> Für die SSH-Verbindung wird Port 2200 und nicht der Standardport 22 verwendet.

## DC/OS-Tunnel

Öffnen Sie einen Tunnel zu den zu DC/OS gehörigen Endpunkten, indem Sie einen Befehl wie den folgenden ausführen:

```bash
# ssh sample

sudo ssh -L 80:localhost:80 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Nun können Sie wie folgt auf die zu DC/OS gehörigen Endpunkte zugreifen:

- DC/OS: `http://localhost/`
- Marathon: `http://localhost/marathon`
- Mesos: `http://localhost/mesos`

Sie erreichen die REST-APIs für jede Anwendung über diesen Tunnel auf ähnliche Weise:

## Swarm-Tunnel

Führen Sie zum Öffnen eines Tunnels zum Swarm-Endpunkt einen Befehl wie diesen aus:

```bash
# ssh sample

ssh -L 2375:localhost:2375 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Sie können die DOCKER\_HOST-Umgebungsvariable jetzt wie folgt festlegen: Sie können Ihre Docker-Befehlszeilenschnittstelle (CLI) wie gewohnt weiterverwenden.

```bash
export DOCKER_HOST=:2375
```

## Erstellen eines SSH-Tunnels unter Windows

Es gibt mehrere Möglichkeiten, wie Sie SSH-Tunnel unter Windows erstellen können. In diesem Dokument wird beschrieben, wie Sie PuTTY verwenden, um dies zu erreichen.

Laden Sie PuTTY auf Ihr Windows-System herunter, und führen Sie die Anwendung aus.

Geben Sie einen Hostnamen ein, der aus dem Administratorbenutzernamen für den Cluster und dem öffentlichen DNS-Namen für den ersten Master im Cluster besteht. Der **Hostname** sieht wie folgt aus: `adminuser@PublicDNS`. Geben Sie 2200 als **Port** ein.

![PuTTY-Konfiguration 1](media/putty1.png)

Wählen Sie **SSH** und **Authentifizierung**. Fügen Sie Ihre private Schlüsseldatei für die Authentifizierung hinzu.

![PuTTY-Konfiguration 2](media/putty2.png)

Wählen Sie **Tunnel**, und konfigurieren Sie die folgenden weitergeleiteten Ports:
- **Quellport:** Ihr Wert – 80 für DC/OS bzw. 2375 für Swarm verwenden.
- **Ziel:** „localhost:80“ für DC/OS bzw. „localhost:2375“ für Swarm verwenden.

Das folgende Beispiel ist für DC/OS konfiguriert, sieht für Docker Swarm aber ähnlich aus.

>[AZURE.NOTE] Port 80 darf beim Erstellen dieses Tunnels nicht verwendet werden.

![PuTTY-Konfiguration 3](media/putty3.png)

Speichern Sie nach Abschluss des Vorgangs die Verbindungskonfiguration, und stellen Sie die Verbindung mit der PuTTY-Sitzung her. Beim Herstellen der Verbindung können Sie die Portkonfiguration im PuTTY-Ereignisprotokoll anzeigen.

![PuTTY-Ereignisprotokoll](media/putty4.png)

Wenn Sie den Tunnel für DC/OS konfiguriert haben, können Sie wie folgt auf den zugehörigen Endpunkt zugreifen:

- DC/OS: `http://localhost/`
- Marathon: `http://localhost/marathon`
- Mesos: `http://localhost/mesos`

Wenn Sie den Tunnel für Docker Swarm konfiguriert haben, können Sie über die Docker-Befehlszeilenschnittstelle auf den Swarm-Cluster zugreifen. Sie müssen zunächst eine Windows-Umgebungsvariable mit dem Namen `DOCKER_HOST` und dem Wert ` :2375` konfigurieren.

## Nächste Schritte

Bereitstellen und Verwalten von Containern mit DC/OS oder Swarm:

- [Verwenden von Azure Container Service und DC/OS](container-service-mesos-marathon-rest.md)
- [Verwenden von Azure Container Service und Docker Swarm](container-service-docker-swarm.md)

<!---HONumber=AcomDC_0615_2016-->