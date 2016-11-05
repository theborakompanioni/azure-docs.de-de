---
title: Schützen Ihrer Anwendungen in Azure Security Center | Microsoft Docs
description: Dieses Dokument behandelt Empfehlungen in Azure Security Center, die zum Schutz Ihrer Azure-Anwendungen sowie zur Einhaltung von Sicherheitsrichtlinien beitragen.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''

ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2016
ms.author: terrylan

---
# Schützen Ihrer Anwendungen in Azure Security Center
Azure Security Center analysiert den Sicherheitsstatus Ihrer Azure-Ressourcen. Werden potenzielle Sicherheitslücken erkannt, erstellt Security Center Empfehlungen, die Sie beim Konfigurieren der erforderlichen Steuerelemente unterstützen. Die Empfehlungen gelten für folgende Azure-Ressourcentypen: virtuelle Computer (Virtual Machines, VMs), Netzwerk, SQL und Anwendungen.

In diesem Artikel werden Empfehlungen für Anwendungen behandelt. Bei den Anwendungsempfehlungen steht die Bereitstellung einer Web Application Firewall im Mittelpunkt. Der folgenden Tabelle können Sie entnehmen, welche Anwendungsempfehlungen verfügbar sind und welche Aktionen jeweils ausgeführt werden, wenn Sie sie anwenden:

## Verfügbare Anwendungsempfehlungen
| Empfehlung | Beschreibung |
| --- | --- |
| [Web Application Firewall hinzufügen](security-center-add-web-application-firewall.md) |Empfiehlt Ihnen, eine Web Application Firewall (WAF) für Webendpunkte bereitzustellen. Sie können mehrere Webanwendungen in Security Center schützen, indem Sie diese Anwendungen Ihren vorhandenen WAF-Bereitstellungen hinzufügen. WAF-Geräte (erstellt mit dem Ressourcen-Manager-Bereitstellungsmodell) müssen in einem separaten virtuellen Netzwerk bereitgestellt werden. WAF-Geräte (erstellt mit dem klassischen Bereitstellungsmodell) sind auf die Verwendung einer Netzwerksicherheitsgruppe beschränkt. Diese Unterstützung wird in Zukunft auf eine vollständig angepasste Bereitstellung eines WAF-Geräts (klassisch) erweitert. |
| [Finalize application protection (Anwendungsschutz abschließen)](security-center-add-web-application-firewall.md#finalize-application-protection) |Um die Konfiguration einer WAF abzuschließen, muss Datenverkehr an das WAF-Gerät umgeleitet werden. Nach dieser Empfehlung werden die erforderlichen Setupänderungen vorgenommen. |

## Siehe auch
Weitere Informationen zu Empfehlungen für andere Arten von Azure-Ressourcen finden Sie in den folgenden Themen:

* [Schützen Ihrer virtuellen Computer in Azure Security Center](security-center-virtual-machine-recommendations.md)
* [Schützen Ihres Netzwerks in Azure Security Center](security-center-network-recommendations.md)
* [Schützen Ihres Azure SQL-Diensts in Azure Security Center](security-center-sql-service-recommendations.md)

Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Hier erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.

<!---HONumber=AcomDC_0810_2016-->