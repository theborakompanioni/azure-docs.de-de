---
title: "Schützen Ihrer virtuellen Computer in Azure Security Center | Microsoft Docs"
description: Dieses Dokument behandelt Empfehlungen in Azure Security Center, die zum Schutz Ihrer virtuellen Computer sowie zur Einhaltung von Sicherheitsrichtlinien beitragen.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 47fa1f76-683d-4230-b4ed-d123fef9a3e8
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: terrylan
ms.translationtype: HT
ms.sourcegitcommit: c3ea7cfba9fbf1064e2bd58344a7a00dc81eb148
ms.openlocfilehash: 9f08f73eb4ce8fc46b4f4ba5c2d1fd22319537d2
ms.contentlocale: de-de
ms.lasthandoff: 07/19/2017

---
# <a name="protecting-your-virtual-machines-in-azure-security-center"></a>Schützen Ihrer virtuellen Computer in Azure Security Center
Azure Security Center analysiert den Sicherheitsstatus Ihrer Azure-Ressourcen. Werden potenzielle Sicherheitslücken erkannt, erstellt Security Center Empfehlungen, die Sie beim Konfigurieren der erforderlichen Steuerelemente unterstützen.  Die Empfehlungen gelten für folgende Azure-Ressourcentypen: virtuelle Computer (Virtual Machines, VMs), Netzwerk, SQL und Anwendungen.

In diesem Artikel werden Empfehlungen für virtuelle Computer behandelt.  Bei Empfehlungen für virtuelle Computer stehen das Sammeln von Daten, das Anwenden von Systemupdates, das Bereitstellen von Antischadsoftware, das Verschlüsseln von VM-Datenträgern und Ähnliches im Mittelpunkt.  Der folgenden Tabelle können Sie entnehmen, welche Empfehlungen für virtuelle Computer verfügbar sind und welche Aktionen jeweils ausgeführt werden, wenn Sie sie anwenden:

## <a name="available-vm-recommendations"></a>Verfügbare Empfehlungen für virtuelle Computer
| Empfehlung | Beschreibung |
| --- | --- |
| [Sammlung von Daten für Abonnements aktivieren](security-center-enable-data-collection.md) |Empfiehlt Ihnen, in der Sicherheitsrichtlinie die Datensammlung für alle Abonnements und alle virtuellen Computer in Ihren Abonnements zu aktivieren. |
| [Aktivieren der Verschlüsselung für das Azure Storage-Konten](security-center-enable-encryption-for-storage-account.md) | Empfiehlt die Aktivierung von Azure Storage Service Encryption für ruhende Daten. Storage Service Encryption (SSE) verschlüsselt die Daten, wenn diese in Azure Storage geschrieben werden, und entschlüsselt sie vor dem Abrufen. SSE steht zurzeit nur für den Azure Blob-Dienst zur Verfügung und kann für Blockblobs, Seitenblobs und Anfügeblobs verwendet werden. Weitere Informationen finden Sie unter [Storage Service Encryption für ruhende Daten](../storage/storage-service-encryption.md).</br>SSE wird nur für Resource Manager-Speicherkonten unterstützt. Klassische Speicherkonten werden zurzeit nicht unterstützt. Weitere Informationen zum klassischen Bereitstellungsmodell und zum Resource Manager-Bereitstellungsmodell finden Sie unter [Azure-Bereitstellungsmodellen](../azure-classic-rm.md). |
| [Betriebssystem-Sicherheitsrisiken beheben](security-center-remediate-os-vulnerabilities.md) |Empfiehlt Ihnen, Ihre Betriebssystemkonfigurationen an den empfohlenen Konfigurationsregeln zu orientieren und z.B. nicht zuzulassen, dass Kennwörter gespeichert werden. |
| [Systemupdates anwenden](security-center-apply-system-updates.md) |Empfiehlt Ihnen, fehlende Systemupdates und kritische Updates für virtuelle Computer bereitzustellen. |
| [JIT-Netzwerkzugriffssteuerung anwenden](security-center-just-in-time.md) | Empfiehlt Ihnen, Just-In-Time-VM-Zugriff anzuwenden. Das Just-In-Time-Feature befindet sich in der Vorschauphase und ist im Standard-Tarif von Security Center verfügbar. Weitere Informationen zu den Tarifen von Security Center finden Sie unter [Preise](security-center-pricing.md). |
| [Neustart nach Systemupdates](security-center-apply-system-updates.md#reboot-after-system-updates) |Empfiehlt Ihnen, einen virtuellen Computer neu zu starten, um die Anwendung von Systemupdates abzuschließen. |
| [Endpoint Protection installieren](security-center-install-endpoint-protection.md) |Empfiehlt Ihnen, Antischadsoftware für virtuelle Maschinen bereitzustellen (nur Windows-VMs). |
| [Endpoint Protection-Integritätswarnungen auflösen](security-center-resolve-endpoint-protection-health-alerts.md) |Empfiehlt, dass Sie Endpoint Protection-Fehler beheben. |
| [VM-Agent aktivieren](security-center-enable-vm-agent.md) |Ermöglicht Ihnen, anzuzeigen, welche VMs den VM-Agent benötigen. Der VM-Agent muss auf virtuellen Maschinen installiert werden, um das Überprüfen von Patches, Überprüfen von Grundregeln und Antischadsoftware bereitzustellen. Der VM-Agent wird standardmäßig für virtuelle Maschinen installiert, die vom Azure Marketplace bereitgestellt werden. Der Artikel [VM-Agent und Erweiterungen – Teil 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) enthält Informationen zum Installieren des VM-Agents. |
| [Datenträgerverschlüsselung anwenden](security-center-apply-disk-encryption.md) |Empfiehlt Ihnen, dass Sie Ihre VM-Datenträger per Azure Disk Encryption (Windows- und Linux-VMs) verschlüsseln. Die Verschlüsselung wird sowohl für die Betriebssystem- als auch für die Datenvolumes auf Ihrer VM empfohlen. |
| [Betriebssystemversion aktualisieren](security-center-update-os-version.md) |Die Empfehlung lautet, dass Sie die Betriebssystemversion für den Clouddienst auf die aktuelle Version aktualisieren sollten, die für Ihre Betriebssystemfamilie verfügbar ist.  Weitere Informationen zu Cloud Services finden Sie unter [Übersicht über Cloud Services](../cloud-services/cloud-services-choose-me.md). |
| [Sicherheitsrisikobewertung nicht installiert](security-center-vulnerability-assessment-recommendations.md) |Empfiehlt die Installation einer Lösung zur Sicherheitsrisikobewertung auf dem virtuellen Computer. |
| [Sicherheitsrisiken beheben](security-center-vulnerability-assessment-recommendations.md#review-the-recommendation) |Ermöglicht das Anzeigen von System- und Anwendungssicherheitslücken, die von der auf dem virtuellen Computer installierten Lösung zur Sicherheitsrisikobewertung erkannt werden. |

## <a name="see-also"></a>Weitere Informationen
Weitere Informationen zu Empfehlungen für andere Arten von Azure-Ressourcen finden Sie in den folgenden Themen:

* [Schützen Ihrer Anwendungen in Azure Security Center](security-center-application-recommendations.md)
* [Schützen Ihres Netzwerks in Azure Security Center](security-center-network-recommendations.md)
* [Schützen Ihres Azure SQL-Diensts in Azure Security Center](security-center-sql-service-recommendations.md)

Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md) : Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.

