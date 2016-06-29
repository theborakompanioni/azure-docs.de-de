<properties
   pageTitle="Notfallwiederherstellung für Azure-Anwendungen | Microsoft Azure"
   description="Technische Übersicht und ausführliche Informationen zum Entwerfen von Anwendungen für die Notfallwiederherstellung in Microsoft Azure."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="hongfeig"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/18/2016"
   ms.author="hanuk;jroth;aglick"/>

#Notfallwiederherstellung für in Microsoft Azure erstellte Anwendungen

Hohe Verfügbarkeit bezieht sich auf den Umgang mit vorübergehenden Ausfällen. Bei der Notfallwiederherstellung (Disaster Recovery, DR) geht es dagegen um einen schwerwiegenden Verlust von Anwendungsfunktionalität. Nehmen wir als Beispiel ein Szenario mit einem Ausfall einer Region. In diesem Fall müssen Sie einen Plan zum Ausführen der Anwendung oder zum Zugreifen auf die Daten außerhalb der Azure-Region haben. Die Umsetzung dieses Plans beinhaltet Personen, Prozesse und unterstützende Anwendungen, die die Funktionstüchtigkeit des Systems ermöglichen. Die Unternehmens- und Technologieverantwortlichen, die den Betriebsmodus des Systems für Notfälle definieren, bestimmen auch den Umfang der Funktionalität für den Dienst bei einem Notfall. Für den Grad der Funktionalität sind viele Formen denkbar: überhaupt nicht verfügbar, teilweise verfügbar (beeinträchtigte Funktionalität oder verzögerte Verarbeitung) oder vollständig verfügbar.

##Azure-Funktionen für die Notfallwiederherstellung

Wie auch bei Überlegungen zur Verfügbarkeit gibt es in Azure einen [technischen Leitfaden zur Resilienz](./resiliency-technical-guidance.md), der zur Unterstützung bei der Notfallwiederherstellung dient. Es besteht auch eine Beziehung zwischen einigen der Verfügbarkeitsfunktionen von Azure und der Notfallwiederherstellung. Beispielsweise erhöht die Verwaltung von Rollen über Fehlerdomänen hinweg die Verfügbarkeit einer Anwendung. Ohne diese Verwaltung würde ein nicht behandelter Hardwarefehler zu einem „Notfall“ werden. Daher ist die richtige Anwendung von Funktionen und Strategien im Zusammenhang mit der Verfügbarkeit als ein wichtiger Teil beim Schutz Ihrer Anwendung vor Notfällen. In diesem Artikel werden allerdings nicht die Probleme mit der allgemeinen Verfügbarkeit behandelt, sondern schwerwiegendere (und seltenere) Notfälle.

##Mehrere Rechenzentrumsregionen

Azure betreibt Rechenzentren in vielen Regionen auf der ganzen Welt. Von dieser Infrastruktur werden unterschiedliche Notfallwiederherstellungsszenarien unterstützt, z.B. die vom System bereitgestellte Georeplikation von Azure Storage in sekundären Regionen. Dies bedeutet auch, dass Sie einfach und kostengünstig einen Clouddienst an mehreren Standorten auf der ganzen Welt bereitstellen können. Vergleichen Sie dies mit den Kosten und den Schwierigkeiten des Betriebs eigener Rechenzentren in mehreren Regionen. Das Bereitstellen von Daten und Diensten in mehreren Regionen hilft, Ihre Anwendung vor schwerwiegenden Ausfällen in einer Region zu schützen.

##Azure Traffic Manager

Wenn ein regionsspezifischer Ausfall auftritt, müssen Sie den Datenverkehr an Dienste oder Bereitstellungen in einer anderen Region umleiten. Diese Umleitung kann manuell erfolgen, aber es ist effizienter, einen automatisierten Prozess zu verwenden. Azure Traffic Manager wurde für diese Aufgabe entwickelt. Sie können damit das Failover von Benutzerdatenverkehr in eine andere Region automatisch verwalten, sollte die primäre Region ausfallen. Da die Datenverkehrsverwaltung einen bedeutenden Teil der Gesamtstrategie darstellt, ist es wichtig, die Grundlagen von Traffic Manager zu verstehen.

Im folgenden Diagramm stellen Benutzer eine Verbindung mit einer URL her, die für Traffic Manager angegeben ist (__http://myATMURL.trafficmanager.net__) und die tatsächlichen URLs der Website abstrahiert (__http://app1URL.cloudapp.net__ und \_\___http://app2URL.cloudapp.net__)). Abhängig davon, wie Sie die Kriterien für die Weiterleitung von Benutzern konfigurieren, werden sie an den richtigen tatsächlichen Standort weitergeleitet, wenn die Richtlinie dies vorgibt. Die Richtlinienoptionen sind Roundrobin, Leistung oder Failover. In diesem Artikel beschäftigen wir uns nur mit der Option „Failover“.

![Routing über Azure Traffic Manager](./media/resiliency-disaster-recovery-azure-applications/routing-using-azure-traffic-manager.png)

Beim Konfigurieren von Traffic Manager geben Sie ein neues Traffic Manager-DNS-Präfix an. Dies ist das URL-Präfix, das Sie den Benutzern für den Zugriff auf Ihren Dienst bereitstellen. Traffic Manager führt jetzt den Lastenausgleich eine Ebene weiter oben und nicht auf Regionsebene aus. Der Traffic Manager-DNS-Eintrag entspricht einem CNAME-Eintrag für alle von ihm verwalteten Bereitstellungen.

In Traffic Manager geben Sie die Priorität der Bereitstellungen an, zu denen Benutzer weitergeleitet werden, wenn ein Ausfall auftritt. Traffic Manager überwacht die Endpunkte der Bereitstellungen und bemerkt, wenn die primäre Bereitstellung ausfällt. Bei einem Ausfall analysiert Traffic Manager die priorisierte Liste der Bereitstellungen und leitet die Benutzer zur nächsten Bereitstellung in der Liste weiter.

Obwohl Traffic Manager entscheidet, wohin bei einem Failover die Umleitung erfolgt, können Sie entscheiden, ob Ihre Failoverdomäne inaktiv oder aktiv ist, wenn Sie sich nicht im Failovermodus befinden. Diese Funktionalität ist von Azure Traffic Manager unabhängig. Traffic Manager erkennt einen Fehler am primären Standort und führt ein Rollover an den Failoverstandort aus. Traffic Manager führt das Rollover unabhängig davon aus, ob dieser Standort gerade aktiv Benutzeranforderungen verarbeitet.

Weitere Informationen zur Funktionsweise von Azure Traffic Manager finden Sie in den folgenden Themen:

 * [Traffic Manager – Übersicht](../traffic-manager/traffic-manager-overview.md)
 * [Konfigurieren der Routingmethode „Failover“](../traffic-manager/traffic-manager-configure-failover-routing-method.md)

##Azure-Notfallszenarien

Die folgenden Abschnitte enthalten verschiedene Typen von Notfallszenarien. Regionsweite Dienstunterbrechungen sind nicht die einzige Ursache von anwendungsweiten Ausfällen. Ein schlechter Entwurf oder Verwaltungsfehler können auch zu Ausfällen führen. Es ist wichtig, die möglichen Ursachen eines Ausfalls während der Entwurfs- und Testphase des Wiederherstellungsplans zu berücksichtigen. Ein guter Plan nutzt die Vorteile der Azure-Funktionen und erweitert sie um anwendungsspezifische Strategien. Die ausgewählte Reaktion wird durch die Bedeutung der Anwendung, das RPO (Recovery Point Objective) und das RTO (Recovery Time Objective) vorgegeben.

###Anwendungsausfall

Azure Traffic Manager behandelt Ausfälle automatisch, die von der zugrunde liegenden Hardware oder Betriebssystemsoftware auf dem virtuellen Hostcomputer verursacht werden. Azure erstellt eine neue Instanz der Rolle auf einem funktionsfähigen Server und fügt sie der Lastenausgleichsrotation hinzu. Wenn die Anzahl der Rolleninstanzen größer als eins ist, verlagert Azure die Verarbeitung an die aktiven Rolleninstanzen, während der ausgefallene Knoten ersetzt wird.

Es gibt schwerwiegende Anwendungsfehler, die unabhängig von Hardware- oder Betriebssystemfehlern auftreten. Die Anwendung kann aufgrund der schwerwiegenden Ausnahmen ausfallen, die durch fehlerhafte Logik oder Probleme mit der Datenintegrität verursacht werden. Sie müssen genügend Telemetriedaten in den Code integrieren, damit ein Überwachungssystem Fehlerbedingungen erkennen und einen Anwendungsadministrator benachrichtigen kann. Ein Administrator, der die Notfallwiederherstellungsverfahren genau kennt, kann sich für das Starten eines Failoverprozesses entscheiden. Alternativ kann ein Administrator einfach einen Verfügbarkeitsausfall akzeptieren, um die kritischen Fehler zu beheben.

###Datenbeschädigung

Azure speichert Ihre Azure SQL-Datenbank und Azure Storage-Daten automatisch drei Mal redundant in verschiedenen Fehlerdomänen in derselben Region. Wenn Georeplikation verwendet wird, werden die Daten drei weitere Male in einer anderen Region gespeichert. Wenn ein Benutzer oder die Anwendung jedoch die Daten in der primären Kopie beschädigt, werden diese schnell in den anderen Kopien repliziert. Leider führt dies zu drei Kopien mit beschädigten Daten.

Um mit möglichen Beschädigungen von Daten umzugehen, haben Sie zwei Optionen. Erstens können Sie eine benutzerdefinierte Sicherungsstrategie verwalten. Abhängig von Ihren Geschäftsanforderungen oder Governancevorschriften können Sie die Sicherungen in Azure oder lokal speichern. Eine weitere Option ist die Verwendung der neuen Wiederherstellungsoption „Point-in-Time-Wiederherstellung“ für die SQL-Datenbank. Weitere Informationen finden Sie im Abschnitt zu [Datenstrategien für die Notfallwiederherstellung](#data-strategies-for-disaster-recovery).

###Netzwerkausfall

Wenn auf Teile des Azure-Netzwerks nicht zugegriffen werden kann, können Sie möglicherweise Ihre Anwendung oder Ihre Daten nicht abrufen. Wenn eine oder mehrere Rolleninstanzen aufgrund von Netzwerkproblemen nicht verfügbar sind, nutzt Azure die verbleibenden verfügbaren Instanzen der Anwendung. Wenn Ihre Anwendung aufgrund eines Azure-Netzwerkausfalls nicht auf die Daten zugreifen kann, können Sie lokal einen beeinträchtigten Modus mithilfe von zwischengespeicherten Daten nutzen. Sie müssen die Notfallwiederherstellungsstrategie für die Ausführung im beeinträchtigten Modus in Ihre Anwendung einbinden. Für einige Anwendungen ist dies möglicherweise nicht praktikabel.

Eine andere Möglichkeit ist, Daten an einem alternativen Speicherort zu speichern, bis die Verbindung wiederhergestellt wurde. Falls der beeinträchtigte Modus nicht möglich ist, sind die verbleibenden Optionen Ausfallzeiten der Anwendung oder ein Failover in eine alternative Region. Der Entwurf einer Anwendung zur Ausführung im beeinträchtigten Modus ebenso eine geschäftliche Entscheidung wie eine technische. Informationen dazu erhalten Sie im Abschnitt zu [beeinträchtigten Anwendungsfunktionen](#degraded-application-functionality).

###Ausfall eines abhängigen Diensts

Azure bietet viele Dienste, bei denen regelmäßig Ausfallzeiten auftreten können. Nehmen wir als Beispiel [Azure Redis Cache](https://azure.microsoft.com/services/cache/). Dieser mehrinstanzenfähige Dienst bietet Ihrer Anwendung Funktionen zum Zwischenspeichern. Sie müssen bedenken, was in Ihrer Anwendung passiert, wenn der abhängige Dienst nicht verfügbar ist. Dieses Szenario ist in vielerlei Hinsicht dem Szenario eines Netzwerkausfalls ähnlich. Wenn Sie allerdings jeden Dienst einzeln betrachten, erreichen Sie potenzielle Verbesserungen des globalen Plans.

Azure Redis Cache ermöglicht das Zwischenspeichern für Ihre Anwendung in der Clouddienstbereitstellung und bietet Vorteile für die Notfallwiederherstellung. Zunächst wird der Dienst jetzt mit Rollen ausgeführt, die für die Bereitstellung lokal sind. Aus diesem Grund können Sie den Status des Caches als Teil der globalen Verwaltungsprozesse für den Clouddienst besser überwachen und verwalten. Diese Art von Zwischenspeicherung bietet neue Funktionen. Eine der neuen Funktionen ist die hohe Verfügbarkeit für zwischengespeicherte Daten. Dadurch können die zwischengespeicherten Daten beibehalten werden, wenn ein einzelner Knoten ausfällt, da Duplikate auf anderen Knoten vorhanden sind.

Beachten Sie, dass durch die hohe Verfügbarkeit der Durchsatz verringert und die Latenz erhöht wird, da die sekundäre Kopie bei Schreibvorgängen aktualisiert wird. Zudem wird der Arbeitsspeicher verdoppelt, der für jedes Element genutzt wird. Planen Sie das also ein. Dieses Beispiel veranschaulicht, dass jeder abhängige Dienst Funktionen aufweisen kann, die die allgemeine Verfügbarkeit sowie die Absicherung gegen schwerwiegende Ausfälle verbessern.

Für jeden abhängigen Dienst sollten Sie die Auswirkungen einer Dienstunterbrechung kennen. Im Beispiel mit der Zwischenspeicherung können Sie möglicherweise auf die Daten direkt in einer Datenbank zugreifen, bis Sie Ihren Cache wiederherstellen. Dies wäre hinsichtlich der Leistung ein beeinträchtigter Modus, in Bezug auf die Daten hätten Sie jedoch die volle Funktionalität.

###Regionsweite Dienstunterbrechung

Die vorherigen Fehler waren in erster Linie Fehler, die innerhalb derselben Azure-Region bewältigt werden können. Allerdings müssen Sie sich auch auf die Möglichkeit vorbereiten, dass eine Dienstunterbrechung die gesamte Region betrifft. Bei einer regionsweiten Dienstunterbrechung sind die lokal redundanten Kopien Ihrer Daten nicht verfügbar. Wenn Sie die Georeplikation aktiviert haben, sind drei zusätzliche Kopien der Blobs und Tabellen in einer anderen Region vorhanden. Wenn Microsoft die Region als verloren deklariert, ordnet Azure alle DNS-Einträge der Region mit Georeplikation neu zu.

>[AZURE.NOTE] Denken Sie daran, dass Sie keine Kontrolle über diesen Prozess haben, der nur bei einer regionsweiten Dienstunterbrechung auftritt. Setzen Sie daher auf andere anwendungsspezifische Sicherungsstrategien, um eine möglichst hohe Verfügbarkeit zu erreichen. Weitere Informationen finden Sie im Abschnitt zu [Datenstrategien für die Notfallwiederherstellung](#data-strategies-for-disaster-recovery).

###Vollständige Dienstunterbrechung in Azure

Im Notfallplan müssen Sie die gesamte Bandbreite möglicher Notfälle abdecken. Eine der schwerwiegendsten Dienstunterbrechungen würde alle Azure-Regionen gleichzeitig betreffen. Wie bei anderen Ausfällen könnten Sie entscheiden, dass Sie in diesem Fall das Risiko einer vorübergehenden Ausfallzeit in Kauf nehmen. Ausgedehnte Dienstunterbrechungen, die mehrere Regionen umfassen, sollten viel seltener als isolierte Dienstunterbrechungen sein, die abhängige Dienste oder einzelnen Regionen betreffen.

Allerdings könnten Sie für einige unternehmenskritische Anwendungen entscheiden, dass auch für dieses Szenario ein Sicherungsplan vorhanden sein muss. Der Plan für dieses Ereignis kann ein Failover zu Diensten in einer [alternativen Cloud](#alternative-cloud) oder eine [hybride Lösungen aus lokaler Bereitstellung und Cloudbereitstellung](#hybrid-on-premises-and-cloud-solution) sein.

###Beeinträchtigte Anwendungsfunktionen

Eine überlegt entworfene Anwendung verwendet in der Regel eine Sammlung von Modulen, die über die Implementierung lose gekoppelter Informationsaustauschmuster miteinander kommunizieren. Eine Anwendung, die die Notfallwiederherstellung erleichtert, erfordert die Trennung von Aufgaben auf Modulebene. Dadurch wird verhindert, dass die gesamte Anwendung durch eine Unterbrechung eines abhängigen Diensts beendet wird. Betrachten Sie z.B. eine E-Commerce-Anwendung für Unternehmen Y, die aus den folgenden Modulen bestehen kann:

 * Der __Produktkatalog__ ermöglicht Benutzern, nach Produkten zu suchen.
 * Der __Einkaufswagen__ ermöglicht Benutzern, Produkte dem Einkaufwagen hinzuzufügen und daraus zu entfernen.
 * Der __Bestellstatus__ zeigt den Versandstatus von Benutzerbestellungen an.
 * Die __Übermittlung der Bestellung__ schließt die Einkaufssitzung ab, indem die Bestellung übermittelt und bezahlt wird.
 * Die __Auftragsverarbeitung__ überprüft die Bestellung auf Datenintegrität und die Verfügbarkeit der Bestellmenge.

Wenn ein abhängiges Element eines Moduls in dieser Anwendung ausfällt, wie funktioniert das Modul, bis dieses Element wiederhergestellt wurde? Ein System mit durchdachter Architektur implementiert durch die Trennung von Aufgaben zur Entwurfszeit und zur Laufzeit Isolationsgrenzen. Sie können jeden Fehler als behebbar oder nicht behebbar kategorisieren. Durch nicht behebbare Fehler fällt das Modul aus, aber Sie können einen behebbaren Fehler durch Alternativen ausgleichen. Wie im Abschnitt zur hohen Verfügbarkeit erläutert, können Sie einige Probleme vor den Benutzern verbergen, indem Sie Fehler behandeln und alternative Maßnahmen ergreifen. Bei einer schwerwiegenderen Dienstunterbrechung kann die Anwendung überhaupt nicht verfügbar sein. Eine dritte Option ist jedoch, die Benutzeranforderungen im beeinträchtigten Modus zu verarbeiten.

Beispiel: Wenn die Datenbank zum Hosten von Bestellungen ausfällt, kann das Modul für die Auftragsverarbeitung Verkaufstransaktionen nicht mehr verarbeiten. Je nach Architektur kann es schwierig oder unmöglich sein, die Anwendungskomponenten „Übermittlung der Bestellung“ und „Auftragsverarbeitung“ weiter zu nutzen. Wenn die Anwendung nicht zum Behandeln dieses Szenarios ausgelegt ist, kann die gesamte Anwendung offline geschaltet werden.

In diesem Szenario ist es jedoch möglich, dass die Produktdaten an einem anderen Speicherort gespeichert sind. In diesem Fall kann das Modul „Produktkatalog“ immer noch zum Anzeigen von Produkten verwendet werden. Im beeinträchtigten Modus ist die Anwendung weiterhin für verfügbare Funktionen wie das Anzeigen des Produktkatalogs für Benutzer verfügbar. Andere Teile der Anwendung sind jedoch nicht verfügbar, z.B. Bestellungen und Bestandsabfragen.

Eine andere Variante des beeinträchtigten Modus bezieht sich auf die Leistung und nicht auf die Funktionen. Stellen Sie sich beispielsweise ein Szenario vor, in dem der Produktkatalog mithilfe von Azure Redis Cache zwischengespeichert wird. Wenn der Cachedienst nicht mehr verfügbar ist, ruft Anwendung Produktkataloginformationen möglicherweise direkt aus dem Serverspeicher ab. Dieser Zugriff ist aber möglicherweise langsamer als der auf die zwischengespeicherte Version. Aus diesem Grund ist die Anwendungsleistung beeinträchtigt, bis der Cachedienst vollständig wiederhergestellt ist.

Die Entscheidung, in welchem Umfang eine Anwendung im beeinträchtigten Modus weiterhin funktionsfähig ist, ist sowohl eine geschäftliche als auch eine technische Entscheidung. Die Anwendung muss auch entscheiden, wie die Benutzer über vorübergehende Probleme informiert werden. In diesem Beispiel kann die Anwendung die Anzeige von Produkten und sogar das Hinzufügen zu einem Einkaufswagen ermöglichen. Wenn der Benutzer allerdings versucht, einen Kauf zu tätigen, benachrichtigt die Anwendung den Benutzer, dass auf das Verkaufsmodul vorübergehend nicht zugegriffen werden kann. Dies ist nicht ideal für den Kunden, verhindert aber eine anwendungsweite Dienstunterbrechung.

##Datenstrategien für die Notfallwiederherstellung

Die richtige Behandlung von Daten ist der schwierigste Bereich in einem Notfallwiederherstellungsplan. Das Wiederherstellen von Daten ist auch der Teil der Wiederherstellung, der in der Regel am längsten dauert. Verschiedene Auswahlmöglichkeiten für beeinträchtigte Modi führen zu schwierigen Herausforderungen für die Datenwiederherstellung und die Konsistenz nach einem Ausfall.

Einer der Faktoren ist die Notwendigkeit, eine Kopie der Daten der Anwendung wiederherzustellen oder zu verwalten. Sie verwenden diese Daten als Verweis und für Transaktionen an einem sekundären Standort. Eine lokale Einstellung erfordert eine arbeits- und zeitaufwendige Planung, um eine Notfallwiederherstellungsstrategie für mehrere Regionen zu implementieren. Praktischerweise ermöglichen die meisten Cloudanbieter, einschließlich Azure, eine einfache Bereitstellung von Anwendungen in mehreren Regionen. Diese Regionen sind geografisch so verteilt, dass eine Dienstunterbrechung in mehreren Regionen nur sehr selten vorkommen sollte. Die Strategie zum Verarbeiten von Daten über Regionen hinweg ist einer der entscheidenden Faktoren für den Erfolg eines jeden Notfallwiederherstellungsplans.

In den folgenden Abschnitten werden die Notfallwiederherstellungsmethoden im Zusammenhang mit Datensicherungen, Verweisdaten und Transaktionsdaten erläutert.

###Sichern und Wiederherstellen

Regelmäßige Sicherungen von Anwendungsdaten können einige Szenarien der Notfallwiederherstellung unterstützen. Für verschiedene Speicherressourcen sind unterschiedliche Verfahren erforderlich.

Für die SQL-Datenbanktarife Basic, Standard und Premium können Sie die Point-in-Time-Wiederherstellung zur Wiederherstellung der Datenbanken nutzen. Weitere Informationen finden Sie unter [Übersicht: Geschäftskontinuität für die Cloud und Notfallwiederherstellung für Datenbanken mit SQL-Datenbank](../sql-database/sql-database-business-continuity.md). Eine andere Möglichkeit ist die Verwendung der aktiven Georeplikation für die SQL-Datenbank. Damit werden Datenbankänderungen automatisch in sekundären Datenbanken in derselben Azure-Region oder sogar in einer anderen Azure-Region repliziert. Dies ist eine mögliche Alternative für einige der eher manuellen Datensynchronisierungsmethoden, die in diesem Artikel erläutert werden. Weitere Informationen finden Sie unter [Übersicht: Aktive Georeplikation in Azure SQL-Datenbank](../sql-database/sql-database-geo-replication-overview.md).

Sie können auch einen stärker manuellen Ansatz für die Sicherung und Wiederherstellung verwenden. Verwenden Sie den Befehl für Datenbankkopien, um eine Kopie der Datenbank zu erstellen. Sie müssen diesen Befehl verwenden, um eine Sicherung mit Transaktionskonsistenz zu erhalten. Sie können auch den Import/Export-Dienst von Azure SQL-Datenbank nutzen. Er unterstützt das Exportieren von Datenbanken in BACPAC-Dateien, die im Azure-Blobspeicher gespeichert werden.

Die integrierte Redundanz von Azure Storage erstellt zwei Replikate der Sicherungsdatei in einer Region. Jedoch bestimmt die Häufigkeit der Ausführung des Sicherungsvorgangs Ihre RPO, also den Umfang an Daten, die Sie bei einem Notfall verlieren können. Beispiel: Angenommen, Sie führen eine Sicherung zur vollen Stunde aus, und der Notfall tritt zwei Minuten vorher ein. Sie verlieren die Daten von 58 Minuten, die seit der letzten Sicherung vergangen sind. Darüber hinaus sollten Sie zum Schutz vor einer regionsweiten Dienstunterbrechung die BACPAC-Dateien in eine alternative Region kopieren. Sie können dann diese Sicherungen in der alternativen Region wiederherstellen. Weitere Details finden Sie unter [Übersicht: Geschäftskontinuität für die Cloud und Notfallwiederherstellung für Datenbanken mit SQL-Datenbank](../sql-database/sql-database-business-continuity.md).

Für Azure Storage können Sie Ihren eigenen benutzerdefinierten Sicherungsvorgang entwickeln. Sie können aber auch eines der vielen Sicherungstools von Drittanbietern verwenden. Beachten Sie, dass in den meisten Anwendungsentwürfen, in denen Speicherressourcen aufeinander verweisen, zusätzliche Komplexität vorliegt. Stellen Sie sich beispielsweise eine SQL-Datenbank vor, die über eine Spalte mit einem Link zu einem Blob in Azure Storage verfügt. Wenn die Sicherungen nicht gleichzeitig erfolgen, enthält die Datenbank möglicherweise einer Zeiger auf ein Blob, das vor dem Ausfall nicht gesichert wurde. Die Anwendung oder der Notfallwiederherstellungsplan muss Prozesse implementieren, damit diese Inkonsistenz nach einer Wiederherstellung behandelt wird.

###Verweisdatenmuster für die Notfallwiederherstellung

Verweisdaten sind schreibgeschützte Daten, die die Anwendungsfunktionalität unterstützen. Sie werden in der Regel nicht häufig geändert. Sicherung und Wiederherstellung stellen eine Methode zum Behandeln von regionsweiten Dienstunterbrechungen dar, jedoch ist die RTO relativ lang. Wenn Sie die Anwendung in einer sekundären Region bereitstellen, gibt es einige Strategien, die RTO für Verweisdaten zu verbessern.

Da Verweisdaten nur selten geändert werden, können Sie die RTO verbessern, indem Sie eine dauerhafte Kopie der Verweisdaten in der sekundären Region verwalten. Dadurch wird der Zeitaufwand für die Wiederherstellung von Sicherungen bei einem Notfall beseitigt. Um die Anforderungen der Notfallwiederherstellung in mehreren Regionen zu erfüllen, müssen Sie die Anwendung und die Verweisdaten zusammen in mehreren Regionen bereitstellen. Wie unter [Verweisdatenmuster für hohe Verfügbarkeit](./resiliency-high-availability-azure-applications.md#reference-data-pattern-for-high-availability) dargestellt, können Sie Verweisdaten in der Rolle selbst, in einem externen Speicher oder in einer Kombination aus beidem bereitstellen.

Mit dem Modell zur Bereitstellung von Verweisdaten in Computeknoten werden die Anforderungen an die Notfallwiederherstellung implizit erfüllt. Die Bereitstellung von Verweisdaten in der SQL-Datenbank erfordert, dass Sie eine Kopie der Verweisdaten in jeder Region bereitstellen. Die gleiche Strategie gilt für Azure Storage. Sie müssen eine Kopie aller Verweisdaten, die in Azure Storage gespeichert sind, in der primären und den sekundären Regionen bereitstellen.

![Veröffentlichung von Verweisdaten in primären und sekundären Regionen](./media/resiliency-disaster-recovery-azure-applications/reference-data-publication-to-both-primary-and-secondary-regions.png)

Sie müssen Ihre eigenen anwendungsspezifischen Sicherungsroutinen für alle Daten, einschließlich Verweisdaten, implementieren. Regionsübergreifende georeplizierte Kopien werden nur bei einer regionsweiten Dienstunterbrechung verwendet. Um eine längere Ausfallzeit zu verhindern, stellen Sie die unternehmenskritischen Teile der Anwendungsdaten in der sekundären Region bereit. Ein Beispiel dieser Topologie ist das [Aktiv/Passiv-Modell](#active-passive).

###Transaktionsdatenmuster für die Notfallwiederherstellung

Die Implementierung einer vollständig funktionsfähigen Strategie für den Notfallmodus erfordert die asynchrone Replikation der Transaktionsdaten in der sekundären Region. Die Zeitfenster, in denen die Replikation erfolgen kann, bestimmen die RPO-Eigenschaften der Anwendung. Möglicherweise können Sie die Daten wiederherstellen, die in der primären Region während des Replikationsfensters verloren gegangen sind. Auch eine spätere Zusammenführung mit der sekundären Region kann möglich sein.

Die folgenden Architekturbeispiele vermitteln Ihnen einige Ideen der verschiedenen Arten, mit Transaktionsdaten in einem Failoverszenario umzugehen. Sie müssen beachten, dass diese Beispiele nicht vollständig sind. Zwischengeschaltete Speicherorte wie Warteschlangen können z.B. durch Azure SQL-Datenbank ersetzt werden. Die Warteschlangen selbst können Azure Storage- oder Azure Service Bus-Warteschlangen sein (siehe [Azure-Warteschlangen und Azure Service Bus-Warteschlangen – Vergleich und Gegenüberstellung](../service-bus/service-bus-azure-and-service-bus-queues-compared-contrasted.md)). Serverspeicherziele können auch variieren, z.B. Azure-Tabellen anstelle einer SQL-Datenbank. Darüber hinaus werden möglicherweise Workerrollen als Vermittler in verschiedene Schritte eingefügt. Wichtig ist, diese Architekturen nicht genau nachzubilden, sondern die diversen Alternativen beim Wiederherstellen von Transaktionsdaten und zugehörigen Modulen zu prüfen.

####Replikation von Transaktionsdaten zur Vorbereitung der Notfallwiederherstellung

Nehmen Sie eine Anwendung, die Azure Storage-Warteschlangen zum Speichern von Transaktionsdaten verwendet. Dadurch können Workerrollen die Transaktionsdaten in der Serverdatenbank in einer entkoppelten Architektur verarbeiten. Dann muss für Transaktionen eine Form der temporären Zwischenspeicherung genutzt werden, wenn die Front-End-Rollen die Daten direkt abfragen müssen. Je nach Grad der Datenverlusttoleranz können Sie wählen, ob die Warteschlangen, die Datenbank oder sämtliche Speicherressourcen repliziert werden sollen. Wenn Sie nur die Datenbank replizieren: Fällt die primäre Region aus, können Sie weiterhin die Daten in den Warteschlangen wiederherstellen, sobald die primäre Region wieder verfügbar ist.

Das folgende Diagramm zeigt eine Architektur, in der die Serverdatenbank über Regionen hinweg synchronisiert wird.

![Replikation von Transaktionsdaten zur Vorbereitung der Notfallwiederherstellung](./media/resiliency-disaster-recovery-azure-applications/replicate-transactional-data-in-preparation-for-disaster-recovery.png)

Die größte Herausforderung beim Implementieren dieser Architektur ist die Strategie für die Replikation zwischen Regionen. Der Azure SQL-Datensynchronisierungsdienst ermöglicht diese Art der Replikation. Jedoch befindet sich dieser Dienst noch in der Vorschauphase und sollte nicht in Produktionsumgebungen verwendet werden. Weitere Informationen finden Sie unter [Übersicht: Geschäftskontinuität für die Cloud und Notfallwiederherstellung für Datenbanken mit SQL-Datenbank](../sql-database/sql-database-business-continuity.md). Für Produktionsanwendungen müssen Sie in eine Lösung eines Drittanbieters investieren oder Ihre eigene Logik für die Replikation im Code erstellen. Abhängig von der Architektur kann die Replikation möglicherweise bidirektional sein, also auch komplexer.

Bei einer möglichen Implementierung kann die zwischengeschaltete Warteschlange aus dem vorherigen Beispiel verwendet werden. Die Workerrolle, die die Daten verarbeitet, um das endgültige Speicherziel zu finden, kann die Änderung in der primären und sekundären Region vornehmen. Dies sind keine einfachen Aufgaben, doch eine vollständige Anleitung im Zusammenhang mit dem Replikationscode ist nicht Gegenstand dieses Artikels. Der wichtigste Punkt ist, dass Sie einen Großteil Ihrer Zeit und Tests darauf konzentrieren sollten, wie die Daten in der sekundären Region repliziert werden. Zusätzliche Verarbeitungsschritte und Tests können helfen sicherzustellen, dass die Failover- und Wiederherstellungsprozesse jede mögliche Dateninkonsistenz und doppelte Transaktionen ordnungsgemäß verarbeiten.

>[AZURE.NOTE] Dieser Artikel beschäftigt sich schwerpunktmäßig mit PaaS (Platform as a Service). Es gibt jedoch zusätzliche Replikations- und Verfügbarkeitsoptionen für Hybridanwendungen, die Azure Virtual Machines verwenden. Diese Hybridanwendungen nutzen IaaS (Infrastructure as a Service), um SQL Server auf virtuellen Computern in Azure zu hosten. Dadurch werden herkömmliche Verfügbarkeitsansätze in SQL Server ermöglicht, z.B. AlwaysOn-Verfügbarkeitsgruppen oder Protokollversand. Einige Techniken, wie z.B. AlwaysOn, funktionieren nur zwischen lokalen SQL Server-Instanzen und virtuellen Azure-Computern. Weitere Informationen finden Sie unter [Hochverfügbarkeit und Notfallwiederherstellung für SQL Server in Azure Virtual Machines](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

####Beeinträchtigter Anwendungsmodus für die Transaktionserfassung

Prüfen Sie eine zweite Architektur, die im beeinträchtigten Modus ausgeführt wird. Die Anwendung in der sekundären Region deaktiviert alle Funktionen, z.B. die Berichterstellung, Business Intelligence (BI) oder das Leeren von Warteschlangen. Sie akzeptiert nur die wichtigsten Typen von Transaktionsworkflows, wie durch die Unternehmensanforderungen definiert. Das System erfasst die Transaktionen und schreibt sie in Warteschlangen. Das System kann die Verarbeitung der Daten in der Anfangsphase der Dienstunterbrechung aufschieben. Wenn das System in der primären Region innerhalb des erwarteten Zeitfensters reaktiviert wird, können die Workerrollen in der primären Region die Warteschlangen leeren. Dadurch entfällt die Notwendigkeit, die Datenbanken zusammenzuführen. Dauert die Dienstunterbrechung in der primären Region länger als das tolerierbare Zeitfenster, kann die Anwendung die Verarbeitung der Warteschlangen starten.

In diesem Szenario enthält die Datenbank in der sekundären Region inkrementelle Transaktionsdaten, die zusammengeführt werden müssen, nachdem die primäre Region reaktiviert wurde. Das folgende Diagramm zeigt diese Strategie für die temporäre Speicherung der Transaktionsdaten, bis die primäre Region wiederhergestellt ist.

![Beeinträchtigter Anwendungsmodus für die Transaktionserfassung](./media/resiliency-disaster-recovery-azure-applications/degraded-application-mode-for-transaction-capture.png)

Nähere Informationen zu Datenverwaltungstechniken für robuste Azure-Anwendungen finden Sie unter [Failsafe: Guidance for Resilient Cloud Architectures](https://channel9.msdn.com/Series/FailSafe) (Ausfallsicherheit: Leitfaden für robuste Cloudarchitekturen).

##Bereitstellungstopologien für die Notfallwiederherstellung

Sie müssen unternehmenskritische Anwendungen auf die Möglichkeit einer regionsweiten Dienstunterbrechung vorbereiten. Hierzu können Sie eine Strategie für die Bereitstellung in mehreren Regionen in die operative Planung einbinden.

Zu Bereitstellungen in mehreren Regionen können Prozesse von IT-Experten gehören, um die Anwendungs- und Verweisdaten nach einem Notfall in der sekundären Region zu veröffentlichen. Wenn die Anwendung ein sofortiges Failover erfordert, kann der Bereitstellungsprozess eine Aktiv/Passiv- oder eine Aktiv/Aktiv-Konfiguration umfassen. Zu dieser Art der Bereitstellung gehören vorhandene Instanzen der Anwendung, die in der alternativen Region ausgeführt werden. Ein Routingtool wie Azure Traffic Manager bietet Lastenausgleichsdienste auf DNS-Ebene. Es kann Dienstunterbrechungen erkennen und die Benutzer bei Bedarf an andere Regionen weiterleiten.

Teil einer erfolgreichen Azure-Notfallwiederherstellung ist, die Wiederherstellung von Beginn in die Lösung einzubinden. Die Cloud bietet zusätzliche Optionen für die Wiederherstellung bei Ausfällen während eines Notfalls, die bei einem herkömmlichen Hostinganbieter nicht verfügbar sind. Insbesondere können Sie schnell und dynamisch Ressourcen einer anderen Region zuordnen. Aus diesem Grund müssen Sie nicht viel für Ressourcen im Leerlauf bezahlen, während Sie auf einen Ausfall warten.

Die folgenden Abschnitte enthalten verschiedene Bereitstellungstopologien für die Notfallwiederherstellung. In der Regel führt zusätzliche Verfügbarkeit zu steigenden Kosten und mehr Komplexität.

###Bereitstellung in einer Region

Die Bereitstellung in einer einzelnen Region ist nicht wirklich eine Notfallwiederherstellungstopologie, aber sie dient als Vergleich mit anderen Architekturen. Bereitstellungen in einer einzelnen Region sind für Anwendungen in Azure verbreitet. Dieser Bereitstellungstyp stellt allerdings keine ernsthafte Alternative zu einem Notfallwiederherstellungsplan dar.

Das folgende Diagramm zeigt eine Anwendung, die in einer einzelnen Azure-Region ausgeführt wird. Azure Traffic Manager und die Verwendung von Fehler- und Upgradedomänen erhöhen die Verfügbarkeit der Anwendung innerhalb der Region.

![Bereitstellung in einer Region](./media/resiliency-disaster-recovery-azure-applications/single-region-deployment.png)

Hier wird ersichtlich, dass die Datenbank ein Single Point of Failure ist. Zwar repliziert Azure die Daten in verschiedenen Fehlerdomänen in internen Replikaten, dies erfolgt jedoch alles in derselben Region. Dies bietet der Anwendung keinen Schutz vor einem schwerwiegenden Fehler. Wenn die Region ausfällt, fallen alle Fehlerdomänen aus, die alle Dienstinstanzen und Speicherressourcen enthalten.

Sie müssen für alle Anwendungen – auch die weniger wichtigen – einen Plan zum Bereitstellen Ihrer Anwendungen in mehreren Regionen entwickeln. Sie sollten auch RTO und Kosteneinschränkungen bei den Überlegungen berücksichtigen, welche Bereitstellungstopologie genutzt werden soll.

Sehen wir uns jetzt ein bestimmtes Muster zur Unterstützung des Failovers in unterschiedlichen Regionen an. In allen Beispielen werden zwei Regionen verwendet, um den Prozess zu beschreiben.

###Erneute Bereitstellung in einer sekundären Azure-Region

Bei diesem Muster werden nur in der primären Region Anwendungen und Datenbanken ausgeführt. Die sekundäre Region wird nicht für ein automatisches Failover eingerichtet. Wenn ein Notfall eintritt, müssen Sie daher alle Teile des Diensts in der neuen Region einrichten. Dies umfasst das Hochladen eines Clouddiensts in Azure, das Bereitstellen der Clouddienste, das Wiederherstellen der Daten und das Ändern des DNS zum Umleiten des Datenverkehrs.

Obwohl dies unter den Optionen für mehrere Regionen die kostengünstigste Option ist, sind die RTO-Eigenschaften jedoch am schlechtesten. Bei diesem Modell werden das Dienstpaket und die Datenbanksicherungen lokal oder in der Azure Blob Storage-Instanz der sekundären Region gespeichert. Sie müssen jedoch einen neuen Dienst bereitstellen und die Daten wiederherstellen, bevor der Betrieb fortgesetzt wird. Auch wenn Sie die Datenübertragung aus dem Sicherungsspeicher vollständig automatisieren, bedeutet das Einrichten der neuen Datenbankumgebung einen großen Zeitaufwand. Das Verschieben von Daten aus dem Sicherungsspeicher in die leere Datenbank in der sekundären Region ist der aufwendigste Teil des Wiederherstellungsprozesses. Sie müssen dies jedoch tun, um die neue Datenbank in einen funktionsfähigen Zustand zu versetzen, da sie nicht repliziert wurde.

Der beste Ansatz ist, die Dienstpakete im Blobspeicher in der sekundären Region zu speichern. Dadurch muss das Paket nicht in Azure hochgeladen werden. Dies geschieht bei einer Bereitstellung über einen lokalen Entwicklungscomputer. Sie können die Dienstpakete schnell mithilfe von PowerShell-Skripts aus dem Blobspeicher einem neuen Clouddienst bereitstellen.

Diese Option ist nur für nicht kritische Anwendungen praktikabel, die eine hohe RTO tolerieren können. Es kann z.B. für eine Anwendung funktionieren, die mehrere Stunden inaktiv sein kann, aber innerhalb von 24 Stunden wieder ausgeführt werden sollte.

![Erneute Bereitstellung in einer sekundären Azure-Region](./media/resiliency-disaster-recovery-azure-applications/redeploy-to-a-secondary-azure-region.png)

###Aktiv/Passiv

Das Aktiv/Passiv-Muster ist die Option, die von vielen Unternehmen bevorzugt wird. Dieses Muster bietet gegenüber dem Muster mit erneuter Bereitstellung RTO-Verbesserungen bei einem relativ geringen Anstieg der Kosten. In diesem Szenario gibt es wieder eine primäre und eine sekundäre Azure-Region. Der gesamte Datenverkehr wird an die aktive Bereitstellung in der primären Region weitergeleitet. Die sekundäre Region ist besser für die Notfallwiederherstellung vorbereitet, da die Datenbank in beiden Regionen ausgeführt wird. Darüber hinaus gibt es einen Synchronisierungsmechanismus zwischen den Regionen. Dieser Standbyansatz kann zwei Varianten umfassen: ein reiner Datenbankansatz oder eine vollständige Bereitstellung in der sekundären Region.

####Nur Datenbank

Bei der ersten Variante des Aktiv/Passiv-Musters weist nur die primäre Region eine bereitgestellte Clouddienstanwendung auf. Im Gegensatz zum Muster mit erneuter Bereitstellung werden jedoch beide Regionen mit dem Inhalt der Datenbank synchronisiert. (Weitere Informationen finden Sie im Abschnitt zu [Transaktionsdatenmuster für die Notfallwiederherstellung](#transactional-data-pattern-for-disaster-recovery).) Bei einem Notfall gibt es weniger Aktivierungsanforderungen. Sie starten die Anwendung in der sekundären Region, ändern die Verbindungszeichenfolgen in die neue Datenbank und ändern die DNS-Einträge zum Umleiten des Datenverkehrs.

Wie beim Muster mit erneuter Bereitstellung sollten die Dienstpakete bereits im Azure-Blobspeicher in der sekundären Region für eine schnellere Bereitstellung gespeichert sein. Im Gegensatz zum Muster mit erneuter Bereitstellung fällt ein Großteil des Aufwands nicht an, der für die Wiederherstellung der Datenbank benötigt wird. Die Datenbank ist funktionsfähig und wird ausgeführt. Dies spart viel Zeit, sodass dieses Muster für die Notfallwiederherstellung erschwinglich ist. Es ist auch das am häufigsten verwendete Muster für die Notfallwiederherstellung.

![Aktiv/Passiv, nur Datenbank](./media/resiliency-disaster-recovery-azure-applications/active-passive-database-only.png)

####Vollständiges Replikat

Bei der zweiten Variante des Aktiv/Passiv-Musters weisen die primäre und die sekundäre Region eine vollständige Bereitstellung auf. Diese Bereitstellung umfasst die Clouddienste und eine synchronisierte Datenbank. Allerdings werden nur in der primären Region aktiv Netzwerkanforderungen der Benutzer verarbeitet. Die sekundäre Region wird nur aktiv, wenn in der primären Region eine Dienstunterbrechung auftritt. In diesem Fall werden alle neuen Netzwerkanforderungen an die sekundäre Region weitergeleitet. Azure Traffic Manager kann dieses Failover automatisch verwalten.

Ein Failover erfolgt schneller als in der reinen Datenbankvariante, da die Dienste bereits bereitgestellt wurden. Dieses Muster bietet ein sehr niedriges RTO. Die sekundäre Failoverregion muss unmittelbar nach einem Ausfall der primären Region bereit sein.

Zusammen mit einer kürzeren Reaktionszeit hat dieses Muster den Vorteil der Vorabzuordnung und Bereitstellung von Datensicherungsdiensten. Sie müssen sich keine Gedanken darüber machen, dass in einer Region der Speicherplatz für die Zuordnung neuer Instanzen in einem Notfall nicht ausreicht. Dies ist wichtig, wenn in der sekundären Azure-Region die Kapazität fast erreicht ist. Es gibt keine Garantie (Vereinbarung zum Servicelevel, SLA), dass Sie sofort eine Reihe von neuen Clouddiensten in einer beliebigen Region bereitstellen können.

Für die schnellste Reaktionszeit mit diesem Modell müssen Sie in der primären und sekundären Region über eine ähnliche Skalierung (Anzahl der Rolleninstanzen) verfügen. Trotz der Vorteile ist es teuer, für ungenutzte Compute-Instanzen zu zahlen, und dies stellt möglicherweise nicht die sinnvollste finanzielle Entscheidung dar. Aus diesem Grund wird häufig eine leicht reduzierte Version der Clouddienste in der sekundären Region verwendet. Anschließend können Sie bei Bedarf schnell ein Failover ausführen und die sekundäre Bereitstellung horizontal hochskalieren. Sie sollten den Failoverprozess automatisieren, damit Sie bei einem Ausfall der primären Region zusätzliche Instanzen abhängig von der Last aktivieren können. Dies kann die Nutzung eines Mechanismus für die automatische Skalierung wie [VM-Skalierungsgruppen](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) umfassen.

Das folgende Diagramm zeigt das Modell, in dem die primären und sekundären Regionen einen vollständig bereitgestellten Clouddienst in einem Aktiv/Passiv-Muster enthalten.

![Aktiv/Passiv, vollständiges Replikat](./media/resiliency-disaster-recovery-azure-applications/active-passive-full-replica.png)

###Aktiv/Aktiv

Inzwischen haben Sie wahrscheinlich die Entwicklung der Muster erkannt: Verringern des RTO steigert Kosten und Komplexität. Die Aktiv/Aktiv-Lösung durchbricht tatsächlich diese Tendenz hinsichtlich der Kosten.

In einem Aktiv/Aktiv-Muster werden die Clouddienste und die Datenbank vollständig in beiden Regionen bereitgestellt. Im Gegensatz zum Aktiv/Passiv-Modell empfangen beide Regionen Benutzerdatenverkehr. Diese Option bietet die kürzeste Wiederherstellungszeit. Die Dienste sind bereits skaliert, um einen Teil der Last in jeder Region zu verarbeiten. DNS ist bereits vorbereitet, die sekundäre Region zu verwenden. Es gibt eine zusätzliche Komplexität, da bestimmt werden muss, wie Benutzer an die entsprechende Region weitergeleitet werden. Roundrobinplanung ist möglich. Es ist wahrscheinlicher, dass einige Benutzer eine bestimmte Region verwenden würden, in der sich die primäre Kopie ihrer Daten befindet.

Im Fall eines Failovers deaktivieren Sie einfach DNS in der primären Region. Dadurch wird der gesamte Datenverkehr an die sekundäre Region weitergeleitet.

Auch bei diesem Modell gibt es einige Varianten. Das folgende Diagramm zeigt z.B. ein Modell, in dem die primäre Region die Masterkopie der Datenbank besitzt. Die Clouddienste in beiden Regionen schreiben in diese primäre Datenbank. Die sekundäre Bereitstellung kann aus der primären oder aus der replizierten Datenbank lesen. Die Replikation erfolgt in diesem Beispiel unidirektional.

![Aktiv/Aktiv](./media/resiliency-disaster-recovery-azure-applications/active-active.png)

Es gibt jedoch einen Nachteil der Aktiv/Aktiv-Architektur in der vorherigen Abbildung. Die zweite Region muss auf die Datenbank in der ersten Region zugreifen, da sich die Masterkopie dort befindet. Die Leistung wird erheblich beeinträchtigt, wenn Sie auf Daten außerhalb einer Region zugreifen. Bei regionsübergreifenden Datenbankaufrufen sollten Sie eine Form von Batchverarbeitungsstrategie zur Verbesserung der Leistung dieser Aufrufe in Betracht ziehen. Weitere Informationen finden Sie unter [Gewusst wie: Verbessern der Leistung von SQL-Datenbankanwendungen mithilfe von Batchverarbeitung](../sql-database/sql-database-use-batching-to-improve-performance.md).

Eine alternative Architektur kann vorsehen, dass jede Region auf eine eigene Datenbank direkt zugreift. In diesem Modell ist eine Form von bidirektionaler Replikation zum Synchronisieren der Datenbanken in jeder Region erforderlich.

Im Aktiv/Aktiv-Muster benötigen Sie möglicherweise nicht so viele Instanzen in der primären Region wie im Aktiv/Passiv-Muster. Wenn Sie in der primären Region in einer Aktiv/Passiv-Architektur 10 Instanzen haben, sollten Sie nur 5 in jeder Region in einer Aktiv/Aktiv-Architektur benötigen. Beide Regionen teilen sich jetzt die Last. Dies kann Kosteneinsparungen gegenüber dem Aktiv/Passiv-Muster bedeuten, wo Sie einen betriebsbereiten Standby in der passiven Region mit 10 Instanzen bereit halten, die auf das Failover warten.

Beachten Sie, dass bis zur Wiederherstellung der primären Region in der sekundären Region die Anzahl neuer Benutzer rapide ansteigen kann. Wenn es zum Zeitpunkt der Dienstunterbrechung in der primären Region auf jedem Server 10.000 Benutzer waren, muss die sekundäre Region plötzlich 20.000 Benutzer verarbeiten. Überwachungsregeln in der sekundären Region müssen diese Erhöhung erkennen und die Anzahl der Instanzen in der sekundären Region verdoppeln. Weitere Informationen hierzu finden Sie im Abschnitt zur [Ausfallerkennung](#failure-detection).

##Hybride Lösungen mit lokaler und Cloudbereitstellung

Eine weitere Strategie für die Notfallwiederherstellung ist, eine Hybridanwendung zu entwickeln, die lokal und in der Cloud ausgeführt wird. Abhängig von der Anwendung kann die primäre Region einer der beiden Orte sein. Betrachten Sie die zuvor beschriebenen Architekturen, und stellen Sie sich die primäre oder die sekundäre Region als einen lokalen Speicherort vor.

In diesen Hybridarchitekturen gibt es einige Herausforderungen. Erstens wurden in diesem Artikel schwerpunktmäßig PaaS-Architekturmuster (Platform as a Service) behandelt. Typische PaaS-Anwendungen in Azure basieren auf Azure-spezifischen Konstrukten wie Rollen, Clouddiensten und Traffic Manager. Zum Erstellen einer lokalen Lösung für diese Art von PaaS-Anwendung ist eine grundlegend andere Architektur erforderlich. Diese kann aus Verwaltungs- und Kostengründen nicht sinnvoll sein.

Allerdings bedeutet eine Hybridlösung für die Notfallwiederherstellung weniger Herausforderungen, wenn herkömmliche Architekturen einfach in die Cloud verschoben wurden. Dies gilt für Architekturen, die IaaS (Infrastructure as a Service) nutzen. IaaS-Anwendungen verwenden virtuelle Computer in der Cloud, die direkte lokale Entsprechungen haben können. Durch die Verwendung von virtuellen Netzwerken können Sie zudem Computer in der Cloud mit lokalen Netzwerkressourcen verbinden. Dadurch ergeben sich eine Reihe von Möglichkeiten, die mit reinen PaaS-Anwendungen nicht möglich sind. SQL Server kann z.B. die Notfallwiederherstellungslösungen wie AlwaysOn-Verfügbarkeitsgruppen und Datenbankspiegelung nutzen. Weitere Informationen finden Sie unter [Hohe Verfügbarkeit und Notfallwiederherstellung für SQL Server auf virtuellen Azure-Computern](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

IaaS-Lösungen bieten zudem einen einfacheren Weg für lokale Anwendungen, Azure als Failoveroption zu verwenden. Möglicherweise verfügen Sie über eine voll funktionsfähige Anwendung in einer vorhandenen lokalen Region. Was geschieht aber, wenn die Ressourcen nicht ausreichen, um eine geografisch getrennte Region für ein Failover zu verwalten? Sie könnten virtuelle Computer und virtuelle Netzwerke verwenden, damit Ihre Anwendung in Azure ausgeführt wird. Definieren Sie in diesem Fall Prozesse, um die Daten mit der Cloud zu synchronisieren. Die Azure-Bereitstellung wird dann zur sekundären Region, die für das Failover verwendet wird. Die primäre Region bleibt die lokale Anwendung. Weitere Informationen zu IaaS-Architekturen und -Funktionen finden Sie in der [Dokumentation zu Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/).

##Alternative Clouds

Es gibt Situationen, in denen selbst die Stabilität der Microsoft Cloud interne Complianceregeln oder in Ihrer Organisation einzuhaltende Richtlinien nicht erfüllen kann. Auch die beste Vorbereitung und ein Entwurf zur Implementierung von Sicherungssystemen während eines Notfalls reichen im Fall einer globalen Dienstunterbrechung eines Clouddienstanbieters nicht aus.

Sie sollten Verfügbarkeitsanforderungen mit den Kosten und der Komplexität einer höheren Verfügbarkeit abgleichen. Führen Sie eine Risikoanalyse durch, und definieren Sie RTO und RPO für Ihre Lösung. Wenn Ihre Anwendung keine Ausfallzeiten tolerieren kann, kann es sinnvoll sein, dass Sie eine andere Cloudlösung in Betracht ziehen. Sofern nicht das gesamte Internet ausfällt, steht eine andere Cloudlösung möglicherweise immer noch zur Verfügung, wenn auf Azure nicht mehr global zugegriffen werden kann.

Wie beim Hybridszenario können die Failoverbereitstellungen in den vorherigen Architekturen für die Notfallwiederherstellung auch in einer anderen Cloudlösung vorhanden sein. Alternative Cloudstandorte für Notfallwiederherstellungen sollten für Lösungen nur verwendet werden, wenn das RTO nur geringe bzw. überhaupt keine Ausfallzeiten zulässt. Beachten Sie, dass eine Lösung, die einen Standort für die Notfallwiederherstellung außerhalb von Azure nutzt, mehr Aufwand beim Konfigurieren, Entwickeln, Bereitstellen und Verwalten verursacht. Es ist auch schwieriger, bewährte Methoden in einer Architektur mit mehreren Clouds zu implementieren. Obwohl Cloudplattformen ähnliche allgemeine Konzepte aufweisen, unterscheiden sich die APIs und die Architekturen.

Wenn Sie sich entscheiden, Ihre Notfallwiederherstellung auf verschiedene Plattformen zu verteilen, empfiehlt es sich, in den Entwurf der Lösung Abstraktionsebenen einzubinden. Wenn Sie dies tun, müssen Sie bei einem Notfall nicht zwei verschiedene Versionen der gleichen Anwendung für unterschiedliche Cloudplattformen entwickeln und verwalten. Wie beim Hybridszenario kann die Verwendung von Azure Virtual Machines und Azure Container Service in diesen Fällen einfacher sein als die Nutzung cloudspezifischer PaaS-Entwürfe.

##Automatisierung

Einige der soeben erläuterten Muster erfordern die unverzügliche Aktivierung von Offlinebereitstellungen sowie die Wiederherstellung bestimmter Teile eines Systems. Automatisierung (mithilfe von Skripts) unterstützt die Fähigkeit, Ressourcen bei Bedarf zu aktivieren und Lösungen schnell bereitzustellen. In diesem Artikel wird die Automatisierung im Zusammenhang mit der Notfallwiederherstellung mit [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx) gleichgesetzt, aber die [REST-API der Dienstverwaltung](https://msdn.microsoft.com/library/azure/ee460799.aspx) ist eine weitere Option.

Das Entwickeln von Skripts erleichtert die Verwaltung der Teile der Notfallwiederherstellung, die von Azure nicht transparent verarbeitet werden. Dies hat den Vorteil, dass jedes Mal konsistente Ergebnisse geliefert werden, wodurch das Risiko von Benutzerfehlern reduziert wird. Mit vorab definierten Skripts für die Notfallwiederherstellung wird auch die Zeit reduziert, die die Wiederherstellung eines Systems und seiner Bestandteile in einem Notfall dauert. Sie sollten nicht versuchen, manuell herauszufinden, wie Sie Ihren Standort nach einem Ausfall wiederherstellen können, während Sie minütlich Geld verlieren.

Nachdem Sie die Skripts erstellt haben, testen Sie sie gründlich von A bis Z. Wenn Sie die grundlegenden Funktionen überprüft haben, müssen Sie sie durch die [Simulation von Notfällen](#disaster-simulation) testen. Dadurch können Sie Fehler in Skripts oder Prozessen aufdecken.

Eine bewährte Methode bei der Automatisierung ist die Erstellung eines Repositorys mit PowerShell- oder CLI-Skripts (Command-Line Interface, Befehlszeilenschnittstelle) für die Azure-Notfallwiederherstellung. Kennzeichnen und kategorisieren Sie sie eindeutig, um sie schnell zu finden. Benennen Sie eine Person, die das Repository und die Versionen der Skripts verwaltet. Dokumentieren Sie die Skripts mit Erklärungen der Parameter und Beispielen für die Skriptverwendung. Stellen Sie außerdem sicher, dass diese Dokumentation mit Ihren Azure-Bereitstellungen synchronisiert bleibt. Dies verdeutlicht, warum eine Person für alle Teile des Repositorys verantwortlich sein sollte.

##Ausfallerkennung

Um Probleme mit der Verfügbarkeit und der Notfallwiederherstellung korrekt zu behandeln, müssen Sie Lage sein, Ausfälle zu erkennen und zu diagnostizieren. Sie sollten eine erweiterte Server- und Bereitstellungsüberwachung durchführen, damit Sie schnell erkennen können, wenn ein System oder seine Teile plötzlich ausgefallen sind. Überwachungstools, die die Gesamtintegrität des Clouddiensts und seiner Abhängigkeiten analysieren, können einen Teil dieser Arbeit übernehmen. Ein Microsoft-Tool ist [System Center 2016](https://www.microsoft.com/de-DE/server-cloud/products/system-center-2016/). Tools von Drittanbietern können ebenfalls Überwachungsfunktionen bieten. Die meisten Überwachungslösungen verfolgen wichtige Leistungsindikatoren und die Verfügbarkeit von Diensten.

Obwohl diese Tools wichtig sind, ersetzen sie nicht die Notwendigkeit, Ausfallerkennung und entsprechende Berichte in einen Clouddienst zu implementieren. Sie müssen die ordnungsgemäße Verwendung der Azure-Diagnose einplanen. Benutzerdefinierte Leistungsindikatoren oder Ereignisprotokolleinträge können auch Teil der Gesamtstrategie sein. Dadurch erhalten Sie mehr Daten bei Ausfällen, um schnell das Problem zu diagnostizieren und den vollen Funktionsumfang wiederherzustellen. Darüber hinaus stehen Ihnen so weitere Metriken für die Überwachungstools bereit, mit denen Sie den Zustand der Anwendung bestimmen können. Weitere Informationen finden Sie unter [Aktivieren der Azure-Diagnose in Azure Cloud Services](../cloud-services/cloud-services-dotnet-diagnostics.md). Eine Beschreibung der Planung eines globalen „Integritätsmodells“ finden Sie unter [Failsafe: Guidance for Resilient Cloud Architectures](https://channel9.msdn.com/Series/FailSafe) (Ausfallsicherheit: Leitfaden für robuste Cloudarchitekturen).

##Simulation von Notfällen

Bei Simulationstests werden kleine reale Situationen bei der Arbeit herbeigeführt, um zu beobachten, wie die Mitglieder des Teams reagieren. Simulationen zeigen außerdem die Wirksamkeit der Lösungen im Wiederherstellungsplan. Führen Sie Simulationen so aus, dass die erstellten Szenarien sich zwar wie „reale“ Situationen anfühlen, aber den Geschäftsbetrieb nicht unterbrechen.

Sie könnten eine Art „Schalttafel“ in die Anwendung einbinden, um Verfügbarkeitsprobleme manuell zu simulieren. Lösen Sie z.B. über einen „Soft Switch“ Ausnahmen für den Datenbankzugriff für ein Bestellmodul aus, um eine Fehlfunktion zu verursachen. Ähnliche einfache Methoden können für andere Module auf Netzwerkschnittstellenebene gewählt werden.

Alle unzureichend behandelten Probleme werden während der Simulation hervorgehoben. Die simulierten Szenarien müssen vollständig steuerbar sein. Dies bedeutet: Selbst wenn der Wiederherstellungsplan fehlzuschlagen scheint, müssen Sie den Normalzustand wiederherstellen können, ohne dass wesentliche Schäden entstehen. Es ist auch wichtig, dass Sie das übergeordnete Management informieren, wann und wie die Simulationsübungen ausgeführt werden. Dieser Plan sollte Informationen über die Zeit oder Ressourcen enthalten, die während der Ausführung des Simulationstests unproduktiv werden können. Wenn Sie Ihren Notfallwiederherstellungsplan einem Test unterziehen, müssen Sie auch definieren, wie der Erfolg gemessen wird.

Es gibt verschiedene Techniken, mit denen Sie Notfallwiederherstellungspläne testen können. Die meisten von ihnen sind jedoch einfach abgewandelte Versionen dieser grundlegenden Techniken. Der wichtigste Grund für diese Tests ist, die Durchführbarkeit und Funktionsfähigkeit des Wiederherstellungsplans zu bewerten. Notfallwiederherstellungstests befassen sich mit den Details, um Lücken im grundlegenden Wiederherstellungsplan zu ermitteln.

##Nächste Schritte

Dieser Artikel ist Teil einer Reihe von Artikeln mit Fokus auf [Notfallwiederherstellung und hohe Verfügbarkeit für in Microsoft Azure erstellte Anwendungen](./resiliency-disaster-recovery-high-availability-azure-applications.md). Der vorangegangene Artikel dieser Reihe ist [Hohe Verfügbarkeit für in Microsoft Azure erstellte Anwendungen](./resiliency-high-availability-azure-applications.md).

<!---HONumber=AcomDC_0615_2016-->