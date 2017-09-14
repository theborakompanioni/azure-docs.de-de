---
title: "Containerüberwachungslösung in Azure Log Analytics | Microsoft-Dokumentation"
description: "Die Containerüberwachungslösung in Log Analytics unterstützt Sie beim Anzeigen und Verwalten Ihrer Docker- und Windows-Containerhosts an einem zentralen Ort."
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
ms.date: 08/30/2017
ms.author: magoedte;banders
ms.translationtype: HT
ms.sourcegitcommit: 3eb68cba15e89c455d7d33be1ec0bf596df5f3b7
ms.openlocfilehash: cd21a08de9dbf795b9a295de22e55a24fa9535ef
ms.contentlocale: de-de
ms.lasthandoff: 09/01/2017

---
# <a name="container-monitoring-solution-in-log-analytics"></a>Containerüberwachungslösung in Log Analytics

![Container-Symbol](./media/log-analytics-containers/containers-symbol.png)

In diesem Artikel wird das Einrichten und Verwenden der Containerüberwachungslösung in Log Analytics beschrieben, die Sie beim Anzeigen und Verwalten Ihrer Docker- und Windows-Containerhosts an einem zentralen Ort unterstützt. Docker ist ein Softwarevirtualisierungssystem zum Erstellen von Containern, durch die das Bereitstellen von Software in einer IT-Infrastruktur automatisiert werden kann.

Die Lösung zeigt, welche Container, welches Containerimage und wo Container ausgeführt werden. Sie können ausführliche Überwachungsinformationen anzeigen, die auch die mit Containern verwendeten Befehle enthalten. Außerdem können Sie die Probleme mit Containern beheben, indem Sie zentralisierte Protokolle anzeigen und durchsuchen, ohne eine Remoteanzeige der Docker- und Windows-Hosts zu benötigen. Sie können Container suchen, die Störungen verursachen und übermäßig viele Ressourcen auf einem Host verbrauchen. Darüber hinaus können Sie an einem zentralen Ort Informationen zur Leistung und Auslastung von CPU, Arbeitsspeicher, Speicher und Netzwerk zu Containern anzeigen. Auf Windows-Computern können Sie die Protokolle von Windows Server-, Hyper-V- und Docker-Containern zentralisieren und vergleichen. Die Lösung unterstützt die folgenden Containerorchestratoren:

- Docker Swarm
- DC/OS
- kubernetes
- Service Fabric
- Red Hat OpenShift


Das folgende Diagramm zeigt die Beziehungen zwischen verschiedenen Containerhosts und Agents mit OMS.

![Containerdiagramm](./media/log-analytics-containers/containers-diagram.png)

## <a name="system-requirements-and-supported-platforms"></a>Systemanforderungen und unterstützte Plattformen

Prüfen Sie zunächst anhand der folgenden Informationen, ob die Voraussetzungen erfüllt sind.

### <a name="container-monitoring-solution-support-for-docker-orchestrator-and-os-platform"></a>Unterstützung der Containerüberwachungslösung für Docker Orchestrator und das Betriebssystem
In der folgenden Tabelle werden die Docker-Orchestrierung und die Unterstützung der Betriebssystemüberwachung von Containerbestand, Leistung und Protokollen mit Log Analytics aufgeführt.   

| | ACS | Linux | Windows | Container<br>Inventory | Image<br>Inventory | Knoten<br>Inventory | Container<br>Leistung | Container<br>Ereignis | Ereignis<br>Protokoll | Container<br>Protokoll |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| kubernetes | &#8226; | &#8226; | | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; |
| Mesosphere<br>DC/OS | &#8226; | &#8226; | | &#8226; | &#8226; | &#8226; | &#8226;| &#8226; | &#8226; | &#8226; |
| Docker<br>Swarm | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | | &#8226; |
| Dienst<br>Fabric | | | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; |
| Red Hat Open<br>Shift | | &#8226; | | &#8226; | &#8226;| &#8226; | &#8226; | &#8226; | | &#8226; |
| Windows Server<br>(eigenständig) | | | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | | &#8226; |
| Linux-Server<br>(eigenständig) | | &#8226; | | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | | &#8226; |


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
    - Kubernetes-Ereignisse, Kubernetes-Inventar und Containerprozesse werden nur mit Version 1.4.1-45 und höher vom OMS-Agent für Linux unterstützt
- ACS Docker Swarm

### <a name="supported-windows-operating-system"></a>Unterstützte Windows-Betriebssysteme

- Windows Server 2016
- Windows 10 Anniversary Edition (Professional oder Enterprise)

### <a name="docker-versions-supported-on-windows"></a>Unter Windows unterstützte Docker-Versionen

- Docker 1.12 und 1.13
- Docker 17.03.0 und höher

## <a name="installing-and-configuring-the-solution"></a>Installieren und Konfigurieren der Lösung
Verwenden Sie die folgenden Informationen zum Installieren und Konfigurieren der Lösung.

1. Fügen Sie mithilfe des [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ContainersOMS?tab=Overview) oder des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md) beschriebenen Prozesses Ihrem OMS-Arbeitsbereich die Containerüberwachungslösung hinzu.

2. Installieren und verwenden Sie Docker mit einem OMS-Agent. Basierend auf Ihrem Betriebssystem und Docker-Orchestrator können Sie die folgenden Methoden verwenden, um den Agent zu konfigurieren.
  - Für eigenständige Hosts:
    - Auf unterstützten Linux-Betriebssystemen installieren Sie zunächst Docker, und führen Sie es aus. Anschließend installieren und konfigurieren Sie den [OMS-Agent für Linux](log-analytics-agent-linux.md).  
    - Unter CoreOS kann der OMS-Agent für Linux nicht ausgeführt werden. Stattdessen führen Sie eine Containerversion des OMS-Agents für Linux aus. Lesen Sie [Linux-Containerhosts mit CoreOS](#for-all-linux-container-hosts-including-coreos) oder [Azure Government-Linux-Containerhosts mit CoreOS](#for-all-azure-government-linux-container-hosts-including-coreos), wenn Sie in Azure Government Cloud mit Containern arbeiten.
    - Installieren Sie unter Windows Server 2016 und Windows 10 das Docker-Modul und den Client, und stellen Sie dann eine Verbindung mit einem Agent her, um Informationen zu sammeln und sie an Log Analytics zu senden. Wenn Sie über eine Windows-Umgebung verfügen, lesen Sie [Installieren und Konfigurieren von Windows-Containerhosts](#install-and-configure-windows-container-hosts).
  - Für Docker-Orchestrierung mit mehreren Hosts:
    - Falls Sie über eine Red Hat OpenShift-Umgebung verfügen, helfen Ihnen die Informationen unter [Konfigurieren eines OMS-Agents für Red Hat OpenShift](#configure-an-oms-agent-for-red-hat-openshift) weiter.
    - Wenn Sie über einen Kubernetes-Cluster verfügen, der den Azure Container Service verwendet, lesen Sie [Konfigurieren eines OMS-Agents für Kubernetes](#configure-an-oms-agent-for-kubernetes).
    - Wenn Sie über einen DC/OS-Cluster in Azure Container Service verfügen, finden Sie unter [Überwachen eines Azure Container Service-DC/OS-Clusters mit Operations Management Suite](../container-service/dcos-swarm/container-service-monitoring-oms.md) weitere Informationen.
    - Wenn Sie über eine Docker Swarm-Modus-Umgebung verfügen, erhalten Sie weitere Informationen unter [Konfigurieren eines OMS-Agent für Docker Swarm](#configure-an-oms-agent-for-docker-swarm).
    - Wenn Sie Container mit Service Fabric verwenden, helfen Ihnen die Informationen unter [Übersicht über Azure Service Fabric ](../service-fabric/service-fabric-overview.md) weiter.

Weitere Informationen zum Installieren und Konfigurieren von Docker-Modulen auf Windows-Computern finden Sie im Artikel [Docker-Modul unter Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon).

> [!IMPORTANT]
> Docker muss ausgeführt werden, **bevor** Sie den [OMS-Agent für Linux](log-analytics-agent-linux.md) auf Ihren Containerhosts installieren. Wenn Sie den Agent bereits vor der Installation von Docker installiert haben, müssen Sie den OMS-Agent für Linux erneut installieren. Weitere Informationen zu Docker finden Sie auf der [Docker-Website](https://www.docker.com).


### <a name="install-and-configure-linux-container-hosts"></a>Installieren und Konfigurieren von Linux-Containerhosts

Verwenden Sie nach dem Installieren von Docker die folgenden Einstellungen für den Containerhost, um den Agent für die Verwendung mit Docker zu konfigurieren. Zuerst benötigen Sie Ihre ID und den Schlüssel für den OMS-Arbeitsbereich. Diese Daten finden Sie im Azure-Portal. Klicken Sie in Ihrem Arbeitsbereich auf **Schnellstart** > **Computer**, um Ihre **Arbeitsbereichs-ID** und Ihren **Primärschlüssel** anzuzeigen.  Kopieren Sie beide Angaben, und fügen Sie sie in den von Ihnen bevorzugten Editor ein.

**Für alle Linux-Containerhosts mit Ausnahme von CoreOS:**

- Weitere Informationen und Anleitungen zum Installieren des OMS-Agents für Linux finden Sie unter [Verbinden Ihrer Linux-Computer mit Operations Management Suite (OMS)](log-analytics-agent-linux.md).

**Für alle Linux-Containerhosts mit CoreOS:**

Starten Sie den OMS-Container, den Sie überwachen möchten. Verwenden Sie das folgende Beispiel, und passen Sie es an:

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -e WSID="your workspace id" -e KEY="your key" -h=`hostname` -p 127.0.0.1:25225:25225 --name="omsagent" --restart=always microsoft/oms
```

**Für alle Azure Government-Linux-Containerhosts mit CoreOS:**

Starten Sie den OMS-Container, den Sie überwachen möchten. Verwenden Sie das folgende Beispiel, und passen Sie es an:

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -v /var/log:/var/log -e WSID="your workspace id" -e KEY="your key" -e DOMAIN="opinsights.azure.us" -p 127.0.0.1:25225:25225 -p 127.0.0.1:25224:25224/udp --name="omsagent" -h=`hostname` --restart=always microsoft/oms
```

**Wechseln von der Verwendung eines installierten Linux-Agents zu einem in einem Container enthaltenen**

Wenn Sie zuvor den direkt installierten Agent verwendet haben und stattdessen einen Agent verwenden möchten, der in einem Container ausgeführt wird, müssen Sie zunächst den OMS-Agent für Linux entfernen. Informationen zum erfolgreichen Deinstallieren des Agents finden Sie unter [Deinstallieren des OMS-Agents für Linux](log-analytics-agent-linux.md#uninstalling-the-oms-agent-for-linux).  

#### <a name="configure-an-oms-agent-for-docker-swarm"></a>Konfigurieren eines OMS-Agent für Docker Swarm

Sie können den OMS-Agent als globalen Dienst in Docker Swarm ausführen. Erstellen Sie mithilfe der folgenden Informationen einen OMS-Agent-Dienst. Sie müssen Ihre OMS-Arbeitsbereichs-ID und den Primärschlüssel einfügen.

- Führen Sie auf dem Masterknoten Folgendes aus.

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock  -e WSID="<WORKSPACE ID>" -e KEY="<PRIMARY KEY>" -p 25225:25225 -p 25224:25224/udp  --restart-condition=on-failure microsoft/oms
    ```

##### <a name="secure-secrets-for-docker-swarm"></a>Sichern von Geheimnissen für Docker Swarm

Verwenden Sie für Docker Swarm die folgenden Informationen, um die Geheimnisinformationen zu erstellen, sobald das Geheimnis für die Arbeitsbereichs-ID und den Primärschlüssel erstellt ist.

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

#### <a name="configure-an-oms-agent-for-red-hat-openshift"></a>Konfigurieren eines OMS-Agents für Red Hat OpenShift
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

#### <a name="configure-an-oms-agent-for-kubernetes"></a>Konfigurieren eines OMS-Agents für Kubernetes

Für Kubernetes verwenden Sie ein Skript, um die YAML-Datei mit Geheimnissen für Ihre Arbeitsbereichs-ID und den Primärschlüssel zu generieren. Auf der Seite [OMS Docker Kubernetes GitHub](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes) finden sich Dateien, die Sie mit Ihren oder ohne Ihre Geheimnisinformationen verwenden können.

- Das standardmäßige OMS-Agent-DaemonSet enthält keine Informationen zu Geheimnissen („omsagent.yaml“).
- Die OMS-Agent-DaemonSet-YAML-Datei verwendet Geheimnisinformationen („omsagent-ds-secrets.yaml“) mit Skripts zum Generieren von Geheimnissen, um die YAML-Geheimnisdatei („omsagentsecret.yaml“) zu generieren.

Sie können OMS-Agent-DaemonSets mit oder ohne Geheimnisse erstellen.

**Standardmäßige OMSagent-DaemonSet-YAML-Datei ohne Geheimnisse**

- Ersetzen Sie für die standardmäßige OMS-Agent-DaemonSet-YAML-Datei `<WSID>` und `<KEY>` durch Ihre Angaben für WSID und KEY. Kopieren Sie die Datei in Ihren Masterknoten, und führen Sie Folgendes aus:

    ```
    sudo kubectl create -f omsagent.yaml
    ```

**Standardmäßige OMSagent-DaemonSet-YAML-Datei mit Geheimnissen**

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

### <a name="install-and-configure-windows-container-hosts"></a>Installieren und Konfigurieren von Windows-Containerhosts

Verwenden Sie die Informationen in diesem Abschnitt zum Installieren und Konfigurieren von Windows-Containerhosts.

#### <a name="preparation-before-installing-windows-agents"></a>Vorbereitung vor der Installation von Windows-Agents

Vor der Installation von Agents auf Windows-Computern müssen Sie den Docker-Dienst konfigurieren. Die Konfiguration ermöglicht dem Windows-Agent oder der Log Analytics-Erweiterung für virtuelle Computer, den Docker-TCP-Socket zu verwenden, sodass die Agents remote auf den Docker-Daemon zugreifen und Daten für die Überwachung erfassen können.

##### <a name="to-start-docker-and-verify-its-configuration"></a>So starten Sie Docker und prüfen seine Konfiguration

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


#### <a name="install-windows-agents"></a>Installieren des Windows-Agents

Um die Überwachung von Windows- und Hyper-V-Containern zu aktivieren, installieren Sie den Microsoft Monitoring Agent (MMA) auf Windows-Computern, bei denen es sich um Containerhosts handelt. Informationen zu Windows-Computern in Ihrer lokalen Umgebung finden Sie unter [Verbinden von Windows-Computern mit Log Analytics](log-analytics-windows-agents.md). Verbinden Sie virtuelle Computer in Azure unter Verwendung der [VM-Erweiterung](log-analytics-azure-vm-extension.md) mit Log Analytics.

Sie können Windows-Container überwachen, die unter Service Fabric ausgeführt werden. Allerdings werden derzeit nur [unter Azure ausgeführte virtuelle Computer](log-analytics-azure-vm-extension.md) und [Windows-Computer, die in Ihrer lokalen Umgebung ausgeführt werden](log-analytics-windows-agents.md), für Service Fabric unterstützt.

Sie können überprüfen, ob die Containerüberwachungslösung für Windows korrekt festgelegt ist. Um zu überprüfen, ob das Management Pack ordnungsgemäß heruntergeladen wurde, suchen Sie nach *ContainerManagement.xxx*. Die Dateien sollten sich im Verzeichnis „C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs“ befinden.


## <a name="solution-components"></a>Lösungskomponenten

Wenn Sie Windows-Agents verwenden, wird auf jedem Computer mit einem Agent beim Hinzufügen dieser Lösung das folgende Management Pack installiert. Für das Management Pack ist keine Konfiguration oder Wartung erforderlich.

- *ContainerManagement.xxx* wird unter „C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs“ installiert.

## <a name="container-data-collection-details"></a>Details zur Datensammlung in Containern
Die Containerüberwachungslösung sammelt verschiedene Leistungsmetriken und Protokolldaten von Containerhosts und Containern mit Agents, die Sie aktivieren.

Alle drei Minuten werden von den folgenden Agent-Typen Daten gesammelt.

- [OMS Agent für Linux](log-analytics-linux-agents.md)
- [Windows-Agent](log-analytics-windows-agents.md)
- [Log Analytics-VM-Erweiterung](log-analytics-azure-vm-extension.md)


### <a name="container-records"></a>Containerdatensätze

Die folgende Tabelle enthält Beispiele für von der Containerüberwachungslösung gesammelte Datentypen, die in Protokollsuchergebnissen angezeigt werden.

| Datentyp | Datentyp in der Protokollsuche | Felder |
| --- | --- | --- |
| Leistung von Hosts und Containern | `Type=Perf` | Computer, ObjectName, CounterName &#40;%Processor Time, Disk Reads MB, Disk Writes MB, Memory Usage MB, Network Receive Bytes, Network Send Bytes, Processor Usage sec, Network&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem |
| Containerinhalt | `Type=ContainerInventory` | TimeGenerated, Computer, Containername, ContainerHostname, Image, ImageTag, ContainerState, ExitCode, EnvironmentVar, Command, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID |
| Containerimageinhalt | `Type=ContainerImageInventory` | TimeGenerated, Computer, Image, ImageTag, ImageSize, VirtualSize, Running, Paused, Stopped, Failed, SourceSystem, ImageID, TotalContainer |
| Containerprotokoll | `Type=ContainerLog` | TimeGenerated, Computer, Image-ID, Containername, LogEntrySource, LogEntry, SourceSystem, ContainerID |
| Containerdienstprotokoll | `Type=ContainerServiceLog`  | TimeGenerated, Computer, TimeOfCommand, Image, Command, SourceSystem, ContainerID |
| Containerknotenbestand | `Type=ContainerNodeInventory_CL`| TimeGenerated, Computer, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem|
| Kubernetes-Bestand | `Type=KubePodInventory_CL` | TimeGenerated, Computer, PodLabel_deployment_s, PodLabel_deploymentconfig_s, PodLabel_docker_registry_s, Name_s, Namespace_s, PodStatus_s, PodIp_s, PodUid_g, PodCreationTimeStamp_t, SourceSystem |
| Containerprozess | `Type=ContainerProcess_CL` | TimeGenerated, Computer, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, SourceSystem |
| Kubernetes-Ereignisse | `Type=KubeEvents_CL` | TimeGenerated, Computer, Name_s, ObjectKind_s, Namespace_s, Reason_s, Type_s, SourceComponent_s, SourceSystem, Message |

Bezeichnungen, die an *PodLabel*-Datentypen angefügt werden, sind Ihre eigenen benutzerdefinierten Bezeichnungen. Die angefügten PodLabel-Bezeichnungen in der Tabelle stellen Beispiele dar. Deshalb unterscheiden sich `PodLabel_deployment_s`, `PodLabel_deploymentconfig_s`, `PodLabel_docker_registry_s` im Dataset Ihrer Umgebung und ähneln allgemein `PodLabel_yourlabel_s`.


## <a name="monitor-containers"></a>Überwachen von Containern
Nachdem Sie die Lösung im OMS-Portal aktiviert haben, werden auf der Kachel **Container** zusammenfassende Informationen zu den Containerhosts und den auf den Hosts ausgeführten Containern angezeigt.

![Kachel „Container“](./media/log-analytics-containers/containers-title.png)

Die Kachel zeigt eine Übersicht über die Anzahl der Container in der Umgebung an. Außerdem erfahren Sie, ob Container fehlerhaft sind, ausgeführt werden oder beendet wurden.

### <a name="using-the-containers-dashboard"></a>Verwenden des Containerdashboards
Klicken Sie auf die Kachel **Container**. Dort sind die Ansichten wie folgt angeordnet:

- **Containerereignisse** – Zeigt den Containerstatus und die Computer mit fehlerhaften Containern an.
- **Containerprotokolle** – Zeigt ein Diagramm von Containerprotokolldateien, die mit der Zeit generiert wurden, und eine Liste von Computern mit der höchsten Anzahl von Protokolldateien an.
- **Kubernetes-Ereignisse** – Zeigt ein Diagramm von Kubernetes-Ereignissen, die mit der Zeit generiert wurden, und eine Liste der Gründe an, warum die Ereignisse von Pods generiert wurden. *Dieses Dataset wird nur in Linux-Umgebungen verwendet.*
- **Kubernetes-Namespacebestand** – Zeigt die Anzahl der Namespaces und Pods und ihre Hierarchie an. *Dieses Dataset wird nur in Linux-Umgebungen verwendet.*
- **Containerknotenbestand** – Zeigt die Anzahl der Orchestrierungstypen an, die auf Containerknoten/-hosts verwendet werden. Die Computerknoten/-hosts werden auch in der Anzahl von Containern aufgeführt. *Dieses Dataset wird nur in Linux-Umgebungen verwendet.*
- **Containerimagebestand** – Zeigt die Gesamtanzahl der verwendeten Containerimages und die Anzahl der Imagetypen an. Die Anzahl der Images wird auch im Imagetag aufgeführt.
- **Containerstatus** – Zeigt die Gesamtanzahl der Containerknoten/Hostcomputer an, auf denen Container ausgeführt werden. Computer werden auch in der Anzahl der ausgeführten Hosts aufgelistet.
- **Containerprozess** – Zeigt ein Liniendiagramm von Containerprozessen an, die über einen bestimmten Zeitraum ausgeführt werden. Container werden auch durch Ausführen des Befehls bzw. Prozesses in Containern aufgeführt. *Dieses Dataset wird nur in Linux-Umgebungen verwendet.*
- **Container-CPU-Leistung** – Zeigt ein Liniendiagramm der durchschnittlichen CPU-Auslastung im Zeitverlauf für Computerknoten/-hosts an. Zudem werden die Computerknoten/-hosts basierend auf der durchschnittlichen CPU-Auslastung aufgeführt.
- **Containerspeicherleistung** – Zeigt ein Liniendiagramm der Speicherauslastung im Zeitverlauf an. Zudem wird die Computerspeicherauslastung basierend auf dem Instanznamen aufgeführt.
- **Computerleistung** – Zeigt Liniendiagramme zum Prozentsatz der CPU-Leistung im Zeitverlauf, den Prozentsatz der Speicherauslastung im Zeitverlauf und die Menge des freien Speicherplatzes auf dem Datenträger in MB im Zeitverlauf an. Sie können den Mauszeiger über eine beliebige Zeile in einem Diagramm bewegen, um weitere Details anzuzeigen.


Jeder Bereich des Dashboards ist eine visuelle Darstellung einer Suche, die über die gesammelten Daten ausgeführt wird.

![Containerdashboard](./media/log-analytics-containers/containers-dash01.png)

![Containerdashboard](./media/log-analytics-containers/containers-dash02.png)

Klicken Sie im Bereich **Containerstatus** auf den oberen Bereich, wie unten dargestellt.

![Containerstatus](./media/log-analytics-containers/containers-status.png)

Die Protokollsuche wird geöffnet. Darin werden Informationen zum Status Ihrer Container angezeigt.

![Protokollsuche für Container](./media/log-analytics-containers/containers-log-search.png)

Hier können Sie die Suchabfrage bearbeiten, um die für Sie interessanten spezifischen Informationen zu suchen. Weitere Informationen zur Verwendung der Protokollsuche finden Sie unter [Protokollsuchvorgänge in Log Analytics](log-analytics-log-searches.md).

## <a name="troubleshoot-by-finding-a-failed-container"></a>Beheben von Problemen durch das Suchen fehlerhafter Container

Log Analytics kennzeichnet einen Container als **Fehlerhaft**, wenn dieser mit einem anderen Exitcode als null beendet wurde. Im Bereich **Container mit Fehlern** wird eine Übersicht über die Fehler und Ausfälle in der Umgebung angezeigt.

### <a name="to-find-failed-containers"></a>So suchen Sie nach Containern mit Fehlern
1. Klicken Sie auf den Bereich **Containerstatus**.  
   ![Containerstatus](./media/log-analytics-containers/containers-status.png)
2. Die Protokollsuche wird geöffnet. Darin wird der Status Ihrer Container wie folgt angezeigt.  
   ![Containerzustand](./media/log-analytics-containers/containers-log-search.png)
3. Klicken Sie dann auf den aggregierten Wert der fehlerhaften Container, um zusätzliche Informationen anzuzeigen. Erweitern Sie **Mehr anzeigen**, um die Image-ID anzuzeigen.  
   ![Container mit Fehlern](./media/log-analytics-containers/containers-state-failed.png)  
4. Geben Sie als Nächstes Folgendes in die Suchabfrage ein. `Type=ContainerInventory <ImageID>`, um Details zum Image wie Imagegröße sowie Anzahl der beendeten und fehlerhaften Images anzuzeigen.  
   ![Container mit Fehlern](./media/log-analytics-containers/containers-failed04.png)

## <a name="search-logs-for-container-data"></a>Suchprotokolle für Containerdaten
Wenn Sie Probleme mit einen bestimmten Fehler beheben möchten, kann es hilfreich sein, herauszufinden, wo dieser in Ihrer Umgebung auftritt. Mit den folgenden Protokolltypen können Sie Abfragen erstellen, die die gewünschten Informationen zurückgeben.


- **ContainerImageInventory:** Verwenden Sie diesen Typ, wenn Sie die Informationen organisiert nach Image suchen und Imageinformationen wie Image-IDs oder -Größen anzeigen möchten.
- **ContainerInventory:** Verwenden Sie diesen Typ, wenn Sie Informationen zum Containerspeicherort und -namen sowie zu den darin ausgeführten Images benötigen.
- **ContainerLog:** Verwenden Sie diesen Typ, wenn Sie bestimmte Informationen und Einträge im Fehlerprotokoll suchen möchten.
- **ContainerNodeInventory_CL** Verwenden Sie diesen Typ, wenn Sie Informationen zum Host bzw. Knoten, auf dem sich Container befinden, benötigen. Dieser gibt Informationen zu Docker-Version, Orchestrierungstyp, Speicher und Netzwerk an.
- **ContainerProcess_CL** Verwenden Sie diesen Typ, um schnell den Prozess, der im Container ausgeführt wird, anzuzeigen.
- **ContainerServiceLog:** Verwenden Sie diesen Typ, wenn Sie Audit-Trail-Informationen zum Docker-Daemon suchen, z.B. zu den Befehlen „start“, „stop“, „delete“ oder „pull“.
- **KubeEvents_CL** Verwenden Sie diesen Typ, um die Kubernetes-Ereignisse anzuzeigen.
- **KubePodInventory_CL** Verwenden Sie diesen Typ, wenn Sie Informationen zur Clusterhierarchie einsehen möchten.


### <a name="to-search-logs-for-container-data"></a>So durchsuchen Sie Protokolle nach Containerdaten
* Wählen Sie ein Image aus, bei dem kürzlich ein Fehler aufgetreten ist, und suchen Sie die zugehörigen Fehlerprotokolle. Suchen Sie zunächst mit **ContainerInventory** nach dem Namen eines Containers, in dem das Image ausgeführt wird. Suchen Sie beispielsweise nach `Type=ContainerInventory ubuntu Failed`.  
    ![Durchsuchen von Ubuntu-Containern](./media/log-analytics-containers/search-ubuntu.png)

  Notieren Sie sich den Namen des Containers neben **Namen**, und suchen Sie nach den zugehörigen Protokollen. In diesem Beispiel lautet er `Type=ContainerLog cranky_stonebreaker`.

**Anzeigen von Leistungsinformationen**

Wenn Sie mit dem Erstellen von Abfragen beginnen, ist es möglicherweise hilfreich, zunächst zu erfahren, was alles möglich ist. Wenn Sie beispielsweise alle Leistungsdaten anzeigen möchten, können Sie die folgende allgemeine Abfrage eingeben.

```
Type=Perf
```

![Containerleistung](./media/log-analytics-containers/containers-perf01.png)

Sie können die angezeigten Leistungsdaten auf einen bestimmten Container einschränken, indem Sie den Namen rechts von Ihrer Abfrage eingeben.

```
Type=Perf <containerName>
```

Damit zeigen Sie die Liste der Leistungsmetriken an, die für einen einzelnen Container gesammelt wurden.

![Containerleistung](./media/log-analytics-containers/containers-perf03.png)

## <a name="example-log-search-queries"></a>Beispielabfragen für die Protokollsuche
Es ist oft hilfreich, die Erstellung von Abfragen ausgehend von einem oder zwei Beispielen zu beginnen und diese dann an die eigene Umgebung anzupassen. Als Ausgangspunkt können Sie im Bereich **Beispielabfragen** experimentieren, um komplexere Abfragen zu erstellen.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Containerabfragen](./media/log-analytics-containers/containers-queries.png)


## <a name="saving-log-search-queries"></a>Speichern von Abfragen für die Protokollsuche
Das Speichern von Abfragen ist ein Standardfeature in Log Analytics. Durch die Speicherung können Sie schnell auf die Abfragen zurückgreifen, die Sie als besonders hilfreich empfinden.

Nachdem Sie eine Abfrage erstellt haben, die für Sie nützlich ist, speichern Sie sie, indem Sie oben auf der Seite „Protokollsuche“ auf **Favoriten** klicken. Sie können später auf der Seite **Mein Dashboard** auf einfache Weise darauf zugreifen.

## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich über [Protokollsuchen](log-analytics-log-searches.md) für die Anzeige ausführlicher Containerdatensätze.

