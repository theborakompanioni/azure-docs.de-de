---
title: "Fehler „RequestDisallowedByPolicy“ bei Azure-Ressourcenrichtlinien | Microsoft-Dokumentation"
description: "Beschreibt die Ursache des Fehlers „RequestDisallowedByPolicy“."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: genlin
manager: cshepard
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: genli
ms.translationtype: HT
ms.sourcegitcommit: 54454e98a2c37736407bdac953fdfe74e9e24d37
ms.openlocfilehash: 182a27e444c2f5db66d518a1a0c608d3e319d553
ms.contentlocale: de-de
ms.lasthandoff: 07/13/2017

---
# <a name="requestdisallowedbypolicy-error-with-azure-resource-policy"></a>Fehler „RequestDisallowedByPolicy“ bei Azure-Ressourcenrichtlinien

Dieser Artikel beschreibt die Ursache des Fehlers „RequestDisallowedByPolicy“ und bietet eine Lösung für diesen Fehler.

## <a name="symptom"></a>Symptom

Wenn Sie versuchen, während der Bereitstellung eine Aktion auszuführen, erhalten Sie möglicherweise den Fehler **RequestDisallowedByPolicy**, der verhindert, dass die Aktion ausgeführt wird. Im Folgenden finden Sie ein Beispiel des Fehlers:

```
{
  "statusCode": "Forbidden",
  "serviceRequestId": null,
  "statusMessage": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}",
  "responseBody": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}"
}
```

## <a name="troubleshooting"></a>Problembehandlung

Verwenden Sie zum Abrufen von Details zur Richtlinie, die die Bereitstellung blockiert hat, eine der folgenden Methoden:

### <a name="method-1"></a>Methode 1

Geben Sie in PowerShell diese Richtlinienkennung als **Id**-Parameter an, um Details zur Richtlinie abzurufen, die Ihre Bereitstellung blockiert hat.

```PowerShell
(Get-AzureRmPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

### <a name="method-2"></a>Methode 2 

Geben Sie in Azure CLI 2.0 den Namen der Richtliniendefinition an: 

```azurecli
az policy definition show --name regionPolicyAssignment
```

## <a name="solution"></a>Lösung

Für Sicherheit und Kompatibilität kann Ihre IT-Abteilung eine Ressourcenrichtlinie erzwingen, die das Erstellen von öffentlichen IP-Adressen, Netzwerksicherheitsgruppen, benutzerdefinierten Routen oder Routentabellen verhindert. Im Beispiel der Fehlermeldung, das im Abschnitt „Symptome“ beschrieben wird, heißt die Richtlinie **regionPolicyDefinition**. Auch andere Namen sind möglich.
Zum Beheben dieses Problems arbeiten Sie mit Ihrer IT-Abteilung zusammen, um die Ressourcenrichtlinien zu überprüfen und zu bestimmen, wie die angeforderte Aktion in Übereinstimmung mit diesen Richtlinien ausgeführt werden kann.


Weitere Informationen finden Sie in den folgenden Artikeln:

- [Übersicht über Ressourcenrichtlinien](resource-manager-policy.md)
- [Häufige Bereitstellungsfehler: RequestDisallowedByPolicy](resource-manager-common-deployment-errors.md#requestdisallowedbypolicy)

 



