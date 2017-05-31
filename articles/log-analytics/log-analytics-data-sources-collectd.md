---
title: Sammeln von Daten aus CollectD in OMS Log Analytics | Microsoft-Dokumentation
description: "CollectD ist ein Open-Source-Linux-Deamon, der regelmäßig Daten aus Anwendungen und Informationen aus der Betriebssystemebene sammelt.  In diesem Artikel wird beschrieben, wie Daten aus CollectD in Log Analytics gesammelt werden."
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
ms.date: 05/02/2017
ms.author: magoedte
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: a63b15ca5126b45451f0694c9ee75d7b67b1ceaf
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017


---
# <a name="collect-data-from-collectd-on-linux-agents-in-log-analytics"></a>Sammeln von Daten mit Linux-Agenten aus CollectD in Log Analytics
[CollectD](https://collectd.org/) ist ein Open-Source-Linux-Deamon, der regelmäßig Leistungsmetriken aus Anwendungen und Informationen aus der Betriebssystemebene sammelt. Beispiele für diese Anwendungen sind Java Virtual Machine (JVM), MySQL Server und Nginx. In diesem Artikel wird beschrieben, wie Leistungsdaten aus CollectD in Log Analytics gesammelt werden.

Eine vollständige Liste der verfügbaren Plug-Ins finden Sie unter [Plug-In-Tabelle](https://collectd.org/wiki/index.php/Table_of_Plugins).

![CollectD (Übersicht)](media/log-analytics-data-sources-collectd/overview.png)

Die folgende CollectD-Konfiguration ist im OMS-Agenten für Linux enthalten und dient dazu, CollectD-Daten am den OMS-Agenten für Linux zu übermitteln.

    LoadPlugin write_http

    <Plugin write_http>
         <Node "oms">
         URL "127.0.0.1:26000/oms.collectd"
         Format "JSON"
         StoreRates true
         </Node>
    </Plugin>

Sollten Sie eine ältere collectD-Version als 5.5 verwenden, müssen Sie stattdessen die folgende Konfiguration verwenden.

    LoadPlugin write_http

    <Plugin write_http>
       <URL "127.0.0.1:26000/oms.collectd">
        Format "JSON"
         StoreRates true
       </URL>
    </Plugin>

Die CollectD-Konfiguration verwendet das standardmäßige `write_http`-Plug-In, um Leistungsmetrikdaten über Port 26000 an den OMS-Agenten für Linux zu senden. 

> [!NOTE]
> Falls notwendig kann dieser Port individuell konfiguriert werden.

Der OMS-Agent für Linux agiert an Port 26000 auch als Listener für collectC-Metriken und wandelt diese in OMS-Schemametriken um. Im Folgenden wird der OMS-Agent für die Linux-Konfiguration beschrieben`collectd.conf`.

    <source>
      type http
      port 26000
      bind 127.0.0.1
    </source>

    <filter oms.collectd>
      type filter_collectd
    </filter>


## <a name="versions-supported"></a>Unterstützte Versionen
- Log Analytics unterstützt derzeit CollectD Version 4.8 und höher.
- Für die CollectD-Metriksammlung ist der OMS-Agent für Linux v1.1.0-217 oder höher erforderlich.


## <a name="configuration"></a>Konfiguration
Im Folgenden werden die grundlegenden Schritte für die Konfiguration des Sammelns von CollectD-Daten in Log Analytics beschrieben.

1. Konfigurieren Sie CollectD mithilfe des Plug-Ins write_http zum Senden von Daten an den OMS-Agenten für Linux.  
2. Konfigurieren Sie den OMS-Agenten für Linux als Listener, der am entsprechenden Port CollectD-Daten überwacht.
3. Starten Sie CollectD und den OMS-Agenten für Linux neu.

### <a name="configure-collectd-to-forward-data"></a>Konfigurieren Sie CollectD zum Weiterleiten von Daten 

1. Um Daten aus CollectD an den OMS-Agenten für Linux übermitteln zu können, muss `oms.conf` zum Konfigurationsverzeichnis von CollectD hinzugefügt werden. Das Ziel dieser Datei richtet sich nach dem Linux-Distributionsverzeichnis auf Ihrem Computer.

    Ihr CollectD-Konfigurationsverzeichnis befindet sich in /etc/collectd.d/:

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd.d/oms.conf

    Ihr CollectD-Konfigurationsverzeichnis befindet sich in /etc/collectd/collectd.conf.d/:

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd/collectd.conf.d/oms.conf

    >[!NOTE]
    >Für CollectD-Versionen vor 5.5 müssen Sie die Tags in `oms.conf` wie oben beschrieben ändern.
    >

2. Kopieren Sie collectd.conf in das gewünschte omsagent-Konfigurationsverzeichnis des Arbeitsbereichs.

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/collectd.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/
        sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/collectd.conf

3. Starten Sie CollectD und den OMS-Agenten für Linux mit den folgenden Befehlen neu.

    sudo service collectd restart  sudo /opt/microsoft/omsagent/bin/service_control restart

## <a name="collectd-metrics-to-log-analytics-schema-conversion"></a>Sammeln von CollectD-Metriken für die Log Analytics-Schemakonvertierung
Um ein vertrautes Modell zwischen den bereits vom OMS-Agenten für Linux gesammelten Infrastrukturmetriken und den neuen, von CollectD gesammelten Metriken zu erhalten, wird die folgende Schemazuordnung verwendet:

| CollectD-Metrikfeld | Log Analytics-Feld |
|:--|:--|
| host | Computer |
| Plug-In | Keine |
| plugin_instance | Instanzname<br>Wenn **Plugin_instance**=*null,* gilt InstanceName = "*_Total*" |
| type | ObjectName |
| type_instance | CounterName<br>Wenn **type_instance**=*null,* gilt CounterName=**blank** |
| dsnames[] | CounterName |
| dstypes | Keine |
| values[] | CounterValue |

## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich über [Protokollsuchvorgänge](log-analytics-log-searches.md) zum Analysieren der aus Datenquellen und Lösungen gesammelten Daten. 
* Verwenden Sie [benutzerdefinierte Felder](log-analytics-custom-fields.md) , um Daten aus Syslog-Datensätzen in einzelnen Feldern zu analysieren.


