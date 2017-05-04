---
title: "Konfigurieren des Routings (Peerings) für eine ExpressRoute-Verbindung: Resource Manager: Azure | Microsoft-Dokumentation"
description: "In diesem Artikel werden Sie durch die Schritte zum Erstellen und Bereitstellen des privaten, öffentlichen und Microsoft-Peerings einer ExpressRoute-Verbindung geführt. Außerdem wird veranschaulicht, wie Sie den Status überprüfen, Updates durchführen oder Peerings für die Verbindung löschen."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 8c2a7ed2-ae5c-4e49-81f6-77cf9f2b2ac9
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 64bd7f356673b385581c8060b17cba721d0cf8e3
ms.openlocfilehash: 01fc643ce3fe82cd664cfca5ac02851611d7058e
ms.lasthandoff: 05/02/2017


---
# <a name="create-and-modify-peering-for-an-expressroute-circuit"></a>Erstellen und Ändern des Peerings für eine ExpressRoute-Verbindung
> [!div class="op_single_selector"]
> * [Resource Manager – Azure-Portal](expressroute-howto-routing-portal-resource-manager.md)
> * [Resource Manager – PowerShell](expressroute-howto-routing-arm.md)
> * [Klassisch – PowerShell](expressroute-howto-routing-classic.md)
> * [Video – Privates Peering](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [Video – Öffentliches Peering](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [Video – Microsoft-Peering](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> 
> 

In diesem Artikel erfahren Sie, wie Sie die Routingkonfiguration einer ExpressRoute-Verbindung mit dem Azure-Portal und dem Resource Manager-Bereitstellungsmodell erstellen und verwalten.

## <a name="configuration-prerequisites"></a>Konfigurationsvoraussetzungen
* Stellen Sie sicher, dass Sie vor Beginn der Konfiguration die Seiten [Voraussetzungen](expressroute-prerequisites.md), [Routinganforderungen](expressroute-routing.md) und [Workflows](expressroute-workflows.md) gelesen haben.
* Sie benötigen eine aktive ExpressRoute-Verbindung. Führen Sie die Schritte zum [Erstellen einer ExpressRoute-Verbindung](expressroute-howto-circuit-portal-resource-manager.md) aus, und lassen Sie sie vom Konnektivitätsanbieter aktivieren, bevor Sie fortfahren. Die ExpressRoute-Verbindung muss sich im Zustand „provisioned“ und „enabled“ befinden, damit Sie die unten beschriebenen Cmdlets ausführen können.
* Wenn Sie einen freigegebenen Schlüssel/MD5-Hash verwenden möchten, achten Sie darauf, dass Sie ihn an beiden Seiten des Tunnels benutzen, und begrenzen Sie die Anzahl der Zeichen auf ein Maximum von 25.

Diese Anweisungen gelten nur für Verbindungen, die über Service Provider erstellt wurden, von denen Layer 2-Konnektivitätsdienste angeboten werden. Wenn Sie einen Service Provider nutzen, der verwaltete Layer 3-Dienste anbietet (meist ein IPVPN, z. B. MPLS), übernimmt Ihr Konnektivitätsanbieter die Konfiguration und Verwaltung des Routings für Sie. 

> [!IMPORTANT]
> Derzeit kündigen wir keine von Service Providern konfigurierten Peerings über das Service Management Portal an. Wir arbeiten daran, dass diese Funktion schnell bereitgestellt wird. Informieren Sie sich bei Ihrem Service Provider, bevor Sie BGP-Peerings konfigurieren.
> 
> 

Sie können eine, zwei oder alle drei Peerings (Azure privat, Azure öffentlich und Microsoft) für eine ExpressRoute-Verbindung konfigurieren. Sie können Peerings in beliebiger Reihenfolge konfigurieren. Sie müssen jedoch sicherstellen, dass Sie die Konfiguration jedes Peerings einzeln nacheinander durchführen. 

## <a name="azure-private-peering"></a>Privates Azure-Peering
Dieser Abschnitt enthält Anweisungen zum Erstellen, Abrufen, Aktualisieren und Löschen der privaten Azure-Peeringkonfiguration für eine ExpressRoute-Verbindung. 

### <a name="to-create-azure-private-peering"></a>So erstellen Sie ein privates Azure-Peering
1. Konfigurieren Sie die ExpressRoute-Verbindung. Stellen Sie vor dem Fortfahren sicher, dass die Verbindung vom Konnektivitätsanbieter vollständig bereitgestellt wird.
   
    ![](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Konfigurieren Sie das private Azure-Peering für die Verbindung. Stellen Sie sicher, dass Sie über die folgenden Elemente verfügen, bevor Sie mit den nächsten Schritten fortfahren:
   
   * Ein /30-Subnetz für die primäre Verknüpfung. Dieses darf nicht Teil eines Adressraums sein, der für virtuelle Netzwerke reserviert ist.
   * Ein /30-Subnetz für die sekundäre Verknüpfung. Dieses darf nicht Teil eines Adressraums sein, der für virtuelle Netzwerke reserviert ist.
   * Eine gültige VLAN-ID zum Einrichten dieses Peerings. Stellen Sie sicher, dass kein anderes Peering der Verbindung die gleiche VLAN-ID verwendet.
   * AS-Nummer für Peering. Sie können sowohl AS-Nummern mit 2 Byte als auch mit 4 Byte verwenden. Sie können eine private AS-Nummer für dieses Peering verwenden. Achten Sie darauf, dass Sie nicht 65515 verwenden.
   * Einen MD5-Hash, wenn Sie sich für dessen Einsatz entscheiden. **Dies ist optional**.
3. Wählen Sie die Zeile für das private Azure-Peering wie unten gezeigt aus.
   
    ![](./media/expressroute-howto-routing-portal-resource-manager/rprivate1.png)
4. Konfigurieren Sie das private Azure-Peering. Die Abbildung unten enthält ein Konfigurationsbeispiel.
   
    ![](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)
5. Speichern Sie die Konfiguration, nachdem Sie alle Parameter angegeben haben. Nachdem die Konfiguration erfolgreich akzeptiert wurde, sehen Sie eine Anzeige, die dem unten angegebenen Beispiel ähnelt.
   
    ![](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="to-view-azure-private-peering-details"></a>So zeigen Sie Details zum privaten Azure-Peering an
Sie können die Eigenschaften des privaten Azure-Peerings anzeigen, indem Sie das Peering auswählen.

![](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="to-update-azure-private-peering-configuration"></a>So aktualisieren Sie die Konfiguration für privates Azure-Peering
Sie können die Zeile für das Peering auswählen und die Peeringeigenschaften ändern. 

![](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)

### <a name="to-delete-azure-private-peering"></a>So löschen Sie ein privates Azure-Peering
Sie können Ihre Peeringkonfiguration entfernen, indem Sie das Löschsymbol wie unten gezeigt auswählen.

![](./media/expressroute-howto-routing-portal-resource-manager/rprivate4.png)

## <a name="azure-public-peering"></a>Öffentliches Azure-Peering
Dieser Abschnitt enthält Anweisungen zum Erstellen, Abrufen, Aktualisieren und Löschen der öffentlichen Azure-Peeringkonfiguration für eine ExpressRoute-Verbindung. 

### <a name="to-create-azure-public-peering"></a>So erstellen Sie ein öffentliches Azure-Peering
1. Konfigurieren Sie die ExpressRoute-Verbindung. Stellen Sie vor dem Fortfahren sicher, dass die Verbindung vom Konnektivitätsanbieter vollständig bereitgestellt wird.
   
    ![](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Konfigurieren Sie das öffentliche Azure-Peering für die Verbindung. Stellen Sie sicher, dass Sie über die folgenden Elemente verfügen, bevor Sie mit den nächsten Schritten fortfahren:
   
   * Ein /30-Subnetz für die primäre Verknüpfung. 
   * Ein /30-Subnetz für die sekundäre Verknüpfung. 
   * Alle IP-Adressen, die zum Einrichten dieses Peerings verwendet werden, müssen gültige öffentliche IPv4-Adressen sein.
   * Eine gültige VLAN-ID zum Einrichten dieses Peerings. Stellen Sie sicher, dass kein anderes Peering der Verbindung die gleiche VLAN-ID verwendet.
   * AS-Nummer für Peering. Sie können sowohl AS-Nummern mit 2 Byte als auch mit 4 Byte verwenden.
   * Einen MD5-Hash, wenn Sie sich für dessen Einsatz entscheiden. **Dies ist optional**.
3. Wählen Sie die Zeile für das öffentliche Azure-Peering wie unten gezeigt aus.
   
    ![](./media/expressroute-howto-routing-portal-resource-manager/rpublic1.png)
4. Konfigurieren Sie das öffentliche Peering. Die Abbildung unten enthält ein Konfigurationsbeispiel.
   
    ![](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)
5. Speichern Sie die Konfiguration, nachdem Sie alle Parameter angegeben haben. Nachdem die Konfiguration erfolgreich akzeptiert wurde, sehen Sie eine Anzeige, die dem unten angegebenen Beispiel ähnelt.
   
    ![](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="to-view-azure-public-peering-details"></a>So zeigen Sie Details zum öffentlichen Azure-Peering an
Sie können die Eigenschaften des öffentlichen Azure-Peerings anzeigen, indem Sie das Peering auswählen.

![](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="to-update-azure-public-peering-configuration"></a>Aktualisieren der Konfiguration für öffentliches Azure-Peering
Sie können die Zeile für das Peering auswählen und die Peeringeigenschaften ändern. 

![](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)

### <a name="to-delete-azure-public-peering"></a>So löschen Sie ein öffentliches Azure-Peering
Sie können Ihre Peeringkonfiguration entfernen, indem Sie das Löschsymbol wie unten gezeigt auswählen.

![](./media/expressroute-howto-routing-portal-resource-manager/rpublic4.png)

## <a name="microsoft-peering"></a>Microsoft-Peering
Dieser Abschnitt enthält Anweisungen zum Erstellen, Abrufen, Aktualisieren und Löschen der Microsoft-Peeringkonfiguration für eine ExpressRoute-Verbindung. 

### <a name="to-create-microsoft-peering"></a>So erstellen Sie Microsoft-Peering
1. Konfigurieren Sie die ExpressRoute-Verbindung. Stellen Sie vor dem Fortfahren sicher, dass die Verbindung vom Konnektivitätsanbieter vollständig bereitgestellt wird.
   
    ![](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Konfigurieren Sie das Microsoft-Peering für die Verbindung. Stellen Sie vorab sicher, dass die folgenden Informationen vorliegen:
   
   * Ein /30-Subnetz für die primäre Verknüpfung. Dies muss ein gültiges öffentliches IPv4-Präfix sein, das sich in Ihrem Besitz befindet und über eine RIR/IRR-Registrierung verfügt.
   * Ein /30-Subnetz für die sekundäre Verknüpfung. Dies muss ein gültiges öffentliches IPv4-Präfix sein, das sich in Ihrem Besitz befindet und über eine RIR/IRR-Registrierung verfügt.
   * Eine gültige VLAN-ID zum Einrichten dieses Peerings. Stellen Sie sicher, dass kein anderes Peering der Verbindung die gleiche VLAN-ID verwendet.
   * AS-Nummer für Peering. Sie können sowohl AS-Nummern mit 2 Byte als auch mit 4 Byte verwenden.
   * **Angekündigte Präfixe:** Sie müssen eine Liste mit allen Präfixen bereitstellen, die Sie über die BGP-Sitzung ankündigen möchten. Nur öffentliche IP-Adresspräfixe werden akzeptiert. Sie können eine kommagetrennte Liste senden, wenn Sie planen, einen Satz mit Präfixen zu senden. Diese Präfixe müssen über eine RIR/IRR-Registrierung für Sie verfügen.
   * **Kunden-ASN** : Wenn Sie Präfixe ankündigen, die nicht für die Peering-AS-Nummer registriert sind, können Sie die AS-Nummer angeben, unter der sie registriert sind. **Dies ist optional**.
   * **Routing-Registrierungsname:** Sie können den RIR/IRR-Wert angeben, unter dem die AS-Nummer und die Präfixe registriert sind. **Dies ist optional.**
   * Einen MD5-Hash, wenn Sie sich für dessen Einsatz entscheiden. **Dies ist optional.**
3. Sie können das Peering, das Sie konfigurieren möchten, wie unten gezeigt auswählen. Wählen Sie die Zeile für das Microsoft-Peering aus.
   
    ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft1.png)
4. Konfigurieren Sie das Microsoft-Peering. Die Abbildung unten enthält ein Konfigurationsbeispiel.
   
   ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft2.png)
5. Speichern Sie die Konfiguration, nachdem Sie alle Parameter angegeben haben. 
   
    Wenn Ihre Verbindung wie oben dargestellt in den Zustand „validation needed“ (Überprüfung erforderlich) versetzt wird, müssen Sie ein Supportticket öffnen, um gegenüber unserem Supportteam nachzuweisen, dass sich die Präfixe in Ihrem Besitz befinden.    
   
    ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft5.png)

    Sie können ein Supportticket wie unten gezeigt direkt über das Portal öffnen.     

    ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft6.png)


1. Nachdem die Konfiguration erfolgreich akzeptiert wurde, sehen Sie eine Anzeige, die dem unten angegebenen Beispiel ähnelt.
   
    ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="to-view-microsoft-peering-details"></a>So zeigen Sie die Details zum Microsoft-Peering an
Sie können die Eigenschaften des öffentlichen Azure-Peerings anzeigen, indem Sie das Peering auswählen.

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft3.png)

### <a name="to-update-microsoft-peering-configuration"></a>So aktualisieren Sie die Konfiguration des Microsoft-Peerings
Sie können die Zeile für das Peering auswählen und die Peeringeigenschaften ändern. 

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="to-delete-microsoft-peering"></a>So löschen Sie das Microsoft-Peering
Sie können Ihre Peeringkonfiguration entfernen, indem Sie das Löschsymbol wie unten gezeigt auswählen.

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft4.png)

## <a name="next-steps"></a>Nächste Schritte
Nächster Schritt: [Verknüpfen eines VNET mit einer ExpressRoute-Verbindung](expressroute-howto-linkvnet-portal-resource-manager.md)
* Weitere Informationen zu ExpressRoute-Workflows finden Sie unter [ExpressRoute-Workflows](expressroute-workflows.md).
* Weitere Informationen zum Verbindungspeering finden Sie unter [ExpressRoute-Verbindungen und Routingdomänen](expressroute-circuit-peerings.md).
* Weitere Informationen zur Arbeit mit virtuellen Netzwerken finden Sie unter [Virtuelle Netzwerke im Überblick](../virtual-network/virtual-networks-overview.md).


