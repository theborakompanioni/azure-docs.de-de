<properties
   pageTitle="Entwurfsmuster für mehrinstanzenfähige SaaS-Anwendungen mit Azure SQL-Datenbank | Microsoft Azure" 
   description="In diesem Artikel werden die relevanten Anforderungen und allgemeinen Datenarchitekturmuster von mehrinstanzenfähigen Datenbankanwendungen beschrieben, die in einer Cloudumgebung ausgeführt werden, sowie die unterschiedlichen Vor- und Nachteile dieser Muster. Außerdem wird beschrieben, wie der Azure SQL-Datenbank-Dienst mit seinen Pools für elastische Datenbanken und elastischen Tools eingesetzt werden kann, um diese Anforderungen kompromisslos zu erfüllen."
   keywords=""
   services="sql-database"
   documentationCenter=""
   authors="carlrabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-design"
   ms.date="06/07/2016"
   ms.author="carlrab"/>

# Entwurfsmuster für mehrinstanzenfähige SaaS-Anwendungen und Azure SQL-Datenbank

Dieser Artikel enthält Informationen zu den relevanten Anforderungen und allgemeinen Datenarchitekturmustern von mehrinstanzenfähigen SaaS-Datenbankanwendungen (Software as a Service), die in einer Cloudumgebung ausgeführt werden, und zu den Vor- und Nachteilen dieser Muster. Außerdem wird beschrieben, wie der Azure SQL-Datenbank-Dienst mit seinen Pools für elastische Datenbanken und elastischen Tools eingesetzt werden kann, um diese Anforderungen kompromisslos zu erfüllen.

## Einführung

Während der Zusammenarbeit mit Anbietern von SaaS-Anwendungen in den letzten Jahren haben wir erlebt, dass Entwickler beim Entwerfen der Mandantenmodelle für die Datenebenen ihrer mehrinstanzenfähigen Anwendungen häufig Entscheidungen treffen, die ihren langfristigen Interessen entgegenstehen. Zumindest am Anfang werden die Einfachheit der Entwicklung und geringe Kosten für den Cloudanbieter als wichtiger als die Mandantenisolation oder die Skalierbarkeit der Anwendungen angesehen. Dies führt häufig zu Problemen mit der Kundenzufriedenheit und teuren Kurskorrekturen zu einem späteren Zeitpunkt.

Im Kontext dieses Artikels wird unter einer mehrinstanzenfähigen Anwendung eine Anwendung verstanden, die in einer Cloudumgebung gehostet wird und die gleichen Dienste für viele Mandanten (Hunderte oder Tausende) bereitstellt, die ihre Daten untereinander nicht freigeben oder sehen. Eine typische Kategorie eines solchen Diensts ist eine SaaS-Anwendung, mit der Dienste für Mandanten in einer in der Cloud gehosteten Umgebung bereitgestellt werden.

## Mehrinstanzenfähige Anwendungen

Mehrinstanzenfähige Anwendungen sind ein gutes Beispiel für eine Art von Anwendung, deren Daten und Workloads leicht partitioniert werden können. Bei mehrinstanzenfähigen Anwendungen können Daten und Workloads beispielsweise meist entlang von Mandantengrenzen partitioniert werden, da die meisten Anforderungen innerhalb der Grenzen eines Mandanten durchgeführt werden. Diese Eigenschaft gilt für die Daten und die Workload und ist eine gute Grundlage für die Anwendungsmuster, die im restlichen Teil des Artikels beschrieben werden.

Anwendungen dieser Art finden sich im gesamten Spektrum cloudbasierter Anwendungen, z.B.:
- ISV-Datenbankanwendungen, die als SaaS-Anwendungen in die Cloud verlagert werden
- SaaS-Anwendungen, die von Grund auf für die Cloud erstellt wurden
- Anwendungen für Konsumenten/Endbenutzer
- Unternehmensanwendungen für Mitarbeiter

Sowohl für die Cloud entwickelte SaaS-Anwendungen als auch in ISV-Datenbankanwendungen verankerte SaaS-Anwendungen führen häufig zu mehrinstanzenfähigen Anwendungen. Mit diesen SaaS-Anwendungen wird eine spezialisierte Softwareanwendung als Service für die Mandanten bereitgestellt. Mandanten haben Zugriff auf den Anwendungsdienst und sind die alleinigen Besitzer der zugehörigen Daten, die im Rahmen der Anwendung gespeichert werden. Um die Vorteile von SaaS nutzen zu können, müssen die Mandanten aber einen Teil der Kontrolle über ihre Daten abgeben und dem SaaS-Anbieter vertrauen, dass diese sicher aufbewahrt und von den Daten anderer Mandanten isoliert werden. Gängige Beispiele hierfür sind MyOB, SnelStart, Salesforce usw. All diese Anwendungen ermöglichen die Partitionierung entlang von Mandantengrenzen und unterstützen daher die Anwendungsmuster, die in den folgenden Abschnitten dieses Artikels beschrieben werden.

Anwendungen, die einen direkten Service für Konsumenten oder Mitarbeiter einer Organisation (häufig nicht als Mandanten bezeichnet, sondern als Benutzer) darstellen, bilden eine weitere Kategorie von mehrinstanzenfähigen Anwendungen. Kunden abonnieren den Dienst und sind nicht die Besitzer der Daten, die vom Service Provider gesammelt und gespeichert werden. Für Service Provider gelten weniger strenge Anforderungen in Bezug auf die Isolierung der Kundendaten (bis auf von Behörden vorgegebene Datenschutzregeln). Gängige Beispiele hierfür sind Anbieter von Medieninhalten, z.B. Netflix, Spotify, Xbox Live usw. Weitere Beispiele für leicht partitionierbare Anwendungen sind kundenorientierte Internetanwendungen oder IoT-Anwendungen (Internet of Things), bei denen jeder Kunde oder jedes Gerät als Partition dienen kann und zwischen unterschiedlichen Benutzern oder Geräten Partitionsgrenzen gezogen werden können.

Nicht alle Anwendungspartitionen lassen sich aber einfach anhand einer einzelnen Eigenschaft erkennen, z.B. Mandant, Kunde, Benutzer oder Gerät. Denken Sie beispielsweise an komplexe ERP-Anwendungen (Enterprise Resource Planning) mit Produkten, Bestellungen und Kunden. Sie verfügen meist über ein komplexes Schema mit Tausenden von verknüpften Tabellen. Es gibt keine einzelne Partitionsstrategie, mit der alle Tabellen abgedeckt werden können und die über die gesamte Workload hinweg funktioniert. Daher schließen wir diese Anwendungen aus den Beschreibungen in diesem Artikel ausdrücklich aus.

Die Entwurfsmuster mit Mehrinstanzenfähigkeit, die wir in den folgenden Abschnitten erläutern, gelten für alle obigen Anwendungsmuster mit leicht partitionierbaren Daten und Workloads. Der Einfachheit halber wird aber durchgängig die Bezeichnung „mehrinstanzenfähige SaaS-Anwendung“ verwendet.

## Mehrinstanzenfähige Anwendungen – Vor- und Nachteile des Entwurfs

Für Entwickler, die in der Cloud mehrinstanzenfähige Anwendungen erstellen, sind folgende Punkte am wichtigsten:

-	***Mandantenisolation***: Entwickler müssen sicherstellen, dass Mandanten keinen unerwünschten Zugriff auf die Daten anderer Mandanten erhalten. Diese Isolationsanforderung gilt auch für andere Bereiche, z.B. den Schutz vor „Noisy Neighbors“, die Möglichkeit zur Wiederherstellung von Daten eines Mandanten, mandantenspezifische Anpassungen usw.
-	***Kosten für Cloudressourcen***: Die SaaS-Anwendung muss in Bezug auf die Kosten konkurrenzfähig sein. Aus diesem Grund streben Entwickler von SaaS-Anwendungen eine Optimierung auf einen geringeren Kostenaufwand für die Ressourcennutzung in der Cloud an (Compute, Storage usw.), wenn sie ihre mehrinstanzenfähigen Anwendungen entwickeln.
-	***DevOps-Einfachheit***: Anbieter von mehrinstanzenfähigen Anwendungen müssen einen Isolationsschutz erstellen, die Anwendung und das Datenbankschema verwalten, die Integrität überwachen und die Probleme der Mandanten beheben. Die Komplexität der Entwicklung und des Betriebs von Anwendungen führt direkt zu Mehrkosten und einer geringeren Zufriedenheit der Mandanten.
-	***Skalierbarkeit***: Die Möglichkeit zum inkrementellen Hinzufügen von weiteren Mandanten ist für den erfolgreichen Betrieb einer SaaS-Anwendung von entscheidender Bedeutung. Dies gilt auch für das Hinzufügen von weiterer Kapazität für einzelne Mandanten, die mehr Ressourcen benötigen.

Diese Punkte sind jeweils mit Vor- und Nachteilen verbunden. Das kostengünstigste Cloudangebot bietet nicht unbedingt auch die benutzerfreundlichste Entwicklungsumgebung. Wir erleben es häufig, dass Entwickler in Bezug auf die vier oben genannten Punkte wenig fundierte Entscheidungen treffen.

Für mehrinstanzenfähige SaaS-Anwendungen, die für andere Unternehmen eingesetzt werden, ist die Mandantenisolation oft eine Grundvoraussetzung. Entwickler lassen sich häufig von den scheinbaren Vorteilen verleiten und vernachlässigen aus Gründen der Einfachheit und Kostenersparnis die Mandantenisolation und Skalierbarkeit. Dies kann sich als komplex und teuer herausstellen, wenn der Umfang des Diensts zunimmt und die Anforderungen an die Mandantenisolation wichtiger werden und auf Anwendungsebene erfüllt werden müssen.

Für mehrinstanzenfähige Anwendungen, die für Endbenutzer einen direkt konsumentenorientierten Dienst bereitstellen, hat die Mandantenisolation unter Umständen eine geringere Priorität als die Optimierung der Kosten für die Cloudressourcen.

Ein gängiges Entwicklungsmuster besteht darin, mehrere Mandanten zusammen in einer oder mehreren Datenbanken anzuordnen. Die (scheinbaren) Vorteile dieses Ansatzes sind geringere Kosten, da für eine niedrigere Anzahl von Datenbanken gezahlt werden muss, und die Einfachheit, indem mit so wenig Datenbanken wie möglich gearbeitet wird. Für mehrinstanzenfähige SaaS-Anwendungen ergeben sich bei einer Entscheidung dieser Art im Laufe der Zeit erhebliche Nachteile in Bezug auf die Mandantenisolation und Skalierbarkeit. Wenn die Mandantenisolation nicht benötigt wird, fällt weiterer Aufwand dafür an, die Mandantendaten im gemeinsamen Speicher vor unberechtigtem Zugriff oder „Noisy Neighbors“ zu schützen. Dies kann zu einem erheblichen Anstieg des Aufwands für die Anwendungsentwicklung und der Kosten für die Isolationswartung führen. Außerdem sind beim Hinzufügen neuer Mandanten für die richtige Skalierung der Datenebene einer Anwendung dieser Art meist spezielle Entwicklungskenntnisse erforderlich, um die Mandantendaten auf die Datenbanken zu verteilen.

## Mehrinstanzenfähige Datenmodelle 

Allgemeine Entwurfspraktiken zum Anordnen von Mandantendaten richten sich nach diesen drei Modellen.


  ![Mehrinstanzenfähige Datenmodelle](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-multi-tenant-data-models.png) Abbildung 1: Allgemeine Entwurfspraktiken für mehrinstanzenfähige Datenmodelle
  
1.	***Datenbank pro Mandant***: Bei diesem Ansatz wird jeder Mandant in einer eigenen Datenbank angeordnet. Alle mandantenspezifischen Daten sind auf seine Datenbank beschränkt und von anderen Mandanten und ihren Daten isoliert.
2.	***Gemeinsame Datenbank – Sharding***: Bei diesem Ansatz werden mehrere Datenbanken verwendet, und mehrere Mandanten teilen sich eine Datenbank. Mit einer Partitionierungsstrategie wird jeder Datenbank eine Gruppe von Mandanten zugewiesen, z.B. Hash-, Bereichs- oder Listenpartitionierung. Diese Strategie der Datenverteilung wird häufig als „Sharding“ bezeichnet.
3.	***Gemeinsame Datenbank – Einzeln***: Bei diesem Ansatz wird eine einzelne und ggf. große Datenbank mit Daten für alle Mandanten verwendet. Zur Unterscheidung ist eine Spalte mit der Mandanten-ID vorhanden.
  
> [AZURE.NOTE] Es kann auch sein, dass unterschiedliche Mandanten unter verschiedenen Datenbankschemas angeordnet werden, wobei der Schemaname zur Unterscheidung zwischen den verschiedenen Mandanten genutzt wird. Dies ist keine empfohlene Vorgehensweise, da normalerweise dynamischer SQL-Code verwendet werden muss und die Planzwischenspeicherung nicht effektiv eingesetzt werden kann. Daher liegt der Schwerpunkt dieses Artikels in dieser Kategorie nun auf dem Ansatz mit der gemeinsamen Tabelle.
 
## Beschreibung gängiger mehrinstanzenfähiger Datenmodelle

Beim Auswerten dieser mehrinstanzenfähigen Datenmodelle ist es wichtig, die Vor- und Nachteile in Bezug auf den Anwendungsentwurf zu beachten, die im vorherigen Abschnitt erläutert wurden.

-	***Isolation***: Der Grad der Isolation zwischen Mandanten als Maß dafür, inwieweit die Mandantenisolation erreicht wird.
-	***Kosten für Cloudressourcen***: Der Umfang der gemeinsamen Nutzung von Ressourcen durch Mandanten, um die Kosten für die Cloudressourcen zu optimieren. Eine Ressource kann in Form von Compute- und Speicherkosten definiert werden.
-	***DevOps-Kosten***: Die Gesamtkosten für den SaaS-Betrieb können durch eine einfache Anwendungsentwicklung, -bereitstellung und -verwaltbarkeit reduziert werden.

Anhand dieser Punkte können wir die weiter oben beschriebenen mehrinstanzenfähigen Datenmodelle und die damit verbundene Datenbanknutzung anhand des Quadranten unten in Abbildung 2 charakterisieren. Der Grad der Mandantenisolation und der Umfang der gemeinsamen Ressourcennutzung sind auf der Y- bzw. X-Achse des Bereichs aufgeführt. Der große diagonale Pfeil in der Mitte zeigt die DevOps-Kosten an.

![Gängige Muster](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-popular-application-patterns.png) Abbildung 2: Beschreibung gängiger mehrinstanzenfähiger Datenmodelle

Der in Abbildung 2 unten rechts angeordnete Quadrant verdeutlicht die Verwendung einer potenziell großen gemeinsamen Datenbank zusammen mit dem Ansatz der gemeinsam genutzten Tabelle (oder eines separaten Schemas) als eines der Anwendungsmuster. Die Ressourcen können hierbei gut gemeinsam genutzt werden, da alle Mandanten dieselben Datenbankressourcen (CPU, Arbeitsspeicher und E/A) in einer Einzeldatenbank verwenden. Die Mandantenisolation ist aber eingeschränkt. Es müssen zusätzliche Schritte ausgeführt werden, um Mandanten auf Anwendungsebene voreinander zu schützen. Dies kann zu einer deutlichen Erhöhung der DevOps-Kosten bei der Entwicklung und Verwaltung der Anwendung führen. Außerdem ist die Skalierbarkeit durch den Umfang der Hardware eingeschränkt, die zum Hosten der Datenbank eingesetzt wird.

Im mittleren Bereich sind mehrere Mandanten dargestellt, die per Sharding auf unterschiedliche Datenbanken verteilt sind (meist unterschiedliche Hardwareskalierungseinheiten), wobei jede Datenbank eine Teilmenge der Mandanten hostet. Hierdurch werden die Skalierbarkeitsanforderungen aus dem vorherigen Muster erfüllt. Wenn neue Kapazität für neue Mandanten erforderlich ist, können diese leicht in neuen Datenbanken angeordnet werden, die neuen Hardwareskalierungseinheiten zugeordnet sind. Der Umfang der gemeinsamen Nutzung von Ressourcen ist aber begrenzt, da nur die Mandanten in denselben Skalierungseinheiten ihre Ressourcen gemeinsam nutzen. Außerdem bietet dieser Ansatz nur eine geringfügige Verbesserung gegenüber der Mandantenisolation, da viele Mandanten an denselben Orten zusammengefasst werden, ohne dass sie automatisch vor den Aktionen anderer Mandanten geschützt sind. Die Komplexität der Anwendung bleibt somit hoch. Dieser Ansatz wird häufig als „Sharding“ bezeichnet.

Im Quadranten oben links in Abbildung 2 ist der dritte Ansatz dargestellt. Hierbei wird jeder Mandant in einer eigenen Datenbank angeordnet. Der Ansatz verfügt über gute Eigenschaften für die Mandantenisolation, aber die gemeinsame Nutzung von Ressourcen ist stark eingeschränkt, wenn jede Datenbank ihre eigenen dedizierten Ressourcen bereitstellt. Dieser Ansatz eignet sich gut, wenn alle Mandanten vorhersagbare Workloads aufweisen. Falls die Mandantenworkloads aber nicht mehr gut vorhersagbar sind (häufigster Fall für SaaS), kann die gemeinsame Nutzung von Ressourcen nicht optimiert werden. Der Anbieter muss entweder zu viele Ressourcen bereitstellen, um den Bedarf zu decken, oder die Ressourcen reduzieren, was entweder zu höheren Kosten oder einer geringeren Zufriedenheit der Mandanten führt. Aus diesen Gründen wird eine stärkere gemeinsame Nutzung von Ressourcen durch die Mandanten wünschenswert, um die Kosteneffektivität der Lösung zu erhöhen. Wenn die Anzahl von Datenbanken erhöht wird, erhöhen sich auch die DevOps-Kosten für die Bereitstellung und Wartung der Anwendung. Wir werden im nächsten Abschnitt noch einmal auf diese Punkte eingehen. Sie können sich aber merken, dass diese Methode gegenüber den anderen Methoden die beste und einfachste Isolation für Mandanten bietet.

Die Wahl des Entwurfsmusters durch Kunden wird auch von diesen zusätzlichen Faktoren beeinflusst:

-	***Eigentümerschaft der Mandantendaten***: Wenn die Daten der Mandanten in ihrem Besitz bleiben, spricht dies für das Muster mit einer Datenbank pro Mandant.
-	***Skalierung***: Wenn die Anwendung für Hunderttausende oder Millionen von Mandanten bestimmt ist, spricht dies für Ansätze mit gemeinsamer Datenbanknutzung, z.B. Sharding. Die Anforderungen an die Isolation können aber trotzdem eine Herausforderung darstellen.
-	***Wert- und Geschäftsmodell***: Wenn der Umsatz pro Mandant sehr klein ist (weniger als 1 USD), sind die Isolationsanforderungen weniger wichtig, und die Verwendung einer gemeinsamen Datenbank ist sinnvoll. Falls der Umsatz ein paar US-Dollar oder mehr beträgt, ist die Verwendung einer Datenbank pro Mandant möglich, und die DevOps-Kosten für die Anwendung können reduziert werden.

Basierend auf den in Abbildung 2 dargestellten Vor- und Nachteilen lässt sich Folgendes festhalten: Ein ideales mehrinstanzenfähiges Modell verfügt über hervorragende Eigenschaften für die Mandantenisolation mit optimaler gemeinsamer Nutzung von Ressourcen durch die Mandanten. Dies ist das im Quadranten oben rechts dargestellte Modell.

## Unterstützung der Mehrinstanzenfähigkeit mit Azure SQL-Datenbank

Azure SQL-Datenbank unterstützt alle mehrinstanzenfähigen Anwendungsmuster, die in Abbildung 2 dargestellt sind. Mit Pools für elastische Datenbanken wird jetzt noch ein neues Anwendungsmuster unterstützt, bei dem eine gute gemeinsame Nutzung von Ressourcen mit den Isolationsvorteilen des Ansatzes „Eine Datenbank pro Mandant“ kombiniert ist (Quadrant oben rechts in Abbildung 3). Die Tools und Funktionen von elastischen Datenbanken in Azure SQL-Datenbank wirken sich positiv auf die DevOps-Kosten aus, wenn eine Anwendung mit vielen Datenbanken entwickelt und betrieben wird (schattierter Bereich in Abbildung 3). Diese Tools dienen als Hilfe beim Erstellen und Verwalten von Anwendungen, für die eines dieser Muster mit mehreren Datenbanken genutzt wird. In den folgenden Unterabschnitten werden die Funktionen der SQL-Datenbank und die damit verbundene Unterstützung für diese mehrinstanzenfähigen Modelle ausführlicher beschrieben.

![Muster der SQL-Datenbank](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-patterns-sqldb.png) Abbildung 3: Mehrinstanzenfähige Anwendungsmuster mit Azure SQL-Datenbank

## Modell „Eine Datenbank pro Mandant“ mit elastischen Pools und Tools

Azure SQL-Datenbank stellt „Pools für elastische Datenbanken“ bereit, um den Ansatz „Eine Datenbank pro Mandant“ besser zu unterstützen. Hierbei wird die Mandantenisolation mit der gemeinsamen Ressourcennutzung zwischen Datenbanken kombiniert. Die Lösung ist als Lösung auf Datenebene ausgelegt, mit der SaaS-Anbieter mehrinstanzenfähige Anwendungen erstellen können. Bei der Kombination mit Tools für elastische Datenbanken ist die Mehrinstanzenfähigkeit integriert. Die Last der gemeinsamen Ressourcennutzung durch Mandanten wird so aus der Anwendung auf die Datenbankdienstebene verschoben. Die Komplexität der datenbankübergreifenden Verwaltung bzw. Durchführung von Abfragen bei guter Skalierbarkeit wird durch elastische Aufträge, Abfragen, Transaktionen und die Clientbibliothek für elastische Datenbanken vereinfacht.

| Anwendungsanforderungen | Funktionen der SQL-Datenbank |
| ------------------------ | ------------------------- |
| Mandantenisolation und gemeinsame Ressourcennutzung | [Pools für elastische Datenbanken:](sql-database-elastic-pool.md) Mit diesem Feature können Sie einen Pool mit SQLDB-Ressourcen zuordnen und die Ressourcen für mehrere Datenbanken freigeben. Einzelne Datenbanken können außerdem so viele Ressourcen wie benötigt aus dem Pool abrufen, um Kapazitätsspitzen aufgrund von Änderungen der Mandantenworkloads abzudecken. Der elastische Pool selbst kann je nach Bedarf zentral hoch- oder herunterskaliert werden. Elastische Pools ermöglichen darüber hinaus eine einfache Verwaltbarkeit, Überwachung und Problembehandlung auf Poolebene. |
| Einfache DevOps-Abläufe über Datenbanken hinweg | [Pools für elastische Datenbanken:](sql-database-elastic-pool.md) Wie oben aufgeführt.|
||[Elastische Abfrage:](sql-database-elastic-query-horizontal-partitioning.md) Ermöglicht Abfragen über mehrere Datenbanken hinweg für die Berichterstellung oder mandantenübergreifende Analysen.|
||[Elastische Aufträge:](sql-database-elastic-jobs-overview.md) Ermöglichen das Verpacken und zuverlässige Bereitstellen von Datenbank-Wartungsvorgängen oder Datenbank-Schemaänderungen für mehrere Datenbanken.|
||[Elastische Transaktionen:](sql-database-elastic-scale.md) Ermöglichen das Verarbeiten von Änderungen an mehreren Datenbanken auf atomische und isolierte Weise. Dies ist erforderlich, wenn Anwendungen für eine Reihe von Datenbankvorgängen Garantien der Art „Alles oder nichts“ benötigen. |
||[Clientbibliothek für elastische Datenbanken:](sql-database-elastic-database-client-library.md) Diese Funktion ermöglicht das Verwalten von Datenverteilungen und das Zuordnen von Mandanten zu Datenbanken. |
||||

## Modelle mit gemeinsamer Nutzung

Wie bereits beschrieben, können sich bei Ansätzen mit „Modellen mit gemeinsamer Nutzung“ für die meisten SaaS-Anbieter Probleme mit der Mandantenisolation und komplexe Anforderungen an die Anwendungsentwicklung und -wartung ergeben. Bei mehrinstanzenfähigen Anwendungen, mit denen ein Dienst direkt für Endkunden bereitgestellt wird, haben Anforderungen in Bezug auf die Mandantenisolation ggf. nicht eine so hohe Priorität wie die Optimierung der Kosten. Mandanten werden unter Umständen mit sehr hoher Dichte in einer oder mehreren Datenbanken angeordnet, um die Kosten zu senken. Bei Modellen mit gemeinsamer Datenbanknutzung, bei denen eine Einzeldatenbank oder mehrere Datenbanken mit Sharding verwendet werden, können sich eine Effizienzsteigerung in Bezug auf die gemeinsame Nutzung von Ressourcen und eine Reduzierung der Gesamtkosten ergeben. Azure SQL-Datenbank verfügt über einige Features, mit denen Kunden die Isolation einrichten können, um auf Datenebene für Sicherheit und eine Verwaltung mit guter Skalierbarkeit zu sorgen.

| Anwendungsanforderungen | Funktionen der SQL-Datenbank |
| ------------------------ | ------------------------- |
| Features für die Sicherheitsisolation | [Sicherheit auf Zeilenebene](https://msdn.microsoft.com/library/dn765131.aspx) |
|| [Datenbankschema](https://msdn.microsoft.com/library/dd207005.aspx) |
| Einfache DevOps-Abläufe über Datenbanken hinweg | [Elastische Abfrage](sql-database-elastic-query-horizontal-partitioning.md) |
|| [Elastische Aufträge](sql-database-elastic-jobs-overview.md) |
|| [Elastische Transaktionen](sql-database-elastic-transactions-overview.md) |
|| [Clientbibliothek für elastische Datenbanken](sql-database-elastic-database-client-library.md) |
|| [Trennen/Zusammenführen von elastischen Datenbanken](sql-database-elastic-scale-overview-split-and-merge.md) |
||||

## Zusammenfassung

Die Mandantenisolation ist für die meisten mehrinstanzenfähigen SaaS-Anwendungen eine sehr wichtige Anforderung. Für die Umsetzung der Isolation eignet sich am besten der Ansatz „Eine Datenbank pro Mandant“. Die beiden anderen Ansätze sind mit umfangreichen Investitionen auf Anwendungsebene verbunden, und es werden erfahrene Entwickler benötigt, um die Isolation einzurichten. Hierdurch können sich die Kosten und das Risiko deutlich erhöhen. Wenn Isolationsanforderungen nicht zu einem frühen Zeitpunkt der Dienstentwicklung berücksichtigt werden, kann deren nachträgliche Einrichtung sogar teurer als bei den ersten beiden Modellen werden. Die Hauptnachteile des Modells „Eine Datenbank pro Mandant“ sind höhere Kosten für Cloudressourcen aufgrund einer geringeren gemeinsamen Nutzung und der Pflege und Verwaltung einer großen Zahl von Datenbanken. Für Entwickler von SaaS-Anwendungen ist es häufig nicht leicht, in Bezug auf diese Vor- und Nachteile die richtigen Entscheidungen zu treffen.

Die Vor- und Nachteile können für die meisten Anbieter von Clouddatenbanken eine große Hürde darstellen, aber der Azure SQL-Datenbankdienst beseitigt diese Hürden mit seinem „Pool für elastische Datenbanken“ und den „Funktionen für elastische Datenbanken“. SaaS-Entwickler können die Isolationsmerkmale des Modells „Eine Datenbank pro Mandant“ kombinieren und die gemeinsame Nutzung von Ressourcen optimieren und die verbesserte Verwaltbarkeit einer großen Zahl von Datenbanken mit elastischen Pools und den dazugehörigen Tools nutzen.

Für Anbieter von mehrinstanzenfähigen Anwendungen, bei denen die Mandantenisolation nicht erforderlich ist und die Mandanten mit sehr hoher Dichte in einer Datenbank anordnen können, um Kosten zu sparen, können Modelle mit gemeinsamer Nutzung zu einer effizienteren Ressourcennutzung und einer Reduzierung der Gesamtkosten führen. Die Tools für elastische Datenbanken, das Sharding von Bibliotheken und die Sicherheitsfeatures von Azure SQL-Datenbank unterstützen SaaS-Anbieter beim Erstellen und Verwalten dieser mehrinstanzenfähigen Anwendungen.

## Nächste Schritte

Unter [Erste Schritte mit Tools für elastische Datenbanken](sql-database-elastic-scale-get-started.md) finden Sie eine Beispiel-App zur Veranschaulichung der Clientbibliothek.

Unter [Elastic Pool Custom Dashboard for Saas](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools-custom-dashboard) (Benutzerdefiniertes Dashboard für SaaS mit elastischem Pool) finden Sie eine Beispiellösung für ein SaaS-Szenario (Software-as-a-Service), bei dem über elastische Pools ein kostengünstiges, skalierbares Datenbank-Back-End für eine SaaS-Anwendung bereitgestellt wird.

Informationen zum Umwandeln von vorhandenen Datenbanken für die Verwendung der Tools finden Sie unter [Migrieren von vorhandenen Datenbanken zu horizontaler Hochskalierung](sql-database-elastic-convert-to-use-elastic-tools.md).

Informationen zum Erstellen eines neuen elastischen Pools finden Sie in [diesem Tutorial](sql-database-elastic-pool-create-portal.md).

Informationen zum Überwachen und Verwalten eines Pools für elastische Datenbanken finden Sie unter [Überwachen und Verwalten eines Pools für elastische Datenbanken über das Azure-Portal](sql-database-elastic-pool-manage-portal.md).

## Zusätzliche Ressourcen

- [Was ist ein Azure-Pool für elastische Datenbanken?](sql-database-elastic-pool.md)
- [Übersicht über Features für elastische Datenbanken](sql-database-elastic-scale-introduction.md)
- [Mehrinstanzenfähige Anwendungen mit elastischen Datenbanktools und zeilenbasierter Sicherheit](sql-database-elastic-tools-multi-tenant-row-level-security.md)
- [Authentication in multitenant apps, using Azure AD and OpenID Connect (Authentifizierung in mehrinstanzenfähigen Apps mithilfe von Azure AD und OpenID Connect)](../guidance/guidance-multitenant-identity-authenticate.md)
- [Tailspin-Anwendung „Surveys“](../guidance/guidance-multitenant-identity-tailspin.md)
- [Schnellstartlösungen](sql-database-solution-quick-starts.md)

## Fragen und Funktionswünsche

Bei Fragen erreichen Sie uns im [SQL-Datenbankforum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted), Featureanforderungen können Sie im [SQL-Datenbank-Feedbackforum](https://feedback.azure.com/forums/217321-sql-database/) einreichen.









	

<!---HONumber=AcomDC_0629_2016-->