---
title: Erfassen von Nagios- und Zabbix-Warnungen in OMS Log Analytics | Microsoft-Dokumentation
description: "Nagios und Zabbix sind Open-Source-Überwachungstools. Sie können Warnungen von diesen Tools in Log Analytics erfassen, um diese zusammen mit Warnungen aus anderen Quellen zu analysieren.  Dieser Artikel beschreibt, wie der OMS-Agent für Linux für die Erfassung von Warnungen von diesen Systemen konfiguriert wird."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: f1d5bde4-6b86-4b8e-b5c1-3ecbaba76198
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/04/2017
ms.author: magoedte
ms.translationtype: Human Translation
ms.sourcegitcommit: 3bbc9e9a22d962a6ee20ead05f728a2b706aee19
ms.openlocfilehash: 0b64c32e1031e704d50aab0b38eaea41e27d134b
ms.contentlocale: de-de
ms.lasthandoff: 06/10/2017


---
# <a name="collect-alerts-from-nagios-and-zabbix-in-log-analytics-from-oms-agent-for-linux"></a>Erfassen von Warnungen von Nagios und Zabbix in Log Analytics durch den OMS-Agent für Linux 
[Nagios](https://www.nagios.org/) und [Zabbix](http://www.zabbix.com/) sind Open-Source Überwachungstools.  Sie können Warnungen in Log Analytics über diese Tools erfassen, um sie zusammen mit [Warnungen aus anderen Quellen](log-analytics-alerts.md) zu analysieren.  Dieser Artikel beschreibt, wie der OMS-Agent für Linux für die Erfassung von Warnungen von diesen Systemen konfiguriert wird.
 
## <a name="configure-alert-collection"></a>Konfigurieren der Erfassung von Warnungen

### <a name="configuring-nagios-alert-collection"></a>Konfigurieren der Erfassung von Nagios-Warnungen
Führen Sie die folgenden Schritte auf dem Nagios-Server aus, um Warnungen zu erfassen.

1. Gewähren Sie dem Benutzer **omsagent** Lesezugriff auf die Nagios-Protokolldatei (d.h. `/var/log/nagios/nagios.log`). Sie können den Benutzer **omsagent** der **nagios**-Gruppe hinzufügen, vorausgesetzt, die „nagios.log“-Datei gehört der Gruppe `nagios`. 

    sudo usermod -a -G nagios omsagent

2.  Ändern Sie die Konfigurationsdatei in (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`). Stellen Sie sicher, dass die folgenden Einträge vorhanden und nicht auskommentiert sind:  

        <source>  
          type tail  
          #Update path to point to your nagios.log  
          path /var/log/nagios/nagios.log  
          format none  
          tag oms.nagios  
        </source>  
      
        <filter oms.nagios>  
          type filter_nagios_log  
        </filter>  

3. Starten Sie den omsagent-Daemon neu.

    ```
    sudo sh /opt/microsoft/omsagent/bin/service_control restart
    ```

### <a name="configuring-zabbix-alert-collection"></a>Konfigurieren der Erfassung von Zabbix-Warnungen
Sie müssen einen Benutzer und ein Kennwort in *Klartext* eingeben, um Warnungen von einem Zabbix-Server zu erfassen. Dies ist nicht ideal, aber es wird empfohlen, dass Sie den Benutzer erstellen und nur Überwachungsberechtigungen gewähren.

Führen Sie die folgenden Schritte auf dem Nagios-Server aus, um Warnungen zu erfassen.

1. Ändern Sie die Konfigurationsdatei in (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`). Stellen Sie sicher, dass die folgenden Einträge vorhanden und nicht auskommentiert sind.  Ändern Sie den Benutzernamen und das Kennwort in Werte für Ihre Zabbix-Umgebung.

        <source>
         type zabbix_alerts
         run_interval 1m
         tag oms.zabbix
         zabbix_url http://localhost/zabbix/api_jsonrpc.php
         zabbix_username Admin
         zabbix_password zabbix
        </source>

2. Starten Sie den omsagent-Daemon neu.

    sudo sh /opt/microsoft/omsagent/bin/service_control restart


## <a name="alert-records"></a>Warnungsdatensätze
Sie können mit [Protokollsuchen](log-analytics-log-searches.md) Warnungsdatensätze von Nagios und Zabbix in Log Analytics abrufen.

### <a name="nagios-alert-records"></a>Nagios-Warnungsdatensätze

Von Nagios erfasste Datensätze haben den **Typ** **Warnung** und das **SourceSystem** **Nagios**.  Sie verfügen über die Eigenschaften, die in der folgenden Tabelle aufgeführt sind.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Typ |*Warnung* |
| SourceSystem |*Nagios* |
| AlertName |Name der Warnung. |
| AlertDescription | Beschreibung der Warnung. |
| AlertState | Status des Diensts oder Hosts<br><br>OK<br>WARNING<br>UP<br>DOWN |
| HostName | Der Name des Hosts, der die Warnung erstellt hat |
| PriorityNumber | Prioritätsstufe der Warnung. |
| StateType | Der Statustyp der Warnung<br><br>SOFT: Problem, das nicht erneut geprüft wurde<br>HARD: Problem, das so oft wie angegeben erneut geprüft wurde  |
| TimeGenerated |Datum und Uhrzeit der Warnungserstellung. |


### <a name="zabbix-alert-records"></a>Zabbix-Warnungsdatensätze
Von Zabbix erfasste Datensätze haben den **Typ** **Warnung** und das **SourceSystem** **Zabbix**.  Sie verfügen über die Eigenschaften, die in der folgenden Tabelle aufgeführt sind.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Typ |*Warnung* |
| SourceSystem |*Zabbix* |
| AlertName | Name der Warnung. |
| AlertPriority | Schweregrad der Warnung<br><br>not classified<br>Information<br>Warnung<br>average<br>high<br>disaster  |
| AlertState | Status der Warnung<br><br>0: Status ist auf dem neuesten Stand.<br>1: Status ist unbekannt.  |
| AlertTypeNumber | Gibt an, ob Warnungen mehrere Problemereignisse generieren können<br><br>0: Status ist auf dem neuesten Stand.<br>1: Status ist unbekannt.    |
| Kommentare | Zusätzliche Kommentare zur Warnung |
| HostName | Der Name des Hosts, der die Warnung erstellt hat |
| PriorityNumber | Der Wert, der den Schweregrad der Warnung angibt<br><br>0: Nicht klassifiziert<br>1: Information<br>2: Warnung<br>3: Durchschnitt<br>4: Hoch<br>5: Notfall |
| TimeGenerated |Datum und Uhrzeit der Warnungserstellung. |
| TimeLastModified |Datum und Uhrzeit der letzten Änderung des Status der Warnung |


## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zu [Warnungen](log-analytics-alerts.md) finden Sie in Log Analytics.
* Erfahren Sie mehr über [Protokollsuchvorgänge](log-analytics-log-searches.md) zum Analysieren der aus Datenquellen und Lösungen gesammelten Daten. 

