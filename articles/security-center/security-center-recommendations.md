---
title: Verwalten von Sicherheitsempfehlungen in Azure Security Center | Microsoft Docs
description: "In diesem Dokument erfahren Sie, wie Sicherheitsempfehlungen in Azure Security Center Ihnen helfen, Ihre Azure-Ressourcen zu schützen und Ihre Sicherheitsrichtlinien einzuhalten."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 86c50c9f-eb6b-4d97-acb3-6d599c06133e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/03/2017
ms.author: terrylan
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: b16d8607ca0dbb08421b332df11e462267eb80f5
ms.lasthandoff: 04/03/2017


---
# <a name="managing-security-recommendations-in-azure-security-center"></a>Verwalten von Sicherheitsempfehlungen in Azure Security Center
In diesem Dokument erfahren Sie, wie Sie Sicherheitsempfehlungen in Azure Security Center verwenden, um Ihre Azure-Ressourcen zu schützen.

> [!NOTE]
> Der Dienst wird anhand einer Beispielbereitstellung vorgestellt.  Dieses Dokument ist keine Schritt-für-Schritt-Anleitung.
>
>

## <a name="what-are-security-recommendations"></a>Was sind Sicherheitsempfehlungen?
Security Center analysiert in regelmäßigen Abständen den Sicherheitsstatus der Azure-Ressourcen. Wenn Security Center potenzielle Sicherheitsrisiken identifiziert, werden Empfehlungen erstellt. Entsprechend den Empfehlungen werden Sie durch den Prozess der Konfiguration des erforderlichen Sicherheitsmechanismus geführt.

## <a name="implementing-security-recommendations"></a>Implementieren von Sicherheitsempfehlungen
### <a name="set-recommendations"></a>Festlegen von Empfehlungen
Unter [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md)wird Folgendes beschrieben:

* Konfigurieren von Sicherheitsrichtlinien
* Aktivieren der Datensammlung
* Auswählen der Empfehlungen, die Sie als Teil Ihrer Sicherheitsrichtlinie verwenden möchten

Aktuelle Richtlinienempfehlungen beziehen sich auf Systemupdates, Grundregeln, Antischadsoftware, [Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-nsg.md) für Subnetze und Netzwerkschnittstellen, Überwachung der SQL-Datenbank, Transparent Data Encryption für die SQL-Datenbank und Web Application Firewalls.  [Einrichten von Sicherheitsrichtlinien](security-center-policies.md) enthält eine Beschreibung der einzelnen Empfehlungsoptionen.

### <a name="monitor-recommendations"></a>Überwachen von Empfehlungen
Nach Einstellung einer Sicherheitsrichtlinie analysiert Security Center den Sicherheitsstatus Ihrer Ressourcen, um potenzielle Sicherheitsrisiken zu erkennen. Auf der Kachel **Empfehlungen** des Blatts **Security Center** können Sie die Gesamtzahl der von Security Center identifizierten Empfehlungen sehen.

![Kachel „Empfehlungen“][1]

So zeigen Sie die Details jeder Empfehlung an:

1. Klicken Sie auf dem Blatt **Security Center** auf die Kachel **Empfehlungen**. Das Blatt **Empfehlungen** wird geöffnet.

Die Empfehlungen werden in einem Tabellenformat angezeigt, wobei jede Zeile eine bestimmte Empfehlung darstellt. Die Spalten dieser Tabelle sind:

* **BESCHREIBUNG**: Eine Erläuterung der Empfehlung und der erforderlichen Schritte.
* **RESSOURCE**: Eine Liste mit den Ressourcen, für die diese Empfehlung gilt.
* **STATUS**: Beschreibt den aktuellen Status der Empfehlung:
  * **Offen**: Die Empfehlung wurde noch nicht bearbeitet.
  * **In Bearbeitung**: Die Empfehlung wird derzeit auf die Ressourcen angewendet, und es ist keine Aktion Ihrerseits erforderlich.
  * **Gelöst**: Die Empfehlung wurde bereits abgeschlossen (in diesem Fall ist die Zeile ausgegraut).
* **SCHWEREGRAD**: Beschreibt den Schweregrad der jeweiligen Empfehlung:
  * **Hoch**: Ein Sicherheitsrisiko betrifft eine wichtige Ressource (etwa eine Anwendung, einen virtuellen Computer oder eine Netzwerksicherheitsgruppe) und erfordert eine Aktion.
  * **Mittel**: Es besteht ein Sicherheitsrisiko, und es sind nicht kritische bzw. weitere Schritte erforderlich, um es zu beseitigen oder einen Prozess abzuschließen.
  * **Niedrig**: Es besteht ein Sicherheitsrisiko, das behandelt werden sollte, aber es ist keine unmittelbare Aktion erforderlich. (Standardmäßig werden Empfehlungen mit dem Status „Niedrig“ nicht angezeigt, aber Sie können bei Bedarf nach diesen Empfehlungen filtern.)

Der folgenden Tabelle können Sie entnehmen, welche Empfehlungen verfügbar sind und welche Aktionen bei ihrer Anwendung jeweils ausgeführt werden.

> [!NOTE]
> Es empfiehlt sich, sich mit dem [klassischen Bereitstellungsmodell und dem Resource Manager-Bereitstellungsmodell](../azure-classic-rm.md) für Azure-Ressourcen vertraut zu machen.
>
>

| Empfehlung | Beschreibung |
| --- | --- |
| [Sammlung von Daten für Abonnements aktivieren](security-center-enable-data-collection.md) |Empfiehlt Ihnen, in der Sicherheitsrichtlinie die Datensammlung für alle Abonnements und alle virtuellen Computer in Ihren Abonnements zu aktivieren. |
| [Betriebssystem-Sicherheitsrisiken beheben](security-center-remediate-os-vulnerabilities.md) |Empfiehlt Ihnen, Ihre Betriebssystemkonfigurationen mit den empfohlenen Konfigurationsregeln in Einklang zu bringen und etwa nicht zuzulassen, dass Kennwörter gespeichert werden. |
| [Systemupdates anwenden](security-center-apply-system-updates.md) |Empfiehlt Ihnen, fehlende Systemupdates und kritische Updates für virtuelle Computer bereitzustellen. |
| [Neustart nach Systemupdates](security-center-apply-system-updates.md#reboot-after-system-updates) |Empfiehlt Ihnen, einen virtuellen Computer neu zu starten, um die Anwendung von Systemupdates abzuschließen. |
| [Web Application Firewall hinzufügen](security-center-add-web-application-firewall.md) |Empfiehlt Ihnen, eine Web Application Firewall (WAF) für Webendpunkte bereitzustellen. Eine WAF-Empfehlung wird für jede öffentlich zugängliche IP-Adresse (sowohl auf Instanzebene als auch mit Lastenausgleich) angezeigt, die über eine zugeordnete Netzwerksicherheitsgruppe mit offenen eingehenden Webports (80, 443) verfügt. </br>Security Center empfiehlt die Bereitstellung einer WAF zum Schutz vor Angriffen auf Ihre Webanwendungen auf virtuellen Computern und in der App Service-Umgebung. Eine App Service-Umgebung ist eine Option des [Premium](https://azure.microsoft.com/pricing/details/app-service/)-Tarifs von Azure App Service, die eine vollständig isolierte und dedizierte Umgebung zur sicheren Ausführung von Azure App Service-Apps bereitstellt. Weitere Informationen zu ASE finden Sie unter [Dokumentation zur App Service-Umgebung](../app-service/app-service-app-service-environments-readme.md).</br>Sie können mehrere Webanwendungen in Security Center schützen, indem Sie diese Anwendungen Ihren vorhandenen WAF-Bereitstellungen hinzufügen. |
| [Finalize application protection (Anwendungsschutz abschließen)](security-center-add-web-application-firewall.md#finalize-application-protection) |Um die Konfiguration einer WAF abzuschließen, muss Datenverkehr an das WAF-Gerät umgeleitet werden. Nach dieser Empfehlung werden die erforderlichen Setupänderungen vorgenommen. |
| [Firewall der nächsten Generation hinzufügen](security-center-add-next-generation-firewall.md) |Empfiehlt, dass Sie zum Verbessern Ihrer Sicherheitsmaßnahmen eine Firewall der nächsten Generation (NGFW) eines Microsoft-Partners hinzufügen |
| [Datenverkehr nur durch Firewall der nächsten Generation leiten](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only) |Empfiehlt, dass Sie die Regeln der Netzwerksicherheitsgruppe so konfigurieren, dass zu Ihrer VM eingehender Datenverkehr durch Ihre Firewall der nächsten Generation geleitet werden muss. |
| [Endpoint Protection installieren](security-center-install-endpoint-protection.md) |Empfiehlt Ihnen, Antischadsoftware für virtuelle Maschinen bereitzustellen (nur Windows-VMs). |
| [Endpoint Protection-Integritätswarnungen auflösen](security-center-resolve-endpoint-protection-health-alerts.md) |Empfiehlt, dass Sie Endpoint Protection-Fehler beheben. |
| [Netzwerksicherheitsgruppen in Subnetzen oder auf virtuellen Computern aktivieren](security-center-enable-network-security-groups.md) |Empfiehlt, dass Sie NSGs in Subnetzen oder auf virtuellen Computern aktivieren. |
| [Zugriff über Endpunkt mit Internetzugriff einschränken](security-center-restrict-access-through-internet-facing-endpoints.md) |Empfiehlt Ihnen, Regeln für eingehenden Datenverkehr für NSGs zu konfigurieren. |
| [Aktivieren der Überwachung und Bedrohungserkennung auf SQL-Servern](security-center-enable-auditing-on-sql-servers.md) |Empfiehlt, die Überwachung und Bedrohungserkennung für Azure SQL-Server zu aktivieren. (Nur Azure SQL-Dienst. Schließt nicht die Ausführung von SQL auf Ihren virtuellen Computern mit ein.) |
| [Aktivieren der Überwachung und Bedrohungserkennung in SQL-Datenbanken](security-center-enable-auditing-on-sql-databases.md) |Empfiehlt, die Überwachung und Bedrohungserkennung für Azure SQL-Datenbanken zu aktivieren. (Nur Azure SQL-Dienst. Schließt nicht die Ausführung von SQL auf Ihren virtuellen Computern mit ein.) |
| [Transparent Data Encryption für SQL-Datenbanken aktivieren](security-center-enable-transparent-data-encryption.md) |Empfiehlt die Verschlüsselung für SQL-Datenbanken zu aktivieren. (Nur Azure SQL-Dienst.) |
| [VM-Agent aktivieren](security-center-enable-vm-agent.md) |Ermöglicht Ihnen, anzuzeigen, welche VMs den VM-Agent benötigen. Der VM-Agent muss auf virtuellen Computern installiert werden, um die Überprüfung von Patches, die Überprüfung von Grundregeln und Antischadsoftware bereitzustellen. Der VM-Agent wird standardmäßig für virtuelle Maschinen installiert, die vom Azure Marketplace bereitgestellt werden. Der Artikel [VM-Agent und Erweiterungen – Teil 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) enthält Informationen zum Installieren des VM-Agents. |
| [Datenträgerverschlüsselung anwenden](security-center-apply-disk-encryption.md) |Empfiehlt Ihnen, dass Sie Ihre VM-Datenträger per Azure Disk Encryption (Windows- und Linux-VMs) verschlüsseln. Die Verschlüsselung wird sowohl für die Betriebssystem- als auch für die Datenvolumes auf Ihrer VM empfohlen. |
| [Sicherheitskontaktinformationen bereitstellen](security-center-provide-security-contact-details.md) |Empfiehlt, dass Sie Sicherheitskontaktinformationen für all Ihre Abonnements bereitstellen. Die Kontaktinformationen bestehen aus einer E-Mail-Adresse und einer Telefonnummer. Die Informationen werden verwendet, um mit Ihnen Kontakt aufzunehmen, sobald unser Sicherheitsteam feststellt, dass Ihre Ressourcen kompromittiert wurden. |
| [Betriebssystemversion aktualisieren](security-center-update-os-version.md) |Die Empfehlung lautet, dass Sie die Betriebssystemversion für den Clouddienst auf die aktuelle Version aktualisieren sollten, die für Ihre Betriebssystemfamilie verfügbar ist.  Weitere Informationen zu Cloud Services finden Sie unter [Übersicht über Cloud Services](../cloud-services/cloud-services-choose-me.md). |
| [Sicherheitsrisikobewertung nicht installiert](security-center-vulnerability-assessment-recommendations.md) |Empfiehlt die Installation einer Lösung zur Sicherheitsrisikobewertung auf dem virtuellen Computer. |
| [Sicherheitsrisiken beheben](security-center-vulnerability-assessment-recommendations.md#review-recommendation) |Ermöglicht das Anzeigen von System- und Anwendungssicherheitslücken, die von der auf dem virtuellen Computer installierten Lösung zur Sicherheitsrisikobewertung erkannt werden. |
| [Aktivieren der Verschlüsselung für das Azure Storage-Konten](security-center-enable-encryption-for-storage-account.md) | Empfiehlt die Aktivierung von Azure Storage Service Encryption für ruhende Daten. Storage Service Encryption (SSE) verschlüsselt die Daten, wenn diese in Azure Storage geschrieben werden, und entschlüsselt sie vor dem Abrufen. SSE steht zurzeit nur für den Azure Blob-Dienst zur Verfügung und kann für Blockblobs, Seitenblobs und Anfügeblobs verwendet werden. Weitere Informationen finden Sie unter [Storage Service Encryption für ruhende Daten](../storage/storage-service-encryption.md).</br>SSE wird nur für Resource Manager-Speicherkonten unterstützt. |

Sie können Empfehlungen filtern und verwerfen.

1. Klicken Sie auf dem Blatt **Empfehlungen** auf **Filter**. Das Blatt **Filter** wird geöffnet. Sie können Werte für Schweregrad und Status auswählen, die Sie anzeigen möchten.

    ![Empfehlungen filtern][2]
2. Wenn Sie feststellen, dass eine Empfehlung nicht anwendbar ist, können Sie die Empfehlung verwerfen und dann aus der Ansicht filtern. Es gibt zwei Möglichkeiten, eine Empfehlung zu verwerfen. Klicken Sie mit der rechten Maustaste auf ein Element, und wählen Sie die **Verwerfen**. Oder bewegen Sie den Mauszeiger auf ein Element, klicken Sie auf die drei rechts angezeigten Punkte, und wählen Sie **Verwerfen**. Sie können verworfene Empfehlungen anzeigen, indem Sie auf **Filter** klicken und dann **Verworfen** auswählen.

    ![Empfehlung verwerfen][3]

### <a name="apply-recommendations"></a>Anwenden von Empfehlungen
Nach Auswertung aller Empfehlungen entscheiden Sie, welche zuerst angewendet werden soll. Es ist ratsam, den Schweregrad als wichtigsten Parameter bei der Entscheidung heranzuziehen, welche Empfehlungen zuerst angewendet werden sollen.

Wählen Sie in der obigen Tabelle der Empfehlungen eine Empfehlung aus, und durchlaufen Sie das entsprechende Beispiel, um sie umzusetzen.

## <a name="see-also"></a>Siehe auch
In diesem Dokument wurden Ihnen die Sicherheitsempfehlungen in Security Center vorgestellt. Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Überwachen der Sicherheitsintegrität in Azure Security Center:](security-center-monitoring.md) Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Erfahren Sie, wie der Integritätsstatus Ihrer Partnerlösungen überwacht wird.
* [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Azure-Sicherheit und -Compliance.

<!--Image references-->
[1]: ./media/security-center-recommendations/recommendations-tile.png
[2]: ./media/security-center-recommendations/filter-recommendations.png
[3]: ./media/security-center-recommendations/dismiss-recommendations.png

