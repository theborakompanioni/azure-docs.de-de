---
title: "Problembehandlung bei häufigen Azure-Bereitstellungsfehlern | Microsoft Docs"
description: "Informationen zum Beheben gängiger Fehler beim Bereitstellen von Ressourcen in Azure mit Azure Resource Manager."
services: azure-resource-manager
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
keywords: Bereitstellungsfehler, Azure-Bereitstellung, in Azure bereitstellen.
ms.assetid: c002a9be-4de5-4963-bd14-b54aa3d8fa59
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2017
ms.author: tomfitz
ms.translationtype: HT
ms.sourcegitcommit: 847eb792064bd0ee7d50163f35cd2e0368324203
ms.openlocfilehash: 30adc10d01290f14a3e116813b19916fa36ab0bc
ms.contentlocale: de-de
ms.lasthandoff: 08/19/2017

---
# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a>Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager
In diesem Thema wird beschrieben, wie Sie einige häufige Fehler beheben, die bei der Bereitstellung in Azure auftreten können.

In diesem Thema werden die folgenden Fehlercodes beschrieben:

* [AccountNameInvalid](#accountnameinvalid)
* [Fehler bei der Autorisierung](#authorization-failed)
* [BadRequest](#badrequest)
* [DeploymentFailed](#deploymentfailed)
* [DisallowedOperation](#disallowedoperation)
* [InvalidContentLink](#invalidcontentlink)
* [InvalidTemplate](#invalidtemplate)
* [MissingSubscriptionRegistration](#noregisteredproviderfound)
* [NotFound](#notfound)
* [NoRegisteredProviderFound](#noregisteredproviderfound)
* [OperationNotAllowed](#quotaexceeded)
* [ParentResourceNotFound](#parentresourcenotfound)
* [QuotaExceeded](#quotaexceeded)
* [RequestDisallowedByPolicy](#requestdisallowedbypolicy)
* [ResourceNotFound](#notfound)
* [SkuNotAvailable](#skunotavailable)
* [StorageAccountAlreadyExists](#storagenamenotunique)
* [StorageAccountAlreadyTaken](#storagenamenotunique)

## <a name="deploymentfailed"></a>DeploymentFailed

Mit diesem Fehlercode wird ein allgemeiner Bereitstellungsfehler angegeben, aber es handelt sich nicht um den Fehlercode, den Sie zu Beginn der Problembehandlung benötigen. Der Fehlercode, mit dessen Hilfe Sie das Problem tatsächlich beheben können, befindet sich meist eine Stufe unter diesem Fehler. Die folgende Abbildung zeigt beispielsweise den Fehlercode **RequestDisallowedByPolicy**, der sich unter dem Bereitstellungsfehler befindet.

![Fehlercode anzeigen](./media/resource-manager-common-deployment-errors/error-code.png)

## <a name="skunotavailable"></a>SkuNotAvailable

Beim Bereitstellen einer Ressource (in der Regel ein virtueller Computer) werden möglicherweise der folgende Fehlercode und die folgende Fehlermeldung angezeigt:

```
Code: SkuNotAvailable
Message: The requested tier for resource '<resource>' is currently not available in location '<location>' 
for subscription '<subscriptionID>'. Please try another tier or deploy to a different location.
```

Sie erhalten diesen Fehler, wenn die ausgewählte Ressourcen-SKU (z.B. die Größe des virtuellen Computers) für den ausgewählten Standort nicht verfügbar ist. Zum Beheben dieses Problems müssen Sie ermitteln, welche SKUs in einer Region verfügbar sind. Sie können PowerShell, das Portal oder einen REST-Vorgang verwenden, um nach verfügbaren SKUs zu suchen.

- Verwenden Sie für PowerShell [Get-AzureRmComputeResourceSku](/powershell/module/azurerm.compute/get-azurermcomputeresourcesku) , und filtern Sie nach Standort. Für diesen Befehl benötigen Sie die aktuelle Version von PowerShell.

  ```powershell
  Get-AzureRmComputeResourceSku | where {$_.Locations.Contains("southcentralus")}
  ```

  Die Ergebnisse enthalten eine Liste von SKUs für den Standort und alle Einschränkungen für diese SKU.

  ```powershell
  ResourceType                Name      Locations Restriction                      Capability Value
  ------------                ----      --------- -----------                      ---------- -----
  availabilitySets         Classic southcentralus             MaximumPlatformFaultDomainCount     3
  availabilitySets         Aligned southcentralus             MaximumPlatformFaultDomainCount     3
  virtualMachines      Standard_A0 southcentralus
  virtualMachines      Standard_A1 southcentralus
  virtualMachines      Standard_A2 southcentralus
  ```

- Melden Sie sich zum Verwenden des [Portals](https://portal.azure.com) am Portal an, und fügen Sie über die Oberfläche eine Ressource hinzu. Beim Festlegen der Werte werden die verfügbaren SKUs für die Ressource angezeigt. Sie müssen die Bereitstellung nicht abschließen.

    ![Verfügbare SKUs](./media/resource-manager-common-deployment-errors/view-sku.png)

- Senden Sie die folgende Anforderung, um die REST-API für virtuelle Computer zu verwenden:

  ```HTTP 
  GET
  https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/skus?api-version=2016-03-30
  ```

  Verfügbare SKUs und Regionen werden im folgenden Format zurückgegeben:

  ```json
  {
    "value": [
      {
        "resourceType": "virtualMachines",
        "name": "Standard_A0",
        "tier": "Standard",
        "size": "A0",
        "locations": [
          "eastus"
        ],
        "restrictions": []
      },
      {
        "resourceType": "virtualMachines",
        "name": "Standard_A1",
        "tier": "Standard",
        "size": "A1",
        "locations": [
          "eastus"
        ],
        "restrictions": []
      },
      ...
    ]
  }    
  ```

Wenn Sie keine geeignete SKU in dieser Region oder einer anderen Region finden, die Ihre Geschäftsanforderungen erfüllt, übermitteln Sie eine [SKU-Anforderung](https://aka.ms/skurestriction) an den Azure-Support.

## <a name="disallowedoperation"></a>DisallowedOperation

```
Code: DisallowedOperation
Message: The current subscription type is not permitted to perform operations on any provider 
namespace. Please use a different subscription.
```

Wenn Sie diese Fehlermeldung erhalten, verwenden Sie ein Abonnement, das nicht zum Zugriff auf andere Azure-Dienste als Azure Active Directory berechtigt ist. Möglicherweise besitzen Sie diesen Abonnementtyp, wenn Sie im klassischen Portal zugreifen müssen, aber nicht zum Bereitstellen von Ressourcen berechtigt sind. Um dieses Problem zu beheben, müssen Sie ein Abonnement verwenden, das über die Berechtigung zum Bereitstellen von Ressourcen verfügt.  

Zum Anzeigen Ihrer verfügbaren Abonnements mit PowerShell verwenden Sie:

```powershell
Get-AzureRmSubscription
```

Verwenden Sie außerdem zum Festlegen des aktuellen Abonnements:

```powershell
Set-AzureRmContext -SubscriptionName {subscription-name}
```

Zum Anzeigen Ihrer verfügbaren Abonnements mit Azure CLI 2.0 verwenden Sie:

```azurecli
az account list
```

Verwenden Sie außerdem zum Festlegen des aktuellen Abonnements:

```azurecli
az account set --subscription {subscription-name}
```

## <a name="invalidtemplate"></a>InvalidTemplate
Dieser Fehler kann aus verschiedenen Arten von Fehlern entstehen.

- Syntaxfehler

   Wenn Sie eine Fehlermeldung erhalten, die auf eine fehlerhafte Vorlagenüberprüfung hinweist, ist in Ihrer Vorlage möglicherweise ein Syntaxproblem vorhanden.

  ```
  Code=InvalidTemplate
  Message=Deployment template validation failed
  ```

   Solche Fehler können sehr leicht passieren, da Vorlagenausdrücke recht kompliziert sein können. Beispielsweise enthält die folgenden Namenszuweisung für ein Speicherkonto Klammern, drei Funktionen, drei Sätze mit Klammern, einen Satz mit einfachen Anführungszeichen und eine Eigenschaft:

  ```json
  "name": "[concat('storage', uniqueString(resourceGroup().id))]",
  ```

   Wenn Sie nicht die passende Syntax bereitstellen, erzeugt die Vorlage einen Wert, der nicht Ihrer Absicht entspricht.

   Wenn Sie diese Art von Fehler erhalten, überprüfen Sie sorgfältig die Ausdruckssyntax. Verwenden Sie ggf. einen JSON-Editor, der Sie auf Syntaxfehler aufmerksam machen kann (z.B. [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) oder [Visual Studio Code](resource-manager-vs-code.md)).

- Falsche Segmentlängen

   Ein weiterer Fehler vom Typ „Ungültige Vorlage“ tritt auf, wenn der Ressourcenname nicht im richtigen Format vorliegt.

  ```
  Code=InvalidTemplate
  Message=Deployment template validation failed: 'The template resource {resource-name}'
  for type {resource-type} has incorrect segment lengths.
  ```

   Eine Ressource auf Stammebene muss im Namen ein Segment weniger aufweisen als im Ressourcentyp. Die einzelnen Segmente sind jeweils durch einen Schrägstrich getrennt. Im folgenden Beispiel weist der Typ zwei Segmente auf und der Name eins, sodass es sich hier um einen **gültigen Namen** handelt.

  ```json
  {
    "type": "Microsoft.Web/serverfarms",
    "name": "myHostingPlanName",
    ...
  }
  ```

   Im nächsten Beispiel liegt dagegen **kein gültiger Name** vor, da er die gleiche Anzahl von Segmenten besitzt wie der Typ.

  ```json
  {
    "type": "Microsoft.Web/serverfarms",
    "name": "appPlan/myHostingPlanName",
    ...
  }
  ```

   Bei untergeordneten Ressourcen besitzen Typ und Name die gleiche Anzahl von Segmenten. Diese Segmentanzahl ist sinnvoll, da der vollständige Name und Typ der untergeordneten Ressource den Namen und den Typ der übergeordneten Ressource enthalten. Aus diesem Grund weist der vollständige Name weiterhin ein Segment weniger auf als der vollständige Typ.

  ```json
  "resources": [
      {
          "type": "Microsoft.KeyVault/vaults",
          "name": "contosokeyvault",
          ...
          "resources": [
              {
                  "type": "secrets",
                  "name": "appPassword",
                  ...
              }
          ]
      }
  ]
  ```

   Die richtige Angabe der Segmente kann bei Resource Manager-Typen problematisch sein, die ressourcenanbieterübergreifend angewendet werden. Wenn Sie beispielsweise eine Website mit einer Ressourcensperre belegen möchten, benötigen Sie einen Typ mit vier Segmenten. Folglich umfasst der Name drei Segmente:

  ```json
  {
      "type": "Microsoft.Web/sites/providers/locks",
      "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
      ...
  }
  ```

- copy-Index wird nicht erwartet

   Dieser **InvalidTemplate**-Fehler tritt auf, wenn Sie das **copy**-Element auf einen Teil der Vorlage angewendet haben, der dieses Element nicht unterstützt. Sie können das copy-Element nur auf einen Ressourcentyp anwenden. Sie können „copy“ nicht auf eine Eigenschaft in einem Ressourcentyp anwenden. Beispiel: Sie wenden „copy“ auf einen virtuellen Computer an, können das Element aber nicht auf die Betriebssystem-Datenträger für einen virtuellen Computer anwenden. In einigen Fällen können Sie eine untergeordnete Ressource in eine übergeordnete Ressource umwandeln, um eine Kopierschleife zu erstellen. Weitere Informationen zur Verwendung von „copy“ finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen in Azure Resource Manager](resource-group-create-multiple.md).

- Parameter ist ungültig

   Wenn die Vorlage zulässige Werte für einen Parameter angibt und Sie einen Wert bereitstellen, der keinem dieser Werte entspricht, erhalten Sie eine Fehlermeldung ähnlich der folgenden:

  ```
  Code=InvalidTemplate;
  Message=Deployment template validation failed: 'The provided value {parameter value}
  for the template parameter {parameter name} is not valid. The parameter value is not
  part of the allowed values
  ``` 

   Überprüfen Sie die zulässigen Werte in der Vorlage, und geben Sie während der Bereitstellung einen gültigen Wert an.

- Ringabhängigkeit erkannt

   Sie erhalten diesen Fehler, wenn Ressourcen auf eine Weise voneinander abhängig sind, die das Starten der Bereitstellung verhindert. Eine Kombination aus Abhängigkeiten bewirkt, dass zwei oder mehr Ressourcen auf andere Ressourcen warten, die sich ebenfalls im Wartezustand befinden. Beispielsweise kann „resource1“ von „resource3“, „resource2“ von „resource1“ und „resource3“ von „resource2“ abhängig sein. Dieses Problem lässt sich normalerweise beheben, indem nicht benötigte Abhängigkeiten entfernt werden. 

<a id="notfound" />
### <a name="notfound-and-resourcenotfound"></a>NotFound und ResourceNotFound
Wenn Ihre Vorlage einen Ressourcennamen enthält, der nicht aufgelöst werden kann, erhalten Sie eine Fehlermeldung ähnlich der folgenden:

```
Code=NotFound;
Message=Cannot find ServerFarm with name exampleplan.
```

Wenn Sie versuchen, die fehlende Ressource in der Vorlage bereitzustellen, prüfen Sie, ob Sie eine Abhängigkeit hinzufügen müssen. Resource Manager optimiert die Bereitstellung, indem, sofern möglich, gleichzeitig Ressourcen erstellt werden. Wenn eine Ressource nach einer anderen Ressource bereitgestellt werden muss, müssen Sie das **dependsOn**-Element in der Vorlage verwenden, um eine Abhängigkeit zu der anderen Ressource herzustellen. Beim Bereitstellen einer Web-App muss z. B. der App Service-Plan vorhanden sein. Wenn Sie nicht angegeben haben, dass die Web-App vom App Service-Plan abhängig ist, erstellt Resource Manager beide Ressourcen zur gleichen Zeit. Beim Versuch, eine Eigenschaft für die Web-App festzulegen, erhalten Sie eine Fehlermeldung, dass die App Service-Planressource nicht gefunden werden kann, da sie noch nicht vorhanden ist. Sie verhindern diesen Fehler, indem Sie die Abhängigkeit in der Web-App festlegen.

```json
{
  "apiVersion": "2015-08-01",
  "type": "Microsoft.Web/sites",
  "dependsOn": [
    "[variables('hostingPlanName')]"
  ],
  ...
}
```

Vorschläge zur Problembehandlung für Abhängigkeitsfehler finden Sie unter [Überprüfen der Bereitstellungssequenz](#check-deployment-sequence).

Dieser Fehler wird auch angezeigt, wenn die Ressource in einer anderen Ressourcengruppe als der vorhanden ist, in der die Ressource bereitgestellt wird. Verwenden Sie in diesem Fall die [resourceId-Funktion](resource-group-template-functions-resource.md#resourceid), um den vollqualifizierten Namen der Ressource abzurufen.

```json
"properties": {
    "name": "[parameters('siteName')]",
    "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
}
```

Wenn Sie versuchen, die Funktionen [reference](resource-group-template-functions-resource.md#reference) oder [listKeys](resource-group-template-functions-resource.md#listkeys) mit einer Ressource zu verwenden, die nicht aufgelöst werden kann, erhalten Sie folgenden Fehler:

```
Code=ResourceNotFound;
Message=The Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource
group {resource group name} was not found.
```

Suchen Sie nach einem Ausdruck, der die **reference**-Funktion enthält. Überprüfen Sie, ob die Parameterwerte richtig sind.

## <a name="parentresourcenotfound"></a>ParentResourceNotFound

Wenn eine Ressource einer anderen übergeordnet ist, muss die übergeordnete Ressource vor dem Erstellen der untergeordneten Ressource bereits vorhanden sein. Wenn diese noch nicht vorhanden ist, erhalten Sie den folgenden Fehler:

```
Code=ParentResourceNotFound;
Message=Can not perform requested operation on nested resource. Parent resource 'exampleserver' not found."
```

Der Name der untergeordneten Ressource enthält den Namen der übergeordneten Ressource. Zum Beispiel könnte eine SQL-Datenbank wie folgt definiert werden:

```json
{
  "type": "Microsoft.Sql/servers/databases",
  "name": "[concat(variables('databaseServerName'), '/', parameters('databaseName'))]",
  ...
```

Wenn Sie allerdings keine Abhängigkeit von der übergeordneten Ressource angeben, wird die untergeordnete Ressource möglicherweise vor der übergeordneten bereitgestellt. Um diesen Fehler aufzulösen, geben Sie eine Abhängigkeit an.

```json
"dependsOn": [
    "[variables('databaseServerName')]"
]
```

<a id="storagenamenotunique" />

## <a name="storageaccountalreadyexists-and-storageaccountalreadytaken"></a>StorageAccountAlreadyExists und StorageAccountAlreadyTaken
Bei Speicherkonten müssen Sie einen Namen für die Ressource angeben, der in Azure eindeutig ist. Wenn Sie keinen eindeutigen Namen angeben, erhalten Sie einen Fehler wie diesen:

```
Code=StorageAccountAlreadyTaken
Message=The storage account named mystorage is already taken.
```

Sie können einen eindeutigen Namen erstellen, indem Sie Ihre Benennungskonvention mit dem Ergebnis der [uniqueString](resource-group-template-functions-string.md#uniquestring) -Funktion verketten.

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]",
"type": "Microsoft.Storage/storageAccounts",
```

Wenn Sie ein Speicherkonto bereitstellen, das den gleichen Namen hat wie ein in Ihrem Abonnement vorhandenes Speicherkonto, aber einen anderen Speicherort angeben, erhalten Sie eine Fehlermeldung, dass das Speicherkonto bereits an einem anderen Speicherort existiert. Löschen Sie das vorhandene Speicherkonto, oder geben Sie den gleichen Speicherort wie für das vorhandene Speicherkonto an.

## <a name="accountnameinvalid"></a>AccountNameInvalid
Beim Versuch, einem Speicherkonto einen Namen zuzuweisen, der nicht zulässige Zeichen enthält, wird Ihnen der Fehler **AccountNameInvalid** angezeigt. Speicherkontonamen müssen zwischen 3 und 24 Zeichen lang sein und dürfen nur Zahlen und Kleinbuchstaben enthalten. Die Funktion [uniqueString](resource-group-template-functions-string.md#uniquestring) gibt 13 Zeichen zurück. Wenn Sie ein Präfix mit dem Ergebnis von **uniqueString** verketten, sollte das Präfix maximal elf Zeichen lang sein.

## <a name="badrequest"></a>BadRequest

Sie erhalten möglicherweise den Status „BadRequest“, wenn Sie einen ungültigen Wert für die Eigenschaft angeben. Wenn Sie zum Beispiel einen falschen SKU-Wert für ein Speicherkonto angeben, tritt bei der Bereitstellung ein Fehler auf. Sie können die gültigen Werte für eine Eigenschaft ermitteln, indem Sie sich die [REST-API](/rest/api) für den von Ihnen bereitgestellten Ressourcentyp ansehen.

<a id="noregisteredproviderfound" />

## <a name="noregisteredproviderfound-and-missingsubscriptionregistration"></a>„NoRegisteredProviderFound“ und „MissingSubscriptionRegistration“
Beim Bereitstellen von Ressourcen werden möglicherweise der folgende Fehlercode und die folgende Fehlermeldung angezeigt:

```
Code: NoRegisteredProviderFound
Message: No registered resource provider found for location {location}
and API version {api-version} for type {resource-type}.
```

Oder Sie erhalten eine ähnliche Meldung, die Folgendes besagt:

```
Code: MissingSubscriptionRegistration
Message: The subscription is not registered to use namespace {resource-provider-namespace}
```

Für diese Fehler gibt es drei Gründe:

1. Der Ressourcenanbieter wurde für Ihr Abonnement nicht registriert.
2. Die API-Version wird für den Ressourcentyp nicht unterstützt.
3. Der Standort wird für den Ressourcentyp nicht unterstützt.

Die Fehlermeldung enthält in der Regel Vorschläge für die unterstützten Standorte und API-Versionen. Sie können Ihre Vorlage in einen der vorgeschlagenen Werte ändern. Die meisten Anbieter werden automatisch über das Azure-Portal oder die verwendete Befehlszeilenschnittstelle registriert. Wenn Sie zuvor noch keinen bestimmten Ressourcenanbieter verwendet haben, müssen Sie diesen Anbieter unter Umständen registrieren. Weitere Informationen zu Ressourcenanbietern erhalten Sie in PowerShell oder der Azure-Befehlszeilenschnittstelle.

**Portal**

Sie können den Registrierungsstatus sehen und einen Ressourcenanbieter-Namespace über das Portal registrieren.

1. Wählen Sie für Ihr Abonnement die Option **Ressourcenanbieter** aus.

   ![Ressourcenanbieter auswählen](./media/resource-manager-common-deployment-errors/select-resource-provider.png)

2. Betrachten Sie die Liste der Ressourcenanbieter, und wählen Sie bei Bedarf den Link **Registrieren** aus, um den Ressourcenanbietertyp zu registrieren, den Sie bereitstellen möchten.

   ![Liste der Ressourcenanbieter](./media/resource-manager-common-deployment-errors/list-resource-providers.png)

**PowerShell**

Verwenden Sie zum Anzeigen des Registrierungsstatus **Get-AzureRmResourceProvider**.

```powershell
Get-AzureRmResourceProvider -ListAvailable
```

Verwenden Sie **Register-AzureRmResourceProvider** , um einen Anbieter zu registrieren, und geben Sie den Namen des zu registrierenden Ressourcenanbieters an.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Cdn
```

Verwenden Sie zum Abrufen der unterstützten Standorte für einen bestimmten Ressourcentyp Folgendes:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

Verwenden Sie zum Abrufen der unterstützten API-Versionen für einen bestimmten Ressourcentyp Folgendes:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
```

**Azure-CLI**

Mit dem Befehl `azure provider list` können Sie ermitteln, ob der Anbieter registriert ist.

```azurecli
az provider list
```

Verwenden Sie den Befehl `azure provider register` , um einen Ressourcenanbieter zu registrieren, und geben Sie den zu registrierenden *Namespace* an.

```azurecli
az provider register --namespace Microsoft.Cdn
```

Verwenden Sie zum Anzeigen der unterstützten Standorte und API-Versionen für einen Ressourcentyp Folgendes:

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

<a id="quotaexceeded" />

## <a name="quotaexceeded-and-operationnotallowed"></a>QuotaExceeded und OperationNotAllowed
Probleme können auftreten, wenn eine Bereitstellung ein Kontingent überschreitet (etwa für eine Ressourcengruppe, ein Abonnement, ein Konto oder Ähnliches). Ihr Abonnement kann beispielsweise so konfiguriert werden, um die Anzahl der Kerne für eine Region zu begrenzen. Wenn Sie versuchen, einen virtuellen Computer mit mehr als der zulässigen Anzahl von Kernen bereitzustellen, erhalten Sie eine Fehlermeldung, die darauf hinweist, dass das Kontingent überschritten wurde.
Die vollständigen Kontingentinformationen finden Sie unter [Grenzwerte, Kontingente und Einschränkungen für Azure-Abonnements und -Dienste](../azure-subscription-service-limits.md).

Mit dem Befehl `azure vm list-usage` können Sie über die Azure-Befehlszeilenschnittstelle die Kernkontingente Ihres eigenen Abonnements untersuchen. Im folgenden Beispiel wird veranschaulicht, dass das Kernkontingent für ein kostenloses Testkonto 4 ist:

```azurecli
az vm list-usage --location "South Central US"
```

Ausgabe des Befehls:

```azurecli
[
  {
    "currentValue": 0,
    "limit": 2000,
    "name": {
      "localizedValue": "Availability Sets",
      "value": "availabilitySets"
    }
  },
  ...
]
```

Wenn Sie in der Region „USA, Westen“ eine Vorlage bereitstellen, die mehr als vier Kerne erstellt, erhalten Sie einen Bereitstellungsfehler ähnlich dem folgenden:

```
Code=OperationNotAllowed
Message=Operation results in exceeding quota limits of Core.
Maximum allowed: 4, Current in use: 4, Additional requested: 2.
```

In PowerShell können Sie alternativ das Cmdlet **Get-AzureRmVMUsage** verwenden.

```powershell
Get-AzureRmVMUsage
```

Ausgabe des Befehls:

```powershell
...
CurrentValue : 0
Limit        : 4
Name         : {
                 "value": "cores",
                 "localizedValue": "Total Regional Cores"
               }
Unit         : null
...
```

In diesen Fällen sollten Sie zum Portal navigieren und ein Supportproblem einreichen, um Ihr Kontingent für die Region, in der Sie diese bereitstellen möchten, zu erhöhen.

> [!NOTE]
> Denken Sie daran, dass für Ressourcengruppen das Kontingent für jede einzelne Region und nicht für das gesamte Abonnement gilt. Wenn Sie 30 Kerne in der Region "USA, Westen" bereitstellen möchten, müssen Sie 30 Ressourcen-Manager-Kerne für "USA, Westen" anfordern. Wenn Sie 30 Kerne in einer der Regionen bereitstellen möchten, auf die Sie Zugriff haben, sollten Sie 30 Resource Manager-Kerne in allen Regionen anfordern.
>
>

## <a name="invalidcontentlink"></a>InvalidContentLink
Wenn Sie die folgende Fehlermeldung erhalten, gilt:

```
Code=InvalidContentLink
Message=Unable to download deployment content from ...
```

Sie haben wahrscheinlich versucht, eine geschachtelte Vorlage zu verknüpfen, die nicht verfügbar ist. Überprüfen Sie den URI, den Sie für die geschachtelte Vorlage angegeben haben. Wenn die Vorlage in einem Speicherkonto vorhanden ist, stellen Sie sicher, dass auf den URI zugegriffen werden kann. Möglicherweise müssen Sie ein SAS-Token übergeben. Weitere Informationen finden Sie unter [Verwenden von verknüpften Vorlagen mit Azure-Ressourcen-Manager](resource-group-linked-templates.md).

## <a name="requestdisallowedbypolicy"></a>RequestDisallowedByPolicy
Sie erhalten diesen Fehler, wenn Ihr Abonnement eine Ressourcenrichtlinie enthält, die eine Aktion verhindert, die Sie während der Bereitstellung ausführen möchten. Suchen Sie in der Fehlermeldung die Richtlinienkennung.

```
Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'
```

Geben Sie in **PowerShell** diese Richtlinienkennung als **Id**-Parameter an, um Details zur Richtlinie abzurufen, die Ihre Bereitstellung blockiert.

```powershell
(Get-AzureRmPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

Geben Sie an der **Azure-Befehlszeilenschnittstelle** den Namen der Richtliniendefinition an:

```azurecli
az policy definition show --name regionPolicyAssignment
```

Weitere Informationen finden Sie in den folgenden Artikeln:

- [RequestDisallowedByPolicy-Fehler](resource-manager-policy-requestdisallowedbypolicy-error.md)
- [Verwenden von Richtlinien für Ressourcenverwaltung und Zugriffssteuerung](resource-manager-policy.md)

## <a name="authorization-failed"></a>Fehler bei der Autorisierung
Möglicherweise wird während der Bereitstellung ein Fehler angezeigt, da das Konto oder ein Dienstprinzipal, der versucht die Ressourcen bereitzustellen, keinen Zugriff zum Ausführen dieser Aktionen hat. Mit Azure Active Directory können Sie oder Ihr Administrator sehr genau kontrollieren, welche Identitäten auf welche Ressourcen Zugriff haben. Wenn Ihrem Konto die Leserrolle zugewiesen ist, können Sie keine neuen Ressourcen erstellen. In diesem Fall wird eine Fehlermeldung angezeigt, die darauf hinweist, dass die Autorisierung fehlgeschlagen ist.

Weitere Informationen zur rollenbasierten Zugriffssteuerung finden Sie unter [Verwenden von Rollenzuweisungen zum Verwalten Ihrer Azure Active Directory-Ressourcen](../active-directory/role-based-access-control-configure.md).


## <a name="next-steps"></a>Nächste Schritte
* Informationen zur Überwachung von Aktionen finden Sie unter [Überwachen von Vorgängen mit Resource Manager](resource-group-audit.md).
* Weitere Informationen zu Aktionen zum Bestimmen von Fehlern während der Bereitstellung finden Sie unter [Anzeigen von Bereitstellungsvorgängen mit dem Azure-Portal](resource-manager-deployment-operations.md).

