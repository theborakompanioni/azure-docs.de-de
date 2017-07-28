---
title: "Azure PowerShell-Skriptbeispiel: Lastenausgleich für den Datenverkehr zu virtuellen Computern für hohe Verfügbarkeit | Microsoft-Dokumentation"
description: "Azure PowerShell-Skriptbeispiel: Lastenausgleich für den Datenverkehr zu virtuellen Computern für hohe Verfügbarkeit"
services: load-balancer
documentationcenter: load-balancer
author: georgewallace
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: load-balancer
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 05/16/2017
ms.author: gwallace
ms.translationtype: Human Translation
ms.sourcegitcommit: 09f24fa2b55d298cfbbf3de71334de579fbf2ecd
ms.openlocfilehash: c77def8906b151f2cc6e4bbc4188be8ecbeac732
ms.contentlocale: de-de
ms.lasthandoff: 06/07/2017

---

# <a name="load-balance-traffic-to-vms-for-high-availability"></a>Lastenausgleich für den Datenverkehr zu virtuellen Computern für hohe Verfügbarkeit

Dieses Beispielskript erstellt alle Komponenten, die zum Ausführen mehrerer Windows-VMs in einer Konfiguration mit hoher Verfügbarkeit und Lastenausgleich benötigt werden. Nach dem Ausführen dieses Skripts verfügen Sie über drei virtuelle Computer, die in einer Azure-Verfügbarkeitsgruppe zusammengefasst und über eine Azure Load Balancer-Instanz zugänglich sind.

Installieren Sie bei Bedarf Azure PowerShell anhand der Anleitung im [Azure PowerShell-Handbuch](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), und führen Sie dann `Login-AzureRmAccount` aus, um eine Verbindung mit Azure herzustellen.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.ps1 "Schnelles Erstellen einer VM")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, einen virtuellen Computer, eine Verfügbarkeitsgruppe, einen Load Balancer und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Erstellt eine Subnetzkonfiguration. Diese Konfiguration wird mit dem Prozess der Erstellung des virtuellen Netzwerks verwendet. |
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Erstellt ein virtuelles Azure-Netzwerk und ein Subnetz. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress)  | Erstellt eine öffentliche IP-Adresse mit einer statischen IP-Adresse und zugeordnetem DNS-Namen. |
| [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer)  | Erstellt eine Azure Load Balancer-Instanz |
| [New-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) | Erstellt einen Lastenausgleichtest. Ein Lastenausgleichtest wird verwendet, um jedem virtuellen Computer in der Load Balancer-Gruppe zu überwachen. Falls eine VM nicht verfügbar ist, wird der Datenverkehr nicht an diese VM geroutet. |
| [New-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) | Erstellt eine Load Balancer-Regel. In diesem Beispiel wird eine Regel für den Port 80 erstellt. Wenn HTTP-Datenverkehr beim Load Balancer eingeht, wird dieser an Port 80 einer der VMs in der Load Balancer-Gruppe geroutet. |
| [New-AzureRmLoadBalancerInboundNatRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerinboundnatruleconfig) | Erstellt eine Regel des Load Balancers (Network Address Translation, NAT).  Mithilfe von NAT-Regeln wird ein Load Balancer-Port einem Port auf einer VM zugeordnet. In diesem Beispiel wird eine NAT-Regel für den SSH-Datenverkehr an jede VM in der Load Balancer-Gruppe erstellt.  |
| [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Erstellt eine Netzwerksicherheitsgruppe (NSG), die als Sicherheitsgrenze zwischen dem Internet und dem virtuellen Computer fungiert. |
| [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Erstellt eine NSG-Regel zum Zulassen von eingehendem Datenverkehr. In diesem Beispiel wird Port 22 für SSH-Datenverkehr geöffnet. |
| [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Erstellt eine virtuelle Netzwerkkarte und verbindet diese mit dem virtuellen Netzwerk, dem Subnetz und der NSG. |
| [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset) | Erstellt eine Verfügbarkeitsgruppe. Verfügbarkeitsgruppen stellen die Verfügbarkeit von Anwendungen sicher, indem sie die virtuellen Computer auf physische Ressourcen verteilen. So wirken sich Ausfälle nicht auf die gesamte Gruppe aus. |
| [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Erstellt eine VM-Konfiguration. Diese Konfiguration umfasst Informationen wie VM-Name, Betriebssystem und Administratoranmeldeinformationen. Die Konfiguration wird während der VM-Erstellung verwendet. |
| [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm)  | Erstellt den virtuellen Computer und verbindet diesen mit der Netzwerkkarte, dem virtuellen Netzwerk, dem Subnetz und der NSG. Dieser Befehl legt außerdem das zu verwendende VM-Image und die Administratoranmeldeinformationen fest.  |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/azure/overview).

Zusätzliche Netzwerk-PowerShell-Skriptbeispiele finden Sie unter [Azure CLI-Beispiele für Netzwerke](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).

