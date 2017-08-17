---
title: "Überwachen der Java-Web-App-Leistung unter Linux – Azure | Microsoft-Dokumentation"
description: "Erweiterte Überwachung der Anwendungsleistung Ihrer Java-Website mit dem Plug-In „CollectD“ für Application Insights."
services: application-insights
documentationcenter: java
author: harelbr
manager: carmonm
ms.assetid: 40c68f45-197a-4624-bf89-541eb7323002
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/24/2016
ms.author: bwren
ms.translationtype: Human Translation
ms.sourcegitcommit: 0c4554d6289fb0050998765485d965d1fbc6ab3e
ms.openlocfilehash: 5a39d4ec9bbf1c7672267c7e89c957ebc49f1f3a
ms.contentlocale: de-de
ms.lasthandoff: 04/13/2017

---
# <a name="collectd-linux-performance-metrics-in-application-insights"></a>CollectD: Linux-Leistungsmetriken in Application Insights


Um Leistungsmetriken für Linux-Systeme in [Application Insights](app-insights-overview.md) zu untersuchen, installieren Sie [CollectD](http://collectd.org/) zusammen mit dem entsprechenden Application Insights-Plug-In. Diese Open Source-Lösung sammelt verschiedene System- und Netzwerkstatistiken.

CollectD wird üblicherweise verwendet, wenn Sie Ihren [Java-Webdienst bereits mit Application Insights][java] instrumentiert haben. Die Lösung liefert weitere Daten, auf deren Grundlage Sie die Leistung Ihrer Anwendung verbessern oder Probleme diagnostizieren können. 

![Beispieldiagramme](./media/app-insights-java-collectd/sample.png)

## <a name="get-your-instrumentation-key"></a>Abrufen des Instrumentationsschlüssels
Öffnen Sie im [Microsoft Azure-Portal](https://portal.azure.com) die Ressource [Application Insights](app-insights-overview.md), in der die Daten angezeigt werden sollen. (Oder [erstellen Sie eine neue Ressource](app-insights-create-new-resource.md).)

Kopieren Sie den Instrumentationsschlüssel, der die Ressource identifiziert.

![Durchsuchen Sie alle Ressourcen, öffnen Sie Ihre Ressource, wählen Sie in der Dropdownliste „Essentials“ den Instrumentationsschlüssel aus, und kopieren Sie ihn.](./media/app-insights-java-collectd/02-props.png)

## <a name="install-collectd-and-the-plug-in"></a>Installieren von collectd und des Plug-Ins
Auf Linux-Servercomputern:

1. Installieren Sie [collectd](http://collectd.org/) , Version 5.4.0 oder höher.
2. Laden Sie das [collectd-Writer-Plug-In für Application Insights](https://aka.ms/aijavasdk)herunter. Beachten Sie die Versionsnummer.
3. Kopieren Sie die Plug-In-JAR in `/usr/share/collectd/java`.
4. Bearbeiten Sie `/etc/collectd/collectd.conf`:
   * Stellen Sie sicher, dass [das Java-Plug-In](https://collectd.org/wiki/index.php/Plugin:Java) aktiviert ist.
   * Aktualisieren Sie den JVMArg-Wert für "java.class.path", sodass die folgende JAR-Datei enthalten ist. Aktualisieren Sie die Versionsnummer, sodass sie mit der übereinstimmt, die Sie heruntergeladen haben:
   * `/usr/share/collectd/java/applicationinsights-collectd-1.0.5.jar`
   * Fügen Sie diesen Codeausschnitt mit dem Instrumentationsschlüssel der Ressource hinzu:

```XML

     LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"
     <Plugin ApplicationInsightsWriter>
        InstrumentationKey "Your key"
     </Plugin>
```

Er ist Teil der Beispielkonfigurationsdatei:

```XML

    ...
    # collectd plugins
    LoadPlugin cpu
    LoadPlugin disk
    LoadPlugin load
    ...

    # Enable Java Plugin
    LoadPlugin "java"

    # Configure Java Plugin
    <Plugin "java">
      JVMArg "-verbose:jni"
      JVMArg "-Djava.class.path=/usr/share/collectd/java/applicationinsights-collectd-1.0.5.jar:/usr/share/collectd/java/collectd-api.jar"

      # Enabling Application Insights plugin
      LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"

      # Configuring Application Insights plugin
      <Plugin ApplicationInsightsWriter>
        InstrumentationKey "12345678-1234-1234-1234-123456781234"
      </Plugin>

      # Other plugin configurations ...
      ...
    </Plugin>
    ...
```

Konfigurieren Sie andere [collectd-Plug-Ins](https://collectd.org/wiki/index.php/Table_of_Plugins), die verschiedenste Daten aus unterschiedlichen Quellen sammeln können.

Starten Sie collectd gemäß dem [Handbuch](https://collectd.org/wiki/index.php/First_steps)neu.

## <a name="view-the-data-in-application-insights"></a>Anzeigen der Daten in Application Insights
Öffnen Sie in der Application Insights-Ressource den [Metrik-Explorer, und fügen Sie Diagramme][metrics] hinzu, indem Sie die Metriken auswählen, die aus der benutzerdefinierten Kategorie angezeigt werden sollen.

![](./media/app-insights-java-collectd/result.png)

Standardmäßig werden die Metriken für alle Hostcomputer aggregiert, von denen die Metriken gesammelt wurden. Aktivieren Sie zum Anzeigen der Metriken pro Host auf dem Blatt mit Diagrammdetails "Gruppierung", und wählen Sie dann aus, dass nach "CollectD-Host" gruppiert werden soll.

## <a name="to-exclude-upload-of-specific-statistics"></a>So schließen Sie den Upload bestimmter Statistiken aus
Standardmäßig sendet das Application Insights-Plug-In alle Daten, die von allen aktivierten collectd-Lese-Plug-Ins gesammelt wurden. 

So schließen Sie Daten von bestimmten Plug-Ins oder Datenquellen aus:

* Bearbeiten Sie die Konfigurationsdatei. 
* Fügen Sie in `<Plugin ApplicationInsightsWriter>`folgende Direktivenzeilen hinzu:

| Direktive | Effekt |
| --- | --- |
| `Exclude disk` |Schließen Sie alle vom `disk` -Plug-In gesammelten Daten aus. |
| `Exclude disk:read,write` |Schließen Sie die Quellen mit den Namen `read` und `write` aus dem `disk`-Plug-In aus. |

Trennen Sie Direktiven mit einem Zeilenumbruch.

## <a name="problems"></a>Probleme?
*Daten werden im Portal nicht angezeigt.*

* Öffnen Sie [Search][diagnostic], um zu überprüfen, ob die Rohereignisse angekommen sind. Manchmal dauert es länger, bis sie im Metrik-Explorer angezeigt werden.
* Gegebenenfalls müssen Sie [Firewallausnahmen für ausgehende Daten festlegen](app-insights-ip-addresses.md)
* Aktivieren Sie die Ablaufverfolgung im Application Insights-Plug-In. Fügen Sie diese Zeile in `<Plugin ApplicationInsightsWriter>`hinzu:
  * `SDKLogger true`
* Öffnen Sie ein Terminal, und starten Sie collectd im ausführlichen Modus, um alle gemeldeten Probleme anzuzeigen:
  * `sudo collectd -f`

## <a name="known-issue"></a>Bekannte Probleme

Das Write-Plug-In von Application Insights ist mit bestimmten Read-Plug-Ins nicht kompatibel. Manche Plug-Ins senden zuweilen „NaN“, wenn das Application Insights-Plug-In eine Gleitkommazahl erwartet.

Symptom: Im collectd-Protokoll werden Fehler angezeigt, die den Text „AI:... Syntaxfehler: Unerwartetes Token N“ enthalten.

Problemumgehung: Schließen Sie Daten aus, die von den problematischen Write-Plug-Ins gesammelt werden. 

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md



