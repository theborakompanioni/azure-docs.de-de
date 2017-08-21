---
title: "Auswählen von Windows-VM-Images in Azure | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über die Verwendung von Azure PowerShell, um den Herausgeber, das Angebot, die SKU und die Version für Marketplace-VM-Images zu ermitteln."
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 188b8974-fabd-4cd3-b7dc-559cbb86b98a
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/12/2017
ms.author: danlep
ms.translationtype: HT
ms.sourcegitcommit: 818f7756189ed4ceefdac9114a0b89ef9ee8fb7a
ms.openlocfilehash: 630f555b003b0efc45b372a7009dbf036aa8c737
ms.contentlocale: de-de
ms.lasthandoff: 07/14/2017

---
# <a name="how-to-find-windows-vm-images-in-the-azure-marketplace-with-azure-powershell"></a>Vorgehensweise zum Suchen nach Windows-VM-Images im Azure Marketplace mit Azure PowerShell

Dieses Thema beschreibt, wie Sie mit Azure PowerShell nach VM-Images im Azure Marketplace suchen. Verwenden Sie diese Informationen, um bei der Erstellung einer Windows-VM ein Marketplace-Image anzugeben.

Stellen Sie sicher, dass Sie das neueste [Azure PowerShell-Modul](/powershell/azure/install-azurerm-ps) installiert und konfiguriert haben.



## <a name="table-of-commonly-used-windows-images"></a>Tabelle mit häufig verwendeten Windows-Images
| PublisherName | Angebot | Sku |
|:--- |:--- |:--- |:--- |
| MicrosoftWindowsServer |Windows Server |2016-Datacenter |
| MicrosoftWindowsServer |Windows Server |2016-Datacenter-Server-Core |
| MicrosoftWindowsServer |Windows Server |2016-Datacenter-with-Containers |
| MicrosoftWindowsServer |Windows Server |2016-Nano-Server |
| MicrosoftWindowsServer |Windows Server |2012-R2-Datacenter |
| MicrosoftWindowsServer |Windows Server |2008-R2-SP1 |
| MicrosoftDynamicsNAV |DynamicsNAV |2017 |
| MicrosoftSharePoint |MicrosoftSharePointServer |2016 |
| MicrosoftSQLServer |SQL2016-WS2016 |Enterprise |
| MicrosoftSQLServer |SQL2014SP2-WS2012R2 |Enterprise |
| MicrosoftWindowsServerHPCPack |WindowsServerHPCPack |2012R2 |
| MicrosoftWindowsServerEssentials |WindowsServerEssentials |WindowsServerEssentials |

## <a name="find-specific-images"></a>Suchen nach bestimmten Images


Wenn Sie mit dem Azure-Ressourcen-Manager einen neuen virtuellen Computer erstellen, kann es sein, dass Sie in einigen Fällen ein Datenträgerabbild mit den folgenden Datenträgerabbild-Eigenschaften angeben müssen:

* Herausgeber
* Angebot
* SKU

Verwenden Sie diese Werte beispielsweise mit dem PowerShell-Cmdlet [Set-AzureRMVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) oder mit einer Vorlage einer Ressourcengruppe, in der Sie den Typ der zu erstellenden VM angeben müssen.

Wenn Sie diese Werte bestimmen müssen, können Sie zum Navigieren zu den Images die Cmdlets [Get-AzureRMVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher), [Get-AzureRMVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer) und [Get-AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) ausführen. Sie können die folgenden Werte ermitteln:

1. Auflistung der Herausgeber von Images
2. Auflistung der Angebote eines bestimmten Anbieters
3. Auflistung der SKUs eines bestimmten Angebots

Listen Sie zunächst die Herausgeber mit den folgenden Befehlen auf:

```powershell
$locName="<Azure location, such as West US>"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
```

Tragen Sie den von Ihnen gewählten Herausgebernamen ein, und führen Sie die folgenden Befehle aus:

```powershell
$pubName="<publisher>"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Tragen Sie den von Ihnen gewählten Angebotsnamen ein, und führen Sie die folgenden Befehle aus:

```powershell
$offerName="<offer>"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Sie können der Ausgabe des Befehls `Get-AzureRMVMImageSku` alle Informationen entnehmen, die Sie brauchen, um das Image für einen neuen virtuellen Computer anzugeben.

Im Folgenden lernen Sie ein vollständiges Beispiel kennen:

```powershell
$locName="West US"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName

```

Ausgabe:

```
PublisherName
-------------
a10networks
aiscaler-cache-control-ddos-and-url-rewriting-
alertlogic
AlertLogic.Extension
Barracuda.Azure.ConnectivityAgent
barracudanetworks
basho
boxless
bssw
Canonical
...
```

Für den Herausgeber von „MicrosoftWindowsServer“:

```powershell
$pubName="MicrosoftWindowsServer"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Ausgabe:

```
Offer
-----
Windows-HUB
WindowsServer
WindowsServer-HUB
```

Für das Angebot „WindowsServer“:

```powershell
$offerName="WindowsServer"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Ausgabe:

```
Skus
----
2008-R2-SP1
2008-R2-SP1-smalldisk
2012-Datacenter
2012-Datacenter-smalldisk
2012-R2-Datacenter
2012-R2-Datacenter-smalldisk
2016-Datacenter
2016-Datacenter-Server-Core
2016-Datacenter-Server-Core-smalldisk
2016-Datacenter-smalldisk
2016-Datacenter-with-Containers
2016-Nano-Server
```

Wenn Sie den gewählten SKU-Namen aus dieser Liste kopieren, besitzen Sie alle Informationen für das PowerShell-Cmdlet `Set-AzureRMVMSourceImage` oder eine Ressourcengruppenvorlage.

## <a name="next-steps"></a>Nächste Schritte
Jetzt können Sie genau das Datenträgerabbild auswählen, das Sie verwenden möchten. Um mit den Imageinformationen schnell einen virtuellen Computer zu erstellen, nach dem Sie soeben gesucht haben, lesen Sie den Abschnitt [Erstellen eines virtuellen Windows-Computers mit PowerShell](quick-create-powershell.md).

