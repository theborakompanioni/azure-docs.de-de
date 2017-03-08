---
title: "Überprüfen des Datenverkehrs mit der IP-Datenflussüberprüfung in Azure Network Watcher – Azure CLI | Microsoft-Dokumentation"
description: "Dieser Artikel beschreibt, wie Sie mithilfe der Azure CLI überprüfen, ob bei einem virtuellen Computer eingehender und ausgehender Datenverkehr zugelassen oder verweigert wird."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 92b857ed-c834-4c1b-8ee9-538e7ae7391d
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 1d0136b044f6049e59fa09d824cf244cac703c45
ms.openlocfilehash: 3c0287783e3f2b7acaeefd87acbda30885f93c22
ms.lasthandoff: 02/23/2017


---
# <a name="check-if-traffic-is-allowed-or-denied-to-or-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a>Überprüfen mit der IP-Datenflussüberprüfung (einer Komponente von Azure Network Watcher), ob bei einem virtuellen Computer eingehender und ausgehender Datenverkehr zugelassen oder verweigert wird

> [!div class="op_single_selector"]
> - [Azure-Portal](network-watcher-check-ip-flow-verify-portal.md)
> - [PowerShell](network-watcher-check-ip-flow-verify-powershell.md)
> - [BEFEHLSZEILENSCHNITTSTELLE (CLI)](network-watcher-check-ip-flow-verify-cli.md)
> - [Azure-REST-API](network-watcher-check-ip-flow-verify-rest.md)

Die IP-Datenflussüberprüfung ist ein Feature von Network Watcher, mit dem Sie überprüfen können, ob bei einem virtuellen Computer eingehender oder ausgehender Datenverkehr zugelassen wird. Dieses Szenario eignet sich zum Abrufen des aktuellen Status der Kommunikation eines virtuellen Computers mit einer externen Ressource oder einem Back-End. Anhand der IP-Datenflussüberprüfung können Sie überprüfen, ob die NSG-Regeln (Netzwerksicherheitsgruppe) ordnungsgemäß konfiguriert sind, und eine Problembehandlung für Datenflüsse durchführen, die durch die NSG-Regeln blockiert werden. Ein weiterer Grund für die Verwendung der IP-Datenflussüberprüfung besteht darin sicherzustellen, dass Datenverkehr, der blockiert werden soll, durch die NSG ordnungsgemäß blockiert wird.

## <a name="before-you-begin"></a>Voraussetzungen

Dieses Szenario setzt voraus, dass Sie die Schritte unter [Erstellen einer Network Watcher-Instanz](network-watcher-create.md) bereits durchgeführt haben, um eine Network Watcher-Instanz zu erstellen, bzw. dass Sie eine vorhandene Instanz von Network Watcher verwenden. Ferner wird davon ausgegangen, dass eine Ressourcengruppe mit einem gültigen virtuellen Computer vorhanden ist und verwendet werden kann.

[!INCLUDE [network-watcher-preview](../../includes/network-watcher-public-preview-notice.md)]

## <a name="scenario"></a>Szenario

Dieses Szenario verwendet die IP-Datenflussüberprüfung, um zu überprüfen, ob ein virtueller Computer mit einer bekannten Bing-IP-Adresse kommunizieren kann. Wenn der Datenverkehr abgelehnt wird, wird die Sicherheitsregel zurückgegeben, die den jeweiligen Datenverkehr verweigert. Weitere Informationen zur IP-Datenflussüberprüfung finden Sie in der [Übersicht zur IP-Datenflussüberprüfung](network-watcher-ip-flow-verify-overview.md).


## <a name="get-a-vm"></a>Abrufen eines virtuellen Computers

Die IP-Datenflussüberprüfung testet den bei einer IP-Adresse eines virtuellen Computers eingehenden oder ausgehenden Datenverkehr von einem bzw. an ein Remoteziel. Eine ID eines virtuellen Computers ist zum Ausführen des Cmdlets erforderlich. Wenn Sie die ID des virtuellen Computers bereits kennen, können Sie diesen Schritt überspringen.

```
azure vm show -g resourceGroupName -n virtualMachineName
```

## <a name="get-the-nics"></a>Abrufen der NICs

Die IP-Adresse einer NIC auf dem virtuellen Computer wird benötigt. In diesem Beispiel rufen wir die NICs auf einem virtuellen Computer ab. Wenn Sie die IP-Adresse bereits kennen, die Sie auf dem virtuellen Computer testen möchten, können Sie diesen Schritt überspringen.

```
azure network nic show -g resourceGroupName -n nicName
```

## <a name="run-ip-flow-verify"></a>Ausführen der IP-Datenflussüberprüfung

Da wir jetzt die zum Ausführen des Cmdlets benötigten Informationen besitzen, führen wir das Cmdlet `network watcher ip-flow-verify` aus, um den Datenverkehr zu testen. In diesem Beispiel verwenden wir die erste IP-Adresse der ersten NIC.

```
azure network watcher ip-flow-verify -g resourceGroupName -n networkWatcherName -t targetResourceId -d directionInboundorOutbound -p protocolTCPorUDP -o localPort -m remotePort -l localIpAddr -r remoteIpAddr
```

> [!NOTE]
> Für die IP-Datenflussüberprüfung ist es erforderlich, dass die VM-Ressource für die Ausführung zugeordnet wird.

## <a name="review-results"></a>Überprüfen der Ergebnisse

Nach der Ausführung von `network watcher ip-flow-verify` werden die Ergebnisse zurückgegeben. Das folgende Beispiel zeigt die im vorhergehenden Schritt zurückgegebenen Ergebnisse.

```
data:    Access                          : Deny
data:    Rule Name                       : defaultSecurityRules/DefaultInboundDenyAll
info:    network watcher ip-flow-verify command OK
```

## <a name="next-steps"></a>Nächste Schritte

Wenn Datenverkehr fälschlicherweise blockiert wird, finden Sie unter [Verwalten von Netzwerksicherheitsgruppen](../virtual-network/virtual-network-manage-nsg-arm-portal.md) Informationen zum Ermitteln der Netzwerksicherheitsgruppe und der definierten Sicherheitsregeln.

Informationen zur Überwachung Ihrer NSG-Einstellungen finden Sie unter [Überwachen von Netzwerksicherheitsgruppen (NSG) mit Network Watcher](network-watcher-nsg-auditing-powershell.md).

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png

