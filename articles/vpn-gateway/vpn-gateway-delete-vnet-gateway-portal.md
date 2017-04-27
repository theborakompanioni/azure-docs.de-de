---
title: "Löschen eines Gateways des virtuellen Netzwerks: Azure-Portal: Resource Manager | Microsoft-Dokumentation"
description: "Löschen Sie ein Gateway des virtuellen Netzwerks mit PowerShell im Resource Manager-Bereitstellungsmodell."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/29/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: b4949db62ccd31cf6ce3d19df5459367cac99b59
ms.lasthandoff: 03/31/2017


---
# <a name="delete-a-virtual-network-gateway-using-the-portal"></a>Löschen eines Gateways für virtuelle Netzwerke über das Portal
> [!div class="op_single_selector"]
> * [Resource Manager – Azure-Portal](vpn-gateway-delete-vnet-gateway-portal.md)
> * [Resource Manager – PowerShell](vpn-gateway-delete-vnet-gateway-powershell.md)
> * [Klassisch – PowerShell](vpn-gateway-delete-vnet-gateway-classic-powershell.md)
>
>

Sie können zwischen ein paar unterschiedlichen Ansätzen wählen, wenn Sie ein Gateway des virtuellen Netzwerks für eine VPN-Gatewaykonfiguration löschen möchten.

- Wenn Sie alles löschen und von Neuem anfangen möchten, wie im Fall einer Testumgebung, können Sie die Ressourcengruppe löschen. Wenn Sie eine Ressourcengruppe löschen, werden alle Ressourcen innerhalb der Gruppe gelöscht. Diese Vorgehensweise wird nur empfohlen, wenn Sie keine der Ressourcen aus der Ressourcengruppe beibehalten möchten. Sie können mit diesem Ansatz nicht selektiv nur ein paar Ressourcen löschen.

- Wenn Sie einige der Ressourcen aus der Ressourcengruppe beibehalten möchten, ist das Löschen eines Gateways des virtuellen Netzwerks etwas komplizierter. Bevor Sie das Gateway des virtuellen Netzwerks löschen können, müssen Sie zuerst alle Ressourcen löschen, die von dem Gateway abhängig sind. Welche Schritte Sie ausführen, hängt vom Typ der Verbindungen ab, die Sie erstellt haben, und den abhängigen Ressourcen für jede Verbindung.

##<a name="deletegw"></a>Löschen eines VPN Gateway

Um ein Gateway für virtuelle Netzwerke zu löschen, müssen Sie zunächst jede Ressource löschen, die zu dem Gateway für virtuelle Netzwerke gehört. Ressourcen müssen aufgrund von Abhängigkeiten in einer bestimmten Reihenfolge gelöscht werden.

###<a name="step-1-navigate-to-the-virtual-network-gateway"></a>Schritt 1: Navigieren zum Gateway für virtuelle Netzwerke

Klicken Sie im [Azure-Portal](https://portal.azure.com) in **Alle Ressourcen** auf das zu löschende Gateway für virtuelle Netzwerke. Die Grafik für ein Gateway sieht wie folgt aus:

![Suchen des Gateways für virtuelle Netzwerke](./media/vpn-gateway-delete-vnet-gateway-portal/gw.png)

###<a name="step-2-delete-connections"></a>Schritt 2: Löschen von Verbindungen

1. Klicken Sie auf dem Blatt für das Gateway für virtuelle Netzwerke auf **Verbindungen**, um alle Verbindungen mit dem Gateway anzuzeigen.
2. Klicken Sie auf **...** in der Zeile mit dem Namen der Verbindung, und wählen Sie in der Dropdownliste die Option **Löschen** aus.
3. Klicken Sie auf **Ja**, um zu bestätigen, dass Sie die Verbindung löschen möchten. Wenn Sie über mehrere Verbindungen verfügen, löschen Sie jede Verbindung.

###<a name="step-3-delete-the-local-network-gateways"></a>Schritt 3: Löschen der lokalen Netzwerkgateways
1. Suchen Sie in **Alle Ressourcen** nach den lokalen Netzwerkgateways, die den einzelnen Verbindungen zugeordnet wurden. Die Grafik für ein lokales Netzwerkgateway sieht wie folgt aus:

    ![Suchen des lokalen Netzwerkgateways](./media/vpn-gateway-delete-vnet-gateway-portal/lng.png)
2. Klicken Sie auf dem Blatt **Übersicht** für das lokale Netzwerkgateway auf **Löschen**.

###<a name="step-4-delete-the-virtual-network-gateway"></a>Schritt 4: Löschen des Gateways für virtuelle Netzwerke
1. Suchen Sie in **Alle Ressourcen** nach dem zu löschenden Gateway für virtuelle Netzwerke.
2. Klicken Sie auf dem Blatt **Übersicht** auf **Löschen**, um das Gateway zu löschen.

>[!NOTE]
> Wenn Sie neben der S2S-Konfiguration noch über eine P2S-Konfiguration für dieses VNET verfügen, werden automatisch alle P2S-Clients ohne Warnung getrennt, wenn Sie das Gateway des virtuellen Netzwerks löschen.
>
>

###<a name="step-5-delete-the-public-ip-address-for-the-gateway"></a>Schritt 5: Löschen der öffentlichen IP-Adresse für das Gateway

1. Suchen Sie in **Alle Ressourcen** nach der öffentlichen IP-Adresse, die dem Gateway zugewiesen wurde. Wenn das Gateway des virtuellen Netzwerks aktiv/aktiv war, sehen Sie zwei öffentliche IP-Adressen. Die Grafik für die öffentliche IP-Adresse sieht wie folgt aus:

    ![Öffentliche IP-Adresse](./media/vpn-gateway-delete-vnet-gateway-portal/pip.png)

2. Klicken Sie auf der Seite **Übersicht** für die öffentliche IP-Adresse auf **Löschen** und zum Bestätigen auf **Ja**.

###<a name="step-6-delete-the-gateway-subnet"></a>Schritt 6: Löschen des Gatewaysubnetzes

1. Suchen Sie in **Alle Ressourcen** nach dem virtuellen Netzwerk. 
2. Klicken Sie auf dem Blatt **Subnetze** auf das **GatewaySubnet**, und klicken Sie anschließend auf **Löschen**. 
3. Klicken Sie auf **Ja**, um zu bestätigen, dass Sie das Gatewaysubnetz löschen möchten.

##<a name="deleterg"></a>Löschen eines VPN-Gateways durch Löschen der Ressourcengruppe

Wenn Sie Ihre bestehenden Ressourcen in der Ressourcengruppe nicht behalten müssen und neu beginnen möchten, können Sie eine gesamte Ressourcengruppe löschen. Sie lernen hier eine schnelle Möglichkeit kennen, alles zu entfernen. Die folgenden Schritte gelten für das Resource Manager-Bereitstellungsmodell.

1. Suchen Sie in **Alle Ressourcen** nach der Ressourcengruppe, und klicken Sie darauf, um das Blatt zu öffnen.
2. Klicken Sie auf **Löschen**. Auf dem Blatt „Löschen“ werden die betroffenen Ressourcen angezeigt. Vergewissern Sie sich, dass alle diese Ressourcen gelöscht werden sollen. Führen Sie andernfalls die Schritte unter [Löschen eines VPN Gateway](#deletegw) am Anfang dieses Artikels aus.
3. Geben Sie zum Fortfahren den Namen der zu löschenden Ressourcengruppe ein, und klicken Sie auf **Löschen**.
