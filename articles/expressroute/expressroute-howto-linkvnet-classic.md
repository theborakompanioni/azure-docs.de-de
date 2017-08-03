---
title: "Verknüpfen eines virtuellen Netzwerks mit einer ExpressRoute-Verbindung: PowerShell: klassisch: Azure | Microsoft-Dokumentation"
description: "Dieses Dokument bietet Ihnen eine Übersicht über das Verknüpfen virtueller Netzwerke (VNETs) mit ExpressRoute-Verbindungen über das klassische Bereitstellungsmodell und PowerShell."
services: expressroute
documentationcenter: na
author: ganesr
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 9b53fd72-9b6b-4844-80b9-4e1d54fd0c17
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/28/2017
ms.author: ganesr
ms.translationtype: HT
ms.sourcegitcommit: 54774252780bd4c7627681d805f498909f171857
ms.openlocfilehash: 8df8a4c6ff0897c821e13248e0494b17e1a4992d
ms.contentlocale: de-de
ms.lasthandoff: 07/27/2017

---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit-using-powershell-classic"></a>Verbinden eines virtuellen Netzwerks mit einer ExpressRoute-Verbindung mithilfe von PowerShell (klassisch)
> [!div class="op_single_selector"]
> * [Azure-Portal](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Azure-CLI](howto-linkvnet-cli.md)
> * [Video – Azure-Portal](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (klassisch)](expressroute-howto-linkvnet-classic.md)
>

Dieser Artikel unterstützt Sie beim Verknüpfen virtueller Netzwerke (VNETs) mit Azure ExpressRoute-Verbindungen über das klassische Bereitstellungsmodell und PowerShell. Virtuelle Netzwerke können Teil desselben Abonnements sein oder zu einem anderen Abonnement gehören.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Informationen zu Azure-Bereitstellungsmodellen**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="configuration-prerequisites"></a>Konfigurationsvoraussetzungen
1. Sie benötigen die neueste Version der Azure PowerShell-Module. Sie können das neueste PowerShell-Modul im Abschnitt „PowerShell“ auf der Seite [Downloads](https://azure.microsoft.com/downloads/)für Azure herunterladen. Eine Schritt-für-Schritt-Anleitung zum Konfigurieren des Computers für die Verwendung der Azure PowerShell-Module erhalten Sie auf der Seite [Gewusst wie: Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview) .
2. Stellen Sie sicher, dass Sie vor Beginn der Konfiguration die Seiten [Voraussetzungen](expressroute-prerequisites.md), [Routinganforderungen](expressroute-routing.md) und [Workflows](expressroute-workflows.md) gelesen haben.
3. Sie benötigen eine aktive ExpressRoute-Verbindung.
   * Führen Sie die Schritte zum [Erstellen einer ExpressRoute-Verbindung](expressroute-howto-circuit-classic.md) aus, und lassen Sie sie vom Konnektivitätsanbieter aktivieren.
   * Stellen Sie sicher, dass privates Azure-Peering für die Verbindung konfiguriert ist. Informationen zum Routing finden Sie unter [Konfigurieren des Routings](expressroute-howto-routing-classic.md) .
   * Vergewissern Sie sich, dass das private Azure-Peering konfiguriert wurde und das BGP-Peering zwischen Ihrem Netzwerk und Microsoft aktiv ist, damit End-to-End-Konnektivität bereitgestellt werden kann.
   * Sie müssen ein virtuelles Netzwerk und ein virtuelles Netzwerkgateway erstellt und vollständig bereitgestellt haben. Befolgen Sie die Anweisungen zum [Konfigurieren eines virtuellen Netzwerks für ExpressRoute](expressroute-howto-vnet-portal-classic.md).

Sie können bis zu 10 virtuelle Netzwerke mit einer ExpressRoute-Verbindung verknüpfen. Alle virtuellen Netzwerke müssen sich in derselben geopolitischen Region befinden. Wenn Sie das ExpressRoute Premium-Add-On aktiviert haben, können Sie eine größere Anzahl von virtuellen Netzwerken mit der ExpressRoute-Verbindung oder virtuelle Netzwerke verknüpfen, die sich in anderen geopolitischen Regionen befinden. Weitere Informationen zum Premium-Add-On finden Sie in den [häufig gestellten Fragen](expressroute-faqs.md) .

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>Herstellen einer Verbindung zwischen einem virtuellen Netzwerk in demselben Abonnement und einer Verbindung
Sie können das folgende Cmdlet verwenden, um ein virtuelles Netzwerk mit einer ExpressRoute-Verbindung zu verknüpfen. Stellen Sie sicher, dass das Gateway für das virtuelle Netzwerk erstellt wurde und für das Erstellen von Verknüpfungen bereit ist, bevor Sie das Cmdlet ausführen.

    New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"
    Provisioned

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>Herstellen einer Verbindung zwischen einem virtuellen Netzwerk in einem anderen Abonnement und einer Verbindung
Sie können eine ExpressRoute-Verbindung für mehrere Abonnements freigeben. Die folgende Abbildung zeigt eine einfache schematische Darstellung der Freigabe von Lasten für ExpressRoute-Verbindungen für mehrere Abonnements.

Jede der kleineren Clouds innerhalb der großen Cloud stellt Abonnements dar, die zu verschiedenen Abteilungen innerhalb einer Organisation gehören. Jede der Abteilungen innerhalb der Organisation kann ihr eigenes Abonnement zum Bereitstellen von Diensten verwenden, für die Verbindung mit dem lokalen Netzwerk können die Abteilungen jedoch eine einzelne gemeinsam genutzte ExpressRoute-Verbindung verwenden. Eine einzelne Abteilung (in diesem Beispiel: IT) kann die ExpressRoute-Verbindung besitzen. Andere Abonnements innerhalb der Organisation können die ExpressRoute-Verbindung nutzen.

> [!NOTE]
> Konnektivitäts- und Bandbreitengebühren für die dedizierte Verbindung werden dem Besitzer der ExpressRoute-Verbindung in Rechnung gestellt. Alle virtuellen Netzwerke verwenden gemeinsam dieselbe Bandbreite.
> 
> 

![Abonnementübergreifende Konnektivität](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration"></a>Verwaltung
Der *Verbindungsbesitzer* ist der Administrator/Co-Administrator des Abonnements, in dem die ExpressRoute-Verbindung erstellt wird. Der Besitzer der Verbindung kann Administratoren/Co-Administratoren anderer Abonnements ( *Verbindungsbenutzer*genannt) für die Nutzung der dedizierten Verbindung in seinem Besitz autorisieren. Verbindungsbenutzer, die für die Nutzung der ExpressRoute-Verbindung einer Organisation autorisiert sind, können ein virtuelles Netzwerk in ihrem Abonnement mit der ExpressRoute-Verbindung verknüpfen, sobald sie autorisiert wurden.

Der Besitzer der Verbindung hat die Möglichkeit, Autorisierungen jederzeit zu ändern und aufzuheben. Das Aufheben einer Autorisierung führt dazu, dass alle Verknüpfungen aus dem Abonnement gelöscht werden, dessen Zugriff aufgehoben wurde.

### <a name="circuit-owner-operations"></a>Aktionen als Verbindungsbesitzer

**Erstellen einer Autorisierung**

Der Verbindungsbesitzer autorisiert die Administratoren anderer Abonnements für die Nutzung der angegebenen Verbindung. Im folgenden Beispiel ermöglicht der Administrator der Verbindung (Contoso IT) dem Administrator eines anderen Abonnements (Dev-Test) das Verknüpfen von bis zu zwei virtuellen Netzwerken mit der Verbindung. Der Contoso-IT-Administrator ermöglicht dies durch die Angabe der Microsoft-ID von Dev-Test. Das Cmdlet sendet keine E-Mail an die angegebene Microsoft-ID. Der Verbindungsbesitzer muss den Besitzer des anderen Abonnements explizit darüber benachrichtigen, dass die Autorisierung erfolgt ist.

    New-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -Description "Dev-Test Links" -Limit 2 -MicrosoftIds 'devtest@contoso.com'

    Description         : Dev-Test Links
    Limit               : 2
    LinkAuthorizationId : **********************************
    MicrosoftIds        : devtest@contoso.com
    Used                : 0

**Überprüfen von Autorisierungen**

Mit dem folgenden Cmdlet kann der Besitzer einer Verbindung alle für eine bestimmte Verbindung ausgestellten Autorisierungen überprüfen:

    Get-AzureDedicatedCircuitLinkAuthorization -ServiceKey: "**************************"

    Description         : EngineeringTeam
    Limit               : 3
    LinkAuthorizationId : ####################################
    MicrosoftIds        : engadmin@contoso.com
    Used                : 1

    Description         : MarketingTeam
    Limit               : 1
    LinkAuthorizationId : @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    MicrosoftIds        : marketingadmin@contoso.com
    Used                : 0

    Description         : Dev-Test Links
    Limit               : 2
    LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    MicrosoftIds        : salesadmin@contoso.com
    Used                : 2


**Aktualisieren von Autorisierungen**

Mit dem folgenden Cmdlet kann der Besitzer einer Verbindung die Autorisierungen bearbeiten:

    Set-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -AuthorizationId "&&&&&&&&&&&&&&&&&&&&&&&&&&&&"-Limit 5

    Description         : Dev-Test Links
    Limit               : 5
    LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    MicrosoftIds        : devtest@contoso.com
    Used                : 0


**Löschen von Autorisierungen**

Mit dem folgenden Cmdlet kann der Besitzer einer Verbindung Autorisierungen für einen Benutzer widerrufen oder löschen:

    Remove-AzureDedicatedCircuitLinkAuthorization -ServiceKey "*****************************" -AuthorizationId "###############################"


### <a name="circuit-user-operations"></a>Aktionen als Verbindungsbenutzer

**Überprüfen von Autorisierungen**

Mit dem folgenden Cmdlet können Benutzer einer Verbindung Autorisierungen überprüfen.

    Get-AzureAuthorizedDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : ContosoIT
    Location                         : Washington DC
    MaximumAllowedLinks              : 2
    ServiceKey                       : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled
    UsedLinks                        : 0

**Abrufen von Verknüpfungsautorisierungen**

Mit dem folgenden Cmdlet können Benutzer einer Verbindung eine Verknüpfungsautorisierung abrufen:

    New-AzureDedicatedCircuitLink –servicekey "&&&&&&&&&&&&&&&&&&&&&&&&&&" –VnetName 'SalesVNET1'

    State VnetName
    ----- --------
    Provisioned SalesVNET1

Führen Sie diesen Befehl im neu verknüpften Abonnement für das virtuelle Netzwerk aus:

    New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen über ExpressRoute finden Sie unter [ExpressRoute – FAQ](expressroute-faqs.md).


