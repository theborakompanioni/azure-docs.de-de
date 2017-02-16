---
title: "Hinzufügen von Bedingungen und Trigger für die Ausführung von Workflows – Azure Logic Apps | Microsoft-Dokumentation"
description: "Steuern Sie durch Hinzufügen bedingter Logik, von Triggern, Aktionen und Parametern, wie Workflows in Azure Logic Apps ausgeführt werden."
author: stepsic-microsoft-com
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: e4e24de4-049a-4b3a-a14c-3bf3163287a8
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/28/2017
ms.author: stepsic
translationtype: Human Translation
ms.sourcegitcommit: d090ce5a912a2079d2e47d13caf60ca701f0e548
ms.openlocfilehash: 330359583346f7e1944b53ec3fd34104f10e1261


---
# <a name="use-logic-apps-features"></a>Verwenden von Logik-App-Features
Im [vorherigen Thema](../logic-apps/logic-apps-create-a-logic-app.md)haben Sie Ihre erste Logik-App erstellt. Nun erfahren Sie, wie einen vollständigeren Prozess mithilfe von App Services-Logik-Apps erstellen können. In diesem Thema werden die folgenden neuen Logik-App-Konzepte vorgestellt:

* Bedingte Logik, die eine Aktion ausführt, wenn eine bestimmte Bedingung erfüllt ist.
* Codeansicht zum Bearbeiten einer vorhandenen Logik-App.
* Optionen zum Starten eines Workflows.

Bevor Sie dieses Thema durchgehen, sollten Sie die Schritte unter [Erstellen einer neuen Logik-App](../logic-apps/logic-apps-create-a-logic-app.md)ausgeführt haben. Navigieren Sie im [Azure-Portal]zu Ihrer Logik-App, und klicken Sie in der Übersicht auf **Trigger und Aktionen** , um die Definition der Logik-App zu bearbeiten.

## <a name="reference-material"></a>Referenzmaterial
Die folgenden Dokumente könnten hilfreich sein:

* [Verwaltungs- und Laufzeit-REST-APIs](https://msdn.microsoft.com/library/azure/mt643787.aspx) : hier wird auch erklärt, wie Logik-Apps direkt aufgerufen werden.
* [Sprachreferenz](https://msdn.microsoft.com/library/azure/mt643789.aspx) : eine umfassende Liste aller Funktionen und Ausdrücke, die unterstützt werden.
* [Trigger- und Aktionstypen](https://msdn.microsoft.com/library/azure/mt643939.aspx) : die verschiedenen Arten von Aktionen und die dazugehörigen Eingaben.
* [Übersicht über App Service](../app-service/app-service-value-prop-what-is.md) : Beschreibung, welche Komponenten für das Erstellen welcher Lösung ausgewählt werden.

## <a name="adding-conditional-logic"></a>Hinzufügen von bedingter Logik
Wenngleich der ursprüngliche Datenfluss funktioniert, gibt es einige Bereiche, die verbessert werden könnten.

### <a name="conditional"></a>Bedingt
Diese Logik-App könnte dazu führen, dass Sie sehr viele E-Mail-Nachrichten erhalten. In den folgenden Schritten wird Logik hinzugefügt, um sicherzustellen, dass Sie nur eine E-Mail-Nachricht erhalten, wenn der Tweet von einer Person mit einer bestimmten Anzahl von Followern stammt.

1. Klicken Sie auf das Pluszeichen, und suchen Sie die Aktion *Benutzer abrufen* für Twitter.
2. Übergeben Sie das Feld **Getweetet von** aus dem Trigger, um Informationen zum Twitter-Benutzer abzurufen.

    ![Benutzer abrufen](media/logic-apps-use-logic-app-features/getuser.png)
3. Klicken Sie erneut auf das Pluszeichen, aber wählen Sie dieses Mal **Add Condition**
4. Klicken Sie im ersten Feld unter **Get User** (Benutzer abrufen) auf **...**, um das Feld **Followers count** (Anzahl der Follower) zu suchen.
5. Wählen Sie in der Dropdownliste **ist größer als**
6. Geben Sie im zweiten Feld die Anzahl von Followern ein, die ein Benutzer haben sollte.

    ![Bedingt](media/logic-apps-use-logic-app-features/conditional.png)
7. Ziehen Sie abschließend das Feld mit der E-Mail in das Feld **If Yes** . Damit erhalten Sie nur dann eine E-Mail, wenn die Anzahl der Follower erreicht ist.

## <a name="repeating-over-a-list-with-foreach"></a>Iteration einer Liste mit forEach
Die forEach-Schleife erwartet ein Array, über das eine Aktion wiederholt wird. Handelt es sich nicht um ein Array, schlägt der Ablauf fehl. Wenn Aktion1 beispielsweise ein Array von Nachrichten ausgibt, und Sie jede Nachricht senden möchten, können Sie diese forEach-Anweisung in den Eigenschaften Ihrer Aktion einfügen: forEach : "@action('action1').outputs.messages"

## <a name="using-the-code-view-to-edit-a-logic-app"></a>Verwenden der Codeansicht zum Bearbeiten einer Logik-App
Außer im Designer können Sie die Definition einer Logik-App wie folgt direkt bearbeiten.

1. Klicken Sie auf der Befehlsleiste auf die Schaltfläche **Codeansicht** .

    Dadurch wird ein vollständiger Editor geöffnet, der die Definition zeigt, die Sie gerade bearbeitet haben.

    ![Codeansicht](media/logic-apps-use-logic-app-features/codeview.png)

    Mithilfe des Text-Editors können Sie eine beliebige Anzahl von Aktionen innerhalb der gleichen oder zwischen Logik-Apps kopieren und einfügen. Sie können auch auf einfache Weise in der gesamten Definition Abschnitte hinzufügen oder entfernen und auch Definitionen mit anderen Benutzern gemeinsam nutzen.
2. Nachdem Sie in der Codeansicht Änderungen vorgenommen haben, klicken Sie auf **Speichern**.

### <a name="parameters"></a>Parameter
Es gibt einige Funktionen von Logik-Apps, die nur in der Codeansicht verwendet werden können. Ein Beispiel dafür sind Parameter. Parameter erleichtern das Wiederverwenden von Werten in der gesamten Logik-App. Wenn Sie z. B. eine E-Mail-Adresse haben, die in verschiedenen Aktionen verwendet werden soll, sollten Sie sie als Parameter definieren.

Der folgende Code aktualisiert die vorhandene Logik-App so, dass Parameter für den Abfrageausdruck verwendet werden.

1. Suchen Sie in der Codeansicht das `parameters : {}` -Objekt, und fügen Sie das folgende "Topic"-Objekt hinzu:

        "topic" : {
            "type" : "string",
            "defaultValue" : "MicrosoftAzure"
        }
2. Scrollen Sie zur `twitterconnector`-Aktion, suchen Sie den Abfragewert, und ersetzen Sie ihn durch `#@{parameters('topic')}`.
    Außerdem können Sie die **Concat**-Funktion verwenden, um zwei oder mehr Zeichenfolgen zu verknüpfen, z.B. ist `@concat('#',parameters('topic'))` identisch mit der oben genannten.

Parameter sind eine gute Möglichkeit, Werte auszuwählen, die Sie wahrscheinlich häufig ändern. Sie sind besonders nützlich, wenn Sie Parameter in verschiedenen Umgebungen überschreiben müssen. Weitere Informationen zum Überschreiben von Parametern auf Grundlage der Umgebung finden Sie in unserer [REST-API-Dokumentation](https://msdn.microsoft.com/library/mt643787.aspx).

Wenn Sie nun auf **Speichern** klicken, werden Ihnen stündlich alle neuen Tweets mit mehr als fünf Retweets in einen Ordner namens **Tweets** in Ihrer Dropbox übermittelt.

Mehr Informationen zu Logik-App-Definitionen finden Sie unter [Logik-App-Definitionen erstellen](../logic-apps/logic-apps-author-definitions.md).

## <a name="starting-a-logic-app-workflow"></a>Starten des Workflows einer Logik-App
Es gibt mehrere Möglichkeiten zum Starten des Workflows, der in der Logik-App definiert ist. Ein Workflow kann bei Bedarf stets im [Azure-Portal]gestartet werden.

### <a name="recurrence-triggers"></a>Wiederholungstrigger
Ein Wiederholungstrigger wird in einem von Ihnen angegebenen Intervall ausgeführt. Wenn der Trigger bedingte Logik aufweist, bestimmt der Trigger, ob der Workflow ausgeführt werden muss oder nicht. Ein Trigger gibt an, dass er ausgeführt werden sollte, indem der Statuscode `200` zurückgegeben wird. Wenn er nicht ausgeführt werden muss, wird der Statuscode `202` zurückgegeben.

### <a name="callback-using-rest-apis"></a>Rückruf mit REST-APIs
Dienste können einen Logik-App-Endpunkt zum Starten eines Workflows aufrufen. Weitere Informationen finden Sie unter [Logik-Apps als aufrufbare Endpunkte](../logic-apps/logic-apps-http-endpoint.md) . Um eine solche Logik-App bei Bedarf zu starten, klicken Sie in der Befehlsleiste auf die Schaltfläche **Jetzt ausführen** . 

<!-- Shared links -->
[Azure-Portal]: https://portal.azure.com



<!--HONumber=Jan17_HO4-->


