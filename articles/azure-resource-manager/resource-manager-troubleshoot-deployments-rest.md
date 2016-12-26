---
title: "Anzeigen von Bereitstellungsvorgängen mit der REST-API | Microsoft Docs"
description: "Beschreibt, wie Sie mithilfe der Azure Resource Manager-REST-API Probleme in der Resource Manager-Bereitstellung erkennen können"
services: azure-resource-manager,virtual-machines
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: af2abbef-5454-45d3-b5cf-4ae10347e630
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: infrastructure
ms.date: 06/13/2016
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: f6e684b08ed481cdf84faf2b8426da72f98fc58c
ms.openlocfilehash: e26a41a38fb78c0e6e075f952e05f40d4e87f1e5


---
# <a name="view-deployment-operations-with-azure-resource-manager-rest-api"></a>Anzeigen von Bereitstellungsvorgängen mit der Azure Resource Manager-REST-API
> [!div class="op_single_selector"]
> * [Portal](resource-manager-troubleshoot-deployments-portal.md)
> * [PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
> * [Azure-Befehlszeilenschnittstelle](resource-manager-troubleshoot-deployments-cli.md)
> * [REST-API](resource-manager-troubleshoot-deployments-rest.md)
> 
> 

Wenn Sie einen Fehler beim Bereitstellen von Ressourcen in Azure erhalten haben, empfiehlt es sich, weitere Details zu den ausgeführten Bereitstellungsvorgängen anzuzeigen. Die REST-API bietet Vorgänge, die es Ihnen ermöglichen, die Fehler zu finden und potenzielle Korrekturen zu ermitteln.

[!INCLUDE [resource-manager-troubleshoot-introduction](../../includes/resource-manager-troubleshoot-introduction.md)]

Einige Fehler lassen sich vermeiden, indem Sie Ihre Vorlage und die Infrastruktur vor der Bereitstellung überprüfen. Sie können auch zusätzliche Anforderungs- und Antwortinformationen während der Bereitstellung protokollieren, die später für die Problembehandlung hilfreich sein können. Unter [Bereitstellen eine Ressourcengruppe mit Azure Resource Manager-Vorlage](resource-group-template-deploy-rest.md)erfahren Sie mehr über die Überprüfung und Protokollierung von Anforderungs- und Antwortinformationen.

## <a name="troubleshoot-with-rest-api"></a>Problembehandlung mit der REST-API
1. Stellen Sie Ihre Ressourcen mit dem Vorgang zum Erstellen einer Vorlagenbereitstellung mithilfe des Befehls [Create or Update](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_CreateOrUpdate) bereit. Um Informationen beizubehalten, die zum Debuggen hilfreich sein könnten, legen Sie die Eigenschaft **debugSetting** in der JSON-Anforderung auf **requestContent** und/oder **responseContent** fest. 
   
        PUT https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}
          <common headers>
          {
            "properties": {
              "templateLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
                "contentVersion": "1.0.0.0",
              },
              "mode": "Incremental",
              "parametersLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
                "contentVersion": "1.0.0.0",      
              },
              "debugSetting": {
                "detailLevel": "requestContent, responseContent"
              }
            }
          }
   
    Der Wert **debugSetting** ist standardmäßig auf **none** festgelegt. Prüfen Sie beim Angeben des **debugSetting** -Werts sorgfältig den Informationstyp, den Sie während der Bereitstellung übergeben. Indem Sie Informationen über die Anforderung oder die Antwort protokollieren, machen Sie möglicherweise sensible Daten verfügbar, die durch die Bereitstellungsvorgänge abgerufen werden. 
2. Rufen Sie Informationen über eine Bereitstellung mit dem Vorgang zum Abrufen von Informationen zu einer Vorlage mithilfe des Befehls [Get or Update](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Get) ab.
   
        GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}
   
    Beachten Sie bei der Antwort insbesondere die Elemente **provisioningState**, **correlationId** und **error**. **correlationId** wird verwendet, um verknüpfte Ereignisse nachzuverfolgen, und kann hilfreich sein, wenn mit dem technischen Support an einer Problembehandlung gearbeitet wird.
   
        { 
          ...
          "properties": {
            "provisioningState":"Failed",
            "correlationId":"d5062e45-6e9f-4fd3-a0a0-6b2c56b15757",
            ...
            "error":{
              "code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see http://aka.ms/arm-debug for usage details.",
              "details":[{"code":"Conflict","message":"{\r\n  \"error\": {\r\n    \"message\": \"Conflict\",\r\n    \"code\": \"Conflict\"\r\n  }\r\n}"}]
            }  
          }
        }
3. Rufen Sie Informationen zu Bereitstellungsvorgängen mit dem Vorgang zum Auflisten aller Vorlagenbereistellungsvorgänge mithilfe des Befehls [List or Update](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_List) ab. 
   
        GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}/operations?$skiptoken={skiptoken}&api-version={api-version}
   
    Die Antwort wird Anforderungs- und/oder Antwortinformationen enthalten, basierend darauf, was Sie während der Bereitstellung in der **debugSetting** -Eigenschaft angegeben haben.
   
        {
          ...
          "properties": 
          {
            ...
            "request":{
              "content":{
                "location":"West US",
                "properties":{
                  "accountType": "Standard_LRS"
                }
              }
            },
            "response":{
              "content":{
                "error":{
                  "message":"Conflict","code":"Conflict"
                }
              }
            }
          }
        }
4. Rufen Sie Ereignisse aus den Überwachungsprotokollen für die Bereitstellung mit dem Vorgang [Auflisten der Verwaltungsereignisse in einem Abonnement](https://msdn.microsoft.com/library/azure/dn931934.aspx) ab.
   
        GET https://management.azure.com/subscriptions/{subscription-id}/providers/microsoft.insights/eventtypes/management/values?api-version={api-version}&$filter={filter-expression}&$select={comma-separated-property-names}

## <a name="next-steps"></a>Nächste Schritte
* Unterstützung beim Beheben bestimmter Bereitstellungsfehler finden Sie unter [Beheben von häufigen Fehlern beim Bereitstellen von Ressourcen in Azure mit Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Informationen zur Überwachung anderer Arten von Aktionen anhand der Überwachungsprotokolle finden Sie unter [Überwachen von Vorgängen mit Resource Manager](resource-group-audit.md).
* Informationen zum Überprüfen der Bereitstellung vor der Ausführung finden Sie unter [Bereitstellen einer Ressourcengruppe mit Azure Resource Manager-Vorlagen](resource-group-template-deploy.md).




<!--HONumber=Nov16_HO3-->


