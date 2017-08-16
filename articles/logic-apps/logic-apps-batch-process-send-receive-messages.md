---
title: "Batchverarbeitung von Nachrichten als Gruppe oder Sammlung – Azure Logic Apps | Microsoft-Dokumentation"
description: "Senden und Empfangen von Nachrichten für die Batchverarbeitung in Logik-Apps"
keywords: Batch, Batchverarbeitung
author: jonfancey
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/7/2017
ms.author: LADocs; estfan; jonfan
ms.translationtype: HT
ms.sourcegitcommit: 14915593f7bfce70d7bf692a15d11f02d107706b
ms.openlocfilehash: 480ffce5dbe7c25181bb0ba5639de884e98ff4e6
ms.contentlocale: de-de
ms.lasthandoff: 08/10/2017

---

# <a name="send-receive-and-batch-process-messages-in-logic-apps"></a>Versand, Empfang und Batchverarbeitung von Nachrichten in Logik-Apps

Nachrichten können in Gruppen verarbeitet werden. Hierzu können Sie Datenelemente (oder Nachrichten) an einen *Batch* senden und die Elemente dann als Batch verarbeiten. Dieser Ansatz ist hilfreich, wenn Datenelemente auf eine bestimmte Weise gruppiert und gemeinsam verarbeitet werden sollen. 

Logik-Apps, die Elemente als Batch empfangen, können mithilfe des Triggers **Batch** erstellt werden. Logik-Apps, die Elemente an einen Batch senden, können dann mithilfe der Aktion **Batch** erstellt werden.

In diesem Thema erfahren Sie, wie Sie mit folgenden Aufgaben eine Batchverarbeitungslösung erstellen: 

* [Erstellen einer Logik-App, die Elemente empfängt und zu einem Batch zusammenfasst.](#batch-receiver) Diese Logik-App für den Batchempfang gibt den Batchnamen und die Freigabekriterien an, die erfüllt sein müssen, damit die Empfangs-Logik-App die Elemente verarbeitet. 

* [Erstellen einer Logik-App, die Elemente an einen Batch sendet.](#batch-sender) Diese Logik-App für den Batchversand gibt an, wohin Elemente gesendet werden sollen. Bei dem Ziel muss es sich um eine vorhandene Logik-App für den Batchempfang handeln. Darüber hinaus können Sie einen eindeutigen Schlüssel (beispielsweise eine Kundennummer) angeben, um den Zielbatch auf der Grundlage dieses Schlüssels zu unterteilen (partitionieren). Dadurch werden alle Elemente mit diesem Schlüssel zusammengefasst und gemeinsam verarbeitet. 

## <a name="requirements"></a>Anforderungen

Für dieses Beispiel benötigen Sie Folgendes:

* Ein Azure-Abonnement. Falls Sie über kein Abonnement verfügen, können Sie [mit einem kostenlosen Azure-Konto beginnen](https://azure.microsoft.com/free/). Sie können sich aber auch [für ein Abonnement mit nutzungsbasierter Bezahlung registrieren](https://azure.microsoft.com/pricing/purchase-options/).

* Grundlegende Kenntnisse über die [Erstellung von Logik-Apps](../logic-apps/logic-apps-create-a-logic-app.md) 

* Ein E-Mail-Konto bei einem [von Azure Logic Apps unterstützten E-Mail-Anbieter](../connectors/apis-list.md)

<a name="batch-receiver"></a>

## <a name="create-logic-apps-that-receive-messages-as-a-batch"></a>Erstellen von Logik-Apps, die Nachrichten als Batch empfangen

Um Nachrichten an einen Batch senden zu können, müssen Sie mit dem Trigger **Batch** zunächst eine Logik-App für den Batchempfang erstellen. Dadurch können Sie die Logik-App für den Empfang auswählen, wenn Sie die Logik-App für den Versand erstellen. Für den Empfänger werden der Batchname, die Freigabekriterien und weitere Einstellungen angegeben. 

Logik-Apps für den Versand müssen wissen, wohin die Elemente gesendet werden sollen, während Logik-Apps für den Empfang keinerlei Absenderinformationen benötigen.

1. Erstellen Sie über das [Azure-Portal](https://portal.azure.com) eine Logik-App namens „BatchReceiver“. 

2. Fügen Sie im Designer für Logik-Apps den Trigger **Batch** hinzu, um den Workflow Ihrer Logik-App zu starten. Geben Sie im Suchfeld den Begriff „Batch“ als Filter ein. Wählen Sie den Trigger **Batch – Batchnachrichten** aus.

   ![Hinzufügen des Triggers „Batch“](./media/logic-apps-batch-process-send-receive-messages/add-batch-receiver-trigger.png)

3. Geben Sie einen Namen und Freigabekriterien für den Batch an:

   * **Batchname**: Der Name zum Identifizieren des Batchs („TestBatch“ in diesem Beispiel).
   * **Nachrichtenanzahl**: Die Anzahl von Nachrichten, die in einem Batch gesammelt werden sollen, bevor er zur Verarbeitung freigegeben wird („5“ in diesem Beispiel).

   ![Angeben von Details für den Batchtrigger](./media/logic-apps-batch-process-send-receive-messages/receive-batch-trigger-details.png)

4. Fügen Sie eine weitere Aktion hinzu, die eine E-Mail sendet, wenn der Batchtrigger ausgelöst wird. Die Logik-App sendet eine E-Mail, sobald der Batch fünf Elemente enthält.

   1. Wählen Sie unter dem Batchtrigger die Optionen **+ Neuer Schritt** > **Aktion hinzufügen** aus.

   2. Geben Sie im Suchfeld den Begriff „E-Mail“ als Filter ein.
   Wählen Sie basierend auf Ihrem E-Mail-Anbieter einen E-Mail-Connector aus.
   
      Wenn Sie also beispielsweise über ein Geschäfts-, Schul- oder Unikonto verfügen, wählen Sie den Office 365 Outlook-Connector aus. 
      Wenn Sie ein Gmail-Konto besitzen, wählen Sie den Gmail-Connector aus.

   3. Wählen Sie für den Connector die folgende Aktion aus: **{*E-Mail-Anbieter*} - E-Mail senden**

      Beispiel:

      ![Auswählen der Aktion „E-Mail senden“ für Ihren E-Mail-Anbieter](./media/logic-apps-batch-process-send-receive-messages/add-send-email-action.png)

5. Falls Sie noch keine Verbindung für Ihren E-Mail-Anbieter erstellt haben, geben Sie Ihre E-Mail-Anmeldeinformationen für die Authentifizierung ein, wenn Sie dazu aufgefordert werden. Weitere Informationen zum Authentifizieren Ihrer E-Mail-Anmeldeinformationen finden Sie [hier](../logic-apps/logic-apps-create-a-logic-app.md).

6. Legen Sie die Eigenschaften für die soeben hinzugefügte Aktion fest.

   * Geben Sie im Feld **An** die E-Mail-Adresse des Empfängers ein. 
   Zu Testzwecken können Sie hier Ihre eigene E-Mail-Adresse angeben.

   * Wählen Sie im Feld **Betreff** nach Erscheinen der Liste **Dynamischer Inhalt** das Feld **Partitionsname** aus.

     ![Auswählen von „Partitionsname“ in der Liste „Dynamischer Inhalt“](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details.png)

     In einem späteren Abschnitt können Sie einen eindeutigen Partitionsschlüssel angeben, um den Zielbatch in logische Gruppen zu unterteilen, an die Nachrichten gesendet werden können. 
     Jede Gruppe besitzt eine eindeutige Nummer, die durch Logik-App für den Versand generiert wird. 
     Dadurch können Sie einen einzelnen Batch mit mehreren Untergruppen verwenden und die einzelnen Untergruppen jeweils mit einem Namen Ihrer Wahl definieren.

   * Wählen Sie im Feld **Text** nach Erscheinen der Liste **Dynamischer Inhalt** das Feld **Nachrichten-ID** aus.

     ![Auswählen von „Nachrichten-ID“ unter „Text“](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details-for-each.png)

     Da es sich bei der Eingabe für die Aktion „E-Mail senden“ um ein Array handelt, fügt der Designer die Aktion **E-Mail senden** automatisch in eine Schleife vom Typ **For each** ein. 
     Diese Schleife führt die eingeschlossene Aktion für jedes Element im Batch aus. 
     Wenn der Batchtrigger also auf fünf Elemente festgelegt ist, erhalten Sie jeweils fünf E-Mails, wenn der Trigger ausgelöst wird.

7.  Nachdem Sie nun über eine Logik-App für den Batchempfang verfügen, speichern Sie Ihre Logik-App.

    ![Speichern Ihrer Logik-App](./media/logic-apps-batch-process-send-receive-messages/save-batch-receiver-logic-app.png)

<a name="batch-sender"></a>

## <a name="create-logic-apps-that-send-messages-to-a-batch"></a>Erstellen von Logik-Apps, die Nachrichten an einen Batch senden

Erstellen Sie als Nächstes mindestens eine Logik-App, die Elemente an den Batch sendet, der durch die Logik-App für den Empfang definiert wird. Geben Sie für den Absender die Logik-App für den Empfang und den Batchnamen, den Nachrichteninhalt sowie ggf. weitere Einstellungen an. Optional können Sie einen eindeutigen Partitionsschlüssel angeben, um den Batch zu unterteilen und Elemente mit dem entsprechenden Schlüssel zu sammeln.

Logik-Apps für den Versand müssen wissen, wohin die Elemente gesendet werden sollen, während Logik-Apps für den Empfang keinerlei Absenderinformationen benötigen.

1. Erstellen Sie eine weitere Logik-App namens „BatchSender“.

   1. Geben Sie im Suchfeld den Begriff „Wiederholung“ als Filter ein. 
   Wählen Sie den Trigger **Zeitplan: Wiederholung** aus.

      ![Hinzufügen des Triggers „Zeitplan: Wiederholung“](./media/logic-apps-batch-process-send-receive-messages/add-schedule-trigger-batch-receiver.png)

   2. Legen Sie die Häufigkeit und das Ausführungsintervall der Logik-App für den Versand auf einmal pro Minute fest.

      ![Festlegen von Häufigkeit und Intervall für den Wiederholungstrigger](./media/logic-apps-batch-process-send-receive-messages/recurrence-trigger-batch-receiver-details.png)

2. Fügen Sie einen neuen Schritt hinzu, um Nachrichten an einen Batch zu senden.

   1. Wählen Sie unter dem Wiederholungstrigger die Optionen **+ Neuer Schritt** > **Aktion hinzufügen** aus.

   2. Geben Sie im Suchfeld den Begriff „Batch“ als Filter ein. 

   3. Wählen Sie die Aktion **Nachrichten an Batch senden – Logik-App-Workflow mit Batchtrigger auswählen** aus.

      ![Auswählen von „Nachrichten an Batch senden“](./media/logic-apps-batch-process-send-receive-messages/send-messages-batch-action.png)

   4. Wählen Sie nun die zuvor erstellte (und als Aktion angezeigte) Logik-App „BatchReceiver“ aus.

      ![Auswählen der Logik-App „BatchReceiver“](./media/logic-apps-batch-process-send-receive-messages/send-batch-select-batch-receiver.png)

      > [!NOTE]
      > Die Liste enthält außerdem weitere Logik-Apps mit Batchtrigger.

3. Legen Sie die Batcheigenschaften fest.

   * **Batchname**: Der von der Logik-App für den Empfang definierte Batchname. Dieser wird zur Laufzeit überprüft und lautet in diesem Beispiel „TestBatch“.

     > [!IMPORTANT]
     > Lassen Sie den Batchnamen unverändert. Er muss dem Batchnamen entsprechen, der durch die Logik-App für den Empfang angegeben wird.
     > Wenn Sie den Batchnamen ändern, funktioniert die Logik-App für den Versand nicht.

   * **Nachrichteninhalt**: Der zu sendende Nachrichteninhalt. 
   Fügen Sie in diesem Beispiel den folgenden Ausdruck hinzu, um das aktuelle Datum und die aktuelle Uhrzeit in den Nachrichteninhalt einzufügen, den Sie an den Batch senden:

     1. Wenn die Liste **Dynamischer Inhalt** erscheint, wählen Sie **Ausdruck** aus. 
     2. Geben Sie den Ausdruck **utcnow()** ein, und wählen Sie **OK** aus. 

        ![Wählen Sie unter „Nachrichteninhalt“ die Option „Ausdruck“ aus. Geben Sie „utcnow()“ ein.](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-details.png)

4. Richten Sie nun eine Partition für den Batch ein. Wählen Sie in der Aktion „BatchReceiver“ die Option **Erweiterte Optionen anzeigen** aus.

   * **Partitionsname**: Ein optionaler eindeutiger Partitionsschlüssel zur Unterteilung des Zielbatchs. In diesem Beispiel fügen wir einen Ausdruck hinzu, der eine Zufallszahl zwischen eins und fünf generiert.
   
     1. Wenn die Liste **Dynamischer Inhalt** erscheint, wählen Sie **Ausdruck** aus.
     2. Geben Sie den Ausdruck **rand(1,6)** ein.

        ![Einrichten einer Partition für den Zielbatch](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-partition-advanced-options.png)

        Diese Funktion vom Typ **rand** generiert eine Zahl zwischen eins und fünf. 
        Der Batch wird also in fünf nummerierte Partitionen unterteilt, die dynamisch durch diesen Ausdruck festgelegt werden.

   * **Nachrichten-ID**: Eine optionale Nachrichten-ID (eine generierte GUID, wenn nichts angegeben ist). 
   Lassen Sie das Feld in diesem Beispiel leer.

5. Speichern Sie Ihre Logik-App. Ihre Logik-App für den Versand sieht nun in etwa wie folgt aus:

   ![Speichern Ihrer Logik-App für den Versand](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-details-finished.png)

## <a name="test-your-logic-apps"></a>Testen Ihrer Logik-Apps

Lassen Sie Ihre Logik-Apps zum Testen Ihrer Batchverarbeitungslösung einige Minuten laufen. In Kürze erhalten Sie E-Mails in Fünfergruppen, die jeweils über den gleichen Partitionsschlüssel verfügen.

Die Logik-App „BatchSender“ wird einmal pro Minute ausgeführt. Sie generiert eine Zufallszahl zwischen eins und fünf und verwendet diese generierte Zahl als Partitionsschlüssel für den Zielbatch, an den Nachrichten gesendet werden. Sobald der Batch fünf Elemente mit dem gleichen Partitionsschlüssel enthält, wird die Logik-App „BatchReceiver“ ausgelöst und sendet pro Nachricht jeweils eine E-Mail.

> [!IMPORTANT]
> Vergessen Sie am Ende Ihres Tests nicht, die Logik-App „BatchSender“ zu deaktivieren, um zu verhindern, dass die Logik-App weiterhin Nachrichten sendet und dadurch Ihren Posteingang überlädt.

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen von Workflowdefinitionen für Logik-Apps per JSON-Code](../logic-apps/logic-apps-author-definitions.md)
* [Erstellen einer serverlosen App mit Azure Logic Apps und Functions](../logic-apps/logic-apps-serverless-get-started-vs.md)
* [Ausnahmebehandlung und Fehlerprotokollierung für Logik-Apps](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)

