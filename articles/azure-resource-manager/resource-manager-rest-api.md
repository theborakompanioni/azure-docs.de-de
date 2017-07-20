---
title: Resource Manager-REST-APIs | Microsoft Docs
description: "Enthält eine Übersicht über die Authentifizierung mit Resource Manager-REST-APIs und Beispiele zur Verwendung."
services: azure-resource-manager
documentationcenter: na
author: navalev
manager: timlt
editor: 
ms.assetid: e8d7a1d2-1e82-4212-8288-8697341408c5
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/13/2017
ms.author: navale;tomfitz;
ms.translationtype: Human Translation
ms.sourcegitcommit: a30a90682948b657fb31dd14101172282988cbf0
ms.openlocfilehash: 2f7ba23775545637de865f9ef63680ae22c62164
ms.contentlocale: de-de
ms.lasthandoff: 05/25/2017


---
# <a name="resource-manager-rest-apis"></a>Resource Manager-REST-APIs
> [!div class="op_single_selector"]
> * [Azure PowerShell](powershell-azure-resource-manager.md)
> * [Azure-Befehlszeilenschnittstelle](xplat-cli-azure-resource-manager.md)
> * [Portal](resource-group-portal.md) 
> * [REST-API](resource-manager-rest-api.md)
> 
> 

Hinter jedem Aufruf von Azure Resource Manager, hinter jeder bereitgestellten Vorlage und hinter jedem konfigurierten Speicherkonto steht mindestens ein Aufruf der RESTful-API von Azure Resource Manager. In diesem Thema geht es um diese APIs und das Aufrufen ganz ohne SDK. Dieser Ansatz kann sehr nützlich sein, wenn Sie die vollständige Kontrolle über alle Anforderungen an Azure haben möchten oder wenn das SDK für Ihre bevorzugte Sprache nicht verfügbar ist oder die benötigten Vorgänge nicht unterstützt werden.

In diesem Artikel gehen wir nicht auf alle APIs ein, die in Azure verfügbar gemacht werden, sondern es werden einige Vorgänge als Beispiele dafür verwendet, wie Sie eine Verbindung damit herstellen können. Sobald Sie mit Grundlagen vertraut sind, können Sie sich mit dem Artikel [Azure Resource Manager-REST-API-Referenz](https://docs.microsoft.com/rest/api/resources/) befassen, der ausführliche Informationen dazu enthält, wie Sie die anderen APIs verwenden.

## <a name="authentication"></a>Authentifizierung
Die Authentifizierung für Resource Manager wird von Azure Active Directory (AD) abgewickelt. Zum Herstellen einer Verbindung mit einer API müssen Sie sich zuerst bei Azure AD authentifizieren, um ein Authentifizierungstoken zu erhalten, das Sie für jede Anforderung übergeben können. Da wir ein direktes Aufrufen der REST-APIs vorstellen, gehen wir davon aus, dass Sie sich nicht authentifizieren möchten, indem Sie zur Eingabe eines Benutzernamens und Kennworts aufgefordert werden. Wir gehen ferner davon aus, dass Sie keine Zwei-Faktor-Authentifizierungsmechanismen verwenden. Daher erstellen wir zuerst eine so genannte Azure AD-Anwendung und einen Dienstprinzipal, die für die Anmeldung verwendet werden. Bedenken Sie aber, dass Azure AD mehrere Authentifizierungsprozeduren unterstützt, die alle zum Abrufen dieses Authentifizierungstokens verwendet werden, das wir für nachfolgende API-Anforderungen benötigen.
Eine Schrittanleitung finden Sie unter [Erstellen einer Azure AD-Anwendung und eines Dienstprinzipals](resource-group-create-service-principal-portal.md).

### <a name="generating-an-access-token"></a>Generieren eines Zugriffstokens
Die Authentifizierung gegenüber Azure AD wird durchgeführt, indem Azure AD unter „login.microsoftonline.com“ aufgerufen wird. Zum Authentifizieren benötigen Sie die folgenden Informationen:

* Azure AD-Mandanten-ID (Name der Azure AD-Instanz, die Sie zum Anmelden verwenden und die häufig, aber nicht zwangsläufig, der Instanz Ihres Unternehmens entspricht)
* Anwendungs-ID (bei der Erstellung der Azure AD-Anwendung ermittelt)
* Kennwort (bei der Erstellung der Azure AD-Anwendung ausgewählt)

Stellen Sie bei der nachfolgenden HTTP-Anforderung sicher, dass Sie „Azure AD Tenant ID“, „Application ID“ und „Password“ durch die richtigen Werte ersetzen.

**Generische HTTP-Anforderung:**

```HTTP
POST /<Azure AD Tenant ID>/oauth2/token?api-version=1.0 HTTP/1.1 HTTP/1.1
Host: login.microsoftonline.com
Cache-Control: no-cache
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&resource=https%3A%2F%2Fmanagement.core.windows.net%2F&client_id=<Application ID>&client_secret=<Password>
```

... ähnelt (bei erfolgreicher Authentifizierung) dieser Antwort:

```json
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1448199959",
  "not_before": "1448196059",
  "resource": "https://management.core.windows.net/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhb...86U3JI_0InPUk_lZqWvKiEWsayA"
}
```
(Die „access_token“-Elemente in der obigen Antwort wurden gekürzt, um die Lesbarkeit zu verbessern.)

**Generieren des Zugriffstokens mit Bash:**

```console
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=client_credentials&resource=https://management.core.windows.net/&client_id=<application id>&client_secret=<password you selected for authentication>" https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0
```

**Generieren des Zugriffstokens mit PowerShell:**

```powershell
Invoke-RestMethod -Uri https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0 -Method Post
 -Body @{"grant_type" = "client_credentials"; "resource" = "https://management.core.windows.net/"; "client_id" = "<application id>"; "client_secret" = "<password you selected for authentication>" }
```

Die Antwort enthält ein Zugriffstoken, Informationen zur Gültigkeitsdauer des Tokens und Informationen dazu, für welche Ressourcen Sie dieses Token verwenden können.
Das Zugriffstoken, das Sie im vorherigen HTTP-Aufruf erhalten haben, muss für alle Anforderungen an die Ressourcen-Manager-API übergeben werden. Sie übergeben es als Headerwert mit dem Namen „Authorization“ und dem Wert „Bearer YOUR_ACCESS_TOKEN“. Beachten Sie das Leerzeichen zwischen „Bearer“ und Ihrem Zugriffstoken.

Wie Sie anhand des obigen HTTP-Ergebnisses erkennen können, ist das Token für einen bestimmten Zeitraum gültig, in dem Sie dieses Token zwischenspeichern und wiederverwenden können. Es ist zwar ggf. möglich, die Authentifizierung gegenüber Azure AD für jeden API-Aufruf durchzuführen, aber dies wäre sehr ineffizient.

## <a name="calling-resource-manager-rest-apis"></a>Aufrufen von Ressourcen-Manager-REST-APIs
In diesem Thema werden nur einige der APIs zum Erläutern der grundlegenden Verwendung von REST-Vorgängen vorgestellt. Informationen zu sämtlichen Vorgängen finden Sie unter [Ressourcen-Manager-REST-APIs](https://docs.microsoft.com/rest/api/resources/).

### <a name="list-all-subscriptions"></a>Auflisten aller Abonnements
Einer der einfachsten Vorgänge ist das Auflisten der verfügbaren Abonnements, auf die Sie zugreifen können. In der folgenden Anforderung sehen Sie, wie das Zugriffstoken als Header übergeben wird:

(Ersetzen Sie YOUR_ACCESS_TOKEN durch Ihr Zugriffstoken.)

```HTTP
GET /subscriptions?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

Sie erhalten dann eine Liste mit den Abonnements, auf die dieser Dienstprinzipal zugreifen kann.

(Die Abonnement-IDs wurden gekürzt, um die Lesbarkeit zu verbessern.)

```json
{
  "value": [
    {
      "id": "/subscriptions/3a8555...555995",
      "subscriptionId": "3a8555...555995",
      "displayName": "Azure Subscription",
      "state": "Enabled",
      "subscriptionPolicies": {
        "locationPlacementId": "Internal_2014-09-01",
        "quotaId": "Internal_2014-09-01"
      }
    }
  ]
}
```

### <a name="list-all-resource-groups-in-a-specific-subscription"></a>Auflisten aller Ressourcengruppen in einem bestimmten Abonnement
Alle Ressourcen, die für die Ressourcen-Manager-APIs verfügbar sind, sind in einer Ressourcengruppe geschachtelt. Sie können Ressourcen-Manager auf in Ihrem Abonnement vorhandene Ressourcengruppen mithilfe der folgenden HTTP GET-Anforderung abfragen. Beachten Sie, wie die Abonnement-ID dieses Mal als Teil der URL übergeben wird.

(Ersetzen Sie YOUR_ACCESS_TOKEN und SUBSCRIPTION_ID durch Ihr Zugriffstoken bzw. Ihre Abonnement-ID.)

```HTTP
GET /subscriptions/SUBSCRIPTION_ID/resourcegroups?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

Die Antwort hängt davon ab, ob und wie viele Ressourcengruppen Sie definiert haben.

(Die Abonnement-IDs wurden gekürzt, um die Lesbarkeit zu verbessern.)

```json
{
    "value": [
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/myfirstresourcegroup",
            "name": "myfirstresourcegroup",
            "location": "eastus",
            "properties": {
                "provisioningState": "Succeeded"
            }
        },
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/mysecondresourcegroup",
            "name": "mysecondresourcegroup",
            "location": "northeurope",
            "tags": {
                "tagname1": "My first tag"
            },
            "properties": {
                "provisioningState": "Succeeded"
            }
        }
    ]
}
```

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Bisher haben wir nur die Ressourcen-Manager-APIs auf Informationen abgefragt. Es ist Zeit, einige Ressourcen zu erstellen. Lassen Sie uns mit dem einfachsten Typ beginnen, einer Ressourcengruppe. Die folgende HTTP-Anforderung erstellt eine Ressourcengruppe in einer Region bzw. am Standort Ihrer Wahl und fügt ihr ein Tag hinzu.

(Ersetzen Sie YOUR_ACCESS_TOKEN, SUBSCRIPTION_ID und RESOURCE_GROUP_NAME durch Ihr Zugriffstoken, die Abonnement-ID und den Namen der Ressourcengruppe, die Sie erstellen möchten.)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  }
}
```

Bei erfolgreicher Ausführung erhalten Sie eine Antwort, die etwa wie folgt aussieht:

```json
{
  "id": "/subscriptions/3a8555...555995/resourceGroups/RESOURCE_GROUP_NAME",
  "name": "RESOURCE_GROUP_NAME",
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  },
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

Sie haben eine Ressourcengruppe in Azure erfolgreich erstellt. Glückwunsch!

### <a name="deploy-resources-to-a-resource-group-using-a-resource-manager-template"></a>Bereitstellen von Ressourcen in einer Ressourcengruppe mit einer Ressourcen-Manager-Vorlage
Mit Ressourcen-Manager können Sie Ihre Ressourcen mithilfe von Vorlagen bereitstellen. Ein Vorlage definiert verschiedene Ressourcen und ihre Abhängigkeiten. In diesem Abschnitt wird davon ausgegangen, dass Sie mit Ressourcen-Manager-Vorlagen vertraut sind. Zudem wird nur gezeigt, wie der API-Aufruf zum Starten der Bereitstellung erfolgt. Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).

Die Bereitstellung einer Vorlage unterscheidet sich nicht sehr vom Aufrufen anderer APIs. Ein wichtiger Aspekt ist, dass die Bereitstellung einer Vorlage ziemlich lange dauern kann. Für den API-Aufruf erfolgt lediglich eine Rückgabe. Es ist Ihre Aufgabe als Entwickler, den Status der Bereitstellung abzufragen, um herauszufinden, wann die Bereitstellung erfolgt ist. Weitere Informationen finden Sie unter [Nachverfolgen asynchroner Vorgänge in Azure](resource-manager-async-operations.md).

Für dieses Beispiel verwenden wir eine öffentlich verfügbar gemachte Vorlage, die auf [GitHub](https://github.com/Azure/azure-quickstart-templates)zur Verfügung steht. Diese Vorlage stellt eine Linux-VM in der Region USA, Westen bereit. Auch wenn bei diesem Beispiel eine Vorlage verwendet wird, die in einem öffentlichen Repository wie GitHub verfügbar ist, können Sie stattdessen auch die vollständige Vorlage als Teil der Anforderung übergeben. Beachten Sie, dass wir Parameterwerte in der Anforderung bereitstellen, die in der bereitgestellten Vorlage verwendet werden.

(Ersetzen Sie SUBSCRIPTION_ID, RESOURCE_GROUP_NAME, DEPLOYMENT_NAME, YOUR_ACCESS_TOKEN, GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME, ADMIN_USER_NAME, ADMIN_PASSWORD und DNS_NAME_FOR_PUBLIC_IP durch die geeigneten Werte für Ihre Anforderung.)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME/providers/microsoft.resources/deployments/DEPLOYMENT_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "properties": {
    "templateLink": {
      "uri": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json",
      "contentVersion": "1.0.0.0",
    },
    "mode": "Incremental",
    "parameters": {
        "newStorageAccountName": {
          "value": "GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME"
        },
        "adminUsername": {
          "value": "ADMIN_USER_NAME"
        },
        "adminPassword": {
          "value": "ADMIN_PASSWORD"
        },
        "dnsNameForPublicIP": {
          "value": "DNS_NAME_FOR_PUBLIC_IP"
        },
        "ubuntuOSVersion": {
          "value": "15.04"
        }
    }
  }
}
```

Die lange JSON-Antwort auf diese Anforderung wurde weggelassen, um die Lesbarkeit dieser Dokumentation zu verbessern. Die Antwort enthält Informationen zur vorlagengestützten Bereitstellung, die Sie erstellt haben.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Arbeiten mit asynchronen REST-Vorgängen finden Sie unter [Nachverfolgen asynchroner Vorgänge in Azure](resource-manager-async-operations.md).

