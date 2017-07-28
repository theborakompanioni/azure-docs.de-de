---
title: Azure-Containerregistrierungsrepositorys | Microsoft-Dokumentation
description: "Verwenden der Azure-Containerregistrierungsrepositorys für Docker-Images"
services: container-registry
documentationcenter: 
author: cristy
manager: balans
editor: dlepow
ms.service: container-registry
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/30/2017
ms.author: cristyg
ms.translationtype: Human Translation
ms.sourcegitcommit: 245ce9261332a3d36a36968f7c9dbc4611a019b2
ms.openlocfilehash: 2fb060749c203a445196530bb7711d50d83c2923
ms.contentlocale: de-de
ms.lasthandoff: 06/09/2017


---

# <a name="create-a-private-docker-container-registry-using-the-azure-powershell"></a>Erstellen einer privaten Docker-Containerregistrierung mit Azure PowerShell
Verwenden Sie Befehle in [Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/overview), um eine Containerregistrierung zu erstellen und die Einstellungen dafür über Ihren Windows-Computer zu verwalten. Sie können Containerregistrierungen auch mit dem [Azure-Portal](container-registry-get-started-portal.md), der [Azure-Befehlszeilenschnittstelle](container-registry-get-started-azure-cli.md) oder programmgesteuert mit der [REST-API](https://go.microsoft.com/fwlink/p/?linkid=834376) für die Containerregistrierung erstellen und verwalten.


* Hintergrundinformationen und Konzepte finden Sie in der [Übersicht](container-registry-intro.md).
* Eine vollständige Liste der unterstützten Cmdlets finden Sie unter [Azure Container Registry Management Cmdlets (Azure-Containerregistrierung – Verwaltungs-Cmdlets)](https://docs.microsoft.com/en-us/powershell/module/azurerm.containerregistry/).


## <a name="prerequisites"></a>Voraussetzungen
* **Azure PowerShell**: Informationen zur Installation und den ersten Schritten mit Azure PowerShell finden Sie in der [Installationsanleitung](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps). Melden Sie sich an Ihrem Azure-Abonnement an, indem Sie `Login-AzureRMAccount` ausführen. Weitere Informationen finden Sie unter [Erste Schritte mit Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azurep).
* **Ressourcengruppe**: Erstellen Sie eine [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md#resource-groups), bevor Sie eine Containerregistrierung erstellen, oder verwenden Sie eine vorhandene Ressourcengruppe. Stellen Sie sicher, dass sich die Ressourcengruppe an einem Standort befindet, für den der Containerregistrierungsdienst [verfügbar](https://azure.microsoft.com/regions/services/) ist. Weitere Informationen zum Erstellen einer Ressourcengruppe mit Azure PowerShell finden Sie unter [PowerShell-Referenz](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps#create-a-resource-group).
* **Speicherkonto** (optional): Erstellen Sie ein standardmäßiges Azure-[Speicherkonto](../storage/storage-introduction.md) zum Sichern der Containerregistrierung an demselben Standort. Wenn Sie beim Erstellen einer Registrierung mit `New-AzureRMContainerRegistry` kein Speicherkonto angeben, führt der Befehl dies für Sie durch. Weitere Informationen zum Erstellen eines Speicherkontos mit PowerShell finden Sie unter [PowerShell-Referenz](https://docs.microsoft.com/en-us/powershell/module/azure/new-azurestorageaccount). Storage Premium wird derzeit nicht unterstützt.
* **Dienstprinzipal** (optional): Wenn Sie mit PowerShell eine Registrierung erstellen, wird der Zugriff standardmäßig nicht eingerichtet. Je nach Ihren Anforderungen können Sie einen vorhandenen Azure Active Directory-Dienstprinzipal einer Registrierung zuweisen (oder einen neuen erstellen und zuweisen). Alternativ können Sie das Administratorbenutzerkonto der Registrierung aktivieren. Informationen hierzu finden Sie in den Abschnitten weiter unten in diesem Artikel. Weitere Informationen zum Zugreifen auf die Registrierung finden Sie unter [Authenticate with the container registry](container-registry-authentication.md) (Authentifizieren bei der Containerregistrierung).

## <a name="create-a-container-registry"></a>Erstellen einer Containerregistrierung
Führen Sie den Befehl `New-AzureRMContainerRegistry` aus, um eine Containerregistrierung zu erstellen.

> [!TIP]
> Geben Sie beim Erstellen einer Registrierung einen global eindeutigen Namen einer Domäne der obersten Ebene an, der nur Buchstaben und Zahlen enthält. Der Registrierungsname in den Beispielen lautet `MyRegistry`. Ersetzen Sie ihn durch einen eigenen eindeutigen Namen.
>
>

Im folgenden Befehl werden die minimal erforderlichen Parameter zum Erstellen der Containerregistrierung `MyRegistry` in der Ressourcengruppe `MyResourceGroup` für den Standort „USA, Süden-Mitte“ verwendet:

```PowerShell
$Registry = New-AzureRMContainerRegistry -ResourceGroupName "MyResourceGroup" -Name "MyRegistry"
```

* `-StorageAccountName` ist optional. Wenn nichts angegeben ist, wird in der angegebenen Ressourcengruppe ein Speicherkonto mit einem Namen erstellt, der aus dem Registrierungsnamen und einem Zeitstempel besteht.

## <a name="assign-a-service-principal"></a>Zuweisen eines Dienstprinzipals
Verwenden Sie PowerShell-Befehle, um einen Azure Active Directory-[Dienstprinzipal](../azure-resource-manager/resource-group-authenticate-service-principal.md) einer Registrierung zuzuweisen. Dem Dienstprinzipal wird in diesen Beispielen die Rolle „Besitzer“ zugewiesen, aber Sie können bei Bedarf auch [andere Rollen](../active-directory/role-based-access-control-configure.md) zuweisen.

### <a name="create-a-service-principal"></a>Erstellen eines Dienstprinzipals
Mit dem folgenden Befehl wird ein neuer Dienstprinzipal erstellt. Geben Sie mit dem Parameter `-Password` ein sicheres Kennwort an.

```PowerShell
$ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName ApplicationDisplayName -Password "MyPassword"
```

### <a name="assign-a-new-or-existing-service-principal"></a>Zuweisen eines neuen oder vorhandenen Dienstprinzipals
Sie können einer Registrierung einen neuen oder vorhandenen Dienstprinzipal zuweisen. Wenn Sie über die Rolle „Besitzer“ Zugriff auf die Registrierung zuweisen möchten, können Sie einen ähnlichen Befehl wie im folgenden Beispiel ausführen:

```PowerShell
New-AzureRMRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Registry.Id
```

##<a name="sign-in-to-the-registry-with-the-service-principal"></a>Anmeldung bei der Registrierung mit Dienstprinzipal
Nachdem Sie der Registrierung den Dienstprinzipal zugewiesen haben, können Sie sich mithilfe des folgenden Befehls anmelden:

```PowerShell
docker login -u $ServicePrincipal.ApplicationId -p myPassword
```

## <a name="manage-admin-credentials"></a>Verwalten von Administratoranmeldeinformationen
Ein Administratorkonto wird automatisch für jede Containerregistrierung erstellt und ist standardmäßig deaktiviert. In den folgenden Beispielen werden PowerShell-Befehle verwendet, um die Administratoranmeldeinformationen für Ihre Containerregistrierung zu verwalten.

### <a name="obtain-admin-user-credentials"></a>Abrufen von Administratorbenutzer-Anmeldeinformationen
```PowerShell
Get-AzureRMContainerRegistryCredential -ResourceGroupName "MyResourceGroup" -Name "MyRegistry"
```

### <a name="enable-admin-user-for-an-existing-registry"></a>Aktivieren des Administratorbenutzers für eine vorhandene Registrierung
```PowerShell
Update-AzureRMContainerRegistry -ResourceGroupName "MyResourceGroup" -Name "MyRegistry" -EnableAdminUser
```

### <a name="disable-admin-user-for-an-existing-registry"></a>Deaktivieren des Administratorbenutzers für eine vorhandene Registrierung
```PowerShell
Update-AzureRMContainerRegistry -ResourceGroupName "MyResourceGroup" -Name "MyRegistry" -DisableAdminUser
```

## <a name="next-steps"></a>Nächste Schritte
* [Freigeben Ihres ersten Image mit der Docker CLI](container-registry-get-started-docker-cli.md)

