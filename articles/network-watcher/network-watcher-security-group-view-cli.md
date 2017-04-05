---
title: "Analysieren der Netzwerksicherheit mit der Azure Network Watcher-Sicherheitsgruppenansicht – Azure CLI | Microsoft-Dokumentation"
description: "In diesem Artikel wird die Verwendung der Azure CLI zum Analysieren der Sicherheit eines virtuellen Computers über die Sicherheitsgruppenansicht beschrieben."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: a986ff4f-7e0c-4994-95e1-4ac824986500
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 6e0ad6b5bec11c5197dd7bded64168a1b8cc2fdd
ms.openlocfilehash: 9c539f5885a5014f98ccc1083c93daa4eee3646c
ms.lasthandoff: 03/28/2017

---

# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-azure-cli"></a>Analysieren der Sicherheit Ihres virtuellen Computers über die Sicherheitsgruppenansicht mithilfe der Azure CLI

> [!div class="op_single_selector"]
> - [PowerShell](network-watcher-security-group-view-powershell.md)
> - [BEFEHLSZEILENSCHNITTSTELLE (CLI)](network-watcher-security-group-view-cli.md)
> - [REST-API](network-watcher-security-group-view-rest.md)

Die Sicherheitsgruppenansicht gibt konfigurierte und effektive Netzwerksicherheitsregeln zurück, die auf einen virtuellen Computer angewendet werden. Diese Funktion eignet sich zur Überwachung und Diagnose von Netzwerksicherheitsgruppen und Regeln, die auf einem virtuellen Computer konfiguriert sind, um sicherzustellen, dass Datenverkehr ordnungsgemäß zugelassen oder verweigert wird. In diesem Artikel erfahren Sie, wie Sie die konfigurierten und effektiven Sicherheitsregeln mithilfe der Azure CLI auf einen virtuellen Computer abrufen.

In diesem Artikel wird die plattformübergreifende Azure CLI 1.0 verwendet, die für Windows, Mac und Linux zur Verfügung steht. Network Watcher verwendet derzeit Azure CLI 1.0 als Unterstützung für die Befehlszeilenschnittstelle.

## <a name="before-you-begin"></a>Voraussetzungen

Dieses Szenario setzt voraus, dass Sie die Schritte unter [Erstellen einer Network Watcher-Instanz](network-watcher-create.md) bereits durchgeführt haben, um eine Network Watcher-Instanz zu erstellen.

## <a name="scenario"></a>Szenario

In dem in diesem Artikel beschriebenen Szenario werden die konfigurierten und effektiven Sicherheitsregeln für einen bestimmten virtuellen Computer abgerufen.

## <a name="get-a-vm"></a>Abrufen eines virtuellen Computers

Ein virtueller Computer ist zum Ausführen des Cmdlets `vm list` erforderlich. Der folgende Befehl listet die virtuellen Computer in einer Ressourcengruppe auf:

```azurecli
azure vm list -g resourceGroupName
```

Sobald Sie den virtuellen Computer kennen, können Sie die Ressourcen-ID mit dem Cmdlet `vm show` abrufen:

```azurecli
azure vm show -g resourceGroupName -n virtualMachineName
```

## <a name="retrieve-security-group-view"></a>Abrufen der Sicherheitsgruppenansicht

Im nächsten Schritt wird das Ergebnis der Sicherheitsgruppenansicht abgerufen. Durch Hinzufügen des Flags „--json“ werden die Ergebnisse im JSON-Format formatiert.

```azurecli
azure network watcher security-group-view -g resourceGroupName -n networkWatcherName -t targetResourceId --json
```

## <a name="viewing-the-results"></a>Anzeigen der Ergebnisse

Das folgende Beispiel ist eine verkürzte Antwort der zurückgegebenen Ergebnisse. In den Ergebnissen werden alle effektiven und angewendeten Sicherheitsregeln auf dem virtuellen Computer in folgende Gruppen unterteilt angezeigt: **NetworkInterfaceSecurityRules**, **DefaultSecurityRules** und **EffectiveSecurityRules**.

```json
{
  "networkInterfaces": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkInterfaces/testnic",
      "securityRuleAssociations": {
        "networkInterfaceAssociation": {
          "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkInterfaces/testvm",
          "securityRules": [
            {
              "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/test-nsg/securityRules/default-allow-rdp",
              "protocol": "TCP",
              "sourcePortRange": "*",
              "destinationPortRange": "3389",
              "sourceAddressPrefix": "*",
              "destinationAddressPrefix": "*",
              "access": "Allow",
              "priority": 1000,
              "direction": "Inbound",
              "provisioningState": "Succeeded",
              "name": "default-allow-rdp",
              "etag": "W/\"00000000-0000-0000-0000-000000000000\""
            }
          ]
        },
        "defaultSecurityRules": [
          {
            "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/networkSecurityGroups//defaultSecurityRules/",
            "description": "Allow inbound traffic from all VMs in VNET",
            "protocol": "*",
            "sourcePortRange": "*",
            "destinationPortRange": "*",
            "sourceAddressPrefix": "VirtualNetwork",
            "destinationAddressPrefix": "VirtualNetwork",
            "access": "Allow",
            "priority": 65000,
            "direction": "Inbound",
            "provisioningState": "Succeeded",
            "name": "AllowVnetInBound"
          }
        ]
      }
    }
  ]
}
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Automatisierung der Überprüfung von Netzwerksicherheitsgruppen finden Sie unter [Überwachen von Netzwerksicherheitsgruppen (NSG) mit Network Watcher](network-watcher-nsg-auditing-powershell.md).

Weitere Informationen zu den Sicherheitsregeln, die auf Ihre Netzwerkressourcen angewendet werden, finden Sie unter [Übersicht über die Sicherheitsgruppenansicht](network-watcher-security-group-view-overview.md).

