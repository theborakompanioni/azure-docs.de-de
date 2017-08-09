---
title: Erfassen und Analysieren von Syslog-Nachrichten in OMS Log Analytics | Microsoft-Dokumentation
description: "Syslog ist ein gängiges Protokoll zur Ereignisprotokollierung für Linux. Dieser Artikel beschreibt die Konfiguration der Sammlung von Syslog-Nachrichten in Log Analytics sowie Details zu den Datensätzen, die im OMS-Repository erstellt werden."
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
ms.date: 07/12/2017
ms.author: magoedte;bwren
ms.translationtype: HT
ms.sourcegitcommit: 137671152878e6e1ee5ba398dd5267feefc435b7
ms.openlocfilehash: 7513f405d5c7c05a8e6e2b7b0e6313f23a319c84
ms.contentlocale: de-de
ms.lasthandoff: 07/28/2017

---
# <a name="syslog-data-sources-in-log-analytics"></a>Syslog-Datenquellen in Log Analytics
Syslog ist ein gängiges Protokoll zur Ereignisprotokollierung für Linux.  Anwendungen senden Nachrichten, die auf dem lokalen Computer gespeichert oder an einen Syslog-Sammler übermittelt werden können.  Wenn der OMS-Agent für Linux installiert ist, konfiguriert er den lokalen Syslog-Daemon zum Weiterleiten von Nachrichten an den Agent.  Der Agent sendet die Nachricht dann an Log Analytics, wo ein entsprechender Datensatz im OMS-Repository erstellt wird.  

> [!NOTE]
> Log Analytics unterstützt die Sammlung der von „rsyslog“ oder „syslog-ng“ gesendeten Nachrichten, wobei „rsyslog“ der Standarddaemon ist. Der Standard-syslog-Daemon in Version 5 von Red Hat Enterprise Linux, CentOS und Oracle Linux-Version (sysklog) wird für die syslog-Ereigniserfassung nicht unterstützt. Der [Rsyslog-Daemon](http://rsyslog.com) sollte installiert und so konfiguriert werden, dass er Sysklog ersetzt, um Syslog-Daten von dieser Version dieser Verteilungen zu sammeln.
>
>

![Syslog-Sammlung](media/log-analytics-data-sources-syslog/overview.png)

## <a name="configuring-syslog"></a>Konfigurieren von Syslog
Der OMS-Agent für Linux sammelt nur Ereignisse mit den Einrichtungen und Schweregraden, die in ihrer Konfiguration angegeben werden.  Sie können Syslog über das OMS-Portal oder durch die Verwaltung von Konfigurationsdateien für die Linux-Agents konfigurieren.

### <a name="configure-syslog-in-the-oms-portal"></a>Konfigurieren von Syslog im OMS-Portal
Konfigurieren Sie Syslog über das [Menü „Daten“ in den Einstellungen von Log Analytics](log-analytics-data-sources.md#configuring-data-sources).  Diese Konfiguration wird in der Konfigurationsdatei für jeden Linux-Agent bereitgestellt.

Sie können eine neue Einrichtung hinzufügen, indem Sie ihren Namen eingeben und auf **+**.  Für jede Einrichtung werden nur Ereignisse mit den ausgewählten Schweregraden gesammelt.  Markieren Sie die Schweregrade für die jeweilige Einrichtung, aus der Sie Daten sammeln möchten.  Sie können keine zusätzlichen Kriterien angeben, um Nachrichten zu filtern.

![Konfigurieren von Syslog](media/log-analytics-data-sources-syslog/configure.png)

Standardmäßig werden alle Konfigurationsänderungen automatisch per Push an alle Agents weitergegeben.  Wenn Sie Syslog manuell auf jedem Linux-Agent ändern möchten, deaktivieren Sie das Kontrollkästchen *Nachstehende Konfiguration auf meine Linux-Computer anwenden*.

### <a name="configure-syslog-on-linux-agent"></a>Konfigurieren von Syslog auf dem Linux-Agent
Wenn der [OMS-Agent auf einem Linux-Client installiert ist](log-analytics-linux-agents.md), installiert er eine standardmäßige Syslog-Konfigurationsdatei, die Einrichtung und Schweregrad der gesammelten Nachrichten definiert.  Sie können diese Datei ändern, um die Konfiguration zu ändern.  Die Konfigurationsdatei unterscheidet sich je nach dem Syslog-Daemon, den der Client installiert hat.

> [!NOTE]
> Wenn Sie die syslog-Konfiguration bearbeiten, müssen Sie den syslog-Daemon neu starten, damit die Änderungen wirksam werden.
>
>

#### <a name="rsyslog"></a>rsyslog
Die Konfigurationsdatei für rsyslog befindet sich unter **/etc/rsyslog.d/95-omsagent.conf**.  Ihr Standardinhalt wird unten aufgeführt.  So werden Syslog-Nachrichten gesammelt, die der lokale Agent für alle Einrichtungen auf Warnungsebene oder höher gesendet hat.

    kern.warning       @127.0.0.1:25224
    user.warning       @127.0.0.1:25224
    daemon.warning     @127.0.0.1:25224
    auth.warning       @127.0.0.1:25224
    syslog.warning     @127.0.0.1:25224
    uucp.warning       @127.0.0.1:25224
    authpriv.warning   @127.0.0.1:25224
    ftp.warning        @127.0.0.1:25224
    cron.warning       @127.0.0.1:25224
    local0.warning     @127.0.0.1:25224
    local1.warning     @127.0.0.1:25224
    local2.warning     @127.0.0.1:25224
    local3.warning     @127.0.0.1:25224
    local4.warning     @127.0.0.1:25224
    local5.warning     @127.0.0.1:25224
    local6.warning     @127.0.0.1:25224
    local7.warning     @127.0.0.1:25224

Sie können eine Einrichtung entfernen, indem Sie ihren Abschnitt aus der Konfigurationsdatei entfernen.  Sie können die Schweregrade, die für eine bestimmte Einrichtung gesammelt werden, durch Ändern des Eintrags dieser Einrichtung beschränken.  Um z.B. die Einrichtung „user“ auf Nachrichten mit mindestens Fehlerschweregrad zu begrenzen, ändern Sie diese Zeile der Konfigurationsdatei wie folgt:

    user.error    @127.0.0.1:25224


#### <a name="syslog-ng"></a>syslog-ng
Die Konfigurationsdatei für „syslog-ng“ befindet sich am Speicherort **/etc/syslog-ng/syslog-ng.conf**.  Ihr Standardinhalt wird unten aufgeführt.  So werden Syslog-Nachrichten gesammelt, die der lokale Agent für alle Einrichtungen und alle Schweregrade gesendet hat.   

    #
    # Warnings (except iptables) in one file:
    #
    destination warn { file("/var/log/warn" fsync(yes)); };
    log { source(src); filter(f_warn); destination(warn); };

    #OMS_Destination
    destination d_oms { udp("127.0.0.1" port(25224)); };

    #OMS_facility = auth
    filter f_auth_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(auth); };
    log { source(src); filter(f_auth_oms); destination(d_oms); };

    #OMS_facility = authpriv
    filter f_authpriv_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(authpriv); };
    log { source(src); filter(f_authpriv_oms); destination(d_oms); };

    #OMS_facility = cron
    filter f_cron_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(cron); };
    log { source(src); filter(f_cron_oms); destination(d_oms); };

    #OMS_facility = daemon
    filter f_daemon_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(daemon); };
    log { source(src); filter(f_daemon_oms); destination(d_oms); };

    #OMS_facility = kern
    filter f_kern_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(kern); };
    log { source(src); filter(f_kern_oms); destination(d_oms); };

    #OMS_facility = local0
    filter f_local0_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local0); };
    log { source(src); filter(f_local0_oms); destination(d_oms); };

    #OMS_facility = local1
    filter f_local1_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local1); };
    log { source(src); filter(f_local1_oms); destination(d_oms); };

    #OMS_facility = mail
    filter f_mail_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(mail); };
    log { source(src); filter(f_mail_oms); destination(d_oms); };

    #OMS_facility = syslog
    filter f_syslog_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(syslog); };
    log { source(src); filter(f_syslog_oms); destination(d_oms); };

    #OMS_facility = user
    filter f_user_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };

Sie können eine Einrichtung entfernen, indem Sie ihren Abschnitt aus der Konfigurationsdatei entfernen.  Sie können die Schweregrade, die für eine bestimmte Einrichtung gesammelt werden, beschränken, indem Sie sie aus deren Liste entfernen.  Um z.B. die Einrichtung „user“ auf Warnungen und kritische Nachrichten zu begrenzen, ändern Sie diesen Abschnitt der Konfigurationsdatei wie folgt:

    #OMS_facility = user
    filter f_user_oms { level(alert,crit) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };


### <a name="collecting-data-from-additional-syslog-ports"></a>Sammeln der Daten von weiteren Syslog-Ports
Der OMS-Agent lauscht auf dem lokalen Client am Port 25224 auf Syslog-Nachrichten.  Wenn der Agent installiert ist, wird eine Syslog-Standardkonfiguration angewendet, die an folgendem Speicherort verfügbar ist:

* Rsyslog: `/etc/rsyslog.d/95-omsagent.conf`
* Syslog-ng: `/etc/syslog-ng/syslog-ng.conf`

Sie können die Portnummer ändern, indem Sie zwei Konfigurationsdateien erstellen: eine FluentD-Konfigurationsdatei und eine rsyslog- oder syslog-ng-Konfigurationsdatei, abhängig vom installierten Syslog-Daemon.  

* Die FluentD-Konfigurationsdatei muss eine neue Datei im Verzeichnis `/etc/opt/microsoft/omsagent/conf/omsagent.d` sein. Ersetzen Sie dann den Wert im Eintrag **Port** mit Ihrer benutzerdefinierten Portnummer.

        <source>
          type syslog
          port %SYSLOG_PORT%
          bind 127.0.0.1
          protocol_type udp
          tag oms.syslog
        </source>
        <filter oms.syslog.**>
          type filter_syslog
        </filter>

* Für „rsyslog“ müssen Sie eine neue Konfigurationsdatei im Verzeichnis `/etc/rsyslog.d/` erstellen. Ersetzen Sie dann den Wert „%SYSLOG_PORT“ mit Ihrer benutzerdefinierten Portnummer.  

    > [!NOTE]
    > Wenn Sie diesen Wert in der Konfigurationsdatei `95-omsagent.conf` ändern, wird er überschrieben, wenn der Agent eine Standardkonfiguration anwendet.
    >

        # OMS Syslog collection for workspace %WORKSPACE_ID%
        kern.warning              @127.0.0.1:%SYSLOG_PORT%
        user.warning              @127.0.0.1:%SYSLOG_PORT%
        daemon.warning            @127.0.0.1:%SYSLOG_PORT%
        auth.warning              @127.0.0.1:%SYSLOG_PORT%

* Ändern Sie die syslog-ng-Konfiguration durch Kopieren der unten gezeigten Beispielkonfiguration und Hinzufügen der benutzerdefinierten geänderten Einstellungen am Ende der Konfigurationsdatei „syslog-ng.conf“ im Verzeichnis `/etc/syslog-ng/`.  Verwenden Sie **nicht** die Standardbezeichnung **%WORKSPACE_ID%_oms** oder **%WORKSPACE_ID_OMS**, definieren Sie eine benutzerdefinierte Bezeichnung, um die Änderungen kenntlich zu machen.  

    > [!NOTE]
    > Wenn Sie die Standardwerte in der Konfigurationsdatei ändern, werden sie überschrieben, wenn der Agent eine Standardkonfiguration anwendet.
    >

        filter f_custom_filter { level(warning) and facility(auth; };
        destination d_custom_dest { udp("127.0.0.1" port(%SYSLOG_PORT%)); };
        log { source(s_src); filter(f_custom_filter); destination(d_custom_dest); };

Nach Abschluss der Änderungen müssen Syslog und der OMS-Agent-Dienst neu gestartet werden, um sicherzustellen, dass die Konfigurationsänderungen wirksam werden.   

## <a name="syslog-record-properties"></a>Eigenschaften der Syslog-Datensätze
Syslog-Datensätze sind vom Typ **Syslog** und besitzen die in der folgenden Tabelle aufgeführten Eigenschaften.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Computer |Computer, auf dem das Ereignis gesammelt wurde. |
| Facility |Definiert den Teil des Systems, der die Meldung generiert hat. |
| HostIP |IP-Adresse des Systems, das die Nachricht sendet. |
| HostName |Name des Systems, das die Nachricht sendet. |
| SeverityLevel |Schweregrad des Ereignisses. |
| SyslogMessage |Text der Nachricht. |
| ProcessID |ID des Prozesses, der die Meldung generiert hat. |
| EventTime |Datum und Uhrzeit der Ereignisgenerierung. |

## <a name="log-queries-with-syslog-records"></a>Protokollieren von Abfragen mit Syslog-Datensätzen
Die folgende Tabelle zeigt verschiedene Beispiele für Protokollabfragen, die Syslog-Protokolldatensätze abrufen.

| Abfrage | Beschreibung |
|:--- |:--- |
| Type=Syslog |Alle Syslog-Datensätze. |
| Type=Syslog SeverityLevel=error |Alle Syslog-Datensätze mit Fehlerschweregrad. |
| Type=Syslog &#124; measure count() by Computer |Anzahl der Syslog-Datensätze je Computer. |
| Type=Syslog &#124; measure count() by Facility |Anzahl der Syslog-Datensätze je Einrichtung. |

>[!NOTE]
> Falls für Ihren Arbeitsbereich ein Upgrade auf die [neue Log Analytics-Abfragesprache](log-analytics-log-search-upgrade.md) durchgeführt wurde, müssen die obigen Abfragen wie folgt geändert werden:

> | Abfrage | Beschreibung |
|:--- |:--- |
| syslog |Alle Syslog-Datensätze. |
| Syslog &#124; where SeverityLevel == "error" |Alle Syslog-Datensätze mit Fehlerschweregrad. |
| Syslog &#124; summarize AggregatedValue = count() by Computer |Anzahl der Syslog-Datensätze je Computer. |
| Syslog &#124; summarize AggregatedValue = count() by Facility |Anzahl der Syslog-Datensätze je Einrichtung. |

## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich über [Protokollsuchvorgänge](log-analytics-log-searches.md) zum Analysieren der aus Datenquellen und Lösungen gesammelten Daten.
* Verwenden Sie [benutzerdefinierte Felder](log-analytics-custom-fields.md) , um Daten aus Syslog-Datensätzen in einzelnen Feldern zu analysieren.
* [Konfigurieren Sie Linux-Agents](log-analytics-linux-agents.md) zum Sammeln anderer Datentypen.

