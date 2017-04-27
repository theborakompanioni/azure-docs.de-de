---
title: "Erstellen und Bereitstellen von Logik-Apps in Visual Studio – Azure Logic Apps | Microsoft-Dokumentation"
description: Erstellen Sie Visual Studio-Projekte zum Entwerfen, Erstellen und Bereitstellen von Azure Logic Apps.
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
ms.custom: H1Hack27Feb2017
ms.date: 2/14/2017
ms.author: jehollan
translationtype: Human Translation
ms.sourcegitcommit: 988e7fe2ae9f837b661b0c11cf30a90644085e16
ms.openlocfilehash: ad18896548449d85e2af8a91ddd90c8192db1ab2
ms.lasthandoff: 04/06/2017

---

# <a name="design-build-and-deploy-azure-logic-apps-in-visual-studio"></a>Entwerfen, Erstellen und Bereitstellen von Azure Logic Apps in Visual Studio

Azure Logic Apps lassen sich zwar hervorragend über das [Azure-Portal](https://portal.azure.com/) erstellen und verwalten, für das Entwerfen, die Erstellung und die Bereitstellung von Logik-Apps empfiehlt sich aber unter Umständen die Verwendung von Visual Studio. Visual Studio bietet umfassende Tools zum Erstellen von Logik-Apps mithilfe des Logik-Apps-Designers, zum Konfigurieren von Bereitstellungs- und Automatisierungsvorlagen sowie zum Bereitstellen in einer beliebigen Umgebung. 

Eine Einführung in Azure Logic Apps finden Sie im Tutorial [Erstellen Ihrer ersten Logik-App über das Azure-Portal](logic-apps-create-a-logic-app.md).

## <a name="installation-steps"></a>Installationsschritte

Führen Sie zum Installieren und Konfigurieren der Visual Studio-Tools für Azure Logic Apps die folgenden Schritte aus.

### <a name="prerequisites"></a>Voraussetzungen

* [Visual Studio 2017](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx) oder Visual Studio 2015
* [Aktuelles Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 oder höher)
* [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)
* Internetzugriff bei Verwendung des eingebetteten Designers

### <a name="install-visual-studio-tools-for-azure-logic-apps"></a>Installieren der Visual Studio-Tools für Azure Logic Apps

Gehen Sie nach der Installation der erforderlichen Komponenten wie folgt vor:

1. Öffnen Sie Visual Studio. Wählen Sie im Menü **Extras** die Option **Erweiterungen und Updates** aus.
2. Erweitern Sie die Kategorie **Online**, um eine Onlinesuche durchzuführen.
3. Navigieren Sie zu **Logic Apps** (oder suchen Sie danach), um **Azure Logic Apps-Tools für Visual Studio** zu finden.
4. Klicken Sie auf **Herunterladen**, um die Erweiterung herunterzuladen und zu installieren.
5. Starten Sie Visual Studio nach der Installation neu.

> [!NOTE]
> Sie können die [Azure Logic Apps-Tools für Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551) und die [Azure Logic Apps-Tools für Visual Studio 2015](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio) auch direkt über den Visual Studio Marketplace herunterladen.

Nach Abschluss der Installation können Sie das Azure-Ressourcengruppenprojekt mit dem Designer für Logik-Apps verwenden.

## <a name="create-your-project"></a>Erstellen Ihres Projekts

1. Navigieren Sie im Menü **Datei** zu **Neu**, und wählen Sie **Projekt** aus. Oder: Navigieren Sie zu **Hinzufügen**, und wählen Sie **Neues Projekt** aus, um Ihr Projekt einer bereits vorhandenen Projektmappe hinzuzufügen.

    ![Menü "Datei"](./media/logic-apps-deploy-from-vs/filemenu.png)

2. Navigieren Sie im Fenster **Neues Projekt** zu **Cloud**, und wählen Sie **Azure-Ressourcengruppe** aus. Benennen Sie Ihr Projekt, und klicken Sie auf **OK**.

    ![Neues Projekt hinzufügen](./media/logic-apps-deploy-from-vs/addnewproject.png)

3. Wählen Sie die Vorlage **Logik-App** aus, um eine leere Logik-App-Bereitstellungsvorlage zu erstellen, mit der Sie arbeiten können. Klicken Sie nach der Wahl der Vorlage auf **OK**.

    ![Auswählen der Logik-App-Vorlage](./media/logic-apps-deploy-from-vs/selectazuretemplate1.png)

    Damit haben Sie Ihr Logik-App-Projekt Ihrer Projektmappe hinzugefügt. 
    Im Projektmappen-Explorer sollte nun Ihre Bereitstellungsdatei angezeigt werden.

    ![Bereitstellungsdatei](./media/logic-apps-deploy-from-vs/deployment.png)

## <a name="create-your-logic-app-with-logic-app-designer"></a>Erstellen Ihrer Logik-App mit Logik-App-Designer

Wenn Sie über ein Azure-Ressourcengruppenprojekt mit einer Logik-App verfügen, können Sie in Visual Studio den Designer für Logik-Apps öffnen, um Ihren Workflow zu erstellen. 

> [!NOTE]
> Der Designer erfordert eine Internetverbindung, um verfügbare Eigenschaften und Daten von Connectors abfragen zu können. Bei Verwendung des Dynamics CRM Online-Connectors fragt der Designer beispielsweise Ihre CRM-Instanz ab, um die verfügbaren benutzerdefinierten und standardmäßigen Eigenschaften anzuzeigen.

1. Klicken Sie mit der rechten Maustaste auf die Datei `<template>.json`, und wählen Sie **Open with Logic App Designer** (Mit Designer für Logik-Apps öffnen) aus. (`Ctrl+L`)

2. Wählen Sie Ihr Azure-Abonnement, die Ressourcengruppe und den Speicherort für Ihre Bereitstellungsvorlage aus.

    > [!NOTE]
    > Beim Entwerfen einer Logik-App werden API-Verbindungsressourcen zum Abfragen von Eigenschaften erstellt. Diese Verbindungen werden von Visual Studio zur Entwurfszeit unter Verwendung der ausgewählte Ressourcengruppe erstellt. Navigieren Sie zum Anzeigen oder Ändern von API-Verbindungen im Azure-Portal zu **API-Verbindungen**.

    ![Abonnementauswahl](./media/logic-apps-deploy-from-vs/designer_picker.png)

    Für das Rendering verwendet der Designer die Definition in der Datei `<template>.json`.

4. Erstellen und gestalten Sie Ihre Logik-App. Ihre Bereitstellungsvorlage wird mit Ihren Änderungen aktualisiert.

    ![Designer für Logik-Apps in Visual Studio](./media/logic-apps-deploy-from-vs/designer_in_vs.png)

Für alle Verbindungen, die Ihre Logik-App benötigt, fügt Visual Studio Ihrer Ressourcendatei Ressourcen vom Typ `Microsoft.Web/connections` hinzu. Diese Verbindungseigenschaften können während der Bereitstellung festgelegt und nach der Bereitstellung im Azure-Portal unter **API-Verbindungen** verwaltet werden.

### <a name="switch-to-json-code-view"></a>Wechseln zur JSON-Codeansicht

Über die Registerkarte **Codeansicht** am unteren Rand des Designers können Sie zur JSON-Darstellung Ihrer Logik-App wechseln.

Wenn Sie wieder zum vollständigen JSON-Ressourcencode zurückkehren möchten, klicken Sie mit der rechten Maustaste auf die Datei `<template>.json`, und wählen Sie **Öffnen** aus.

### <a name="add-references-for-dependent-resources-to-visual-studio-deployment-templates"></a>Hinzufügen von Verweisen auf abhängige Ressourcen zu Visual Studio-Bereitstellungsvorlagen

Wenn Ihre Logik-App auf abhängige Ressourcen verweisen soll, können Sie in Ihrer Logik-App-Bereitstellungsvorlage [Azure Resource Manager-Vorlagenfunktionen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions) (etwa Parameter) verwenden. So kann Ihre Logik-App beispielsweise auf eine Azure-Funktion oder auf ein Integrationskonto verweisen, die bzw. das Sie zusammen mit Ihrer Logik-App bereitstellen möchten. Halten Sie sich bei der Verwendung von Parametern in Ihrer Bereitstellungsvorlage an die folgenden Richtlinien, damit das Rendering des Designers für Logik-Apps ordnungsgemäß funktioniert. 

Logik-App-Parameter können in folgenden Arten von Triggern und Aktionen verwendet werden:

*   Untergeordneter Workflow
*   Funktionen-App
*   APIM-Aufruf
*   Laufzeit-URL für API-Verbindung

Außerdem können Sie folgende Vorlagenfunktionen verwenden: „list below“, „includes parameters“, „variables“, „resourceId“, „concat“ usw. Das folgende Beispiel zeigt, wie Sie die Ressourcen-ID einer Azure-Funktion ersetzen können:

```
"parameters":{
    "functionName": {
    "type":"string",
    "minLength":1,
    "defaultValue":"<FunctionName>"
    }
},
```

Und hier sehen Sie, wo Sie „parameters“ verwenden können:

```
"MyFunction": {
        "type": "Function",
        "inputs": {
        "body":{},
        "function":{
        "id":"[resourceid('Microsoft.Web/sites/functions','functionApp',parameters('functionName'))]"
        }
    },
    "runAfter":{}
}
```

> [!NOTE] 
> Damit der Designer für Logik-Apps bei Verwendung von „parameters“ ordnungsgemäß funktioniert, müssen Standardwerte angegeben werden. Beispiel:
> 
> ```
> "parameters": {
>     "IntegrationAccount": {
>     "type":"string",
>     "minLength":1,
>     "defaultValue":"/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Logic/integrationAccounts/<integrationAccountName>"
>     }
> },
> ```

### <a name="save-your-logic-app"></a>Speichern Ihrer Logik-App

Sie können Ihre Logik-App jederzeit über **Datei** > **Speichern** speichern. (`Ctrl+S`) 

Falls Ihre Logik-App beim Speichern einen Fehler enthält, wird dieser im Visual Studio-Fenster **Ausgaben** angezeigt.

## <a name="deploy-your-logic-app-from-visual-studio"></a>Bereitstellen Ihrer Logik-App von Visual Studio aus

Nachdem Sie Ihre App konfiguriert haben, können Sie sie in nur wenigen Schritten direkt über Visual Studio bereitstellen. 

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und navigieren Sie zu **Bereitstellen** > **Neue Bereitstellung...**.

    ![Neue Bereitstellung](./media/logic-apps-deploy-from-vs/newdeployment.png)

2. Melden Sie sich bei Ihrem Azure-Abonnement an, wenn Sie dazu aufgefordert werden. 

3. Wählen Sie nun die Details für die Ressourcengruppe aus, in der Sie Ihre Logik-App bereitstellen möchten. Wählen Sie anschließend **Bereitstellen** aus.

    > [!NOTE]
    > Achten Sie darauf, die richtige Vorlage und Parameterdatei für die Ressourcengruppe auszuwählen. Wählen Sie also beispielsweise die Produktionsparameterdatei aus, um eine Bereitstellung in einer Produktionsumgebung durchzuführen.

    ![Für Ressourcengruppe bereitstellen](./media/logic-apps-deploy-from-vs/deploytoresourcegroup.png)

    Der Bereitstellungsstatus wird im Fenster **Ausgabe** angezeigt. 
    Unter Umständen muss in der Liste **Ausgabe anzeigen von** die Option **Azure-Bereitstellung** ausgewählt werden.

    ![Ausgabe des Bereitstellungsstatus](./media/logic-apps-deploy-from-vs/output.png)

In Zukunft können Sie Ihre Logik-App in der Quellcodeverwaltung bearbeiten und Visual Studio zum Bereitstellen neuer Versionen verwenden.

> [!NOTE]
> Wenn Sie die Definition direkt im Azure-Portal ändern, werden die vorgenommenen Änderungen bei der nächsten Bereitstellung über Visual Studio überschrieben. 

## <a name="add-your-logic-app-to-an-existing-resource-group-project"></a>Hinzufügen Ihrer Logik-App zu einem vorhandenen Ressourcengruppenprojekt

Wenn Sie über ein vorhandenes Ressourcengruppenprojekt verfügen, können Sie Ihre Logik-App im Fenster mit der JSON-Gliederung dem Projekt hinzufügen. Außerdem können Sie eine weitere Logik-App zusammen mit der zuvor erstellten App hinzufügen.

1. Öffnen Sie die Datei `<template>.json` .

2. Navigieren Sie zum Öffnen des Fensters mit der JSON-Gliederung zu **Ansicht** > **Andere Fenster** > **JSON-Gliederung**.

3. Klicken Sie am oberen Rand des Fensters mit der JSON-Gliederung auf **Ressource hinzufügen**, um der Vorlagendatei eine Ressource hinzuzufügen. Klicken Sie im Fenster mit der JSON-Gliederung mit der rechten Maustaste auf **Ressourcen**, und wählen Sie **Neue Ressource hinzufügen** aus.

    ![Fenster mit der JSON-Gliederung](./media/logic-apps-deploy-from-vs/jsonoutline.png)
    
4. Wählen Sie im Dialogfeld **Ressource hinzufügen** die Option **Logik-App** aus. Benennen Sie Ihre Logik-App, und wählen Sie **Hinzufügen** aus.

    ![Ressource hinzufügen](./media/logic-apps-deploy-from-vs/addresource.png)

## <a name="next-steps"></a>Nächste Schritte

* [Verwalten von Azure Logic Apps mit dem Cloud-Explorer von Visual Studio](logic-apps-manage-from-vs.md)
* [Anzeigen allgemeiner Beispiele und Szenarien](logic-apps-examples-and-scenarios.md)
* [Automatisieren von Geschäftsprozessen mit Azure Logic Apps](http://channel9.msdn.com/Events/Build/2016/T694)
* [Integrieren Ihrer Systeme in Azure Logic Apps](http://channel9.msdn.com/Events/Build/2016/P462)

