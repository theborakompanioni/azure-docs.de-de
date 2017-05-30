---
title: "Erstellen einer Funktion in Azure, die durch Warteschlangenmeldungen ausgelöst wird | Microsoft-Dokumentation"
description: Erstellen Sie mithilfe von Azure Functions eine serverlose Funktion, die durch Meldungen an eine Azure Storage-Warteschlange aufgerufen wird.
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 361da2a4-15d1-4903-bdc4-cc4b27fc3ff4
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/25/2017
ms.author: glenga
ms.translationtype: Human Translation
ms.sourcegitcommit: a30a90682948b657fb31dd14101172282988cbf0
ms.openlocfilehash: 42fabff59577db5feb98e5c03d7a2b6d3d2461c3
ms.contentlocale: de-de
ms.lasthandoff: 05/25/2017


---
# <a name="create-a-function-triggered-by-azure-queue-storage"></a>Erstellen einer Funktion, die durch Azure Queue Storage ausgelöst wird

Erfahren Sie, wie Sie eine Funktion erstellen, die ausgelöst wird, wenn Meldungen an eine Azure Storage-Warteschlange gesendet werden.

![Zeigen Sie die Meldung in den Protokollen an.](./media/functions-create-storage-queue-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Voraussetzungen

- Laden Sie den [Microsoft Azure Storage Explorer](http://storageexplorer.com/) herunter, und installieren Sie ihn.

- Sie benötigen ein Azure-Abonnement. Wenn Sie keins besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Erstellen einer Azure Function-App

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Die Funktionen-App wurde erfolgreich erstellt.](./media/functions-create-first-azure-function/function-app-create-success.png)

Erstellen Sie als Nächstes in der neuen Funktionen-App eine Funktion.

<a name="create-function"></a>

## <a name="create-a-queue-triggered-function"></a>Erstellen einer Funktion mit Auslösung durch Warteschlange

Erweitern Sie Ihre Funktionen-App, klicken Sie neben den **Funktionen** auf die Schaltfläche **+**, und klicken Sie anschließend auf die Vorlage **QueueTrigger** für die gewünschte Sprache. Verwenden Sie dann die in der Tabelle angegebenen Einstellungen, und klicken Sie anschließend auf **Erstellen**.

![Erstellen Sie die Funktion, die durch die Speicherwarteschlange ausgelöst wird.](./media/functions-create-storage-queue-triggered-function/functions-create-queue-storage-trigger-portal.png)

| Einstellung | Empfohlener Wert | Beschreibung |
|---|---|---|
| **Warteschlangenname**   | myqueue-items    | Der Name der zu verknüpfenden Warteschlange in Ihrem Speicherkonto. |
| **Speicherkontoverbindung** | AzureWebJobStorage | Sie können die Speicherkontoverbindung verwenden, die bereits von Ihrer Funktionen-App verwendet wird, oder eine neue erstellen.  |
| **Name Ihrer Funktion** | Eindeutig in Ihrer Funktionen-App | Der Name dieser Funktion mit Auslösung durch Warteschlange. |

Als Nächstes stellen Sie eine Verbindung zu Ihrem Azure Storage-Konto her und erstellen die **myqueue-items**-Speicherwarteschlange.

## <a name="create-the-queue"></a>Erstellen der Warteschlange

1. Klicken Sie in Ihrer Funktion auf **Integrieren**, erweitern Sie **Dokumentation**, und kopieren Sie **Kontoname** und **Kontoschlüssel**. Diese Anmeldeinformationen benötigen Sie für die Verbindung mit dem Speicherkonto. Wenn Sie bereits eine Verbindung zu Ihrem Speicherkonto hergestellt haben, fahren Sie mit Schritt 4 fort.

    ![Beschaffen Sie die Anmeldeinformationen für die Speicherkontenverbindung.](./media/functions-create-storage-queue-triggered-function/functions-storage-account-connection.png)v

1. Führen Sie das Tool [Microsoft Azure Storage-Explorer](http://storageexplorer.com/) aus, klicken Sie auf das Verbindungssymbol auf der linken Seite, wählen Sie **Use a storage account name and key** (Name und Schlüssel eines Speicherkontos verwenden) aus, und klicken Sie anschließend auf **Weiter**.

    ![Führen Sie das Tool Storage Account-Explorer aus.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-1.png)

1. Geben Sie den **Kontonamen** und den **Kontoschlüssel** aus Schritt 1 ein, klicken Sie auf **Weiter** und dann auf **Verbinden**.

    ![Geben Sie die Speicheranmeldeinformationen ein, und stellen Sie eine Verbindung her.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-2.png)

1. Erweitern Sie das angefügte Speicherkonto, klicken Sie mit der rechten Maustaste auf **Warteschlangen**, und klicken Sie dann auf **Warteschlange erstellen**. Geben Sie `myqueue-items` ein, und drücken Sie dann die EINGABETASTE.

    ![Erstellen Sie eine Speicherwarteschlange.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-create-queue.png)

Nun verfügen Sie über eine Speicherwarteschlange und können die Funktion durch Hinzufügen einer Meldung in die Warteschlange testen.

## <a name="test-the-function"></a>Testen der Funktion

1. Gehen Sie zurück zum Azure-Portal, navigieren Sie zu Ihrer Funktion, erweitern Sie die **Protokolle** am unteren Rand der Seite, und stellen Sie sicher, dass das Protokollstreaming nicht angehalten ist.

1. Erweitern Sie im Storage-Explorer Ihr Speicherkonto, **Warteschlangen** und **myqueue-items**, und klicken Sie dann auf **Meldung hinzufügen**.

    ![Fügen Sie eine Meldung in die Warteschlange hinzu.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-add-message.png)

1. Geben Sie die Meldung "Hello World!" in den **Meldungstext** ein, und klicken Sie auf **OK**.

1. Warten Sie einige Sekunden, wechseln Sie dann zurück zu den Funktionsprotokollen, und stellen Sie sicher, dass die neue Meldung aus der Warteschlange gelesen wurde.

    ![Zeigen Sie die Meldung in den Protokollen an.](./media/functions-create-storage-queue-triggered-function/functions-queue-storage-trigger-view-logs.png)

1. Gehen Sie zurück zum Storage-Explorer, klicken Sie auf **Aktualisieren**, und stellen Sie sicher, dass die Meldung verarbeitet wurde und sich nicht mehr in der Warteschlange befindet.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie haben eine Funktion erstellt, die ausgeführt wird, wenn eine Meldung in eine Speicherwarteschlange hinzugefügt wird.

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Weitere Informationen zu Queue Storage-Auslösern finden Sie unter [Azure Storage-Warteschlangenbindungen in Azure Functions](functions-bindings-storage-queue.md).
