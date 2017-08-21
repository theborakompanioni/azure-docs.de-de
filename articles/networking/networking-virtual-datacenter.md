---
title: Virtuelle Microsoft Azure-Rechenzentren | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie Ihr virtuelles Rechenzentrum in Azure erstellen.
services: networking
author: tracsman
manager: rossort
tags: azure-resource-manager
ms.service: virtual-network
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: jonor
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: fd656c68b5c3b6858b0aa04c51bdd28f3f0adc24
ms.contentlocale: de-de
ms.lasthandoff: 07/10/2017

---

# <a name="microsoft-azure-virtual-data-center"></a>Virtuelle Microsoft Azure-Rechenzentren
**Microsoft Azure**: Schneller agieren, Geld sparen, lokale Apps und Daten integrieren

## <a name="overview"></a>Übersicht
Durch das Migrieren lokaler Anwendungen in Azure können Organisationen ohne größere Änderungen („Lift and Shift“) eine sichere und kosteneffiziente Infrastruktur bereitstellen. Um die mit Cloud Computing mögliche Agilität und die Azure-Funktionen bestmöglich auszunutzen, sollten Unternehmen ihre Architekturen jedoch weiterentwickeln. Microsoft Azure bietet hyperskalierbare Dienste und Infrastrukturen, Zuverlässigkeit und Funktionen für Unternehmen und vielfältige Auswahlmöglichkeiten für Hybridkonnektivität. Kunden können entweder über das Internet oder mit Azure ExpressRoute auf diese Clouddienste zugreifen. Letzteres stellt private Netzwerkkonnektivität bereit. Microsoft Azure Platform ermöglicht Kunden eine nahtlose Erweiterung ihrer Infrastruktur auf die Cloud und den Aufbau mehrstufiger Architekturen. Darüber hinaus bieten Microsoft-Partner erweiterte Funktionen in Form von Sicherheitsdiensten und virtuellen Geräten an, die für die Verwendung in Azure optimiert wurden.

Dieser Artikel bietet einen Überblick über die Muster und Entwürfe, mit denen Kunden die architektonischen Herausforderungen hinsichtlich der Skalierung, Leistung und Sicherheit meistern können, die bei einer Migration in die Cloud auf sie zukommen. Außerdem wird mit einem Schwerpunkt auf Sicherheitsanforderungen und Kostenoptimierung besprochen, wie verschiedene IT-Organisationsrollen in die Verwaltung und Governance des Systems passen.

## <a name="what-is-a-virtual-data-center"></a>Was ist ein virtuelles Rechenzentrum?
Cloudlösungen wurden ursprünglich dazu entworfen, einzelne, relativ isolierte Anwendungen im öffentlichen Spektrum zu hosten. Dieser Ansatz hat sich für einige Jahre bewährt. Als jedoch die Vorteile von Cloudlösungen deutlich und mehrere umfangreiche Workloads in der Cloud gehostet wurden, wurden Sicherheit, Zuverlässigkeit, Leistung und der Kostenfaktor von Bereitstellungen in mindestens einer Region zu entscheidenden Aspekten während des Lebenszyklus des Clouddiensts.

Die folgende Abbildung einer Cloudbereitstellung veranschaulicht einige Beispiele für Sicherheitslücken (rotes Kästchen) und für Möglichkeiten, wie virtuelle Netzwerkgeräte in verschiedenen Workloads optimiert werden können (gelbes Kästchen).

[![0]][0]

Die Idee eines virtuellen Rechenzentrums (Virtual Data Center, vDC) entstand aus Notwendigkeit zu skalieren, um Workloads von Unternehmen zu unterstützen, und mit den Problemen umzugehen, die durch die Unterstützung umfangreicher Anwendungen in der öffentlichen Cloud entstanden.

Ein virtuelles Rechenzentrum besteht nicht nur aus den Workloads in der Cloud, sondern auch den Netzwerk-, Sicherheits-, Verwaltungs- und Infrastrukturworkloads (z.B. DNS und Verzeichnisdiensten). Es stellt in der Regel eine private Verbindung zurück zu einem lokalen Netzwerk oder Rechenzentrum bereit. Je mehr Workloads zu Azure verschoben werden, desto wichtiger ist es, die unterstützende Infrastruktur und die Objekte zu bedenken, in denen diese Workloads platziert werden. Überlegungen im Vorfeld zur Strukturierung von Ressourcen können Hunderte von „Workload-Inseln“ vermeiden, die mit getrennt unabhängigen Anforderungen in Bezug auf Datenflüssen, Sicherheitsmodellen und Compliance verwaltet werden müssen.

Ein virtuelles Rechenzentrum ist im Wesentlichen eine Auflistung von separaten, jedoch miteinander verwandte Entitäten mit gemeinsamen genutzten unterstützenden Funktionen, Features und Infrastruktur. Wenn Sie Ihre Workloads als integriertes vDC begreifen, können Sie aufgrund von Degressionsgewinnen, optimierter Sicherheit durch die Zentralisierung von Komponenten und Datenflüssen, einfacheren Operationen, Verwaltung und Complianceprüfungen Kosten sparen.

> [!NOTE]
> Virtuelle Rechenzentren sind jedoch **kein** diskretes Azure-Produkt, sondern eine bestimmte Kombination verschiedener Features und Funktionen, die Ihre Anforderungen erfüllen. Sie stellen ein bestimmtes Verständnis von Workloads und Ihrer Azure-Nutzung dar, mit denen sich Ihre Ressourcen und Möglichkeiten in der Cloud maximieren lassen. Virtuelle Rechenzentren können daher hinsichtlich der Organisationsrollen und Verantwortlichkeiten als ein modularer Ansatz zum Aufbauen von IT-Diensten in Azure verstanden werden.

Sie können Unternehmen dabei helfen, Workloads und Anwendungen für die folgenden Szenarios in Azure zu verschieben:

-   Das Hosten von mehrere verwandten Workloads
-   Migrieren von Workloads aus einer lokalen Umgebung in Azure
-   Implementieren von gemeinsam genutzten oder zentralisierten Sicherheits- und Zugriffsanforderungen in verschiedenen Workloads
-   Ordnungsgemäßes Zusammenlegen von DevOps und der zentralen IT für große Unternehmen

Um virtuelle Rechenzentren voll ausnutzen zu können, ist eine zentrale Topologie (Hub und Spoke) mit verschiedenen Azure-Funktionen notwendig: [Azure VNET][VNet], [NSGs][NSG], [VNET-Peering][VNetPeering], [benutzerdefinierte Routen (UDR)][UDR] und eine Azure-Identität mit der [rollenbasierten Zugriffssteuerung (Role Base Access Control, RBAC)][RBAC].

## <a name="who-needs-a-virtual-data-center"></a>Wer braucht ein virtuelles Rechenzentrum?
Alle Azure-Kunden, die mehr als ein paar Workloads in Azure verschieben müssen, können von gemeinsamen Ressourcen profitieren. Je nach Größe können auch einzelne Anwendungen Vorteile aus den Mustern und den Komponenten ziehen, die zum Erstellen eines vDC verwendet werden.

Falls Ihre Organisation eine zentrale IT-, Netzwerk-, Sicherheits- und/oder Complianceabteilung hat, können virtuelle Rechenzentren dabei helfen, Richtlinien zu erzwingen, Aufgaben zu trennen und die Einheitlichkeit der zugrunde liegenden gemeinsamen Komponenten sicherzustellen. Gleichzeitig haben Anwendungsteams so viel Freiheit und Kontrolle, wie es Ihren Anforderungen entspricht.

Organisationen, die auf DevOps setzen, können mit vDC-Konzepten autorisierte Taschen von Azure-Ressourcen bereitstellen und sicherstellen, dass sie innerhalb dieser Gruppe vollständige Kontrolle haben (Abonnement oder Ressourcengruppe in einem allgemeinen Abonnement). Die Netzwerk- und Sicherheitsbegrenzung bleiben jedoch konform, so wie von einer zentrale Richtlinie in einem Hub-VNET und einer Ressourcengruppe definiert.

## <a name="considerations-on-implementing-a-virtual-data-center"></a>Überlegungen zum Implementieren eines virtuellen Rechenzentrums
Beim Entwerfen eines vDC gibt es mehrere entscheidende Punkte zu berücksichtigen:

-   Identitäts- und Verzeichnisdienste
-   Sicherheitsinfrastruktur
-   Verbindung mit der Cloud
-   Konnektivität in der Cloud

##### <a name="identity-and-directory-service"></a>*Identität- und Verzeichnisdienste*
Identitäts-und Verzeichnisdienste sind ein wichtiger Aspekt von allen Rechenzentren, sowohl lokal als auch in der Cloud. Identität bezieht sich auf alle Aspekte des Zugriffs und der Autorisierung innerhalb des virtuellen Rechenzentrums. Um sicherzustellen, dass nur autorisierte Benutzer und Prozesse auf Ihr Azure-Konto und die entsprechenden Ressourcen zugreifen, verwendet Azure mehrere Typen von Anmeldeinformationen für die Authentifizierung. Dazu gehören Kennwörter (um auf das Azure-Konto zuzugreifen), kryptografische Schlüssel, digitale Signaturen und Zertifikate. [*Microsoft Azure Multi-Factor Authentication* (MFA)][MFA] ist eine zusätzliche Sicherheitsebene für den Zugriff auf Azure-Dienste. Azure MFA stellt über eine Reihe einfacher Überprüfungsoptionen – Telefonanruf, SMS oder Benachrichtigung in einer mobilen App – eine sichere Authentifizierung bereit und ermöglicht es Kunden, die von ihnen bevorzugte Methode zu wählen.

Alle großen Unternehmen müssen einen Identitätsverwaltungsprozess definieren, der die Verwaltung der einzelnen Identitäten, deren Authentifizierung, Autorisierung, Rollen und Berechtigungen innerhalb eines oder in mehreren virtuellen Rechenzentren beschreibt. Mit diesem Prozess sollte Sicherheit und Produktivität gesteigert und gleichzeitig Kosten, Ausfallzeiten und sich wiederholende manuelle Aufgaben verringert werden.

Unternehmen/Organisationen können eine anspruchsvolle Kombination von Diensten für verschiedene Branchen (Line-of-Business, LOB) erfordern, und Mitarbeiter haben häufig unterschiedliche Rollen zugleich, wenn sie an verschiedenen Projekten arbeiten. Ein virtuelles Rechenzentrum erfordert gute Zusammenarbeit zwischen verschiedenen Teams, von denen jedes bestimmte Rollendefinitionen hat, um Systeme verantwortungsbewusst aufzusetzen. Die Matrix aus Zuständigkeiten, Zugriffen und Rechten kann ausgesprochen komplex sein. Identitätsverwaltung in virtuellen Rechenzentren wird über [*Azure Active Directory* (AAD)][AAD] und die rollenbasierten Zugriffssteuerung implementiert.

Ein Verzeichnisdienst ist eine freigegebene Informationstruktur für die Suche, die Verwaltung und die Organisation von alltäglichen Elementen und Netzwerkressourcen. Diese Ressourcen können Volumes, Ordner, Dateien, Drucker, Benutzer, Gruppen, Geräte und andere Objekte enthalten. Jede Ressource im Netzwerk wird vom Verzeichnisserver als Objekt angesehen. Informationen zu einer Ressource werden als eine Auflistung von dieser Ressource oder diesem Objekt zugeordneten Attributen gespeichert.

Alle Microsoft-Onlinedienste für Unternehmen nutzen Azure Active Directory (AAD) für Anmeldungen und andere Identitätsanforderungen. Azure Active Directory ist eine umfassende, hochverfügbare Cloudlösung für die Identitäts- und Zugriffsverwaltung, die grundlegende Verzeichnisdienste, erweiterte Identitätsgovernance und Zugriffsverwaltung für Anwendungen kombiniert. AAD kann in ein lokales Active Directory integriert werden, um das einmalige Anmelden für alle cloudbasierten und lokal gehosteten Anwendungen zu aktivieren. Die Benutzerattribute des lokalen Active Directory können automatisch mit AAD synchronisiert werden.

In einem virtuellen Rechenzentrum muss nicht ein einzelner globaler Administrator alle Berechtigungen zuweisen. Die erforderlichen Berechtigungen zum Verwalten der eigenen Ressourcen innerhalb eines virtuellen Rechenzentrums können stattdessen an die einzelnen Abteilungen (oder Gruppe von Benutzern oder Diensten im Verzeichnisdienst) selbst verteilt werden. Das Strukturieren von Berechtigungen muss gut durchdacht sein. Zu viele Berechtigungen können die Leistungseffizienz beeinträchtigen, und zu wenige oder ungenau definierte Berechtigungen können Sicherheitsrisiken erhöhen. Die rollenbasierte Zugriffssteuerung in Azure (Role-Based Access Control, RBAC) begegnet diesem Problem, indem sie eine präzise Zugriffsverwaltung für die Ressourcen eines virtuellen Rechenzentrums ermöglicht.

##### <a name="security-infrastructure"></a>*Sicherheitsinfrastruktur*
Mit der Sicherheitsinfrastruktur wird in einem virtuellen Rechenzentrum hauptsächlich der Datenverkehr in einem bestimmten virtuellen Netzwerksegment getrennt und ein- und ausgehenden Datenflüsse im gesamten virtuellen Rechenzentrum gesteuert. Die Azure-Architektur enthält mehrere Mandanten. So wird nicht autorisierter oder unbeabsichtigter Datenverkehr zwischen Bereitstellungen mithilfe der Isolation virtueller Netzwerke (VNETs), Zugriffssteuerungslisten (ACLs), Load Balancern, IP-Filter und Richtlinien zum Datenverkehrfluss verhindert. Die Netzwerkadressenübersetzung (Network Address Translation, NAT) trennt den internen vom externen Netzwerkdatenverkehr.

Das Azure-Fabric ordnet Infrastrukturressourcen Mandantenworkloads zu und verwaltet die ein- und ausgehende Kommunikation von virtuellen Computern (Virtual Machines, VMs). Der Azure-Hypervisor erzwingt eine Speicher- und Prozesstrennung zwischen virtuellen Computern und routet den Netzwerkdatenverkehr sicher zu den Gastbetriebssystem-Mandanten.

##### <a name="connectivity-to-the-cloud"></a>*Verbindung mit der Cloud*
Das virtuelle Rechenzentrum benötigt eine Verbindung mit externen Netzwerken, um Kunden, Partnern und/oder internen Benutzern Dienste anzubieten. Damit ist normalerweise nicht nur eine Verbindung mit dem Internet, sondern auch mit lokalen Netzwerke und Rechenzentren gemeint.

Kunden können eigene Sicherheitsrichtlinien erstellen, um zu steuern, auf welche von virtuellen Rechenzentren gehostete Dienste über das Internet zugegriffen werden kann und wie bzw. welche Dienste sich mit dem Internet verbinden. In beiden Fällen werden virtuelle Netzwerkgeräte (mit Filtern und der Überprüfung von Datenverkehr), benutzerdefinierte Routingrichtlinien und Netzwerkfilterung (benutzerdefiniertes Routing und Netzwerksicherheitsgruppen) verwendet.

Unternehmen müssen virtuelle Rechenzentren häufig mit lokalen Rechenzentren oder andere Ressourcen verbinden. Die Konnektivität zwischen Azure und lokalen Netzwerken ist daher ein entscheidender Aspekt beim Entwerfen einer effektiven Architektur. Unternehmen haben zwei Möglichkeiten, eine Verbindung zwischen einem virtuellen und einem lokalen Rechenzentrum in Azure zu erstellen: eine Übertragung über das Internet und/oder über private direkte Verbindungen.

Eine [**Azure-Standort-zu-Standort-VPN-Verbindung**][VPN] ist ein Verbindungsdienst über das Internet zwischen lokalen Netzwerken und dem virtuellen Rechenzentrum, die über sichere, verschlüsselte Verbindungen (IPSec-/IKE-Tunnel) hergestellt wird. Azure-Standort-zu-Standort-Verbindungen sind flexibel und können schnell erstellt werden. Außerdem sind keine weiteren Bereitstellungen erforderlich, da alle Verbindungen über das Internet hergestellt werden.

[**ExpressRoute**][ExR] ist ein Azure-Konnektivitätsdienst, mit dem sich private Verbindungen zwischen virtuellen Rechenzentren und lokalen Netzwerken erstellen lassen. ExpressRoute-Verbindungen erfolgen nicht über das öffentliche Internet und bieten eine höhere Sicherheit, größere Zuverlässigkeit und schnellere Geschwindigkeit (bis zu 10 Gbps) bei konstanter Latenz. ExpressRoute ist sehr nützlich für virtuelle Rechenzentren, da ExpressRoute-Kunden von den Complianceregeln privater Verbindungen profitieren.

Für die Bereitstellung von ExpressRoute-Verbindungen ist ein ExpressRoute-Dienstanbieter erforderlich. Kunden, die schnell die Arbeit aufnehmen müssen, verwenden anfangs üblicherweise Standort-zu-Standort-VPN-Verbindungen, um eine Verbindung zwischen dem virtuellen Rechenzentrum und den lokalen Ressourcen herzustellen, und migrieren anschließend zu einer ExpressRoute-Verbindung.

##### <a name="connectivity-within-the-cloud"></a>*Konnektivität in der Cloud*
[VNETs][VNet] und [VNET-Peering][VNetPeering] sind die grundlegenden Netzwerkkonnektivitätsdienste innerhalb eines virtuellen Rechenzentrums. Ein VNET gewährleistet eine natürliche Isolationslinie zwischen den Ressourcen in einem virtuellen Rechenzentrum. Darüber hinaus ermöglicht das VNet-Peering die Kommunikation zwischen verschiedenen VNETs innerhalb derselben Azure-Region. Bei der Steuerung des Datenverkehrs innerhalb eines VNETs und VNET übergreifend müssen einige Sicherheitsregeln erfüllt werden, die in Zugriffssteuerungslisten ([Netzwerksicherheitsgruppen][NSG]), [virtuelle Netzwerkgerät][NVA] und benutzerdefinierten Routingtabellen ([UDR][UDR]) angegeben sind.

## <a name="virtual-data-center-overview"></a>Übersicht über virtuelle Rechenzentren

### <a name="topology"></a>Topologie
Das Hub- und Spoke-Modell hat das Konzept von virtuellen Rechenzentren innerhalb einer einzelnen Azure-Region erweitert.

[![1]][1]

Der Hub ist der zentrale Bereich, der den eingehenden und/oder ausgehenden Datenverkehr zwischen den verschiedenen Bereichen steuert und überprüft: im Internet, lokal und in den Spokes. Die Hub- und Spoke-Topologie stellt für die IT-Abteilung eine effektive Möglichkeit dar, Sicherheitsrichtlinien an einem zentralen Standort durchzusetzen, während das Risiko von Fehlkonfigurationen und Offenlegungen verringert wird.

Der Hub enthält die allgemeinen Dienstkomponenten, die von den Spokes genutzt werden. Im Folgenden finden Sie einige typische Beispiele für allgemeine zentrale Dienste:

-   Die Windows Active Directory-Infrastruktur (mit den zugehörigen Active Directory-Verbunddiensten), die für die Benutzerauthentifizierung von Drittanbietern erforderlich ist, die über nicht vertrauenswürdige Netzwerke zugreifen, bevor sie Zugriff auf die Workloads im Spoke erhalten
-   Ein DNS-Dienst, mit dem die Namen der Workloads in den Spokes aufgelöst werden, um lokal und über das Internet auf Ressourcen zuzugreifen
-   Eine PKI-Infrastruktur, mit der das einmalige Anmelden in Workloads implementiert wird
-   Flusssteuerung (TCP/UDP) zwischen den Spokes und dem Internet
-   Flusssteuerung zwischen dem Spoke und dem lokalen Standort
-   Falls gewünscht: Flusssteuerung zwischen zwei Spokes

Das virtuelle Rechenzentrum reduziert die Gesamtkosten mithilfe der gemeinsamen Hub-Infrastruktur, die mehrere Spokes umfasst.

Die Rolle eines jeden Spokes kann über verschiedene Arten von Workloads gehostet werden. Die Spokes können auch einen modularen Ansatz für wiederholbare Bereitstellungen desselben Workloads bereitstellen, z.B. für Entwicklung und Tests, Benutzerakzeptanztests, die Präproduktion und die Produktion. Mit den Spokes können auch verschiedene Gruppen in Ihrer Organisation (z.B. DevOps-Gruppen) getrennt und aktiviert werden. Innerhalb eines Spokes ist es möglich, einen einfachen oder komplexe Workloads mit mehreren Ebenen mit einer Steuerung des Datenverkehrs zwischen den Ebenen bereitzustellen.

##### <a name="subscription-limits-and-multiple-hubs"></a>Abonnementlimit und mehrere Hubs
In Azure wird jede Komponente, unabhängig vom Typ, in einem Azure-Abonnement bereitgestellt. Die Isolation von Azure-Komponenten in verschiedenen Azure-Abonnements kann die Anforderungen verschiedener Branchen erfüllen, z.B. das Einrichten unterschiedlicher Zugriffs- und Autorisierungsebenen.

Ein einzelnes virtuelles Rechenzentrum kann auf eine große Anzahl von Spokes zentral hochskalieren. Allerdings gibt es Plattformenbeschränkungen, wie bei jedem IT-System. Die Hub-Bereitstellung ist an ein bestimmtes Azure-Abonnement gebunden, das Einschränkungen hat, z.B. eine maximale Anzahl von VNET-Peerings. Weitere Informationen finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen][Limits]. In Fällen, in denen diese Beschränkungen ein Problem darstellen, kann die Architektur weiter zentral hochskaliert werden, indem das Modell von einem einzelnen Hub und Spoke auf ein Cluster von Hub und Spokes erweitert wird. Mehrere Hubs in einer oder mehreren Azure-Regionen können mit Expressroute oder einer Standort-zu-Standort-VPN-Verbindung miteinander verbunden werden.

[![2]][2]

Die Einführung von mehreren Hubs erhöht die Kosten und den Verwaltungsaufwand für das System und ist nur durch die Skalierbarkeit (z.B. Systemeinschränkungen oder Redundanz) und die regionale Replikation (z.B. Leistung für Endbenutzer oder Notfallwiederherstellung) gerechtfertigt. In Szenarios, die mehrere Hubs erfordern, sollten alle Hubs die gleichen Dienste anbieten, um die Vorgänge nicht zu erschweren.

##### <a name="interconnection-between-spokes"></a>Verbindung zwischen Spokes
Innerhalb eines einzelnen Spokes ist es möglich, komplexe Workloads mit mehreren Ebenen zu implementieren. Konfigurationen mit mehreren Ebenen können mit Subnetzen (eines für jede Ebene) im gleichen VNET implementiert werden. Dabei werden die Flüsse mit Netzwerksicherheitsgruppen gefiltert.

Andererseits möchte ein Architekt vielleicht eine Workload mit mehreren für mehrere VNETs bereitstellen. Mit dem VNET-Peering können Spokes Verbindungen zu anderen Spokes im gleichen oder anderen Hubs herstellen. Ein typisches Beispiel für dieses Szenario sind Anwendungsverarbeitungsserver, die sich in einem Spoke (VNET) befinden, während die Datenbank in einem anderen Spoke (VNET) bereitgestellt wird. In diesem Fall ist es einfach, die Spokes mit VNET-Peering zu verbinden und den Hub dabei zu umgehen. Die Architektur und Sicherheit sollte sorgfältig überprüft werden, um sicherzustellen, dass bei der Umgehung des Hubs keine wichtigen Sicherheits- oder Prüfpunkte im Hub umgangen werden.

[![3]][3]

Spokes können auch mit einem Spoke verbunden werden, der als Hub fungiert. Bei diesem Ansatz wird eine Hierarchie mit zwei Ebenen erstellt: Der Spoke auf der höheren Ebene (Ebene 0) wird der Hub des unteren Spokes (Stufe 1) in der Hierarchie. Die Spokes des virtuellen Rechenzentrums müssen den Datenverkehr an den zentralen Hub weiterleiten, um sich entweder mit dem lokalen Netzwerk oder dem Internet zu verbinden. Eine Architektur mit zwei Hub-Ebenen führt komplexes Routing ein, das die Vorteile einer einfachen Hub-Spoke-Beziehung aufhebt.

Obwohl Azure komplexe Topologien zulässt, ist eines der wesentlichen Prinzipien virtueller Rechenzentren die Wiederholbarkeit und Einfachheit. Um den Verwaltungsaufwand zu minimieren, wird ein einfacher Hub-Spoke-Entwurf für Referenzarchitektur von virtuellen Rechenzentren empfohlen.

### <a name="components"></a>Komponenten
Ein virtuelles Rechenzentrum besteht aus vier grundlegenden Komponententypen: der **Infrastruktur**, den **Umkreisnetzwerken**, den **Workloads** und der **Überwachung**.

Jeder Komponententyp besteht aus verschiedenen Azure-Funktionen und -Ressourcen. Ihr virtuelles Rechenzentrum besteht aus Instanzen verschiedener Komponententypen und mehreren Variationen des gleichen Komponententyps. Angenommen, Sie haben viele verschiedene, logisch getrennte Workload-Instanzen, die verschiedene Anwendungen darstellen. Sie verwenden diese verschiedene Komponententypen und Instanzen, um letztendlich das virtuelle Rechenzentrum zu erstellen.

[![4]][4]

Die vorangehende allgemeine Architektur eines virtuellen Rechenzentrums zeigt verschiedene Komponententypen aus verschiedenen Zonen der Hub-Spoke-Topologie. Die Abbildung zeigt die Infrastrukturkomponenten in verschiedenen Teilen der Architektur.

Es empfiehlt sich (für einen lokales oder virtuelles Rechenzentrum), Zugriffsrechte und Berechtigungen gruppenbasiert zu entwerfen. Der Umgang mit Gruppen statt einzelnen Benutzern hilft bei der kontinuierlichen und teamübergreifenden Pflege von Zugriffsrichtlinien und der Minimierung von Konfigurationsfehlern. Das Zuweisen und Entfernen von Benutzern zu und aus den entsprechenden Gruppen erleichtert die Aktualisierung der Berechtigungen von bestimmten Benutzern.

Jede Rollengruppe sollte ein eindeutiges Präfix im Namen tragen, um die Zuordnung zwischen Gruppe und Workloads zu erleichtern. Eine Workload, die einen Authentifizierungsdienst hostet, könnte Gruppen namens *AuthServiceNetOps, AuthServiceSecOps, AuthServiceDevOps und AuthServiceInfraOps* enthalten. Nach dem gleichen Muster kann das Präfix für zentrale Rollen oder Rollen, die in keinem Zusammenhang mit einem bestimmten Dienst stehen, „Corp“ lauten, wie in *CorpNetOps* z.B.

Viele Organisationen verwenden in etwa die folgenden Gruppen, um Rollen bereitzustellen:

-   Die *zentrale IT-Gruppe (Corp)* hat die Besitzrechte, um Infrastrukturkomponenten (z.B. Netzwerk- und Sicherheitskomponenten) zu steuern, und braucht daher die Teilnehmerrolle im Abonnement (und Kontrolle über den Hub) und Netzwerkteilnehmerrechte in den Spokes. Große Unternehmen teilen diese Verwaltungsaufgaben häufig zwischen mehreren Teams auf, z.B. zwischen einer Gruppe für Netzwerkvorgänge (Network Operations, „CorpNetOps“), mit exklusivem Fokus auf den Netzwerkbetrieb, und einer Gruppe für Sicherheitsvorgänge (Security Operations, „CorpSecOps“), verantwortlich für Firewall- und Sicherheitsrichtlinien. In diesem speziellen Fall müssen zwei unterschiedliche Gruppen für die Zuweisung dieser benutzerdefinierten Rollen erstellt werden.
-   Die *Entwicklungs- & Testgruppe (AppDevOps)* ist dafür verantwortlich, Workloads bereitzustellen (Apps oder Dienste). Diese Gruppe übernimmt die Rolle eines Mitwirkender von virtuellen Computern für IaaS-Bereitstellungen und/oder eine oder mehreren PaaS-Mitwirkenden. Informationen dazu finden Sie unter [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure][Roles]. Optional benötigt das Entwicklungs- und Testteam möglicherweise Sichtbarkeit auf Sicherheitsrichtlinien (NSGs) und Routingrichtlinien (UDR) im Hub oder einem bestimmte Spoke. Daher braucht die Gruppe zusätzlich zur Rolle des Mitwirkenden für Workloads auch die Rolle des Netzwerklesers.
-   Die *Betriebs- und Wartungsgruppe (CorpInfraOps oder AppInfraOps)* hat die Verantwortung, Workloads in der Produktion zu verwalten. Diese Gruppe muss ein Mitwirkender des Abonnements von Workloads in jedem Produktionsabonnement sein. Einige Organisationen können auch auswerten, ob sie ein zusätzliches Eskalations-Supportteam mit der Rolle des Mitwirkenden des Abonnements in der Produktion und im zentralen Hub-Abonnement benötigen, um mögliche Konfigurationsprobleme in der Produktionsumgebung zu korrigieren.

Ein virtuelles Rechenzentrum ist so strukturiert, dass es für Gruppen, die für zentrale den Hub verwaltende IT-Gruppen erstellt wurden, entsprechende Gruppen auf Workload-Ebene gibt. Zusätzlich zur Verwaltung der Hub-Ressourcen könnten nur die zentralen IT-Gruppen den externen Zugriff und die Berechtigungen der obersten Ebene für das Abonnement steuern. Allerdings wären Workloadgruppen in der Lage, Ressourcen und Berechtigungen ihres VNETs unabhängig von der zentralen IT zu steuern.

Das virtuelle Rechenzentrum muss partitioniert werden, um verschiedene Projekte sicher in unterschiedlichen Brachen zu hosten. Alle Projekte erfordern unterschiedliche isolierte Umgebungen (Dev, UAT, Produktion). Einzelne Azure-Abonnements für jede dieser Umgebungen stellen natürliche Isolationsstufen bereit.

[![5]][5]

Die vorherige Abbildung veranschaulicht die Beziehung zwischen den Projekten, Benutzern und den Umgebungen einer Organisation, in denen die Azure-Komponenten bereitgestellt werden.

In der Regel ist eine Umgebung (oder Ebene) in der IT ein System, in dem mehrere Anwendungen bereitgestellt und ausgeführt werden. Große Unternehmen verwenden eine Entwicklungsumgebung (in der Änderungen ursprünglich vorgenommen und getestet werden) und eine Produktionsumgebung (die die Endbenutzer verwenden). Diese Umgebungen werden häufig durch mehrere Stagingumgebungen getrennt, um eine stufenweise Bereitstellung (Rollout), Tests und bei Problemen ein Rollback zu ermöglichen. Bereitstellungsarchitekturen unterscheiden sich erheblich. In der Regel wird aber dem Grundmuster (Entwicklung (DEV) = Beginn, Produktion (PROD) = Ende) gefolgt.

Eine übliche Architektur für diese Arten von Umgebungen mit mehreren Ebenen besteht aus DevOps (Entwicklung und Tests), UAT (Staging) und Produktionsumgebungen. Organisationen können einzelne oder mehrere Azure AD-Mandanten nutzen, um Zugriff und Rechte auf diese Umgebungen zu definieren. Die vorherige Abbildung veranschaulicht die Verwendung von zwei verschiedenen Azure AD-Mandanten: einen für DevOps und UAT und der andere ausschließlich für die Produktion.

Wenn mehrere Azure AD-Mandanten vorhanden sind, müssen die Umgebungen getrennt werden. Dieselbe Benutzergruppe (z.B. die zentrale IT) muss sich mit einem anderen URI authentifizieren, um Zugriff auf einen anderen AD-Mandanten zu erhalten oder die Rollen oder Berechtigungen der DevOps oder der Produktionsumgebungen eines Projekts zu ändern. Wenn für den Zugriff auf verschiedenen Umgebungen auch verschiedene Methoden der Benutzerauthentifizierung bereitstehen, wird das Risiko möglicher Ausfälle und anderer Probleme reduziert, die auf menschlichem Versagen beruhen.

#### <a name="component-type-infrastructure"></a>Komponententyp: Infrastruktur
In diesem Komponententyp befindet sich der Großteil der unterstützenden Infrastruktur. Dort verbringen auch die zentrale IT-Sicherheit und/oder die Complianceteams die meiste Zeit.

[![6]][6]

Infrastrukturkomponenten stellen eine Verbindung zwischen den verschiedenen Komponenten eines virtuellen Rechenzentrums bereit, und sind sowohl in dem Hub als auch in den Spokes vorhanden. Die Verantwortung für die Verwaltung und Wartung der Infrastrukturkomponenten wird in der Regel der zentralen IT und/oder dem Sicherheitsteam zugewiesen.

Eine der wichtigsten Aufgaben des IT-Infrastrukturteams ist die Gewährleistung der Konsistenz von IP-Adressenschemata im gesamten Unternehmen. Der private IP-Adressraum, der dem virtuellen Rechenzentrum zugewiesen ist, muss konsistent sein und darf sich NICHT mit privaten IP-Adressen in Ihren lokalen Netzwerken überlappen.

Obwohl die Netzwerkadressenübersetzung (Network Address Translation, NAT) in den lokalen Edgeroutern oder in Azure-Umgebungen IP-Adressenkonflikte vermeiden kann, bedeutet sie Komplikationen für Ihre Infrastrukturkomponenten. Die Vereinfachung der Verwaltung ist eines der wichtigsten Ziele von virtuellen Rechenzentren. Aus diesem Grund ist es nicht empfehlenswert, NAT für die Behandlung von IP-Problemen zu verwenden.

Infrastrukturkomponenten enthalten die folgende Funktionen:

-   [**Identitäts-und Verzeichnisdienste**][AAD]. Der Zugriff auf jeden Ressourcentyp in Azure wird durch eine in einem Verzeichnisdienst gespeicherte Identität gesteuert. Der Verzeichnisdienst speichert nicht nur die Liste der Benutzer, sondern auch die Zugriffsrechte für Ressourcen in einem bestimmten Azure-Abonnement. Diese Dienste können rein cloudbasiert sein, oder sie können mit einer lokalen Identität in Active Directory synchronisiert werden.
-   [**Virtuelle Netzwerke**][VPN]. Virtuelle Netzwerke sind eine der Hauptkomponenten eines virtuellen Rechenzentrums und ermöglichen es Ihnen, die eine Isolationsgrenze für den Datenverkehr auf der Azure-Plattform zu erstellen. Ein virtuelles Netzwerk besteht aus einem einzelnen oder mehreren virtuellen Netzwerksegmenten, die jeweils ein bestimmtes Präfix für IP-Netzwerke (ein Subnetz) haben. Das virtuelle Netzwerk definiert einen internen Umkreisbereich, in dem virtuelle IaaS-Computer und PaaS-Dienste private Kommunikation herstellen können. Virtuelle Computer (und PaaS-Dienste) in einem virtuellen Netzwerk können nicht direkt mit virtuellen Computern (und PaaS-Diensten) in anderen virtuellen Netzwerken kommunizieren, selbst wenn beide virtuelle Netzwerke vom gleichen Kunden im selben Abonnement erstellt wurden. Isolation ist eine wichtige Eigenschaft, mit der sichergestellt wird, dass VMs und die Kommunikation von Kunden innerhalb eines virtuellen Netzwerks privat bleiben.
-   [**UDR**][UDR]. Datenverkehr in einem virtuellen Netzwerk wird standardmäßig basierend auf der Systemroutingtabelle weitergeleitet. Eine benutzerdefinierte Route ist eine benutzerdefinierte Routingtabelle, die Netzwerkadministratoren einem oder mehreren Subnetzen zum Überschreiben des Verhaltens der Systemroutingtabelle und zum Definieren eines Kommunikationspfads innerhalb eines virtuellen Netzwerks zuordnen können. UDRs gewährleisten, dass ausgehender Datenverkehr aus dem Spoke durch einen bestimmten benutzerdefinierten virtuellen Computer und/oder virtuelle Netzwerkgeräte und Load Balancer im Hub und in den Spokes geleitet wird.
-   [**NSG**][NSG]. Eine Netzwerksicherheitsgruppe (NSG) ist eine Liste von Sicherheitsregeln, die als Datenverkehrfilter für IP-Quellen, IP-Ziele, Protokolle, IP-Quellports und IP-Zielports fungieren. Die NSG kann auf ein Subnetz, auf eine virtuelle Netzwerkkarte (NIC), die mit einer Azure-VM verknüpft ist, oder beide angewendet werden. Die Netzwerksicherheitsgruppen sind entscheidend für die Implementierung einer korrekten Flusssteuerung im Hub und den Spokes. Das Maß an Sicherheit, das die NSGs bieten, hängt davon ab, welche Ports Sie öffnen und zu welchem Zweck. Kunden sollten zusätzliche Filter für jede VM mit hostbasierten Firewalls anwenden, z.B. IPtables oder die Windows-Firewall.
-   **DNS**. Die Namensauflösung von Ressourcen in die VNETs eines virtuellen Rechenzentrums wird über DNS bereitgestellt. Der Bereich der Namensauflösung der standardmäßigen DNS ist auf das VNET beschränkt. In der Regel muss ein benutzerdefinierter DNS-Dienst im Hub als Teil eines gemeinsamen Diensts bereitgestellt werden, während die wichtigsten Benutzer von DNS-Diensten sich im Spoke befinden. Bei Bedarf können Kunden eine hierarchische DNS-Struktur mit Delegierung von DNS-Zonen an die Spokes erstellen.
-   [**Abonnement][SubMgmt] und [Ressourcengruppenverwaltung][RGMgmt]**. Ein Abonnement definiert eine natürliche Grenze, um mehrere Ressourcengruppen in Azure zu erstellen. Ressourcen in einem Abonnement werden in logischen Containern namens Ressourcengruppen zusammen assembliert. Die Ressourcengruppe stellt eine logische Gruppe dar, in der die Ressourcen eines virtuellen Rechenzentrums organisiert werden.
-   [**RBAC**][RBAC]. Über RBAC ist es möglich, die Organisationsrolle zusammen mit der Zugriffsberechtigung für bestimmte Azure-Ressourcen zuzuordnen und gleichzeitig Benutzer auf eine bestimmte Teilmenge von Aktionen zu beschränken. Mit RBAC können Sie Zugriff gewähren, indem Sie Benutzern, Gruppen und Anwendungen innerhalb des relevanten Bereichs die entsprechende Rolle zuweisen. Der Bereich einer Rollenzuweisung kann ein Azure-Abonnement, eine Ressourcengruppe oder eine einzelne Ressource sein. RBAC ermöglicht die Vererbung von Berechtigungen. Mit einer Rolle, die einem übergeordneten Bereich zugewiesen ist, wird außerdem der Zugriff auf die darin enthaltenen untergeordneten Elemente gewährt. Mit RBAC können Sie Aufgaben verteilen und Benutzern nur den Zugriff gewähren, den sie zur Ausführung ihrer Aufgaben benötigen. Gestatten Sie z.B. mit RBAC einem Mitarbeiter die Verwaltung virtueller Computer in einem Abonnement, während ein anderer im gleichen Abonnement SQL-Datenbanken verwalten kann.
-   [**VNET-Peering**][VNetPeering]. Die grundlegende Funktion für das Erstellen der Infrastruktur eines virtuellen Rechenzentrums ist das VNET-Peering. Dabei werden zwei virtuelle Netzwerke in der gleichen Region über das Netzwerk des Azure-Rechenzentrums verbunden.

#### <a name="component-type-perimeter-networks"></a>Komponententyp: Umkreisnetzwerke
[Umkreisnetzwerk][DMZ]-Komponenten (auch DMZ-Netzwerk) ermöglichen Netzwerkverbindungen mit lokalen oder physischen Netzwerken von Rechenzentren und darüber hinaus eine Verbindung mit dem Internet. Dafür bringen Ihre Netzwerk- und Sicherheitsteams wahrscheinlich den Großteil ihrer Zeit auf.

Die eingehenden Pakete sollten durch die Sicherheitsgeräte im Hub wie die Firewall, IDS und IPS geleitet werden, bevor sie die Back-End-Server erreichen. Von den Workloads an das Internet gesendete Pakete sollten zur Richtlinienerzwingung, zur Überprüfung und zu Überwachungszwecken ebenfalls über die Sicherheitsgeräte im Umkreisnetzwerk geleitet werden, bevor sie das Netzwerk verlassen.

Umkreisnetzwerk-Komponenten stellen folgende Funktionen bereit:

-   [Virtuelle Netzwerke][VNet], [UDR][UDR], [NSG][NSG]
-   [Virtuelle Netzwerkgeräte][NVA]
-   [Load Balancer][ALB]
-   [Application Gateway][AppGW] / [WAF][WAF]
-   [Öffentliche IP-Adressen][PIP]

In der Regel haben die zentralen IT- und Sicherheitsteams die Verantwortung für die Anforderungsdefinition und die Vorgänge in den Umkreisnetzwerken.

[![7]][7]

Die obige Abbildung veranschaulicht die Durchsetzung von zwei Perimetern mit Zugriff auf das Internet und einem lokalen Netzwerk, beide im Hub. In einem einzelnen Hub kann das Umkreisnetzwerk zentral zum Internet hochskalieren, um eine große Anzahl von Branchen zu unterstützen, und verwendet dazu mehrere Farmen von Web Application Firewalls (WAFs) und/oder Firewalls.

[**Virtuelle Netzwerke**][VNet]: Der Hub basiert normalerweise auf einem VNET mit mehreren Subnetzen, um verschiedene Arten von Diensten zu hosten, die den Datenverkehr aus dem Internet über NVAs, WAFs und Azure Application Gateways filtern und überprüfen.

[**UDR**][UDR]: Mit UDR können Kunden Firewalls, IDS/IPS und andere virtuelle Geräte bereitstellen und den Netzwerkdatenverkehr über diese Sicherheitsgeräte routen, um Sicherheitsrichtlinien zu erzwingen, zur Überprüfung und zu Überwachungszwecken. UDRs können sowohl im Hub als auch in den Spokes erstellt werden, um sicherzustellen, dass Datenverkehr durch bestimmte benutzerdefinierte VMs, virtuelle Netzwerkgeräte und Load Balancer geleitet wird, die vom virtuellen Rechenzentrum verwendet werden. Um zu gewährleisten, dass der von virtuellen Computern generierte Datenverkehr im Spoke zu den richtigen virtuellen Geräten geleitet wird, muss ein UDR in den Subnetzen des Spokes festgelegt werden, indem die Front-End-IP-Adresse des internen Load Balancers als nächster Hop festgelegt wird. Der interne Load Balancer verteilt den internen Datenverkehr auf die virtuellen Geräte (Back-End-Pool des Load Balancers).

[![8]][8]

[**Virtuelle Netzwerkgeräte**][NVA]: Im Hub wird das Umkreisnetzwerk mit Zugriff auf das Internet normalerweise über eine Farm von Firewalls und/oder Web Application Firewalls (WAFs) verwaltet.

In verschiedenen Branchen werden häufig viele Webanwendungen verwendet. Diese Anwendungen weisen häufig verschiedene Sicherheitsrisiken und Exploits auf. Web Applications Firewalls sind besondere Produkte, die Angriffe auf Webanwendungen (HTTP/HTTPS) genauer als eine generische Firewall erkennen. Verglichen mit herkömmlichen Firewalls besitzen WAFs einen Satz von bestimmten Features, um den internen Webserver vor Bedrohungen zu schützen.

Eine Firewall-Farm besteht aus der Gruppe von Firewalls, die gemeinsam unter der selben Verwaltung mit einem Satz von Sicherheitsregeln ausgeführt werden, um die in den Spokes gehosteten Workloads zu schützen den Zugriff auf lokale Netzwerke zu steuern. Eine Firewall-Farm verfügt im Vergleich zu einem WAF über weniger spezialisierte Software, muss aber einen umfassenden Anwendungsbereich filtern und alle Arten von ein- oder ausgehendem Datenverkehr überprüfen. Firewall-Farmen werden normalerweise in Azure über virtuelle Netzwerkgeräte (Network Virtual Appliances, NVAs) implementiert, die im Azure Marketplace verfügbar sind.

Es wird empfohlen, einen Satz von NVAs für Datenverkehr aus dem Internet zu verwenden und einen anderen für Datenverkehr mit lokalem Ursprung. Wenn Sie für beides nur einen Satz von NVAs verwenden, stellt dies ein Sicherheitsrisiko dar, da kein wirksamer Sicherheitsbereich zwischen den beiden Sätzen von Netzwerkdatenverkehr aufgebaut wird. Separate NVAs erleichtern das Überprüfen von Sicherheitsregeln und verdeutlichen, welche Regeln der eingehenden Netzwerkanforderung entsprechen.

Die meisten großen Unternehmen verwalten mehrere Domänen. Mit Azure DNS lassen sich DNS-Einträge für eine bestimmte Domäne hosten. Die virtuelle IP-Adresse (VIP) des externen Azure Load Balancers (oder der WAFs) kann beispielsweise im A-Eintrag eines Azure DNS-Datensatzes registriert werden.

[**Azure Load Balancer**][ALB]: Der Azure Load Balancer bietet einen Dienst mit hoher Verfügbarkeit der Ebene 4 (TCP, UDP), der eingehenden Datenverkehr zwischen Dienstinstanzen verteilen kann, die in einem Satz mit Lastenausgleich definiert sind. Der Datenverkehr, der von den Front-End-Endpunkten (öffentliche IP-Endpunkte oder private IP-Endpunkte) an den Load Balancer gesendet wird, kann mit oder ohne Adressübersetzung auf einen Satz von Back-End-IP-Adresspools weiterverteilt werden (z.B. virtuelle Netzwerkgeräte oder VMs).

Der Azure Load Balancer kann die Integrität der verschiedenen Serverinstanzen überprüfen, und wenn bei einem Test die Antwort ausbleibt, beendet der Load Balancer das Senden von Datenverkehr an die fehlerhafte Instanz. In einem virtuellen Rechenzentrum gibt es einen externen Load Balancer im Hub (z.B. für den Datenverkehr zu NVAs) und in den Spokes (für den Datenverkehr zwischen verschiedenen VMs in einer Anwendung mit mehreren Ebenen).

[**Microsoft Azure Application Gateway**][AppGW]: Microsoft Azure Application Gateway ist ein dediziertes virtuelles Gerät mit einem ADC (Application Delivery Controller) als Dienst und bietet verschiedene Lastenausgleichsfunktionen der Ebene 7 für Ihre Anwendung. Sie können damit die Produktivität von Webfarmen steigern, indem sie die CPU-intensive SSL-Beendigung an das Anwendungsgateway auslagern. Darüber hinaus werden noch weitere Routingfunktionen der Ebene 7 bereitgestellt. Hierzu zählen etwa die Roundrobin-Verteilung des eingehenden Datenverkehrs, cookiebasierte Sitzungsaffinität, Routing auf URL-Pfadbasis und die Möglichkeit zum Hosten mehrerer Websites hinter einer einzelnen Application Gateway-Instanz. Eine Web Application Firewall (WAF) wird auch als Teil des WAF SKU des Anwendungsgateways bereitgestellt. Dieser SKU bietet Schutz für Webanwendungen vor allgemeinen Onlinesicherheitsrisiken und Exploits. Application Gateway kann als Gateway mit Internetanbindung, rein internes Gateway oder als Kombination dieser beiden Optionen konfiguriert werden. 

[**Öffentliche IP-Adressen**][PIP]: Einige Azure-Funktionen ermöglichen es Ihnen, einer öffentlichen IP-Adresse Dienstendpunkte zuzuordnen, sodass über das Internet auf die Ressource zugegriffen werden kann. Der Endpunkt verwendet NAT (Network Address Translation), um Datenverkehr an die interne Adresse und den Port im virtuellen Azure-Netzwerk zu routen. Dieser Pfad ist der primäre Weg, auf dem externer Datenverkehr in das virtuelle Netzwerk gelangt. Die öffentlichen IP-Adressen können so konfiguriert werden, dass sie festlegen, welcher Datenverkehr zugelassen wird bzw. wie und wo eine Übersetzung in das virtuelle Netzwerk stattfindet.

#### <a name="component-type-monitoring"></a>Komponententyp: Überwachung
Überwachungskomponenten bieten Sichtbarkeit und Warnungen aus allen anderen Komponententypen. Alle Teams sollten die Komponenten und Dienste, auf die sie Zugriff haben, überwachen können. Wenn Sie einen zentralen Helpdesk oder Betriebsteams haben, brauchen diese integrierten Zugriff auf die von diesen Komponenten bereitgestellten Daten.

Azure bietet verschiedene Arten der Protokollierung und Überwachung von Diensten, um das Verhalten von in Azure gehosteten Ressourcen nachzuverfolgen. Governance und Steuerung von Workloads in Azure basiert nicht nur auf dem Sammeln von Protokolldaten, sondern auch auf der Möglichkeit, Aktionen auf der Grundlage von bestimmten gemeldeten Ereignissen auszulösen.

Es gibt zwei Hauptarten von Protokollen in Azure:

-   [**Aktivitätsprotokolle**][ActLog] (auch „Betriebsprotokolle“) bieten Einblicke in die Vorgänge, die auf Ressourcen im Azure-Abonnement ausgeführt wurden. Diese Protokolle melden die Ereignisse der Steuerungsebene für Ihre Abonnements. Jede Azure-Ressource erzeugt Überwachungsprotokolle.

-   [**Azure-Diagnoseprotokolle**][DiagLog] sind von einer Ressource generierte Protokolle mit umfangreichen, in kurzen Abständen erfassten Betriebsdaten der Ressource. Der Inhalt dieser Protokolle variiert je nach Ressourcentyp.

[![9]][9]

In einem virtuellen Rechenzentrum ist es äußerst wichtig, die Netzwerksicherheitsgruppen-Protokolle nachzuverfolgen, besonders die folgenden Informationen:

-   [**Ereignisprotokolle**][NSGLog]: Diese Protokolle stellen Informationen dazu bereit, welche NSG-Regeln auf virtuelle Computer und Instanzrollen auf Grundlage der MAC-Adresse angewendet werden.
-   [**Leistungsindikatorenprotokolle**][NSGLog]: Diese Protokolle verfolgen, wie oft jede NSG-Regel ausgeführt wurde, um Datenverkehr zuzulassen oder zu verweigern.

Alle Protokolle können zur Überwachung, statischen Analyse oder aus Sicherungszwecken in Azure Storage-Konten gespeichert werden. Wenn die Protokolle in einem Azure Storage-Konto gespeichert werden, können Kunden verschiedene Typen von Frameworks verwenden, um diese Daten abzurufen, vorzubereiten, zu analysieren und zu visualisieren, um den Status und die Integrität von Cloudressourcen zu melden.

Große Unternehmen sollten bereits ein Standard-Framework für die Überwachung lokaler Systeme erworben haben und können dieses Framework so erweitern, dass Protokolle integriert werden, die von Cloudbereitstellungen generiert wurden. Für Organisationen, die die gesamte Protokollierung in der Cloud durchführen möchten, ist die [Microsoft Operations Management Suite (OMS)][OMS] eine gute Lösung. Da OMS als cloudbasierter Dienst implementiert wird, ist die Lösung mit minimalen Investitionen in Infrastrukturdienste schnell betriebsbereit. OMS kann auch in System Center-Komponenten wie System Center Operations Manager integriert werden, um Ihre bestehenden Investitionen in die Cloud zu erweitern.

OMS Log Analytics ist eine Komponente des OMS-Frameworks, mit dem von Betriebssystemen, Anwendungen und Komponenten der Cloud-Infrastruktur generierte Protokoll- und Leistungsdaten erfasst, korreliert, durchsucht und bearbeitet werden können. Mit dieser Komponente erhalten Kunden betriebliche Einblicke in Echtzeit, wenn mithilfe der integrierten Suche und den benutzerdefinierten Dashboards alle Datensätze in allen Workloads in einem virtuellen Rechenzentrum analysiert werden.

#### <a name="component-type-workloads"></a>Komponententyp: Workloads
Workload-Komponenten befinden sich dort, wo auch Ihre tatsächlichen Anwendungen und Dienste sind. Dort verbringen auch Ihre Anwendungsentwicklungsteams die meiste Zeit.

Die Möglichkeiten für Workoads sind endlos. Im Folgenden finden Sie einige der möglichen Workload-Typen:

**Interne Branchenanwendungen**

Branchenanwendungen sind Computeranwendungen, die für den laufenden Unternehmensbetrieb entscheidend sind. Branchenanwendungen haben einige gemeinsame Merkmale:

-   **Interaktiv**: Branchenanwendungen sind naturgemäß interaktiv: Daten werden eingegeben, und Ergebnisse/Berichte werden zurückgegeben.
-   **Datengesteuert**: Branchenanwendungen sind datenintensiv und greifen häufig auf die Datenbanken oder einen anderen Speicher zu.
-   **Integriert**: Branchenanwendungen bieten Integration mit anderen Systemen innerhalb oder außerhalb der Organisation.

**Websites für Kunden (im Internet oder intern)**: Die meisten Anwendungen, die mit dem Internet interagieren, sind Websites. Azure bietet die Möglichkeit, eine Website auf einer IaaS-VM oder von einer [Azure-Web-Apps][WebApps]-Website (PaaS) auszuführen. Azure-Web-Apps unterstützen die Integration mit VNETs, die die Bereitstellung von Web-Apps im Spoke eines virtuellen Rechenzentrums ermöglichen. Bei der VNET-Integration müssen Sie keinen öffentlichen Endpunkt für Anwendungen auf Ihrer VM verfügbar machen, sondern Sie können stattdessen die privaten, nicht über das Internet routbaren Adressen der Ressourcen Ihres privaten VNETs verwenden.

**Big Data/Analyse**: Wenn Daten zentral auf ein sehr großes Volumen hochskaliert werden müssen, werden Datenbanken möglicherweise nicht ordnungsgemäß hochskaliert. Die Hadoop-Technologie bietet ein System zum parallelen Ausführen von verteilten Abfragen auf einer großen Anzahl von Knoten. Kunden haben die Möglichkeit, Datenworkloads auf IaaS-VMs oder PaaS auszuführen ([HDInsight][HDI]). HDInsight unterstützt das Bereitstellen in einem lokalen VNet: Die Bereitstellungen kann in einem Cluster in einem Spoke eines virtuellen Rechenzentrums erfolgen.

**Ereignisse und Nachrichten**: 
[Azure Event Hubs][EventHubs] ist ein Dienst mit Hyperskalierung für die Erfassung von Telemetriedaten, der Millionen von Ereignissen sammelt, transformiert und speichert. Diese verteilte Streamingplattform bietet niedrige Latenz und konfigurierbare Aufbewahrungszeiten, wodurch Sie riesige Mengen an Telemetriedaten in Azure einspeisen und die Daten verschiedener Anwendungen lesen können. In Event Hubs kann ein einziger Datenstrom in Echtzeit und batchbasierte Pipelines unterstützen.

Ein höchst zuverlässiger Cloudmessagingdienst zwischen Anwendungen und Diensten kann über [Azure Service Bus][ServiceBus] implementiert werden. Dieser Dienst bietet asynchrones Brokermessaging zwischen Client und Server und außerdem strukturiertes First-In-First-Out-Messaging (FIFO) und Funktionen zum Veröffentlichen/Abonnieren.

[![10]][10]

### <a name="multiple-vdc"></a>Mehrere virtuelle Rechenzentren
Bisher hat sich dieser Artikel auf ein einzelnes virtuelles Rechenzentrum konzentriert und die grundlegenden Komponenten und die Architektur beschrieben, das zu einem robusten virtuellen Rechenzentrum beiträgt. Azure-Funktionen wie der Azure Load Balancer, NVAs, Verfügbarkeitsgruppen, Skalierungsgruppen und andere Mechanismen bilden ein System, mit dem Sie solide SLA-Ebenen in Ihre Produktionsdienste integrieren können.

Ein einzelnes virtuelles Rechenzentrum, das innerhalb einer Region gehostet wird, ist anfällig für einen größeren Ausfall, der die gesamte Region betreffen kann. Kunden, die hohe SLAs erzielen möchten, müssen die Dienste über Bereitstellungen desselben Projekts in zwei (oder mehr) virtuellen Rechenzentren schützen, die in unterschiedlichen Regionen platziert werden müssen.

Zusätzlich zu den Überlegungen zu SLAs gibt es einige allgemeine Szenarios, in denen die Bereitstellung mehrerer virtueller Rechenzentren sinnvoll ist:

-   Lokale/Globale Präsenz
-   Notfallwiederherstellung
-   Mechanismus zum Umleiten von Datenverkehr zwischen DCs

#### <a name="regionalglobal-presence"></a>Lokale/Globale Präsenz
Azure-Rechenzentren befinden sich in zahlreichen Regionen weltweit. Wenn Sie mehrere Azure-Rechenzentren auswählen, müssen Kunden zwei zusammenhängende Faktoren berücksichtigen: die geografische Entfernung und die Latenz. Kunden müssen die geografische Entfernung zwischen den virtuellen Rechenzentren und den Abstand zwischen dem Rechenzentrum und den Endbenutzern bedenken, um die bestmögliche Benutzererfahrung zu bieten.

Die Azure-Region, in der die virtuellen Rechenzentren gehostet werden, müssen auch gesetzlichen Anforderungen der Gesetzgebung erfüllen, der Ihre Organisation unterliegt.

#### <a name="disaster-recovery"></a>Notfallwiederherstellung
Die Implementierung des Plans zur Notfallwiederherstellung ist stark von der Art der betroffenen Workload und der Möglichkeit abhängig, den Zustand der Workload zwischen verschiedenen virtuellen Rechenzentren zu synchronisieren. Die meisten Kunden möchten im Idealfall Anwendungsdaten zwischen Bereitstellungen synchronisieren, die in zwei verschiedenen virtuellen Rechenzentren ausgeführt werden, um einen schnellen Failover-Mechanismus zu implementieren. Die meisten Anwendungen reagieren empfindlich auf Latenz, was mögliche Timeouts und Verzögerungen bei der Datensynchronisierung verursachen kann.

Die Überwachung der Synchronisierung oder des Takts von Anwendungen in verschiedenen virtuellen Rechenzentren erfordert Kommunikation zwischen den Rechenzentren. Zwei virtuelle Rechenzentren in unterschiedlichen Regionen können wie folgt verbunden werden:

-   Über das private Peering mit ExpressRoute, wenn die Hubs der virtuellen Rechenzentren mit der gleichen ExpressRoute-Verbindung verbunden sind
-   Mehrere ExpressRoute-Verbindungen, die über Ihren firmeneigenen Backbone und Ihre virtuellen Rechenzentren mit den ExpressRoute-Verbindungen verbunden sind
-   Standort-zu-Standort-VPN-Verbindungen zwischen Ihren Hubs in den virtuellen Rechenzentren in jeder Azure-Region

In der Regel ist die ExpressRoute-Verbindung der bevorzugte Mechanismus, da es zu Latenz beim Passieren des Microsoft-Backbone zu höherer Bandbreite und einer konsistenten kommt.

Es gibt kein magisches Rezept, um eine zwischen zwei (oder mehr) unterschiedlichen virtuellen Rechenzentren in unterschiedlichen Regionen verteilte Anwendung zu überprüfen. Kunden sollten Netzwerkqualifizierungstests ausführen, um die Latenz und Bandbreite der Verbindungen zu überprüfen, und analysieren, ob die synchrone oder asynchrone Datenreplikation geeignet ist, und was das optimale RTO (Recovery Time Objective) für ihre Workloads sein kann.

#### <a name="mechanism-to-divert-traffic-between-dc"></a>Mechanismus zum Umleiten von Datenverkehr zwischen DCs
DNS ist eine effektive Methode, um den eingehenden Datenverkehr in einem Domänencontroller zu einem anderen umzuleiten. [Azure Traffic Manager][TM] verwendet den Domain Name System-Mechanismus (DNS), um den Endbenutzer-Datenverkehr an den am besten geeigneten öffentlichen Endpunkt in einem bestimmten virtuellen Rechenzentrum weiterzuleiten. Traffic Manager überprüft regelmäßig den Integritätsstatus der öffentlichen Endpunkte in verschiedenen virtuellen Rechenzentren, und bei einem Ausfall dieser Endpunkte wird automatisch an das sekundäre virtuelle Rechenzentrum geroutet.

Traffic Manager funktioniert auf öffentlichen Azure-Endpunkten und kann z.B. dazu verwendet werden, um Datenverkehr zu steuern oder ihn an virtuelle Azure-Computer und -Web-Apps im entsprechenden virtuellen Netzwerk umzuleiten. Traffic Manager ist robust, selbst wenn eine gesamte Azure-Region fehlschlägt, und kann die Verteilung des Benutzerdatenverkehrs für Dienstendpunkte in verschiedenen virtuellen Rechenzentren auf der Grundlage mehrerer Kriterien (z.B. Fehler eines Diensts in einem bestimmten virtuellen Rechenzentrum oder Auswählen des virtuellen Rechenzentrums mit der geringsten Netzwerklatenz für den Client) steuern.

### <a name="conclusion"></a>Zusammenfassung
Das virtuelle Rechenzentrum ist eine Methode für die Migration von Rechenzentren in die Cloud, die eine Kombination von Features und Funktionen verwendet, um eine skalierbare Architektur in Azure zu erstellen, die die Verwendung von Cloudressourcen maximiert, die Kosten senkt und die Systemgovernance vereinfacht. Das Konzept von virtuellen Rechenzentren basiert auf einer Hub-Spoke-Topologie, in der allgemein freigegebene Dienste auf dem Hub bereitstellt und bestimmte Anwendungen/Workloads in den Spokes zugelassen werden. Ein virtuelles Rechenzentrum entspricht der Struktur der Unternehmensrollen, in der verschiedene Abteilungen (zentrale IT, DevOps, Betrieb und Wartung) jeweils mit einer bestimmten Liste von Rollen und Aufgaben zusammenarbeiten. Ein virtuelles Rechenzentrum erfüllt die Anforderungen einer „Lift- und Shift“-Migration, stellt aber außerdem zahlreiche Vorteile nativer Cloudbereitstellungen bereit.

## <a name="references"></a>Referenzen
Die folgenden Features wurden in diesem Dokument erläutert. Klicken Sie auf die Links, um mehr zu erfahren.

| | | |
|-|-|-|
|Netzwerkfunktionen|Lastenausgleich|Konnektivität|
|[Virtuelle Azure-Netzwerke][VNet]</br>[Netzwerksicherheitsgruppen][NSG]</br>[Protokollanalysen für Netzwerksicherheitsgruppen (NSGs)][NSGLog]</br>[Benutzerdefiniertes Routing][UDR]</br>[Virtuelle Netzwerkgeräte][NVA]</br>[Öffentliche IP-Adressen][PIP]|[Azure Load Balancer (L3) ][ALB]</br>[Application Gateway (L7) ][AppGW]</br>[Web Application Firewall][WAF]</br>[Azure Traffic Manager][TM] |[VNET-Peering][VNetPeering]</br>[Virtuelle private Netzwerke][VPN]</br>[ExpressRoute][ExR]
|Identity</br>|Überwachung</br>|Bewährte Methoden</br>|
|[Azure Active Directory][AAD]</br>[Multi-Factor Authentication][MFA]</br>[Rollenbasierte Zugriffssteuerung][RBAC]</br>[Standard-AAD-Rollen][Roles] |[Aktivitätsprotokolle][ActLog]</br>[Diagnoseprotokolle][DiagLog]</br>[Microsoft Operations Management Suite][OMS]</br> |[Bewährte Methoden für Umkreisnetzwerke][DMZ]</br>[Abonnementverwaltung][SubMgmt]</br>[Verwaltung von Ressourcengruppen][RGMgmt]</br>[Einschränkungen für Azure-Abonnements][Limits] |
|Weitere Azure-Dienste|
|[Azure-Web-Apps][WebApps]</br>[HDInsights (Hadoop)][HDI]</br>[Event Hubs][EventHubs]</br>[Service Bus][ServiceBus]|



## <a name="next-steps"></a>Nächste Schritte
 - Lernen Sie das [VNET-Peering][VNetPeering] kennen, die technologische Grundlage für Hub-Spoke-Entwürfe von virtuellen Rechenzentren.
 - Implementieren Sie [AAD][AAD], um mehr über [RBAC][RBAC] zu erfahren.
 - Entwickeln Sie ein Abonnement- und Ressourcenverwaltungsmodell und ein RBAC-Modell, um die Struktur, die Anforderungen und die Richtlinien Ihres Unternehmens zu erfüllen. Die wichtigste Aktivität ist die Planung. Planen Sie Neuorganisationen, Fusionen, neue Produktlinien etc. so weit wie möglich.

<!--Image References-->
[0]: ./media/networking-virtual-datacenter/redundant-equipment.png "Beispiele für Komponentenüberlappung" 
[1]: ./media/networking-virtual-datacenter/vdc-high-level.png "Allgemeines Beispiel für virtuelles Hub- und Spoke-Rechenzentrum"
[2]: ./media/networking-virtual-datacenter/hub-spokes-cluster.png "Cluster mit Hub und Spokes"
[3]: ./media/networking-virtual-datacenter/spoke-to-spoke.png "Spoke-zu-Spoke"
[4]: ./media/networking-virtual-datacenter/vdc-block-level-diagram.png "Blockebenenabbildung des virtuellen Rechenzentrums"
[5]: ./media/networking-virtual-datacenter/users-groups-subsciptions.png "Benutzer, Gruppen, Abonnements und Projekte"
[6]: ./media/networking-virtual-datacenter/infrastructure-high-level.png "Abbildung der allgemeinen Infrastruktur"
[7]: ./media/networking-virtual-datacenter/highlevel-perimeter-networks.png "Abbildung der allgemeinen Infrastruktur"
[8]: ./media/networking-virtual-datacenter/vnet-peering-perimeter-neworks.png "VNET-Peering und Umkreisnetzwerke"
[9]: ./media/networking-virtual-datacenter/high-level-diagram-monitoring.png "Übersichtsabbildung über die Überwachung"
[10]: ./media/networking-virtual-datacenter/high-level-workloads.png "Übersichtsabbildung über Workloads"

<!--Link References-->
[Limits]: https://docs.microsoft.com/azure/azure-subscription-service-limits
[Roles]: https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles
[VNet]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview
[NSG]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg 
[VNetPeering]: https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview 
[UDR]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview 
[RBAC]: https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is
[MFA]: https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication
[AAD]: https://docs.microsoft.com/azure/active-directory/active-directory-whatis
[VPN]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways 
[ExR]: https://docs.microsoft.com/azure/expressroute/expressroute-introduction 
[NVA]: https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha
[SubMgmt]: https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-subscription-governance 
[RGMgmt]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview
[DMZ]: https://docs.microsoft.com/azure/best-practices-network-security
[ALB]: https://docs.microsoft.com/azure/load-balancer/load-balancer-overview
[PIP]: https://docs.microsoft.com/azure/virtual-network/resource-groups-networking#public-ip-address
[AppGW]: https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction
[WAF]: https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview
[ActLog]: https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs 
[DiagLog]: https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs
[NSGLog]: https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log
[OMS]: https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview
[WebApps]: https://docs.microsoft.com/azure/app-service-web/
[HDI]: https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-introduction
[EventHubs]: https://docs.microsoft.com/azure/event-hubs/event-hubs-what-is-event-hubs 
[ServiceBus]: https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview
[TM]: https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview

