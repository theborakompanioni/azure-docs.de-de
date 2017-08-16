---
title: "Verwalten von Logik-Apps in Visual Studio – Azure Logic Apps | Microsoft-Dokumentation"
description: Verwalten von Logik-Apps und anderen Azure-Ressourcen mit dem Cloud-Explorer von Visual Studio
author: klam
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 12/19/2016
ms.author: LADocs; klam
ms.translationtype: HT
ms.sourcegitcommit: 9633e79929329470c2def2b1d06d95994ab66e38
ms.openlocfilehash: a5bf24de1a7a2b6d4c1ae6416c95d83ef7506da3
ms.contentlocale: de-de
ms.lasthandoff: 08/04/2017

---

# <a name="manage-your-logic-apps-with-visual-studio-cloud-explorer"></a>Verwalten von Logik-Apps mit dem Cloud-Explorer von Visual Studio

Das [Azure-Portal](https://portal.azure.com/) bietet Ihnen zwar eine hervorragende Möglichkeit zum Entwerfen und zum Verwalten von Azure Logic Apps, aber Sie können den Cloud-Explorer von Visual Studio nutzen, um viele Azure-Ressourcen zu verwalten, z.B. Logik-Apps. Mit dem Cloud-Explorer von Visual Studio können Sie veröffentlichte Logik-Apps durchsuchen, verwalten, bearbeiten und herunterladen. Zu den Verwaltungsaufgaben zählt auch das Aktivieren, Deaktivieren und Anzeigen des Ausführungsverlaufs. 

Bevor Sie auf Ihre Logik-Apps in Visual Studio zugreifen und sie verwalten können, sollten Sie diese Azure Logic Apps-Tools für Visual Studio installieren und konfigurieren. 

## <a name="prerequisites"></a>Voraussetzungen

* [Visual Studio 2015 oder Visual Studio 2017](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
* [Aktuelles Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 oder höher)
* [Cloud-Explorer von Visual Studio](https://marketplace.visualstudio.com/items?itemName=MicrosoftCloudExplorer.CloudExplorerforVisualStudio2015)
* Internetzugriff bei Verwendung des eingebetteten Designers

## <a name="install-visual-studio-tools-for-logic-apps"></a>Installieren der Visual Studio-Tools für Logik-Apps

Laden Sie die Azure Logic Apps-Tools für Visual Studio herunter, und installieren Sie sie, nachdem Sie die erforderlichen Komponenten installiert haben.

1. Öffnen Sie Visual Studio. Wählen Sie im Menü **Extras** die Option **Erweiterungen und Updates** aus.
2. Erweitern Sie die Kategorie **Online**, damit Sie online in der Visual Studio Gallery suchen können.
3. Navigieren Sie zu **Logic Apps** (oder suchen Sie danach), um **Azure Logic Apps-Tools für Visual Studio** zu finden.
4. Klicken Sie auf **Herunterladen**, um die Erweiterung herunterzuladen und zu installieren.
5. Starten Sie Visual Studio nach der Installation neu.

> [!NOTE]
> Navigieren Sie zum direkten Herunterladen der Azure Logic Apps-Tools für Visual Studio zum [Visual Studio Marketplace](https://visualstudiogallery.msdn.microsoft.com/e25ad307-46cf-412e-8ba5-5b555d53d2d9).

## <a name="browse-for-logic-apps-in-cloud-explorer"></a>Suchen nach Logik-Apps im Cloud-Explorer

1.  Wählen Sie zum Öffnen des Cloud-Explorers im Menü **Ansicht** die Option **Cloud-Explorer**.
2.  Suchen Sie entweder anhand von Ressourcengruppe oder Ressourcentyp nach Ihrer Logik-App. 

    * Gehen Sie wie folgt vor, wenn Sie anhand des Ressourcentyps suchen: Wählen Sie Ihr Azure-Abonnement aus, erweitern Sie den Abschnitt **Logik-Apps**, und wählen Sie dann Ihre Logik-App aus. 
    * Beim Suchen anhand der Ressourcengruppe: Erweitern Sie die Ressourcengruppe, die Ihre Logik-App enthält, und wählen Sie Ihre Logik-App aus.

    Klicken Sie zum Anzeigen von Befehlen für Ihre Logik-App entweder mit der rechten Maustaste auf die Logik-App, oder wählen Sie unten im Cloud-Explorer im Menü **Aktionen** die gewünschte Option.

    ![Suchen nach einer Logik-App](./media/logic-apps-manage-from-vs/browse.png)

## <a name="edit-your-logic-app-with-logic-apps-designer"></a>Bearbeiten Ihrer Logik-App mit dem Designer für Logik-Apps

Im Cloud-Explorer können Sie eine derzeit bereitgestellte Logik-App in demselben Designer öffnen, den Sie im Azure-Portal verwenden. 

* Klicken Sie zum Bearbeiten Ihrer Logik-App im Cloud-Explorer mit der rechten Maustaste auf Ihre Logik-App, und wählen Sie **Mit Logik-App-Editor öffnen**. 

* Wählen Sie zum Veröffentlichen Ihrer Updates in der Cloud die Option **Veröffentlichen**. 

* Wählen Sie **Trigger ausführen**, um eine neue Ausführung zu starten.

![Designer für Logik-Apps](./media/logic-apps-manage-from-vs/designer.png)

Mit dem Designer können Sie für eine Logik-App auch einen **Download** durchführen. Bei dieser Aktion wird die Logik-App-Definition automatisch parametrisiert und als Vorlage für die Azure Resource Manager-Bereitstellung gespeichert. Sie können diese Bereitstellungsvorlage Ihrem Azure-Ressourcengruppenprojekt hinzufügen.

## <a name="browse-your-logic-app-run-history"></a>Durchsuchen des Ausführungsverlauf einer Logik-App

Zum Anzeigen des Ausführungsverlaufs für Ihre Logik-App klicken Sie mit der rechten Maustaste auf die Logik-App, und wählen Sie **Ausführungsverlauf öffnen**. Klicken Sie auf die Spaltenüberschrift, um den Ausführungsverlauf basierend auf einer der angezeigten Eigenschaften neu anzuordnen.

![Ausführungsverlauf](media/logic-apps-manage-from-vs/runs.png)

Um den Ausführungsverlauf für eine Instanz und damit die Ausführungsergebnisse, einschließlich der Eingabe und Ausgabe der einzelnen Schritte, zum Prüfen anzuzeigen, doppelklicken Sie auf eine der ausgeführten Instanzen.

![Ergebnisse des Ausführungsverlaufs, Eingaben und Ausgaben der Schritte](./media/logic-apps-manage-from-vs/history.png)

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen Ihrer ersten Logik-App](logic-apps-create-a-logic-app.md)
* [Design, build, and deploy logic apps in Visual Studio](logic-apps-deploy-from-vs.md) (Entwerfen, Erstellen und Bereitstellen von Logik-Apps in Visual Studio)
* [Anzeigen allgemeiner Beispiele und Szenarien](logic-apps-examples-and-scenarios.md)
* [Video: Automatisieren von Geschäftsprozessen mit Azure Logic Apps](http://channel9.msdn.com/Events/Build/2016/T694)
* [Video: Integrieren Ihrer Systeme in Azure Logic Apps](http://channel9.msdn.com/Events/Build/2016/P462)

