---
title: Entwickeln von Azure Functions mithilfe von Visual Studio| Microsoft-Dokumentation
description: "In diesem Artikel erhalten Sie Informationen über das Entwickeln und Testen von Azure-Funktionen mithilfe von Azure Functions-Tools für Visual Studio 2017."
services: functions
documentationcenter: .net
author: ggailey777
manager: erikre
editor: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: glenga, donnam
ms.translationtype: HT
ms.sourcegitcommit: c30998a77071242d985737e55a7dc2c0bf70b947
ms.openlocfilehash: fab2b3042faf870fe4b9d09d5e96dadb7155c450
ms.contentlocale: de-de
ms.lasthandoff: 08/02/2017

---
# <a name="azure-functions-tools-for-visual-studio"></a>Azure Functions-Tools für Visual Studio  

Azure Functions-Tools für Visual Studio 2017 ist eine Erweiterung für Visual Studio, mit der Sie C#-Funktionen entwickeln, testen und in Azure bereitstellen können. Wenn Sie zum ersten mal mit Azure Functions arbeiten, finden Sie unter [Einführung in Azure Functions](functions-overview.md) weitere Informationen.

Azure Functions-Tools bieten folgende Vorteile: 

* Bearbeiten, Erstellen und Ausführen von Funktionen auf Ihrem lokalen Entwicklungscomputer. 
* Veröffentlichen von Azure Functions-Projekten direkt in Azure. 
* Verwenden von WebJobs-Attributen zum Deklarieren von Funktionsbindungen direkt in C#-Code. Sie müssen keine separate function.json-Datei für Bindungsdefinitionen mehr beibehalten.
* Entwickeln und Bereitstellen vorkompilierter C#-Funktionen. Vorkompilierte Funktionen bieten eine bessere Leistung beim Kaltstart als auf skriptbasierende Funktionen von C#. 
* Codieren Ihrer Funktionen in C# während Sie von allen Vorteilen der Entwicklung mit Visual Studio profitieren. 

In diesem Thema wird gezeigt, wie Sie mithilfe von Azure Functions-Tools für Visual Studio 2017 Ihre Funktionen in C# entwickeln. Außerdem erfahren Sie, wie Sie Ihr Projekt in Azure als .NET-Assembly veröffentlichen.

## <a name="prerequisites"></a>Voraussetzungen

Vor der Installation der Azure Functions-Tools muss [Visual Studio 2017, Version 15.3 (Vorschau)](https://www.visualstudio.com/vs/preview/) inklusive eine der folgenden Workloads installiert sein:

* Azure-Entwicklung
* ASP.NET und Webentwicklung

Zum Erstellen und Bereitstellen von Funktionen benötigen Sie ebenso:

* Ein aktives Azure-Abonnement. Wenn Sie noch kein Azure-Abonnement haben, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen.

* Azure Storage-Konto Informationen zum Erstellen eines Speicherkontos finden Sie unter [Erstellen Sie ein Speicherkonto](../storage/storage-create-storage-account.md#create-a-storage-account).  

## <a name="install-the-azure-functions-tools"></a>Installieren von Azure Functions-Tools

Sie können das [Erweiterungspaket herunterladen und installieren](https://marketplace.visualstudio.com/vsgallery/e3705d94-7cc3-4b79-ba7b-f43f30774d28) oder die folgenden Schritte befolgen, um es über Visual Studio zu installieren.  

[!INCLUDE [Install the Azure Functions Tools for Visual Studio](../../includes/functions-install-vstools.md)] 


## <a name="create-an-azure-functions-project"></a>Erstellen eines Azure Functions-Projekts 

[!INCLUDE [Create a project using the Azure Functions](../../includes/functions-vstools-create.md)]


## <a name="configure-the-project-for-local-development"></a>Konfigurieren des Projekts für die lokale Entwicklung

Wenn Sie ein neues Projekt mithilfe der Azure Functions-Vorlage erstellen, erhalten Sie ein leeres C#-Projekt, das die folgenden Dateien enthält:

* **host.json**: Ermöglicht das Konfigurieren des Functions-Hosts. Diese Einstellungen gelten für die lokale Ausführung und die Ausführung in Azure. Weitere Informationen finden Sie im Referenzartikel [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).
    
* **local.settings.json**: Behält Einstellungen beim lokalen Ausführen von Funktionen bei. Diese Einstellungen werden nicht von Azure benutzt, sondern von den [Azure Functions Core-Tools](functions-run-local.md). Verwenden Sie diese Datei, um Einstellungen wie Verbindungszeichenfolgen zu anderen Azure-Diensten anzugeben. Fügen Sie dem **Values**-Array für jede Verbindung, die von Funktionen in Ihrem Projekt benötigt wird, einen neuen Schlüssel hinzu. Weitere Informationen finden Sie unter [Local settings file (Datei mit lokalen Einstellungen)](functions-run-local.md#local-settings-file) im Thema zu Azure Functions Core-Tools.

Die Functions-Laufzeit verwendet intern ein Azure-Speicherkonto. Sie müssen für alle Triggertypen außer HTTP und Webhooks den Schlüssel **Values.AzureWebJobsStorage** auf eine gültige Verbindungszeichenfolge des Azure-Speicherkontos festlegen.

[!INCLUDE [Note to not use local storage](../../includes/functions-local-settings-note.md)]

 So legen Sie die Speicherkonto-Verbindungszeichenfolge fest:

1. Öffnen Sie in Visual Studio den **Cloud-Explorer**, erweitern Sie **Speicherkonto** > **Ihr Speicherkonto**, wählen Sie **Eigenschaften** aus, und kopieren Sie dann den Wert von **Primäre Verbindungszeichenfolge**.   

2. Öffnen Sie in Ihrem Projekt die Projektdatei „local.settings.json“, und legen Sie den Wert des Schlüssels **AzureWebJobsStorage** auf die kopierte Verbindungszeichenfolge fest.

3. Wiederholen Sie den vorherigen Schritt zum Hinzufügen von eindeutigen Schlüsseln zum **Values**-Array für alle anderen Verbindungen, die von Ihren Funktionen benötigt werden.  

## <a name="create-a-function"></a>Erstellen einer Funktion

In vorab kompilierten Funktionen werden die von der Funktion verwendeten Bindungen durch Anwendung von Attributen im Code definiert. Bei der Verwendung von Azure Functions-Tools zum Erstellen von Funktionen aus den bereitgestellten Vorlagen werden diese Attribute für Sie angewendet. 

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektknoten, und wählen Sie **Hinzufügen** > **Neues Element** aus. Wählen Sie **Azure Function** aus, geben Sie einen **Namen** für die Klasse ein, und klicken Sie auf **Hinzufügen**.

2. Wählen Sie den Trigger aus, legen Sie die Bindungseigenschaften fest, und klicken Sie auf **Erstellen**. Im folgenden Beispiel werden die Einstellungen beim Erstellen einer Funktion gezeigt, die durch Queue Storage ausgelöst wird. 

    ![](./media/functions-develop-vs/functions-vstools-create-queuetrigger.png)
    
    Ein Verbindungszeichenfolgen-Schlüssel mit dem Namen **QueueStorage** wird bereitgestellt. Dieser wird in der Datei „local.setting.json“ definiert. 
 
3. Untersuchen Sie die neu hinzugefügte Klasse. Es wird eine statische **Run**-Methode angezeigt, der das Attribut **FunctionName** zugewiesen wird. Dieses Attribut gibt an, dass die Methode den Einstiegspunkt für die Funktion darstellt. 

    Die folgende C#-Klasse stellt beispielsweise eine einfache Funktion dar, die durch Queue Storage ausgelöst wird:

    ````csharp
    using System;
    using Microsoft.Azure.WebJobs;
    using Microsoft.Azure.WebJobs.Host;
    
    namespace FunctionApp1
    {
        public static class Function1
        {
            [FunctionName("QueueTriggerCSharp")]        
            public static void Run([QueueTrigger("myqueue-items", Connection = "QueueStorage")]string myQueueItem, TraceWriter log)
            {
                log.Info($"C# Queue trigger function processed: {myQueueItem}");
            }
        }
    } 
    ````
 
    Ein bindungsspezifisches Attribut wird auf jeden Bindungsparameter angewendet, der der Einstiegspunktmethode bereitgestellt wird. Das Attribut verwendet die Bindungsinformationen als Parameter. Im vorherigen Beispiel wurde auf den ersten Parameter ein **QueueTrigger**-Attribut angewendet, das die durch eine Warteschlange ausgelöste Funktion angab. Die Namen der Warteschlange und der Einstellung der Verbindungszeichenfolge werden als Parameter übergeben.  

## <a name="testing-functions"></a>Testen von Funktionen

Mit Azure Functions Core-Tools können Sie ein Azure Functions-Projekt auf dem lokalen Entwicklungscomputer ausführen. Sie werden beim ersten Starten einer Funktion in Visual Studio zum Installieren dieser Tools aufgefordert.  

Drücken Sie F5, um Ihre Funktion zu testen. Akzeptieren Sie die entsprechende Aufforderung von Visual Studio zum Herunterladen und Installieren der Azure Functions Core (CLI)-Tools.  Sie müssen möglicherweise auch eine Firewallausnahme aktivieren, damit die Tools HTTP-Anforderungen verarbeiten können.

Wenn das Projekt ausgeführt wird, können Sie Ihren Code wie eine bereitgestellte Funktion testen. Weitere Informationen finden Sie unter [Strategien zum Testen Ihres Codes in Azure Functions](functions-test-a-function.md). Bei der Ausführung im Debug-Modus werden in Visual Studio wie erwartet Haltepunkte erreicht. 

Ein Beispiel zum Testen einer durch eine Warteschlange ausgelöste Funktion finden Sie im Tutorial [Erstellen einer Funktion, die durch Azure Queue Storage ausgelöst wird](functions-create-storage-queue-triggered-function.md#test-the-function).  

Weitere Informationen über die Verwendung der Azure Functions Core-Tools finden Sie unter [Lokales Codieren und Testen von Azure-Funktionen](functions-run-local.md).

## <a name="publish-to-azure"></a>Veröffentlichen in Azure

[!INCLUDE [Publish the project to Azure](../../includes/functions-vstools-publish.md)]

>[!NOTE]  
>Alle Einstellungen, die Sie in „local.settings.json“ hinzugefügt haben, müssen auch der Funktions-App in Azure hinzugefügt werden. Diese Einstellungen werden nicht automatisch hinzugefügt. Sie können der Funktions-App die erforderlichen Einstellungen mit einem der folgenden Verfahren hinzufügen:
>
>* [Mit dem Azure-Portal](functions-how-to-use-azure-function-app-settings.md#settings)
>* [Mit der `--publish-local-settings` Option „Veröffentlichen“ in Azure Functions Core Tools](functions-run-local.md#publish)
>* [Mit der Azure-Befehlszeilenschnittstelle](/cli/azure/functionapp/config/appsettings#set) 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Functions-Tools finden Sie im Abschnitt „Common Questions“ (häufig gestellte Fragen) des Blogbeitrags [Visual Studio 2017 Tools for Azure Functions](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/) (in englischer Sprache).

Weitere Informationen zu Azure Functions Core-Tools finden Sie unter [Lokales Codieren und Testen von Azure-Funktionen](functions-run-local.md).  
Weitere Informationen zum Entwickeln von Funktionen wie .NET-Klassenbibliotheken finden Sie unter [Using .NET class libraries with Azure Functions](functions-dotnet-class-library.md) (Verwenden von .NET-Klassenbibliotheken mit Azure Functions). Dieses Thema enthält auch Beispiele für die Verwendung von Attributen zum Deklarieren der verschiedenen Typen von Bindungen, die von Azure Functions unterstützt werden.    

