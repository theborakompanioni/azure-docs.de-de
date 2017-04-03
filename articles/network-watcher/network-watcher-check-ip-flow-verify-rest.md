---
title: "Überprüfen des Datenverkehrs mit der IP-Datenflussüberprüfung in Azure Network Watcher – REST | Microsoft-Dokumentation"
description: "Dieser Artikel beschreibt, wie Sie überprüfen, ob bei einem virtuellen Computer eingehender und ausgehender Datenverkehr zugelassen oder verweigert wird."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 3307a79f-03be-46a0-aaaf-b2079cb5f3b2
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 2f03ba60d81e97c7da9a9fe61ecd419096248763
ms.openlocfilehash: 18b98300ee103e3f3118f6db4e1c1a0e169eecad
ms.lasthandoff: 03/04/2017


---
# <a name="check-if-traffic-is-allowed-or-denied-with-ip-flow-verify-a-component-of-azure-network-watcher"></a>Überprüfen mit der IP-Datenflussüberprüfung (einer Komponente von Azure Network Watcher), ob eingehender und ausgehender Datenverkehr zugelassen oder verweigert wird

> [!div class="op_single_selector"]
> - [Azure-Portal](network-watcher-check-ip-flow-verify-portal.md)
> - [PowerShell](network-watcher-check-ip-flow-verify-powershell.md)
> - [BEFEHLSZEILENSCHNITTSTELLE (CLI)](network-watcher-check-ip-flow-verify-cli.md)
> - [Azure-REST-API](network-watcher-check-ip-flow-verify-rest.md)

Die IP-Datenflussüberprüfung ist ein Feature von Network Watcher, mit dem Sie überprüfen können, ob bei einem virtuellen Computer eingehender oder ausgehender Datenverkehr zugelassen wird. Die Überprüfung kann für den eingehenden oder ausgehenden Datenverkehr durchgeführt werden. Dieses Szenario eignet sich zum Abrufen des aktuellen Status der Kommunikation eines virtuellen Computers mit einer externen Ressource oder einem Back-End. Anhand der IP-Datenflussüberprüfung können Sie überprüfen, ob die NSG-Regeln (Netzwerksicherheitsgruppe) ordnungsgemäß konfiguriert sind, und eine Problembehandlung für Datenflüsse durchführen, die durch die NSG-Regeln blockiert werden. Ein weiterer Grund für die Verwendung der IP-Datenflussüberprüfung besteht darin sicherzustellen, dass Datenverkehr, der blockiert werden soll, durch die NSG ordnungsgemäß blockiert wird.

## <a name="before-you-begin"></a>Voraussetzungen

ARMClient dient zum Aufrufen der REST-API mithilfe von PowerShell. Sie finden [ARMClient auf Chocolatey](https://chocolatey.org/packages/ARMClient).

Dieses Szenario setzt voraus, dass Sie die Schritte unter [Erstellen einer Network Watcher-Instanz](network-watcher-create.md) bereits durchgeführt haben, um eine Network Watcher-Instanz zu erstellen.

## <a name="scenario"></a>Szenario

Dieses Szenario verwendet die IP-Datenflussüberprüfung, um zu überprüfen, ob ein virtueller Computer über den Port 443 mit einem anderen Computer kommunizieren kann. Wenn der Datenverkehr abgelehnt wird, wird die Sicherheitsregel zurückgegeben, die den jeweiligen Datenverkehr verweigert. Weitere Informationen zur IP-Datenflussüberprüfung finden Sie in der [Übersicht zur IP-Datenflussüberprüfung](network-watcher-ip-flow-verify-overview.md)

In diesem Szenario führen Sie Folgendes durch:

* Abrufen eines virtuellen Computers
* Aufrufen der IP-Datenflussüberprüfung
* Überprüfen der Ergebnisse

## <a name="log-in-with-armclient"></a>Anmelden mit ARMClient

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a>Abrufen eines virtuellen Computers

Führen Sie das folgende Skript aus, um einen virtuellen Computer zurückzugeben. Der folgende Code benötigt Werte für die folgenden Variablen:

* **subscriptionId:** die zu verwendende Abonnement-ID.
* **resourceGroupName:** der Name einer Ressourcengruppe, die virtuelle Computer enthält.

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

Benötigt wird hier die ID unter dem Typ `Microsoft.Compute/virtualMachines`. Die Ergebnisse sollten in etwa wie in folgendem Codebeispiel aussehen:

```json
...,
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "/subscriptions/{00000000-0000-0000-0000-000000000000}/resourceGroups/ContosoExampleRG/providers/Microsoft
.Network/networkInterfaces/contosovm842"
            }
          ]
        },
        "provisioningState": "Succeeded"
      },
      "resources": [
        {
          "id": "/subscriptions/{00000000-0000-0000-0000-000000000000}/resourceGroups/ContosoExampleRG/providers/Microsoft.Com
pute/virtualMachines/ContosoVM/extensions/CustomScriptExtension"
        }
      ],
      "type": "Microsoft.Compute/virtualMachines",
      "location": "westcentralus",
      "id": "/subscriptions/{00000000-0000-0000-0000-000000000000}/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute
/virtualMachines/ContosoVM",
      "name": "ContosoVM"
    }
  ]
}
```

## <a name="call-ip-flow-verify"></a>Aufrufen der IP-Datenflussüberprüfung

Das folgende Beispiel erstellt eine Anforderung zur Überprüfung des Datenverkehrs für einen angegebenen virtuellen Computer. Die Antwort gibt zurück, ob Datenverkehr zugelassen oder verweigert wird. Wenn der Datenverkehr verweigert wird, enthält sie auch die Regel, die den Datenverkehr blockiert.

> [!NOTE]
> Für die IP-Datenflussüberprüfung ist es erforderlich, dass die VM-Ressource zugeordnet ist.

Das Skript benötigt die Ressourcen-ID eines virtuellen Computers und einer Netzwerkschnittstellenkarte auf dem virtuellen Computer. Diese Werte finden Sie in der obigen Ausgabe.

> [!Important]
> Für alle REST-Aufrufe von Network Watcher enthält der Ressourcengruppenname im Anforderungs-URI die Network Watcher-Instanz, nicht die Ressourcen, für die Sie die Diagnoseaktionen ausführen.

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"
$networkWatcherName = "<network watcher name>"
$vmName = "<vm name>"
$vmNICName = "<vm NIC name>"
$direction = "<direction of traffic>" # Examples are: Inbound or Outbound
$localIP = "<source IP>"
$localPort = "<source Port>"
$remoteIP = "<destination IP>"
$remotePort = "<destination Port>" # Examples are: 80, or 80-120
$protocol = "<UDP, TCP or *>"
$targetUri = "<uri of target resource>" # Example: /subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.compute/virtualMachine/${vmName}
$targetNic = "<uri of target nic resource>" # Example: /subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkInterfaces/${vmNICName}

$requestBody = @"
{
    'targetResourceId':  '$targetUri',
    'direction':  '$direction',
    'protocol':  '$protocol',
    'localPort':  '$localPort',
    'remotePort':  '$remotePort',
    'localIPAddress':  '$localIP',
    'remoteIPAddress':  '$remoteIP',
    'targetNICResourceId':  '$targetNic'
}
"@
        
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/ipFlowVerify?api-version=2016-12-01" $requestBody -verbose
```

## <a name="understanding-the-results"></a>Grundlegendes zu den Ergebnissen

Die Antwort, gibt Aufschluss darüber, ob der Datenverkehr zugelassen oder verweigert wird. Die Antwort sieht in etwa wie in den folgenden Beispielen aus:

**Zulässig**

```json
{
  "access": "Allow",
  "ruleName": "defaultSecurityRules/AllowInternetOutBound"
}
```

**Verweigert**

```json
{
  "access": "Deny",
  "ruleName": "defaultSecurityRules/DefaultInboundDenyAll"
}
```

## <a name="next-steps"></a>Nächste Schritte

Wenn Datenverkehr fälschlicherweise blockiert wird, finden Sie unter [Verwalten von Netzwerksicherheitsgruppen](../virtual-network/virtual-network-manage-nsg-arm-portal.md) Informationen zu Netzwerksicherheitsgruppen.













