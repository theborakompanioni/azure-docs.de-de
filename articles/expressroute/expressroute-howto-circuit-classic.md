---
title: "Erstellen und Ändern einer ExpressRoute-Verbindung: PowerShell: Azure (klassisch) | Microsoft-Dokumentation"
description: "In diesem Artikel werden Sie durch die Schritte zum Erstellen und Bereitstellen einer ExpressRoute-Verbindung geführt. Außerdem wird veranschaulicht, wie Sie Status überprüfen, Updates durchführen oder die Verbindung löschen oder deren Bereitstellung aufheben."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 0134d242-6459-4dec-a2f1-4657c3bc8b23
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: ganesr;cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 0bec803e4b49f3ae53f2cc3be6b9cb2d256fe5ea
ms.openlocfilehash: efdec32e565bf1d11b562d283e56bd8ed5d292b9
ms.lasthandoff: 03/24/2017


---
# <a name="create-and-modify-an-expressroute-circuit-using-powershell-classic"></a>Erstellen und Ändern einer ExpressRoute-Verbindung mit PowerShell (klassisch)
> [!div class="op_single_selector"]
> * [Resource Manager – Azure-Portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [Resource Manager – PowerShell](expressroute-howto-circuit-arm.md)
> * [Klassisch – PowerShell](expressroute-howto-circuit-classic.md)
> * [Video – Azure-Portal](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> 
>

In diesem Artikel werden Sie durch die Schritte zum Erstellen einer Azure ExpressRoute-Verbindung mithilfe von PowerShell-Cmdlets und des klassischen Bereitstellungsmodells geführt. In diesem Artikel wird auch veranschaulicht, wie Sie den Status prüfen, ein Update durchführen oder eine ExpressRoute-Verbindung löschen bzw. deren Bereitstellung aufheben.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]


**Informationen zu Azure-Bereitstellungsmodellen**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a>Voraussetzungen
### <a name="step-1-review-the-prerequisites-and-workflow-articles"></a>Schritt 1: Überprüfen der Voraussetzungen und Workflowartikel
Stellen Sie sicher, dass Sie vor Beginn der Konfiguration die [Voraussetzungen](expressroute-prerequisites.md) und [Workflows](expressroute-workflows.md) gelesen haben.  

### <a name="step-2-install-the-latest-versions-of-the-azure-service-management-sm-powershell-modules"></a>Schritt 2: Installieren Sie die aktuellen Versionen der PowerShell-Module für die Azure-Dienstverwaltung.
Eine ausführliche Anleitung zum Konfigurieren des Computers für die Verwendung der Azure PowerShell-Module erhalten Sie auf der Seite [Erste Schritte mit Azure PowerShell-Cmdlets](/powershell/azureps-cmdlets-docs).

### <a name="step-3-log-in-to-your-azure-account-and-select-a-subscription"></a>Schritt 3: Anmelden bei Ihrem Azure-Konto und Auswählen eines Abonnements
1. Öffnen Sie die PowerShell-Konsole mit erhöhten Rechten, und stellen Sie eine Verbindung mit Ihrem Konto her. Verwenden Sie das folgende Beispiel, um eine Verbindung herzustellen:

        Login-AzureRmAccount

2. Überprüfen Sie die Abonnements für das Konto.

        Get-AzureRmSubscription

3. Wenn Sie über mehr als ein Abonnement verfügen, wählen Sie das Abonnement aus, das Sie verwenden möchten.

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

4. Verwenden Sie als nächstes das folgende Cmdlet, um PowerShell Ihr Azure-Abonnement für das klassische Bereitstellungsmodell hinzuzufügen.

        Add-AzureAccount

## <a name="create-and-provision-an-expressroute-circuit"></a>Erstellen und Bereitstellen einer ExpressRoute-Verbindung
### <a name="step-1-import-the-powershell-modules-for-expressroute"></a>Schritt 1: Importieren der PowerShell-Module für ExpressRoute
 Falls nicht bereits geschehen, müssen Sie die Azure- und ExpressRoute-Module in die PowerShell-Sitzung importieren, um die ExpressRoute-Cmdlets verwenden zu können. Sie importieren die Module aus dem Speicherort, im dem sie auf dem lokalen Computer installiert waren. Je nach der Methode, die Sie verwendet haben, um die Module zu installieren, weicht der Speicherort möglicherweise vom folgenden Beispiel ab. Ändern Sie das Beispiel, sofern erforderlich.  

    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

### <a name="step-2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>Schritt 2: Rufen Sie die Liste der unterstützten Anbieter, Standorte und Bandbreiten ab.
Vor dem Erstellen einer ExpressRoute-Verbindung benötigen Sie die Liste der unterstützten Konnektivitätsanbieter, Standorte und Bandbreitenoptionen.

Das PowerShell-Cmdlet `Get-AzureDedicatedCircuitServiceProvider` gibt diese Informationen zurück, die Sie in späteren Schritten verwenden werden:

    Get-AzureDedicatedCircuitServiceProvider

Überprüfen Sie, ob Ihr Konnektivitätsanbieter hier aufgeführt ist. Notieren Sie sich die folgenden Informationen, da Sie diese später beim Erstellen der Verbindung benötigen:

* Name
* PeeringLocations
* BandwidthsOffered

Sie können nun eine ExpressRoute-Verbindung erstellen.         

### <a name="step-3-create-an-expressroute-circuit"></a>Schritt 3: Erstellen Sie eine ExpressRoute-Verbindung.
Das folgende Beispiel zeigt, wie Sie eine ExpressRoute-Verbindung mit 200 MBit/s über Equinix im Silicon Valley herstellen können. Wenn Sie einen anderen Anbieter und andere Einstellungen verwenden, ersetzen Sie bei Ihrer Anforderung die entsprechenden Informationen.

> [!IMPORTANT]
> Ihre ExpressRoute-Verbindung wird von dem Moment an berechnet, in dem ein Dienstschlüssel ausgegeben wird. Stellen Sie sicher, dass Sie diesen Vorgang ausführen, sobald der Konnektivitätsanbieter dazu bereit ist, die Verbindung bereitzustellen.
> 
> 

Nachfolgend sehen Sie eine Beispielanforderung für einen neuen Dienstschlüssel:

    $Bandwidth = 200
    $CircuitName = "MyTestCircuit"
    $ServiceProvider = "Equinix"
    $Location = "Silicon Valley"

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Standard -BillingType MeteredData

Verwenden Sie das nachstehende Beispiel, falls Sie eine ExpressRoute-Verbindung mit dem Premium-Add-On erstellen möchten. Weitere Details zum Premium-Add-On finden Sie unter [ExpressRoute – Häufig gestellte Fragen](expressroute-faqs.md) .

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Premium - BillingType MeteredData


Die Antwort enthält den Dienstschlüssel. Ausführliche Beschreibungen aller Parameter erhalten Sie, wenn Sie Folgendes ausführen:

    get-help new-azurededicatedcircuit -detailed

### <a name="step-4-list-all-the-expressroute-circuits"></a>Schritt 4: Auflisten aller ExpressRoute-Verbindungen
Um eine Liste mit allen von Ihnen erstellten ExpressRoute-Verbindungen abzurufen, können Sie den Befehl `Get-AzureDedicatedCircuit` ausführen:

    Get-AzureDedicatedCircuit

Die Antwort ähnelt dem folgenden Beispiel:

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Sie können diese Informationen jederzeit mithilfe des Cmdlets `Get-AzureDedicatedCircuit` abrufen. Wenn Sie den Aufruf ohne Parameter durchführen, werden alle Verbindungen aufgelistet. Der Dienstschlüssel wird im Feld *ServiceKey* aufgeführt.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Ausführliche Beschreibungen aller Parameter erhalten Sie, wenn Sie Folgendes ausführen:

    get-help get-azurededicatedcircuit -detailed

### <a name="step-5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>Schritt 5: Senden Sie den Dienstschlüssel zur Bereitstellung an Ihren Konnektivitätsanbieter.
*ServiceProviderProvisioningState* enthält Informationen zum aktuellen Zustand der Bereitstellung auf Dienstanbieterseite. *Status* wird der Zustand auf Microsoft-Seite angegeben. Weitere Informationen zu den Bereitstellungszuständen einer Verbindung finden Sie im Artikel [Workflows](expressroute-workflows.md#expressroute-circuit-provisioning-states) .

Wenn Sie eine neue ExpressRoute-Verbindung erstellen, weist die Verbindung folgenden Zustand auf:

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


Die Verbindung wird in den folgenden Zustand versetzt, wenn sie vom Konnektivitätsanbieter für Sie aktiviert wird:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Eine ExpressRoute-Verbindung muss sich im folgenden Zustand befinden, damit Sie sie verwenden können:

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled


### <a name="step-6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>Schritt 6: Überprüfen Sie regelmäßig den Status und Zustand des Verbindungsschlüssels.
So sind Sie informiert, wenn Ihr Anbieter die Verbindung aktiviert hat. Sobald die Verbindung konfiguriert wurde, zeigt *ServiceProviderProvisioningState* wie im folgenden Beispiel den Status *Bereitgestellt* an:

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

### <a name="step-7-create-your-routing-configuration"></a>Schritt 7 Erstellen Sie die Routingkonfiguration.
Eine detaillierte Anleitung finden Sie auf der Seite [Routingkonfiguration für ExpressRoute-Verbindung (Erstellen und Ändern von Verbindungspeerings)](expressroute-howto-routing-classic.md) .

> [!IMPORTANT]
> Diese Anweisungen gelten nur für Verbindungen, die über Dienstanbieter erstellt wurden, von denen Layer 2-Konnektivitätsdienste angeboten werden. Wenn Sie einen Dienstanbieter nutzen, der verwaltete Layer 3-Dienste anbietet (meist ein IP VPN, z.B. MPLS), übernimmt Ihr Konnektivitätsanbieter die Konfiguration und Verwaltung des Routings für Sie.
> 
> 

### <a name="step-8-link-a-virtual-network-to-an-expressroute-circuit"></a>Schritt 8: Verknüpfen eines virtuellen Netzwerks mit einer ExpressRoute-Verbindung
Verknüpfen Sie anschließend ein virtuelles Netzwerk mit Ihrer ExpressRoute-Verbindung. Lesen Sie die Schritt-für-Schritt-Anweisungen unter [Verknüpfen von virtuellen Netzwerken mit ExpressRoute-Verbindungen](expressroute-howto-linkvnet-classic.md) . Wenn Sie ein virtuelles Netzwerk für ExpressRoute mithilfe des klassischen Bereitstellungsmodells erstellen möchten, hilft Ihnen die Anleitung [Erstellen eines virtuellen Netzwerks für ExpressRoute](expressroute-howto-vnet-portal-classic.md) weiter.

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Abrufen des Status einer ExpressRoute-Verbindung
Sie können diese Informationen jederzeit mithilfe des Cmdlets `Get-AzureCircuit` abrufen. Wenn Sie den Aufruf ohne Parameter durchführen, werden alle Verbindungen aufgelistet.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

    Bandwidth                        : 1000
    CircuitName                      : MyAsiaCircuit
    Location                         : Singapore
    ServiceKey                       : #################################
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Sie können Informationen zu einer bestimmten ExpressRoute-Verbindung erhalten, indem Sie den Dienstschlüssel als Parameter an den Aufruf übergeben.

    Get-AzureDedicatedCircuit -ServiceKey "*********************************"

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled


Ausführliche Beschreibungen aller Parameter erhalten Sie, wenn Sie folgendes Beispiel ausführen:

    get-help get-azurededicatedcircuit -detailed

## <a name="modifying-an-expressroute-circuit"></a>Ändern einer ExpressRoute-Verbindung
Sie können bestimmte Eigenschaften einer ExpressRoute-Verbindung ändern, ohne die Konnektivität zu beeinträchtigen.

Sie können folgende Aktionen ausführen, ohne Ausfallzeiten zu verursachen:

* Aktivieren oder deaktivieren Sie ein ExpressRoute Premium-Add-On für Ihre ExpressRoute-Verbindung.
* Erhöhen Sie die Bandbreite der ExpressRoute-Verbindung, sofern Kapazität am Port vorhanden ist. Beachten Sie, dass ein Downgrade der Bandbreite einer Verbindung nicht unterstützt wird. 
* Sie können den Abrechnungsplan von „Volumentarif“ zu „Datenflatrate“ ändern. Beachten Sie, dass eine Änderung des Abrechnungsplans von „Datenflatrate“ zu „Volumentarif“ nicht unterstützt wird.
* Sie können die Option *Klassische Vorgänge zulassen*aktivieren und deaktivieren.

Weitere Informationen zu Beschränkungen und Grenzwerten finden Sie unter [ExpressRoute – Häufig gestellte Fragen](expressroute-faqs.md) .

### <a name="to-enable-the-expressroute-premium-add-on"></a>Aktivieren des ExpressRoute Premium-Add-Ons
Für Ihre bereits vorhandene Verbindung können Sie das ExpressRoute Premium-Add-On mit dem folgenden PowerShell-Cmdlet aktivieren:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Premium

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Premium
    Status                           : Enabled

Für Ihre Verbindung sind nun die Features des ExpressRoute Premium-Add-Ons aktiviert. Beachten Sie, dass die Gebühren für das Premium-Add-On ab der erfolgreichen Ausführung des Befehls erhoben werden.

### <a name="to-disable-the-expressroute-premium-add-on"></a>Deaktivieren des ExpressRoute Premium-Add-Ons
> [!IMPORTANT]
> Bei diesem Vorgang kann ein Fehler auftreten, wenn Sie Ressourcen verwenden, die die zulässige Menge für die Standardverbindung überschreiten.
> 
> 

#### <a name="considerations"></a>Überlegungen

* Sie müssen sicherstellen, dass die Anzahl von virtuellen Netzwerken, die mit der Verbindung verknüpft sind, kleiner als zehn ist, bevor Sie ein Downgrade von Premium auf Standard durchführen. Wenn Sie dies nicht beachten, tritt für die Updateanforderung ein Fehler auf, und Ihnen werden die Premium-Gebühren berechnet.
* Sie müssen die Verknüpfung für alle virtuellen Netzwerke in anderen geopolitischen Regionen aufheben. Wenn Sie dies nicht beachten, tritt für die Updateanforderung ein Fehler auf, und Ihnen werden die Premium-Gebühren berechnet.
* Ihre Routentabelle muss für das private Peering weniger als 4.000 Routen aufweisen. Wenn Ihre Routentabelle mehr als 4.000 Routen umfasst, wird die BGP-Sitzung verworfen. Eine erneute Aktivierung ist erst dann wieder möglich, wenn die Anzahl der angekündigten Präfixe unter 4.000 fällt.

#### <a name="disable-the-premium-add-on"></a>Deaktivieren des Premium-Add-Ons
Für Ihre bereits vorhandene Verbindung können Sie das ExpressRoute Premium-Add-On mit dem folgenden PowerShell-Cmdlet deaktivieren:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Standard

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled



### <a name="to-update-the-expressroute-circuit-bandwidth"></a>Aktualisieren der Bandbreite für die ExpressRoute-Verbindung
Informationen zu unterstützten Bandbreitenoptionen für Ihren Anbieter finden Sie unter [ExpressRoute – Häufig gestellte Fragen](expressroute-faqs.md) . Sie können eine beliebige Größe auswählen, die größer als die vorhandene Verbindung ist, sofern der physische Port (für den die Verbindung erstellt wird) dies zulässt.

> [!IMPORTANT]
> Unter Umständen müssen Sie die ExpressRoute-Verbindung neu erstellen, wenn nicht ausreichend Kapazität am vorhandenen Port verfügbar ist. Die Verbindung kann nicht aktualisiert werden, wenn an dieser Stelle keine zusätzliche Kapazität verfügbar ist.
>
> Es ist nicht möglich, die Bandbreite einer ExpressRoute-Verbindung ohne Störungen zu reduzieren. Ein Downgrade der Bandbreite erfordert, dass Sie die Bereitstellung der ExpressRoute-Verbindung aufheben und dann eine neue ExpressRoute-Verbindung bereitstellen.
> 
> 

#### <a name="resize-a-circuit"></a>Ändern der Größe einer Verbindung

Sobald Sie sich für die benötigte Größe entschieden haben, können Sie den folgenden Befehl verwenden, um die Größe der Verbindung anzupassen.

    Set-AzureDedicatedCircuitProperties -ServiceKey ********************************* -Bandwidth 1000

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Die Größe Ihrer Verbindung wurde bereits auf der Microsoft-Seite angepasst. Sie müssen sich an Ihren Konnektivitätsanbieter wenden, um die entsprechende Aktualisierung der Konfigurationen auf Anbieterseite vornehmen zu lassen. Beachten Sie, dass wir Ihnen ab diesem Zeitpunkt die aktualisierte Bandbreitenoption in Rechnung stellen.

Wenn beim Erhöhen der Verbindungsbandbreite der folgende Fehler angezeigt wird, reicht die restliche Bandbreite auf dem physischen Port, auf dem die vorhandene Verbindung erstellt ist, nicht aus. Sie müssen diese Verbindung löschen und eine neue Verbindung mit der erforderlichen Größe erstellen. 

    Set-AzureDedicatedCircuitProperties : InvalidOperation : Insufficient bandwidth available to perform this circuit
    update operation
    At line:1 char:1
    + Set-AzureDedicatedCircuitProperties -ServiceKey ********************* ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Set-AzureDedicatedCircuitProperties], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.SetAzureDedicatedCircuitPropertiesCommand


## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Aufheben der Bereitstellung und Löschen einer ExpressRoute-Verbindung

### <a name="considerations"></a>Überlegungen

* Sie müssen die Verknüpfung aller virtuellen Netzwerke mit der ExpressRoute-Verbindung aufheben, damit dieser Vorgang erfolgreich durchgeführt werden kann. Überprüfen Sie, ob noch virtuelle Netzwerke mit der Verbindung verknüpft sind, falls dieser Vorgang nicht erfolgreich ist.
* Wenn der Bereitstellungsstatus des ExpressRoute-Verbindungsdienstanbieters **Bereitstellung** oder **Bereitgestellt** lautet, arbeiten Sie mit Ihrem Dienstanbieter zusammen, um die Verbindungsbereitstellung auf Anbieterseite aufzuheben. Microsoft reserviert weiterhin Ressourcen für Sie und stellt Ihnen dies in Rechnung, bis der Dienstanbieter die Aufhebung der Verbindungsbereitstellung abgeschlossen hat und uns benachrichtigt.
* Wenn der Dienstanbieter die Bereitstellung der Verbindung aufgehoben hat (Bereitstellungsstatus des Dienstanbieters lautet **Nicht bereitgestellt**), können Sie die Verbindung löschen. Damit wird die Abrechnung für die Verbindung beendet.

#### <a name="delete-a-circuit"></a>Löschen einer Verbindung

Sie können die ExpressRoute-Verbindung löschen, indem Sie den folgenden Befehl ausführen:

    Remove-AzureDedicatedCircuit -ServiceKey "*********************************"



## <a name="next-steps"></a>Nächste Schritte
Führen Sie nach dem Erstellen Ihrer Verbindung folgende Vorgänge aus:

* [Erstellen und Ändern des Routings für Ihre ExpressRoute-Verbindung](expressroute-howto-routing-classic.md)
* [Verknüpfen Ihres virtuelles Netzwerks mit Ihrer ExpressRoute-Verbindung](expressroute-howto-linkvnet-classic.md)


