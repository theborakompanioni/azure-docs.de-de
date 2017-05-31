---
title: Erstellen einer serverlosen App in Visual Studio | Microsoft-Dokumentation
description: Erste Schritte mit Ihrer ersten serverlosen App beim Erstellen, Bereitstellen und Verwalten der App in Visual Studio.
keywords: 
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: LADocs; jehollan
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: 4ed26f1a30501d9ce67845795606f6d8e77ac53f
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017

---
# <a name="build-a-serverless-app-in-visual-studio-with-logic-apps-and-functions"></a>Erstellen Sie eine serverlose App mit Logik-Apps und Functions in Visual Studio

Mit serverlosen Tools und Functions in Azure können Cloudanwendungen schnell entwickelt und bereitgestellt werden.  Dieses Dokument konzentriert sich darauf, wie Sie damit beginnen können, in Visual Studio eine serverlose Anwendung zu erstellen.  Eine Übersicht über serverlose Funktionen in Azure [finden Sie in diesem Artikel](logic-apps-serverless-overview.md).

## <a name="getting-everything-ready"></a>Voraussetzungen

Für das Erstellen von serverlosen Anwendungen in Visual Studio müssen die folgenden Voraussetzungen erfüllt sein:

* [Visual Studio 2017](https://www.visualstudio.com/vs/) oder Visual Studio 2015 - Community, Professional oder Enterprise
* [Logik-Apps-Tools für Visual Studio](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551)
* [Aktuelles Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 oder höher)
* [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)
* [Azure Functions-CLI](https://www.npmjs.com/package/azure-functions-cli), um Functions lokal debuggen zu können
* Internetzugriff bei Verwendung des eingebetteten Logik-App-Designers

## <a name="getting-started-with-a-deployment-template"></a>Erste Schritte mit einer Bereitstellungsvorlage

Ressourcen werden in Azure innerhalb von Ressourcengruppen verwaltet.  Eine Ressourcengruppe ist eine logische Gruppierung von Ressourcen.  Mit Ressourcengruppen können Ressourcensammlungen bereitgestellt und verwaltet werden.  Für eine serverlose Anwendung in Azure umfasst unsere Ressourcengruppe sowohl Azure Logik-Apps als auch Azure Functions.  Bei Verwendung des Ressourcengruppenobjekts innerhalb von Visual Studio kann die gesamte Anwendung als Einzelobjekt entwickelt, verwaltet und bereitgestellt werden.

### <a name="create-a-resource-group-project-in-visual-studio"></a>Erstellen eines Ressourcengruppenprojekts in Visual Studio

1. Klicken Sie in Visual Studio auf **Neues Projekt**, um ein neues Projekt hinzufügen
1. Wählen Sie in der Kategorie **Cloud** die Option zum Erstellen eines **Ressourcengruppenprojekts**  
 * Wenn die Kategorie oder das Projekt nicht aufgelistet sind, prüfen Sie, ob Azure SDK für Visual Studio installiert ist
1. Geben Sie dem Projekt einen Namen und Speicherort, und wählen Sie **Ok**, um Visual Studios-Eingabeaufforderungen für die Vorlagenauswahl zu erstellen.  Sie können von Leer, mit einer Logik-App oder mit einer sonstigen Ressource beginnen.  Hier verwenden wir eine Azure-Schnellstartvorlage für die ersten Schritte mit einer serverlosen App.
1. Wählen Sie die Vorlagenanzeige aus **Azure Schnellstart**
    ![Auswählen von Azure-Schnellstartvorlagen][1]
1. Wählen Sie die serverlose Schnellstartvorlage: **101-logic-app-and-function-app**, und klicken Sie auf **Ok**

Die Schnellstartvorlage erstellt in Ihrem Ressourcengruppenprojekt eine Bereitstellungsvorlage.  Diese enthält eine einfache Logik-App, die einen Azure Functions-Aufruf startet und das Ergebnis zurückgibt.  Wenn Sie die Datei `azuredeploy.json` im Projektmappen-Explorer öffnen, werden die Ressourcen für die serverlose App angezeigt.

## <a name="deploying-the-serverless-application"></a>Bereitstellen der serverlosen Anwendung

Um den visuellen Logik-App-Designer in Visual Studio öffnen zu können, muss eine zuvor bereitgestellte Azure-Ressourcengruppe vorhanden sein.  Mit dieser kann der Designer Verbindungen zu Ressourcen und Diensten in der Logik-App aufbauen und verwenden.  Um die ersten Schritte durchführen zu können, muss lediglich die erstellte Projektmappe bereitgestellt werden.

1. Klicken Sie mit der rechten Maustaste in Visual Studio, wählen Sie **Bereitstellen**, und erstellen Sie eine **neue** Bereitstellung  ![Auswählen einer neuen Ressourcenbereitstellung][2]
1. Wählen Sie ein gültiges Azure-Abonnement und eine gültige Ressourcengruppe aus.
1. Wählen Sie die Option **Bereitstellen** der Projektmappe aus.
1. Geben Sie den Namen der Logik-App und der Azure Function-App ein.  Der Azure Function-Name muss nicht global eindeutig sein.

Die serverlose Projektmappe wird in der spezifizierten Ressourcengruppe bereitgestellt.  In der **Ausgabe** in Visual Studio sehen Sie den Bereitstellungstatus.

## <a name="editing-the-logic-app-in-visual-studio"></a>Bearbeiten der Logik-App in Visual Studio

Nachdem die Projektmappe in einer beliebigen Ressourcengruppe bereitgestellt wurde, kann die Logik-App mit dem visuellen Designer bearbeitet und geändert werden.

1. Klicken Sie mit der rechten Maustaste auf die `azuredeploy.json`-Datei im Projektmappen-Explorer, und wählen Sie **Open With Logic Apps Designer** (Mit dem Designer für Logik-Apps öffnen) aus.
1. Wählen Sie die **Ressourcengruppe** und **den Speicherort** aus, an dem die Projektmappe bereitgestellt wurde, und wählen Sie **OK**

Der visuelle Designer von Logik-App wird nun in Visual Studio angezeigt.  Sie können weitere Schritte hinzufügen, den Workflow ändern und die Änderungen speichern.  Sie können auch Logik-Apps aus Visual Studio erstellen.  Wenn Sie mit der rechten Maustaste die **Ressourcen** im Vorlagennavigator anklicken, können Sie eine **Logik-App** zum Projekt hinzufügen.  Sie können leere Logik-Apps ohne vorherige Bereitstellung in einer Ressourcengruppe in den visuellen Designer laden.

### <a name="managing-and-viewing-run-history-for-a-deployed-logic-app"></a>Verwalten und Anzeigen des Ausführungsverlaufs einer bereitgestellten Logik-App

Sie können den Ausführungsverlauf der in Azure bereitgestellten Logik-Apps auch verwalten und anzeigen.  Wenn Sie das Tool **Cloud-Explorer** in Visual Studio öffnen, können Sie mit der rechten Maustaste auf eine beliebige Logik-App klicken und diese bearbeiten, deaktivieren, ihre Eigenschaften oder den Ausführungsverlauf anzeigen.  Wenn Sie auf Bearbeiten klicken, können Sie auch eine veröffentlichte Logik-App in ein Visual Studio-Ressourcengruppenprojekt herunterladen.  So können Sie Ihre Logik-App auch dann, wenn Sie im Azure-Portal mit dem Erstellen begonnen haben, in Visual Studio importieren und dort verwalten.

## <a name="developing-an-azure-function-in-visual-studio"></a>Entwickeln einer Azure-Funktion in Visual Studio

Die Bereitstellungsvorlage stellt alle Azure-Funktionen bereit, die in der Projektmappe für das in den `azuredeploy.json`- Variablen angegebene Git-Repository enthalten sind.  Wenn Sie in der Projektmappe ein Funktionsprojekt erstellen, checken Sie es in die Quellcodeverwaltung ein (GitHub, Visual Studio Team Services etc.), und aktualisieren Sie die Variable `repo`. Die Vorlage wird in der Azure-Funktion bereitgestellt.

### <a name="creating-an-azure-function-project"></a>Erstellen eines Azure-Funktionsprojekts

Wenn Sie JavaScript, Python, F#, Bash, Batches oder PowerShell verwenden und ein Projekt erstellen möchten, führen Sie die [Schritte in den CLI-Funktionen aus](../azure-functions/functions-run-local.md).  Wenn Sie eine Funktion in C# entwickeln, können Sie eine [C#-Klassenbibliothek](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/16/publishing-a-net-class-library-as-a-function-app/) in der aktuellen Projektmappe für die Azure-Funktion verwenden.

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen eines serverlosen sozialen Dashboards](logic-apps-scenario-social-serverless.md)
* [Verwalten einer Logik-App im Cloud-Explorer von Visual Studio](logic-apps-manage-from-vs.md)
* [Definitionssprache für Logik-App-Workflows](logic-apps-workflow-definition-language.md)

<!-- Image references -->
[1]: ./media/logic-apps-serverless-get-started-vs/select-template.png
[2]: ./media/logic-apps-serverless-get-started-vs/deploy.png
