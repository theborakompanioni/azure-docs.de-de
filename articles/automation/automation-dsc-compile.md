---
title: Kompilieren von Konfigurationen in Azure Automation DSC | Microsoft Docs
description: "In diesem Artikel wird das Kompilieren von DSC-Konfigurationen (Desired State Configuration, Konfiguration des gewünschten Zustands) für Azure Automation beschrieben."
services: automation
documentationcenter: na
author: eslesar
manager: carmonm
ms.assetid: 49f20b31-4fa5-4712-b1c7-8f4409f1aecc
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 02/07/2017
ms.author: magoedte; eslesar
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: e0aa7db15451bd3ea868932455127e000619aa5d
ms.lasthandoff: 04/27/2017

---

# <a name="compiling-configurations-in-azure-automation-dsc"></a>Kompilieren von Konfigurationen in Azure Automation DSC

Sie können DSC-Konfigurationen (Desired State Configuration, Konfiguration des gewünschten Zustands) in Azure Automation auf zwei Arten kompilieren: im Azure-Portal und mit Windows PowerShell. Die folgenden Informationen helfen bei der Entscheidung, welche Methode sich in welcher Situation am besten eignet:

### <a name="azure-portal"></a>Azure-Portal

* Einfachste Methode mit interaktiver Benutzeroberfläche
* Formular zum Bereitstellen von einfachen Parameterwerten
* Einfaches Nachverfolgen des Auftragsstatus
* Authentifizierter Zugriff über Azure-Anmeldung

### <a name="windows-powershell"></a>Windows PowerShell

* Aufruf über Befehlszeile mit Windows PowerShell-Cmdlets
* Kann in eine automatisierte Lösung mit mehreren Schritten eingebunden werden
* Bereitstellen von einfachen und komplexen Parameterwerten
* Nachverfolgen des Auftragsstatus
* Client erforderlich zur Unterstützung der PowerShell-Befehle
* Übergeben von ConfigurationData-Werten
* Kompilieren von Konfigurationen mit Anmeldeinformationen

Wenn Sie sich für eine Kompilierungsmethode entschieden haben, folgen Sie dem jeweiligen unten beschriebenen Verfahren, um mit der Kompilierung zu beginnen.

## <a name="compiling-a-dsc-configuration-with-the-azure-portal"></a>Kompilieren einer DSC-Konfiguration mit dem Azure-Portal

1. Klicken Sie in Ihrem Automation-Konto auf **DSC-Konfigurationen**.
2. Klicken Sie auf eine Konfiguration, um das zugehörige Blatt zu öffnen.
3. Klicken Sie auf **Kompilieren**.
4. Wenn die Konfiguration keine Parameter enthält, werden Sie dazu aufgefordert, zu bestätigen, dass Sie die Konfiguration kompilieren möchten. Wenn die Konfiguration Parameter enthält, wird das Blatt **Konfiguration kompilieren** geöffnet, auf dem Sie Parameterwerte angeben können. Weitere Informationen zu Parametern finden Sie im Abschnitt [**Grundlegende Parameter**](#basic-parameters) weiter unten.
5. Das Blatt **Kompilierungsauftrag** wird geöffnet, auf dem Sie den Status des Kompilierungsauftrags sowie die Knotenkonfigurationen (MOF-Konfigurationsdokumente) nachverfolgen können, die durch den Auftrag auf dem Azure Automation DSC-Pullserver platziert werden.

## <a name="compiling-a-dsc-configuration-with-windows-powershell"></a>Kompilieren einer DSC-Konfiguration mit Windows PowerShell

Sie können [`Start-AzureRmAutomationDscCompilationJob`](/powershell/module/azurerm.automation/start-azurermautomationdsccompilationjob) verwenden, um mit der Kompilierung mit Windows PowerShell zu beginnen. Der folgende Beispielcode startet die Kompilierung einer DSC-Konfiguration namens **SampleConfig**.

```powershell
Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig"
```

`Start-AzureRmAutomationDscCompilationJob` gibt ein Kompilierungsauftragsobjekt zurück, das zum Nachverfolgen des Auftragsstatus verwendet werden kann. Sie können dieses Kompilierungsauftragsobjekt mit [`Get-AzureRmAutomationDscCompilationJob`](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjob) verwenden, um den Status des Kompilierungsauftrags zu ermitteln. Wenn Sie das Objekt mit [`Get-AzureRmAutomationDscCompilationJobOutput`](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjoboutput) verwenden, können Sie die Datenströme (Ausgabe) anzeigen. Der folgende Beispielcode startet die Kompilierung der **SampleConfig**-Konfiguration, wartet, bis die Kompilierung abgeschlossen ist, und zeigt dann die Datenströme an.

```powershell
$CompilationJob = Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig"

while($CompilationJob.EndTime –eq $null -and $CompilationJob.Exception –eq $null)
{
    $CompilationJob = $CompilationJob | Get-AzureRmAutomationDscCompilationJob
    Start-Sleep -Seconds 3
}

$CompilationJob | Get-AzureRmAutomationDscCompilationJobOutput –Stream Any
```

## <a name="basic-parameters"></a>Grundlegende Parameter
Die Parameterdeklaration in DSC-Konfigurationen, einschließlich der Parametertypen und -eigenschaften, funktioniert genauso wie in Azure Automation-Runbooks. Informationen zu Runbookparametern finden Sie unter [Starten eines Runbooks in Azure Automation](automation-starting-a-runbook.md) .

Das folgende Beispiel verwendet zwei Parameter namens **FeatureName** und **IsPresent**, um die Werte der Eigenschaften in der während der Kompilierung generierten **ParametersExample.sample**-Knotenkonfiguration zu ermitteln.

```powershell
Configuration ParametersExample
{
    param(
        [Parameter(Mandatory=$true)]

        [string] $FeatureName,

        [Parameter(Mandatory=$true)]
        [boolean] $IsPresent
    )

    $EnsureString = "Present"
    if($IsPresent -eq $false)
    {
        $EnsureString = "Absent"
    }

    Node "sample"
    {
        WindowsFeature ($FeatureName + "Feature")
        {
            Ensure = $EnsureString
            Name = $FeatureName
        }
    }
}
```

Sie können DSC-Konfigurationen kompilieren, die grundlegende Parameter im Azure Automation DSC-Portal oder in Azure PowerShell verwenden:

### <a name="portal"></a>Portal

Im Portal können Sie Parameterwerte eingeben, nachdem Sie auf **Kompilieren**geklickt haben.

![alt text](./media/automation-dsc-compile/DSC_compiling_1.png)

### <a name="powershell"></a>PowerShell

PowerShell erfordert Parameter in einer [Hashtabelle](http://technet.microsoft.com/library/hh847780.aspx) , in der der Schlüssel dem Parameternamen entspricht und der Wert gleich dem Parameterwert ist.

```powershell
$Parameters = @{
    "FeatureName" = "Web-Server"
    "IsPresent" = $False
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "ParametersExample" -Parameters $Parameters
```

Informationen zum Übergeben von PSCredentials als Parameter finden Sie im Abschnitt <a href="#credential-assets">**Anmeldeinformationen**</a> weiter unten.

## <a name="configurationdata"></a>ConfigurationData
**ConfigurationData** können Sie bei Verwendung von PowerShell DSC die Konfiguration der Struktur von jeglicher umgebungsspezifischer Konfiguration trennen. Informationen zu [ConfigurationData](http://blogs.msdn.com/b/powershell/archive/2014/01/09/continuous-deployment-using-dsc-with-minimal-change.aspx) finden Sie unter **Separating "What" from "Where" in PowerShell DSC**.

> [!NOTE]
> Sie können **ConfigurationData** während der Kompilierung in Azure Automation DSC mithilfe von Azure PowerShell verwenden, nicht jedoch im Azure-Portal.

Die folgende DSC-Beispielkonfiguration verwendet **ConfigurationData** über die Schlüsselwörter **$ConfigurationData** und **$AllNodes**. Für dieses Beispiel benötigen Sie außerdem das [Modul **xWebAdministration**](https://www.powershellgallery.com/packages/xWebAdministration/):

```powershell
Configuration ConfigurationDataSample
{
    Import-DscResource -ModuleName xWebAdministration -Name MSFT_xWebsite

    Write-Verbose $ConfigurationData.NonNodeData.SomeMessage

    Node $AllNodes.Where{$_.Role -eq "WebServer"}.NodeName
    {
        xWebsite Site
        {
            Name = $Node.SiteName
            PhysicalPath = $Node.SiteContents
            Ensure   = "Present"
        }
    }
}
```

Sie können die obige DSC-Konfiguration über PowerShell kompilieren. Mit dem folgenden PowerShell-Skript werden dem Azure Automation DSC-Pullserver die beiden Knotenkonfigurationen **ConfigurationDataSample.MyVM1** und **ConfigurationDataSample.MyVM3** hinzugefügt:

```powershell
$ConfigData = @{
    AllNodes = @(
        @{
            NodeName = "MyVM1"
            Role = "WebServer"
        },
        @{
            NodeName = "MyVM2"
            Role = "SQLServer"
        },
        @{
            NodeName = "MyVM3"
            Role = "WebServer"
        }
    )

    NonNodeData = @{
        SomeMessage = "I love Azure Automation DSC!"
    }
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "ConfigurationDataSample" -ConfigurationData $ConfigData
```

## <a name="assets"></a>Objekte

Objektverweise in Azure Automation DSC-Konfigurationen und -Runbooks sind gleich. Weitere Informationen finden Sie unter den folgenden Links:

* [Zertifikate](automation-certificates.md)
* [Verbindungen](automation-connections.md)
* [Anmeldeinformationen](automation-credentials.md)
* [Variablen](automation-variables.md)

### <a name="credential-assets"></a>Anmeldeinformationen

DSC-Konfigurationen in Azure Automation können mithilfe von **Get-AzureRmAutomationCredential** auf Anmeldeinformationen verweisen. Diese Informationen können aber bei Bedarf auch über Parameter übergeben werden. Wenn in einer Konfiguration ein Parameter des Typs **PSCredential** vorhanden ist, müssen Sie den Zeichenfolgennamen eines Anmeldeinformationsobjekts von Azure Automation als Wert dieses Parameters und nicht als PSCredential-Objekt übergeben. Im Hintergrund wird das Azure Automation-Anmeldeinformationsobjekt mit diesem Namen abgerufen und an die Konfiguration übergeben.

Um die Sicherheit von Anmeldeinformationen in Knotenkonfigurationen (MOF-Konfigurationsdokumente) zu gewährleisten, müssen diese Informationen in der MOF-Datei der Knotenkonfiguration verschlüsselt werden. Azure Automation geht sogar noch einen Schritt weiter und verschlüsselt die gesamte MOF-Datei. Zurzeit müssen Sie PowerShell DSC jedoch noch darüber informieren, dass die Anmeldeinformationen während der Generierung der Knotenkonfiguration-MOF-Datei im Klartext übergeben werden. PowerShell DSC hat keinerlei Informationen darüber, dass Azure Automation die gesamte MOF-Datei nach dem Generieren über einen Kompilierungsauftrag verschlüsselt.

Verwenden Sie [**ConfigurationData**](#configurationdata). Übergeben Sie `PSDscAllowPlainTextPassword = $true` über **ConfigurationData** für den Namen jedes Knotenblocks, der in der DSC-Konfiguration angezeigt wird und Anmeldeinformationen verwendet.

Das folgende Beispiel zeigt eine DSC-Konfiguration, die ein Automation-Anmeldeinformationsobjekt verwendet.

```powershell
Configuration CredentialSample
{
    $Cred = Get-AzureRmAutomationCredential -ResourceGroupName "ResourceGroup01" -AutomationAccountName "AutomationAcct" -Name "SomeCredentialAsset"

    Node $AllNodes.NodeName
    {
        File ExampleFile
        {
            SourcePath = "\\Server\share\path\file.ext"
            DestinationPath = "C:\destinationPath"
            Credential = $Cred
        }
    }
}
```

Sie können die obige DSC-Konfiguration über PowerShell kompilieren. Mit dem folgenden PowerShell-Skript werden dem Azure Automation DSC-Pullserver die beiden Knotenkonfigurationen **CredentialSample.MyVM1** und **CredentialSample.MyVM2** hinzugefügt.

```powershell
$ConfigData = @{
    AllNodes = @(
        @{
            NodeName = "*"
            PSDscAllowPlainTextPassword = $True
        },
        @{
            NodeName = "MyVM1"
        },
        @{
            NodeName = "MyVM2"
        }
    )
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "CredentialSample" -ConfigurationData $ConfigData
```

## <a name="importing-node-configurations"></a>Importieren von Knotenkonfigurationen

Sie können auch Knotenkonfigurationen (MOF-Dateien) importieren, die Sie außerhalb von Azure kompiliert haben. Ein Vorteil besteht darin, dass diese Knotenkonfigurationen signiert werden können.
Eine signierte Knotenkonfiguration wird lokal vom DSC-Agent auf einem verwalteten Knoten überprüft, um sicherzustellen, dass die auf den Knoten angewendete Konfiguration von einer autorisierten Quelle stammt.

> [!NOTE]
> Sie können signierte Konfigurationen in Ihr Azure Automation-Konto importieren, Azure Automation unterstützt aber derzeit nicht das Kompilieren von signierten Konfigurationen.

> [!NOTE]
> Knotenkonfigurationsdateien dürfen nicht größer als 1 MB sein, damit sie in Azure Automation importiert werden können.

Informationen zum Signieren von Knotenkonfigurationen finden Sie unter https://msdn.microsoft.com/en-us/powershell/wmf/5.1/dsc-improvements#how-to-sign-configuration-and-module.

### <a name="importing-a-node-configuration-in-the-azure-portal"></a>Importieren von Knotenkonfigurationen im Azure-Portal

1. Klicken Sie in Ihrem Automation-Konto auf **DSC-Knotenkonfigurationen**.

    ![DSC-Knotenkonfigurationen](./media/automation-dsc-compile/node-config.png)
2. Klicken Sie auf dem Blatt **DSC-Knotenkonfigurationen** auf **Knotenkonfiguration hinzufügen**.
3. Klicken Sie auf dem Blatt **Importieren** auf das Ordnersymbol neben dem Textfeld **Knotenkonfigurationsdatei**, um eine Knotenkonfigurationsdatei (MOF) auf dem lokalen Computer zu suchen.

    ![Suchen einer lokalen Datei](./media/automation-dsc-compile/import-browse.png)
4. Geben Sie im Textfeld **Konfigurationsname** einen Namen ein. Dieser Name muss mit dem Namen der Konfiguration übereinstimmen, aus der die Knotenkonfiguration kompiliert wurde.
5. Klicken Sie auf **OK**.

### <a name="importing-a-node-configuration-with-powershell"></a>Importieren einer Knotenkonfiguration mit PowerShell

Sie können das Cmdlet [Import-AzureRmAutomationDscNodeConfiguration](/powershell/module/azurerm.automation/import-azurermautomationdscnodeconfiguration) verwenden, um eine Knotenkonfiguration in Ihr Automation-Konto zu importieren.

```powershell
Import-AzureRmAutomationDscNodeConfiguration -AutomationAccountName "MyAutomationAccount" -ResourceGroupName "MyResourceGroup" -ConfigurationName "MyNodeConfiguration" -Path "C:\MyConfigurations\TestVM1.mof"
```




