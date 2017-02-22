---
title: Azure Government-Compliance | Microsoft Docs
description: "Bietet eine Übersicht über die verfügbaren Compliancedienste für Azure Government"
services: azure-government
cloud: gov
documentationcenter: 
author: jomolesk
manager: zakramer
ms.assetid: 1d2e0938-482f-4f43-bdf6-0a5da2e9a185
ms.service: azure-government
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 12/07/2016
ms.author: jomolesk
translationtype: Human Translation
ms.sourcegitcommit: 474ee83dfc8532f8410b79c6426af3cad6f3d320
ms.openlocfilehash: cd54456c8f9e0f37f53973fca80b8bd3514b62d1


---
# <a name="azure-government-compliance"></a>Azure Government-Compliance

## <a name="azure-blueprint"></a>Azure Blueprint

Das Azure Blueprint-Programm ist darauf ausgelegt, die sichere und konforme Nutzung von Azure durch Behörden und Drittanbieter, die im Auftrag der Regierung Builds erstellen, zu ermöglichen. Azure Government-Kunden können die P-ATO (Provisional Authority to Operate, vorläufige Betriebsgenehmigung) FedRAMP JAB oder die PA (Provisional Authorization, vorläufige Autorisierung) DoD von Azure Government nutzen und so den Umfang der Sicherheitskontrollen im Verantwortungsbereich des Kunden in Azure-basierten Systemen verringern. Durch Erben der Implementierung von Sicherheitskontrollen von Azure Government können Kunden sich auf die Implementierung von Kontrollmaßnahmen konzentrieren, die spezifisch für ihre in Azure erstellten IaaS-, PaaS- oder SaaS-Umgebungen sind.

> [!NOTE]
> Im Rahmen von Azure Blueprint bezieht sich „Kunde“ auf die Organisation, die Builds direkt in Azure erstellt. Azure-Kunden sind z.B. unabhängige Softwareanbieter, die Builds im Namen der Regierung erstellen, oder Behörden, die Builds direkt in Azure erstellen.

## <a name="blueprint-customer-responsibilities-matrix"></a>Blueprint Customer Responsibilities Matrix

Die Azure Blueprint Customer Responsibilities Matrix (CRM) dient zur Unterstützung von Azure Government-Kunden beim Implementieren und Dokumentieren systemspezifischer Sicherheitskontrollen, die in Azure implementiert werden. In der CRM werden alle Anforderungen an das Sicherheitsprotokoll [NIST SP 800-53](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-53r4.pdf) für FedRAMP- und DISA-Baselines, bei denen die Anforderung einer Implementierung durch den Kunden eingeschlossen ist, ausdrücklich aufgeführt. Hierzu gehören Kontrollmaßnahmen mit gemeinsamer Verantwortung zwischen Azure und Azure-Kunden sowie Kontrollmaßnahmen, die vollständig von Azure-Kunden implementiert werden müssen. Falls zutreffend, sind Kontrollmaßnahmen mit Abstufungen für untergeordnete Kontrollanforderungen unterteilt, die präzisere Hilfe ermöglicht.

Das CRM-Format ist als Hilfestellung ausgelegt und betrifft nur die genaue Dokumentation des Kundenanteils implementierter Sicherheitskontrollen.

Kontrollmaßnahme AC-1 erfordert beispielsweise dokumentierte Access Control-Richtlinien und -Prozeduren für Systeme, die eine Betriebsgenehmigung beantragen. Für diese Kontrollmaßnahme weist Microsoft interne Azure-spezifische Richtlinien und Prozeduren in Bezug auf Access Control-Mechanismen auf, die zur Verwaltung der Azure-Infrastruktur und Azure-Plattform verwendet werden. Kunden müssen zudem ihre eigenen Access Control-Richtlinien und -Prozeduren erstellen, die in ihrem spezifischen in Azure erstellten System Anwendung finden. In der CRM werden die Teile AC-1a und AC-1b der Kontrollmaßnahmen dokumentiert. In AC-1a wird gefordert, dass die Richtlinien und Prozeduren spezifischen Inhalt aufweisen müssen, und in AC-1b wird gefordert, dass Kunden diese Dokumente einmal jährlich prüfen und aktualisieren müssen.

Die Blueprint CRM ist als Microsoft Excel-Arbeitsmappe für die FedRAMP-Baselines „Mittel“ und „Hoch“ und die DISA-Baselines für Cloud Computing SRG L4 und L5 verfügbar.

Um eine Kopie der Azure Blueprint CRM anzufordern oder Feedback abzugeben, senden Sie eine E-Mail an [AzureBlueprint@microsoft.com](mailto:AzureBlueprint@microsoft.com).

## <a name="blueprint-system-security-plan"></a>Blueprint System Security Plan

Die Azure Blueprint SSP-Vorlage (System Security Plan) ist kundenorientiert und für die Verwendung bei der Entwicklung eines SSP ausgelegt, der sowohl Kundenimplementierungen von Sicherheitskontrollen als auch von Azure geerbte Kontrollmaßnahmen dokumentiert. Kontrollmaßnahmen, die eine Kundenverantwortung einschließen, enthalten Leitfäden für die Dokumentation der Implementierung von Kontrollmaßnahmen mit ausführlicher und konformer Antwort. In Azure-Vererbungsabschnitten wird dokumentiert, wie Sicherheitskontrollen von Azure für den Kunden implementiert werden.

Der Azure Blueprint-SSP ist für die FedRAMP-Baselines „Mittel“ und „Hoch“ und die DISA-Baselines für Cloud Computing SRG L4 und L5 verfügbar. 

Um eine Kopie der Azure Blueprint SSP anzufordern oder Feedback abzugeben, senden Sie eine E-Mail an [AzureBlueprint@microsoft.com](mailto:AzureBlueprint@microsoft.com).

## <a name="azure-blueprint-implementation-guidance"></a>Azure Blueprint-Implementierungsleitfaden

Der Leitfaden zur Azure Blueprint-Implementierung erleichtert Cloudlösungsarchitekten und Sicherheitsmitarbeitern das Verständnis, wie Azure Government-Dienste und -Features bereitgestellt werden können, um einen Teil der im Verantwortungsbereich des Kunden liegenden FedRAMP- und DoD-Sicherheitskontrollen zu implementieren. Eine Vielzahl von Dokumentationen, Tools, Vorlagen und anderen Ressourcen steht als Hilfestellung für die sichere Bereitstellung von Azure-Diensten und -Features zur Verfügung. Azure-Ressourcen können mithilfe von [Bausteinen](https://github.com/mspnp/template-building-blocks) für Azure Resource Manager-Vorlagen oder von der Community bereitgestellten Azure-[Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/) oder durch [vom Kunden autorisierte](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) JSON-basierte Resource Manager-Vorlagen bereitgestellt werden. Die Architektur einer einfachen Bereitstellung in Azure umfasst Compute-, Netzwerk- und Speicherressourcen. In diesem Implementierungsleitfaden wird erläutert, wie diese Ressourcen so bereitgestellt werden können, dass die Anforderungen an die Implementierung von Sicherheitskontrollen erfüllt werden.

### <a name="virtual-machines"></a>Virtual Machines

Mit Azure Resource Manager-Vorlagen können vorkonfigurierte virtuelle Computer bereitgestellt werden, die die sicherheitstechnischen Implementierungsrichtlinien (Security Technical Implementation Guides, STIGs) und Sicherheitsbenchmarks der US-Behörden und der Branche erfüllen. Benutzerdefinierte virtuelle Azure-Computer können mit einem bestehenden vorkonfigurierten Computer erstellt werden, und es ist möglich, einen neuen virtuellen Computer bereitzustellen und eine Sicherheitsrichtlinie mithilfe von Active Directory für mit der Domäne verbundene Computer bzw. des Hilfsprogramms für lokale Gruppenrichtlinienobjekte für eigenständige Computer zu übernehmen. Die benutzerdefinierte Skripterweiterung für virtuelle Azure-Computer kann zum Verwalten dieser Konfigurationsaufgaben nach der Bereitstellung verwendet werden.

Die Konfiguration von virtuellen Windows-Computern kann Implementierungen von Sicherheitskontrollen enthalten. Dazu gehöre u.a. die folgenden:

- Benachrichtigungen und Bestätigungen zur Systemnutzung [NIST SP 800-53, Kontrollmaßnahme AC-8]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AC-8: *Ein konfigurationsgesteuertes Gruppenrichtlinienobjekt wird auf alle Windows-Computer in Azure angewendet, die vom Kunden kontrolliert werden. Das Gruppenrichtlinienobjekt wird gemäß dem CIS-Benchmark für Windows Server 2012 R2 konfiguriert und an die Anforderungen der Organisation angepasst. Dazu gehört auch die Verwendung geeigneter Benachrichtigungen zur Systemnutzung, die Benutzer vor der Anmeldung bestätigen müssen (siehe CIS-Benchmark, Abschnitte 2.3.7.4, 2.3.7.5).*

- Einschränkungen für das lokale Computerkonto einschließlich Kontosperre [NIST SP 800-53, Kontrollmaßnahme AC-7] Anzahl gleichzeitiger Sitzungen [NIST SP 800-53, Kontrollmaßnahme AC-10], Sitzungssperre [NIST SP 800-53, Kontrollmaßnahme AC-11], Authentifikatorverwaltung [NIST SP 800-53, Kontrollmaßnahme IA-5] und andere

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AC-7: *Ein konfigurationsgesteuertes Gruppenrichtlinienobjekt wird auf alle Windows-Computer in Azure angewendet, die vom Kunden kontrolliert werden. Das Gruppenrichtlinienobjekt wird gemäß dem CIS-Benchmark für Windows Server 2012 R2 konfiguriert und an die Anforderungen der Organisation angepasst. Dazu gehört auch die Einleitung einer Kontosperre nach einer bestimmten Anzahl aufeinanderfolgender, nicht erfolgreicher Anmeldeversuche (siehe CIS-Benchmark, Abschnitte 1.2.1, 1.2.2, 1.2.3. Hinweis: Die Standardwerte im CIS-Benchmark müssen an die Parameteranforderungen von FedRAMP und DoD angepasst werden).*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AC-10: *Ein konfigurationsgesteuertes Gruppenrichtlinienobjekt wird auf alle Windows-Computer in Azure angewendet, die vom Kunden kontrolliert werden. Das Gruppenrichtlinienobjekt wird gemäß dem CIS-Benchmark für Windows Server 2012 R2 konfiguriert und an die Anforderungen der Organisation angepasst. Dazu gehört auch die Beschränkung von Benutzern auf eine Sitzung des Remotedesktopdiensts gemäß den Anforderungen von FedRAMP und DoD (siehe CIS-Benchmark, Abschnitt 18.9.48.3.2).*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AC-11: *Ein konfigurationsgesteuertes Gruppenrichtlinienobjekt wird auf alle Windows-Computer in Azure angewendet, die vom Kunden kontrolliert werden. Das Gruppenrichtlinienobjekt wird gemäß dem CIS-Benchmark für Windows Server 2012 R2 konfiguriert und an die Anforderungen der Organisation angepasst. Dazu gehört auch die Einleitung einer Sitzungssperre nach 900 Sekunden (15 Minuten) Inaktivität bis zur erneuten Authentifizierung durch den Benutzer (siehe CIS-Benchmark, Abschnitte 2.3.7.3, 19.1.3.1, 19.1.3.3).*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme IA-5: *Ein konfigurationsgesteuertes Gruppenrichtlinienobjekt wird auf alle Windows-Computer in Azure angewendet, die vom Kunden kontrolliert werden. Das Gruppenrichtlinienobjekt wird gemäß dem CIS-Benchmark für Windows Server 2012 R2 konfiguriert und an die Anforderungen der Organisation angepasst. Dazu gehören auch Einschränkungen für die Kennwörter von lokalen Konten, die Werte für die kürzeste (1 Tag) und höchste Gültigkeitsdauer (60 Tage) sowie Bedingungen für die erneute Verwendung (24 Kennwörter), die Länge (14 Zeichen) und die Komplexität erzwingen müssen, um die FedRAMP-Anforderungen zu erfüllen (siehe. CIS-Benchmark, Abschnitte 1.1.1, 1.1.2, 1.1.3, 1.1.4, 1.1.5. Hinweis: Die Standardwerte im CIS-Benchmark müssen an die Parameteranforderungen von DoD angepasst werden).*

- Konfigurationseinstellungen, einschließlich der Mindestfunktionalität [NIST SP 800-53, Kontrollmaßnahmen CM-6, CM-7]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme CM-6: *Ein konfigurationsgesteuertes Gruppenrichtlinienobjekt wird für alle Windows-Computer in Azure verwaltet, die vom Kunden kontrolliert werden. Das Gruppenrichtlinienobjekt wird gemäß dem CIS-Benchmark für Windows Server 2012 R2 konfiguriert und an die Anforderungen der Organisation und die Betriebsanforderungen des Systems angepasst. Dieses Gruppenrichtlinienobjekt wird zusätzlich zu anderen Konfigurationsressourcen eingerichtet, um die einheitliche Implementierung von Konfigurationseinstellungen zu dokumentieren und sicherzustellen.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme CM-7: *Ein konfigurationsgesteuertes Gruppenrichtlinienobjekt wird für alle Windows-Computer in Azure verwaltet, die vom Kunden kontrolliert werden. Das Gruppenrichtlinienobjekt wird gemäß dem CIS-Benchmark für Windows Server 2012 R2 konfiguriert und an die Anforderungen der Organisation und die Betriebsanforderungen des Systems angepasst. Das Gruppenrichtlinienobjekt wird zum Einrichten einer Baseline für Images von virtuellen Computern verwendet, die für alle Windows-Computer in Azure verwaltet wird. Sie gibt lediglich die mindestens erforderlichen Funktionen an, die für den Betrieb des Systems notwendig sind.*

  Baseline-Wartungstools für die Sicherheit, zu denen vorkonfigurierte Gruppenrichtlinienobjekte (für Windows-Computer) und Shell-Skripts (für Linux-Computer) gehören, stehen bei [Microsoft](https://technet.microsoft.com/itpro/windows/keep-secure/windows-security-baselines) (für Windows-Computer) und beim [Center for Internet Security](https://benchmarks.cisecurity.org/) sowie der [Defense Information Systems Agency (DISA)](http://iase.disa.mil/stigs) (für Windows- und Linux-Computer) zur Verfügung.  

Virtuelle Azure-Computer können verschlüsselt werden, um die Sicherheitsanforderungen zum Schutz von ruhenden Daten einzuhalten [NIST SP 800-53, Kontrollmaßnahme SC-28]. Es stehen Azure-Schnellstartvorlagen für das Bereitstellen der Verschlüsselung für [neue](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image) und vorhandene virtuelle [Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm)- und [Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm)-Computer zur Verfügung. 

> Beispielangabe für die Implementierung der Kontrollmaßnahme SC-28: *Kundengesteuerte virtuelle Computer in Azure implementieren eine Datenträgerverschlüsselung zum Schutz der Vertraulichkeit und Integrität der ruhenden Daten. Die Azure Disk Encryption für Windows wird mithilfe des BitLocker-Features von Windows implementiert. Die Datenträgerverschlüsselung für virtuelle Linux-Computer wird mit dem Feature „DM-Crypt“ von Linux implementiert.

Es können verschiedene Erweiterungen für virtuelle Azure-Computer bereitgestellt werden, um Anforderungen für Sicherheitskontrollen zu erfüllen. Die [VM-Erweiterung Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) kann für neue und vorhandene virtuelle Computer bereitgestellt werden. Mit der Antimalware-Erweiterung können Echtzeitschutz und regelmäßige geplante Überprüfungen eingerichtet werden [NIST SP 800-53, Kontrollmaßnahme SI-3].

> Beispielangabe für die Implementierung der Kontrollmaßnahme SI-3: *Hostbasierter Schutz von Antischadsoftware für vom Kunden kontrollierte virtuelle Windows-Computer in Azure wird mithilfe der VM-Erweiterung Microsoft Antimalware implementiert. Diese Erweiterung ist so konfiguriert, dass sowohl in Echtzeit als auch regelmäßig (wöchentlich) Überprüfungen durchgeführt werden, dass das Antischadsoftware-Modul und die Schutzsignaturen automatisch aktualisiert werden, dass automatische Wartungsaufgaben durchgeführt werden und dass Benachrichtigungen über Azure-Diagnose bereitgestellt werden.*

*Weitere Tools und Ressourcen*

[Bausteine](https://github.com/mspnp/template-building-blocks) für Azure Resource Manager-Vorlagen können angepasst und zum Bereitstellen von Azure-Ressourcen verwendet werden. Dies umfasst auch die Bereitstellung von [virtuellen Computern](https://github.com/mspnp/template-building-blocks/tree/master/templates/buildingBlocks/multi-vm-n-nic-m-storage) und [VM-Erweiterungen](https://github.com/mspnp/template-building-blocks/tree/master/templates/buildingBlocks/virtualMachine-extensions). 

### <a name="virtual-network"></a>Virtuelles Netzwerk

Azure Virtual Network (VNET) ermöglicht die vollständige Steuerung der Sicherheitsrichtlinien und der Weiterleitungen in Architekturen mit virtuellen Netzwerken durch die Bereitstellung und Konfiguration von Subnetzen, Netzwerksicherheitsgruppen und benutzerdefinierter Routen. Netzwerksicherheitsgruppen können auf Subnetze oder einzelne Computer angewendet werden, um Ressourcen logisch nach der Workload, basierend auf einer Architektur mit mehreren Ebenen oder zu einem anderen Zweck logisch voneinander zu trennen. In der Referenzarchitektur unten werden Ressourcen in separate Subnetze für die Ebenen Web, Business und Daten sowie Subnetze für Active Directory-Ressourcen und die Verwaltung unterteilt. Auf jedes Subnetz werden Netzwerksicherheitsgruppen angewendet, um den Netzwerkdatenverkehr innerhalb des virtuellen Netzwerks einzuschränken. 
 
![N-Ebenen-Architektur mit Microsoft Azure](./media/documentation-government-plan-compliance/compute-n-tier.png)

Netzwerksicherheitsgruppen ermöglichen die vollständige Kontrolle der Kommunikationspfade zwischen Ressourcen [NIST SP 800-53, Kontrollmaßnahme AC-4].

> Beispielangabe für die Implementierung der Kontrollmaßnahme AC-4: *Kundengesteuerte virtuelle Computer in Azure, die Funktionen der Ebenen Web, Business und Daten implementieren, werden durch Subnetze voneinander getrennt. Für jedes Subnetz werden Netzwerksicherheitsgruppen definiert und angewendet, die den Informationsfluss auf der Netzwerkebene auf ein Minimum für die Funktionsfähigkeit des Systems einschränken.*

Netzwerksicherheitsgruppen können auf die ausgehende Kommunikation von Subnetzen und virtuellen Computern angewendet werden. Dies ermöglicht die vollständige Kontrolle über die Kommunikation zwischen Informationssystemkomponenten in Azure und externen Informationssystemen [NIST SP 800-53, Kontrollmaßnahme CA-3 (5)]. Die Verarbeitung von Netzwerksicherheitsgruppen-Regeln erfolgt so, dass zunächst alle Berechtigungen verweigert und nur Ausnahmen zugelassen werden. Darüber hinaus können benutzerdefinierte Routen konfiguriert werden, um die eingehende und ausgehende Kommunikation bestimmter Subnetze und virtueller Computer über ein virtuelles Gerät, wie z.B. eine Firewall oder IDS/IPS, zu leiten, um die Kommunikation im System präziser zu steuern.

> Beispielangabe für die Implementierung der Kontrollmaßnahme CA-3 (5): *Die gesamte von kundengesteuerten Ressourcen in Azure ausgehende Kommunikation wird durch die Implementierung von Netzwerksicherheitsgruppen eingeschränkt, deren konfigurierter ausgehender Regelsatz den gesamten Datenverkehr verweigert, der nicht explizit zugelassen wird, um Funktionen des Informationssystems zu unterstützen. Damit werden die Anforderungen von FedRAMP und DoD L4 erfüllt. Darüber hinaus werden benutzerdefinierte Routen konfiguriert, um den gesamten ausgehenden Datenverkehr über ein virtuelles Firewallgerät weiterzuleiten, das mit einem Regelsatz konfiguriert ist, der die Kommunikation nur mit zugelassenen externen Informationssystemen genehmigt.*

Die oben dargestellte Referenzarchitektur veranschaulicht, wie Azure-Ressourcen logisch in separaten Subnetzen gruppiert werden können, auf die Regelsätze für Netzwerksicherheitsgruppen angewendet werden, um sicherzustellen, dass Sicherheitsfunktionen von anderen Funktionen isoliert sind. In diesem Fall werden die drei Webanwendungsebenen vom Active Directory-Subnetz und dem Verwaltungssubnetz isoliert, das Tools und Ressourcen für das Informationssystem und das Sicherheitsmanagement hosten könnte [NIST SP 800-53, Kontrollmaßnahme SC-3].

> Beispielangabe für die Implementierung der Kontrollmaßnahme SC-3: *Sicherheitsfunktionen werden von nicht sicherheitsrelevanten Funktionen innerhalb der kundenkontrollierten Azure-Umgebung getrennt, indem Subnetze sowie Netzwerksicherheitsgruppen, die auf diese Subnetzen angewendet werden, implementiert werden. Die Ressourcen des Informationssystems für die Ebenen Web, Business und Daten der Webanwendung werden logisch vom Verwaltungssubnetz getrennt, in dem sicherheitsrelevante Aufgaben für das Informationssystem ausgeführt werden.*

Außerdem implementiert die Referenzarchitektur verwaltete Access Control-Punkte für den Remotezugriff auf das Informationssystem [NIST SP 800-53, Kontrollmaßnahme AC-17 (3)]. Ein Load Balancer für den Internetzugriff wird zum Verteilen des eingehenden Internetdatenverkehrs für die Webanwendung bereitgestellt. Außerdem enthält das Verwaltungssubnetz eine Jumpbox oder einen geschützten Host, über den der gesamte verwaltungsbezogene Remotezugriff auf das System kontrolliert wird. Netzwerksicherheitsgruppen schränken den Datenverkehr innerhalb des virtuellen Netzwerke ein, um sicherzustellen, dass externer Datenverkehr nur an die angegebenen öffentlichen Ressourcen weitergeleitet wird.

> Beispielangabe für die Implementierung der Kontrollmaßnahme AC-17 (3): *Remotezugriff auf kundenkontrollierte Komponenten des Informationssystems innerhalb von Azure wird auf zwei verwaltete Access Control-Punkte im Netzwerk beschränkt. 1) Internetdatenverkehr für die Webanwendung wird über einen Load Balancer mit Internetzugriff verwaltet, der den Datenverkehr auf Ressourcen der Webebene verteilt. (2) Remoteverwaltungszugriffe werden über einen geschützten Host an ein separates Subnetz in der Umgebung verwaltet. Die auf das Verwaltungssubnetz mit der Jumpbox angewendete Netzwerksicherheitsgruppe erlaubt nur Verbindungen von öffentlichen IP-Adressen auf einer Whitelist. Außerdem sind Verbindungen auf Remotedesktop-Datenverkehr beschränkt.*

Netzwerksicherheitsgruppen ermöglichen die vollständige Kontrolle über die Kommunikation zwischen Azure-Ressourcen und externen Hosts und Systemen sowie zwischen internen Subnetzen und Hosts. Sie trennen Komponenten des Informationssystems, die als öffentlich zugänglich konfiguriert sind, von solchen, für die dies unzulässig ist. Zusätzlich zu den Lösungen in der Referenzarchitektur oben ermöglicht Azure die Bereitstellung virtueller Geräte, z.B. Firewall- und IDS/IPS-Lösungen, die bei Verwendung in Verbindung mit benutzerdefinierten Routen die Verbindungen zwischen Azure-Ressourcen und externen Netzwerken und Informationssystemen zusätzlich schützen und steuern [NIST SP 800-53, Kontrollmaßnahme SC-7].

> Beispielangabe für die Implementierung der Kontrollmaßnahme SC-7: *Kundenkontrollierte Ressourcen in Azure werden durch mehrere Mechanismen zum Schutz der Grenzen geschützt. Netzwerksicherheitsgruppen werden definiert und auf Netzwerksubnetze angewendet, um den Datenverkehr an den Grenzen des Informationssystems und innerhalb des virtuellen Netzwerks zu beschränken. Netzwerksicherheitsgruppen enthalten Regeln für Zugriffssteuerungslisten, die den Datenverkehr zu und von Subnetzen und virtuellen Computern steuern. Load Balancer werden für das Verteilen des eingehenden Internetdatenverkehrs auf bestimmte Ressourcen bereitgestellt. Subnetze stellen sicher, dass öffentlich zugängliche Komponenten des Informationssystems logisch von nicht öffentlichen Ressourcen getrennt sind.*

Regelsätze für Netzwerksicherheitsgruppen ermöglichen Einschränkungen für bestimmte Ports und Protokolle im Netzwerk. Dies ist ein zentraler Aspekt, um sicherzustellen, dass in der Implementierung von Informationssystemen nur grundlegende Funktionen bereitgestellt werden [NIST SP 800-53, Kontrollmaßnahme CM-7].

> Beispielangabe für die Implementierung der Kontrollmaßnahme CM-7: *Die Anwendung der Netzwerksicherheitsgruppen auf Subnetze und virtuelle Computer in der kundenkontrollierten Azure-Umgebung erfolgt so, dass zunächst sämtliche Berechtigungen verweigert und nur Ausnahmen zugelassen werden. Dadurch wird sichergestellt, dass nur Netzwerkdatenverkehr über explizit zugelassene Ports und Protokolle zugelassen wird, um das Konzept der geringsten Funktionalität umzusetzen.*

*Weitere Tools und Ressourcen*

Die Dokumentation zur oben dargestellten Referenzarchitektur finden Sie auf der [Dokumentationswebsite](https://docs.microsoft.com/azure/guidance/guidance-compute-n-tier-vm) von Azure. Azure Resource Manager-Vorlagen zum Bereitstellen der Referenzarchitektur finden Sie auf derselben Seite. Die Azure-Dokumentationswebsite enthält auch ausführliche Informationen zu [Netzwerksicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) und [benutzerdefinierten Routen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview).

### <a name="storage"></a>Speicher

In Azure Storage gespeicherte Daten werden repliziert, um eine hohe Verfügbarkeit sicherzustellen. Während mehrere Replikationsoptionen verfügbar sind, sorgen georedundanter Speicher (GRS) und georedundanter Speicher mit Lesezugriff (RA-GRS) dafür, dass die Daten in eine zweite Region repliziert werden. Die primäre und sekundäre Region werden zugeordnet, um sicherzustellen, dass ausreichend Abstand zwischen den Rechenzentren besteht, um bei einem Ausfall eines großen Gebiets oder bei einem Notfall die Verfügbarkeit sicherzustellen [NIST SP 800-53, Kontrollmaßnahme CP-9]. Wählen Sie für georedundante Speicherung mit hoher Verfügbarkeit beim Erstellen eines neuen Speicherkontos entweder georedundanten Speicher (GRS) oder georedundanter Speicher mit Lesezugriff (RA-GRS) aus. 

> Beispielangabe für die Implementierung der Kontrollmaßnahme CP-9: *Alle kundenkontrollierten Speicherkonten in Azure implementieren georedundanten Speicher, der sicherstellt, dass sechs Kopien sämtlicher Daten auf separaten Knoten in zwei Rechenzentren verwaltet werden.*

Azure Storage Service Encryption (SSE) schützt ruhende Daten in Azure Storage-Konten [NIST SP 800-53, Kontrollmaßnahme SC-28 SC-28 (1)]. Bei Aktivierung verschlüsselt Azure Daten vor der Weitergabe an den Speicher automatisch. Die Daten werden mithilfe von 256-Bit-AES-Verschlüsselung verschlüsselt. SSE unterstützt die Verschlüsselung von Block-, Anfüge- und Seitenblobs.

> Beispielangabe für die Implementierung der Kontrollmaßnahmen SC-28 und SC-28 (1): *Vertraulichkeit und Integrität aller Speicherblobs in der kundenkontrollierten Azure-Umgebung werden mithilfe von Azure SSE geschützt. Dabei kommt eine 256-Bit-AES-Verschlüsselung für alle ruhenden Data zum Einsatz.*

*Weitere Tools und Ressourcen*

Die Azure-Dokumentationswebsite enthält ausführliche Informationen zu [Storage Service Encryption](https://docs.microsoft.com/azure/storage/storage-service-encryption) und zur [Speicherreplikation](https://docs.microsoft.com/azure/storage/storage-redundancy). 

### <a name="azure-active-directory"></a>Azure Active Directory

Azure Active Directory stellt Identitäts- und Zugriffsfunktionen für Informationssysteme in Microsoft Azure bereit. Durch die Verwendung von Verzeichnisdiensten, Sicherheitsgruppen und Gruppenrichtlinien können Kunden die Zugriffs- und Sicherheitsrichtlinien von Computern steuern, die Azure Active Directory verwenden. Mithilfe von Konten und Sicherheitsgruppen kann der Zugriff auf das Informationssystem verwaltet werden. Gruppenrichtlinien können sicherstellen, dass die Konfigurationsanforderungen erfüllt sind.

Azure Active Directory-Sicherheitsgruppen und Verzeichnisdienste können helfen, Schemas für die rollenbasierten Zugriffskontrolle (RBAC) zu implementieren und den Zugriff auf das Informationssystem zu steuern. Dies kann auch Implementierungen von Sicherheitskontrollen für Folgendes umfassen:

- Kontenverwaltung [NIST SP 800-53, Kontrollmaßnahme AC-2]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AC-2.a: *Azure Active Directory verwaltet den Zugriff durch die Implementierung der rollenbasierten Zugriffssteuerung anhand von Active Directory-Gruppen. Es gibt festgelegte Anforderungen für die Kontoverwaltung und den Zugriff auf Konten in den von AAD unterstützten Domänen. Der Zugriff auf Mitgliedsservern der Domäne wird nur von Sicherheitsgruppen unterstützt. Jede Gruppe hat einen primären und einen sekundären Besitzer. Diese Besitzer sind verantwortlich für die Einhaltung von Gruppenmitgliedschaften und Berechtigungen sowie für die Beschreibung.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AC-2.c: *Wenn ein Benutzer den Zugriff auf eine Sicherheitsgruppe anfordert, muss die Anforderung vom Besitzer der Gruppe basierend auf den für die Mitgliedschaft definierten Kriterien genehmigt werden. Die Kontobedingungen werden von den Besitzern der Sicherheitsgruppe bestimmt.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AC-2.f: *Azure Active Directory dient zum Verwalten des Kontrollzugriffs auf Informationssysteme. Kontoadministratoren erstellen, aktivieren, ändern, deaktivieren oder entfernen Informationssystemkonten nach Bedarf und gemäß den Verwaltungsrichtlinien.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AC-2 (1): *Azure Active Directory verwaltet den Zugriff durch die Implementierung der rollenbasierten Zugriffssteuerung anhand von Active Directory-Gruppen. Es gibt festgelegte Anforderungen für die Kontoverwaltung und den Zugriff auf Konten in den von AAD unterstützten Domänen. Der Zugriff auf Mitgliedsservern der Domäne wird nur von Sicherheitsgruppen unterstützt. Jede Gruppe hat einen primären und einen sekundären Besitzer. Diese Besitzer sind verantwortlich für die Einhaltung von Gruppenmitgliedschaften und Berechtigungen sowie für die Beschreibung.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AC-2 (2): *Azure Active Directory dient zum Verwalten des Kontrollzugriffs auf Informationssysteme. Kontoadministratoren erstellen temporäre Konten gemäß den Richtlinien für die Kontoverwaltung. Für diese temporären Konten muss entsprechend den Richtlinienanforderungen ein Ablauf festgelegt werden.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AC-2 (3): *Azure Active Directory verwaltet den Kontrollzugriff auf Informationssysteme. Kontoadministratoren erstellen temporäre Konten gemäß den Richtlinien für die Kontoverwaltung. Für diese temporären Konten muss entsprechend den Richtlinienanforderungen ein Ablauf festgelegt werden.*

  >Beispielangabe für die Implementierung der Kontrollmaßnahme AC-2 (7): *Azure Active Directory verwaltet den Kontrollzugriff auf Informationssystemen gemäß dem Schema für den rollenbasierten Zugriff, der Berechtigungen für das Informationssystem in Rollen organisiert, die Sicherheitsgruppen zugewiesen werden. Sicherheitsgruppen-Administratoren sind dafür verantwortlich, Benutzern den Zugriff zu gewähren, indem sie diese der richtigen Sicherheitsgruppe zuweisen. Jeder Sicherheitsgruppe werden Berechtigungen mit den minimal erforderlichen Zugriffsrechten für die ordnungsgemäße Erfüllung ihrer Aufgaben zugewiesen.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AC-2 (11): *Azure Active Directory verwaltet den Kontrollzugriff auf Informationssysteme in Azure. Nutzungseinschränkungen, die aufgrund der Sicherheitsrichtlinien erforderlich sind, werden definiert und in Azure Active Directory von Kontoadministratoren erzwungen.*

- Zugriffserzwingung [NIST SP 800-53, Kontrollmaßnahme AC-3]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AC-3: *Azure Active Directory erzwingt genehmigte Autorisierungen für die Kundenumgebung anhand der rollenbasierten Zugriffssteuerung. Der Zugriff auf Azure Active Directory-Sicherheitsgruppen wird von Sicherheitsgruppen-Administratoren verwaltet. Benutzer werden gemäß ihren Rollen den entsprechenden Sicherheitsgruppen zugewiesen, wobei das Prinzip der geringsten Berechtigungen gilt.*

- Geringste Berechtigungen [NIST SP 800-53, Kontrollmaßnahme AC-6]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AC-6 (10): *Azure Active Directory erzwingt genehmigte Autorisierungen für die Kundenumgebung anhand der rollenbasierten Zugriffssteuerung. Der Zugriff auf Azure Active Directory-Sicherheitsgruppen wird von Sicherheitsgruppen-Administratoren verwaltet. Nicht berechtigte Benutzer erhalten keinen Zugriff auf Sicherheitsgruppen, die ihnen die Möglichkeit zum Zugriff auf privilegierte Funktionen bieten würden, einschließlich Berechtigungen zum Deaktivieren, Umgehen oder Ändern von Sicherheitsvorkehrungen.*

- Remotezugriff [NIST SP 800-53, Kontrollmaßnahme AC-17]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AC-17 (1): *Azure Active Directory dient zum Überwachen und Steuern des gesamten Remotezugriffs. Azure Active Directory umfasst Sicherheits-, Aktivitäts- und Überwachungsberichte für Ihr Verzeichnis.*

- Schutz von Überwachungsinformationen [NIST SP 800-53, Kontrollmaßnahme AU-9]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AU-9: *Strikt verwaltete Access Control-Maßnahmen dienen dem Schutz von Überwachungsinformationen und Tools vor nicht autorisiertem Zugriff, Änderungen und Löschungen. Azure Active Directory erzwingt die Genehmigung des logischen Zugriffs für Benutzer über Ebenen für die Autorisierung von Zugriffen und logische Kontrollen mithilfe von Active Directory-Richtlinien und rollenbasierten Gruppenmitgliedschaften. Die Möglichkeit zum Anzeigen von Überwachungsinformationen und Verwenden von Überwachungstools ist auf Benutzer beschränkt, die über diese Berechtigungen verfügen.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AU-9 (4): *Azure Active Directory beschränkt die Verwaltung von Überwachungsfunktionen auf Mitglieder der entsprechenden Sicherheitsgruppen. Nur Mitarbeiter, die Zugriff auf die Verwaltung von Überwachungsfunktionen zwingend benötigen, erhalten diese Berechtigungen.*

- Zugriffseinschränkungen für Änderungen [NIST SP 800-53, Kontrollmaßnahme CM-5]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme CM-5: *RBAC wird von Azure Active Directory erzwungen, um logische Zugriffsbeschränkungen im Zusammenhang mit Änderungen zu definieren, zu dokumentieren, zu genehmigen und zu erzwingen. Alle Konten, die innerhalb des Systems erstellt werden, sind rollenbasiert. Die Mitarbeiter fordern von den Kontoadministratoren den Zugriff an. Bei einer Genehmigung werden sie gemäß ihren Rollen den entsprechenden Sicherheitsgruppen zugeordnet. Der Zugriff auf die Produktionsumgebung ist nur Mitgliedern bestimmter Sicherheitsgruppen nach einer Genehmigung erlaubt.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme CM-5 (1): *Azure Active Directory erzwingt Beschränkungen für den logischen Zugriff über die Mitgliedschaft in Sicherheitsgruppen. Dazu müssen die Besitzer von Sicherheitsgruppen den Zugriff auf eine bestimmte Sicherheitsgruppe gewähren.*

- Benutzererkennung und -authentifizierung [NIST SP 800-53, Kontrollmaßnahme IA-2]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme IA-2: *Mitarbeiter, die auf die Umgebung des Informationssystems zugreifen, werden durch ihren Benutzernamen eindeutig identifiziert und mithilfe von Azure Active Directory authentifiziert. Es ist eine Authentifizierung in Azure Active Directory erforderlich, um Zugriff auf das Informationssystem zu erhalten.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme IA-2 (8): *Der Zugriff auf die Produktionsumgebung wird durch die integrierte Kerberos-Funktion von Azure Active Directory vor Replay-Angriffen geschützt. Bei der Kerberos-Authentifizierung enthält der vom Client gesendete Authentifikator zusätzliche Daten, z.B. eine verschlüsselte IP-Adressenliste, die Clientzeitstempel und die Ticketlebensdauer. Wenn ein Paket erneut verwendet wird, erfolgt eine Überprüfung des Zeitstempels. Ist der Zeitstempel älter als oder genauso alt wie der vorherige Authentifikator, wird das Paket zurückgewiesen.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme IA-2 (9): *Alle Konten werden durch die integrierte Kerberos-Funktion von Azure Active Directory vor Replay-Angriffen geschützt. Bei der Kerberos-Authentifizierung enthält der vom Client gesendete Authentifikator zusätzliche Daten, z.B. eine verschlüsselte IP-Adressenliste, die Clientzeitstempel und die Ticketlebensdauer. Wenn ein Paket erneut verwendet wird, erfolgt eine Überprüfung des Zeitstempels. Ist der Zeitstempel älter als oder genauso alt wie der vorherige Authentifikator, wird das Paket zurückgewiesen.*

- ID-Verwaltung [NIST SP 800-53, Kontrollmaßnahme IA-4]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme IA-4.b: *Azure Active Directory-Konto-IDs werden zur Identifizierung von Benutzern verwendet. Diese eindeutigen IDs werden nicht wiederverwendet.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme IA-4.c: *Azure Active Directory ist das zentrale Kontorepository, über das der Zugriff auf die Dienstumgebung gewährt wird. Wenn in Azure Active Directory Konten erstellt werden, wird diesen jeweils die eindeutige ID des Benutzers zugewiesen.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme IA-4.d: *Eindeutige Benutzer-IDs werden nie wiederverwendet. Dies wird von Azure Active Directory erzwungen.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme IA-4.e: *Alle Konten in Azure Active Directory sind so konfiguriert, dass sie nach 35 Tagen Inaktivität automatisch deaktiviert werden.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme IA-4 (4): *Azure Active Directory kennzeichnet Vertragspartner und Lieferanten mithilfe von Benennungskonventionen, die auf ihre eindeutigen Anmeldeinformationen in Azure Active Directory angewendet werden.*

- Authentifikatorverwaltung [NIST SP 800-53, Kontrollmaßnahme IA-5]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme IA-5.b: *Azure Active Directory weist eine eindeutige ID und ein zufälliges, temporäres Kennwort zu, das zum Zeitpunkt der Kontoerstellung den Richtlinienanforderungen entspricht. Azure Active Directory verwaltet die eindeutige ID, die dem Konto zugeordnet ist, über die gesamte Lebensdauer des Kontos. Konto-IDs werden in Azure Active Directory nie wiederholt.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme IA-5.c: *Azure Active Directory stellt sicher, dass die ausgegebenen Kennwörter die Richtlinienanforderungen zur Kennwortkomplexität erfüllen.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme IA-5.d: *Die Verfahren zur Erstausgabe von Authentifikatoren werden von Kontoadministratoren durchgeführt. Zugriffsänderungen bei Benutzerkonten durchlaufen den für AC-2 angegebenen Kontoverwaltungsprozess. Wenn ein Authentifikator verloren geht oder kompromittiert wird, führen die Kontoadministratoren nach Bedarf die vorgeschriebenen Prozesse für das Zurücksetzen, das erneute Ausstellen oder das Aufheben des Authentifikators durch.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme IA-5 (1).c: *Azure Active Directory stellt sicher, dass alle Kennwörter während der Speicherung und Übertragung kryptografisch geschützt werden. Von Azure Active Directory gespeicherte Kennwörter werden automatisch durch Azure Active Directory-Funktionen in Hashs umgewandelt.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme IA-5 (1).f: *Azure Active Directory dient zum Verwalten des Kontrollzugriffs auf das Informationssystem. Wenn ein Konto neu erstellt oder ein temporäres Kennwort generiert wird, erzwingt Azure Active Directory, dass der Benutzer sein Kennwort bei der nächsten Anmeldung ändert.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme IA-5 (4): *Azure Active Directory ist das automatisierte Tool, mit dem bestimmt wird, ob Kennwortauthentifikatoren ausreichend stark sind, um die in IA-5 (1) angegebenen Anforderungen zur Länge, Komplexität, Rotation und Lebensdauer zu erfüllen. Azure Active Directory stellt sicher, dass die Kennwortauthentifikatorstärke bei der Erstellung diese Standards erfüllt.*

Die Konfiguration von Azure Active Directory-Gruppenrichtlinien kann verwendet werden, um Sicherheitsrichtlinien für Kunden bereitzustellen, die bestimmte Sicherheitsanforderungen erfüllen. Diese Richtlinien können von Azure Active Directory auf virtuelle Azure-Kundencomputer angewendet werden.
Konfigurationen für Azure Active Directory-Gruppenrichtlinien können Implementierungen von Sicherheitskontrollen für Folgendes umfassen:

- Geringste Berechtigungen [NIST SP 800-53, Kontrollmaßnahme AC-6]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AC-6 (9): *Ein konfigurationsgesteuertes Gruppenrichtlinienobjekt wird auf alle Windows-Computer in Azure angewendet, die vom Kunden kontrolliert werden. Das Gruppenrichtlinienobjekt wird angepasst, um die Anforderungen der Organisation, einschließlich der Überwachung für die Ausführung von privilegierten Funktionen, zu erfüllen.*

- Nicht erfolgreiche Anmeldeversuche [NIST SP 800-53, Kontrollmaßnahme AC-7]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AC-7: *Ein konfigurationsgesteuertes Gruppenrichtlinienobjekt wird auf alle Windows-Computer in Azure angewendet, die vom Kunden kontrolliert werden. Das Gruppenrichtlinienobjekt wird an die Anforderungen der Organisation angepasst. Dazu gehört auch die Einleitung einer Kontosperre nach einer bestimmten Anzahl aufeinanderfolgender nicht erfolgreicher Anmeldeversuche.*

- Anzahl gleichzeitiger Sitzungen [NIST SP 800-53, Kontrollmaßnahme AC-10], Sitzungssperre [NIST SP 800-53, Kontrollmaßnahme AC-11], Sitzungsbeendigung [NIST SP 800-53, Kontrollmaßnahme AC-12]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AC-10: *Ein konfigurationsgesteuertes Gruppenrichtlinienobjekt wird auf alle Windows-Computer in Azure angewendet, die vom Kunden kontrolliert werden. Das Gruppenrichtlinienobjekt wird an die Anforderungen der Organisation angepasst. Dazu gehört auch die Beschränkung von Benutzern auf eine Sitzung des Remotedesktopdiensts gemäß den Anforderungen von FedRAMP und DoD.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AC-11.a: *Ein konfigurationsgesteuertes Gruppenrichtlinienobjekt wird auf alle Windows-Computer in Azure angewendet, die vom Kunden kontrolliert werden. Das Gruppenrichtlinienobjekt wird an die Anforderungen der Organisation angepasst. Dazu gehört auch die Einleitung einer Sitzungssperre nach 900 Sekunden (15 Minuten) Inaktivität bis zur erneuten Authentifizierung durch den Benutzer.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AC-12: *Windows beendet Remotedesktopsitzungen automatisch nach dem Empfang einer Abmeldeanforderung vom Kundenbenutzer. Außerdem wird ein konfigurationsgesteuertes Gruppenrichtlinienobjekt auf alle Windows-Computer in Azure angewendet, die vom Kunden kontrolliert werden. Das Gruppenrichtlinienobjekt wird an die Anforderungen der Organisation angepasst. Dazu gehört auch die Beendigung einer Remotedesktopsitzung nach 900 Sekunden (15 Minuten) Inaktivität.*

- Authentifikatorverwaltung [NIST SP 800-53, Kontrollmaßnahme IA-5]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme IA-5.f: *Ein konfigurationsgesteuertes Gruppenrichtlinienobjekt wird auf alle Windows-Computer in Azure angewendet, die vom Kunden kontrolliert werden. Das Gruppenrichtlinienobjekt wird an die Anforderungen der Organisation angepasst. Dazu gehören auch Einschränkungen für die Kennwörter von lokalen Konten, die Werte für die kürzeste (1 Tag) und höchste Gültigkeitsdauer (60 Tage) sowie Bedingungen für die erneute Verwendung (24 Kennwörter), die Länge (14 Zeichen) und die Komplexität erzwingen müssen, um die FedRAMP-Anforderungen zu erfüllen.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme IA-5.g: *Ein konfigurationsgesteuertes Gruppenrichtlinienobjekt wird auf alle Windows-Computer in Azure angewendet, die vom Kunden kontrolliert werden. Das Gruppenrichtlinienobjekt wird an die Anforderungen der Organisation angepasst. Dazu gehören auch Einschränkungen für die Kennwörter von lokalen Konten, die Werte für die höchste Gültigkeitsdauer (60 Tage) erzwingen müssen, um die FedRAMP-Anforderungen zu erfüllen.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme IA-5 (1).a: *Ein konfigurationsgesteuertes Gruppenrichtlinienobjekt wird auf alle Windows-Computer in Azure angewendet, die vom Kunden kontrolliert werden. Das Gruppenrichtlinienobjekt wird an die Anforderungen der Organisation angepasst. Dazu gehören auch Komplexitätsanforderungen, um die FedRAMP-Anforderungen zu erfüllen.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme IA-5 (1).b: *Ein konfigurationsgesteuertes Gruppenrichtlinienobjekt wird auf alle Windows-Computer in Azure angewendet, die vom Kunden kontrolliert werden. Das Gruppenrichtlinienobjekt wird an die Anforderungen der Organisation angepasst. Dazu gehören auch Komplexitätsanforderungen, um die FedRAMP-Anforderungen zu erfüllen.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme IA-5 (1).d: *Ein konfigurationsgesteuertes Gruppenrichtlinienobjekt wird auf alle Windows-Computer in Azure angewendet, die vom Kunden kontrolliert werden. Das Gruppenrichtlinienobjekt wird an die Anforderungen der Organisation angepasst. Dazu gehören auch Einschränkungen für die Kennwörter von lokalen Konten, die Werte für die kürzeste (1 Tag) und höchste Gültigkeitsdauer (60 Tage) erzwingen müssen, um die FedRAMP-Anforderungen zu erfüllen.

  > Beispielangabe für die Implementierung der Kontrollmaßnahme IA-5 (1).e: *Ein konfigurationsgesteuertes Gruppenrichtlinienobjekt wird auf alle Windows-Computer in Azure angewendet, die vom Kunden kontrolliert werden. Das Gruppenrichtlinienobjekt wird an die Anforderungen der Organisation angepasst. Dazu gehören auch Einschränkungen für die erneute Verwendung (24 Kennwörter), um die FedRAMP-Anforderungen zu erfüllen.*

*Weitere Tools und Ressourcen*

Dokumentationen, Tools, Vorlagen und andere Ressourcen stehen als Hilfestellung für die sichere Bereitstellung von Azure-Diensten und -Features zur Verfügung. Erste Schritte mit Azure Active Directory finden Sie unter [Microsoft Azure Docs](https://docs.microsoft.com/azure/active-directory/).

### <a name="key-vault"></a>Schlüsseltresor

Azure Key Vault schützt kryptografische Schlüssel und Geheimnisse, die von Cloudanwendungen und -diensten verwendet werden. Durch die Verwendung von Azure Key Vault können Kunden Schlüssel und Geheimnisse erstellen, verwalten und schützen. Sichere Container (Tresore) können zum Speichern und Verwalten von kryptografischen Schlüsseln und Geheimnissen verwendet werden. Azure Key Vault kann verwendet werden, um kryptografische Schlüssel mithilfe von HSMs zu generieren, die nach FIPS 140-2 Level 2 geprüft sind.

Azure Key Vault-Container können kryptografische Schlüssel sicher und mit hoher Verfügbarkeit speichern. Dies kann auch Implementierungen von Sicherheitskontrollen für Folgendes umfassen:

-   Schutz von Authentifikatoren [NIST SP 800-53, Kontrollmaßnahme IA-5 (7)]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme IA-5 (7): *Die Verwendung von unverschlüsselten, statischen Authentifikatoren, die in Anwendungen, Zugriffsskripts oder Funktionstasten eingebettet sind, ist ausdrücklich verboten. Skripts oder Anwendungen, die einen Authentifikator verwendet, rufen vor jeder Verwendung einen Azure Key Vault-Container auf. Der Zugriff auf Azure Key Vault-Container wird überwacht. Damit können Verstöße gegen dieses Verbot erkannt werden, wenn für den Zugriff auf ein System ein Dienstkonto ohne einen zugehörigen Aufruf an den Azure Key Vault-Container verwendet wird.*

- Einrichtung und Verwaltung kryptografischer Schlüssel [NIST SP 800-53, Kontrollmaßnahme SC-12 (1)]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme SC-12 (1): *Azure Key Vault wird zum Speichern von kryptografischen Schlüsseln und Geheimnissen verwendet. Dieser Dienst verfolgt und überwacht den Zugriff auf Geheimnisse. Dieser Dienst stellt sicher, dass Geheimnisse nicht verloren gehen.*

Azure Key Vault kann verwendet werden, um kryptografische Schlüssel mithilfe von HSMs zu erstellen, die nach FIPS 140-2 Level 2 geprüft sind. Konfigurationen für Azure Active Directory-Gruppenrichtlinien können Implementierungen von Sicherheitskontrollen für Folgendes umfassen:

- Einrichtung und Verwaltung kryptografischer Schlüssel [NIST SP 800-53, Kontrollmaßnahme SC-12 (2)]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme SC-12 (2): *Azure Key Vault wird zum Erstellen, Kontrollieren und Verteilen von kryptografischen Schlüsseln verwendet. Diese Schlüssel werden mithilfe von HSMs generiert, die nach FIPS 140-2 Level 2 geprüft sind. Diese Schlüssel werden in sicher verschlüsselten Containern in Azure Key Vault gespeichert und verwaltet.*

*Weitere Tools und Ressourcen*

Dokumentationen, Tools, Vorlagen und andere Ressourcen stehen als Hilfestellung für die sichere Bereitstellung von Azure-Diensten und -Features zur Verfügung. Erste Schritte mit Azure Key Vault finden Sie unter [Microsoft Azure Docs](https://docs.microsoft.com/azure/key-vault/).

### <a name="operations-management-suite"></a>Operations Management Suite

Microsoft Operations Management Suite (OMS) ist die cloudbasierte IT-Verwaltungslösung von Microsoft, die Ihnen das Verwalten und Schützen Ihrer lokalen und cloudbasierten Infrastruktur erleichtert. Durch Sicherheit und Compliance können Sie Sicherheitsrisiken für Ihre Infrastruktur identifizieren, bewerten und minimieren. Diese Features von OMS werden über mehrere Lösungen implementiert, einschließlich Log Analytics, die Protokolldaten analysieren und Sicherheitskonfigurationen überwachen.

Sicherheits- und Überwachungsdienste mit OMS bieten Unterstützung bei der Implementierung von Kontoüberwachung und -protokollierung. Dies kann auch Implementierungen von Sicherheitskontrollen für Folgendes umfassen:

- Kontenverwaltung [NIST SP 800-53, Kontrollmaßnahme AC-2]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AC-2.g: *Microsoft Operation Management Suite (OMS) wird zum Überwachen der Verwendung von Systemkonten verwendet. OMS erstellt Überwachungsprotokolle für Systemkonten, die mit den Analysefunktionen von OMS analysiert werden können und Warnungen ausgeben, die auf einem definierten Satz von Kriterien basieren.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AC-2 (4): *Microsoft Operation Management Suite (OMS) wird verwendet, um Überwachungsprotokolle für die Kontenverwaltungsfunktionen wie das Erstellen, Ändern, Aktivieren, Deaktivieren und Entfernen von Konten zu generieren. Mit OMS wird der Besitzer des Systems gewarnt, wenn eines der oben genannten Bedingungen erfüllt wurde.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AC-2 (12): *Microsoft Operation Management Suite (OMS) wird zum Überwachen bei untypischer Verwendung von Systemkonten verwendet. OMS erstellt Überwachungsprotokolle für Systemkonten, die mit den Analysefunktionen von OMS analysiert werden können und Warnungen an die entsprechenden Mitarbeiter ausgeben, die auf einem definierten Satz von Kriterien basieren.*

- Geringste Berechtigungen [NIST SP 800-53, Kontrollmaßnahme AC-6]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AC-6 (9): *Microsoft Operation Management Suite (OMS) wird zum Überwachen der Ausführung von privilegierten Funktionen verwendet. OMS erstellt Überwachungsprotokolle für eine Liste angegebener Funktionen, die mit den Analysefunktionen von OMS analysiert werden können und Warnungen ausgeben, die auf einem definierten Satz von Kriterien basieren.*

 - Remotezugriff [NIST SP 800-53, Kontrollmaßnahme AC-17]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AC-17 (1): *Microsoft Operation Management Suite (OMS) wird zum Überwachen und Steuern des Remotezugriffs verwendet. OMS umfasst Sicherheits-, Aktivitäts- und Überwachungsberichte für Ihr Verzeichnis.*

- Überwachungsereignisse [NIST SP 800-53, Kontrollmaßnahme AU-2]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AU-2: *Microsoft Operation Management Suite (OMS) wird verwendet, um Folgendes zu überwachen: erfolgreiche und nicht erfolgreiche Anmeldeversuche beim Konto, Verwaltungsereignisse für das Konto, Objektzugriffe, Richtlinienänderungen, Ausführung privilegierter Funktionen, Nachverfolgung von Prozessen und Systemereignisse. Für Webanwendungen: alle Administratoraktivitäten, Authentifizierungsprüfungen, Autorisierungsprüfungen, Datenlöschungen, Datenzugriffe, Datenänderungen und Berechtigungsänderungen. Darüber hinaus auch für alle anderen vom Kunden definierten Ereignisse.*

- Inhalt von Überwachungsdatensätzen [NIST SP 800-53, Kontrollmaßnahme AU-3]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AU-3 (2): *Microsoft Operation Management Suite (OMS) dient der zentralen Verwaltung und Konfiguration aller Überwachungsdatensätze von Netzwerk-, Speicher- und Computinggeräten.*

- Überwachungsspeicherkapazität [NIST SP 800-53, Kontrollmaßnahme AU-4]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AU-4: *Microsoft Operation Management Suite (OMS) dient der zentralen Verwaltung und Konfiguration aller Überwachungsdatensätze von Netzwerk-, Speicher- und Computinggeräten. Das zentrale Verwaltungstool dient zum Konfigurieren der Speicherkapazität für Überwachungsdatensätze.*

- Reaktion auf Fehler beim Verarbeiten der Überwachung [NIST SP 800-53, Kontrollmaßnahme AU-5]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AU-5: *Microsoft Operation Management Suite (OMS) wird zusammen mit der Sicherheits- und Überwachungslösung verwendet, um die in der Organisation festgelegten Mitarbeiter über Verarbeitungsfehler bei der Überwachung zu warnen.*

  > Angabe für die Implementierung der Kontrollmaßnahme AU-5 (1): *Microsoft Operation Management Suite (OMS) wird zusammen mit der Sicherheits- und Überwachungslösung verwendet, um die in der Organisation festgelegten Mitarbeiter über einen in der Organisation definierten Zeitraum zu warnen, wenn der Speicher für Überwachungsdatensätze einen in der Organisation festgelegten Prozentsatz des maximalen Repositoryvolumens erreicht hat.*

  > Angabe für die Implementierung der Kontrollmaßnahme AU-5 (2): *Microsoft Operation Management Suite (OMS) wird zusammen mit der Sicherheits- und Überwachungslösung verwendet, um die in der Organisation festgelegten Mitarbeiter in Echtzeit zu warnen, wenn bei den laut Organisationsrichtlinien zu überwachenden Ereignissen Fehler auftreten.*

- Prüfung der Überwachung, Analyse und Berichterstellung [NIST SP 800-53, Kontrollmaßnahme AU-6]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AU-6 (3): *Microsoft Operation Management Suite (OMS) wird zusammen mit der Sicherheits- und Überwachungslösung und der Log Analytics-Lösung verwendet, um verschiedene Repositorys mit Überwachungsprotokollen zu analysieren, um die Informationen in der gesamten Organisation zugänglich zu machen. Das Analysetool wird verwendet, um Berichte zu Überwachungsprotokollen zu erstellen, die über die aktuelle Situation informieren.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AU-6 (4): *Microsoft Operation Management Suite (OMS) dient der zentralen Verwaltung und Konfiguration aller Überwachungsdatensätze. Das zentrale Verwaltungstool ermöglicht die Überprüfung und Analyse von Überwachungsdatensätzen aus sämtlichen Quellen.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AU-6 (5): *Microsoft Operation Management Suite (OMS) wird zusammen mit der Sicherheits- und Überwachungslösung und der Log Analytics-Lösung verwendet, um verschiedene sicherheitsrelevante Daten, die bei der Überprüfung auf Sicherheitsrisiken und der Überwachung des Informationssystems generiert wurden, sowie leistungsrelevante Daten zu analysieren. Die Analyse dieser unterschiedlichen Ressourcen verbesserte die Möglichkeiten zur Erkennung verdächtiger Aktivitäten.*

- Überwachungsreduzierung und Berichterstellung [NIST SP 800-53-Kontrollmaßnahme AU-7]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AU-7: *Microsoft Operation Management Suite (OMS) wird zusammen mit der Sicherheits- und Überwachungslösung und der Log Analytics-Lösung verwendet, um bei Bedarf lesbare Berichte zu generieren, die Untersuchungen nach Sicherheitsvorfällen ermöglichen. Die Verwendung von Microsoft Log Analytics führt nicht zu dauerhaften oder nicht mehr umkehrbaren Änderungen am Inhalt oder der zeitlichen Einordnung der ursprünglichen Überwachungsdatensätze.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AU-7 (1): *Microsoft Operation Management Suite (OMS) wird zusammen mit der Sicherheits- und Überwachungslösung verwendet, um die in der Organisation festgelegten Überwachungsereignisse abzufragen.*

- Schutz von Überwachungsinformationen [NIST SP 800-53, Kontrollmaßnahme AU-9]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AU-9: *Microsoft Operation Management Suite (OMS) wird zusammen mit der Sicherheitsgruppen zum Verwalten des Zugriffs auf Überwachungsinformationen und -tools verwendet, um nicht autorisierte Zugriffe, Änderungen und Löschungen von Überwachungsdatensätzen zu verhindern. Die Möglichkeit zum Anzeigen von Überwachungsinformationen und Verwenden von Überwachungstools ist auf Benutzer beschränkt, die über diese Berechtigungen verfügen.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AU-9 (2): *Microsoft Operation Management Suite (OMS) wird zusammen mit der Sicherheits- und Überwachungslösung und Azure Backup verwendet, um die Überwachungsprotokolle in Azure zu sichern. Dort werden die Daten dann repliziert, um die Zuverlässigkeit und Verfügbarkeit der Daten zu gewährleisten. Azure Backup stellt einen sicheren Speicherort für Überwachungsprotokolle dar, die auf einem anderen System als dem überwachten gespeichert werden müssen.* 

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AU-9 (4): *Microsoft Operation Management Suite (OMS) wird zusammen mit der Sicherheitsgruppen zum Verwalten des Zugriffs auf Überwachungsinformationen und -tools verwendet. Nur Mitarbeiter, die Zugriff auf die Verwaltung von Überwachungsfunktionen zwingend benötigen, erhalten diese Berechtigungen.*

- Aufbewahrung von Überwachungsdatensätzen [NIST SP 800-53, Kontrollmaßnahme AU-11]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AU-11: *Microsoft Operation Management Suite (OMS) wird zusammen mit der Sicherheits- und Überwachungslösung verwendet, um die Aufbewahrung der Überwachungsprotokolle zu konfigurieren. Die Aufbewahrung von Überwachungsprotokollen wird mit mindestens 90 Tagen konfiguriert.*

- Erstellungsüberwachung [NIST SP 800-53, Kontrollmaßnahme AU-12]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AU-12: *Microsoft Operation Management Suite (OMS) wird zusammen mit der Sicherheits- und Überwachungslösung verwendet, um die in AU-02 festgelegten Überwachungsereignisse von Komponenten des Informationssystems abzufragen. Die Sicherheits-und Überwachungslösung von OMS wird von den von der Organisation festgelegten Mitarbeitern verwendet, um festzulegen, welche Überwachungsereignisse von bestimmten Geräten gesammelt werden.*

  > Beispielangabe für die Implementierung der Kontrollmaßnahme AU-12 (1): *Microsoft Operation Management Suite (OMS) wird zusammen mit der Sicherheits- und Überwachungslösung und der Log Analytics-Lösung verwendet, um Überwachungsdatensätze von Systemkomponenten zu kompilieren. Diese können nach Zeitstempeln sortiert werden, um einen systemweiten Audit-Trail zu erstellen.*

- Zugriffseinschränkungen für Änderungen [NIST SP 800-53, Kontrollmaßnahme CM-5]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme CM-5 (1): *Microsoft Operation Management Suite (OMS) wird zusammen mit der Sicherheits- und Überwachungslösung und Active Directory verwendet, um Beschränkungen für den logischen Zugriff über die Mitgliedschaft in Sicherheitsgruppen zu erzwingen. Aktionen von diesen Sicherheitsgruppen werden von OMS überwacht.*

- Komponentenbestand des Informationssystems [NIST SP 800-53, Kontrollmaßnahme CM-8]

  > Beispielangabe für die Implementierung der Kontrollmaßnahme CM-8 (3): *Microsoft Operation Management Suite (OMS) wird zusammen mit der Sicherheits- und Überwachungslösung und der Antischadsoftwarelösung verwendet, um nicht autorisierte Software zu erkennen. Bei einer Erkennung deaktiviert OMS die infizierte Komponente und sendet eine Warnung an die für die Organisation festgelegten Mitarbeiter.*

Dokumentationen, Tools, Vorlagen und andere Ressourcen stehen als Hilfestellung für die sichere Bereitstellung von Azure-Diensten und -Features zur Verfügung. Erste Schritte mit Operation Management Suite finden Sie unter [Microsoft Azure Docs](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview/).

### <a name="additional-implementation-guidance"></a>Zusätzlicher Implementierungsleitfaden

Zusätzlich zu den oben beschriebenen Azure-Kerndiensten unterstützen verschiedene integrierte Features Organisationen dabei, die Complianceanforderungen zur Sicherheit zu erfüllen. Alle Azure-Ressourcen werden in Ressourcengruppen organisiert. Assets können weitergehend organisiert werden, indem ihnen Tags zugewiesen werden. Dies sind Schlüssel-Wert-Paare, die vom Kunden ausgewählt werden können, um Ressourcen nach Kategorie oder einer anderen Eigenschaft zu identifizieren. Azure-Ressourcengruppen stellen eine vollständige Identifikation und Überwachung aller Kundenressourcen, die in Azure bereitgestellt werden, sicher [NIST SP 800-53, Kontrollmaßnahme CM-8]. 

> Beispielangabe für die Implementierung der Kontrollmaßnahme CM-8: *Alle intern bereitgestellten, kundenkontrollierten Ressourcen gehören zur Ressourcengruppe „Kundenressourcen“. In Azure-Portal werden alle bereitgestellten Assets auf dem Blatt der Ressourcengruppe identifiziert, um eine präzise und aktuelle Inventur bereitzustellen. Tags werden bei Bedarf auf Ressourcen angewendet, um Anforderungen für eine Nachverfolgung oder Berichterstellung zu erfüllen.*

Azure Resource Manager-Vorlagen ermöglichen Kunden die Festlegung der Konfiguration von Ressourcen in ihrer Azure-Bereitstellung. Resource Manager-Vorlagen basieren auf JSON und verwenden deklarative Syntax für das Dokumentieren von Ressourcen sowie für Konfigurationsparameter und andere Aspekte der Azure-Umgebung. Resource Manager-Vorlagen werden automatisch generiert, wenn Azure-Lösungen im Azure-Portal bereitgestellt werden. Sie können aber auch manuell erstellt und bearbeitet werden, wenn bestimmte Kundenanforderungen erfüllt werden müssen. Diese Resource Manager-Vorlagen können als Darstellung der Baselinekonfiguration für ein Informationssystem dienen [NIST SP 800-53, Kontrollmaßnahme CM-2]. Vorhandene Resource Manager-Vorlagen, z.B. die Bausteine, auf die oben verwiesen wird, können an die Anforderungen jeder Bereitstellung angepasst werden.

> Beispielangabe für die Implementierung der Kontrollmaßnahme CM-2: *Als eine Komponente der Baselinekonfiguration des Informationssystems werden Azure Resource Manager-Vorlagen mit Konfigurationskontrolle verwaltet, die die bereitgestellten, kundenkontrollierten Ressourcen und die Konfiguration der Komponenten des Informationssystems in Azure darstellen. Diese Vorlagen erfassen bereitgestellte Ressourcen, einschließlich virtueller Computer, Speicher- und Netzwerkressourcen (auch die Konfigurationen von Netzwerkressourcen), die den Netzwerk-Datenverkehr steuern, der in die Azure-Umgebung des Kunden eingeht, aus dieser ausgeht und innerhalb der Umgebung weitergeleitet wird.*

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie Fragen im Zusammenhang mit Prozessen von Azure Blueprint, FedRAMP, DoD oder Agency ATO oder zur Compliance haben oder wenn Sie Feedback zu Azure Blueprint haben, schreiben Sie eine E-Mail an [AzureBlueprint@microsoft.com](mailto:AzureBlueprint@microsoft.com).

[Microsoft Trust Center](https://www.microsoft.com/trustcenter/Compliance/default.aspx)

[Microsoft Azure Government-Blog](https://blogs.msdn.microsoft.com/azuregov/)



<!--HONumber=Jan17_HO4-->


