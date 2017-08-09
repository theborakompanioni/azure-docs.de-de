---
title: "Erstellen und Ändern einer Azure ExpressRoute-Verbindung: CLI | Microsoft-Dokumentation"
description: "Dieser Artikel beschreibt das Erstellen, Bereitstellen, Überprüfen, Aktualisieren, Löschen und Aufheben der Bereitstellung einer ExpressRoute-Verbindung mithilfe der CLI."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: anzaman;cherylmc
ms.translationtype: HT
ms.sourcegitcommit: 349fe8129b0f98b3ed43da5114b9d8882989c3b2
ms.openlocfilehash: 1a1c9a96b772868e2c832e9ff57874038c0db2d4
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

---
# <a name="create-and-modify-an-expressroute-circuit-using-cli"></a>Erstellen und Ändern einer ExpressRoute-Verbindung mit der CLI


Dieser Artikel beschreibt, wie eine Azure ExpressRoute-Verbindung mit der Befehlszeilenschnittstelle (CLI) erstellt wird. Außerdem wird veranschaulicht, wie Sie den Status überprüfen, Updates durchführen oder eine Verbindung löschen bzw. deren Bereitstellung aufheben. Wenn Sie eine andere Methode für die Arbeit mit ExpressRoute-Verbindungen verwenden möchten, können Sie einen Artikel aus der folgenden Liste auswählen:

> [!div class="op_single_selector"]
> * [Azure-Portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure-CLI](howto-circuit-cli.md)
> * [Video – Azure-Portal](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klassisch)](expressroute-howto-circuit-classic.md)
> 

## <a name="before-you-begin"></a>Voraussetzungen

* Installieren Sie als Vorbereitung die aktuelle Version der CLI-Befehle (2.0 oder höher). Informationen zum Installieren der CLI-Befehle finden Sie unter [Install Azure CLI 2.0](/cli/azure/install-azure-cli) (Installieren der Azure CLI 2.) und [Get Started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli) (Erste Schritte mit der Azure CLI 2.0).
* Lesen Sie vor Beginn der Konfiguration die Seiten zu den [Voraussetzungen](expressroute-prerequisites.md) und [Workflows](expressroute-workflows.md).

## <a name="create-and-provision-an-expressroute-circuit"></a>Erstellen und Bereitstellen einer ExpressRoute-Verbindung

### <a name="1-sign-in-to-your-azure-account-and-select-your-subscription"></a>1. Melden Sie sich bei Ihrem Azure-Konto an, und wählen Sie Ihr Abonnement aus.

Um mit der Konfiguration zu beginnen, melden Sie sich bei Ihrem Azure-Konto an. Verwenden Sie die folgenden Beispiele, um eine Verbindung herzustellen:

```azurecli
az login
```

Überprüfen Sie die Abonnements für das Konto.

```azurecli
az account list
```

Wählen Sie das Abonnement aus, für das eine ExpressRoute-Verbindung erstellt werden soll.

```azurecli
az account set --subscription "<subscription ID>"
```

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. Rufen Sie die Liste der unterstützten Anbieter, Standorte und Bandbreiten ab.

Vor dem Erstellen einer ExpressRoute-Verbindung benötigen Sie die Liste der unterstützten Konnektivitätsanbieter, Standorte und Bandbreitenoptionen. Der CLI-Befehl „az network express-route list-service-providers“ gibt folgende Informationen zurück, die Sie in den späteren Schritten verwenden:

```azurecli
az network express-route list-service-providers
```

Die Antwort ähnelt dem folgenden Beispiel:

```azurecli
[
  {
    "bandwidthsOffered": [
      {
        "offerName": "50Mbps",
        "valueInMbps": 50
      },
      {
        "offerName": "100Mbps",
        "valueInMbps": 100
      },
      {
        "offerName": "200Mbps",
        "valueInMbps": 200
      },
      {
        "offerName": "500Mbps",
        "valueInMbps": 500
      },
      {
        "offerName": "1Gbps",
        "valueInMbps": 1000
      },
      {
        "offerName": "2Gbps",
        "valueInMbps": 2000
      },
      {
        "offerName": "5Gbps",
        "valueInMbps": 5000
      },
      {
        "offerName": "10Gbps",
        "valueInMbps": 10000
      }
    ],
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/expressRouteServiceProviders/",
    "location": null,
    "name": "AARNet",
    "peeringLocations": [
      "Melbourne",
      "Sydney"
    ],
    "provisioningState": "Succeeded",
    "resourceGroup": "",
    "tags": null,
    "type": "Microsoft.Network/expressRouteServiceProviders"
  },
```

Überprüfen Sie die Antwort dahingehend, ob Ihr Konnektivitätsanbieter aufgeführt ist. Notieren Sie sich die folgenden Informationen, da Sie diese beim Erstellen einer Verbindung benötigen:

* Name
* PeeringLocations
* BandwidthsOffered

Sie können nun eine ExpressRoute-Verbindung erstellen.

### <a name="3-create-an-expressroute-circuit"></a>3. Erstellen Sie eine ExpressRoute-Verbindung.

> [!IMPORTANT]
> Ihre ExpressRoute-Verbindung wird von dem Moment an berechnet, in dem ein Dienstschlüssel ausgegeben wird. Führen Sie diesen Vorgang durch, wenn der Konnektivitätsanbieter bereit ist, die Verbindung bereitzustellen.
> 
> 

Wenn Sie noch keine Ressourcengruppe besitzen, müssen Sie zuerst eine erstellen, bevor Sie Ihre ExpressRoute-Verbindung erstellen. Sie können mithilfe des folgenden Befehls eine Ressourcengruppe erstellen:

```azurecli
az group create -n ExpressRouteResourceGroup -l "West US"
```

Das folgende Beispiel zeigt, wie Sie eine ExpressRoute-Verbindung mit 200 MBit/s über Equinix im Silicon Valley herstellen können. Wenn Sie einen anderen Anbieter und andere Einstellungen verwenden, ersetzen Sie bei Ihrer Anforderung die entsprechenden Informationen. 

Stellen Sie sicher, dass Sie die richtige SKU-Ebene und die richtige SKU-Familie angeben.

* Die SKU-Ebene bestimmt, ob ein ExpressRoute Standard- oder ein ExpressRoute Premium-Add-On aktiviert wird. Sie können „Standard“ für die Standard-SKU bzw. „Premium“ für das Premium-Add-On angeben.
* Die SKU-Familie bestimmt den Abrechnungstyp. Sie können „Metereddata“ für einen Volumentarif und „Unlimiteddata“ für einen unbegrenzten Datentarif auswählen. Sie können den Abrechnungstyp von „Metereddata“ in „Unlimiteddata“ ändern, nicht jedoch umgekehrt („Unlimiteddata“ in „Metereddata“).


Ihre ExpressRoute-Verbindung wird von dem Moment an berechnet, in dem ein Dienstschlüssel ausgegeben wird. Nachfolgend sehen Sie eine Beispielanforderung für einen neuen Dienstschlüssel:

```azurecli
az network express-route create --bandwidth 200 -n MyCircuit --peering-location "Silicon Valley" -g ExpressRouteResourceGroup --provider "Equinix" -l "West US" --sku-family MeteredData --sku-tier Standard
```

Die Antwort enthält den Dienstschlüssel.

### <a name="4-list-all-expressroute-circuits"></a>4. Listen Sie alle ExpressRoute-Verbindungen auf.

Um eine Liste mit allen von Ihnen erstellten ExpressRoute-Verbindungen abzurufen, führen Sie den Befehl „az network express-route list“ aus. Sie können diese Informationen jederzeit mithilfe dieses Befehls abrufen. Um alle Verbindungen aufzulisten, führen Sie den Aufruf ohne Parameter durch.

```azurecli
az network express-route list
```

Ihr Dienstschlüssel wird im Feld *ServiceKey* der Antwort aufgeführt.

```azurecli
"allowClassicOperations": false,
"authorizations": [],
"circuitProvisioningState": "Enabled",
"etag": "W/\"1262c492-ffef-4a63-95a8-a6002736b8c4\"",
"gatewayManagerEtag": null,
"id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit",
"location": "westus",
"name": "MyCircuit",
"peerings": [],
"provisioningState": "Succeeded",
"resourceGroup": "ExpressRouteResourceGroup",
"serviceKey": "1d05cf70-1db5-419f-ad86-1ca62c3c125b",
"serviceProviderNotes": null,
"serviceProviderProperties": {
  "bandwidthInMbps": 200,
  "peeringLocation": "Silicon Valley",
  "serviceProviderName": "Equinix"
},
"serviceProviderProvisioningState": "NotProvisioned",
"sku": {
  "family": "UnlimitedData",
  "name": "Standard_MeteredData",
  "tier": "Standard"
},
"tags": null,
"type": "Microsoft.Network/expressRouteCircuits]
```

Ausführliche Beschreibungen aller Parameter erhalten Sie, wenn Sie den Befehl mit dem Parameter „-h“ ausführen.

```azurecli
az network express-route list -h
```

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. Senden Sie den Dienstschlüssel zur Bereitstellung an Ihren Konnektivitätsanbieter.

„ServiceProviderProvisioningState“ stellt Informationen zum aktuellen Zustand der Bereitstellung auf Dienstanbieterseite bereit. Der Status gibt den Zustand auf der Microsoft-Seite an. Weitere Informationen finden Sie im Artikel [Workflow](expressroute-workflows.md#expressroute-circuit-provisioning-states).

Wenn Sie eine neue ExpressRoute-Verbindung erstellen, weist die Verbindung folgenden Zustand auf:

```azurecli
"serviceProviderProvisioningState": "NotProvisioned"
"circuitProvisioningState": "Enabled"
```

Die Verbindung wechselt in den folgenden Zustand, wenn sie vom Konnektivitätsanbieter für Sie aktiviert wird:

```azurecli
"serviceProviderProvisioningState": "Provisioning"
"circuitProvisioningState": "Enabled"
```

Damit Sie eine ExpressRoute-Verbindung verwenden können, muss sie sich im folgenden Zustand befinden:

```azurecli
"serviceProviderProvisioningState": "Provisioned"
"circuitProvisioningState": "Enabled
```

### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. Überprüfen Sie regelmäßig den Status und Zustand des Verbindungsschlüssels.

Durch das Überprüfen des Status und des Zustands des Verbindungsschlüssels werden Sie informiert, wann Ihr Anbieter Ihre Verbindung aktiviert hat. Sobald die Verbindung konfiguriert wurde, zeigt „ServiceProviderProvisioningState“ wie im folgenden Beispiel den Status „Provisioned“ an:

```azurecli
az network express-route show --resource-group ExpressRouteResourceGroup --name MyCircuit
```

Die Antwort ähnelt dem folgenden Beispiel:

```azurecli
"allowClassicOperations": false,
"authorizations": [],
"circuitProvisioningState": "Enabled",
"etag": "W/\"1262c492-ffef-4a63-95a8-a6002736b8c4\"",
"gatewayManagerEtag": null,
"id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit",
"location": "westus",
"name": "MyCircuit",
"peerings": [],
"provisioningState": "Succeeded",
"resourceGroup": "ExpressRouteResourceGroup",
"serviceKey": "1d05cf70-1db5-419f-ad86-1ca62c3c125b",
"serviceProviderNotes": null,
"serviceProviderProperties": {
  "bandwidthInMbps": 200,
  "peeringLocation": "Silicon Valley",
  "serviceProviderName": "Equinix"
},
"serviceProviderProvisioningState": "NotProvisioned",
"sku": {
  "family": "UnlimitedData",
  "name": "Standard_MeteredData",
  "tier": "Standard"
},
"tags": null,
"type": "Microsoft.Network/expressRouteCircuits]
```

### <a name="7-create-your-routing-configuration"></a>7. Erstellen Sie die Routingkonfiguration.

Eine detaillierte Anleitung zum Erstellen und Ändern von Verbindungspeerings finden Sie im Artikel [Erstellen und Ändern des Routings für eine ExpressRoute-Verbindung](howto-routing-cli.md) .

> [!IMPORTANT]
> Diese Anweisungen gelten nur für Verbindungen, die über Dienstanbieter erstellt wurden, von denen Layer 2-Konnektivitätsdienste angeboten werden. Wenn Sie einen Dienstanbieter nutzen, der verwaltete Layer 3-Dienste anbietet (meist ein IP-VPN, z.B. MPLS), übernimmt Ihr Konnektivitätsanbieter die Konfiguration und Verwaltung des Routings für Sie.
> 
> 

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. Verknüpfen eines virtuellen Netzwerks mit einer ExpressRoute-Verbindung

Verknüpfen Sie anschließend ein virtuelles Netzwerk mit Ihrer ExpressRoute-Verbindung. Lesen Sie den Artikel [Verknüpfen von virtuellen Netzwerken mit ExpressRoute-Verbindungen](howto-linkvnet-cli.md).

## <a name="modify"></a>Ändern einer ExpressRoute-Verbindung

Sie können bestimmte Eigenschaften einer ExpressRoute-Verbindung ändern, ohne die Konnektivität zu beeinträchtigen. Sie können ohne Ausfallzeiten folgende Änderungen vornehmen:

* Sie können ein ExpressRoute Premium-Add-On für Ihre ExpressRoute-Verbindung aktivieren oder deaktivieren.
* Sie können die Bandbreite der ExpressRoute-Verbindung erhöhen, sofern Kapazität am Port vorhanden ist. Ein Downgrade der Bandbreite einer Verbindung wird allerdings nicht unterstützt. 
* Sie können den Abrechnungstarif von „Volumentarif“ in „Unbegrenzte Daten“ ändern. Eine Änderung des Abrechnungstarifs von „Unbegrenzte Daten“ in „Volumentarif“ wird jedoch nicht unterstützt.
* Sie können die Option *Klassische Vorgänge zulassen*aktivieren und deaktivieren.

Weitere Informationen zu Grenzwerten und Beschränkungen finden Sie unter [ExpressRoute – FAQs](expressroute-faqs.md).

### <a name="to-enable-the-expressroute-premium-add-on"></a>Aktivieren des ExpressRoute Premium-Add-Ons

Für Ihre bereits vorhandene Verbindung können Sie das ExpressRoute Premium-Add-On mit dem folgenden Befehl aktivieren:

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-tier Premium
```

Für die Verbindung sind nun die Features des ExpressRoute Premium-Add-Ons aktiviert. Die Gebühren für das Premium-Add-On werden ab der erfolgreichen Ausführung des Befehls erhoben.

### <a name="to-disable-the-expressroute-premium-add-on"></a>Deaktivieren des ExpressRoute Premium-Add-Ons

> [!IMPORTANT]
> Bei diesem Vorgang kann ein Fehler auftreten, wenn Sie Ressourcen verwenden, die die zulässige Menge für die Standardverbindung überschreiten.
> 
> 

Vor der Deaktivierung des ExpressRoute Premium-Add-Ons sollten Sie folgende Kriterien beachten:

* Bevor Sie ein Downgrade von Premium auf Standard durchführen, müssen Sie sicherstellen, dass die Anzahl von virtuellen Netzwerken, die mit der Verbindung verknüpft sind, kleiner als zehn ist. Andernfalls verursacht Ihre Updateanforderung einen Fehler, und Microsoft berechnet Ihnen Premium-Gebühren.
* Sie müssen die Verknüpfung für alle virtuellen Netzwerke in anderen geopolitischen Regionen aufheben. Andernfalls verursacht Ihre Updateanforderung einen Fehler, und Microsoft berechnet Ihnen Premium-Gebühren.
* Ihre Routentabelle muss für das private Peering weniger als 4.000 Routen aufweisen. Wenn die Routentabelle mehr als 4.000 Routen enthält, wird die BGP-Sitzung gelöscht. Die Sitzung wird erst wieder aktiviert, wenn die Anzahl der angekündigte Präfixe unter 4.000 liegt.

Für Ihre bereits vorhandene Verbindung können Sie das ExpressRoute Premium-Add-On anhand des folgenden Beispiels deaktivieren:

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-tier Standard
```

### <a name="to-update-the-expressroute-circuit-bandwidth"></a>Aktualisieren der Bandbreite für die ExpressRoute-Verbindung

Informationen zu unterstützten Bandbreitenoptionen für Ihren Anbieter finden Sie unter [ExpressRoute – FAQs](expressroute-faqs.md). Sie können jede Größe auswählen, die die Größe der vorhandenen Verbindung übersteigt.

> [!IMPORTANT]
> Wenn nicht ausreichend Kapazität am vorhandenen Port verfügbar ist, müssen Sie die ExpressRoute-Verbindung unter Umständen neu erstellen. Die Verbindung kann nicht aktualisiert werden, wenn an dieser Stelle keine zusätzliche Kapazität verfügbar ist.
>
> Es ist nicht möglich, die Bandbreite einer ExpressRoute-Verbindung ohne Störungen zu reduzieren. Ein Downgrade der Bandbreite erfordert, dass Sie die Bereitstellung der ExpressRoute-Verbindung aufheben und dann eine neue ExpressRoute-Verbindung bereitstellen.
>

Sobald Sie sich für die benötigte Größe entschieden haben, verwenden Sie den folgenden Befehl, um die Größe der Verbindung anzupassen:

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --bandwidth 1000
```

Die Größe Ihrer Verbindung wird auf der Microsoft-Seite angepasst. Anschließend müssen Sie sich an Ihren Konnektivitätsanbieter wenden, um das entsprechende Update der Konfigurationen auf Anbieterseite vornehmen zu lassen. Nachdem Sie diese Benachrichtigung gesendet haben, beginnt Microsoft, die Gebühren für die aktualisierte Bandbreitenoption abzurechnen.

### <a name="to-move-the-sku-from-metered-to-unlimited"></a>Ändern der SKU von einem Volumentarif zu einer Datenflatrate

Sie können die SKU einer ExpressRoute-Verbindung anhand des folgenden Beispiels ändern:

```azurecli
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-family UnlimitedData
```

### <a name="to-control-access-to-the-classic-and-resource-manager-environments"></a>Steuern des Zugriffs auf die klassische und die Resource Manager-Umgebung

Lesen Sie die Anweisungen unter [Verschieben von ExpressRoute-Verbindungen vom klassischen zum Resource Manager-Bereitstellungsmodell](expressroute-howto-move-arm.md).

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Aufheben der Bereitstellung und Löschen einer ExpressRoute-Verbindung

Um die Bereitstellung einer ExpressRoute-Verbindung aufzuheben und diese zu löschen, stellen Sie sicher, dass Sie folgende Kriterien berücksichtigt haben:

* Sie müssen die Verknüpfung aller virtuellen Netzwerke mit der ExpressRoute-Verbindung aufheben. Falls dieser Vorgang nicht erfolgreich ist, überprüfen Sie, ob noch virtuelle Netzwerke mit der Verbindung verknüpft sind.
* Wenn der Bereitstellungszustand des ExpressRoute-Verbindungsdienstanbieters **Provisioning** oder **Provisioned** lautet, arbeiten Sie mit Ihrem Dienstanbieter zusammen, um die Verbindungsbereitstellung auf Anbieterseite aufzuheben. Microsoft reserviert weiterhin Ressourcen für Sie und stellt Ihnen dies in Rechnung, bis der Dienstanbieter die Aufhebung der Verbindungsbereitstellung abgeschlossen hat und uns benachrichtigt.
* Wenn der Dienstanbieter die Bereitstellung der Verbindung aufgehoben hat, können Sie die Verbindung löschen. Nachdem die Bereitstellung einer Verbindung aufgehoben wurde, ist der Bereitstellungszustand des Dienstanbieters auf **Not provisioned** festgelegt. Damit wird die Abrechnung für die Verbindung beendet.

Sie können die ExpressRoute-Verbindung löschen, indem Sie den folgenden Befehl ausführen:

```azurecli
az network express-route delete  -n MyCircuit -g ExpressRouteResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

Führen Sie nach dem Erstellen Ihrer Verbindung folgende Aufgaben durch:

* [Erstellen und Ändern des Routings für Ihre ExpressRoute-Verbindung](howto-routing-cli.md)
* [Verknüpfen Ihres virtuelles Netzwerks mit Ihrer ExpressRoute-Verbindung](howto-linkvnet-cli.md)
