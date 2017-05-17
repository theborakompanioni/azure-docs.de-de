---
title: "Azure CLI-Beispielskript – Weiterleiten von Datenverkehr über ein virtuelles Netzwerkgerät | Microsoft-Dokumentation"
description: "Azure CLI-Beispielskript – Weiterleiten von Datenverkehr über ein virtuelles Firewall-Netzwerkgerät."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 04/21/2017
ms.author: jdial
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: e1ab4fd899b31eba3929b8c78e04bc8c09f39e14
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---

# <a name="route-traffic-through-a-network-virtual-appliance"></a>Weiterleiten von Datenverkehr über ein virtuelles Netzwerkgerät

In diesem Skriptbeispiel wird ein virtuelles Netzwerk mit Front-End- und Back-End-Subnetz erstellt. Es wird auch ein virtueller Computer mit IP-Weiterleitung erstellt, der zur Weiterleitung von Datenverkehr zwischen den zwei Subnetzen aktiviert ist. Nach dem Ausführen des Skripts können Sie dem virtuellen Computer Netzwerksoftware bereitstellen, z.B. eine Firewallanwendung.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript


[!code-azurecli[main](../../../cli_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.sh "Weiterleiten von Datenverkehr über ein virtuelles Netzwerkgerät")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, ein virtuelles Netzwerk und Netzwerksicherheitsgruppen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az network vnet create](/cli/azure/network/vnet#create) | Erstellt ein virtuelles Azure-Netzwerk und ein Front-End-Subnetz. |
| [az network subnet create](/cli/azure/network/vnet/subnet#create) | Erstellt Back-End- und DMZ-Subnetz. |
| [az network public-ip create](/cli/azure/network/public-ip#create) | Erstellt eine öffentliche IP-Adresse für den Zugriff auf den virtuellen Computer aus dem Internet. |
| [az network nic create](/cli/azure/network/nic#create) | Erstellt eine virtuelle Netzwerkschnittstelle und aktiviert die IP-Weiterleitung dafür. |
| [az network nsg create](/cli/azure/network/nsg#create) | Erstellt eine Netzwerksicherheitsgruppe (NSG). |
| [az network nsg rule create](/cli/azure/network/nsg/rule#create) | Erstellt NSG-Regeln, die eingehende HTTP- und HTTPS-Ports für den virtuellen Computer ermöglichen. |
| [az network vnet subnet update](/cli/azure/network/vnet/subnet#update)| Ordnet die NSGs und Routentabellen Subnetzen zu. |
| [az network route-table create](/cli/azure/network/route-table#create)| Erstellt eine Routingtabelle für alle Routen. |
| [az network route-table route create](/cli/azure/network/route-table/route#create)| Erstellt die Routen zum Weiterleiten von Datenverkehr zwischen Subnetzen und dem Internet über den virtuellen Computer. |
| [az vm create](/cli/azure/vm#create) | Erstellt einen virtuellen Computer und fügt ihm eine NIC an. Dieser Befehl legt außerdem das zu verwendende VM-Image und die Administratoranmeldeinformationen fest. |
| [az group delete](/cli/azure/group#delete) | Löscht eine Ressourcengruppe und alle darin enthaltenen Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure/overview).

Zusätzliche Netzwerk-CLI-Skriptbeispiele finden Sie unter [Azure CLI Samples for networking](../cli-samples.md) (Azure CLI-Beispiele für Netzwerke).
