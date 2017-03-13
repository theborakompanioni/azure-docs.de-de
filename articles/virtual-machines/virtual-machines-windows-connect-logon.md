---
title: Herstellen einer Verbindung mit einem virtuellen Windows Server-Computer | Microsoft Docs
description: Hier erfahren Sie, wie Sie unter Verwendung des Azure-Portals und des Resource Manager-Bereitstellungsmodells eine Verbindung mit einem virtuellen Windows-Computer herstellen und sich bei diesem Computer anmelden.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: ef62b02e-bf35-468d-b4c3-71b63fe7f409
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/01/2017
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 00f6b2e60c20eb27771d9d54df63f930ee88a55a
ms.openlocfilehash: 7427c8126ab73a851bc696d4925366b3b714616d
ms.lasthandoff: 03/02/2017


---
# <a name="how-to-connect-and-log-on-to-an-azure-virtual-machine-running-windows"></a>Gewusst wie: Herstellen einer Verbindung mit einem virtuellen Azure-Computer unter Windows und Anmelden auf diesem Computer
Verwenden Sie die Schaltfläche **Verbinden** im Azure-Portal, um eine Remotedesktopsitzung (RDP) von einem Windows-Desktop zu starten. Zuerst stellen Sie eine Verbindung mit dem virtuellen Computer her, und dann melden Sie sich an.

Wenn Sie von einem Mac eine Verbindung mit einem virtuellen Windows-Computer herstellen möchten, müssen Sie einen RDP-Client für Mac installieren, etwa [Microsoft-Remotedesktop](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417).

## <a name="connect-to-the-virtual-machine"></a>Herstellen einer Verbindung mit dem virtuellen Computer
1. Melden Sie sich am [Azure-Portal](https://portal.azure.com/)an, falls Sie dies noch nicht getan haben.
2. Klicken Sie im Hub-Menü auf **Virtuelle Computer**.
3. Wählen Sie den gewünschten virtuellen Computer aus der Liste aus.
4. Klicken Sie auf dem Blatt für den virtuellen Computer auf **Verbinden**.
   
    ![Screenshot des Azure-Portals beim Herstellen einer Verbindung mit Ihrem virtuellen Computer](./media/virtual-machines-windows-connect-logon/connect.png)
   
   > [!TIP]
   > Wenn die Schaltfläche **Verbinden** im Portal ausgeblendet ist und keine [ExpressRoute](../expressroute/expressroute-introduction.md)- oder [Site-to-Site-VPN-Verbindung](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) mit Azure besteht, müssen Sie eine öffentliche IP-Adresse erstellen und dem virtuellen Computer zuweisen, damit Sie RDP nutzen können. Lesen Sie mehr über [öffentliche IP-Adressen in Azure](../virtual-network/virtual-network-ip-addresses-overview-arm.md).
   > 
   > 

## <a name="log-on-to-the-virtual-machine"></a>Melden Sie sich beim virtuellen Computer an.
[!INCLUDE [virtual-machines-log-on-win-server](../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a>Nächste Schritte
Falls beim Herstellen der Verbindung Probleme auftreten, helfen Ihnen die Informationen unter [Problembehandlung bei Remotedesktopverbindungen](virtual-machines-windows-troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)weiter. Dieser Artikel führt Sie durch die Diagnose und Behebung von häufigen Problemen.


