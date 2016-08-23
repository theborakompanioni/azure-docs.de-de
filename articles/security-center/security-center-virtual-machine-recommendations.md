<properties
   pageTitle="Schützen Ihrer virtuellen Computer in Azure Security Center | Microsoft Azure"
   description="Dieses Dokument behandelt Empfehlungen in Azure Security Center, die zum Schutz Ihrer virtuellen Computer sowie zur Einhaltung von Sicherheitsrichtlinien beitragen."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/04/2016"
   ms.author="terrylan"/>

# Schützen Ihrer virtuellen Computer in Azure Security Center

Azure Security Center analysiert den Sicherheitsstatus Ihrer Azure-Ressourcen. Werden potenzielle Sicherheitslücken erkannt, erstellt Security Center Empfehlungen, die Sie beim Konfigurieren der erforderlichen Steuerelemente unterstützen. Die Empfehlungen gelten für folgende Azure-Ressourcentypen: virtuelle Computer (Virtual Machines, VMs), Netzwerk, SQL und Anwendungen.

In diesem Artikel werden Empfehlungen für virtuelle Computer behandelt. Bei Empfehlungen für virtuelle Computer stehen das Sammeln von Daten, das Anwenden von Systemupdates, das Bereitstellen von Antischadsoftware, das Verschlüsseln von VM-Datenträgern und Ähnliches im Mittelpunkt. Der folgenden Tabelle können Sie entnehmen, welche Empfehlungen für virtuelle Computer verfügbar sind und welche Aktionen jeweils ausgeführt werden, wenn Sie sie anwenden:

## Verfügbare Empfehlungen für virtuelle Computer

|Empfehlung|Beschreibung|
|-----|-----|
|[Sammlung von Daten für Abonnements aktivieren](security-center-enable-data-collection.md)|Empfiehlt Ihnen, in der Sicherheitsrichtlinie die Datensammlung für alle Abonnements und alle virtuellen Computer in Ihren Abonnements zu aktivieren.|
|[Betriebssystem-Sicherheitsrisiken beheben](security-center-remediate-os-vulnerabilities.md)|Empfiehlt Ihnen, Ihre Betriebssystemkonfigurationen an den empfohlenen Konfigurationsregeln zu orientieren und z.B. nicht zuzulassen, dass Kennwörter gespeichert werden.|
|[Systemupdates anwenden](security-center-apply-system-updates.md)|Empfiehlt Ihnen, fehlende Systemupdates und kritische Updates für virtuelle Computer bereitzustellen.|
|[Neustart nach Systemupdates](security-center-apply-system-updates.md#reboot-after-system-updates)|Empfiehlt Ihnen, einen virtuellen Computer neu zu starten, um die Anwendung von Systemupdates abzuschließen.|
|[Endpoint Protection installieren](security-center-install-endpoint-protection.md)|Empfiehlt Ihnen, Antischadsoftware für virtuelle Maschinen bereitzustellen (nur Windows-VMs).|
|[Endpoint Protection-Integritätswarnungen auflösen](security-center-resolve-endpoint-protection-health-alerts.md)|Empfiehlt, dass Sie Endpoint Protection-Fehler beheben.|
|[VM-Agent aktivieren](security-center-enable-vm-agent.md)|Ermöglicht Ihnen, anzuzeigen, welche VMs den VM-Agent benötigen. Der VM-Agent muss auf virtuellen Maschinen installiert werden, um das Überprüfen von Patches, Überprüfen von Grundregeln und Antischadsoftware bereitzustellen. Der VM-Agent wird standardmäßig für virtuelle Maschinen installiert, die vom Azure Marketplace bereitgestellt werden. Der Artikel [VM-Agent und Erweiterungen – Teil 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) enthält Informationen zum Installieren des VM-Agents.|
| [Datenträgerverschlüsselung anwenden](security-center-apply-disk-encryption.md) |Empfiehlt Ihnen, dass Sie Ihre VM-Datenträger per Azure Disk Encryption (Windows- und Linux-VMs) verschlüsseln. Die Verschlüsselung wird sowohl für die Betriebssystem- als auch für die Datenvolumes auf Ihrer VM empfohlen.|
| [Betriebssystemversion aktualisieren](security-center-update-os-version.md) | Die Empfehlung lautet, dass Sie die Betriebssystemversion für den Clouddienst auf die aktuelle Version aktualisieren sollten, die für Ihre Betriebssystemfamilie verfügbar ist. Weitere Informationen zu Cloud Services finden Sie in der [Übersicht über Cloud Services](../cloud-services/cloud-services-choose-me.md). |

## Siehe auch

Weitere Informationen zu Empfehlungen für andere Arten von Azure-Ressourcen finden Sie in den folgenden Themen:

- [Schützen Ihrer Anwendungen in Azure Security Center](security-center-application-recommendations.md)
- [Schützen Ihres Netzwerks in Azure Security Center](security-center-network-recommendations.md)
- [Schützen Ihres Azure SQL-Diensts in Azure Security Center](security-center-sql-service-recommendations.md)

Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md): Hier erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
- [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md): Hier erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
- [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md): Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.

<!---HONumber=AcomDC_0810_2016-->