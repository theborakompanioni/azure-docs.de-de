<properties 
   pageTitle="Datenquellen in Log Analytics | Microsoft Azure"
   description="Datenquellen definieren die Daten, die Log Analytics aus Agents und anderen verbundenen Quellen sammelt. Dieser Artikel beschreibt das Konzept, nach dem Log Analytics Datenquellen verwendet, erläutert Details zur Konfiguration der Quellen und bietet eine Übersicht über die verschiedenen verfügbaren Datenquellen."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/02/2016"
   ms.author="bwren" />

# Datenquellen in Log Analytics

Log Analytics sammelt Daten aus den verbundenen Datenquellen in Ihrem OMS-Arbeitsbereich und speichert sie im OMS-Repository. Welche Daten gesammelt werden, wird durch die von Ihnen konfigurierten Datenquellen definiert. Daten im OMS-Repository werden als Gruppe von Datensätzen gespeichert. Jede Datenquelle erstellt Datensätze eines bestimmten Typs, von denen jeder über einen eigenen Satz von Eigenschaften verfügt.

![Log Analytics-Datensammlung](./media/log-analytics-data-sources/overview.png)

Datenquellen unterscheiden sich von OMS-Lösungen, die ebenfalls Daten aus verbundenen Quellen sammeln und Datensätze im OMS-Repository erstellen. Lösungen können Ihrem Arbeitsbereich über den Lösungskatalog hinzugefügt werden und stellen im Allgemeinen zusätzliche Analysetools im OMS-Portal bereit.

## Übersicht über Datenquellen

In der folgenden Tabelle werden die zurzeit in Log Analytics verfügbaren Datenquellen aufgeführt. In den Links zu den Datenquellen finden Sie weitere Informationen zur jeweiligen Datenquelle.

| Datenquelle | Ereignistyp | Beschreibung |
|:--|:--|:--|
| [Benutzerdefinierte Protokolle](log-analytics-data-sources-custom-logs.md) | \<LogName\>_CL | Textdateien in Windows- oder Linux-Agents mit Protokollinformationen. |
| [Windows-Ereignisprotokolle](log-analytics-data-sources-windows-events.md) | Event | Aus dem Ereignisprotokoll auf Windows-Computern gesammelte Ereignisse. |
| [Windows-Leistungsindikatoren](log-analytics-data-sources-performance-counters.md) | Perf | Von Windows-Computern gesammelte Leistungsindikatoren. |
| [Linux-Leistungsindikatoren](log-analytics-data-sources-performance-counters.md) | Perf | Von Linux-Computern gesammelte Leistungsindikatoren. |
| [IIS-Protokolle](log-analytics-data-sources-iis-logs.md) | W3CIISLog | IIS-Protokolle (Internetinformationsdienste) im W3C-Format. |
| Syslog | Syslog | Syslog-Ereignisse auf Windows- oder Linux-Computern. |

## Konfigurieren von Datenquellen

Sie können Datenquellen über das Menü **Daten** in den **Einstellungen** von Log Analytics konfigurieren. Jede Konfiguration wird an alle verbundenen Quellen in Ihrem OMS-Arbeitsbereich übermittelt. Sie können zurzeit keine Agents aus dieser Konfiguration ausschließen.

![Windows-Ereignisse konfigurieren](./media/log-analytics-data-sources/configure-events.png)

2. Wählen Sie in der OMS-Konsole die Kachel **Einstellungen** aus.
3. Wählen Sie **Daten** aus.
4. Klicken Sie auf die Datenquelle, die Sie konfigurieren möchten.
5. Folgen Sie den Links in der oben stehenden Tabelle, um zur Dokumentation für jede Datenquelle zu gelangen und detaillierte Informationen zur jeweiligen Konfiguration zu erhalten.

## Datensammlung

Die Konfigurationen der Datenquellen werden innerhalb weniger Minuten an Agents übermittelt, die direkt mit OMS verbunden sind. Die angegebenen Daten werden vom Agent gesammelt und in den für jede Datenquelle spezifischen Intervallen direkt an Log Analytics übermittelt. Informationen zu diesen Spezifikationen finden Sie in der Dokumentation zu jeder Datenquelle.

Bei SCOM-Agents (System Center Operations Manager) in einer verbundenen Verwaltungsgruppe werden Datenquellenkonfigurationen in Management Packs übersetzt und standardmäßig alle fünf Minuten an die Verwaltungsgruppe übermittelt. Der Agent lädt das Management Pack wie jedes andere Management Pack herunter, sammelt die angegebenen Daten und sendet sie an einen Verwaltungsserver, der sie wiederum an OMS weiterleitet. Der Agent benötigt für keine der Datenquellen eine direkte Kommunikation mit OMS. Informationen zum Verbinden von SCOM und OMS und zum Ändern der Häufigkeit, mit der die Konfiguration übermittelt wird, finden Sie unter [Configure Integration with System Center Operations Manager](log-analytics-om-agents.md) (Konfigurieren der Integration mit System Center Manager).

## Log Analytics-Datensätze

Alle von Log Analytics gesammelten Daten werden im OMS-Repository als Datensätze gespeichert. Datensätze, die aus verschiedenen Datenquellen gesammelt wurde, verfügen über einen eigenen Eigenschaftensatz und werden über die Eigenschaft **Typ** identifiziert. Weitere Informationen zu den Datensatztypen finden Sie in der Dokumentation zur jeweiligen Datenquelle und Lösung.


## Nächste Schritte

- Erfahren Sie mehr über [Lösungen](log-analytics-add-solutions.md), die Log Analytics um zusätzliche Funktionen erweitern und ebenfalls Daten für das OMS-Repository sammeln.
- Erfahren Sie mehr über [Protokollsuchvorgänge](log-analytics-log-searches.md) zum Analysieren der aus Datenquellen und Lösungen gesammelten Daten.  
- Konfigurieren Sie Warnungen, damit Sie bei kritischen Daten, die aus Datenquellen und Lösungen gesammelt werden, proaktiv benachrichtigt werden.

<!---HONumber=AcomDC_0504_2016-->
