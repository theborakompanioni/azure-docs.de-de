---
title: Erstellen Ihrer erste Funktion in Azure Functions | Microsoft Docs
description: Erstellen Sie erste Funktion in Azure Functions, einer serverlosen Anwendung, in weniger als zwei Minuten.
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 4a1669e7-233e-4ea2-9b83-b8624f2dbe59
ms.service: functions
ms.devlang: multiple
ms.topic: hero-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/14/2016
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: afe143848fae473d08dd33a3df4ab4ed92b731fa
ms.openlocfilehash: a797910e286cd2aacf5a8aa6c509e2b0f5f60276
ms.lasthandoff: 03/17/2017


---
# <a name="create-your-first-azure-function"></a>Erstellen Sie Ihre erste Funktion in Azure Functions

Dieses Thema behandelt die Verwendung des Azure Functions-Schnellstarts im Portal zum Erstellen einer einfachen Funktion „Hello World“, die von einer HTTP-Anforderung aufgerufen wird. Weitere Informationen zu Azure Functions finden Sie in der [Übersicht zu Azure Functions](functions-overview.md).

Sie müssen über ein Azure-Konto verfügen, um beginnen zu können. Es sind [kostenlose Konten](https://azure.microsoft.com/free/) verfügbar. Sie können auch [Azure Functions ausprobieren](https://azure.microsoft.com/try/app-service/functions/), ohne dass eine Registrierung für Azure erforderlich ist.

## <a name="create-a-function-from-the-portal-quickstart"></a>Erstellen einer Funktion über den Schnellstart im Portal

1. Wechseln Sie zum [Azure Functions-Portal](https://functions.azure.com/signin), und melden Sie sich mit Ihrem Azure-Konto an. 
 
2. Geben Sie einen eindeutigen **Namen** für Ihre neue Funktionen-App ein, oder übernehmen Sie den automatisch generierten Namen, wählen Sie die bevorzugte **Region** aus, und klicken Sie anschließend auf **Erstellen und erste Schritte durchführen**. Ein gültiger Name darf nur Buchstaben, Zahlen und Bindestriche enthalten. Unterstriche (**_**) sind nicht zulässig.

3. Klicken Sie auf der Registerkarte **Schnellstart** auf **WebHook + API**, und wählen Sie eine Sprache für Ihre Funktion aus. Klicken Sie anschließend auf **Create a function** (Funktion erstellen). In Ihrer gewählten Sprache wird eine vordefinierte Funktion erstellt. 
   
    ![](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)

4. (Optional) Nun können Sie auswählen, ob Sie eine kurze Übersicht über die Features von Azure Functions im Portal erhalten möchten. Nachdem Sie die Schnelleinführung durchlaufen oder übersprungen haben, können Sie Ihre neue Funktion testen, indem Sie eine HTTP-Anforderung senden.

## <a name="test-the-function"></a>Testen der Funktion
[!INCLUDE [Functions quickstart test](../../includes/functions-quickstart-test.md)]

## <a name="watch-the-video"></a>Video ansehen
Das folgende Video zeigt die Ausführung der grundlegenden Schritte in diesem Tutorial. 

> [!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-your-first-Azure-Function-simple/player]
> 


## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [Functions quickstart next steps](../../includes/functions-quickstart-next-steps.md)]

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]


