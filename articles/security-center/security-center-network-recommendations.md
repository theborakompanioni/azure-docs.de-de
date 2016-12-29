---
title: "Schützen Ihres Netzwerks in Azure Security Center | Microsoft Docs"
description: Dieses Dokument behandelt Empfehlungen in Azure Security Center, die zum Schutz Ihres Azure-Netzwerks sowie zur Einhaltung von Sicherheitsrichtlinien beitragen.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 96c55a02-afd6-478b-9c1f-039528f3dea0
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: terrylan
translationtype: Human Translation
ms.sourcegitcommit: 174546d59124296711731de6c8d8929bada56baf
ms.openlocfilehash: 00b715507a7c3a4d784b800e7bf0c700f6ea6ff1


---
# <a name="protecting-your-network-in-azure-security-center"></a>Schützen Ihres Netzwerks in Azure Security Center
Azure Security Center analysiert den Sicherheitsstatus Ihrer Azure-Ressourcen. Werden potenzielle Sicherheitslücken erkannt, erstellt Security Center Empfehlungen, die Sie beim Konfigurieren der erforderlichen Steuerelemente unterstützen.  Die Empfehlungen gelten für folgende Azure-Ressourcentypen: virtuelle Computer (Virtual Machines, VMs), Netzwerk, SQL und Anwendungen.

In diesem Artikel werden Empfehlungen für virtuelle Computer behandelt.  Bei Netzwerkempfehlungen stehen Firewalls der nächsten Generation, Netzwerksicherheitsgruppen, das Konfigurieren von Regeln für den eingehenden Datenverkehr und vieles mehr im Mittelpunkt.  Entnehmen Sie der folgenden Tabelle, welche Netzwerkempfehlungen verfügbar sind und welche Aktionen jeweils ausgeführt werden, wenn Sie sie anwenden.

## <a name="available-network-recommendations"></a>Verfügbare Netzwerkempfehlungen
| Empfehlung | Beschreibung |
| --- | --- |
| [Firewall der nächsten Generation hinzufügen](security-center-add-next-generation-firewall.md) |Empfiehlt, dass Sie zum Verbessern Ihrer Sicherheitsmaßnahmen eine Firewall der nächsten Generation (NGFW) eines Microsoft-Partners hinzufügen |
| [Datenverkehr nur durch Firewall der nächsten Generation leiten](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only) |Empfiehlt, dass Sie die Regeln der Netzwerksicherheitsgruppe so konfigurieren, dass zu Ihrer VM eingehender Datenverkehr durch Ihre Firewall der nächsten Generation geleitet werden muss. |
| [Netzwerksicherheitsgruppen in Subnetzen oder auf virtuellen Computern aktivieren](security-center-enable-network-security-groups.md) |Empfiehlt, dass Sie NSGs in Subnetzen oder auf virtuellen Computern aktivieren. |
| [Zugriff über Endpunkt mit Internetzugriff einschränken](security-center-restrict-access-through-internet-facing-endpoints.md) |Empfiehlt Ihnen, Regeln für eingehenden Datenverkehr für NSGs zu konfigurieren. |

## <a name="see-also"></a>Siehe auch
Weitere Informationen zu Empfehlungen für andere Arten von Azure-Ressourcen finden Sie in den folgenden Themen:

* [Schützen Ihrer virtuellen Computer in Azure Security Center](security-center-virtual-machine-recommendations.md)
* [Schützen Ihrer Anwendungen in Azure Security Center](security-center-application-recommendations.md)
* [Schützen Ihres Azure SQL-Diensts in Azure Security Center](security-center-sql-service-recommendations.md)

Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md) : Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.



<!--HONumber=Nov16_HO3-->


