---
title: "Häufig gestellte Fragen zu Microsoft Azure Service Fabric | Microsoft-Dokumentation"
description: "Häufig gestellte Fragen zu Service Fabric und entsprechende Antworten"
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: 
ms.assetid: 5a179703-ff0c-4b8e-98cd-377253295d12
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: chackdan
ms.translationtype: HT
ms.sourcegitcommit: 847eb792064bd0ee7d50163f35cd2e0368324203
ms.openlocfilehash: ee334186dffaa1f88cf05717b6a5ba1e819a8cdc
ms.contentlocale: de-de
ms.lasthandoff: 08/19/2017

---


# <a name="commonly-asked-service-fabric-questions"></a>Häufig gestellte Fragen zu Service Fabric

Es gibt viele häufig gestellte Fragen zu den Funktionen und zur Verwendung von Service Fabric. In diesem Dokument werden viele dieser häufig gestellten Fragen und die zugehörigen Antworten behandelt.

## <a name="cluster-setup-and-management"></a>Clustereinrichtung und -verwaltung

### <a name="can-i-create-a-cluster-that-spans-multiple-azure-regions-or-my-own-datacenters"></a>Kann ich einen Cluster erstellen, der mehrere Azure-Regionen oder eigene Datencenter umfasst?

Ja. 

Mit der Service Fabric-Kernclusteringtechnologie können Computer an beliebigen Orten auf der ganzen Welt verknüpft werden, solange diese über ein Netzwerk miteinander verbunden sind. Die Erstellung und Ausführung eines solchen Clusters kann jedoch komplex sein.

Wenn Sie sich für dieses Szenario interessieren, wird empfohlen, entweder über die [Liste mit Service Fabric-Problemen auf Github ](https://github.com/azure/service-fabric-issues) den Kontakt herzustellen oder sich an einen Supportmitarbeiter zu wenden, um weitere Anleitungen zu erhalten. Das Service Fabric-Team arbeitet an der Bereitstellung zusätzlicher Anleitungen und Empfehlungen für dieses Szenario. 

Einige Aspekte, die zu berücksichtigen sind: 

1. Die Service Fabric-Clusterressource in Azure ist derzeit regional, ebenso wie die VM-Skalierungsgruppen, auf denen der Cluster beruht. Das bedeutet, dass Sie bei einem regionalen Ausfall den Cluster nicht mehr über Azure Resource Manager oder das Azure-Portal verwalten können. Dieser Fall kann eintreten, auch wenn der Cluster weiterhin ausgeführt wird und Sie direkt mit ihm interagieren können. Darüber hinaus bietet Azure heute nicht die Möglichkeit, ein einzelnes virtuelles Netzwerk einzurichten, das in alle Regionen verwendet werden kann. Das bedeutet, dass für einen Cluster mit mehreren Regionen in Azure entweder [öffentliche IP-Adressen für jeden virtuellen Computer in den VM-Skalierungsgruppen](../virtual-machine-scale-sets/virtual-machine-scale-sets-networking.md#public-ipv4-per-virtual-machine) oder [Azure-VPN-Gateways](../vpn-gateway/vpn-gateway-about-vpngateways.md) erforderlich sind. Diese Netzwerkoptionen haben unterschiedliche Auswirkungen auf Kosten, Leistung und – in gewissem Maße – auf das Anwendungsdesign. Daher ist vor der Bereitstellung einer solchen Umgebung eine sorgfältige Analyse und Planung erforderlich.
2. Wartung, Verwaltung und Überwachung dieser Computer können kompliziert werden, insbesondere, wenn sie sich über verschiedene _Arten_ von Umgebungen erstrecken (beispielsweise zwischen verschiedenen Cloudanbietern oder zwischen lokalen Ressourcen und Azure). Machen Sie sich daher unbedingt mit Upgrades, Überwachung, Verwaltung und Diagnose für Cluster und Anwendungen vertraut, bevor Sie Produktionsworkloads in einer solchen Umgebung ausführen. Wenn Sie bei der Behebung derartiger Probleme in Azure oder Ihren eigenen Datencentern bereits viel Erfahrung haben, können die gleichen Lösungsansätze wahrscheinlich auch beim Erstellen oder Ausführung des Service Fabric-Clusters angewendet werden. 

### <a name="do-service-fabric-nodes-automatically-receive-os-updates"></a>Erhalten Service Fabric-Knoten automatisch Betriebssystemupdates?

Heute noch nicht, aber auch dies ist eine häufig gestellte Anforderung, die Azure umsetzen möchte.

In der Zwischenzeit haben wir eine [Anwendung](service-fabric-patch-orchestration-application.md) zum Patchen und Aktualisieren der Betriebssysteme bereitgestellt, die Ihren SF-Knoten zugrunde liegen.

Die Herausforderung von Betriebssystemupdates besteht darin, dass dafür in der Regel ein Neustart des Computers erforderlich ist, was zu einer vorübergehenden Unterbrechung der Verfügbarkeit führt. Dies ist für sich gesehen noch kein Problem, weil Service Fabric Datenverkehr für diese Dienste automatisch an andere Knoten umleitet. Wenn Betriebssystemupdates jedoch nicht über den Cluster koordiniert werden, besteht die Gefahr, dass viele Knoten gleichzeitig ausfallen. Diese gleichzeitigen Neustarts können für einen Dienst oder zumindest für eine bestimmte Partition (bei einem zustandsbehafteten Dienst) zum vollständigen Verlust der Verfügbarkeit führen.

Wir planen die zukünftige Unterstützung einer Betriebssystemupdate-Richtlinie, die Updatedomänen-übergreifend vollständig automatisiert und koordiniert ist und sicherstellt, dass die Verfügbarkeit trotz Neustarts und anderen unerwarteten Fehlern beibehalten wird.

### <a name="can-i-use-large-virtual-machine-scale-sets-in-my-sf-cluster"></a>Kann ich große VM-Skalierungsgruppen in meinem SF-Cluster verwenden? 

**Kurze Antwort**: Nein. 

**Lange Antwort:** Obwohl Sie dank großer VM-Skalierungsgruppen eine VM-Skalierungsgruppe mit bis zu 1.000 VM-Instanzen skalieren können, werden dazu Platzierungsgruppen (PGs) verwendet. Fehlerdomänen (FDs) und Upgradedomänen (UDs) sind nur innerhalb einer Platzierungsgruppe konsistent. Service Fabric verwendet FDs und UDs für Platzierungsentscheidungen, die Ihre Dienstreplikate/Dienstinstanzen betreffen. Da die FDs und UDs nur innerhalb einer Platzierungsgruppe vergleichbar sind, kann SF sie nicht verwenden. Wenn beispielsweise VM1 in PG1 über eine Topologie FD=0 und VM9 in PG2 über eine Topologie FD=4 verfügt, bedeutet dies nicht, dass VM1 und VM2 sich in zwei verschiedenen Hardwareracks befinden, daher kann SF die FD-Werte in diesem Fall nicht für Platzierungsentscheidungen verwenden.

Es gibt derzeit andere Probleme mit großen VM-Skalierungsgruppen, z.B. den Mangel an Unterstützung von Ebene-4-Lastenausgleich. Weitere Informationen finden Sie unter [Näheres zu großen Skalierungsgruppen](../virtual-machine-scale-sets/virtual-machine-scale-sets-placement-groups.md).



### <a name="what-is-the-minimum-size-of-a-service-fabric-cluster-why-cant-it-be-smaller"></a>Was ist die Mindestgröße für einen Service Fabric-Cluster? Warum darf er nicht kleiner sein?

Die unterstützte Mindestgröße für einen Service Fabric-Cluster, auf dem Produktionsworkloads ausgeführt werden, beträgt fünf Knoten. Für Entwicklungs-/Testszenarios unterstützen wir Cluster mit drei Knoten.

Diese Mindestwerte gelten, weil auf dem Service Fabric-Cluster ein Satz zustandsbehafteter Systemdienste ausgeführt werden, darunter der Naming Service und der Failover-Manager. Diese Dienste verfolgen, welche Dienste auf dem Cluster bereitgestellt wurden und wo sie zurzeit gehostet werden, und sind von starker Konsistenz abhängig. Diese starke Konsistenz ist wiederum von der Möglichkeit abhängig, ein *Quorum* für jedes Update am Zustand dieser Dienste abzurufen, wobei ein Quorum die strikte Mehrheit der Replikate (N/2 + 1) für einen bestimmten Dienst darstellt.

Untersuchen wir vor diesem Hintergrund einige mögliche Clusterkonfigurationen:

**Ein Knoten**: Diese Option bietet keine hohe Verfügbarkeit, da jeglicher Verlust dieses einzigen Knotens den Verlust des gesamten Clusters bedeutet.

**Zwei Knoten**: Ein Quorum für einen Dienst, der über zwei Knoten bereitgestellt wird (N = 2), ist 2 (2/2 + 1 = 2). Wenn ein einzelnes Replikat verloren geht, ist es unmöglich, ein Quorum zu erstellen. Da es zum Ausführen eines Dienstupgrades erforderlich ist, ein Replikat vorübergehend außer Betrieb zu nehmen, ist dies keine nützlich Konfiguration.

**Drei Knoten**: Mit drei Knoten (N = 3) liegt die Anforderung zum Erstellen eines Quorums noch immer bei zwei Knoten (3/2 + 1 = 2). Dies bedeutet, dass Sie einen einzelnen Knoten verlieren und weiterhin ein Quorum beibehalten können.

Die Clusterkonfiguration mit drei Knoten wird für Dev/Test unterstützt, da Sie sicher Upgrades ausführen und den Ausfall einzelner Knoten überstehen können, solange sie nicht gleichzeitig ausfallen. Für Produktionsworkflows müssen Sie solche gleichzeitigen Ausfälle abfedern können, daher sind fünf Knoten erforderlich.

### <a name="can-i-turn-off-my-cluster-at-nightweekends-to-save-costs"></a>Kann ich meinen Cluster am Abend/Wochenende abschalten, um Kosten zu sparen?

Im Allgemeinen nicht. Service Fabric speichert den Zustand auf lokalen, kurzlebigen Festplatten. Dies bedeutet beim Verschieben des virtuellen Computers auf einen anderen Host, dass die Daten nicht mit verschoben werden. Im normalen Betrieb ist das kein Problem, da der neue Knoten von anderen Knoten auf den neuesten Stand gebracht wird. Wenn Sie jedoch alle Knoten beenden und später neu starten, besteht die erhebliche Möglichkeit, dass die meisten der Knoten auf neuen Hosts gestartet werden, sodass das System nicht wiederhergestellt werden kann.

Wenn Sie Cluster erstellen möchten, um Ihre Anwendung vor der Bereitstellung zu testen, empfiehlt es sich, diese Cluster dynamisch als Teil Ihrer [Continuous Integration/Continuous Deployment-Pipeline](service-fabric-set-up-continuous-integration.md) zu erstellen.


### <a name="how-do-i-upgrade-my-operating-system-for-example-from-windows-server-2012-to-windows-server-2016"></a>Wie aktualisiere ich mein Betriebssystem (z.B. von Windows Server 2012 auf Windows Server 2016)?

Wir arbeiten an einer optimierten Lösung, derzeit müssen Sie jedoch das Upgrade durchführen. Sie müssen das Betriebssystemimage nacheinander auf den einzelnen virtuellen Computern des Clusters aktualisieren. 

## <a name="container-support"></a>Containerunterstützung

### <a name="why-are-my-containers-that-are-deployed-to-sf-unable-to-resolve-dns-addresses"></a>Warum können meine Container, die in SF bereitgestellt werden, keine DNS-Adressen auflösen?

Dieses Problem wurde für Cluster gemeldet, die Version 5.6.204.9494 verwenden. 

**Abhilfemaßnahmen:** Befolgen Sie die Angaben in [diesem Dokument](service-fabric-dnsservice.md), um den DNS-Service Fabric-Dienst in Ihrem Cluster zu aktivieren.

**Behebung:** Führen Sie ein Upgrade auf eine unterstützte Clusterversion über 5.6.204.9494 durch, sobald diese verfügbar ist. Wenn für Ihren Cluster automatische Upgrades festgelegt wurden, wird der Cluster automatisch auf die Version aktualisiert, in der dieses Problem behoben wurde.

  
## <a name="application-design"></a>Anwendungsentwurf

### <a name="whats-the-best-way-to-query-data-across-partitions-of-a-reliable-collection"></a>Was ist die beste Möglichkeit, Daten über Partitionen einer zuverlässigen Sammlung hinweg abzufragen?

Zuverlässige Sammlungen sind in der Regel [partitioniert](service-fabric-concepts-partitioning.md), um eine horizontale Hochskalierung für bessere Leistung und einen verbesserten Durchsatz zu ermöglichen. Das bedeutet, dass der Zustand für einen bestimmten Dienst über Dutzende oder Hunderte von Computern verteilt sein kann. Um Vorgänge über dieses vollständige Dataset auszuführen, stehen Ihnen einige Optionen zur Verfügung:

- Erstellen Sie einen Dienst, der alle Partitionen eines anderen Diensts abfragt, um die erforderlichen Daten abzurufen.
- Erstellen Sie einen Dienst, der Daten von allen Partitionen eines anderen Diensts empfangen kann.
- Verschieben Sie Daten aus den einzelnen Diensten regelmäßig mithilfe von Push in einen externen Speicher. Dieser Ansatz ist nur geeignet, wenn die von Ihnen durchgeführten Abfragen nicht Teil Ihrer Kerngeschäftslogik sind.


### <a name="whats-the-best-way-to-query-data-across-my-actors"></a>Welche Möglichkeit bietet sich an, um Daten akteurübergreifend abzufragen?

Akteure sind als unabhängige Einheiten von Zustand und Compute vorgesehen, daher wird davon abgeraten, zur Laufzeit umfassende Abfragen des Akteurzustands durchzuführen. Wenn Sie eine Abfrage über den vollständigen Satz von Akteurzuständen durchführen müssen, sollten Sie Folgendes in Betracht ziehen:

- Ersetzen Sie Ihre Akteurdienste durch zustandsbehaftete zuverlässige Dienste, z.B. die Anzahl von Netzwerkanforderungen, um alle Daten von der Anzahl von Akteuren bis hin zur Anzahl der Partitionen in Ihrem Dienst zu erfassen.
- Entwerfen Sie Ihre Akteure so, dass sie in regelmäßigen Abständen ihren Zustand mithilfe von Push an einen externen Speicher übertragen, um eine einfachere Abfrage zu ermöglichen. Wie oben erwähnt, ist dieser Ansatz nur umsetzbar, wenn die von Ihnen durchgeführten Abfragen für das Laufzeitverhalten nicht erforderlich sind.

### <a name="how-much-data-can-i-store-in-a-reliable-collection"></a>Wie viele Daten kann ich in einer zuverlässigen Sammlung speichern?

Zuverlässige Dienste werden in der Regel partitioniert, sodass die speicherbare Menge nur durch die Anzahl der Computer im Cluster und durch die Menge des verfügbaren Arbeitsspeichers auf diesen Computern begrenzt wird.

Nehmen Sie beispielsweise an, Sie hätten eine zuverlässige Sammlung in einem Dienst mit 100 Partitionen und 3 Replikaten, in der Objekte von durchschnittlich 1 KB gespeichert werden. Nehmen Sie außerdem an, Sie hätten einen Cluster mit 10 Computern mit jeweils 16 GB Arbeitsspeicher. Gehen wir der Einfachheit halber und für eine sehr konservative Schätzung davon aus, dass das Betriebssystem und die Systemdienste, die Service Fabric-Runtime und Ihre Dienste 6 GB davon beanspruchen, sodass pro Computer 10 GB oder für den gesamten Cluster 100 GB übrig bleiben.

Denken Sie daran, dass jedes Objekt dreimal gespeichert werden muss (ein primäres Objekt und zwei Replikate). Demnach hätten Sie genügend Arbeitsspeicher für ca. 35 Millionen Objekte in Ihrer Sammlung, wenn Sie mit voller Kapazität arbeiten. Wir empfehlen jedoch, den gleichzeitigen Ausfall einer Fehlerdomäne und einer Upgradedomäne aufzufangen, was in etwa 1/3 der Kapazität ausmacht, und würden daher die Anzahl auf ungefähr 23 Millionen verringern.

Beachten Sie, dass bei dieser Berechnung zudem Folgendes vorausgesetzt wird:

- Die Daten sind einigermaßen gleichmäßig über die Partitionen verteilt, oder Sie melden Auslastungsmetriken an den Resource Manager des Clusters. Standardmäßig führt Service Fabric basierend auf der Replikatanzahl einen Lastenausgleich durch. In unserem obigen Beispiel würden 10 primäre Replikate und 20 sekundäre Replikate auf jedem Knoten im Cluster platziert. Dies funktioniert gut für Lasten, die gleichmäßig auf die Partitionen verteilt sind. Wenn die Last nicht gleichmäßig verteilt ist, müssen Sie die Last melden, damit der Resource Manager kleinere Replikate zusammenlegen und zulassen kann, dass größere Replikate auf einem einzelnen Knoten mehr Arbeitsspeicher beanspruchen.

- Der betreffende zuverlässige Dienst ist der einzige im Cluster, auf dem der Zustand gespeichert wird. Da Sie einem Cluster mehrere Dienste bereitstellen können, müssen Sie die Ressourcen berücksichtigen, die jeder Dienst zum Ausführen und Verwalten seines Zustands benötigt.

- Der Cluster selbst wird nicht vergrößert oder verkleinert. Wenn Sie weitere Computer hinzufügen, führt Service Fabric zum Nutzen der zusätzlichen Kapazität einen erneuten Ausgleich der Replikate durch, bis die Anzahl der Computer die Anzahl der Partitionen in Ihrem Dienst übersteigt, da ein einzelnes Replikat sich nicht auf mehrere Computer erstrecken kann. Wenn Sie im Gegensatz dazu die Größe des Clusters durch das Entfernen von Computern reduzieren, werden die Replikate enger gepackt und weisen eine geringere Gesamtkapazität auf.

### <a name="how-much-data-can-i-store-in-an-actor"></a>Wie viele Daten können in einem Akteur gespeichert werden?

Wie bei zuverlässigen Diensten wird die Menge der Daten, die Sie in einem Akteurdienst speichern können, nur durch den insgesamt verfügbaren Speicherplatz und Arbeitsspeicher auf den Knoten in Ihrem Cluster begrenzt. Einzelne Akteure sind jedoch am effektivsten, wenn sie zum Kapseln einer kleinen Menge von Zustandslogik und der zugehörigen Geschäftslogik verwendet werden. Allgemein gilt, dass ein einzelner Akteur einen Zustand aufweisen sollte, der in KB gemessen werden kann.

## <a name="other-questions"></a>Weitere Fragen

### <a name="how-does-service-fabric-relate-to-containers"></a>Welcher Zusammenhang besteht zwischen Service Fabric und Containern?

Container bieten eine einfache Möglichkeit zum Paketieren von Diensten und ihren Abhängigkeiten, sodass sie konsistent in allen Umgebungen und in isolierter Weise auf einem einzelnen Computer ausgeführt werden können. Service Fabric bietet eine Möglichkeit zum Bereitstellen und Verwalten von Diensten, darunter [Dienste, die in einem Container paketiert wurden](service-fabric-containers-overview.md).

### <a name="are-you-planning-to-open-source-service-fabric"></a>Planen Sie die Open Source-Bereitstellung von Service Fabric?

Wir möchten die Reliable Services- und Reliable Actors-Frameworks als Open Source-Projekte auf GitHub bereitstellen und nehmen Community-Beiträge zu diesen Projekten an. Folgen Sie dem [Service Fabric-Blog](https://blogs.msdn.microsoft.com/azureservicefabric/), in dem Sie weitere Details finden, sobald sie angekündigt werden.

Derzeit gibt es keine Pläne für die Open Source-Bereitstellung der Service Fabric-Laufzeit.

## <a name="next-steps"></a>Nächste Schritte

- [Informationen zu wesentlichen Service Fabric-Konzepten und bewährten Methoden](https://mva.microsoft.com/en-us/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965)

