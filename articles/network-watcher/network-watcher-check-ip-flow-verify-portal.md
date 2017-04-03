---
title: "Überprüfen des Datenverkehrs mit der IP-Datenflussüberprüfung in Azure Network Watcher – Azure-Portal | Microsoft-Dokumentation"
description: "Dieser Artikel beschreibt, wie Sie überprüfen, ob bei einem virtuellen Computer eingehender und ausgehender Datenverkehr zugelassen oder verweigert wird."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: e0e3e9a8-70eb-409a-a744-0ce9deb27148
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 1aa8fff31d8d1908b1ea93cb970487d08ea4adf6
ms.openlocfilehash: ddd6fcc8de133a5ff5f427d070c6c6ec207d9236
ms.lasthandoff: 03/31/2017


---
# <a name="check-if-traffic-is-allowed-or-denied-to-or-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a>Überprüfen mit der IP-Datenflussüberprüfung (einer Komponente von Azure Network Watcher), ob bei einem virtuellen Computer eingehender und ausgehender Datenverkehr zugelassen oder verweigert wird

> [!div class="op_single_selector"]
> - [Azure-Portal](network-watcher-check-ip-flow-verify-portal.md)
> - [PowerShell](network-watcher-check-ip-flow-verify-powershell.md)
> - [BEFEHLSZEILENSCHNITTSTELLE (CLI)](network-watcher-check-ip-flow-verify-cli.md)
> - [Azure-REST-API](network-watcher-check-ip-flow-verify-rest.md)

Die IP-Datenflussüberprüfung ist ein Feature von Network Watcher, mit dem Sie überprüfen können, ob bei einem virtuellen Computer eingehender oder ausgehender Datenverkehr zugelassen wird. Die Überprüfung kann für den eingehenden oder ausgehenden Datenverkehr durchgeführt werden. Dieses Szenario eignet sich zum Abrufen des aktuellen Status der Kommunikation eines virtuellen Computers mit einer externen Ressource oder einer anderen Ressource. Anhand der IP-Datenflussüberprüfung können Sie überprüfen, ob die NSG-Regeln (Netzwerksicherheitsgruppe) ordnungsgemäß konfiguriert sind, und eine Problembehandlung für Datenflüsse durchführen, die durch die NSG-Regeln blockiert werden. Ein weiterer Grund für die Verwendung der IP-Datenflussüberprüfung besteht darin, sicherzustellen, dass Datenverkehr, der blockiert werden soll, durch die NSG ordnungsgemäß blockiert wird.

## <a name="before-you-begin"></a>Voraussetzungen

Dieses Szenario setzt voraus, dass Sie die Schritte unter [Erstellen einer Network Watcher-Instanz](network-watcher-create.md) bereits durchgeführt haben, um eine Network Watcher-Instanz zu erstellen, bzw. dass Sie eine vorhandene Instanz von Network Watcher verwenden. Ferner wird davon ausgegangen, dass eine Ressourcengruppe mit einem gültigen virtuellen Computer vorhanden ist und verwendet werden kann.

## <a name="scenario"></a>Szenario

Dieses Szenario verwendet die IP-Datenflussüberprüfung, um zu überprüfen, ob ein virtueller Computer über den Port 443 mit einem anderen Computer kommunizieren kann. Wenn der Datenverkehr abgelehnt wird, wird die Sicherheitsregel zurückgegeben, die den jeweiligen Datenverkehr verweigert. Weitere Informationen zur IP-Datenflussüberprüfung finden Sie in der [Übersicht zur IP-Datenflussüberprüfung](network-watcher-ip-flow-verify-overview.md).

### <a name="run-ip-flow-verify"></a>Ausführen der IP-Datenflussüberprüfung

Navigieren Sie zu Network Watcher, und klicken Sie auf **Überprüfen des IP-Flusses**. Wählen Sie den virtuellen Computer und die Netzwerkschnittstelle aus, über die Sie Datenverkehr überprüfen möchten. Geben Sie ggf. zusätzliche Filterinformationen ein, und klicken Sie auf **Überprüfen**.

Nach dem Klicken auf **Überprüfen** wird der Datenfluss basierend auf Ihren angegebenen Kriterien überprüft. Das Ergebnis ist entweder **Zugriff gewährt** oder **Zugriff verweigert**. Bei Verweigerung des Zugriffs werden eine Netzwerksicherheitsgruppe (NSG) und eine Sicherheitsregel, die Datenverkehr blockieren, bereitgestellt. Wenn die Verweigerung von Datenverkehr erwartet wird, war die Regel erfolgreich.

> [!NOTE]
> Für die IP-Datenflussüberprüfung ist es erforderlich, dass die VM-Ressource zugeordnet ist.

Wie Sie in der folgenden Abbildung sehen können, wurde der ausgehende HTTPS-Datenverkehr zugelassen.

![Übersicht zu IP-Datenflussüberprüfung][1]

Wie in der folgenden Abbildung gezeigt, wird der Datenverkehr in eingehend geändert und der eingehende Port in 123. Datenverkehr wird nun verweigert, die Meldung „Zugriff verweigert“ wird zusammen mit der Netzwerksicherheitsgruppe und der Sicherheitsregel bereitgestellt, die den Datenverkehr verweigern.

![IP-Datenflussergebnisse][2]

## <a name="next-steps"></a>Nächste Schritte

Wenn Datenverkehr fälschlicherweise blockiert wird, finden Sie unter [Verwalten von Netzwerksicherheitsgruppen](../virtual-network/virtual-network-manage-nsg-arm-portal.md) Informationen zum Ermitteln der Netzwerksicherheitsgruppe und der definierten Sicherheitsregeln.

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png














