---
title: Erstellen von Azure Logic Apps in Visual Studio | Microsoft-Dokumentation
description: "Es wird beschrieben, wie Sie in Visual Studio ein Projekt erstellen, um die Logik-App erstellen und bereitstellen zu können."
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: e484e5ce-77e9-4fa9-bcbe-f851b4eb42a6
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: jehollan
translationtype: Human Translation
ms.sourcegitcommit: a9f786259676a1bc9aa616f2db2935dff45475e2
ms.openlocfilehash: db818b2b76d3a8d8c3324c9556237139d319efb9


---
# <a name="build-and-deploy-logic-apps-in-visual-studio"></a>Erstellen und Bereitstellen von Logik-Apps in Visual Studio
Auch wenn das [Azure-Portal](https://portal.azure.com/) Ihnen eine hervorragende Möglichkeit zum Entwerfen und Verwalten von Logik-Apps bietet, können Sie die Logik-App stattdessen auch mit Visual Studio entwerfen und bereitstellen.  Logik-Apps verfügen über ein umfassendes Visual Studio-Toolset, mit dem Sie eine Logik-App mit dem Designer erstellen, Vorlagen für Bereitstellung und Automation konfigurieren und die Bereitstellung in einer beliebigen Umgebung durchführen können.  

## <a name="installation-steps"></a>Installationsschritte
Unten sind die Schritte zum Installieren und Konfigurieren der Visual Studio-Tools für Logik-Apps angegeben.

### <a name="prerequisites"></a>Voraussetzungen
* [Visual Studio 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
* [Aktuelles Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 oder höher)
* [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)
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

Nach der Installation können Sie das Azure-Ressourcengruppenprojekt mit dem Logik-App-Designer verwenden.

## <a name="create-a-project"></a>Erstellen eines Projekts
1. Klicken Sie auf das Menü **Datei**, und wählen Sie **Neu** >  **Projekt** (oder wählen Sie **Hinzufügen** und dann **Neues Projekt**, um es einer vorhandenen Projektmappe hinzuzufügen):  ![Dateimenü](./media/logic-apps-deploy-from-vs/filemenu.png)
2. Suchen Sie im Dialogfeld den Eintrag **Cloud**, und wählen Sie dann **Azure-Ressourcengruppe**. Geben Sie unter **Name** einen Namen ein, und klicken Sie dann auf **OK**.
    ![Neues Projekt hinzufügen](./media/logic-apps-deploy-from-vs/addnewproject.png)
3. Wählen Sie die Vorlage **Logik-App** aus. Als Starthilfe wird eine leere Vorlage für die Bereitstellung einer Logik-App erstellt.
    ![Azure-Vorlage auswählen](./media/logic-apps-deploy-from-vs/selectazuretemplate1.png)
4. Nachdem Sie die **Vorlage** ausgewählt haben, klicken Sie auf **OK**.
   
    Ihr Logik-App-Projekt wird der Projektmappe hinzugefügt. Die Bereitstellungsdatei sollte im Projektmappen-Explorer angezeigt werden:  
   
    ![Bereitstellung](./media/logic-apps-deploy-from-vs/deployment.png)

## <a name="using-the-logic-app-designer"></a>Verwenden des Logik-App-Designers
Wenn Sie über ein Azure-Ressourcengruppenprojekt mit einer Logik-App verfügen, können Sie den Designer als Hilfe beim Erstellen des Workflows in Visual Studio öffnen.  Für den Designer ist eine Internetverbindung erforderlich, um die Connectors für die verfügbaren Eigenschaften und Daten abzufragen (z.B. fragt der Designer bei Verwendung des Dynamics CRM Online-Connectors Ihre CRM-Instanz ab, um die verfügbaren benutzerdefinierten Eigenschaften und Standardeigenschaften aufzulisten).

1. Klicken Sie mit der rechten Maustaste auf die Datei `<template>.json`, und wählen Sie die Option **Mit Logik-App-Designer öffnen** (oder drücken Sie `Ctrl+L`).
2. Wählen Sie das Abonnement, die Ressourcengruppe und den Speicherort für die Bereitstellungsvorlage aus.
   * Beachten Sie unbedingt, dass beim Entwerfen einer Logik-App Ressourcen vom Typ **API-Verbindung** erstellt werden, um während des Entwurfsvorgangs Eigenschaften abfragen zu können.  Die ausgewählte Ressourcengruppe wird zum Erstellen dieser Verbindungen zur Entwurfszeit verwendet.  Sie können alle API-Verbindungen anzeigen oder ändern, indem Sie auf das Azure-Portal zugreifen und nach **API-Verbindungen** suchen.
   
     ![Abonnementauswahl](./media/logic-apps-deploy-from-vs/designer_picker.png)
3. Der Designer sollte basierend auf der Definition in der Datei `<template>.json` gerendert werden.
4. Sie können die Logik-App jetzt erstellen und entwerfen. Die Änderungen werden in der Bereitstellungsvorlage aktualisiert.
    ![Designer in Visual Studio](./media/logic-apps-deploy-from-vs/designer_in_vs.png)

`Microsoft.Web/connections`-Ressourcen werden Ihrer Ressourcendatei für alle Verbindungen hinzugefügt, die für die Funktion der Logik-App benötigt werden.  Diese Verbindungseigenschaften können während der Bereitstellung festgelegt und nach der Bereitstellung im Azure-Portal unter **API-Verbindungen** verwaltet werden.

### <a name="switching-to-the-json-code-view"></a>Wechseln zur JSON-Codeansicht
Sie können unten im Designer die Registerkarte **Codeansicht** wählen, um zur JSON-Darstellung der Logik-App zu wechseln.  Um zurück zum vollständigen JSON-Ressourcencode zu wechseln, klicken Sie mit der rechten Maustaste auf die Datei `<template>.json` und wählen **Öffnen**.

### <a name="saving-the-logic-app"></a>Speichern der Logik-App
Sie können die Logik-App jederzeit mit der Schaltfläche **Speichern** oder der Tastenkombination `Ctrl+S` speichern.  Falls beim Speichern der Logik-App Fehler auftreten, werden diese im Visual Studio-Fenster **Ausgaben** angezeigt.

## <a name="deploying-your-logic-app"></a>Bereitstellen Ihrer Logik-App
Nach der Konfiguration Ihrer App können Sie diese in nur wenigen Schritten direkt von Visual Studio aus bereitstellen. 

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wechseln Sie zu **Bereitstellen** > **Neue Bereitstellung...**
    ![Neue Bereitstellung](./media/logic-apps-deploy-from-vs/newdeployment.png).
2. Sie werden dazu aufgefordert, sich bei Ihrem Azure-Abonnement anzumelden. 
3. Nun müssen Sie die Details der Ressourcengruppe auswählen, der Sie die Logik-App bereitstellen möchten. 
    ![Für Ressourcengruppe bereitstellen](./media/logic-apps-deploy-from-vs/deploytoresourcegroup.png)
   
   > [!NOTE]
   > Achten Sie darauf, die richtigen Vorlagen- und Parameterdateien für die Ressourcengruppe auszuwählen. (Wenn Sie die App zum Beispiel in einer Produktionsumgebung bereitstellen möchten, sollten Sie die Parameterdatei der Produktion auswählen). 
   > 
   > 
4. Klicken Sie auf die Schaltfläche „Bereitstellen“.
5. Der Status der Bereitstellung wird im Fenster **Ausgabe** angezeigt (möglicherweise müssen Sie die Option **Azure-Bereitstellung** auswählen). 
    ![Ausgabe](./media/logic-apps-deploy-from-vs/output.png)

In Zukunft können Sie Ihre Logik-App in der Quellcodeverwaltung überarbeiten und Visual Studio zum Bereitstellen neuer Versionen verwenden. 

> [!NOTE]
> Wenn Sie die Definition direkt im Azure-Portal ändern, werden diese Änderungen bei der nächsten Bereitstellung in Visual Studio überschrieben.
> 
> 

## <a name="adding-a-logic-app-to-an-existing-resource-group-project"></a>Hinzufügen einer Logik-App zu einem vorhandenen Ressourcengruppenprojekt
Wenn Sie ein vorhandenes Ressourcengruppenprojekt haben, können Sie diesem im Fenster mit der JSON-Gliederung eine Logik-App hinzufügen oder die zuvor erstellte Logik-App mit einer weiteren ergänzen.
1. Öffnen Sie die Datei `<template>.json` .
2. Öffnen Sie das Fenster mit der JSON-Gliederung.  Das Fenster „JSON-Gliederung“ finden Sie unter **Ansicht** > **Andere Fenster** > **JSON-Gliederung**.
3. Zum Hinzufügen einer Ressource klicken Sie entweder oben im Fenster „JSON-Gliederung“ auf die Schaltfläche „Ressource hinzufügen“ oder mit der rechten Maustaste auf **Ressourcen** und wählen dann **Neue Ressource** hinzufügen.

    ![JSON-Gliederung](./media/logic-apps-deploy-from-vs/jsonoutline.png)
    
4. Navigieren Sie im Dialogfeld **Ressource hinzufügen** zur Option **Logik-App**, wählen Sie sie aus, geben Sie ihr einen Namen, und wählen Sie **Hinzufügen**.

    ![Ressource hinzufügen](./media/logic-apps-deploy-from-vs/addresource.png)

## <a name="next-steps"></a>Nächste Schritte
* Befolgen Sie zum Einstieg in Logik-Apps das Lernprogramm [Erstellen einer Logik-App](logic-apps-create-a-logic-app.md) .  
* [Anzeigen allgemeiner Beispiele und Szenarien](logic-apps-examples-and-scenarios.md)
* [Sie können Geschäftsprozesse mit Logik-Apps automatisieren.](http://channel9.msdn.com/Events/Build/2016/T694) 
* [Erfahren Sie, wie Sie Ihre Systeme in Logik-Apps integrieren.](http://channel9.msdn.com/Events/Build/2016/P462)




<!--HONumber=Jan17_HO3-->


