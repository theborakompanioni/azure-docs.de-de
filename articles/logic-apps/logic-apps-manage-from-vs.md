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
ms.author: klam
translationtype: Human Translation
ms.sourcegitcommit: 2fafb3c127183c4903ed7ee0da5811988db9116c
ms.openlocfilehash: 9a542f83eb35b159de04f0395cd850376da498de
ms.lasthandoff: 03/02/2017

---

# <a name="manage-your-logic-apps-with-visual-studio-cloud-explorer"></a>Verwalten von Logik-Apps mit dem Cloud-Explorer von Visual Studio

Das [Azure-Portal](https://portal.azure.com/) bietet Ihnen zwar eine hervorragende Möglichkeit zum Entwerfen und zum Verwalten von Azure Logic Apps, Sie können jedoch viele Azure-Ressourcen in Visual Studio, einschließlich Logik-Apps, auch mit dem Cloud-Explorer von Visual Studio verwalten. Sie können veröffentlichte Logik-Apps durchsuchen, Aktionen wie Aktivieren und Deaktivieren von Logik-Apps ausführen sowie Ausführungsverläufe anzeigen und bearbeiten. 

## <a name="installation-steps"></a>Installationsschritte

Führen Sie zum Installieren und Konfigurieren der Visual Studio-Tools für Azure Logic Apps die folgenden Schritte aus.

### <a name="prerequisites"></a>Voraussetzungen

* [Visual Studio 2015 oder Visual Studio 2017](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
* [Aktuelles Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 oder höher)
* [Cloud-Explorer von Visual Studio](https://marketplace.visualstudio.com/items?itemName=MicrosoftCloudExplorer.CloudExplorerforVisualStudio2015)
* Internetzugriff bei Verwendung des eingebetteten Designers

### <a name="install-visual-studio-tools-for-logic-apps"></a>Installieren der Visual Studio-Tools für Logik-Apps

Gehen Sie nach der Installation der erforderlichen Komponenten wie folgt vor:

1. Öffnen Sie Visual Studio. Wählen Sie im Menü **Extras** die Option **Erweiterungen und Updates** aus.
2. Erweitern Sie die Kategorie **Online**, um eine Onlinesuche durchzuführen.
3. Navigieren Sie zu **Logic Apps** (oder suchen Sie danach), um **Azure Logic Apps-Tools für Visual Studio** zu finden.
4. Klicken Sie auf **Herunterladen**, um die Erweiterung herunterzuladen und zu installieren.
5. Starten Sie Visual Studio nach der Installation neu.

> [!NOTE]
> Die Azure Logic Apps-Tools für Visual Studio können auch direkt über den [Visual Studio Marketplace](https://visualstudiogallery.msdn.microsoft.com/e25ad307-46cf-412e-8ba5-5b555d53d2d9) heruntergeladen werden.

## <a name="browse-for-logic-apps-in-cloud-explorer"></a>Suchen nach Logik-Apps im Cloud-Explorer

1.    Wählen Sie zum Öffnen des Cloud-Explorers im Menü **Ansicht** die Option **Cloud-Explorer**.
2.    Suchen Sie entweder anhand von Ressourcengruppe oder Ressourcentyp nach Ihrer Logik-App. 

    Wenn Sie anhand des Ressourcentyps suchen, wählen Sie Ihre Azure-Abonnement aus, erweitern Sie den Logic Apps-Bereich, und wählen Sie dann eine Logik-App aus. 
    Sie können entweder mit der rechten Maustaste auf eine Logik-App klicken oder eine Option im Menü **Aktionen** im unteren Bereich des Cloud-Explorers auswählen.

    ![Suchen nach einer Logik-App](./media/logic-apps-manage-from-vs/browse.png)

## <a name="edit-your-logic-app-with-logic-app-designer"></a>Bearbeiten Ihrer Logik-App mit Logik-App-Designer

Zum Öffnen einer derzeit bereitgestellten Logik-App im gleichen Designer wie im Azure-Portal klicken Sie mit der rechten Maustaste auf Ihre Logik-App und wählen **Mit Logik-App-Editor öffnen**. 

Im Designer können Sie Ihre Logik-App bearbeiten, Updates in der Cloud speichern und mit **Trigger ausführen** eine neue Ausführung starten.

![Logik-App-Designer](./media/logic-apps-manage-from-vs/designer.png)

## <a name="browse-your-logic-app-run-history"></a>Durchsuchen des Ausführungsverlauf einer Logik-App

Zum Anzeigen des Ausführungsverlaufs für Ihre Logik-App klicken Sie mit der rechten Maustaste auf die Logik-App, und wählen Sie **Ausführungsverlauf öffnen**. Klicken Sie auf die Spaltenüberschrift, um den Ausführungsverlauf basierend auf einer der angezeigten Eigenschaften neu anzuordnen.

![Ausführungsverlauf](media/logic-apps-manage-from-vs/runs.png)

Um den Ausführungsverlauf für eine Instanz und damit die Ausführungsergebnisse, einschließlich der Eingabe und Ausgabe der einzelnen Schritte anzuzeigen, doppelklicken Sie auf eine der ausgeführten Instanzen.

![Ergebnisse des Ausführungsverlaufs, Eingaben und Ausgaben der Schritte](./media/logic-apps-manage-from-vs/history.png)

## <a name="next-steps"></a>Nächste Schritte

*    Eine Einführung in Azure Logic Apps finden Sie im Thema zum [Erstellen Ihrer ersten Logik-App über das Azure-Portal](logic-apps-create-a-logic-app.md).
* [Design, build, and deploy logic apps in Visual Studio](logic-apps-deploy-from-vs.md) (Entwerfen, Erstellen und Bereitstellen von Logik-Apps in Visual Studio)
* [Anzeigen allgemeiner Beispiele und Szenarien](logic-apps-examples-and-scenarios.md)
* [Automatisieren von Geschäftsprozessen mit Azure Logic Apps](http://channel9.msdn.com/Events/Build/2016/T694)
* [Integrieren Ihrer Systeme in Azure Logic Apps](http://channel9.msdn.com/Events/Build/2016/P462)

