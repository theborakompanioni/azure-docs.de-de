---
title: 'DocumentDB-Kunden: Willkommen zu Azure Cosmos DB | Microsoft-Dokumentation'
description: "Enthält weitere Informationen zu der Ankündigung auf der //build 2017, dass DocumentDB-Kunden jetzt Azure Cosmos DB-Kunden sind."
services: cosmosdb
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmosdb
ms.devlang: n/a
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: database
ms.date: 05/12/2017
ms.author: mimig
ms.translationtype: Human Translation
ms.sourcegitcommit: c308183ffe6a01f4d4bf6f5817945629cbcedc92
ms.openlocfilehash: f60c3502ed51424647088fa8109f13cabc4e73da
ms.contentlocale: de-de
ms.lasthandoff: 05/17/2017

---

# <a name="welcome-documentdb-customers-to-azure-cosmos-db"></a>DocumentDB-Kunden: Willkommen zu Azure Cosmos DB

An alle DocumentDB-Kunden: Wir freuen uns, Ihnen mitteilen zu können, dass Sie jetzt der Azure Cosmos DB-Familie angehören! 

Azure Cosmos DB wurde [auf der Microsoft Build-Konferenz 2017 angekündigt](https://azure.microsoft.com/blog/azure-cosmos-db-microsofts-globally-distributed-multi-model-database-service/) und ist der erste global verteilte Datenbankdienst, mit dem Sie Durchsatz und Speicher über eine beliebige Anzahl von geografischen Regionen hinweg elastisch skalieren können. Gleichzeitig ist für geringe Wartezeiten, hohe Verfügbarkeit und Konsistenz gesorgt, Sie erhalten die umfassendsten SLAs der Branche, und es werden viele verschiedene Datenmodelle und APIs unterstützt. 

![Azure Cosmos DB ist ein weltweit verteilter Datenbankdienst von Microsoft mit elastischer horizontaler Hochskalierung, garantierter geringer Wartezeit, fünf Konsistenzmodellen und umfassenden SLA-Garantien.](./media/welcome-documentdb-customers/azure-cosmos-db.png)

Eine der APIs, die von Azure Cosmos DB unterstützt werden, ist die DocumentDB-API mit dem Dokumentdatenmodell. Sie sind mit den DocumentDB-APIs bereits vertraut. Dies sind die APIs, die Sie bereits verwenden, um Ihre aktuellen DocumentDB-Anwendungen auszuführen. Diese APIs ändern sich _nicht_. Das NuGet-Paket, die Namespaces und alle Abhängigkeiten bleiben gleich. **Sie müssen keine Änderungen vornehmen**, um Ihre mit der DocumentDB-API erstellten Apps weiter ausführen zu können. Azure Cosmos DB ist einfach der Name des Diensts, dem diese Komponenten nun angehören. 

Hier sind einige Fragen aufgeführt, deren Beantwortung für Sie unter Umständen hilfreich ist. 

## <a name="why-move-to-azure-cosmos-db"></a>Warum wurde die Umstellung auf Azure Cosmos DB durchgeführt? 

Azure Cosmos DB steht für den nächsten großen Schritt in Bezug auf global verteilte, bedarfsorientierte Clouddatenbanken. Als DocumentDB-Kunde haben Sie jetzt Zugang zu diesem neuen bahnbrechenden System und den Funktionen von Azure Cosmos DB.

Azure Cosmos DB wurde 2010 als „Project Florence“ eingeführt, um die Problembereiche zu beseitigen, mit denen sich Entwickler umfangreicher Anwendungen bei Microsoft konfrontiert sahen. Als klar wurde, dass die Probleme beim Erstellen von global verteilten Apps nicht nur für Microsoft galten, haben wir im Jahr 2015 die erste Generation dieser Technologie als „Azure DocumentDB“ für Azure-Entwickler zur Verfügung gestellt. 

Seit der Einführung haben wir neue Features hinzugefügt und bemerkenswerte neue Funktionen vorgestellt.  Das Ergebnis dieser Arbeit ist Azure Cosmos DB.  Im Rahmen dieser Version von Azure Cosmos DB werden DocumentDB-Kunden – mit ihren Daten – automatisch zu Azure Cosmos DB-Kunden. Der Übergang erfolgt reibungslos, und Sie haben jetzt Zugang zum neuen bahnbrechenden System und den Funktionen von Azure Cosmos DB. Diese Funktionen beziehen sich auf die Bereiche des Kerndatenbankmoduls sowie auf die globale Verteilung, die elastische Skalierbarkeit und branchenführende, umfassende SLAs. Wir haben das Azure Cosmos DB-Datenbankmodul so weiterentwickelt, dass alle beliebten Datenmodelle, Typsysteme und APIs dem zugrunde liegenden Datenmodell von Azure Cosmos DB effizient zugeordnet werden können. 

Für Entwickler ist dies derzeit durch die neue Unterstützung für [Gremlin](graph-introduction.md) und [Table Storage-APIs](table-introduction.md) erkennbar. Und dies ist erst der Anfang… Im Laufe der Zeit werden wir weitere beliebte APIs und neuere Datenmodelle mit Verbesserungen in Bezug auf die globale Leistung und Speicherung hinzufügen. 

In diesem Zusammenhang ist der Hinweis wichtig, dass der [SQL-Dialekt](../documentdb/documentdb-sql-query.md) von DocumentDB schon immer nur eine der vielen APIs gewesen ist, die von der zugrunde liegenden Cosmos DB-Technologie unterstützt werden konnten. Wenn Entwickler einen vollständig verwalteten Dienst wie Azure Cosmos DB nutzen, stellen die vom Dienst verfügbar gemachten APIs die einzige Schnittstelle zum Dienst dar. In dieser Hinsicht ändert sich für Sie als bestehender DocumentDB-Kunde so gut wie nichts. Azure Cosmos DB stellt genau die gleiche SQL-API wie DocumentDB bereit. Jetzt (und in Zukunft) erhalten Sie aber zusätzlich Zugriff auf andere Funktionen, die bisher noch nicht zugänglich waren. 

Ein weiterer Beweis für unsere fortwährende Arbeit ist das erweiterte Fundament für die globale und elastische Skalierbarkeit des Durchsatzes und Speichers. Eine der ersten Funktionen ist [RU/m](request-units-per-minute.md) (Anforderungseinheiten pro Minute), aber wir werden für diese Bereiche bald noch weitere Funktionen ankündigen. Mit diesen neuen Funktionen können wir die Kosten für unsere Kunden für unterschiedliche Workloads reduzieren. Wir haben am globalen Verteilungssystem einige grundlegende Verbesserungen vorgenommen. Einer der vielen Vorteile für Entwickler, die sich daraus ergeben, ist das Konsistenzmodell für Präfixe (insgesamt fünf klar definierte Konsistenzmodelle). Wir werden aber noch viele weitere interessante Funktionen veröffentlichen, sobald diese dafür bereit sind. 

## <a name="what-do-i-need-to-do-to-ensure-my-documentdb-resources-continue-to-run-on-azure-cosmos-db"></a>Was muss ich tun, um sicherzustellen, dass meine DocumentDB-Ressourcen unter Azure Cosmos DB weiterhin ausgeführt werden können?

Nichts. Sie müssen keine Änderungen vornehmen. Ihre DocumentDB-Ressourcen sind jetzt Azure Cosmos DB-Ressourcen. Für diese Umstellung war keine Unterbrechung des Diensts erforderlich.

## <a name="what-changes-do-i-need-to-make-for-my-app-to-work-with-azure-cosmos-db"></a>Welche Änderungen muss ich vornehmen, damit meine App mit Azure Cosmos DB funktioniert?

Es sind keine Änderungen erforderlich. Die Namen von Klassen, Namespaces und NuGet-Paketen haben sich nicht geändert. Wie immer gilt die Empfehlung, dass Sie Ihre SDKs aktuell halten sollten, um die neuesten Features und Verbesserungen nutzen zu können. 

## <a name="whats-changed-in-the-azure-portal"></a>Was hat sich im Azure-Portal geändert?

Azure DocumentDB wird im Portal nicht mehr als Azure-Dienst angezeigt. Stattdessen wird Azure Cosmos DB mit einem neuen Symbol angezeigt. Dies ist in der folgenden Abbildung dargestellt. Ihre Sammlungen sind wie gewohnt verfügbar, und Sie können weiterhin den Durchsatz skalieren, die Konsistenz ändern und SLAs überwachen. Die Funktionen des **Daten-Explorers (Vorschauversion)** wurden verbessert. Sie können nun Dokumente anzeigen und bearbeiten, Abfragen erstellen und ausführen und auf einer Seite mit gespeicherten Prozeduren, Triggern und UDFs arbeiten. Dies ist in der folgenden Abbildung dargestellt. 

![Anzeigen und Kopieren eines Zugriffsschlüssels im Azure-Portal auf dem Blatt „Schlüssel“](./media/welcome-documentdb-customers/cosmos-db-data-explorer.png)

## <a name="are-there-changes-to-pricing"></a>Haben sich die Preise geändert?

Nein. Die Kosten für die Ausführung Ihrer App unter Azure Cosmos DB sind genauso hoch wie vorher. Sie haben aber die Möglichkeit, vom neuen **Feature „Anforderungseinheiten pro Minute“** zu profitieren, das wir angekündigt haben. Weitere Informationen finden Sie im Artikel zur [minutenbezogenen Skalierung des Durchsatzes](request-units-per-minute.md).

## <a name="are-there-changes-to-the-service-level-agreements-slas"></a>Wurden Änderungen an den Vereinbarungen zum Servicelevel (SLAs) vorgenommen?

Nein. Die SLAs für Verfügbarkeit, Konsistenz, Wartezeit und Durchsatz sind unverändert geblieben und werden im Portal weiterhin angezeigt. Ausführliche Informationen zu den SLAs finden Sie unter [SLA für Azure Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/).
   
![To-Do-App mit Beispieldaten](./media/welcome-documentdb-customers/azure-cosmosdb-portal-metrics-slas.png)

## <a name="whats-next-with-azure-cosmos-db"></a>Wie geht es mit Azure Cosmos DB weiter?

Azure Cosmos DB ist ein Datenbankdienst, der ständig weiterentwickelt wird. Alle neuen Funktionen werden bei Microsoft anhand von umfangreichen Anwendungen überprüft, anschließend für wichtige externe Kunden verfügbar gemacht und schließlich weltweit veröffentlicht. 

Die neue Unterstützung für Gremlin und die Table Storage-API ist im Hinblick auf die beliebten APIs und Datenmodelle, die von Azure Cosmos DB unterstützt werden, nur der Anfang. Wir freuen uns darauf, nach der entsprechenden Ankündigung weitere neue Datenmodelle für Sie bereitzustellen. Darüber hinaus erzielen wir große Fortschritte bei der Leistung und Speicherung auf globaler Ebene. 

Wir danken Ihnen dafür, dass Sie Azure DocumentDB-Kunde sind, und freuen uns, dass wir Azure Cosmos DB für Sie – die Entwickler und Kunden in aller Welt – zur Verfügung stellen können. Unser Ziel besteht darin, der vertrauenswürdigste Datenbankdienst weltweit zu sein und Ihnen die Möglichkeit zu geben, leichter sehr leistungsstarke Cosmos-Apps zu erstellen. Wir laden Sie ein, die neuen Funktionen in Azure Cosmos DB auszuprobieren und uns Ihre Meinung dazu mitzuteilen!  Es wird spannend sein zu beobachten, was Sie mithilfe von Cosmos DB alles erstellen.

— Ihr Team von Azure Cosmos DB [@AzureCosmosDB](https://twitter.com/AzureCosmosDB)

