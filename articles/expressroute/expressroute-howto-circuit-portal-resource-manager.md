<properties
   pageTitle="Erstellen und Ändern einer ExpressRoute-Verbindung mit Resource Manager und dem Azure-Portal | Microsoft Azure"
   description="Dieser Artikel beschreibt das Erstellen, Bereitstellen, Überprüfen, Aktualisieren, Löschen und Aufheben der Bereitstellung einer ExpressRoute-Verbindung."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/15/2016"
   ms.author="cherylmc"/>

# Erstellen und Ändern einer ExpressRoute-Verbindung

> [AZURE.SELECTOR]
[Azure Portal - Resource Manager](expressroute-howto-circuit-portal-resource-manager.md)
[PowerShell - Resource Manager](expressroute-howto-circuit-arm.md)
[PowerShell - Classic](expressroute-howto-circuit-classic.md)

In diesem Artikel wird beschrieben, wie Sie eine Azure ExpressRoute-Verbindung mithilfe des Azure-Portals und des Azure Resource Manager-Bereitstellungsmodells erstellen. In den folgenden Schritten wird auch veranschaulicht, wie Sie den Status der Verbindung prüfen, aktualisieren, löschen oder deren Bereitstellung aufheben.

**Informationen zu Azure-Bereitstellungsmodellen**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]


## Voraussetzungen


- Lesen Sie vor Beginn der Konfiguration die Seiten zu den [Voraussetzungen](expressroute-prerequisites.md) und [Workflows](expressroute-workflows.md).
- Stellen Sie sicher, dass Sie auf das [Azure-Portal](https://portal.azure.com) zugreifen können.
- Stellen Sie sicher, dass Sie über die notwendigen Berechtigungen verfügen, um neue Netzwerkressourcen zu erstellen. Wenden Sie sich an Ihren Kontoadministrator, wenn Sie nicht über die richtigen Berechtigungen verfügen.

## Erstellen und Bereitstellen einer ExpressRoute-Verbindung

### 1\. Melden Sie sich auf dem Azure-Portal an.

Navigieren Sie in einem Browser zum [Azure-Portal](http://portal.azure.com), und melden Sie sich mit Ihrem Azure-Konto an.

### 2\. Erstellen Sie eine neue ExpressRoute-Verbindung.

>[AZURE.IMPORTANT] Ihre ExpressRoute-Verbindung wird von dem Moment an berechnet, in dem ein Dienstschlüssel ausgegeben wird. Stellen Sie sicher, dass Sie diesen Vorgang ausführen, sobald der Konnektivitätsanbieter dazu bereit ist, die Verbindung bereitzustellen.

1. Sie können eine ExpressRoute-Verbindung erstellen, indem Sie die Option zum Erstellen einer neuen Ressource auswählen. Klicken Sie auf **Neu** > **Netzwerk** > **ExpressRoute**, wie in der Abbildung unten gezeigt:

	![Erstellen Sie eine ExpressRoute-Verbindung.](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit1.png)

2. Nachdem Sie auf **ExpressRoute** geklickt haben, wird das Blatt **ExpressRoute-Verbindung erstellen** angezeigt. Wenn Sie auf diesem Blatt Werte eintragen, stellen Sie sicher, dass Sie den richtigen SKU-Tarif und die richtige Datenabrechnung angeben.

	- Der **Tarif** bestimmt, ob ein ExpressRoute Standard- oder ein ExpressRoute Premium-Add-On aktiviert wird. Sie können **Standard** für die Standard-SKU bzw. **Premium** für das Premium-Add-On angeben.

	- Die **Datenabrechnung** bestimmt den Abrechnungstyp. Sie können **Metered** für einen Volumentarif und **Unlimited** für einen Tarif mit Datenflatrate auswählen. Beachten Sie, dass Sie den Abrechnungstyp von **Metered** in **Unlimited** ändern können, nicht jedoch umgekehrt (**Unlimited** in **Metered**).

	![Konfigurieren von SKU-Tarif und Datenmessung](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit2.png)


### 3\. Anzeigen von Verbindungen und Eigenschaften

**Anzeigen aller Verbindungen**

Sie können alle erstellten Verbindungen anzeigen, indem Sie im Menü auf der linken Seite **Alle Ressourcen** auswählen.
	
![Verbindungen anzeigen](./media/expressroute-howto-circuit-portal-resource-manager/listresource.png)

**Anzeigen der Eigenschaften**

	You can view the properties of the circuit by selecting it. On this blade, note the service key for the circuit. You must copy the circuit key for your circuit and pass it down to the service provider to complete the provisioning process. The circuit key is specific to your circuit.

![Eigenschaften anzeigen](./media/expressroute-howto-circuit-portal-resource-manager/listproperties1.png)


### 4\. Senden Sie den Dienstschlüssel zur Bereitstellung an Ihren Konnektivitätsanbieter.

Auf diesem Blatt bietet der **Anbieterstatus** Informationen zum aktuellen Zustand der Bereitstellung auf Dienstanbieterseite. Mit **Schaltkreisstatus** wird der Zustand auf Microsoft-Seite angegeben. Weitere Informationen zu den Bereitstellungszuständen für eine Verbindung finden Sie im Artikel [Workflows](expressroute-workflows.md#expressroute-circuit-provisioning-states).

Wenn Sie eine neue ExpressRoute-Verbindung erstellen, weist die Verbindung folgenden Zustand auf:

Anbieterstatus: Nicht bereitgestellt<BR> Schaltkreisstatus: Aktiviert

![Bereitstellung initiieren](./media/expressroute-howto-circuit-portal-resource-manager/viewstatus.png)

Die Verbindung wechselt in den folgenden Zustand, wenn sie vom Konnektivitätsanbieter für Sie aktiviert wird:

Anbieterstatus: Wird bereitgestellt<BR> Schaltkreisstatus: Aktiviert

Damit Sie eine ExpressRoute-Verbindung verwenden können, muss sie sich im folgenden Zustand befinden:

Anbieterstatus: Bereitgestellt<BR> Schaltkreisstatus: Aktiviert


### 5\. Überprüfen Sie regelmäßig den Status und Zustand des Verbindungsschlüssels.

Sie können die Eigenschaften der gewünschten Verbindung anzeigen, indem Sie die Verbindung auswählen. Überprüfen Sie den **Anbieterstatus**, und stellen Sie sicher, dass er in **Bereitgestellt** geändert wurde, bevor Sie fortfahren.


![Verbindungs- und Anbieterstatus](./media/expressroute-howto-circuit-portal-resource-manager/viewstatusprovisioned.png)


### 6\. Erstellen Sie die Routingkonfiguration.

Eine Schritt-für-Schritt-Anleitung zum Erstellen und Ändern von Verbindungspeerings finden Sie im Artikel [Routingkonfiguration für ExpressRoute-Verbindung](expressroute-howto-routing-portal-resource-manager.md).

>[AZURE.IMPORTANT] Diese Anweisungen gelten nur für Verbindungen, die über Dienstanbieter erstellt wurden, von denen Layer 2-Konnektivitätsdienste angeboten werden. Wenn Sie einen Dienstanbieter nutzen, der verwaltete Layer 3-Dienste anbietet (meist ein IP VPN, z.B. MPLS), übernimmt Ihr Konnektivitätsanbieter die Konfiguration und Verwaltung des Routings für Sie.

### 7\. Verknüpfen eines virtuellen Netzwerks mit einer ExpressRoute-Verbindung

Verknüpfen Sie anschließend ein virtuelles Netzwerk mit Ihrer ExpressRoute-Verbindung. Lesen Sie den Artikel [Verknüpfen von virtuellen Netzwerken mit ExpressRoute-Verbindungen](expressroute-howto-linkvnet-arm.md), wenn Sie mit dem Resource Manager-Bereitstellungsmodell arbeiten.

## Abrufen des Status einer ExpressRoute-Verbindung

Sie können den Status einer Verbindung anzeigen, indem Sie die Verbindung auswählen.

![Status einer ExpressRoute-Verbindung](./media/expressroute-howto-circuit-portal-resource-manager/listproperties1.png)


## Ändern einer ExpressRoute-Verbindung

Sie können bestimmte Eigenschaften einer ExpressRoute-Verbindung ändern, ohne die Konnektivität zu beeinträchtigen. Zurzeit können Sie Eigenschaften von ExpressRoute-Verbindungen nicht über das Azure-Portal ändern. Sie können jedoch PowerShell verwenden, um Verbindungseigenschaften zu bearbeiten. Informationen dazu finden Sie im Abschnitt [Ändern einer ExpressRoute-Verbindung mit PowerShell](expressroute-howto-circuit-arm.md#modify).

Sie können folgende Aktionen ausführen, ohne Ausfallzeiten zu verursachen:

- Aktivieren oder deaktivieren Sie ein ExpressRoute Premium-Add-On für Ihre ExpressRoute-Verbindung.

- Erhöhen Sie die Bandbreite Ihrer ExpressRoute-Verbindung. Beachten Sie, dass ein Downgrade der Bandbreite einer Verbindung nicht unterstützt wird.

- Sie können den Abrechnungsplan von „Volumentarif“ zu „Datenflatrate“ ändern. Beachten Sie, dass eine Änderung des Abrechnungsplans von „Datenflatrate“ zu „Volumentarif“ nicht unterstützt wird.

-  Sie können die Option **Klassische Vorgänge zulassen** aktivieren und deaktivieren.

Weitere Informationen zu Beschränkungen und Grenzwerten finden Sie unter [ExpressRoute – Häufig gestellte Fragen](expressroute-faqs.md).


## Löschen und Aufheben der Bereitstellung einer ExpressRoute-Verbindung

Sie können Ihre ExpressRoute-Verbindung löschen. Wählen Sie dazu das Symbol **Löschen** aus. Beachten Sie Folgendes:

- Sie müssen die Verknüpfung aller virtuellen Netzwerke mit der ExpressRoute-Verbindung aufheben. Falls dieser Vorgang nicht erfolgreich ist, überprüfen Sie, ob noch virtuelle Netzwerke mit der Verbindung verknüpft sind.

- Wenn der Bereitstellungszustand des Dienstanbieters für die ExpressRoute-Verbindung „Aktiviert“ lautet, wechselt der Status vom aktivierten Zustand zu **Wird deaktiviert**. Arbeiten Sie mit Ihrem Service Provider zusammen, um die Bereitstellung der Verbindung auf Anbieterseite aufzuheben. Microsoft reserviert weiterhin Ressourcen für Sie und stellt Ihnen dies in Rechnung, bis der Dienstanbieter die Aufhebung der Verbindungsbereitstellung abgeschlossen hat und uns benachrichtigt.

- Wenn der Dienstanbieter die Bereitstellung der Verbindung aufgehoben hat (Bereitstellungsstatus des Service Providers lautet **Nicht bereitgestellt**), bevor Sie das obige Cmdlet ausführen, führen wir die Aufhebung der Verbindungsbereitstellung durch und stellen Ihnen keine Gebühren mehr in Rechnung.

## Nächste Schritte

Führen Sie nach dem Erstellen Ihrer Verbindung folgende Vorgänge aus:

- [Erstellen und Ändern des Routings für Ihre ExpressRoute-Verbindung](expressroute-howto-routing-portal-resource-manager.md)
- [Verknüpfen Ihres virtuelles Netzwerks mit Ihrer ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md)

<!---HONumber=AcomDC_0518_2016-->