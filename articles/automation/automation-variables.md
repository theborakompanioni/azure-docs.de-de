---
title: Variablenobjekte in Azure Automation | Microsoft Docs
description: "Variablenobjekte sind Werte, die allen Runbooks und DSC-Konfigurationen in Azure Automation zur Verfügung stehen.  Dieser Artikel stellt eine ausführliche Beschreibung von Variablen bereit und zeigt, wie diese in Textrunbooks und grafischen Runbooks eingesetzt werden."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: b880c15f-46f5-4881-8e98-e034cc5a66ec
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/09/2017
ms.author: magoedte;bwren
ms.translationtype: HT
ms.sourcegitcommit: 2c6cf0eff812b12ad852e1434e7adf42c5eb7422
ms.openlocfilehash: 43a08898abecb220f3df892473dddfb2729f0561
ms.contentlocale: de-de
ms.lasthandoff: 09/13/2017

---
# <a name="variable-assets-in-azure-automation"></a>Variable Objekte in Azure Automation

Variablenobjekte sind Werte, die allen Runbooks und DSC-Konfigurationen in Ihrem Automation-Konto zur Verfügung stehen. Sie können über das Azure-Portal, in Windows PowerShell sowie in einem Runbook oder einer DSC-Konfiguration erstellt, geändert und abgerufen werden. Automatisierungsvariablen sind in folgenden Szenarien hilfreich:

- Gemeinsame Nutzung eines Werts durch mehrere Runbooks oder DSC-Konfigurationen

- Gemeinsame Nutzung eines Werts durch mehrere Aufträge des gleichen Runbooks oder der gleichen DSC-Konfiguration

- Verwalten eines Werts über das Portal oder über die von Runbooks oder DSC-Konfigurationen verwendete Windows PowerShell-Befehlszeile, z.B. ein Satz von allgemeinen Konfigurationselementen wie eine bestimmte Liste von VM-Namen, eine bestimmte Ressourcengruppe, ein AD-Domänenname usw.  

Automation-Variablen werden persistent gespeichert und bleiben daher auch dann verfügbar, wenn die Ausführung eines Runbooks oder einer DSC-Konfiguration misslingt.  Dadurch kann ein Wert von einem Runbook festgelegt und anschließend von einem anderen oder vom gleichen Runbook bzw. von einer anderen oder von der gleichen DSC-Konfiguration bei der nächsten Ausführung verwendet werden.

Beim Erstellen einer Variablen können Sie festlegen, dass diese verschlüsselt gespeichert wird.  Wenn eine Variable verschlüsselt wird, wird sie sicher in Azure Automation gespeichert, und ihr Wert kann vom Cmdlet [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx) , das zum Lieferumfang des Azure PowerShell-Moduls gehört, nicht abgerufen werden.  Ein verschlüsselter Wert kann ausschließlich über die Aktivität **Get-AutomationVariable** in einem Runbook oder einer DSC-Konfiguration abgerufen werden.

> [!NOTE]
> Zu den sicheren Objekten in Azure Automation gehören Anmeldeinformationen, Zertifikate, Verbindungen und verschlüsselte Variablen. Diese Objekte werden mithilfe eines eindeutigen Schlüssels verschlüsselt und in Azure Automation gespeichert, der für jedes Automation-Konto generiert wird. Dieser Schlüssel wird mit einem Masterzertifikat verschlüsselt und in Azure Automation gespeichert. Vor dem Speichern eines sicheren Objekts wird der Schlüssel für das Automation-Konto mit dem Masterzertifikat verschlüsselt und anschließend zum Verschlüsseln des Objekts verwendet.

## <a name="variable-types"></a>Variablentypen

Beim Erstellen einer Variablen über das Azure-Portal müssen Sie einen Datentyp aus der Dropdownliste angeben, damit das entsprechende Steuerelement zur Eingabe des Variablenwerts im Portal angezeigt werden kann. Die Variable ist nicht auf diesen Datentyp beschränkt, Sie müssen die Variable jedoch in Windows PowerShell festlegen, wenn Sie einen anderen Wertetyp angeben möchten. Wenn Sie **Nicht definiert** angeben, wird der Wert der Variablen auf **$null** festgelegt, und Sie müssen den Wert mithilfe des Cmdlets [Set-AzureAutomationVariable](http://msdn.microsoft.com/library/dn913767.aspx) oder der Aktivität **Set-AutomationVariable** festlegen.  Sie können den Wert für einen komplexen Variablentyp nicht über das Portal festlegen, Sie können jedoch einen Wert jedes Typs in Windows PowerShell bereitstellen. Komplexe Typen werden als [PSCustomObject](http://msdn.microsoft.com/library/system.management.automation.pscustomobject.aspx)zurückgegeben.

Sie können mehrere Werte in einer einzigen Variable speichern, indem Sie ein Array oder eine Hashtabelle erstellen und in der Variable speichern.

Im Folgenden finden Sie eine Liste von in Automation verfügbaren Variablentypen:

* String
* Integer
* DateTime
* Boolean
* Null

## <a name="scripting-the-creation-and-management-of-variables"></a>Skripts für die Erstellung und Verwaltung von Variablen

Die Cmdlets in der folgenden Tabelle werden zum Erstellen und Verwalten von Automation-Variablen mit Windows PowerShell verwendet. Sie gehören zum Funktionsumfang des [Azure PowerShell-Moduls](../powershell-install-configure.md) , das zur Verwendung in Automation-Runbooks und DSC-Konfigurationen verfügbar ist.

|Cmdlets|Beschreibung|
|:---|:---|
|[Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx)|Ruft den Wert einer vorhandenen Variable ab.|
|[New-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603613.aspx)|Erstellt eine neue Variable und legt ihren Wert fest.|
|[Remove-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt619354.aspx)|Entfernt eine vorhandene Variable.|
|[Set-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603601.aspx)|Legt den Wert für eine vorhandene Variable fest.|

Die Workflowaktivitäten in der folgenden Tabelle werden für den Zugriff auf Automation-Variablen in einem Runbook verwendet. Sie sind nur zur Verwendung in einem Runbook oder einer DSC-Konfiguration verfügbar und gehören nicht zum Funktionsumfang des Azure PowerShell-Moduls.

|Workflowaktivitäten|Beschreibung|
|:---|:---|
|Get-AutomationVariable|Ruft den Wert einer vorhandenen Variable ab.|
|Set-AutomationVariable|Legt den Wert für eine vorhandene Variable fest.|

> [!NOTE] 
> Vermeiden Sie die Verwendung von Variablen im Parameter „–Name“ von **Get-AutomationVariable** in einem Runbook oder einer DSC-Konfiguration, da dies die Ermittlung von Abhängigkeiten zwischen Runbooks bzw. DSC-Konfigurationen und Automation-Variablen zur Entwurfszeit erschweren kann.

Die Funktionen in der folgenden Tabelle werden zum Zugreifen auf und Abrufen von Variablen in einem Python2-Runbook verwendet. 

|Python2-Funktionen|Beschreibung|
|:---|:---|
|automationassets.get_automation_variable|Ruft den Wert einer vorhandenen Variable ab. |
|automationassets.set_automation_variable|Legt den Wert für eine vorhandene Variable fest. |

> [!NOTE] 
> Sie müssen das Modul „automationassets“ oben im Python-Runbook importieren, um auf die Ressourcenfunktionen zugreifen zu können.

## <a name="creating-a-new-automation-variable"></a>Erstellen einer neuen Automation-Variablen

### <a name="to-create-a-new-variable-with-the-azure-portal"></a>So erstellen Sie eine neue Variable über das Azure-Portal

1. Klicken Sie in Ihrem Automation-Konto auf die Kachel **Objekte** und dann das Blatt **Objekte**, und wählen Sie **Variablen** aus.
2. Klicken Sie auf dem Blatt **Variablen** auf **Variable hinzufügen**.
3. Gehen Sie die Optionen auf dem Blatt **Neue Variable** durch, und klicken Sie auf **Erstellen**, um die neue Variable zu speichern.

### <a name="to-create-a-new-variable-with-windows-powershell"></a>So erstellen Sie eine neue Variable mit Windows PowerShell

Das Cmdlet [New-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603613.aspx) erstellt eine neue Variable und legt ihren anfänglichen Wert fest. Sie können den Wert mithilfe von [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx)abrufen. Wenn es sich um einen einfachen Wert handelt, wird der gleiche Typ zurückgegeben. Wenn es sich um einen komplexen Wert handelt, wird ein **PSCustomObject** zurückgegeben.

Die folgenden Beispielbefehle zeigen, wie eine Variable vom Typ "string" erstellt und anschließend der Wert dieser Variablen zurückgegeben wird.

    New-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" 
    –AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable' `
    –Encrypted $false –Value 'My String'
    $string = (Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable').Value

Die folgenden Beispielbefehle zeigen, wie eine Variable eines komplexen Typs erstellt wird und anschließend die Eigenschaften dieser Variablen zurückgegeben werden. In diesem Fall wird ein Objekt für einen virtuellen Computer von **Get-AzureRmVm** verwendet.

    $vm = Get-AzureRmVm -ResourceGroupName "ResourceGroup01" –Name "VM01"
    New-AzureRmAutomationVariable –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable" –Encrypted $false –Value $vm
    
    $vmValue = (Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable").Value
    $vmName = $vmValue.Name
    $vmIpAddress = $vmValue.IpAddress



## <a name="using-a-variable-in-a-runbook-or-dsc-configuration"></a>Verwenden einer Variablen in einem Runbook oder einer DSC-Konfiguration

Verwenden Sie die Aktivität **Set-AutomationVariable**, um den Wert einer Automation-Variablen in einem PowerShell-Runbook oder einer DSC-Konfiguration festzulegen, und die Aktivität **Get-AutomationVariable**, um den Wert abzurufen.  Die Verwendung der Cmdlets **Set-AzureAutomationVariable** oder **Get-AzureAutomationVariable** in einem Runbook oder einer DSC-Konfiguration empfiehlt sich nicht, da sie weniger effizient sind als die Workflowaktivitäten.  Außerdem können Sie den Wert sicherer Variablen mit **Get-AzureAutomationVariable**nicht abrufen.  Die einzige Möglichkeit, eine neue Variable in einem Runbook oder einer DSC-Konfiguration zu erstellen, ist die Verwendung des Cmdlets [New-AzureAutomationVariable](http://msdn.microsoft.com/library/dn913771.aspx).


### <a name="textual-runbook-samples"></a>Beispiele für Textrunbooks

#### <a name="setting-and-retrieving-a-simple-value-from-a-variable"></a>Festlegen und Abrufen eins einfachen Werts aus einer Variablen

Die folgenden Beispielbefehle zeigen, wie Sie eine Variable in einem Textrunbook festlegen und abrufen. In diesem Beispiel wird angenommen, dass die Variablen des Typs „integer“ *NumberOfIterations* und *NumberOfRunnings* sowie die Variable des Typs „string“ *SampleMessage* bereits erstellt wurden.

    $NumberOfIterations = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfIterations'
    $NumberOfRunnings = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfRunnings'
    $SampleMessage = Get-AutomationVariable -Name 'SampleMessage'
    
    Write-Output "Runbook has been run $NumberOfRunnings times."
    
    for ($i = 1; $i -le $NumberOfIterations; $i++) {
       Write-Output "$i`: $SampleMessage"
    }
    Set-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" –AutomationAccountName "MyAutomationAccount" –Name NumberOfRunnings –Value ($NumberOfRunnings += 1)

#### <a name="setting-and-retrieving-a-complex-object-in-a-variable"></a>Festlegen und Abrufen eines komplexen Objekts in einer Variablen

Der folgende Beispielcode zeigt, wie eine Variable mit einem komplexen Wert in einem Textrunbook aktualisiert wird. In diesem Beispiel wird ein virtueller Azure-Computer mit **Get-AzureVM** abgerufen und in einer vorhandenen Automation-Variablen gespeichert.  Wie im Abschnitt [Variablentypen](#variable-types)erläutert, erfolgt die Speicherung als "PSCustomObject".

    $vm = Get-AzureVM -ServiceName "MyVM" -Name "MyVM"
    Set-AutomationVariable -Name "MyComplexVariable" -Value $vm

Im folgenden Code wird der Wert aus der Variablen abgerufen und zum Starten des virtuellen Computers verwendet.

    $vmObject = Get-AutomationVariable -Name "MyComplexVariable"
    if ($vmObject.PowerState -eq 'Stopped') {
       Start-AzureVM -ServiceName $vmObject.ServiceName -Name $vmObject.Name
    }


#### <a name="setting-and-retrieving-a-collection-in-a-variable"></a>Festlegen und Abrufen eine Auflistung in einer Variablen

Der folgende Beispielcode zeigt, wie eine Variable mit einer Auflistung komplexer Werte in einem Textrunbook aktualisiert wird. In diesem Beispiel werden mehrere virtuelle Azure-Computer mit **Get-AzureVM** abgerufen und in einer vorhandenen Automation-Variablen gespeichert.  Wie im Abschnitt [Variablentypen](#variable-types)erläutert, erfolgt die Speicherung als PSCustomObject-Auflistung gespeichert.

    $vms = Get-AzureVM | Where -FilterScript {$_.Name -match "my"}     
    Set-AutomationVariable -Name 'MyComplexVariable' -Value $vms

Im folgenden Code wird die Auflistung aus der Variablen abgerufen und zum Starten der virtuellen Computer verwendet.

    $vmValues = Get-AutomationVariable -Name "MyComplexVariable"
    ForEach ($vmValue in $vmValues)
    {
       if ($vmValue.PowerState -eq 'Stopped') {
          Start-AzureVM -ServiceName $vmValue.ServiceName -Name $vmValue.Name
       }
    }
    
#### <a name="setting-and-retrieving-a-variable-in-python2"></a>Festlegen und Abrufen einer Variablen in Python2
Der folgende Beispielcode veranschaulicht, wie Sie eine Variable verwenden, eine Variable festlegen und eine Ausnahme für eine nicht vorhandene Variable in einem Python2-Runbook behandeln.

    import automationassets
    from automationassets import AutomationAssetNotFound

    # get a variable
    value = automationassets.get_automation_variable("test-variable")
    print value

    # set a variable (value can be int/bool/string)
    automationassets.set_automation_variable("test-variable", True)
    automationassets.set_automation_variable("test-variable", 4)
    automationassets.set_automation_variable("test-variable", "test-string")

    # handle a non-existent variable exception
    try:
        value = automationassets.get_automation_variable("non-existing variable")
    except AutomationAssetNotFound:
        print "variable not found"


### <a name="graphical-runbook-samples"></a>Beispiel für grafische Runbooks

In einem grafischen Runbook fügen Sie **Get-AutomationVariable** oder **Set-AutomationVariable** hinzu, indem Sie im Bereich „Bibliothek“ des grafischen Editors mit der rechten Maustaste auf die Variable klicken und die gewünschte Aktivität auswählen.

![Hinzufügen einer Variablen zum Zeichenbereich](media/automation-variables/runbook-variable-add-canvas.png)

#### <a name="setting-values-in-a-variable"></a>Festlegen von Werten in einer Variablen
Die folgende Abbildung zeigt Beispielaktivitäten zum Aktualisieren einer Variablen mit einem einfachen Wert in einem grafischen Runbook. In diesem Beispiel wird ein einzelner virtueller Azure-Computer mit **Get-AzureRmVM** abgerufen, und der Name des Computers wird in einer vorhandenen Automation-Variablen vom Typ „string“ gespeichert.  Es spielt keine Rolle, ob es sich bei der [Verknüpfung um eine Pipeline oder eine Sequenz](automation-graphical-authoring-intro.md#links-and-workflow) handelt, da in der Ausgabe nur ein einziges Objekt zu erwarten ist.

![Festlegen einer einfachen Variablen](media/automation-variables/runbook-set-simple-variable.png)

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur Verknüpfung von Aktivitäten bei der grafischen Inhaltserstellung finden Sie unter [Links bei der grafischen Erstellung](automation-graphical-authoring-intro.md#links-and-workflow)
* Informationen über die ersten Schritte mit grafischen Runbooks finden Sie unter [Mein erstes grafisches Runbook](automation-first-runbook-graphical.md) 


