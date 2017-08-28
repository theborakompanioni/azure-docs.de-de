---
title: Azure Event Hubs-Verwaltungsbibliotheken | Microsoft-Dokumentation
description: "Verwalten von Event Hubs-Namespaces und -Entitäten über .NET"
services: event-hubs
cloud: na
documentationcenter: na
author: sethmanheim
manager: timlt
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/15/2017
ms.author: sethm
ms.translationtype: HT
ms.sourcegitcommit: 1e6fb68d239ee3a66899f520a91702419461c02b
ms.openlocfilehash: 0d659cb860a6c98342b548212820efe046decfcc
ms.contentlocale: de-de
ms.lasthandoff: 08/16/2017

---

# <a name="event-hubs-management-libraries"></a>Event Hubs-Verwaltungsbibliotheken

Die Event Hubs-Verwaltungsbibliotheken können dynamisch Event Hubs-Namespaces und -Entitäten bereitstellen. Dies ermöglicht komplexe Bereitstellungen und Messagingszenarien sowie eine programmgesteuerte Bestimmung der bereitzustellenden Entitäten. Die Bibliotheken sind derzeit für .NET verfügbar.

## <a name="supported-functionality"></a>Unterstützte Funktionen

* Erstellen, Aktualisieren und Löschen von Namespaces
* Erstellen, Aktualisieren und Löschen von Event Hubs
* Erstellen, Aktualisieren und Löschen von Consumergruppen

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung von Event Hubs-Verwaltungsbibliotheken müssen Sie zunächst eine Authentifizierung mit Azure Active Directory (AAD) durchführen. AAD erfordert, dass Sie sich als Dienstprinzipal authentifizieren. Dadurch erhalten Sie Zugriff auf Ihre Azure-Ressourcen. Informationen zum Erstellen eines Dienstprinzipals finden Sie in einem der folgenden Artikel:  

* [Erstellen einer Active Directory-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff mithilfe des Portals](../azure-resource-manager/resource-group-create-service-principal-portal.md)
* [Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe von Azure PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe der Azure-Befehlszeilenschnittstelle](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md)

In diesen Tutorials erhalten Sie Werte für `AppId` (Client-ID), `TenantId` und `ClientSecret` (Authentifizierungsschlüssel). Diese werden von den Verwaltungsbibliotheken für die Authentifizierung verwendet. Für die Ressourcengruppe, für die die Ausführung erfolgen soll, müssen Sie über **Besitzer**-Berechtigungen verfügen.

## <a name="programming-pattern"></a>Muster für die Programmierung

Das Muster zum Bearbeiten einer beliebigen Event Hubs-Ressource folgt einem gemeinsamen Protokoll:

1. Abrufen eines Tokens aus AAD mithilfe der `Microsoft.IdentityModel.Clients.ActiveDirectory`-Bibliothek.
    ```csharp
    var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

    var result = await context.AcquireTokenAsync(
        "https://management.core.windows.net/",
        new ClientCredential(clientId, clientSecret)
    );
    ```

1. Erstellen Sie das Objekt `EventHubManagementClient`.
    ```csharp
    var creds = new TokenCredentials(token);
    var ehClient = new EventHubManagementClient(creds)
    {
        SubscriptionId = SettingsCache["SubscriptionId"]
    };
    ```

1. Legen Sie die `CreateOrUpdate`-Parameter auf Ihre spezifischen Werte fest.
    ```csharp
    var ehParams = new EventHubCreateOrUpdateParameters()
    {
        Location = SettingsCache["DataCenterLocation"]
    };
    ```

1. Führen Sie den Aufruf aus.
    ```csharp
    await ehClient.EventHubs.CreateOrUpdateAsync(resourceGroupName, namespaceName, EventHubName, ehParams);
    ```

## <a name="next-steps"></a>Nächste Schritte
* [.NET-Verwaltungsbeispiel](https://github.com/Azure-Samples/event-hubs-dotnet-management/)
* [Microsoft.Azure.Management.EventHub-Referenz](/dotnet/api/Microsoft.Azure.Management.EventHub) 

