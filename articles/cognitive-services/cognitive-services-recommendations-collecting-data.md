<properties
	pageTitle="Erfassen von Daten zum Trainieren Ihres Modells: Machine Learning-Empfehlungs-API | Microsoft Azure"
	description="Azure Machine Learning-Empfehlungen – Erfassen von Daten zum Trainieren Ihres Modells"
	services="cognitive-services"
	documentationCenter=""
	authors="luiscabrer"
	manager="paulettm"
	editor="cgronlun"/>

<tags
	ms.service="cognitive-services"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/06/2016"
	ms.author="luisca"/>

#  Erfassen von Daten zum Trainieren Ihres Modells #

Die Empfehlungs-API lernt aus vergangenen Transaktionen, um zu ermitteln, welche Artikel für einen bestimmten Benutzer empfohlen werden sollen.

Damit Sie das Modell trainieren können, müssen Sie nach der Modellerstellung Folgendes angeben: eine Katalogdatei und Nutzungsdaten.

>   Wenn Sie dies noch nicht getan haben, empfehlen wir Ihnen, die [Kurzanleitung](cognitive-services-recommendations-quick-start.md) durchzugehen.


## Katalogdaten ##

### Format der Katalogdatei ###

Die Katalogdatei enthält Informationen zu den Artikeln, die Sie Ihren Kunden anbieten. Die Katalogdaten müssen das folgende Format aufweisen:

- Ohne Features: `<Item Id>,<Item Name>,<Item Category>[,<Description>]`

- Mit Features: `<Item Id>,<Item Name>,<Item Category>,[<Description>],<Features list>`

#### Beispielzeilen in einer Katalogdatei

Ohne Features:

    AAA04294,Office Language Pack Online DwnLd,Office
    AAA04303,Minecraft Download Game,Games
    C9F00168,Kiruna Flip Cover,Accessories

Mit Features:

    AAA04294,Office Language Pack Online DwnLd,Office, softwaretype=productivity, compatibility=Windows
    BAB04303,Minecraft DwnLd,Games, softwaretype=gaming, compatibility=iOS, agegroup=all
    C9F00168,Kiruna Flip Cover,Accessories, compatibility=lumia, hardwaretype=mobile

#### Formatdetails

| Name | Erforderlich | Typ | Beschreibung |
|:---|:---|:---|:---|
| Element-ID |Ja | [A-z], [a-z], [0-9], [\_] &#40;Unterstrich&#41;, [-] &#40;Bindestrich&#41;<br> Max. Länge: 50 | Eindeutiger Bezeichner eines Artikels. | | Artikelname | Ja | Alle alphanumerischen Zeichen<br> Max. Länge: 255 | Artikelname. | | Artikelkategorie | Ja | Alle alphanumerischen Zeichen <br> Max. Länge: 255 | Kategorie, zu der dieser Artikel gehört (z.B. Kochbücher, Drama…). Darf leer sein. | | Beschreibung | Nein, es sei denn, es sind Features vorhanden (darf jedoch leer sein) | Alle alphanumerischen Zeichen <br> Max. Länge: 4.000 | Dies ist die Beschreibung des Artikels. | | Featureliste | Nein | Alle alphanumerischen Zeichen <br> Max. Länge: 4.000; max. Anzahl von Features: 20 | Dies ist eine kommagetrennte Liste im Format „Featurename=Featurewert“ zum Verbessern der Modellempfehlung.|

#### Hochladen einer Katalogdatei

Informationen zum Hochladen einer Katalogdatei finden Sie in der [API-Referenz](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f316efeda5650db055a3e1).

Beachten Sie, dass der Inhalt der Katalogdatei als Anforderungstext übergeben werden sollte.

Wenn Sie durch mehrere Aufrufe mehrere Katalogdateien zum gleichen Modell hochladen, werden nur die neuen Katalogelemente eingefügt. Vorhandene Elemente bleiben innerhalb der ursprünglichen Werte. Mit dieser Methode können Sie keine Katalogdaten aktualisieren.

>   Hinweis: Die maximale Dateigröße beträgt 200 MB. Der Katalog kann maximal 100.000 Artikel enthalten.


## Gründe für das Hinzufügen von Features zum Katalog

Das Empfehlungsmodul erstellt ein Statistikmodell, das Aufschluss darüber gibt, welche Artikel einem Kunden voraussichtlich gefallen oder wahrscheinlich von diesem gekauft werden. Bei einem neuen Produkt, für das noch keine Interaktion stattgefunden hat, kann kein Modell erstellt werden, das ausschließlich auf Kookkurrenzen basiert. Angenommen, Sie bieten eine neue Violine für Kinder in Ihrem Shop an. Da Sie diese Violine zuvor noch nie verkauft haben, wissen Sie nicht, welche anderen Artikel mit der Violine angeboten werden sollten.

Falls das Modul Informationen zu dieser Violine besitzt (etwa, dass es sich um ein Musikinstrument handelt, sie für Kinder im Alter von sieben bis 10 Jahren gedacht ist, es keine teure Violine ist usw.), kann das Modul von anderen Produkten mit ähnlichen Features lernen. Beispiel: Sie haben in der Vergangenheit bereits Violinen verkauft, und Kunden, die eine Violine erwerben, kaufen häufig auch CDs mit klassischer Musik und Notenständer. Das System kann diese Verbindungen zwischen Features ermitteln und Empfehlungen basierend auf den Features aussprechen, während für die neue Violine noch wenig Nutzungsdaten vorliegen.

Features werden als Teil der Katalogdaten importiert, und wenn ein Rangfolgebuild abgeschlossen ist, wird ihnen ein Rang (die Bedeutung des Features im Modell) zugeordnet. Der Rang der Features kann sich je nach Nutzungsdatenmuster und Elementtyp ändern. Bei konsistenter Nutzung und konsistenten Elementen dürfte der Rang jedoch nur geringfügigen Schwankungen unterliegen. Der Rang eines Features ist eine nicht-negative Zahl. Die Zahl 0 bedeutet, dass das Feature noch nicht eingestuft wurde (dies ist der Fall, wenn diese API vor dem Abschluss des ersten Rangfolgebuilds aufgerufen wurde). Das Datum, an dem der Rang zugeordnet wurde, wird als Aktualität der Bewertung bezeichnet.

Wenn Sie Features als Teil Ihres Builds verwenden möchten, müssen Sie folgende Schritte ausführen:

1. Sicherstellen, dass Ihr Katalog beim Hochladen Features enthält

2. Auslösen eines Rangfolgebuilds. Dadurch wird eine Analyse der Bedeutung/des Rangs der einzelnen Features ausgeführt.

3. Auslösen eines Empfehlungsbuilds. Legen Sie dabei die folgenden Buildparameter fest: Legen Sie „useFeaturesInModel“ und „allowColdItemPlacement“ auf TRUE und für „modelingFeatureList“ die kommagetrennte Liste mit Features fest, die Sie zum Optimieren Ihres Modells verwenden möchten. Weitere Informationen finden Sie unter [Recommendations build type parameters](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3d0) (Parameter für Empfehlungsbuilds).


## Nutzungsdaten ##
Eine Nutzungsdatei enthält Informationen zur Verwendungsweise dieser Artikel oder zu den Transaktionen Ihres Geschäfts.

#### Nutzungsformatdetails
Eine Nutzungsdatei ist eine kommagetrennte Datei (CSV-Datei). Jede Zeile in einer Nutzungsdatei stellt eine Interaktion zwischen einem Benutzer und einem Artikel dar. Für jede Zeile wird das folgende Format verwendet:<br> `<User Id>,<Item Id>,<Time>,[<Event>]`



| Name | Erforderlich | Typ | Beschreibung
|-------|------------|------|---------------
|Benutzer-ID| Ja|[A-z], [a-z], [0-9], [\_] &#40;Unterstrich&#41;, [-] &#40;Bindestrich&#41;<br> Max. Länge: 255 |Eindeutiger Bezeichner eines Benutzers. |Element-ID|Ja|[A-z], [a-z], [0-9], [&#95;] &#40;Unterstrich&#41;, [-] &#40;Bindestrich&#41;<br> Max. Länge: 50|Eindeutiger Bezeichner eines Artikels. |Zeit|Ja|Datum im Format: JJJJ/MM/TTTHH:MM:SS (z.B. 2013/06/20T10:00:00)|Zeitpunkt der Daten. |Ereignis|Nein | Eines der folgenden:<br>• Click<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Purchase| Der Transaktionstyp. |

#### Beispielzeilen in einer Nutzungsdatei

    00037FFEA61FCA16,288186200,2015/08/04T11:02:52,Purchase
    0003BFFDD4C2148C,297833400,2015/08/04T11:02:50,Purchase
    0003BFFDD4C2118D,297833300,2015/08/04T11:02:40,Purchase
    00030000D16C4237,297833300,2015/08/04T11:02:37,Purchase
    0003BFFDD4C20B63,297833400,2015/08/04T11:02:12,Purchase
    00037FFEC8567FB8,297833400,2015/08/04T11:02:04,Purchase

#### Hochladen einer Nutzungsdatei

Informationen zum Hochladen von Nutzungsdateien finden Sie in der [API-Referenz](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f316efeda5650db055a3e2). Beachten Sie, dass Sie den Inhalt der Nutzungsdatei als Textkörper des HTTP-Aufrufs übergeben müssen.

>  Hinweis:

>  Maximale Dateigröße: 200 MB. Sie können mehrere Nutzungsdateien hochladen.

>  Sie müssen eine Katalogdatei hochladen, bevor Sie dem Modell Nutzungsdaten hinzufügen. Während der Trainingsphase werden nur Artikel in der Katalogdatei verwendet. Alle anderen Artikel werden ignoriert.

## Wie viele Daten benötigen Sie?

Die Qualität Ihres Modells hängt stark von der Qualität und Menge der Daten ab. Das System lernt, wenn Benutzer unterschiedliche Artikel kaufen (dies wird als Kookkurrenzen bezeichnet). FBT-Builds beispielsweise müssen auch wissen, welche Artikel in der gleichen Transaktion gekauft werden.

Nach einer guten Faustregel sollten die meisten Artikel in mindestens 20 Transaktionen vorkommen – wenn Ihr Katalog also 10.000 Artikel enthält, sollten Sie mindestens über das 20-fache an Transaktionen verfügen, also etwa 200.000. Wie gesagt, dies ist eine Faustregel. Sie müssen mit Ihren Daten experimentieren.

Nach der Erstellung eines Modells können Sie eine [Offlineauswertung](cognitive-services-recommendations-buildtypes.md) ausführen, um die wahrscheinliche Leistung Ihres Modells zu überprüfen.

<!---HONumber=AcomDC_0608_2016-->