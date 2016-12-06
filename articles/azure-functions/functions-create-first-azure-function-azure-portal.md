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
ms.date: 11/14/2016
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: 638054fb49ace081a7d67817d9dcecb8f5ca1a94
ms.openlocfilehash: b2ad194b62b61e3b16d660718aee155ff2dbf197


---
# <a name="create-a-function-from-the-azure-portal"></a>Erstellen einer Funktion im Azure-Portal
## <a name="overview"></a>Übersicht
Azure-Funktionen ist eine ereignisgesteuerte On-Demand-Computeumgebung, mit der die vorhandene Azure-Anwendungsplattform um Funktionen zur Implementierung von Code erweitert wird, der von Ereignissen ausgelöst wird, die in anderen Azure-Diensten, SaaS-Produkten sowie in lokalen Systemen auftreten können. Mit Azure Functions werden Ihre Anwendungen basierend auf dem Bedarf skaliert, und Sie zahlen nur für die Ressourcen, die Sie tatsächlich nutzen. Azure Functions ermöglicht Ihnen das Erstellen von geplanten oder ausgelösten, in verschiedenen Programmiersprachen implementierten Codeeinheiten. Weitere Informationen zu Azure Functions finden Sie in der [Übersicht zu Azure Functions](functions-overview.md).

In diesem Thema erfahren Sie, wie Sie über das Azure-Portal eine einfache Node.js-Azure-Funktion vom Typ „hello world“ erstellen, die von einem HTTP-Trigger aufgerufen wird. Bevor Sie eine Funktion im Azure-Portal erstellen können, müssen Sie explizit eine Funktionen-App in Azure App Service erstellen. Informationen dazu, wie Sie die Funktionen-App automatisch erstellen lassen, finden Sie im [anderen Schnellstart-Tutorial zu Azure Functions](functions-create-first-azure-function.md), das eine einfachere Erläuterung für den Schnellstart bietet und ein Video umfasst.

## <a name="create-a-function-app"></a>Erstellen einer Funktionen-App
Eine Funktionen-App hostet die Ausführung Ihrer Funktionen in Azure. Führen Sie die folgenden Schritte aus, um eine Funktionen-App im Azure-Portal zu erstellen.

Zum Erstellen Ihrer ersten Funktion benötigen Sie ein aktives Azure-Konto. Falls Sie noch kein Azure-Konto haben, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/).

1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com) , und melden Sie sich mit Ihrem Azure-Konto an.
2. Klicken Sie auf **+Neu** > **Compute** > **Funktionen-App**, wählen Sie Ihr **Abonnement** aus, geben Sie einen eindeutigen **App-Namen** zur Identifizierung Ihrer Funktionen-App ein, und geben Sie anschließend folgende Einstellungen an:
   
   * **[Ressourcengruppe](../azure-resource-manager/resource-group-overview.md)**: Wählen Sie **Neu erstellen** aus, und geben Sie einen Namen für die neue Ressourcengruppe ein. Sie können auch eine vorhandene Ressourcengruppe auswählen. Möglicherweise können Sie jedoch keinen dynamischen App Service-Plan für Ihre Funktionen-App erstellen.
   * **[Hostingplan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)**. Mögliche Optionen: 
     * **Dynamischer Plan**: Der Standardplantyp für Azure Functions. Wenn Sie einen dynamischen Plan auswählen, müssen Sie auch den **Standort** und die **Speicherbelegung** (in MB) festlegen. Informationen zu den Auswirkungen der Speicherbelegung auf die Kosten finden Sie unter [Azure Functions – Preise](https://azure.microsoft.com/pricing/details/functions/). 
     * **App Service-Plan**: Bei Verwendung eines App Service-Plans müssen Sie einen **App Service-Plan/Standort** erstellen oder einen bereits vorhandenen App Service-Plan/Standort auswählen. Diese Einstellungen bestimmen den [Standort, die Funktionen, die Kosten und die Computeressourcen](https://azure.microsoft.com/pricing/details/app-service/) Ihrer App.  
   * **Speicherkonto**: Für jede Funktionen-App ist ein Speicherkonto erforderlich. Sie können ein vorhandenes Speicherkonto auswählen oder ein neues erstellen. 
     
     ![Erstellen einer neuen Funktionen-App im Azure-Portal](./media/functions-create-first-azure-function-azure-portal/function-app-create-flow.png)

3. Klicken Sie auf **Erstellen** , um die neue Funktionen-App bereitzustellen.  

Nach der Bereitstellung der Funktionen-App können Sie Ihre erste Funktion erstellen.

## <a name="create-a-function"></a>Erstellen einer Funktion
In diesen Schritten wird mithilfe des Schnellstarts von Azure Functions eine Funktion erstellt.

1. Klicken Sie auf der Registerkarte **Schnellstart** auf **WebHook + API**, auf **JavaScript** und anschließend auf **Funktion erstellen**. Eine neue vordefinierte Node.js-Funktion wird erstellt. 
   
    ![](./media/functions-create-first-azure-function-azure-portal/function-app-quickstart-node-webhook.png)

2. (Optional) Nun können Sie auswählen, ob Sie eine kurze Übersicht über die Features von Azure Functions im Portal erhalten möchten.    Nachdem Sie die Schnelleinführung durchlaufen oder übersprungen haben, können Sie Ihre neue Funktion mit dem HTTP-Trigger testen.

## <a name="test-the-function"></a>Testen der Funktion
Da der Schnellstart von Azure Functions Funktionscode enthält, können Sie Ihre neue Funktion sofort testen.

1. Sehen Sie sich auf der Registerkarte **Entwickeln** das Fenster **Code** an. Wie Sie hier sehen können, erfordert dieser Node.js-Code eine HTTP-Anforderung mit einem Wert für *Name*, der entweder im Nachrichtentext oder in einer Abfragezeichenfolge übergeben wird. Wenn die Funktion ausgeführt wird, wird dieser Wert in der Antwortnachricht zurückgegeben.
   
2. Klicken Sie auf **Testen**, um den integrierten HTTP-Anforderungstestbereich für die Funktion anzuzeigen.
 
    ![](./media/functions-create-first-azure-function-azure-portal/function-app-develop-tab-testing.png)

2. Legen Sie im Textfeld **Anforderungstext** den Wert der Eigenschaft *name* auf Ihren Namen fest, und klicken Sie anschließend auf **Ausführen**. Die Ausführung wird durch eine HTTP-Testanforderung ausgelöst, Informationen werden in die Streamingprotokolle geschrieben, und die Antwort „hello“ wird in der **Ausgabe**angezeigt. 

3. Wenn Sie die Ausführung der gleichen Funktion in einem anderen Browserfenster oder in einer anderen Browserregisterkarte auslösen möchten, kopieren Sie den Wert der **Funktions-URL** auf der Registerkarte **Entwickeln**, und fügen Sie ihn in die Adressleiste des Browsers ein. Hängen Sie anschließend den Abfragezeichenfolgewert `&name=yourname` an, und drücken Sie die EINGABETASTE. Dabei werden die gleichen Informationen protokolliert, und der Browser zeigt wie zuvor die Antwort „Hello“ an.

## <a name="next-steps"></a>Nächste Schritte
In dieser Schnellstartanleitung wird eine einfache Ausführung einer grundlegenden, über HTTP ausgelösten Funktion veranschaulicht. Weitere Informationen zur Verwendung von Azure Functions in Apps finden Sie unter den folgenden Themen:

* [Azure Functions developer reference (Azure Functions-Entwicklerreferenz) (Azure Functions-Entwicklerreferenz)](functions-reference.md)  
   Referenz zum Programmieren von Funktionen sowie zum Festlegen von Triggern und Bindungen.
* [Testing Azure Functions (Testen von Azure Functions) (Testen von Azure Functions)](functions-test-a-function.md)  
   Beschreibt verschiedene Tools und Techniken zum Testen Ihrer Funktionen
* [How to scale Azure Functions (Skalieren von Azure Functions) (Skalieren von Azure Functions)](functions-scale.md)  
  Beschreibt die für Azure Functions verfügbaren Servicepläne (einschließlich des dynamischen Serviceplans) und enthält Informationen zur Wahl des geeigneten Plans. 
* [Was ist Azure App Service?](../app-service/app-service-value-prop-what-is.md)  
   Azure Functions nutzt die Azure App Service-Plattform für Kernfunktionen wie Bereitstellungen, Umgebungsvariablen und Diagnosen. 

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]




<!--HONumber=Nov16_HO3-->


