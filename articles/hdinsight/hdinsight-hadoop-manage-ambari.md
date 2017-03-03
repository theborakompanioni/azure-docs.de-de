---
title: "Überwachen und Verwalten von Azure HDInsight-Clustern mithilfe der Ambari-Webbenutzeroberfläche | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Ambari zum Überwachen und Verwalten von Linux-basierten HDInsight-Clustern verwenden. In diesem Dokument erfahren Sie, wie Sie die in HDInsight-Clustern enthaltene Webbenutzeroberfläche Ambari verwenden."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 4787f3cc-a650-4dc3-9d96-a19a67aad046
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/08/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: e80bf82df28fbce8a1019c6eb07cfcae4cbba930
ms.openlocfilehash: 34f5c92dc565e644bd8c569bfbea65e92ace5a19
ms.lasthandoff: 02/09/2017


---
# <a name="manage-hdinsight-clusters-by-using-the-ambari-web-ui"></a>Verwalten von HDInsight-Clustern mithilfe der Ambari-Webbenutzeroberfläche

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Apache Ambari vereinfacht die Verwaltung und Überwachung von Hadoop-Clustern durch die Bereitstellung einer benutzerfreundlichen Webbenutzeroberfläche und REST-API. Ambari ist in Linux-basierten HDInsight-Clustern enthalten und wird verwendet, um den Cluster zu überwachen und Konfigurationsänderungen vorzunehmen.

In diesem Dokument erfahren Sie, wie Sie die Webbenutzeroberfläche Ambari mit einem HDInsight-Cluster verwenden.

## <a name="a-idwhatisawhat-is-ambari"></a><a id="whatis"></a>Was ist Ambari?

[Apache Ambari](http://ambari.apache.org) vereinfacht die Hadoop-Verwaltung durch die Bereitstellung einer benutzerfreundlichen Webbenutzeroberfläche, die zum Bereitstellen, Verwalten und Überwachen von Hadoop-Clustern verwendet werden kann. Entwickler können diese Funktionen mithilfe der [Ambari-REST-APIs](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)in ihre Anwendungen integrieren.

Die Ambari-Webbenutzeroberfläche wird standardmäßig mit HDInsight-Clustern mit dem Linux-Betriebssystem bereitgestellt.

> [!IMPORTANT]
> Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date). 

## <a name="connectivity"></a>Konnektivität

Die Ambari-Webbenutzeroberfläche ist in Ihrem HDInsight-Cluster unter „HTTPS://CLUSTERNAME.azurehdidnsight.net“ verfügbar, wobei **CLUSTERNAME** der Name Ihres Clusters ist. 

> [!IMPORTANT]
> Zum Herstellen einer Verbindung mit Ambari in HDInsight ist HTTPS erforderlich. Sie müssen sich bei Ambari auch mit dem Administratorkontonamen (standardmäßig **admin**) und dem Kennwort authentifizieren, die Sie bei der Erstellung des Clusters angegeben haben. 

## <a name="ssh-tunnel-proxy"></a>SSH-Tunnel (Proxy)

> [!NOTE]
> Während Ambari für Ihren Cluster direkt über das Internet erreichbar ist, werden einige Links der Ambari-Webbenutzeroberfläche (z. B. JobTracker) nicht im Internet verfügbar gemacht. Sie erhalten daher Fehler, die darauf hinweisen, dass der Server nicht gefunden wurde, wenn Sie versuchen, auf diese Funktionen zuzugreifen und keinen SSH-Tunnel (Secure Shell) für den Webdatenverkehr via Proxy zum Hauptknoten des Clusters verwenden.

Informationen zum Erstellen eines SSH-Tunnels, der mit Ambari funktioniert, finden Sie unter [Verwenden von SSH-Tunneling zum Zugriff auf die Ambari-Webbenutzeroberfläche, ResourceManager, JobHistory, NameNode, Oozie und andere Webbenutzeroberflächen](hdinsight-linux-ambari-ssh-tunnel.md).

## <a name="ambari-web-ui"></a>Ambari-Webbenutzeroberfläche

Wenn Sie eine Verbindung zur Webbenutzeroberfläche Ambari herstellen, werden Sie aufgefordert, die Seite zu authentifizieren. Verwenden Sie den Clusteradministrator (standardmäßig „Admin“) und das Kennwort, die Sie während der Erstellung des Clusters verwendet haben.

Beachten Sie beim Öffnen der Seite die Leiste im oberen Bereich. Diese enthält die folgenden Informationen und Steuerelemente:

![Ambari-Navigation](./media/hdinsight-hadoop-manage-ambari/ambari-nav.png)

* **Ambari-Logo** : Zeigt das Dashboard an, mit dem der Cluster überwacht werden kann.

* **Clustername (Anzahl) ops** : Zeigt die Anzahl der laufenden Ambari-Vorgänge an. Wenn Sie den Clusternamen oder die **(Anzahl) ops** auswählen, wird eine Liste der Hintergrundvorgänge angezeigt.

* **(Anzahl) alerts**: Warnungen oder kritische Warnungen, die ggf. für den Cluster angezeigt werden. Wenn Sie diese Option auswählen, wird eine Liste der Warnungen angezeigt.

* **Dashboard** : Zeigt das Dashboard an.

* **Services** : Informationen und Konfigurationseinstellungen für die Dienste im Cluster.

* **Hosts** : Informationen und Konfigurationseinstellungen für die Knoten im Cluster.

* **Alerts** : Eine Liste von Informationen, Warnungen und kritischen Warnungen.

* **Admin** : Softwarestapel/Dienste, die im Cluster installiert sind, Dienstkontoinformationen sowie die Kerberos-Sicherheit.

* **Admin** : Ambari-Verwaltung, Benutzereinstellungen und Abmeldung.

## <a name="monitoring"></a>Überwachung

### <a name="alerts"></a>Warnungen

Ambari gibt häufig Warnungen mit einer der folgenden Statusangaben aus:

* **OK**
* **Warning**
* **CRITICAL**
* **UNKNOWN**

Bei anderen Warnungen als **OK** wird am Seitenanfang unter dem Eintrag **(Anzahl) alerts** die Anzahl der Warnungen anzeigt. Wenn Sie diesen Eintrag auswählen, werden die Warnungen und deren Statusangaben angezeigt.

Warnungen sind in mehrere Standardgruppen angeordnet, die über die Seite **Alerts** angezeigt werden können.

![Seite "Warnungen"](./media/hdinsight-hadoop-manage-ambari/alerts.png)

Sie können die Gruppen verwalten, indem Sie das Menü **Aktionen** und dann die Option **Warnungsgruppen verwalten** auswählen. Dadurch können Sie vorhandene Gruppen ändern oder neue Gruppen erstellen.

![Warnungsgruppen verwalten (Dialogfeld)](./media/hdinsight-hadoop-manage-ambari/manage-alerts.png)

Über den Eintrag __Warnungsbenachrichtigungen verwalten__ im Menü **Aktionen** können Sie ebenfalls Warnungsmethoden verwalten und Warnungsbenachrichtigungen erstellen. Über diesen Menüeintrag zeigen Sie aktuelle Benachrichtigungen an und können auch neue Benachrichtigungen erstellen. Die Benachrichtigungen können bei bestimmten Kombinationen aus Warnung und Schweregrad über **E-Mail** oder **SNMP** gesendet werden. Sie können z.B. eine Warnung senden, wenn eine der Benachrichtigungen in der Gruppe **YARN Default** auf **Kritisch** festgelegt ist.

![Warnung erstellen (Dialogfeld)](./media/hdinsight-hadoop-manage-ambari/create-alert-notification.png)

Über den Eintrag __Warnungseinstellungen verwalten__ im Menü __Aktionen__ können Sie schließlich festlegen, wie oft eine Warnung ausgegeben werden muss, bevor eine Benachrichtigung gesendet wird. Dadurch können Benachrichtigungen für vorübergehende Fehler, wie den Ausfall eines Dienstes auf einem Hauptknoten und den anschließenden Neustart des Dienstes auf einem anderen Hauptknoten, vermieden werden.

### <a name="cluster"></a>Cluster

Die Registerkarte **Metrics** im Dashboard enthält eine Reihe von Widgets, die das Überwachen des Clusterstatus auf einen Blick erleichtern. Verschiedene Widgets wie **CPU Usage**bieten zusätzliche Informationen, wenn Sie darauf klicken.

![Dashboard mit Metriken](./media/hdinsight-hadoop-manage-ambari/metrics.png)

Die Registerkarte **Heatmaps** zeigt Metriken als farbige Wärmebilder an, die von Grün bis Rot reichen.

![Dashboard mit Heatmaps](./media/hdinsight-hadoop-manage-ambari/heatmap.png)

Ausführlichere Informationen zu den Knoten im Cluster finden Sie unter **Hosts**. Wählen Sie dort den jeweiligen Knoten, an dem Sie interessiert sind.

![Hostdetails](./media/hdinsight-hadoop-manage-ambari/host-details.png)

### <a name="services"></a>Dienste

Die Seitenleiste **Dienste** des Dashboards bietet einen schnellen Überblick über den Status der Dienste, die im Cluster aktiv sind. Es werden verschiedene Symbole verwendet, um den Status oder auszuführende Aktionen anzuzeigen, z. B. ein gelbes Recycling-Symbol, wenn ein Dienst wiederverwendet werden muss.

![Seitenleiste "Services"](./media/hdinsight-hadoop-manage-ambari/service-bar.png)

> [!NOTE]
> Je nach Typ und Version des HDInsight-Clusters werden unterschiedliche Dienste angezeigt. Hier werden daher möglicherweise andere Dienste angezeigt als für Ihren Cluster.

Wenn Sie einen Dienst auswählen, werden weitere ausführliche Informationen zum Dienst angezeigt.

![Zusammenfassungsinformationen zum Dienst](./media/hdinsight-hadoop-manage-ambari/service-details.png)

#### <a name="quick-links"></a>Quicklinks

Einige Dienste zeigen oben auf der Seite den Link **Quicklinks** an. Hierüber können Sie auf dienstspezifische Webbenutzeroberflächen zugreifen. Beispiel:

* **Job History** : MapReduce-Auftragsverlauf.
* **Resource Manager** : Benutzeroberfläche des YARN-Ressourcen-Managers.
* **NameNode** : NameNode-Benutzeroberfläche im Hadoop Distributed File System (HDFS).
* **Oozie Web UI** : Oozie-Benutzeroberfläche.

Wenn Sie einen dieser Links öffnen, wird eine Registerkarte im Browser geöffnet, auf der die ausgewählte Seite angezeigt wird.

> [!NOTE]
> Wenn Sie einen **Quick Link** für einen Dienst auswählen, führt dies zum Fehler „Server nicht gefunden“, sofern Sie nicht einen SSL-Tunnel (Secure Sockets Layer) verwenden, um den Webdatenverkehr per Proxy an den Cluster weiterzuleiten. Die Ursache ist, dass die Webanwendungen, mit denen diese Informationen angezeigt werden, nicht im Internet verfügbar gemacht werden.
> 
> Informationen zum Verwenden eines SSL-Tunnels mit HDInsight finden Sie unter [Verwenden von SSH-Tunneling zum Zugriff auf die Ambari-Webbenutzeroberfläche, ResourceManager, JobHistory, NameNode, Oozie und andere Webbenutzeroberflächen](hdinsight-linux-ambari-ssh-tunnel.md)

## <a name="management"></a>Verwaltung

### <a name="ambari-users-groups-and-permissions"></a>Ambari-Benutzer, -Gruppen und -Berechtigungen

Das Arbeiten mit Benutzern, Gruppen und Berechtigungen wird bei Verwendung eines in die [Domäne eingebundenen](hdinsight-domain-joined-introduction.md) HDInsight-Clusters unterstützt. Informationen zur Verwendung der Ambari-Verwaltungsoberfläche in einem in die Domäne eingebundenen Cluster finden Sie unter [Verwalten von in die Domäne eingebundenen HDInsight-Clustern](hdinsight-domain-joined-introduction.md).

> [!WARNING]
> Ändern Sie nicht das Kennwort für den Ambari-Watchdog (hdinsightwatchdog) in Ihrem Linux-basierten HDInsight-Cluster. Durch eine Kennwortänderung wird die Möglichkeit zum Verwenden von Skriptaktionen oder zum Durchführen von Skalierungsvorgängen mit Ihren Cluster deaktiviert.

### <a name="hosts"></a>Host

Die Seite **Hosts** listet alle Hosts im Cluster auf. Gehen Sie folgendermaßen vor, um Hosts zu verwalten.

![Seite "Hosts"](./media/hdinsight-hadoop-manage-ambari/hosts.png)

> [!NOTE]
> Für HDInsight-Cluster sollte kein Host hinzugefügt, zurückgesetzt oder wiederhergestellt werden.

1. Wählen Sie die Hosts aus, die Sie verwalten möchten.

2. Verwenden Sie das Menü **Actions** , um die Aktion auswählen, die Sie durchführen möchten:
   
   * **Start all components** : Alle Komponenten auf dem Host starten.

   * **Stop all components** : Alle Komponenten auf dem Host beenden.

   * **Restart all components** : Alle Komponenten auf dem Host neu starten.

   * **Wartungsmodus aktivieren**: Unterdrückt Warnungen für den Host. Dies sollte aktiviert werden, wenn Sie Aktionen ausführen, die Warnungen generieren, z. B. Neustarten eines Dienstes, den ausgeführte Dienste benötigen.

   * **Turn off maintenance mode** : Versetzt den Host wieder in den normalen Warnmodus.

   * **Stop** : Beendet "DataNode" oder "NodeManager" auf dem Host.

   * **Start** : Startet "DataNode" oder "NodeManager" auf dem Host.

   * **Restart** : Beendet "DataNode" oder "NodeManager" auf dem Host und startet sie dann neu.

   * **Decommission** : Entfernt einen Host aus dem Cluster.
     
     > [!NOTE]
     > Verwenden Sie diese Aktion nicht für HDInsight-Cluster.

   * **Recommission** : Fügt einen zuvor außer Betrieb gesetzten Host zum Cluster hinzu.
     
     > [!NOTE]
     > Verwenden Sie diese Aktion nicht für HDInsight-Cluster.

### <a name="a-idserviceaservices"></a><a id="service"></a>Dienste

Verwenden Sie auf der Seite **Dashboard** oder **Dienste** am Ende der Dienstliste die Schaltfläche **Aktionen**, um alle Dienste zu beenden und zu starten.

![Service Actions](./media/hdinsight-hadoop-manage-ambari/service-actions.png)

> [!WARNING]
> **Dienst hinzufügen** wird in diesem Menü zwar aufgeführt, sollte jedoch nicht verwendet werden, um dem HDInsight-Cluster Dienste hinzuzufügen. Neue Dienste sollten während der Clusterbereitstellung mit einer Skriptaktion hinzugefügt werden. Weitere Informationen zur Verwendung von Skriptaktionen finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md).

Während die Schaltfläche **Actions** alle Dienste neu starten kann, ist es möglicherweise häufig erforderlich, dass Sie bestimmte Dienste starten, beenden oder neu starten möchten. Gehen Sie wie folgt vor, um Aktionen für einzelne Dienste auszuführen:

1. Wählen Sie auf der Seite **Dashboard** oder **Dienste** einen Dienst aus.

2. Verwenden Sie am oberen Rand der Registerkarte **Zusammenfassung** die Schaltfläche **Dienstaktionen**, und wählen Sie dann die auszuführende Aktion aus. Dadurch wird der Dienst auf allen Knoten neu gestartet.
   
    ![Dienstaktion](./media/hdinsight-hadoop-manage-ambari/individual-service-actions.png)
   
   > [!NOTE]
   > Der Neustart einiger Dienste bei aktivem Cluster kann dazu führen, dass Warnungen generiert werden. Um dies zu vermeiden, können Sie die Schaltfläche **Dienstaktionen** verwenden, um den **Wartungsmouds** für den Dienst zu aktivieren, bevor Sie den Neustart durchführen.

3. Nachdem eine Aktion ausgewählt wurde, wird der Eintrag **(Anzahl) ops** am Seitenanfang erhöht, um anzuzeigen, dass ein Hintergrundvorgang ausgeführt wird. Sofern die Anzeige konfiguriert ist, wird die Liste der Hintergrundvorgänge angezeigt.
   
   > [!NOTE]
   > Wenn Sie den **Wartungsmodus** für den Dienst aktiviert haben, denken Sie daran, ihn mithilfe der Schaltfläche **Dienstaktionen** wieder zu deaktivieren, sobald der Vorgang abgeschlossen ist.

Gehen Sie wie folgt vor, um einen Dienst zu konfigurieren:

1. Wählen Sie auf der Seite **Dashboard** oder **Dienste** einen Dienst aus.

2. Wählen Sie die Registerkarte **Configs** aus. Daraufhin wird die aktuelle Konfiguration angezeigt. Zudem wird eine Liste der vorherigen Konfigurationen angezeigt.
   
    ![Konfigurationen](./media/hdinsight-hadoop-manage-ambari/service-configs.png)

3. Verwenden Sie die angezeigten Felder, um die Konfiguration zu ändern, und klicken Sie dann auf **Save**. Sie können auch eine vorherige Konfiguration und dann **Make current** auswählen, um die vorherigen Einstellungen zu reaktivieren.

## <a name="ambari-views"></a>Ambari-Ansichten

Ambari-Ansichten ermöglichen Entwicklern das Hinzufügen von Benutzeroberflächenelementen zur Ambari-Webbenutzeroberfläche mit dem [Ambari Views Framework](https://cwiki.apache.org/confluence/display/AMBARI/Views). HDInsight bietet für Hadoop-Clustertypen die folgenden Ansichten:

* Yarn-Warteschlangen-Manager: Der Warteschlangen-Manager bietet eine einfache Benutzeroberfläche zum Anzeigen und Ändern von YARN-Warteschlangen.

* Hive-Ansicht: In der Hive-Ansicht können Sie Hive-Abfragen direkt in Ihrem Webbrowser ausführen. Sie können Abfragen speichern sowie Ergebnisse anzeigen, im Clusterspeicher speichern oder auf Ihr lokales System herunterladen. Weitere Informationen zum Verwenden von Hive-Ansichten finden Sie unter [Verwenden von Hive-Ansichten mit HDInsight](hdinsight-hadoop-use-hive-ambari-view.md).

* Tez-Ansicht: Mithilfe der Tez-Ansicht können Sie Aufträge besser verstehen und optimieren, indem Sie Informationen dazu anzeigen, wie Tez-Aufträge ausgeführt und welche Ressourcen von den Aufträgen verwendet werden.


