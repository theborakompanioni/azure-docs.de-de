---
title: "Erstellen einer Funktion in Azure, die nach einem Zeitplan ausgeführt wird | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie in Azure eine Funktion erstellen können, die nach einem von Ihnen definierten Zeitplan ausgeführt wird."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: ba50ee47-58e0-4972-b67b-828f2dc48701
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/02/2017
ms.author: glenga
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 10461bddeb4d5adf4a6e4f65159ba2653dbef8a4
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
#  <a name="create-a-function-in-azure-that-is-triggered-by-a-timer"></a>Erstellen einer Funktion in Azure, die von einem Timer ausgelöst wird

Erfahren Sie, wie Sie mithilfe von Azure Functions eine Funktion erstellen können, die nach einem von Ihnen definierten Zeitplan ausgeführt wird. 

![Erstellen einer Funktionen-App im Azure-Portal](./media/functions-create-scheduled-function/function-app-in-portal-editor.png)

Für die Schritte in diesem Thema werden in der Regel weniger als fünf Minuten benötigt.

## <a name="prerequisites"></a>Voraussetzungen 

[!INCLUDE [Previous quickstart note](../../includes/functions-quickstart-previous-topics.md)]

In diesem Thema erstellen Sie eine Funktion mit Auslösung per Timer in der vorhandenen Funktionen-App. 

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)] 

## <a name="create-function"></a>Erstellen einer Funktion mit Auslösung per Timer

1. Erweitern Sie Ihre Funktionen-App, klicken Sie neben den **Funktionen** auf die Schaltfläche **+**, und klicken Sie anschließend auf die Vorlage **TimerTrigger** für die gewünschte Sprache. Verwenden Sie dann die in der Tabelle angegebenen Einstellungen, und klicken Sie anschließend auf **Erstellen**:

    | Einstellung      |  Empfohlener Wert   | Beschreibung                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Name Ihrer Funktion** | TimerTriggerCSharp1 | Definiert den Namen der Funktion mit Auslösung per Timer
    | **[Zeitplan](http://en.wikipedia.org/wiki/Cron#CRON_expression)** | 0 */1 * * * * | Ein sechs Felder umfassender [CRON-Ausdruck](http://en.wikipedia.org/wiki/Cron#CRON_expression), der die minütliche Ausführung der Funktion festlegt. |

    Eine Funktion wird in der gewählten Sprache erstellt. Sie wird minütlich ausgeführt. 

4. Überprüfen Sie, ob die Funktion ausgeführt wird, indem Sie sich die Nachverfolgungsinformationen in den Protokollen ansehen. 

    ![Viewer der Funktionsprotokolle im Azure-Portal](./media/functions-create-scheduled-function/functions-timer-trigger-view-logs2.png)

Jetzt können Sie den Zeitplan der Funktion anpassen, sodass sie nicht mehr so häufig ausgeführt wird, z.B. nur noch stündlich. 

## <a name="update-the-timer-schedule"></a>Aktualisieren des Timerzeitplans

1. Erweitern Sie die Funktion, und klicken Sie auf **Integrieren**. Hier können Sie Eingabe- und Ausgabebindungen für Ihre Funktion definieren und den Zeitplan festlegen. 

2. Geben Sie einen neuen Wert des **Zeitplans** von `0 0 */1 * * *` ein, und klicken Sie anschließend auf **Speichern**.  

![Aktualisieren des Timerzeitplans von Funktionen im Azure-Portal](./media/functions-create-scheduled-function/functions-timer-trigger-change-schedule.png)

Ihre Funktion wird nun stündlich ausgeführt. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte
Sie haben eine Funktion erstellt, die nach einem Zeitplan ausgeführt wird. 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Weitere Informationen zu Triggern mit Timern finden Sie unter [Planen der Ausführung von Code mit Azure Functions](functions-bindings-timer.md). 




