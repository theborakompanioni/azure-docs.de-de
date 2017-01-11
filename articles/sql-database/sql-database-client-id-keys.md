---
title: "Abrufen der erforderlichen Werte aus dem Code zum Authentifizieren einer Anwendung für den Zugriff auf die SQL-Datenbank | Microsoft Docs"
description: "Erstellen Sie einen Dienstprinzipalnamen für den Zugriff auf SQL-Datenbank aus dem Code."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
tags: 
ms.assetid: b43e43bb-6660-49e6-b069-abde97eb5770
ms.service: sql-database
ms.custom: development
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 09/30/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: 2956ce36ab851de6dd4a6c0725044cf832c213a0
ms.openlocfilehash: 16a735bf5c0c85a41b960c20dc9ff8877f9a3855


---
# <a name="get-the-required-values-for-authenticating-an-application-to-access-sql-database-from-code"></a>Abrufen der erforderlichen Werte aus dem Code zum Authentifizieren einer Anwendung für den Zugriff auf die SQL-Datenbank
Zum Erstellen und Verwalten der SQL-Datenbank über den Code müssen Sie Ihre App in der Azure Active Directory-Domäne (AAD) in dem Abonnement registrieren, in dem die Azure-Ressourcen erstellt wurden.

## <a name="create-a-service-principal-to-access-resources-from-an-application"></a>Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen aus einer Anwendung
Die aktuelle Version von [Azure PowerShell](https://msdn.microsoft.com/library/mt619274.aspx) muss installiert sein und ausgeführt werden. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).

Mit dem folgenden PowerShell-Skript werden die Active Directory (AD)-Anwendung und der Dienstprinzipal erstellt, den wir zum Authentifizieren der C#-App benötigen. Das Skript gibt Werte aus, die für das vorhergehende C#-Beispiel erforderlich sind. Ausführliche Informationen finden Sie unter [Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe von Azure PowerShell](../resource-group-authenticate-service-principal.md).

    # Sign in to Azure.
    Add-AzureRmAccount

    # If you have multiple subscriptions, uncomment and set to the subscription you want to work with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId

    # Provide these values for your new AAD app.
    # $appName is the display name for your app, must be unique in your directory.
    # $uri does not need to be a real uri.
    # $secret is a password you create.

    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"

    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret

    # Create a Service Principal for the app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

    # To avoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15

    # If you still get a PrincipalNotFound error, then rerun the following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid


    # Output the values we need for our C# application to successfully authenticate

    Write-Output "Copy these values into the C# sample app"

    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret




## <a name="see-also"></a>Siehe auch
* [Erstellen einer SQL-Datenbank mit C#](sql-database-get-started-csharp.md)
* [Herstellen einer Verbindung mit SQL-Datenbank unter Verwendung der Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md)




<!--HONumber=Dec16_HO1-->


