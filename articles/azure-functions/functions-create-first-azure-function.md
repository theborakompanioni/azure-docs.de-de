---
title: Erstellen Ihrer ersten Funktion im Azure-Portal | Microsoft Docs
description: Willkommen bei Azure. Erstellen Sie Ihre erste Azure-Funktion im Azure-Portal.
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 96cf87b9-8db6-41a8-863a-abb828e3d06d
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/10/2017
ms.author: glenga
ms.custom: welcome-email
ROBOTS: NOINDEX, NOFOLLOW
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 7542280ca6dbe1a8d110155e521228d675c0d994
ms.lasthandoff: 04/12/2017


---
# <a name="create-your-first-function-in-the-azure-portal"></a>Erstellen Ihrer ersten Funktion im Azure-Portal

In diesem Thema wird veranschaulicht, wie Sie mithilfe von Azure Functions eine „hello world“-Funktion erstellen, die über eine HTTP-Anforderung aufgerufen wird. Damit Sie eine Funktion im Azure-Portal erstellen können, müssen Sie eine Funktionen-App erstellen, die die serverlose Ausführung der Funktion hostet.

Für diesen Schnellstart benötigen Sie ein Azure-Konto. Es sind [kostenlose Konten](https://azure.microsoft.com/free/) verfügbar. Sie können auch [Azure Functions ausprobieren](https://azure.microsoft.com/try/app-service/functions/), ohne dass eine Registrierung für Azure erforderlich ist.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.

## <a name="create-a-function-app"></a>Erstellen einer Funktionen-App

[!INCLUDE [functions-create-function-app-portal](../../includes/functions-create-function-app-portal.md)]

Weitere Informationen finden Sie unter [Erstellen einer Funktionen-App über das Azure-Portal](functions-create-function-app-portal.md).

## <a name="create-a-function"></a>Erstellen einer Funktion
Mit diesen Schritten wird mithilfe des Schnellstarts von Azure Functions eine Funktion in der neuen Funktionen-App erstellt.

1. Klicken Sie auf die Schaltfläche **Neu** und anschließend auf **WebHook + API**, wählen Sie eine Sprache für die Funktion aus, und klicken Sie dann auf **Diese Funktion erstellen**. Es wird eine Funktion in der ausgewählten Sprache mit der über HTTP ausgelösten Funktionsvorlage erstellt.  
   
    ![](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)

Nachdem die Funktion erstellt wurde, können Sie sie durch Senden einer HTTP-Anforderung testen.

## <a name="test-the-function"></a>Testen der Funktion

Da die Funktionsvorlagen gültigen Code enthalten, können Sie Ihre neue Funktion direkt im Portal testen.

1. Klicken Sie in der Funktionen-App auf die neue Funktion und prüfen Sie den Code aus der Vorlage. Sie stellen fest, dass die Funktion eine HTTP-Anforderung erwartet, deren *name*-Wert entweder im Nachrichtentext oder in einer Abfragezeichenfolge übergeben wird. Wenn die Funktion ausgeführt wird, wird dieser Wert in der Antwortnachricht zurückgegeben. Das Beispiel unten ist eine JavaScript-Funktion.
   
2. Klicken Sie auf **Ausführen**, um die Funktion auszuführen. Die Ausführung wird durch eine HTTP-Testanforderung ausgelöst, Informationen werden in die Protokolle geschrieben, und die Antwort „hello...“ wird in der **Ausgabe** auf der Registerkarte **Test** angezeigt.
 
    ![](./media/functions-create-first-azure-function/function-app-develop-tab-testing.png)

3. Legen Sie im Textfeld **Anforderungstext** den Wert der Eigenschaft *name* auf Ihren Namen fest, und klicken Sie erneut auf **Ausführen**. Dieses Mal enthält die Antwort in der **Ausgabe** Ihren Namen.   

4. Wenn Sie die Ausführung der gleichen Funktion über ein HTTP-Testtool oder in einem anderen Browserfenster auslösen möchten, klicken Sie auf **</> Funktions-URL abrufen**, kopieren Sie die Anforderungs-URL, und fügen Sie sie in das Tool bzw. die Adressleiste des Browsers ein. Hängen Sie anschließend den Abfragezeichenfolgewert `&name=yourname` an, und führen Sie die Anforderung aus. Die gleichen Informationen werden in die Protokolle geschrieben, und der Text der Antwortnachricht enthält die gleiche Zeichenfolge.

    ![](./media/functions-create-first-azure-function/function-app-browser-testing.png)


## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [Functions quickstart next steps](../../includes/functions-quickstart-next-steps.md)]

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]


