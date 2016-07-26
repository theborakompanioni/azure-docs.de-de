<properties
	pageTitle="Einfaches Experiment in Machine Learning Studio | Microsoft Azure"
	description="In diesem Machine Learning-Tutorial werden die Schritte eines einfachen Data Science-Experiments beschrieben. Wir sagen den Preis für ein Auto vorher, indem wir einen Regressionsalgorithmus verwenden."
	keywords="Experiment,Lineare Regression,Machine Learning-Algorithmen,Machine Learning-Tutorial,Verfahren für Vorhersagemodellierung,Data Science-Experiment"
	services="machine-learning"
	documentationCenter=""
	authors="garyericson"
	manager="paulettm"
	editor="cgronlun"/>

<tags
	ms.service="machine-learning"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="hero-article"
	ms.date="07/14/2016"
	ms.author="garye"/>

# Machine Learning-Tutorial: Erstellen Ihres ersten Data Science-Experiments in Azure Machine Learning Studio

In diesem Machine Learning-Tutorial werden die Schritte eines einfachen Data Science-Experiments beschrieben. Wir erstellen ein Modell für die lineare Regression, mit dem der Preis eines Autos anhand von verschiedenen Variablen wie Hersteller und technische Angaben prognostiziert wird. Für dieses Vorgehen verwenden wir Azure Machine Learning Studio, um ein einfaches Predictive Analytics-Experiment zu entwickeln und schrittweise zu verfeinern.

*Predictive Analytics* ist eine Art von Data Science, bei der aktuelle Daten genutzt werden, um zukünftige Ergebnisse vorherzusagen. Ein sehr einfaches Beispiel für Predictive Analytics ist im vierten Video der Reihe „Data Science für Einsteiger“ enthalten: [Vorhersagen einer Antwort mit einem einfachen Modell](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) (Dauer: 7:42).

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## Welchen Beitrag leistet Machine Learning Studio?

Mit Machine Learning Studio ist es einfach, ein Experiment einzurichten, indem Sie Drag&Drop-Module verwenden, die basierend auf der Vorhersagemodellierung vorprogrammiert sind. Zum Ausführen des Experiments und Vorhersagen einer Antwort verwenden Sie Machine Learning Studio, um *ein Modell zu erstellen*, *das Modell zu trainieren* und *das Modell zu bewerten und zu testen*.

Öffnen Sie Machine Learning Studio: [https://studio.azureml.net](https://studio.azureml.net). Wenn Sie sich bereits an Machine Learning Studio angemeldet haben, können Sie auf **Hier anmelden** klicken. Klicken Sie andernfalls auf **Registrieren**, und wählen Sie die Free-Option oder eine kostenpflichtige Option.

Allgemeine Informationen zu Machine Learning Studio finden Sie unter [Was ist Machine Learning Studio?](machine-learning-what-is-ml-studio.md).

## Fünf Schritte zum Erstellen von Hypothesen

In diesem Lernprogramm für maschinelles Lernen führen Sie fünf grundlegende Schritte zur Experimenterstellung in Machine Learning Studio aus, um Ihr Modell zu erstellen, zu trainieren und zu bewerten:

- Modellerstellung
	- [Schritt 1: Bereitstellen von Daten]
	- [Schritt 2: Vorverarbeiten von Daten]
	- [Schritt 3: Definieren von Funktionen]
- Modelltraining
	- [Schritt 4: Auswählen und Anwenden eines Lernalgorithmus]
- Modellbewertung und -Tests
	- [Schritt 5: Erstellen von Preisprognosen für neue Fahrzeuge]

[Schritt 1: Bereitstellen von Daten]: #step-1-get-data
[Schritt 2: Vorverarbeiten von Daten]: #step-2-preprocess-data
[Schritt 3: Definieren von Funktionen]: #step-3-define-features
[Schritt 4: Auswählen und Anwenden eines Lernalgorithmus]: #step-4-choose-and-apply-a-learning-algorithm
[Schritt 5: Erstellen von Preisprognosen für neue Fahrzeuge]: #step-5-predict-new-automobile-prices


## Schritt 1: Bereitstellen von Daten

Machine Learning Studio enthält bereits zahlreiche Beispiel-DataSets, unter denen Sie wählen können, und Sie können Daten aus vielen Quellen importieren. Für dieses Beispiel verwenden wir den mitgelieferten Beispieldatensatz **Automobile price data (Raw)**. Dieses DataSet enthält Einträge für eine Reihe verschiedener Automobile, einschließlich Informationen wie Marke, Modell, technischen Angaben und Preis.

1. Starten Sie ein neues Experiment, indem Sie am unteren Rand des Fensters von Machine Learning Studio auf **+NEU** klicken und anschließend **EXPERIMENT** und dann **Blank Experiment** auswählen. Wählen Sie den Standardnamen am oberen Rand des Bereichs aus, und geben Sie einen aussagekräftigeren Namen ein, z. B. **Automobilpreisvorhersage**.

2. Links vom Experimentbereich finden Sie eine Palette mit Datensätzen und Modulen. Geben Sie **automobile** in das Suchfeld im oberen Bereich dieser Palette ein, um das Dataset mit dem Namen **Automobile price data (Raw)** zu finden.

	![Palettensuche][screen1a]

3. Ziehen Sie den Datensatz in den Experimentbereich.

	![Datensatz][screen1]

Sie können auf den Ausgabeport im unteren Bereich des Automobil-Datasets klicken und dann **Visualisieren** auswählen, um die enthaltenen Daten anzuzeigen.

![Modulausgangsport][screen1c]

Die Variablen im Datensatz werden als Spalten angezeigt, und jede Instanz eines Automobils füllt eine Zeile. Die Spalte ganz rechts (Spalte 26 mit dem Titel "price") ist die Zielvariable, die wir vorhersagen möchten.

![Datensatzvisualisierung][screen1b]

Schließen Sie das Visualisierungsfenster, indem Sie auf das "**X**" in der oberen rechten Ecke klicken.

## Schritt 2: Vorverarbeiten von Daten

DataSets müssen vor der Analyse normalerweise vorverarbeitet werden. Möglicherweise sind Ihnen bereits die fehlenden Werte in den Spalten verschiedener Zeilen aufgefallen. Damit das Modell die Daten richtig analysieren kann, müssen diese fehlenden Werte bereinigt werden. In unserem Fall entfernen wir alle Zeilen, in denen Werte fehlen. Außerdem enthält die Spalte **normalisierte Verluste** viele fehlende Werte, daher schließen wir diese Spalte komplett aus dem Modell aus.

> [AZURE.TIP] Die Bereinigung fehlender Werte aus den Eingabedaten ist eine Voraussetzung für die Verwendung der meisten Module.

Wir entfernen zunächst die Spalte **normalized-losses** und anschließend alle Zeilen, in denen Daten fehlen.

1. Geben Sie **select columns** in das Suchfeld im oberen Bereich der Modulpalette ein, um das Modul [Select Columns in Dataset][select-columns] zu suchen, ziehen Sie dieses in den Experimentbereich, und verbinden Sie es dann mit dem Ausgabeport des Datasets **Automobile price data (Raw)**. Mit diesem Modul können wir auswählen, welche Daten wir in unserem Modell ein- bzw. ausschließen möchten.

2. Wählen Sie das Modul [Select Columns in Dataset][select-columns] aus, und klicken Sie im Bereich **Eigenschaften** auf **Spaltenauswahl starten**.

	- Klicken Sie auf der linken Seite auf **With rules** (Mit Regeln).
	- Klicken Sie unter **Begin With** (Beginnen mit) auf **All columns** (Alle Spalten). Damit wird [Select Columns in Dataset][select-columns] angewiesen, alle Spalten zu durchlaufen (mit Ausnahme derer, die wir jetzt ausschließen werden).
	- Wählen Sie in den Dropdownlisten die Optionen **Ausschließen** und **Spaltennamen**, und klicken Sie in das Textfeld. Eine Liste von Spalten wird angezeigt. Wählen Sie **normalized-losses** aus. Daraufhin wird die Spalte dem Textfeld hinzugefügt.
	- Klicken Sie auf die Schaltfläche mit einem Häkchen ("OK"), um die Spaltenauswahl zu schließen.

    ![Spalten auswählen][screen3]

	Das Eigenschaftenpanel für **Select Columns in Dataset** zeigt an, dass alle Spalten des Datasets mit Ausnahme von **normalized-losses** durchlaufen werden.

    ![Eigenschaften für „Select Columns in Dataset“][screen4]

    > [AZURE.TIP] Sie können einen Kommentar zu einem Modul eingeben, indem Sie auf das Modul doppelklicken und Text eingeben. Auf diese Weise können Sie mit einem Blick sehen, welche Funktion das Modul in Ihrem Experiment erfüllt. Doppelklicken Sie in diesem Fall auf das [Select Columns in Dataset][select-columns]-Modul, und geben Sie den Kommentar „normalized-losses ausschließen“ ein.

3. Ziehen Sie das Modul [Clean Missing Data][clean-missing-data] in den Experimentbereich, und verbinden Sie es mit dem [Select Columns in Dataset][select-columns]-Modul. Wählen Sie im Panel **Eigenschaften** unter **Bereinigungsmodus** die Option **Gesamte Zeile entfernen** aus, um die Daten zu bereinigen, indem Sie alle Zeilen entfernen, in denen Werte fehlen. Doppelklicken Sie auf das Modul, und geben Sie den Kommentar "Remove missing value rows" ein.

	![Bereinigen fehlender Dateneigenschaften][screen4a]

4. Führen Sie das Experiment aus, indem Sie unterhalb des Experimentbereichs auf **AUSFÜHREN** klicken.

Nach Abschluss des Experiments sind alle Module mit einem grünen Häkchen markiert, um anzuzeigen, dass diese erfolgreich abgeschlossen wurden. Beachten Sie auch den Status **Finished running** in der oberen rechten Ecke.

![Erste Experimentausführung][screen5]

Bislang haben wir im Experiment nur Daten bereinigt. Wenn Sie das bereinigte DataSet anzeigen möchten, klicken Sie auf den linken Ausgabeports des Moduls [Clean Missing Data][clean-missing-data] ("Cleaned dataset"), und wählen Sie **Visualisieren** aus. Beachten Sie, dass die Spalte **normalized-losses** nicht mehr aufgeführt wird und keine fehlenden Werte auftreten.

Nach der Bereinigung der Daten können wir nun angeben, welche Funktionen wir im Vorhersagemodell verwenden möchten.

## Schritt 3: Definieren von Funktionen

Bei Machine Learning versteht man unter *Funktionen* einzeln messbare Eigenschaften des untersuchten Gesamtobjekts. In unserem DataSet stellt jede Zeile ein Automobil dar, und jede Spalte ist eine Funktion dieses Automobils.

Einen guten Satz von Funktionen für die Erstellung eines Vorhersagemodells finden Sie durch Ausprobieren und Kenntnisse des zu lösenden Problems. Manche Funktionen eignen sich besser für die Vorhersage des Ziels als andere. Außerdem haben manche Funktionen eine starke Korrelation mit anderen Funktionen, z. B. "city-mpg" und "highway-mpg". Diese liefern kaum neue Informationen für das Modell und können entfernt werden.

Wir werden ein Modell erstellen, das eine Teilmenge der Funktionen in unserem Datensatz verwendet. Sie können später jederzeit andere Funktionen auswählen, das Experiment erneut ausführen und versuchen, bessere Ergebnisse zu erhalten. Hier probieren wir vorerst die folgenden Funktionen aus:

	make, body-style, wheel-base, engine-size, horsepower, peak-rpm, highway-mpg, price


1. Ziehen Sie ein weiteres [Select Columns in Dataset][select-columns]-Modul in den Experimentbereich, und verbinden Sie es mit dem linken Ausgabeport des Moduls [Clean Missing Data][clean-missing-data]. Doppelklicken Sie auf das Modul, und geben Sie "Select features for prediction" ein.

2. Klicken Sie im Bereich **Eigenschaften** auf **Launch column selector**.

3. Klicken Sie auf **With rules** (Mit Regeln).

4. Klicken Sie unter **Begin With** (Beginnen mit) auf **No columns** (Keine Spalten), und wählen Sie dann **Include** (Einschließen) und **column names** (Spaltennamen) in der Filterzeile aus. Geben Sie unsere Liste der Spaltennamen ein. Damit weisen Sie das Modul an, nur die angegebenen Spalten zu durchlaufen.

	> [AZURE.TIP] Durch das Ausführen des Experiments haben wir sichergestellt, dass die Spaltendefinitionen für unsere Daten aus dem Dataset durch das Modul [Fehlende Daten bereinigen][clean-missing-data] verlaufen. Dies bedeutet, dass andere Module, mit denen Sie eine Verbindung herstellen, auch über Informationen aus dem Dataset verfügen.

5. Klicken Sie auf das Häkchen ("OK").

![Spalten auswählen][screen6]

Dieser Vorgang erzeugt das DataSet, das wir in den nächsten Schritten im Lernalgorithmus verwenden werden. Sie können den Vorgang später jederzeit mit einer anderen Auswahl an Funktionen wiederholen.

## Schritt 4: Auswählen und Anwenden eines Lernalgorithmus

Nachdem die Daten vorbereitet sind, können Sie das Vorhersagemodell anhand von Training und Tests erarbeiten. Wir werden das Modell zunächst mit unseren Daten trainieren und dann testen, wie genau seine Preisvorhersagen zutreffen. Vorerst kümmern wir uns noch nicht darum, warum wir ein Modell trainieren und anschließend testen müssen.

*Klassifizierung* und *Regression* sind zwei Techniken für beaufsichtigtes maschinelles Lernen. Mit der Klassifizierung wird eine Antwort aus einem definierten Satz mit Kategorien vorhergesagt, z.B. eine Farbe (Rot, Blau oder Grün). Die Regression wird verwendet, um eine Zahl vorherzusagen.

Da wir einen Preis vorhersagen möchten (also eine Zahl), verwenden wir ein Regressionsmodell. Für dieses Beispiel trainieren wir ein einfaches *lineares Regressionsmodell* und testen es anschließend im nächsten Schritt.

1. Wir verwenden die uns vorliegenden Daten sowohl zum Trainieren als auch zum Testen, indem wir sie in separate Trainings- und Testsätze aufteilen. Ziehen Sie das Modul [Split Data][split] in den Experimentbereich, und verbinden Sie es mit der Ausgabe des letzten [Select Columns in Dataset][select-columns]-Moduls. Setzen Sie **Anteil der Zeilen im ersten Ausgabedatensatz** auf 0,75. Mit dieser Einstellung verwenden wir 75 Prozent der Daten zum Trainieren des Modells und halten 25 Prozent für Tests zurück.

	> [AZURE.TIP] Sie können den Parameter **Zufälliger Ausgangswert** ändern, um unterschiedliche zufällige Proben für Training und Tests zu erstellen. Dieser Parameter steuert den Ausgangswert des Pseudo-Zufallszahlengenerators.

2. Führen Sie das Experiment aus. Dadurch können die Module [Select Columns in Dataset][select-columns] und [Split Data][split] die Spaltendefinitionen an die Module übergeben, die wir als Nächstes hinzufügen werden.

3. Um den Lernalgorithmus auszuwählen, erweitern Sie die Kategorie **Machine Learning** in der Modulpalette links vom Experimentbereich, und erweitern Sie anschließend **Modell initialisieren**. Daraufhin werden verschiedene Kategorien von Modulen angezeigt, die zur Initialisierung eines Algorithmus für maschinelles Lernen verwendet werden können.

	Wählen Sie für dieses Experiment das [Linear Regression][linear-regression]-Modul unter der Kategorie **Regression** aus (oder geben Sie "linear regression" im Suchfeld der Palette ein), und ziehen Sie das Modul in den Experimentbereich.

4. Suchen Sie das Modul [Modell trainieren][train-model], und ziehen Sie es ebenfalls in den Experimentbereich. Verbinden Sie die Ausgabe des linken Eingangsports mit dem Ausgang des Moduls [Linear Regression][linear-regression]. Verbinden Sie den rechten Eingangsport mit dem Trainingsdatenausgang (linker Port) des [Split Data][split]-Moduls.

5. Wählen Sie das Modul [Train Model][train-model] aus, klicken Sie im Bereich **Eigenschaften** auf **Spaltenauswahl starten**, und wählen Sie dann die Spalte **price** aus. Dies ist der Wert, den unser Modell vorhersagen wird.

	![Spaltenauswahl "price"][screen7]

6. Führen Sie das Experiment aus.

Als Ergebnis erhalten Sie ein trainiertes Regressionsmodul, mit dem Sie neue Proben bewerten können, um Vorhersagen zu machen.

![Anwenden des Algorithmus zum maschinellen Lernen][screen8]

## Schritt 5: Erstellen von Preisprognosen für neue Fahrzeuge

Wir haben das Modell nun unter Verwendung von 75 Prozent unserer Daten trainiert und können die restlichen 25 Prozent der Daten dafür aufwenden, zu bewerten, wie gut unser Modell funktioniert.

1. Suchen Sie das Modul [Modell bewerten][score-model], ziehen Sie es in den Experimentbereich, und verbinden Sie den linken Eingangsport mit dem Ausgang des [Train Model][train-model]-Moduls. Verbinden Sie den rechten Eingangsport mit dem Testdatenausgang (rechter Port) des [Split Data][split]-Moduls.

	![Modul Modell bewerten][screen8a]

2. Um das Experiment auszuführen und die Ausgabe des [Score Model][score-model]-Moduls anzuzeigen, doppelklicken Sie auf den Ausgabeport, und wählen Sie dann **Visualisieren** aus. Die Ausgabe zeigt die vorhergesagten Preiswerte zusammen mit den bekannten Werten aus den Testdaten an.

3. Um abschließend die Qualität der Ergebnisse zu überprüfen, ziehen Sie das [Evaluate Model][evaluate-model]-Modul in den Experimentbereich, und verbinden Sie den linken Eingangsport mit dem Ausgang des Modells [Score Model][score-model]. (Es gibt zwei Eingangsports, da das [Evaluate Model][evaluate-model]-Modul verwendet werden kann, um zwei Modelle zu vergleichen.)

4. Führen Sie das Experiment aus.

Um die Ausgabe des [Evaluate Model][evaluate-model]-Moduls anzuzeigen, klicken Sie auf den Ausgabeport, und wählen Sie dann **Visualisieren** aus. Die folgenden Statistiken werden für unser Modell angezeigt:

- **Mean Absolute Error** (MAE) – der Mittelwert der absoluten Fehler (ein *Fehler* ist die Differenz zwischen vorhergesagtem und tatsächlichem Wert).
- **Root Mean Squared Error** (RMSE) – die Quadratwurzel des Durchschnitts des Quadrats der Vorhersagefehler für das Test-DataSet.
- **Relative Absolute Error** – der Mittelwert der absoluten Fehler relativ zur absoluten Differenz zwischen tatsächlichen Werten und dem Durchschnitt aller tatsächlichen Werte.
- **Relative Squared Error** – der Durchschnitt der quadrierten Fehler relativ zur quadrierten Differenz zwischen tatsächlichen Werten und dem Durchschnitt aller tatsächlichen Werte.
- **Coefficient of Determination** – dieser auch als **R-Quadrat** bekannte Wert ist eine statistische Kenngröße, die angibt, wie gut ein Modell zu den Daten passt.

Für jede Fehlerstatistik sind kleinere Werte besser. Ein kleinerer Wert gibt an, dass die Vorhersagen genauer mit den tatsächlichen Werten übereinstimmen. Für den **Bestimmungskoeffizienten** gilt: Je näher der Bestimmungskoeffizient am Wert eins (1,0) liegt, desto besser die Vorhersage.

![Auswertung der Ergebnisse][screen9]

Das fertige Experiment sollte folgendermaßen aussehen:

![Lernprogramm für maschinelles Lernen: Führen Sie ein lineares Regressionsexperiment mit Vorhersagemodell-Techniken aus.][screen10]

## Nächste Schritte

Da Sie jetzt ein erstes Lernprogramm zum maschinellen Lernen abgeschlossen und das Experiment eingerichtet haben, können Sie es wiederholen, um das Modell zu verbessern. Sie können z. B. die Funktionen ändern, die Sie in Ihrer Vorhersage verwenden. Oder Sie können die Eigenschaften des [Linearen Regressionsalgorithmus][linear-regression] ändern oder einen völlig anderen Algorithmus ausprobieren. Sie können Ihrem Ereignis sogar mehrere Algorithmen zum maschinellen Lernen gleichzeitig hinzufügen und jeweils zwei vergleichen, indem Sie das [Evaluate Model][evaluate-model]-Modul verwenden.

> [AZURE.TIP] Mit der Schaltfläche **SPEICHERN ALS** unterhalb des Experimentbereichs können Sie Kopien der Iterationen Ihres Experiments speichern. Sie können alle Iterationen Ihres Experiments anzeigen, indem Sie unterhalb des Experimentbereichs auf **AUSFÜHRUNGSVERLAUF ANZEIGEN** klicken. Weitere Informationen finden Sie unter [Verwalten von Experimentiterationen in Azure Machine Learning-Studio][runhistory].

[runhistory]: machine-learning-manage-experiment-iterations.md

Wenn Sie mit Ihrem Modell zufrieden sind, können Sie es als Webdienst bereitstellen, der Automobilpreise anhand neuer Daten vorhersagt. Weitere Informationen finden Sie unter [Bereitstellen eines Azure Machine Learning-Webdiensts][publish].

[publish]: machine-learning-publish-a-machine-learning-web-service.md

Eine umfassendere und ausführlichere exemplarische Vorgehensweise für Vorhersagemodell-Techniken zum Erstellen, Trainieren, Bewerten und Bereitstellen eines Modells finden Sie unter [Entwickeln einer Vorhersagelösung mit Azure Machine Learning][walkthrough].

[walkthrough]: machine-learning-walkthrough-develop-predictive-solution.md

<!-- Images -->
[screen1]: ./media/machine-learning-create-experiment/screen1.png
[screen1a]: ./media/machine-learning-create-experiment/screen1a.png
[screen1b]: ./media/machine-learning-create-experiment/screen1b.png
[screen1c]: ./media/machine-learning-create-experiment/screen1c.png
[screen2]: ./media/machine-learning-create-experiment/screen2.png
[screen3]: ./media/machine-learning-create-experiment/screen3.png
[screen4]: ./media/machine-learning-create-experiment/screen4.png
[screen4a]: ./media/machine-learning-create-experiment/screen4a.png
[screen5]: ./media/machine-learning-create-experiment/screen5.png
[screen6]: ./media/machine-learning-create-experiment/screen6.png
[screen7]: ./media/machine-learning-create-experiment/screen7.png
[screen8]: ./media/machine-learning-create-experiment/screen8.png
[screen8a]: ./media/machine-learning-create-experiment/screen8a.png
[screen9]: ./media/machine-learning-create-experiment/screen9.png
[screen10]: ./media/machine-learning-create-experiment/complete-linear-regression-experiment.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/

<!---HONumber=AcomDC_0720_2016-->