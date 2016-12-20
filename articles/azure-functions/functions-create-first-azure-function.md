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
ms.date: 11/14/2016
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: ae5837b4fce52aad4c8b39434c27c450aafc1310
ms.openlocfilehash: 6f42f79abed80df12148463e25935893a4bbdcde


---
# <a name="create-your-first-azure-function"></a>Erstellen Sie Ihre erste Funktion in Azure Functions
## <a name="overview"></a>Übersicht
Azure-Funktionen ist eine ereignisgesteuerte On-Demand-Computeumgebung, mit der die vorhandene Azure-Anwendungsplattform um Funktionen zur Implementierung von Code erweitert wird, der von Ereignissen ausgelöst wird, die in anderen Azure-Diensten, SaaS-Produkten sowie in lokalen Systemen auftreten können. Mit Azure Functions werden Ihre Anwendungen basierend auf dem Bedarf skaliert, und Sie zahlen nur für die Ressourcen, die Sie tatsächlich nutzen. Azure Functions ermöglicht Ihnen das Erstellen von geplanten oder ausgelösten, in verschiedenen Programmiersprachen implementierten Codeeinheiten. Weitere Informationen zu Azure Functions finden Sie in der [Übersicht zu Azure Functions](functions-overview.md).

Dieses Thema behandelt die Verwendung des Azure Functions-Schnellstarts im Portal zum Erstellen einer einfachen Node.js-Funktion „Hello World“, die von einem HTTP-Trigger aufgerufen wird. Sie können sich auch ein kurzes Video ansehen, das die Ausführung dieser Schritte im Portal zeigt.

## <a name="watch-the-video"></a>Video ansehen
Das folgende Video zeigt die Ausführung der grundlegenden Schritte in diesem Tutorial. 

> [!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-your-first-Azure-Function-simple/player]
> 
> 

## <a name="create-a-function-from-the-quickstart"></a>Erstellen einer Funktion über den Schnellstart
Eine Funktionen-App hostet die Ausführung Ihrer Funktionen in Azure. Führen Sie diese Schritte aus, um eine Funktionen-App mit der neuen Funktion zu erstellen. Die Funktionen-App wird mit einer Standardkonfiguration erstellt. Ein Beispiel für die explizite Erstellung der Funktionen-App finden Sie im [anderen Schnellstart-Tutorial zu Azure Functions](functions-create-first-azure-function-azure-portal.md).

Bevor Sie Ihre erste Funktion erstellen können, müssen Sie über ein aktives Azure-Konto verfügen. Wenn Sie noch kein Azure-Konto haben, [erstellen Sie ein kostenloses Konto](https://azure.microsoft.com/free/).

1. Wechseln Sie zum [Azure Functions-Portal](https://functions.azure.com/signin) , und melden Sie sich mit Ihrem Azure-Konto an.
2. Geben Sie einen eindeutigen **Namen** für Ihre neue Funktionen-App ein, oder übernehmen Sie den generierten Namen, wählen Sie die bevorzugte **Region** aus, und klicken Sie anschließend auf **Erstellen und starten**. 
3. Klicken Sie auf der Registerkarte **Schnellstart** auf **WebHook + API**, auf **JavaScript** und anschließend auf **Funktion erstellen**. Eine neue vordefinierte Node.js-Funktion wird erstellt. 
   
    ![](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)
4. (Optional) Nun können Sie auswählen, ob Sie eine kurze Übersicht über die Features von Azure Functions im Portal erhalten möchten. Nachdem Sie die Schnelleinführung durchlaufen oder übersprungen haben, können Sie Ihre neue Funktion mit dem HTTP-Trigger testen.

## <a name="test-the-function"></a>Testen der Funktion
Da der Schnellstart von Azure Functions Funktionscode enthält, können Sie Ihre neue Funktion sofort testen.

1. Sehen Sie sich auf der Registerkarte **Entwickeln** das Fenster **Code** an. Wie Sie hier sehen können, erfordert dieser Node.js-Code eine HTTP-Anforderung mit einem Wert für *Name*, der entweder im Nachrichtentext oder in einer Abfragezeichenfolge übergeben wird. Wenn die Funktion ausgeführt wird, wird dieser Wert in der Antwortnachricht zurückgegeben.
   
2. Klicken Sie auf **Testen**, um den integrierten HTTP-Anforderungstestbereich für die Funktion anzuzeigen.
 
    ![](./media/functions-create-first-azure-function/function-app-develop-tab-testing.png)

2. Legen Sie im Textfeld **Anforderungstext** den Wert der Eigenschaft *name* auf Ihren Namen fest, und klicken Sie anschließend auf **Ausführen**. Die Ausführung wird durch eine HTTP-Testanforderung ausgelöst, Informationen werden in die Streamingprotokolle geschrieben, und die Antwort „hello“ wird in der **Ausgabe**angezeigt.
 
3. Wenn Sie die Ausführung der gleichen Funktion in einem anderen Browserfenster oder in einer anderen Browserregisterkarte auslösen möchten, kopieren Sie den Wert der **Funktions-URL** auf der Registerkarte **Entwickeln**, und fügen Sie ihn in die Adressleiste des Browsers ein. Hängen Sie anschließend den Abfragezeichenfolgewert `&name=yourname` an, und drücken Sie die EINGABETASTE. Dabei werden die gleichen Informationen protokolliert, und der Browser zeigt wie zuvor die Antwort „Hello“ an.

## <a name="next-steps"></a>Nächste Schritte
In dieser Schnellstartanleitung wird eine einfache Ausführung einer grundlegenden, über HTTP ausgelösten Funktion veranschaulicht. Weitere Informationen zur Verwendung von Azure Functions in Apps finden Sie unter den folgenden Themen:

* [Bewährte Methoden für Azure Functions](functions-best-practices.md)
* [Azure Functions developer reference (Azure Functions-Entwicklerreferenz) (Azure Functions-Entwicklerreferenz)](functions-reference.md)  
   Referenz zum Programmieren von Funktionen sowie zum Festlegen von Triggern und Bindungen.
* [Testing Azure Functions (Testen von Azure Functions) (Testen von Azure Functions)](functions-test-a-function.md)  
   Beschreibt verschiedene Tools und Techniken zum Testen Ihrer Funktionen
* [How to scale Azure Functions (Skalieren von Azure Functions) (Skalieren von Azure Functions)](functions-scale.md)  
  Beschreibt die für Azure Functions verfügbaren Servicepläne (einschließlich des Hostingplans „Verbrauchstarif“) und enthält Informationen zur Wahl des geeigneten Plans. 
* [Was ist Azure App Service?](../app-service/app-service-value-prop-what-is.md)  
   Azure Functions nutzt die Azure App Service-Plattform für Kernfunktionen wie Bereitstellungen, Umgebungsvariablen und Diagnosen. 

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]




<!--HONumber=Nov16_HO4-->


