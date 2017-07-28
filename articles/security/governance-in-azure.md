---

title: Governance in Azure | Microsoft-Dokumentation
description: "Enthält Informationen zu cloudbasierten Computingdiensten mit einer großen Auswahl an Computeinstanzen und -diensten, die automatisch zentral hoch- und herunterskaliert werden können, um die Anforderungen Ihrer Anwendung bzw. Ihres Unternehmens zu erfüllen."
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/01/2017
ms.author: TomSh
ms.translationtype: Human Translation
ms.sourcegitcommit: 43aab8d52e854636f7ea2ff3aae50d7827735cc7
ms.openlocfilehash: 91fb3c70d95cca46dd68e3f15ad67c914cfbfa5b
ms.contentlocale: de-de
ms.lasthandoff: 06/03/2017


---

# <a name="governance-in-azure"></a>Governance (Kontrolle) in Azure

Wir wissen, dass Sicherheit in der Cloud an erster Stelle steht und wie wichtig es ist, dass Sie präzise und zeitnahe Informationen zur Azure-Sicherheit finden. Eines der schlagkräftigsten Argumente dafür, Azure für Anwendungen und Dienste zu verwenden, ist die Vielzahl an Sicherheitstools und -funktionen. Diese Tools und Funktionen ermöglichen die Erstellung sicherer Lösungen auf der Grundlage der sicheren Azure-Plattform.

Damit Sie das Array von Governancekontrollen, die in Microsoft Azure implementiert sind, sowohl aus Kundensicht als auch aus Sicht der Microsoft-Vorgänge besser verstehen, ist dieser Artikel, „Governance in Azure“, so konzipiert, dass er einen umfassenden Überblick über die in Microsoft Azure verfügbaren Governance-Features gibt.

## <a name="azure-platform"></a>Azure Platform

Azure ist eine öffentliche Clouddienstplattform, die eine breite Palette an Betriebssystemen, Programmiersprachen, Frameworks, Tools, Datenbanken und Geräten unterstützt. Die Plattform kann Linux-Container mit Docker-Integration ausführen und Apps mit JavaScript, Python, .NET, PHP, Java und Node.js sowie Back-Ends für iOS-, Android- und Windows-Geräte erstellen. Die öffentlichen Azure-Clouddienste unterstützen dieselben Technologien, die bereits von Millionen von Entwicklern und IT-Profis zuverlässig eingesetzt werden.

Wenn Sie IT-Ressourcen darauf erstellen oder zu einem öffentlichen Clouddienstanbieter migrieren, verlassen Sie sich darauf, dass die jeweilige Organisation Ihre Anwendungen und Daten mit den Diensten und Steuerungsmöglichkeiten schützen kann, die sie zum Verwalten der Sicherheit Ihrer cloudbasierten Ressourcen bereitstellt.

Die Infrastruktur von Azure ist von den Hardwareressourcen bis zu den Anwendungen vollständig auf das gleichzeitige Hosten von Millionen von Kunden ausgelegt und stellt für Unternehmen eine vertrauenswürdige Grundlage zur Erfüllung ihrer Sicherheitsanforderungen dar. Darüber hinaus bietet Azure Ihnen viele Sicherheitsoptionen, die Sie selbst steuern können, um die Sicherheit an die individuellen Anforderungen der Bereitstellungen Ihrer Organisation anzupassen.

Dieses Dokument hilft Ihnen dabei zu verstehen, wie Ihnen die Governance-Funktionen in Azure dabei helfen können, diese Anforderungen zu erfüllen.

## <a name="abstract"></a>Zusammenfassung

Cloud-Governance in Microsoft Azure bietet einen integrierten Ansatz zur Überwachung und Beratung für Organisationen, bei dem die Verwendung der Azure-Plattform überprüft wird und Organisationen entsprechend beraten werden. Cloud-Governance in Microsoft Azure bezieht sich auf Entscheidungsfindungsprozesse, Kriterien und Richtlinien, die für die Planung, Architektur, Beschaffung, Bereitstellung, Ausführung und Verwaltung von Cloud Computing notwendig sind.

Um einen Plan für Cloud-Governance in Microsoft Azure zu erstellen, müssen Sie die aktuell beteiligten Menschen, Prozesse und Technologien genau unter die Lupe nehmen und anschließend Frameworks erstellen, mit der die IT Geschäftsanforderungen durchgehend unterstützen und gleichzeitig Endbenutzern die Flexibilität zur Verwendung der leistungsstarken Features von Microsoft Azure bieten kann.

In diesem Dokument wird beschrieben, wie Sie ein höheres Level von Governance (Kontrolle) Ihrer IT-Ressourcen in Microsoft Azure erreichen. Dieses Dokument vermittelt Ihnen ein Verständnis der in Microsoft Azure integrierten Sicherheits- und Governance-Features.

Hier finden Sie die Hauptthemen, die in diesem Dokument besprochen werden:

- Implementierung von Richtlinien, Prozessen und Prozeduren gemäß der Organisationsziele.

- Sicherheit und kontinuierliche Konformität mit den Standards der Organisation

- Warnung und Überwachung

## <a name="implementation-of-policies-processes-and-procedures"></a>Implementierung von Richtlinien, Prozessen und Prozeduren 

Die Verwaltung hat Rollen und Verantwortlichkeiten eingeführt, um die Implementierung der Richtlinie zur Informationssicherheit und operativer Kontinuität innerhalb von Azure zu überwachen. Die Microsoft Azure-Verwaltung ist für die Überwachung von Sicherheits- und Kontinuitätsmethoden innerhalb ihres jeweiligen Teams (einschließlich Drittanbietern) verantwortlich. Außerdem fallen die Vereinfachung der Einhaltung von Sicherheitsrichtlinien, Prozessen und Standards in ihre Verantwortung.

Hier sehen Sie die entwickelten Faktoren:

- Kontobereitstellung

- Steuerung der Abonnements

- Rollenbasierte Zugriffssteuerung

- Ressourcenverwaltung

- Ressourcennachverfolgung

- Wichtige Ressourcenkontrolle

- API-Zugriff auf Abrechnungsinformationen

- Netzwerkkontrollen

## <a name="account-provisioning"></a>Kontobereitstellung

Das Definieren der Kontohierarchie ist ein wichtiger Schritt zur Verwendung und Strukturierung von Azure-Diensten innerhalb eines Unternehmens und stellt die wichtigste Governance-Struktur dar. Wenn Kunden Teil des Enterprise Agreement sind, können sie die Umgebung in Abteilungen, Konten und Abonnements weiter unterteilen.

![Kontobereitstellung](./media/governance-in-azure/security-governance-in-azure-fig1.png)

Wenn Sie nicht über ein Enterprise Agreement verfügen, sollten Sie [Azure-Tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) auf Abonnementebene verwenden, um die Hierarchie zu definieren. Ein Azure-Abonnement ist die grundlegende Einheit, in der alle Ressourcen enthalten sind. Es definiert außerdem einige Beschränkungen in Azure, wie z.B. die Anzahl der Kerne, Ressourcen usw. Abonnements können [Ressourcengruppen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) enthalten, in denen wiederum Ressourcen enthalten sind. [RBAC](https://docs.microsoft.com/azure/api-management/api-management-role-based-access-control)-Prinzipien gelten für diese drei Ebenen.

Alle Unternehmen sind unterschiedlich, und eine Hierarchie mit Azure-Tags für Nicht-Unternehmenskunden ermöglicht eine enorme Flexibilität im Hinblick auf den Aufbau von Azure im Unternehmen. Vor der Bereitstellung von Ressourcen in Microsoft Azure sollten Sie die Hierarchie gestalten und die Auswirkungen auf die Abrechnung, den Zugriff auf Ressourcen und die Komplexität verstehen.

## <a name="subscription-controls"></a>Steuerung der Abonnements

Abonnements steuern, wie die Nutzung von Ressourcen gemeldet und abgerechnet wird. Abonnements können für separate Abrechnungs- und Zahlungsprozesse eingerichtet werden. Wie bereits erwähnt, kann ein Azure-Konto mehrere Abonnements enthalten. Abonnements können verwendet werden, um die Nutzung von Azure-Ressourcen mehrerer Abteilungen in einem Unternehmen zu ermitteln.

In einem Unternehmen kann es beispielsweise IT-, HR- und Marketingabteilungen geben, die alle verschiedene Projekte durchführen. Die Abrechnung basiert auf dem entsprechenden Verbrauch von Azure-Ressourcen (z.B. virtuelle Computer) jeder Abteilung. Hierdurch können wir die Finanzen der einzelnen Abteilungen kontrollieren.

Azure-Abonnements richten drei Parameter ein:

- eine eindeutige Abonnenten-ID

- einen Rechnungsstandort

- einen Satz der verfügbaren Ressourcen

Für eine Einzelperson würde dies eine Microsoft-Konto-ID, eine Kreditkartennummer und einen vollständigen Satz von Azure-Diensten beinhalten – allerdings gibt Microsoft je nach Art des Abonnements Verbrauchsgrenzen vor.

Azure-Registrierungshierarchien legen fest, wie Dienste innerhalb eines Enterprise Agreement strukturiert sind. Das Enterprise Portal ermöglicht Kunden, den Zugriff auf Azure-Ressourcen aufzuteilen, die einem Enterprise Agreement zugewiesen sind. Dies basiert auf flexiblen Hierarchien und kann so an die individuellen Bedürfnisse eines Unternehmens angepasst werden. Das Muster einer Hierarchie sollte mit der Verwaltung und geografischen Struktur eines Unternehmens übereinstimmen, damit der entsprechende Zugriff auf Abrechnung und Ressourcen genau berücksichtigt werden kann.

Die drei allgemeinen Muster basieren auf Funktionalität, Unternehmenseinheit und geografischen Regionen. Hierbei werden Abteilungen als administratives Konstrukt für Kontogruppierungen verwendet. In jeder Abteilung können Abonnements Konten zugewiesen werden, die Silos für die Abrechnung und zahlreiche Schlüsselgrenzwerte erstellen (z.B. die Anzahl virtueller Computer, Speicherkonten, usw.).

![Steuerung der Abonnements](./media/governance-in-azure/security-governance-in-azure-fig2.png)


Bei Organisationen mit einem Enterprise Agreement folgen Azure-Abonnements einer vierstufigen Hierarchie:

- Administrator für die Unternehmensregistrierung

- Abteilungsadministrator

- Kontobesitzer

- Dienstadministrator

Diese Hierarchie bestimmt Folgendes:

- Abrechnungsbeziehung

- Kontoverwaltung

- Rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) auf Artefakte

- Grenzen/Grenzwerte

- Grenzen

  - Nutzung und Abrechnung (Gebührenkarte basierend auf Anzahl der Angebote)

  - Einschränkungen

  - Virtuelles Netzwerk

- An 1 AAD angefügt (1 AAD kann mehreren Abonnements zugewiesen sein)

- Einem Konto für die Unternehmensregistrierung zugeordnet

## <a name="role-based-access-controls"></a>Rollenbasierte Zugriffssteuerung

Als Azure ursprünglich veröffentlicht wurde, waren Zugriffssteuerungen für ein Abonnement einfach: Administrator oder Co-Administrator. Durch den Zugriff auf ein Abonnement im klassischen Modell war Zugriff auf alle Ressourcen im Portal möglich. Dieses Fehlen einer präziseren Steuerung hat zur Zunahme von Abonnements geführt, um eine angemessene Zugriffssteuerung für eine Azure-Registrierung bereitzustellen.

![Rollenbasierte Zugriffssteuerung](./media/governance-in-azure/security-governance-in-azure-fig3.png)

Diese Zunahme von Abonnements ist nicht mehr erforderlich. Mit der rollenbasierten Zugriffssteuerung können Sie Benutzer zu Standardrollen (z.B. zu den allgemeinen Rollentypen „Leser“ und „Autor“) zuweisen. Sie können auch benutzerdefinierte Rollen definieren.

Die [rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) in Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) ermöglicht eine präzise Zugriffsverwaltung für Azure. Mit RBAC können Sie den Benutzern nur die Zugriffsrechte gewähren, die diese zum Ausführen ihrer Aufgaben benötigen. Sicherheitsorientierte Unternehmen müssen sich darauf konzentrieren, Mitarbeitern genau die Berechtigungen zuzuweisen, die sie benötigen. Zu viele Berechtigungen machen ein Konto zum leichten Angriffsziel. Durch nicht ausreichende Berechtigungen können Mitarbeiter nicht effizient arbeiten. Die rollenbasierte Zugriffssteuerung in Azure (Role-Based Access Control, RBAC) begegnet diesem Problem, indem sie eine präzise Zugriffsverwaltung für Azure ermöglicht. Mithilfe von RBAC können Sie Aufgaben in Ihrem Team verteilen und Benutzern nur den Zugriff gewähren, den sie zur Ausführung ihrer Aufgaben benötigen. Anstatt allen uneingeschränkte Berechtigungen in Ihrem Azure-Abonnement oder Ihren Ressourcen zu gewähren, können Sie die Berechtigungen auf bestimmte Aktionen beschränken.

Gestatten Sie z.B. mit RBAC einem Mitarbeiter die Verwaltung virtueller Computer in einem Abonnement, während ein anderer im gleichen Abonnement SQL-Datenbanken verwalten kann.

Azure RBAC verfügt über drei grundlegende Rollen, die für alle Ressourcentypen gelten:

- **Besitzer** verfügen über vollständigen Zugriff auf alle Ressourcen, einschließlich des Rechts, den Zugriff an andere Personen zu delegieren.

- **Mitwirkende** können alle Arten von Azure-Ressourcen erstellen und verwalten, aber keinen anderen Personen Zugriff gewähren.

- **Leser** können vorhandene Azure-Ressourcen anzeigen.

Die verbleibenden RBAC-Rollen in Azure ermöglichen die Verwaltung von bestimmten Azure-Ressourcen. Mit der Rolle „Mitwirkender von virtuellen Computern“ können Benutzer beispielsweise virtuelle Computer erstellen und verwalten. Sie haben damit keinen Zugriff auf das virtuelle Netzwerk oder das Subnetz, mit dem der virtuelle Computer verbunden ist.

[Integrierte RBAC-Rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) sind die Rollen aufgeführt, die in Azure verfügbar sind. Hierbei werden die Vorgänge und der Bereich angegeben, die mit jeder integrierten Rolle für Benutzer gewährt werden.

Gewähren Sie Zugriff, indem Sie Benutzern, Gruppen und Anwendungen die jeweils geeignete RBAC-Rolle für einen bestimmten Bereich zuweisen. Der Bereich einer Rollenzuweisung kann ein Abonnement, eine Ressourcengruppe oder eine einzelne Ressource sein. Mit einer Rolle, die einem übergeordneten Bereich zugewiesen ist, wird außerdem der Zugriff auf die darin enthaltenen untergeordneten Elemente gewährt.

Ein Benutzer mit Zugriff auf eine Ressourcengruppe kann beispielsweise alle Ressourcen verwalten, die darin enthalten sind, z.B. Websites, virtuelle Computer und Subnetze.

Azure RBAC unterstützt nur Verwaltungsvorgänge von Azure-Ressourcen im Azure-Portal und in den Azure Resource Manager-APIs. Azure RBAC kann nicht alle Vorgänge auf Datenebene für Azure-Ressourcen autorisieren. Sie können z.B. eine Person zum Verwalten von Speicherkonten autorisieren, jedoch nicht zum Verwalten von Blobs oder Tabellen innerhalb eines Speicherkontos. Ebenso kann eine SQL-Datenbank verwaltet werden, aber nicht die darin enthaltenen Tabellen.

Weitere Informationen dazu, wie RBAC Sie bei der Zugriffsverwaltung unterstützt, finden Sie unter [Get started with access management in the Azure portal](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is)(Erste Schritte mit der Zugriffsverwaltung im Azure-Portal).

Sie können auch eine [benutzerdefinierte Rolle](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles) in der rollenbasierten Zugriffssteuerung von Azure (Role-Based Access Control, RBAC) erstellen, falls keine der integrierten Rollen Ihre speziellen Zugriffsanforderungen erfüllt. Benutzerdefinierte Rollen können mit [Azure PowerShell](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-powershell), der [Azure-Befehlszeilenschnittstelle (CLI)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-azure-cli) und der [REST-API](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-rest) erstellt werden. Genau wie integrierte Rollen können auch benutzerdefinierte Rollen Benutzern, Gruppen und Anwendungen auf Abonnement-, Ressourcengruppen- und Ressourcenebene zugewiesen werden.

In jedem Abonnement können Sie bis zu 2.000 Rollenzuweisungen gewähren.

## <a name="resource-management"></a>Ressourcenverwaltung

In Azure war ursprünglich nur das klassische Bereitstellungsmodell verfügbar. In diesem Modell war jede Ressource unabhängig von den anderen vorhanden. Zusammengehörige Ressourcen konnten nicht gruppiert werden. Stattdessen mussten Sie manuell nachverfolgen, aus welchen Ressourcen die Lösung oder Anwendung bestand, und daran denken, sie mit einem koordinierten Ansatz zu verwalten.

Zum Bereitstellen einer Lösung mussten Sie entweder jede Ressource einzeln über das klassische Portal oder ein Skript erstellen, mit dem alle Ressourcen in der richtigen Reihenfolge bereitgestellt wurden. Beim Löschen einer Lösung mussten Sie jede Ressource einzeln löschen. Es war nicht einfach möglich, Richtlinien für die Zugriffssteuerung für zusammengehörige Ressourcen anzuwenden und zu aktualisieren. Außerdem gab es keinen Weg, Tags auf Ressourcen anzuwenden, um diese zur Unterstützung der Überwachung Ihrer Ressourcen und der Abrechnungsverwaltung mit Begriffen zu versehen.

Im Jahr 2014 wurden Resource Manager und das Konzept der Ressourcengruppen von Azure eingeführt. Eine Ressourcengruppe ist ein Container für Ressourcen, die über einen gemeinsamen Lebenszyklus verfügen. Das Resource Manager-Bereitstellungsmodell bietet verschiedene Vorteile:

- Sie müssen die Dienste für Ihre Lösung nicht mehr einzeln bearbeiten, sondern können sie als Gruppe bereitstellen, verwalten und überwachen.

- Sie können die Lösung während ihres Lebenszyklus wiederholt bereitstellen und sicher sein, dass Ihre Ressourcen einheitlich bereitgestellt werden.

- Sie können die Zugriffssteuerung auf alle Ressourcen der Ressourcengruppe anwenden. Diese Richtlinien werden automatisch angewendet, wenn der Ressourcengruppe neue Ressourcen hinzugefügt werden.

- Sie können Tags auf Ressourcen anwenden, um alle Ressourcen in Ihrem Abonnement logisch zu organisieren.

- Sie können JavaScript Object Notation (JSON) verwenden, um die Infrastruktur für Ihre Lösung zu definieren. Die JSON-Datei wird als Resource Manager-Vorlage bezeichnet.

- Sie können die Abhängigkeiten zwischen Ressourcen definieren, sodass diese in der richtigen Reihenfolge bereitgestellt werden.

![Ressourcenverwaltung](./media/governance-in-azure/security-governance-in-azure-fig4.png)

Mit Resource Manager können Sie Ressourcen in aussagekräftige Gruppen für Verwaltung, Abrechnung oder natürliche Affinität aufnehmen. Wie bereits erwähnt, weist Azure zwei Bereitstellungsmodelle auf. Im älteren [klassischen Modell](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model) war die grundlegende Verwaltungseinheit das Abonnement. Es war schwierig, Ressourcen innerhalb eines Abonnements aufzuschlüsseln. Dies hat zur Erstellung einer großen Anzahl von Abonnements geführt. Mit dem Resource Manager-Modell wurden auch Ressourcengruppen eingeführt.

Eine Ressourcengruppe ist ein Container, der verwandte Ressourcen für eine Azure-Lösung enthält. [Die Ressourcengruppe](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) kann alle Ressourcen für die Lösung oder nur die Ressourcen enthalten, die Sie als Gruppe verwalten möchten. Sie entscheiden in Abhängigkeit davon, was für Ihre Organisation am sinnvollsten ist, wie Sie die Ressourcen den Ressourcengruppen zuordnen möchten.

Empfehlungen zu Vorlagen finden Sie unter [Bewährte Methoden für das Erstellen von Azure Resource Manager-Vorlagen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices).

Azure Resource Manager analysiert Abhängigkeiten, um sicherzustellen, dass Ressourcen in der richtigen Reihenfolge erstellt werden. Wenn eine Ressource von einem Wert einer anderen Ressource abhängig ist (z.B. ein virtueller Computer, der ein Speicherkonto für Datenträger benötigt), legen Sie eine Abhängigkeit fest.

>[!Note]
>Weitere Informationen finden Sie unter [Definieren von Abhängigkeiten in Azure-Ressourcen-Manager-Vorlagen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-define-dependencies).

Sie können die Vorlage auch für Aktualisierungen der Infrastruktur verwenden. Beispielsweise können Sie Ihrer Lösung eine Ressource sowie Konfigurationsregeln für die Ressourcen hinzufügen, die bereits bereitgestellt wurden. Wenn in der Vorlage die Erstellung einer Ressource angegeben ist, diese aber bereits vorhanden ist, führt der Azure Resource Manager anstelle der Erstellung einer neuen Ressource eine Aktualisierung durch. Der Azure-Ressourcen-Manager aktualisiert die vorhandene Ressource auf den Zustand, der für eine neue Ressource gelten würde.

Der Ressourcen-Manager verfügt über Erweiterungen für Szenarios, in denen zusätzliche Vorgänge, wie z.B. das Installieren von Software, die nicht Teil des Setups ist, erforderlich sind.

## <a name="resource-tracking"></a>Ressourcennachverfolgung

Wenn Benutzer in Ihrer Organisation Ressourcen zum Abonnement hinzufügen, wird es zunehmend wichtig, die Ressourcen der entsprechenden Abteilung, dem Kunden und der Umgebung zuzuordnen. Sie können über Tags Metadaten an Ressourcen anfügen. Verwenden Sie [Tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags), um Informationen zur Ressource oder zum Besitzer bereitzustellen. Mit Tags können Sie Ressourcen nicht nur auf verschiedene Arten aggregieren und gruppieren, Sie können diese Daten auch für die verbrauchsbasierte Kostenzuteilung verwenden.

Die Verwendung von Tags (Markierungen) kann ratsam sein, wenn Sie über eine komplexe Sammlung von Ressourcengruppen und Ressourcen verfügen und diese Ressourcen auf möglichst sinnvolle Weise visualisieren müssen. Beispielsweise können Sie Ressourcen markieren, die in Ihrer Organisation eine ähnliche Funktion haben oder zu derselben Abteilung gehören.

Benutzer in Ihrer Organisation können mehrere Ressourcen erstellen, die ohne Tags später schwer zu identifizieren und zu verwalten sind. Es kann beispielsweise sein, dass Sie alle Ressourcen für ein Projekt löschen möchten. Wenn diese Ressourcen für das Projekt nicht mit Tags versehen sind, müssen Sie manuell danach suchen. Mit Tags lassen sich unter Umständen unnötige Kosten in Ihrem Abonnement vermeiden.

Ressourcen müssen sich nicht in derselben Ressourcengruppe befinden, um ein gemeinsames Tag aufzuweisen. Sie können Ihre eigene Tag-Taxonomie erstellen, um dafür zu sorgen, dass alle Benutzer in Ihrer Organisation die gleichen Tags verwenden. So wird verhindert, dass Benutzer versehentlich leicht unterschiedliche Tags nutzen (z. B. „dept“ anstelle von „department“).

Mithilfe von Ressourcenrichtlinien können Sie Standardregeln für Ihre Organisation erstellen. Sie können Richtlinien erstellen, mit denen sichergestellt wird, dass Ressourcen durch die entsprechenden Werte gekennzeichnet werden.

> [!Note]
> Weitere Informationen finden Sie unter [Anwenden von Ressourcenrichtlinien für Tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-policy-tags).

Sie haben auch die Möglichkeit, Ressourcen mit Tags über das Azure-Portal anzuzeigen.

Der [Nutzungsbericht](https://docs.microsoft.com/azure/billing/billing-understand-your-bill) für Ihr Abonnement enthält Tagnamen und -werte, sodass Sie die Kosten nach Tags aufschlüsseln können.

> [!Note]
> Weitere Informationen zu Tags finden Sie unter [Verwenden von Tags zum Organisieren von Azure-Ressourcen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags).

Für Tags gelten folgende Einschränkungen:

- Jede Ressource oder Ressourcengruppe kann maximal 15 Tagschlüssel-Wert-Paare haben. Diese Einschränkung gilt nur für Tags, die direkt auf die Ressourcengruppe oder die Ressource angewendet werden. Eine Ressourcengruppe kann zahlreiche Ressourcen mit jeweils 15 Tagschlüssel-Wert-Paaren enthalten.

- Der Tagname darf maximal 512 Zeichen lang sein.

- Der Tagwert darf maximal 256 Zeichen lang sein.

- Auf die Ressourcengruppe angewendete Tags werden nicht von den in dieser Ressourcengruppe enthaltenen Ressourcen geerbt.

Wenn Sie einer Ressource mehr als 15 Werte zuordnen müssen, verwenden Sie eine JSON-Zeichenfolge für den Tagwert. Die JSON-Zeichenfolge kann zahlreiche Werte enthalten, die auf einen einzelnen Tagschlüssel angewendet werden.

### <a name="tags-and-billing"></a>Tags und Abrechnung

Mithilfe von Tags können Sie Ihre Abrechnungsdaten gruppieren. Beispiel: Wenn Sie mehrere virtuelle Computer für verschiedene Organisationen betreiben, gruppieren Sie die Nutzung mithilfe von Tags nach Kostenstelle. Mit Tags können Sie auch Kosten nach Laufzeitumgebung kategorisieren (beispielsweise zur Abrechnung der Nutzung virtueller Computer in der Produktionsumgebung).

Informationen zu Tags können Sie über die [Azure-Ressourcennutzungs- und RateCard-APIs](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview) oder aus der Nutzungsdatei im CSV-Format abrufen. Sie laden die Nutzungsdatei aus dem [Azure-Kontenportal](https://account.windowsazure.com/) oder dem [EA-Portal](https://ea.azure.com/) herunter.

>[!Note]
> Weitere Informationen zum programmgesteuerten Zugriff auf Abrechnungsinformationen finden Sie unter [Gewinnen von Einblicken in den Ressourcenverbrauch unter Microsoft Azure](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview). Hinweise zu REST-API-Vorgängen finden Sie unter [Azure Billing REST API Reference (Preview)](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c)(in englischer Sprache).

Wenn Sie die CSV-Nutzungsdatei für Dienste herunterladen, die die Verwendung von Tags für die Abrechnung unterstützen, sind die Tags in der Spalte Tags enthalten.

## <a name="critical-resource-controls"></a>Wichtige Ressourcenkontrollen

Wenn Ihre Organisation dem Abonnement zentrale Dienste hinzufügt, wird es zunehmend wichtig sicherzustellen, dass diese Dienste verfügbar sind, um eine Unterbrechung des Geschäftsbetriebs zu vermeiden. [Ressourcensperren](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources) ermöglichen Ihnen, Vorgänge auf wertvollen Ressourcen einzuschränken, bei denen das Ändern oder Löschen einen erheblichen Einfluss auf Ihre Anwendungen oder die Cloudinfrastruktur hätte. Sie können Sperren auf ein Abonnement, eine Ressourcengruppe oder eine Ressource anwenden. Normalerweise wenden Sie Sperren auf grundlegende Ressourcen wie Speicherkonten, virtuelle Netzwerke und Gateways an.

Ressourcensperren unterstützen aktuell zwei Werte: CanNotDelete und ReadOnly. CanNotDelete bedeutet, dass Benutzer (mit den entsprechenden Berechtigungen) eine Ressource lesen oder ändern können, sie können sie jedoch nicht löschen. ReadOnly bedeutet, dass autorisierte Benutzer eine Ressource nicht löschen oder ändern können.

Resource Manager-Sperren gelten nur für Vorgänge auf der Verwaltungsebene (also für Vorgänge, die an <https://management.azure.com> gesendet werden). Die Ausführung ressourceneigener Funktionen wird durch die Sperren nicht begrenzt. Die Ressourcenänderungen sind eingeschränkt, die Ressourcenvorgänge jedoch nicht. So verhindert beispielsweise eine ReadOnly-Sperre für eine SQL-Datenbank-Instanz zwar, dass die Datenbank gelöscht oder angepasst wird, nicht aber das Erstellen, Aktualisieren oder Löschen von Daten in der Datenbank.

Die Anwendung von **ReadOnly** kann zu unerwarteten Ergebnissen führen, da einige Vorgänge, bei denen es sich scheinbar um Lesevorgänge handelt, zusätzliche Aktionen erfordern. Das Festlegen von **ReadOnly** für ein Speicherkonto hindert beispielsweise alle Benutzer am Auflisten der Schlüssel. Der Vorgang zum Auflisten von Schlüsseln wird über eine POST-Anforderung behandelt, da die zurückgegebenen Schlüssel für Schreibvorgänge zur Verfügung stehen.

![Wichtige Ressourcenkontrollen](./media/governance-in-azure/security-governance-in-azure-fig5.png)

Weiteres Beispiel: Das Festlegen von ReadOnly für eine App Service-Ressource verhindert, dass der Server-Explorer von Visual Studio Dateien für die Ressource anzeigen kann, da für diese Interaktion Schreibzugriff erforderlich ist.

Im Gegensatz zur rollenbasierten Zugriffssteuerung verwenden Sie Verwaltungssperren, um eine Einschränkung für alle Benutzer und Rollen zu aktivieren. Informationen zum Festlegen von Benutzer- und Rollenberechtigungen finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure).

Wenn Sie eine Sperre in einem übergeordneten Bereich anwenden, erben alle Ressourcen in diesem Bereich die entsprechende Sperre. Auch Ressourcen, die Sie später hinzufügen, erben die Sperre aus dem übergeordneten Element. Die restriktivste Sperre in der Vererbung hat Vorrang.

Zum Erstellen oder Löschen von Verwaltungssperren müssen Sie Zugriff auf Aktionen vom Typ Microsoft.Authorization/ oder _Microsoft.Authorization/locks/_ haben. Unter den integrierten Rollen können nur **Besitzer** und **Benutzerzugriffsadministrator** diese Aktionen ausführen.

## <a name="api-access-to-billing-information"></a>API-Zugriff auf Abrechnungsinformationen

Verwenden Sie Azure-Abrechnungs-APIs, um Nutzungs- und Ressourcendaten mittels Pull in Ihre bevorzugten Datenanalysetools abzurufen. Mithilfe der Azure-Ressourcennutzungs- und -Gebührenkarten-APIs können Sie Ihre Kosten genau vorhersagen und verwalten. Die APIs werden als Ressourcenanbieter implementiert und sind Teil der API-Familie, die von Azure Resource Manager verfügbar gemacht wird.

### <a name="azure-resource-usage-api-preview"></a>Azure-API zur Ressourcennutzung (Preview)

Mit der [Azure-Ressourcennutzungs-API](https://msdn.microsoft.com/library/azure/mt219003) können Sie Ihre geschätzten Azure-Nutzungsdaten abrufen. Die API umfasst:

- **Rollenbasierte Zugriffssteuerung für Azure**: Sie können Ihre Zugriffsrichtlinien im [Azure-Portal](https://portal.azure.com/) oder mit [Azure PowerShell-Cmdlets](https://docs.microsoft.com/powershell/azure/overview) konfigurieren, um festzulegen, welche Benutzer oder Anwendungen Zugriff auf die Nutzungsdaten eines Abonnements erhalten. Aufrufer müssen zur Authentifizierung standardmäßige Azure Active Directory-Token verwenden. Ordnen Sie dem Aufrufer die Rolle „Abrechnungsleser“, „Leser“, „Besitzer“ oder „Mitwirkender“ zu, um Zugriff auf die Nutzungsdaten für ein bestimmtes Azure-Abonnement zu erhalten.

- **Stündliche oder tägliche Aggregationen** – Aufrufer können angeben, ob sie ihre Azure-Nutzungsdaten in stündlichen Buckets oder täglichen Buckets erhalten möchten. Die Standardeinstellung ist „Täglich“.

- **Instanzmetadaten (inkl. Ressourcentags)**: Details der Instanzebene, z.B. der vollqualifizierte Ressourcen-URI „(/subscriptions/{subscription-id}/..)“, Ressourcengruppeninformationen und Ressourcentags. Mit diesen Metadaten können Sie die Nutzung auf deterministische und programmgesteuerte Weise anhand der Tags zuordnen, z.B. bei der internen Verrechnung.

- **Ressourcenmetadaten** – Ressourcendetails (z.B. Meter Name, Meter Category, Meter Sub-Category, Unit und Region) lassen den Aufrufer besser verstehen, was genutzt wurde. Außerdem arbeiten wir an einer Vereinheitlichung der Terminologie für Ressourcenmetadaten im Azure-Portal, in der CSV-Datei zur Azure-Nutzung, in der CSV-Datei zur EA-Abrechnung und in anderen öffentlich zugänglichen Benutzeroberflächen, um Ihnen die Korrelation von Daten über verschiedene Umgebungen hinweg zu ermöglichen.

- **Nutzung für alle Angebotstypen** – Nutzungsdaten sind für alle Angebotstypen verfügbar, z.B. „Nutzungsbasierte Bezahlung“, „MSDN“, „Verbindliche Zusage“, „Guthaben“ und „EA“.

**Azure-API für Ressourcengebührenkarte (Preview)**

Mit der Azure-Ressourcengebührenkarten-API (Resource RateCard) können Sie die Liste der verfügbaren Azure-Ressourcen sowie jeweils die geschätzten Preise abrufen. Die API umfasst:

- **Rollenbasierte Zugriffssteuerung für Azure:** Sie können Ihre Zugriffsrichtlinien im Azure-Portal oder mit Azure PowerShell-Cmdlets konfigurieren, um festzulegen, welche Benutzer oder Anwendungen Zugriff auf die Daten der Gebührenkarte erhalten. Aufrufer müssen zur Authentifizierung standardmäßige Azure Active Directory-Token verwenden. Ordnen Sie dem Aufrufer die Rolle „Leser“, „Besitzer“ oder „Mitwirkender“ zu, um Zugriff auf die Nutzungsdaten für ein bestimmtes Azure-Abonnement zu erhalten.

- **Unterstützung für die Angebote „Nutzungsbasierte Bezahlung“, „MSDN“, „Verbindliche Zusage“ und „Guthaben“ („EA“ wird nicht unterstützt)** – Mit dieser API werden Preisinformationen der Azure-Angebotsebene bereitgestellt. Aufrufer dieser API müssen die Angebotsinformationen übergeben, um Ressourcendetails und -preise abzurufen. Wir können derzeit keine EA-Preise angeben, da EA-Angebote über angepasste Preise pro Registrierung verfügen. Hier sind einige Szenarios angegeben, die aufgrund der Kombination aus Nutzungs- und Gebührenkarten-APIs möglich sind:

- **Azure-Ausgaben während des Monats** – Setzen Sie die Nutzungs- und Gebührenkarten-APIs zusammen ein, um einen besseren Einblick in ihre Cloudausgaben während eines Monats zu erhalten. Hierzu analysieren sie die stündlichen und täglichen Buckets für die Nutzung und die Gebührenschätzungen.

- **Einrichten von Warnungen**  – Verwenden Sie die Nutzungs- und Gebührenkarten-APIs, um die Verbrauchs- und Gebührenschätzungen für die Cloud abzurufen und ressourcenbasierte oder kostenbasierte Warnungen einzurichten.

- **Vorhersagen des Rechnungsbetrags** – Rufen Sie Ihren geschätzten Verbrauch und die geschätzten Cloudausgaben ab, und wenden Sie Machine Learning-Algorithmen an, um vorherzusagen, welcher Betrag sich am Ende des Abrechnungszeitraums ergibt.

- **Kostenanalyse vor der Nutzung**  – Mit der Gebührenkarten-API können Sie vorhersagen, wie hoch ihre Rechnung bei der erwarteten Nutzung wäre, wenn sie ihre Workloads auf Azure verlagern. Wenn Sie über vorhandene Workloads in anderen Clouds bzw. Private Clouds verfügen, können Sie die Nutzung auch den Azure-Preisen zuordnen, um eine bessere Schätzung der Azure-Ausgaben zu erhalten. Diese Schätzungen bieten die Pivot-Funktion für ein Angebot an und ermöglichen Ihnen, verschiedene Angebotstypen über die nutzungsbasierte Bezahlung hinaus zu vergleichen und gegenüberzustellen, z.B. verbindliche Zusage und Guthaben. Außerdem können Sie mit den APIs Kostenunterschiede nach Region anzeigen und eine Was-wäre-wenn-Analyse durchführen, um Bereitstellungsentscheidungen zu erleichtern.

- **Was-wäre-wenn-Analyse:** Sie können ermitteln, ob es kostengünstiger ist, Ihre Workloads in einer anderen Region oder unter einer anderen Konfiguration der Azure-Ressource auszuführen. Azure-Ressourcenkosten können je nach der Azure-Region variieren, in der sie ausgeführt werden.

- Außerdem können Sie ermitteln, ob ein anderer Azure-Angebotstyp zu einem besseren Preis für eine Azure-Ressource führt.

## <a name="networking-controls"></a>Netzwerkkontrollen

Der Zugriff auf Ressourcen kann intern (innerhalb des Unternehmensnetzwerks) oder extern (über das Internet) erfolgen. Für Benutzer in Ihrer Organisation ist es einfach, versehentlich Ressourcen an der falschen Stelle zu platzieren und so böswilligen Zugriff darauf zu ermöglichen. Wie bei lokalen Geräten müssen Unternehmen die entsprechende Kontrolle implementieren, um sicherzustellen, dass Azure-Benutzer die richtigen Entscheidungen treffen.

![Netzwerkkontrollen](./media/governance-in-azure/security-governance-in-azure-fig6.png)

Für die Abonnementgovernance wurden Hauptressourcen identifiziert, die eine grundlegende Kontrolle des Zugriffs bieten. Zu diesen Hauptressourcen gehören:

### <a name="network-connectivity"></a>Netzwerkverbindung

[Virtuelle Netzwerke](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) sind Containerobjekte für Subnetze. Obwohl dies nicht zwingend notwendig ist, werden sie häufig beim Verbinden von Anwendungen mit internen Unternehmensressourcen verwendet. Mit dem Dienst für das virtuelle Azure-Netzwerk (Azure Virtual Network) können Sie Azure-Ressourcen über virtuelle Netzwerke (VNets) sicher miteinander verbinden.

Ein VNet ist eine Darstellung Ihres eigenen Netzwerks in der Cloud. Bei einem VNet handelt es sich um eine logische Isolation von der dedizierten Azure-Cloud für Ihr Abonnement. Sie können VNets auch mit Ihrem lokalen Netzwerk verbinden.

Folgendes sind Funktionen für virtuelle Azure-Netzwerke:

- **Isolation**: VNets sind voneinander isoliert. Sie können separate VNets für Entwicklung, Tests und Produktion erstellen, die die gleichen CIDR-Adressblöcke verwenden. Im Gegensatz dazu können Sie mehrere VNets erstellen, für die unterschiedliche CIDR-Adressblöcke verwendet werden, und die Netzwerke verbinden. Sie können ein VNet in mehrere Subnetze segmentieren. Azure ermöglicht die interne Namensauflösung für VMs und Cloud Services-Rolleninstanzen, die mit einem VNet verbunden sind. Sie können ein VNet optional so konfigurieren, dass Ihre eigenen DNS-Server verwendet werden, anstatt die interne Namensauflösung von Azure zu nutzen.

- **Internetkonnektivität**: Für alle Azure Virtual Machines (VMs) und Cloud Services-Rolleninstanzen, die mit einem VNet verbunden sind, besteht standardmäßig Zugriff auf das Internet. Bei Bedarf können Sie auch den Zugriff in eingehender Richtung auf bestimmte Ressourcen ermöglichen.

- **Konnektivität von Azure-Ressourcen**: Azure-Ressourcen, z.B. Cloud Services und VMs, können mit demselben VNet verbunden werden. Für die Verbindung miteinander können für die Ressourcen auch dann private IP-Adressen verwendet werden, wenn sie sich in unterschiedlichen Subnetzen befinden. Azure verfügt über Standardrouting zwischen Subnetzen, VNets und lokalen Netzwerken, sodass Sie keine Routen konfigurieren und verwalten müssen.

- **VNet-Konnektivität**: VNets können miteinander verbunden werden, sodass Ressourcen, für die eine Verbindung mit einem VNet besteht, mit Ressourcen in einem anderen VNet kommunizieren können.

- **Lokale Konnektivität**: VNets können mit lokalen Netzwerken verbunden werden, indem private Netzwerkverbindungen zwischen Ihrem Netzwerk und Azure oder Site-to-Site-VPN-Verbindungen über das Internet verwendet werden.

- **Filterung von Datenverkehr**: Netzwerkdatenverkehr von VM- und Cloud Services-Rolleninstanzen kann in eingehender und ausgehender Richtung nach IP-Quelladresse und -port, IP-Zieladresse und -port und Protokoll gefiltert werden.

- **Routing**: Sie können das Standardrouting von Azure optional außer Kraft setzen, indem Sie Ihre eigenen Routen konfigurieren oder BGP-Routen über ein Netzwerkgateway verwenden.

## <a name="network-access-controls"></a>Kontrolle des Netzwerkzugriffs

[Netzwerksicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) ähneln einer Firewall und bieten Regeln dafür, wie eine Ressource über das Netzwerk „kommunizieren“ kann. Mit ihnen können Sie präzise steuern, wie/ob ein Subnetz (oder ein virtueller Computer) eine Verbindung mit dem Internet oder anderen Subnetzen im gleichen virtuellen Netzwerk herstellen kann.

Eine Netzwerksicherheitsgruppe (NSG) enthält eine Liste mit Sicherheitsregeln, mit denen Netzwerkdatenverkehr für Ressourcen, die mit virtuellen Azure-Netzwerken (VNet) verbunden sind, zugelassen oder abgelehnt wird. NSGs können Subnetzen, einzelnen VMs (klassisch) oder einzelnen Netzwerkschnittstellen (NICs), die mit VMs (Resource Manager) verbunden sind, zugeordnet werden.

Wenn eine NSG einem Subnetz zugeordnet ist, gelten die Regeln für alle Ressourcen, die mit dem Subnetz verbunden sind. Der Datenverkehr kann weiter eingeschränkt werden, indem eine NSG außerdem einer VM oder NIC zugeordnet wird.

## <a name="security-and-continuous-compliance-with-organizational-standards"></a>Sicherheit und kontinuierliche Konformität mit den Standards der Organisation

Jedes Unternehmen hat unterschiedliche Anforderungen, und jedes Unternehmen profitiert unterschiedlich von Cloud-Lösungen. Dennoch haben Kunden jeder Art die gleichen grundlegenden Bedenken über den Wechsel zur Cloud. Sie fragen sich, wie Sie die Kontrolle über Ihre Daten behalten, und Sie möchten die Daten sicher und privat aufbewahren, während Transparenz und Kompatibilität beibehalten werden sollen.

Kunden wünschen sich Folgendes von ihren Cloud-Anbietern:

- **Sichern unserer Daten**. Obwohl IT-Experten anerkennen, dass die Cloud die Datensicherheit und administrative Steuerung verbessern kann, haben sie immer noch Bedenken, dass ein Wechsel zur Cloud sie anfälliger für Hacker macht als ihre aktuellen internen Lösungen.

- **Geheimhaltung von Daten**. Clouddienste werfen besondere Herausforderungen für Unternehmen im Hinblick auf den Datenschutz auf. Während Unternehmen die Cloud als Möglichkeit in Betracht ziehen, Infrastrukturkosten zu sparen und ihre Flexibilität zu verbessern, haben sie gleichzeitig Bedenken, die Kontrolle darüber zu verlieren, wo die Daten gespeichert werden, wer auf sie zugreift und wie sie verwendet werden.

- **Unsere Kontrolle beizubehalten**. Auch wenn Unternehmen von der Bereitstellung innovativerer Lösungen mit der Cloud profitieren, sind sie sehr besorgt über den Verlust ihrer Daten. Aktuelle Enthüllungen, dass Behörden mit legalen und anderen Mitteln auf Kundendaten zugreifen, lassen manche CIOs vor dem Speichern ihrer Daten in der Cloud zurückschrecken.

- **Förderung der Transparenz** Während Sicherheit, Privatsphäre und Kontrolle wichtig für alle Entscheidungsträger in der Geschäftswelt sind, möchten sie auch die Möglichkeit haben, unabhängig zu überprüfen, wie ihre Daten gespeichert und gesichert werden und wie darauf zugegriffen wird.

- **Einhalten von Richtlinien**. Während Unternehmen ihre Nutzung von Cloudtechnologien erweitern, entwickeln sich Komplexität und Umfang von Standards und Richtlinien stetig weiter. Unternehmen müssen sicher sein, dass ihre Kompatibilitätsstandards eingehalten werden und sich die Kompatibilität weiterentwickelt während sich Richtlinien mit der Zeit ändern.

## <a name="security-configuration-monitoring-and-alerting"></a>Konfiguration der Sicherheit, Überwachung und Warnung

Azure-Abonnenten können ihre Cloudumgebungen über verschiedene Geräte verwalten. Hierzu zählen etwa Arbeitsstationen für die Verwaltung, Entwickler-PCs und sogar geeignete Endbenutzergeräte, die über die aufgabenspezifischen Berechtigungen verfügen. In einigen Fällen werden Administratorfunktionen über webbasierte Konsolen ausgeführt, z.B. das Azure-Portal. In anderen Fällen können auch direkte Verbindungen mit Azure von lokalen Systemen über Virtual Private Networks (VPNs), Terminal Services, Clientanwendungsprotokolle oder (programmgesteuert) die Azure-Dienstverwaltungs-API (SMAPI) bestehen. Außerdem können Clientendpunkte entweder Mitglied einer Domäne oder isoliert und unverwaltet sein, z.B. Tablets oder Smartphones.

Dank mehrerer Zugriffs- und Verwaltungsfunktionen stehen zwar vielfältige Optionen zur Verfügung, diese Vielfalt kann aber auch ein erhebliches Risiko für eine Cloudbereitstellung darstellen. Administrative Aktionen lassen sich dadurch unter Umständen schwerer verwalten, nachvollziehen und überprüfen. Diese Variabilität kann auch mit Sicherheitsbedrohungen verbunden sein, wenn nicht regulierter Zugriff auf Clientendpunkte besteht, die zum Verwalten von Clouddiensten verwendet werden. Die Verwendung allgemeiner oder persönlicher Arbeitsstationen zum Entwickeln und Verwalten einer Infrastruktur geht mit unberechenbaren Bedrohungsvektoren einher. Hierzu zählen beispielsweise das Surfen im Web (etwa Watering Hole-Angriffe) oder E-Mails (etwa Social Engineering und Phishing).

Die Überwachung, die Protokollierung und das Auditing sind eine Basis für das Nachverfolgen und Verstehen von Verwaltungsaktivitäten. Aufgrund der Menge der generierten Daten ist es unter Umständen aber nicht immer möglich, alle Aktionen bis ins kleinste Detail zu überwachen und zu überprüfen. Das Überprüfen der Effektivität von Verwaltungsrichtlinien ist aber eine bewährte Methode.

Azure-Sicherheitsbestimmungen von AD DS-Gruppenrichtlinienobjekten, um alle Windows-Schnittstellen der Administratoren, z.B. die Dateifreigabe, zu kontrollieren. Binden Sie Verwaltungsarbeitsstationen in die Prozesse der Bereiche Auditing, Überwachung und Protokollierung ein. Verfolgen Sie für Administratoren und Entwickler den gesamten Zugriff und die Nutzung.

### <a name="azure-security-center"></a>Azure Security Center

Das [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) bietet einen zentralen Überblick über den Sicherheitsstatus von Ressourcen in den Abonnements und gibt Empfehlungen, mit deren Hilfe die Gefährdung von Ressourcen verhindert wird. Es kann detailliertere Richtlinien ermöglichen (z.B. Richtlinien auf bestimmte Ressourcengruppen anwenden, mit denen das Unternehmen anpassen kann, wie es einem bestimmten Risiko ausgesetzt wird).

![Azure Security Center](./media/governance-in-azure/security-governance-in-azure-fig7.png)

Security Center bietet integrierte Sicherheitsüberwachung und Richtlinienverwaltung für Ihre Azure-Abonnements, hilft bei der Erkennung von Bedrohungen, die andernfalls möglicherweise unbemerkt bleiben, und kann gemeinsam mit einem breiten Spektrum an Sicherheitslösungen verwendet werden. Nach Aktivierung der [Sicherheitsrichtlinien](https://docs.microsoft.com/azure/security-center/security-center-policies) für die Ressourcen eines Abonnements analysiert Security Center die Sicherheit Ihrer Ressourcen, um mögliche Sicherheitsrisiken zu erkennen. Informationen zur Netzwerkkonfiguration sind sofort verfügbar.

Azure Security Center stellt eine Kombination aus bewährten Analysemethoden und Verwaltung der Sicherheitsrichtlinien für alle Ressourcen in einem Azure-Abonnement dar. Dieses leistungsfähige und einfach zu verwendende Tool ermöglicht Sicherheitsteams und Risikobeauftragten Sicherheitsrisiken vorzubeugen, diese zu erkennen und darauf zu reagieren. Es sammelt automatisch Sicherheitsdaten aus Ihren Azure-Ressourcen, dem Netzwerk und Partnerlösungen wie Antischadsoftwareprogrammen und Firewalls und analysiert diese.

Darüber hinaus werden erweiterte Analyselösungen angewendet, darunter Machine Learning und Verhaltensanalyse, während globale Bedrohungsinformationen (Threat Intelligence) von Microsoft-Produkten und -Diensten von der Microsoft Digital Crimes Unit (DCU), vom Microsoft Security Response Center (MSRC) sowie von externen Feeds genutzt werden. [Sicherheitsbestimmungen](https://www.credera.com/blog/credera-site/azure-governance-part-4-other-tools-in-the-toolbox/) können allgemein auf Abonnementebene angewendet werden oder auf spezifische präzise Anforderungen eingegrenzt werden, die über die Richtliniendefinition angewendet werden.

Abschließend analysiert Azure Security Center basierend auf diesen Richtlinien die Sicherheitsintegrität der Ressource und verwendet diese Analyse, um informative Dashboards und Warnungen für Ereignisse wie die Erkennung von Schadsoftware und schädlichen IP-Verbindungsversuchen bereitzustellen.

>[!Note]
> Weitere Informationen zur Anwendung der Empfehlungen finden Sie unter [Implementieren von Sicherheitsempfehlungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-recommendations).

Security Center sammelt Daten von Ihren virtuellen Computern, um den Sicherheitsstatus zu bewerten, Sicherheitsempfehlungen bereitzustellen und vor Bedrohungen zu warnen. Beim ersten Zugriff auf Security Center wird die Datensammlung für alle virtuellen Computer in Ihrem Abonnement aktiviert. Die Datensammlung wird zwar empfohlen, kann aber in der Security Center-Richtlinie [deaktiviert](https://docs.microsoft.com/azure/security-center/security-center-faq) werden.

Zudem ist das Azure Security Center eine offene Plattform, über die Microsoft-Partner und unabhängige Softwarehersteller Software erstellen können, die in das Azure Security Center eingebunden wird, um die Funktionen zu erweitern.

Azure Security Center überwacht die folgenden Azure-Ressourcen:

- Virtuelle Computer (Virtual Machines, VMs) (auch Clouddienste)

- Virtuelle Azure-Netzwerke

- Azure SQL-Dienst

- Partnerlösungen, die in Ihr Azure-Abonnement integriert sind, z.B. die Firewall einer Webanwendung auf virtuellen Computern und in einer [App Service-Umgebung](https://docs.microsoft.com/azure/app-service/app-service-app-service-environments-readme).

### <a name="operations-management-suite"></a>Operations Management Suite

Das Informationssicherheits- und [Governance-Programm](https://github.com/Microsoft/azure-docs/blob/master/articles/log-analytics/log-analytics-security.md) des OMS-Softwareentwicklungs- und Service-Teams unterstützt die geschäftlichen Anforderungen und entspricht Gesetzen und Vorschriften, wie unter [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) und [Microsoft Trust Center Compliance](https://www.microsoft.com/TrustCenter/Compliance/default.aspx) beschrieben. Außerdem wird beschrieben, wie OMS Sicherheitsanforderungen einrichtet, Sicherheitskontrollen identifiziert und Risiken verwaltet und überwacht. Richtlinien, Standards, Verfahren und Leitlinien werden jährlich überprüft.

Jedes Mitglied des OMS-Entwicklungsteams erhält eine formale Anwendungssicherheitsschulung. Intern verwenden wir ein Versionskontrollsystem für die Softwareentwicklung. Jedes Softwareprojekt wird durch das Versionskontrollsystem geschützt.

Microsoft hat ein Sicherheits- und Compliance-Team, das alle Microsoft-Dienste überwacht und bewertet. In diesem Team sind Information Security Officer, die nicht mit den Entwicklungsabteilungen verbunden sind, die OMS entwickeln. Die Sicherheitsbeauftragten verfügen über eine eigene Managementkette und führen unabhängige Beurteilungen von Produkten und Dienstleistungen durch, um Sicherheit und Compliance zu gewährleisten.

Bei Operations Management Suite (OMS) handelt es sich um eine Sammlung von Verwaltungsdiensten, die von Grund auf für die Cloud konzipiert wurden. Anstatt lokale Ressourcen bereitzustellen und zu verwalten, werden OMS-Komponenten vollständig in Azure gehostet. Der Konfigurationsaufwand ist minimal, und die Lösung ist innerhalb weniger Minuten einsatzbereit.

![Operations Manager Suite](./media/governance-in-azure/security-governance-in-azure-fig8.png)

Die Tatsache, dass OMS-Dienste in der Cloud ausgeführt werden, bedeutet nicht, dass sie nicht auch zur effektiven Verwaltung Ihrer lokalen Umgebung genutzt werden können.

Platzieren Sie einfach einen Agent auf einem beliebigen Windows- oder Linux-Computer in Ihrem Rechenzentrum. Dieser sendet dann Daten an Log Analytics, wo sie zusammen mit all den anderen Daten aus cloudbasierten oder lokalen Diensten analysiert werden. Verwenden Sie Azure Backup und Azure Site Recovery, um die Cloud für die Sicherung und hohe Verfügbarkeit lokaler Ressourcen zu nutzen.

Runbooks in der Cloud können üblicherweise nicht auf lokale Ressourcen zugreifen. Sie können jedoch ebenfalls einen Agent auf Computern installieren, um Runbooks in Ihrem Rechenzentrum zu hosten. Geben Sie beim Starten eines Runbooks einfach an, ob es in der Cloud oder für einen lokalen Worker ausgeführt werden soll.

Die Kernfunktionen von OMS werden durch eine Reihe von Diensten bereitgestellt, die in Azure ausgeführt werden. Jeder Dienst bietet eine bestimmte Verwaltungsfunktion, und Sie können Dienste miteinander kombinieren, um verschiedene Verwaltungsszenarien zu bewältigen.

![Operations Manager Suite](./media/governance-in-azure/security-governance-in-azure-fig9.JPG)

Azure Operations Manager erweitert die Funktionalität durch die Bereitstellung von Lösungen für die Verwaltung. [Verwaltungslösungen](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solutions) sind vorgefertigte Logiksätze zur Implementierung eines Verwaltungsszenarios unter Verwendung mindestens eines OMS-Diensts.

![Verwaltung von Azure-Vorgang](./media/governance-in-azure/security-governance-in-azure-fig10.png)

Von Microsoft und von Partnern werden verschiedene Lösungen angeboten, die Sie problemlos Ihrem Azure-Abonnement hinzufügen und dadurch den Nutzen Ihrer OMS-Investition erhöhen können.

Partner können eigene Lösungen zur Unterstützung ihrer Anwendungen und Dienste erstellen und sie Benutzern über den Azure Marketplace oder über Schnellstartvorlagen zur Verfügung stellen.

## <a name="performance-alerting-and-monitoring"></a>Warnungen und Überwachung der Leistung

### <a name="alerting"></a>Warnungen

Mithilfe von Warnungen können Sie Metriken, Ereignisse oder Protokolle zu Ihren Azure-Ressourcen überwachen und sich bei Eintreten bestimmter von Ihnen festgelegter Bedingungen benachrichtigen lassen.

**Warnungen bei unterschiedlichen Azure-Diensten**

Warnungen sind für verschiedene Dienste verfügbar, z.B. die folgenden:

- Application Insights: Ermöglicht Webtest- und Metrikwarnungen.

>[!Note]
> Informationen dazu finden Sie unter [Einrichten von Warnungen in Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-alerts) und [Überwachen der Verfügbarkeit und Reaktionsfähigkeit von Websites](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability).

- Log Analytics (Operations Management Suite): Ermöglicht die Weiterleitung von Aktivitäts- und Diagnoseprotokollen an Log Analytics. Die Operations Management Suite ermöglicht Metrik-, Protokollwarnungen und andere Typen von Warnungen.

>[!Note]
> Weitere Informationen finden Sie in [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts) unter „Warnungen“.

- Azure Monitor: Ermöglicht das Festlegen von Warnungen basierend auf Metrikwerten und Aktivitätsprotokollereignissen. Mit der [Azure Monitor-REST-API](https://msdn.microsoft.com/library/dn931943.aspx) können Sie Benachrichtigungen verwalten.

>[!Note]
> Weitere Informationen finden Sie unter [Nutzung des Azure-Portals, von PowerShell oder der Befehlszeilenschnittstelle zum Erstellen von Warnungen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-alerts-portal).

### <a name="monitoring"></a>Überwachung

Leistungsprobleme in Ihrer Cloud-App können Ihr Unternehmen beeinträchtigen. Durch mehrere miteinander verbundene Komponenten und häufige Versionswechsel können Leistungseinbußen jederzeit vorkommen. Bei der Entwicklung einer App werden von Ihren Benutzern in der Regel Probleme entdeckt, die Sie beim Testen nicht gefunden haben. Sie sollten sofort über diese Probleme Bescheid wissen und über Tools zu deren Diagnose und Beseitigung verfügen. Microsoft Azure umfasst eine Reihe von Tools zum Identifizieren dieser Probleme.

**Wie überwache ich meine Azure-Cloud-Apps?**

Es gibt eine Reihe von Tools zum Überwachen von Azure-Anwendungen und -Diensten. Einige ihrer Funktionen überschneiden sich. Dies hat teils historische Gründe und liegt teils auch an der verschwommenen Grenze zwischen der Entwicklung und dem Betrieb einer Anwendung.

Hier sind die wichtigsten Tools aufgeführt:

- **Azure Monitor** ist ein grundlegendes Tool zum Überwachen von Diensten, die in Azure ausgeführt werden. Dieses Tool bietet Ihnen Daten auf Infrastrukturebene über den Durchsatz eines Diensts und über dessen Umgebung. Wenn Sie Ihre Apps vollständig in Azure verwalten und entscheiden, ob Sie Ihre Ressourcen zentral hoch- oder herunterskalieren, bietet Azure Monitor Ihnen einen Ausgangspunkt.

- **Application Insights** kann für die Entwicklung und als Überwachungslösung für die Produktion verwendet werden. Dieses Tool installiert ein Paket in Ihrer App und bietet Ihnen damit eine interne Ansicht der Geschehnisse. Seine Daten umfassen die Antwortzeiten von Abhängigkeiten, Ausnahmeablaufverfolgungen, Debuggingmomentaufnahmen und Ausführungsprofilen. Das Tool stellt Ihnen leistungsstarke intelligente Tools zum Analysieren all dieser Telemetriedaten bereit, um Sie beim Debuggen einer App zu unterstützen und zu veranschaulichen, wie sie von den Benutzern genutzt wird. Sie können feststellen, ob eine Spitze in den Antwortzeiten auf ein Problem in einer App oder auf ein externes Ressourcenproblem zurückzuführen ist. Wenn Sie Visual Studio verwenden und die App fehlerhaft ist, können Sie direkt zu den problematischen Codezeilen gelangen und das Problem beheben.

- **Log Analytics** ist für Personen bestimmt, die die Leistung optimieren und die Wartung für Anwendungen planen müssen, die in der Produktionsumgebung ausgeführt werden. Dieses Tool basiert auf Azure. Es erfasst und aggregiert Daten aus vielen verschiedenen Quellen, wenn auch mit einer Verzögerung von 10 bis 15 Minuten. Es bietet eine ganzheitliche IT-Verwaltungslösung für Azure-Infrastrukturen, lokale Infrastrukturen und cloudbasierte Drittanbieterinfrastrukturen (z.B. Amazon Web Services). Es stellt umfassende Tools zur quellenübergreifenden Datenanalyse bereit, erlaubt komplexe Abfragen in allen Protokollen und kann beim Eintreten angegebener Bedingungen proaktiv Warnungen ausgeben. Sie können sogar benutzerdefinierte Daten im zugehörigen zentralen Repository sammeln, um sie abfragen und visualisieren zu können.

- **System Center Operations Manager (SCOM)** dient zur Verwaltung und Überwachung großer Cloudinstallationen. Möglicherweise kennen Sie dieses Tool bereits als Verwaltungstool für lokale Windows Server- und Hyper-V-basierte Clouds. Es kann jedoch auch in Azure-Apps integriert werden und diese verwalten. Unter anderem kann es Application Insights in vorhandenen Live-Apps installieren. Wenn eine App ausfällt, werden Sie innerhalb von Sekunden darauf hingewiesen.


## <a name="next-steps"></a>Nächste Schritte

- [Bewährte Methoden für das Erstellen von Azure Resource Manager-Vorlagen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices).

- [Beispiele für das Implementieren eines Azure-Unternehmensgerüsts](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-subscription-examples).

- [Microsoft Azure Government](https://docs.microsoft.com/azure/azure-government/).

