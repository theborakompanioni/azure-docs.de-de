<properties
    pageTitle="Containerlösung in Log Analytics | Microsoft Azure"
    description="Die Containerlösung in Log Analytics unterstützt Sie beim Anzeigen und Verwalten Ihrer Docker-Containerhosts an einem Ort."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>




# <a name="containers-(preview)-solution-log-analytics"></a>Log Analytics-Containerlösung (Vorschau)

In diesem Artikel wird das Einrichten und Verwenden der Containerlösung in Log Analytics beschrieben, die Sie beim Anzeigen und Verwalten Ihrer Docker-Containerhosts an einem zentralen Ort unterstützt. Docker ist ein Softwarevirtualisierungssystem zum Erstellen von Containern, durch die das Bereitstellen von Software in einer IT-Infrastruktur automatisiert werden kann.

Mit der Lösung können Sie erkennen, welche Container auf Ihren Containerhosts ausgeführt und welche Images in den Containern ausgeführt werden. Sie können ausführliche Überwachungsinformationen anzeigen, die auch die mit Containern verwendeten Befehle enthalten. Außerdem können Sie die Probleme mit Containern beheben, indem Sie zentralisierte Protokolle anzeigen und durchsuchen, ohne eine Remoteanzeige der Docker-Hosts zu benötigen. Sie können Container suchen, die Störungen verursachen und übermäßig viele Ressourcen auf einem Host verbrauchen. Darüber hinaus können Sie an einem zentralen Ort Informationen zur Leistung und Auslastung von CPU, Arbeitsspeicher, Speicher und Netzwerk zu Containern anzeigen.

## <a name="installing-and-configuring-the-solution"></a>Installieren und Konfigurieren der Lösung

Verwenden Sie die folgenden Informationen zum Installieren und Konfigurieren der Lösung.

Fügen Sie mithilfe des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md) beschriebenen Prozesses Ihrem OMS-Arbeitsbereich die Containerlösung hinzu.

Es gibt zwei Methoden für das Installieren und Verwenden von Docker mit OMS:

- Auf unterstützten Linux-Betriebssystemen installieren Sie zunächst Docker und führen es aus. Anschließend installieren und konfigurieren Sie den OMS-Agent für Linux.
- Unter CoreOS installieren Sie Docker und führen es aus und konfigurieren dann OMSAgent für die Ausführung innerhalb eines Containers.

Überprüfen Sie die unterstützten Versionen von Docker und Linux-Betriebssystemen für Ihren Containerhost auf [GitHub](https://github.com/Microsoft/OMS-docker).

>[AZURE.IMPORTANT] Docker muss ausgeführt werden, **bevor** Sie den [OMS-Agent für Linux](log-analytics-linux-agents.md) auf Ihren Containerhosts installieren. Wenn Sie vor der Installation von Docker bereits den Agent installiert haben, müssen Sie den OMS-Agent für Linux erneut installieren. Weitere Informationen zu Docker finden Sie auf der [Docker-Website](https://www.docker.com).

Sie müssen die folgenden Einstellungen auf den Containerhosts konfigurieren, bevor Sie Container überwachen können.

## <a name="configure-settings-for-the-linux-container-host"></a>Konfigurieren von Einstellungen für den Linux-Containerhost

Verwenden Sie nach dem Installieren von Docker die folgenden Einstellungen für den Containerhost, um den Agent für die Verwendung mit Docker zu konfigurieren. CoreOS unterstützt diese Konfigurationsmethode nicht.

### <a name="to-configure-settings-for-the-container-host---systemd-(suse,-opensuse,-centos-7.x,-rhel-7.x,-and-ubuntu-15.x-and-higher)"></a>So konfigurieren Sie die Einstellungen für den Containerhost – systemd (SUSE, openSUSE, CentOS 7.x, RHEL 7.x und Ubuntu 15.x und höher)

1. Bearbeiten Sie „docker.service“, und fügen Sie Folgendes hinzu:

    ```
    [Service]
    ...
    Environment="DOCKER_OPTS=--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ...
    ```

2. Fügen Sie $DOCKER\_OPTS in &quot;ExecStart=/usr/bin/docker daemon&quot; in der Datei „docker.service“ hinzu. Verwenden Sie das folgende Beispiel.

    ```
    [Service]
    Environment="DOCKER_OPTS=--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ExecStart=/usr/bin/docker daemon -H fd:// $DOCKER_OPTS
    ```

3. Starten Sie den Docker-Dienst neu. Beispiel:

    ```
    sudo systemctl restart docker.service
    ```

### <a name="to-configure-settings-for-the-container-host---upstart-(ubuntu-14.x)"></a>So konfigurieren Sie die Einstellungen für den Containerhost – Upstart (Ubuntu 14.x)

1. Bearbeiten Sie „/etc/default/docker“, und fügen Sie Folgendes hinzu:

    ```
    DOCKER_OPTS="--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ```

2. Speichern Sie die Datei, und starten Sie den Docker- und den OMS-Dienst neu.

    ```
    sudo service docker restart
    ```

### <a name="to-configure-settings-for-the-container-host---amazon-linux"></a>So konfigurieren Sie die Einstellungen für den Containerhost – Amazon Linux

1. Bearbeiten Sie „/etc/sysconfig/docker“, und fügen Sie Folgendes hinzu:

    ```
    OPTIONS="--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ```

2. Speichern Sie die Datei, und starten Sie den Docker-Dienst neu.

    ```
    sudo service docker restart
    ```

## <a name="configure-settings-for-coreos-containers"></a>Konfigurieren von Einstellungen für CoreOS-Container

Nachdem Sie Docker installiert haben, verwenden Sie die folgenden Einstellungen für CoreOS, um Docker auszuführen und einen Container zu erstellen. Sie können bei dieser Konfigurationsmethode jede unterstützte Version von Linux verwenden – einschließlich CoreOS. Sie benötigen Ihre [OMS-Arbeitsbereichs-ID und den Schlüssel](log-analytics-linux-agents.md).

### <a name="to-use-oms-for-all-containers-with-coreos"></a>So verwenden Sie OMS für alle Container mit CoreOS

- Starten Sie den OMS-Container, den Sie überwachen möchten. Verwenden Sie das folgende Beispiel, und passen Sie es an.

  ```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -e WSID="your workspace id" -e KEY="your key" -h=`hostname` -p 127.0.0.1:25224:25224/udp -p 127.0.0.1:25225:25225 --name="omsagent" --log-driver=none --restart=always microsoft/oms
```

### <a name="switching-from-using-an-installed-agent-to-one-in-a-container"></a>Wechseln von der Verwendung eines installierten Agents zu einem in einem Container enthaltenen

Wenn Sie zuvor den direkt installierten Agent verwendet haben und stattdessen einen Agent verwenden möchten, der in einem Container ausgeführt wird, müssen Sie zunächst OMSAgent entfernen. Lesen Sie unter [Schritte zum Installieren des OMS-Agents für Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md) nach.

## <a name="containers-data-collection-details"></a>Details zur Datensammlung in Containern

Die Containerlösung sammelt verschiedene Leistungsmetriken und Protokolldaten von Containerhosts und Containern. Dazu verwendet sie die OMS-Agents für Linux, die Sie aktiviert haben, und OMSAgent, das in Containern ausgeführt wird.

Die folgende Tabelle enthält die Datensammlungsmethoden und andere Details dazu, wie Daten für Container erfasst werden.

| Plattform | OMS-Agent für Linux | SCOM-Agent | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
|---|---|---|---|---|---|---|
| Linux|![Ja](./media/log-analytics-containers/oms-bullet-green.png)|![Nein](./media/log-analytics-containers/oms-bullet-red.png)|![Nein](./media/log-analytics-containers/oms-bullet-red.png)|            ![Nein](./media/log-analytics-containers/oms-bullet-red.png)|![Nein](./media/log-analytics-containers/oms-bullet-red.png)| Alle 3 Minuten|


Die folgende Tabelle zeigt Beispiele für Datentypen, die von der Containersammlung gesammelt werden:

| Datentyp | Felder |
| --- | --- |
| Leistung von Hosts und Containern | Computer, ObjectName, CounterName &#40;%Processor Time, Disk Reads MB, Disk Writes MB, Memory Usage MB, Network Receive Bytes, Network Send Bytes, Processor Usage sec, Network&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem |
| Containerinhalt | TimeGenerated, Computer, Containername, ContainerHostname, Image, ImageTag, ContinerState, ExitCode, EnvironmentVar, Command, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID |
| Containerimageinhalt | TimeGenerated, Computer, Image, ImageTag, ImageSize, VirtualSize, Running, Paused, Stopped, Failed, SourceSystem, ImageID, TotalContainer |
| Containerprotokoll | TimeGenerated, Computer, Image-ID, Containername, LogEntrySource, LogEntry, SourceSystem, ContainerID |
| Containerdienstprotokoll | TimeGenerated, Computer, TimeOfCommand, Image, Command, SourceSystem, ContainerID |

## <a name="monitor-containers"></a>Überwachen von Containern

Nachdem Sie die Lösung im OMS-Portal aktiviert haben, werden auf der Kachel **Container** zusammenfassende Informationen zu den Containerhosts und den auf den Hosts ausgeführten Containern angezeigt.

![Kachel „Container“](./media/log-analytics-containers/containers-title.png)

Die Kachel zeigt eine Übersicht über die Anzahl der Container in der Umgebung an. Außerdem erfahren Sie, ob Container fehlerhaft sind, ausgeführt werden oder beendet wurden.

### <a name="using-the-containers-dashboard"></a>Verwenden des Containerdashboards

Klicken Sie auf die Kachel **Container**. Dort sind die Ansichten wie folgt angeordnet:

- Containerereignisse
- Fehler
- Containerstatus
- Containerimageinhalt
- Leistung von CPU und Arbeitsspeicher

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

- **ContainerInventory:** Verwenden Sie diesen Typ, wenn Sie Informationen zum Containerspeicherort und -namen sowie zu den darin ausgeführten Images benötigen.
- **ContainerImageInventory:** Verwenden Sie diesen Typ, wenn Sie die Informationen organisiert nach Image suchen und Imageinformationen wie Image-IDs oder -Größen anzeigen möchten.
- **ContainerLog:** Verwenden Sie diesen Typ, wenn Sie bestimmte Informationen und Einträge im Fehlerprotokoll suchen möchten.
- **ContainerServiceLog:** Verwenden Sie diesen Typ, wenn Sie Audit-Trail-Informationen zum Docker-Daemon suchen, z.B. zu den Befehlen „start“, „stop“, „delete“ oder „pull“.

### <a name="to-search-logs-for-container-data"></a>So durchsuchen Sie Protokolle nach Containerdaten

- Wählen Sie ein Image aus, bei dem kürzlich ein Fehler aufgetreten ist, und suchen Sie die zugehörigen Fehlerprotokolle. Suchen Sie zunächst mit **ContainerInventory** nach dem Namen eines Containers, in dem das Image ausgeführt wird. Suchen Sie beispielsweise nach `Type=ContainerInventory ubuntu Failed`.  
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

![Containerabfragen](./media/log-analytics-containers/containers-queries.png)

## <a name="saving-log-search-queries"></a>Speichern von Abfragen für die Protokollsuche

Das Speichern von Abfragen ist ein Standardfeature in Log Analytics. Durch die Speicherung können Sie schnell auf die Abfragen zurückgreifen, die Sie als besonders hilfreich empfinden.

Nachdem Sie eine Abfrage erstellt haben, die für Sie nützlich ist, speichern Sie sie, indem Sie oben auf der Seite „Protokollsuche“ auf **Favoriten** klicken. Sie können später auf der Seite **Mein Dashboard** auf einfache Weise darauf zugreifen.

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über [Protokollsuchen](log-analytics-log-searches.md) für die Anzeige ausführlicher Containerdatensätze.



<!--HONumber=Oct16_HO2-->


