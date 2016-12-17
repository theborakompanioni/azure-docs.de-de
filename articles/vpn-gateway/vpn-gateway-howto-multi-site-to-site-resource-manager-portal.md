---
title: "Hinzufügen mehrerer Site-to-Site Verbindungen (S2S) mit VPN Gateway zu einem virtuellen Netzwerk für das Resource Manager-Bereitstellungsmodell mithilfe des Azure-Portals | Microsoft Docs"
description: "Fügen Sie mehrere S2S-Verbindungen zu einem VPN-Gateway hinzu, für das bereits eine Verbindung besteht"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: f3e8b165-f20a-42ab-afbb-bf60974bb4b1
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 9906602e0a154c2dc3d6e458179c7e9346df2852


---
# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a>Hinzufügen einer Site-to-Site-Verbindung (S2S) zu einem VNet mit einer vorhandenen VPN-Gatewayverbindung
> [!div class="op_single_selector"]
> * [Resource Manager – Portal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
> * [Klassisch – PowerShell](vpn-gateway-multi-site.md)
> 
> 

In diesem Artikel wird beschrieben, wie Sie mithilfe des Azure-Portals Site-to-Site-Verbindungen (S2S) zu einem VPN-Gateway hinzufügen, für das bereits eine Verbindung besteht. Diese Art der Verbindung wird häufig als Multi-Site-Konfiguration bezeichnet. 

Sie können mithilfe dieses Artikels eine S2S-Verbindung zu einem VNet hinzufügen, für das bereits eine S2S-Verbindung, Punkt-zu-Standort-Verbindung oder VNet-zu-VNet-Verbindung besteht. Beim Hinzufügen von Verbindungen gibt es einige Einschränkungen. Überprüfen Sie den Abschnitt [Voraussetzungen](#before) in diesem Artikel, bevor Sie die Konfiguration beginnen. 

Dieser Artikel bezieht sich auf VNets, die mithilfe des Azure Resource Manager-Bereitstellungsmodells erstellt wurden, das über ein VPN-Gateway als RouteBased verfügt. Diese Schritte gelten nicht für Konfigurationen von parallel bestehenden ExpressRoute- und S2S-Verbindungen. Informationen zu parallel bestehenden Verbindungen finden Sie unter [Konfigurieren von parallel bestehenden ExpressRoute- und Standort-zu-Standort-Verbindungen für das klassische Bereitstellungsmodell](../expressroute/expressroute-howto-coexist-resource-manager.md).

### <a name="deployment-models-and-methods"></a>Bereitstellungsmodelle und -methoden
[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Die folgende Tabelle wird aktualisiert, wenn neue Artikel und weitere Tools für diese Konfiguration verfügbar werden. Wenn ein Artikel verfügbar ist, fügen wir in der Tabelle einen direkten Link dazu ein.

[!INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)]

## <a name="a-namebeforeabefore-you-begin"></a><a name="before"></a>Voraussetzungen
Überprüfen Sie folgende Maßnahmen:

* Sie erstellen keine parallel bestehende ExpressRoute/S2S-Verbindung.
* Sie verfügen über ein virtuelles Netzwerk, das mithilfe des Resource Manager-Bereitstellungsmodells mit einer bestehenden Verbindung erstellt wurde.
* Das Gateway für virtuelle Netzwerke für Ihr VNet ist RouteBased. Wenn Sie über ein PolicyBased-VPN Gateway verfügen, müssen Sie das Gateway für virtuelle Netzwerke löschen, und ein neues VPN Gateway als RouteBased erstellen.
* Keine der Adressbereiche überlappen sich mit einem der VNets, mit der dieses VNet eine Verbindung herstellt.
* Sie haben ein kompatibles VPN-Gerät (und eine Person, die es konfigurieren kann). Weitere Informationen finden Sie unter [Informationen zu VPN-Geräten](vpn-gateway-about-vpn-devices.md). Wenn Sie sich mit dem Konfigurieren des VPN-Geräts oder mit den IP-Adressbereichen Ihrer lokalen Netzwerkkonfiguration nicht auskennen, müssen Sie sich an eine Person wenden, die Ihnen diese Details liefern kann.
* Sie haben eine externe öffentliche IP-Adresse für Ihr VPN-Gerät. Diese IP-Adresse darf sich nicht hinter einer NAT befinden.

## <a name="a-namepart1apart-1---configure-a-connection"></a><a name="part1"></a>Teil 1 – Konfigurieren einer Verbindung
1. Navigieren Sie in einem Browser zum [Azure-Portal](http://portal.azure.com) , und melden Sie sich, falls erforderlich, mit Ihrem Azure-Konto an.
2. Klicken Sie auf **Alle Ressourcen**, suchen Sie Ihr **Gateway für virtuelle Netzwerke** in der Liste der Ressourcen, und klicken Sie darauf.
3. Klicken Sie auf dem Blatt **Gateway für virtuelle Netzwerke** auf **Verbindungen**.
   
    ![Blatt „Verbindungen“](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connectionsblade.png "Connections blade")<br>
4. Klicken Sie auf dem Blatt **Verbindungen** auf **+Hinzufügen**.
   
    ![Hinzufügen der Schaltfläche „Verbindung“](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addbutton.png "Add connection button")<br>
5. Füllen Sie auf dem Blatt **Verbindung hinzufügen** folgende Felder aus:
   
   * **Name:** Der Name, den Sie dem Standort zuweisen möchten, zu dem Sie die Verbindung herstellen.
   * **Verbindungstyp:** Wählen Sie **Site-to-Site (IPSec)** aus.
     
     ![Blatt Verbindung hinzufügen](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addconnectionblade.png "Add connection blade")<br>

## <a name="a-namepart2apart-2---add-a-local-network-gateway"></a><a name="part2"></a>Teil 2 – Lokales Netzwerkgateway hinzufügen
1. Klicken Sie auf **Lokales Netzwerkgateway** ***Ein lokales Netzwerkgateway auswählen***. Das Blatt **Lokales Netzwerkgateway auswählen** wird geöffnet.
   
    ![Auswählen eines lokalen Netzwerkgateways](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/chooselng.png "Choose local network gateway")<br>
2. Klicken Sie auf **Neu erstellen**, um das Blatt **Lokales Netzwerkgateway erstellen** zu öffnen.
   
    ![Blatt Erstellen eines lokalen Netzwerkgateways](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/createlngblade.png "Create local network gateway")<br>
3. Füllen Sie auf dem Blatt **Erstellen eines lokalen Netzwerkgateways** die folgenden Felder aus:
   
   * **Name:** Der Name, den Sie der lokalen Netzwerkgateway-Ressource geben möchten.
   * **IP-Adresse:** Die öffentliche IP-Adresse des VPN-Geräts auf dem Standort, mit dem Sie eine Verbindung herstellen möchten.
   * **Adressraum:** Der Adressraum, der an den Standort des lokalen Netzwerks weitergeleitet werden soll.
4. Klicken Sie auf **OK** auf dem Blatt **Lokales Netzwerkgateway erstellen**, um die Änderungen zu speichern.

## <a name="a-namepart3apart-3---add-the-shared-key-and-create-the-connection"></a><a name="part3"></a>Teil 3 – Hinzufügen des gemeinsam verwendeten Schlüssels, und erstellen der Verbindung
1. Fügen Sie auf dem Blatt **Verbindung hinzufügen** den gemeinsam verwendeten Schlüssel hinzu, den Sie verwenden möchten, um Ihre Verbindung zu erstellen. Sie können den gemeinsam verwendeten Schlüssel entweder über Ihr VPN-Gerät erhalten, oder hier einen Schüssel erstellen, und Ihr VPN-Gerät dann so konfigurieren, dass es den gleichen gemeinsam verwendeten Schlüssel verwendet. Wichtig ist, dass die Schlüssel genau übereinstimmen.
   
    ![Gemeinsam verwendeter Schlüssel](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/sharedkey.png "Shared key")<br>
2. Klicken Sie unten auf dem Blatt auf **OK**, um die Verbindung zu erstellen.

## <a name="a-namepart4apart-4---verify-the-vpn-connection"></a><a name="part4"></a>Teil 4 – Überprüfen der VPN-Verbindung
Sie können Ihre VPN-Verbindung entweder im Portal oder mithilfe von PowerShell überprüfen.

[!INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="next-steps"></a>Nächste Schritte
* Sobald die Verbindung hergestellt ist, können Sie Ihren virtuellen Netzwerken virtuelle Computer hinzufügen. Weitere Informationen finden Sie unter dem [Lernpfad](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) für virtuelle Computer.




<!--HONumber=Nov16_HO3-->


