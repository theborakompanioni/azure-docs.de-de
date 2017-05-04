---
title: Verbindungsobjekte in Azure Automation | Microsoft Docs
description: "Verbindungsobjekte in Azure Automation enthalten die Informationen, die zur Verbindungsherstellung mit einem externen Dienst oder einer externen Anwendung aus einem Runbook oder einer DSC-Konfiguration erforderlich sind. Dieser Artikel stellt eine ausführliche Beschreibung von Verbindungen bereit und zeigt, wie diese in Textrunbooks und grafischen Runbooks eingesetzt werden."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: f0239017-5c66-4165-8cca-5dcb249b8091
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/13/2017
ms.author: magoedte; bwren
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: bfb479020238bb4c2763f439c744aeddf97c8908
ms.lasthandoff: 04/27/2017

---

# <a name="connection-assets-in-azure-automation"></a>Verbindungsobjekte in Azure Automation

Ein Automation-Verbindungsobjekt enthält die Informationen, die zur Verbindungsherstellung mit einem externen Dienst oder einer externen Anwendung aus einem Runbook oder einer DSC-Konfiguration erforderlich sind. Dies kann Informationen einschließen, die für die Authentifizierung erforderlich sind, beispielsweise ein Benutzername und ein Kennwort oder Verbindungsinformationen wie eine URL oder ein Port. Der Wert einer Verbindung liegt darin, dass alle Eigenschaften zur Verbindungsherstellung mit einer bestimmten Anwendung in einem Objekt gespeichert werden, statt mehrere Variablen zu erstellen. Der Benutzer kann die Werte für eine Verbindung an einem Ort bearbeiten, und Sie können den Namen einer Verbindung in einem einzelnen Parameter an ein Runbook oder eine DSC-Konfiguration übergeben. Auf die Eigenschaften für eine Verbindung kann im Runbook oder in der DSC-Konfiguration über die Aktivität **Get-AutomationConnection** zugegriffen werden.

Wenn Sie eine Verbindung erstellen, müssen Sie einen *Verbindungstyp*angeben. Der Verbindungstyp ist eine Vorlage, die einen Satz von Eigenschaften definiert. Die Verbindung definiert Werte für jede der Eigenschaften, die im zugehörigen Verbindungstyp definiert sind. Verbindungstypen werden Azure Automation in Integrationsmodulen hinzugefügt oder mit der [Azure Automation-API](http://msdn.microsoft.com/library/azure/mt163818.aspx) erstellt, wenn das Integrationsmodul einen Verbindungstyp enthält und in Ihr Automation-Konto importiert wird. Andernfalls müssen Sie eine Metadatendatei erstellen, um einen Automation-Verbindungstyp anzugeben.  Weitere Informationen dazu finden Sie unter [Integrationsmodule](automation-integration-modules.md).  

>[!NOTE] 
>Zu den sicheren Objekten in Azure Automation gehören Anmeldeinformationen, Zertifikate, Verbindungen und verschlüsselte Variablen. Diese Objekte werden mithilfe eines eindeutigen Schlüssels verschlüsselt und in Azure Automation gespeichert, der für jedes Automation-Konto generiert wird. Dieser Schlüssel wird mit einem Masterzertifikat verschlüsselt und in Azure Automation gespeichert. Vor dem Speichern eines sicheren Objekts wird der Schlüssel für das Automation-Konto mit dem Masterzertifikat verschlüsselt und anschließend zum Verschlüsseln des Objekts verwendet.

## <a name="windows-powershell-cmdlets"></a>Windows PowerShell-Cmdlets

Die Cmdlets in der folgenden Tabelle werden zum Erstellen und Verwalten von Automation-Verbindungen mit Windows PowerShell verwendet. Sie gehören zum Funktionsumfang des [Azure PowerShell-Moduls](/powershell/azure/overview) , das zur Verwendung in Automation-Runbooks und DSC-Konfigurationen verfügbar ist.

|Cmdlet|Beschreibung|
|:---|:---|
|[Get-AzureRmAutomationConnection](/powershell/module/azurerm.automation/get-azurermautomationconnection)|Ruft eine Verbindung ab. Enthält eine Hashtabelle mit den Werten der Felder für die Verbindung.|
|[New-AzureRmAutomationConnection](/powershell/module/azurerm.automation/new-azurermautomationconnection)|Erstellt eine neue Verbindung.|
|[Remove-AzureRmAutomationConnection](/powershell/module/azurerm.automation/remove-azurermautomationconnection)|Entfernt eine vorhandene Verbindung.|
|[Set-AzureRmAutomationConnectionFieldValue](/powershell/module/azurerm.automation/set-azurermautomationconnectionfieldvalue)|Legt den Wert für ein bestimmtes Feld einer vorhandenen Verbindung fest.|

## <a name="activities"></a>Aktivitäten

Die Aktivitäten in der folgenden Tabelle werden für den Zugriff auf Verbindungen in einem Runbook oder einer DSC-Konfiguration verwendet.

|Aktivitäten|Beschreibung|
|---|---|
|[Get-AutomationConnection](/powershell/module/azure/get-azureautomationconnection?view=azuresmps-3.7.0)|Ruft eine zu verwendende Verbindung ab. Gibt eine Hashtabelle mit den Eigenschaften der Verbindung zurück.|

>[!NOTE] 
>Vermeiden Sie die Verwendung von Variablen mit dem Parameter „-Name“ von **Get-AutomationConnection**, da dies die Ermittlung von Abhängigkeiten zwischen Runbooks oder DSC-Konfigurationen und Verbindungsobjekten zur Entwurfszeit erschweren kann.

## <a name="creating-a-new-connection"></a>Erstellen einer neuen Verbindung

### <a name="to-create-a-new-connection-with-the-azure-portal"></a>So erstellen Sie eine neue Verbindung mit dem Azure-Portal

1. Klicken Sie in Ihrem Automation-Konto auf **Objekte**, um das Blatt **Objekte** zu öffnen.
2. Klicken Sie auf den Bereich **Verbindungen**, um das Blatt **Verbindungen** zu öffnen.
3. Klicken Sie oben im Blatt auf **Verbindung hinzufügen** .
4. Wählen Sie in der Dropdownliste **Typ** die Art der Verbindung, die Sie erstellen möchten. Das Formular zeigt die Eigenschaften für den gewählten Verbindungstyp an.
5. Geben Sie die erforderlichen Daten ein, und klicken Sie auf **Erstellen** , um die neue Verbindung zu speichern.

### <a name="to-create-a-new-connection-with-the-azure-classic-portal"></a>So erstellen Sie eine neue Verbindung mit dem klassischen Azure-Portal

1. Klicken Sie in Ihrem Automation-Konto im oberen Fensterbereich auf **Objekte** .
2. Klicken Sie unten im Fenster auf **Einstellung hinzufügen**.
3. Klicken Sie auf **Verbindung hinzufügen**.
4. Wählen Sie in der Dropdownliste **Verbindungstyp** die Art der Verbindung, die Sie erstellen möchten.  Der Assistent zeigt die Eigenschaften für den gewählten Verbindungstyp an.
5. Schließen Sie den Assistenten ab, und aktivieren Sie das Kontrollkästchen, um die neue Verbindung zu speichern.

### <a name="to-create-a-new-connection-with-windows-powershell"></a>So erstellen Sie eine neue Verbindung mit Windows PowerShell

Erstellen Sie mit dem Windows PowerShell-Cmdlet [New-AzureRmAutomationConnection](/powershell/module/azurerm.automation/new-azurermautomationconnection) eine neue Verbindung. Dieses Cmdlet verfügt über den Parameter **ConnectionFieldValues** , der eine [Hashtabelle](http://technet.microsoft.com/library/hh847780.aspx) erwartet, mit deren Hilfe die Werte für jede der im Verbindungstyp definierten Eigenschaften festgelegt werden.

Wenn Sie mit dem [ausführenden Konto](automation-sec-configure-azure-runas-account.md) von Automation zum Authentifizieren von Runbooks mithilfe des Dienstprinzipals vertraut sind, verwenden Sie das PowerShell-Skript, das als Alternative zum Erstellen des ausführenden Kontos über das Portal bereitgestellt wurde, mit den folgenden Beispielbefehlen zum Erstellen eines neuen Verbindungsobjekts.  

    $ConnectionAssetName = "AzureRunAsConnection"
    $ConnectionFieldValues = @{"ApplicationId" = $Application.ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Cert.Thumbprint; "SubscriptionId" = $SubscriptionId}
    New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -ConnectionTypeName AzureServicePrincipal -ConnectionFieldValues $ConnectionFieldValues 

Sie können das Skript verwenden, um das Verbindungsobjekt zu erstellen. Als Sie nämlich Ihr Automation-Konto erstellt haben, enthielt es automatisch standardmäßig mehrere globale Module sowie den Verbindungstyp **AzurServicePrincipal** zum Erstellen des **AzureRunAsConnection**-Verbindungsobjekts.  Sie müssen dies bedenken. Wenn Sie nämlich versuchen, ein neues Verbindungsobjekt für eine Verbindung mit einem Dienst oder einer Anwendung mit einer anderen Authentifizierungsmethode zu erstellen, wird dies fehlschlagen, da der Verbindungstyp noch nicht in Ihrem Automation-Konto definiert ist.  Weitere Informationen zum Erstellen eines eigenen Verbindungstyps für Ihr benutzerdefiniertes Modul oder ein Modul aus dem [PowerShell-Katalog](https://www.powershellgallery.com) finden Sie unter [Integrationsmodule](automation-integration-modules.md).
  
## <a name="using-a-connection-in-a-runbook-or-dsc-configuration"></a>Verwenden einer Verbindung in einem Runbook oder einer DSC-Konfiguration

Rufen Sie eine Verbindung in einem Runbook oder einer DSC-Konfiguration mit dem Cmdlet **Get-AutomationConnection** ab.  Sie können die Aktivität [Get AzureRmAutomationConnection](https://docs.microsoft.com/powershell/resourcemanager/azurerm.automation/v1.0.12/Get-AzureRmAutomationConnection?redirectedfrom=msdn) nicht verwenden.  Mithilfe dieser Aktivität werden die Werte der verschiedenen Felder in der Verbindung abgerufen und als [Hashtabelle](http://go.microsoft.com/fwlink/?LinkID=324844) zurückgegeben, die anschließend mithilfe geeigneter Befehle im Runbook oder in der DSC-Konfiguration verwendet werden kann.

### <a name="textual-runbook-sample"></a>Beispiel für ein Textrunbook

Die folgenden Beispielbefehle zeigen, wie das zuvor erwähnte ausführende Konto für die Authentifizierung bei Azure Resource Manager-Ressourcen in Ihrem Runbook verwendet wird.  Dabei wird das Verbindungsobjekt verwendet, das für das ausführende Konto steht und auf den zertifikatbasierten Dienstprinzipal verweist (und keine Anmeldeinformationen).  

    $Conn = Get-AutomationConnection -Name AzureRunAsConnection 
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint 

### <a name="graphical-runbook-samples"></a>Beispiel für grafische Runbooks

Sie können einem grafischen Runbook eine **Get-AutomationConnection**-Aktivität hinzufügen, indem Sie im Bibliotheksbereich des grafischen Editors mit der rechten Maustaste auf die Verbindung klicken und **Zum Zeichenbereich hinzufügen** auswählen.

![](media/automation-connections/connection-add-canvas.png)

Die folgende Abbildung zeigt ein Beispiel für die Verwendung einer Verbindung in einem grafischen Runbook.  Dies ist das gleiche Beispiel wie oben für die Authentifizierung mit dem ausführenden Konto mit einem Textrunbook.  Dieses Beispiel verwendet das Dataset **Konstanter Wert** für die Aktivität **RunAs-Verbindung abrufen**, die ein Verbindungsobjekt für die Authentifizierung nutzt.  Hier wird eine [Pipelineverknüpfung](automation-graphical-authoring-intro.md#links-and-workflow) verwendet, da der ServicePrincipalCertificate-Parametersatz ein einzelnes Objekt erwartet.

![](media/automation-connections/automation-get-connection-object.png)

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie [Verknüpfungen bei der grafischen Erstellung](automation-graphical-authoring-intro.md#links-and-workflow), um zu verstehen, wie Sie den Ablauf der Logik in Ihren Runbooks steuern können.  

- Weitere Informationen zur Verwendung von PowerShell-Modulen in Azure Automation und bewährte Methoden zum Erstellen eigener PowerShell-Module, die als Integrationsmodule in Azure Automation verwendet werden können, finden Sie unter [Integrationsmodule](automation-integration-modules.md).  

