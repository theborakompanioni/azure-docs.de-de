<properties
   pageTitle="Azure SQL-Datenbank – Azure-Fallstudie: Daxko/CSI | Microsoft Azure"
   description="Erfahren Sie, wie Daxko/CSI SQL-Datenbank verwendet, um den Entwicklungszyklus zu beschleunigen und sowohl Kundendienste als auch Leistung zu verbessern."
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
   ms.workload="NA"
   ms.date="09/08/2016"
   ms.author="carlrab"/>
   
# Daxko/CSI verwendet Azure, um den Entwicklungszyklus zu beschleunigen und sowohl Kundendienste als auch Leistung zu verbessern

![Logo](./media/sql-database-case-study-daxko/csidaxkologo25.png)

Daxko/CSI Software stand vor einer großen Herausforderung: Der Kundenstamm an Fitness- und Erholungszentren wuchs dank des Erfolgs der umfassenden Unternehmenssoftwarelösung sehr schnell, aber die IT-Infrastrukturanforderungen für diesen wachsenden Kundenstamm stellten die IT-Mitarbeiter des Unternehmens auf eine harte Probe. Das Unternehmen hatte mit einem immer höheren Betriebsaufwand zu kämpfen, insbesondere für die Verwaltung der wachsenden Datenbanken. Schlimmer noch: Dieser Betriebsaufwand beanspruchte auch Entwicklungsressourcen, die eigentlich für neue Initiativen benötigt wurden, beispielsweise für neue Mobilitätsfunktionen für die Software des Unternehmens.

Wie David Molina, Director of Product Development bei Daxko/CSI, erläutert, stellte Azure ein PaaS-Modell (Platform-as-a-Service) für CSI Software bereit, mit dem das Unternehmen die Datenbankverwaltung vereinfachen, die Skalierbarkeit erhöhen und Ressourcen freigeben konnte, die sich jetzt wieder auf die Software anstatt auf den Betrieb konzentrieren können. „Azure SQL-Datenbank war eine hervorragende Option für uns. Wir müssen uns nicht mehr um die Verwaltung von SQL Server-Instanzen, Failoverclustern und all der anderen Infrastrukturkomponenten kümmern – die ideale Lösung für uns.“

Seit der Migration zu Azure benötigt CSI Software nur noch zwei Mitarbeiter, um die mehr als 600 Kundendatenbanken zu verwalten. Das Unternehmen verwendet elastische Azure SQL-Datenbankpools, um Kundendatenbanken je nach Größe und Bedarf zu verschieben.

Mr. Molina führt weiter aus: „Unsere Kunden haben die Veränderung sofort bemerkt. Bevor wir elastische Pools einsetzten, kam es zu Zeiten hoher Auslastungen zuweilen zu Timeouts und anderen Problemen. Mit den elastischen Azure-Pools können unsere Kunden das System beliebig hoch belasten und die Software dennoch ohne Probleme verwenden.“

Die elastischen Azure-Datenbankpools verbesserten nicht nur die Leistung für Kunden, sondern setzten auch Ressourcen bei CSI Software frei, die sich wieder auf die Entwicklung neuer Dienste und Funktionen konzentrieren können und sich nicht mehr um Betrieb und Verwaltung kümmern müssen. Dank dieser IT-Ressourcen konnte CSI Software seine Unternehmenssoftware SpectrumNG verbessern, um die Dienste für die Mitglieder der Fitnessstudios zu optimieren, die Effizienz der Mitarbeiter zu verbessern und sowohl den Mitgliedern als auch den Mitarbeitern mobilen Zugriff auf interaktive Vorgänge und Echtzeitbenachrichtigungen zu gewähren.

Azure unterstützte CSI Software mithilfe von Automatisierungsoptionen auch dabei, die Entwicklungs- und Qualitätssicherungszyklen zu verkürzen und zu verbessern. Dank der Azure-Implementierung des Unternehmens können Buildmanager Komponenten mit einem einzigen Mausklick packen. David Molina beschreibt es so: „Im Rahmen des Releasezyklus kann die Qualitätssicherung jetzt eine Testumgebung in Azure bereitstellen, die den Produktionsstapel nahezu perfekt imitiert. Wir können Builds sofort in unserer Entwicklungsumgebung bereitstellen, um Änderungen zu überprüfen. Das ist ein enormer Vorteil für uns, da wir vorher keine gleichwertigen Systeme für Tests hatten.“

## Auslagern in die Cloud

Vor dem Wechsel in die Cloud hatte CSI Software in einem lokalen Rechenzentrum in Houston erfolgreich eine eigene mehrinstanzenfähige Infrastruktur aufgebaut. Als das Unternehmen expandierte, wurde es immer schwieriger, all die Hardware- und Softwarekomponenten zu erwerben, bereitzustellen und zu verwalten, die zur Unterstützung der Kunden benötigt wurden. Die Zahl der für den Betrieb erforderlichen IT-Mitarbeiter wurde ein weiteres Problem, das zu einer verlangsamten Bereitstellung von neuen Ressourcen und neuen Diensten für die Kunden führte.

CSI Software prüfte verschiedene Cloudoptionen, um diesen Aufwand zu eliminieren und sich statt auf den Betrieb wieder auf die Softwareentwicklung konzentrieren zu können. Das Unternehmen fand heraus, dass viele der wichtigsten Cloudanbieter nur IaaS-Lösungen (Infrastructure-as-a-Service) anbieten, die immer noch sehr viele IT-Mitarbeiter zur Verwaltung des IaaS-Stapels erfordern. Am Ende stellte CSI Software fest, dass die Azure-PaaS-Lösung die Anforderungen des Unternehmens am besten erfüllte. Mr. Molina erklärt: „Azure räumt uns die Hardware und Systemsoftware aus dem Weg, sodass wir uns auf unsere Softwareangebote konzentrieren und den IT-Aufwand reduzieren können.“

## Der Übergang zu Azure

Nachdem CSI Software sich für seine PaaS-Lösung für Azure entschieden hatte, begann das Unternehmen damit, die Back-End-Infrastruktur und -Datenbanken in die Cloud zu migrieren. Vor dem Umstieg auf Azure mussten SpectrumNG-Kunden eine Clientanwendung installieren, die mit einem WCF-Dienst (Windows Communication Foundation) im Back-End kommunizierte. Mr. Molina erläutert: „Obwohl einige Kunden alle Daten und Komponenten in ihren eigenen Rechenzentren gehostet haben, haben wir das Produkt mehrinstanzenfähig entwickelt. Wir haben alle Elemente in einem Rechenzentrum in Houston gehostet und SQL Server als Datenspeicher verwendet.

Unser Produktangebot umfasste auch ein Webportal für die Mitglieder (geschrieben in ASP.net), das als White-Label-Lösung von unseren Kunden in ihrer eigenen Onlinepräsenz eingesetzt werden konnte, sowie eine SOAP-API zur Unterstützung der Onlineseiten und der Integration von Drittanbieterlösungen.“

Die Migration der Architektur in die Cloud dauerte nicht lang. David Molina führt aus: „Der Hauptteil der Arbeit bestand darin, die Art und Weise zu ändern, in der unser System Konfigurationsdateien liest. Außerdem mussten einige Verbindungszeichenfolgen zentral geändert sowie das Packen, Hochladen und Bereitstellen unserer Versionen automatisiert werden.“

Für die Entwicklung der Buildautomatisierung verwendeten die Techniker bei CSI Software Azure PowerShell und REST-APIs, um Pakete zu erstellen und in eine Stagingumgebung hochzuladen, von der aus sie jede Nacht bereitgestellt werden. Der Übergang zur Bereitstellung in der Azure-Cloud lief für das IT-Team von CSI Software insgesamt schnell und reibungslos ab. „Insgesamt hatten wir innerhalb von drei oder vier Wochen nach Start des Projekts eine Betaumgebung in der Cloud. Das war ein überraschender Gewinn für uns“, erzählt Molina.

Nach dem Konfigurieren und Testen der Umgebung begann CSI Software damit, die Kunden zu migrieren. Für Kunden, die bereits die Hostingdienste von CSI Software nutzten, war der Übergang praktisch reibungslos. Bei Kunden, die bisher mit einer lokalen Bereitstellung gearbeitet hatten, dauerte die Migration in die Cloud etwas länger, lief jedoch sowohl für die Kunden als auch für CSI Software zum größten Teil ohne Probleme ab.

Für das Onboarding neuer Kunden in Azure verwenden die IT-Mitarbeiter von CSI Software folgenden Prozess:

1.	Mithilfe von Azure PowerShell wird eine neue Datenbank für den Kunden eingerichtet. Alle Kunden starten in einem Premiumtarif, um einen ausreichend hohen Durchsatz für den Übergang sicherzustellen.
2.	Wenn möglich, nutzt CSI Software den Azure SQL-Migrations-Assistenten, um vorhandene Daten in eine Azure SQL-Datenbankinstanz zu verschieben.
3.	Schließlich werden Microsoft SQL Server Integration Services (SSIS) verwendet, um eventuelle Datendiskrepanzen zu beheben oder bei Bedarf Daten zu bereinigen.

Heute werden etwa 99 Prozent aller Kunden von CSI Software in vier regionalen Azure-Rechenzentren gehostet (Norden-Mitte, Süden-Mitte, Osten und Westen). Da sich die Rechenzentren in der jeweiligen geografischen Region der Kunden befinden, wird die Latenz auf ein Minimum reduziert.

## Elastische Azure-Datenbankpools setzen IT-Ressourcen frei

Viele der Azure-Funktionen haben CSI Software dabei geholfen, von einem infrastruktur- und vorgangsbasierten System auf eine Lösung umzustellen, die sich auf Funktionen und Entwicklung konzentriert. Dabei profitierte das Unternehmen wahrscheinlich am meisten von den elastischen Datenbankpools.

CSI Software stellt zurzeit etwa 550 Datenbanken für Kunden bereit. Bevor elastische Pools eingesetzt wurden, war es schwierig, diese Mengen an Datenbanken innerhalb einer Struktur mit verschiedenen Ebenen zu verwalten. Betriebsmanager mussten Leistungsebenen basierend auf den Burstanforderungen der Kunden zuweisen, was einen erheblichen Aufwand an IT-Ressourcen erforderte. Mit elastischen Datenbankpools können die Manager Mandanten je nach Bedarf einen Premium- oder Standardpool zuweisen und die Kunden dann basierend auf Größe und Anforderungen verschieben. Die Kunden bemerkten die Auswirkungen der elastischen Datenbankpools fast sofort: Vor dem Einsatz dieser Pools hatten Kunden bei einem plötzlichen Anstieg der Nutzung mit Timeouts und anderen Problemen zu kämpfen – die elastischen Pools können solche plötzlichen Aktivitätsanstiege ganz einfach bewältigen, und die Kunden können SpectrumNG völlig reibungslos weiterverwenden.

## Beschleunigte Berichterstellung dank aktiver Georeplikation in Azure

Viele Kunden von CSI Software profitieren auch von der aktiven Georeplikation in Azure. Mit der aktiven Georeplikation können bis zu vier lesbare sekundäre Datenbanken in der gleichen oder einer anderen Rechenzentrumsregion konfiguriert werden. CSI Software nutzt die aktive Georeplikation auf zwei Arten: Erstens stehen die sekundären Datenbanken zur Verfügung, falls ein Rechenzentrum ausfällt oder keine Verbindung mit der primären Datenbank hergestellt werden kann. Zweitens sind die sekundären Datenbanken lesbar und können zum Auslagern von schreibgeschützten Arbeitsauslastungen wie beispielsweise Berichtsaufträgen verwendet werden. Einige Kunden von CSI Software nutzen diese Vorteile, um Berichtsworkflows zu beschleunigen.

## Anwendungslogik und Architektur von CSI Software

SpectrumNG arbeitet mit Webrollen. Da die Anwendung mehrinstanzenfähig ist, wird ein WCF-Dienst zur Verarbeitung der anfänglichen Verbindungsanforderung der Kunden verwendet. Mr. Molina erläutert: „Die Anforderung identifiziert jeden einzelnen Kunden, sodass wir eine Verbindungszeichenfolge erstellen können, mit der eine Verbindung mit der Datenbank des jeweiligen Kunden hergestellt wird, damit wir die gewünschten Vorgänge in der Datenbank ausführen können.“

Für die Webebene des Diensts nutzt CSI Software die automatische Skalierungsfunktion von Azure, basierend auf Tag und Uhrzeit. Entsprechend der Zeitzone jedes regionalen Rechenzentrums werden verfügbare Ressourcen automatisch erhöht, um die höhere Auslastung während der Geschäftszeiten zu verarbeiten. Die Ressourcen sind so eingerichtet, dass sie während des Wochenendes, wenn die Kundenanforderungen geringer sind, herunterskaliert werden.

     
![Abbildung 1](./media/sql-database-case-study-daxko/figure1.png)

Abbildung 1. Eine Clouddienst-Workerrolle ruft strukturierte Daten aus einer Azure SQL-Datenbank und semistrukturierte Daten aus einem Tabellenspeicher ab. SpectrumNG-Benutzer interagieren mit diesen Daten über eine Clouddienst-Webrolle.

## Verwenden von Web-Apps und einer Webplanebene für mobile Apps

Die Nutzung von Azure SQL-Datenbank setzte Ressourcen bei CSI Software frei, die neue Initiativen starten konnten, z.B. eine vollständige mobile Plattform, die auf einer in Azure-Web-Apps gehosteten benutzerdefinierten API basiert. Auf dieser Plattform können Mitglieder und Mitarbeiter von Fitnessstudios über mobile Geräte Terminpläne prüfen, Kurse buchen und Nachrichten empfangen.

Die Plattform verwendet eine serviceorientierte Architektur (SOA), um eine einzelne Komponente aufzugreifen – ähnlich wie ein Verkaufsstellen- oder Vertriebssystem –, im laufenden Betrieb in einen anderen Webplan zu verschieben und dann einen Dienst zu erstellen, der diese Komponente unterstützt. Alle anderen Komponenten und Elemente verbleiben im ursprünglichen Webplan. Diese Fähigkeit bietet CSI Software ein enormes Maß an Flexibilität und hilft gleichzeitig dabei, die Kosten niedrig zu halten.

## Entwickler von CSI Software können sich dank Azure auf Apps und Dienste konzentrieren

Azure SQL-Datenbank ist nicht nur ein Segen für SpectrumNG-Kunden, die sich über den schnellen und zuverlässigen Dienst freuen – es bietet auch den IT-Mitarbeitern und Entwicklern von CSI Software erhebliche Vorteile. Indem Betriebsfunktionen in die Azure-Lösung in die Cloud ausgelagert wurden, konnte CSI Software den Aufwand für Ressourcen und Infrastruktur senken und die Entwicklungszyklen erheblich verkürzen. Außerdem muss das Unternehmen nicht mehr jedes kleinste Detail verwalten, um die Datenbankleistung für seine Mandanten zu optimieren.

## Weitere Informationen

- Weitere Informationen zu elastischen Azure-Datenbankpools finden Sie unter [Pools für elastische Datenbanken](sql-database-elastic-pool.md).

- Weitere Informationen zu Datenbanktools und elastischer Skalierung finden Sie unter [Tools für elastische Datenbanken und elastische Skalierung](sql-database-elastic-scale-get-started.md).

- Weitere Informationen zum Migrieren einer SQL Server-Datenbank finden Sie unter [Azure SQL-Migrations-Assistent](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md).

- Weitere Informationen zur aktiven Georeplikation finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md).

- Weitere Informationen zu Webrollen und Workerrollen finden Sie unter [Workerrollen](../fundamentals-introduction-to-azure.md#compute).

- Weitere Informationen zu Azure Service Bus finden Sie unter [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).

- Weitere Informationen zur automatischen Skalierung finden Sie unter [Skalieren von Clouddiensten](../cloud-services/cloud-services-how-to-scale.md).

<!---HONumber=AcomDC_0921_2016-->