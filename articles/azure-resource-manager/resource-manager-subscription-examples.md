---
title: "Szenarien und Beispiele für die Abonnementgovernance | Microsoft Docs"
description: "Enthält Beispiele für das Implementieren der Azure-Abonnementgovernance für allgemeine Szenarien."
services: azure-resource-manager
documentationcenter: na
author: rdendtler
manager: timlt
editor: tysonn
ms.assetid: e8fbeeb8-d7a1-48af-804f-6fe1a6024bcb
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2017
ms.author: rodend;karlku;tomfitz
translationtype: Human Translation
ms.sourcegitcommit: 4f541e34e7c0696e4074613c4ab0734a096c6d12
ms.openlocfilehash: ccbf4b3819051f4520205ce6ebfb66bc39a9dd3b


---
# <a name="examples-of-implementing-azure-enterprise-scaffold"></a>Beispiele für das Implementieren eines Azure-Unternehmensgerüsts
Dieses Thema enthält Beispiele dafür, wie ein Unternehmen die Empfehlungen für ein [Azure-Unternehmensgerüst](resource-manager-subscription-governance.md) implementieren kann. Ein fiktives Unternehmen mit dem Namen Contoso wird verwendet, um die bewährten Methoden für allgemeine Szenarien zu veranschaulichen.

## <a name="background"></a>Hintergrund
Contoso ist ein globales Unternehmen, das Supply Chain-Lösungen für Kunden bietet, die von einem Software-as-a-Service-Modell bis hin zu einem lokal bereitgestellten gepackten Modell alles abdecken.  Sie entwickeln Software auf der ganzen Welt, und betreiben große Entwicklungseinrichtungen in Indien, den USA und Kanada.

Der ISV-Bereich des Unternehmens ist in mehrere unabhängige Unternehmenseinheiten unterteilt, die Produkte in einem erheblichen Umfang verwalten. Jede Unternehmenseinheit verfügt über eigene Entwickler, Produktmanager und Architekten.

Die Unternehmenseinheit Enterprise Technologie Services (ETS) bietet zentrale IT-Funktionen und verwaltet mehrere Rechenzentren, in denen Unternehmenseinheiten ihre Anwendungen hosten. Neben der Verwaltung der Rechenzentren bietet und verwaltet die ETS-Organisation zentralisierte Dienste für Zusammenarbeit (z.B. E-Mail und Websites) sowie für Netzwerk/Telefonie. Sie verwalten zudem kundenorientierte Workloads für kleinere Unternehmenseinheiten, die kein Betriebspersonal haben.

In diesem Thema werden die folgenden Personas verwendet:

* Dave ist der Azure-Administrator von ETS.
* Alice ist bei Contoso die Leiterin der Entwicklung in der Supply Chain-Unternehmenseinheit.

Contoso muss eine branchenspezifische App und eine kundenorientierte App erstellen. Das Unternehmen hat entschieden, die Apps in Azure auszuführen. Dave liest das Thema zur [präskriptiven Abonnementgovernance](resource-manager-subscription-governance.md) und kann jetzt die Empfehlungen implementieren.

## <a name="scenario-1-line-of-business-application"></a>Szenario 1: Branchenanwendung
Contoso baut ein Quellcodeverwaltungssystem (BitBucket) auf, das von Entwicklern auf der ganzen Welt verwendet werden soll.  Die Anwendung nutzt Infrastructure-as-a-Service zum Hosten und besteht aus Webservern und einem Datenbankserver. Entwickler greifen auf Server in ihren Entwicklungsumgebungen zu, aber sie benötigen keinen Zugriff auf die Server in Azure. Contoso ETS möchte dem Anwendungsbesitzer und dem Team das Verwalten der Anwendung ermöglichen. Die Anwendung ist nur über das Unternehmensnetzwerk von Contoso verfügbar. Dave muss das Abonnement für diese Anwendung einrichten. Das Abonnement soll in der Zukunft auch andere Software für Entwickler hosten.  

### <a name="naming-standards--resource-groups"></a>Benennungsstandards und Ressourcengruppen
Dave erstellt ein Abonnement, um Entwicklertools zu unterstützen, die in allen Unternehmenseinheiten genutzt werden. Er muss aussagekräftige Namen für die Abonnement und die Ressourcengruppen (für die Anwendung und die Netzwerke) erstellen. Er erstellt das Abonnement und die Ressourcengruppen wie folgt:

| Item | Name | Beschreibung |
| --- | --- | --- |
| Abonnement |Contoso ETS DeveloperTools Production |Unterstützt allgemeine Entwicklertools |
| Ressourcengruppe |rgBitBucket |Enthält den Anwendungswebserver und den Datenbankserver |
| Ressourcengruppe |rgCoreNetworks |Enthält die virtuellen Netzwerke und die Site-to-Site-Gatewayverbindung |

### <a name="role-based-access-control"></a>Rollenbasierte Zugriffssteuerung
Nach dem Erstellen seines Abonnement möchte Dave sicherstellen, dass die entsprechenden Teams und Anwendungsbesitzer ihre Ressourcen zugreifen können. Dave erkennt, dass jedes Team andere Anforderungen hat. Er nutzt die Gruppen, die aus der lokalen Active Directory-Instanz (AD) von Contoso in Azure Active Directory synchronisiert wurden, und bietet den Teams die richtige Zugriffsebene.

Dave weist die folgenden Rollen für das Abonnement zu:

| Rolle | Zugewiesen zu | Beschreibung |
| --- | --- | --- |
| [Besitzer](../active-directory/role-based-access-built-in-roles.md#owner) |Verwaltete-ID aus AD von Contoso |Diese ID wird mit Just-in-Time-Zugriff (JIT) über das Identitätsverwaltungstool von Contoso gesteuert und stellt sicher, dass der Zugriff des Abonnementbesitzers vollständig überwacht wird. |
| [Sicherheits-Manager](../active-directory/role-based-access-built-in-roles.md#security-manager) |Abteilung für Sicherheits- und Risikomanagement |Diese Rolle ermöglicht es Benutzern, das Azure Security Center und den Status der Ressourcen anzuzeigen. |
| [Mitwirkender von virtuellem Netzwerk](../active-directory/role-based-access-built-in-roles.md#network-contributor) |Netzwerkteam |Diese Rolle ermöglicht es dem Netzwerkteam von Contoso, das Site-to-Site-VPN und die virtuellen Netzwerke zu verwalten. |
| *Benutzerdefinierte Rolle* |Anwendungsbesitzer |Dave erstellt eine Rolle, die die Berechtigung zum Ändern von Ressourcen innerhalb der Ressourcengruppe gewährt. Weitere Informationen finden Sie unter [Benutzerdefinierte Rollen in Azure RBAC](../active-directory/role-based-access-control-custom-roles.md). |

### <a name="policies"></a>Richtlinien
Dave hat die folgenden Anforderungen für die Verwaltung von Ressourcen im Abonnement:

* Da die Entwicklungstools Entwickler auf der ganzen Welt unterstützen, möchte er die Benutzer nicht daran hindern, Ressourcen in einer beliebigen Region zu erstellen. Er muss jedoch wissen, wo Ressourcen erstellt werden.
* Er macht sich Gedanken über die Kosten. Aus diesem Grund möchte er verhindern, dass Anwendungsbesitzer unnötig teure virtuelle Computer erstellen.  
* Da diese Anwendung Entwicklern in vielen Unternehmenseinheiten dient, möchte er jede Ressource mit der Unternehmenseinheit und dem Anwendungsbesitzer kennzeichnen. Mithilfe dieser Tags kann ETS die Ressourcen den richtigen Teams in Rechnung stellen.

Er erstellt die folgenden [Resource Manager-Richtlinien](resource-manager-policy.md):

| Feld | Effekt | Beschreibung |
| --- | --- | --- |
| location |audit |Überwachen der Erstellung von Ressourcen in einer beliebigen Region |
| Typ |deny |Verweigern der Erstellung von virtuellen Computern der G-Serie |
| tags |deny |Erfordern des Tags für Anwendungsbesitzer |
| tags |deny |Erfordern Tags für Kostenstellen |
| tags |append |Anfügen des Tagnamens **BusinessUnit** und des Tagwerts **ETS** an alle Ressourcen |

### <a name="resource-tags"></a>Ressourcentags
Dave weiß, dass in der Rechnung spezifische Informationen enthalten sein müssen, um die Kostenstelle für die BitBucket-Implementierung zu identifizieren. Darüber hinaus möchte Dave alle Ressourcen kennen, die ETS besitzt.

Er fügt die folgenden [Tags](resource-group-using-tags.md) den Ressourcengruppen und Ressourcen hinzu.

| Tagname | Tagwert |
| --- | --- |
| ApplicationOwner |Der Name der Person, die diese Anwendung verwaltet. |
| CostCenter |Die Kostenstelle der Gruppe, die für die Azure-Nutzung bezahlt. |
| BusinessUnit |**ETS** (die dem Abonnement zugeordneten Unternehmenseinheit) |

### <a name="core-network"></a>Kernnetzwerk
Das Contoso ETS-Team für Informationssicherheit und Risikomanagement überprüft den von Dave vorgeschlagenen Plan, die Anwendung nach Azure zu verschieben. Sie möchten sicherstellen, dass die Anwendung nicht über das Internet verfügbar gemacht wird.  Dave verfügt auch über Entwickler-Apps, die in der Zukunft nach Azure verschoben werden. Für diese Apps sind öffentliche Schnittstellen erforderlich.  Um diese Anforderungen zu erfüllen, stellt er interne und externe virtuelle Netzwerke und eine Netzwerksicherheitsgruppe zum Einschränken des Zugriffs bereit.

Er erstellt die folgenden Ressourcen:

| Ressourcentyp | Name | Beschreibung |
| --- | --- | --- |
| Virtual Network |vnInternal |Wird mit der Anwendung BitBucket verwendet und ist über ExpressRoute mit dem Contoso-Unternehmensnetzwerk verbunden.  Ein Subnetz (sbBitBucket) stellt der Anwendung einen bestimmten IP-Adressbereich abereit. |
| Virtual Network |vnExternal |Für zukünftige Anwendungen verfügbar, die öffentliche Endpunkte erfordern. |
| Netzwerksicherheitsgruppen (NSG) |nsgBitBucket |Stellt sicher, dass die Angriffsfläche dieser Workload minimiert wird, indem Verbindungen für das Subnetz mit der Anwendung (sbBitBucket) nur über Port 443 zugelassen werden. |

### <a name="resource-locks"></a>Ressourcensperren
Dave erkennt, dass die Konnektivität zwischen dem Unternehmensnetzwerk von Contoso und dem internen virtuellen Netzwerk vor fehlerhaften Skripts oder versehentlichem Löschen geschützt werden muss.

Er erstellt die folgende [Ressourcensperre](resource-group-lock-resources.md):

| Sperrtyp | Ressource | Beschreibung |
| --- | --- | --- |
| **CanNotDelete** |vnInternal |Verhindert, dass Benutzer das virtuelle Netzwerk oder Subnetze löschen, verhindert jedoch nicht, dass neue Subnetze hinzugefügt werden. |

### <a name="azure-automation"></a>Azure-Automatisierung
Dave möchte für diese Anwendung nichts automatisieren. Obwohl er ein Azure Automation-Konto erstellt hat, verwendet er es zunächst nicht.

### <a name="azure-security-center"></a>Azure Security Center
Die IT-Dienstverwaltung von Contoso muss Bedrohungen schnell identifizieren und behandeln. Sie möchten wissen, welche Probleme vorliegen können.  

Um diese Anforderungen zu erfüllen, aktiviert Dave das [Azure Security Center](../security-center/security-center-intro.md) und bietet Zugriff auf die Rolle „Sicherheits-Manager“.

## <a name="scenario-2-customer-facing-app"></a>Szenario 2: Kundenorientierte App
Die Unternehmensführung in der Supply Chain-Unternehmenseinheit hat verschiedene Möglichkeiten identifiziert, um mithilfe einer Kundenkarte die Kundenbindung für Contoso zu verbessern. Das Team von Alice muss diese Anwendung erstellen und beschließt, dass mit Azure die Anforderungen ihres Unternehmens besser erfüllt werden können. Alice arbeitet mit Dave aus der ETS-Abteilung zusammen, um zwei Abonnements zum Entwickeln und Betreiben dieser Anwendung zu konfigurieren.

### <a name="azure-subscriptions"></a>Azure-Abonnements
Dave meldet sich beim Azure Enterprise Portal an und stellt fest, dass die Supply Chain-Abteilung bereits vorhanden ist.  Da das Projekt jedoch das erste Entwicklungsprojekt für das Supply Chain-Team in Azure ist, erkennt Dave, dass ein neues Konto für das Entwicklungsteam von Alice eingerichtet werden muss.  Er erstellt das Konto „R&D“ für das Team und gewährt Alice Zugriff. Alice meldet sich über das Kontoportal an und erstellt zwei Abonnements: eines für die Entwicklungsserver und eines für die Produktionsserver.  Sie befolgt beim Erstellen der folgenden Abonnements die zuvor festgelegten Benennungsstandards:

| Abonnementnutzung | Name |
| --- | --- |
| Entwicklung |SupplyChain ResearchDevelopment LoyaltyCard Development |
| Bereitstellung |SupplyChain Operations LoyaltyCard Production |

### <a name="policies"></a>Richtlinien
Dave und Alice besprechen die Anwendung und legen fest, dass diese Anwendung nur Kunden in der Region Nordamerika dienen soll.  Alice und ihr Team möchten die Azure Application Service-Umgebung und Azure SQL verwenden, um die Anwendung zu erstellen. Möglicherweise müssen sie während der Entwicklung virtuelle Computer erstellen.  Alice und ihr Team möchten sicherstellen, dass ihre Entwickler über die erforderlichen Ressourcen verfügen, um Probleme ohne Unterstützung durch ETS zu untersuchen.

Für das **Entwicklungsabonnement** erstellen sie die folgende Richtlinie:

| Feld | Effekt | Beschreibung |
| --- | --- | --- |
| location |audit |Überwachen der Erstellung von Ressourcen in einer beliebigen Region. |

Sie begrenzen den Typ von SKU nicht, die ein Benutzer bei der Entwicklung erstellen kann, und sie erfordern keine Tags für Ressourcen oder Ressourcengruppen.

Für das **Produktionsabonnement** erstellen sie die folgenden Richtlinien:

| Feld | Effekt | Beschreibung |
| --- | --- | --- |
| location |deny |Verweigern der Erstellung von Ressourcen außerhalb der US-amerikanischen Rechenzentren. |
| tags |deny |Erfordern des Tags für Anwendungsbesitzer |
| tags |deny |Anfordern eines Abteilungstags. |
| tags |append |Anfügen von Tags an jede Ressourcengruppe in der Produktionsumgebung. |

Sie beschränken den Typ von SKU nicht, den ein Benutzer in der Produktion erstellen kann.

### <a name="resource-tags"></a>Ressourcentags
Dave erkennt, dass er spezifische Informationen benötigt, um die richtigen Unternehmensgruppen für Abrechnung und Besitz zu identifizieren. Er definiert Ressourcentags für Ressourcengruppen und Ressourcen.

| Tagname | Tagwert |
| --- | --- |
| ApplicationOwner |Der Name der Person, die diese Anwendung verwaltet. |
| Department |Die Kostenstelle der Gruppe, die für die Azure-Nutzung bezahlt. |
| EnvironmentType |**Produktion** (Auch wenn das Abonnement das Wort **Production** im Namen enthält, ermöglicht dieses Tag eine leichte Identifikation der Ressourcen im Portal oder auf der Rechnung.) |

### <a name="core-networks"></a>Kernnetzwerke
Das Contoso ETS-Team für Informationssicherheit und Risikomanagement überprüft den von Dave vorgeschlagenen Plan, die Anwendung nach Azure zu verschieben. Sie möchten sicherstellen, dass die Anwendung für die Kundenkarte angemessen isoliert und in einem DMZ-Netzwerk geschützt ist.  Um diese Anforderung zu erfüllen, erstellen Dave und Alice ein externes virtuelles Netzwerk und eine Netzwerksicherheitsgruppe. Damit wird die Anwendung für die Kundenkarte vom Contoso-Unternehmensnetzwerk isoliert.  

Für das **Entwicklungsabonnement** erstellen sie Folgendes:

| Ressourcentyp | Name | Beschreibung |
| --- | --- | --- |
| Virtual Network |vnInternal |Ist für die Entwicklungsumgebung der Contoso-Kundenkarte vorgesehen und über ExpressRoute mit dem Contoso-Unternehmensnetzwerk verbunden. |

Für das **Produktionsabonnement** erstellen sie Folgendes:

| Ressourcentyp | Name | Beschreibung |
| --- | --- | --- |
| Virtual Network |vnExternal |Hostet die Anwendung für die Kundenkarte und ist nicht direkt mit der ExpressRoute-Instanz von Contoso verbunden. Code wird über das Quellcodesystem direkt an die PaaS-Dienste übertragen. |
| Netzwerksicherheitsgruppen (NSG) |nsgBitBucket |Stellt sicher, dass die Angriffsfläche dieser Workload minimiert wird, indem eingehende Kommunikation nur über TCP 443 zugelassen wird.  Contoso untersucht auch die Nutzung einer Web Application Firewall für zusätzlichen Schutz. |

### <a name="resource-locks"></a>Ressourcensperren
Dave und Alice beschließen, Ressourcensperren für einige der wichtigsten Ressourcen in der Umgebung hinzuzufügen, um ein versehentliches Löschen bei einem fehlerhaften Codepush zu verhindern.

Die folgende Sperre wird erstellt:

| Sperrtyp | Ressource | Beschreibung |
| --- | --- | --- |
| **CanNotDelete** |vnExternal |Verhindert, dass das virtuelle Netzwerk oder die Subnetze gelöscht werden. Die Sperre verhindert nicht das Hinzufügen neuer Subnetze. |

### <a name="azure-automation"></a>Azure-Automatisierung
Alice und ihr Entwicklungsteam verfügen über umfangreiche Runbooks zum Verwalten der Umgebung für diese Anwendung. Die Runbooks ermöglichen das Hinzufügen/Löschen von Knoten für die Anwendung und andere DevOps-Aufgaben.

Um diese Runbooks zu verwenden, aktivieren sie [Automation](../automation/automation-intro.md).

### <a name="azure-security-center"></a>Azure Security Center
Die IT-Dienstverwaltung von Contoso muss Bedrohungen schnell identifizieren und behandeln. Sie möchten wissen, welche Probleme vorliegen können.  

Um diese Anforderungen zu erfüllen, aktiviert Dave das Azure Security Center. Er stellt sicher, dass das Azure Security Center die Ressourcen überwacht, und gewährt den DevOps- und Sicherheitsteams Zugriff.

## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Erstellen von Resource Manager-Vorlagen finden Sie unter [Bewährte Methoden für das Erstellen von Azure Resource Manager-Vorlagen](resource-manager-template-best-practices.md).

*[Karl Kuhnhausen](https://github.com/karlkuhnhausen) hat an diesem Thema mitgewirkt.*



<!--HONumber=Nov16_HO3-->


