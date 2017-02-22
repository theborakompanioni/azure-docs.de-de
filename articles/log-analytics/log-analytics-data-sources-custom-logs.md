---
title: Erfassen benutzerdefinierter Protokolle in OMS Log Analytics | Microsoft-Dokumentation
description: "Log Analytics kann Ereignisse aus Textdateien auf Windows- und Linux-Computern sammeln.  Dieser Artikel enthält Informationen zum Definieren eines neuen benutzerdefinierten Protokolls sowie Details zu den Datensätzen, die im OMS-Repository erstellt werden."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: aca7f6bb-6f53-4fd4-a45c-93f12ead4ae1
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: bwren
translationtype: Human Translation
ms.sourcegitcommit: 653696779e612726ed5b75829a5c6ed2615553d7
ms.openlocfilehash: a9c70810c4f731b2d8b395873fa6b94db78306aa


---
# <a name="custom-logs-in-log-analytics"></a>Benutzerdefinierte Protokolle in Log Analytics
Mithilfe der Datenquelle „Custom Logs“ (Benutzerdefinierte Protokolle) in Log Analytics können Ereignisse aus Textdateien auf Windows- und Linux-Computern gesammelt werden. Viele Anwendungen protokollieren Informationen nicht in standardmäßigen Protokollierungsdiensten wie Windows-Ereignisprotokoll oder Syslog, sondern in Textdateien.  Mithilfe des Log Analytics-Features [Benutzerdefinierte Felder](log-analytics-custom-fields.md) können gesammelte Datensätze im Protokoll individuell analysiert und einzelnen Feldern zugeordnet werden.

![Benutzerdefinierte Protokollsammlung](media/log-analytics-data-sources-custom-logs/overview.png)

Die zu sammelnden Protokolldateien müssen folgende Kriterien erfüllen:

* Das Protokoll muss entweder pro Zeile einen einzelnen Eintrag enthalten, oder die Einträge müssen jeweils mit einem Zeitstempel in einem der folgenden Formate beginnen:
  
    JJJJ-MM-TT HH:MM:SS  <br>
    M/T/JJJJ HH:MM:SS AM/PM <br>
    Mon TT,JJJJ HH:MM:SS
* Die Protokolldatei darf keine zirkulären Updates zulassen, bei denen die Datei mit neuen Einträgen überschrieben wird. 

## <a name="defining-a-custom-log"></a>Definieren eines benutzerdefinierten Protokolls
Gehen Sie zum Definieren einer benutzerdefinierten Protokolldatei wie folgt vor.  Am Ende dieses Artikels finden Sie eine exemplarische Vorgehensweise für das Hinzufügen eines benutzerdefinierten Protokolls.

### <a name="step-1-open-the-custom-log-wizard"></a>Schritt 1: Öffnen des Assistenten für benutzerdefinierte Protokolle
Mit dem im OMS-Portal ausgeführten Assistenten für benutzerdefinierte Protokolle können Sie ein neues benutzerdefiniertes Protokoll für die Sammlung definieren.

1. Navigieren Sie im OMS-Portal zu **Einstellungen**.
2. Klicken Sie auf **Daten** und anschließend auf **Benutzerdefinierte Protokolle**.
3. Standardmäßig werden alle Konfigurationsänderungen automatisch per Push an alle Agents weitergegeben.  Bei Linux-Agents wird eine Konfigurationsdatei an den Fluentd-Datensammler gesendet.  Wenn Sie diese Datei manuell auf jedem Linux-Agent ändern möchten, deaktivieren Sie das Kontrollkästchen *Nachstehende Konfiguration auf meine Linux-Computer anwenden*.
4. Klicken Sie auf **Hinzufügen+** , um den Assistenten für benutzerdefinierte Protokolle zu öffnen.

### <a name="step-2-upload-and-parse-a-sample-log"></a>Schritt 2. Hochladen und Analysieren eines Beispielprotokolls
Als Erstes muss ein benutzerdefiniertes Beispielprotokoll hochgeladen werden.  Der Assistent überprüft die Einträge in dieser Datei und zeigt sie zur Überprüfung an.  Log Analytics verwendet bei der Identifizierung der einzelnen Datensätze das von Ihnen angegebene Trennzeichen.

Das Standardtrennzeichen **Neue Zeile** wird für Protokolldateien verwendet, die pro Zeile jeweils einen einzelnen Eintrag enthalten.  Falls die Zeile mit einer Datums- und Uhrzeitangabe in einem kompatiblen Format beginnt, können Sie als Trennzeichen die Option **Zeitstempel** angeben. Diese Option unterstützt Einträge, die sich über mehrere Zeilen erstrecken. 

Bei Verwendung der Zeitstempeloption wird die TimeGenerated-Eigenschaft der einzelnen, in OMS gespeicherten Datensätze in der Protokolldatei mit der Datums-/Uhrzeitangabe des jeweiligen Eintrags aufgefüllt.  Bei Verwendung eines Trennzeichens vom Typ „Neue Zeile“ wird die TimeGenerated-Eigenschaft mit dem Zeitpunkt (Datum und Uhrzeit) aufgefüllt, an dem Log Analytics den Eintrag gesammelt hat. 

> [!NOTE]
> Log Analytics behandelt Datums-/Uhrzeitangaben, die in einem Protokoll mithilfe des Zeitstempeltrennzeichens gesammelt werden, momentan als UTC-Werte.  Dies wird jedoch bald geändert, sodass die Zeitzone für den Agent verwendet wird. 
> 
> 

1. Klicken Sie auf **Durchsuchen**, und navigieren Sie zu einer Beispieldatei.  Hinweis: Bei manchen Browsern ist diese Schaltfläche unter Umständen mit **Datei auswählen** beschriftet.
2. Klicken Sie auf **Weiter**. 
3. Der Assistent für benutzerdefinierte Protokolle lädt die Datei hoch und führt die ermittelten Datensätze auf.
4. Ändern Sie das Trennzeichen, das zur Identifizierung eines neuen Datensatzes verwendet wird, und wählen Sie das Trennzeichen aus, das für die Einträge in Ihrer Protokolldatei am besten geeignet ist.
5. Klicken Sie auf **Weiter**.

### <a name="step-3-add-log-collection-paths"></a>Schritt 3. Hinzufügen von Protokollsammlungspfaden
Definieren Sie für den Agent mindestens einen Pfad, an dem sich das benutzerdefinierte Protokoll befindet.  Sie können entweder einen bestimmten Pfad und Namen für die Protokolldatei angeben oder einen Pfad mit einem Platzhalter für den Namen verwenden.  Dadurch werden Anwendungen unterstützt, die täglich oder bei Erreichen einer bestimmten Dateigröße eine neue Datei erstellen.  Sie können auch mehrere Pfade für eine einzelne Protokolldatei angeben.

Ein Beispiel: Angenommen, eine Anwendung erstellt jeden Tag eine Protokolldatei, und das Datum ist jeweils Teil des Dateinamens (etwa „log20100316.txt“). In einem solchen Fall kann beispielsweise ein Muster wie *log\*.txt* verwendet werden, um sämtliche Protokolldateien abzudecken, die nach dem Benennungsschema der Anwendung erstellt werden.

Die folgende Tabelle enthält Musterbeispiele für die Angabe verschiedener Protokolldateien: 

| Beschreibung | Pfad |
|:--- |:--- |
| Alle Dateien auf dem Windows-Agent im Verzeichnis *C:\Logs* mit der Erweiterung „.txt“ |C:\Logs\\\*.txt |
| Alle Dateien auf dem Windows-Agent im Verzeichnis *C:\Logs* mit einem Namen, der mit „log“ beginnt und die Erweiterung „.txt“ besitzt |C:\Logs\log\*.txt |
| Alle Dateien auf dem Linux-Agent im Verzeichnis */var/log/audit* mit der Erweiterung „.txt“ |/var/log/audit/*.txt |
| Alle Dateien auf dem Linux-Agent im Verzeichnis */var/log/audit* mit einem Namen, der mit „log“ beginnt und die Erweiterung „.txt“ besitzt |/var/log/audit/log\*.txt |

1. Wählen Sie „Windows“ oder „Linux“ aus, um anzugeben, welches Pfadformat Sie hinzufügen.
2. Geben Sie den Pfad ein, und klicken Sie auf die Schaltfläche **+** .
3. Wiederholen Sie den Vorgang für jeden zusätzlichen Pfad.

### <a name="step-4-provide-a-name-and-description-for-the-log"></a>Schritt 4: Angeben eines Namens und einer Beschreibung für das Protokoll
Der angegebene Name wird für den oben beschriebenen Protokolltyp verwendet.  Er endet immer mit „_CL“, um ihn als benutzerdefiniertes Protokoll zu kennzeichnen.

1. Geben Sie einen Namen für das Protokoll ein.  Das Suffix **\_CL** wird automatisch angehängt.
2. Geben Sie ggf. eine **Beschreibung** ein.
3. Klicken Sie auf **Weiter** , um die Definition des benutzerdefinierten Protokolls zu speichern.

### <a name="step-5-validate-that-the-custom-logs-are-being-collected"></a>Schritt 5: Überprüfen, ob die benutzerdefinierten Protokolle gesammelt werden
Es kann bis zu einer Stunde dauern, bis die ersten Daten aus einem neuen benutzerdefinierten Protokoll in Log Analytics erscheinen.  Die Sammlung von Einträgen aus Protokollen, die sich am angegebenen Pfad befinden, beginnt, nachdem Sie das benutzerdefinierte Protokoll definiert haben.  Die bei der Erstellung des benutzerdefinierten Protokolls hochgeladenen Einträge werden zwar nicht gespeichert, es werden aber bereits vorhandene Einträge aus den gefundenen Protokolldateien gesammelt.

Sobald Log Analytics mit dem Sammeln von Einträgen aus dem benutzerdefinierten Protokoll begonnen hat, stehen die entsprechenden Datensätze bei einer Protokollsuche zur Verfügung.  Geben Sie in Ihrer Abfrage den Namen, mit dem Sie das benutzerdefinierte Protokoll benannt haben, als **Typ** an.

> [!NOTE]
> Sollte die RawData-Eigenschaft bei der Suche nicht vorhanden sein, müssen Sie unter Umständen Ihren Browser schließen und wieder öffnen.
> 
> 

### <a name="step-6-parse-the-custom-log-entries"></a>Schritt 6: Analysieren der Einträge des benutzerdefinierten Protokolls
Der gesamte Protokolleintrag wird in einer einzelnen Eigenschaft namens **RawData**gespeichert.  Sie möchten vermutlich die verschiedenen Einzelinformationen der jeweiligen Einträge als einzelne Eigenschaften im Datensatz speichern.  Hierzu wird das Log Analytics-Feature [Benutzerdefinierte Felder](log-analytics-custom-fields.md) verwendet.

An dieser Stelle wird keine ausführliche Anleitung zum Analysieren des benutzerdefinierten Protokolleintrags bereitgestellt.  Diese finden Sie in der Dokumentation zu [Benutzerdefinierte Felder](log-analytics-custom-fields.md) .

## <a name="disabling-a-custom-log"></a>Deaktivieren eines benutzerdefinierten Protokolls
Sie können die Definition eines benutzerdefinierten Protokolls nicht mehr entfernen, nachdem dieses erstellt wurde, aber Sie können es deaktivieren, indem Sie alle Sammlungspfade entfernen.

1. Navigieren Sie im OMS-Portal zu **Einstellungen**.
2. Klicken Sie auf **Daten** und anschließend auf **Benutzerdefinierte Protokolle**.
3. Klicken Sie auf **Details** neben der Definition des zu deaktivierenden benutzerdefinierten Protokolls.
4. Entfernen Sie alle Sammlungspfade für die Definition des benutzerdefinierten Protokolls.

## <a name="data-collection"></a>Datensammlung
Log Analytics sammelt etwa alle fünf Minuten neue Einträge aus den einzelnen benutzerdefinierten Protokollen.  Der Agent protokolliert für jede Protokolldatei, aus der er Daten sammelt, seine Position.  Wenn der Agent für einen bestimmten Zeitraum offline geht, sammelt Log Analytics Einträge ab dem Zeitpunkt der letzten Sammlung, unabhängig davon, ob die Einträge erstellt wurden, während der Agent offline war.

Sämtliche Inhalte des Protokolleintrags werden in eine einzelne Eigenschaft namens **RawData** geschrieben.  Diese kann in mehrere Eigenschaften aufgeschlüsselt werden, die separat analysiert und durchsucht werden können. Zu diesem Zweck müssen nach der Erstellung des benutzerdefinierten Protokolls [benutzerdefinierte Felder](log-analytics-custom-fields.md) definiert werden.

## <a name="custom-log-record-properties"></a>Eigenschaften benutzerdefinierter Protokolldatensätze
Benutzerdefinierte Protokolldatensätze besitzen einen Typ mit dem von Ihnen angegebenen Protokollnamen sowie die Eigenschaften aus der folgenden Tabelle:

| Eigenschaft | Beschreibung |
|:--- |:--- |
| TimeGenerated |Der Zeitpunkt (Datum und Uhrzeit), zu dem der Datensatz von Log Analytics gesammelt wurde.  Falls das Protokoll ein zeitbasiertes Trennzeichen verwendet, handelt es sich hierbei um die Zeitangabe aus dem Eintrag. |
| SourceSystem |Die Art des Agents, auf dem das Ereignis gesammelt wurde. <br> OpsManager: Windows-Agent (Direktverbindung oder SCOM) <br> Linux: Alle Linux-Agents |
| RawData |Der vollständige Text des gesammelten Eintrags. |
| ManagementGroupName |Name der Verwaltungsgruppe für SCOM-Agents.  Bei anderen Agents lautet dieser „AOI-\<Arbeitsbereich-ID\>“. |

## <a name="log-searches-with-custom-log-records"></a>Protokollsuchvorgänge mit benutzerdefinierten Protokolleinträgen
Datensätze aus benutzerdefinierten Protokollen werden genau wie Datensätze aus anderen Datenquellen im OMS-Repository gespeichert.  Sie besitzen einen Typ mit dem Namen, den Sie beim Definieren des Protokolls angegeben haben. Dadurch können Sie bei der Suche die Type-Eigenschaft verwenden, um Datensätze abzurufen, die in einem bestimmten Protokoll gesammelt wurden.

Die folgende Tabelle zeigt verschiedene Beispiele für Protokollsuchvorgänge, die Datensätze aus benutzerdefinierten Protokollen abrufen:

| Abfrage | Beschreibung |
|:--- |:--- |
| Type=MyApp_CL |Alle Ereignisse aus einem benutzerdefinierten Protokoll namens „MyApp_CL“. |
| Type=MyApp_CL Severity_CF=error |Alle Ereignisse aus einem benutzerdefinierten Protokoll namens „MyApp_CL“ mit dem Wert *error* in einem benutzerdefinierten Feld namens *Severity_CF*. |

## <a name="sample-walkthrough-of-adding-a-custom-log"></a>Exemplarische Vorgehensweise zum Hinzufügen eines benutzerdefinierten Protokolls
Der folgende Abschnitt enthält ein Beispiel für die Erstellung eines benutzerdefinierten Protokolls.  Das Beispielprotokoll enthält in jeder Zeile einen einzelnen Eintrag, der jeweils mit einer Datums- und Uhrzeitangabe beginnt, gefolgt von kommagetrennten Feldern für Code, Status und Meldung.  Hier einige Beispieleinträge:

    2016-03-10 01:34:36 207,Success,Client 05a26a97-272a-4bc9-8f64-269d154b0e39 connected
    2016-03-10 01:33:33 208,Warning,Client ec53d95c-1c88-41ae-8174-92104212de5d disconnected
    2016-03-10 01:35:44 209,Success,Transaction 10d65890-b003-48f8-9cfc-9c74b51189c8 succeeded
    2016-03-10 01:38:22 302,Error,Application could not connect to database
    2016-03-10 01:31:34 303,Error,Application lost connection to database

### <a name="upload-and-parse-a-sample-log"></a>Hochladen und Analysieren eines Beispielprotokolls
Wir stellen eine der Protokolldateien bereit und sehen die Ereignisse, die gesammelt werden.  In diesem Fall kann als Trennzeichen die Option „Neue Zeile“ verwendet werden.  Wenn sich allerdings im Protokoll ein einzelner Eintrag über mehrere Zeilen erstrecken kann, muss ein Trennzeichen vom Typ „Zeitstempel“ verwendet werden.

![Hochladen und Analysieren eines Beispielprotokolls](media/log-analytics-data-sources-custom-logs/delimiter.png)

### <a name="add-log-collection-paths"></a>Hinzufügen von Protokollsammlungspfaden
Die Protokolldateien befinden sich im Verzeichnis *C:\MyApp\Logs*.  Jeden Tag wird eine neue Datei erstellt, deren Name jeweils das Datum enthält. Dabei wird in das Muster *appJJJJMMTT.log* verwendet.  Eine geeignetes Muster für dieses Protokoll wäre *C:\MyApp\Logs\\\*.log*.

![Protokollsammlungspfad](media/log-analytics-data-sources-custom-logs/collection-path.png)

### <a name="provide-a-name-and-description-for-the-log"></a>Angeben eines Namens und einer Beschreibung für das Protokoll
Wir verwenden den Namen *MyApp_CL* und geben eine **Beschreibung** ein.

![Protokollname](media/log-analytics-data-sources-custom-logs/log-name.png)

### <a name="validate-that-the-custom-logs-are-being-collected"></a>Überprüfen, ob die benutzerdefinierten Protokolle gesammelt werden
Wir verwenden die Abfrage *Type=MyApp_CL*, um alle Datensätze aus dem gesammelten Protokoll zurückzugeben.

![Protokollabfrage ohne benutzerdefinierte Felder](media/log-analytics-data-sources-custom-logs/query-01.png)

### <a name="parse-the-custom-log-entries"></a>Analysieren der Einträge des benutzerdefinierten Protokolls
Wir definieren mithilfe benutzerdefinierter Felder die Felder *EventTime*, *Code*, *Status* und *Message* und sehen die Unterschiede bei den Datensätzen, die durch die Abfrage zurückgegeben werden.

![Protokollabfrage mit benutzerdefinierten Feldern](media/log-analytics-data-sources-custom-logs/query-02.png)

## <a name="next-steps"></a>Nächste Schritte
* Verwenden Sie [benutzerdefinierte Felder](log-analytics-custom-fields.md) , um die Einträge des benutzerdefinierten Protokolls in einzelne Felder aufzuschlüsseln.
* Informieren Sie sich über [Protokollsuchvorgänge](log-analytics-log-searches.md) zum Analysieren der aus Datenquellen und Lösungen gesammelten Daten. 




<!--HONumber=Jan17_HO4-->


