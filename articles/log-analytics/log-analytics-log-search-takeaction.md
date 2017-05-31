---
title: Benutzerinitiierte Azure Automation-Runbook-Aktion in Log Analytics | Microsoft-Dokumentation
description: "In diesem Artikel wird beschrieben, wie ein Automation-Runbook über ein Log Analytics-Suchergebnis bei Bedarf ausgeführt wird."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/04/2017
ms.author: magoedte
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: ff938697add98f3d21b4971175432335ee2e39ba
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017

---

# <a name="take-action-with-an-automation-runbook-from-a-log-analytics-log-search-result"></a>Führen Sie die Aktion mit einem Automation-Runbook über ein Log Analytics-Protokollsuchergebnis aus

Sie können in einem Protokollsuchergebnis in Azure Log Analytics nun **Take action** (Aktion ausführen) auswählen, um ein Automation-Runbook auszuführen.  Das Runbook kann verwendet werden, um das Problem zu beheben oder eine andere Aktion auszuführen, z.B. Informationen zur Problembehandlung sammeln, eine E-Mail senden oder eine Dienstanforderung erstellen. 

## <a name="components-and-features-used"></a>Verwendete Komponenten und Features
* [Azure Automation-Konto](../automation/automation-offering-get-started.md)
* [Log Analytics-Arbeitsbereich](../log-analytics/log-analytics-overview.md)

## <a name="to-initiate-runbook-from-log-search"></a>So initiieren Sie ein Runbook über die Protokollsuche

Zum Ausführen einer Aktion zu einem Ereignis und Initiieren eines Runbooks über die Protokollsuchergebnisse erstellen Sie zuerst eine Protokollsuche. Über die Ergebnisse können Sie bei Bedarf ein Runbook aufrufen.  Dies kann über die Protokollsuchfunktion in Azure oder [OMS-Portal](../log-analytics/log-analytics-log-searches.md) durchgeführt werden.  In diesem Beispiel führen Sie eine Protokollsuche über das Azure-Portal mit einer einfachen Demonstration dieser Funktion aus.

1. Klicken Sie im Azure-Portal im Hub-Menü auf **Weitere Dienste**, und wählen Sie **Log Analytics** aus.  
2. Wählen Sie auf dem Blatt „Log Analytics“ Ihren Log Analytics-Arbeitsbereich und auf dem Arbeitsbereichsblatt **Protokollsuche** aus.  
3. Führen Sie auf dem Blatt „Protokollsuche“ eine Protokollsuche aus.  
4. Klicken Sie in den Protokollsuchergebnissen auf die Auslassungszeichen auf der linken Seite eines der Felder, und wählen Sie **Take action on** (Aktion ausführen für) aus.<br><br> ![Ausführen von Aktionen aus Suchergebnissen](./media/log-analytics-log-search-takeaction/log-search-takeaction-menuoption.png) 
5. Wählen Sie auf dem Blatt „Take action“ (Aktion ausführen) **Runbook ausführen** aus. Auf dem Blatt **Runbook ausführen** können Sie ein Runbook auswählen, das ausgeführt werden soll.  Sie können jedes Runbook im Automation-Konto auswählen, das mit dem Log Analytics-Arbeitsbereich verknüpft ist.  Beachten Sie Folgendes:

    * Runbooks werden nach Tags organisiert
    * Die Werte der Runbook-Eingabeparameter können direkt aus den Feldern des Suchergebnisses ausgewählt werden.  Eine Dropdownliste wird angezeigt, in der alle verfügbaren Felder des Ergebnisses eingeblendet werden, in denen Sie eine Wahl treffen können.  
    * Sie können angeben, dass das Runbook auf einem [Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md) ausgeführt werden soll, den Sie auf dem Computer installiert haben, auf dem das Problem auftritt. Voraussetzung ist jedoch eine entsprechende Hybrid Runbook Worker-Gruppe, die nur diesen Computer als Mitglied enthält.  Wenn der Name der Hybrid Worker-Gruppe mit dem Namen des Computers aus dem Protokollsuchergebnis übereinstimmt, wird die Gruppe automatisch ausgewählt.    

6. Nachdem Sie auf **Ausführen** geklickt haben, wird das Runbookauftragsblatt geöffnet, in dem Sie den Status des Auftrags überprüfen können.   

Wenn Sie ein Runbook auswählen, das so konfiguriert wurde, dass es [von einer Log Analytics-Warnung aufgerufen wird](../automation/automation-invoke-runbook-from-omsla-alert.md), hat es einen Eingabeparameter mit dem Namen **WebhookData**, der vom Typ **Object** ist.  Wenn der Eingabeparameter obligatorisch ist, müssen Sie die Suchergebnisse an das Runbook übergeben, damit es die JSON-formatierte Zeichenfolge in einen Objekttyp konvertieren kann, sodass Sie nach bestimmten Elementen filtern können, auf die Sie in Runbook-Aktivitäten verweisen.  Wählen Sie dazu **Suchergebnis (Objekt)** in der Dropdownliste aus.<br><br> ![Wählen Sie das Webhook-Datenobjekt für den Runbook-Parameter aus](media/log-analytics-log-search-takeaction/select-runbook-and-properties.png)   
    
## <a name="next-steps"></a>Nächste Schritte

* Lesen Sie sich die [Referenz zur Log Analytics-Suche](log-analytics-search-reference.md) durch, um sich über alle Suchfelder und Facetten zu informieren, die in Log Analytics verfügbar sind.
* Informationen dazu, wie Sie ein Automation-Runbook automatisch aufrufen, finden Sie unter [Aufrufen eines Azure Automation-Runbooks über eine OMS-Log Analytics-Warnung](../automation/automation-invoke-runbook-from-omsla-alert.md).  
