---
title: Verwenden der switch-Anweisung in Azure Logic Apps | Microsoft-Dokumentation
description: "Mit der switch-Anweisung können Sie anhand des Werts eines Ausdrucks in Logic Apps ganz einfach verschiedene Aktionen ausführen."
services: logic-apps
documentationcenter: dev-center-name
manager: erikre
ms.service: logic-apps
ms.devlang: wdl
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: deli
translationtype: Human Translation
ms.sourcegitcommit: 3a028507f9bbf15c8fd52ccc7c22a5763a9b1b3e
ms.openlocfilehash: 284cfca17b5abf785f7af9569c518c4400fe36fd


---
# <a name="use-switch-statement-in-logic-apps"></a>Verwenden der switch-Anweisung in Logic Apps
Beim Erstellen eines Workflows müssen Sie häufig verschiedene Aktionen ausführen, die auf dem Wert eines Objekts oder Ausdrucks basieren. Beispielsweise soll sich ihre Logic App abhängig vom Statuscode einer HTTP-Anforderung oder der gewählten Option einer Genehmigungs-E-Mail anders verhalten.

Diese Szenarien erreichen Sie mit einer switch-Anweisung: Die Logik-App wertet ein Token oder einen Ausdruck aus und wählt den Fall mit dem gleichen Wert, um Aktionen darin auszuführen. Nur ein Fall sollte mit der switch-Anweisung übereinstimmen.

 > [!TIP]
 > Wie alle Programmiersprachen unterstützt die switch-Anweisung nur Gleichheitsoperatoren. Verwenden Sie eine Bedingungsanweisung, wenn Sie andere relationale Operatoren (z. B. größer als) benötigen.
 >
 > Um deterministisches Ausführungsverhalten zu gewährleisten, müssen Fälle einen eindeutigen und statischen Wert statt eines dynamischen Tokens oder Ausdrucks enthalten.

## <a name="prerequisites"></a>Voraussetzungen

- Ein aktives Azure-Abonnement.
    - Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen oder [Logic Apps kostenlos](https://tryappservice.azure.com/) testen.
- [Grundlegende Kenntnisse zu Logic Apps](logic-apps-what-are-logic-apps.md).

## <a name="working-with-switch-statement-in-designer"></a>Arbeiten mit der switch-Anweisung im Designer
Um die Verwendung der switch-Anweisung zu demonstrieren, erstellen wir eine Logic App, die in Dropbox hochgeladene Dateien überwacht. Die Logic App wird eine Genehmigungs-E-Mail senden, um festzustellen, ob sie in SharePoint übertragen werden sollen. Wir verwenden die switch-Anweisung, um abhängig vom Wert der genehmigenden Person verschiedene Aktionen auszuführen.

1. Beginnen Sie mit dem Erstellen einer Logic App, indem Sie den Trigger **Dropbox - When a file is created** (Dropbox – Wenn eine Datei erstellt wird) wählen.

 ![Verwenden des Triggers „Dropbox - When a file is created“ (Dropbox – Wenn eine Datei erstellt wird)](./media/logic-apps-switch-case/dropbox-trigger.jpg)

2. Verwenden Sie die Aktion **Outlook.com - Send approval email** (Outlook.com – Genehmigungs-E-Mail senden) zum Nachverfolgen des Triggers.

 > [!TIP]
 > Logic Apps unterstützt auch das Szenario der E-Mail-Genehmigung über ein Office 365 Outlook-Konto.

 - Wenn es noch keine Verbindung gibt, werden Sie aufgefordert, eine zu erstellen.
 - Füllen Sie alle Pflichtfelder aus. Wir senden ein E-Mail an approvers@contoso.com.
 - Geben Sie unter *Benutzeroptionen* die Option `Approve, Reject` ein.

 ![Konfigurieren der Verbindung](./media/logic-apps-switch-case/send-approval-email-action.jpg)

3. Fügen Sie eine switch-Anweisung hinzu.
 - Wählen Sie **+ Neuer Schritt**, **... More** (Mehr), **Add a switch statement** (switch-Anweisung hinzufügen) aus.
 - Wir möchten den auszuführenden Vorgang basierend auf der `SelectedOptions`-Ausgabe des Vorgangs *Send approval email* (Genehmigungs-E-Mail senden) auswählen, den Sie in der Auswahl **Add dynamic content** (Dynamische Inhalte hinzufügen) finden.
 - Verwenden Sie *Fall 1*, wenn der Benutzer `Approve` wählt.
    - Wenn die Genehmigung erfolgt ist, kopieren Sie die Originaldatei mit der Aktion **SharePoint Online - Create file** (SharePoint Online – Datei erstellen) in SharePoint Online.
    - Fügen Sie dem Fall eine weitere Aktion hinzu, um Benutzer auf die Verfügbarkeit einer neuen Datei in SharePoint aufmerksam zu machen.
 - Fügen Sie einen weiteren Fall hinzu, wenn der Benutzer `Reject` wählt.
    - Versenden Sie im Falle einer Ablehnung eine E-Mail-Benachrichtigung, um andere genehmigende Personen zu informieren, dass die Datei abgelehnt wurde und keine weitere Aktion erforderlich ist.
 - Für `SelectedOptions` stehen nur zwei Optionen bereit, der Fall *default* (Standard) kann leer gelassen werden.

 ![switch-Anweisung](./media/logic-apps-switch-case/switch.jpg)

 > [!NOTE]
 > Die switch-Anweisung benötigt neben dem Standardfall mindestens einen weiteren Fall.

4. Löschen Sie nach der Switch-Anweisung die ursprüngliche Datei, die mit der Aktion **Dropbox - Delete File** (Dropbox – Datei löschen) in die Dropbox hochgeladen wurde.

5. Speichern Sie Ihre Logik-App, und testen Sie sie, indem Sie eine Datei in Dropbox hochladen. Kurz danach erhalten Sie eine Genehmigungs-E-Mail, worauf Sie eine Option wählen und das Verhalten beobachten.
 > [!TIP]
 > Lesen Sie, wie Sie [Logik-Apps überwachen](logic-apps-monitor-your-logic-apps.md).

## <a name="understanding-code-behind"></a>Grundlegendes zum Code
Sie haben nun eine Logic App mit einer switch-Anweisung erstellt. Sehen wir uns den Code dazu an.

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

`"Switch"` ist der Name der switch-Anweisung. Er kann zur besseren Lesbarkeit umbenannt werden. `"type": "Switch"` gibt an, dass die Aktion eine switch-Anweisung ist. `"expression"` ist in diesem Fall die ausgewählte Option des Benutzers und wird weiter unten in der Definition für jeden Fall bewertet. `"cases"` kann eine beliebige Anzahl an Fällen enthalten. Wenn keiner der Fälle der switch-Anweisung entspricht, werden Aktionen im Fall `"default"` ausgeführt.

In `"cases"` ist eine beliebige Anzahl an Fällen möglich. In jedem Fall ist `"Case 1"` der Name der switch-Anweisung. Er kann zur besseren Lesbarkeit umbenannt werden. `"case"` gibt die Fallbezeichnung für den Vergleich des switch-Ausdrucks an und muss ein konstanter und eindeutiger Wert sein.  

## <a name="next-steps"></a>Nächste Schritte
- Probieren Sie weitere [Logic App-Features](logic-apps-use-logic-app-features.md) aus.
- Erfahren Sie mehr über [Fehler- und Ausnahmebehandlung](logic-apps-exception-handling.md).
- Entdecken Sie weitere [Funktionalitäten der Workflow-Sprache](logic-apps-author-definitions.md).
- Verfassen Sie einen Kommentar mit Ihren Fragen oder Ihren Feedback, oder [teilen Sie uns mit, wie wir Logic Apps verbessern können](https://feedback.azure.com/forums/287593-logic-apps).


<!--HONumber=Feb17_HO2-->


