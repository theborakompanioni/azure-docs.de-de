---
title: "Erstellen einer Funktion über das Azure-Portal | Microsoft-Dokumentation"
description: Erstellen Sie erste Funktion in Azure Functions, einer serverlosen Anwendung, in weniger als zwei Minuten.
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
ms.date: 02/02/2017
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: 1e6ae31b3ef2d9baf578b199233e61936aa3528e
ms.openlocfilehash: 4ee741cbec8db6b6400ff9f27daa2a0120bd2618
ms.lasthandoff: 03/03/2017


---
# <a name="create-a-function-from-the-azure-portal"></a>Erstellen einer Funktion im Azure-Portal
## <a name="overview"></a>Übersicht
Azure-Funktionen ist eine ereignisgesteuerte On-Demand-Computeumgebung, mit der die vorhandene Azure-Anwendungsplattform um Funktionen zur Implementierung von Code erweitert wird, der von Ereignissen ausgelöst wird, die in anderen Azure-Diensten, SaaS-Produkten sowie in lokalen Systemen auftreten können. Mit Azure Functions werden Ihre Anwendungen basierend auf dem Bedarf skaliert, und Sie zahlen nur für die Ressourcen, die Sie tatsächlich nutzen. Azure Functions ermöglicht Ihnen das Erstellen von geplanten oder ausgelösten, in verschiedenen Programmiersprachen implementierten Codeeinheiten. Weitere Informationen zu Azure Functions finden Sie in der [Übersicht zu Azure Functions](functions-overview.md).

In diesem Thema erfahren Sie, wie Sie über das Azure-Portal eine einfache Node.js-Azure-Funktion vom Typ „hello world“ erstellen, die von einem HTTP-Trigger aufgerufen wird. Bevor Sie eine Funktion im Azure-Portal erstellen können, müssen Sie explizit eine Funktionen-App in Azure App Service erstellen. Informationen dazu, wie Sie die Funktionen-App automatisch erstellen lassen, finden Sie im [anderen Schnellstart-Tutorial zu Azure Functions](functions-create-first-azure-function.md), das eine einfachere Erläuterung für den Schnellstart bietet und ein Video umfasst.

## <a name="create-a-function-app"></a>Erstellen einer Funktionen-App
Eine Funktions-App hostet die Ausführung Ihrer Funktionen in Azure. Falls Sie noch kein Azure-Konto besitzen, sehen Sie sich die Seite zum Ausprobieren von Funktionen ([Try Functions](https://functions.azure.com/try)) an, oder [erstellen Sie ein kostenloses Azure-Konto](https://azure.microsoft.com/free/). Führen Sie die folgenden Schritte aus, um eine Funktionen-App im Azure-Portal zu erstellen.

1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com) , und melden Sie sich mit Ihrem Azure-Konto an.
2. Klicken Sie auf **+Neu** > **Compute** > **Funktionen-App**, wählen Sie Ihr **Abonnement** aus, geben Sie einen eindeutigen **App-Namen** zur Identifizierung Ihrer Funktionen-App ein, und geben Sie anschließend folgende Einstellungen an:
   
   * **[Ressourcengruppe](../azure-resource-manager/resource-group-overview.md)**: Wählen Sie **Neu erstellen** aus, und geben Sie einen Namen für die neue Ressourcengruppe ein. Sie können auch eine vorhandene Ressourcengruppe auswählen. Möglicherweise können Sie jedoch keinen verbrauchsbasierten App Service-Plan für Ihre Funktionen-App erstellen.
   * **[Hostingplan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)**. Mögliche Optionen: 
     * **Verbrauchsplan**: Der Standardplantyp für Azure Functions. Wenn Sie einen Verbrauchsplan auswählen, müssen Sie auch den **Standort** und die **Speicherbelegung** (in MB) festlegen. Informationen zu den Auswirkungen der Speicherbelegung auf die Kosten finden Sie unter [Azure Functions – Preise](https://azure.microsoft.com/pricing/details/functions/). 
     * **App Service-Plan**: Bei Verwendung eines App Service-Plans müssen Sie einen **App Service-Plan/Standort** erstellen oder einen bereits vorhandenen App Service-Plan/Standort auswählen. Diese Einstellungen bestimmen den [Standort, die Funktionen, die Kosten und die Computeressourcen](https://azure.microsoft.com/pricing/details/app-service/) Ihrer App.  
   * **Speicherkonto**: Für jede Funktionen-App ist ein Speicherkonto erforderlich. Sie können ein vorhandenes Speicherkonto auswählen oder ein neues erstellen. 
     
     ![Erstellen einer neuen Funktionen-App im Azure-Portal](./media/functions-create-first-azure-function-azure-portal/function-app-create-flow.png)

    Beachten Sie, dass Sie einen gültigen **App-Namen** eingeben müssen, der nur Buchstaben, Zahlen und Bindestriche enthalten kann. Unterstriche (**_**) sind nicht zulässig.

3. Klicken Sie auf **Erstellen** , um die neue Funktionen-App bereitzustellen.  

### <a name="storage-account-requirements"></a>Anforderungen an das Speicherkonto

Beim Erstellen einer Funktionen-App müssen Sie ein allgemeines Azure Storage-Konto erstellen oder verknüpfen, das Blob-, Warteschlangen- und Tabellenspeicher unterstützt. Intern verwendet Azure Functions Azure Storage für Vorgänge wie das Verwalten von Triggern und Ausführungen von Protokollierfunktionen. Manche Speicherkonten unterstützen keine Warteschlangen und Tabellen, wie z.B. reine Blobspeicherkonten (darunter Storage Premium) und allgemeine Speicherkonten mit Replikation von ZRS. Diese Konten werden aus dem Blatt „Speicherkonto“ herausgefiltert, wenn eine neue Funktionen-App erstellt wird.
Wenn der verbrauchsbasierte Hostingplan verwendet wird, wird der Inhalt von Funktionen-Apps (z.B. Funktionscodedateien und Bindungskonfigurationen) in Azure Files-Freigaben auf dem Hauptspeicherkonto gespeichert. Wenn Sie das Hauptspeicherkonto löschen, wird dieser Inhalt ebenfalls gelöscht und kann nicht wiederhergestellt werden.

Weitere Informationen zu Typen von Speicherkonten finden Sie unter [Einführung in die Azure Storage-Dienste] (../storage/storage-introduction.md#introducing-the-azure-storage-services).

## <a name="create-a-function"></a>Erstellen einer Funktion
In diesen Schritten wird mithilfe des Schnellstarts von Azure Functions eine Funktion erstellt.

1. Klicken Sie auf der Registerkarte **Schnellstart** auf **WebHook + API**, auf **JavaScript** und anschließend auf **Funktion erstellen**. Eine neue vordefinierte Node.js-Funktion wird erstellt. 
   
    ![](./media/functions-create-first-azure-function-azure-portal/function-app-quickstart-node-webhook.png)

2. (Optional) Nun können Sie auswählen, ob Sie eine kurze Übersicht über die Features von Azure Functions im Portal erhalten möchten.    Nachdem Sie die Schnelleinführung durchlaufen oder übersprungen haben, können Sie Ihre neue Funktion mit dem HTTP-Trigger testen.


## <a name="test-the-function"></a>Testen der Funktion
[!INCLUDE [Functions quickstart test](../../includes/functions-quickstart-test.md)]

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [Functions quickstart next steps](../../includes/functions-quickstart-next-steps.md)]

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]


