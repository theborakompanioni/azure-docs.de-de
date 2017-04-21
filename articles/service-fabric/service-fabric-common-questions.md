---
title: "Häufig gestellte Fragen zu Microsoft Azure Service Fabric | Microsoft-Dokumentation"
description: "Häufig gestellte Fragen zu Service Fabric und entsprechende Antworten"
services: service-fabric
documentationcenter: .net
author: seanmck
manager: timlt
editor: 
ms.assetid: 5a179703-ff0c-4b8e-98cd-377253295d12
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/08/2017
ms.author: seanmck
translationtype: Human Translation
ms.sourcegitcommit: cfe4957191ad5716f1086a1a332faf6a52406770
ms.openlocfilehash: 6c0c6b24f9d669e7ed45e6b2acf2e75390e5e1f4
ms.lasthandoff: 03/09/2017

---


# <a name="commonly-asked-service-fabric-questions"></a>Häufig gestellte Fragen zu Service Fabric

Es gibt viele häufig gestellte Fragen zu den Funktionen und zur Verwendung von Service Fabric. In diesem Dokument werden viele dieser häufig gestellten Fragen und die zugehörigen Antworten behandelt.

## <a name="cluster-setup-and-management"></a>Clustereinrichtung und -verwaltung

### <a name="can-i-create-a-cluster-that-spans-multiple-azure-regions"></a>Kann ich einen Cluster erstellen, der mehrere Azure-Regionen umfasst?

Heute noch nicht, aber dies ist eine häufig gestellte Anforderung, die wir weiter untersuchen.

Der Service Fabric-Kernclusteringtechnologie sind Azure-Regionen unbekannt. Sie kann zum Verbinden von Computern an beliebigen Orten in der ganzen Welt verwendet werden, solange diese über gemeinsame Netzwerkkonnektivität verfügen. Allerdings ist die Service Fabric-Clusterressource in Azure regional, ebenso wie die VM-Skalierungsgruppen, auf denen der Cluster beruht. Darüber hinaus gibt es ein inhärentes Problem bei der Übermittlung einer stark konsistenten Datenreplikation zwischen Computern, die weit auseinander liegen. Wir möchten sicherstellen, dass die Leistung vorhersagbar und akzeptabel ist, bevor wir regionsübergreifende Cluster unterstützen.

### <a name="do-service-fabric-nodes-automatically-receive-os-updates"></a>Erhalten Service Fabric-Knoten automatisch Betriebssystemupdates?

Heute noch nicht, aber auch dies ist eine häufig gestellte Anforderung, die wir umsetzen möchten.

Die Herausforderung von Betriebssystemupdates besteht darin, dass dafür in der Regel ein Neustart des Computers erforderlich ist, was zu einer vorübergehenden Unterbrechung der Verfügbarkeit führt. Dies ist für sich gesehen noch kein Problem, weil Service Fabric Datenverkehr für diese Dienste automatisch an andere Knoten umleitet. Wenn Betriebssystemupdates jedoch nicht über den Cluster koordiniert werden, besteht die Gefahr, dass viele Knoten gleichzeitig ausfallen. Diese gleichzeitigen Neustarts können für einen Dienst oder zumindest für eine bestimmte Partition (bei einem zustandsbehafteten Dienst) zum vollständigen Verlust der Verfügbarkeit führen.

Künftig werden wir eine Betriebssystemupdate-Richtlinie unterstützen, die Updatedomänen-übergreifend vollständig automatisiert und koordiniert ist und sicherstellt, dass die Verfügbarkeit trotz Neustarts und anderen unerwarteten Fehlern beibehalten wird.

In der Zwischenzeit haben wir [ein Skript bereitgestellt](https://blogs.msdn.microsoft.com/azureservicefabric/2017/01/09/os-patching-for-vms-running-service-fabric/), mit denen ein Clusteradministrator das Patchen jedes Knotens auf sichere Weise manuell starten kann.

### <a name="can-i-use-large-virtual-scale-sets-in-my-sf-cluster"></a>Kann ich große virtuelle Skalierungsgruppen in meinem SF-Cluster verwenden? 

**Kurze Antwort**: Nein. 

**Lange Antwort**: Obwohl Sie mit großen virtuellen Skalierungsgruppen (VMSS) eine VMSS mit bis zu 1.000 VM-Instanzen skalieren können, geschieht dies mithilfe von Platzierungsgruppen (PGs). Fehlerdomänen (FDs) und Upgradedomänen (UDs) sind nur innerhalb einer Platzierungsgruppe konsistent. Service Fabric verwendet FDs und UDs für Platzierungsentscheidungen, die Ihre Dienstreplikate/Dienstinstanzen betreffen. Da die FDs und UDs nur innerhalb einer Platzierungsgruppe vergleichbar sind, kann SF sie nicht verwenden. Wenn beispielsweise VM1 in PG1 über eine Topologie FD=0 und VM9 in PG2 über eine Topologie FD=4 verfügt, bedeutet dies nicht, dass VM1 und VM2 sich in zwei verschiedenen Hardwareracks befinden, daher kann SF die FD-Werte in diesem Fall nicht für Platzierungsentscheidungen verwenden.

Es gibt derzeit andere Probleme mit großen VMSS, z.B. den Mangel an Unterstützung von Ebene-4-Lastenausgleich. Hier erfahren Sie [Näheres zu großen VMSS](../virtual-machine-scale-sets/virtual-machine-scale-sets-placement-groups.md).



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

