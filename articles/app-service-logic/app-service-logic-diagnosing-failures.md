<properties
   pageTitle="Durchführen der Diagnose für Logik-Apps | Microsoft Azure"
   description="Es wird beschrieben, welche gängigen Ansätze es gibt und warum es bei Apps zu Fehlern kommt."
   services="app-service\logic"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="app-service-logic"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/18/2016"
   ms.author="jehollan"/>
   
# Durchführen der Diagnose für Logik-Apps

Wenn für eine Logik-App Probleme oder Fehler auftreten, können Sie einige geeignete Ansätze wählen, um besser zu verstehen, welche Ursache die Fehler haben.

## Tools im Verwaltungsportal

Das Azure-Portal enthält viele Tools zum Diagnostizieren der einzelnen Logik-Apps im jeweiligen Schritt.

### Triggerverlauf

Jede Logik-App verfügt über mindestens einen Trigger. Wenn Sie Apps bemerken, die nicht ausgelöst werden, sollten Sie sich zuerst den Triggerverlauf ansehen. Sie können auf den Triggerverlauf über das Hauptblatt der Logik-App zugreifen:

![][1]

Es sind alle Triggerversuche aufgeführt, die von der Logik-App unternommen wurden. Sie können jeweils auf die Einträge klicken, um die nächste Detailebene anzuzeigen (speziell alle Eingaben und Ausgaben, die von einem Triggerversuch generiert wurden). Falls Sie fehlerhafte Trigger bemerken, sollten Sie auf den Triggerversuch klicken und sich den Ausgabelink (Outputs Link) genauer ansehen. So erhalten Sie weitere Informationen zu den Fehlermeldungen, die ggf. generiert werden (z.B. ungültige FTP-Anmeldeinformationen).

Unter Umständen werden die folgenden unterschiedlichen Status angezeigt:

* Skipped (Übersprungen): Der Endpunkt wurde abgefragt, um zu ermitteln, ob Daten verfügbar sind. Die Antwort lautete, dass keine Daten verfügbar sind.
* Succeeded (Erfolgreich): Der Trigger hat eine Antwort erhalten, dass Daten verfügbar sind. Sie kann von einem manuellen Trigger, Wiederholungstrigger oder Abfragetrigger stammen. Die Antwort umfasst wahrscheinlich auch den Zusatz „Fired“ (Ausgelöst). Dies ist aber nicht der Fall, wenn eine Bedingung oder ein splitOn-Zustand in der Codeansicht nicht erfüllt wurde.
* Failed (Fehler): Ein Fehler wurde generiert.

#### Manuelles Starten eines Triggers

Wenn die Logik-App sofort eine Prüfung auf einen verfügbaren Trigger durchführen soll (ohne auf die nächste Wiederholung zu warten), können Sie auf dem Hauptblatt auf die Schaltfläche **Select Trigger** (Trigger auswählen) klicken, um den Vorgang zu erzwingen. Wenn Sie das Klicken beispielsweise mit einem Dropbox-Trigger ausführen, fragt der Workflow Dropbox sofort auf neue Dateien ab.

### Ausführungsverlauf

Wenn ein Trigger ausgelöst wird, führt dies jeweils zu einer Ausführung. Sie können auf dem Hauptblatt auf Ausführungen zugreifen. Ausführungen enthalten viele Informationen und tragen zum Verständnis der Vorgänge während des Ablaufs bei.

![][2]

Eine Ausführung kann einen der folgenden Status haben:

* Succeeded (Erfolgreich): Alle Aktionen waren erfolgreich. Falls ein Fehler aufgetreten ist, wurde er von einer Aktion zu einem späteren Zeitpunkt im Workflow behoben (z.B. war für eine Aktion die Ausführung nach einer fehlgeschlagenen Aktion festgelegt).
* Failed (Fehler): Für mindestens eine Aktion ist ein Fehler aufgetreten, der nicht von einer Aktion später im Workflow behoben wurde.
* Cancelled (Abbruch): Der Workflow wurde ausgeführt, hat dann aber eine Abbruchanforderung erhalten.
* Running (Wird ausgeführt): Dies ist der Status, wenn ein Workflow derzeit ausgeführt wird. Dies kann auch für Abläufe unter dem aktuellen App Service-Plan passieren, die gedrosselt werden. Details finden Sie unter den Aktionsbeschränkungen auf der [Seite mit den Preisen](https://azure.microsoft.com/pricing/details/app-service/plans/). Unter „Konfigurieren der Diagnose“ (Diagramme unterhalb des Ausführungsverlaufs) werden Sie auch über alle Drosselungsereignisse informiert, die auftreten.

Wenn Sie eine Ausführung aufgerufen haben, können Sie weitere Details anzeigen.

#### Triggerausgaben

Unter den Triggerausgaben werden die Daten angezeigt, die vom Trigger empfangen wurden. Dies kann das Verständnis fördern, ob alle Eigenschaften wie erwartet zurückgegeben werden.

>[AZURE.NOTE] Es kann hilfreich sein zu verstehen, wie Logik-Apps [unterschiedliche Inhaltstypen behandeln](app-service-logic-content-type.md), wenn Sie unbekannte Inhalte sehen.

![][3]

#### Eingänge und Ausgänge von Aktionen

Sie können einen Drilldown in die Eingaben und Ausgaben durchführen, die von einer Aktion empfangen wurden. Dies ist nützlich für das Verständnis der Größe und Form der Ausgaben und zur Anzeige aller Fehlermeldungen, die ggf. generiert wurden.

![][4]

## Debuggen der Workflowlaufzeit

Zusätzlich zum Überwachen der Eingaben, Ausgaben und Trigger einer Ausführung kann es hilfreich sein, zur Unterstützung des Debuggens einige Schritte in einem Workflow hinzuzufügen. Ein leistungsfähiges Tool, das Sie als Schritt in einem Workflow hinzufügen können, ist [RequestBin](http://requestb.in). Mit RequestBin können Sie einen HTTP-Anforderungsinspektor einrichten, um die Größe, die Form und das Format einer HTTP-Anforderung genau zu verstehen. Sie können ein neues RequestBin-Element erstellen und die URL in eine Logik-App-HTTP POST-Aktion einfügen – mit beliebigem Textinhalt, den Sie testen möchten (Ausdruck, andere Schrittausgabe usw.). Nach dem Ausführen der Logik-App können Sie das RequestBin-Element aktualisieren, um anzuzeigen, wie die Anforderung beim Generieren über das Logik-App-Modul gebildet wurde.




<!-- image references -->
[1]: ./media/app-service-logic-diagnosing-failures/triggerHistory.PNG
[2]: ./media/app-service-logic-diagnosing-failures/runHistory.PNG
[3]: ./media/app-service-logic-diagnosing-failures/triggerOutputsLink.PNG
[4]: ./media/app-service-logic-diagnosing-failures/ActionOutputs.PNG

<!---HONumber=AcomDC_0601_2016-->