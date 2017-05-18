---
title: "Erstellen einer Funktion in Azure, die durch einen GitHub-Webhook ausgelöst wird | Microsoft-Dokumentation"
description: Erstellen Sie mithilfe von Azure Functions eine serverlose Funktion, die durch einen GitHub-Webhook aufgerufen wird.
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 36ef34b8-3729-4940-86d2-cb8e176fcc06
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/02/2017
ms.author: glenga
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 34988ef05a27062ca109a1640e39695b52b8773f
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="create-a-function-triggered-by-a-github-webhook"></a>Erstellen einer Funktion, die durch einen GitHub-Webhook ausgelöst wird

Erfahren Sie, wie Sie eine Funktion erstellen können, die von einer HTTP-Webhookanforderung mit einer GitHub-spezifischen Nutzlast ausgelöst wird. 

![Eine Funktion im Azure-Portal, die durch einen GitHub-Webhook ausgelöst wird](./media/functions-create-github-webhook-triggered-function/function-app-in-portal-editor.png)

Für die Schritte in diesem Thema werden in der Regel weniger als fünf Minuten benötigt.

## <a name="prerequisites"></a>Voraussetzungen 

[!INCLUDE [Previous quickstart note](../../includes/functions-quickstart-previous-topics.md)]

Außerdem benötigen Sie ein GitHub-Konto mit mindestens einem Projekt. Sie können sich [für ein kostenloses GitHub-Konto registrieren](https://github.com/join), falls Sie noch keines besitzen.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)] 

## <a name="create-function"></a>Erstellen einer Funktion, die durch einen GitHub-Webhook ausgelöst wird

1. Erweitern Sie Ihre Funktionen-App, klicken Sie neben den **Funktionen** auf die Schaltfläche **+**, und klicken Sie anschließend auf die Vorlage **GitHubWebHook** für die gewünschte Sprache. **Benennen Sie die Funktion**, und klicken Sie anschließend auf **Erstellen**. 

2. Klicken Sie in der neuen Funktion auf **</> Funktions-URL abrufen**, kopieren Sie die Werte, und speichern Sie diese. Führen Sie die gleichen Schritte für **</> GitHub-Geheimnis abrufen** aus. Diese Werte benötigen Sie, um den Webhook in GitHub zu konfigurieren. 

    ![Überprüfen des Funktionscodes](./media/functions-create-github-webhook-triggered-function/functions-copy-function-url-github-secret.png) 
         
Erstellen Sie als Nächstes einen Webhook in Ihrem GitHub-Repository. 

## <a name="configure-the-webhook"></a>Konfigurieren des Webhooks
1. Navigieren Sie in GitHub zu einem Repository, das Sie besitzen. Sie können auch ein anderes Repository verwenden, das Sie verzweigt haben. Wenn Sie ein Verzeichnis verzweigen müssen, verwenden Sie <https://github.com/Azure-Samples/functions-quickstart>. 
 
2. Klicken Sie auf **Einstellungen** > **Webhooks** > **Webhook hinzufügen**.
   
    ![Hinzufügen eines GitHub-Webhooks](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-2.png)

3. Verwenden Sie die in der Tabelle angegebenen Einstellungen, und klicken Sie anschließend auf **Webhook hinzufügen**.
 
    ![Hinzufügen der Webhook-URL und Festlegen des Geheimnisses](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-3.png)

    | Einstellung      |  Empfohlener Wert   | Beschreibung                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **URL der Nutzlast** | Der kopierte Wert | Verwenden Sie den von **</> Abrufen der Funktions-URL** zurückgegebenen Wert. |
    | **Geheimnis**   | Der kopierte Wert | Verwenden Sie den von **</> Abrufen des GitHub-Geheimnisses** zurückgegebenen Wert. |
    | **Inhaltstyp** | Anwendung/json | Die Funktion erwartet eine JSON-Nutzlast. |
    | Ereignistrigger | Auswählen einzelner Ereignisse erlauben | Es sollen nur Problemmeldungsereignisse ausgelöst werden.  |
    |                | Problembericht                    |  |

Der Webhook ist nun so konfiguriert, dass Ihre Funktion ausgelöst wird, wenn ein neuer Problembericht hinzugefügt wird. 

## <a name="test-the-function"></a>Testen der Funktion
1. Öffnen Sie in Ihrem GitHub-Repository die Registerkarte **Probleme** in einem neuen Browserfenster.

2. Klicken Sie in dem neuen Fenster auf **Neues Problem**, geben Sie einen Titel ein, und klicken Sie anschließend auf **Submit new issue** (Neues Problem übermitteln). 

2. Geben Sie in die Problemmeldung einen Kommentar ein, und klicken Sie auf **Kommentar**. 

    ![Hinzufügen einer Problemmeldung in GitHub](./media/functions-create-github-webhook-triggered-function/functions-github-webhook-add-comment.png) 

3. Kehren Sie zum Portal zurück, und schauen Sie sich die Protokolle an. Sie sollten einen Nachverfolgungseintrag mit dem Text der neuen Meldung sehen. 
    
     ![Ansicht des Meldungstexts in den Protokollen.](./media/functions-create-github-webhook-triggered-function/function-app-view-logs.png)
 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie haben eine Funktion erstellt, die ausgeführt wird, wenn eine Anforderung von einem GitHub-Webhook empfangen wurde. 
[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)] Weitere Informationen zu Webhooktriggern finden Sie unter [HTTP- und Webhookbindungen in Azure Functions](functions-bindings-http-webhook.md). 




