<properties 
	pageTitle="Erstellen von Logik-Apps in Visual Studio | Microsoft Azure" 
	description="Es wird beschrieben, wie Sie in Visual Studio ein Projekt erstellen, um die Logik-App erstellen und bereitstellen zu können." 
	authors="jeffhollan" 
	manager="erikre" 
	editor="" 
	services="logic-apps" 
	documentationCenter=""/>

<tags
	ms.service="logic-apps"
	ms.workload="integration"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/26/2016"
	ms.author="jehollan"/>
	
# Erstellen und Bereitstellen von Logik-Apps in Visual Studio

Auch wenn das [Azure-Portal](https://portal.azure.com/) Ihnen eine hervorragende Möglichkeit zum Entwerfen und Verwalten von Logik-Apps bietet, können Sie die Logik-App stattdessen auch mit Visual Studio entwerfen und bereitstellen. Logik-Apps verfügen über ein umfassendes Visual Studio-Toolset, mit dem Sie eine Logik-App mit dem Designer erstellen, Vorlagen für Bereitstellung und Automation konfigurieren und die Bereitstellung in einer beliebigen Umgebung durchführen können.

## Installationsschritte

Unten sind die Schritte zum Installieren und Konfigurieren der Visual Studio-Tools für Logik-Apps angegeben.

### Voraussetzungen

- [Visual Studio 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
- [Aktuelles Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 oder höher)
- Internetzugriff bei Verwendung des eingebetteten Designers

### Installieren der Visual Studio-Tools für Logik-Apps

Gehen Sie nach der Installation der erforderlichen Komponenten wie folgt vor:

1. Öffnen Sie in Visual Studio 2015 das Menü **Extras**, und wählen Sie **Erweiterungen und Updates**.
1. Wählen Sie die Kategorie **Online** für die Onlinesuche.
1. Suchen Sie nach **Logik-Apps**, um die **Azure Logic Apps-Tools für Visual Studio** anzuzeigen.
1. Klicken Sie auf die Schaltfläche **Herunterladen**, um die Erweiterung herunterzuladen und zu installieren.
1. Starten Sie Visual Studio nach der Installation neu.

> [AZURE.NOTE] Sie können die Erweiterung auch direkt über [diesen Link](https://visualstudiogallery.msdn.microsoft.com/e25ad307-46cf-412e-8ba5-5b555d53d2d9) herunterladen.

Nach der Installation können Sie das Azure-Ressourcengruppenprojekt mit dem Logik-App-Designer verwenden.

## Erstellen eines Projekts

1. Klicken Sie auf das Menü **Datei**, und wählen Sie **Neu** > **Projekt** (oder wählen Sie **Hinzufügen** und dann **Neues Projekt**, um es einer vorhandenen Projektmappe hinzuzufügen): ![Menü "Datei"](./media/app-service-logic-deploy-from-vs/filemenu.png)

1. Suchen Sie im Dialogfeld den Eintrag **Cloud**, und wählen Sie dann **Azure-Ressourcengruppe**. Geben Sie einen **Namen** ein, und klicken Sie dann auf **OK**. ![Neues Projekt hinzufügen](./media/app-service-logic-deploy-from-vs/addnewproject.png)

1. Wählen Sie die Vorlage **Logik-App** aus. Als Starthilfe wird eine leere Vorlage für die Bereitstellung einer Logik-App erstellt. ![Azure-Vorlage auswählen](./media/app-service-logic-deploy-from-vs/selectazuretemplate.png)

1. Nachdem Sie die **Vorlage** ausgewählt haben, klicken Sie auf **OK**.

	Ihr Logik-App-Projekt wird der Projektmappe hinzugefügt. Die Bereitstellungsdatei sollte im Projektmappen-Explorer angezeigt werden:

	![Bereitstellung](./media/app-service-logic-deploy-from-vs/deployment.png)

## Verwenden des Logik-App-Designers

Wenn Sie über ein Azure-Ressourcengruppenprojekt mit einer Logik-App verfügen, können Sie den Designer als Hilfe beim Erstellen des Workflows in Visual Studio öffnen. Für den Designer ist eine Internetverbindung erforderlich, um die Connectors für die verfügbaren Eigenschaften und Daten abzufragen (z.B. fragt der Designer bei Verwendung des Dynamics CRM Online-Connectors Ihre CRM-Instanz ab, um die verfügbaren benutzerdefinierten Eigenschaften und Standardeigenschaften aufzulisten).

1. Klicken Sie mit der rechten Maustaste auf die Datei `<template>.json`, und wählen Sie die Option **Open with Logic App Designer** (Mit Logik-App-Designer öffnen) (oder `Ctrl+L`).
1. Wählen Sie das Abonnement, die Ressourcengruppe und den Speicherort für die Bereitstellungsvorlage aus.
	- Es ist wichtig zu beachten, dass beim Entwerfen einer Logik-App Ressourcen vom Typ **API-Verbindung** erstellt werden, um während des Entwurfsvorgangs Eigenschaften abfragen zu können. Die ausgewählte Ressourcengruppe ist die Ressourcengruppe, die zum Erstellen dieser Verbindungen zur Entwurfszeit verwendet wird. Sie können alle API-Verbindungen anzeigen oder ändern, indem Sie auf das Azure-Portal zugreifen und nach **API-Verbindungen** suchen. ![Abonnementauswahl](./media/app-service-logic-deploy-from-vs/designer_picker.png)
1. Der Designer sollte basierend auf der Definition in der Datei `<template>.json` gerendert werden.
1. Sie können die Logik-App jetzt erstellen und entwerfen. Die Änderungen werden in der Bereitstellungsvorlage aktualisiert. ![Designer in Visual Studio](./media/app-service-logic-deploy-from-vs/designer_in_vs.png)

Sie sehen auch, dass Ressourcen vom Typ `Microsoft.Web/connections` der Ressourcendatei für alle Verbindungen hinzugefügt werden, die für die Funktion der Logik-App benötigt werden. Diese Verbindungseigenschaften können während der Bereitstellung festgelegt werden und werden nach der Bereitstellung unter **API-Verbindungen** im Azure-Portal verwaltet.

### Wechseln zur JSON-Codeansicht

Sie können unten im Designer die Registerkarte **Codeansicht** wählen, um zur JSON-Darstellung der Logik-App zu wechseln. Um zurück zum vollständigen JSON-Ressourcencode zu wechseln, klicken Sie mit der rechten Maustaste auf die Datei `<template>.json` und wählen **Öffnen**.

### Speichern der Logik-App

Sie können die Logik-App jederzeit mit der Schaltfläche **Speichern** oder der Tastenkombination `Ctrl+S` speichern. Falls beim Speichern der Logik-App Fehler auftreten, werden diese im Fenster **Ausgaben** von Visual Studio angezeigt.

## Bereitstellen Ihrer Logik-App

Nach der Konfiguration Ihrer App können Sie diese in nur wenigen Schritten direkt von Visual Studio aus bereitstellen.

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wechseln Sie zu **Bereitstellen** > **Neue Bereitstellung...**. ![Neue Bereitstellung](./media/app-service-logic-deploy-from-vs/newdeployment.png)

2. Sie werden dazu aufgefordert, sich bei Ihrem Azure-Abonnement anzumelden.

3. Nun müssen Sie die Details der Ressourcengruppe auswählen, der Sie die Logik-App bereitstellen möchten. ![Für Ressourcengruppe bereitstellen](./media/app-service-logic-deploy-from-vs/deploytoresourcegroup.png)

     > [AZURE.NOTE]    Achten Sie darauf, die richtigen Vorlagen- und Parameterdateien für die Ressourcengruppe auszuwählen. (Wenn Sie die App zum Beispiel in einer Produktionsumgebung bereitstellen möchten, sollten Sie die Parameterdatei der Produktion auswählen).
4. Klicken Sie auf die Schaltfläche „Bereitstellen“.
 
    
6. Der Status der Bereitstellung wird im Fenster **Ausgabe** angezeigt (möglicherweise müssen Sie die Option **Azure-Bereitstellung** auswählen). ![Ausgabe](./media/app-service-logic-deploy-from-vs/output.png)

In Zukunft können Sie Ihre Logik-App in der Quellcodeverwaltung überarbeiten und Visual Studio zum Bereitstellen neuer Versionen verwenden.

> [AZURE.NOTE] Wenn Sie die Definition direkt im Azure-Portal ändern, werden diese Änderungen bei der nächsten Bereitstellung in Visual Studio überschrieben.

## Nächste Schritte

- Befolgen Sie zum Einstieg in Logik-Apps das Lernprogramm [Erstellen einer Logik-App](app-service-logic-create-a-logic-app.md).
- [Anzeigen allgemeiner Beispiele und Szenarien](app-service-logic-examples-and-scenarios.md)
- [Sie können Geschäftsprozesse mit Logik-Apps automatisieren.](http://channel9.msdn.com/Events/Build/2016/T694)
- [Erfahren Sie, wie Sie Ihre Systeme in Logik-Apps integrieren.](http://channel9.msdn.com/Events/Build/2016/P462)

<!---HONumber=AcomDC_0803_2016-->