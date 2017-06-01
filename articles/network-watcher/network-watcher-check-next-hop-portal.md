---
title: "Suchen des nächsten Hops mit dem Azure Network Watcher-Feature „Nächster Hop“ – Azure-Portal | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie mithilfe des Azure-Portals den Typ und die IP-Adresse des nächsten Hops feststellen können."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 7b459dcf-4077-424e-a774-f7bfa34c5975
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: 5434b7972346821985c459fc4620805adb88676b
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017


---

# <a name="find-out-what-the-next-hop-type-is-using-the-next-hop-capability-in-azure-network-watcher-using-the-portal"></a>Herausfinden des Typs des nächsten Hops über das Azure-Portal mithilfe der Funktion „Nächster Hop“ in Azure Network Watcher

> [!div class="op_single_selector"]
> - [Azure-Portal](network-watcher-check-next-hop-portal.md)
> - [PowerShell](network-watcher-check-next-hop-powershell.md)
> - [CLI 1.0](network-watcher-check-next-hop-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-next-hop-cli.md)
> - [Azure-REST-API](network-watcher-check-next-hop-rest.md)

„Nächster Hop“ ist ein Feature von Network Watcher, das Ihnen die Möglichkeit bietet, den Typ und die IP-Adresse des nächsten Hops basierend auf einem angegebenen virtuellen Computer abzurufen. Mithilfe dieses Features können Sie ermitteln, ob der von einem virtuellen Computer ausgehende Datenverkehr ein Gateway, das Internet oder virtuelle Netzwerke durchquert, um zum Ziel zu gelangen.

## <a name="before-you-begin"></a>Voraussetzungen

Dieses Szenario setzt voraus, dass Sie die Schritte unter [Erstellen einer Network Watcher-Instanz](network-watcher-create.md) bereits durchgeführt haben, um eine Network Watcher-Instanz zu erstellen. Ferner wird davon ausgegangen, dass eine Ressourcengruppe mit einem gültigen virtuellen Computer vorhanden ist und verwendet werden kann.

## <a name="scenario"></a>Szenario

In dem in diesem Artikel beschriebenen Szenario wird „Nächster Hop“ verwendet, um Typ und IP-Adresse des nächsten Hops für eine Ressource zu ermitteln. Weitere Informationen zu „Nächster Hop“ finden Sie unter [Übersicht über „Nächster Hop“](network-watcher-next-hop-overview.md).

In diesem Szenario führen Sie Folgendes durch:

* Abrufen des nächsten Hops von einem virtuellen Computer aus.

## <a name="get-next-hop"></a>Abrufen des nächsten Hops

### <a name="step-1"></a>Schritt 1

Navigieren Sie im Azure-Portal zu Ihrer Network Watcher-Ressource.

### <a name="step-2"></a>Schritt 2

Klicken Sie im Navigationsbereich auf **Nächster Hop**, wählen Sie den virtuellen Computer und eine Netzwerkschnittstelle aus, füllen Sie Quell- und Ziel-IP aus, und klicken Sie auf die Schaltfläche **Nächster Hop**.

> [!NOTE]
> „Nächster Hop“ setzt voraus, dass die VM-Ressource für die Ausführung zugeordnet wird.

![Übersicht zum Abrufen des nächsten Hops][1]

### <a name="step-3"></a>Schritt 3

Nach Abschluss des Vorgangs werden die Ergebnisse bereitgestellt. Die IP-Adresse und der Typ des Geräts, das der nächste Hop ist, werden angezeigt. Die folgende Tabelle zeigt die verfügbaren zurückgegebenen Werte im Portal.

**Typ des nächsten Hops**

* Internet
* VirtualAppliance
* VirtualNetworkGateway
* VnetLocal
* HyperNetGateway
* VnetPeering
* Keine

Wenn eine benutzerdefinierte Route zum Weiterleiten dieses Datenverkehrs verwendet wurde, wird die benutzerdefinierte Route (User-Defined Route, UDR) auch mit den Ergebnissen angezeigt.

![Abrufen der Ergebnisse des nächsten Hops][2]

## <a name="next-steps"></a>Nächste Schritte

Informationen zur programmgesteuerten Überprüfung der Einstellungen Ihrer Netzwerksicherheitsgruppe finden Sie unter [NSG-Überwachung mit Network Watcher](network-watcher-nsg-auditing-powershell.md).

[1]: ./media/network-watcher-check-next-hop-portal/figure1.png
[2]: ./media/network-watcher-check-next-hop-portal/figure2.png















