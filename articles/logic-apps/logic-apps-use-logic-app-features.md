---
title: "Hinzufügen von Bedingungen und Starten von Workflows – Azure Logic Apps | Microsoft-Dokumentation"
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
ms.sourcegitcommit: 7c69630688e4bcd68ab3b4ee6d9fdb0e0c46d04b
ms.openlocfilehash: e632c48ed31e82536db55a9c54438bece0c38fd4
ms.contentlocale: de-de
ms.lasthandoff: 06/24/2017


---
# <a name="use-logic-apps-features"></a>Verwenden von Logik-App-Features

In einem [vorherigen Thema](../logic-apps/logic-apps-create-a-logic-app.md) haben Sie Ihre erste Logik-App erstellt. Um Ihren Logik-App-Workflow zu steuern, können Sie unterschiedliche Pfade für die Ausführung der Logik-App angeben und festlegen, wie Daten in Arrays, Auflistungen und Batches verarbeitet werden. Sie können diese Elemente in Ihrem Logik-App-Workflow einschließen:

* Durch [switch-Anweisungen](../logic-apps/logic-apps-switch-case.md) und Bedingungen, kann Ihre Logik-App verschiedene Aktionen basierend darauf ausführen, ob bestimmte Bedingungen erfüllt sind.

* Durch [Schleifen](../logic-apps/logic-apps-loops-and-scopes.md) führt Ihre Logik-App Schritte wiederholt aus. Sie können beispielsweise Aktionen über ein Array wiederholen, wenn Sie eine **For_each**-Schleife verwenden. Alternativ können Sie Aktionen wiederholen, bis eine Bedingung erfüllt ist, wenn Sie eine **Bis**-Schleife verwenden.

* Durch [Bereiche](../logic-apps/logic-apps-loops-and-scopes.md) können Sie Abfolgen von Aktionen gruppieren, z.B. zum Implementieren der Ausnahmebehandlung.

* Durch das [Auflösen von Batches](../logic-apps/logic-apps-loops-and-scopes.md) kann Ihre Logik-App getrennte Workflows für Elemente in einem Array starten, wenn Sie den **SplitOn**-Befehl verwenden.

In diesem Thema werden Ihnen andere Konzepte zum Erstellen Ihrer Logik-App vorgestellt:

* Codeansicht zum Bearbeiten einer vorhandenen Logik-App
* Optionen zum Starten eines Workflows

## <a name="conditions-run-steps-only-after-meeting-a-condition"></a>Bedingungen: Führen Sie Schritte erst aus, wenn eine Bedingung erfüllt ist

Damit Ihre Logik-App die Schritte nur ausführt, wenn die Daten bestimmte Kriterien erfüllen, können Sie eine Bedingung hinzufügen, die Daten im Workflow anhand bestimmter Felder oder Werte vergleicht.

Angenommen Sie verfügen über eine Logik-App, die Ihnen zu viele E-Mails über Beiträge auf dem RSS-Feed einer Website sendet. Dann können Sie eine Bedingung hinzufügen, sodass Ihre Logik-App nur dann E-Mails sendet, wenn der neue Beitrag zu einer bestimmten Kategorie gehört.

1. Suchen oder öffnen Sie Ihre Logik-App im [Azure-Portal](https://portal.azure.com) im Logik-App-Designer.

2. Fügen Sie eine Bedingung zum gewünschten Workflow-Speicherort hinzu. 

   Um die Bedingung zwischen vorhandenen Schritten in den Logik-App-Workflow hinzuzufügen, bewegen Sie den Zeiger auf den Pfeil, zu dem Sie die Bedingung hinzufügen möchten. 
   Wählen Sie das **Pluszeichen** (**+**) und dann **Bedingung hinzufügen** aus. Beispiel:

   ![Hinzufügen einer Bedingung zur Logik-App](./media/logic-apps-use-logic-app-features/add-condition.png)

   > [!NOTE]
   > Wenn Sie eine Bedingung am Ende des aktuellen Workflows hinzufügen möchten, wechseln Sie zum unteren Rand der Logik-App, und wählen Sie **+ Neuen Schritt** aus.

3. Definieren Sie nun die Bedingung. Geben Sie das Quellfeld an, das Sie auswerten möchten, den auszuführenden Vorgang und den Zielwert oder das Zielfeld. Um vorhandene Felder zu Ihrer Bedingung hinzuzufügen, wählen Sie aus der **Liste „Dynamischen Inhalt hinzufügen“** aus.

   Beispiel:

   ![Bearbeiten einer Bedingung im Standardmodus](./media/logic-apps-use-logic-app-features/edit-condition-basic-mode.png)

   Hier ist die vollständige Bedingung:

   ![Vollständige Bedingung](./media/logic-apps-use-logic-app-features/edit-condition-basic-mode-2.png)

   > [!TIP]
   > Um die Bedingung im Code zu definieren, wählen Sie **Im erweiterten Modus bearbeiten**. Beispiel:
   > 
   > ![Bearbeiten einer Bedingung im Code](./media/logic-apps-use-logic-app-features/edit-condition-advanced-mode.png)

4. Fügen Sie unter **Wenn Ja** und **Wenn Nein** die auszuführenden Schritte basierend darauf hinzu, ob die Bedingung erfüllt ist.

   Beispiel:

   ![Bedingung mit Ja- und Nein-Pfaden](./media/logic-apps-use-logic-app-features/condition-yes-no-path.png)

   > [!TIP]
   > Sie können die vorhandenen Aktionen in die **Wenn Ja** und **Wenn Nein**-Pfade ziehen.

5. Wenn Sie fertig sind, speichern Sie Ihre Logik-App.

Jetzt erhalten Sie E-Mail-Nachrichten ausschließlich dann, wenn die Beiträge Ihre Bedingung erfüllen.

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

## <a name="parameters"></a>Parameter

Einige Funktionen von Logik-Apps sind nur in der Codeansicht verfügbar, z.B. Parameter. Parameter erleichtern das Wiederverwenden von Werten in der gesamten Logik-App. Wenn Sie z.B. eine E-Mail-Adresse haben, die in verschiedenen Aktionen verwendet werden soll, sollten Sie die E-Mail-Adresse als Parameter definieren.

Parameter sind eine gute Möglichkeit, Werte auszuwählen, die Sie wahrscheinlich häufig ändern. Sie sind besonders nützlich, wenn Sie Parameter in verschiedenen Umgebungen überschreiben müssen. Weitere Informationen zum Überschreiben von Parametern auf Grundlage der Umgebung finden Sie unter [Erstellen von Logik-App-Definitionen](../logic-apps/logic-apps-author-definitions.md) und [REST-API-Dokumentation](https://docs.microsoft.com/rest/api/logic).

Dieses Beispiel zeigt, wie Sie Ihre vorhandene Logik-App so aktualisieren, dass Sie Parameter als Abfrageausdruck verwenden können.

1. Suchen Sie in der Codeansicht nach dem `parameters : {}`-Objekt, und fügen Sie ein `currentFeedUrl`-Objekt hinzu:

        "currentFeedUrl" : {
            "type" : "string",
            "defaultValue" : "http://rss.cnn.com/rss/cnn_topstories.rss"
        }

2. Wechseln Sie zur `When_a_feed-item_is_published`-Aktion, suchen Sie den `queries`-Bereich, und ersetzen Sie den Abfragewert durch: `"feedUrl": "#@{parameters('currentFeedUrl')}"` 

    Sie können auch die `concat`-Funktion verwenden, um zwei oder mehr Zeichenfolgen zu verknüpfen. 
    Beispielsweise funktioniert `"@concat('#',parameters('currentFeedUrl'))"` 
    genauso wie das Beispiel oben.

3.  Wenn Sie fertig sind, wählen Sie **Speichern** aus. 

    Jetzt können Sie den RSS-Feed der Website ändern, indem Sie eine andere URL über das `currentFeedURL`-Objekt übergeben.

Erfahren Sie mehr darüber, [wie Sie Logik-App-Definitionen erstellen](../logic-apps/logic-apps-author-definitions.md).

## <a name="start-logic-app-workflows"></a>Starten des Logik-App-Workflows

Sie haben mehrere Möglichkeiten zum Starten des Workflows, der in der Logik-App definiert ist. Sie können einen Workflow immer bei Bedarf im [Azure-Portal] starten.

### <a name="recurrence-triggers"></a>Wiederholungstrigger

Ein Wiederholungstrigger wird in einem von Ihnen angegebenen Intervall ausgeführt. Wenn der Trigger bedingte Logik aufweist, bestimmt der Trigger, ob der Workflow ausgeführt werden muss. Ein Trigger gibt an, dass der Workflow ausgeführt werden muss, indem der Statuscode `200` zurückgegeben wird. Wenn der Workflow nicht ausführt werden muss, gibt der Trigger den Statuscode `202` zurück.

### <a name="callback-using-rest-apis"></a>Rückruf mit REST-APIs

Dienste können einen Logik-App-Endpunkt zum Starten eines Workflows aufrufen. Um eine solche Logik-App bei Bedarf zu starten, wählen Sie auf der Befehlsleiste **Jetzt ausführen** aus. Weitere Informationen finden Sie unter [Logik-Apps als aufrufbare Endpunkte](../logic-apps/logic-apps-http-endpoint.md). 

<!-- Shared links -->
[Azure-Portal]: https://portal.azure.com

## <a name="next-steps"></a>Nächste Schritte

* [Wechsel von Anweisungen](../logic-apps/logic-apps-switch-case.md) 
* [Schleifen, Bereiche und Auflösen von Batches](../logic-apps/logic-apps-loops-and-scopes.md)
* [Erstellen von Logik-App-Definitionen](../logic-apps/logic-apps-author-definitions.md)
