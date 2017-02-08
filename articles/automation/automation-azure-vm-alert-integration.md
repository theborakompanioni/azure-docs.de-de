---
title: " Wartung von Azure-VM-Warnungen mit Automation-Runbooks | Microsoft Docs"
description: Dieser Artikel beschreibt das Integrieren von Azure Virtual Machine-Warnungen in Azure Automation-Runbooks und die automatische Problembehandlung.
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 1f7baa7f-7283-4a4f-9385-3f5cd1062c7f
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/14/2016
ms.author: csand;magoedte
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 9abb6ee8eed01ef84ee10fc2c70ea23bf482dd1c


---
# <a name="azure-automation-scenario---remediate-azure-vm-alerts"></a>Azure Automation-Szenario – Wartung nach Azure-VM-Warnungen
In Azure Automation und Azure Virtual Machines wurde ein neues Feature veröffentlicht, mit dem Sie VM-Warnungen für die Ausführung von Automation-Runbooks konfigurieren können. Diese neue Funktion ermöglicht es Ihnen, als Reaktion auf VM-Warnungen automatisch eine Standardwartung durchzuführen, beispielsweise das Neustarten oder Beenden des virtuellen Computers.

Zuvor konnten Sie während der Erstellung von VM-Warnungsregeln [einen Automation-Webhook](https://azure.microsoft.com/blog/using-azure-automation-to-take-actions-on-azure-alerts/) für ein Runbook angeben, um das Runbook immer dann auszuführen, wenn die Warnung ausgelöst wurde. Dazu mussten Sie jedoch das Runbook erstellen, den Webhook für das Runbook erstellen und anschließend den Webhub beim Erstellen der Warnungsregel kopieren und einfügen. Mit dieser neuen Version ist der Prozess sehr viel einfacher, weil Sie ein Runbook bei der Erstellung von Warnungsregeln direkt aus einer Liste auswählen können. Außerdem können Sie ein Automation-Konto für die Ausführung des Runbooks wählen oder einfach ein Konto erstellen.

In diesem Artikel wird gezeigt, wie einfach es ist, eine Azure-VM-Warnung einzurichten und ein Automation-Runbook so zu konfigurieren, dass es bei jedem Auslösen der Warnung ausgeführt wird. Beispiele für Szenarien sind das Neustarten eines virtuellen Computers, wenn die Speicherauslastung einen Schwellenwert überschreitet, weil eine Anwendung auf dem virtuellen Computer einen Arbeitsspeicherverlust aufweist, oder das Beenden eines virtuellen Computers, wenn die CPU-Benutzerzeit seit einer Stunde weniger als 1 % beträgt und der virtuelle Computer nicht verwendet wird. Außerdem erklären wir, wie die automatische Erstellung eines Dienstprinzipals in Ihrem Automation-Konto die Verwendung von Runbooks bei der Wartung nach Azure-Warnungen vereinfacht.

## <a name="create-an-alert-on-a-vm"></a>Erstellen einer Warnung auf einem virtuellen Computer
Führen Sie die folgenden Schritte durch, um eine Warnung so zu konfigurieren, dass bei Erreichen des zugehörigen Schwellenwerts ein Runbook gestartet wird.

> [!NOTE]
> In dieser Version unterstützen wir nur virtuelle V2-Maschinen. Die Unterstützung für klassische virtuelle Computer wird bald hinzugefügt.  
> 
> 

1. Melden Sie sich beim Azure-Portal an, und klicken Sie auf **Virtual Machines**.  
2. Wählen Sie einen Ihrer virtuellen Computer aus.  Das Dashboardblatt des virtuellen Computers wird angezeigt, rechts davon das Blatt **Einstellungen** .  
3. Wählen Sie auf dem Blatt **Einstellungen** unter dem Abschnitt „Überwachung“ die Option **Warnungsregeln** aus.
4. Klicken Sie auf dem Blatt **Warnungsregeln** auf **Warnung hinzufügen**.

Dadurch wird das Blatt **Warnungsregel hinzufügen** geöffnet. Auf diesem Blatt können Sie die Bedingungen für die Warnung konfigurieren und eine oder alle der folgenden Optionen wählen: eine E-Mail an jemanden senden, die Warnung über einen Webhook an ein anderes System weiterleiten und/oder als Reaktion auf das Problem ein Automation-Runbook ausführen.

## <a name="configure-a-runbook"></a>Konfigurieren eines Runbooks
Um ein Runbook zu konfigurieren, das ausgeführt wird, wenn der Warnungsschwellenwert für den virtuellen Computer erreicht ist, wählen Sie **Automation-Runbook**. Auf dem Blatt **Runbook konfigurieren** können Sie das auszuführende Runbook sowie das Automation-Konto auswählen, in dem das Runbook ausgeführt wird.

![Konfigurieren eines Automation-Runbooks und Erstellen eines neuen Automation-Kontos](media/automation-azure-vm-alert-integration/ConfigureRunbookNewAccount.png)

> [!NOTE]
> In dieser Version können Sie aus drei Runbooks auswählen, die vom Dienst bereitgestellt werden: VM neu starten, VM beenden oder VM entfernen (löschen).  Die Möglichkeit zum Auswählen anderer Runbooks oder eines Ihrer eigenen Runbooks steht in einer zukünftigen Version zur Verfügung.
> 
> 

![Zur Auswahl stehende Runbooks](media/automation-azure-vm-alert-integration/RunbooksToChoose.png)

Nach dem Auswählen eines der drei verfügbaren Runbooks wird die Dropdownliste **Automation-Konto** angezeigt. Darin können Sie ein Automation-Konto auswählen, das als ausführendes Konto für das Runbook verwendet wird. Runbooks müssen im Kontext eines [Automation-Kontos](automation-security-overview.md) ausgeführt werden, das Teil Ihres Azure-Abonnements ist. Sie können ein Automation-Konto auswählen, das Sie bereits erstellt haben, oder Sie können ein neues Automation-Konto erstellen.

Die bereitgestellten Runbooks authentifizieren sich bei Azure über einen Dienstprinzipal. Wenn Sie das Runbook in einem Ihrer vorhandenen Automation-Konten ausführen möchten, erstellen wir automatisch den Dienstprinzipal für Sie. Wenn Sie ein neues Automation-Konto erstellen möchten, erstellen wir automatisch das Konto und den Dienstprinzipal. In beiden Fällen werden auch im Automation-Konto zwei Assets erstellt: Ein Zertifikatasset mit dem Namen **AzureRunAsCertificate** und ein Verbindungsasset mit dem Namen **AzureRunAsConnection**. Die Runbooks verwenden **AzureRunAsConnection** für die Authentifizierung bei Azure, um die Verwaltungsaktion für den virtuellen Computer auszuführen.

> [!NOTE]
> Der Dienstprinzipal wird im Rahmen des Abonnements erstellt und bekommt eine Teilnehmerrolle zugewiesen. Diese Rolle ist erforderlich, damit das Konto die Berechtigung zum Ausführen von Automation-Runbooks für die Verwaltung von virtuellen Azure-Computern erhält.  Die Erstellung eines Automaton-Kontos und/oder Dienstprinzipals ist eine einmalige Aufgabe. Nachdem sie erstellt wurden, können Sie dieses Konto zum Ausführen von Runbooks für andere Azure-VM-Warnungen verwenden.
> 
> 

Wenn Sie auf **OK** klicken, wird die Warnung konfiguriert, und wenn Sie die Option zum Erstellen eines neuen Automation-Kontos ausgewählt haben, wird dieses zusammen mit dem Dienstprinzipal erstellt.  Dieser Vorgang kann einige Sekunden dauern.  

![Runbook, das konfiguriert wird](media/automation-azure-vm-alert-integration/RunbookBeingConfigured.png)

Nach Abschluss der Konfiguration wird der Name des Runbooks auf dem Blatt **Warnungsregel hinzufügen** angezeigt.

![Konfiguriertes Runbook](media/automation-azure-vm-alert-integration/RunbookConfigured.png)

Klicken Sie auf dem Blatt **Warnungsregel hinzufügen** auf **OK**. Die Warnungsregel wird erstellt und aktiviert, wenn der virtuelle Computer ausgeführt wird.

### <a name="enable-or-disable-a-runbook"></a>Aktivieren oder Deaktivieren eines Runbooks
Wenn Sie ein Runbook für eine Warnung konfiguriert haben, können Sie es deaktivieren, ohne die Runbookkonfiguration zu entfernen. Dadurch kann die Warnung weiterhin ausgeführt werden, und Sie können vielleicht einige Warnungsregeln testen und das Runbook später wieder aktivieren.

## <a name="create-a-runbook-that-works-with-an-azure-alert"></a>Erstellen eines Runbooks, das mit einer Azure-Warnung funktioniert
Wenn Sie ein Runbook als Teil einer Azure-Warnungsregel auswählen, muss das Runbook eine Logik zum Verwalten der Warnungsdaten enthalten, die ihm übergeben werden.   Beim Konfigurieren eines Runbooks innerhalb einer Warnungsregel wird ein Webhook für das Runbook erstellt. Mithilfe dieses Webhooks wird das Runbook gestartet, sobald die Warnung ausgelöst wird.  Als eigentlicher Aufruf zum Starten des Runbooks dient eine HTTP POST-Anforderung an die Webhook-URL. Der Text der POST-Anforderung enthält ein JSON-formatiertes Objekt, das nützliche Eigenschaften im Zusammenhang mit der Warnung enthält.  Wie nachfolgend zu sehen ist, beinhalten die Warnungsdaten detaillierte Informationen wie subscriptionID, resourceGroupName, resourceName und resourceType.

### <a name="example-of-alert-data"></a>Beispiel für Warnungsdaten
```
{
    "WebhookName": "AzureAlertTest",
    "RequestBody": "{
    \"status\":\"Activated\",
    \"context\": {
        \"id\":\"/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/microsoft.insights/alertrules/AlertTest\",
        \"name\":\"AlertTest\",
        \"description\":\"\",
        \"condition\": {
            \"metricName\":\"CPU percentage guest OS\",
            \"metricUnit\":\"Percent\",
            \"metricValue\":\"4.26337916666667\",
            \"threshold\":\"1\",
            \"windowSize\":\"60\",
            \"timeAggregation\":\"Average\",
            \"operator\":\"GreaterThan\"},
        \"subscriptionId\":\<subscriptionID> \",
        \"resourceGroupName\":\"TestResourceGroup\",
        \"timestamp\":\"2016-04-24T23:19:50.1440170Z\",
        \"resourceName\":\"TestVM\",
        \"resourceType\":\"microsoft.compute/virtualmachines\",
        \"resourceRegion\":\"westus\",
        \"resourceId\":\"/subscriptions/<subscriptionId>/resourceGroups/TestResourceGroup/providers/Microsoft.Compute/virtualMachines/TestVM\",
        \"portalLink\":\"https://portal.azure.com/#resource/subscriptions/<subscriptionId>/resourceGroups/TestResourceGroup/providers/Microsoft.Compute/virtualMachines/TestVM\"
        },
    \"properties\":{}
    }",
    "RequestHeader": {
        "Connection": "Keep-Alive",
        "Host": "<webhookURL>"
    }
}
```

Wenn der Automation-Webhook-Dienst die HTTP POST-Anforderung empfängt, extrahiert er die Warnungsdaten und übergibt sie dem Runbook im Runbook-Eingabeparameter „WebhookData“.  Im folgenden Beispiel für ein Runbook wird gezeigt, wie der Parameter „WebhookData“ verwendet wird und wie die Warnungsdaten extrahiert und zum Verwalten der Azure-Ressource genutzt werden, die die Warnung ausgelöst hat.

### <a name="example-runbook"></a>Beispiel für ein Runbook
```
#  This runbook will restart an ARM (V2) VM in response to an Azure VM alert.

[OutputType("PSAzureOperationResponse")]

param ( [object] $WebhookData )

if ($WebhookData)
{
    # Get the data object from WebhookData
    $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

    # Assure that the alert status is 'Activated' (alert condition went from false to true)
    # and not 'Resolved' (alert condition went from true to false)
    if ($WebhookBody.status -eq "Activated")
    {
        # Get the info needed to identify the VM
        $AlertContext = [object] $WebhookBody.context
        $ResourceName = $AlertContext.resourceName
        $ResourceType = $AlertContext.resourceType
        $ResourceGroupName = $AlertContext.resourceGroupName
        $SubId = $AlertContext.subscriptionId

        # Assure that this is the expected resource type
        Write-Verbose "ResourceType: $ResourceType"
        if ($ResourceType -eq "microsoft.compute/virtualmachines")
        {
            # This is an ARM (V2) VM

            # Authenticate to Azure with service principal and certificate
            $ConnectionAssetName = "AzureRunAsConnection"
            $Conn = Get-AutomationConnection -Name $ConnectionAssetName
            if ($Conn -eq $null) {
                throw "Could not retrieve connection asset: $ConnectionAssetName. Check that this asset exists in the Automation account."
            }
            Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Write-Verbose
            Set-AzureRmContext -SubscriptionId $SubId -ErrorAction Stop | Write-Verbose

            # Restart the VM
            Restart-AzureRmVM -Name $ResourceName -ResourceGroupName $ResourceGroupName
        } else {
            Write-Error "$ResourceType is not a supported resource type for this runbook."
        }
    } else {
        # The alert status was not 'Activated' so no action taken
        Write-Verbose ("No action taken. Alert status: " + $WebhookBody.status)
    }
} else {
    Write-Error "This runbook is meant to be started from an Azure alert only."
}
```

## <a name="summary"></a>Zusammenfassung
Bei der Konfiguration einer Warnung auf einem virtuellen Azure-Computer haben Sie jetzt die Möglichkeit, ein Automation-Runbook einfach so zu konfigurieren, dass bei Auslösen der Warnung automatisch Wartungsaktionen durchgeführt werden. In dieser Version können Sie, je nach Warnungsszenario, aus Runbooks zum Neustarten, Beenden oder Löschen eines virtuellen Computers auswählen. Dies ist erst der Anfang im Hinblick auf Szenarien zum Steuern von Aktionen (Benachrichtigung, Problembehandlung, Wartung), die beim Auslösen einer Warnung automatisch ausgeführt werden.

## <a name="next-steps"></a>Nächste Schritte
* Informationen über die ersten Schritte mit grafischen Runbooks finden Sie unter [Mein erstes grafisches Runbook](automation-first-runbook-graphical.md)
* Informationen über die ersten Schritte mit PowerShell-Workflow-Runbooks finden Sie unter [Mein erstes PowerShell-Workflow-Runbook](automation-first-runbook-textual.md)
* Informationen über die verschiedenen Runbooktypen, ihre Vorteile und Einschränkungen finden Sie unter [Azure Automation-Runbooktypen](automation-runbook-types.md)




<!--HONumber=Nov16_HO3-->


