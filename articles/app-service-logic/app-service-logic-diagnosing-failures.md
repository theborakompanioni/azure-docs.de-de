---
title: Diagnostizieren von Fehlern bei Logik-Apps | Microsoft-Dokumentation
description: "Gängige Ansätze zur Ermittlung von Fehlern bei Logik-Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: erikre
editor: 
ms.assetid: a6727ebd-39bd-4298-9e68-2ae98738576e
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: jehollan
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: fb3d9e2f8d148bddfd43555254e422805c668128


---
# <a name="diagnosing-logic-app-failures"></a>Diagnostizieren von Fehlern bei Logik-Apps
Wenn beim Logik-Apps-Feature von Azure App Service Fehler oder Probleme auftreten, gibt es einige Möglichkeiten, herauszufinden, wodurch die Fehler bzw. Probleme entstehen.  

## <a name="azure-portal-tools"></a>Tools des Azure-Portals
Das Azure-Portal enthält viele Tools zum Diagnostizieren der einzelnen Schritte jeder Logik-App.

### <a name="trigger-history"></a>Triggerverlauf
Jede Logik-App verfügt über mindestens einen Trigger. Wenn Sie feststellen, dass Apps keine Ereignisse auslösen, sollten Sie als Erstes im Triggerverlauf nach weiteren Informationen suchen. Sie können vom Hauptblatt der Logik-App auf den Triggerverlauf zugreifen:

![Ermitteln des Triggerverlaufs][1]

Damit werden alle Auslöseversuche Ihrer App aufgelistet. Sie können auf die einzelnen Auslöseversuche klicken, um die nächste Detailebene anzuzeigen (insbesondere alle Eingaben und Ausgaben, die von einem Auslöseversuch generiert wurden). Falls Sie fehlerhafte Trigger bemerken, klicken Sie auf den Auslöseversuch und sehen sich den Link **Outputs** genauer an. So erhalten Sie weitere Informationen zu den Fehlermeldungen, die ggf. generiert wurden (z.B. ungültige FTP-Anmeldeinformationen).

Es können verschiedene Status angezeigt werden:

* **Übersprungen**. Der Trigger hat eine Abfrage zum Abruf von Daten an den Endpunkt gesendet und die Antwort erhalten, dass keine Daten verfügbar sind.
* **Erfolgreich**. Der Trigger hat die Antwort erhalten, dass Daten verfügbar sind. Diese Antwort kann von einem manuellen Trigger, Wiederholungstrigger oder einem Abfragetrigger stammen. Die Antwort umfasst wahrscheinlich auch den Zusatz **Fired**. Dies ist aber nicht der Fall, wenn eine Bedingung oder ein SplitOn-Befehl in der Codeansicht nicht erfüllt wurde.
* **Fehler**. Ein Fehler wurde generiert.

#### <a name="starting-a-trigger-manually"></a>Manuelles Starten eines Triggers
Wenn die Logik-App sofort nach einem verfügbaren Trigger suchen soll (ohne auf die nächste Wiederholung zu warten), können Sie auf dem Hauptblatt auf **Trigger auswählen** klicken, um den Vorgang zu erzwingen. Wenn Sie beispielsweise mit einem Dropbox-Trigger auf diesen Link klicken, fragt der Workflow Dropbox sofort nach neuen Dateien ab.

### <a name="run-history"></a>Ausführungsverlauf
Der ausgelöste Trigger führt zu einer Ausführung. Sie können vom Hauptblatt aus auf eine Vielzahl Informationen zur Ausführung zugreifen, die Ihnen dabei helfen herauszufinden, was während des Workflows passiert.

![Ermitteln des Ausführungsverlaufs][2]

Eine Ausführung zeigt einen der folgenden Status an:

* **Erfolgreich**. Alle Aktionen wurden erfolgreich ausgeführt bzw., wenn ein Fehler aufgetreten ist, wurde dieser von einer Aktion später im Workflow behoben. Anders gesagt: Der Fehler wurde von einer Aktion behoben, die zur Ausführung nach einer fehlerhaften Aktion eingerichtet wurde.
* **Fehler**. Bei mindestens einer Aktion ist ein Fehler aufgetreten, der nicht von einer Aktion später im Workflow behoben wurde.
* **Abgebrochen**. Der Workflow wurde ausgeführt, hat dann aber eine Abbruchanforderung erhalten.
* **Wird ausgeführt**. Der Workflow wird derzeit ausgeführt. Dies kann bei Workflows passieren, die gedrosselt werden, oder aufgrund des aktuellen App Service-Plans. Details finden Sie unter den Aktionsbeschränkungen auf der Seite mit der [Preisübersicht](https://azure.microsoft.com/pricing/details/app-service/plans/). Eine Diagnose (die Diagramme unterhalb des Ausführungsverlaufs) kann auch Informationen zu auftretenden Drosselungsereignissen bieten.

Im Ausführungsverlauf können Sie weitere Details anzeigen.  

#### <a name="trigger-outputs"></a>Triggerausgaben
Triggerausgaben zeigen die Daten an, die vom Trigger empfangen wurden. Damit können Sie ermitteln, ob alle Eigenschaften erwartungsgemäß zurückgegeben werden.

> [!NOTE]
> Es kann hilfreich sein zu wissen, wie das Logik-Apps-Feature [unterschiedliche Inhaltstypen behandelt](app-service-logic-content-type.md) , wenn Sie unbekannte Inhalte sehen.
> 
> 

![Beispiele für Triggerausgaben][3]

#### <a name="action-inputs-and-outputs"></a>Eingaben und Ausgaben von Aktionen
Sie können einen Drilldown in die Eingaben und Ausgaben durchführen, die von einer Aktion empfangen wurden. Dies ist nützlich für das Verständnis der Größe und Form der Ausgaben und zur Anzeige aller Fehlermeldungen, die ggf. generiert wurden.

![Eingaben und Ausgaben von Aktionen][4]

## <a name="debugging-workflow-runtime"></a>Debuggen der Workflowlaufzeit
Zusätzlich zum Überwachen der Eingaben, Ausgaben und Trigger einer Ausführung kann es hilfreich sein, zur Unterstützung des Debuggens einige Schritte in einem Workflow hinzuzufügen. [RequestBin](http://requestb.in) ist ein leistungsfähiges Tool, das Sie als Schritt in einem Workflow hinzufügen können. Mit RequestBin können Sie einen HTTP-Anforderungsinspektor einrichten, um die Größe, die Form und das Format einer HTTP-Anforderung zu bestimmen. Sie können ein neues RequestBin-Element erstellen und die URL in eine HTTP POST-Aktion für eine Logik-App einfügen – mit dem Textinhalt, den Sie testen möchten (z.B. ein Ausdruck oder andere Schrittausgabe). Nach dem Ausführen der Logik-App können Sie das RequestBin-Element aktualisieren, um anzuzeigen, wie die Anforderung beim Generieren durch das Logik-Apps-Modul gebildet wurde.

<!-- image references -->
[1]: ./media/app-service-logic-diagnosing-failures/triggerHistory.PNG
[2]: ./media/app-service-logic-diagnosing-failures/runHistory.PNG
[3]: ./media/app-service-logic-diagnosing-failures/triggerOutputsLink.PNG
[4]: ./media/app-service-logic-diagnosing-failures/ActionOutputs.PNG



<!--HONumber=Nov16_HO3-->


