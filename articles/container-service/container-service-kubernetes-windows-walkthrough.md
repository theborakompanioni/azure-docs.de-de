---
title: "Azure Kubernetes-Cluster für Windows | Microsoft-Dokumentation"
description: "Bereitstellung und erste Schritte für ein Kubernetes-Cluster für Windows-Container in Azure Container Service"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/20/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: eb3af43b8a13eaaebfa9147848383ff889119d97
ms.lasthandoff: 04/03/2017


---

# <a name="get-started-with-kubernetes-and-windows-containers-in-container-service"></a>Erste Schritte mit Kubernetes und Windows-Containern in Container Service


Dieser Artikel zeigt, wie Sie einen Kubernetes-Cluster in Azure Container Service erstellen, der Windows-Knoten zum Ausführen von Windows-Containern enthält. 

> [!NOTE]
> Die Unterstützung für Windows-Container mit Kubernetes in Azure Container Service befindet sich in der Vorschauphase. Verwenden Sie das Azure-Portal oder eine Resource Manager-Vorlage, um einen Kubernetes-Cluster mit Windows-Knoten zu erstellen. Diese Funktion wird für Azure CLI 2.0 derzeit nicht unterstützt.
>



In der folgenden Abbildung ist die Architektur eines Kubernetes-Clusters in Azure Container Service mit einem Linux-Masterknoten und zwei Windows-Agentknoten dargestellt. 

* Der Linux-Master sorgt für die Bereitstellung der Kubernetes-REST-API und ist per SSH über Port 22 oder per `kubectl` über Port 443 zugänglich. 
* Die Windows-Agentknoten werden in einer Azure-Verfügbarkeitsgruppe gruppiert und dienen zum Ausführen Ihrer Container. Auf die Windows-Knoten kann über einen RDP-SSH-Tunnel per Masterknoten zugegriffen werden. Azure-Lastenausgleichsregeln werden dem Cluster je nach den verfügbar gemachten Diensten hinzugefügt.


![Abbildung: Kubernetes-Cluster in Azure](media/container-service-kubernetes-windows-walkthrough/kubernetes-windows.png)

Alle VMs befinden sich in demselben privaten virtuellen Netzwerk, und untereinander besteht Vollzugriff. Für alle VMs werden ein Kubelet, Docker und ein Proxy ausgeführt.

## <a name="prerequisites"></a>Voraussetzungen


* **Öffentlicher SSH-RSA-Schlüssel**: Bei der Bereitstellung über das Portal oder eine der Azure-Schnellstartvorlagen müssen Sie einen öffentlichen SSH-RSA-Schlüssel angeben, der zur Authentifizierung für virtuelle Azure Container Service-Computer verwendet werden soll. Informationen zum Erstellen von Secure Shell (SSH)-RSA-Schlüsseln finden Sie in den Anleitungen für [OS X und Linux](../virtual-machines/linux/mac-create-ssh-keys.md) oder für [Windows](../virtual-machines/linux/ssh-from-windows.md). 

* **Client-ID und Geheimnis des Dienstprinzipals**: Weitere Informationen und eine Anleitung finden Sie unter [Dienstprinzipal für einen Kubernetes-Cluster](container-service-kubernetes-service-principal.md).




## <a name="create-the-cluster"></a>Cluster erstellen

Sie können das Azure-Portal zum [Erstellen eines Kubernetes-Clusters](container-service-deployment.md#create-a-cluster-by-using-the-azure-portal) mit Windows-Agentknoten verwenden. Beachten Sie beim Erstellen des Clusters die folgenden Einstellungen:

* Wählen Sie auf dem Blatt **Grundlagen** in **Orchestrator** die Option **Kubernetes**. 

  ![Wählen des Kubernetes-Orchestrators](media/container-service-kubernetes-windows-walkthrough/portal-select-kubernetes.png)

* Geben Sie auf dem Blatt **Masterkonfiguration** die Benutzeranmeldeinformationen und Dienstprinzipal-Anmeldeinformationen für die Linux-Masterknoten ein. Wählen Sie einen, drei oder fünf Master.

* Wählen Sie auf dem Blatt **Agent-Konfiguration** unter **Betriebssystem** die Option **Windows (Vorschau)**. Geben Sie die Administratoranmeldeinformationen für die Windows-Agent-Knoten ein.

  ![Auswählen von Windows-Agents](media/container-service-kubernetes-windows-walkthrough/portal-select-windows.png)

Weitere Details finden Sie unter [Bereitstellen eines Azure Container Service-Clusters](container-service-deployment.md).

## <a name="connect-to-the-cluster"></a>Verbinden mit dem Cluster

Verwenden Sie das Befehlszeilentool `kubectl`, um von Ihrem lokalen Computer aus eine Verbindung mit dem Masterknoten des Kubernetes-Clusters herzustellen. Schritte zum Installieren und Einrichten von `kubectl` finden Sie unter [Verbinden mit einem Azure Container Service-Cluster](container-service-connect.md#connect-to-a-kubernetes-cluster). Sie können `kubectl`-Befehle zum Zugreifen auf die Kubernetes-Webbenutzeroberfläche und zum Erstellen und Verwalten von Windows-Containerworkloads verwenden.

## <a name="create-your-first-kubernetes-service"></a>Erstellen des ersten Kubernetes-Diensts

Nach dem Erstellen des Clusters und dem Herstellen der Verbindung mit `kubectl` können Sie versuchen, eine grundlegende Windows-Web-App zu starten und für den Zugriff über das Internet verfügbar zu machen. In diesem Beispiel geben Sie die Containerressourcen mit einer YAML-Datei an und erstellen sie dann mit `kubctl apply`.

1. Geben Sie `kubectl get nodes` ein, um eine Liste mit Ihren Knoten anzuzeigen. Geben Sie Folgendes ein, wenn Sie alle Details der Knoten anzeigen möchten:  

    ```
    kubectl get nodes -o yaml
    ```

2. Erstellen Sie eine Datei mit dem Namen `simpleweb.yaml`, und kopieren Sie Folgendes: Mit dieser Datei wird eine Web-App mit dem Windows Server 2016 Server Core-Betriebssystem-Basisimage von [Docker Hub](https://hub.docker.com/r/microsoft/windowsservercore/) eingerichtet.  

```yaml
  apiVersion: v1
  kind: Service
  metadata:
    name: win-webserver
    labels:
      app: win-webserver
  spec:
    ports:
      # the port that this service should serve on
    - port: 80
      targetPort: 80
    selector:
      app: win-webserver
    type: LoadBalancer
  ---
  apiVersion: extensions/v1beta1
  kind: Deployment
  metadata:
    labels:
      app: win-webserver
    name: win-webserver
  spec:
    replicas: 1
    template:
      metadata:
        labels:
          app: win-webserver
        name: win-webserver
      spec:
        containers:
        - name: windowswebserver
          image: microsoft/windowsservercore
          command:
          - powershell.exe
          - -command
          - "<#code used from https://gist.github.com/wagnerandrade/5424431#> ; $$listener = New-Object System.Net.HttpListener ; $$listener.Prefixes.Add('http://*:80/') ; $$listener.Start() ; $$callerCounts = @{} ; Write-Host('Listening at http://*:80/') ; while ($$listener.IsListening) { ;$$context = $$listener.GetContext() ;$$requestUrl = $$context.Request.Url ;$$clientIP = $$context.Request.RemoteEndPoint.Address ;$$response = $$context.Response ;Write-Host '' ;Write-Host('> {0}' -f $$requestUrl) ;  ;$$count = 1 ;$$k=$$callerCounts.Get_Item($$clientIP) ;if ($$k -ne $$null) { $$count += $$k } ;$$callerCounts.Set_Item($$clientIP, $$count) ;$$header='<html><body><H1>Windows Container Web Server</H1>' ;$$callerCountsString='' ;$$callerCounts.Keys | % { $$callerCountsString+='<p>IP {0} callerCount {1} ' -f $$_,$$callerCounts.Item($$_) } ;$$footer='</body></html>' ;$$content='{0}{1}{2}' -f $$header,$$callerCountsString,$$footer ;Write-Output $$content ;$$buffer = [System.Text.Encoding]::UTF8.GetBytes($$content) ;$$response.ContentLength64 = $$buffer.Length ;$$response.OutputStream.Write($$buffer, 0, $$buffer.Length) ;$$response.Close() ;$$responseStatus = $$response.StatusCode ;Write-Host('< {0}' -f $$responseStatus)  } ; "
        nodeSelector:
          beta.kubernetes.io/os: windows
  ```

      
> [!NOTE] 
> Die Konfiguration enthält `type: LoadBalancer`. Diese Einstellung bewirkt, dass der Dienst über einen Azure Load Balancer (Lastenausgleich) für den Zugriff über das Internet verfügbar gemacht wird. Weitere Informationen finden Sie unter [Lastenausgleich für Container in einem Kubernetes-Cluster in Azure Container Service](container-service-kubernetes-load-balancing.md).
>

## <a name="start-the-application"></a>Starten der Anwendung

1. Geben Sie Folgendes ein, um die Anwendung zu starten:  

    ```
    kubectl apply -f simpleweb.yaml
    ```  
  
  
2. Geben Sie Folgendes ein, um die Bereitstellung des Diensts (Dauer ca. 30 Sekunden) zu überprüfen:  

    ```
    kubectl get pods
    ```

3. Geben Sie Folgendes ein, um bei ausgeführtem Dienst die internen und externen IP-Adressen anzuzeigen:

    ```
    kubectl get svc
    ``` 
  
    ![IP-Adressen des Windows-Diensts](media/container-service-kubernetes-windows-walkthrough/externalipa.png)

    Das Hinzufügen der externen IP-Adresse dauert mehrere Minuten. Bevor die externe Adresse vom Lastenausgleich konfiguriert wird, wird dafür `<pending>` angezeigt.

4. Wenn die externe IP-Adresse verfügbar ist, ist der Dienst über Ihren Webbrowser erreichbar.

    ![Windows-Server-App im Browser](media/container-service-kubernetes-windows-walkthrough/wincontainerwebserver.png)


## <a name="access-the-windows-nodes"></a>Zugreifen auf die Windows-Knoten
Auf Windows-Knoten kann über einen lokalen Windows-Computer per Remotedesktopverbindung zugegriffen werden. Es wird empfohlen, einen RDP-SSH-Tunnel über den Masterknoten zu verwenden. 

Es gibt mehrere Möglichkeiten, wie Sie SSH-Tunnel unter Windows erstellen können. In diesem Abschnitt wird beschrieben, wie Sie PuTTY zum Erstellen des Tunnels verwenden.

1. [Laden Sie PuTTY auf Ihr Windows-System herunter](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

2. Führen Sie die Anwendung aus.

3. Geben Sie einen Hostnamen ein, der aus dem Administratorbenutzernamen für den Cluster und dem öffentlichen DNS-Namen für den ersten Master im Cluster besteht. Der **Hostname** hat das Format `adminuser@PublicDNSName`. Geben Sie 22 als **Port** ein.

  ![PuTTY-Konfiguration 1](media/container-service-kubernetes-windows-walkthrough/putty1.png)

4. Wählen Sie **SSH > Auth**. Fügen Sie der Datei mit dem privaten Schlüssel (PPK-Format) für die Authentifizierung einen Pfad hinzu. Sie können ein Tool wie [PuTTYgen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) nutzen, um diese Datei aus dem SSH-Schlüssel zu generieren, der zum Erstellen des Clusters verwendet wurde.

  ![PuTTY-Konfiguration 2](media/container-service-kubernetes-windows-walkthrough/putty2.png)

5. Wählen Sie **SSH > Tunnel**, und konfigurieren Sie die weitergeleiteten Ports. Da für Ihren lokalen Windows-Computer bereits Port 3389 verwendet wird, wird die Verwendung der folgenden Einstellungen empfohlen, um Windows-Knoten 0 und Windows-Knoten 1 zu erreichen. (Behalten Sie dieses Muster für weitere Windows-Knoten bei.)

    **Windows-Knoten 0**

    * **Quellport:** 3390
    * **Ziel:** 10.240.245.5:3389

    **Windows-Knoten 1**

    * **Quellport:** 3391
    * **Ziel:** 10.240.245.6:3389

    ![Image von Windows-RDP-Tunneln](media/container-service-kubernetes-windows-walkthrough/rdptunnels.png)

6. Klicken Sie nach Abschluss der Schritte auf **Sitzung > Speichern**, um die Verbindungskonfiguration zu speichern.

7. Klicken Sie zum Herstellen der Verbindung mit der PuTTY-Sitzung auf **Öffnen**. Stellen Sie die Verbindung mit dem Masterknoten her.

8. Starten Sie die Remotedesktopverbindung. Geben Sie zum Herstellen der Verbindung mit dem ersten Windows-Knoten für **Computer** die Daten `localhost:3390` an, und klicken Sie auf **Verbinden**. (Geben Sie zum Herstellen der Verbindung mit dem zweiten Knoten `localhost:3390` an usw.) Geben Sie als Abschluss für Ihre Verbindung das Kennwort des lokalen Windows-Administrators ein, das Sie während der Bereitstellung konfiguriert haben.


## <a name="next-steps"></a>Nächste Schritte

Unter diesen empfohlenen Links finden Sie weitere Informationen zu Kubernetes:

* [Kubernetes Bootcamp](https://kubernetesbootcamp.github.io/kubernetes-bootcamp/index.html): Es wird veranschaulicht, wie Sie Anwendungen in Containern bereitstellen, skalieren, aktualisieren und debuggen.
* [Kubernetes User Guide](http://kubernetes.io/docs/user-guide/) (Kubernetes-Benutzerhandbuch): Enthält Informationen zum Ausführen von Programmen in einem vorhandenen Kubernetes-Cluster.
* [Kubernetes Examples](https://github.com/kubernetes/kubernetes/tree/master/examples) (Kubernetes-Beispiele): Enthält Beispiele für die Ausführung von echten Anwendungen mit Kubernetes.
