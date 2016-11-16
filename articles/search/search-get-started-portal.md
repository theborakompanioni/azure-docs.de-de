---
title: Erste Schritte mit Azure Search | Microsoft Docs
description: "Hier erfahren Sie, wie Sie anhand dieses Tutorials und auf der Grundlage von DocumentDB-Beispieldaten Ihren ersten Azure Search-Index erstellen. Bei dieser portalbasierten Übung ohne Code wird der Assistent „Daten importieren“ verwendet."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 21adc351-69bb-4a39-bc59-598c60c8f958
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.date: 10/03/2016
ms.author: heidist
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: dddbcbcd82900d7537c2d60631cc1753554d9486


---
# <a name="get-started-with-azure-search-in-the-portal"></a>Erste Schritte mit Azure Search im Portal
In diesem Artikel erhalten Sie eine Einführung in Microsoft Azure Search ohne Code, indem die direkt in das Portal integrierten Funktionen beschrieben werden. 

Im Tutorial wird die Verwendung einer [Azure DocumentDB-Beispieldatenbank](#apdx-sampledata) vorausgesetzt, die Sie mithilfe unserer Daten und der Anleitung leicht erstellen können. Sie können diese Schritte aber auch für Ihre vorhandenen Daten in DocumentDB oder einer SQL-Datenbank anpassen.

> [!NOTE]
> Für dieses Tutorial zu den ersten Schritten sind ein [Azure-Abonnement](/pricing/free-trial/?WT.mc_id=A261C142F) und ein [Azure Search-Dienst](search-create-service-portal.md) erforderlich. 
> 
> 

## <a name="find-your-service"></a>Suchen nach dem Dienst
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Öffnen Sie das Service-Dashboard des Azure-Suchdiensts. Im Anschluss finden Sie einige Möglichkeiten, um zum Dashboard zu gelangen.
   
   * Klicken Sie auf der Navigationsleiste auf **Suchdienste**. Auf der Navigationsleiste werden alle Dienste angegeben, die unter Ihrem Abonnement bereitgestellt werden. Wenn ein Suchdienst definiert wurde, wird in der Liste der Eintrag **Suchdienste** angezeigt.
   * Klicken Sie auf der Navigationsleiste auf **Durchsuchen** , und geben Sie im Suchfeld „Suche“ ein, um eine Liste mit allen Suchdiensten zu generieren, die für Ihre Abonnements erstellt wurden.

## <a name="check-for-space"></a>Überprüfen des Speicherplatzes
Viele Kunden beginnen mit dem kostenlosen Dienst (Free). Diese Version ist auf drei Indizes, drei Datenquellen und drei Indexer beschränkt. Stellen Sie sicher, dass Sie über ausreichend Platz für zusätzliche Elemente verfügen, bevor Sie beginnen. In dieser exemplarischen Vorgehensweise wird ein Objekt für jede Objektgruppe erstellt.

## <a name="create-an-index-and-load-data"></a>Erstellen eines Index und Laden von Daten
Suchabfragen durchlaufen einen *Index* mit durchsuchbaren Daten, Metadaten und Konstrukten, die zur Optimierung bestimmter Suchverhaltensweisen verwendet werden. Im ersten Schritt definieren Sie einen Index und füllen ihn auf.

Es gibt mehrere Methoden zum Erstellen eines Index. Wenn Sie über Daten in einem Datenspeicher verfügen, der von Azure Search per Crawler durchlaufen werden kann – beispielsweise Azure SQL-Datenbank, SQL Server auf einem virtuellen Azure-Computer oder DocumentDB –, können Sie einen Index einfach erstellen und auffüllen, indem Sie einen *Indexer*verwenden.

Um diese Aufgabe über das Portal durchführen zu können, verwenden wir Daten aus DocumentDB, die über den Assistenten **Daten importieren** mit einem Indexer durchlaufen werden können. 

Erstellen Sie vor dem Fortfahren eine [DocumentDB-Beispieldatenbank](#apdx-sampledata) für dieses Tutorial. Kehren Sie anschließend zu diesem Abschnitt zurück, um die unten angegebenen Schritte auszuführen.

<a id="defineDS"></a>

#### <a name="step-1-define-the-data-source"></a>Schritt 1: Definieren der Datenquelle
1. Klicken Sie auf dem Dashboard des Azure Search-Diensts auf der Befehlsleiste auf **Daten importieren** , um einen Assistenten zu starten, mit dem ein Index erstellt und aufgefüllt wird.
   
    ![][7]
2. Klicken Sie im Assistenten auf **Datenquelle** > **DocumentDB** > **Name**, und geben Sie einen Namen für die Datenquelle ein. Eine Datenquelle ist ein Verbindungsobjekt in Azure Search, das mit anderen Indexern verwendet werden kann. Nachdem die Datenquelle erstellt wurde, wird sie in Ihrem Dienst als „vorhandene Datenquelle“ verfügbar gemacht.
3. Wählen Sie Ihr vorhandenes DocumentDB-Konto sowie die Datenbank und die Sammlung aus. Bei Verwendung der von uns bereitgestellten Beispieldaten sieht Ihre Datenquellendefinition wie folgt aus:
   
    ![][2]

Beachten Sie, dass wir diese Abfrage überspringen. Der Grund ist, dass wir die Änderungsnachverfolgung hier nicht in unserem Dataset implementieren. Wenn Ihr Dataset ein Feld enthält, mit dem der Zeitpunkt der Datensatzaktualisierung nachverfolgt wird, können Sie einen Azure Search-Indexer konfigurieren, um die Änderungsnachverfolgung für selektive Updates Ihres Index zu nutzen.

Klicken Sie auf **OK** , um diesen Schritt des Assistenten abzuschließen.

#### <a name="step-2-define-the-index"></a>Schritt 2: Definieren des Index
Klicken Sie im Assistenten auf **Index** , und sehen Sie sich die Entwurfsoberfläche zum Erstellen eines Azure Search-Index an. Für einen Index sind mindestens ein Name und eine Felderauflistung erforderlich, wobei ein Feld als Dokumentschlüssel gekennzeichnet ist. Da wir ein DocumentDB-Dataset verwenden, werden die Felder vom Assistenten automatisch erkannt, und Felder und Datentypzuweisungen werden vorab in den Index geladen. 

  ![][3]

Die Felder und Datentypen sind zwar konfiguriert, aber Sie müssen trotzdem Attribute zuweisen. Die Kontrollkästchen oben in der Liste der Felder sind *Indexattribute* , mit denen die Verwendung des Felds gesteuert wird. 

* **Abrufbar** bedeutet, dass es in der Liste mit den Suchergebnissen angezeigt wird. Sie können einzelne Felder kennzeichnen, indem Sie dieses Kontrollkästchen deaktivieren, um sie aus den Suchergebnissen auszuschließen, z. B. wenn Felder nur in Filterausdrücken verwendet werden. 
* Mit **Filterbar**, **Sortierbar** und **Facettenreich** wird angegeben, ob ein Feld in einer Filter-, Sortier- oder Facettennavigationsstruktur verwendet werden kann. 
* **Durchsuchbar** bedeutet, dass ein Feld in die Volltextsuche einbezogen wird. Zeichenfolgen können normalerweise durchsucht werden. Numerische Felder und boolesche Felder werden häufig als „Nicht durchsuchbar“ markiert. 

Markieren Sie für die Felder im Index vor dem Verlassen der Seite die Verwendung der folgenden Optionen (Abrufbar, Durchsuchbar usw.). Die meisten Felder sind abrufbar. Die meisten Zeichenfolgenfelder sind durchsuchbar (Sie müssen den Schlüssel nicht durchsuchbar machen). Einige Felder, z. B. „genre“, „orderableOnline“, „rating“ und „tags“, sind auch filterbar, sortierbar und facettenreich. 

| Feld | Typ | Options |
| --- | --- | --- |
| id |Edm.String | |
| albumTitle |Edm.String |Abrufbar, durchsuchbar |
| albumUrl |Edm.String |Abrufbar, durchsuchbar |
| genre |Edm.String |Abrufbar, durchsuchbar, filterbar, sortierbar, facettenreich |
| genreDescription |Edm.String |Abrufbar, durchsuchbar |
| artistName |Edm.String |Abrufbar, durchsuchbar |
| orderableOnline |Edm.Boolean |Abrufbar, filterbar, sortierbar, facettenreich |
| tags |Collection(Edm.String) |Abrufbar, filterbar, facettenreich |
| price |Edm.Double |Abrufbar, filterbar, facettenreich |
| margin |Edm.Int32 | |
| rating |Edm.Int32 |Abrufbar, filterbar, sortierbar, facettenreich |
| inventory |Edm.Int32 |Abrufbar |
| lastUpdated |Edm.DateTimeOffset | |

Zu Vergleichszwecken ist der folgende Screenshot eine Darstellung eines Index, der anhand der Spezifikation in der vorherigen Tabelle erstellt wurde.

 ![][4]

Klicken Sie auf **OK** , um diesen Schritt des Assistenten abzuschließen.

#### <a name="step-3-define-the-indexer"></a>Schritt 3: Definieren des Indexers
Klicken Sie im Assistenten **Daten importieren** auf **Indexer** > **Name**, geben Sie einen Namen für den Indexer ein, und verwenden Sie für alle anderen Werte die Standardwerte. Mit diesem Objekt wird ein ausführbarer Prozess definiert. Nach der Erstellung können Sie einen wiederkehrenden Zeitplan einrichten. Verwenden Sie hier aber die Standardoption, um den Indexer einmal sofort auszuführen, wenn Sie auf **OK** klicken. 

Ihre Dateneinträge für den Import sollten eingefügt und bereit sein.

  ![][5]

Klicken Sie zum Ausführen des Assistenten auf **OK** , um den Import zu starten und den Assistenten zu schließen.

## <a name="check-progress"></a>Überprüfen des Status
Wechseln Sie zum Überprüfen des Status zurück zum Dashboard des Diensts, scrollen Sie nach unten, und doppelklicken Sie auf die Kachel **Indexer**, um die Liste mit den Indexern zu öffnen. Der gerade erstellte Indexer sollte nun in der Liste angezeigt werden. Der Status sollte „In Bearbeitung“ oder „Erfolg“ lauten, und die Anzahl der für Azure Search indizierten Dokumente sollte angegeben werden.

  ![][6]

## <a name="query-the-index"></a>Indexabfragen
Sie verfügen jetzt über einen Suchindex, der bereit für Abfragen ist. 

**Suchexplorer** ist ein in das Portal integriertes Abfragetool. Es verfügt über ein Suchfeld, mit dem Sie überprüfen können, ob für eine Sucheingabe die erwarteten Daten zurückgegeben werden. 

1. Klicken Sie auf der Befehlsleiste auf **Suchexplorer** .
2. Achten Sie darauf, welcher Index aktiv ist. Falls dies nicht der gerade erstellte Index ist, können Sie auf der Befehlsleiste auf **Index ändern** klicken, um den gewünschten Index auszuwählen.
3. Lassen Sie das Suchfeld leer, und klicken Sie dann auf die Schaltfläche **Suche** , um eine Platzhaltersuche auszuführen, bei der alle Dokumente zurückgegeben werden.
4. Geben Sie einige Volltextsuche-Abfragen ein. Sie können die Ergebnisse Ihrer Platzhaltersuche überprüfen, um sich mit Interpreten, Alben und Genres für Abfragen vertraut zu machen.
5. Ideen für eine andere Abfragesyntax finden Sie in den [Beispielen am Ende dieses Artikels](https://msdn.microsoft.com/library/azure/dn798927.aspx). Ändern Sie dann Ihre Abfrage, um Suchzeichenfolgen zu verwenden, die in Ihrem Index mit hoher Wahrscheinlichkeit gefunden werden.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie den Assistenten einmal ausgeführt haben, können Sie einen Schritt zurück machen und einzelne Komponenten anzeigen oder ändern: Index, Indexer oder Datenquelle. Einige Bearbeitungen, z. B. das Ändern des Felddatentyps, sind für den Index nicht zulässig, aber die meisten Eigenschaften und Einstellungen können geändert werden. Klicken Sie zum Anzeigen der einzelnen Komponenten auf dem Dashboard auf die Kachel **Index**, **Indexer** oder **Datenquellen**, um eine Liste mit vorhandenen Objekten anzuzeigen.

Weitere Informationen zu anderen Features, die in diesem Artikel erwähnt werden, finden Sie unter diesen Links:

* [Indexer](search-indexer-overview.md)
* [Create Index (enthält eine ausführliche Erklärung der Indexattribute)](https://msdn.microsoft.com/library/azure/dn798941.aspx)
* [Suchexplorer](search-explorer.md)
* [Search Documents (enthält Beispiele für Abfragesyntax)](https://msdn.microsoft.com/library/azure/dn798927.aspx)

Sie können denselben Workflow auch ausprobieren, indem Sie den Assistenten „Daten importieren“ für andere Datenquellen wie Azure SQL-Datenbank oder SQL Server auf virtuellen Azure-Maschinen verwenden.

> [!NOTE]
> Neu angekündigt ist die Indexerunterstützung für das Durchsuchen von Azure Blob Storage per Crawler. Dieses Feature befindet sich aber noch in der Vorschauphase und ist noch keine Option im Portal. Um diesen Indexer auszuprobieren, müssen Sie Code schreiben. Weitere Informationen finden Sie unter [Indizieren von Dokumenten in Azure Blob Storage mit Azure Search](search-howto-indexing-azure-blob-storage.md).
> <a id="apdx-sampledata"></a>
> 
> 

## <a name="appendix-create-sample-data-in-documentdb"></a>Anhang: Erstellen von Beispieldaten in DocumentDB
In diesem Abschnitt wird eine kleine Datenbank in DocumentDB erstellt, die zum Durchführen der Aufgaben in diesem Tutorial verwendet werden kann.

Im Folgenden erhalten Sie eine allgemeine Anleitung, die aber nicht alle Aspekte vollständig abdeckt. Sollten Sie bei der Navigation oder bei den Aufgaben im DocumentDB-Portal weitere Hilfe benötigen, verwenden Sie die DocumentDB-Dokumentation. Die meisten benötigten Befehle stehen aber entweder oben im Dashboard auf der Befehlsleiste des Diensts oder auf dem Blatt der Datenbank zur Verfügung. 

  ![][1]

### <a name="create-musicstoredb-for-this-tutorial"></a>Erstellen von musicstoredb für dieses Tutorial
1. [hier](https://github.com/HeidiSteen/azure-search-get-started-sample-data) , um eine ZIP-Datei mit den JSON-Datendateien für den Music Store herunterzuladen. Wir stellen 246 JSON-Dokumente für dieses Dataset bereit.
2. Fügen Sie DocumentDB Ihrem Abonnement hinzu, und öffnen Sie anschließend das Dashboard des Diensts.
3. Klicken Sie auf **Datenbank hinzufügen**, um eine neue Datenbank mit der ID `musicstoredb` zu erstellen. Sie wird nach der Erstellung in der Datenbankkachel weiter unten auf der Seite angezeigt.
4. Klicken Sie auf den Namen der Datenbank, um das Blatt für die Datenbank zu öffnen.
5. Klicken Sie auf **Sammlung hinzufügen**, um eine Sammlung mit der ID `musicstorecoll` zu erstellen.
6. Klicken Sie auf **Dokument-Explorer**.
7. Klicken Sie auf **Hochladen**.
8. Navigieren Sie unter **Dokument hochladen**zu dem lokalen Ordner, der die zuvor heruntergeladenen JSON-Dateien enthält. Wählen Sie JSON-Dateien in Gruppen von maximal 100 Dateien aus.
   * 386.json
   * 387.json
   *  zu erstellen und zu verwalten.  erforderlich.  zu erstellen und zu verwalten.
   * 486. json
9. Wiederholen Sie diesen Schritt, um die nächste Gruppe von Dateien hochzuladen, bis Sie die letzte Datei (669.json) hochgeladen haben.
10. Klicken Sie auf **Abfrage-Explorer** , um zu überprüfen, ob die Daten hochgeladen wurden und die Uploadanforderungen von Dokument-Explorer erfüllt wurden.

Eine einfache Möglichkeit ist hierbei die Verwendung der Standardabfrage. Sie können die Standardabfrage aber auch so ändern, dass die ersten 300 Elemente ausgewählt werden (in diesem Dataset sind weniger als 300 Elemente vorhanden).

Sie sollten eine JSON-Ausgabe zurückerhalten, die mit der Dokumentnummer 386 beginnt und mit Dokument 669 endet. Nach dem Laden der Daten können Sie [zu den Schritten in dieser exemplarischen Vorgehensweise zurückkehren](#defineDS), um mit dem **Datenimport-Assistenten** einen Index zu erstellen.

<!--Image references-->
[1]: ./media/search-get-started-portal/AzureSearch-GetStart-Docdbmenu1.png
[2]: ./media/search-get-started-portal/AzureSearch-GetStart-DataSource.png
[3]: ./media/search-get-started-portal/AzureSearch-GetStart-DefaultIndex.png
[4]: ./media/search-get-started-portal/AzureSearch-GetStart-FinishedIndex.png
[5]: ./media/search-get-started-portal/AzureSearch-GetStart-ImportReady.png
[6]: ./media/search-get-started-portal/AzureSearch-GetStart-IndexerList.png
[7]: ./media/search-get-started-portal/search-data-import-wiz-btn.png



<!--HONumber=Nov16_HO2-->


