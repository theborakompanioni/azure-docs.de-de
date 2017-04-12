---
title: "Erstellen von Vorlagen mit Erweiterungen für virtuelle Windows-Computer | Microsoft Docs"
description: "Hier finden Sie Informationen zum Erstellen von Azure Resource Manager-Vorlagen mit Erweiterungen für virtuelle Windows-Computer."
services: virtual-machines-windows
documentationcenter: 
author: kundanap
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 418dd1f7-ded8-45ab-9a5a-a59d245e2555
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/29/2016
ms.author: kundanap
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: 338668df33783d8ef62c6710f4d96ce805296fe5
ms.lasthandoff: 03/31/2017


---
# <a name="authoring-azure-resource-manager-templates-with-windows-vm-extensions"></a>Erstellen von Azure Resource Manager-Vorlagen mit Erweiterungen für virtuelle Windows-Computer
[!INCLUDE [virtual-machines-common-extensions-authoring-templates](../../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Führen Sie über Azure PowerShell das folgende Azure PowerShell-Cmdlet aus:

      Get-AzureVMAvailableExtension


Dieses Cmdlet gibt den Herausgebernamen, den Erweiterungsnamen und die Version folgendermaßen zurück:

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

Diese drei Eigenschaften sind im oben dargestellten Vorlagenausschnitt „publisher“, „type“ und „typeHandlerVersion“ zugeordnet.

> [!NOTE]
> Es empfiehlt sich, die aktuelle Erweiterungsversion zu verwenden, um die neuesten Funktionen zu erhalten.
> 
> 

## <a name="identifying-the-schema-for-the-extension-configuration-parameters"></a>Identifizieren des Schemas für die Erweiterungskonfigurationsparameter
Im nächsten Schritt zur Erstellung einer Erweiterungsvorlage wird das Format für die Bereitstellung von Konfigurationsparametern festgelegt. Jede Erweiterung unterstützt einen eigenen Satz von Parametern.

Beispielkonfigurationen für Windows-Erweiterungen finden Sie unter [Beispiele für Windows-Erweiterungen](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Eine vollständige Vorlage mit Erweiterungen für virtuelle Computer finden Sie hier:

[Benutzerdefinierte Skripterweiterung auf einem virtuellen Windows-Computer](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/)

Die erstellte Vorlage können Sie anschließend über Azure PowerShell bereitstellen.


