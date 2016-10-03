<properties
	pageTitle="Verwenden von Beispiel-DataSets in Machine Learning Studio | Microsoft Azure"
	description="Beschreibungen von in Beispielmodellen verwendete DataSets in ML Studio. Sie können diese Beispiel-DataSets für Experimente verwenden."
	services="machine-learning"
	documentationCenter=""
	authors="garyericson"
	manager="jhubbard"
	editor="cgronlun"/>

<tags
	ms.service="machine-learning"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/16/2016"
	ms.author="garye"/>


# Verwenden von Beispiel-DataSets in Azure Machine Learning Studio

[top]: #machine-learning-sample-datasets

Beim Erstellen eines neuen Arbeitsbereichs in Azure Machine Learning ist eine Reihe von Beispielexperimenten und -DataSets standardmäßig enthalten. Viele dieser Beispiel-Datasets sind für die Beispielmodelle im [Katalog für Azure Cortana Intelligence](http://gallery.cortanaintelligence.com/) vorgesehen, während andere als Beispiele für verschiedene Arten von Daten für Machine Learning enthalten sind.

Einige dieser DataSets sind im Azure-Blob-Speicher verfügbar. Für diese DataSets finden Sie in der folgende Tabelle einen direkten Link. Diese Datasets können Sie mithilfe des [Import Data][import-data]-Moduls in Ihren Experimenten verwenden.

Der Rest dieser Beispiel-DataSets wird in der Modulpalette auf der linken Seite des Experimentcanvas unter **Saved Datasets** angezeigt, wenn Sie in ML Studio ein neues Experiment öffnen oder erstellen. Sie können alle diese DataSets für Ihre eigenen Experimente verwenden, indem Sie sie auf Ihren Experimentcanvas ziehen.


<!--
For a list of sample experiments available in ML Studio, see [Machine Learning Sample Experiments][sample-experiments].

[sample-experiments]: machine-learning-sample-experiments.md
-->

[AZURE.INCLUDE [machine-learning-kostenlose-Testversion](../../includes/machine-learning-free-trial.md)]


<table>

<tr>
  <th align=left>Datasetname</th>
  <th align=left>Datasetbeschreibung</th>
</tr>

<tr>
  <td valign=top>Dataset "Adult Census Income Binary Classification"</td>
  <td valign=top>
Eine Teilmenge der Volkszählungsdatenbank von 1994, die arbeitende Erwachsene (älter als 16 Jahre) mit einem bereinigten Einkommensindex verwendet, der über 100 liegt.<p> </p><b>Verwendung</b>: Klassifizierung von Personen mithilfe von demografischen Daten für die Vorhersage, ob eine Person mehr als 50.000 pro Jahr verdient.<p> </p><b>Zugehörige Untersuchungen</b>: Kohavi, R., Becker, B., (1996). UCI Machine Learning Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, School of Information and Computer Science
  </td>
</tr>

<tr ID=airport-codes-dataset>
  <td valign=top>Flughafencodes-Dataset</td>
  <td valign=top>
Flughafencodes (USA).<p> </p>Dieses Dataset enthält eine Zeile für jeden Flughafen in den USA, in der die ID-Nummer des Flughafens zusammen mit dem Ort und Bundesstaat enthalten ist.
  </td>
</tr>

<tr>
  <td valign=top>Automobile price data (Raw)</td>
  <td valign=top>
Informationen zu Automobilen nach Marke und Modell, einschließlich Preis und Merkmalen wie Zylinderanzahl und Verbrauch sowie einer Risikoeinstufung der Versicherung.<p> </p>Die Risikoeinstufung wird anfänglich dem Fahrzeugpreis zugeordnet und dann dem tatsächlichen Risiko in einem Prozess angepasst. Der Wert +3 weist auf ein Fahrzeug mit hohem Risiko hin, während der Wert -3 auf ein voraussichtlich sicheres Fahrzeug hinweist.<p> </p><b>Verwendung</b>: Vorhersage der Risikoeinstufung nach Merkmalen unter Verwendung der Regression oder multivariaten Klassifizierung. <p> </p><b>Zugehörige Untersuchungen</b>: Schlimmer, J.&#160;C. (1987). UCI Machine Learning Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, School of Information and Computer Science
  </td>
</tr>

<tr ID=bike-rental-uci-dataset>
  <td valign=top>Fahrradvermietung UCI-Datensatz</td>
  <td valign=top>
UCI Bike Rental-Dataset basierend auf tatsächlichen Daten der Firma Capital Bikeshare, die ein Fahrradverleih-Netzwerk in Washington DC betreibt.<p> </p>Dieses Dataset enthält eine Zeile pro Stunde für jeden Tag der Jahre 2011 und 2012, insgesamt 17.379 Zeilen. Der Bereich der stündlich verliehenen Fahrräder reicht von 1 bis 977.

  </td>
</tr>

<tr ID=bill-gates-rgb-image>
  <td valign=top>RGB-Bild von Bill Gates</td>
  <td valign=top>
Öffentlich verfügbare, in CSV-Daten konvertierte Imagedatei.<p> </p>Der Code zum Konvertieren des Images wird auf der Detailseite des Modells <strong>Farbquantifizierung mithilfe von K-Means-Clustering</strong> bereitgestellt.
  </td>
</tr>

<tr>
  <td valign=top>Blutspendedaten</td>
  <td valign=top>
Eine Teilmenge der Daten aus der Blutspendedatenbank des Blood Transfusion Service Centers von Hsin-Chu City, Taiwan.<p> </p>Die Spenderdaten beinhalten die Anzahl der Monate seit der letzten Spende, die Häufigkeit oder Gesamtanzahl der Spenden, der seit der letzten Spende vergangenen Zeit sowie die Menge des gespendeten Bluts.<p> </p><b>Verwendung</b>: Das Ziel ist die Vorhersage über die Klassifizierung, ob der Spender im März&#160;2007 Blut gespendet hat, wobei „1“ auf einen Spender im Zielzeitraum und „0“ auf eine Person hinweist, die nicht gespendet hat. <p> </p><b>Zugehörige Untersuchungen</b>: Yeh, I.&#160;C., (2008). UCI Machine Learning Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, School of Information and Computer Science <p> </p>Yeh, I-Cheng, Yang, King-Jang, and Ting, Tao-Ming, „Knowledge discovery on RFM model using Bernoulli sequence“, Expert Systems with Applications, 2008, <a href="http://dx.doi.org/10.1016/j.eswa.2008.07.018">http://dx.doi.org/10.1016/j.eswa.2008.07.018</a>
  </td>
</tr>

<tr ID=book-reviews-from-amazon>
  <td valign=top>Buchbesprechungen von Amazon</td>
  <td valign=top>
Rezensionen zu Büchern bei Amazon, die von Forschern der University of Pennsylvania (<a href="http://www.cs.jhu.edu/~mdredze/datasets/sentiment/">sentiment</a>) von der Website „amazon.com“ übernommen wurden. Weitere Informationen finden Sie im Forschungsbericht "Biographies, Bollywood, Boom-boxes and Blenders: Domain Adaptation for Sentiment Classification" von John Blitzer, Mark Dredze und Fernando Pereira; Association of Computational Linguistics (ACL), 2007.<p> </p>Das ursprüngliche Dataset umfasst 975.000 Bewertungen mit den Rangfolgen 1, 2, 3, 4 oder 5. Die Bewertungen wurden in Englisch verfasst und stammen aus dem Zeitraum 1997-2007. Dieses DataSet wurde auf 10.000 Bewertungen reduziert.
  </td>
</tr>

<tr>
  <td valign=top>Brustkrebsdaten</td>
  <td valign=top>
Eines von drei krebsbezogenen DataSets, das vom Onkologieinstitut bereitgestellt wurde, das häufig in Dokumenten zum maschinellen Lernen verwendet wird. Kombiniert Diagnoseinformationen mit Merkmalen der Laboranalyse von über 300 Gewebeproben.<p> </p><b>Verwendung</b>: Klassifizierung des Krebstyps auf Basis von neun Attributen, von denen einige linear, andere kategorisch sind. <p> </p><b>Zugehörige Untersuchungen</b>: Wohlberg, W.&#160;H., Street, W.&#160;N., &amp; Mangasarian, O.&#160;L. (1995). UCI Machine Learning Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, School of Information and Computer Science
  </td>
</tr>

<tr ID=breast-cancer-features>
  <td valign=top>Brustkrebsfunktionen
  <td valign=top>
Das Dataset enthält Informationen über 102.000 verdächtige Regionen (Kandidaten) von Röntgenbildern, jede beschrieben durch 117 Merkmale. Die Funktionen sind proprietär und ihre Bedeutung wird vom Dataset-Ersteller (Siemens Healthcare) nicht offengelegt. 
  </td>
</tr>

<tr ID=breast-cancer-info>
  <td valign=top>Brustkrebsdaten</td>
  <td valign=top>
Das Dataset enthält zusätzliche Informationen für jede verdächtige Region eines Röntgenbilds. Jedes Beispiel enthält Informationen (z.&#160;B. Überschrift, Patienten-ID, die Koordinaten des Patches in Bezug auf das gesamte Bild) über die Nummer der entsprechenden Zeile im Dataset Bruskrebs-Funktionen. Jeder Patient hat eine Anzahl von Beispielen. Für Patienten, die an Krebs erkrankt sind, sind einige Beispiele positiv und einige negativ. Für Patienten, die nicht an Krebs erkrankt sind, sind alle Beispiele negativ. Das Dataset hat 102.000 Beispiele. Das Dataset ist verzerrt, 0,6&#160;% der Punkte sind positiv, der Rest ist negativ. Das Dataset wurde von Siemens Healthcare zur Verfügung gestellt.
  </td>
</tr>

<tr ID=crm-appetency-labels-shared>
  <td valign=top>CRM Appetency Bezeichnungen freigegeben</td>
  <td valign=top>
Beschriftungen vom KDD Cup 2009 (Kundenbeziehungsprognose, <a href="http://www.sigkdd.org/site/2009/files/orange_small_train_appetency.labels">orange_small_train_appetency.labels</a>).
  </td>
</tr>

<tr ID=crm-churn-labels-shared>
  <td valign=top>CRM Codeänderung Bezeichnungen freigegeben</td>
  <td valign=top>
Beschriftungen vom KDD Cup 2009 (Kundenbeziehungsprognose, <a href="http://www.sigkdd.org/site/2009/files/orange_small_train_churn.labels">orange_small_train_churn.labels</a>).
  </td>
</tr>

<tr ID=crm-dataset-shared>
  <td valign=top>CRM-Dataset gemeinsam genutzt</td>
  <td valign=top>
Diese Daten stammen vom KDD Cup 2009 (Kundenbeziehungsprognose, <a href="http://www.sigkdd.org/kdd-cup-2009-customer-relationship-prediction - orange_small_train.data.zip">orange_small_train.data.zip</a>).<p> </p>Das Dataset enthält 50.000 Kunden des französischen Telekommunikationsunternehmens Orange. Jeder Kunde verfügt über 230 anonymisierte Merkmale, von denen 190 numerisch und 40 kategorisch sortiert sind. Die Merkmale sind sehr karg.
  </td>
</tr>

<tr ID=crm-upselling-labels-shared>
  <td valign=top>CRM Upselling Bezeichnungen freigegeben</td>
  <td valign=top>
Beschriftungen vom KDD Cup 2009 (Kundenbeziehungsprognose, <a href="http://www.sigkdd.org/site/2009/files/orange_large_train_upselling.labels">orange_large_train_upselling.labels</a>).
  </td>
</tr>

<tr>
  <td valign=top>Daten zur Energieeffizienzregression</td>
  <td valign=top>
Eine Sammlung von simulierten Energieprofilen, die auf 12 verschiedenen Gebäudeformen basieren. Die Gebäude werden anhand von acht Merkmalen wie Verglasungsfläche, Verglasungsflächenverteilung und -ausrichtung unterschieden.<p> </p><b>Verwendung</b>: Verwendung der Regression oder Klassifizierung zur Vorhersage der Energieeffizienzbewertung, die die Grundlage für eine von zwei Realwertantworten bildet. Für die Multiklassen-Klassifizierung wird die Antwortvariable auf die nächste ganze Zahl gerundet. <p> </p><b>Zugehörige Untersuchungen</b>: Xifara, A. und Tsanas, A. (2012). UCI Machine Learning Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, School of Information and Computer Science
  </td>
</tr>

<tr ID=flight-delays-data>
  <td valign=top>Flugverspätungsdaten</td>
  <td valign=top>
Pünktlichkeitsratendaten zu Passagierflügen aus der TranStats-Datensammlung des US-Verkehrsministeriums (<a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">On-Time</a>).<p> </p>Der Datensatz deckt den Zeitraum April bis Oktober 2013 ab. Das Dataset wurde vor dem Hochladen in Azure ML Studio wie folgt verarbeitet:<ul><li>Es wurde gefiltert, damit nur die 70 verkehrsreichsten Flughäfen in Kontinental-USA enthalten waren.</li><li>Ausgefallene Flüge wurden als um mehr als 15 Minuten verspätet gekennzeichnet.</li><li>Umgeleitete Flüge wurden herausgefiltert.</li><li>Die folgenden Spalten wurden ausgewählt: Year, Month, DayofMonth, DayOfWeek, Carrier, OriginAirportID, DestAirportID, CRSDepTime, DepDelay, DepDel15, CRSArrTime, ArrDelay, ArrDel15, Cancelled</li></ul>
</td>
</tr>

<tr>
  <td valign=top>Planmäßige Durchführung von Flügen (unformatiert)</td>
  <td valign=top>
Datensätze zu Ankunft und Abflug von Flügen innerhalb der USA von Oktober 2011.<p> </p><b>Verwendung</b>: Vorhersage von Verspätungen im Flugverkehr. <p> </p><b>Zugehörige Untersuchungen</b>: Vom US Dept. of Transportation <a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&amp;DB_Short_Name=On-Time</a>.
  </td>
</tr>

<tr>
  <td valign=top>Waldbranddaten</td>
  <td valign=top>
Enthält Wetterdaten (wie Temperatur- und Luftfeuchtigkeitsindizes sowie Windgeschwindigkeit) aus einem Gebiet im Nordosten von Portugal, die mit Datensätzen von Waldbränden kombiniert wurden.<p> </p><b>Verwendung</b>: Dies ist eine schwierige Regressionsaufgabe, bei der die bei Waldbränden verbrannte Fläche vorhergesagt werden soll. <p> </p><b>Zugehörige Untersuchungen</b>: Cortez, P. und Morais, A. (2008). UCI Machine Learning Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, School of Information and Computer Science <p> </p>[Cortez and Morais, 2007] P. Cortez und A. Morais. A Data Mining Approach to Predict Forest Fires using Meteorological Data. In: J. Neves, M.&#160;F. Santos und J. Machado Hrsg., New Trends in Artificial Intelligence, Proceedings of the 13th EPIA 2007 – Portuguese Conference on Artificial Intelligence, Dezember, Guimarães, Portugal, S. 512–523, 2007. APPIA, ISBN-13 978-989-95618-0-9. Verfügbar unter: <a href="http://www.dsi.uminho.pt/~pcortez/fires.pdf">http://www.dsi.uminho.pt/~pcortez/fires.pdf</a>.
  </td>
</tr>

<tr ID=german-credit-card-uci-dataset>
  <td valign=top>Deutsche Kreditkarte UCI-Dataset</td>
  <td valign=top>
Das DataSet "UCI Statlog" (deutsche Kreditkarte) (<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">Statlog+German+Credit+Data</a>), das die Datei „german.data“ verwendet.<p> </p>Dieses Dataset klassifiziert Personen anhand verschiedener Attribute in hohes und niedriges Kreditrisiko. Jedes Beispiel stellt eine Person dar. Insgesamt existieren 20 numerische und kategorische Merkmale sowie eine binäre Beschriftung (der Wert für das Kreditrisiko). Hohe Kreditrisiken tragen die Beschriftung = 2, während geringe Kreditrisiken die Beschriftung = 1 tragen. Die Belastung für die falsche Klassifizierung eines Beispiels mit geringem Risiko beträgt 1, während die Belastung für die falsche Klassifizierung eines Beispiels mit hohem Risiko 5 beträgt.
  </td>
</tr>

<tr ID=imdb-movie-titles>
  <td valign=top>IMDB-Filmtitel</td>
  <td valign=top>
Das DataSet enthält Informationen über Filme, die in Tweets auf Twitter bewertet wurden: Film-ID in der IMDB, Filmname und Genre, Produktionsjahr. Das DataSet enthält 17.000 Filme. Das DataSet wurde in dem Dokument „S. Dooms, T. De Pessemier und L. Martens. MovieTweetings: a Movie Rating Dataset Collected From Twitter. Workshop on Crowdsourcing and Human Computation for Recommender Systems, CrowdRec at RecSys 2013“ eingeführt.
  </td>
</tr>

<tr>
  <td valign=top>Zwei-Klassen-Daten zu Schwertlilien</td>
  <td valign=top>
Dies ist wahrscheinlich die bekannteste Datenbank, die in der Literatur zur Mustererkennung gefunden werden kann. Das Dataset ist relativ klein und enthält 50 Proben, die jeweils Blütenblatteinstufungen von drei Unterarten der Schwertlilie enthalten.<p> </p><b>Verwendung</b>: Vorhersage der Schwertlilienart aus diesen Einstufungen.  <p> </p><b>Zugehörige Untersuchungen</b>: Fisher, R.&#160;A. (1988). UCI Machine Learning Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, School of Information and Computer Science
  </td>
</tr>

<tr ID=movie-tweets>
  <td valign=top>Film-Tweets</td>
  <td valign=top>
Das Dataset ist eine erweiterte Version des Films Tweetings Datasets. Das Dataset enthält 170K Bewertungen für Filme, extrahiert aus gut strukturierten Tweets auf Twitter. Jede Instanz stellt einen Tweet dar und ist ein Tupel: Benutzer-ID, IMDB Film-ID, Bewertung, Timestamp, Anzahl von Favoriten für diesen Tweet und Anzahl der Retweets dieses Tweets. Das Dataset wurde von A. Said, S. Dooms, B. Loni und D. Tikk für Recommender Systems Challenge 2014 zur Verfügung gestellt.
  </td>
</tr>

<tr>
  <td valign=top>Verbrauchsdaten für verschiedene Fahrzeuge</td>
  <td valign=top>
Dieses DataSet ist eine leicht abgewandelte Version des DataSets, das von der StatLib-Bibliothek der Carnegie Mellon University bereitgestellt wurde. Das Dataset wurde 1983 in der Ausführung der American Statistical Association verwendet.<p> </p>Die Daten listen den Kraftstoffverbrauch für verschiedene Automobile zusammen mit anderen Informationen auf, z.&#160;B. Anzahl der Zylinder, Hubraum, Leistung, Gesamtgewicht und Beschleunigung.<p> </p><b>Verwendung</b>: Vorhersage der Kraftstoffeinsparung auf Basis von drei mehrwertigen Attributen und fünf kontinuierlichen Attributen. <p> </p><b>Zugehörige Untersuchungen</b>: StatLib, Carnegie Mellon University, (1993). UCI Machine Learning Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, School of Information and Computer Science
  </td>
</tr>

<tr>
  <td valign=top>Dataset für binäre Klassifizierung der Diabetes bei Pima-Indianern</td>
  <td valign=top>
Eine Teilmenge der Daten der Datenbank des National Institute of Diabetes and Digestive and Kidney Diseases. Das Dataset wurde gefiltert, um sich auf weibliche Patienten zu konzentrieren, die von den Pima-Indianern abstammen. Die Daten umfassen medizinische Daten wie Zucker- und Insulinspiegel sowie Lebensstilfaktoren.<p> </p><b>Verwendung</b>: Vorhersage einer Diabeteserkrankung für den Patienten (binäre Klassifikation). <p> </p><b>Zugehörige Untersuchungen</b>: Sigillito, V. (1990). UCI Machine Learning Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, School of Information and Computer Science
  </td>
</tr>

<tr>
  <td valign=top>Daten zu Restaurantkunden</td>
  <td valign=top>
Eine Sammlung von Metadaten zu Kunden, einschließlich Demografie und Präferenzen.<p> </p><b>Verwendung</b>: Verwendung dieses DataSets in Kombination mit den anderen beiden Restaurant-DataSets, um ein Empfehlungssystem zu trainieren und zu testen. <p> </p><b>Zugehörige Untersuchungen</b>: Bache, K. und Lichman, M. (2013). UCI Machine Learning Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, C.&#160;A.: University of California, School of Information and Computer Science.
  </td>
</tr>

<tr>
  <td valign=top>Daten zu Restaurantmerkmalen</td>
  <td valign=top>
Eine Sammlung von Metadaten zu Restaurants und ihren Merkmalen wie Speisetyp, Stil und Lage.<p> </p><b>Verwendung</b>: Verwendung dieses DataSets in Kombination mit den anderen beiden Restaurant-DataSets, um ein Empfehlungssystem zu trainieren und zu testen. <p> </p><b>Zugehörige Untersuchungen</b>: Bache, K. und Lichman, M. (2013). UCI Machine Learning Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, C.&#160;A.: University of California, School of Information and Computer Science.
  </td>
</tr>

<tr>
  <td valign=top>Restaurantbewertungen</td>
  <td valign=top>
Enthält von Benutzern abgegebene Bewertungen für Restaurants auf einer Skala von 0 bis 2.<p> </p><b>Verwendung</b>: Verwendung dieses DataSets in Kombination mit den anderen beiden Restaurant-DataSets, um ein Empfehlungssystem zu trainieren und zu testen. <p> </p><b>Zugehörige Untersuchungen</b>: Bache, K. und Lichman, M. (2013). UCI Machine Learning Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, C.&#160;A.: University of California, School of Information and Computer Science.
  </td>
</tr>

<tr>
  <td valign=top>Multiklassen-Dataset zur Stahlhärtung</td>
  <td valign=top>
Dieses Dataset enthält eine Reihe von Datensätzen aus Stahlhärtungsversuchen mit den physikalischen Attributen (Breite, Stärke, Typ) der sich ergebenden Stahltypen.<p> </p><b>Verwendung</b>: Vorhersage eines von zwei numerischen Klassenattributen: Härte oder Belastbarkeit. Sie können auch Korrelationen zwischen den Attributen analysieren.<p> </p>Die Stahlsorten folgen einem festgelegten Standard, der von SAE und anderen Organisationen definiert wurde. Sie suchen nach einer bestimmten „Sorte“ (die Klassenvariable) und möchten die erforderlichen Werte verstehen. <p> </p><b>Zugehörige Untersuchungen</b>: Sterling, D. und Buntine, W., (NA). UCI Machine Learning Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University of California, School of Information and Computer Science <p> </p>Ein nützliches Handbuch zu Stahlsorten finden Sie hier: <a href="http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf">http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf</a>
  </td>
</tr>

<tr>
  <td valign=top>Teleskopdaten</td>
  <td valign=top>
Datensätze von hochenergetischen Gammapartikelausbrüchen zusammen mit Hintergrundrauschen, die beide mithilfe eines Monte Carlo-Prozesses simuliert wurden.<p> </p>Das Ziel der Simulation war die Optimierung der Genauigkeit von bodengestützten atmosphärischen Tscherenkow-Gamma-Teleskopen mithilfe statistischer Methoden, um zwischen gewünschtem Signal (Tscherenkow-Strahlungsschauer) und Hintergrundrauschen (hadronische Schauer, die durch kosmische Strahlung in der oberen Atmosphäre ausgelöst werden) zu unterscheiden.<p> </p>Die Daten wurden vorverarbeitet, um ein gestrecktes Bündel zu erstellen, bei dem die Längsachse zur Kameramitte ausgerichtet ist. Die Merkmale dieser Ellipse (die häufig als Hillas-Parameter bezeichnet werden) gehören zu den Bildparametern, die zur Unterscheidung verwendet werden können.<p> </p><b>Verwendung</b>: Vorhersage, ob das Bild eines Schauers ein Signal oder Hintergrundrauschen darstellt.<p> </p><b>Hinweise</b>: Eine einfache Klassifizierungsgenauigkeit ist für diese Daten nicht aussagekräftig, da die Klassifizierung eines Hintergrundereignisses als Signal schlechter ist als die Klassifizierung eines Signals als Hintergrundrauschen. Für den Vergleich der verschiedenen Klassifizierer sollte das ROC-Diagramm verwendet werden. Die Wahrscheinlichkeit der Annahme eines Hintergrundereignisses als Signal muss unter einem der folgenden Schwellenwerte liegen: 0,01 , 0,02 , 0,05 , 0,1 oder 0,2.<p> </p>Beachten Sie zudem, dass die Anzahl der Hintergrundereignisse (h, für hadronische Schauer) unterbewertet ist, während bei realen Messungen der Wert „h“ oder die Geräuschklasse die Mehrheit der Ereignisse ausmacht. <p> </p><b>Zugehörige Untersuchungen</b>: Bock, R.&#160;K. (1995). UCI Machine Learning Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, C.&#160;A.: University of California, School of Information
  </td>
</tr>

<tr ID=weather-dataset>
  <td valign=top>Wetter-Dataset</td>
  <td valign=top>
Stündliche flächenbasierte Wetterbeobachtungen aus NOAA (<a href="http://cdo.ncdc.noaa.gov/qclcd_ascii/, merged data from 201304 to 201310">zusammengeführte Daten von 201304 bis 201310</a>).<p> </p>Die Wetterdaten umfassen Beobachtungen der Wetterstationen von Flughäfen für den Zeitraum April-Oktober 2013. Das Dataset wurde vor dem Hochladen in Azure ML Studio wie folgt verarbeitet:<ul><li>Die IDs der Wetterstationen wurden den entsprechenden Flughafen-IDs zugeordnet.</li><li>Wetterstationen, die nicht zu den 70 verkehrsreichsten Flughäfen gehören, wurden herausgefiltert</li><li>Die Datumsspalte wurde in separate Jahres-, Monats- und Tagesspalten aufgeteilt.</li><li>Die folgenden Spalten wurden ausgewählt: AirportID, Year, Month, Day, Time, TimeZone, SkyCondition, Visibility, WeatherType, DryBulbFarenheit, DryBulbCelsius, WetBulbFarenheit, WetBulbCelsius, DewPointFarenheit, DewPointCelsius, RelativeHumidity, WindSpeed, WindDirection, ValueForWindCharacter, StationPressure, PressureTendency, PressureChange, SeaLevelPressure, RecordType, HourlyPrecip, Altimeter</li></ul>
  </td>
</tr>

<tr ID=wikipedia-sp-500-dataset>
  <td valign=top>Wikipedia SP 500 Dataset</td>
  <td valign=top>
Die Daten stammen aus Wikipedia (<a href="http://www.wikipedia.org/">http://www.wikipedia.org/</a>) und basieren auf Artikeln der einzelnen S&amp;P-500-Unternehmen, die als XML-Daten gespeichert wurden.<p> </p>Das Dataset wurde vor dem Hochladen in Azure ML Studio wie folgt verarbeitet:<ul><li>Extrahieren der Textinhalte für die einzelnen Unternehmen</li><li>Entfernen der Wiki-Formatierung</li><li>Entfernen aller nicht alphanumerischen Zeichen</li><li>Konvertieren sämtlicher Texte in Kleinbuchstaben</li><li>Bekannte Firmenkategorien wurden hinzugefügt.</li></ul><p> </p>Beachten Sie, dass für einige Unternehmen keine Artikel gefunden werden konnten, daher ist die Anzahl der Datensätze kleiner als 500.
  </td>
</tr>





<tr ID=direct-marketing>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/direct_marketing.csv">direct_marketing.csv</a></td>
  <td valign=top>
Das Dataset enthält Kundendaten und Angaben über ihre Reaktion auf eine direkte Mailing-Kampagne. Jede Zeile stellt einen Kunden dar. Das Dataset enthält 9 Funktionen über demografische Benutzerdaten und Verhalten in der Vergangenheit und 3 Spalten (Visit, Conversion und Spend). „Visit“ ist eine binäre Spalte, die angibt, dass ein Kunde nach der Marketingkampagne die Seite besucht hat, „Conversion“ bedeutet, dass ein Kunde etwas gekauft hat, und „Spend“ ist der Betrag, den er ausgegeben hat. Das Dataset wurde von Kevin Hillstrom für MineThatData E-Mail Analytics And Data Mining Challenge zur Verfügung gestellt.
  </td>
</tr>

<tr ID=lyrl2004-tokens-test>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_test.csv">lyrl2004_tokens_test.csv</a></td>
  <td valign=top>
Merkmale der Beispiele im Reuters-Nachrichtendataset RCV1 V2. Das Dataset besteht aus 781.000 Nachrichtenbeiträgen sowie deren IDs (erste Spalte des Datasets). Alle Artikel sind mit Token und Stoppwort versehen und bezeichnet. Das Dataset wurde von David D. Lewis zur Verfügung gestellt.
  </td>
</tr>

<tr ID=lyrl2004-tokens-train>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_train.csv">lyrl2004_tokens_train.csv</a></td>
  <td valign=top>
Merkmale von Schulungsbeispielen im Reuters-Nachrichtendataset RCV1 V2. Das Dataset besteht aus 23.000 Nachrichtenbeiträgen sowie deren IDs (erste Spalte des Datasets). Alle Artikel sind mit Token und Stoppwort versehen und bezeichnet. Das Dataset wurde von David D. Lewis zur Verfügung gestellt.
  </td>
</tr>

<tr ID=intrusion-detection>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a><br></td>
  <td valign=top>
DataSet vom KDD Cup 1999 Knowledge Discovery and Data Mining Tools Competition (<a href="http://kdd.ics.uci.edu/databases/kddcup99/kddcup99.html">kddcup99.html</a>).<p> </p>Der Datensatz wurde heruntergeladen und im Azure-Blobspeicher abgelegt (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a>) und enthält Trainings- und Testdatensätze. Der Schulungsdatensatz enthält ca. 126.000 Zeilen und 43 Spalten, inklusive Überschriften; 3 Spalten sind Teil der Überschrifteninformationen, und die anderen 40 Spalten bestehen aus numerischen und Zeichenfolgen-/Kategoriefeatures, sind verfügbar, um das Modell anzulernen. Die Testdaten enthalten ca. 22.500 Testbeispiele, die die gleichen 43 Spalten wie die Schulungsdaten enthalten.

  </td>
</tr>

<tr ID=rcv1-v2-topics-qrels>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/rcv1-v2.topics.qrels.csv">rcv1-v2.topics.qrels.csv</a></td>
  <td valign=top>
Themenzuweisungen für Nachrichtenartikel im Reuters-Nachrichtendataset RCV1-V2 Ein Nachrichtenartikel kann verschiedenen Themen zugewiesen werden. Das Format jeder Zeile ist "<topic name> <document id> 1". Das Dataset enthält 2,6 Mio. Themenzuweisungen. Das DataSet wurde von David D. Lewis zur Verfügung gestellt.
  </td>
</tr>

<tr ID=student-performance>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a></td>
  <td valign=top>
Diese Daten stammen aus dem KDD Cup 2010 zur Leistungsbewertung von Schülern (<a href="http://www.kdd.org/kdd-cup-2010-student-performance-evaluation">Schülerleistungsbewertung</a>). Die verwendeten Daten stammen aus dem Trainingssatz „Algebra_2008_2009“ (Stamper, J., Niculescu-Mizil, A., Ritter, S., Gordon, G.J., &amp; Koedinger, K.R. (2010). Algebra I 2008-2009. DataSet des KDD Cup 2010 – Educational Data Mining Challenge. Sie finden das Dataset unter <a href="http://pslcdatashop.web.cmu.edu/KDDCup/downloads.jsp">downloads.jsp</a> oder <a href="http://www.kdd.org/sites/default/files/kddcup/site/2010/files/algebra_2008_2009.zip">algebra_2008_2009.zip</a>.<p> </p>Das Dataset wurde in Azure-Blobspeicher heruntergeladen und gespeichert (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a>). Es enthält Protokolldateien aus einem Tutorensystem für Schüler. Die bereitgestellten Funktionen umfassen eine Problem-ID und eine zugehörige Kurzbeschreibung, die Schüler-ID, einen Zeitstempel und die Anzahl der Versuche, bevor das Problem vom Schüler ordnungsgemäß gelöst wurde. Das ursprüngliche DataSet umfasst 8,9 Millionen Datensätze. Dieses DataSet wurde auf die ersten 100.000 Zeilen reduziert. Das DataSet umfasst 23 durch Tabulatoren getrennte Spalten mit verschiedenen Typen: numerisch, kategorisch und Zeitstempel.

  </td>
</tr>




</table>


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

<!---HONumber=AcomDC_0921_2016-->