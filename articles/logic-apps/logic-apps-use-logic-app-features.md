---
title: "Hinzufügen von bedingter Logik und Starten von Workflows – Azure Logic Apps | Microsoft-Dokumentation"
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
ms.author: LADocs; stepsic
ms.translationtype: Human Translation
ms.sourcegitcommit: 9f7d623ec213de6d46f59547aff9d4417ac95ede
ms.openlocfilehash: 41aafe94d24f0e22fe2256ab213c7668b670764c
ms.contentlocale: de-de
ms.lasthandoff: 02/15/2017


---
# <a name="use-logic-apps-features"></a>Verwenden von Logik-App-Features
Im [vorherigen Thema](../logic-apps/logic-apps-create-a-logic-app.md)haben Sie Ihre erste Logik-App erstellt. Jetzt erstellen Sie einen umfassenderen Prozess mit Azure Logic Apps. In diesem Thema werden die folgenden neuen Azure Logic Apps-Konzepte vorgestellt:

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

## <a name="add-conditional-logic-to-your-logic-app"></a>Hinzufügen von bedingter Logik zu Ihrer Logik-App

Auch wenn der ursprüngliche Flow Ihrer Logik-App funktioniert, konnten wir einige Bereiche verbessern.

### <a name="conditional"></a>Bedingt

Mit Ihrer ersten Logik-App könnten Sie zu viele E-Mails erhalten. In den folgenden Schritten wird bedingte Logik hinzugefügt, damit Sie nur eine E-Mail erhalten, wenn der Tweet von einer Person mit einer bestimmten Anzahl von Followern stammt.

0. Wählen Sie im Logik-App-Designer **Neuer Schritt** (+) > **Aktion hinzufügen** aus.
0.    Suchen Sie nach der Aktion **Benutzer abrufen** für Twitter, und fügen Sie sie hinzu.
0. Um die Informationen über den Twitter-Benutzer zu erhalten, suchen Sie das Feld **Getwittert von** aus dem Trigger, und fügen Sie es hinzu.

    ![Benutzer abrufen](media/logic-apps-use-logic-app-features/getuser.png)

0. Wählen Sie **Neuer Schritt** (+) > **Bedingung hinzufügen** aus.
0. Um nach der Anzahl der Follower von Benutzern zu filtern, wählen Sie unter **Objektname** die Option **Dynamischen Inhalt hinzufügen** aus. 
0.    Suchen Sie im Suchfeld nach dem Feld **Anzahl der Follower**, und fügen Sie es hinzu.
0. Wählen Sie unter **Beziehung** die Option **ist größer als** aus.
0. Geben Sie im Feld **Wert** die Anzahl von Followern ein, die Benutzer haben sollten.

    ![Bedingt](media/logic-apps-use-logic-app-features/conditional.png)

0. Ziehen Sie abschließend das Feld **E-Mail senden** in das Feld**Wenn ja**. 

Jetzt erhalten Sie nur E-Mails, wenn die Anzahl der Follower die Bedingung erfüllt.

## <a name="repeat-actions-over-a-list-with-foreach"></a>Wiederholen von Aktionen über eine Liste mit forEach

Die forEach-Schleife erwartet ein Array, über das eine Aktion wiederholt wird. Handelt es sich nicht um ein Array, schlägt der Flow fehl. Wenn „action1“ beispielsweise ein Array von Nachrichten ausgibt und Sie jede Nachricht senden möchten, können Sie diese forEach-Anweisung in den Eigenschaften Ihrer Aktion einfügen: `forEach : "@action('action1').outputs.messages"`

## <a name="edit-the-code-definition-for-a-logic-app"></a>Bearbeiten der Codedefinition für eine Logik-App

Zwar verfügen Sie über den Logik-App-Designer, Sie können aber den Code, der eine Logik-App definiert, auch direkt bearbeiten.

1. Wählen Sie auf der Befehlsleiste **Codeansicht** aus.

    Ein vollständiger Editor wird geöffnet und zeigt die Definition an, die Sie bearbeitet haben.

    ![Codeansicht](media/logic-apps-use-logic-app-features/codeview.png)

    In dem Text-Editor können Sie eine beliebige Anzahl von Aktionen innerhalb der gleichen oder zwischen Logik-Apps kopieren und einfügen. 
    Sie können auch auf einfache Weise in der gesamten Definition Abschnitte hinzufügen oder entfernen und auch Definitionen mit anderen Benutzern gemeinsam nutzen.

2. Um Ihre Änderungen zu speichern, wählen Sie **Speichern** aus.

### <a name="parameters"></a>Parameter

Einige Funktionen von Logik-Apps sind nur in der Codeansicht verfügbar, z.B. Parameter. Parameter erleichtern das Wiederverwenden von Werten in der gesamten Logik-App. Wenn Sie z.B. eine E-Mail-Adresse haben, die in verschiedenen Aktionen verwendet werden soll, sollten Sie die E-Mail-Adresse als Parameter definieren.

Parameter sind eine gute Möglichkeit, Werte auszuwählen, die Sie wahrscheinlich häufig ändern. Sie sind besonders nützlich, wenn Sie Parameter in verschiedenen Umgebungen überschreiben müssen. Weitere Informationen zum Überschreiben von Parametern auf Grundlage der Umgebung finden Sie in der [REST-API-Dokumentation](https://docs.microsoft.com/rest/api/logic).

Dieses Beispiel zeigt, wie Sie Ihre vorhandene Logik-App so aktualisieren, dass Sie Parameter als Abfrageausdruck verwenden können.

1. Suchen Sie in der Codeansicht nach dem `parameters : {}`-Objekt, und fügen Sie ein topic-Objekt hinzu:

        "topic" : {
            "type" : "string",
            "defaultValue" : "MicrosoftAzure"
        }

2. Wechseln Sie zur `twitterconnector`-Aktion, suchen Sie den Abfragewert, und ersetzen Sie diesen Wert durch `#@{parameters('topic')}`. 

    Sie können auch die `concat`-Funktion verwenden, um zwei oder mehr Zeichenfolgen zu verknüpfen. 
    Beispielsweise funktioniert `@concat('#',parameters('topic'))` genauso wie das Beispiel oben.

3.    Wenn Sie fertig sind, wählen Sie **Speichern** aus. 

    Jetzt werden Ihnen stündlich alle neuen Tweets mit mehr als fünf Retweets in einen Ordner namens **Tweets** in Ihrer Dropbox übermittelt.

Mehr Informationen zu Logik-App-Definitionen finden Sie unter [Logik-App-Definitionen erstellen](../logic-apps/logic-apps-author-definitions.md).

## <a name="start-logic-app-workflows"></a>Starten des Logik-App-Workflows

Sie haben mehrere Möglichkeiten zum Starten des Workflows, der in der Logik-App definiert ist. Sie können einen Workflow immer bei Bedarf im [Azure-Portal] starten.

### <a name="recurrence-triggers"></a>Wiederholungstrigger

Ein Wiederholungstrigger wird in einem von Ihnen angegebenen Intervall ausgeführt. Wenn der Trigger bedingte Logik aufweist, bestimmt der Trigger, ob der Workflow ausgeführt werden muss. Ein Trigger gibt an, dass der Workflow ausgeführt werden muss, indem der Statuscode `200` zurückgegeben wird. Wenn der Workflow nicht ausführt werden muss, gibt der Trigger den Statuscode `202` zurück.

### <a name="callback-using-rest-apis"></a>Rückruf mit REST-APIs

Dienste können einen Logik-App-Endpunkt zum Starten eines Workflows aufrufen. Um eine solche Logik-App bei Bedarf zu starten, wählen Sie auf der Befehlsleiste **Jetzt ausführen** aus. Weitere Informationen finden Sie unter [Logik-Apps als aufrufbare Endpunkte](../logic-apps/logic-apps-http-endpoint.md). 

<!-- Shared links -->
[Azure-Portal]: https://portal.azure.com

