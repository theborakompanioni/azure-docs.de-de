---
title: "US-Verteidigungsministerium in Azure Government – Übersicht | Microsoft-Dokumentation"
description: "Dies bietet einen Vergleich der Features und Richtlinien zum Entwickeln von Anwendungen für Azure Government"
services: azure-government
cloud: gov
documentationcenter: 
author: ryansoc
manager: zakramer
ms.assetid: cba97199-851d-43ae-a75a-c601f3f81601
ms.service: azure-government
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 01/12/2017
ms.author: ryansoc
translationtype: Human Translation
ms.sourcegitcommit: 71e8742d71ce6ebab08988751a886df4df000f8a
ms.openlocfilehash: 4e815c5fce563155f7a747262a3695a5180706e3


---
# <a name="department-of-defense-dod-in-azure-government"></a>US-Verteidigungsministerium (DoD) in Azure Government
## <a name="overview"></a>Übersicht
Azure Government wird von Instanzen des US-Verteidigungsministeriums (Department of Defense, DoD) verwendet, um eine Vielzahl von Workloads und Lösungen bereitzustellen, einschließlich der Workloads, die vom Handbuch für Sicherheitsanforderungen für Cloudcomputing des US-Verteidigungsministeriums, <a href="http://iasecontent.disa.mil/cloud/SRG/index.html">The DoD Cloud Computing Security Requirements Guide, Version 1, Release 2</a>, auf Impact Level 4 (L4) und Impact Level 5 (L5) abgedeckt werden.

Azure Government ist der erste und einzige hyperskalierbare kommerzielle Clouddienst, dem von der Behörde für Informationssysteme des US-Verteidigungsministeriums (Defense Information Systems Agency) eine „Information Impact Level 5 DoD Provisional Authorization“ erteilt wurde. Darüber hinaus sind Azure Government-Regionen, die speziell auf Kundenworkloads des US-Verteidigungsministeriums ausgelegt sind, jetzt allgemein verfügbar.

Einer der zentralen Faktoren für das DoD beim Wechsel in die Cloud ist es, Organisationen zu erlauben, sich auf ihre Aufgaben zu konzentrieren, und Ablenkungen durch das Erstellen und Verwalten von internen IT-Lösungen zu minimieren.

Auf Azure Government basierende Cloud-Architekturen erlauben es den Mitarbeitern des US-Verteidigungsministeriums, sich auf Ziele und die Verwaltung von IT-Commodity-Diensten, wie z.B. SharePoint, und andere Anwendungsworkloads zu konzentrieren.  Dies ermöglicht es, wichtige IT-Ressourcen neu auszurichten, um den Fokus auf Anwendungsentwicklung, Analysen und Cybersicherheit zu richten.

Die Elastizität und Flexibilität von Azure bietet enorme Vorteile für die Kunden im US-Verteidigungsministerium. Es ist einfacher, schneller und kosteneffizienter, eine Workload zentral in der Cloud hochzuskalieren, als traditionelle Beschaffungsprozesse für Hardware und Dienste zu durchlaufen, wenn Sie lokal oder in Rechenzentren des US-Verteidigungsministeriums arbeiten. Beispielsweise kann die Anschaffung neuer, aus mehreren Servern bestehender Hardware selbst für eine Testumgebung mehrere Monate dauern und es müssen signifikante Ausgaben genehmigt werden. Im Gegensatz dazu kann durch die Verwendung von Azure eine Testmigration für eine vorhandene Workload innerhalb von Wochen oder sogar Tagen kostensparend konfiguriert werden (wenn der Test abgeschlossen ist, kann die Umgebung ohne laufende Kosten entfernt werden).

Diese Flexibilität ist von Bedeutung. Wenn Kunden im US-Verteidigungsministerium zu Azure wechseln, sparen sie nicht nur Geld – die Cloud bietet neue Möglichkeiten. Es ist z.B. einfach, eine Testumgebung zu erstellen, um Einblicke in neue Technologien zu erhalten. Sie können eine Anwendung migrieren und sie in Azure testen, bevor Sie ein Commit in einer Produktionsbereitstellung in der Cloud ausführen. Für Aufgaben zuständige Personen können kosteneffizientere Optionen leichter und ohne Risiko erkunden.

Sicherheit ist ein anderer wichtiger Bereich, und obwohl jede Cloud-Bereitstellung eine genaue Planung voraussetzt, um eine sichere und zuverlässige Dienstbereitstellung sicherzustellen, sind in der Realität die meisten ordnungsgemäß konfigurierten, cloudbasierten Workloads (bis zu und einschließlich L4-Workloads) in Azure Government sicherer als viele herkömmliche Bereitstellungen an DoD-Speicherorten und Rechenzentren. Der Grund dafür ist, dass Verteidigungsbehörden über die Erfahrung und das Fachwissen verfügen, um alle Assets physisch zu schützten. Jedoch stellen die IT-Oberflächen andere Herausforderungen dar. Cybersicherheit ist ein sich schnell verändernder Bereich und erfordert fachliche Kompetenzen und die Fähigkeit, Gegenmaßnahmen schnell zu entwickeln und bereitzustellen, falls nötig. Die Azure-Plattform – egal ob kommerziell oder Government – unterstützt nun hunderttausende Kunden und diese Dimension ermöglicht es Microsoft, sich entwickelnde Angriffsvektoren schnell zu erkennen und anschließend die Ressourcen in die rasche Entwicklung und Implementierung der geeigneten Verteidigungsstrategien zu leiten.

## <a name="dod-region-qa"></a>Fragen und Antworten zu Regionen des US-Verteidigungsministeriums

### <a name="what-are-the-azure-government-dod-regions"></a>Was sind die DoD-Regionen in Azure Government? 
Die Regionen „US DoD, Osten“ und „US DoD, Mitte“ sind physisch getrennte Regionen von Microsoft Azure, die für die Sicherheitsanforderungen des US-Verteidigungsministeriums (US Department of Defense, DoD) konzipiert wurden und hier speziell auf Daten ausgelegt sind, die gemäß dem Handbuch für Sicherheitsanforderungen für Cloudcomputing des US-Verteidigungsministeriums (DoD Cloud Computing Security Requirements Guide, SRG) auf DoD Impact Level 5 eingestuft sind.   

### <a name="what-is-the-difference-between-azure-government-and-the-azure-government-dod-regions"></a>Worin unterscheiden sich Azure Government und die Azure Government-DoD-Regionen? 
Azure Government ist eine Communitycloud für die US-amerikanische Regierung, die Dienste auf Bundes-, Bundesstaats- und Kommunalebene für Kunden und Entitäten bereitstellt, die den ITAR (International Traffic in Arms Regulations, Regelungen des internationalen Waffenhandels) unterliegen. Des Weiteren werden Dienste für Lösungsanbieter bereitgestellt, die im Auftrag dieser Kunden und Entitäten arbeiten. Alle Azure Government-Regionen sind auf die Sicherheitsanforderungen für Daten gemäß DoD Impact Level 5 und auf die FedRAMP-Standards „High“ ausgelegt und werden nach diesen Anforderungen und Standards betrieben.

Die Azure Government-DoD-Regionen unterstützen die Anforderungen an die physische Trennung von Impact Level 5-Daten, indem sie dedizierte Compute- und Speicherinfrastrukturen bereitstellen, die ausschließlich von DoD-Kunden verwendet werden.  

#### <a name="what-is-the-difference-between-impact-level-4-and-impact-level-5-data"></a>Worin unterscheiden sich Daten auf Impact Level 4 und Impact Level 5?  
Impact Level 4-Daten sind kontrollierte, nicht klassifizierte Informationen (Controlled Unclassified Information, CUI). Hierzu gehören Daten, die möglicherweise der Exportkontrolle unterliegen, per besonderem Datenschutz geschützte Gesundheitsinformationen sowie weitere Daten, die eine explizite CUI-Kennzeichnung erfordern (z.B. For Official Use Only, Law Enforcement Sensitive, Sensitive Security Information).

Impact Level 5-Daten umfassen kontrollierte, nicht klassifizierte Informationen (Controlled Unclassified Information, CUI), die ein höheres Maß an Schutz erfordern als vom Informationsbesitzer, per Gesetz oder durch behördliche Anordnung für notwendig erachtet.  Zu Impact Level 5-Daten gehören auch nicht klassifizierte nationale Sicherheitssysteme (National Security Systems, NSSs).  Weitere Informationen zu den Auswirkungsstufen (Impact Levels) sowie ihre charakteristischen Anforderungen und Merkmale stehen in Abschnitt 3 des Handbuchs für Sicherheitsanforderungen für Cloudcomputing des US-Verteidigungsministeriums zur Verfügung.  

### <a name="what-data-is-categorized-as-impact-level-5"></a>Welche Daten sind als Impact Level 5 kategorisiert? 
Level 5 gilt für kontrollierte, nicht klassifizierte Informationen (Controlled Unclassified Information, CUI), die ein höheres Maß an Schutz erfordern als auf Level 4 gewährleistet oder vom Informationsbesitzer, per Gesetz oder durch anderweitige behördliche Anordnung für notwendig erachtet. Level 5 unterstützt auch nicht klassifizierte nationale Sicherheitssysteme (National Security Systems, NSSs).  Dieses Level gilt für NSS- und CUI-Informationskategorisierungen basierend auf CNSSI-1253 bis zu einer mittleren Vertraulichkeits- und Integritätsstufe (M-M-x).

### <a name="what-is-microsoft-doing-differently-to-support-impact-level-5-data"></a>Was macht Microsoft anders, um Impact Level 5-Daten zu unterstützen? 
Impact Level 5-Daten können per definitionem nur in einer dedizierten Infrastruktur verarbeitet werden, die eine physische Trennung der DoD-Kunden von Mandanten sicherstellt, die nicht zur US-Regierung gehören.  Mit den Regionen „US DoD, Osten“ und „US DoD, Mitte“ stellt Microsoft einen exklusiven Dienst für DoD-Kunden bereit, der die vom DoD vorgegebenen Anforderungen übertrifft und mehr Datenschutz und Funktionalität bietet als jede andere hyperskalierbare kommerzielle Cloudlösung.

### <a name="do-these-regions-support-classified-data-requirements"></a>Unterstützen diese Regionen Anforderungen an klassifizierte Daten? 
Diese Azure Government DoD-Regionen unterstützen nur nicht klassifizierte Daten bis einschließlich Impact Level 5.  Impact Level 6-Daten sind als klassifizierte Daten bis hin zu geheimen Daten definiert.

### <a name="what-organizations-in-the-dod-can-use-the-azure-government-dod-regions"></a>Welche Organisationen im US-Verteidigungsministerium können die Azure Government DoD-Regionen verwenden? 
Die Regionen „US DoD, Osten“ und „US DoD, Mitte“ wurden zur Unterstützung des Kundenstamms des US-Verteidigungsministeriums erstellt.  Dies umfasst:
* The Office of the Secretary of Defense
* The Joint Chiefs of Staff
* The Joint Staff
* The Defense Agencies
* Department of Defense Field Activities
* The Department of the Army
* The Department of the Navy (einschließlich des United States Marine Corps)
* The Department of the Air Force
* The United States Coast Guard
* The Unified Combatant Commands
* Weitere Büros, Behörden, Aktivitäten und Kommandos unter der Kontrolle oder Dienstaufsicht einer der oben aufgeführten zugelassenen Entitäten.

### <a name="are-the-dod-regions-more-secure"></a>Sind die DoD-Regionen sicherer? 
Microsoft betreibt alle Azure-Rechenzentren und die unterstützende Infrastruktur gemäß den lokalen und internationalen Standards für Sicherheit und Compliance und ist hinsichtlich der Investitionen und Erfolge führender Anbieter auf dem Markt der kommerziellen Cloudplattformen.  Diese neuen DoD-Regionen bieten spezielle Sicherheiten und Zusagen, um die Vorgaben zu erfüllen, die im Handbuch für Sicherheitsanforderungen für Cloudcomputing des US-Verteidigungsministeriums definiert sind.

### <a name="why-are-there-multiple-dod-regions"></a>Warum gibt es mehrere DoD-Regionen? 
Durch Bereitstellung mehrerer Regionen bietet Microsoft Kunden die Möglichkeit, ihre Lösung für Notfallszenarien über mehrere Regionen hinweg zu erstellen. So können die Kunden Geschäftskontinuität sicherstellen und Anforderungen an die Zulassung von Systemen erfüllen.  Darüber hinaus können Kunden die Leistung optimieren, indem sie Lösungen in der Region bereitstellen, die sich in größter geografischer Nähe zu ihrem physischen Standort befindet.

### <a name="are-these-dod-regions-connected-to-the-niprnet"></a>Sind diese DoD-Regionen mit dem NIPRNet verbunden? 
Das US-Verteidigungsministerium gibt vor, dass bei kommerziellen Clouddiensten, die für CUI verwendet werden, die Verbindung mit den Kunden über einen Cloudzugriffspunkt (Cloud Access Point, CAP) erfolgen muss.  Aus diesem Grund werden die Azure DoD-Regionen über redundante Verbindungen zu mehreren geografisch verteilten CAPs mit dem NIPRNet verbunden.  Ein DoD-CAP ist ein System von Geräten für den Schutz und die Überwachung von Netzwerkgrenzen, die das Netzwerk und die Dienste der DoD-Informationssysteme schützen.

### <a name="what-does-general-availability-mean"></a>Was bedeutet allgemeine Verfügbarkeit? 
Allgemeine Verfügbarkeit bedeutet, dass die DoD-Regionen in Azure Government zur Unterstützung von Produktionsworkloads verwendet werden können und dass finanziell abgesicherte SLAs für alle in den Regionen bereitgestellten allgemein verfügbaren Dienste unterstützt werden.

### <a name="how-does-a-dod-customer-acquire-azure-government-dod-services"></a>Wie kann ein DoD-Kunde Azure Government DoD-Dienste beziehen? 
Azure Government DoD-Dienste können von qualifizierten Entitäten über die gleichen Handelspartnerkanäle wie Azure Government-Dienste erworben werden.  Gemäß der Verpflichtung von Microsoft, die Planung und Kostenschätzung beim Erwerb von Clouddiensten so einfach wie möglich zu gestalten, werden die Preise für Azure Government DoD-Regionen zum Zeitpunkt der allgemeinen Verfügbarkeit in den Azure-Preisrechner integriert.  Die Azure Government DoD-Dienste können jederzeit bedarfsgesteuert zentral hoch- oder herunterskaliert werden, und Sie bezahlen nur Ihre tatsächliche Nutzung.
Für Enterprise Agreement-Kunden, die Azure Government bereits nutzen, sind keinerlei vertragliche Änderungen erforderlich.  

### <a name="how-are-the-dod-regions-priced"></a>Wie sind die Preise für die DoD-Regionen gestaltet? 
Die Preise basieren auf den DoD-Regionen.  Das bedeutet, dass die Dienstkosten für überprüfte DoD-Kunden auf der Azure Government-Region basieren, in der Sie Ihre Workloads ausführen.  Detailliertere Preisinformationen erhalten Sie von Ihrem Microsoft-Kundenbetreuer.  Die Preise für die DoD-Regionen werden zu einem späteren Zeitpunkt über den Preisrechner auf Azure.com bereitgestellt.

### <a name="how-does-a-dod-organization-get-validated-for-the-azure-government-dod-regions"></a>Wie wird eine DoD-Organisation für die Azure Government DoD-Regionen überprüft? 
Um Zugriff auf die Azure DoD-Regionen zu erhalten, müssen Kunden einen Prozess zur Vorabqualifizierung durchlaufen, um ihre Organisation und die beabsichtigte Nutzung der Azure DoD-Umgebung überprüfen zu lassen.  Nach erfolgreichem Abschluss der Vorabqualifizierung stellt Microsoft der antragstellenden Organisation weitere Anweisungen bereit, um ein Abonnement zu erstellen, auf die Umgebung zuzugreifen und anderen Mitgliedern der Organisation rollenbasierte Zugriffssteuerung zu gewähren.

### <a name="can-independent-software-vendors-and-solution-providers-building-on-azure-deploy-solutions-in-the-azure-government-dod-regions"></a>Können unabhängige Softwarehersteller und Lösungsanbieter, die Azure nutzen, Lösungen in den Azure Government DoD-Regionen bereitstellen? 
Lösungsanbieter mit auf Azure aufgebauten Clouddienstangeboten können reine DoD-Lösungen für einen oder mehrere Mandanten in den Azure Government DoD-Regionen betreiben.  Diese Anbieter müssen anhand eines Vertrags mit einer zugelassenen DoD-Entität ihre Berechtigung nachweisen oder ein Sponsorenschreiben von einer zugelassenen DoD-Entität vorlegen.  Anbieter mit Diensten in den Azure Government-DoD-Regionen müssen Maßnahmen für die Verteidigung von Computernetzwerken, Systeme für die Berichterstellung bei Incidents sowie überprüfte Mitarbeiter für den Betrieb von Lösungen zur Verarbeitung von Impact Level 5-Daten in ihr Angebot aufnehmen.  Weitere Informationen für Lösungsanbieter finden sich im Handbuch für Sicherheitsanforderungen für Cloudcomputing des US-Verteidigungsministeriums.

### <a name="will-office-365-or-microsoft-dynamics-365-be-a-part-of-this-offering"></a>Werden Office 365 oder Microsoft Dynamics 365 zu diesem Angebot gehören? 
Im Rahmen dieses Angebots stellt Microsoft Office 365-Dienste für das US-Verteidigungsministerium auf Impact Level 5 bereit.  Dynamics 365 plant, zu einem späteren Zeitpunkt Impact Level 5-Dienste in den Azure DoD-Regionen bereitzustellen.

### <a name="how-do-i-connect-to-the-dod-regions-once-i-have-a-subscription"></a>Wie stelle ich eine Verbindung mit den DoD-Regionen her, sobald ich über ein Abonnement verfüge? 
Die DoD-Regionen für Azure Government stehen über das Azure Government-Verwaltungsportal zur Verfügung.  DoD-Kunden, die für die Nutzung zugelassen sind, werden die Regionen als verfügbare Optionen angezeigt, wenn diese verfügbare Dienste bereitstellen.  Allgemeine Hinweise zum Verwalten Ihrer Azure Government-Abonnements finden Sie in unserer Dokumentation.

### <a name="what-services-are-part-of-your-impact-level-5-accreditation-scope"></a>Welche Dienste gehören zum Umfang Ihrer Impact Level 5-Zulassung? 
Azure ist ein Angebot, das ständig aktualisiert wird und dem wöchentlich neue Dienste und Funktionen hinzugefügt werden – die Anzahl der angebotenen Dienste wird regelmäßig erweitert.  Aktuelle Informationen finden Sie in unserem<a href="https://www.microsoft.com/en-us/TrustCenter/Compliance/DISA"> Microsoft Trust Center.


## <a name="next-steps"></a>Nächste Schritte:
<a href="https://www.microsoft.com/en-us/TrustCenter/Compliance/DISA">Microsoft Trust Center – Webseite des US-Verteidigungsministeriums</a>

<a href="http://iasecontent.disa.mil/cloud/SRG/index.html"> The DoD Cloud Computing Security Requirements Guide, Version 1, Release 2</a> (Handbuch für Sicherheitsanforderungen für Cloudcomputing des US-Verteidigungsministeriums, Version 1, Ausgabe 2)

<a href="https://azure.microsoft.com/en-us/offers/azure-government/"> Azure Government-Handelspartnerkanäle

<a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government-Blog </a>




<!--HONumber=Jan17_HO3-->


