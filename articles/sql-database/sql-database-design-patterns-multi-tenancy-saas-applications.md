---
title: "Entwurfsmuster für mehrinstanzenfähige SaaS-Anwendungen und Azure SQL-Datenbank | Microsoft Docs"
description: "In diesem Artikel werden die Anforderungen und allgemeinen Datenarchitekturmuster von mehrinstanzenfähigen Datenbankanwendungen, die in einer Cloudumgebung ausgeführt werden, sowie die verschiedenen Vor- und Nachteile dieser Muster erläutert. Außerdem wird beschrieben, wie diese Anforderungen mit den elastischen Pools und elastischen Tools von Azure SQL-Datenbank ohne Kompromisse erfüllt werden können."
keywords: 
services: sql-database
documentationcenter: 
author: srinia
manager: jhubbard
editor: 
ms.assetid: 1dd20c6b-ddbb-40ef-ad34-609d398d008a
ms.service: sql-database
ms.custom: development
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: sqldb-design
ms.date: 02/01/2017
ms.author: srinia
translationtype: Human Translation
ms.sourcegitcommit: e210fb7ead88a9c7f82a0d0202a1fb31043456e6
ms.openlocfilehash: c30f1d879f46805cf802679613089a16dc47ad40
ms.lasthandoff: 02/16/2017


---
# <a name="design-patterns-for-multitenant-saas-applications-and-azure-sql-database"></a>Entwurfsmuster für mehrinstanzenfähige SaaS-Anwendungen und Azure SQL-Datenbank
Dieser Artikel enthält Informationen zu den Anforderungen und allgemeinen Datenarchitekturmustern von mehrinstanzenfähigen SaaS-Datenbankanwendungen (Software-as-a-Service), die in einer Cloudumgebung ausgeführt werden. Zudem werden die zu berücksichtigenden Faktoren sowie die Vor- und Nachteile verschiedener Entwurfsmuster erläutert. Pools und Tools für elastische Datenbanken in Azure SQL-Datenbank können Ihnen dabei helfen, Ihre spezifischen Anforderungen zu erfüllen, ohne Abstriche bei anderen Zielen zu machen.

Entwickler treffen beim Entwerfen von Mandantenmodellen für die Datenebenen von mehrinstanzenfähigen Anwendungen mitunter Entscheidungen, die ihren langfristigen Interessen entgegenwirken. Zumindest am Anfang können die Einfachheit der Entwicklung und die geringeren Kosten für den Clouddienstanbieter dem Entwickler wichtiger erscheinen als die Mandantenisolation oder die Skalierbarkeit einer Anwendung. Diese Wahl kann später zu Problemen mit der Kundenzufriedenheit führen und eine kostspielige Kurskorrektur erforderlich machen.

Eine mehrinstanzenfähige Anwendung ist eine Anwendung, die in einer Cloudumgebung gehostet wird und die gleichen Dienste für Hunderte oder Tausende von Mandanten bereitstellt, die ihre Daten weder untereinander freigeben noch die Daten anderer Mandanten sehen. Ein Beispiel ist eine SaaS-Anwendung, die Dienste für Mandanten in einer in der Cloud gehosteten Umgebung bereitstellt.

## <a name="multitenant-applications"></a>Mehrinstanzenfähige Anwendungen
In mehrinstanzenfähigen Anwendungen können Daten und Workloads problemlos partitioniert werden. Sie können Daten und Workloads beispielsweise entlang von Mandantengrenzen partitionieren, da die meisten Anforderungen innerhalb der Grenzen eines Mandanten durchgeführt werden. Dies ist eine inhärente Eigenschaft der Daten und Workloads und begünstigt die in diesem Artikel beschriebenen Anwendungsmuster.

Entwickler verwenden diesen Anwendungstyp für das gesamte Spektrum cloudbasierter Anwendungen, z.B.:

* Partnerdatenbankanwendungen, die als SaaS-Anwendungen in die Cloud verlagert werden
* SaaS-Anwendungen, die von Grund auf für die Cloud erstellt wurden
* Direkt kundenorientierte Anwendungen
* Unternehmensanwendungen für Mitarbeiter

Für die Cloud entworfene SaaS-Anwendungen und SaaS-Anwendungen, die auf Partnerdatenbankanwendungen zurückgehen, sind in der Regel mehrinstanzenfähige Anwendungen. Mit diesen SaaS-Anwendungen wird eine spezialisierte Softwareanwendung als Service für die Mandanten bereitgestellt. Mandanten können auf den Anwendungsdienst zugreifen und sind die alleinigen Besitzer der zugehörigen Daten, die im Rahmen der Anwendung gespeichert werden. Um die Vorteile von SaaS nutzen zu können, müssen Mandanten die Kontrolle über ihre eigenen Daten jedoch zum Teil abgeben. Sie verlassen sich darauf, dass der Anbieter des SaaS-Diensts die Sicherheit ihrer Daten gewährleistet und sie von den Daten anderer Mandanten isoliert. Beispiele für diese Art von mehrinstanzenfähigen SaaS-Anwendungen sind MYOB, SnelStart und Salesforce.com. Jede dieser Anwendungen kann entlang von Mandantengrenzen partitioniert werden und die in diesem Artikel erörterten Entwurfsmuster unterstützen.

Anwendungen, die einen direkten Service für Kunden oder Mitarbeiter eines Unternehmens (häufig nicht als Mandanten bezeichnet, sondern als Benutzer) darstellen, sind eine weitere Kategorie von mehrinstanzenfähigen Anwendungen. Kunden abonnieren den Dienst und sind nicht die Besitzer der Daten, die vom Service Provider gesammelt und gespeichert werden. Für Service Provider gelten abgesehen von den behördlich vorgegebenen Datenschutzregeln weniger strenge Anforderungen in Bezug auf die Isolation der Kundendaten. Beispiele für diese Art von kundenorientierten mehrinstanzenfähigen Anwendungen sind Medieninhaltsanbieter wie Netflix, Spotify und Xbox LIVE. Weitere Beispiele für leicht partitionierbare Anwendungen sind kundenorientierte Internetanwendungen oder IoT-Anwendungen (Internet of Things, Internet der Dinge), bei denen jeder Kunde oder jedes Gerät als Partition dienen kann. Partitionsgrenzen können Benutzer und Geräte trennen.

Nicht alle Anwendungen lassen sich einfach anhand einer einzelnen Eigenschaft wie Mandant, Kunde, Benutzer oder Gerät partitionieren. Eine komplexe ERP-Anwendung (Enterprise Resource Planning) umfasst beispielsweise Produkte, Aufträge und Kunden. Sie verfügt meist über ein komplexes Schema mit Tausenden von verknüpften Tabellen.

Es gibt keine einzelne Partitionsstrategie, die für alle Tabellen verwendet werden kann und für die gesamte Workload der Anwendung funktioniert. Den Schwerpunkt dieses Artikels bilden mehrinstanzenfähige Anwendungen mit problemlos partitionierbaren Daten und Workloads.

## <a name="multitenant-application-design-trade-offs"></a>Mehrinstanzenfähige Anwendungen – bei der Wahl des Entwurfsmusters zu berücksichtigende Faktoren
Das Entwurfsmuster, für das sich der Entwickler einer mehrinstanzenfähigen Anwendung entscheidet, basiert in der Regel auf folgenden Faktoren:

* **Mandantenisolation**. Der Entwickler muss sicherstellen, dass kein Mandant unerwünschten Zugriff auf die Daten anderer Mandanten hat. Die Isolationsanforderung gilt auch für andere Bereiche, z.B. den Schutz vor „Noisy Neighbors“, die Möglichkeit zur Wiederherstellung der Daten eines Mandanten und die Implementierung mandantenspezifischer Anpassungen.
* **Kosten von Cloudressourcen**. Eine SaaS-Anwendung muss in Bezug auf die Kosten konkurrenzfähig sein. Der Entwickler einer mehrinstanzenfähigen Anwendung kann sich dafür entscheiden, die Kosten für die Nutzung von Cloudressourcen (z.B. Speicher- und Computekosten) durch entsprechende Optimierung zu reduzieren.
* **Einfache DevOps-Abläufe**. Der Entwickler einer mehrinstanzenfähigen Anwendung muss einen Isolationsschutz integrieren, die Integrität seiner Anwendung und des Datenbankschemas aufrechterhalten und überwachen sowie Probleme der Mandanten beheben. Sind die Entwicklung und der Betrieb einer Anwendung komplex, führt dies direkt zu Mehrkosten und erschwert es, die Mandanten zufrieden zu stellen.
* **Skalierbarkeit**. Die Möglichkeit, inkrementell weitere Mandanten und bei Bedarf Kapazität für Mandanten hinzuzufügen, ist für einen erfolgreichen SaaS-Betrieb unerlässlich.

Jeder dieser Faktoren hat Vor- und Nachteile, die gegeneinander abgewägt werden müssen. Das kostengünstigste Cloudangebot bietet möglicherweise nicht die benutzerfreundlichste Entwicklungsumgebung. Entwickler müssen beim Entwerfen der Anwendung informierte Entscheidungen bezüglich dieser Optionen sowie ihrer Vor- und Nachteile treffen.

Ein gängiges Entwicklungsmuster besteht darin, mehrere Mandanten zusammen in einer oder mehreren Datenbanken anzuordnen. Die Vorteile dieses Ansatzes sind die niedrigeren Kosten, da Sie für eine kleine Anzahl von Datenbanken bezahlen, und die relative Einfachheit der Arbeit mit einer begrenzten Anzahl von Datenbanken. Im Laufe der Zeit wird ein Entwickler von mehrinstanzenfähigen SaaS-Anwendungen jedoch feststellen, dass diese Entscheidung erhebliche Nachteile in puncto Mandantenisolation und Skalierbarkeit mit sich bringt. Wenn die Mandantenisolation an Bedeutung gewinnt, ist zusätzlicher Aufwand erforderlich, um Mandantendaten im gemeinsam genutzten Speicher vor nicht autorisiertem Zugriff oder „Noisy Neigbors“ zu schützen. Dieser zusätzliche Aufwand kann den Entwicklungsaufwand und die Kosten zur Aufrechterhaltung der Isolation erheblich steigern. Wenn Mandanten hinzugefügt werden müssen, sind bei diesem Entwurfsmuster meist auch Kenntnisse in der Neuverteilung von Mandantendaten in Datenbanken erforderlich, um die Datenebene einer Anwendung korrekt zu skalieren.  

Die Mandantenisolation ist bei mehrinstanzenfähigen SaaS-Anwendungen für Unternehmen und Organisationen oft eine grundlegende Anforderung. Die scheinbaren Vorteile der Einfachheit und Kostenersparnis können einen Entwickler dazu verleiten, die Mandantenisolation und Skalierbarkeit zu vernachlässigen. Dieser Kompromiss kann sich als kompliziert und teuer herausstellen, wenn der Umfang des Diensts zunimmt und die Anforderungen an die Mandantenisolation wichtiger werden und in der Anwendungsschicht verwaltet werden müssen. Bei mehrinstanzenfähigen Anwendungen, die einen direkt kundenorientierten Dienst für Endverbraucher bereitstellen, hat die Mandantenisolation unter Umständen jedoch eine geringere Priorität als die Optimierung der Kosten für die Cloudressourcen.

## <a name="multitenant-data-models"></a>Mehrinstanzenfähige Datenmodelle
Die allgemeinen Entwurfspraktiken für die Speicherung von Mandantendaten basieren auf den in Abbildung 1 dargestellten drei Modellen.

![Mehrinstanzenfähige Anwendungsdatenmodelle](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-multi-tenant-data-models.png)

Abbildung 1: Allgemeine Entwurfspraktiken für mehrinstanzenfähige Datenmodelle

* **Eine Datenbank pro Mandant**. Jeder Mandant verfügt über eine eigene Datenbank. Alle mandantenspezifischen Daten werden ausschließlich in der Datenbank des Mandanten gespeichert und von anderen Mandanten sowie deren Daten isoliert.
* **Gemeinsame Datenbank – Sharding**. Mehrere Mandanten nutzen eine von mehreren Datenbanken gemeinsam. Jeder Datenbank wird mithilfe einer Partitionierungsstrategie (z.B. Hash-, Bereichs- oder Listenpartitionierung) eine eigene Gruppe von Mandanten zugewiesen. Diese Strategie der Datenverteilung wird als „Sharding“ bezeichnet.
* **Gemeinsame Datenbank – einzeln**. Eine einzelne, manchmal große Datenbank enthält Daten für alle Mandanten, zu deren Unterscheidung eine Spalte mit der Mandanten-ID verwendet wird.

> [!NOTE]
> Ein Anwendungsentwickler kann für verschiedene Mandanten unterschiedliche Datenbankschemas verwenden und die Mandanten dann anhand des Schemanamens unterscheiden. Dieser Ansatz wird nicht empfohlen, da er in der Regel die Verwendung von dynamischem SQL-Code erfordert und eine effektive Zwischenspeicherung des Plans nicht möglich ist. Im restlichen Teil dieses Artikels konzentrieren wir uns auf den Ansatz mit der gemeinsam genutzten Tabelle für diese Kategorie von mehrinstanzenfähiger Anwendung.
> 
> 

## <a name="popular-multitenant-data-models"></a>Gängige mehrinstanzenfähige Datenmodelle
Es ist wichtig, die verschiedenen Arten von mehrinstanzenfähigen Datenmodellen in Bezug auf die bereits genannten Faktoren, die beim Anwendungsentwurf abzuwägen sind, zu bewerten. Auf Grundlage dieser Faktoren lassen sich die drei zuvor beschriebenen gängigsten mehrinstanzenfähigen Datenmodelle und ihre Datenbanknutzung wie in Abbildung 2 dargestellt charakterisieren.

* **Isolation**. Der Grad der Isolation zwischen Mandanten kann als Maß für die mit einem Datenmodell erzielte Mandantenisolation dienen.
* **Kosten von Cloudressourcen**. Der Umfang der gemeinsamen Ressourcennutzung zwischen Mandanten kann die Kosten für Cloudressourcen optimieren. Eine Ressource kann in Form von Compute- und Speicherkosten definiert werden.
* **DevOps-Kosten**. Die Gesamtkosten für den SaaS-Betrieb können durch eine einfache Anwendungsentwicklung, -bereitstellung und -verwaltbarkeit reduziert werden.  

Der Grad der Mandantenisolation ist in Abbildung 2 auf der y-Achse dargestellt. Die x-Achse zeigt den Umfang der gemeinsamen Ressourcennutzung. Der graue diagonale Pfeil in der Mitte stellt die Entwicklung der DevOps-Kosten dar und zeigt, ob sie eher zu- oder abnehmen.

![Gängige Entwurfsmuster für mehrinstanzenfähige Anwendungen](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-popular-application-patterns.png)

Abbildung 2: Gängige mehrinstanzenfähige Datenmodelle

Der untere rechte Quadrant in Abbildung 2 zeigt ein Anwendungsmuster, bei dem eine potenziell große gemeinsame Einzeldatenbank und der Ansatz der gemeinsam genutzten Tabelle (oder eines separaten Schemas) verwendet werden. Dieses Muster eignet sich gut für die gemeinsame Nutzung von Ressourcen, da alle Mandanten die gleichen Datenbankressourcen (CPU, Arbeitsspeicher, Eingabe/Ausgabe) in einer Einzeldatenbank verwenden. Die Mandantenisolation ist jedoch eingeschränkt. Möglicherweise müssen Sie zusätzliche Schritte ausführen, um Mandanten in der Anwendungsschicht voreinander zu schützen. Diese zusätzlichen Schritte können die DevOps-Kosten für die Entwicklung und Verwaltung der Anwendung deutlich erhöhen. Die Skalierbarkeit ist durch den Umfang der Hardware eingeschränkt, die die Datenbank hostet.

Der untere linke Quadrant in Abbildung 2 zeigt mehrere Mandanten, die per Sharding auf mehrere Datenbanken verteilt sind (meist unterschiedliche Hardwareskalierungseinheiten). Jede Datenbank hostet eine Teilmenge von Mandanten, sodass die bei anderen Mustern bestehenden Probleme mit der Skalierbarkeit entfallen. Wenn mehr Kapazität für neue Mandanten erforderlich ist, können die Mandanten leicht in neuen Datenbanken platziert werden, die neuen Hardwareskalierungseinheiten zugeordnet sind. Ressourcen werden jedoch in geringerem Umfang gemeinsam genutzt. Nur Mandanten in denselben Skalierungseinheiten nutzen Ressourcen gemeinsam. Dieser Ansatz stellt nur eine geringfügige Verbesserung in Bezug auf die Mandantenisolation dar, da weiterhin viele Mandanten zusammengefasst werden, ohne dass sie automatisch vor den Aktionen anderer Mandanten geschützt sind. Die Komplexität der Anwendung bleibt somit hoch.

Der obere linke Quadrant in Abbildung 2 stellt den dritten Ansatz dar. Hier werden die Daten jedes Mandanten in einer eigenen Datenbank gespeichert. Dieser Ansatz bietet eine gute Mandantenisolation, schränkt aber die gemeinsame Nutzung von Ressourcen ein, wenn jede Datenbank über eigene dedizierte Ressourcen verfügt. Der Ansatz eignet sich gut, wenn alle Mandanten vorhersagbare Workloads haben. Wenn die Mandantenworkloads irgendwann weniger vorhersagbar werden, kann der Anbieter die gemeinsame Nutzung von Ressourcen nicht optimieren. Unvorhersehbarkeit ist bei SaaS-Anwendungen häufig. Der Anbieter muss entweder zu viele Ressourcen bereitstellen, um den Bedarf zu decken, oder die Ressourcen reduzieren. Dies führt entweder zu höheren Kosten oder beeinträchtigt die Zufriedenheit der Mandanten. Um die Kosteneffektivität der Lösung zu erhöhen, wird daher eine stärkere gemeinsame Nutzung von Ressourcen zwischen Mandanten wünschenswert. Wenn die Anzahl von Datenbanken erhöht wird, erhöhen sich auch die DevOps-Kosten für die Bereitstellung und Wartung der Anwendung. Trotz dieser Probleme bietet diese Methode die beste und einfachste Isolation für Mandanten.

Die folgenden Faktoren haben ebenfalls Einfluss auf das von einem Kunden gewählte Entwurfsmuster:

* **Besitz von Mandantendaten**. Bei einer Anwendung, bei der Mandanten Besitzer ihrer eigenen Daten bleiben, ist das Muster „eine Einzeldatenbank pro Mandant“ von Vorteil.
* **Skalierung:**Bei einer Anwendung für Hunderttausende oder Millionen von Mandanten sind Ansätze mit gemeinsamer Datenbanknutzung vorteilhaft, z.B. Sharding. Die Anforderungen an die Isolation können aber trotzdem eine Herausforderung darstellen.
* **Wert- und Geschäftsmodell**. Wenn der Umsatz pro Mandant einer Anwendung gering ist (weniger als&1; US-Dollar), sind die Isolationsanforderungen weniger wichtig, und die Verwendung einer gemeinsamen Datenbank ist sinnvoll. Beträgt der Umsatz pro Mandant ein paar US-Dollar oder mehr, ist die Verwendung eines Modells mit einer Datenbank pro Mandant eher möglich. Dieses Modell kann die Entwicklungskosten reduzieren.

Den in Abbildung 2 dargestellten Vor- und Nachteilen zufolge muss ein ideales mehrinstanzenfähiges Modell über hervorragende Eigenschaften für die Mandantenisolation mit optimaler gemeinsamer Nutzung von Ressourcen zwischen Mandanten verfügen. Dieses Modell ist im oberen rechten Quadranten von Abbildung 2 dargestellt.

## <a name="multitenancy-support-in-azure-sql-database"></a>Unterstützung der Mehrinstanzenfähigkeit in Azure SQL-Datenbank
Azure SQL-Datenbank unterstützt alle mehrinstanzenfähigen Anwendungsmuster, die in Abbildung 2 dargestellt sind. Mit elastischen Pools wird jetzt ein neues Anwendungsmuster unterstützt, das eine gute gemeinsame Nutzung von Ressourcen mit den Isolationsvorteilen des Ansatzes „eine Datenbank pro Mandant“ kombiniert (oberer rechter Quadrant in Abbildung 3). Tools und Funktionen für elastische Datenbanken in SQL-Datenbank können Ihnen dabei helfen, die Kosten für das Entwickeln und Betreiben einer Anwendung mit vielen Datenbanken zu reduzieren (dargestellt im schattierten Bereich in Abbildung 3). Diese Tools ermöglichen Ihnen das Erstellen und Verwalten von Anwendungen mit jedem der Muster mit mehreren Datenbanken.

![Muster in Azure SQL-Datenbank](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-patterns-sqldb.png)

Abbildung 3: Mehrinstanzenfähige Anwendungsmuster in Azure SQL-Datenbank

## <a name="database-per-tenant-model-with-elastic-pools-and-tools"></a>Modell „Eine Datenbank pro Mandant“ mit elastischen Pools und Tools
Pools für elastische Datenbanken in SQL-Datenbank kombinieren die Mandantenisolation mit der gemeinsamen Ressourcennutzung zwischen Mandantendatenbanken und bieten dadurch bessere Unterstützung für den Ansatz „eine Datenbank pro Mandant“. SQL-Datenbank ist eine Datenebenenlösung für SaaS-Anbieter, die mehrinstanzenfähige Anwendungen erstellen. Die Last der Ressourcenfreigabe zwischen Mandanten wechselt von der Anwendungsebene zur Dienstebene der Datenbank. Die Komplexität der datenbankübergreifenden Verwaltung und Durchführung von Abfragen bei guter Skalierbarkeit wird durch elastische Aufträge, elastische Abfrage, elastische Transaktionen und die Clientbibliothek für elastische Datenbanken reduziert.

| Anwendungsanforderungen | Funktionen von SQL-Datenbank |
| --- | --- |
| Mandantenisolation und gemeinsame Ressourcennutzung |[Elastische Pools:](sql-database-elastic-pool.md)Sie können einen Pool von SQL-Datenbank-Ressourcen zuordnen und die Ressourcen mehrere Datenbanken übergreifend nutzen. Einzelne Datenbanken können außerdem so viele Ressourcen wie benötigt aus dem Pool abrufen, um Kapazitätsspitzen aufgrund von Änderungen der Mandantenworkloads abzudecken. Der elastische Pool selbst kann je nach Bedarf zentral hoch- oder herunterskaliert werden. Elastische Pools verbessern zudem die Verwaltbarkeit und vereinfachen die Überwachung sowie die Problembehandlung auf der Poolebene. |
| Einfache DevOps-Abläufe über Datenbanken hinweg |[Elastische Pools:](sql-database-elastic-pool.md)Siehe oben. |
| | [Elastische Abfrage:](sql-database-elastic-query-horizontal-partitioning.md)Zur Berichterstellung oder mandantenübergreifenden Analyse können datenbankübergreifende Abfragen ausgeführt werden. |
| | [Elastische Aufträge:](sql-database-elastic-jobs-overview.md)Wartungsvorgänge oder Schemaänderungen für mehrere Datenbanken können verpackt und zuverlässig bereitgestellt werden. |
| | [Elastische Transaktionen:](sql-database-elastic-transactions-overview.md)Änderungen an mehreren Datenbanken können auf atomische und isolierte Weise verarbeitet werden. Elastische Transaktionen sind erforderlich, wenn Anwendungen für eine Reihe von Datenbankvorgängen Garantien der Art „Alles oder nichts“ benötigen. |
| | [Clientbibliothek für elastische Datenbanken](sql-database-elastic-database-client-library.md): Sie können Datenverteilungen verwalten und Mandanten Datenbanken zuordnen. |

## <a name="shared-models"></a>Modelle mit gemeinsamer Nutzung
Wie bereits beschrieben, können sich bei Ansätzen mit gemeinsamer Nutzung für die meisten SaaS-Anbieter Probleme mit der Mandantenisolation und Schwierigkeiten bei der Anwendungsentwicklung und -wartung ergeben. Für mehrinstanzenfähige Anwendungen, die einen Dienst direkt für Kunden bereitstellen, hat die Mandantenisolation jedoch ggf. nicht eine so hohe Priorität wie die Minimierung der Kosten. Bei solchen Anwendungen können Mandanten möglicherweise mit hoher Dichte in einer oder mehreren Datenbanken zusammengefasst werden, um die Kosten zu senken. Bei Modellen mit gemeinsamer Datenbanknutzung, bei denen eine Einzeldatenbank oder mehrere Datenbanken mit Sharding verwendet werden, sind dadurch Effizienzsteigerungen in Bezug auf die gemeinsame Nutzung von Ressourcen und die Gesamtkosten möglich. Azure SQL-Datenbank bietet einige Features, mit denen Kunden die Isolation einrichten können, um die Sicherheit und Verwaltung mit guter Skalierbarkeit auf der Datenebene zu verbessern.

| Anwendungsanforderungen | Funktionen von SQL-Datenbank |
| --- | --- |
| Features für die Sicherheitsisolation |[Sicherheit auf Zeilenebene](https://msdn.microsoft.com/library/dn765131.aspx) |
| [Datenbankschema](https://msdn.microsoft.com/library/dd207005.aspx) | |
| Einfache DevOps-Abläufe über Datenbanken hinweg |[Elastische Abfrage](sql-database-elastic-query-horizontal-partitioning.md) |
| | [Elastische Aufträge](sql-database-elastic-jobs-overview.md) |
| | [Elastische Transaktionen](sql-database-elastic-transactions-overview.md) |
| | [Clientbibliothek für elastische Datenbanken](sql-database-elastic-database-client-library.md) |
| | [Trennen und Zusammenführen von elastischen Datenbanken](sql-database-elastic-scale-overview-split-and-merge.md) |

## <a name="summary"></a>Zusammenfassung
Die Mandantenisolation ist für die meisten mehrinstanzenfähigen SaaS-Anwendungen eine wichtige Anforderung. Zum Bereitstellen der Isolation eignet sich am besten der Ansatz „eine Datenbank pro Mandant“. Die beiden anderen Ansätze erfordern Investitionen in komplexe Anwendungsschichten, bei denen erfahrene Entwickler zum Einrichten der Isolation erforderlich sind, was Kosten und Risiko deutlich reduziert. Wenn Isolationsanforderungen nicht zu einem frühen Zeitpunkt der Dienstentwicklung berücksichtigt werden, kann deren nachträgliche Einrichtung sogar teurer als bei den ersten beiden Modellen werden. Die Hauptnachteile des Modells „eine Datenbank pro Mandant“ sind die höheren Kosten für Cloudressourcen aufgrund der geringeren gemeinsamen Nutzung sowie die Wartung und Verwaltung vieler Datenbanken. Für Entwickler von SaaS-Anwendungen ist es häufig nicht leicht, diese Vor- und Nachteile gegeneinander abzuwägen.

Für die meisten Anbieter von Clouddatenbank-Diensten kann es eine große Hürde darstellen, einen geeigneten Kompromiss zwischen den Vor- und Nachteilen zu finden. Azure SQL-Datenbank beseitigt diese Hürden jedoch mit seinem elastischen Pool und den Funktionen für elastische Datenbanken. Mithilfe von elastischen Pools und den zugehörigen Tools können SaaS-Entwickler die Isolationsmerkmale des Modells „eine Datenbank pro Mandant“ kombinieren, die gemeinsame Nutzung von Ressourcen optimieren und von der besseren Verwaltbarkeit vieler Datenbanken profitieren.

Anbieter von mehrinstanzenfähigen Anwendungen, die keine Mandantenisolation benötigen und Mandanten mit hoher Dichte in einer Datenbank platzieren können, können mit Datenmodellen mit gemeinsamer Nutzung möglicherweise eine effizientere Ressourcennutzung und eine Senkung der Gesamtkosten erzielen. Die Tools für elastische Datenbanken, das Sharding von Bibliotheken und die Sicherheitsfeatures von Azure SQL-Datenbank unterstützen SaaS-Anbieter beim Erstellen und Verwalten mehrinstanzenfähiger Anwendungen.

## <a name="next-steps"></a>Nächste Schritte
[Erste Schritte mit Tools für elastische Datenbanken](sql-database-elastic-scale-get-started.md) finden Sie eine Beispiel-App, die die Clientbibliothek veranschaulicht.

Erstellen Sie mit einer Beispiel-App, die mit elastischen Pools eine kostengünstige, skalierbare Datenbanklösung realisiert, ein [benutzerdefiniertes Dashboard für SaaS mit elastischen Pool](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools-custom-dashboard) .

Verwenden Sie die Tools von Azure SQL-Datenbank zum [Migrieren von vorhandenen Datenbanken zu horizontaler Hochskalierung](sql-database-elastic-convert-to-use-elastic-tools.md).

Informationen zum Erstellen eines Pools für elastische Datenbanken über das Azure-Portal finden Sie unter [Erstellen eines Pools für elastische Datenbanken](sql-database-elastic-pool-manage-portal.md).  

Erfahren Sie, wie Sie einen [elastischen Pool überwachen und verwalten](sql-database-elastic-pool-manage-portal.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Was ist ein elastischer Azure-Pool?](sql-database-elastic-pool.md)
* [Übersicht über Features für elastische Datenbanken](sql-database-elastic-scale-introduction.md)
* [Mehrinstanzenfähige Anwendungen mit elastischen Datenbanktools und zeilenbasierter Sicherheit](sql-database-elastic-tools-multi-tenant-row-level-security.md)
* [Authentication in multitenant apps by using Azure Active Directory and OpenID Connect (Authentifizierung in mehrinstanzenfähigen Apps mithilfe von Azure Active Directory und OpenID Connect)](../guidance/guidance-multitenant-identity-authenticate.md)
* [Tailspin-Anwendung „Surveys“](../guidance/guidance-multitenant-identity-tailspin.md)


## <a name="questions-and-feature-requests"></a>Fragen und Featureanfragen
Falls Sie Fragen haben, finden Sie uns im [SQL-Datenbank-Forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted). Featureanfragen können im [SQL-Datenbank-Feedbackforum](https://feedback.azure.com/forums/217321-sql-database/)hinzugefügt werden.


