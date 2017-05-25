---
title: Verwenden der switch-Anweisung in Azure Logic Apps | Microsoft-Dokumentation
description: "Auswählen anderer in Logik-Apps auszuführender Aktionen basierend auf Ausdruckswerten unter Verwendung einer switch-Anweisung"
services: logic-apps
keywords: switch-Anweisung
author: derek1ee
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: LADocs; deli
ms.translationtype: Human Translation
ms.sourcegitcommit: 303cb9950f46916fbdd58762acd1608c925c1328
ms.openlocfilehash: 196f6d03567cbad8e061d45be546bc86015ede2e
ms.contentlocale: de-de
ms.lasthandoff: 04/04/2017

---

# <a name="perform-different-actions-in-logic-apps-with-a-switch-statement"></a>Ausführen verschiedener Aktionen in Logik-Apps mit einer switch-Anweisung

Beim Erstellen eines Workflows müssen Sie häufig verschiedene Aktionen ausführen, die auf dem Wert eines Objekts oder Ausdrucks basieren. Beispielsweise soll sich ihre Logik-App abhängig vom Statuscode einer HTTP-Anforderung oder der gewählten Option in einer E-Mail anders verhalten.

Sie können diese Szenarios mit einer switch-Anweisung implementieren. Ihre Logik-App kann ein Token oder einen Ausdruck auswerten und den Fall mit dem gleichen Wert zur Ausführung der angegebenen Aktion auswählen. Nur ein Fall sollte mit der switch-Anweisung übereinstimmen.

> [!TIP]
> Wie alle Programmiersprachen unterstützt die switch-Anweisung nur Gleichheitsoperatoren. Wenn Sie andere relationale Operatoren verwenden müssen – z.B. „größer als“ – verwenden Sie eine Bedingungsanweisung.
> Um deterministisches Ausführungsverhalten zu gewährleisten, müssen Fälle einen eindeutigen und statischen Wert statt eines dynamischen Tokens oder Ausdrucks enthalten.

## <a name="prerequisites"></a>Voraussetzungen

- Ein aktives Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen oder [Logic Apps kostenlos](https://tryappservice.azure.com/) testen.
- [Grundlegende Kenntnisse über Logik-Apps](logic-apps-what-are-logic-apps.md)

## <a name="add-a-switch-statement-to-your-workflow"></a>Hinzufügen einer switch-Anweisung zum Workflow

Um zu zeigen, wie die switch-Anweisung funktioniert, wird in diesem Beispiel eine Logik-App erstellt, die in Dropbox hochgeladene Dateien überwacht. Nachdem die neuen Dateien hochgeladen wurden, sendet die Logik-App eine E-Mail an eine genehmigende Person, die entscheidet, ob diese Dateien in SharePoint übertragen werden. Die App verwendet eine switch-Anweisung, die basierend auf dem Wert, den die genehmigende Person auswählt, verschiedene Aktionen ausführt.

1. Erstellen Sie eine Logik-App, und wählen Sie diesen Trigger: **Dropbox – Wenn eine Datei erstellt wird**.

   ![Verwenden des Triggers „Dropbox - When a file is created“ (Dropbox – Wenn eine Datei erstellt wird)](./media/logic-apps-switch-case/dropbox-trigger.jpg)

2. Fügen Sie unter dem Trigger diese Aktion hinzu: **Outlook.com – Genehmigungs-E-Mail senden**.

   > [!TIP]
   > Logik-Apps unterstützen auch das Szenario des Sendens der Genehmigungs-E-Mail über ein Office 365-Outlook-Konto.

   - Wenn noch keine Verbindung vorhanden ist, werden Sie aufgefordert, eine herzustellen.
   - Füllen Sie die erforderlichen Felder aus. Geben Sie beispielsweise unter **An** die E-Mail-Adresse für das Senden der E-Mail an die genehmigende Person an.
   - Geben Sie unter **Benutzeroptionen** die Option `Approve, Reject` ein.

   ![Konfigurieren der Verbindung](./media/logic-apps-switch-case/send-approval-email-action.jpg)

3. Fügen Sie eine switch-Anweisung hinzu.

   - Wählen Sie **+ Neuer Schritt** > **... Weitere** > **Parameter hinzufügen**. 
   - Jetzt möchten wir die basierend auf der `SelectedOptions`-Ausgabe der *Genehmigungs-E-Mail senden*-Aktion auszuführende Aktion auswählen. 
   Sie finden dieses Feld im **Dynamischen Inhalt hinzufügen**-Selektor.
   - Verwenden Sie *Fall 1*, wenn die genehmigende Person `Approve` wählt.
     - Wenn die Genehmigung erfolgt ist, kopieren Sie die ursprüngliche Datei mit der [**SharePoint Online – Datei erstellen**-Aktion](../connectors/connectors-create-api-sharepointonline.md) in SharePoint Online.
     - Fügen Sie dem Fall eine weitere Aktion hinzu, um Benutzer auf die Verfügbarkeit einer neuen Datei in SharePoint aufmerksam zu machen.
   - Fügen Sie einen weiteren Fall hinzu, der verwendet werden soll, wenn der Benutzer `Reject` wählt.
     - Versenden Sie im Falle einer Ablehnung eine E-Mail-Benachrichtigung, um andere genehmigende Personen zu informieren, dass die Datei abgelehnt wurde und keine weitere Aktion erforderlich ist.
   - `SelectedOptions` bietet nur zwei Optionen, sodass wir den **Standard**-Fall leer lassen können.

   ![switch-Anweisung](./media/logic-apps-switch-case/switch.jpg)

   > [!NOTE]
   > Eine switch-Anweisung benötigt neben dem Standardfall mindestens einen weiteren Fall.

4. Löschen Sie nach der switch-Anweisung die ursprüngliche Datei, die in Dropbox hochgeladen wurde, indem Sie diese Aktion hinzufügen: **Dropbox – Datei löschen**.

5. Speichern Sie Ihre Logik-App. Testen Sie die App durch Hochladen einer Datei in Dropbox. Sie sollten in Kürze eine Genehmigungs-E-Mail erhalten. Wählen Sie eine Option aus, und beobachten Sie das Verhalten.

   > [!TIP]
   > Lesen Sie, wie Sie [Logik-Apps überwachen](logic-apps-monitor-your-logic-apps.md) können.

## <a name="understand-the-code-behind-switch-statements"></a>Verstehen des Codes, der switch-Anweisungen zugrunde liegt

Da Sie nun erfolgreich eine Logik-App mit einer switch-Anweisung erstellt haben, werden wir den Code betrachten, der einer switch-Anweisung zugrunde liegt.

```json
"Switch": {
    "type": "Switch",
    "expression": "@body('Send_approval_email')?['SelectedOption']",
    "cases": {
        "Case 1" : {
            "case" : "Approved",
            "actions" : {}
        },
        "Case 2" : {
            "case" : "Rejected",
            "actions" : {}
        }
    },
    "default": {
        "actions": {}
    },
    "runAfter": {
        "Send_approval_email": [
            "Succeeded"
        ]
    }
}
```

* `"Switch"` ist der Name der switch-Anweisung. Sie können sie zur besseren Lesbarkeit umbenennen. 
* `"type": "Switch"` gibt an, dass die Aktion eine switch-Anweisung ist. 
* `"expression"` ist in diesem Beispiel die ausgewählte Option der genehmigenden Person und wird weiter unten in der Definition für jeden Fall bewertet. 
* `"cases"` kann eine beliebige Anzahl von Fällen enthalten. Für jeden Fall ist `"Case *"` der Standardname, den Sie zur besseren Lesbarkeit ändern können. 
`"case"` gibt die Fallbezeichnung an, die der switch-Ausdruck für den Vergleich verwendet, und muss ein konstanter und eindeutiger Wert sein. Wenn keiner der Fälle dem switch-Ausdruck entspricht, werden Aktionen unter `"default"` ausgeführt.

## <a name="get-help"></a>Hier erhalten Sie Hilfe

Im [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) können Sie Fragen stellen, Fragen beantworten und sich über die Aktivitäten anderer Azure Logic Apps-Benutzer informieren.

Zur Optimierung von Azure Logic Apps und Connectors können Sie auf der [Benutzerfeedbackwebsite für Azure Logic Apps](http://aka.ms/logicapps-wish) über Ideen abstimmen oder selbst Ideen einreichen.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie [Bedingungen hinzufügen](logic-apps-use-logic-app-features.md).
- Erfahren Sie mehr über [Fehler- und Ausnahmebehandlung](logic-apps-exception-handling.md).
- Entdecken Sie weitere [Funktionalitäten der Workflow-Sprache](logic-apps-author-definitions.md).
