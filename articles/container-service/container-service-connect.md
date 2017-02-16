---
title: Verbinden mit einem Azure Container Service-Cluster | Microsoft Docs
description: Es wird beschrieben, wie Sie von einem Remotecomputer aus eine Verbindung mit einem Kubernetes-, DC/OS- oder Docker Swarm-Cluster in Azure Container Service herstellen.
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Container, Microservices, Kubernetes, DC/OS, Azure
ms.assetid: ff8d9e32-20d2-4658-829f-590dec89603d
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/12/2017
ms.author: rogardle
translationtype: Human Translation
ms.sourcegitcommit: ea59ff3f527d051e01baf12f596ff44af8a0dfc1
ms.openlocfilehash: 7fe3bc6a5eab1d1b9a8b73ab3c88f9808817369a


---
# <a name="connect-to-an-azure-container-service-cluster"></a>Verbinden mit einem Azure Container Service-Cluster
Nach dem Erstellen eines Azure Container Service-Clusters müssen Sie eine Verbindung mit dem Cluster herstellen, um Workloads bereitzustellen und zu verwalten. In diesem Artikel wird beschrieben, wie Sie von einem Remotecomputer aus eine Verbindung mit dem virtuellen Mastercomputer des Clusters herstellen. Für die Kubernetes-, DC/OS- und Docker Swarm-Cluster werden jeweils REST-Endpunkte verfügbar gemacht. Für Kubernetes wird dieser Endpunkt auf sichere Weise im Internet verfügbar gemacht, und Sie können darauf zugreifen, indem Sie das Befehlszeilentool `kubectl` auf einem Computer mit Internetverbindung ausführen. Für DC/OS und Docker Swarm müssen Sie einen SSH-Tunnel (Secure Shell) erstellen, um eine sichere Verbindung mit dem REST-Endpunkt herzustellen. 

> [!NOTE]
> Die Kubernetes-Unterstützung in Azure Container Service befindet sich derzeit in der Vorschauphase.
>

## <a name="prerequisites"></a>Voraussetzungen

* Ein Kubernetes-, DC/OS- oder Swarm-Cluster, der [in Azure Container Service bereitgestellt wurde](container-service-deployment.md).
* SSH-Datei mit passendem privatem Schlüssel für den öffentlichen Schlüssel, der dem Cluster während der Bereitstellung hinzugefügt wurde. Bei diesen Befehlen wird vorausgesetzt, dass sich der private SSH-Schlüssel auf Ihrem Computer unter `$HOME/.ssh/id_rsa` befindet. Weitere Informationen finden Sie in der Anleitung für [OS X und Linux](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) bzw. [Windows](../virtual-machines/virtual-machines-linux-ssh-from-windows.md). Wenn die SSH-Verbindung nicht funktioniert, müssen Sie ggf. [Ihre SSH-Schlüssel zurücksetzen](../virtual-machines/virtual-machines-linux-troubleshoot-ssh-connection.md).

## <a name="connect-to-a-kubernetes-cluster"></a>Herstellen einer Verbindung mit einem Kubernetes-Cluster

Führen Sie diese Schritte aus, um `kubectl` auf Ihrem Computer zu installieren und zu konfigurieren.

> [!NOTE] 
> Unter Linux oder OS X müssen Sie die Befehle dieses Abschnitts unter Umständen mit `sudo` ausführen.
> 

### <a name="install-kubectl"></a>Installieren von kubectl
Eine Möglichkeit zum Installieren dieses Tools ist der Befehl `az acs kubernetes install cli` der Azure-CLI 2.0 (Vorschau). Stellen Sie zum Ausführen dieses Befehls sicher, dass Sie die aktuelle Version von Azure-CLI 2.0 (Vorschau) [installiert](/cli/azure/install-az-cli2) haben und an einem Azure-Konto (`az login`) angemeldet sind.

```azurecli
# Linux or OS X
az acs kubernetes install-cli [--install-location=/some/directory/kubectl]

# Windows
az acs kubernetes install-cli [--install-location=C:\some\directory\kubectl.exe]
```

Alternativ hierzu können Sie den Client direkt über die [Übersicht der verschiedenen Releases](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md#downloads-for-v146) herunterladen.

### <a name="download-cluster-credentials"></a>Herunterladen von Clusteranmeldeinformationen
Nachdem `kubectl` installiert wurde, müssen Sie die Clusteranmeldeinformationen auf Ihren Computer kopieren. Eine Möglichkeit zum Abrufen der Anmeldeinformationen ist der Befehl `az acs kubernetes get-credentials`. Übergeben Sie den Namen der Ressourcengruppe und den Namen der Container Service-Ressource:


```azurecli
az acs kubernetes get-credentials --resource-group=<cluster-resource-group> --name=<cluster-name>
```

Mit diesem Befehl werden die Clusteranmeldeinformationen an den Speicherort `$HOME/.kube/config` heruntergeladen, da dies der von `kubectl` angenommene Speicherort ist.

Sie können alternativ auch `scp` verwenden, um die Datei sicher von `$HOME/.kube/config` auf dem virtuellen Mastercomputer auf Ihren lokalen Computer zu kopieren. Beispiel:

```console
mkdir $HOME/.kube/config
scp azureuser@<master-dns-name>:.kube/config $HOME/.kube/config
```

Bei Verwendung von Windows müssen Sie Bash unter Ubuntu unter Windows, den PuTTY Secure File Copy Client oder ein ähnliches Tool nutzen.



### <a name="use-kubectl"></a>Verwenden von kubectl

Nachdem Sie `kubectl` konfiguriert haben, können Sie die Verbindung durch Auflisten der Knoten im Cluster testen:

```console
kubectl get nodes
```

Sie können auch andere `kubectl`-Befehle ausprobieren. Beispielsweise können Sie das Kubernetes-Dashboard anzeigen. Führen Sie zuerst einen Proxy auf dem Kubernetes-API-Server aus:

```console
kubectl proxy
```

Die Kubernetes-UI ist jetzt auf der folgenden Seite verfügbar: `http://localhost:8001/ui`.

Weitere Informationen finden Sie in der [Schnellstartanleitung von Kubernetes](http://kubernetes.io/docs/user-guide/quick-start/).

## <a name="connect-to-a-dcos-or-swarm-cluster"></a>Herstellen einer Verbindung mit einem DC/OS- oder Swarm-Cluster

Für die DC/OS- und Docker Swarm-Cluster, die vom Azure Container Service bereitgestellt werden, werden REST-Endpunkte verfügbar gemacht. Diese Endpunkte sind aber nicht öffentlich zugänglich. Zum Verwalten dieser Endpunkte müssen Sie einen SSH-Tunnel (Secure Shell) erstellen. Sobald ein SSH-Tunnel eingerichtet wurde, können Sie Befehle auf die Cluster-Endpunkte anwenden und die Cluster-Benutzeroberfläche über einen Browser auf Ihrem System anzeigen. In den folgenden Abschnitten wird Schritt für Schritt beschrieben, wie Sie einen SSH-Tunnel auf Computern mit den Betriebssystemen Linux, OS X und Windows erstellen.

> [!NOTE]
> Sie können eine SSH-Sitzung mit einem Clusterverwaltungssystem erstellen. Dies ist aber nicht zu empfehlen. Wenn Sie direkt auf einem Verwaltungssystem arbeiten, besteht das Risiko unbeabsichtigter Konfigurationsänderungen.
> 

### <a name="create-an-ssh-tunnel-on-linux-or-os-x"></a>Erstellen eines SSH-Tunnels unter Linux oder OS X
Als Erstes ermitteln Sie beim Erstellen eines SSH-Tunnels unter Linux oder OS X den öffentlichen DNS-Namen von Masterelementen mit Lastenausgleich. Folgen Sie diesen Schritten:


1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu der Ressourcengruppe, die Ihren Container Service-Cluster enthält. Erweitern Sie die Ressourcengruppe, damit jede Ressource angezeigt wird. 

2. Suchen Sie nach dem virtuellen Computer der Mastereinheit, und wählen Sie ihn aus. In einem DC/OS-Cluster hat diese Ressource einen Namen, der mit **dcos-master-** beginnt. 

    Das Blatt **Virtueller Computer** enthält Informationen zur öffentlichen IP-Adresse, einschließlich des DNS-Namens. Speichern Sie diesen Namen für die spätere Verwendung. 

    ![Öffentlicher DNS-Name](media/pubdns.png)

3. Öffnen Sie als Nächstes eine Shell, und führen Sie den Befehl `ssh` aus, indem Sie die folgenden Werte angeben: 

    **PORT** ist der Port des Endpunkts, den Sie verfügbar machen möchten. Verwenden Sie für Swarm Port 2375. Verwenden Sie für DC/OS Port 80.  
    **BENUTZERNAME** ist der Benutzername, der bei der Bereitstellung des Clusters angegeben wurde.  
    **DNSPREFIX** ist das DNS-Präfix, das bei der Bereitstellung des Clusters angegeben wurde.  
    **REGION** ist die Region, in der sich die Ressourcengruppe befindet.  
    **PATH_TO_PRIVATE_KEY** [OPTIONAL] ist der Pfad zum privaten Schlüssel, der zu dem öffentlichen Schlüssel passt, den Sie beim Erstellen des Clusters angegeben haben. Verwenden Sie diese Option mit dem Flag `-i`.

    ```bash
    ssh -L PORT:localhost:PORT -f -N [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com -p 2200
    ```
    > [!NOTE]
    > Für die SSH-Verbindung wird Port 2200 und nicht der Standardport 22 verwendet. In einem Cluster mit mehr als einem virtuellen Mastercomputer ist dies der Verbindungsport für den ersten virtuellen Mastercomputer.
    > 

Die folgenden Abschnitte enthalten Beispiele für DC/OS und Swarm.    

### <a name="dcos-tunnel"></a>DC/OS-Tunnel
Öffnen Sie einen Tunnel zu den Endpunkten von DC/OS, indem Sie beispielsweise den folgenden Befehl ausführen:

```bash
sudo ssh -L 80:localhost:80 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Nun können Sie wie folgt auf die zu DC/OS gehörigen Endpunkte zugreifen:

* DC/OS: `http://localhost/`
* Marathon: `http://localhost/marathon`
* Mesos: `http://localhost/mesos`

Sie erreichen die REST-APIs für jede Anwendung über diesen Tunnel auf ähnliche Weise:

### <a name="swarm-tunnel"></a>Swarm-Tunnel
Führen Sie zum Öffnen eines Tunnels zum Swarm-Endpunkt einen Befehl wie diesen aus:

```bash
ssh -L 2375:localhost:2375 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Sie können die DOCKER_HOST-Umgebungsvariable jetzt wie folgt festlegen: Sie können Ihre Docker-Befehlszeilenschnittstelle (CLI) wie gewohnt weiterverwenden.

```bash
export DOCKER_HOST=:2375
```

### <a name="create-an-ssh-tunnel-on-windows"></a>Erstellen eines SSH-Tunnels unter Windows
Es gibt mehrere Möglichkeiten, wie Sie SSH-Tunnel unter Windows erstellen können. In diesem Abschnitt wird beschrieben, wie Sie PuTTY zum Erstellen des Tunnels verwenden.

1. [Laden Sie PuTTY auf Ihr Windows-System herunter](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

2. Führen Sie die Anwendung aus.

3. Geben Sie einen Hostnamen ein, der aus dem Administratorbenutzernamen für den Cluster und dem öffentlichen DNS-Namen für den ersten Master im Cluster besteht. Der **Hostname** hat das Format `adminuser@PublicDNSName`. Geben Sie 2200 als **Port**ein.

    ![PuTTY-Konfiguration 1](media/putty1.png)

4. Wählen Sie **SSH > Auth**. Fügen Sie der Datei mit dem privaten Schlüssel (PPK-Format) für die Authentifizierung einen Pfad hinzu. Sie können ein Tool wie [PuTTYgen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) nutzen, um diese Datei aus dem SSH-Schlüssel zu generieren, der zum Erstellen des Clusters verwendet wurde.

    ![PuTTY-Konfiguration 2](media/putty2.png)

5. Wählen Sie **SSH > Tunnel**, und konfigurieren Sie die folgenden weitergeleiteten Ports:

    * **Quellport:** Verwenden Sie 80 für DC/OS und 2375 für Swarm.
    * **Ziel**: Verwenden Sie „localhost:80“ für DC/OS bzw. „localhost:2375“ für Swarm.

    Das folgende Beispiel ist für DC/OS konfiguriert, sieht für Docker Swarm aber ähnlich aus.

    > [!NOTE]
    > Port 80 darf beim Erstellen dieses Tunnels nicht verwendet werden.
    > 

    ![PuTTY-Konfiguration 3](media/putty3.png)

6. Klicken Sie nach Abschluss der Schritte auf **Sitzung > Speichern**, um die Verbindungskonfiguration zu speichern.

7. Klicken Sie zum Herstellen der Verbindung mit der PuTTY-Sitzung auf **Öffnen**. Beim Herstellen der Verbindung können Sie die Portkonfiguration im PuTTY-Ereignisprotokoll anzeigen.

    ![PuTTY-Ereignisprotokoll](media/putty4.png)

Nachdem Sie den Tunnel für DC/OS konfiguriert haben, können Sie wie folgt auf den zugehörigen Endpunkt zugreifen:

* DC/OS: `http://localhost/`
* Marathon: `http://localhost/marathon`
* Mesos: `http://localhost/mesos`

Öffnen Sie nach dem Konfigurieren des Tunnels für Docker Swarm Ihre Windows-Einstellungen, um eine Systemumgebungsvariable mit dem Namen `DOCKER_HOST` mit dem Wert `:2375` zu konfigurieren. Anschließend können Sie über die Docker CLI auf den Swarm-Cluster zugreifen.

## <a name="next-steps"></a>Nächste Schritte
Bereitstellen und Verwalten von Containern in Ihrem Cluster:

* [Microsoft Azure Container Service Engine - Kubernetes Walkthrough (Microsoft Azure Container Service-Modul – Exemplarische Vorgehensweise für Kubernetes)](container-service-kubernetes-ui.md)
* [Verwenden von Azure Container Service und DC/OS](container-service-mesos-marathon-rest.md)
* [Verwenden von Azure Container Service und Docker Swarm](container-service-docker-swarm.md)




<!--HONumber=Jan17_HO3-->


