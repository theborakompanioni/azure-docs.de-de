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
ms.date: 04/18/2017
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: 9eafbc2ffc3319cbca9d8933235f87964a98f588
ms.openlocfilehash: d4354546f3342d65353a86a4cec7d02547ab92e7
ms.lasthandoff: 04/22/2017


---
# <a name="create-a-function-triggered-by-a-github-webhook"></a>Erstellen einer Funktion, die durch einen GitHub-Webhook ausgelöst wird

Hier erfahren Sie, wie Sie eine Funktion erstellen, die durch einen GitHub-Webhook ausgelöst wird. 

![Erstellen einer Funktionen-App im Azure-Portal](./media/functions-create-github-webhook-triggered-function/function-app-in-portal-editor.png)

Für dieses Thema werden die Ressourcen benötigt, die im Thema [Erstellen Ihrer ersten Funktion im Azure-Portal](functions-create-first-azure-function.md) erstellt wurden.

Außerdem benötigen Sie ein GitHub-Konto. Sie können sich [für ein kostenloses GitHub-Konto registrieren](https://github.com/join), falls Sie noch keines besitzen. 

Für die Schritte in diesem Thema werden in der Regel weniger als fünf Minuten benötigt.

## <a name="find-your-function-app"></a>Suchen Ihrer Funktionen-App    

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an. 

2. Geben Sie oben im Portal auf der Suchleiste den Namen der Funktionen-App ein, und wählen Sie sie in der Liste aus.

## <a name="create-function"></a>Erstellen einer Funktion, die durch einen GitHub-Webhook ausgelöst wird

1. Klicken Sie in Ihrer Funktionen-App neben **Funktionen** auf die Schaltfläche **+**, klicken Sie auf die Vorlage **GitHubWebHook** für die gewünschte Sprache, und klicken Sie anschließend auf **Erstellen**.
   
    ![Erstellen einer Funktion, die durch einen GitHub-Webhook ausgelöst wird, im Azure-Portal](./media/functions-create-github-webhook-triggered-function/functions-create-github-webhook-trigger.png) 

2. Klicken Sie auf **</> Funktions-URL abrufen**, kopieren Sie die Werte, und speichern Sie sie. Führen Sie die gleichen Schritte für **</> GitHub-Geheimnis abrufen** aus. Diese Werte benötigen Sie, um den Webhook in GitHub zu konfigurieren. 

    ![Überprüfen des Funktionscodes](./media/functions-create-github-webhook-triggered-function/functions-copy-function-url-github-secret.png) 
         
Erstellen Sie als Nächstes einen Webhook in Ihrem GitHub-Repository. 

## <a name="configure-the-webhook"></a>Konfigurieren des Webhooks
1. Navigieren Sie in GitHub zu einem Repository, das Sie besitzen. Sie können auch ein anderes Repository verwenden, das Sie verzweigt haben.
 
2. Klicken Sie auf **Einstellungen** > **Webhooks** > **Webhook hinzufügen**.
   
    ![Hinzufügen eines GitHub-Webhooks](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-2.png)

3. Fügen Sie die Funktions-URL und den geheimen Schlüssel unter **Nutzlast-URL** und **Geheimnis** ein, und wählen Sie unter **Inhaltstyp** die Option **application/json** aus.

4. Klicken Sie auf **Einzelne Ereignisse auswählen**, wählen Sie **Problembericht** aus, und klicken Sie anschließend auf **Webhook hinzufügen**.
   
    ![Hinzufügen der Webhook-URL und Festlegen des Geheimnisses](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-3.png)

Der Webhook ist nun so konfiguriert, dass Ihre Funktion ausgelöst wird, wenn ein neuer Problembericht hinzugefügt wird. 

## <a name="test-the-function"></a>Testen der Funktion
1. Öffnen Sie in Ihrem GitHub-Repository die Registerkarte **Probleme** in einem neuen Browserfenster.

2. Klicken Sie in dem neuen Fenster auf **Neues Problem**, geben Sie einen Titel ein, und klicken Sie anschließend auf **Submit new issue** (Neues Problem übermitteln). 

2. Geben Sie in die Problemmeldung einen Kommentar ein, und klicken Sie auf **Kommentar**. 

3. Klicken Sie im anderen GitHub-Fenster neben Ihrem neuen Webhook auf **Bearbeiten**, scrollen Sie nach unten zu **Recent Deliveries** (Aktuelle Übermittlungen), und vergewissern Sie sich, dass von Ihrer Funktion eine Webhookanforderung verarbeitet wurde. 
 
    ![Hinzufügen der Webhook-URL und Festlegen des Geheimnisses](./media/functions-create-github-webhook-triggered-function/functions-github-webhook-triggered.png)

   Die Antwort Ihrer Funktion sollte `New GitHub comment: <Your issue comment text>` enthalten.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]


