---
title: "Erstellen eines Index (Portal – Azure Search) | Microsoft-Dokumentation"
description: "Erstellen Sie einen Azure Search-Index im Azure-Portal."
services: search
manager: jhubbard
author: heidisteen
documentationcenter: 
ms.assetid: 
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 06/20/2017
ms.author: heidist
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: a7d98ab0937a7d3f932d5df34c19ae091129804e
ms.contentlocale: de-de
ms.lasthandoff: 07/12/2017

---
# <a name="create-an-azure-search-index-using-the-azure-portal"></a>Erstellen eines Azure Search-Indexes im Azure-Portal
> [!div class="op_single_selector"]
> * [Übersicht](search-what-is-an-index.md)
> * [Portal](search-create-index-portal.md)
> * [.NET](search-create-index-dotnet.md)
> * [REST](search-create-index-rest-api.md)
> 
> 

Verwenden Sie den integrierten Index-Designer im Azure-Portal zum Aufbauen oder Erstellen eines [Suchindex](search-what-is-an-index.md) für Ihren Azure Search-Dienst. 

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird vorausgesetzt, dass Sie über ein [Azure-Abonnement](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) und über einen [Azure Search-Dienst](search-create-service-portal.md) verfügen.  

## <a name="find-your-search-service"></a>Suchen nach dem Suchdienst
1. Melden Sie sich beim Azure-Portal an, und überprüfen Sie die [Suchdienste für Ihr Abonnement](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices).
2. Wählen Sie Ihren Azure Search-Dienst aus.

## <a name="name-the-index"></a>Benennen des Index

1. Klicken Sie auf der Befehlsleiste oben auf der Seite auf die Schaltfläche **Index hinzufügen**.
2. Benennen Sie den Azure Search-Index. 
   * Beginnen Sie mit einem Buchstaben.
   * Verwenden Sie nur Kleinbuchstaben, Ziffern oder Bindestriche („-“).
   * Beschränken Sie den Namen und 60 Zeichen.

  Der Indexname wird Teil der Endpunkt-URL, die für Verbindungen mit dem Index und zum Senden von HTTP-Anforderungen in der Azure Search-REST-API verwendet wird.

## <a name="define-the-fields-of-your-index"></a>Definieren der Felder des Index

Zur Indexerstellung gehört eine *Feldersammlung*, die durchsuchbare Daten im Index definiert. Genauer gesagt, gibt sie die Struktur von Dokumenten an, die Sie separat hochladen. Die Feldersammlung umfasst erforderliche und optionale Felder (mit Name und Typ) mit Indexattributen, um zu bestimmen, wie das Feld verwendet werden kann.

1. Klicken Sie auf dem Blatt **Index hinzufügen** auf **Felder >**, um das Blatt für die Felddefinition zu öffnen. 

2. Akzeptieren Sie das generierte *Schlüsselfeld* mit dem Typ „Edm.String“. Standardmäßig heißt das Feld *id*. Allerdings können Sie es umbenennen, solange die Zeichenfolge den [Benennungsregeln](https://docs.microsoft.com/rest/api/searchservice/Naming-rules) entspricht. Für jeden Azure Search-Index ist ein Schlüsselfeld erforderlich, und es muss eine Zeichenfolge sein.

3. Fügen Sie Felder hinzu, um die Dokumente vollständig anzugeben, die Sie hochladen möchten. Wenn Dokumente aus *ID*, *Hotelname*, *Adresse*, *Stadt* und *Region* bestehen, erstellen Sie jeweils ein entsprechendes Feld im Index. Im [Entwurfsleitfaden im Abschnitt weiter unten](#design) finden Sie Hilfe zum Festlegen von Attributen.

4. Fügen Sie optional Felder hinzu, die intern in Filterausdrücken verwendet werden. Attribute können für das Feld festgelegt werden, um Felder aus Suchvorgängen auszuschließen.

5. Wenn Sie fertig sind, klicken Sie auf **OK**, um den Index zu speichern und zu erstellen.

## <a name="tips-for-adding-fields"></a>Tipps zum Hinzufügen von Feldern

Das Erstellen eines Index im Portal erfordert einen intensiven Einsatz der Tastatur. Minimieren Sie mit diesem Workflow die Schritte:

1. Erstellen Sie zuerst die Feldliste, indem Sie Namen eingeben und Datentypen festlegen.

2. Verwenden Sie dann die Kontrollkästchen oben bei den einzelnen Attributen, um die Einstellung gleichzeitig für alle Felder zu aktivieren. Deaktivieren Sie anschließend die Kontrollkästchen selektiv für einige Felder, für die sie nicht erforderlich sind. Zeichenfolgenfelder sind beispielsweise in der Regel durchsuchbar. Daher könnten Sie auf **Abrufbar** und **Durchsuchbar** klicken, um die Werte des Felds in den Suchergebnissen zurückzugeben und eine Volltextsuche für das Feld zu ermöglichen. 

<a name="design"></a>
## <a name="design-guidance-for-setting-attributes"></a>Entwurfsleitfaden für das Festlegen von Attributen

Obwohl Sie jederzeit neue Felder hinzufügen können, sind vorhandene Felddefinitionen für die Lebensdauer des Index gesperrt. Aus diesem Grund verwenden Entwickler in der Regel das Portal zum Erstellen einfacher Indizes und zum Testen von Ideen. Oder sie verwenden Seiten des Portals, um eine Einstellung zu suchen. Häufige Iterationen über einen Indexentwurf sind effizienter, wenn Sie einem codebasierten Ansatz folgen, sodass Sie den Index einfach neu erstellen können.

Analysen und Vorschläge werden Feldern zugeordnet, bevor der Index gespeichert wird. Sie sollten sich durch jede Registerkartenseite klicken, um der Indexdefinition Analysen und Vorschläge für Sprachen hinzuzufügen.

Zeichenfolgenfelder werden häufig als **Durchsuchbar** und **Abrufbar** gekennzeichnet.

Zu den Feldern zum Eingrenzen der Suchergebnisse gehören **Sortierbar**, **Filterbar** und **Facettenreich**.

Feldattribute bestimmen, wie ein Feld verwendet wird, z.B. ob es in der Volltextsuche, in der Facettennavigation, für Sortiervorgänge usw. verwendet wird. In der folgenden Tabelle werden die einzelnen Attribute beschrieben.

|Attribut|Beschreibung|  
|---------------|-----------------|  
|**Durchsuchbar**|Volltextsuche ist möglich, unterliegt einer lexikalischen Analyse, z.B. Worttrennung, während der Indizierung. Wenn Sie ein durchsuchbares Feld auf einen Wert wie „sunny day“ festlegen, wird es intern in die einzelnen Token „sunny“ und „day“ unterteilt. Weitere Informationen finden Sie unter [Funktionsweise der Volltextsuche](search-lucene-query-architecture.md).|  
|**Filterbar**|Wird in **$filter**-Abfragen angegeben. Filterbare Felder vom Typ `Edm.String` oder `Collection(Edm.String)` werden keiner Worttrennung unterzogen, sodass nur nach exakten Übereinstimmungen gesucht wird. Beispiel: Wenn Sie ein solches Feld „f“ auf „sunny day“ festlegen, werden mit `$filter=f eq 'sunny'` keine Übereinstimmungen gefunden, während Sie mit `$filter=f eq 'sunny day'` Suchergebnisse erhalten. |  
|**Sortierbar**|Standardmäßig sortiert das System Ergebnisse nach Bewertung, aber Sie können die Sortierung auf Grundlage von Feldern in den Dokumenten konfigurieren. Felder vom Typ `Collection(Edm.String)` können nicht **sortierbar** sein. |  
|**Facettenreich**|Wird in der Regel in einer Darstellung der Suchergebnisse verwendet, die eine Trefferanzahl nach Kategorie umfasst (z.B. Hotels in einer bestimmten Stadt). Diese Option kann nicht für Felder vom Typ `Edm.GeographyPoint` verwendet werden. Felder des Typs `Edm.String`, die **filterbar**, **sortierbar** oder **facettenreich** sind, können höchstens 32 KB lang sein. Weitere Details finden Sie unter [Erstellen des Index (REST API)](https://docs.microsoft.com/rest/api/searchservice/create-index).|  
|**key**|Eindeutiger Bezeichner für Dokumente im Index. Es muss genau ein Feld als Schlüsselfeld ausgewählt werden, und es muss vom Typ `Edm.String` sein.|  
|**Abrufbar**|Legt fest, ob das Feld in einem Suchergebnis zurückgegeben werden kann. Dies ist hilfreich, wenn Sie ein Feld (z.B. *Gewinnspanne*) zum Filtern, Sortieren oder Bewerten verwenden möchten, das Feld jedoch für den Endbenutzer nicht sichtbar sein soll. Dieses Attribut muss für `key`-Felder auf `true` gesetzt sein.|  

## <a name="create-the-hotels-index-used-in-example-api-sections"></a>Erstellen des Hotelindex, der in den Abschnitten mit Beispiel-APIs verwendet wird

Die Azure Search-API-Dokumentation enthält Codebeispiele mit einem einfachen *hotels*-Index. In den nachfolgenden Screenshots sehen Sie die Indexdefinition, einschließlich der Analysen für die französische Sprache, die während der Indexdefinition angegeben wurde. Sie können sie als Übung im Portal neu erstellen.

![](./media/search-create-index-portal/field-definitions.png)

![](./media/search-create-index-portal/set-analyzer.png)

## <a name="next-steps"></a>Nächste Schritte

Nach dem Erstellen eines Azure Search-Index können Sie mit dem nächsten Schritt fortfahren: [Hochladen durchsuchbarer Daten in den Index](search-what-is-data-import.md).

Alternativ können Sie sich auch genauer mit Indizes beschäftigen. Zusätzlich zur Feldersammlung gibt ein Index auch Analysen, Vorschläge, Bewertungsprofile und CORS-Einstellungen an. Das Portal bietet Registerkartenseiten zum Definieren der gängigsten Elemente: Felder, Analysen und Vorschläge. Zum Erstellen oder Ändern anderer Elemente können Sie die REST-API oder das .NET-SDK verwenden.

## <a name="see-also"></a>Weitere Informationen

 [Funktionsweise der Volltextsuche](search-lucene-query-architecture.md)  
 [Suchdienst-REST API](https://docs.microsoft.com/rest/api/searchservice/) [.NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)


