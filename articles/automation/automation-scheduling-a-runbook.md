---
title: "Zeitpläne für Runbooks in Azure Automation | Microsoft Docs"
description: Beschreibt die Erstellung eines Zeitplans in Azure Automation, sodass ein Runbook automatisch zu einer bestimmten Uhrzeit oder nach einem sich wiederholenden Zeitplan gestartet wird.
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 710979ff-99d8-41e4-ac6d-6bf26b8ea654
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/09/2016
ms.author: bwren
ROBOTS: NOINDEX, NOFOLLOW
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: ed7bd79face77a132a4a6588e72b7511bab4b4d4

---

# <a name="scheduling-a-runbook-in-azure-automation"></a>Planen eines Runbooks in Azure Automation
Um ein Runbook in Azure-Automation für die Ausführung zu einer bestimmten Uhrzeit zu planen, müssen Sie es mit einem oder mehreren Zeitplänen verknüpfen. Es kann ein Ausführungszeitplan für die einmalige Ausführung oder die stündliche oder tägliche Wiederholung für Runbooks im klassischen Azure-Portal und für Runbooks im Azure-Portal konfiguriert werden. Außerdem können Sie die Ausführung wöchentlich, monatlich oder für bestimmte Wochen- oder Monatstage bzw. einen bestimmten festen Tag des Monats planen.  Ein Runbook kann mit mehreren Zeitplänen verknüpft werden, und mit einem Zeitplan können mehrere Runbooks verknüpft sein.

## <a name="creating-a-schedule"></a>Erstellen eines Zeitplans
Sie können einen neuen Zeitplan für Runbooks im Azure-Portal, im klassischen Portal oder mit Windows PowerShell erstellen. Außerdem haben Sie die Möglichkeit, einen neuen Zeitplan zu erstellen, wenn Sie ein Runbook über das klassische Azure-Portal oder das Azure-Portal mit einem Zeitplan verknüpfen.

> [!NOTE]
> Wenn Sie einen Zeitplan mit einem Runbook verknüpfen, speichert Automation die aktuellen Versionen der Module in Ihrem Konto und verknüpft sie mit diesem Zeitplan.  Dies bedeutet: Wenn Ihr Konto zu dem Zeitpunkt, als Sie einen Zeitplan erstellten, ein Modul mit Version 1.0 enthielt, und Sie dann das Modul auf Version 2.0 aktualisieren, verwendet der Zeitplan weiterhin 1.0.  Um die aktualisierte Modulversion verwenden zu können, müssen Sie einen neuen Zeitplan erstellen. 
> 
> 

### <a name="to-create-a-new-schedule-in-the-azure-classic-portal"></a>So erstellen Sie einen neuen Zeitplan im klassischen Azure-Portal
1. Wählen Sie im klassischen Azure-Portal die Option „Automation“, und wählen Sie den Namen eines Automation-Kontos aus.
2. Wählen Sie die Registerkarte **Objekte** aus.
3. Klicken Sie unten im Fenster auf **Einstellung hinzufügen**.
4. Klicken Sie auf **Zeitplan hinzufügen**.
5. Geben Sie einen **Namen** und optional eine **Beschreibung** für den neuen Zeitplan ein. Ihr Zeitplan wird **Einmalig**, **Stündlich**, **Täglich**, **Wöchentlich** oder **Monatlich** ausgeführt.
6. Geben Sie eine **Startzeit** sowie weitere Optionen an, je nachdem, welche Art von Zeitplan Sie ausgewählt haben.

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a>So erstellen Sie einen neuen Zeitplan im Azure-Portal
1. Klicken Sie im Azure-Portal unter Ihrem Automation-Konto auf die Kachel **Ressourcen**, um das Blatt **Ressourcen** zu öffnen.
2. Klicken Sie auf die Kachel **Zeitpläne**, um das Blatt **Zeitpläne** zu öffnen.
3. Klicken Sie oben auf dem Blatt auf **Zeitplan hinzufügen** .
4. Geben Sie auf dem Blatt **Neuer Zeitplan** einen **Namen** und optional eine **Beschreibung** für den neuen Zeitplan ein.
5. Wählen Sie aus, ob der Zeitplan einmalig oder nach einem Zeitplan häufiger ausgeführt werden soll, indem Sie **Einmalig** oder **Wiederholung** angeben.  Geben Sie bei Auswahl von **Einmalig** eine **Startzeit** an, und klicken Sie auf **Erstellen**.  Geben Sie bei Auswahl von **Wiederholung** eine **Startzeit** und den Wert an, der festlegt, wie oft die Ausführung des Runbooks wiederholt werden soll: **Stunde**, **Tag**, **Woche** oder **Monat**.  Wenn Sie in der Dropdownliste die Option **Woche** oder **Monat** auswählen, wird auf dem Blatt die Option **Wiederholung** angezeigt. Bei Auswahl dieser Option wird das Blatt **Wiederholung** angezeigt, und Sie können den Wochentag auswählen, wenn Sie zuvor **Woche** gewählt haben.  Falls Sie **Monat** gewählt haben, können Sie **Wochentage** oder bestimmte Tage des Monats im Kalender auswählen. Außerdem können Sie angeben, ob die Ausführung am letzten Tag des Monats durchgeführt werden soll. Klicken Sie anschließend auf **OK**.   

### <a name="to-create-a-new-schedule-with-windows-powershell"></a>So erstellen Sie einen neuen Zeitplan mit Windows PowerShell
Sie können das [New-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690271.aspx)-Cmdlet verwenden, um in Azure Automation einen neuen Zeitplan für klassische Runbooks zu erstellen, oder das [New-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx)-Cmdlet für Runbooks im Azure-Portal. Sie müssen die Startzeit für den Zeitplan und die Häufigkeit der Ausführung angeben.

Die folgenden Beispielbefehle zeigen, wie ein neuer Zeitplan erstellt wird, der ab 20. Januar 2015 jeden Tag um 15:30 mit einem Azure-Dienstverwaltungs-Cmdlet ausgeführt wird.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName –StartTime "1/20/2016 15:30:00" –DayInterval 1

Mit den folgenden Beispielbefehlen wird veranschaulicht, wie Sie einen Zeitplan für den 15. und 30. jedes Monats mit einem Azure Resource Manager-Cmdlet erstellen.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
    -DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"


## <a name="linking-a-schedule-to-a-runbook"></a>Verknüpfen eines Zeitplans mit einem Runbook
Ein Runbook kann mit mehreren Zeitplänen verknüpft werden, und mit einem Zeitplan können mehrere Runbooks verknüpft sein. Wenn ein Runbook über Parameter verfügt, können Sie Werte für diese bereitstellen. Sie müssen Werte für alle verbindlichen Parameter angeben; für optionale Parameter können Sie Werte angeben.  Diese Werte werden jedes Mal verwendet, wenn das Runbook durch diesen Zeitplan gestartet wird.  Sie können das gleiche Runbook einem anderen Zeitplan zuordnen und dabei andere Parameterwerte festlegen.

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-classic-portal"></a>So verknüpfen Sie einen Zeitplan mit einem Runbook mit dem klassischen Azure-Portal
1. Wählen Sie im klassischen Azure-Portal die Option **Automation** , und klicken Sie auf den Namen eines Automation-Kontos.
2. Wählen Sie die Registerkarte **Runbooks** .
3. Klicken Sie auf den Namen des Runbooks, das Sie mit einem Zeitplan verknüpfen möchten.
4. Klicken Sie auf die Registerkarte **Zeitplan** .
5. Wenn das Runbook gegenwärtig nicht mit einem Zeitplan verknüpft ist, stehen Ihnen die Optionen **Mit einem neuen Zeitplan verknüpfen** oder **Mit einem vorhandenen Zeitplan verknüpfen** zur Verfügung.  Wenn das Runbook gegenwärtig mit einem Zeitplan verknüpft ist, klicken Sie auf **Link** am unteren Rand des Fensters, um auf diese Optionen zuzugreifen.
6. Wenn das Runbook über Parameter verfügt, werden Sie zur Eingabe von Werten für diese Parameter aufgefordert.  

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a>So verknüpfen Sie einen Zeitplan mit einem Runbook mit dem klassischen Azure-Portal
1. Klicken Sie im Azure-Portal unter Ihrem Automation-Konto auf die Kachel **Runbooks**, um das Blatt **Runbooks** zu öffnen.
2. Klicken Sie auf den Namen des Runbooks, das Sie mit einem Zeitplan verknüpfen möchten.
3. Wenn das Runbook gegenwärtig nicht mit einem Zeitplan verknüpft ist, stehen Ihnen die Optionen zum Erstellen eines neuen Zeitplans oder Verknüpfen mit einem vorhandenen Zeitplan zur Verfügung.  
4. Falls das Runbook über Parameter verfügt, können Sie die Option **Ausführungseinstellungen ändern (Standard: Azure)** wählen. Das Blatt **Parameter** wird angezeigt, auf dem Sie die Informationen entsprechend eingeben können.  

### <a name="to-link-a-schedule-to-a-runbook-with-windows-powershell"></a>So verknüpfen Sie einen Zeitplan mit einem Runbook mit Windows PowerShell
Sie können [Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) verwenden, um einen Zeitplan mit einem klassischen Runbook zu verknüpfen, oder das [Register-AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx)-Cmdlet für Runbooks im Azure-Portal.  Sie können Werte für die Runbookparameter im Parameter "Parameters" angeben. Weitere Informationen zum Angeben von Parameterwerten finden Sie unter [Starten eines Runbooks in Azure Automation](automation-starting-a-runbook.md) .

Die folgenden Beispielbefehle verdeutlichen, wie Sie einen Zeitplan verknüpfen, indem Sie ein Azure-Dienstverwaltungs-Cmdlet mit Parametern verwenden.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params

Die folgenden Beispielbefehle verdeutlichen, wie Sie einen Zeitplan mit einem Runbook verknüpfen, indem Sie ein Azure Resource Manager-Cmdlet mit Parametern verwenden.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params `
    -ResourceGroupName "ResourceGroup01"

## <a name="disabling-a-schedule"></a>Deaktivieren eines Zeitplans
Wenn Sie einen Zeitplan deaktivieren, werden sämtliche damit verknüpften Runbooks nicht mehr nach diesem Zeitplan ausgeführt. Sie können einen Zeitplan manuell deaktivieren oder während der Erstellung für Zeitpläne eine Ablaufzeit festlegen. Wenn der Ablaufzeitpunkt erreicht ist, wird der Zeitplan deaktiviert.

### <a name="to-disable-a-schedule-from-the-azure-classic-portal"></a>So deaktivieren Sie einen Zeitplan über das klassische Azure-Portal
Sie können einen Zeitplan im klassischen Azure-Portal über die Seite mit den Zeitplandetails für den Zeitplan deaktivieren.

1. Wählen Sie im klassischen Azure-Portal die Option „Automation“, und klicken Sie auf den Namen eines Automation-Kontos.
2. Wählen Sie die Registerkarte "Objekte" aus.
3. Klicken Sie auf den Namen eines Zeitplans, um die Detailseite zu öffnen.
4. Ändern Sie **Aktiviert** in **Nein**.

### <a name="to-disable-a-schedule-from-the-azure-portal"></a>So deaktivieren Sie einen Zeitplan über das Azure-Portal
1. Klicken Sie im Azure-Portal unter Ihrem Automation-Konto auf die Kachel **Ressourcen**, um das Blatt **Ressourcen** zu öffnen.
2. Klicken Sie auf die Kachel **Zeitpläne**, um das Blatt **Zeitpläne** zu öffnen.
3. Klicken Sie auf den Namen eines Zeitplans, um das Blatt mit den Details zu öffnen.
4. Ändern Sie **Aktiviert** in **Nein**.

### <a name="to-disable-a-schedule-with-windows-powershell"></a>So deaktivieren Sie einen Zeitplan mit Windows PowerShell
Sie können das [Set-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx)-Cmdlet verwenden, um die Eigenschaften eines vorhandenen Zeitplans für ein klassisches Runbook zu ändern, oder das [Set-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx)-Cmdlet für Runbooks im Azure-Portal. Um den Zeitplan zu deaktivieren, legen Sie für den Parameter **IsEnabled** den Wert **false** fest.

Die folgenden Beispielbefehle verdeutlichen, wie Sie einen Zeitplan deaktivieren, indem Sie das Azure-Dienstverwaltungs-Cmdlet verwenden.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false

Die folgenden Beispielbefehle verdeutlichen, wie Sie einen Zeitplan für ein Runbook deaktivieren, indem Sie ein Azure Resource Manager-Cmdlet verwenden.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"


## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Verwenden von Zeitplänen finden Sie unter [Zeitpläne in Azure Automation](http://msdn.microsoft.com/library/azure/dn940016.aspx)
* Informationen zu den ersten Schritten mit Runbooks in Azure Automation finden Sie unter [Starten eines Runbooks in Azure Automation](automation-starting-a-runbook.md) 




<!--HONumber=Nov16_HO3-->


