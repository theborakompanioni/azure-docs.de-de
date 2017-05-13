---
title: Leitfaden zur Behandlung von Problemen bei Azure Stream Analytics | Microsoft-Dokumentation
description: "Informationen zur Behandlung von Problemen in Bezug auf Stream Analytics-Aufträge"
keywords: Leitfaden zur Problembehandlung
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeffstok
ms.translationtype: Human Translation
ms.sourcegitcommit: 7f8b63c22a3f5a6916264acd22a80649ac7cd12f
ms.openlocfilehash: 3fbaa005e21ffbbd1a71febc718d36c9231cbd26
ms.contentlocale: de-de
ms.lasthandoff: 05/01/2017


---

# <a name="troubleshooting-guide-for-azure-stream-analytics"></a>Leitfaden zur Behandlung von Problemen bei Azure Stream Analytics

Die Azure Stream Analytics-Problembehandlung kann auf den ersten Blick komplex wirken. Infolge der Zusammenarbeit mit zahlreichen Benutzern haben wir dieses Handbuch erstellt, das dabei unterstützen soll, den Prozess zu optimieren und Fragen zu Ein- und Ausgaben sowie Abfragen und Funktionen zu beseitigen.

## <a name="troubleshoot-your-stream-analytics-job"></a>Behandlung von Problemen in Bezug auf Stream Analytics-Aufträge

Um optimale Ergebnisse bei der Behandlung von Problemen in Bezug auf Stream Analytics-Aufträge zu erzielen, verwenden Sie folgende Richtlinien:

1.  Testen Sie Ihre Verbindung:
    - Überprüfen Sie die Verbindung mit Ein- und Ausgaben mithilfe der Schaltfläche **Verbindung testen** für die einzelnen Ein- und Ausgaben.

2.  Überprüfen Sie die Eingabedaten:
    - Stellen Sie mit [Service Bus-Explorer](https://code.msdn.microsoft.com/windowsapps/Service-Bus-Explorer-f2abca5a) eine Verbindung mit Azure Event Hub her (bei Verwendung der Event Hub-Eingabe), um zu überprüfen, ob Eingabedaten an Event Hub übertragen werden.  
    - Verwenden Sie die Schaltfläche [**Beispieldaten**](stream-analytics-sample-data-input.md) für jede Eingabe, und laden Sie die Eingabebeispieldaten herunter.
    - Überprüfen Sie die Beispieldaten, um die Form der Daten zu verstehen: das Schema und die [Datentypen](https://msdn.microsoft.com/library/azure/dn835065.aspx).

3.  Testen Sie Ihre Abfrage:
    - Verwenden Sie auf der Registerkarte **Abfrage** die Schaltfläche **Testen**, um die Abfrage zu testen. [Testen Sie die Abfrage](stream-analytics-test-query.md) außerdem mithilfe der heruntergeladenen Beispieldaten. Untersuchen Sie alle Fehler, und versuchen Sie, diese zu korrigieren.
    - Stellen Sie bei Verwendung von [**Zeitstempel nach**](https://msdn.microsoft.com/library/azure/mt573293.aspx) sicher, dass die Zeitstempel der Ereignisse aktueller sind als die [Startzeit des Auftrags](stream-analytics-out-of-order-and-late-events.md).

4.  Debuggen Sie Ihre Abfrage:
    - Erstellen Sie die Abfrage Schritt für Schritt neu. Beginnen Sie dabei mit einer einfachen Select-Anweisung, und arbeiten Sie sich unter Verwendung einzelner Schritte nach und nach zu komplexeren Aggregaten vor. Um die Abfragelogik Schritt für Schritt zu erstellen, verwenden Sie [WITH](https://msdn.microsoft.com/library/azure/dn835049.aspx)-Klauseln.
    - Verwenden Sie zum Debuggen der Abfrage [SELECT INTO](stream-analytics-select-into.md).

5.  Beseitigen Sie häufige Fallen wie Folgende:
    - Eine [**WHERE**](https://msdn.microsoft.com/library/azure/dn835048.aspx)-Klausel in der Abfrage hat alle Ereignisse herausgefiltert, die das Generieren von Ausgaben verhindert.
    - Warten Sie bei der Verwendung von Fensterfunktionen die gesamte Fensterdauer ab, um die Ausgabe der Abfrage zu sehen.
    - Der Zeitstempel für Ereignisse liegt vor der Startzeit des Auftrags, und daher werden Ereignisse verworfen.

6.  Verwenden Sie die Ereignisreihenfolge:
    - Wenn alle vorherigen Schritte problemlos ausgeführt wurden, navigieren Sie zum Blatt **Einstellungen**, und wählen Sie [**Ereignisreihenfolge**](stream-analytics-out-of-order-and-late-events.md) aus. Stellen Sie sicher, dass diese Richtlinie entsprechend Ihrem Szenario konfiguriert ist. Die Richtlinie wird *nicht* angewendet, wenn Sie die Abfrage mithilfe der Schaltfläche **Testen** testen. Dieses Ergebnis ist ein Unterschied zwischen dem Testen im Browser und der tatsächlichen Ausführung des Auftrags.

7.  Führen Sie ein Debuggen mithilfe von Metriken durch:
    - Erfolgt nach der erwarteten Dauer (basierend auf der Abfrage) keine Ausgabe, probieren Sie Folgendes:
        - Sehen Sie sich die [**Überwachungsmetriken**](stream-analytics-monitoring.md) auf der Registerkarte **Überwachen** an. Da die Werte aggregiert werden, werden die Metriken mit einer Verzögerung von wenigen Minuten angezeigt.
            - Wenn für „Eingabeereignisse“ ein höherer Wert als „0“ angezeigt wird, kann der Auftrag Eingabedaten lesen. Wenn für „Eingabeereignisse“ ein kleinerer Wert als „0“ angezeigt wird, gehen Sie wie folgt vor:
                - Um festzustellen, ob die Datenquelle gültige Daten enthält, überprüfen Sie diese mithilfe des [Service Bus-Explorer](https://code.msdn.microsoft.com/windowsapps/Service-Bus-Explorer-f2abca5a). Diese Prüfung gilt, wenn der Auftrag Event Hub als Eingabe verwendet.
                - Überprüfen Sie, ob das gewünschte Datenserialisierungsformat und die gewünschte Datencodierung verwendet werden.
                - Wenn der Auftrag einen Event Hub verwendet wird, überprüfen Sie, ob der Text der Meldung *Null* lautet.
            - Wenn für „Datenkonvertierungsfehler“ ein höherer Wert als „0“ angezeigt wird und sich dieser Wert ständig erhöht, kann dies Folgendes bedeuten:
                - Der Auftrag kann die Ereignisse unter Umständen nicht deserialisieren.
                - Das Ereignisschema stimmt möglicherweise nicht mit dem festgelegten oder erwarteten Schema der Ereignisse in der Abfrage überein.
                - Die Datentypen einiger Felder im Ereignis entsprechen möglicherweise nicht den Erwartungen.
            - Wenn für „Laufzeitfehler“ ein höherer Wert als „0“ angezeigt wird, bedeutet das, dass der Auftrag die Daten empfangen kann, bei der Verarbeitung der Abfrage jedoch Fehler generiert werden.
                - Navigieren Sie zur Fehlersuche zu [Überwachungsprotokolle](../azure-resource-manager/resource-group-audit.md) und filtern Sie nach dem Status *Fehler*.
            - Wenn für „InputEvents“ ein höherer Wert als „0“ und für „OutputEvents“ der Wert „0“ angezeigt wird, kann dies Folgendes bedeuten:
                - Die Abfrageverarbeitung hat zu null Ausgabeereignissen geführt.
                - Ereignisse oder die jeweiligen Felder sind unter Umständen falsch formatiert, daher wurden bei der Abfrageverarbeitung keine Ausgaben generiert.
                - Im Rahmen des Auftrags können Daten aus Konnektivitäts- oder Authentifizierungsgründen nicht per Push an die [Ausgabesenke](stream-analytics-select-into.md) übertragen werden.
        - Für alle zuvor genannten Fehler enthalten die Meldungen der Vorgangsprotokolle weitere Details (einschließlich Details dazu, was geschieht). Ausgenommen sind Fälle, in denen die Abfragelogik alle Ereignisse herausgefiltert hat. Wenn bei der Verarbeitung mehrerer Ereignisse Fehler generiert werden, protokolliert Stream Analytics innerhalb von 10 Minuten die ersten drei Fehlermeldungen desselben Typs in Vorgangsprotokollen. Weitere identische Fehler werden dann mit folgender Meldung unterdrückt: „Es treten zu schnell Fehler auf. Diese werden unterdrückt.“

8. Führen Sie ein Debuggen mithilfe von Überwachungs- und Diagnoseprotokollen durch:
    - Verwenden Sie [Überwachungsprotokolle](../azure-resource-manager/resource-group-audit.md), und filtern Sie diese, um Fehler festzustellen und zu debuggen.
    - Verwenden Sie [Auftragsdiagnoseprotokolle](stream-analytics-job-diagnostic-logs.md), um Fehler festzustellen und zu debuggen.

9. Überprüfen Sie die Ausgaben:
    - Wenn der Auftragsstatus *Wird ausgeführt* lautet, können Sie abhängig von der in der Abfrage festgelegten Dauer die Ausgabe in der Senkendatenquelle sehen.
    - Wenn Sie keine Ausgaben eines bestimmten Ausgabetyps sehen können, leiten Sie diese an einen weniger komplexen Ausgabetyp um (z.B. einen Azure Blob). Überprüfen Sie mithilfe des Speicher-Explorer, ob die Ausgabe angezeigt werden kann. Überprüfen Sie darüber hinaus, ob Einschränkungsgrenzwerte bei der Ausgabe den Empfang von Daten verhindern.

10. Verwenden Sie die Datenflussanalyse mit Auftragsdiagrammmetriken:
    - Verwenden Sie [Auftragsdiagramm mit Metriken](stream-analytics-job-diagram-with-metrics.md), um den Datenfluss zu analysieren und Probleme festzustellen.

11. Öffnen Sie einen Supportfall:
    - Wenn alle Maßnahmen fehlschlagen, öffnen Sie anhand der SubscriptionId, die Ihren Auftrag enthält, einen Microsoft-Supportfall.

## <a name="get-help"></a>Hier erhalten Sie Hilfe

Um Hilfe zu erhalten, nutzen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nächste Schritte

* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

