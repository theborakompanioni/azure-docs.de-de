<properties
	pageTitle="Einrichten des Unternehmensglossars für gesteuertes Markieren | Microsoft Azure"
	description="Anleitungsartikel zum Unternehmensglossar in Azure Data Catalog zum Definieren und Verwenden eines allgemeinen Geschäftsvokabulars zum Markieren registrierter Datenassets."
	services="data-catalog"
	documentationCenter=""
	authors="steelanddata"
	manager="NA"
	editor=""
	tags=""/>
<tags
	ms.service="data-catalog"
	ms.devlang="NA"
	ms.topic="article"
	ms.tgt_pltfrm="NA"
	ms.workload="data-catalog"
	ms.date="09/21/2016"
	ms.author="maroche"/>

# Einrichten des Unternehmensglossars für gesteuertes Markieren

## Einführung

Azure Data Catalog enthält Funktionen für die Datenquellenermittlung. Hiermit können Benutzer Datenquellen, die sie zum Durchführen von Analysen und Treffen von Entscheidungen benötigen, leicht ermitteln und verstehen. Diese Ermittlungsfunktionen haben den größten Nutzen, wenn die Benutzer möglichst viele verfügbare Datenquellen finden und verstehen können.

Ein Data Catalog-Feature, dass das Verständnis von Assetdaten fördert, ist das Markieren. Das Markieren ermöglicht Benutzern, Schlüsselwörtern ein Asset oder eine Spalte zuzuordnen, was wiederum das Ermitteln des Assets über Suchvorgänge vereinfacht, und erleichtert Benutzern, den Kontext und die Absicht des Assets zu verstehen.

Das Markieren kann jedoch gelegentlich spezielle Probleme verursachen. Dies sind Beispiele für Probleme, die das Markieren verursachen kann:

1.	Benutzer verwenden beim Markieren Abkürzungen für manche Assets und erweiterten Text für andere. Diese Inkonsistenz behindert die Ermittlung von Assets, obwohl das Ziel war, die Assets mit dem gleichen Tag zu markieren.
2.	Tags, die in unterschiedlichen Kontexten unterschiedliche Bedeutungen haben. So könnte sich z.B. ein Tag namens „Umsatz“ auf einem Kundendataset auf den Umsatz pro Kunde beziehen, aber das gleiche Tag könnte sich auf einem Quartalsverkaufsdataset auf den Quartalsumsatz des Unternehmens beziehen.

Um diesen und anderen, ähnlichen Problemen zu begegnen, enthält Data Catalog ein Unternehmensglossar.

Das Unternehmensglossar von Data Catalog ermöglicht Organisationen, die wichtigsten Geschäftsbegriffe und ihre Definitionen zu dokumentieren, um ein allgemeines Geschäftsvokabular zu erstellen. Diese Governance sorgt für Konsistenz bei der Verwendung von Daten in der gesamten Organisation. Nachdem Begriffe im Unternehmensglossar definiert worden sind, können sie Datenassets im Katalog zugewiesen werden, wobei der gleiche Ansatz wie beim Markieren verwendet und so _gesteuertes Markieren_ ermöglicht wird.

> [AZURE.NOTE] Die in diesem Artikel beschriebenen Funktionen sind nur in der Standard Edition von Azure Data Catalog verfügbar. Die kostenlose Edition bietet keine Funktionen für gesteuertes Markieren bzw. ein Unternehmensglossar.

## Glossarverfügbarkeit und Berechtigungen

/*Das Unternehmensglossar ist in der Standard Edition von Azure Data Catalog verfügbar. Die kostenlose Edition von Data Catalog enthält kein Glossar.*/

Der Zugriff auf das Unternehmensglossar ist über die Option „Glossar“ im Navigationsmenü des Data Catalog-Portals möglich.

![Zugriff auf das Unternehmensglossar](./media/data-catalog-how-to-business-glossary/01-portal-menu.png)


Data Catalog-Administratoren und Mitglieder der Rolle „Glossaradministratoren“ können Glossarbegriffe im Unternehmensglossar erstellen, bearbeiten und löschen. Alle Data Catalog-Benutzer können die Begriffsdefinitionen anzeigen und Assets mit Glossarbegriffen markieren.

![Hinzufügen eines neuen Glossarbegriffs](./media/data-catalog-how-to-business-glossary/02-new-term.png)


## Erstellen von Glossarbegriffen

Data Catalog-Administratoren und Glossaradministratoren können neue Glossarbegriffe erstellen, indem sie auf die Schaltfläche „Neuer Begriff“ klicken, um Glossarbegriffe mit den folgenden Feldern zu erstellen:

* Eine geschäftliche Definition für den Begriff
* Eine Beschreibung, die die beabsichtigte Verwendung oder die Geschäftsregeln für das Asset/die Spalte erfasst
* Eine Liste der Beteiligten, die den Begriff am besten kennen
* Der übergeordnete Begriff, der die Hierarchie definiert, in die der Begriff eingegliedert ist


## Hierarchien von Glossarbegriffen

Mit dem Data Catalog-Unternehmensglossar können Sie Ihr Geschäftsvokabular in einer Hierarchie von Begriffen darstellen. So können Organisationen eine Klassifizierung der Begriffe erstellen, die ihre Unternehmenstaxonomie besser darstellt.

Der Name eines Begriffs muss auf einer bestimmten Ebene der Hierarchie eindeutig sein – doppelte Namen sind nicht zulässig. Es gibt keine Beschränkung der Anzahl der Hierarchieebenen, aber eine aus höchstens drei Ebenen bestehende Hierarchie ist oft leichter zu verstehen.

Die Verwendung von Hierarchien im Unternehmensglossar ist optional. Wenn Sie das übergeordnete Begriffsfeld für Glossarbegriffe frei lassen, wird eine flache (nicht hierarchische) Liste von Begriffen im Glossar erstellt.

## Markieren von Assets mit Glossarbegriffen

Sobald Glossarbegriffe innerhalb des Katalogs definiert wurden, ist die Erfahrung des Markierens von Assets optimiert für das Durchsuchen des Glossars, sobald der Benutzer ihren Tag eingibt. Das Data Catalog-Portal zeigt dem Benutzer eine Liste der übereinstimmenden Glossarbegriffe zur Auswahl an. Wenn der Benutzer einen Glossarbegriff aus der Liste auswählt, wird er dem Asset als Tag (auch bekannt als Glossartag) hinzugefügt. Wahlweise kann der Benutzer auch ein neues Tag erstellen, indem er einen nicht im Glossar enthaltenen Begriff eingibt (auch bekannt als Benutzertag).

![Datenasset, das mit einem Benutzertag und zwei Glossartags markiert ist](./media/data-catalog-how-to-business-glossary/03-tagged-asset.png)

> [AZURE.NOTE] Benutzertags sind der einzige Tagtyp, der in der kostenlosen Edition von Data Catalog unterstützt wird.

### Zeigeverhalten bei Tags
Im Data Catalog-Portal unterscheiden sich die zwei Tagtypen visuell, auch ihr Zeigeverhalten ist unterschiedlich. Wenn der Benutzer den Cursor auf ein Benutzertag setzt, sieht er den Tagtext und den oder die Benutzer, die das Tag hinzugefügt haben. Wenn der Benutzer den Cursor auf ein Glossartag setzt, sieht er auch die Definition des Glossarbegriffs und einen Link zum Öffnen des Unternehmensglossars, um die vollständige Definition des Begriffs anzuzeigen.

### Suchfilter für Tags
Sowohl Glossar- als auch Benutzertags können durchsucht und als Filter in einer Suche angewendet werden.

## Zusammenfassung
Dank des Unternehmensglossars in Azure Data Catalog und des dadurch möglichen gesteuerten Markierens können Datenassets konsistent identifiziert, verwaltet und ermittelt werden. Das Unternehmensglossar kann das Lernen des Geschäftsvokabulars unter den Benutzern einer Organisation fördern und unterstützt die Erfassung sinnvoller Metadaten, sodass Ermittlung und Verständnis eines Assets völlig unkompliziert sind.

## Weitere Informationen

- [REST-API-Dokumentation für Unternehmensglossarvorgänge](https://msdn.microsoft.com/library/mt708855.aspx)

<!---HONumber=AcomDC_0921_2016-->