---
title: "Azure-Sicherheitsfeatures, die für virtuelle Azure-Computer verwendet werden | Microsoft-Dokumentation"
description: " Hier finden Sie eine Übersicht über die wichtigsten Sicherheitsfunktionen von Azure, die mit virtuellen Azure-Computern verwendet werden können. Virtuelle Azure-Computer bieten Ihnen die Flexibilität der Virtualisierung, ohne Zeit und Geld für den Kauf und die Verwaltung der Hardware aufwenden zu müssen, mit der die virtuellen Computer betrieben werden. "
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 467b2c83-0352-4e9d-9788-c77fb400fe54
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/04/2017
ms.author: terrylan
ms.translationtype: HT
ms.sourcegitcommit: 14915593f7bfce70d7bf692a15d11f02d107706b
ms.openlocfilehash: f1fb7f876c7dc010c03f01a4f6698ddc18da1100
ms.contentlocale: de-de
ms.lasthandoff: 08/10/2017

---
# <a name="azure-virtual-machines-security-overview"></a>Virtuelle Azure-Computer – Sicherheitsübersicht
Mit Azure Virtual Machines können Sie sehr flexibel eine Vielzahl unterschiedlicher Computinglösungen bereitstellen. Mit Unterstützung für Microsoft Windows, Linux, Microsoft SQL Server, Oracle, IBM, SAP und Azure BizTalk Services können Sie jede Workload und jede Sprache auf fast jedem Betriebssystem bereitstellen.

Ein virtueller Azure-Computer bietet Ihnen die Flexibilität der Virtualisierung, ohne Zeit und Geld für den Kauf und das Verwalten der Hardware aufwenden zu müssen, die den virtuellen Computer hostet.  Sie können Ihre Anwendungen in der Gewissheit bereitstellen, dass Ihre Daten in unseren hochsicheren Datencentern geschützt und abgesichert sind.

Mit Azure können Sie richtlinienkonforme Lösungen mit erweiterter Sicherheit erstellen, die:

* Ihre virtuellen Computer gegen Viren und Schadsoftware schützen
* Ihre sensiblen Daten verschlüsseln
* Netzwerkdatenverkehr absichern
* Bedrohungen erkennen
* Compliancevorgaben einhalten

Das Ziel dieses Artikels ist, eine Übersicht über die wichtigsten Azure-Sicherheitsfunktionen zu bieten, die mit virtuellen Computern verwendet werden können. Wir bieten auch Links zu Artikeln mit weiteren Informationen zu jedem Feature.  

Die wichtigsten Azure-Sicherheitsfunktionen für virtuelle Computer, die in diesem Artikel behandelt werden:

* Antimalware
* Hardwaresicherheitsmodul
* Verschlüsselung der Datenträger virtueller Computer
* Sicherung virtueller Computer
* Azure Site Recovery
* Virtuelle Netzwerke
* Verwaltung von Sicherheitsrichtlinien und Berichtserstellung
* Compliance

## <a name="antimalware"></a>Antimalware
Mit Azure können Sie zum Schützen der virtuellen Computer vor Dateien mit schädlichem Inhalt, Adware und anderen Bedrohungen Antischadsoftware von Anbietern wie Microsoft, Symantec, Trend Micro und Kaspersky verwenden. Artikel zu Partnerlösungen finden Sie unten im Abschnitt „Weitere Informationen“.

Microsoft Antimalware for Azure Cloud Services and Virtual Machines ist eine Echtzeit-Schutzfunktion zum Bestimmen und Entfernen von Viren, Spyware und anderer Schadsoftware.  Microsoft Antimalware gibt konfigurierbare Warnungen aus, wenn bekannte schädliche oder unerwünschte Software versucht, sich selbst auf Ihren Azure-Systemen zu installieren oder dort auszuführen.

Microsoft Antimalware ist eine Lösung mit einem einzelnen Agent für Anwendungen und Mandantenumgebungen, die im Hintergrund ohne Eingreifen des Benutzers ausgeführt wird. Sie können Schutz basierend auf den Anforderungen der Anwendungsworkloads bereitstellen, entweder mit der einfachen Konfiguration, die Schutz durch die Standardeinstellungen bietet, oder mit einer erweiterten benutzerdefinierten Konfiguration, einschließlich Antischadsoftwareüberwachung.

Wenn Sie Microsoft Antimalware bereitstellen und aktivieren, sind die folgenden Kernfunktionen verfügbar:

* Echtzeitschutz – Überwacht die Aktivitäten in Clouddiensten und auf virtuellen Computern, um die Ausführung von Malware zu erkennen und zu blockieren.
* Geplantes Scannen – Führt regelmäßig gezielte Scans aus, um Malware, einschließlich aktiv ausgeführter Programme, zu erkennen.
* Schadsoftwarebehandlung – Führt automatisch Aktionen für erkannte Malware aus, z.B. das Löschen schädlicher Dateien, das Unter-Quarantäne-Stellen schädlicher Dateien und das Bereinigen von schädlichen Registrierungseinträgen.
* Signaturaktualisierungen – Installiert automatisch mit einer vordefinierten Häufigkeit die neuesten Schutzsignaturen (Virendefinitionen), um sicherzustellen, dass der Schutz auf dem neuesten Stand ist.
* Aktualisierungen des Antimalware-Moduls – Aktualisiert automatisch das Microsoft Antimalware-Modul.
* Aktualisierungen der Antimalware-Plattform – Aktualisiert automatisch die Microsoft Antimalware-Plattform.
* Aktiver Schutz – Übermittelt Telemetriemetadaten zu erkannten Bedrohungen und verdächtigen Ressourcen an Azure, um eine schnelle Reaktion sicherzustellen, und ermöglicht eine synchrone Echtzeitübermittlung von Signaturen per MAPS (Microsoft Active Protection Service).
* Übermittlung von Stichproben – Übermittelt Stichproben an den Microsoft Antimalware-Dienst, um den Dienst zu optimieren und die Problembehandlung zu ermöglichen.
* Ausschlüsse – Ermöglicht Anwendungs- und Dienstadministratoren, aus Leistungs- und anderen Gründen bestimmte Dateien, Prozesse und Laufwerke vom Schutz und von Scans auszuschließen.
* Antimalware-Ereigniserfassung – Zeichnet die Integrität des Antimalware-Diensts, verdächtige Aktivitäten und durchgeführte Wiederherstellungsaktionen im Ereignisprotokoll des Betriebssystems auf und erfasst sie im Azure Storage-Konto des Kunden.

Weitere Informationen zu Antischadsoftware zum Schützen Ihrer virtuellen Computer finden Sie unter:

* [Microsoft Antimalware für Azure Cloud Services und Virtual Machines](azure-security-antimalware.md)
* [Deploying Antimalware Solutions on Azure Virtual Machines (Bereitstellen von Antischadsoftware-Lösungen auf virtuellen Azure-Computern, in englischer Sprache)](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
* [Installieren und Konfigurieren von Trend Micro Deep Security als Dienst auf einem virtuellen Windows-Computer](../virtual-machines/windows/classic/install-trend.md)
* [Installieren und Konfigurieren von Symantec Endpoint Protection auf einem virtuellen Windows-Computer](../virtual-machines/windows/classic/install-symantec.md)
* [Willkommen im Azure Marketplace](https://azure.microsoft.com/marketplace/?term=security)

## <a name="hardware-security-module"></a>Hardwaresicherheitsmodul
Verschlüsselungs- und Authentifizierungsschutzmaßnahmen können durch eine höhere Sicherheit verbessert werden. Sie können die Verwaltung und Sicherheit Ihrer geschäftskritischen geheimen Daten und Schlüssel vereinfachen, indem Sie diese in Azure Key Vault speichern. Mit Key Vault können Sie Ihre Schlüssel in Hardwaresicherheitsmodulen (Hardware Security Modules, HSMs) speichern, die gemäß FIPS 140-2 Level 2-Standards zertifiziert sind. Sie können Ihre SQL Server-Verschlüsselungsschlüssel für Sicherungen oder [transparente Datenverschlüsselung](https://msdn.microsoft.com/library/bb934049.aspx) gemeinsam mit den Schlüsseln oder geheimen Daten Ihrer Anwendungen in Key Vault speichern. Der Zugriff auf diese geschützten Elemente sowie die zugehörigen Berechtigungen werden über [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)verwaltet.

Weitere Informationen:

* [Was ist der Azure-Schlüsseltresor?](../key-vault/key-vault-whatis.md)
* [Erste Schritte mit dem Azure-Schlüsseltresor](../key-vault/key-vault-get-started.md)
* [Azure Key Vault-Blog](https://blogs.technet.microsoft.com/kv/)

## <a name="virtual-machine-disk-encryption"></a>Verschlüsselung der Datenträger virtueller Computer
Azure Disk Encryption ist eine neue Funktion, mit der Sie die Datenträger von virtuellen Azure-Computern unter Windows und Linux verschlüsseln können. Azure Disk Encryption verwendet das Branchen-Standardfeature [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) von Windows und das Feature [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt) von Linux, um Volumeverschlüsselung für das Betriebssystem und die Datenträger bereitzustellen.

Die Lösung ist in Azure Key Vault integriert, damit Sie die Schlüssel für die Datenträgerverschlüsselung und die geheimen Schlüssel unter Ihrem Schlüsseltresorabonnement steuern und verwalten können. Gleichzeitig können Sie sicherstellen, dass alle Daten auf den Datenträgern von virtuellen Computern im Ruhezustand in Ihrem Azure-Speicher verschlüsselt sind.

Weitere Informationen:

* [Azure-Datenträgerverschlüsselung für virtuelle Windows- und Linux-IaaS-Computer](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)
* [Azure Disk Encryption für virtuelle Computer unter Windows und Linux](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/azure-disk-encryption-for-linux-and-windows-virtual-machines-public-preview-now-available/)
* [Verschlüsseln eines virtuellen Computers](../security-center/security-center-disk-encryption.md)

## <a name="virtual-machine-backup"></a>Sicherung virtueller Computer
Azure Backup ist eine skalierbare Lösung, die Ihre Anwendungsdaten schützt – ganz ohne Investitionskosten und mit sehr niedrigen Betriebskosten. Anwendungsfehler können Ihre Daten beschädigen, und menschliche Fehler können Bugs in Ihren Anwendungen verursachen. Mit Azure Backup sind Ihre virtuellen Windows- und Linux-Computer geschützt.

Weitere Informationen:

* [Was ist Azure Backup?](../backup/backup-introduction-to-azure-backup.md)
* [Azure Backup-Lernpfad](https://azure.microsoft.com/documentation/learning-paths/backup/)
* [Azure Backup-Dienst – FAQ](../backup/backup-azure-backup-faq.md)

## <a name="azure-site-recovery"></a>Azure Site Recovery
Ein wichtiger Teil der BCDR-Strategie Ihrer Organisation ist die Ermittlung, wie geschäftliche Workloads und Apps verfügbar gehalten werden können, wenn geplante und ungeplante Ausfälle auftreten. Azure Site Recovery unterstützt die Orchestrierung von Replikation, Failover und Wiederherstellung von Workloads und Apps, damit diese Komponenten über einen sekundären Standort zur Verfügung stehen, wenn der primäre Standort ausfällt.

Site Recovery:

* **Vereinfachung Ihrer BCDR-Strategie:** Site Recovery vereinfacht die Replikation, das Failover und die Wiederherstellung mehrerer Geschäftsworkloads und -Apps von einem einzelnen Standort. Site Recovery orchestriert die Replikation und das Failover, aber das Tool fängt keine Anwendungsdaten ab und besitzt auch keinerlei Informationen hierzu.
* **Ermöglicht flexible Replikation** – Mithilfe von Site Recovery können Sie Workloads auf virtuellen Hyper-V-Computern, virtuellen VMware-Computern und physischen Windows-/Linux-Servern replizieren.
* **Einfaches Failover und leichte Wiederherstellung werden unterstützt:** Site Recovery verfügt über Testfailover, mit denen Übungen zur Notfallwiederherstellung unterstützt werden, ohne dass sich dies auf Produktionsumgebungen auswirkt. Außerdem können Sie geplante Failover ohne Datenverlust für erwartete Ausfälle oder ungeplante Failover mit minimalem Datenverlust (je nach Replikationsfrequenz) für unerwartete Notfälle ausführen. Nach dem Failover können Sie ein Failback zu Ihren primären Standorten durchführen. Site Recovery verfügt über Wiederherstellungspläne, die Skripts und Azure Automation-Arbeitsmappen enthalten können, sodass Sie das Failover und die Wiederherstellung von Anwendungen mit mehreren Ebenen anpassen können.
* **Beseitigung des sekundären Datencenters:** Sie können die Replikation zu einem sekundären lokalen Standort oder zu Azure durchführen. Wenn Sie Azure als Ziel für die Notfallwiederherstellung nutzen, vermeiden Sie die Kosten und Komplexität, die mit der Verwaltung eines sekundären Standorts verbunden sind. Replizierte Daten werden in Azure Storage gespeichert.
* **Integration in vorhandene BCDR-Technologien:** Site Recovery kann zusammen mit anderen BCDR-Features für Anwendungen eingesetzt werden. Beispielsweise können Sie Site Recovery verwenden, um das SQL Server-Back-End von geschäftlichen Workloads zu schützen. Dies umfasst auch die native Unterstützung für SQL Server Always On zum Verwalten des Failovers von Verfügbarkeitsgruppen.

Weitere Informationen:

* [Was ist Azure Site Recovery?](../site-recovery/site-recovery-overview.md)
* [Wie funktioniert Azure Site Recovery?](../site-recovery/site-recovery-components.md)
* [Welche Workloads werden durch Azure Site Recovery geschützt?](../site-recovery/site-recovery-workload.md)

## <a name="virtual-networking"></a>Virtuelle Netzwerke
Virtuelle Computer benötigen Netzwerkkonnektivität. Azure erfordert von einem virtuellen Computer eine Verbindung mit einem virtuellen Azure-Netzwerk, damit die Konnektivitätsanforderung unterstützt wird. Ein virtuelles Azure-Netzwerk ist ein logisches Konstrukt, das auf dem physischen Azure-Netzwerkfabric basiert. Jedes logische virtuelle Azure-Netzwerk ist von allen anderen virtuellen Azure-Netzwerken isoliert. Mit dieser Isolierung wird sichergestellt, dass der Netzwerkverkehr in Ihren Bereitstellungen nicht für andere Microsoft Azure-Kunden zugänglich ist.

Weitere Informationen:

* [Übersicht über die Netzwerksicherheit in Azure](security-network-overview.md)
* [Virtuelle Netzwerke im Überblick](../virtual-network/virtual-networks-overview.md)
* [Netzwerkfeatures und Partnerschaften für Enterpriseszenarien](https://azure.microsoft.com/blog/networking-enterprise/)

## <a name="security-policy-management-and-reporting"></a>Verwaltung von Sicherheitsrichtlinien und Berichtserstellung
Azure Security Center unterstützt Sie beim Verhindern, Erkennen und Beheben von Bedrohungen und sorgt für eine größere Transparenz und bessere Kontrolle in Bezug auf die Sicherheit Ihrer Azure-Ressourcen. Es bietet integrierte Sicherheitsüberwachung und Richtlinienverwaltung für Ihre Azure-Abonnements, hilft bei der Erkennung von Bedrohungen, die andernfalls möglicherweise unbemerkt bleiben, und kann gemeinsam mit einem breiten Spektrum an Sicherheitslösungen verwendet werden.

Mit Azure Security Center können Sie virtuelle Computer wie folgt optimieren und überwachen:

* Bereitstellen von [Sicherheitsempfehlungen](../security-center/security-center-recommendations.md) für virtuelle Computer, z.B. Anwenden von Systemupdates, Konfigurieren von ACL-Endpunkten, Aktivieren von Antischadsoftware, Aktivieren von Netzwerksicherheitsgruppen und Anwenden der Datenträgerverschlüsselung.
* Überwachen des Status Ihrer virtuellen Computern

Weitere Informationen:

* [Einführung in Azure Security Center](../security-center/security-center-intro.md)
* [Azure Security Center – Häufig gestellte Fragen](../security-center/security-center-faq.md)
* [Planungs- und Betriebshandbuch für Azure Security Center](../security-center/security-center-planning-and-operations-guide.md)

## <a name="compliance"></a>Compliance
Azure Virtual Machines ist für FISMA, FedRAMP, HIPAA, PCI DSS Level 1 und andere Complianceprogramme zertifiziert. So können Sie auch mit Ihren eigenen Azure-Anwendungen die verschiedenen Complianceanforderungen besser erfüllen, und Ihr Unternehmen kann sicherstellen, dass diverse nationale und internationale rechtliche Vorgaben eingehalten werden.

Weitere Informationen:

* [Microsoft Trust Center – Compliance](https://www.microsoft.com/TrustCenter/Compliance/default.aspx)
* [Die vertrauenswürdige Cloud: Sicherheit, Datenschutz und Compliance von Microsoft Azure](http://download.microsoft.com/download/1/6/0/160216AA-8445-480B-B60F-5C8EC8067FCA/WindowsAzure-SecurityPrivacyCompliance.pdf)

