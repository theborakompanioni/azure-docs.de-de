---
title: "Azure CLI-Skriptbeispiel: Lastenausgleich für mehrere Websites mit der Azure CLI | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel: Lastenausgleich für mehrere Websites auf dem gleichen virtuellen Computer"
services: load-balancer
documentationcenter: load-balancer
author: KumudD
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: load-balancer
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 04/19/2017
ms.author: kumud
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: b4bfc65a4a32e33e6eaecf16e928484862100cd1
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---

# <a name="load-balance-multiple-websites"></a>Lastenausgleich für mehrere Websites

Dieses Skriptbeispiel erstellt ein virtuelles Netzwerk mit zwei virtuellen Computern, die einer Verfügbarkeitsgruppe angehören. Ein Load Balancer leitet Datenverkehr für zwei separate IP-Adressen an die beiden virtuellen Computer. Nach der Ausführung des Skripts können Sie Webserversoftware auf den virtuellen Computern bereitstellen und mehrere Websites (jede mit eigener IP-Adresse) hosten.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript


[!code-azurecli[Main](../../../cli_scripts/load-balancer/load-balance-multiple-web-sites-vm/load-balance-multiple-web-sites-vm.sh  "Lastenausgleich für mehrere Websites")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, ein virtuelles Netzwerk, einen Load Balancer und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet#create) | Erstellt ein virtuelles Azure-Netzwerk und ein Subnetz. |
| [az network public-ip create](https://docs.microsoft.com/cli/azure/network/public-ip#create) | Erstellt eine öffentliche IP-Adresse mit einer statischen IP-Adresse und zugeordnetem DNS-Namen. |
| [az network lb create](https://docs.microsoft.com/cli/azure/network/lb#create) | Erstellt einen Azure Network Load Balancer (NLB). |
| [az network lb probe create](https://docs.microsoft.com/cli/azure/network/lb/probe#create) | Erstellt einen NLB-Test. Mithilfe eines NLB-Tests wird jede VM in der NLB-Gruppe überwacht. Falls eine VM nicht verfügbar ist, wird der Datenverkehr nicht an diese VM geroutet. |
| [az network lb rule create](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | Erstellt eine NLB-Regel. In diesem Beispiel wird eine Regel für den Port 80 erstellt. Wenn HTTP-Datenverkehr beim NLB eingeht, wird dieser an Port 80 einer der VMs in der NLB-Gruppe geroutet. |
| [az network lb frontend-ip create](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip#create) | Erstellt eine Front-End-IP-Adresse für den Load Balancer. |
| [az network lb address-pool create](https://docs.microsoft.com/cli/azure/network/lb/address-pool#create) | Erstellt einen Back-End-Adresspool. |
| [az network nic create](https://docs.microsoft.com/cli/azure/network/nic#create) | Erstellt eine virtuelle Netzwerkkarte und verbindet diese mit dem virtuellen Netzwerk und dem Subnetz. |
| [az vm availability-set create](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | Erstellt eine Verfügbarkeitsgruppe. Verfügbarkeitsgruppen stellen die Verfügbarkeit von Anwendungen sicher, indem sie die virtuellen Computer auf physische Ressourcen verteilen. So wirken sich Ausfälle nicht auf die gesamte Gruppe aus. |
| [az network nic ip-config create](https://docs.microsoft.com/cli/azure/network/nic/ip-config#create) | Erstellt eine IP-Konfiguration. Das Feature „Microsoft.Network/AllowMultipleIpConfigurationsPerNic“ muss für Ihr Abonnement aktiviert sein. Als primäre IP-Konfiguration kann pro NIC nur eine Konfiguration festgelegt werden. Verwenden Sie dazu das Flag „--make-primary“. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | Erstellt den virtuellen Computer und verbindet diesen mit der Netzwerkkarte, dem virtuellen Netzwerk, dem Subnetz und der NSG. Dieser Befehl legt außerdem das zu verwendende VM-Image und die Administratoranmeldeinformationen fest.  |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#set) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche Netzwerk-CLI-Skriptbeispiele finden Sie unter [Azure CLI Samples for networking](../cli-samples.md?toc=%2fazure%2fnetworking%2ftoc.json) (Azure CLI-Beispiele für Netzwerke).

