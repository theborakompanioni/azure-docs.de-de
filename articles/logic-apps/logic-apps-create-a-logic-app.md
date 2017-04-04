---
title: Erstellen von Workflows mit Ihrer ersten Azure-Logik-App | Microsoft Dokumentation
description: 'Erste Schritte: Verbinden von Apps und SaaS-Diensten mit Ihrer ersten Logik-App'
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: ce3582b5-9c58-4637-9379-75ff99878dcd
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/25/2017
ms.author: jehollan
translationtype: Human Translation
ms.sourcegitcommit: 6e0ad6b5bec11c5197dd7bded64168a1b8cc2fdd
ms.openlocfilehash: 221f1b9f0985bbaf0553f6ca01f0f048b9976315
ms.lasthandoff: 03/28/2017


---
# <a name="create-a-new-logic-app-connecting-saas-services"></a>Erstellen einer Logik-App zum Verbinden von SaaS-Diensten
In diesem Thema wird veranschaulicht, wie Sie in nur wenigen Minuten in [Azure Logic Apps](logic-apps-what-are-logic-apps.md)einsteigen können. Es werden die Schritte eines einfachen Workflows beschrieben, mit dem Sie interessante Tweets an Ihre E-Mail-Adresse senden können.

Für dieses Szenario benötigen Sie Folgendes:

* Ein Azure-Abonnement
* Ein Twitter-Konto
* Ein Outlook.com- oder Office 365-Outlook-Konto

## <a name="create-a-new-logic-app-to-email-you-tweets"></a>Erstellen einer neuen Logik-App zum Empfangen von Tweets per E-Mail

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an. 

2. Wählen Sie im Menü auf der linken Seite **Neu** > **Enterprise Integration** > **Logik-App**.

    Sie können auch **Neu** wählen und dann im Suchfeld `logic app` eingeben und die EINGABETASTE drücken. Wählen Sie **Logik-App** > **Erstellen**.

3. Geben Sie einen Namen für Ihre Logik-App ein, wählen Sie Ihr Azure-Abonnement aus, erstellen Sie eine Azure-Ressourcengruppe (oder wählen Sie sie aus), wählen Sie einen Standort aus, und klicken Sie auf **Erstellen**.

    Wenn Sie **An Dashboard anheften** wählen, wird die Logik-App nach der Bereitstellung automatisch geöffnet.

4. Beim ersten Öffnen Ihrer Logik-App können Sie eine Vorlage auswählen, um zu beginnen.
Klicken Sie in diesem Fall auf **Blank Logic App** (Leere Logik-App), um die Erstellung von Grund auf durchzuführen. 

5. Als Erstes müssen Sie den Trigger erstellen. Dies ist das Ereignis, mit dem die Logik-App gestartet wird. Suchen Sie im Suchfeld nach **twitter**, und wählen Sie die Option **Wenn ein neuer Tweet gepostet wird**. Melden Sie sich mit dem Benutzernamen und Kennwort für Ihr Twitter-Konto an.

6. Geben Sie anschließend einen Suchbegriff ein, um die Logik-App auszulösen.

   ![Twitter-Suche](media/logic-apps-create-a-logic-app/twittersearch.png)

    Mit der **Häufigkeit** und dem **Intervall** wird bestimmt, wie häufig Ihre Logik-App eine Prüfung auf neue Tweets durchführt und alle Tweets dieses Zeitraums zurückgibt.

7. Wählen Sie **Neuer Schritt** und dann **Aktion hinzufügen** oder **Bedingung hinzufügen**.

    Bei Auswahl von **Aktion hinzufügen** können Sie nach [verfügbaren Connectors](../connectors/apis-list.md) suchen, um eine Aktion auszuwählen. 

8. Suchen Sie im Suchfeld nach **outlook**, und wählen Sie die Option **E-Mail senden**, um über Ihr Outlook-Konto eine E-Mail an eine angegebene E-Mail-Adresse zu senden.

   ![Actions](media/logic-apps-create-a-logic-app/actions.png)

9. Nun müssen Sie die Parameter für die gewünschten E-Mails ausfüllen: 

   ![Parameter](media/logic-apps-create-a-logic-app/parameters.png)

10. Wählen Sie abschließend **Speichern** , um Ihre Logik-App online zu stellen.

## <a name="manage-your-logic-app-after-creation"></a>Verwalten Ihrer Logik-App nach der Erstellung

Ihre Logik-App wird nun ausgeführt. Unter Verwendung des eingegebenen Suchbegriffs wird eine regelmäßige Prüfung auf Tweets durchgeführt. Wenn ein übereinstimmender Tweet gefunden wird, wird eine E-Mail an Sie gesendet. Schließlich wird noch gezeigt, wie Sie die App deaktivieren.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).

2. Klicken Sie im Menü auf der linken Seite auf **Weitere Dienste**. Wählen Sie unter **Enterprise Integration** die Option **Logik-Apps**. Speichern Sie Ihre Logik-App.

    *    Wählen Sie im Menü der Logik-App die Option **Übersicht**, um den Status, den Ausführungsverlauf und allgemeine Informationen zur App anzuzeigen. Falls Sie die gewünschten Daten nicht finden, können Sie in der Befehlsleiste die Option **Aktualisieren** wählen.

    *    Wählen Sie zum Bearbeiten Ihrer App im Menü der Logik-App die Option **Logik-App-Designer**.

    *    Wählen Sie im Menü der Logik-App die Option **Übersicht**, um die App vorübergehend zu deaktivieren. Wählen Sie in der Befehlsleiste die Option **Deaktivieren**.

    *    Wählen Sie im Menü der Logik-App die Option **Übersicht**, um die App zu löschen. 
    Klicken Sie in der Befehlsleiste auf **Löschen**. Geben Sie den Namen der Logik-App ein, und wählen Sie **Löschen**.

In weniger als 5 Minuten haben Sie eine einfache Logik-App eingerichtet, die in der Cloud ausgeführt wird. Weitere Informationen zur Verwendung von Logik-App-Features finden Sie unter [Verwenden von Logik-App-Features]. Informationen zu den Logik-App-Definitionen selbst finden Sie unter [Logik-App-Definitionen erstellen](../logic-apps/logic-apps-author-definitions.md).

<!-- Shared links -->
[Azure portal]: https://portal.azure.com
[Verwenden von Logik-App-Features]: logic-apps-create-a-logic-app.md
