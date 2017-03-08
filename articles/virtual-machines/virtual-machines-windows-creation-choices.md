---
title: "Verschiedene Möglichkeiten zum Erstellen eines virtuellen Windows-Computers in Azure | Microsoft Docs"
description: "Auflistung der verschiedenen Möglichkeiten zum Erstellen eines virtuellen Windows-Computers mit Ressourcen-Manager"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 809ba8f4-b54e-43c5-bbe3-8e710c49971f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/02/2017
ms.author: cynthn
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 32e30b44c2f7cfa9c1069190fdc53dbe6e9f4cd5
ms.openlocfilehash: d01b71257a4a95294aebcde8c1d056e3fc3ed1be
ms.lasthandoff: 03/01/2017


---
# <a name="different-ways-to-create-a-windows-virtual-machine"></a>Verschiedene Möglichkeiten zum Erstellen eines virtuellen Windows-Computers

Azure bietet verschiedene Möglichkeiten zum Erstellen eines virtuellen Computers, da virtuelle Computer für verschiedene Benutzer und Zwecke geeignet sind. Daher müssen Sie einige Optionen für den virtuellen Computer und dessen Erstellung auswählen. Dieser Artikel bietet einen Überblick über diese Optionen sowie Links zu Anweisungen.

## <a name="azure-portal"></a>Azure-Portal
Über das Azure-Portal lassen sich virtuelle Computer sehr einfach testen. Dies gilt insbesondere dann, wenn Sie noch nicht viel Erfahrung mit Azure haben. 

[Create a virtual machine running Windows using the portal (Erstellen eines virtuellen Windows-Computers mithilfe des Portals)](virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="template"></a>Vorlage
Virtuelle Computer benötigen verschiedene Ressourcen (wie etwa Verfügbarkeitsgruppen und Speicherkonten). Anstatt jede Ressource gesondert bereitzustellen und zu verwalten, können Sie eine Azure Resource Manager-Vorlage erstellen, die für die Bereitstellung aller Ressourcen in einem einzigen koordinierten Vorgang sorgt.

* [Erstellen Sie einen virtuellen Windows-Computer mit einer Resource Manager-Vorlage](virtual-machines-windows-ps-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="azure-powershell"></a>Azure PowerShell
Falls Sie lieber in einer Befehlsshell arbeiten, können Sie Azure PowerShell verwenden.

* [Create a Windows VM using PowerShell (Erstellen eines virtuellen Windows-Computers mithilfe von PowerShell)](virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="visual-studio"></a>Visual Studio
Verwenden Sie Visual Studio, um virtuelle Computer mit den Azure-Tools für Visual Studio und dem Azure SDK zu erstellen, zu verwalten und bereitzustellen.

[Azure-Tools für Visual Studio](https://www.visualstudio.com/features/azure-tools-vs)


