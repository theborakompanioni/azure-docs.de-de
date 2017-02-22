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
ms.date: 01/30/2017
ms.author: rogardle
translationtype: Human Translation
ms.sourcegitcommit: 2464c91b99d985d7e626f57b2d77a334ee595f43
ms.openlocfilehash: 813517a26ccbbd9df7e7fb7de36811cdebb84284


---
# <a name="connect-to-an-azure-container-service-cluster"></a>Verbinden mit einem Azure Container Service-Cluster
Nach dem Erstellen eines Azure Container Service-Clusters müssen Sie eine Verbindung mit dem Cluster herstellen, um Workloads bereitzustellen und zu verwalten. In diesem Artikel wird beschrieben, wie Sie von einem Remotecomputer aus eine Verbindung mit dem virtuellen Mastercomputer des Clusters herstellen. 

Die Kubernetes-, DC/OS- und Docker Swarm-Cluster stellen HTTP-Endpunkte lokal bereit. Für Kubernetes wird dieser Endpunkt auf sichere Weise im Internet verfügbar gemacht, und Sie können darauf zugreifen, indem Sie das Befehlszeilentool `kubectl` auf einem Computer mit Internetverbindung ausführen. 

Für DC/OS und Docker Swarm müssen Sie einen SSH-Tunnel (Secure Shell) zu einem internen System erstellen. Nachdem der Tunnel eingerichtet wurde, können Sie Befehle mit den HTTP-Endpunkten verwenden und die Weboberfläche des Clusters über Ihr lokales System anzeigen. 

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
Eine Möglichkeit zum Installieren dieses Tools ist der Befehl `az acs kubernetes install-cli` der Azure-CLI 2.0 (Vorschau). Stellen Sie zum Ausführen dieses Befehls sicher, dass Sie die aktuelle Version von Azure-CLI 2.0 (Vorschau) [installiert](/cli/azure/install-az-cli2) haben und an einem Azure-Konto (`az login`) angemeldet sind.

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
mkdir $HOME/.kube
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

Wenn Sie die von Azure Container Service bereitgestellten DC/OS- und Docker Swarm-Cluster verwenden möchten, erstellen Sie in Ihrem lokalen Linux-, OS X- oder Windows-System gemäß der folgenden Anleitung einen SSH-Tunnel (Secure Shell). 

> [!NOTE]
> Diese Anleitung konzentriert sich auf die Übertragung von TCP-Datenverkehr über einen SSH-Tunnel. Sie können auch eine interaktive SSH-Sitzung mit einem der internen Clusterverwaltungssysteme starten, dies wird jedoch nicht empfohlen. Bei der direkten Bearbeitung eines internen Systems besteht die Gefahr unbeabsichtigter Konfigurationsänderungen.  
> 

### <a name="create-an-ssh-tunnel-on-linux-or-os-x"></a>Erstellen eines SSH-Tunnels unter Linux oder OS X
Als Erstes ermitteln Sie beim Erstellen eines SSH-Tunnels unter Linux oder OS X den öffentlichen DNS-Namen von Masterelementen mit Lastenausgleich. Folgen Sie diesen Schritten:


1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu der Ressourcengruppe, die Ihren Container Service-Cluster enthält. Erweitern Sie die Ressourcengruppe, damit jede Ressource angezeigt wird. 

2. Klicken Sie auf die Containerdienstressource und anschließend auf **Übersicht**. Der **Master-FQDN** des Clusters wird unter **Zusammenfassung** angezeigt. Speichern Sie diesen Namen für die spätere Verwendung. 

    ![Öffentlicher DNS-Name](media/pubdns.png)

    Führen Sie alternativ den Befehl `az acs show` für Ihren Containerdienst aus. Suchen Sie in der Befehlsausgabe nach der Eigenschaft **Master Profile:fqdn**.

3. Öffnen Sie als Nächstes eine Shell, und führen Sie den Befehl `ssh` aus, indem Sie die folgenden Werte angeben: 

    **LOCAL_PORT** ist der dienstseitige TCP-Port des Tunnels, mit dem eine Verbindung hergestellt werden soll. Legen Sie diesen Wert für Swarm auf „2375“ fest. Legen Sie ihn für DC/OS auf „80“ fest.  
    **REMOTE_PORT** ist der Port des Endpunkts, den Sie verfügbar machen möchten. Verwenden Sie für Swarm Port 2375. Verwenden Sie für DC/OS Port 80.  
    **BENUTZERNAME** ist der Benutzername, der bei der Bereitstellung des Clusters angegeben wurde.  
    **DNSPREFIX** ist das DNS-Präfix, das bei der Bereitstellung des Clusters angegeben wurde.  
    **REGION** ist die Region, in der sich die Ressourcengruppe befindet.  
    **PATH_TO_PRIVATE_KEY** [OPTIONAL] ist der Pfad zum privaten Schlüssel, der zu dem öffentlichen Schlüssel passt, den Sie beim Erstellen des Clusters angegeben haben. Verwenden Sie diese Option mit dem Flag `-i`.

    ```bash
    ssh -fNL PORT:localhost:PORT -p 2200 [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com 
    ```
    > [!NOTE]
    > Für die SSH-Verbindung wird Port 2200 und nicht der Standardport 22 verwendet. In einem Cluster mit mehr als einem virtuellen Mastercomputer ist dies der Verbindungsport für den ersten virtuellen Mastercomputer.
    > 



Die folgenden Abschnitte enthalten Beispiele für DC/OS und Swarm.    

### <a name="dcos-tunnel"></a>DC/OS-Tunnel
Führen Sie einen Befehl wie den folgenden aus, um einen Tunnel für DC/OS-Endpunkte zu öffnen:

```bash
sudo ssh -fNL 80:localhost:80 -p 2200 azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com 
```

> [!NOTE]
> Anstelle von Port 80 können Sie auch einen anderen lokalen Port (beispielsweise Port 8888) angeben. Bei Verwendung dieses Ports funktionieren jedoch einige Links auf Webbenutzeroberflächen unter Umständen nicht.

Sie können nun auf Ihrem lokalen System über die folgenden URLs auf die DC/OS-Endpunkte zugreifen (bei Verwendung des lokalen Ports 80):

* DC/OS: `http://localhost:80/`
* Marathon: `http://localhost:80/marathon`
* Mesos: `http://localhost:80/mesos`

Sie erreichen die REST-APIs für jede Anwendung über diesen Tunnel auf ähnliche Weise:

### <a name="swarm-tunnel"></a>Swarm-Tunnel
Führen Sie einen Befehl wie den folgenden aus, um einen Tunnel zum Swarm-Endpunkt zu öffnen:

```bash
ssh -fNL 2375:localhost:2375 -p 2200 azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com
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




<!--HONumber=Jan17_HO5-->


