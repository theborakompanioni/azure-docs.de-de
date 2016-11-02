<properties
   pageTitle="Anzeigen von Bereitstellungsvorgängen mit PowerShell | Microsoft Azure"
   description="Beschreibt, wie Sie mit Azure PowerShell Probleme in der Resource Manager-Bereitstellung erkennen können."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="06/14/2016"
   ms.author="tomfitz"/>

# Anzeigen von Bereitstellungsvorgängen mit Azure PowerShell

> [AZURE.SELECTOR]
- [Portal](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure-Befehlszeilenschnittstelle](resource-manager-troubleshoot-deployments-cli.md)
- [REST-API](resource-manager-troubleshoot-deployments-rest.md)

Sie können die Vorgänge für eine Bereitstellung über Azure PowerShell anzeigen. Die Anzeige der Vorgänge ist wahrscheinlich dann am interessantesten, wenn während der Bereitstellung ein Fehler auftritt. Daher konzentriert sich dieser Artikel auf das Anzeigen von fehlerhaften Vorgängen. PowerShell enthält Cmdlets, mit denen Sie einfach Fehler finden und potenzielle Korrekturen ermitteln können.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

Einige Fehler lassen sich vermeiden, indem Sie Ihre Vorlage und die Infrastruktur vor der Bereitstellung überprüfen. Sie können auch zusätzliche Anforderungs- und Antwortinformationen während der Bereitstellung protokollieren, die später für die Problembehandlung hilfreich sein können. Unter [Bereitstellen eine Ressourcengruppe mit Azure Resource Manager-Vorlage](resource-group-template-deploy.md) erfahren Sie mehr über die Überprüfung und Protokollierung von Anforderungs- und Antwortinformationen.

## Verwenden von Bereitstellungsvorgängen zur Problembehandlung

1. Verwenden Sie den Befehl **Get-AzureRmResourceGroupDeployment**, um den allgemeinen Status einer Bereitstellung abzurufen. Sie können die Ergebnisse nach den Bereitstellungen filtern, die nicht erfolgreich waren.

        Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup | Where-Object ProvisioningState -eq Failed
        
    Die fehlgeschlagenen Bereitstellungen werden in folgendem Format zurückgegeben:
        
        DeploymentName          : Microsoft.Template
        ResourceGroupName       : ExampleGroup
        ProvisioningState       : Failed
        Timestamp               : 6/14/2016 9:55:21 PM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
                    Name                Type                 Value
                    ===============     ===================  ==========
                    storageAccountName  String               tfstorage9855
                    adminUsername       String               tfadmin
                    adminPassword       SecureString
                    dnsNameforLBIP      String               dns
                    vmNamePrefix        String               myVM
                    imagePublisher      String               MicrosoftWindowsServer
                    imageOffer          String               WindowsServer
                    imageSKU            String               2012-R2-Datacenter
                    lbName              String               myLB
                    nicNamePrefix       String               nic
                    publicIPAddressName String               myPublicIP
                    vnetName            String               myVNET
                    vmSize              String               Standard_D1

        Outputs                 :
        DeploymentDebugLogLevel :

2. Jede Bereitstellung besteht in der Regel aus mehreren Vorgängen. Dabei stellt jeder Vorgang einen Schritt im Bereitstellungsprozess dar. Um einen Bereitstellungsfehler zu ermitteln, benötigen Sie in der Regel Einzelheiten zu den Bereitstellungsvorgängen. Den Status der Vorgänge können Sie mit **Get-AzureRmResourceGroupDeploymentOperation** abrufen.

        Get-AzureRmResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName Microsoft.Template
        
    Mit diesem Befehl werden mehrere Vorgänge im folgenden Format zurückgegeben:
        
        Id             : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/Microsoft.Template/operations/A3EB2DA598E0A780
        OperationId    : A3EB2DA598E0A780
        Properties     : @{provisioningOperation=Create; provisioningState=Succeeded; timestamp=2016-06-14T21:55:15.0156208Z;
                         duration=PT23.0227078S; trackingId=11d376e8-5d6d-4da8-847e-6f23c6443fbf;
                         serviceRequestId=0196828d-8559-4bf6-b6b8-8b9057cb0e23; statusCode=OK; targetResource=}
        PropertiesText : {duration:PT23.0227078S, provisioningOperation:Create, provisioningState:Succeeded,
                         serviceRequestId:0196828d-8559-4bf6-b6b8-8b9057cb0e23...}

3. Um weitere Informationen zu fehlgeschlagenen Vorgängen zu erhalten, rufen Sie die Eigenschaften für Vorgänge mit dem Zustand **Fehler** ab.

        (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object ProvisioningState -eq Failed
        
    Dadurch werden alle fehlerhaften Vorgänge im folgenden Format zurückgegeben:
        
        provisioningOperation : Create
        provisioningState     : Failed
        timestamp             : 2016-06-14T21:54:55.1468068Z
        duration              : PT3.1449887S
        trackingId            : f4ed72f8-4203-43dc-958a-15d041e8c233
        serviceRequestId      : a426f689-5d5a-448d-a2f0-9784d14c900a
        statusCode            : BadRequest
        statusMessage         : @{error=}
        targetResource        : @{id=/subscriptions/{guid}/resourceGroups/ExampleGroup/providers/
                                Microsoft.Network/publicIPAddresses/myPublicIP;
                                resourceType=Microsoft.Network/publicIPAddresses; resourceName=myPublicIP}

    Notieren Sie die Nachverfolgungs-ID für den Vorgang. Sie verwenden sie im nächsten Schritt, um sich auf einen bestimmten Vorgang zu konzentrieren.

4. Verwenden Sie den folgenden Befehl, um die Statusmeldung für einen bestimmten fehlerhaften Vorgang abzurufen:

        ((Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object trackingId -eq f4ed72f8-4203-43dc-958a-15d041e8c233).StatusMessage.error
        
    Ausgabe des Befehls:
        
        code           message                                                                        details
        ----           -------                                                                        -------
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}

## Verwenden von Überwachungsprotokollen zur Problembehandlung

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Gehen Sie wie folgt vor, um Fehler für eine Bereitstellung anzuzeigen:

1. Um Protokolleinträge abzurufen, führen Sie den Befehl **Get-AzureRmLog** aus. Sie können die Parameter **ResourceGroup** und **Status** verwenden, um nur Ereignisse zurückzugeben, die für eine einzelne Ressourcengruppe nicht erfolgreich waren. Wenn Sie keine Start- und Endzeit angeben, werden die Einträge der letzten Stunde zurückgegeben. Führen Sie beispielsweise Folgendes aus, um die nicht erfolgreichen Vorgänge für die letzte Stunde der Ausführung zurückzugeben:

        Get-AzureRmLog -ResourceGroup ExampleGroup -Status Failed

    Sie können hierbei eine bestimmte Zeitspanne angeben. Im nächsten Beispiel suchen wir nach nicht erfolgreichen Aktionen innerhalb des letzten Tages.

        Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-1) -Status Failed
      
    Sie können für fehlerhafte Aktionen auch eine genaue Start- und Endzeit festlegen:

        Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime 2015-08-28T06:00 -EndTime 2015-09-10T06:00 -Status Failed

2. Wenn dieser Befehl zu viele Einträge und Eigenschaften zurückgibt, können Sie die Überwachung genauer fokussieren, indem Sie die **Properties**-Eigenschaft abrufen. Hier wird auch der Parameter **DetailedOutput** einbezogen, damit die Fehlermeldungen angezeigt werden.

        (Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-1) -DetailedOutput).Properties
        
    Eigenschaften der Protokolleinträge werden in folgendem Format zurückgegeben:
        
        Content
        -------
        {} 
        {[statusCode, BadRequest], [statusMessage, {"error":{"code":"DnsRecordInUse","message":"DNS record dns.westus.clouda...
        {[statusCode, BadRequest], [serviceRequestId, a426f689-5d5a-448d-a2f0-9784d14c900a], [statusMessage, {"error":{"code...

3. Sehen wir uns basierend auf diesen Ergebnissen das zweite Element genauer an. Sie können die Suchergebnisse weiter eingrenzen, indem Sie die Statusmeldung für diesen Eintrag anzeigen.

        ((Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -DetailedOutput -StartTime (Get-Date).AddDays(-1)).Properties[1].Content["statusMessage"] | ConvertFrom-Json).error
        
    Ausgabe des Befehls:
        
        code           message                                                                        details
        ----           -------                                                                        -------
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}



## Nächste Schritte

- Unterstützung beim Beheben bestimmter Bereitstellungsfehler finden Sie unter [Beheben von häufigen Fehlern beim Bereitstellen von Ressourcen in Azure mit Azure Resource Manager](resource-manager-common-deployment-errors.md).
- Informationen zur Überwachung anderer Arten von Aktionen anhand der Überwachungsprotokolle finden Sie unter [Überwachen von Vorgängen mit Resource Manager](resource-group-audit.md).
- Informationen zum Überprüfen der Bereitstellung vor der Ausführung finden Sie unter [Bereitstellen einer Ressourcengruppe mit Azure Resource Manager-Vorlagen](resource-group-template-deploy.md).

<!---HONumber=AcomDC_0622_2016-->