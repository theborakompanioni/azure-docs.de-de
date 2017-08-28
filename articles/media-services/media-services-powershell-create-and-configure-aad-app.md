---
title: "Erstellen einer Azure AD-App mithilfe von PowerShell für den Zugriff auf die Azure Media Services-API | Microsoft-Dokumentation"
description: "In diesem Artikel erfahren Sie, wie Sie mithilfe von PowerShell eine Azure AD-App (Azure Active Directory) erstellen und diese für den Zugriff auf die Azure Media Services-API einrichten."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: juliako
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: f1d7ad94686d633b6353f39cef91d1cb1efab773
ms.contentlocale: de-de
ms.lasthandoff: 07/01/2017

---

# <a name="use-powershell-to-create-an-azure-ad-app-to-use-with-the-azure-media-services-api"></a>Erstellen einer Azure AD-App mithilfe von PowerShell für die Verwendung mit der Azure Media Services-API

In diesem Artikel erfahren Sie, wie Sie mithilfe eines PowerShell-Skripts eine Azure AD-Anwendung (Azure Active Directory) und einen Dienstprinzipal erstellen, um auf Azure Media Services-Ressourcen zuzugreifen.  

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto. Beginnen Sie mit einer [kostenlosen Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/), falls Sie kein Konto haben. 
- Media Services-Konto. Weitere Informationen finden Sie unter [Erstellen eines Azure Media Services-Kontos mithilfe des Azure-Portals](media-services-portal-create-account.md).
- Azure PowerShell Version 0.8.8 oder höher. Weitere Informationen finden Sie unter [Vorgehensweise zur Verwendung von Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).
- Azure Resource Manager-Cmdlets  

## <a name="create-an-azure-ad-app-by-using-powershell"></a>Erstellen einer Azure AD-App mithilfe von PowerShell  

```powershell
Login-AzureRmAccount
Import-Module AzureRM.Resources
Set-AzureRmContext -SubscriptionId $SubscriptionId
$ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -Password $Password

Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 
$NewRole = $null
$Scope = "/subscriptions/your subscription id/resourceGroups/userresourcegroup/providers/microsoft.media/mediaservices/your media account"

$Retries = 0;While ($NewRole -eq $null -and $Retries -le 6)
{
    # Sleep here for a few seconds to allow the service principal application to become active (usually, it will take only a couple of seconds)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
}
```

Weitere Informationen finden Sie in den folgenden Artikeln:

- [Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe von Azure PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal.md)
- [Verwalten der rollenbasierten Zugriffssteuerung mit Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md)
- [Vorgehensweise zur manuellen Konfiguration von Daemon-Apps mithilfe von Zertifikaten](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md#add-the-certificate-as-a-key-for-the-todolistdaemonwithcert-application-in-azure-ad)

## <a name="next-steps"></a>Nächste Schritte

Erste Schritte mit dem [Hochladen von Dateien in Ihr Konto](media-services-portal-upload-files.md)

