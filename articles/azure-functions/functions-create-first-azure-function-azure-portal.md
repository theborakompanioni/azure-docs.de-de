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
ms.date: 03/15/2017
ms.author: glenga
ms.custom: welcome-email
ROBOTS: NOINDEX, NOFOLLOW
translationtype: Human Translation
ms.sourcegitcommit: 1429bf0d06843da4743bd299e65ed2e818be199d
ms.openlocfilehash: 6bb15dfd0e2c40b99d06ef2d695e3ba7ca78e91b
ms.lasthandoff: 03/22/2017


---
# <a name="create-your-first-function-in-the-azure-portal"></a>Erstellen Ihrer ersten Funktion im Azure-Portal

In diesem Thema wird erläutert, wie Sie eine einfache Azure-Funktion vom Typ „hello world“ erstellen, die über eine HTTP-Anforderung aufgerufen wird. Damit Sie eine Funktion im Azure-Portal erstellen können, müssen Sie in Azure App Service eine Funktionen-App erstellen, die die Ausführung der Funktion hostet.

Für diesen Schnellstart benötigen Sie ein Azure-Konto. Es sind [kostenlose Konten](https://azure.microsoft.com/free/) verfügbar. Sie können auch [Azure Functions ausprobieren](https://azure.microsoft.com/try/app-service/functions/), ohne dass eine Registrierung für Azure erforderlich ist.

## <a name="create-a-function-app"></a>Erstellen einer Funktionen-App

[!INCLUDE [functions-create-function-app-portal](../../includes/functions-create-function-app-portal.md)]

Weitere Informationen finden Sie unter [Erstellen einer Funktionen-App über das Azure-Portal](functions-create-function-app-portal.md).

## <a name="create-a-function"></a>Erstellen einer Funktion
Mit diesen Schritten wird mithilfe des Schnellstarts von Azure Functions eine Funktion in der neuen Funktionen-App erstellt.

1. Klicken Sie auf der Registerkarte **Schnellstart** auf **WebHook + API**, und wählen Sie eine Sprache für Ihre Funktion aus. Klicken Sie anschließend auf **Diese Funktion erstellen**. In Ihrer gewählten Sprache wird eine vordefinierte Funktion erstellt.  
   
    ![](./media/functions-create-first-azure-function-azure-portal/function-app-quickstart-node-webhook.png)

4. (Optional) Nun können Sie auswählen, ob Sie eine kurze Übersicht über die Features von Azure Functions im Portal erhalten möchten. Nachdem Sie die Schnelleinführung durchlaufen oder übersprungen haben, können Sie Ihre neue Funktion testen, indem Sie eine HTTP-Anforderung senden.


## <a name="test-the-function"></a>Testen der Funktion
[!INCLUDE [Functions quickstart test](../../includes/functions-quickstart-test.md)]

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [Functions quickstart next steps](../../includes/functions-quickstart-next-steps.md)]

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]


