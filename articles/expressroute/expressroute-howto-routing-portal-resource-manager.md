<properties
   pageTitle="Gewusst wie: Konfigurieren des Routings für eine ExpressRoute-Verbindung mit dem Azure-Portal | Microsoft Azure"
   description="In diesem Artikel werden Sie durch die Schritte zum Erstellen und Bereitstellen des privaten, öffentlichen und Microsoft-Peerings einer ExpressRoute-Verbindung geführt. Außerdem wird veranschaulicht, wie Sie den Status überprüfen, Updates durchführen oder Peerings für die Verbindung löschen."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="hero-article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>


# <a name="create-and-modify-routing-for-an-expressroute-circuit"></a>Erstellen und Ändern des Routings für eine ExpressRoute-Verbindung



> [AZURE.SELECTOR]
[Azure-Portal – Resource Manager](expressroute-howto-routing-portal-resource-manager.md)
[PowerShell – Resource Manager](expressroute-howto-routing-arm.md)
[PowerShell – klassisch](expressroute-howto-routing-classic.md)



In diesem Artikel erfahren Sie, wie Sie die Routingkonfiguration einer ExpressRoute-Verbindung mit dem Azure-Portal und dem Resource Manager-Bereitstellungsmodell erstellen und verwalten.

**Informationen zu Azure-Bereitstellungsmodellen**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-prerequisites"></a>Konfigurationsvoraussetzungen

- Stellen Sie sicher, dass Sie vor Beginn der Konfiguration die Seiten [Voraussetzungen](expressroute-prerequisites.md), [Routinganforderungen](expressroute-routing.md) und [Workflows](expressroute-workflows.md) gelesen haben.
- Sie benötigen eine aktive ExpressRoute-Verbindung. Führen Sie die Schritte zum [Erstellen einer ExpressRoute-Verbindung](expressroute-howto-circuit-arm.md) aus, und lassen Sie sie vom Konnektivitätsanbieter aktivieren, bevor Sie fortfahren. Die ExpressRoute-Verbindung muss sich im Zustand „provisioned“ und „enabled“ befinden, damit Sie die unten beschriebenen Cmdlets ausführen können.

Diese Anweisungen gelten nur für Verbindungen, die über Service Provider erstellt wurden, von denen Layer 2-Konnektivitätsdienste angeboten werden. Wenn Sie einen Service Provider nutzen, der verwaltete Layer 3-Dienste anbietet (meist ein IPVPN, z. B. MPLS), übernimmt Ihr Konnektivitätsanbieter die Konfiguration und Verwaltung des Routings für Sie. 


>[AZURE.IMPORTANT] Derzeit kündigen wir keine von Service Providern konfigurierten Peerings über das Service Management Portal an. Wir arbeiten daran, dass diese Funktion schnell bereitgestellt wird. Informieren Sie sich bei Ihrem Service Provider, bevor Sie BGP-Peerings konfigurieren.

Sie können eine, zwei oder alle drei Peerings (Azure privat, Azure öffentlich und Microsoft) für eine ExpressRoute-Verbindung konfigurieren. Sie können Peerings in beliebiger Reihenfolge konfigurieren. Sie müssen jedoch sicherstellen, dass Sie die Konfiguration jedes Peerings einzeln nacheinander durchführen. 

## <a name="azure-private-peering"></a>Privates Azure-Peering

Dieser Abschnitt enthält Anweisungen zum Erstellen, Abrufen, Aktualisieren und Löschen der privaten Azure-Peeringkonfiguration für eine ExpressRoute-Verbindung. 

### <a name="to-create-azure-private-peering"></a>So erstellen Sie ein privates Azure-Peering

1. Konfigurieren Sie die ExpressRoute-Verbindung. Stellen Sie vor dem Fortfahren sicher, dass die Verbindung vom Konnektivitätsanbieter vollständig bereitgestellt wird.

    ![](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)


2. Konfigurieren Sie das private Azure-Peering für die Verbindung. Stellen Sie sicher, dass Sie über die folgenden Elemente verfügen, bevor Sie mit den nächsten Schritten fortfahren:

    - Ein /30-Subnetz für die primäre Verknüpfung. Dieses darf nicht Teil eines Adressraums sein, der für virtuelle Netzwerke reserviert ist.
    - Ein /30-Subnetz für die sekundäre Verknüpfung. Dieses darf nicht Teil eines Adressraums sein, der für virtuelle Netzwerke reserviert ist.
    - Eine gültige VLAN-ID zum Einrichten dieses Peerings. Stellen Sie sicher, dass kein anderes Peering der Verbindung die gleiche VLAN-ID verwendet.
    - AS-Nummer für Peering. Sie können sowohl AS-Nummern mit 2 Byte als auch mit 4 Byte verwenden. Sie können eine private AS-Nummer für dieses Peering verwenden. Achten Sie darauf, dass Sie nicht 65515 verwenden.
    - Einen MD5-Hash, wenn Sie sich für dessen Einsatz entscheiden. **Dies ist optional**.


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

    - Ein /30-Subnetz für die primäre Verknüpfung. 
    - Ein /30-Subnetz für die sekundäre Verknüpfung. 
    - Alle IP-Adressen, die zum Einrichten dieses Peerings verwendet werden, müssen gültige öffentliche IPv4-Adressen sein.
    - Eine gültige VLAN-ID zum Einrichten dieses Peerings. Stellen Sie sicher, dass kein anderes Peering der Verbindung die gleiche VLAN-ID verwendet.
    - AS-Nummer für Peering. Sie können sowohl AS-Nummern mit 2 Byte als auch mit 4 Byte verwenden.
    - Einen MD5-Hash, wenn Sie sich für dessen Einsatz entscheiden. **Dies ist optional**.

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

    - Ein /30-Subnetz für die primäre Verknüpfung. Dies muss ein gültiges öffentliches IPv4-Präfix sein, das sich in Ihrem Besitz befindet und über eine RIR/IRR-Registrierung verfügt.
    - Ein /30-Subnetz für die sekundäre Verknüpfung. Dies muss ein gültiges öffentliches IPv4-Präfix sein, das sich in Ihrem Besitz befindet und über eine RIR/IRR-Registrierung verfügt.
    - Eine gültige VLAN-ID zum Einrichten dieses Peerings. Stellen Sie sicher, dass kein anderes Peering der Verbindung die gleiche VLAN-ID verwendet.
    - AS-Nummer für Peering. Sie können sowohl AS-Nummern mit 2 Byte als auch mit 4 Byte verwenden.
    - **Angekündigte Präfixe:** Sie müssen eine Liste mit allen Präfixen bereitstellen, die Sie über die BGP-Sitzung ankündigen möchten. Nur öffentliche IP-Adresspräfixe werden akzeptiert. Sie können eine kommagetrennte Liste senden, wenn Sie planen, einen Satz mit Präfixen zu senden. Diese Präfixe müssen über eine RIR/IRR-Registrierung für Sie verfügen.
    - **Kunden-ASN** : Wenn Sie Präfixe ankündigen, die nicht für die Peering-AS-Nummer registriert sind, können Sie die AS-Nummer angeben, unter der sie registriert sind. **Dies ist optional**.
    - **Routing-Registrierungsname:** Sie können den RIR/IRR-Wert angeben, unter dem die AS-Nummer und die Präfixe registriert sind. **Dies ist optional.**
    - Einen MD5-Hash, wenn Sie sich für dessen Einsatz entscheiden. **Dies ist optional.**
    
3. Sie können das Peering, das Sie konfigurieren möchten, wie unten gezeigt auswählen. Wählen Sie die Zeile für das Microsoft-Peering aus.
    
    ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft1.png)
    

4.  Konfigurieren Sie das Microsoft-Peering. Die Abbildung unten enthält ein Konfigurationsbeispiel.

    ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft2.png)

    
5. Speichern Sie die Konfiguration, nachdem Sie alle Parameter angegeben haben. 

    Wenn Ihre Verbindung wie oben dargestellt in den Zustand „validation needed“ (Überprüfung erforderlich) versetzt wird, müssen Sie ein Supportticket öffnen, um gegenüber unserem Supportteam nachzuweisen, dass sich die Präfixe in Ihrem Besitz befinden.  
    
    ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft5.png)


    Sie können ein Supportticket wie unten gezeigt direkt über das Portal öffnen.   
    
    ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft6.png)


6. Nachdem die Konfiguration erfolgreich akzeptiert wurde, sehen Sie eine Anzeige, die dem unten angegebenen Beispiel ähnelt.

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

[Verknüpfen Sie als Nächstes ein VNET mit einer ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md).

-  Weitere Informationen zu ExpressRoute-Workflows finden Sie unter [ExpressRoute-Workflows](expressroute-workflows.md).

-  Weitere Informationen zum Verbindungspeering finden Sie unter [ExpressRoute-Verbindungen und Routingdomänen](expressroute-circuit-peerings.md).

-  Weitere Informationen zur Arbeit mit virtuellen Netzwerken finden Sie unter [Virtuelle Netzwerke im Überblick](../virtual-network/virtual-networks-overview.md).




<!--HONumber=Oct16_HO2-->


