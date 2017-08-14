---
title: Erstellen eines Azure Analysis Services-Servers mithilfe von PowerShell | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mithilfe von PowerShell einen Azure Analysis Services-Server erstellen.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/01/2017
ms.author: owend
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 8b857b4a629618d84f66da28d46f79c2b74171df
ms.openlocfilehash: 43700de27ca643bd699159b86fe80c2e45e4acc7
ms.contentlocale: de-de
ms.lasthandoff: 08/04/2017

---

# <a name="create-an-azure-analysis-services-server-by-using-powershell"></a>Erstellen eines Azure Analysis Services-Servers mithilfe von PowerShell

In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe von PowerShell über die Befehlszeile einen Azure Analysis Services-Server in einer [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) in Ihrem Azure-Abonnement erstellen.

Hierzu benötigen Sie mindestens die Azure PowerShell-Modulversion 4.0. Führen Sie ` Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Informationen zum Installieren oder Upgraden finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/install-azurerm-ps). 

> [!NOTE]
> Wenn Sie einen Server erstellen, wird dadurch unter Umständen auch ein neuer abrechenbarer Dienst erstellt. Weitere Informationen finden Sie unter [Analysis Services – Preise](https://azure.microsoft.com/pricing/details/analysis-services/).

## <a name="prerequisites"></a>Voraussetzungen
Für diese Schnellstartanleitung benötigen Sie Folgendes:

* **Azure-Abonnement:** Besuchen Sie die Webseite [Kostenlose Azure-Testversion](https://azure.microsoft.com/offers/ms-azr-0044p/), und erstellen Sie ein Konto.
* **Azure Active Directory:** Ihr Abonnement muss einem Azure Active Directory-Mandanten zugeordnet sein, und Sie müssen über ein Konto in diesem Verzeichnis verfügen. Weitere Informationen finden Sie unter [Authentifizierung und Benutzerberechtigungen](analysis-services-manage-users.md).

## <a name="import-azurermanalysisservices-module"></a>Importieren des Moduls „AzureRm.AnalysisServices“
Für die Servererstellung in Ihrem Abonnement verwenden Sie das Komponentenmodul [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices). Laden Sie das Modul „AzureRm.AnalysisServices“ in Ihre PowerShell-Sitzung.

```powershell
Import-Module AzureRM.AnalysisServices
```

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich mithilfe des Befehls [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) bei Ihrem Azure-Abonnement an. Befolgen Sie die Anweisungen auf dem Bildschirm.

```powershell
Add-AzureRmAccount
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
 
Eine [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und als Gruppe verwaltet werden. Wenn Sie Ihren Server erstellen, müssen Sie eine Ressourcengruppe in Ihrem Abonnement angeben. Falls Sie noch nicht über eine Ressourcengruppe verfügen, können Sie mithilfe des Befehls [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine neue Ressourcengruppe erstellen. Im folgenden Beispiel wird eine Ressourcengruppe namens `myResourceGroup` in der Region „USA, Westen“ erstellt.

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "West US"
```

## <a name="create-a-server"></a>Erstellen eines Servers

Erstellen Sie mithilfe des Befehls [New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) einen neuen Server. Im folgenden Beispiel wird ein Server namens „myServer“ in „myResourceGroup“ in der Region „USA, Westen“ zum Tarif „D1“ erstellt und philipc@adventureworks.com als Serveradministrator angegeben.

```powershell
New-AzureRmAnalysisServicesServer -ResourceGroupName "myResourceGroup" -Name "myServer" -Location West US -Sku D1 -Administrator "philipc@adventure-works.com"
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Mithilfe des Befehls [Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) können Sie den Server aus Ihrem Abonnement entfernen. Falls Sie mit weiteren Schnellstartanleitungen und Tutorials aus dieser Sammlung fortfahren möchten, entfernen Sie den Server nicht. Im folgenden Beispiel wird der im vorherigen Schritt erstellte Server entfernt.


```powershell
Remove-AzureRmAnalysisServicesServer -Name "myServer" -ResourceGroupName "myResourceGroup"
```

## <a name="next-steps"></a>Nächste Schritte
[Verwalten von Azure Analysis Services mit PowerShell](analysis-services-powershell.md)   
[Bereitstellen eines Modells mithilfe von SSDT](analysis-services-deploy.md)   
[Erstellen eines Modells im Azure-Portal](analysis-services-create-model-portal.md)
