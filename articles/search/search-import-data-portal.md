---
title: "Importieren von Daten in Azure Search über das Portal | Microsoft-Dokumentation"
description: Verwenden Sie den Datenimport-Assistenten von Azure Search im Azure-Portal, um Azure-Daten aus NoSQL Azure Cosmos DB, Blob Storage, Table Storage, SQL-Datenbank und SQL Server auf virtuellen Azure-Computern zu durchforsten.
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: Azure Portal
ms.assetid: f40fe07a-0536-485d-8dfa-8226eb72e2cd
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: heidist
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: a3e6dd66197a17bfdc80c04130e198b787692a58
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="import-data-to-azure-search-using-the-portal"></a>Importieren von Daten in Azure Search über das Portal
Im Azure-Portal steht auf dem Azure Search-Dashboard der **Datenimport-Assistent** zur Verfügung, mit dem Sie Daten in einen Index laden können. 

  ![„Daten importieren“ auf der Befehlsleiste][1]

Intern wird von dem Assistenten ein *Indexer*konfiguriert und aufgerufen, um mehrere Schritte des Indizierungsprozesses zu automatisieren: 

* Herstellen einer Verbindung mit einer externen Datenquelle im gleichen Azure-Abonnement
* Generieren eines anpassbaren Indexschemas auf der Grundlage der Quelldatenstruktur
* Laden von JSON-Dokumenten in einen Index unter Verwendung eines aus der Datenquelle abgerufenen Rowsets

Sie können diesen Workflow mithilfe von Beispieldaten in Azure Cosmos DB testen. Eine Anleitung hierzu finden Sie unter [Erste Schritte mit Azure Search im Portal](search-get-started-portal.md) .

> [!NOTE]
> Starten Sie über das Azure Cosmos DB-Dashboard den **Datenimport-Assistenten**, um die Indizierung für diese Datenquelle zu vereinfachen. Navigieren Sie im linken Navigationsbereich zu **Sammlungen** > **Azure Search hinzufügen**, um mit dem Vorgang zu beginnen.

## <a name="data-sources-supported-by-the-import-data-wizard"></a>Vom Datenimport-Assistenten unterstützte Datenquellen
Der Datenimport-Assistent unterstützt die folgenden Datenquellen: 

* Azure SQL-Datenbank
* Relationale SQL Server-Daten auf einem virtuellen Azure-Computer
* Azure Cosmos DB
* Azure-Blobspeicher
* Azure-Tabellenspeicher

Als Eingabe ist ein vereinfachtes Dataset erforderlich. Sie können Daten nur aus einer einzelnen Tabelle, Datenbanksicht oder entsprechenden Datenstruktur importieren. Diese Datenstruktur muss vor dem Ausführen des Assistenten erstellt werden.

## <a name="connect-to-your-data"></a>Herstellen einer Verbindung mit Ihren Daten
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und öffnen Sie das Servicedashboard. Sie können auf der Navigationsleiste auf **Weitere Dienste** klicken, um nach vorhandenen Suchdiensten des aktuellen Abonnements zu suchen. 
2. Klicken Sie auf der Befehlsleiste auf **Daten importieren** , um das gleichnamige Blatt zu öffnen.  
3. Klicken Sie auf **Mit Ihren Daten verbinden** , um eine von einem Indexer verwendete Datenquellendefinition anzugeben. Bei abonnementinternen Datenquellen kann der Assistent Verbindungsinformationen in der Regel erkennen und lesen, was insgesamt den Konfigurationsaufwand minimiert.

|  |  |
| --- | --- |
| **Vorhandene Datenquelle** |Wenn Sie in Ihrem Suchdienst bereits Indexer definiert haben, können Sie eine vorhandene Datenquellendefinition für einen weiteren Importvorgang auswählen. |
| **Azure SQL-Datenbank** |Dienstname, Anmeldeinformationen für einen Datenbankbenutzer mit Leseberechtigung und ein Datenbankname können entweder auf der Seite oder über eine ADO.NET-Verbindungszeichenfolge angegeben werden. Wenn Sie Eigenschaften anzeigen oder anpassen möchten, verwenden Sie die Verbindungszeichenfolgen-Option. <br/><br/>Die Tabelle oder Sicht, die das Rowset bereitstellt, muss auf der Seite angegeben werden. Diese Option wird nach erfolgreicher Verbindungsherstellung in einer Dropdownliste angezeigt. |
| **SQL Server auf virtuellen Azure-Computern** |Geben Sie einen vollqualifizierten Dienstnamen, eine Benutzer-ID, ein Kennwort und eine Datenbank als Verbindungszeichenfolge an. Um diese Datenquelle verwenden zu können, müssen Sie zuvor ein Zertifikat im lokalen Speicher installieren haben, das die Verbindung verschlüsselt. Eine Anleitung finden Sie unter [SQL-VM-Verbindung mit Azure Search](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md). <br/><br/>Die Tabelle oder Sicht, die das Rowset bereitstellt, muss auf der Seite angegeben werden. Diese Option wird nach erfolgreicher Verbindungsherstellung in einer Dropdownliste angezeigt. |
| **Azure Cosmos DB** |Zu den erforderlichen Angaben gehören das Konto, die Datenbank und die Sammlung. Alle Dokumente in der Sammlung werden in den Index aufgenommen. Sie können eine Abfrage zum Vereinfachen oder Filtern des Rowsets oder eine Abfrage zum Erkennen geänderter Dokumente bei nachfolgenden Datenaktualisierungsvorgängen definieren. |
| **Azure Blob Storage** |Zu den erforderlichen Angaben gehören das Speicherkonto und ein Container. Optional: Wenn zu Gruppierungszwecken für Blobnamen eine virtuelle Benennungskonvention verwendet wird, können Sie den virtuellen Verzeichnisteil des Namens als Ordner unter dem Container angeben. Weitere Informationen finden Sie unter [Indizieren von Dokumenten in Azure Blob Storage mit Azure Search](search-howto-indexing-azure-blob-storage.md). |
| **Azure Table Storage** |Zu den erforderlichen Angaben gehören das Speicherkonto und ein Tabellenname. Optional können Sie eine Abfrage zum Abrufen einer Teilmenge der Tabellen angeben. Weitere Informationen finden Sie unter [Indizieren von Azure Table Storage mit Azure Search](search-howto-indexing-azure-tables.md). |

## <a name="customize-target-index"></a>Anpassen des Zielindex
Vom Dataset wird üblicherweise ein vorläufiger Index abgeleitet. Vervollständigen Sie das Schema, indem Sie Felder hinzufügen, bearbeiten oder löschen. Legen Sie außerdem Attribute auf Feldebene fest, um das Verhalten bei nachfolgenden Suchvorgängen zu bestimmen.

1. Geben Sie unter **Zielindex anpassen** den Namen und einen **Schlüssel** zur eindeutigen Identifizierung der einzelnen Dokumente an. Der Schlüssel muss eine Zeichenfolge sein. Wenn Feldwerte Leerzeichen oder Bindestriche enthalten, müssen unter **Import your data** (Daten importieren) erweiterte Optionen festgelegt werden, um die Prüfung auf diese Zeichen zu unterdrücken.
2. Überprüfen und überarbeiten Sie die restlichen Felder. Der Feldname und -typ werden meist für Sie ausgefüllt. Der Datentyp kann bis zur Indexerstellung noch geändert werden. Wenn Sie den Typ später ändern möchten, muss der Index neu erstellt werden.
3. Legen Sie Indexattribute für die einzelnen Felder fest:
   
   * „Abrufbar“ gibt das Feld in Suchergebnissen zurück.
   * „Filterbar“ ermöglicht, dass in Filterausdrücken auf das Feld verwiesen wird.
   * „Sortierbar“ ermöglicht, dass das Feld in einer Sortierung verwendet wird.
   * „Facettierbar“ aktiviert das Feld für die Facettennavigation.
   * „Durchsuchbar“ aktiviert die Volltextsuche.
4. Klicken Sie auf die Registerkarte **Analyse** , wenn Sie eine Sprachanalyse auf Feldebene angeben möchten. Derzeit können nur Sprachanalysen angegeben werden. Zur Verwendung benutzerdefinierter Analysen oder einer sprachfremden Analyse wie Schlüsselwort oder Muster muss Code geschrieben werden.
   
   * Klicken Sie auf **Durchsuchbar** , um die Volltextsuche für das Feld festzulegen und die Analyse-Dropdownliste zu aktivieren.
   * Wählen Sie die gewünschte Analyse aus. Details finden Sie unter [Erstellen eines Indexes für Dokumente in mehreren Sprachen](search-language-support.md).
5. Klicken Sie auf **Vorschläge** , um für ausgewählte Felder Textvervollständigungsfunktionen bei der Abfrage zu aktivieren.

## <a name="import-your-data"></a>Import your data
1. Geben Sie unter **Import your data**(Daten importieren) einen Namen für den Indexer an. Denken Sie daran, dass der Datenimport-Assistent einen Indexer generiert. Wenn Sie diesen später anzeigen oder bearbeiten möchten, können Sie ihn über das Portal auswählen, anstatt erneut den Assistenten auszuführen. 
2. Geben Sie den Zeitplan an. Dieser basiert auf der regionalen Zeitzone, in der der Dienst bereitgestellt wird.
3. Legen Sie erweiterte Optionen zum Angeben von Schwellenwerten an, die bestimmen, ob die Indizierung fortgesetzt werden kann, wenn ein Dokument verworfen wird. Außerdem können Sie angeben, ob Felder vom Typ **Schlüssel** Leerzeichen und Schrägstriche enthalten dürfen.  
4. Klicken Sie auf **OK**, um den Index zu erstellen und die Daten zu importieren.

Die Indizierung kann im Portal überwacht werden. Während Dokumente geladen werden, steigt die Dokumentanzahl für den definierten Index. Manchmal dauert es einige Minuten, bis die Portalseite die neuesten Aktualisierungen widerspiegelt.

Der Index kann abgefragt werden, sobald alle Dokumente geladen wurden.

## <a name="query-an-index-using-search-explorer"></a>Abfragen eines Index mit dem Suchexplorer

Das Portal enthält den **Suchexplorer**, mit dem Sie einen Index abfragen können, ohne Code schreiben zu müssen. Der [Suchexplorer](search-explorer.md) kann für jeden beliebigen Index verwendet werden.

Die Suchumgebung basiert auf Standardeinstellungen, z.B. der [einfachen Syntax](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) und dem standardmäßigen [searchMode-Abfrageparameter(https://docs.microsoft.com/rest/api/searchservice/search-documents). 

Ergebnisse werden in einem ausführlichen JSON-Format zurückgegeben, damit Sie das gesamte Dokument untersuchen können.

## <a name="edit-an-existing-indexer"></a>Bearbeiten eines vorhandenen Indexers
Wie bereits erwähnt erstellt der Datenimport-Assistent einen **Indexer**, den Sie als eigenständiges Konstrukt im Portal ändern können.

Doppelklicken Sie im Dashboard des Diensts auf die Kachel „Indexer“, um eine Liste mit allen Indexern einzublenden, die für Ihr Abonnement erstellt wurden. Doppelklicken Sie auf einen der Indexer, um ihn auszuführen, zu bearbeiten oder zu löschen. Sie können während der Indizierung den Index durch einen anderen vorhandenen Index ersetzen, die Datenquelle ändern und Optionen für Fehlerschwellenwerte festlegen.

## <a name="edit-an-existing-index"></a>Bearbeiten eines vorhandenen Index
Der Assistent erstellt auch einen **Index**. In Azure Search muss ein Index nach einer strukturellen Aktualisierung neu erstellt werden. Die Neuerstellung umfasst das Löschen des Index, das Neuerstellen des Index unter Verwendung eines überarbeiteten Schemas mit den gewünschten Änderungen sowie das erneute Laden der Daten. Zu strukturellen Updates zählen das Ändern des Datentyps und das Umbenennen oder Löschen eines Felds.

Änderungen, für die keine Neuerstellung erforderlich ist, beinhalten das Hinzufügen eines neuen Felds sowie das Ändern von Bewertungsprofilen, Vorschlägen und Sprachanalysen. Weitere Informationen finden Sie unter [Update Index](https://msdn.microsoft.com/library/azure/dn800964.aspx) (Aktualisieren des Index).


## <a name="next-steps"></a>Nächste Schritte
Unter den folgenden Links finden Sie weitere Informationen zu Indexern:

* [Verbinden von Azure SQL-Datenbank mit Azure Search mithilfe von Indexern](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Indizieren von Azure Cosmos DB](search-howto-index-documentdb.md)
* [Indizieren von Dokumenten in Azure Blob Storage mit Azure Search](search-howto-indexing-azure-blob-storage.md)
* [Indizieren von Azure Table Storage mit Azure Search](search-howto-indexing-azure-tables.md)

<!--Image references-->
[1]: ./media/search-import-data-portal/search-import-data-command.png


