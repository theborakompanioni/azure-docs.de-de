---
title: "Erstellen und Ändern einer ExpressRoute-Verbindung mit Resource Manager und dem Azure-Portal | Microsoft Docs"
description: "Dieser Artikel beschreibt das Erstellen, Bereitstellen, Überprüfen, Aktualisieren, Löschen und Aufheben der Bereitstellung einer ExpressRoute-Verbindung."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 68d59d59-ed4d-482f-9cbc-534ebb090613
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: e7931f1b08d09fbe1fa5a5a2d4a11da01e736462
ms.openlocfilehash: 8eb67e0800671f4d41612780db52a243023b9e5a


---
# <a name="create-and-modify-an-expressroute-circuit"></a>Erstellen und Ändern einer ExpressRoute-Verbindung
> [!div class="op_single_selector"]
> * [Azure-Portal – Resource Manager](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell – Resource Manager](expressroute-howto-circuit-arm.md)
> * [PowerShell – klassisch](expressroute-howto-circuit-classic.md)
> 
> 

In diesem Artikel wird beschrieben, wie Sie eine Azure ExpressRoute-Verbindung mithilfe des Azure-Portals und des Azure Resource Manager-Bereitstellungsmodells erstellen. In den folgenden Schritten wird auch veranschaulicht, wie Sie den Status der Verbindung prüfen, aktualisieren, löschen oder deren Bereitstellung aufheben.

**Informationen zu Azure-Bereitstellungsmodellen**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a>Voraussetzungen
* Lesen Sie vor Beginn der Konfiguration die Seiten zu den [Voraussetzungen](expressroute-prerequisites.md) und [Workflows](expressroute-workflows.md).
* Stellen Sie sicher, dass Sie auf das [Azure-Portal](https://portal.azure.com)zugreifen können.
* Stellen Sie sicher, dass Sie über die notwendigen Berechtigungen verfügen, um neue Netzwerkressourcen zu erstellen. Wenden Sie sich an Ihren Kontoadministrator, wenn Sie nicht über die richtigen Berechtigungen verfügen.

## <a name="create-and-provision-an-expressroute-circuit"></a>Erstellen und Bereitstellen einer ExpressRoute-Verbindung
### <a name="1-sign-in-to-the-azure-portal"></a>1. Melden Sie sich auf dem Azure-Portal an.
Navigieren Sie in einem Browser zum [Azure-Portal](http://portal.azure.com) , und melden Sie sich mit Ihrem Azure-Konto an.

### <a name="2-create-a-new-expressroute-circuit"></a>2. Erstellen Sie eine neue ExpressRoute-Verbindung.
> [!IMPORTANT]
> Ihre ExpressRoute-Verbindung wird von dem Moment an berechnet, in dem ein Dienstschlüssel ausgegeben wird. Stellen Sie sicher, dass Sie diesen Vorgang ausführen, sobald der Konnektivitätsanbieter dazu bereit ist, die Verbindung bereitzustellen.
> 
> 

1. Sie können eine ExpressRoute-Verbindung erstellen, indem Sie die Option zum Erstellen einer neuen Ressource auswählen. Klicken Sie auf **Neu** > **Netzwerk** > **ExpressRoute**, wie in der Abbildung unten gezeigt:
   
    ![Erstellen Sie eine ExpressRoute-Verbindung.](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit1.png)
2. Nachdem Sie auf **ExpressRoute** geklickt haben, wird das Blatt **ExpressRoute-Verbindung erstellen** angezeigt. Wenn Sie auf diesem Blatt Werte eintragen, stellen Sie sicher, dass Sie den richtigen SKU-Tarif und die richtige Datenabrechnung angeben.
   
   * Der **Tarif** bestimmt, ob ein ExpressRoute Standard- oder ein ExpressRoute Premium-Add-On aktiviert wird. Sie können **Standard** für die Standard-SKU bzw. **Premium** für das Premium-Add-On angeben.
   * **Datenabrechnung** bestimmt den Abrechnungstyp. Sie können **Taktung** für einen Volumentarif und **Unbegrenzt** für einen Tarif mit Datenflatrate auswählen. Beachten Sie, dass Sie den Abrechnungstyp von **Taktung** in **Unbegrenzt** ändern können, nicht jedoch umgekehrt (**Unbegrenzt** in **Taktung**).
     
     ![Konfigurieren von SKU-Tarif und Datenmessung](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit2.png)

> [!IMPORTANT]
> Beachten Sie, dass der Peeringstandort dem [physischen Standort](expressroute-locations.md) entspricht, an dem Ihr Peering mit Microsoft stattfindet. Dieser ist **nicht** mit der Eigenschaft „Standort“ verknüpft, die sich auf den geografischen Standort des Azure-Netzwerkressourcenanbieters befindet. Obgleich sie nicht miteinander in Zusammenhang stehen, sollten Sie einen Netzwerkressourcenanbieter in geografischer Nähe des Peeringstandorts der Verbindung wählen. 
> 
> 

### <a name="3-view-the-circuits-and-properties"></a>3. Anzeigen von Verbindungen und Eigenschaften
**Anzeigen aller Verbindungen**

Sie können alle erstellten Verbindungen anzeigen, indem Sie im Menü auf der linken Seite **Alle Ressourcen** auswählen.

![Verbindungen anzeigen](./media/expressroute-howto-circuit-portal-resource-manager/listresource.png)

**Anzeigen der Eigenschaften**

    You can view the properties of the circuit by selecting it. On this blade, note the service key for the circuit. You must copy the circuit key for your circuit and pass it down to the service provider to complete the provisioning process. The circuit key is specific to your circuit.

![Eigenschaften anzeigen](./media/expressroute-howto-circuit-portal-resource-manager/listproperties1.png)

### <a name="4-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>4. Senden Sie den Dienstschlüssel zur Bereitstellung an Ihren Konnektivitätsanbieter.
Auf diesem Blatt bietet der **Anbieterstatus** Informationen zum aktuellen Zustand der Bereitstellung auf Dienstanbieterseite. **Schaltkreisstatus** wird der Zustand auf Microsoft-Seite angegeben. Weitere Informationen zu den Bereitstellungszuständen für eine Verbindung finden Sie im Artikel [Workflows](expressroute-workflows.md#expressroute-circuit-provisioning-states) .

Wenn Sie eine neue ExpressRoute-Verbindung erstellen, weist die Verbindung folgenden Zustand auf:

Anbieterstatus: Nicht bereitgestellt<BR>
 Schaltkreisstatus: Aktiviert

![Bereitstellung initiieren](./media/expressroute-howto-circuit-portal-resource-manager/viewstatus.png)

Die Verbindung wechselt in den folgenden Zustand, wenn sie vom Konnektivitätsanbieter für Sie aktiviert wird:

Anbieterstatus: Wird bereitgestellt<BR>
 Schaltkreisstatus: Aktiviert

Damit Sie eine ExpressRoute-Verbindung verwenden können, muss sie sich im folgenden Zustand befinden:

Anbieterstatus: Bereitgestellt<BR>
 Schaltkreisstatus: Aktiviert

### <a name="5-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>5. Überprüfen Sie regelmäßig den Status und Zustand des Verbindungsschlüssels.
Sie können die Eigenschaften der gewünschten Verbindung anzeigen, indem Sie die Verbindung auswählen. Überprüfen Sie den **Anbieterstatus**, und stellen Sie sicher, dass er in **Provisioned** geändert wurde, bevor Sie fortfahren.

![Verbindungs- und Anbieterstatus](./media/expressroute-howto-circuit-portal-resource-manager/viewstatusprovisioned.png)

### <a name="6-create-your-routing-configuration"></a>6. Erstellen Sie die Routingkonfiguration.
Eine Schritt-für-Schritt-Anleitung zum Erstellen und Ändern von Verbindungspeerings finden Sie im Artikel [Routingkonfiguration für ExpressRoute-Verbindung](expressroute-howto-routing-portal-resource-manager.md) .

> [!IMPORTANT]
> Diese Anweisungen gelten nur für Verbindungen, die über Dienstanbieter erstellt wurden, von denen Layer 2-Konnektivitätsdienste angeboten werden. Wenn Sie einen Dienstanbieter nutzen, der verwaltete Layer 3-Dienste anbietet (meist ein IP VPN, z.B. MPLS), übernimmt Ihr Konnektivitätsanbieter die Konfiguration und Verwaltung des Routings für Sie.
> 
> 

### <a name="7-link-a-virtual-network-to-an-expressroute-circuit"></a>7. Verknüpfen eines virtuellen Netzwerks mit einer ExpressRoute-Verbindung
Verknüpfen Sie anschließend ein virtuelles Netzwerk mit Ihrer ExpressRoute-Verbindung. Lesen Sie den Artikel [Verknüpfen von virtuellen Netzwerken mit ExpressRoute-Verbindungen](expressroute-howto-linkvnet-arm.md) , wenn Sie mit dem Resource Manager-Bereitstellungsmodell arbeiten.

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Abrufen des Status einer ExpressRoute-Verbindung
Sie können den Status einer Verbindung anzeigen, indem Sie die Verbindung auswählen. 

![Status einer ExpressRoute-Verbindung](./media/expressroute-howto-circuit-portal-resource-manager/listproperties1.png)

## <a name="modifying-an-expressroute-circuit"></a>Ändern einer ExpressRoute-Verbindung
Sie können bestimmte Eigenschaften einer ExpressRoute-Verbindung ändern, ohne die Konnektivität zu beeinträchtigen. Zurzeit können Sie Eigenschaften von ExpressRoute-Verbindungen nicht über das Azure-Portal ändern. Sie können jedoch PowerShell verwenden, um Verbindungseigenschaften zu bearbeiten. Informationen dazu finden Sie im Abschnitt [Ändern einer ExpressRoute-Verbindung mit PowerShell](expressroute-howto-circuit-arm.md#modify).

Sie können folgende Aktionen ausführen, ohne Ausfallzeiten zu verursachen:

* Aktivieren oder deaktivieren Sie ein ExpressRoute Premium-Add-On für Ihre ExpressRoute-Verbindung.
* Erhöhen Sie die Bandbreite Ihrer ExpressRoute-Verbindung. Beachten Sie, dass ein Downgrade der Bandbreite einer Verbindung nicht unterstützt wird. 
* Sie können den Abrechnungsplan von „Volumentarif“ zu „Datenflatrate“ ändern. Beachten Sie, dass eine Änderung des Abrechnungsplans von „Datenflatrate“ zu „Volumentarif“ nicht unterstützt wird.
* Sie können die Option **Klassische Vorgänge zulassen**aktivieren und deaktivieren.

Weitere Informationen zu Beschränkungen und Grenzwerten finden Sie unter [ExpressRoute – Häufig gestellte Fragen](expressroute-faqs.md).

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Aufheben der Bereitstellung und Löschen einer ExpressRoute-Verbindung
Sie können Ihre ExpressRoute-Verbindung löschen. Wählen Sie dazu das Symbol **Löschen** aus. Beachten Sie Folgendes:

* Sie müssen die Verknüpfung aller virtuellen Netzwerke mit der ExpressRoute-Verbindung aufheben. Falls dieser Vorgang nicht erfolgreich ist, überprüfen Sie, ob noch virtuelle Netzwerke mit der Verbindung verknüpft sind.
* Wenn der Bereitstellungsstatus des ExpressRoute-Verbindungsdienstanbieters **Bereitstellung** oder **Bereitgestellt** lautet, arbeiten Sie mit Ihrem Dienstanbieter zusammen, um die Verbindungsbereitstellung auf Anbieterseite aufzuheben. Microsoft reserviert weiterhin Ressourcen für Sie und stellt Ihnen dies in Rechnung, bis der Dienstanbieter die Aufhebung der Verbindungsbereitstellung abgeschlossen hat und uns benachrichtigt.
* Wenn der Dienstanbieter die Bereitstellung der Verbindung aufgehoben hat (Bereitstellungsstatus des Dienstanbieters lautet **Nicht bereitgestellt**), können Sie die Verbindung löschen. Damit wird die Abrechnung für die Verbindung beendet.

## <a name="next-steps"></a>Nächste Schritte
Führen Sie nach dem Erstellen Ihrer Verbindung folgende Vorgänge aus:

* [Erstellen und Ändern des Routings für Ihre ExpressRoute-Verbindung](expressroute-howto-routing-portal-resource-manager.md)
* [Verknüpfen Ihres virtuelles Netzwerks mit Ihrer ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md)




<!--HONumber=Nov16_HO3-->


