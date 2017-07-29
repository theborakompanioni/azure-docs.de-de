---
title: "VMware-Überwachungslösung in Log Analytics | Microsoft Docs"
description: "Erfahren Sie, wie die VMware-Überwachungslösung dabei helfen kann, Protokolle zu verwalten und ESXi-Hosts zu überwachen."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 16516639-cc1e-465c-a22f-022f3be297f1
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: banders
ms.translationtype: Human Translation
ms.sourcegitcommit: 74f34bdbf5707510c682814716aa0b95c19a5503
ms.openlocfilehash: a9fc8427e76ee8fa48fa8f1ad452c6fe9b544ce2
ms.contentlocale: de-de
ms.lasthandoff: 06/09/2017

---

# <a name="vmware-monitoring-preview-solution-in-log-analytics"></a>VMware-Überwachungslösung (Vorschau) in Log Analytics

![VMware-Symbol](./media/log-analytics-vmware/vmware-symbol.png)

Die VMware-Überwachungslösung in Log Analytics ist eine Lösung, die Ihnen dabei hilft, eine Methode für die zentralisierte Protokollierung und Überwachung für große VMware-Protokolle zu erstellen. Dieser Artikel beschreibt, wie Sie mithilfe der Lösung an einem Ort Probleme mit ESXi-Hosts beheben und die Hosts erfassen und verwalten. Mit der Lösung können Sie detaillierte Daten für alle ESXi-Hosts an einem einzigen Ort anzeigen. Sie sehen die wichtigsten Ereigniszahlen, Statusangaben und Trends der VM- und ESXi-Hosts, die über ESXi-Hostprotokolle bereitgestellt werden. Sie können Probleme behandeln, indem Sie zentralisierte ESXi-Hostprotokolle anzeigen und durchsuchen. Zudem können Sie Warnungen auf der Grundlage von Protokollsuchabfragen erstellen.

Die Lösung verwendet die native Syslog-Funktionalität des ESXi-Hosts, um Daten auf einen virtuellen Zielcomputer zu übertragen, auf dem sich ein OMS-Agent befindet. Die Lösung schreibt allerdings keine Dateien in das Syslog auf dem virtuellen Zielcomputer. Der OMS-Agent öffnet Port 1514 und lauscht dort. Sobald Daten empfangen werden, überträgt der OMS-Agent diese Daten an die OMS.

## <a name="installing-and-configuring-the-solution"></a>Installieren und Konfigurieren der Lösung
Verwenden Sie die folgenden Informationen zum Installieren und Konfigurieren der Lösung.

* Fügen Sie mithilfe des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md) beschriebenen Prozesses Ihrem OMS-Arbeitsbereich die VMware-Überwachungslösung hinzu.

#### <a name="supported-vmware-esxi-hosts"></a>Unterstützte VMware ESXi-Hosts
vSphere ESXi-Host 5.5 und 6.0

#### <a name="prepare-a-linux-server"></a>Vorbereiten eines Linux-Servers
Erstellen Sie eine VM mit Linux-Betriebssystem, um alle Syslog-Daten von den ESXi-Hosts zu empfangen. Vom [OMS-Linux-Agent](log-analytics-linux-agents.md) werden alle Syslog-Daten von ESXi-Hosts gesammelt. Sie können mehrere ESXi-Hosts verwenden, um die Protokolle an einen Linux-Server weiterzuleiten, wie im folgenden Beispiel veranschaulicht.  

   ![Syslog-Datenfluss](./media/log-analytics-vmware/diagram.png)

### <a name="configure-syslog-collection"></a>Konfigurieren der Syslog-Sammlung
1. Richten Sie die Syslog-Weiterleitung für VSphere ein. Ausführliche Informationen zur Einrichtung der Syslog-Weiterleitung finden Sie unter [Configuring syslog on ESXi 5.x and 6.0 (2003322)](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2003322) (Konfigurieren von Syslog auf ESXi 5.x und 6.0 (2003322)). Wechseln Sie zu **ESXi Host Configuration** > **Software** > **Advanced Settings** > **Syslog**.
   ![vsphereconfig](./media/log-analytics-vmware/vsphere1.png)  
2. Fügen Sie im Feld *Syslog.global.logHost* Ihren Linux-Server und die Portnummer *1514* hinzu. Beispiel: `tcp://hostname:1514` oder `tcp://123.456.789.101:1514`
3. Öffnen Sie die ESXi-Hostfirewall für Syslog. **ESXi Host Configuration** > **Software** > **Security Profile** > **Firewall**, und öffnen Sie **Properties**.  

    ![vspherefw](./media/log-analytics-vmware/vsphere2.png)  

    ![vspherefwproperties](./media/log-analytics-vmware/vsphere3.png)  
4. Überprüfen Sie die vSphere-Konsole, um sicherzustellen, dass Syslog ordnungsgemäß eingerichtet ist. Vergewissern Sie sich auf dem ESXI-Host, dass der Port **1514** konfiguriert ist.
5. Laden Sie den OMS-Agent für Linux auf den Linux-Server herunter, und installieren Sie ihn. Weitere Informationen finden Sie in der [Dokumentation zum OMS-Agent für Linux](https://github.com/Microsoft/OMS-Agent-for-Linux).
6. Nachdem der OMS-Agent für Linux installiert wurde, wechseln Sie zum Verzeichnis „/etc/opt/microsoft/omsagent/sysconf/omsagent.d“, und kopieren Sie die Datei „vmware_esxi.conf“ in das Verzeichnis „/etc/opt/microsoft/omsagent/conf/omsagent.d“. Ändern Sie dann den Besitzer/die Gruppe und die Berechtigungen der Datei. Beispiel:

    ```
    sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/vmware_esxi.conf /etc/opt/microsoft/omsagent/conf/omsagent.d
   sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf
    ```
7. Starten Sie den OMS-Agent für Linux neu, indem Sie `sudo /opt/microsoft/omsagent/bin/service_control restart` ausführen.
8. Testen Sie die Konnektivität zwischen dem Linux-Server und dem ESXi-Host mithilfe des Befehls `nc` auf dem ESXi-Host. Beispiel:

    ```
    [root@ESXiHost:~] nc -z 123.456.789.101 1514
    Connection to 123.456.789.101 1514 port [tcp/*] succeeded!
    ```

9. Führen Sie im OMS-Portal eine Protokollsuche nach `Type=VMware_CL` aus. Wenn OMS die Syslog-Daten sammelt, bleibt das Syslog-Format erhalten. Im Portal werden einige bestimmte Felder erfasst, z.B. *Hostname* und *ProcessName*.  

    ![type](./media/log-analytics-vmware/type.png)  

    Wenn die Protokollsuchergebnisse Ihrer Ansicht so ähnlich wie die Abbildung oben aussehen, können Sie das OMS-Dashboard für die VMware-Überwachungslösung verwenden.  

## <a name="vmware-data-collection-details"></a>Details zur VMware-Datensammlung
Die VMware-Überwachungslösung sammelt die verschiedenen Leistungsmetriken und Protokolldateien von ESXi-Hosts mithilfe des OMS-Agents für Linux, den Sie aktiviert haben.

Die folgende Tabelle zeigt die Datensammlungsmethoden und andere Details dazu, wie Daten gesammelt werden.

| Plattform | OMS-Agent für Linux | SCOM-Agent | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- |
| Linux |![Ja](./media/log-analytics-vmware/oms-bullet-green.png) |![Nein](./media/log-analytics-vmware/oms-bullet-red.png) |![Nein](./media/log-analytics-vmware/oms-bullet-red.png) |![Nein](./media/log-analytics-containers/oms-bullet-red.png) |![Nein](./media/log-analytics-vmware/oms-bullet-red.png) |Alle 3 Minuten |

In der folgenden Tabelle sind Beispiele für Datenfelder aufgeführt, die von der VMware-Überwachungslösung erfasst werden:

| Feldname | Beschreibung |
| --- | --- |
| Device_s |VMware-Speichergeräte |
| ESXIFailure_s |Fehlertypen |
| EventTime_t |Zeitpunkt, zu dem das Ereignis aufgetreten ist |
| HostName_s |ESXi-Hostname |
| Operation_s |VM erstellen oder VM löschen |
| ProcessName_s |Ereignisname |
| ResourceId_s |Name des VMware-Hosts |
| ResourceLocation_s |VMware |
| ResourceName_s |VMware |
| ResourceType_s |Hyper-V |
| SCSIStatus_s |VMware-SCSI-Status |
| SyslogMessage_s |Syslog-Daten |
| UserName_s |Benutzer, der die VM erstellt oder gelöscht hat |
| VMName_s |Name des virtuellen Computers |
| Computer |Hostcomputer |
| TimeGenerated |Zeitpunkt, zu dem die Daten generiert wurden |
| DataCenter_s |VMware-Rechenzentrum |
| StorageLatency_s |Speicherlatenz (ms) |

## <a name="vmware-monitoring-solution-overview"></a>Übersicht über die VMware-Überwachungslösung
Die VMware-Kachel wird im OMS-Portal angezeigt. Sie bietet eine allgemeine Ansicht der Fehler. Wenn Sie auf die Kachel klicken, gelangen Sie zu einer Dashboardansicht.

![Kachel](./media/log-analytics-vmware/tile.png)

#### <a name="navigate-the-dashboard-view"></a>Navigieren in der Dashboardansicht
In der **VMware**-Dashboardansicht sind die Blätter nach Folgendem angeordnet:

* Anzahl von Fehlerstatuswerten
* Wichtigste Hosts nach Ereignisanzahl
* Am häufigsten aufgetretene Ereignisse
* Aktivitäten virtueller Computer
* Datenträgerereignisse auf ESXi-Hosts

![Lösung1](./media/log-analytics-vmware/solutionview1-1.png)

![Lösung2](./media/log-analytics-vmware/solutionview1-2.png)

Klicken Sie auf ein Blatt, um den Log Analytics-Suchbereich zu öffnen, der detaillierte Informationen für das Blatt enthält.

Hier können Sie die Suchabfrage bearbeiten, um nach etwas Bestimmtem zu suchen. Ein Tutorial zu den Grundlagen der OMS-Suche finden Sie im [Tutorial zur OMS-Protokollsuche](log-analytics-log-searches.md).

#### <a name="find-esxi-host-events"></a>Suchen von ESXi-Hostereignissen
Ein einzelner ESXi-Host generiert basierend auf seinen Prozessen mehrere Protokolle. Die VMware-Überwachungslösung zentralisiert sie und fasst die auftretenden Ereignisse zusammen. Mit dieser zentralisierten Ansicht können Sie erkennen, auf welchem ESXi-Host eine große Anzahl von Ereignissen auftritt und welche Ereignisse in Ihrer Umgebung am häufigsten vorkommen.

![event](./media/log-analytics-vmware/events.png)

Sie können weitere Details anzeigen, indem Sie auf einen ESXi-Host oder einen Ereignistyp klicken.

Wenn Sie auf einen ESXi-Hostnamen klicken, sehen Sie Informationen von diesem ESXi-Host. Wenn Sie die Ergebnisse mit dem Ereignistyp eingrenzen möchten, fügen Sie Ihrer Suchabfrage `“ProcessName_s=EVENT TYPE”` hinzu. Sie können **ProcessName** im Suchfilter auswählen. So werden die Informationen für Sie eingegrenzt.

![Details](./media/log-analytics-vmware/eventhostdrilldown.png)

#### <a name="find-high-vm-activities"></a>Suchen nach umfangreichen VM-Aktivitäten
Ein virtueller Computer kann auf einem ESXi-Host erstellt und gelöscht werden. Es ist hilfreich für den Administrator zu bestimmen, wie viele virtuelle Computer ein ESXi-Host erstellt. Dies hilft wiederum dabei, die Leistungs- und Kapazitätsplanung zu verstehen. Die Nachverfolgung von VM-Aktivitätsereignissen ist entscheidend, wenn Sie Ihre Umgebung verwalten.

![Details](./media/log-analytics-vmware/vmactivities1.png)

Wenn Sie zusätzliche Daten zur VM-Erstellung auf einem ESXi-Host anzeigen möchten, klicken Sie auf einen ESXi-Hostnamen.

![Details](./media/log-analytics-vmware/createvm.png)

#### <a name="common-search-queries"></a>Allgemeine Suchabfragen
Die Lösung umfasst andere nützliche Abfragen, die Sie bei der Verwaltung Ihrer ESXi-Hosts unterstützen, z.B. hoher Speicherplatzbedarf, Speicherlatenz und Pfadfehler.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Abfragen](./media/log-analytics-vmware/queries.png)


#### <a name="save-queries"></a>Speichern von Abfragen
Das Speichern von Suchabfragen ist ein Standardfeature in OMS und hilft Ihnen, Abfragen zu behalten, die Sie für nützlich befunden haben. Nachdem Sie eine Abfrage erstellt haben, die für Sie nützlich ist, speichern Sie sie, indem Sie auf **Favoriten** klicken. Eine gespeicherte Abfrage können Sie später auf einfache Weise über die Seite [Mein Dashboard](log-analytics-dashboards.md) wiederverwenden, wo Sie eigene benutzerdefinierte Dashboards erstellen können.

![Docker-Dashboardansicht](./media/log-analytics-vmware/dockerdashboardview.png)

#### <a name="create-alerts-from-queries"></a>Erstellen von Warnungen aus Abfragen
Nachdem Sie Ihre Abfragen erstellt haben, könnten Sie die Abfragen dazu verwenden, dass Sie auf bestimmte Ereignisse hingewiesen werden. Informationen zum Erstellen von Warnungen finden Sie unter [Warnungen in Log Analytics](log-analytics-alerts.md). Beispiele für Warnungen durch Abfragen und weitere Beispiele für Abfragen finden Sie im Blogbeitrag [Monitor VMware using OMS Log Analytics](https://blogs.technet.microsoft.com/msoms/2016/06/15/monitor-vmware-using-oms-log-analytics) (Überwachen von VMware mit OMS Log Analytics).

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen
### <a name="what-do-i-need-to-do-on-the-esxi-host-setting-what-impact-will-it-have-on-my-current-environment"></a>Was muss ich bei den ESXi-Hosteinstellungen beachten? Wie wirken diese sich auf meine aktuelle Umgebung aus?
Die Lösung verwendet den nativen Syslog-Weiterleitungsmechanismus des ESXi-Hosts. Sie benötigen keine weitere Microsoft-Software auf dem ESXi-Host, um die Protokolle zu erfassen. Die Auswirkungen auf Ihre vorhandene Umgebung sind gering. Sie müssen die Syslog-Weiterleitung, eine Funktion von ESXi, allerdings einrichten.

### <a name="do-i-need-to-restart-my-esxi-host"></a>Muss ich meinen ESXi-Host neu starten?
Nein. Der Prozess erfordert keinen Neustart. In manchen Fällen aktualisiert vSphere das Syslog nicht ordnungsgemäß. Melden Sie sich in einem solchen Fall beim ESXi-Host an, und laden Sie das Syslog erneut. Auch hierbei müssen Sie den Host nicht neu starten, der Prozess verursacht also keine Unterbrechungen in Ihrer Umgebung.

### <a name="can-i-increase-or-decrease-the-volume-of-log-data-sent-to-oms"></a>Kann ich die Menge an Protokolldaten, die an die OMS gesendet werden, erhöhen oder verringern?
Ja, das ist möglich. Sie können die vSphere-Einstellungen für die Protokollebene des ESXi-Hosts verwenden. Die Protokollsammlung basiert auf der Ebene *Information*. Wenn Sie die Erstellung oder Löschung von virtuellen Computern überwachen möchten, müssen Sie „Hostd“ als Ebene für *Information* beibehalten. Weitere Informationen finden Sie in der [VMware Knowledge Base](https://kb.vmware.com/selfservice/microsites/search.do?&cmd=displayKC&externalId=1017658).

### <a name="why-is-hostd-not-providing-data-to-oms-my-log-setting-is-set-to-info"></a>Warum stellt Hostd keine Daten für die OMS bereit? Die Protokolleinstellungen sind auf „Information“ festgelegt.
Auf dem ESXi-Host ist ein Fehler beim Syslog-Zeitstempel aufgetreten. Weitere Informationen finden Sie in der [VMware Knowledge Base](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2111202). Nachdem Sie die Problemumgehung angewendet haben, sollte Hostd wieder normal funktionieren.

### <a name="can-i-have-multiple-esxi-hosts-forwarding-syslog-data-to-a-single-vm-with-omsagent"></a>Kann ich mehrere ESXi-Hosts für die Weiterleitung von Syslog-Daten an einen einzelnen virtuellen Computer mit OMS-Agent einrichten?
Ja. Sie können mehrere ESXi-Hosts einrichten, die Daten an einen einzelnen virtuellen Computer mit OMS-Agent weiterleiten.

### <a name="why-dont-i-see-data-flowing-into-oms"></a>Warum sehe ich in der OMS keinen Dateneingang?
Dafür kann es mehrere Gründe geben:

* Der ESXi-Host überträgt Daten nicht ordnungsgemäß an den virtuellen Computer, auf dem der OMS-Agent ausgeführt wird. Führen Sie zum Testen die folgenden Schritte aus:

  1. Melden Sie sich zur Bestätigung über SSH beim ESXi-Host an, und führen Sie den folgenden Befehl aus: `nc -z ipaddressofVM 1514`

      Wenn dies nicht erfolgreich ist, sind wahrscheinlich die vSphere-Einstellungen in der erweiterten Konfiguration nicht richtig. Informationen zum Einrichten des ESXi-Hosts für die Syslog-Weiterleitung finden Sie unter [Konfigurieren der Syslog-Sammlung](#configure-syslog-collection).
  2. Wenn die Portverbindung für Syslog erfolgreich hergestellt werden kann, Sie aber weiterhin keine Daten sehen, laden Sie das Syslog auf dem ESXi-Host neu, in dem Sie SSH verwenden, um folgenden Befehl auszuführen: ` esxcli system syslog reload`
* Der virtuelle Computer mit dem OMS-Agent ist nicht ordnungsgemäß eingerichtet. Führen Sie zum Testen die folgenden Schritte aus:

  1. Der OMS-Agent lauscht auf Port 1514 und überträgt Daten an die OMS. Überprüfen Sie mit dem folgenden Befehl, ob der Port offen ist: `netstat -a | grep 1514`
  2. Port `1514/tcp` sollte offen sein. Wenn dies nicht der Fall ist, stellen Sie sicher dass der OMS-Agent ordnungsgemäß installiert ist. Wenn keine Portinformationen angezeigt werden, ist der Syslog-Port auf dem virtuellen Computer nicht offen.

     1. Überprüfen Sie mit dem Befehl `ps -ef | grep oms`, ob der OMS-Agent ausgeführt wird. Wenn dies nicht der Fall ist, starten Sie den Prozess, indem Sie diesen Befehl ausführen: ` sudo /opt/microsoft/omsagent/bin/service_control start`
     2. Öffnen Sie die Datei `/etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf` .

         Überprüfen Sie mit einem Befehl ähnlich dem folgenden, ob die Benutzer- und Gruppeneinstellung richtig und gültig ist: `-rw-r--r-- 1 omsagent omiusers 677 Sep 20 16:46 vmware_esxi.conf`

         Wenn die Datei nicht vorhanden oder die Benutzer- und Gruppeneinstellung nicht richtig ist, beheben Sie das Problem durch [Vorbereiten eines Linux-Servers](#prepare-a-linux-server).

## <a name="next-steps"></a>Nächste Schritte
* Verwenden Sie [Protokollsuchvorgänge](log-analytics-log-searches.md) in Log Analytics, um ausführliche VMware-Hostdaten anzuzeigen.
* [Erstellen Sie eigene Dashboards](log-analytics-dashboards.md), die VMware-Hostdaten anzeigen.
* [Erstellen Sie Warnungen](log-analytics-alerts.md) für das Auftreten bestimmter VMware-Hostereignisse.

