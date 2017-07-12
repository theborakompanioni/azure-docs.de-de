---
title: "Navigieren zu und Auswählen von Windows-VM-Images | Microsoft Docs"
description: "Erfahren Sie, wie Sie den Herausgeber, das Angebot und die SKU für Images ermitteln, wenn Sie mit dem Resource Manager-Bereitstellungsmodell einen virtuellen Windows-Computer erstellen."
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
ms.date: 08/23/2016
ms.author: danlep
ms.translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 28bb214570fcca94c5ceb6071c4851b81ec00c8d
ms.contentlocale: de-de
ms.lasthandoff: 04/27/2017

---
<a id="navigate-and-select-windows-virtual-machine-images-in-azure-with-powershell" class="xliff"></a>

# Navigieren zu und Auswählen von Images virtueller Windows-Computer in Azure mithilfe von PowerShell
In diesem Thema wird beschrieben, wie Sie Herausgeber von VM-Images sowie entsprechende Angebote, SKUs und Versionen für jeden Ort finden, an dem Sie Bereitstellungen ins Auge fassen. Beispielsweise sind einige der häufig verwendeten Windows-VM-Images:

<a id="table-of-commonly-used-windows-images" class="xliff"></a>

## Tabelle mit häufig verwendeten Windows-Images
| PublisherName | Angebot | Sku |
|:--- |:--- |:--- |:--- |
| MicrosoftDynamicsNAV |DynamicsNAV |2015 |
| MicrosoftSharePoint |MicrosoftSharePointServer |2013 |
| MicrosoftSQLServer |SQL 2014 WS2012R2 |Enterprise-Optimized-for-DW |
| MicrosoftSQLServer |SQL 2014 WS2012R2 |Enterprise-Optimized-for-OLTP |
| MicrosoftWindowsServer |Windows Server |2012-R2-Datacenter |
| MicrosoftWindowsServer |Windows Server |2012-Datacenter |
| MicrosoftWindowsServer |Windows Server |2008-R2-SP1 |
| MicrosoftWindowsServer |Windows Server |Windows-Server-Technical-Preview |
| MicrosoftWindowsServerEssentials |WindowsServerEssentials |WindowsServerEssentials |
| MicrosoftWindowsServerHPCPack |WindowsServerHPCPack |2012R2 |

<a id="find-azure-images-with-powershell" class="xliff"></a>

## Suchen nach Azure-Images mit PowerShell
> [!NOTE]
> Installieren und konfigurieren Sie die [neueste Version von Azure PowerShell](/powershell/azure/overview). Wenn Sie Azure PowerShell-Module vor Version 1.0 nutzen, verwenden Sie die folgenden Befehle, zunächst müssen Sie jedoch `Switch-AzureMode AzureResourceManager`ausführen. 
> 
> 

Wenn Sie mit dem Azure-Ressourcen-Manager einen neuen virtuellen Computer erstellen, kann es sein, dass Sie in einigen Fällen ein Datenträgerabbild mit den folgenden Datenträgerabbild-Eigenschaften angeben müssen:

* Herausgeber
* Angebot
* SKU

Diese Werte werden beispielsweise für das PowerShell-Cmdlet `Set-AzureRMVMSourceImage` oder mit einer Vorlagendatei einer Ressourcengruppe benötigt, in der Sie den Typ des zu erstellenden virtuellen Computers angeben müssen.

Wenn Sie diese Werte angeben müssen, können Sie durch die Images navigieren, um die Werte folgendermaßen zu bestimmen:

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

Sie können der Anzeige des Befehls `Get-AzureRMVMImageSku` alle Informationen entnehmen, die Sie brauchen, um das Image für einen neuen virtuellen Computer anzugeben.

Im Folgenden lernen Sie ein vollständiges Beispiel kennen:

```powershell
PS C:\> $locName="West US"
PS C:\> Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName

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
PS C:\> $pubName="MicrosoftWindowsServer"
PS C:\> Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer

Offer
-----
WindowsServer
```

Für das Angebot „WindowsServer“:

```powershell
PS C:\> $offerName="WindowsServer"
PS C:\> Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus

Skus
----
2008-R2-SP1
2012-Datacenter
2012-R2-Datacenter
2016-Nano-Server-Technical-Previe
2016-Technical-Preview-with-Conta
Windows-Server-Technical-Preview
```

Wenn Sie den gewählten SKU-Namen aus dieser Liste kopieren, besitzen Sie alle Informationen für das PowerShell-Cmdlet `Set-AzureRMVMSourceImage` oder eine Ressourcengruppenvorlage.

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
Jetzt können Sie genau das Datenträgerabbild auswählen, das Sie verwenden möchten. Um anhand der soeben gefundenen Imageinformationen schnell einen virtuellen Computer zu erstellen oder eine Vorlage mit diesen Imageinformationen zu verwenden, lesen Sie die Informationen unter [Erstellen einer Windows-VM mit Resource Manager und PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

