---
title: "Einführung in die Konnektivitätsprüfung in Azure Network Watcher | Microsoft-Dokumentation"
description: "Diese Seite enthält eine Übersicht über die Konnektivitätsfunktion von Network Watcher."
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/11/2017
ms.author: jdial
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: c29f5afe59f57112fe1f115df6bc53645f3c0d34
ms.contentlocale: de-de
ms.lasthandoff: 08/23/2017

---

# <a name="introduction-to-connectivity-check-in-azure-network-watcher"></a>Einführung in die Konnektivitätsprüfung in Azure Network Watcher

Die Konnektivitätsfunktion von Network Watcher bietet die Möglichkeit, eine direkte TCP-Verbindung von einem virtuellen Computer zu einem anderen virtuellen Computer (VM), einem vollqualifizierten Domänennamen (FQDN), einem URI oder einer IPv4-Adresse zu überprüfen. Netzwerkszenarios sind komplex: Sie werden mithilfe von Netzwerksicherheitsgruppen, Firewalls, benutzerdefinierten Routen und Ressourcen, die von Azure bereitgestellt werden, implementiert. Komplexe Konfigurationen stellen eine große Herausforderung bei der Behandlung von Konnektivitätsproblemen dar. Network Watcher trägt dazu bei, die zum Suchen und Erkennen von Konnektivitätsproblemen benötigte Zeit zu verkürzen. Die zurückgegebenen Ergebnisse können Einblicke in die Frage geben, ob ein Konnektivitätsproblem auf ein Problem mit einer Plattform oder einer Benutzerkonfiguration zurückzuführen ist. Die Konnektivität kann mit [PowerShell](network-watcher-connectivity-powershell.md), [Azure CLI](network-watcher-connectivity-cli.md) und [REST-API](network-watcher-connectivity-rest.md) überprüft werden.

> [!IMPORTANT]
> Für die Konnektivitätsprüfung ist die VM-Erweiterung `AzureNetworkWatcherExtension` erforderlich. Informationen zur Installation der Erweiterung finden Sie für einen virtuellen Windows-Computer unter [VM-Erweiterung für den Network Watcher-Agent für Windows](../virtual-machines/windows/extensions-nwa.md) und für einen virtuellen Linux-Computer unter [VM-Erweiterung für den Network Watcher-Agent für Linux](../virtual-machines/linux/extensions-nwa.md).

## <a name="response"></a>Antwort

Die folgende Tabelle zeigt die Eigenschaften, die nach Abschluss einer Konnektivitätsprüfung zurückgegeben werden:

|Eigenschaft  |Beschreibung  |
|---------|---------|
|ConnectionStatus     | Status der Konnektivitätsprüfung. Mögliche Ergebnisse sind **Erreichbar** und **Nicht erreichbar**.        |
|AvgLatencyInMs     | Durchschnittliche Wartezeit während der Konnektivitätsprüfung in Millisekunden. (Wird nur angezeigt, wenn der Status „Erreichbar“ ist)        |
|MinLatencyInMs     | Minimale Wartezeit während der Konnektivitätsprüfung in Millisekunden. (Wird nur angezeigt, wenn der Status „Erreichbar“ ist)        |
|MaxLatencyInMs     | Maximale Wartezeit während der Konnektivitätsprüfung in Millisekunden. (Wird nur angezeigt, wenn der Status „Erreichbar“ ist)        |
|ProbesSent     | Anzahl der Tests, die während der Überprüfung gesendet werden. Höchstwert ist 100.        |
|ProbesFailed     | Anzahl der fehlerhaften Tests während der Überprüfung. Höchstwert ist 100.        |
|Hops     | Hop-by-Hop-Pfad von der Quelle zum Ziel.        |
|Hops[].Type     | Ressourcentyp. Mögliche Werte sind **Source**, **VirtualAppliance**, **VnetLocal** und **Internet**.        |
|Hops[].Id | Eindeutiger Bezeichner des Hops.|
|Hops[].Address | IP-Adresse des Hops.|
|Hops[].ResourceId | Ressourcen-ID des Hops, wenn der Hop eine Azure-Ressource ist. Wenn es sich um eine Internetressource handelt, lautet die Ressourcen-ID **Internet**. |
|Hops[].NextHopIds | Eindeutiger Bezeichner des nächsten Hops.|
|Hops[].Issues | Auflistung von Problemen, die während der Überprüfung auf diesem Hop aufgetreten sind. Wert ist leer, wenn keine Probleme aufgetreten sind.|
|Hops[].Issues[].Origin | Auf dem aktuellen Hop, wo das Problem aufgetreten ist. Mögliche Werte:<br/> **Eingehend**: Problem liegt auf der Verbindung vom vorherigen Hop zum aktuellen Hop.<br/>**Ausgehend**: Problem liegt auf der Verbindung vom aktuellen Hop zum nächsten Hop.<br/>**Lokal**: Problem befindet sich auf dem aktuellen Hop.|
|Hops[].Issues[].Severity | Schweregrad des gefundenen Problems. Mögliche Werte sind **Error** und **Warning**. |
|Hops[].Issues[].Type |Typ des gefundenen Problems. Mögliche Werte: <br/>**CPU**<br/>**Arbeitsspeicher**<br/>**GuestFirewall**<br/>**DnsResolution**<br/>**NetworkSecurityRule**<br/>**UserDefinedRoute** |
|Hops[].Issues[].Context |Details zu dem gefundenen Problem.|
|Hops[].Issues[].Context[].key |Schlüssel des zurückgegebenen Schlüssel-Wert-Paars.|
|Hops[].Issues[].Context[].value |Wert des zurückgegebenen Schlüssel-Wert-Paars.|

Im Folgenden finden Sie ein Beispiel für ein Problem, das auf einem Hop gefunden wurde:

```json
"Issues": [
    {
        "Origin": "Outbound",
        "Severity": "Error",
        "Type": "NetworkSecurityRule",
        "Context": [
            {
                "key": "RuleName",
                "value": "UserRule_Port80"
            }
        ]
    }
]
```
## <a name="fault-types"></a>Fehlertypen

Die Konnektivitätsprüfung gibt Typen der Fehler bei der Verbindung zurück. Die folgende Tabelle listet die aktuellen zurückgegebenen Fehlertypen auf:

|Typ  |Beschreibung  |
|---------|---------|
|CPU     | Hohe CPU-Auslastung.       |
|Arbeitsspeicher     | Hohe Speicherauslastung.       |
|GuestFirewall     | Datenverkehr wird aufgrund der Firewall-Konfiguration eines virtuellen Computers blockiert.        |
|DNSResolution     | Fehler bei DNS-Auflösung für Zieladresse.        |
|NetworkSecurityRule    | Datenverkehr wird durch NSG-Regel blockiert (Regel zurückgegeben).        |
|UserDefinedRoute|Datenverkehr wird aufgrund einer benutzerdefinierten oder Systemroute verworfen. |

### <a name="next-steps"></a>Nächste Schritte

Eine Anleitung zum Überprüfen der Konnektivität zu einer Ressource finden Sie unter: [Check connectivity with Azure Network Watcher (Überprüfen der Konnektivität mit Azure Network Watcher)](network-watcher-connectivity-powershell.md).

<!--Image references-->
[1]: ./media/network-watcher-next-hop-overview/figure1.png


