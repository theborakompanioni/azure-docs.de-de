---
title: "Verknüpfen eines virtuellen Netzwerks mit einer ExpressRoute-Verbindung: Azure-Portal | Microsoft-Dokumentation"
description: "In diesem Dokument erhalten Sie einen Überblick über das Verknüpfen von virtuellen Netzwerken (VNets) mit ExpressRoute-Verbindungen."
services: expressroute
documentationcenter: na
author: cherylmc
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: f5cb5441-2fba-46d9-99a5-d1d586e7bda4
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/27/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 75b7cee2485d1d68fee8dcd65ade84995dcdb9be
ms.openlocfilehash: aa473abd00fbddb01e50577cea8984c6f3e9a7f3


---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit"></a>Verbinden eines virtuellen Netzwerks mit einer ExpressRoute-Verbindung
> [!div class="op_single_selector"]
> * [Resource Manager – Azure-Portal](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [Resource Manager – PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Klassisch – PowerShell](expressroute-howto-linkvnet-classic.md)
> * [Video – Azure-Portal](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> 
>  

Dieser Artikel unterstützt Sie beim Verknüpfen virtueller Netzwerke (VNets) mit Azure ExpressRoute-Verbindungen über das Resource Manager-Bereitstellungsmodell und das Azure-Portal. Virtuelle Netzwerke können Teil desselben Abonnements sein oder zu einem anderen Abonnement gehören.

**Informationen zu Azure-Bereitstellungsmodellen**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="configuration-prerequisites"></a>Konfigurationsvoraussetzungen
* Stellen Sie sicher, dass Sie vor Beginn der Konfiguration die Seiten [Voraussetzungen](expressroute-prerequisites.md), [Routinganforderungen](expressroute-routing.md) und [Workflows](expressroute-workflows.md) gelesen haben.
* Sie benötigen eine aktive ExpressRoute-Verbindung.
  
  * Führen Sie die Schritte zum [Erstellen einer ExpressRoute-Verbindung](expressroute-howto-circuit-arm.md) aus, und lassen Sie sie vom Konnektivitätsanbieter aktivieren.
  * Stellen Sie sicher, dass privates Azure-Peering für die Verbindung konfiguriert ist. Informationen zum Routing finden Sie unter [Konfigurieren des Routings](expressroute-howto-routing-portal-resource-manager.md) .
  * Vergewissern Sie sich, dass das private Azure-Peering konfiguriert wurde und das BGP-Peering zwischen Ihrem Netzwerk und Microsoft aktiv ist, damit End-to-End-Konnektivität bereitgestellt werden kann.
  * Vergewissern Sie sich, dass ein virtuelles Netzwerk und ein virtuelles Netzwerkgateway erstellt und vollständig bereitgestellt wurden. Folgen Sie der Anleitung, um ein [VPN-Gateway](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) zu erstellen. (Führen Sie nur die Schritte 1 bis 5 aus.)

Sie können bis zu zehn virtuelle Netzwerke mit einer ExpressRoute-Standardverbindung verknüpfen. Alle virtuellen Netzwerke müssen sich in der gleichen geopolitischen Region befinden, wenn Sie eine ExpressRoute-Standardverbindung verwenden. Wenn Sie das ExpressRoute Premium-Add-On aktiviert haben, können Sie virtuelle Netzwerke außerhalb der geopolitischen Region der ExpressRoute-Verbindung oder eine größere Anzahl von virtuellen Netzwerken mit der Express Route-Verbindung verknüpfen. Weitere Informationen zum Premium-Add-On finden Sie in den [häufig gestellten Fragen](expressroute-faqs.md) .

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>Herstellen einer Verbindung zwischen einem virtuellen Netzwerk in demselben Abonnement und einer Verbindung

### <a name="to-create-a-connection"></a>So stellen Sie eine Verbindung her

1. Stellen Sie sicher, dass Ihre ExpressRoute-Verbindung und das private Azure-Peering erfolgreich konfiguriert wurden. Befolgen Sie die Anweisungen unter [Erstellen einer ExpressRoute-Verbindung](expressroute-howto-circuit-arm.md) und [Konfigurieren des Routings](expressroute-howto-routing-arm.md). Ihre ExpressRoute-Verbindung sollte in etwa wie in der folgenden Abbildung dargestellt aussehen:
    ![Screenshot einer ExpressRoute-Verbindung](./media/expressroute-howto-linkvnet-portal-resource-manager/routing1.png)
   
   > [!NOTE]
   > BGP-Konfigurationsinformationen werden nicht angezeigt, wenn Ihre Peerings vom Layer 3-Anbieter konfiguriert wurden. Wenn sich Ihre Verbindung im bereitgestellten Zustand befindet, sollten Sie Verbindungen erstellen können.
   > 
2. Jetzt können Sie damit beginnen, eine Verbindung zum Verknüpfen des virtuellen Netzwerkgateways mit Ihrer ExpressRoute-Verbindung bereitzustellen. Klicken Sie auf **Verbindung** > **Hinzufügen**, um das Blatt **Verbindung hinzufügen** zu öffnen, und konfigurieren Sie dann die Werte. Sehen Sie sich das folgende Referenzbeispiel an:
    ![Screenshot zum Hinzufügen der Verbindung](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub1.png)  

3. Nachdem die Verbindung erfolgreich konfiguriert wurde, zeigt das Verbindungsobjekt die Daten für die Verbindung an.
     ![Screenshot des Verbindungsobjekts](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub2.png)

### <a name="to-delete-a-connection"></a>So löschen Sie eine Verbindung
Sie können eine Verbindung löschen, indem Sie das Symbol **Löschen** auf dem Blatt für Ihre Verbindung auswählen.

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>Herstellen einer Verbindung zwischen einem virtuellen Netzwerk in einem anderen Abonnement und einer Verbindung
Sie können eine ExpressRoute-Verbindung für mehrere Abonnements freigeben. Die folgende Abbildung zeigt eine einfache schematische Darstellung der Freigabe von Lasten für ExpressRoute-Verbindungen für mehrere Abonnements.

Jede der kleineren Clouds innerhalb der großen Cloud stellt Abonnements dar, die zu verschiedenen Abteilungen innerhalb einer Organisation gehören. Jede der Abteilungen innerhalb der Organisation kann ihr eigenes Abonnement zum Bereitstellen von Diensten verwenden, für die Verbindung mit dem lokalen Netzwerk kann jedoch eine einzelne gemeinsam genutzte ExpressRoute-Verbindung verwendet werden. Eine einzelne Abteilung (in diesem Beispiel: IT) kann die ExpressRoute-Verbindung besitzen. Andere Abonnements innerhalb der Organisation können die ExpressRoute-Verbindung nutzen.

> [!NOTE]
> Konnektivitäts- und Bandbreitengebühren für die dedizierte Verbindung werden dem Besitzer der ExpressRoute-Verbindung in Rechnung gestellt. Alle virtuellen Netzwerke verwenden gemeinsam dieselbe Bandbreite.
> 
> 
![Abonnementübergreifende Konnektivität](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration"></a>Verwaltung
Der *Verbindungsbesitzer* ist autorisierter Hauptbenutzer der ExpressRoute-Verbindungsressource. Der Verbindungsbesitzer kann Autorisierungen erstellen, die durch *Verbindungsbenutzer*eingelöst werden können. *Verbindungsbenutzer* sind Besitzer virtueller Netzwerkgateways (die sich nicht in demselben Abonnement wie die ExpressRoute-Verbindung befinden). *Verbindungsbenutzer* können Autorisierungen einlösen (eine Autorisierung für jedes virtuelle Netzwerk).

Der *Verbindungsbesitzer* hat die Möglichkeit, Autorisierungen jederzeit zu ändern und aufzuheben. Das Aufheben einer Autorisierung führt dazu, dass alle Verbindungen aus dem Abonnement gelöscht werden, dessen Zugriff aufgehoben wurde.

### <a name="circuit-owner-operations"></a>Aktionen als Verbindungsbesitzer

**Erstellen einer Autorisierung**

Der Verbindungsbesitzer erstellt eine Autorisierung. Dies führt zur Erstellung eines Autorisierungsschlüssels, der von einem Verbindungsbenutzer verwendet werden kann, um dessen virtuelle Netzwerkgateways mit der ExpressRoute-Verbindung zu verbinden. Eine Autorisierung gilt nur für jeweils eine Verbindung.

1. Klicken Sie auf dem Blatt „ExpressRoute“ auf **Autorisierungen**, geben Sie dann einen **Namen** für die Autorisierung ein, und klicken Sie auf **Speichern**.
![Abonnementübergreifende Konnektivität](./media/expressroute-howto-linkvnet-portal-resource-manager/authorization.png)

2. Sobald die Konfiguration gespeichert ist, kopieren Sie die **Ressourcen-ID** und den **Autorisierungsschlüssel**.
![Abonnementübergreifende Konnektivität](./media/expressroute-howto-linkvnet-portal-resource-manager/authkey.png)

**Löschen von Autorisierungen**

Sie können eine Verbindung löschen, indem Sie das Symbol „Löschen“ auf dem Blatt für Ihre Verbindung auswählen.

### <a name="circuit-user-operations"></a>Aktionen als Verbindungsbenutzer

   > [!NOTE]
   > Der Verbindungsbenutzer benötigt die Ressourcen-ID und einen Autorisierungsschlüssel vom Verbindungsbesitzer. 

**Einlösen von Verbindungsautorisierungen**

1.  Klicken Sie auf die Schaltfläche **+Neu**.
    ![Abonnementübergreifende Konnektivität](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection1.png)

2.  Suchen Sie im Marketplace nach **Verbindung**, wählen Sie sie aus, und klicken Sie auf **Erstellen**.
    ![Abonnementübergreifende Konnektivität](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection2.png)

3.  Stellen Sie sicher, dass für den **Verbindungstyp** „ExpressRoute“ festgelegt ist.

4.  Tragen Sie die Details ein, und klicken Sie auf der Registerkarte „Grundlagen“ auf **OK**.
    ![Abonnementübergreifende Konnektivität](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection3.png)

5.  Wählen Sie in der Registerkarte **Einstellungen** die Option **Gateway des virtuellen Netzwerks**, und aktivieren Sie das Kontrollkästchen **Autorisierung einlösen**.

6.  Geben Sie den **Autorisierungsschlüssel** und **Peerleitungs-URI** ein, und benennen Sie die Verbindung. Klicken Sie auf **OK**.
    ![Abonnementübergreifende Konnektivität](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection4.png)

7.  Überprüfen Sie die Informationen auf der Seite **Zusammenfassung**, und klicken Sie auf **OK**.

**Freigeben von Verbindungsautorisierungen**

Sie können eine Autorisierung durch das Löschen der Verbindung freigeben, die die ExpressRoute-Verbindung mit dem virtuellen Netzwerk verknüpft.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen über ExpressRoute finden Sie unter [ExpressRoute – FAQ](expressroute-faqs.md).



<!--HONumber=Feb17_HO1-->


