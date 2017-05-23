---
title: "Erstellen Ihres ersten Workflows zwischen Cloud-Apps und Cloud-Diensten – Azure Logic Apps | Microsoft-Dokumentation"
description: "Automatisieren von Geschäftsprozessen für Szenarien mit Systemintegration und Enterprise Application Integration (EAI) durch Erstellen und Ausführen von Workflows in Azure Logic Apps"
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
keywords: "Workflow, Cloud-Apps, Cloud-Dienste, Geschäftsprozesse, Systemintegration, Enterprise Application Integration, EAI"
documentationcenter: 
ms.assetid: ce3582b5-9c58-4637-9379-75ff99878dcd
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/31/2017
ms.author: LADocs; jehollan; estfan
ms.translationtype: Human Translation
ms.sourcegitcommit: c308183ffe6a01f4d4bf6f5817945629cbcedc92
ms.openlocfilehash: 204bf123509729b60b55c306050cef54aa7fecc5
ms.contentlocale: de-de
ms.lasthandoff: 05/17/2017

---

# <a name="create-your-first-logic-app-workflow-to-automate-processes-between-cloud-apps-and-cloud-services"></a>Erstellen Ihres ersten Logik-App-Workflows zur Automatisierung von Prozessen zwischen Cloud-Apps und Cloud-Diensten

Durch die Erstellung und Ausführung von Workflows mit [Azure Logic Apps](logic-apps-what-are-logic-apps.md) können Sie Geschäftsprozesse schneller und einfacher sowie ganz ohne Programmieraufwand automatisieren. In diesem ersten Beispiel erfahren Sie, wie Sie einen einfachen Logik-App-Workflow erstellen, der einen RSS-Feed auf neue Inhalte einer Website überprüft. Falls der Feed der Website neue Elemente enthält, sendet die Logik-App eine E-Mail über ein Outlook- oder Gmail-Konto.

Zum Erstellen und Ausführen einer Logik-App benötigen Sie folgende Elemente:

* Ein Azure-Abonnement. Falls Sie über kein Abonnement verfügen, können Sie [mit einem kostenlosen Azure-Konto beginnen](https://azure.microsoft.com/free/). Sie können sich aber auch [für ein Abonnement mit nutzungsbasierter Bezahlung registrieren](https://azure.microsoft.com/pricing/purchase-options/).

  Die Nutzung von Logik-Apps wird über Ihr Azure-Abonnement abgerechnet. Informieren Sie sich über die [Verwendungsmessung](../logic-apps/logic-apps-pricing.md) und über die [Preise](https://azure.microsoft.com/pricing/details/logic-apps) für Azure Logic Apps.

Für dieses Beispiel wird außerdem Folgendes benötigt:

* Ein Outlook.com-, Office 365 Outlook- oder Gmail-Konto.

    > [!TIP]
    > Besitzer eines privaten [Microsoft-Kontos](https://account.microsoft.com/account) verfügen über ein Outlook.com-Konto. Falls Sie über ein Geschäfts-, Schul- oder Unikonto für Azure verfügen, besitzen Sie ein Konto vom Typ **Office 365 Outlook**.

* Einen Link zum RSS-Feed einer Website. Dieses Beispiel verwendet die [RSS-Feeds für Topmeldungen von der CNN.com-Website](http://rss.cnn.com/rss/cnn_topstories.rss): `http://rss.cnn.com/rss/cnn_topstories.rss`

## <a name="add-a-trigger-that-starts-your-workflow"></a>Hinzufügen eines Triggers zum Starten des Workflows

Ein [*Trigger*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) ist ein Ereignis, das Ihren Logik-App-Workflow startet, und das erste Element, das Ihre Logik-App benötigt.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com "Azure-Portal") an.

2. Wählen Sie im Menü auf der linken Seite **Neu** > **Enterprise Integration** > **Logik-App** aus, wie hier zu sehen:

     ![Azure-Portal, Neu, Enterprise Integration, Logik-App](media/logic-apps-create-a-logic-app/azure-portal-create-logic-app.png)

   > [!TIP]
   > Sie können auch **Neu** wählen und dann im Suchfeld `logic app` eingeben und die EINGABETASTE drücken. Wählen Sie anschließend **Logik-App** > **Erstellen** aus.

3. Benennen Sie Ihre Logik-App, und wählen Sie Ihr Azure-Abonnement aus. Erstellen Sie nun eine Azure-Ressourcengruppe, oder wählen Sie eine aus. Die Azure-Ressourcengruppe erleichtert das Organisieren und Verwalten verwandter Azure-Ressourcen. Wählen Sie abschließend den Datencenterstandort zum Hosten Ihrer Logik-App aus. Wenn alles bereit ist, wählen Sie **An Dashboard anheften** und anschließend **Erstellen** aus.

     ![Logik-App-Details](media/logic-apps-create-a-logic-app/logic-app-settings.png)

   > [!NOTE]
   > Wenn Sie **An Dashboard anheften** auswählen, wird Ihre Logik-App nach der Bereitstellung auf dem der Azure-Dashboard angezeigt und automatisch geöffnet. Sollte Ihre Logik-App nicht auf dem Dashboard angezeigt werden, wählen Sie auf der Kachel **Alle Ressourcen** die Option **Mehr Informationen** und anschließend Ihre Logik-App aus. Oder wählen Sie im Menü auf der linken Seite die Option **Weitere Dienste** aus. Wählen Sie unter **Enterprise Integration** die Option **Logic Apps** und anschließend Ihre Logik-App aus.

4. Wenn Sie Ihre Logik-App zum ersten Mal öffnen, zeigt der Logik-App-Designer Vorlagen an, die Ihnen den Einstieg erleichtern. Wählen Sie in diesem Beispiel **Leere Logik-App** aus, um Ihre Logik-App von Grund auf neu zu erstellen.

    Der Logik-App-Designer wird geöffnet und zeigt die verfügbaren Dienste und *Trigger* an, die Sie in Ihrer Logik-App verwenden können.

5. Geben Sie `RSS` in das Suchfeld ein, und wählen Sie den folgenden Trigger aus: **RSS – Beim Veröffentlichen eines Feedelements**. 

    ![RSS-Trigger](media/logic-apps-create-a-logic-app/rss-trigger.png)

6. Geben Sie den Link für die RSS-Feed der Website ein, den Sie überwachen möchten. 

     Außerdem können Sie die **Häufigkeit** und das **Intervall** ändern. 
     Diese Einstellungen steuern, wie häufig Ihre Logik-App eine Prüfung auf neue Elemente durchführt und alle in diesem Zeitraum gefundenen Elemente zurückgibt.

     In diesem Beispiel soll täglich geprüft werden, ob auf der CNN-Website Topmeldungen veröffentlicht wurden.

     ![Einrichten eines Triggers mit RSS-Feed, Häufigkeit und Intervall](media/logic-apps-create-a-logic-app/rss-trigger-setup.png)

7. Speichern Sie Ihre Arbeit. (Wählen Sie auf der Befehlsleiste des Designers die Option **Speichern** aus.)

   ![Speichern Ihrer Logik-App](media/logic-apps-create-a-logic-app/save-logic-app.png)

   Beim Speichern wird Ihre Logik-App live geschaltet. Momentan prüft Ihre Logik-App allerdings nur, ob der angegebene RSS-Feed neue Elemente enthält. 
   Das ist noch nicht besonders hilfreich. Daher fügen wir als Nächstes eine Aktion hinzu, die von der Logik-App ausgeführt wird, wenn die Bedingung Ihres Triggers erfüllt ist.

## <a name="add-an-action-that-responds-to-your-trigger"></a>Hinzufügen einer Aktion als Reaktion auf den Trigger

Eine [*Aktion*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) ist eine Aufgabe, die von Ihrem Logik-App-Workflow ausgeführt wird. Nachdem Sie Ihrer Logik-App einen Trigger hinzugefügt haben, können Sie eine Aktion zum Ausführen von Vorgängen mit Daten hinzufügen, die von diesem Trigger generiert wurden. In unserem Beispiel fügen wir nun eine Aktion hinzu, die eine E-Mail sendet, wenn der RSS-Feed der Website neue Elemente enthält.

1. Wählen Sie im Designer unter Ihrem Trigger **Neuer Schritt** > **Aktion hinzufügen** aus, wie hier gezeigt:

   ![Hinzufügen einer Aktion](media/logic-apps-create-a-logic-app/add-new-action.png)

   Der Designer zeigt [verfügbare Connectors](../connectors/apis-list.md) an, damit Sie eine Aktion auswählen können, die ausgeführt werden soll, wenn Ihr Trigger ausgelöst wird.

2. Führen Sie abhängig von Ihrem E-Mail-Konto die Schritte für Outlook oder für Gmail aus.

   * Wenn Sie eine E-Mail über Ihr Outlook-Konto senden möchten, geben Sie `outlook` in das Suchfeld ein. Wählen Sie unter **Dienste** die Option **Outlook.com** (privates Microsoft-Konto) oder die Option **Office 365 Outlook** (Geschäfts-, Schul- oder Unikonto für Azure) aus. 
   Wählen Sie unter **Aktionen** die Option **E-Mail senden** aus.

       ![Auswählen der Aktion „E-Mail senden“ für Outlook](media/logic-apps-create-a-logic-app/actions.png)

   * Wenn Sie eine E-Mail über Ihr Gmail-Konto senden möchten, geben Sie `gmail` in das Suchfeld ein. 
   Wählen Sie unter **Aktionen** die Option **E-Mail senden** aus.

       ![Auswählen von „Gmail – E-Mail senden“](media/logic-apps-create-a-logic-app/actions-gmail.png)

3. Wenn Sie zur Eingabe von Anmeldeinformationen aufgefordert werden, melden Sie sich mit dem Benutzernamen und dem Kennwort für Ihr E-Mail-Konto an. 

4. Geben Sie Details für diese Aktion (etwa die Ziel-Adresse für die E-Mail) an, und wählen Sie die Parameter für die Daten aus, die in der E-Mail enthalten sein sollen. Beispiel:

   ![Auswählen der Daten, die die E-Mail enthalten soll](media/logic-apps-create-a-logic-app/rss-action-setup.png)

    Bei Verwendung von Outlook sieht Ihre Logik-App in etwa wie im folgenden Beispiel aus:

    ![Fertige Logik-App](media/logic-apps-create-a-logic-app/save-run-complete-logic-app.png)

5.    Speichern Sie die Änderungen. (Wählen Sie auf der Befehlsleiste des Designers die Option **Speichern** aus.)

6. Zu Testzwecken können Sie Ihre Logik-App nun manuell ausführen. Wählen Sie auf der Befehlsleiste des Designers die Option **Ausführen** aus. Ansonsten kann Ihre Logik-App den angegebenen RSS-Feed auch gemäß dem eingerichteten Zeitplan überprüfen.

   Wenn Ihre Logik-App neue Elemente findet, sendet sie eine E-Mail mit den von Ihnen ausgewählten Daten. 
   Werden keine neuen Elemente gefunden, überspringt Ihre Logik-App die Aktion für den E-Mail-Versand.

7. Überwachungsinformationen sowie den Ausführungs- und Triggerverlauf Ihrer Logik-App finden Sie in Ihrem Logik-App-Menü unter **Übersicht**.

   ![Überwachen und Anzeigen des Ausführungs- und Triggerverlaufs Ihrer Logik-App](media/logic-apps-create-a-logic-app/logic-app-run-trigger-history.png)

   > [!TIP]
   > Sollten Sie nicht die Daten finden, die Sie erwarten, wählen Sie auf der Befehlsleiste die Option **Aktualisieren** aus.

   Weitere Informationen zum Status oder zum Ausführungs- und Triggerverlauf Ihrer Logik-App sowie Informationen zum Diagnostizieren Ihrer Logik-App finden Sie unter [Diagnostizieren von Fehlern bei Logik-Apps](logic-apps-diagnosing-failures.md).

      > [!NOTE]
      > Ihre Logik-App wird weiter ausgeführt, bis Sie sie deaktivieren. Wählen Sie zum Deaktivieren Ihrer App in Ihrem Logik-App-Menü die Option **Übersicht** aus. Wählen Sie in der Befehlsleiste die Option **Deaktivieren**.

Herzlichen Glückwunsch! Sie haben soeben Ihre erste einfache Logik-App eingerichtet und ausgeführt. Außerdem haben Sie gelernt, wie einfach Sie Workflows zur Automatisierung von Prozessen erstellen und Cloud-Apps und Cloud-Dienste integrieren können – und das alles ganz ohne Programmieraufwand.

## <a name="manage-your-logic-app"></a>Verwalten Ihrer Logik-App

Zur Verwaltung Ihrer App können Sie beispielsweise den Status überprüfen, die Logik-App bearbeiten, ihren Verlauf anzeigen und die Logik-App deaktivieren oder löschen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com "Azure-Portal") an.

2. Wählen Sie im Menü auf der linken Seite die Option **Weitere Dienste** aus. Wählen Sie unter **Enterprise Integration** die Option **Logik-Apps**. Speichern Sie Ihre Logik-App. 

   Im Logik-App-Menü stehen folgende Verwaltungsaufgaben für die Logik-App zur Verfügung:

   |Aufgabe|Schritte| 
   |:---|:---| 
   | Anzeigen von Status, Ausführungsverlauf und allgemeinen Informationen zu Ihrer App| Wählen Sie **Übersicht** aus.| 
   | Bearbeiten Ihrer App | Wählen Sie **Logik-App-Designer** aus. | 
   | Anzeigen der JSON-Definition Ihres App-Workflows | Wählen Sie **Logik-App-Codeansicht** aus. | 
   | Anzeigen der Vorgänge, die von Ihrer Logik-App ausgeführt wurden | Wählen Sie **Aktivitätsprotokoll** aus. | 
   | Anzeigen älterer Versionen für Ihre Logik-App | Wählen Sie **Versionen** aus. | 
   | Vorübergehendes Deaktivieren Ihrer App | Wählen Sie **Übersicht** und anschließend auf der Befehlsleiste die Option **Deaktivieren** aus. | 
   | Löschen Ihrer App | Wählen Sie **Übersicht** und anschließend auf der Befehlsleiste die Option **Löschen** aus. Geben Sie den Namen der Logik-App ein, und wählen Sie **Löschen**. | 

## <a name="get-help"></a>Hier erhalten Sie Hilfe

Im [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) können Sie Fragen stellen, Fragen beantworten und sich über die Aktivitäten anderer Azure Logic Apps-Benutzer informieren.

Zur Optimierung von Azure Logic Apps und Connectors können Sie auf der [Benutzerfeedbackwebsite für Azure Logic Apps](http://aka.ms/logicapps-wish) über Ideen abstimmen oder selbst Ideen einreichen.

## <a name="next-steps"></a>Nächste Schritte

*  [Hinzufügen von Bedingungen und Ausführen von Workflows](../logic-apps/logic-apps-use-logic-app-features.md)
*     [Logik-App-Vorlagen](../logic-apps/logic-apps-use-logic-app-templates.md)
*  [Erstellen einer Logik-App mithilfe einer Vorlage](../logic-apps/logic-apps-arm-provision.md)

