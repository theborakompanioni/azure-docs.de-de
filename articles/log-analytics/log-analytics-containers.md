---
title: "Containerlösung in Azure Log Analytics | Microsoft-Dokumentation"
description: "Die Containerlösung in Log Analytics unterstützt Sie beim Anzeigen und Verwalten Ihrer Docker- und Windows-Containerhosts an einem Ort."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: e1e4b52b-92d5-4bfa-8a09-ff8c6b5a9f78
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: magoedte;banders
ms.translationtype: HT
ms.sourcegitcommit: 0aae2acfbf30a77f57ddfbaabdb17f51b6938fd6
ms.openlocfilehash: 5fe0c4c5642fcaa83bcfc830e64600986b8fbf7f
ms.contentlocale: de-de
ms.lasthandoff: 08/09/2017

---
# <a name="containers-preview-solution-in-log-analytics"></a>Containerlösung (Vorschauversion) in Log Analytics

![Container-Symbol](./media/log-analytics-containers/containers-symbol.png)

In diesem Artikel wird das Einrichten und Verwenden der Containerlösung in Log Analytics beschrieben, die Sie beim Anzeigen und Verwalten Ihrer Docker- und Windows-Containerhosts an einem zentralen Ort unterstützt. Docker ist ein Softwarevirtualisierungssystem zum Erstellen von Containern, durch die das Bereitstellen von Software in einer IT-Infrastruktur automatisiert werden kann.

Mit der Lösung können Sie erkennen, welche Container auf Ihren Containerhosts ausgeführt und welche Images in den Containern ausgeführt werden. Sie können ausführliche Überwachungsinformationen anzeigen, die auch die mit Containern verwendeten Befehle enthalten. Außerdem können Sie die Probleme mit Containern beheben, indem Sie zentralisierte Protokolle anzeigen und durchsuchen, ohne eine Remoteanzeige der Docker- und Windows-Hosts zu benötigen. Sie können Container suchen, die Störungen verursachen und übermäßig viele Ressourcen auf einem Host verbrauchen. Darüber hinaus können Sie an einem zentralen Ort Informationen zur Leistung und Auslastung von CPU, Arbeitsspeicher, Speicher und Netzwerk zu Containern anzeigen. Auf Windows-Computern können Sie die Protokolle von Windows Server-, Hyper-V- und Docker-Containern zentralisieren und vergleichen.

Das folgende Diagramm zeigt die Beziehungen zwischen verschiedenen Containerhosts und Agents mit OMS.

![Containerdiagramm](./media/log-analytics-containers/containers-diagram.png)

## <a name="system-requirements"></a>Systemanforderungen
Prüfen Sie zunächst anhand der folgenden Informationen, ob die Voraussetzungen erfüllt sind.

### <a name="container-monitoring-solution-support-for-docker-orchestrator-and-os-platform"></a>Unterstützung der Containerüberwachungslösung für Docker Orchestrator und das Betriebssystem 
In der folgenden Tabelle werden die Docker-Orchestrierung und die Unterstützung der Betriebssystemüberwachung von Containerbestand, Leistung und Protokollen mit Log Analytics aufgeführt.   

| | ACS | Linux | Windows | Container<br>Inventory | Image<br>Inventory | Knoten<br>Inventory | Container<br>Leistung | Container<br>Ereignis | Ereignis<br>Protokoll | Container<br>Protokoll | 
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| kubernetes | Ja | Ja | | Ja | Ja | Ja | Ja | Ja | Ja | Ja | 
| Mesosphere<br>DC/OS | Ja | Ja | | Ja | Ja | Ja | Ja| Ja | Ja | Ja | 
| Docker<br>Swarm | Ja | Ja | Ja | Ja | Ja | Ja | Ja | Ja | | Ja |
| Dienst<br>Fabric | | | Ja | Ja | Ja | Ja | Ja | Ja | Ja | Ja | 
| Red Hat Open<br>Shift | | Ja | | Ja | Ja| Ja | Ja | Ja | | Ja | 
| Windows Server<br>(eigenständig) | | | Ja | Ja | Ja | Ja | Ja | Ja | | Ja |
| Linux-Server<br>(eigenständig) | | Ja | | Ja | Ja | Ja | Ja | Ja | | Ja |


### <a name="docker-versions-supported-on-linux"></a>Unter Linux unterstützte Docker-Versionen

- Docker 1.11 bis 1.13
- Docker CE und EE v17.06

### <a name="x64-linux-distributions-supported-as-container-hosts"></a>Als Containerhosts unterstützte x64-Linux-Distributionen

- Ubuntu 14.04 LTS und 16.04 LTS
- CoreOS (Stable)
- Amazon Linux 2016.09.0
- openSUSE 13.2
- openSUSE LEAP 42.2
- CentOS 7.2 und 7.3
- SLES 12
- RHEL 7.2 und 7.3
- Red Hat OpenShift Container Platform (OCP) 3.4 und 3.5
- ACS Mesosphere DC/OS 1.7.3 bis 1.8.8
- ACS Kubernetes 1.4.5 bis 1.6
- ACS Docker Swarm

### <a name="supported-windows-operating-system"></a>Unterstützte Windows-Betriebssysteme

- Windows Server 2016
- Windows 10 Anniversary Edition (Professional oder Enterprise)

### <a name="docker-versions-supported-on-windows"></a>Unter Windows unterstützte Docker-Versionen

- Docker 1.12 und 1.13
- Docker 17.03.0 

## <a name="installing-and-configuring-the-solution"></a>Installieren und Konfigurieren der Lösung
Verwenden Sie die folgenden Informationen zum Installieren und Konfigurieren der Lösung.

1. Fügen Sie mithilfe des [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ContainersOMS?tab=Overview) oder des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md) beschriebenen Prozesses Ihrem OMS-Arbeitsbereich die Containerlösung hinzu.

2. Installieren und verwenden Sie Docker mit OMS.  Basierend auf dem Betriebssystem können Sie unter den folgenden Methoden wählen:

  * Auf unterstützten Linux-Betriebssystemen installieren Sie zunächst Docker und führen es aus. Anschließend installieren und konfigurieren Sie den OMS-Agent für Linux.  
  * Unter CoreOS kann der OMS-Agent für Linux nicht ausgeführt werden. Stattdessen führen Sie eine Containerversion des OMS-Agents für Linux aus. Lesen Sie [Linux-Containerhosts mit CoreOS](#for-all-linux-container-hosts-including-coreos) oder [Azure Government-Linux-Containerhosts mit CoreOS](#for-all-azure-government-linux-container-hosts-including-coreos), wenn Sie in Azure Government Cloud mit Containern arbeiten.
  * Installieren Sie unter Windows Server 2016 und Windows 10 das Docker-Modul und den Client, und stellen Sie dann eine Verbindung mit einem Agent her, um Informationen zu sammeln und sie an Log Analytics zu senden.  

### <a name="container-services"></a>Containerdienste

- Falls Sie über eine Red Hat OpenShift-Umgebung verfügen, helfen Ihnen die Informationen unter [Konfigurieren eines OMS-Agents für Red Hat OpenShift](#configure-an-oms-agent-for-red-hat-openshift) weiter.
- Wenn Sie über einen Kubernetes-Cluster verfügen, der den Azure Container Service nutzt, helfen Ihnen die Informationen unter [Überwachen eines Azure Container Service-Clusters mit Microsoft Operations Management Suite (OMS)](../container-service/kubernetes/container-service-kubernetes-oms.md) weiter.
- Wenn Sie über einen DC/OS-Cluster in Azure Container Service verfügen, finden Sie unter [Überwachen eines Azure Container Service-DC/OS-Clusters mit Operations Management Suite](../container-service/dcos-swarm/container-service-monitoring-oms.md) weitere Informationen.
- Wenn Sie über eine Docker Swarm-Modus-Umgebung verfügen, erhalten Sie weitere Informationen unter [Konfigurieren eines OMS-Agent für Docker Swarm](#configure-an-oms-agent-for-docker-swarm).
- Wenn Sie Container mit Service Fabric verwenden, helfen Ihnen die Informationen unter [Übersicht über Azure Service Fabric ](../service-fabric/service-fabric-overview.md) weiter.
- Weitere Informationen zum Installieren und Konfigurieren von Docker-Modulen auf Windows-Computern finden Sie im Artikel [Docker-Modul unter Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon).

> [!IMPORTANT]
> Docker muss ausgeführt werden, **bevor** Sie den [OMS-Agent für Linux](log-analytics-agent-linux.md) auf Ihren Containerhosts installieren. Wenn Sie den Agent bereits vor der Installation von Docker installiert haben, müssen Sie den OMS-Agent für Linux erneut installieren. Weitere Informationen zu Docker finden Sie auf der [Docker-Website](https://www.docker.com).
>
>

Sie müssen die folgenden Einstellungen auf den Containerhosts konfigurieren, bevor Sie Container überwachen können.

## <a name="linux-container-hosts"></a>Linux-Containerhosts

Verwenden Sie nach dem Installieren von Docker die folgenden Einstellungen für den Containerhost, um den Agent für die Verwendung mit Docker zu konfigurieren. Zuerst benötigen Sie Ihre ID und den Schlüssel für den OMS-Arbeitsbereich. Diese Daten erhalten Sie, indem Sie zum [klassischen OMS-Portal](https://mms.microsoft.com) wechseln.  Wählen Sie auf der Seite **Übersicht** im obersten Menü die Option **Einstellungen**, und navigieren Sie anschließend zu **Verbundene Quellen\Windows-Server**.  Der Wert wird jeweils rechts von **Arbeitsbereichs-ID** und **Primärschlüssel** angezeigt.  Kopieren Sie beide Angaben, und fügen Sie sie in den von Ihnen bevorzugten Editor ein.    

### <a name="for-all-linux-container-hosts-except-coreos"></a>Für alle Linux-Containerhosts mit Ausnahme von CoreOS

- Weitere Informationen und Anleitungen zum Installieren des OMS-Agents für Linux finden Sie unter [Verbinden Ihrer Linux-Computer mit Operations Management Suite (OMS)](log-analytics-agent-linux.md).

### <a name="for-all-linux-container-hosts-including-coreos"></a>Für alle Linux-Containerhosts mit CoreOS

Starten Sie den OMS-Container, den Sie überwachen möchten. Verwenden Sie das folgende Beispiel, und passen Sie es an:

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -e WSID="your workspace id" -e KEY="your key" -h=`hostname` -p 127.0.0.1:25225:25225 --name="omsagent" --restart=always microsoft/oms
```

### <a name="for-all-azure-government-linux-container-hosts-including-coreos"></a>Für alle Azure Government-Linux-Containerhosts mit CoreOS

Starten Sie den OMS-Container, den Sie überwachen möchten. Verwenden Sie das folgende Beispiel, und passen Sie es an:

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -v /var/log:/var/log -e WSID="your workspace id" -e KEY="your key" -e DOMAIN="opinsights.azure.us" -p 127.0.0.1:25225:25225 -p 127.0.0.1:25224:25224/udp --name="omsagent" -h=`hostname` --restart=always microsoft/oms
```

### <a name="switching-from-using-an-installed-linux-agent-to-one-in-a-container"></a>Wechseln von der Verwendung eines installierten Linux-Agents zu einem in einem Container enthaltenen
Wenn Sie zuvor den direkt installierten Agent verwendet haben und stattdessen einen Agent verwenden möchten, der in einem Container ausgeführt wird, müssen Sie zunächst den OMS-Agent für Linux entfernen. Informationen zum erfolgreichen Deinstallieren des Agents finden Sie unter [Deinstallieren des OMS-Agents für Linux](log-analytics-agent-linux.md#uninstalling-the-oms-agent-for-linux).  

### <a name="configure-an-oms-agent-for-docker-swarm"></a>Konfigurieren eines OMS-Agent für Docker Swarm

Sie können den OMS-Agent als globalen Dienst in Docker Swarm ausführen. Erstellen Sie mithilfe der folgenden Informationen einen OMS-Agent-Dienst. Sie müssen Ihre OMS-Arbeitsbereichs-ID und den Primärschlüssel einfügen.

- Führen Sie auf dem Masterknoten Folgendes aus.

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock  -e WSID="<WORKSPACE ID>" -e KEY="<PRIMARY KEY>" -p 25225:25225 -p 25224:25224/udp  --restart-condition=on-failure microsoft/oms
    ```

### <a name="configure-an-oms-agent-for-red-hat-openshift"></a>Konfigurieren eines OMS-Agents für Red Hat OpenShift
Es gibt drei Möglichkeiten, wie Sie Red Hat OpenShift den OMS-Agent hinzufügen können, um mit dem Sammeln von Daten für die Containerüberwachung zu beginnen. 
 
* [Installieren des OMS-Agents für Linux](log-analytics-agent-linux.md) direkt auf jedem OpenShift-Knoten  
* [Aktivieren der Log Analytics-VM-Erweiterung](log-analytics-azure-vm-extension.md) auf jedem OpenShift-Knoten in Azure  
* Installieren des OMS-Agents als OpenShift-DaemonSet  

In diesem Abschnitt werden die Schritte beschrieben, die zum Installieren des OMS-Agents als OpenShift-DaemonSet erforderlich sind.  

1. Melden Sie sich am OpenShift-Masterknoten an, und kopieren Sie die YAML-Datei [ocp-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-omsagent.yaml) aus GitHub auf Ihren Masterknoten. Nehmen Sie die Änderung mit Ihrer OMS-Arbeitsbereich-ID und dem Primärschlüssel vor.
2. Führen Sie die folgenden Befehle aus, um ein Projekt für OMS zu erstellen, und legen Sie das Benutzerkonto fest.

    ```
    oadm new-project omslogging --node-selector='zone=default'
    oc project omslogging  
    oc create serviceaccount omsagent  
    oadm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

4. Führen Sie Folgendes aus, um das DaemonSet bereitzustellen: 
    
    `oc create -f ocp-omsagent.yaml`

5. Geben Sie Folgendes ein, um zu überprüfen, ob alles richtig konfiguriert ist und funktioniert: 

    `oc describe daemonset omsagent`  
    
    Die Ausgabe sollte in etwa wie folgt aussehen:

    ```
    [ocpadmin@khm-0 ~]$ oc describe ds oms  
    Name:           oms  
    Image(s):       microsoft/oms  
    Selector:       name=omsagent  
    Node-Selector:  zone=default  
    Labels:         agentVersion=1.4.0-12  
                    dockerProviderVersion=10.0.0-25  
                    name=omsagent  
    Desired Number of Nodes Scheduled: 3  
    Current Number of Nodes Scheduled: 3  
    Number of Nodes Misscheduled: 0  
    Pods Status:    3 Running / 0 Waiting / 0 Succeeded / 0 Failed  
    No events.  
    ```

Führen Sie die folgenden Schritte aus, falls Sie Geheimnisse nutzen möchten, um beim Verwenden der YAML-Datei des OMS-Agent-DaemonSet Ihre OMS-Arbeitsbereich-ID und den Primärschlüssel zu schützen:

1. Melden Sie sich am OpenShift-Masterknoten an, und kopieren Sie die YAML-Datei [ocp-ds-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-ds-omsagent.yaml) und das Skript [ocp-secretgen.sh](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-secretgen.sh) zum Generieren von Geheimnissen aus GitHub.  Mit diesem Skript wird die YAML-Geheimnisdatei für die OMS-Arbeitsbereich-ID und den Primärschlüssel generiert, um Ihre Geheimnisinformationen zu schützen.  
2. Führen Sie die folgenden Befehle aus, um ein Projekt für OMS zu erstellen, und legen Sie das Benutzerkonto fest. Sie werden vom Skript zum Generieren von Geheimnissen zum Angeben Ihrer OMS-Arbeitsbereich-ID <WSID> und Ihres Primärschlüssels <KEY> aufgefordert. Wenn der Vorgang abgeschlossen ist, wird die Datei „ocp-secret.yaml“ erstellt.  
    
    ```
    oadm new-project omslogging --node-selector='zone=default'  
    oc project omslogging  
    oc create serviceaccount omsagent  
    oadm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

4. Stellen Sie die Geheimnisdatei bereit, indem Sie Folgendes ausführen:

    `oc create -f ocp-secret.yaml`

5. Überprüfen Sie die Bereitstellung, indem Sie Folgendes ausführen: 

    `oc describe secret omsagent-secret`  

    Die Ausgabe sollte in etwa wie folgt aussehen:  
    
    ```
    [ocpadmin@khocp-master-0 ~]$ oc describe ds oms  
    Name:           oms  
    Image(s):       microsoft/oms  
    Selector:       name=omsagent  
    Node-Selector:  zone=default  
    Labels:         agentVersion=1.4.0-12  
                    dockerProviderVersion=10.0.0-25  
                    name=omsagent  
    Desired Number of Nodes Scheduled: 3  
    Current Number of Nodes Scheduled: 3  
    Number of Nodes Misscheduled: 0  
    Pods Status:    3 Running / 0 Waiting / 0 Succeeded / 0 Failed  
    No events.  
    ```

6. Stellen Sie die YAML-Datei für das OMS-Agent-DaemonSet bereit, indem Sie Folgendes ausführen: 

    `oc create -f ocp-ds-omsagent.yaml`  
  
7. Überprüfen Sie die Bereitstellung, indem Sie Folgendes ausführen: 

    `oc describe ds oms`
  
    Die Ausgabe sollte in etwa wie folgt aussehen:

    ```
    [ocpadmin@khocp-master-0 ~]$ oc describe secret omsagent-secret  
    Name:           omsagent-secret  
    Namespace:      omslogging  
    Labels:         <none>  
    Annotations:    <none>  
    
    Type:   Opaque  
    
     Data  
     ====  
     KEY:    89 bytes  
     WSID:   37 bytes  
    ```

### <a name="secure-your-secret-information-for-docker-swarm-and-kubernetes"></a>Schützen Ihrer Geheimnisinformationen für Docker Swarm und Kubernetes 

Sie können Ihre geheime OMS-Arbeitsbereichs-ID und die Primärschlüssel für Docker Swarm- und Kubernetes-Containerdienste schützen.

#### <a name="secure-secrets-for-docker-swarm"></a>Sichern von Geheimnissen für Docker Swarm

Sobald das Geheimnis für Arbeitsbereichs-ID und Primärschlüssel erstellt wurde, können Sie für Docker Swarm den Docker-Dienst für OMSagent ausführen. Erstellen Sie die Geheimnisinformationen mithilfe der folgenden Informationen.

1. Führen Sie auf dem Masterknoten Folgendes aus.

    ```
    echo "WSID" | docker secret create WSID -
    echo "KEY" | docker secret create KEY -
    ```

2. Vergewissern Sie sich, dass die Geheimnisse ordnungsgemäß erstellt wurden.

    ```
    keiko@swarmm-master-13957614-0:/run# sudo docker secret ls
    ```

    ```
    ID                          NAME                CREATED             UPDATED
    j2fj153zxy91j8zbcitnjxjiv   WSID                43 minutes ago      43 minutes ago
    l9rh3n987g9c45zffuxdxetd9   KEY                 38 minutes ago      38 minutes ago
    ```

3. Führen Sie den folgenden Befehl aus, um die Geheimnisse im OMS-Agent im Container bereitzustellen.

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock --secret source=WSID,target=WSID --secret source=KEY,target=KEY  -p 25225:25225 -p 25224:25224/udp --restart-condition=on-failure microsoft/oms
    ```

#### <a name="secure-secrets-for-kubernetes-with-yaml-files"></a>Sichern von Geheimnissen für Kubernetes mit YAML-Dateien

Für Kubernetes verwenden Sie ein Skript, um die YAML-Datei mit Geheimnissen für Ihre Arbeitsbereichs-ID und den Primärschlüssel zu generieren. Auf der Seite [OMS Docker Kubernetes GitHub](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes) finden sich Dateien, die Sie mit Ihren oder ohne Ihre Geheimnisinformationen verwenden können.

- Das standardmäßige OMS-Agent-DaemonSet enthält keine Informationen zu Geheimnissen („omsagent.yaml“).
- Die OMS-Agent-DaemonSet-YAML-Datei verwendet Geheimnisinformationen („omsagent-ds-secrets.yaml“) mit Skripts zum Generieren von Geheimnissen, um die YAML-Geheimnisdatei („omsagentsecret.yaml“) zu generieren.

Sie können OMS-Agent-DaemonSets mit oder ohne Geheimnisse erstellen.

##### <a name="default-omsagent-daemonset-yaml-file-without-secrets"></a>Standardmäßige OMSagent-DaemonSet-YAML-Datei ohne Geheimnisse

- Ersetzen Sie für die standardmäßige OMS-Agent-DaemonSet-YAML-Datei `<WSID>` und `<KEY>` durch Ihre Angaben für WSID und KEY. Kopieren Sie die Datei in Ihren Masterknoten, und führen Sie Folgendes aus:

    ```
    sudo kubectl create -f omsagent.yaml
    ```

##### <a name="default-omsagent-daemonset-yaml-file-with-secrets"></a>Standardmäßige OMSagent-DaemonSet-YAML-Datei mit Geheimnissen

1. Um das OMS-Agent-DaemonSet mit geheimen Schlüsselinformationen zu verwenden, erstellen Sie zuerst die Geheimnisse.
    1. Kopieren Sie das Skript und die Vorlagendatei für Geheimnisse, und vergewissern Sie sich, dass sie sich im selben Verzeichnis befinden.
        - Skript zum Generieren von Geheimnissen – „secret-gen.sh“
        - Vorlage für Geheimnisse - „secret-template.yaml“
    2. Führen Sie das Skript wie im folgenden Beispiel aus. Das Skript fordert Sie zur Eingabe der OMS-Arbeitsbereichs-ID und des Primärschlüssels auf, und nach deren Eingabe erstellt das Skript eine YAML-Geheimnisdatei, die Sie dann ausführen können.   

        ```
        #> sudo bash ./secret-gen.sh
        ```

    3. Erstellen Sie den Pod für Geheimnisse, indem Sie Folgendes ausführen:
        ```
        sudo kubectl create -f omsagentsecret.yaml
        ```

    4. Führen Sie zur Überprüfung Folgendes aus:

        ```
        keiko@ubuntu16-13db:~# sudo kubectl get secrets
        ```

        Die Ausgabe sollte ähnlich wie die Folgende aussehen:

        ```
        NAME                  TYPE                                  DATA      AGE
        default-token-gvl91   kubernetes.io/service-account-token   3         50d
        omsagent-secret       Opaque                                2         1d
        ```

        ```
        keiko@ubuntu16-13db:~# sudo kubectl describe secrets omsagent-secret
        ```

        Die Ausgabe sollte ähnlich wie die Folgende aussehen:

        ```
        Name:           omsagent-secret
        Namespace:      default
        Labels:         <none>
        Annotations:    <none>

        Type:   Opaque

        Data
        ====
        WSID:   36 bytes
        KEY:    88 bytes
        ```

    5. Erstellen Ihres OMS-Agent-DaemonSet durch Ausführen von ``` sudo kubectl create -f omsagent-ds-secrets.yaml ```

2. Vergewissern Sie sich, dass das OMS-Agent-DaemonSet ausgeführt wird, ähnlich wie das Folgende:

    ```
    keiko@ubuntu16-13db:~# sudo kubectl get ds omsagent
    ```

    ```
    NAME       DESIRED   CURRENT   NODE-SELECTOR   AGE
    omsagent   3         3         <none>          1h
    ```


Verwenden Sie für Kubernetes ein Skript, um die YAML-Datei mit Geheimnissen für die Arbeitsbereichs-ID und den Primärschlüssel zu erstellen. Verwenden Sie die folgenden Beispielinformationen mit der [OMSagent-YAML-Datei](https://github.com/Microsoft/OMS-docker/blob/master/Kubernetes/omsagent.yaml), um Ihre Geheimnisinformationen zu sichern.

```
keiko@ubuntu16-13db:~# sudo kubectl describe secrets omsagent-secret
Name:           omsagent-secret
Namespace:      default
Labels:         <none>
Annotations:    <none>

Type:   Opaque

Data
====
WSID:   36 bytes
KEY:    88 bytes
```

## <a name="windows-container-hosts"></a>Windows-Containerhosts

### <a name="preparation-before-installing-windows-agents"></a>Vorbereitung vor der Installation von Windows-Agents

Vor der Installation von Agents auf Windows-Computern müssen Sie den Docker-Dienst konfigurieren. Die Konfiguration ermöglicht dem Windows-Agent oder der Log Analytics-Erweiterung für virtuelle Computer, den Docker-TCP-Socket zu verwenden, sodass die Agents remote auf den Docker-Daemon zugreifen und Daten für die Überwachung erfassen können.

#### <a name="to-start-docker-and-verify-its-configuration"></a>So starten Sie Docker und prüfen seine Konfiguration

Zum Einrichten einer TCP-Named Pipe für Windows Server müssen mehrere Schritte durchgeführt werden:

1. Aktivieren Sie in Windows PowerShell TCP Pipe und Named Pipe.

    ```
    Stop-Service docker
    dockerd --unregister-service
    dockerd --register-service -H npipe:// -H 0.0.0.0:2375  
    Start-Service docker
    ```

2. Konfigurieren Sie Docker mit der Konfigurationsdatei für die TCP Pipe und Named Pipe. Die Konfigurationsdatei befindet sich unter C:\ProgramData\docker\config\daemon.json.

    In der Datei „daemon.json“ benötigen Sie Folgendes:

    ```
    {
    "hosts": ["tcp://0.0.0.0:2375", "npipe://"]
    }
    ```

Weitere Informationen zur Docker-Daemon-Konfiguration mit Windows-Containern finden Sie unter [Docker-Modul unter Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon).


### <a name="install-windows-agents"></a>Installieren des Windows-Agents

Um die Überwachung von Windows- und Hyper-V-Containern zu aktivieren, installieren Sie den Microsoft Monitoring Agent (MMA) auf Windows-Computern, bei denen es sich um Containerhosts handelt. Informationen zu Windows-Computern in Ihrer lokalen Umgebung finden Sie unter [Verbinden von Windows-Computern mit Log Analytics](log-analytics-windows-agents.md). Verbinden Sie virtuelle Computer in Azure unter Verwendung der [VM-Erweiterung](log-analytics-azure-vm-extension.md) mit Log Analytics.

Sie können Windows-Container überwachen, die unter Service Fabric ausgeführt werden. Allerdings werden derzeit nur [unter Azure ausgeführte virtuelle Computer](log-analytics-azure-vm-extension.md) und [Windows-Computer, die in Ihrer lokalen Umgebung ausgeführt werden](log-analytics-windows-agents.md), für Service Fabric unterstützt.

So überprüfen Sie, ob die Container-Lösung ordnungsgemäß festgelegt ist:

- Überprüfen Sie, ob das Management Pack ordnungsgemäß heruntergeladen wurde, suchen Sie nach *ContainerManagement.xxx*.
    - Die Dateien sollten sich im Verzeichnis „C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs“ befinden.
- Überprüfen Sie in **Systemsteuerung** > **System und Sicherheit**, ob die OMS-Arbeitsbereichs-ID richtig ist.
    - Öffnen Sie **Microsoft Monitoring Agent**, und stellen Sie sicher, dass die Arbeitsbereichsinformationen richtig sind.


## <a name="containers-data-collection-details"></a>Details zur Datensammlung in Containern
Die Containerlösung sammelt verschiedene Leistungsmetriken und Protokolldaten von Containerhosts und Containern mit Agents, die Sie aktivieren.

Die folgende Tabelle enthält die Datensammlungsmethoden und andere Details dazu, wie Daten für Container erfasst werden.

| Plattform | [OMS Agent für Linux](log-analytics-linux-agents.md) | SCOM-Agent | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- |
| Linux |![Ja](./media/log-analytics-containers/oms-bullet-green.png) |![Nein](./media/log-analytics-containers/oms-bullet-red.png) |![Nein](./media/log-analytics-containers/oms-bullet-red.png) |![Nein](./media/log-analytics-containers/oms-bullet-red.png) |![Nein](./media/log-analytics-containers/oms-bullet-red.png) |Alle 3 Minuten |

| Plattform | [Windows-Agent](log-analytics-windows-agents.md) | SCOM-Agent | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |![Ja](./media/log-analytics-containers/oms-bullet-green.png) |![Nein](./media/log-analytics-containers/oms-bullet-red.png) |![Nein](./media/log-analytics-containers/oms-bullet-red.png) |![Nein](./media/log-analytics-containers/oms-bullet-red.png) |![Nein](./media/log-analytics-containers/oms-bullet-red.png) |Alle 3 Minuten |

| Plattform | [Log Analytics-VM-Erweiterung](log-analytics-azure-vm-extension.md) | SCOM-Agent | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- |
| Azure |![Ja](./media/log-analytics-containers/oms-bullet-green.png) |![Nein](./media/log-analytics-containers/oms-bullet-red.png) |![Nein](./media/log-analytics-containers/oms-bullet-red.png) |![Nein](./media/log-analytics-containers/oms-bullet-red.png) |![Nein](./media/log-analytics-containers/oms-bullet-red.png) |Alle 3 Minuten |

Die folgende Tabelle enthält Beispiele für von der Containerlösung gesammelte Datentypen, für in Protokollsuchen verwendete Datentypen sowie für Ergebnisse.

| Datentyp | Datentyp in der Protokollsuche | Felder |
| --- | --- | --- |
| Leistung von Hosts und Containern | `Type=Perf` | Computer, ObjectName, CounterName &#40;%Processor Time, Disk Reads MB, Disk Writes MB, Memory Usage MB, Network Receive Bytes, Network Send Bytes, Processor Usage sec, Network&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem |
| Containerinhalt | `Type=ContainerInventory` | TimeGenerated, Computer, Containername, ContainerHostname, Image, ImageTag, ContinerState, ExitCode, EnvironmentVar, Command, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID |
| Containerimageinhalt | `Type=ContainerImageInventory` | TimeGenerated, Computer, Image, ImageTag, ImageSize, VirtualSize, Running, Paused, Stopped, Failed, SourceSystem, ImageID, TotalContainer |
| Containerprotokoll | `Type=ContainerLog` | TimeGenerated, Computer, Image-ID, Containername, LogEntrySource, LogEntry, SourceSystem, ContainerID |
| Containerdienstprotokoll | `Type=ContainerServiceLog`  | TimeGenerated, Computer, TimeOfCommand, Image, Command, SourceSystem, ContainerID |

## <a name="monitor-containers"></a>Überwachen von Containern
Nachdem Sie die Lösung im OMS-Portal aktiviert haben, werden auf der Kachel **Container** zusammenfassende Informationen zu den Containerhosts und den auf den Hosts ausgeführten Containern angezeigt.

![Kachel „Container“](./media/log-analytics-containers/containers-title.png)

Die Kachel zeigt eine Übersicht über die Anzahl der Container in der Umgebung an. Außerdem erfahren Sie, ob Container fehlerhaft sind, ausgeführt werden oder beendet wurden.

### <a name="using-the-containers-dashboard"></a>Verwenden des Containerdashboards
Klicken Sie auf die Kachel **Container**. Dort sind die Ansichten wie folgt angeordnet:

* Containerereignisse
* Fehler
* Containerstatus
* Containerimageinhalt
* Leistung von CPU und Arbeitsspeicher

Jeder Bereich im Dashboard ist eine visuelle Darstellung einer Suche, die über die gesammelten Daten ausgeführt wird.

![Containerdashboard](./media/log-analytics-containers/containers-dash01.png)

![Containerdashboard](./media/log-analytics-containers/containers-dash02.png)

Klicken Sie auf dem Blatt **Container Status** (Containerstatus) auf den oberen Bereich, wie unten dargestellt.

![Containerstatus](./media/log-analytics-containers/containers-status.png)

Die Protokollsuche wird geöffnet. Darin werden Informationen zu den Hosts und den Containern, die darauf ausgeführt werden, angezeigt.

![Protokollsuche für Container](./media/log-analytics-containers/containers-log-search.png)

Hier können Sie die Suchabfrage bearbeiten, um die für Sie interessanten spezifischen Informationen zu suchen. Weitere Informationen zur Verwendung der Protokollsuche finden Sie unter [Protokollsuchvorgänge in Log Analytics](log-analytics-log-searches.md).

Sie können z.B. die Suchabfrage so ändern, dass alle beendeten Container anstelle der ausgeführten Container angezeigt werden. Dazu ändern Sie **Running** in der Abfrage in **Stopped**.

## <a name="troubleshoot-by-finding-a-failed-container"></a>Beheben von Problemen durch das Suchen fehlerhafter Container
OMS kennzeichnet einen Container als **Failed**, wenn dieser mit einem anderen Exitcode als null beendet wurde. Auf dem Blatt **Container mit Fehlern** wird eine Übersicht über die Fehler und Ausfälle in der Umgebung angezeigt.

### <a name="to-find-failed-containers"></a>So suchen Sie nach Containern mit Fehlern
1. Klicken Sie auf das Blatt **Container Events** (Containerereignisse).  
   ![Containerereignisse](./media/log-analytics-containers/containers-events.png)
2. Die Protokollsuche wird geöffnet. Darin wird der Status von Containern wie folgt angezeigt.  
   ![Containerzustand](./media/log-analytics-containers/containers-container-state.png)
3. Klicken Sie anschließend auf den Wert für Container mit Fehlern, um zusätzliche Informationen wie Imagegröße und Anzahl der beendeten und fehlerhaften Images anzuzeigen. Erweitern Sie **Mehr anzeigen**, um die Image-ID anzuzeigen.  
   ![Container mit Fehlern](./media/log-analytics-containers/containers-state-failed.png)
4. Suchen Sie als Nächstes den Container, in dem dieses Image ausgeführt wird. Geben Sie Folgendes in die Suchabfrage ein.
   `Type=ContainerInventory <ImageID>` Dadurch werden die Protokolle angezeigt. Sie können scrollen, um fehlerhafte Container zu finden.  
   ![Container mit Fehlern](./media/log-analytics-containers/containers-failed04.png)

## <a name="search-logs-for-container-data"></a>Suchprotokolle für Containerdaten
Wenn Sie Probleme mit einen bestimmten Fehler beheben möchten, kann es hilfreich sein, herauszufinden, wo dieser in Ihrer Umgebung auftritt. Mit den folgenden Protokolltypen können Sie Abfragen erstellen, die die gewünschten Informationen zurückgeben.

* **ContainerInventory:** Verwenden Sie diesen Typ, wenn Sie Informationen zum Containerspeicherort und -namen sowie zu den darin ausgeführten Images benötigen.
* **ContainerImageInventory:** Verwenden Sie diesen Typ, wenn Sie die Informationen organisiert nach Image suchen und Imageinformationen wie Image-IDs oder -Größen anzeigen möchten.
* **ContainerLog:** Verwenden Sie diesen Typ, wenn Sie bestimmte Informationen und Einträge im Fehlerprotokoll suchen möchten.
* **ContainerServiceLog:** Verwenden Sie diesen Typ, wenn Sie Audit-Trail-Informationen zum Docker-Daemon suchen, z.B. zu den Befehlen „start“, „stop“, „delete“ oder „pull“.

### <a name="to-search-logs-for-container-data"></a>So durchsuchen Sie Protokolle nach Containerdaten
* Wählen Sie ein Image aus, bei dem kürzlich ein Fehler aufgetreten ist, und suchen Sie die zugehörigen Fehlerprotokolle. Suchen Sie zunächst mit **ContainerInventory** nach dem Namen eines Containers, in dem das Image ausgeführt wird. Suchen Sie beispielsweise nach `Type=ContainerInventory ubuntu Failed`.  
    ![Durchsuchen von Ubuntu-Containern](./media/log-analytics-containers/search-ubuntu.png)

  Notieren Sie sich den Namen des Containers neben **Namen**, und suchen Sie nach den zugehörigen Protokollen. In diesem Beispiel lautet er `Type=ContainerLog adoring_meitner`.

**Anzeigen von Leistungsinformationen**

Wenn Sie mit dem Erstellen von Abfragen beginnen, ist es möglicherweise hilfreich, zunächst zu erfahren, was alles möglich ist. Wenn Sie beispielsweise alle Leistungsdaten anzeigen möchten, können Sie die folgende allgemeine Abfrage eingeben.

```
Type=Perf
```

![Containerleistung](./media/log-analytics-containers/containers-perf01.png)

Sie können dies auch grafisch darstellen, indem Sie in den Ergebnissen auf das Wort **Metriken** klicken.

![Containerleistung](./media/log-analytics-containers/containers-perf02.png)

Sie können die angezeigten Leistungsdaten auf einen bestimmten Container einschränken, indem Sie den Namen rechts von Ihrer Abfrage eingeben.

```
Type=Perf <containerName>
```

Damit zeigen Sie die Liste der Leistungsmetriken an, die für einen einzelnen Container gesammelt wurden.

![Containerleistung](./media/log-analytics-containers/containers-perf03.png)

## <a name="example-log-search-queries"></a>Beispielabfragen für die Protokollsuche
Es ist oft hilfreich, die Erstellung von Abfragen ausgehend von einem oder zwei Beispielen zu beginnen und diese dann an die eigene Umgebung anzupassen. Als Ausgangspunkt können Sie auf dem Blatt **Relevante Abfragen** experimentieren, um komplexere Abfragen zu erstellen.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Containerabfragen](./media/log-analytics-containers/containers-queries.png)


## <a name="saving-log-search-queries"></a>Speichern von Abfragen für die Protokollsuche
Das Speichern von Abfragen ist ein Standardfeature in Log Analytics. Durch die Speicherung können Sie schnell auf die Abfragen zurückgreifen, die Sie als besonders hilfreich empfinden.

Nachdem Sie eine Abfrage erstellt haben, die für Sie nützlich ist, speichern Sie sie, indem Sie oben auf der Seite „Protokollsuche“ auf **Favoriten** klicken. Sie können später auf der Seite **Mein Dashboard** auf einfache Weise darauf zugreifen.

## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich über [Protokollsuchen](log-analytics-log-searches.md) für die Anzeige ausführlicher Containerdatensätze.

