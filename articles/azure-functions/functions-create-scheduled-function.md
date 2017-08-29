---
title: "Erstellen einer Funktion in Azure, die nach einem Zeitplan ausgeführt wird | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie in Azure eine Funktion erstellen können, die nach einem von Ihnen definierten Zeitplan ausgeführt wird."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: ba50ee47-58e0-4972-b67b-828f2dc48701
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/31/2017
ms.author: glenga
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 09f24fa2b55d298cfbbf3de71334de579fbf2ecd
ms.openlocfilehash: 4442d0038a0604d3297871907c1d05d8d3916dcf
ms.contentlocale: de-de
ms.lasthandoff: 06/07/2017

---
# <a name="create-a-function-in-azure-that-is-triggered-by-a-timer"></a>Erstellen einer Funktion in Azure, die von einem Timer ausgelöst wird

Erfahren Sie, wie Sie mithilfe von Azure Functions eine Funktion erstellen können, die nach einem von Ihnen definierten Zeitplan ausgeführt wird.

![Erstellen einer Funktionen-App im Azure-Portal](./media/functions-create-scheduled-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

+ Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Erstellen einer Azure Function-App

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Die Funktionen-App wurde erfolgreich erstellt.](./media/functions-create-first-azure-function/function-app-create-success.png)

Erstellen Sie als Nächstes in der neuen Funktionen-App eine Funktion.

<a name="create-function"></a>

## <a name="create-a-timer-triggered-function"></a>Erstellen einer Funktion mit Auslösung per Timer

1. Erweitern Sie die Funktionen-App, und klicken Sie auf die Schaltfläche **+** neben **Functions**. Wenn dies die erste Funktion in Ihrer Funktionen-App ist, wählen Sie **Benutzerdefinierte Funktion**. Hiermit wird der vollständige Satz von Funktionsvorlagen angezeigt.

    ![Schnellstartseite für Funktionen im Azure-Portal](./media/functions-create-scheduled-function/add-first-function.png)

2. Wählen Sie die Vorlage **TimerTrigger** für die gewünschte Sprache. Verwenden Sie anschließend die Proxyeinstellungen wie in der Tabelle angegeben:

    ![Erstellen Sie im Azure-Portal eine Funktion mit Auslösung per Timer.](./media/functions-create-scheduled-function/functions-create-timer-trigger.png)

    | Einstellung | Empfohlener Wert | Beschreibung |
    |---|---|---|
    | **Name Ihrer Funktion** | TimerTriggerCSharp1 | Definiert den Namen der Funktion mit Auslösung per Timer |
    | **[Zeitplan](http://en.wikipedia.org/wiki/Cron#CRON_expression)** | 0 \*/1 \* \* \* \* | Ein sechs Felder umfassender [CRON-Ausdruck](http://en.wikipedia.org/wiki/Cron#CRON_expression), der die minütliche Ausführung der Funktion festlegt. |

2. Klicken Sie auf **Erstellen**. Eine Funktion wird in der gewählten Sprache erstellt. Sie wird minütlich ausgeführt.

3. Überprüfen Sie, ob die Funktion ausgeführt wird, indem Sie sich die Nachverfolgungsinformationen in den Protokollen ansehen.

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
