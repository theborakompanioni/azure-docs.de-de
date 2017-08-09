---
title: Azure-Ressourcenanbieter und -Ressourcentypen | Microsoft-Dokumentation
description: "Beschreibt die Ressourcenanbieter, die den Ressourcen-Manager, die zugehörigen Schemas und verfügbaren API-Versionen sowie die Regionen unterstützen, die die Ressourcen hosten können."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 3c7a6fe4-371a-40da-9ebe-b574f583305b
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: tomfitz
ms.translationtype: HT
ms.sourcegitcommit: a678700884b612cad6281eb8f3b74ce63a0ebb69
ms.openlocfilehash: 6a9128f45d4199404019cee594842d59c7f1aaf3
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

---

# <a name="resource-providers-and-types"></a>Ressourcenanbieter und -typen

Beim Bereitstellen von Ressourcen müssen Sie häufig Informationen zu den Ressourcenanbietern und -typen abrufen. In diesem Artikel wird Folgendes behandelt:

* Anzeigen aller Ressourcenanbieter in Azure
* Überprüfen des Registrierungsstatus eines Ressourcenanbieters
* Registrieren eines Ressourcenanbieters
* Anzeigen von Ressourcentypen für einen Ressourcenanbieter
* Anzeigen gültiger Standorte für einen Ressourcentyp
* Anzeigen gültiger API-Versionen für einen Ressourcentyp

Diese Schritte können über das Portal, mithilfe von Azure PowerShell oder unter Verwendung der Azure-Befehlszeilenschnittstelle ausgeführt werden.

## <a name="powershell"></a>PowerShell

Verwenden Sie Folgendes, um alle Ressourcenanbieter in Azure und den Registrierungsstatus für Ihr Abonnement anzuzeigen:

```powershell
Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
```

Die Rückgabe sieht in etwa wie folgt aus:

```powershell
ProviderNamespace                RegistrationState
-------------------------------- ------------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

Durch Registrieren eines Ressourcenanbieters wird Ihr Abonnement für die Verwendung mit dem Ressourcenanbieter konfiguriert. Der Gültigkeitsbereich der Registrierung ist immer das Abonnement. Viele Ressourcenanbieter werden standardmäßig automatisch registriert. Einige Ressourcenanbieter müssen Sie jedoch unter Umständen manuell registrieren. Um einen Ressourcenanbieter zu registrieren, benötigen Sie die Berechtigungen zum Ausführen des `/register/action`-Vorgangs für den Ressourcenanbieter. Dieser Vorgang ist in den Rollen „Mitwirkender“ und „Besitzer“ enthalten.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
```

Die Rückgabe sieht in etwa wie folgt aus:

```powershell
ProviderNamespace : Microsoft.Batch
RegistrationState : Registering
ResourceTypes     : {batchAccounts, operations, locations, locations/quotas}
Locations         : {West Europe, East US, East US 2, West US...}
```

Sie können die Registrierung eines Ressourcenanbieters nicht aufheben, wenn in Ihrem Abonnement noch Ressourcentypen aus diesem Ressourcenanbieter vorhanden sind.

Verwenden Sie Folgendes, um Informationen für einen bestimmten Ressourcenanbieter anzuzeigen:

```powershell
Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
```

Die Rückgabe sieht in etwa wie folgt aus:

```powershell
{ProviderNamespace : Microsoft.Batch
RegistrationState : Registered
ResourceTypes     : {batchAccounts}
Locations         : {West Europe, East US, East US 2, West US...}

...
```

Verwenden Sie Folgendes, um die Ressourcentypen für einen Ressourcenanbieter anzuzeigen:

```powershell
(Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes.ResourceTypeName
```

Ausgabe des Befehls:

```powershell
batchAccounts
operations
locations
locations/quotas
```

Die API-Version entspricht einer Version von REST-API-Vorgängen, die vom Ressourcenanbieter freigegeben werden. Wenn ein Ressourcenanbieter neue Features aktiviert, wird eine neue Version der REST-API veröffentlicht. 

Verwenden Sie Folgendes, um die verfügbaren API-Versionen für einen Ressourcentyp abzurufen:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).ApiVersions
```

Ausgabe des Befehls:

```powershell
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

Der Ressourcen-Manager wird in allen Regionen unterstützt, aber die Ressourcen, die Sie bereitstellen, werden möglicherweise nicht in allen Regionen unterstützt. Darüber hinaus liegen möglicherweise Einschränkungen Ihres Abonnements vor, die verhindern, dass Sie einige Regionen verwenden, die die Ressource unterstützen. 

Verwenden Sie Folgendes, um die unterstützten Standorte für einen Ressourcentyp abzurufen:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).Locations
```

Ausgabe des Befehls:

```powershell
West Europe
East US
East US 2
West US
...
```

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle
Verwenden Sie Folgendes, um alle Ressourcenanbieter in Azure und den Registrierungsstatus für Ihr Abonnement anzuzeigen:

```azurecli
az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
```

Die Rückgabe sieht in etwa wie folgt aus:

```azurecli
Provider                         Status
-------------------------------- ----------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

Durch Registrieren eines Ressourcenanbieters wird Ihr Abonnement für die Verwendung mit dem Ressourcenanbieter konfiguriert. Der Gültigkeitsbereich der Registrierung ist immer das Abonnement. Viele Ressourcenanbieter werden standardmäßig automatisch registriert. Einige Ressourcenanbieter müssen Sie jedoch unter Umständen manuell registrieren. Um einen Ressourcenanbieter zu registrieren, benötigen Sie die Berechtigungen zum Ausführen des `/register/action`-Vorgangs für den Ressourcenanbieter. Dieser Vorgang ist in den Rollen „Mitwirkender“ und „Besitzer“ enthalten.

```azurecli
az provider register --namespace Microsoft.Batch
```

Daraufhin wird eine Meldung mit dem Hinweis zurückgegeben, dass die Registrierung noch nicht abgeschlossen ist.

Sie können die Registrierung eines Ressourcenanbieters nicht aufheben, wenn in Ihrem Abonnement noch Ressourcentypen aus diesem Ressourcenanbieter vorhanden sind.

Verwenden Sie Folgendes, um Informationen für einen bestimmten Ressourcenanbieter anzuzeigen:

```azurecli
az provider show --namespace Microsoft.Batch
```

Die Rückgabe sieht in etwa wie folgt aus:

```azurecli
{
    "id": "/subscriptions/####-####/providers/Microsoft.Batch",
    "namespace": "Microsoft.Batch",
    "registrationsState": "Registering",
    "resourceTypes:" [
        ...
    ]
}
```

Verwenden Sie Folgendes, um die Ressourcentypen für einen Ressourcenanbieter anzuzeigen:

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[*].resourceType" --out table
```

Ausgabe des Befehls:

```azurecli
Result
---------------
batchAccounts
operations
locations
locations/quotas
```

Die API-Version entspricht einer Version von REST-API-Vorgängen, die vom Ressourcenanbieter freigegeben werden. Wenn ein Ressourcenanbieter neue Features aktiviert, wird eine neue Version der REST-API veröffentlicht. 

Verwenden Sie Folgendes, um die verfügbaren API-Versionen für einen Ressourcentyp abzurufen:

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].apiVersions | [0]" --out table
```

Ausgabe des Befehls:

```azurecli
Result
---------------
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

Der Ressourcen-Manager wird in allen Regionen unterstützt, aber die Ressourcen, die Sie bereitstellen, werden möglicherweise nicht in allen Regionen unterstützt. Darüber hinaus liegen möglicherweise Einschränkungen Ihres Abonnements vor, die verhindern, dass Sie einige Regionen verwenden, die die Ressource unterstützen. 

Verwenden Sie Folgendes, um die unterstützten Standorte für einen Ressourcentyp abzurufen:

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].locations | [0]" --out table
```

Ausgabe des Befehls:

```azurecli
Result
---------------
West Europe
East US
East US 2
West US
...
```

## <a name="portal"></a>Portal

Wählen Sie **Abonnements** aus, um alle Ressourcenanbieter in Azure und den Registrierungsstatus für Ihr Abonnement anzuzeigen.

![„Abonnements“ auswählen](./media/resource-manager-supported-services/select-subscriptions.png)

Wählen Sie das gewünschte Abonnement aus.

![Abonnement angeben](./media/resource-manager-supported-services/subscription.png)

Wählen Sie **Ressourcenanbieter** aus, und sehen Sie sich die Liste mit den verfügbaren Ressourcenanbietern an.

![Ressourcenanbieter anzeigen](./media/resource-manager-supported-services/show-resource-providers.png)

Durch Registrieren eines Ressourcenanbieters wird Ihr Abonnement für die Verwendung mit dem Ressourcenanbieter konfiguriert. Der Gültigkeitsbereich der Registrierung ist immer das Abonnement. Viele Ressourcenanbieter werden standardmäßig automatisch registriert. Einige Ressourcenanbieter müssen Sie jedoch unter Umständen manuell registrieren. Um einen Ressourcenanbieter zu registrieren, benötigen Sie die Berechtigungen zum Ausführen des `/register/action`-Vorgangs für den Ressourcenanbieter. Dieser Vorgang ist in den Rollen „Mitwirkender“ und „Besitzer“ enthalten. Wählen Sie **Registrieren** aus, um einen Ressourcenanbieter zu registrieren.

![Ressourcenanbieter registrieren](./media/resource-manager-supported-services/register-provider.png)

Sie können die Registrierung eines Ressourcenanbieters nicht aufheben, wenn in Ihrem Abonnement noch Ressourcentypen aus diesem Ressourcenanbieter vorhanden sind.

Wählen Sie **Weitere Dienste** aus, um Informationen für einen bestimmten Ressourcenanbieter anzuzeigen.

![„Weitere Dienste“ auswählen](./media/resource-manager-supported-services/more-services.png)

Suchen Sie nach **Ressourcen-Explorer**, und wählen Sie ihn in den verfügbaren Optionen aus.

![Ressourcen-Explorer auswählen](./media/resource-manager-supported-services/select-resource-explorer.png)

Wählen Sie **Anbieter** aus.

![„Anbieter“ auswählen](./media/resource-manager-supported-services/select-providers.png)

Wählen Sie den Ressourcenanbieter und den Ressourcentyp aus, die Sie anzeigen möchten.

![Ressourcentyp auswählen](./media/resource-manager-supported-services/select-resource-type.png)

Der Ressourcen-Manager wird in allen Regionen unterstützt, aber die Ressourcen, die Sie bereitstellen, werden möglicherweise nicht in allen Regionen unterstützt. Darüber hinaus liegen möglicherweise Einschränkungen Ihres Abonnements vor, die verhindern, dass Sie einige Regionen verwenden, die die Ressource unterstützen. Der Ressourcen-Explorer zeigt gültige Standorte für den Ressourcentyp an.

![Standorte anzeigen](./media/resource-manager-supported-services/show-locations.png)

Die API-Version entspricht einer Version von REST-API-Vorgängen, die vom Ressourcenanbieter freigegeben werden. Wenn ein Ressourcenanbieter neue Features aktiviert, wird eine neue Version der REST-API veröffentlicht. Der Ressourcen-Explorer zeigt gültige API-Versionen für den Ressourcentyp an.

![API-Versionen anzeigen](./media/resource-manager-supported-services/show-api-versions.png)

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Erstellen von Ressourcen-Manager-Vorlagen finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen](resource-group-authoring-templates.md).
* Informationen zum Bereitstellen von Vorlagen finden Sie unter [Bereitstellen einer Anwendung mit einer Azure-Ressourcen-Manager-Vorlage](resource-group-template-deploy.md).
* Informationen zum Anzeigen der Vorgänge für einen Ressourcenanbieter finden Sie unter [Azure-REST-API](/rest/api/).


