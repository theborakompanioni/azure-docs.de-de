<properties
   pageTitle="Was ist Azure Data Catalog? | Microsoft Azure"
   description="Dieser Artikel enthält eine Übersicht über Microsoft Azure Data Catalog, einschließlich seiner Funktionen und der Probleme, die er beheben soll. Data Catalog enthält Funktionen, mit denen alle Benutzer – von Analysten über Datenwissenschaftler bis hin zu Entwicklern – Datenquellen registrieren, ermitteln, verstehen und nutzen können."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="05/06/2016"
   ms.author="maroche"/>

# Was ist Azure Data Catalog?

Microsoft Azure Data Catalog ist ein vollständig verwalteter Clouddienst, der als Registrierungs- und Ermittlungssystem für die Datenquellen von Unternehmen dient. Data Catalog enthält Funktionen, mit denen alle Benutzer – von Analysten über Datenwissenschaftler bis hin zu Entwicklern – Datenquellen ausfindig machen, verstehen und nutzen können. Darüber hinaus können sie mit ihrem Wissen dazu beitragen, eine Community und eine datenorientierte Kultur aufzubauen und zu pflegen.

## Beschreibung des Problems – Motivation und Übersicht

Das Ermitteln der Datenquellen von Unternehmen war bisher ein natürlicher Prozess, der auf „Stammeswissen“ basierte. Dies schafft für Unternehmen, die aus ihren Informationsassets den größtmöglichen Nutzen ziehen möchten, eine Vielzahl von Problemen.

-	Benutzer wissen erst, dass Datenquellen vorhanden sind, wenn sie im Rahmen eines anderen Prozesses darauf stoßen. Es gibt keinen zentralen Speicherort, an dem Datenquellen registriert sind.
-	Nur wenn ein Benutzer den Speicherort der Datenquelle kennt, kann er mit einer Clientanwendung eine Verbindung damit herstellen. Für Oberflächen zur Datennutzung ist es erforderlich, dass Benutzer die Verbindungszeichenfolge bzw. den Pfad kennen.
-	Wenn ein Benutzer den Speicherort der Dokumentation für eine Datenquelle nicht kennt, kann er die beabsichtigte Verwendung der Daten nicht verstehen. Datenquellen und Dokumentation liegen an unterschiedlichen Orten vor und werden über unterschiedliche Umgebungen genutzt.
-	Wenn ein Benutzer Fragen zu einem Informationsasset hat, muss er den für die Daten verantwortlichen Experten bzw. das Team finden und diese Experten offline ansprechen. Es besteht keine explizite Verbindung zwischen Daten und den Personen, die dafür über Expertenwissen zur Nutzung verfügen.
-  Ein Benutzer muss den Prozess zum Anfordern des Zugriff auf die Datenquelle verstehen. Mit der ermittelten Datenquelle und der dazugehörigen Dokumentation allein kann er nämlich noch nicht auf die benötigten Daten zugreifen.

Dies sind die Herausforderungen für die Nutzer der Daten. Aber für Benutzer, die für die Erstellung und Pflege von Informationsassets verantwortlich sind, gelten ebenso besondere Herausforderungen.

-	Das Versehen von Datenquellen mit beschreibenden Metadaten führt häufig nicht zum gewünschten Erfolg. Beschreibungen, die in der Datenquelle gespeichert sind, werden von Clientanwendungen meist ignoriert.
-	Das Erstellen von Dokumentation für Datenquellen ist oft sinnlos. Die ständige Synchronisierung der Dokumentation mit der Datenquelle ist eine kontinuierliche Aufgabe, und Benutzer haben kein Vertrauen in die Dokumentation, da sie häufig als veraltet angesehen wird.
- Eine beständige Herausforderung besteht darin, den Zugriff auf die Datenquelle zu beschränken und sicherzustellen, dass die Datennutzer wissen, wie sie den Zugriff anfordern.

Das Erstellen und Pflegen der Dokumentation für eine Datenquelle ist komplex und zeitaufwendig. Dies gilt in noch stärkerem Maße für die Aufgabe, die Dokumentation für alle Personen leicht zugänglich zu machen, die die Datenquelle verwenden.

Zusammen stellen diese Probleme eine erhebliche Hürde für Unternehmen dar, die die Nutzung und das Verständnis der Unternehmensdaten fördern möchten.

## Dienstbeschreibung

Data Catalog wurde zur Lösung dieser Probleme konzipiert. Unternehmen können damit den größtmöglichen Nutzen aus ihren vorhandenen Informationsassets ziehen, indem sie die Daten für Benutzer, die die Daten für ihren jeweiligen Bereich benötigen, leicht zugänglich und verständlich machen.

Data Catalog bietet einen cloudbasierten Dienst, unter dem Datenquellen registriert werden können. Die Daten verbleiben an ihrem vorhandenen Speicherort, aber eine Kopie der Metadaten wird Data Catalog hinzugefügt – zusammen mit einem Verweis auf den Speicherort der Datenquelle. Diese Metadaten werden außerdem indiziert, damit jede Datenquelle per Suchfunktion leicht ermittelt werden kann und für die Benutzer verständlich ist, die sie ermitteln.

Nachdem eine Datenquelle registriert wurde, können die dazugehörigen Metadaten erweitert werden. Dies kann entweder von dem Benutzer übernommen werden, der die Registrierung durchgeführt hat, oder von anderen Benutzern des Unternehmens. Alle Benutzer können eine Datenquelle mit Anmerkungen versehen, indem Beschreibungen, Tags oder andere Metadaten bereitgestellt werden, z. B. Dokumentation und Prozesse für das Anfordern des Datenquellenzugriffs. Diese beschreibenden Metadaten sind eine Erweiterung der strukturellen Metadaten (z. B. Spaltennamen und Datentypen), die für die Datenquelle registriert wurden, um die Ermittlung und das Verständnis zu erleichtern.

Das Ermitteln und Verstehen von Datenquellen und ihrer Nutzung sind der Hauptzweck bei der Registrierung von Quellen. Wenn Benutzer in Unternehmen Daten für ihre Arbeit benötigen (z. B. Business Intelligence, Anwendungsentwicklung, Datenwissenschaft oder andere Aufgaben, die spezielle Daten erfordern), können sie die Data Catalog-Ermittlungsoberfläche verwenden. Hiermit können sie schnell die passenden Daten für ihre Anforderungen finden, Daten auf ihre Eignung hin untersuchen und Daten nutzen, indem sie die Datenquelle im Tool ihrer Wahl öffnen. Zugleich ermöglicht es Data Catalog Benutzern durch Markieren, Dokumentieren und Kommentieren bereits registrierter Datenquellen sowie durch die Registrierung neuer Datenquellen, welche dann wiederum von anderen Benutzern des Katalogs gefunden, nachvollzogen und genutzt werden können, am Katalog mitzuarbeiten.

![Funktionen von Data Catalog](./media/data-catalog-what-is-data-catalog/data-catalog-capabilities.png)

## Registrieren von Datenquellen

Die Registrierung von Datenquellen erfolgt über das Data Catalog-Tool zum Registrieren von Datenquellen. Sie können diese Anwendung im Data Catalog-Portal herunterladen.

Der Registrierungsprozess umfasst drei grundlegende Schritte:

1.	Herstellen einer Verbindung mit einer Datenquelle: Der Benutzer gibt den Speicherort der Datenquelle und die Anmeldeinformationen an, um die Verbindung mit der Datenquelle herzustellen, z. B. einer SQL Server-Instanz.
2.	Auswählen zu registrierender Objekte: Der Benutzer wählt die Objekte am angegebenen Speicherort aus, die in Data Catalog registriert werden sollen. Dies kann der vollständige Tabellensatz in allen Datenbanken oder eine speziell ausgewählte Teilmenge der Tabellen und Sichten sein.
3.	Vollständige Registrierung: Der Benutzer schließt den Prozess ab, und das Tool für die Datenquellenregistrierung extrahiert die strukturellen Metadaten aus der Datenquelle und sendet sie an den Data Catalog-Clouddienst.

> [AZURE.NOTE] Informationen zum Anzeigen einer Liste mit den von Data Catalog unterstützten Datenquellen und Assettypen finden Sie unter [Von Azure Data Catalog unterstützte Datenquellen](data-catalog-dsr.md).


> [AZURE.IMPORTANT]Beim Registrieren einer Datenquelle in Data Catalog werden die Daten nicht aus der Datenquelle kopiert, es sei denn, Sie wählen im Tool für die Datenquellenregistrierung die Option „Vorschau einfügen“. Bei der Registrierung werden Datenquellenmetadaten kopiert, keine Daten. Beispiele für Metadaten sind die Namen von Tabellen und anderen Datenquellenobjekten sowie die Namen und Datentypen von Spalten und anderen Datenquellenattributen. Die Metadaten enthalten auch den Speicherort der Datenquelle, damit Benutzer, die die Datenquelle mit Data Catalog ermitteln, eine Verbindung damit herstellen können. Wenn Sie die Option „Vorschau einfügen“ wählen, werden mit dem Tool für die Registrierung von Datenquellen auch einige Datensätze in Data Catalog kopiert. Diese Datensätze werden Benutzern angezeigt, die die Datenquelle im Data Catalog-Portal ermitteln.

## Erweitern der Metadaten von Datenquellen

Nachdem die Registrierung abgeschlossen ist, können die Datenquellen ermittelt und genutzt werden. Der eigentliche Nutzen von Data Catalog besteht aber darin, dass aussagekräftige geschäftliche Metadaten in der gleichen Umgebung zur Verfügung stehen wie die strukturellen Metadaten, die aus der Datenquelle extrahiert wurden. Diese zusätzliche Metadaten bieten drei wesentliche Vorteile:

- Die registrierten Datenquellen sind leichter ermittelbar. Die vom Benutzer bereitgestellten Metadaten werden dem Suchindex von Data Catalog hinzugefügt. So können Benutzer Daten ermitteln, indem sie Wörter und Begriffe verwenden, die in der ursprünglichen Datenquelle unter Umständen nicht vorhanden sind. Wenn eine Datenbanktabelle mit Kundendaten beispielsweise den Namen „tbl\_c45“ hat, wird sie durch die Angabe des Anzeigenamens „Kunde“ für Benutzer, die nach Kundendaten suchen, leichter ermittelbar. Ebenso wird eine Datenquelle durch die Angabe einer Beschreibung mit Namen von Berichten, Dashboards oder Prozessen, von denen die Daten verwendet werden, leichter für Benutzer ermittelbar, die nachgeschaltete Artefakte als Suchbegriffe verwenden.
- Die registrierten Datenquellen sind nach ihrer Ermittlung leichter verständlich. Die von Benutzern bereitgestellten Metadaten werden allen Data Catalog-Benutzern angezeigt, die die mit Anmerkungen versehene Datenquelle aufrufen, was die Bereitstellung von zusätzlichem Kontext und Zusatzinformationen ermöglicht. Die meisten Datenquellen enthalten häufig keine aussagekräftigen Beschreibungen oder Dokumentation, und wenn dies doch einmal der Fall ist, liegt der Schwerpunkt häufig auf technischen DBA-Angaben oder Informationen für Datenbankentwickler. Dank der Möglichkeit, Datenquellen in Data Catalog mit zielgruppenrelevanten Beschreibungen und Tags zu versehen, können Benutzer andere Benutzer, die auf die Daten stoßen, über Details und den Nutzungszweck der Daten informieren.
- Mithilfe des Unternehmensglossars von Data Catalog können Data Catalog-Administratoren eine einheitliche Terminologie für das Unternehmen definieren. Registrierte Datenassets können dann mit Begriffen aus dem Glossar gekennzeichnet werden, um zusätzlichen Kontext bereitzustellen und die Aussagekraft der Daten zu erhöhen. Weitere Informationen finden Sie unter [How to set up the Business Glossary for Governed Tagging](data-catalog-how-to-business-glossary.md) (Gewusst wie: Einrichten des Unternehmensglossars für verwaltete Markierungen).
- Jede registrierte Datenquelle kann Informationen zur Zugriffsanforderung einschließen, sodass Benutzer vorhandene Prozesse einfach verstehen und ausführen können, um Zugriff auf die Datenquelle und ihre Daten anzufordern.

> [AZURE.NOTE] Jeder Data Catalog-Benutzer kann eigene Tags und Beschreibungen für Datenassets und Attribute hinzufügen. Data Catalog verfolgt den Wert und die Quelle jeder Anmerkung nach und zeigt den Benutzer an, der die Anmerkung hinzugefügt hat. Mit diesem Crowdsourcing-Ansatz für Metadaten wird sichergestellt, dass jeder Benutzer mit Bezug zu den Daten und ihrer Nutzung Meinungen und Ressourcen mit der gesamten Community teilen kann.

## Untersuchen, Ermitteln und Verstehen

Das Ziel der Registrierung und Erweiterung von Datenquellen in Data Catalog besteht darin, dass sie von Benutzern im gesamten Unternehmen ermittelt, verstanden und genutzt werden können. Das Data Catalog-Portal ist das wichtigste Tool für diesen Prozess.

Für die Untersuchung und Ermittlung von Daten verfügt das Data Catalog-Portal über zwei Hauptmechanismen: Suche und Filterung.

Geben Sie zum Suchen nach Datenquellen mit Data Catalog einfach einen Suchbegriff in das Suchfeld des Data Catalog-Portals ein. Im Portal wird für jede registrierte Datenquelle eine Kachel angezeigt, für die sich eine Übereinstimmung mit dem Suchbegriff ergibt. Die Kacheln enthalten Angaben wie Name, Beschreibung und Tags, die der Datenquelle zugewiesen sind, sowie andere allgemeine Informationen.

Wählen Sie zum Filtern des Inhalts von Data Catalog einfach einen oder mehrere Filter aus, die im Data Catalog-Portal zur Verfügung stehen. So werden die im Portal angezeigten Kacheln auf die Kacheln beschränkt, die mit den angegebenen Filterkriterien übereinstimmen. Sie können Datenquellen filtern, ohne zu suchen, oder Sie können die Ergebnisse einer Suche filtern.

Um ausführlichere Informationen für eine Datenquelle anzuzeigen und zu verstehen, ob sie für eine anstehende Aufgabe geeignet ist, klicken Sie einfach auf die Kachel für die Datenquelle. Der Eigenschaftenbereich wird angezeigt und enthält alle dazugehörigen Metadaten.

Oben im Eigenschaftenbereich befinden sich weitere Schaltflächen:

1.	Vorschau: Wenn Sie diese Schaltfläche auswählen, wird die statische Gruppe der Vorschaudatensätze aus der Datenquelle angezeigt, wenn während der Registrierung der Datenquelle die Vorschaufunktion ausgewählt wurde.
2.	Schema: Wenn Sie diese Schaltfläche auswählen, werden das Schema für die Datenquelle (einschließlich Spaltennamen und Datentypen) sowie alle Metadaten auf Spaltenebene in Data Catalog angezeigt.

> [AZURE.NOTE] Beachten Sie unbedingt, dass die **Ermittlung** auch ein Einstiegspunkt in die **Erweiterung** darstellen kann, nicht nur in die **Nutzung**. Der Crowdsourcing-Ansatz von Data Catalog bedeutet, dass alle Benutzer, die eine registrierte Datenquelle ermitteln, die ermittelten Daten nicht nur nutzen, sondern auch ihre Meinung zu den Daten teilen können.

## Entfernen der Metadaten von Datenquellen

Nachdem eine Datenquelle registriert wurde, kann es manchmal erforderlich sein, den Datenquellenverweis aus Data Catalog zu entfernen. Dies kann daran liegen, dass sich geschäftliche Anforderungen ändern oder dass das Quellsystem eingestellt wird. Unabhängig von der jeweiligen Ursache lassen sich Datenquellen in Data Catalog ganz einfach löschen, sodass sie nicht mehr ermittelt oder genutzt werden können.

> [AZURE.IMPORTANT] Beim Löschen einer Datenquelle aus Data Catalog werden nur die im Data Catalog-Dienst gespeicherten Metadaten gelöscht. Dies wirkt sich nicht auf die ursprüngliche Datenquelle aus.

## Nutzen von Datenquellen

Das ultimative Ziel der Datenermittlung besteht darin, die erforderlichen Daten zu finden und im bevorzugten Datentool zu verwenden. Dies ist in der Data Catalog-Umgebung für die Datennutzung auf zwei Arten möglich.

1.	Für Clientanwendungen, die von Data Catalog direkt unterstützt werden, können Benutzer im Portal auf der Datenquellenkachel auf das Menü **Öffnen in** klicken. Die Clientanwendung wird mit einer hergestellten Verbindung mit der gewählten Datenquelle gestartet.
2.	Für alle Clientanwendungen können Benutzer die Verbindungsinformationen verwenden, die im Eigenschaftenbereich für eine ausgewählte Datenquelle angezeigt werden. Diese Informationen enthalten alle Details (z. B. Servername, Datenbankname und Objektname), die zum Herstellen der Verbindung mit den Daten erforderlich sind. Sie können in die Verbindungsumgebung des Clienttools kopiert werden. Wenn Details zur Zugriffsanforderung für eine Datenquelle bereitgestellt wurden, werden diese Information neben den Verbindungsdetails angezeigt.

<!---HONumber=AcomDC_0511_2016-->