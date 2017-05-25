---
title: "Diagnostizieren von Fehlern – Azure Logic Apps | Microsoft-Dokumentation"
description: "Gängige Möglichkeiten zur Ermittlung von Fehlern bei Logik-Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: a6727ebd-39bd-4298-9e68-2ae98738576e
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.translationtype: Human Translation
ms.sourcegitcommit: 8a531f70f0d9e173d6ea9fb72b9c997f73c23244
ms.openlocfilehash: 25f4520bae3089ce9f11efcf6acd176255043e70
ms.contentlocale: de-de
ms.lasthandoff: 03/10/2017


---
# <a name="diagnose-logic-app-failures"></a>Diagnostizieren von Fehlern bei Logik-Apps
Wenn bei Logik-Apps Fehler oder Probleme auftreten, gibt es verschiedene Möglichkeiten, die Fehlerursache zu ermitteln.  

## <a name="azure-portal-tools"></a>Tools des Azure-Portals
Das Azure-Portal enthält viele Tools zum Diagnostizieren der einzelnen Schritte jeder Logik-App.

### <a name="trigger-history"></a>Triggerverlauf

Jede Logik-App verfügt über mindestens einen Trigger. Wenn Sie feststellen, dass Apps nicht ausgelöst werden, sehen Sie sich zunächst den Triggerverlauf an, um weitere Informationen zu erhalten. Sie können vom Hauptblatt der Logik-App auf den Triggerverlauf zugreifen.

![Ermitteln des Triggerverlaufs][1]

Im Triggerverlauf werden alle Auslöseversuche Ihrer Logik-App aufgelistet. Sie können auf die einzelnen Auslöseversuche klicken, um weitere Details zu erhalten, insbesondere zu den Eingaben und Ausgaben, die von einem Auslöseversuch generiert wurden. Falls Sie fehlerhafte Trigger bemerken, klicken Sie auf den Auslöseversuch, und wählen Sie anschließend den Link **Ausgaben**, um die Fehlermeldungen anzuzeigen, die generiert wurden (z.B. aufgrund ungültiger FTP-Anmeldeinformationen).

Es können verschiedene Status angezeigt werden:

* **Übersprungen**. An den Endpunkt wurde eine Abfrage zum Abruf von Daten gesendet. In der Antwort war angegeben, dass keine Daten verfügbar sind.
* **Erfolgreich**. Der Trigger hat die Antwort erhalten, dass Daten verfügbar sind. Dieser Status wurde unter Umständen durch einen manuellen Trigger, einen Wiederholungstrigger oder einen Abfragetrigger verursacht. Dieser Status umfasst in der Regel auch den Status **Ausgelöst**. Dies ist aber nicht der Fall, wenn eine Bedingung oder ein SplitOn-Befehl in der Codeansicht nicht erfüllt wurde.
* **Fehler**. Ein Fehler wurde generiert.

#### <a name="start-a-trigger-manually"></a>Manuelles Starten eines Triggers

Wenn die Logik-App sofort nach einem verfügbaren Trigger suchen soll, ohne auf die nächste Wiederholung zu warten, klicken Sie auf dem Hauptblatt auf **Trigger auswählen**, um den Vorgang zu erzwingen. Wenn Sie beispielsweise mit einem Dropbox-Trigger auf diesen Link klicken, fragt der Workflow Dropbox sofort nach neuen Dateien ab.

### <a name="run-history"></a>Ausführungsverlauf

Jeder ausgelöste Trigger führt zu einer Ausführung. Sie können vom Hauptblatt aus auf viele Details zur Ausführung zugreifen, mithilfe derer Sie feststellen können, was während des Workflows passiert ist.

![Ermitteln des Ausführungsverlaufs][2]

Eine Ausführung zeigt einen der folgenden Status an:

* **Erfolgreich**. Alle Aktionen waren erfolgreich. Wenn ein Fehler aufgetreten ist, wurde dieser von einer Aktion später im Workflow behoben. Anders gesagt: Der Fehler wurde von einer Aktion behoben, die zur Ausführung nach einer fehlerhaften Aktion eingerichtet wurde.
* **Fehler**. Bei mindestens einer Aktion ist ein Fehler aufgetreten, der nicht von einer Aktion später im Workflow behoben wurde.
* **Abgebrochen**. Der Workflow wurde ausgeführt, hat dann aber eine Abbruchanforderung erhalten.
* **Wird ausgeführt**. Der Workflow wird derzeit ausgeführt. Dieser Status kann bei gedrosselten Workflows oder aufgrund des aktuellen Tarifs auftreten. Details finden Sie unter den Aktionsbeschränkungen auf der Seite mit der [Preisübersicht](https://azure.microsoft.com/pricing/details/app-service/plans/). Eine Diagnose (die Diagramme unterhalb des Ausführungsverlaufs) kann auch Informationen zu auftretenden Drosselungsereignissen enthalten.

Im Ausführungsverlauf können Sie weitere Details anzeigen.  

#### <a name="trigger-outputs"></a>Triggerausgaben

Triggerausgaben zeigen die Daten an, die vom Trigger empfangen wurden. Mithilfe dieser Ausgaben können Sie ermitteln, ob alle Eigenschaften erwartungsgemäß zurückgegeben wurden.

> [!NOTE]
> Wenn Sie unbekannte Inhalte sehen, erhalten Sie hier Informationen dazu, wie Azure Logic Apps [unterschiedliche Inhaltstypen](../logic-apps/logic-apps-content-type.md) behandeln.
> 

![Beispiele für Triggerausgaben][3]

#### <a name="action-inputs-and-outputs"></a>Eingaben und Ausgaben von Aktionen

Sie können einen Drilldown in die Eingaben und Ausgaben durchführen, die von einer Aktion empfangen wurden. Diese Daten sind hilfreich, um Informationen zur Größe und Form der Ausgaben zu erhalten und Fehlermeldungen zu suchen, die ggf. generiert wurden.

![Eingaben und Ausgaben von Aktionen][4]

## <a name="debug-workflow-runtime"></a>Debuggen der Workflowlaufzeit

Zusätzlich zum Überwachen der Eingaben, Ausgaben und Trigger einer Ausführung können Sie auch einige Schritte zu einem Workflow hinzufügen, die beim Debuggen hilfreich sind. 
[RequestBin](http://requestb.in) ist ein leistungsfähiges Tool, das Sie als Schritt in einem Workflow hinzufügen können. Mit RequestBin können Sie einen HTTP-Anforderungsinspektor einrichten, um die Größe, die Form und das Format einer HTTP-Anforderung zu bestimmen. Sie können ein RequestBin-Element erstellen und die URL in eine HTTP POST-Aktion für eine Logik-App einfügen – mit dem Textinhalt, den Sie testen möchten (z.B. ein Ausdruck oder andere Schrittausgabe). Nach dem Ausführen der Logik-App können Sie das RequestBin-Element aktualisieren, um anzuzeigen, wie die Anforderung beim Generieren durch das Logic Apps-Modul gebildet wurde.

<!-- image references -->
[1]: ./media/logic-apps-diagnosing-failures/triggerhistory.png
[2]: ./media/logic-apps-diagnosing-failures/runhistory.png
[3]: ./media/logic-apps-diagnosing-failures/triggeroutputslink.png
[4]: ./media/logic-apps-diagnosing-failures/actionoutputs.png

