<properties
	pageTitle="Importieren von Daten in Azure Search mithilfe von Indexern im Azure-Portal | Microsoft Azure | Gehosteter Cloudsuchdienst"
	description="Verwenden Sie den Datenimport-Assistenten von Azure Search im Azure-Portal, um Daten in Azure Blob Storage, Table Stroage, SQL-Datenbank und SQL Server auf virtuellen Azure-Computern zu durchforsten."
	services="search"
	documentationCenter=""
	authors="HeidiSteen"
	manager="jhubbard"
	editor=""
    tags="Azure Portal"/>

<tags
	ms.service="search"
	ms.devlang="na"
	ms.workload="search"
	ms.topic="get-started-article"
	ms.tgt_pltfrm="na"
	ms.date="08/29/2016"
	ms.author="heidist"/>

# Importieren von Daten in Azure Search über das Portal

Im Azure-Portal steht auf dem Azure Search-Dashboard der **Datenimport-Assistent** zur Verfügung, mit dem Sie Daten in einen Index laden können.

  ![„Daten importieren“ auf der Befehlsleiste][1]

Intern wird von dem Assistenten ein *Indexer* konfiguriert und aufgerufen, um mehrere Schritte des Indizierungsprozesses zu automatisieren:

- Herstellen einer Verbindung mit einer externen Datenquelle im aktuellen Azure-Abonnement
- Automatisches Generieren eines Indexschemas auf der Grundlage der Quelldatenstruktur
- Erstellen von Dokumenten auf der Grundlage eines aus der Datenquelle abgerufenen Rowsets
- Hochladen von Dokumenten an den Index in Ihrem Suchdienst

Sie können diesen Workflow mithilfe von Beispieldaten in DocumentDB testen. Eine Anleitung hierzu finden Sie unter [Erste Schritte mit Azure Search im Portal](search-get-started-portal.md).

## Vom Datenimport-Assistenten unterstützte Datenquellen

Der Datenimport-Assistent unterstützt die folgenden Datenquellen:

- Azure SQL-Datenbank
- Relationale SQL Server-Daten auf einem virtuellen Azure-Computer
- Azure DocumentDB
- Azure Blob Storage (Vorschau)
- Azure Table Storage (Vorschau)

Als Eingabe ist ein vereinfachtes Dataset erforderlich. Sie können Daten nur aus einer einzelnen Tabelle, Datenbanksicht oder entsprechenden Datenstruktur importieren. Diese Datenstruktur muss vor dem Ausführen des Assistenten erstellt werden.

Beachten Sie, dass sich einige der Indexer noch in der Vorschauphase befinden. Das bedeutet, dass die Indexer-Definition von der Vorschauversion der API unterstützt wird. Weitere Informationen und Links finden Sie in der [Indexer-Übersicht](search-indexer-overview.md).

## Herstellen einer Verbindung mit Ihren Daten

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und öffnen Sie das Servicedashboard. Sie können auf der Navigationsleiste auf **Suchdienste** klicken, um die vorhandenen Dienste des aktuellen Abonnements anzuzeigen.

2. Klicken Sie auf der Befehlsleiste auf **Daten importieren**, um das gleichnamige Blatt zu öffnen.

3. Klicken Sie auf **Mit Ihren Daten verbinden**, um eine von einem Indexer verwendete Datenquellendefinition anzugeben. Bei abonnementinternen Datenquellen kann der Assistent Verbindungsinformationen in der Regel erkennen und lesen, was insgesamt den Konfigurationsaufwand minimiert.

| | |
|--------|------------|
|**Vorhandene Datenquelle** | Wenn Sie in Ihrem Suchdienst bereits Indexer definiert haben, können Sie eine vorhandene Datenquellendefinition für einen weiteren Importvorgang auswählen.|
|**Azure SQL-Datenbank** | Dienstname, Anmeldeinformationen für einen Datenbankbenutzer mit Leseberechtigung und ein Datenbankname können entweder auf der Seite oder über eine ADO.NET-Verbindungszeichenfolge angegeben werden. Wenn Sie Eigenschaften anzeigen oder anpassen möchten, verwenden Sie die Verbindungszeichenfolgen-Option. <br/><br/>Die Tabelle oder Sicht, die das Rowset bereitstellt, muss auf der Seite angegeben werden. Diese Option wird nach erfolgreicher Verbindungsherstellung in einer Dropdownliste angezeigt.|
|**SQL Server auf virtuellen Azure-Computern** | Geben Sie einen vollqualifizierten Dienstnamen, eine Benutzer-ID, ein Kennwort und eine Datenbank als Verbindungszeichenfolge an. Um diese Datenquelle verwenden zu können, müssen Sie zuvor ein Zertifikat im lokalen Speicher installieren haben, das die Verbindung verschlüsselt. <br/><br/>Die Tabelle oder Sicht, die das Rowset bereitstellt, muss auf der Seite angegeben werden. Diese Option wird nach erfolgreicher Verbindungsherstellung in einer Dropdownliste angezeigt.
|**DocumentDB** |Zu den erforderlichen Angaben gehören das Konto, die Datenbank und die Sammlung. Alle Dokumente in der Sammlung werden in den Index aufgenommen. Sie können eine Abfrage zum Vereinfachen oder Filtern des Rowsets oder eine Abfrage zum Erkennen geänderter Dokumente bei nachfolgenden Datenaktualisierungsvorgängen definieren.|
|**Azure Blob Storage** | Zu den erforderlichen Angaben gehören das Speicherkonto und ein Container. Optional: Wenn zu Gruppierungszwecken für Blobnamen eine virtuelle Benennungskonvention verwendet wird, können Sie den virtuellen Verzeichnisteil des Namens als Ordner unter dem Container angeben. Weitere Informationen finden Sie unter [Indizieren von Dokumenten in Azure Blob Storage mit Azure Search](search-howto-indexing-azure-blob-storage.md). |
|**Azure Table Storage** | Zu den erforderlichen Angaben gehören das Speicherkonto und ein Tabellenname. Optional können Sie eine Abfrage zum Abrufen einer Teilmenge der Tabellen angeben. Weitere Informationen finden Sie unter [Indizieren von Azure Table Storage mit Azure Search](search-howto-indexing-azure-tables.md). |

## Anpassen des Zielindex

Vom Dataset wird üblicherweise ein vorläufiger Index abgeleitet. Vervollständigen Sie das Schema, indem Sie Felder hinzufügen, bearbeiten oder löschen. Legen Sie außerdem Attribute auf Feldebene fest, um das Verhalten bei nachfolgenden Suchvorgängen zu bestimmen.

1. Geben Sie unter **Zielindex anpassen** den Namen und einen **Schlüssel** zur eindeutigen Identifizierung der einzelnen Dokumente an. Der Schlüssel muss eine Zeichenfolge sein. Wenn Feldwerte Leerzeichen oder Bindestriche enthalten, müssen unter **Import your data** (Daten importieren) erweiterte Optionen festgelegt werden, um die Prüfung auf diese Zeichen zu unterdrücken.

2. Überprüfen und überarbeiten Sie die restlichen Felder. Der Feldname und -typ werden meist für Sie ausgefüllt. Sie können den Datentyp ändern.

3. Legen Sie Indexattribute für die einzelnen Felder fest:

 - „Abrufbar“ gibt das Feld in Suchergebnissen zurück.
 - „Filterbar“ ermöglicht, dass in Filterausdrücken auf das Feld verwiesen wird.
 - „Sortierbar“ ermöglicht, dass das Feld in einer Sortierung verwendet wird.
 - „Facettierbar“ aktiviert das Feld für die Facettennavigation.
 - „Durchsuchbar“ aktiviert die Volltextsuche.
  
4. Klicken Sie auf die Registerkarte **Analyse**, wenn Sie eine Sprachanalyse auf Feldebene angeben möchten. Derzeit können nur Sprachanalysen angegeben werden. Zur Verwendung benutzerdefinierter Analysen oder einer sprachfremden Analyse wie Schlüsselwort oder Muster muss Code geschrieben werden.

 - Klicken Sie auf **Durchsuchbar**, um die Volltextsuche für das Feld festzulegen und die Analyse-Dropdownliste zu aktivieren.
 - Wählen Sie die gewünschte Analyse aus. Details finden Sie unter [Erstellen eines Indexes für Dokumente in mehreren Sprachen](search-language-support.md).

5. Klicken Sie auf **Vorschläge**, um für ausgewählte Felder Textvervollständigungsfunktionen bei der Abfrage zu aktivieren.


## Importieren Ihrer Daten

1. Geben Sie unter **Import your data** (Daten importieren) einen Namen für den Indexer an. Denken Sie daran, dass der Datenimport-Assistent einen Indexer generiert. Wenn Sie diesen später anzeigen oder bearbeiten möchten, können Sie ihn über das Portal auswählen, anstatt erneut den Assistenten auszuführen.

2. Geben Sie den Zeitplan an. Dieser basiert auf der regionalen Zeitzone, in der der Dienst bereitgestellt wird.

3. Legen Sie erweiterte Optionen zum Angeben von Schwellenwerten an, die bestimmen, ob die Indizierung fortgesetzt werden kann, wenn ein Dokument verworfen wird. Außerdem können Sie angeben, ob Felder vom Typ **Schlüssel** Leerzeichen und Schrägstriche enthalten dürfen.

## Bearbeiten eines vorhandenen Indexers

Doppelklicken Sie im Dashboard des Diensts auf die Kachel „Indexer“, um eine Liste mit allen Indexern einzublenden, die für Ihr Abonnement erstellt wurden. Doppelklicken Sie auf einen der Indexer, um ihn auszuführen, zu bearbeiten oder zu löschen. Sie können während der Indizierung den Index durch einen anderen vorhandenen Index ersetzen, die Datenquelle ändern und Optionen für Fehlerschwellenwerte festlegen.

## Bearbeiten eines vorhandenen Index

In Azure Search ist für strukturelle Updates eines Index eine Neuerstellung des Index erforderlich. Dabei wird der Index gelöscht und neu erstellt, und die Daten werden erneut geladen. Zu strukturellen Updates zählen das Ändern des Datentyps und das Umbenennen oder Löschen eines Felds.

Änderungen, für die keine Neuerstellung erforderlich ist, beinhalten das Hinzufügen eines neuen Felds sowie das Ändern von Bewertungsprofilen, Vorschlägen und Sprachanalysen. Weitere Informationen finden Sie unter [Update Index](https://msdn.microsoft.com/library/azure/dn800964.aspx) (Aktualisieren des Index).

## Nächster Schritt

Unter den folgenden Links finden Sie weitere Informationen zu Indexern:

- [Verbinden von Azure SQL-Datenbank mit Azure Search mithilfe von Indexern](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)
- [Herstellen einer Verbindung zwischen DocumentDB und Azure Search unter Verwendung von Indexern](../documentdb/documentdb-search-indexer.md)
- [Indizieren von Dokumenten in Azure Blob Storage mit Azure Search](search-howto-indexing-azure-blob-storage.md)
- [Indizieren von Azure Table Storage mit Azure Search](search-howto-indexing-azure-tables.md)



<!--Image references-->
[1]: ./media/search-import-data-portal/search-import-data-command.png

<!---HONumber=AcomDC_0928_2016-->