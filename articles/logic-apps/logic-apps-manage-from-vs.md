---
title: Verwalten von Azure Logic Apps im Cloud-Explorer von Visual Studio | Microsoft-Dokumentation
description: Verwalten von Azure Logic Apps im Cloud-Explorer von Visual Studio
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
ms.date: 12/19/2016
ms.author: klam
translationtype: Human Translation
ms.sourcegitcommit: a9f786259676a1bc9aa616f2db2935dff45475e2
ms.openlocfilehash: dcb0512b4f9e6c94262a3f39ef59f82fcfbf313e


---
# <a name="manage-logic-apps-with-the-visual-studio-cloud-explorer"></a>Verwalten von Azure Logic Apps mit dem Cloud-Explorer von Visual Studio
Das [Azure-Portal](https://portal.azure.com) bietet Ihnen zwar eine hervorragende Möglichkeit zum Entwerfen und Verwalten Ihrer Logik-Apps, der Cloud-Explorer von Visual Studio ermöglicht Ihnen jedoch die Verwaltung vieler Ihrer Azure-Ressourcen in Visual Studio, einschließlich Logik-Apps.  Mit dem Cloud-Explorer können Sie veröffentlichte Logik-Apps durchsuchen, Aktionen wie Aktivieren, Deaktivieren und Bearbeiten ausführen sowie Ausführungsverläufe anzeigen. 

## <a name="installation-steps"></a>Installationsschritte
Unten sind die Schritte zum Installieren und Konfigurieren der Visual Studio-Tools für Logik-Apps angegeben.

### <a name="prerequisites"></a>Voraussetzungen
* [Visual Studio 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
* [Aktuelles Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 oder höher)
* [Cloud-Explorer von Visual Studio](https://marketplace.visualstudio.com/items?itemName=MicrosoftCloudExplorer.CloudExplorerforVisualStudio2015)
* Internetzugriff bei Verwendung des eingebetteten Designers

### <a name="install-visual-studio-tools-for-logic-apps"></a>Installieren der Visual Studio-Tools für Logik-Apps
Gehen Sie nach der Installation der erforderlichen Komponenten wie folgt vor: 

1. Öffnen Sie in Visual Studio 2015 das Menü **Extras**, und wählen Sie **Erweiterungen und Updates** aus.
2. Wählen Sie die Kategorie **Online** für die Onlinesuche aus.
3. Suchen Sie nach **Logic Apps**, um die **Azure Logic Apps-Tools für Visual Studio** anzuzeigen.
4. Klicken Sie auf die Schaltfläche **Herunterladen** , um die Erweiterung herunterzuladen und zu installieren.
5. Starten Sie Visual Studio nach der Installation neu.

> [!NOTE]
> Sie können die Erweiterung auch direkt über [diesen Link](https://visualstudiogallery.msdn.microsoft.com/e25ad307-46cf-412e-8ba5-5b555d53d2d9)
> 
> 

## <a name="browsing-logic-apps"></a>Durchsuchen von Logic Apps
Zum Durchsuchen von Logik-Apps im Cloud-Explorer öffnen Sie den Cloud-Explorer unter „Ansicht > Cloud-Explorer“. Sie können anhand der Ressourcengruppe oder des Ressourcentyps suchen.  Wenn Sie nach Ressourcentyp durchsuchen, wählen Sie ein Abonnement aus, erweitern den Logik-Apps-Bereich und wählen dann eine Logik-App aus.  Sie können entweder mit der rechten Maustaste auf eine Ihrer Logik-Apps klicken oder das Menü „Aktionen“ im unteren Bereich des Cloud-Explorers verwenden, um die gewünschte Aktion auszuführen.

![Durchsuchen](./media/logic-apps-manage-from-vs/browse.png)

## <a name="edit-logic-app-with-the-designer"></a>Bearbeiten einer Logik-App mit dem Designer
Zum Öffnen der derzeit bereitgestellten Logik-App im gleichen Designer wie im Azure-Portal klicken Sie mit der rechten Maustaste auf die Logik-App und wählen „Mit Logik-App-Editor öffnen“.  Mit dem Designer können Sie die Logik-App bearbeiten, wieder in der Cloud speichern und mit der Schaltfläche „Trigger ausführen“ eine neue Ausführung starten.

![Designer](./media/logic-apps-manage-from-vs/designer.png)

## <a name="browse-logic-app-run-history"></a>Durchsuchen des Ausführungsverlauf einer Logik-App
Zum Auflisten des Ausführungsverlaufs für eine Logik-App klicken Sie mit der rechten Maustaste auf die Logik-App, und wählen Sie „Ausführungsverlauf öffnen“.  Diese Ansicht können Sie nach jeder der angezeigten Eigenschaften anordnen, indem Sie die entsprechende Spaltenüberschrift auswählen.  

![Ausführungen](media/logic-apps-manage-from-vs/runs.png)

Durch Doppelklicken auf eine der ausgeführten Instanzen wird der Ausführungsverlauf für diese Instanz mit den Ergebnissen der Ausführung angezeigt, einschließlich der Eingaben und Ausgaben der einzelnen Schritte.

![Verlauf](./media/logic-apps-manage-from-vs/history.png)

## <a name="next-steps"></a>Nächste Schritte
* Befolgen Sie zum Einstieg in Logik-Apps das Lernprogramm [Erstellen einer Logik-App](logic-apps-create-a-logic-app.md) .  
* [Anzeigen allgemeiner Beispiele und Szenarien](logic-apps-examples-and-scenarios.md)
* [Sie können Geschäftsprozesse mit Logik-Apps automatisieren.](http://channel9.msdn.com/Events/Build/2016/T694) 
* [Erfahren Sie, wie Sie Ihre Systeme in Logik-Apps integrieren.](http://channel9.msdn.com/Events/Build/2016/P462)




<!--HONumber=Jan17_HO3-->


