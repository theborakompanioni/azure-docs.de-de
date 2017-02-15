---
title: Grundlegendes zu Microservices | Microsoft-Dokumentation
description: "Sie erhalten einen Überblick, warum das Erstellen von Cloudanwendungen mit einem Microservice-Ansatz für die moderne Anwendungsentwicklung wichtig ist und wie Azure Service Fabric dafür eine Plattform bereitstellt."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: fae2be85-0ab4-4cd3-9d1f-e0d95fe1959b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/20/2016
ms.author: mfussell
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 9a3a4424e04597e2a06ea4a5df421bc511bc899b


---
# <a name="why-a-microservices-approach-to-building-applications"></a>Gründe für einen Microservice-Ansatz zum Erstellen von Anwendungen
Für Softwareentwickler gibt es nichts Neues, was das Einbeziehen einer Anwendung in Komponententeile betrifft. Es handelt sich hierbei um ein zentrales Paradigma der Objektorientierung, von Softwareabstrahierungen und der „Komponentisierung“. Heute erfolgt diese Faktorisierung meist in Form von Klassen und Schnittstellen zwischen freigegebenen Bibliotheken und Technologieebenen. In der Regel wird ein mehrstufiger Ansatz mit einem Back-End-Speicher, einer Geschäftslogik auf der mittleren Ebene und einer Front-End-Benutzeroberfläche verwendet. Was sich im Laufe der letzten Jahre *doch* geändert hat, ist die Tatsache, dass wir als Entwickler verteilte Anwendungen erstellen, die für die Cloud bestimmt und geschäftlich geboten sind.

Veränderte geschäftliche Anforderungen sind:

* Ein Dienst, der erstellt und bedarfsorientiert betrieben wird, um (beispielsweise) Kunden in neuen geografischen Regionen zu erreichen.
* Schnellere Bereitstellung von Features und Funktionen, damit flexibel auf Kundenanforderungen reagiert werden kann.
* Verbesserte Ressourcenauslastung zur Kostenreduzierung

Diese geschäftlichen Anforderungen beeinflussen, *wie* wir Anwendungen erstellen.

Weitere Informationen zum Azure-Ansatz in Bezug auf Microservices finden Sie unter [Microservices: An application revolution powered by the cloud](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/)(Microservices: Eine cloudbasierte Revolution für Anwendungen).

## <a name="monolithic-vs-microservice-design-approach"></a>Vergleich des monolithischen Entwurfs und Microservice-Entwurfs
Alle Anwendungen entwickeln sich im Laufe der Zeit weiter. Erfolgreiche Anwendungen entwickeln sich weiter, indem Sie für die Benutzer nützlich sind. Nicht erfolgreiche Anwendungen werden nicht weiterentwickelt und schließlich ausgesondert. Es stellt sich nun folgende Frage: Wie viel wissen Sie über Ihre heutigen Anforderungen und deren zukünftige Entwicklung? Angenommen, Sie erstellen eine Berichterstellungsanwendung für eine Abteilung. Sie sind sicher, dass die Anwendung nur innerhalb Ihres Unternehmens verwendet wird und die Berichte nur eine kurze Lebensdauer haben. In diesem Fall wählen Sie einen anderen Ansatz als beispielsweise bei der Erstellung eines Diensts zur Bereitstellung von Videoinhalten für Millionen von Kunden. 

In einigen Fällen ist der treibende Faktor, dass ein Projekt als Machbarkeitsstudie in dem Wissen erstellt werden kann, dass der Entwurf der Anwendung später geändert werden kann. Es ist nicht sehr sinnvoll, zu viel Engineeringaufwand für etwas zu betreiben, was niemals verwendet wird. Dies ist der übliche Kompromiss, der beim Engineering häufig eingegangen wird. Wenn Unternehmen von der Erstellung für die Cloud sprechen, werden aber auch Wachstum und gute Auslastung erwartet. Das Problem hierbei ist, dass Wachstums- und Skalierungsanforderungen nicht vorhersagbar sind. Am liebsten würden wir schnell Prototypen erstellen können und gleichzeitig die Sicherheit haben, dass wir uns auf einem Weg zu zukünftigem Erfolg befinden. Dies entspricht dem schlanken Startup-Ansatz: Erstellen, Messen, Lernen und Wiederholen.

Während der Client/Server-Ära haben wir eher Anwendungen mit mehreren Ebenen erstellt, indem auf jeder Ebene spezielle Technologie verwendet wurde. Hierfür hat sich der Begriff *monolithische* Anwendung etabliert. Die Schnittstellen befanden sich meist zwischen den Ebenen, und innerhalb der einzelnen Ebenen wurde ein Entwurf mit engerer Kopplung zwischen den Komponenten genutzt. Die Entwickler haben also Klassen entworfen und eingebunden, die zu Bibliotheken kompiliert und über einige EXE- und DLL-Dateien verknüpft wurden. 

Dieser monolithische Entwurfsansatz hat auch Vorteile. Er ist häufig einfacher zu entwerfen und verfügt über schnellere Aufrufe zwischen Komponenten, da diese Aufrufe oft per prozessübergreifender Kommunikation (Interprocess Communication, IPC) durchgeführt werden. Außerdem wird jeweils ein einzelnes Produkt getestet, was in Bezug auf den Personalaufwand meist effizienter ist. Der Nachteil ist, dass eine enge Kopplung zwischen mehrstufigen Ebenen entsteht und Sie einzelne Komponenten nicht skalieren können. Wenn Sie Fixes oder Upgrades durchführen müssen, müssen Sie warten, bis andere Personen ihre Tests abgeschlossen haben. Dadurch ist es schwieriger, flexibel vorzugehen.

Microservices stellen die Antwort auf diese Nachteile dar, und sie sind enger an die obigen geschäftlichen Anforderungen angelehnt. Aber auch sie haben sowohl Vor- als auch Nachteile. Der Vorteil von Microservices ist, dass jeder Microservice normalerweise einfachere Geschäftsfunktionen kapselt, die Sie einzeln zentral hoch- und herunterskalieren, testen, bereitstellen und verwalten können. Ein wichtiger Vorteil eines Microservice-Ansatzes ist, dass Teams anders als beim mehrstufigen Ansatz stärker durch Geschäftsszenarien als durch Technologie beeinflusst werden. In der Praxis entwickeln kleinere Teams basierend auf einem Kundenszenario unter Verwendung beliebiger Technologien ihrer Wahl einen Microservice. 

Anders ausgedrückt: Das Unternehmen muss Technologie nicht zur Verwaltung monolithischer Anwendungen standardisieren. Einzelne für Dienste zuständige Teams können die Vorgehensweise wählen, die aufgrund des Know-hows im Team sinnvoll oder am besten zur Behebung des Problems geeignet ist. In der Praxis ist eine Gruppe empfohlener Technologien vorzuziehen, z.B. ein bestimmter NoSQL-Speicher oder ein Webanwendungsframework.

Der Nachteil von Microservices sind die Verwaltung der höheren Anzahl separater Entitäten und die höhere Komplexität von Bereitstellungen und der Versionsverwaltung. Der Netzwerkdatenverkehr zwischen Microservices und auch die entsprechenden Netzwerklatenzen nehmen zu. Eine große Anzahl von sehr präzisen Diensten mit vielen Einzelaufrufen kann in Bezug auf die Leistung schnell zu einem Alptraum werden. Und ohne Tools zum Anzeigen dieser Abhängigkeiten ist es nicht einfach, das gesamte System zu „sehen“. 

Der Microservice-Ansatz funktioniert aufgrund von Standards. Anstatt starre Verträge zu nutzen, wird vereinbart, wie kommuniziert wird, und es werden nur die Dinge toleriert, die Sie von einem Dienst benötigen. Da Dienste unabhängig voneinander aktualisiert werden, ist es wichtig, diese Verträge im Voraus im Entwurf festzulegen. „Serviceorientierte Architektur (SOA) mit feiner Abstufung“ ist eine weitere Beschreibung, die für das Entwerfen mit einem Microservice-Ansatz gilt.

***Im einfachsten Fall geht es beim Microservice-Entwurfsansatz um einen entkoppelten Verbund von Diensten, an denen Änderungen unabhängig voneinander vorgenommen werden und für die bestimmte Kommunikationsstandards vereinbart sind.***

Mit der steigenden Zahl von Cloud-Apps stellen immer mehr Entwickler fest, dass diese Zerlegung der gesamten App in unabhängige Dienste für bestimmte Szenarien langfristig den besseren Ansatz darstellt.

## <a name="comparison-between-application-development-approaches"></a>Vergleich zwischen Ansätzen zur Anwendungsentwicklung
![Service Fabric-Plattform: Anwendungsentwicklung][Image1]

* Eine monolithische App enthält domänenspezifische Funktionen und ist normalerweise in Funktionsebenen unterteilt, z. B. Web, Business und Daten.
* Sie skalieren monolithische Apps, indem Sie sie über mehrere Server, VMs bzw. Container hinweg klonen.
* Bei einer Microservice-Anwendung werden die Funktionen auf separate kleinere Dienste aufgeteilt.
* Beim Microservice-Ansatz erfolgt das horizontale Hochskalieren, indem jeder Dienst unabhängig bereitgestellt wird und Instanzen dieser Dienste über Server, VMs bzw. Container hinweg erstellt werden.

Das Entwerfen eines Microservice-Ansatzes ist kein Patentrezept für alle Projekte. Dieser Ansatz ist jedoch enger an die oben beschriebenen geschäftlichen Ziele angelehnt. Die anfängliche Verwendung eines monolithischen Ansatzes kann akzeptabel sein, wenn Sie wissen, dass Sie später nicht die Gelegenheit haben, den Code bei Bedarf zu überarbeiten und in einen Microservice-Entwurf zu ändern. Normalerweise beginnen Sie mit einer monolithischen App und teilen diese dann nach und nach in Phasen auf. Sie beginnen hierbei mit den Funktionsbereichen, die besser skalierbar oder flexibler werden sollen.

Zusammenfassend lässt sich sagen, dass der Microservice-Ansatz darin besteht, Ihre Anwendung aus vielen kleinen Diensten zusammenzusetzen. Die Dienste werden in Containern ausgeführt, die auf einem Cluster mit Computern bereitgestellt werden. Kleinere Teams entwickeln Dienste für bestimmte Szenarien. Jeder dieser Dienste wird einzeln getestet, versioniert, bereitgestellt und skaliert, um die gesamte Anwendung weiterzuentwickeln.

## <a name="what-is-a-microservice"></a>Was ist ein Microservice?
Es gibt verschiedene Definitionen von Microservices. Im Internet finden Sie viele nützliche Ressourcen, die eigene Sichtweisen und Definitionen enthalten. Die meisten der folgenden Merkmale von Microservices werden aber immer genannt:

* Kapseln eines Kunden- oder Geschäftsszenarios Welches Problem möchten Sie lösen?
* Durchführung der Entwicklung in einem kleinen Engineering-Team
* Verwendung einer beliebigen Programmiersprache und eines beliebigen Frameworks
* Zusammensetzung aus Code und (optional) einem Zustand, die beide unabhängig voneinander versioniert, bereitgestellt und skaliert werden
* Interaktion mit anderen Microservices über gut definierte Schnittstellen und Protokolle
* Eindeutige Namen (URLs), die zum Auflösen ihres Speicherorts verwendet werden
* Aufrechterhaltung der Konsistenz und Verfügbarkeit beim Auftreten von Fehlern

Diese Merkmale lassen sich wie folgt zusammenfassen:

***Microservice-Anwendungen bestehen aus kleinen, einzeln versionierten und skalierbaren Diensten für Kunden, die über Standardprotokolle mit gut definierten Schnittstellen miteinander kommunizieren.***

Nachdem wir die ersten beiden Punkte im vorherigen Abschnitt behandelt haben, werden wir dies nun weiter ausführen und die restlichen Punkte klären.

### <a name="written-in-any-programming-language-and-use-any-framework"></a>Verwendung einer beliebigen Programmiersprache und eines beliebigen Frameworks
Als Entwickler sollten Sie frei wählen können, welche Sprache oder welches Framework Sie verwenden möchten, und zwar je nach Ihren Fähigkeiten bzw. den Anforderungen des Diensts. Bei einigen Diensten kann es sein, dass die Leistungsvorteile von C++ im Vordergrund stehen. Bei anderen Diensten ist dagegen die Einfachheit der verwalteten Entwicklung in C# oder Java am wichtigsten. In einigen Fällen müssen Sie unter Umständen eine bestimmte Partnerbibliothek, Datenspeichertechnologie oder Einrichtung zum Verfügbarmachen des Diensts für Clients verwenden.

Nachdem Sie sich für eine Technologie entschieden haben, müssen Sie sich mit der Betriebs- oder Lebenszyklusverwaltung und der Skalierung des Diensts befassen.

### <a name="allows-code-and-state-to-be-independently-versioned-deployed-and-scaled"></a>Code und Zustand mit unabhängiger Versionierung, Bereitstellung und Skalierung
Unabhängig davon, wie Sie Ihre Microservices schreiben, sollten der Code und optional der Zustand getrennt voneinander bereitgestellt, aktualisiert und skaliert werden. Dies ist eines der schwierigeren Probleme, die gelöst werden müssen, da es hierbei um die Wahl der Technologie geht. Im Hinblick auf die Skalierung stellt das Verständnis der Partitionierung (Sharding) des Codes und Zustands eine Herausforderung dar. Wenn für den Code und den Zustand separate Technologien verwendet werden, was heutzutage häufiger der Fall ist, müssen die Bereitstellungsskripts für Ihren Microservice die Skalierung von beidem bewältigen können. Es geht auch um die Flexibilität, nur einige Microservices aktualisieren zu können, ohne dass alle auf einmal aktualisiert werden müssen.

Wir kehren noch einmal kurz zum Vergleich des monolithischen Ansatzes mit dem Microservice-Ansatz zurück. Das folgende Diagramm zeigt die Unterschiede beim Speichern des Zustands.

#### <a name="state-storage-between-application-styles"></a>Speichern des Zustands für unterschiedliche Anwendungsstile
![Service Fabric-Plattform: Zustandsspeicherung][Image2]

***Der monolithische Ansatz auf der rechten Seite enthält eine Einzeldatenbank und die spezifischen Technologieebenen.***

***Der Microservice-Ansatz auf der rechten Seite verfügt über einen Graphen mit verbundenen Microservices, wobei der Zustand meist auf den Microservice beschränkt ist und verschiedene Technologien verwendet werden.***

Bei einem monolithischen Ansatz wird von der Anwendung in der Regel eine Einzeldatenbank genutzt. Der Vorteil besteht darin, dass ein einzelner Speicherort verwendet und so die Bereitstellung vereinfacht wird. Jede Komponente kann eine einzelne Tabelle zum Speichern des Zustands enthalten. Die Teams müssen den Zustand strikt trennen. Dies ist eine ziemliche Herausforderung. Die Versuchung, einer vorhandenen Kundentabelle eine neue Spalte hinzuzufügen, Tabellen zu verknüpfen und Abhängigkeiten auf der Speicherebene zu erstellen, ist natürlich groß. Wenn dies passiert, können Sie einzelne Komponenten nicht skalieren. 

Beim Microservice-Ansatz verwaltet und speichert jeder Dienst seinen eigenen Zustand. Jeder Dienst ist dafür verantwortlich, Code und Zustand gemeinsam zu skalieren, um die Anforderungen des Diensts zu erfüllen. Dadurch ergibt sich allerdings der Nachteil, dass Sie zum Erstellen von Sichten oder Abfragen für die Daten Ihrer Anwendung verteilte Zustandsspeicher abfragen müssen. Normalerweise wird dies mit einem separaten Microservice gelöst, mit dem eine Sicht für eine Sammlung von Microservices erstellt wird. Falls Sie für die Daten mehrere Ad-hoc-Abfragen durchführen müssen, sollten Sie für jeden Microservice erwägen, dass dessen Daten für die Offlineanalyse in einen Data Warehousing-Dienst geschrieben werden.

Die Versionsverwaltung ist insbesondere für die bereitgestellte Version eines Microservice erforderlich, damit mehrere unterschiedliche Versionen bereitgestellt und parallel ausgeführt werden können. Die Versionsverwaltung unterstützt Szenarien, in denen für eine neuere Version eines Microservice während des Upgrades ein Fehler auftritt und ein Rollback zu einer vorherigen Version durchgeführt werden muss. Das andere Szenario für die Versionsverwaltung ist die Durchführung von A/B-Tests, bei denen unterschiedliche Benutzer unterschiedliche Versionen des Diensts erhalten. Es ist beispielsweise üblich, einen Microservice für eine bestimmte Gruppe von Kunden zu aktualisieren, um neue Funktionen vor dem größeren Rollout zu testen. Nach der Lebenszyklusverwaltung von Microservices geht es nun mit der Kommunikation weiter.

### <a name="interacts-with-other-microservices-over-well-defined-interfaces-and-protocols"></a>Interagiert mit anderen Microservices über gut definierte Schnittstellen und Protokolle
Auf dieses Thema muss hier nicht näher eingegangen werden, da Kommunikationsmuster in der umfangreichen Literatur beschrieben werden, die im Bereich der dienstorientierten Architektur in den letzten zehn Jahren veröffentlicht wurde. Im Allgemeinen wird für die Dienstkommunikation ein REST-Ansatz mit HTTP- und TCP-Protokollen sowie XML oder JSON als Serialisierungsformat eingesetzt. Aus Sicht der Benutzeroberfläche geht es hierbei um den Webdesignansatz. Es hindert Sie aber nichts daran, binäre Protokolle oder eigene Datenformate zu verwenden. Seien Sie darauf vorbereitet, dass die Verwendung Ihrer Microservices für Benutzer schwieriger ist, wenn diese öffentlich verfügbar sind.

### <a name="has-a-unique-name-url-used-to-resolve-its-location"></a>Eindeutiger Name (URL), der zum Auflösen des Speicherorts verwendet wird
Wir erwähnen immer wieder, dass der Microservice-Ansatz Ähnlichkeiten mit dem Web aufweist. Wie das Web auch, muss Ihr Microservice unabhängig vom Ort der Ausführung adressierbar sein. Wenn Sie anfangen, sich Gedanken über Computer und den Ort der Ausführung bestimmter Microservices zu machen, nimmt dies kein gutes Ende. 

Ebenso wie der DNS eine bestimmte URL in einen bestimmten Computer auflöst, muss Ihr Microservice über einen eindeutigen Namen verfügen, damit sein aktueller Speicherort ermittelt werden kann. Für Microservices sind adressierbare Namen erforderlich, damit sie unabhängig von der Infrastruktur sind, auf der sie ausgeführt werden. Dies impliziert, dass eine Interaktion zwischen der Bereitstellung und der Ermittlung Ihres Diensts besteht, weil eine Dienstregistrierung vorhanden sein muss. In gleicher Weise muss der Registrierungsdienst Sie im Fall eines Computerausfalls darüber informieren, wo der Dienst jetzt ausgeführt wird. 

Dies bringt uns zum nächsten Thema: Resilienz (Ausfallsicherheit) und Konsistenz.

### <a name="remains-consistent-and-available-in-the-presence-of-failures"></a>Bleibt beim Auftreten von Fehlern konsistent und verfügbar
Die Behandlung unerwarteter Fehler ist eines der schwierigsten Probleme, die in einem verteilten System gelöst werden müssen. Ein Großteil des Codes, den wir als Entwickler schreiben, dient dem Behandeln von Ausnahmen. Hierfür fällt auch der meiste Testaufwand an. Das Problem ist komplexer als das Schreiben von Code zum Behandeln von Fehlern. Was passiert, wenn der Computer ausfällt, auf dem der Microservice ausgeführt wird? Sie müssen diesen Microservice-Fehler nicht nur entdecken (was schon allein ein Problem darstellt), sondern Sie benötigen auch einen Mechanismus, mit dem der Microservice neu gestartet wird. 

Ein Microservice muss ausfallsicher sein, und aus Verfügbarkeitsgründen muss es möglich sein, ihn häufig auf einem anderen Computer neu zu starten. Hierbei ist auch der Zustand wichtig, der für den Microservice gespeichert wurde, wo der Microservice den Zustand wiederherstellen kann und ob der Microservice den Neustart erfolgreich durchführen kann. Anders ausgedrückt: Die Ausfallsicherheit muss sowohl für den Computebereich (der Prozess wird neu gestartet) als auch für den Zustand oder die Daten (kein Datenverlust und Wahrung der Datenkonsistenz) gelten.

Die mit der Ausfallsicherheit verbundenen Probleme werden bei anderen Szenarien noch verstärkt, z. B. beim Auftreten von Fehlern während eines Anwendungsupgrades. Der Microservice, der mit dem Bereitstellungssystem eingesetzt wird, muss nicht wiederhergestellt werden können. Es muss auch entschieden werden, ob der Schritt zur neueren Version möglich ist oder stattdessen ein Rollback auf die vorherige Version durchgeführt werden muss, um die Konsistenz sicherzustellen. Beispielsweise müssen folgende Fragen beantwortet werden: Sind genügend Computer verfügbar, um die neue Version einzuführen, und wie werden vorherige Versionen des Microservice wiederhergestellt? Vom Microservice müssen Informationen zur Integrität bereitgestellt werden, damit diese Entscheidungen getroffen werden können.

### <a name="reports-health-and-diagnostics"></a>Melden der Integrität und Diagnose
Es ist eigentlich offensichtlich und wird doch oft übersehen, dass der Microservice seine Integrität und den Diagnosezustand melden muss. Andernfalls sind aus betrieblicher Sicht nur wenige Einblicke möglich. Die Herausforderung besteht darin, Diagnoseereignisse in einer Gruppe von unabhängigen Diensten zu korrelieren und Abweichungen der Computeruhren zum Ermitteln der richtigen Ereignisreihenfolge zu berücksichtigen. Genauso wie Sie mit einem Microservice über vereinbarte Protokolle und Datenformate interagieren, ist eine Standardisierung für die Protokollierung von Integritäts- und Diagnoseereignissen erforderlich, die letztendlich zur Abfrage und Anzeige in einem Ereignisspeicher gespeichert werden. Bei einem Microservice-Ansatz ist es wichtig, dass sich die unterschiedlichen Teams auf ein gemeinsames Protokollierungsformat einigen. In der gesamten Anwendung muss ein einheitlicher Ansatz zum Anzeigen von Diagnoseereignissen verwendet werden.

Die Integrität unterscheidet sich von der Diagnose. Bei der Integrität geht es darum, dass der Microservice seinen aktuellen Zustand meldet, damit geeignete Maßnahmen ergriffen werden können. Ein gutes Beispiel ist die Verwendung der Upgrade- und Bereitstellungsmechanismen zum Wahren der Verfügbarkeit. Auch wenn ein Dienst aufgrund eines Prozessabsturzes oder eines Computerneustarts zu einem bestimmten Zeitpunkt unter Umständen keine volle Integrität aufweist, kann er trotzdem noch betriebsbereit sein. Sie sollten auf jeden Fall vermeiden, diesen Zustand zu verschlimmern, indem Sie ein Upgrade durchführen. Die beste Vorgehensweise besteht darin, zuerst eine Untersuchung durchzuführen oder dem Microservice Zeit zum Beheben des Fehlers zu lassen. Anhand von Integritätsereignissen eines Microservice können wir fundierte Entscheidungen treffen und selbstreparierende Dienste erstellen.

## <a name="service-fabric-as-a-microservices-platform"></a>Service Fabric als Microservice-Plattform
Azure Service Fabric ist entstanden, als Microsoft die Umstellung von „Paketprodukten“, die meist monolithisch waren, auf Dienste vollzogen hat. Aus den Erfahrungen, die bei der Erstellung und dem Betrieb großer Dienste wie Azure SQL-Datenbank und Azure DocumentDB gesammelt wurden, ist Service Fabric entstanden. Die Plattform entwickelte sich im Laufe der Zeit weiter und wurde von immer mehr Diensten genutzt. Es war wichtig, dass Service Fabric nicht nur in Azure ausgeführt werden konnte, sondern auch in eigenständigen Windows Server-Bereitstellungen.

***Die Ziele von Service Fabric sind das Lösen der schwierigen Probleme, die mit der Erstellung und Ausführung eines Diensts verbunden sind, und die effiziente Nutzung von Infrastrukturressourcen, damit Teams geschäftliche Probleme mit einem Microservice-Ansatz lösen können.***

Service Fabric verfügt über zwei grobe Bereiche für die Unterstützung der Anwendungserstellung mit einem Microservice-Ansatz:

* Eine Plattform, die Systemdienste zum Bereitstellen, Upgraden, Erkennen und Neustarten ausgefallener Dienste, Ermitteln des Dienstspeicherorts, Verwalten des Zustands und Überwachen der Integrität bietet. Viele der oben beschriebenen Merkmale von Microservices sind auf diese Systemdienste zurückzuführen.
* Programmier-APIs oder Frameworks zum Erstellen von Anwendungen als Microservices: [Reliable Actors und Reliable Services](service-fabric-choose-framework.md). Sie können natürlich beliebigen Code auswählen, um den Microservice zu erstellen. Aber wenn Sie diese APIs verwenden, wird die Aufgabe nicht nur vereinfacht, sondern es findet auch eine tiefere Integration in die Plattform statt. Beispielsweise erhalten Sie entweder Integritäts- und Diagnoseinformationen, oder Sie können die integrierte hohe Verfügbarkeit nutzen.

***Service Fabric ist agnostisch, was die Erstellung Ihres Diensts betrifft, und Sie können eine beliebige Technologie verwenden. Die Plattform verfügt aber über integrierte Programmier-APIs, die die Erstellung von Microservices vereinfachen.***

### <a name="are-microservices-right-for-my-application"></a>Sind Microservices für meine Anwendung geeignet?
Vielleicht. Als immer mehr Teams bei Microsoft damit begannen, aus geschäftlichen Gründen Anwendungen für die Cloud zu entwickeln, erkannten viele von ihnen die Vorteile eines Microservice-Ansatzes. Für Bing werden beispielsweise schon seit Jahren Microservices für die Suche entwickelt. Für andere Teams war der Microservice-Ansatz neu. Die Teams haben festgestellt, dass es schwierige Probleme außerhalb ihrer Kernkompetenzbereiche zu lösen galt. Dies sind die Gründe, warum Service Fabric immer häufiger als Technologie für die Erstellung von Diensten gewählt wird.

Das Ziel von Service Fabric ist die Reduzierung der komplexen Anwendungserstellung durch einen Microservice-Ansatz, damit bei Ihnen nicht mehr so viele teure Überarbeitungen anfallen. Der Ansatz ist wie folgt: Beginnen Sie klein, führen Sie bei Bedarf eine Skalierung durch, mustern Sie Dienste aus, fügen Sie neue hinzu, und entwickeln Sie die Lösung während der Nutzung durch die Kunden weiter. Uns ist auch bewusst, dass noch viele andere Probleme gelöst werden müssen, damit Microservices für die Mehrzahl der Entwickler noch besser zugänglich werden. Container und das Actor-Programmiermodell sind Beispiele für kleine Schritte in diese Richtung, und wir sind uns sicher, dass weitere Innovationen diesen Prozess noch stärker vereinfachen werden.
 
<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Nächste Schritte
* [Übersicht über Service Fabric-Terminologie](service-fabric-technical-overview.md)
* [Microservices: An application revolution powered by the cloud](https://azure.microsoft.com/en-us/blog/microservices-an-application-revolution-powered-by-the-cloud/) (Microservices: Eine cloudbasierte Anwendungsrevolution)

[Bild1]: media/service-fabric-overview-microservices/monolithic-vs-micro.png
[Bild2]: media/service-fabric-overview-microservices/statemonolithic-vs-micro.png



<!--HONumber=Nov16_HO3-->


