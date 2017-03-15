---
title: "Einführung in Azure Network Watcher | Microsoft Docs"
description: "Diese Seite enthält eine Übersicht über den Network Watcher-Dienst für die Überwachung und Visualisierung von verbundenen Ressourcen in Azure."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 14bc2266-99e3-42a2-8d19-bd7257fec35e
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 3e7595baa26ba9eebfcb8a2fd5c2744d9c0fbfcb
ms.openlocfilehash: 03dbde64aefc12145a589d8b741c67865162b9b5
ms.lasthandoff: 02/23/2017

---

# <a name="azure-network-monitoring-overview"></a>Übersicht über die Azure-Netzwerküberwachung

Kunden erstellen ein End-to-End-Netzwerk in Azure und orchestrieren und kombinieren dabei verschiedene Netzwerkressourcen wie z.B. VNET, ExpressRoute, Application Gateway, Load Balancer u.v.m. Überwachung steht für jede der Netzwerkressourcen zur Verfügung. Wir bezeichnen diese Art der Überwachung als Überwachung auf Ressourcenebene.

Das End-to-End-Netzwerk kann über komplexe Konfigurationen und Interaktionen zwischen den Ressourcen verfügen. Dies führt zu komplexen Szenarien, die eine szenariobasierte Überwachung mit Network Watcher erfordern.

Dieser Artikel behandelt die Überwachung auf Szenario- und Ressourcenebene. Die Netzwerküberwachung in Azure ist umfassend und unterteilt sich in zwei übergeordnete Kategorien:

* [**Network Watcher:**](#network-watcher) Die szenariobasierte Überwachung wird mit den Features in Network Watcher bereitgestellt. Dieser Dienst umfasst die Paketerfassung, „Nächster Hop“, die IP-Datenflussüberprüfung, die Sicherheitsgruppenansicht und NSG-Datenflussprotokolle. Die Überwachung auf Szenarioebene bietet im Gegensatz zur Überwachung einzelner Netzwerkressourcen eine End-to-End-Ansicht der Netzwerkressourcen.
* [**Ressourcenüberwachung:**](#network-resource-level-monitoring) Die Überwachung auf Ressourcenebene umfasst vier Features: Diagnoseprotokolle, Metriken, Problembehandlung und Resource Health. Alle diese Features arbeiten auf Netzwerkressourcenebene.

## <a name="network-watcher"></a>Network Watcher

Network Watcher ist ein regionaler Dienst, mit dem Sie Bedingungen auf der Ebene von Netzwerkszenarien in Azure überwachen und diagnostizieren können. Die Tools zur Netzwerkdiagnose und -visualisierung von Network Watcher helfen Ihnen dabei, Ihr Netzwerk in Azure zu verstehen, Diagnosen durchzuführen und Einblicke zu gewinnen.

Network Watcher verfügt derzeit über die folgenden Funktionen:

* **[Topologie:](network-watcher-topology-overview.md)** bietet eine Ansicht auf Netzwerkebene mit verschiedenen Verbindungen und Beziehungen zwischen Netzwerkressourcen innerhalb einer Ressourcengruppe
* **[Variable Paketerfassung:](network-watcher-packet-capture-overview.md)** erfasst am virtuellen Computer ein- und ausgehende Paketdaten Erweiterte Filteroptionen und präzise Steuerelemente erlauben z.B. das Festlegen von Zeitraum und Größeneinschränkungen und bieten damit viel Flexibilität. Die Paketdaten können in einem Blobspeicher oder auf dem lokalen Datenträger im CAP-Format gespeichert werden.
* **[IP-Datenflussüberprüfung:](network-watcher-ip-flow-verify-overview.md)** überprüft basierend auf 5-Tupel-Paketparametern (Ziel-IP, Quell-IP, Zielport, Quellport und Protokoll) anhand der Datenflussinformationen, ob ein Paket zugelassen oder verweigert wird. Wenn das Paket durch eine Sicherheitsgruppe verweigert wird, werden die Namen der Regel und der Gruppe, die das Paket verweigert haben, zurückgegeben.
* **[Nächster Hop:](network-watcher-next-hop-overview.md)** ermittelt den nächsten Hop für Pakete, die im Azure-Netzwerkfabric weitergeleitet werden, sodass Sie eine Diagnose auf falsch konfigurierte benutzerdefinierte Routen durchführen können.
* **[Sicherheitsgruppenansicht:](network-watcher-security-group-view-overview.md)** ruft die geltenden und angewendeten Sicherheitsregeln ab, die auf einen virtuellen Computer angewendet werden.
* **[NSG-Datenflussprotokollierung:](network-watcher-nsg-flow-logging-overview.md)** Mithilfe von Datenflussprotokollen für Netzwerksicherheitsgruppen können Sie Protokolle zum Datenverkehr erfassen, der von den Sicherheitsregeln in der Gruppe zugelassen oder verweigert wird. Der Datenfluss wird durch 5-Tupel-Informationen definiert: Quell-IP-Adresse, Ziel-IP-Adresse, Quellport, Zielport und Protokoll.
* **[Problembehandlung für Virtual Network-Gateways und -Verbindungen:](network-watcher-troubleshoot-manage-rest.md)** bietet die Möglichkeit zum Beheben von Problemen, die bei Virtual Network-Gateways und -Verbindungen auftreten können.
* **[Grenzwerte für Netzwerkabonnements:](#network-subscription-limits)** ermöglicht die Anzeige der Verwendung von Netzwerkressourcen mit bestimmten Grenzwerten.
* **[Konfigurieren von Diagnoseprotokollen:](#diagnostic-logs)** stellt einen zentralen Bereich für das Aktivieren oder Deaktivieren von Diagnoseprotokollen für Netzwerkressourcen in einer Ressourcengruppe bereit.

### <a name="role-based-access-control-rbac-in-network-watcher"></a>Rollenbasierte Zugriffssteuerung (RBAC) in Network Watcher

Network Watcher verwendet das [Azure-Modell zur rollenbasierten Zugriffssteuerung (RBAC)](../active-directory/role-based-access-control-what-is.md). Die folgende Berechtigung ist für Network Watcher in der Vorschau erforderlich. Sie müssen unbedingt sicherstellen, dass die Rolle zum Initiieren von Network Watcher-APIs oder zum Verwenden von Network Watcher über das Portal über die erforderlichen Zugriffsberechtigungen verfügt.

|Ressource| Berechtigung|
|---|---|
|Microsoft.Storage/ |Lesen|
|Microsoft.Authorization/| Lesen|
|Microsoft.Resources/subscriptions/resourceGroups/| Lesen|
|Microsoft.Storage/storageAccounts/listServiceSas/ | Aktion|
|Microsoft.Storage/storageAccounts/listAccountSas/ |Aktion|
|Microsoft.Storage/storageAccounts/listKeys/ | Aktion|
|Microsoft.Compute/virtualMachines/ |Lesen|
|Microsoft.Compute/virtualMachines/ |Schreiben|
|Microsoft.Compute/virtualMachineScaleSets/ |Lesen|
|Microsoft.Compute/virtualMachineScaleSets/ |Schreiben|
|Microsoft.Network/networkWatchers/packetCaptures/| Lesen|
|Microsoft.Network/networkWatchers/packetCaptures/| Schreiben|
|Microsoft.Network/networkWatchers/packetCaptures/| Löschen|
|Microsoft.Network/networkWatchers/ |Schreiben|
|Microsoft.Network/networkWatchers/| Lesen|
|Microsoft.Insights/alertRules/ |*|
|Microsoft.Support/| *|

### <a name="network-subscription-limits"></a>Grenzwerte für Netzwerkabonnements

Grenzwerte für Netzwerkabonnements zeigen Ihnen Details zur Verwendung der einzelnen Netzwerkressourcen in einem Abonnement und einer Region im Vergleich zur Anzahl der maximal verfügbaren Ressourcen an.

![Grenzwert für Netzwerkabonnements][nsl]

## <a name="network-resource-level-monitoring"></a>Netzwerküberwachung auf Ressourcenebene

Die folgenden Features stehen für die Überwachung auf Ressourcenebene zur Verfügung:

### <a name="audit-log"></a>Überwachungsprotokoll

Vorgänge, die im Rahmen der Konfiguration von Netzwerken durchgeführt werden, werden protokolliert. Diese Protokolle können im Azure-Portal angezeigt oder mithilfe von Microsoft-Tools wie Power BI oder Drittanbietertools abgerufen werden. Die Überwachungsprotokolle sind über das Verwaltungsportal, PowerShell, die Befehlszeilenschnittstelle und die Rest-API verfügbar. Weitere Informationen zu Überwachungsprotokollen finden Sie unter [Überwachen von Vorgängen mit Resource Manager](../resource-group-audit.md).

Überwachungsprotokolle stehen für Vorgänge zur Verfügung, die für alle Netzwerkressourcen durchgeführt werden.

### <a name="metrics"></a>Metriken

Metriken sind Leistungsmessungen und -indikatoren, die über einen Zeitraum gesammelt werden. Zurzeit sind Metriken für Application Gateway verfügbar. Metriken können verwendet werden, um basierend auf Schwellenwerten Warnungen auszulösen. Unter [Application Gateway-Diagnose](../application-gateway/application-gateway-diagnostics.md) erfahren Sie, wie Metriken verwendet werden können, um Warnungen zu erstellen.

![Metrikenansicht][metrics]

### <a name="diagnostic-logs"></a>Diagnoseprotokolle

Regelmäßige und spontane Ereignisse werden von Netzwerkressourcen erstellt, in Speicherkonten protokolliert und dann an Event Hub oder Log Analytics gesendet. Diese Protokolle geben Einblick in die Integrität einer Ressource. Diese Protokolle können in Tools wie Power BI und Log Analytics angezeigt werden. Informationen zum Anzeigen von Diagnoseprotokollen finden Sie unter [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md).

Diagnoseprotokolle sind für [Load Balancer](../load-balancer/load-balancer-monitor-log.md), [Netzwerksicherheitsgruppen](../virtual-network/virtual-network-nsg-manage-log.md), Routen und [Application Gateway](../application-gateway/application-gateway-diagnostics.md) verfügbar.

Network Watcher bietet eine Ansicht der Diagnoseprotokolle. Diese Ansicht enthält alle Netzwerkressourcen, die die Diagnoseprotokollierung unterstützen. In dieser Ansicht können Sie Netzwerkressourcen einfach und schnell aktivieren und deaktivieren.

![Protokolle][logs]

### <a name="troubleshooting"></a>Problembehandlung

Das Blatt „Problembehandlung“ – ein Dialogfeld im Portal – wird derzeit für Netzwerkressourcen bereitgestellt, um häufig auftretende Probleme im Zusammenhang mit einer einzelnen Ressource zu diagnostizieren. Dieses Benutzeroberflächenelement ist für die folgenden Netzwerkressourcen verfügbar: ExpressRoute, VPN Gateway, Application Gateway, Netzwerksicherheitsprotokolle, Routen, DNS, Load Balancer und Traffic Manager. Weitere Informationen zur Problembehandlung auf Ressourcenebene finden Sie unter [Diagnose and resolve issues with Azure Troubleshooting](https://azure.microsoft.com/blog/azure-troubleshoot-diagonse-resolve-issues/) (Diagnostizieren und Beheben von Problemen mit der Azure-Problembehandlung).

![Informationen zur Problembehandlung][TS]

### <a name="resource-health"></a>Ressourcenintegrität

Die Integrität von Netzwerkressourcen wird in regelmäßigen Abständen bereitgestellt. Zu diesen Ressourcen zählen VPN Gateway und VPN-Tunnel. Sie können über das Azure-Portal auf Resource Health zugreifen. Weitere Informationen zu Resource Health finden Sie unter [Übersicht über Resource Health](../resource-health/resource-health-overview.md)

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich nach Network Watcher über Folgendes:

Durchführen einer Paketerfassung auf einem virtuellen Computer: [Variable Paketerfassung im Azure-Portal](network-watcher-packet-capture-manage-portal.md)

Durchführen einer proaktiven Überwachung und Diagnose: [Erstellen einer durch Warnungen ausgelösten Paketerfassung](network-watcher-alert-triggered-packet-capture.md)

Erkennen von Sicherheitsrisiken mithilfe von Open-Source-Tools: [Analysieren der Paketerfassung mit Wireshark](network-watcher-deep-packet-inspection.md)

<!--Image references-->
[TS]: ./media/network-watcher-monitoring-overview/troubleshooting.png
[logs]: ./media/network-watcher-monitoring-overview/logs.png
[metrics]: ./media/network-watcher-monitoring-overview/metrics.png
[nsl]: ./media/network-watcher-monitoring-overview/nsl.png












