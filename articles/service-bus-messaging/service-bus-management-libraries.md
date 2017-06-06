---
title: Azure Service Bus-Verwaltungsbibliotheken| Microsoft-Dokumentation
description: "Verwalten von Service Bus-Namespaces und -Messagingentitäten über .NET"
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/03/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: cc9e81de9bf8a3312da834502fa6ca25e2b5834a
ms.openlocfilehash: ec9f2fa3d88f59172d320b58287208deb084856f
ms.contentlocale: de-de
ms.lasthandoff: 04/11/2017


---

# <a name="service-bus-management-libraries"></a>Service Bus-Verwaltungsbibliotheken

Die Service Bus-Verwaltungsbibliotheken können dynamisch Service Bus-Namespaces und -Entitäten bereitstellen. Dies ermöglicht komplexe Bereitstellungen und Messagingszenarien sowie eine programmgesteuerte Bestimmung der bereitzustellenden Entitäten. Die Bibliotheken sind derzeit für .NET verfügbar.

## <a name="supported-functionality"></a>Unterstützte Funktionen

* Erstellen, Aktualisieren und Löschen von Namespaces
* Erstellen, Aktualisieren und Löschen von Warteschlangen
* Erstellen, Aktualisieren und Löschen von Themen
* Erstellen, Aktualisieren und Löschen von Abonnements

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung der Service Bus-Verwaltungsbibliotheken müssen Sie zunächst eine Authentifizierung mit Azure Active Directory (AAD) durchführen. AAD erfordert, dass Sie sich als Dienstprinzipal authentifizieren. Dadurch erhalten Sie Zugriff auf Ihre Azure-Ressourcen. Informationen zum Erstellen eines Dienstprinzipals finden Sie in einem der folgenden Artikel:  

* [Erstellen einer Active Directory-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff mithilfe des Portals](/azure/azure-resource-manager/resource-group-create-service-principal-portal)
* [Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe von Azure PowerShell](/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe der Azure-Befehlszeilenschnittstelle](/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)

In diesen Tutorials erhalten Sie Werte für `AppId` (Client-ID), `TenantId` und `ClientSecret` (Authentifizierungsschlüssel). Diese werden von den Verwaltungsbibliotheken für die Authentifizierung verwendet. Für die Ressourcengruppe, für die die Ausführung erfolgen soll, müssen Sie über Berechtigungen vom Typ „Besitzer“ verfügen.

## <a name="programming-pattern"></a>Muster für die Programmierung

Das Muster zum Bearbeiten einer beliebigen Service Bus-Ressource folgt einem gemeinsamen Protokoll:

1. Rufen Sie ein Token aus Azure Active Directory mithilfe der **Microsoft.IdentityModel.Clients.ActiveDirectory**-Bibliothek ab.
    ```csharp
    var context = new AuthenticationContext($"https://login.windows.net/{tenantId}");

    var result = await context.AcquireTokenAsync(
        "https://management.core.windows.net/",
        new ClientCredential(clientId, clientSecret)
    );
    ```

1. Erstellen Sie das Objekt `ServiceBusManagementClient`.
    ```csharp
    var creds = new TokenCredentials(token);
    var sbClient = new ServiceBusManagementClient(creds)
    {
        SubscriptionId = SettingsCache["SubscriptionId"]
    };
    ```

1. Legen Sie die `CreateOrUpdate`-Parameter auf Ihre spezifischen Werte fest.
    ```csharp
    var queueParams = new QueueCreateOrUpdateParameters()
    {
        Location = SettingsCache["DataCenterLocation"],
        EnablePartitioning = true
    };
    ```

1. Führen Sie den Aufruf aus.
    ```csharp
    await sbClient.Queues.CreateOrUpdateAsync(resourceGroupName, namespaceName, QueueName, queueParams);
    ```

## <a name="next-steps"></a>Nächste Schritte
* [.NET-Verwaltungsbeispiel](https://github.com/Azure-Samples/service-bus-dotnet-management/)
* [Microsoft.Azure.Management.ServiceBus-Referenz](/dotnet/api/Microsoft.Azure.Management.ServiceBus)

