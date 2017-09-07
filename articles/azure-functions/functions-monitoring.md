---
title: "Überwachen von Azure Functions | Microsoft Docs"
description: "Erfahren Sie, wie Sie Azure Functions überwachen."
services: functions
documentationcenter: na
author: wesmc7777
manager: cfowler
editor: 
tags: 
keywords: Azure Functions, Functions, Ereignisverarbeitung, Webhooks, dynamisches Compute, serverlose Architektur
ms.assetid: 501722c3-f2f7-4224-a220-6d59da08a320
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/03/2016
ms.author: wesmc
ms.translationtype: HT
ms.sourcegitcommit: a0b98d400db31e9bb85611b3029616cc7b2b4b3f
ms.openlocfilehash: 3ab6123b6acfdec57f1ca71b404c9e1123d1ff6d
ms.contentlocale: de-de
ms.lasthandoff: 08/29/2017

---

# <a name="monitoring-azure-functions"></a>Überwachen von Azure Functions

## <a name="overview"></a>Übersicht 


Auf der Registerkarte **Überwachen** der Funktionen können Sie jede Ausführung der jeweiligen Funktion überprüfen.

![Registerkarte „Überwachen“ für Azure Functions](./media/functions-monitoring/monitor-tab.png) 

Wenn Sie auf eine Ausführung klicken, können Sie die Dauer, Eingabedaten, Fehler und zugehörigen Protokolldateien überprüfen. Dies ist sehr nützlich zum Debuggen und zur Leistungsoptimierung Ihrer Funktionen.

> [!IMPORTANT]
> Wenn Sie den [Verbrauchstarif](functions-overview.md#pricing) für Azure Functions verwenden, zeigt die Kachel **Überwachung** der Funktionen-App keine Daten. Das liegt an der dynamischen Skalierung und Verwaltung von Computeinstanzen durch die Plattform. Diese Metriken sind in einem Verbrauchstarif ohne Bedeutung. Um die Nutzung Ihrer Funktionen-Apps zu überwachen, sollten Sie stattdessen die Informationen in diesem Artikel verwenden.
> 
> Der folgende Screenshot zeigt ein Beispiel:
> 
> ![Überwachen der Funktion](./media/functions-monitoring/app-service-overview-monitoring.png)


## <a name="real-time-monitoring"></a>Überwachung in Echtzeit

Die Echtzeitüberwachung ist verfügbar, indem Sie, wie unten gezeigt, auf **Liveereignisdatenstrom** klicken. 

![Option „Liveereignisdatenstrom“ auf der Registerkarte für die Überwachung](./media/functions-monitoring/monitor-tab-live-event-stream.png)

Der Liveereignisdatenstrom wird wir im folgenden Beispiel in einem Diagramm auf einer neuen Registerkarte im Browser angezeigt: 

![Liveereignisdatenstrom – Beispiel](./media/functions-monitoring/live-event-stream.png)


> [!NOTE]
> Es gibt ein bekanntes Problem, das dazu führen kann, dass Ihre Daten nicht aufgefüllt werden. In diesem Fall müssen Sie möglicherweise die Browserregisterkarte mit dem Liveereignisdatenstrom schließen und dann erneut auf **Liveereignisdatenstrom** klicken, damit Ihre Ereignisdatenstromdaten ordnungsgemäß aufgefüllt werden. 

Der Liveereignisdatenstrom stellt die folgenden Statistiken für Ihre Funktion grafisch dar:

* Gestartete Ausführungen pro Sekunde
* Abgeschlossene Ausführungen pro Sekunde
* Fehlerhafte Ausführungen pro Sekunde
* Durchschnittliche Ausführungsdauer in Millisekunden

Diese Statistiken werden in Echtzeit erfasst, aber die grafische Darstellung der Ausführungsdaten kann ca. 10 Sekunden verzögert erfolgen.


## <a name="monitoring-log-files-from-a-command-line"></a>Überwachen von Protokolldateien über eine Befehlszeile

Sie können Protokolldateien mithilfe der Azure-Befehlszeilenschnittstelle 1.0 oder über PowerShell in eine Befehlszeilensitzung auf einer lokalen Arbeitsstation streamen.

### <a name="monitoring-function-app-log-files-with-the-azure-cli-10"></a>Überwachen der Protokolldateien von Funktions-Apps mithilfe der Azure-Befehlszeilenschnittstelle 1.0

Als Erstes [installieren Sie die Azure-Befehlszeilenschnittstelle 1.0](../cli-install-nodejs.md).

Melden Sie sich mithilfe des folgenden Befehls bei Ihrem Azure-Konto an. Alternativ können Sie auch eine der anderen unter [Anmelden bei Azure über die Azure-Befehlszeilenschnittstelle (CLI)](../xplat-cli-connect.md) beschriebenen Methoden verwenden.

    azure login

Verwenden Sie den folgenden Befehl, um die Azure CLI 1.0 im klassischen Dienstverwaltungsmodus zu aktivieren:

    azure config mode asm

Wenn Sie über mehrere Abonnements verfügen, verwenden Sie die folgenden Befehle, um Ihre Abonnements aufzulisten und das aktuelle Abonnement als das Abonnement festzulegen, das Ihre Funktionen-App enthält:

    azure account list
    azure account set <subscriptionNameOrId>

Der folgende Befehl streamt die Protokolldateien Ihrer Funktions-App an die Befehlszeile:

    azure site log tail -v <function app name>

### <a name="monitoring-function-app-log-files-with-powershell"></a>Überwachen der Protokolldateien von Funktionen-Apps mit PowerShell

Als Erstes [installieren und konfigurieren Sie Azure PowerShell](/powershell/azure/overview).

Fügen Sie mithilfe des folgenden Befehls Ihr Azure-Konto hinzu:

    PS C:\> Add-AzureAccount

Wenn Sie über mehrere Abonnements verfügen, können Sie diese mit dem folgenden Befehl nach Namen auflisten, um anhand der `IsCurrent`-Eigenschaft festzustellen, ob das richtige Abonnement als aktuelles Abonnement ausgewählt ist:

    PS C:\> Get-AzureSubscription

Wenn Sie das aktive Abonnement als dasjenige Abonnement festlegen müssen, das Ihre Funktionen-App enthält, verwenden Sie folgenden Befehl:

    PS C:\> Get-AzureSubscription -SubscriptionName "MyFunctionAppSubscription" | Select-AzureSubscription

Streamen Sie die Protokolle mit dem folgenden Befehl an Ihre PowerShell-Sitzung:

    PS C:\> Get-AzureWebSiteLog -Name MyFunctionApp -Tail

Weitere Informationen finden Sie unter [Vorgehensweise: Streaming von Protokollen](../app-service-web/web-sites-enable-diagnostic-log.md#streamlogs). 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Testen einer Funktion](functions-test-a-function.md)
* [Skalieren einer Funktion](functions-scale.md)


