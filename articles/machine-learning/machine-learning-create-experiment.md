---
title: Einfaches Experiment in Machine Learning Studio | Microsoft-Dokumentation
description: "In diesem Machine Learning-Tutorial werden die Schritte eines einfachen Data Science-Experiments beschrieben. Wir sagen den Preis für ein Auto vorher, indem wir einen Regressionsalgorithmus verwenden."
keywords: "Experiment,Lineare Regression,Machine Learning-Algorithmen,Machine Learning-Tutorial,Verfahren für Vorhersagemodellierung,Data Science-Experiment"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: b6176bb2-3bb6-4ebf-84d1-3598ee6e01c6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/20/2017
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: 6d749e5182fbab04adc32521303095dab199d129
ms.openlocfilehash: 0539e9d04c61d35de56a29d350c07654c095c576
ms.lasthandoff: 03/22/2017


---

# <a name="machine-learning-tutorial-create-your-first-data-science-experiment-in-azure-machine-learning-studio"></a>Machine Learning-Tutorial: Erstellen Ihres ersten Data Science-Experiments in Azure Machine Learning Studio

Wenn Sie **Azure Machine Learning Studio** noch nie zuvor verwendet haben, hilft Ihnen dieses Tutorial.

In diesem Tutorial wird erläutert, wie Sie Studio zum ersten Mal zum Erstellen eines Experiments mit maschinellem Lernen verwenden. Dieses Experiment testet ein Analysemodell, das den Preis eines Autos anhand von verschiedenen Variablen wie Marke und technischen Angaben prognostiziert.

> [!NOTE]
> Dieses Tutorial vermittelt Ihnen die Grundlagen dazu, wie Sie Module per Drag & Drop in Ihr Experiment aufnehmen, sie miteinander verbinden, das Experiment ausführen und die Ergebnisse anzeigen. Die Themen „maschinelles Lernen“ oder die Frage, wie die über 100 integrierten Algorithmen und die in Studio enthaltenen Module zur Datenbearbeitung verwendet werden, werden hier nicht erörtert.
>
>Wenn dies Ihre erste Berührung mit maschinellem Lernen ist, stellt die Videoreihe [Data Science für Einsteiger](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) möglicherweise einen guten Start dar. Diese Videoreihe bietet eine gute Einführung in das maschinelle Lernen und verwendet alltägliche Sprache und Konzepte.
>
>Wenn Sie bereits mit maschinellem Lernen vertraut sind, aber weitere allgemeine Informationen zu Machine Learning Studio und den darin enthaltenen Algorithmen für maschinelles Lernen wünschen, finden Sie nachstehend einige gute Ressourcen:
>
- [Was ist Machine Learning Studio?](machine-learning-what-is-ml-studio.md) - Hierbei handelt es sich um eine allgemeine Übersicht über Studio.
- [Grundlagen des maschinellen Lernens mit Beispielalgorithmen](machine-learning-basics-infographic-with-algorithm-examples.md): Diese Infografik ist hilfreich, wenn Sie mehr über die verschiedenen Algorithmustypen für maschinelles Lernen in Machine Learning Studio erfahren möchten.
- [Machine Learning Guide (Machine Learning-Handbuch)](https://gallery.cortanaintelligence.com/Tutorial/Machine-Learning-Guide-1): Diese Anleitung enthält ähnliche Informationen wie die oben genannte Infografik, jedoch in einem interaktiven Format.
- [Machine Learning-Cheat Sheet für Algorithmen](machine-learning-algorithm-cheat-sheet.md) und [Auswählen von Algorithmen für Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md): Dieses herunterladbare Poster und der zugehörige Artikel besprechen Studio-Algorithmen im Detail.
- [Machine Learning Studio: Algorithm and Module Help (Machine Learning Studio: Hilfe zu Algorithmen und Modulen)](https://msdn.microsoft.com/library/azure/dn905974.aspx): Dies ist die vollständige Referenz zu allen Studio-Modulen, einschließlich der Machine Learning-Algorithmen.

<!-- -->

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="how-does-machine-learning-studio-help"></a>Welchen Beitrag leistet Machine Learning Studio?

Mit Machine Learning Studio ist es einfach, ein Experiment einzurichten, indem Sie Drag&Drop-Module verwenden, die basierend auf der Vorhersagemodellierung vorprogrammiert sind.

Mithilfe eines interaktiven, visuellen Arbeitsbereiches platzieren Sie ***Datasets*** und ***Analysemodule*** per Drag & Drop in einem interaktiven Bereich. Anschließend verbinden Sie sie zu einem ***Experiment***, das Sie in Machine Learning Studio ausführen.
Sie ***erstellen ein Modell***, ***trainieren*** und ***bewerten und testen es***.

Sie können Ihren Modellentwurf durchlaufen und dabei das Experiment bearbeiten und es ausführen, bis Sie die Ergebnisse erhalten, die Sie suchen. Wenn Ihr Modell bereit ist, können Sie es als ***Webdienst*** veröffentlichen, damit andere Benutzer ihm neue Daten zusenden können und im Gegenzug Vorhersagen erhalten.

## <a name="open-machine-learning-studio"></a>Öffnen von Machine Learning Studio

Wechseln Sie zu [https://studio.azureml.net](https://studio.azureml.net), um Ihre ersten Schritte mit Studio zu tätigen. Wenn Sie sich zuvor schon einmal bei Machine Learning Studio angemeldet haben, klicken Sie einfach auf **Sign In** (Anmelden). Andernfalls klicken Sie auf **Sign up here** (Registrieren Sie sich hier), und wählen Sie zwischen der kostenlosen und der kostenpflichtigen Option aus.

![Melden Sie sich bei Azure Machine Learning Studio an.][sign-in-to-studio]
<br/>
***Melden Sie sich bei Azure Machine Learning Studio an.***

## <a name="five-steps-to-create-an-experiment"></a>Fünf Schritte zum Erstellen von Hypothesen

In diesem Tutorial für maschinelles Lernen wird anhand von fünf grundlegenden Schritten die Experimenterstellung in Machine Learning Studio dargelegt, um Ihr Modell zu erstellen, zu trainieren und zu bewerten:

- **Erstellen des Modells**
    - [Schritt 1: Bereitstellen von Daten]
    - [Schritt 2: Vorbereiten der Daten]
    - [Schritt 3: Definieren von Funktionen]
- **Trainieren des Modells**
    - [Schritt 4: Auswählen und Anwenden eines Lernalgorithmus]
- **Bewerten und Testen des Modells**
    - [Schritt 5: Erstellen von Preisprognosen für neue Fahrzeuge]

[Schritt 1: Bereitstellen von Daten]: #step-1-get-data
[Schritt 2: Vorbereiten der Daten]: #step-2-prepare-the-data
[Schritt 3: Definieren von Funktionen]: #step-3-define-features
[Schritt 4: Auswählen und Anwenden eines Lernalgorithmus]: #step-4-choose-and-apply-a-learning-algorithm
[Schritt 5: Erstellen von Preisprognosen für neue Fahrzeuge]: #step-5-predict-new-automobile-prices

> [!TIP] 
> Eine Arbeitskopie des folgenden Experiments finden Sie in der [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com). Wechseln Sie zu **[Your first data science experiment - Automobile price prediction (Ihr erstes Data Science-Experiment – Vorhersage von Automobilpreisen)](https://gallery.cortanaintelligence.com/Experiment/Your-first-data-science-experiment-Automobile-price-prediction-1)**, und klicken Sie auf **In Studio öffnen**, um eine Kopie des Experiments in Ihren Machine Learning Studio-Arbeitsbereich herunterzuladen.


## <a name="step-1-get-data"></a>Schritt 1: Bereitstellen von Daten

Die Grundvoraussetzung für maschinelles Lernen sind Daten.
Machine Learning Studio enthält bereits einige Beispieldatasets, die Sie verwenden können. Alternativ können Sie Daten aus vielen Quellen importieren. Für dieses Beispiel verwenden wir das Beispieldataset **Automobile price data (Raw)** (Automobilpreisdaten (roh)), das in Ihrem Arbeitsbereich zu finden ist.
Dieses Dataset enthält Einträge für eine Reihe verschiedener Automobile, einschließlich Informationen wie Marke, Modell, technische Angaben und Preis.

Hier wird erklärt, wie Sie das Dataset in Ihr Experiment importieren.

1. Erstellen Sie ein neues Experiment, indem Sie am unteren Rand des Machine Learning Studio-Fensters auf **+NEW** (+NEU) klicken. Wählen Sie anschließend **EXPERIMENT** und **Blank Experiment** (Leeres Experiment) aus.

2. Dieses Experiment erhält einen Standardnamen, der oben im Zeichenbereich angezeigt wird. Wählen Sie den Text aus, und benennen Sie das Experiment mit einem aussagekräftigen Namen um, z.B. **Automobile price prediction** (Vorhersage für Automobilpreise). Der Name muss nicht eindeutig sein.

    ![Umbenennen des Experiments][rename-experiment]

2. Links vom Experimentbereich finden Sie eine Palette mit Datensätzen und Modulen. Geben Sie im oberen Bereich dieser Palette **automobile** in das Suchfeld ein, um das Dataset mit dem Namen **Automobile price data (Raw)** zu finden. Ziehen Sie das Dataset in den Experimentbereich.

    ![Suchen Sie das Automobildataset, und ziehen Sie es in den Experimentbereich.][type-automobile]
    <br/>
    ***Suchen Sie das Automobildataset, und ziehen Sie es in den Experimentbereich.***

Sie können auf den Ausgabeport im unteren Bereich des Automobil-Datasets klicken und dann **Visualisieren**auswählen, um die enthaltenen Daten anzuzeigen.

![Klicken Sie auf den Ausgabeport, und wählen Sie „Visualize“ (Visualisieren) aus.][select-visualize]
<br/>
***Klicken Sie auf den Ausgabeport, und wählen Sie „Visualize“ (Visualisieren) aus.***

> [!TIP]
> Datasets und Module verfügen über Ein- und Ausgabe-Anschlüsse, die in Form kleiner Kreise dargestellt werden: Eingabe-Anschlüsse oben und Ausgabe-Anschlüsse unten.
Zum Erstellen eines Datenflusses über Ihr Experiment verbinden Sie einen Ausgabeport eines Moduls mit dem Eingabeport eines anderen Moduls.
Sie können zu einem beliebigen Zeitpunkt auf den Ausgabe-Anschluss eines Datasets oder eines Moduls klicken, um die Daten an diesem Punkt im Datenfluss anzuzeigen.

In diesem Beispieldataset wird jede Instanz eines Autos als Zeile angezeigt, während die den einzelnen Automobilen zugeordneten Variablen als Spalten angezeigt werden. Anhand der Variablen für ein bestimmtes Automobil werden wir versuchen, den Preis in der äußersten rechten Spalte vorherzusagen (Spalte 26 namens „Price“ (Preis)).

![Zeigen Sie die Automobildaten im Fenster für die Datenvisualisierung an.][visualize-auto-data]
<br/>
***Zeigen Sie die Automobildaten im Fenster für die Datenvisualisierung an.***

Schließen Sie das Visualisierungsfenster, indem Sie auf das "**X**" in der oberen rechten Ecke klicken.

## <a name="step-2-prepare-the-data"></a>Schritt 2: Vorbereiten der Daten

DataSets müssen vor der Analyse normalerweise vorverarbeitet werden. Möglicherweise sind Ihnen z.B. bereits die fehlenden Werte in den Spalten verschiedener Zeilen aufgefallen. Damit das Modell die Daten richtig analysieren kann, müssen diese fehlenden Werte bereinigt werden. In unserem Fall entfernen wir alle Zeilen, in denen Werte fehlen. Außerdem enthält die Spalte **normalisierte Verluste** viele fehlende Werte, daher schließen wir diese Spalte komplett aus dem Modell aus.

> [!TIP]
> Die Bereinigung fehlender Werte aus den Eingabedaten ist eine Voraussetzung für die Verwendung der meisten Module.

Zunächst fügen wir ein Modul hinzu, das die Spalte **normalized-losses** (normalisierte Verluste) vollständig entfernt. Anschließend fügen wir ein weiteres Modul ein, das alle Zeilen mit fehlenden Daten entfernt.

1. Geben Sie im oberen Bereich der Modulpalette **select columns** (Spalten auswählen) in das Suchfeld ein, um das Modul [Select Columns in Dataset][select-columns] (Spalten im Dataset auswählen) zu suchen, und ziehen Sie es in den Experimentbereich. Mit diesem Modul können wir auswählen, welche Daten wir in unserem Modell ein- bzw. ausschließen möchten.

2. Verbinden Sie den Ausgabeport des Datasets **Automobile price data (Raw)** mit dem Eingabeport des Moduls [Select Columns in Dataset][select-columns].

    ![Ziehen Sie das Modul „Select Columns in Dataset“ in den Experimentbereich, und verbinden Sie es.][type-select-columns]
    <br/>
    ***Ziehen Sie das Modul „Select Columns in Dataset“ in den Experimentbereich, und verbinden Sie es.***

3. Klicken Sie auf das Modul [Select Columns in Dataset][select-columns] aus, und klicken Sie im Bereich **Eigenschaften** auf **Spaltenauswahl starten**.

    - Klicken Sie auf der linken Seite auf **With rules**
    - Klicken Sie unter **Begin With** (Beginnen mit) auf **All columns** (Alle Spalten). Damit wird [Select Columns in Dataset][select-columns] angewiesen, alle Spalten zu durchlaufen (mit Ausnahme derer, die wir jetzt ausschließen werden).
    - Wählen Sie in den Dropdownlisten die Optionen **Ausschließen** und **Spaltennamen** aus, und klicken Sie auf das Textfeld. Eine Liste von Spalten wird angezeigt. Wählen Sie **normalized-losses** aus. Daraufhin wird die Spalte dem Textfeld hinzugefügt.
    - Klicken Sie auf die Schaltfläche mit einem Häkchen („OK“), um die Spaltenauswahl zu schließen (unten rechts).

    ![Starten Sie die Spaltenauswahl, und schließen Sie die Spalte „normalized-losses“ aus.][launch-column-selector]
    <br/>
    ***Starten Sie die Spaltenauswahl, und schließen Sie die Spalte „normalized-losses“ aus.***

    Der Eigenschaftenbereich für **Select Columns in Dataset** zeigt nun an, dass mit Ausnahme von **normalized-losses** alle Spalten des Datasets übergeben werden.

    ![Der Eigenschaftenbereich zeigt an, dass die Spalte „normalized-losses“ ausgeschlossen wurde.][showing-excluded-column]
    <br/>
    ***Der Eigenschaftenbereich zeigt an, dass die Spalte „normalized-losses“ ausgeschlossen wurde.***

    > [!TIP]
    Sie können einen Kommentar zu einem Modul eingeben, indem Sie auf das Modul doppelklicken und Text eingeben. Auf diese Weise können Sie mit einem Blick sehen, welche Funktion das Modul in Ihrem Experiment erfüllt. Doppelklicken Sie in diesem Fall auf das Modul [Select Columns in Dataset][select-columns], und geben Sie den Kommentar „normalized-losses ausschließen“ ein.
    >
    >


    ![Doppelklicken Sie auf ein Modul, um einen Kommentar hinzuzufügen.][add-comment]
    <br/>
    ***Doppelklicken Sie auf ein Modul, um einen Kommentar hinzuzufügen.***

3. Ziehen Sie das Modul [Clean Missing Data][clean-missing-data] in den Experimentbereich, und verbinden Sie es mit dem Modul [Select Columns in Dataset][select-columns]. Klicken Sie im **Eigenschaftenbereich** unter **Cleaning mode** (Reinigungsmodus) auf die Option **Remove entire row** (Gesamte Zeile entfernen). Dies weist [Clean Missing Data][clean-missing-data] (Fehlende Daten bereinigen) an, Daten durch das Entfernen von Zeilen mit fehlenden Werten zu bereinigen. Doppelklicken Sie auf das Modul, und geben Sie den Kommentar "Remove missing value rows" ein.

    ![Legen Sie den Reinigungsmodus für das Modul „Clean Missing Data“ auf „Remove entire row“ fest.][set-remove-entire-row]
    <br/>
    ***Legen Sie den Reinigungsmodus für das Modul „Clean Missing Data“ auf „Remove entire row“ fest.***

4. Führen Sie das Experiment, indem Sie am unteren Rand der Seite auf **AUSFÜHREN** klicken.

    Nach Abschluss des Experiments sind alle Module mit einem grünen Häkchen markiert, um anzuzeigen, dass diese erfolgreich abgeschlossen wurden. Beachten Sie auch den Status **Finished running** in der oberen rechten Ecke.

![Nach der Ausführung sollte das Experiment in etwa so aussehen.][early-experiment-run]
<br/>
***Nach der Ausführung sollte das Experiment in etwa so aussehen.***

> [!TIP]
> Warum haben wir das Experiment nun ausgeführt? Durch Ausführen des Experiments übergeben die Spaltendefinitionen für unsere Daten aus dem Dataset die Module [Select Columns in Dataset][select-columns] und [Clean Missing Data][clean-missing-data]. Dies bedeutet, dass alle Module, die wir mit [Clean Missing Data][clean-missing-data] verbinden, über dieselben Informationen verfügen.

Bislang haben wir im Experiment nur Daten bereinigt. Wenn Sie das bereinigte Dataset anzeigen möchten, klicken Sie auf den linken Ausgabeport des Moduls [Clean Missing Data][clean-missing-data], und wählen Sie **Visualize** (Visualisieren). Beachten Sie, dass die Spalte **normalized-losses** nicht mehr aufgeführt wird und keine fehlenden Werte auftreten.

Nach der Bereinigung der Daten können wir nun angeben, welche Funktionen wir im Vorhersagemodell verwenden möchten.

## <a name="step-3-define-features"></a>Schritt 3: Definieren von Funktionen

Bei Machine Learning versteht man unter *Funktionen* einzeln messbare Eigenschaften des untersuchten Gesamtobjekts. In unserem DataSet stellt jede Zeile ein Automobil dar, und jede Spalte ist eine Funktion dieses Automobils.

Einen guten Satz von Funktionen für die Erstellung eines Vorhersagemodells finden Sie durch Ausprobieren und Kenntnisse des zu lösenden Problems. Manche Funktionen eignen sich besser für die Vorhersage des Ziels als andere. Darüber hinaus weisen einige Funktionen eine starke Korrelation mit anderen Funktionen auf und können entfernt werden. So stehen „city-mpg“ und „highway-mpg“ z.B. in enger Beziehung. Wir können also eines behalten und das andere ohne bedeutende Auswirkungen auf die Vorhersage entfernen.

Wir werden ein Modell erstellen, das eine Teilmenge der Funktionen in unserem Datensatz verwendet. Sie können später jederzeit andere Funktionen auswählen, das Experiment erneut ausführen und versuchen, bessere Ergebnisse zu erhalten. Hier probieren wir vorerst die folgenden Funktionen aus:

    make, body-style, wheel-base, engine-size, horsepower, peak-rpm, highway-mpg, price


1. Ziehen Sie ein weiteres Modul [Select Columns in Dataset][select-columns] in den Experimentbereich. Verbinden Sie den linken Ausgabeport des Moduls [Clean Missing Data][clean-missing-data] mit dem Eingabeport des Moduls [Select Columns in Dataset][select-columns].

    ![Verbinden Sie das Modul „Select Columns in Dataset“ mit dem Moduls „Clean Missing Data“.][connect-clean-to-select]
    <br/>
    ***Verbinden Sie das Modul „Select Columns in Dataset“ mit dem Modul „Clean Missing Data“.***

2. Doppelklicken Sie auf das Modul, und geben Sie "Select features for prediction" ein.

2. Klicken Sie im Bereich **Eigenschaften** auf **Spaltenauswahl starten**.

3. Klicken Sie auf **With rules**(Mit Regeln).

4. Klicken Sie unter **Begin With** (Beginnen mit) auf **No columns** (Alle Spalten). Wählen Sie in der Filterzeile **Include** (Einbeziehen) und **column names** (Spaltennamen) aus, und wählen Sie im Textfeld unsere Liste mit Spaltennamen aus. Dies weist das Modul an, keine Spalten (Funktionen) außer den angegebenen zu übergeben.

5. Klicken Sie auf das Häkchen ("OK").

    ![Wählen Sie die Spalten (Funktionen) aus, die in die Vorhersage einbezogen werden sollen.][select-columns-to-include]
    <br/>
    ***Wählen Sie die Spalten (Funktionen) aus, die in die Vorhersage einbezogen werden sollen.***

Hierdurch wird ein gefiltertes Dataset erstellt, das nur diejenigen Funktionen enthält, die wir dem im nächsten Schritt verwendeten Lernalgorithmus übergeben möchten. Sie können den Vorgang später jederzeit mit einer anderen Auswahl an Funktionen wiederholen.

## <a name="step-4-choose-and-apply-a-learning-algorithm"></a>Schritt 4: Auswählen und Anwenden eines Lernalgorithmus

Nachdem die Daten vorbereitet sind, können Sie das Vorhersagemodell anhand von Training und Tests erarbeiten. Wir werden das Modell zunächst mit unseren Daten trainieren und dann testen, wie genau seine Preisvorhersagen zutreffen.
<!-- For now, don't worry about *why* we need to train and then test a model.-->

*Klassifizierung* und *Regression* sind zwei Algorithmen für beaufsichtigtes maschinelles Lernen. Mit der Klassifizierung wird eine Antwort aus einem definierten Satz mit Kategorien vorhergesagt, z.B. eine Farbe (Rot, Blau oder Grün). Die Regression wird verwendet, um eine Zahl vorherzusagen.

Da wir einen Preis vorhersagen möchten (also eine Zahl), verwenden wir einen Regressionsalgorithmus. In diesem Beispiel verwenden wir ein einfaches *lineares Regressionsmodell*.

> [!TIP]
> Wenn Sie mehr zu den verschiedenen Algorithmustypen für maschinelles Lernen und dazu erfahren möchten, wann Sie sie verwenden sollten, können Sie sich das erste Video in der Reihe „Data Science für Einsteiger“ ansehen: [Die 5 Fragen, die Data Science beantwortet](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md). Sie können sich auch die Infografik [Grundlagen des maschinellen Lernens mit Algorithmusbeispielen](machine-learning-basics-infographic-with-algorithm-examples.md) oder das [Machine Learning-Cheat Sheet für Algorithmen](machine-learning-algorithm-cheat-sheet.md) ansehen.

Wir trainieren das Modell, indem wir einen Datensatz mit dem Preis einspeisen. Das Modell überprüft die Daten und sucht nach Korrelationen zwischen den Funktionen eines Automobils und seinem Preis. Anschließend testen wir das Modell: Wir geben einen Satz von Funktionen für bekannte Automobile ein und testen, wie exakt das Modell den bekannten Preis vorhersagt.

Wir verwenden unsere Daten sowohl für das Trainieren als auch das Testen des Modells, indem die Daten in separate Trainings- und Testdatasets aufgeteilt werden.

1. Ziehen Sie das Modul [Split Data][split] in den Experimentbereich, und verbinden Sie es mit dem letzten Modul [Select Columns in Dataset][select-columns].

2. Klicken Sie auf das Modul [Split Data][split], um es auszuwählen. Suchen Sie unter **Fraction of rows in the first output dataset** (Bruchteil der Zeilen im ersten Ausgabedataset) (im **Eigenschaftenbereich** rechts neben dem Zeichenbereich), und legen Sie die Einstellung auf 0,75 fest. Damit verwenden wir 75 Prozent der Daten zum Trainieren des Modells und halten 25 Prozent für Tests zurück (Sie können später versuchen, verschiedene Prozentsätze zu verwenden).

    ![Legen Sie den Aufteilungsanteil des Moduls „Split Data“ auf 0,75 fest.][set-split-data-percentage]
    <br/>
    ***Legen Sie den Aufteilungsanteil des Moduls „Split Data“ auf 0,75 fest.***

    > [!TIP]
    > Sie können den Parameter **Zufälliger Ausgangswert** ändern, um unterschiedliche zufällige Proben für Training und Tests zu erstellen. Dieser Parameter steuert den Ausgangswert des Pseudo-Zufallszahlengenerators.

2. Führen Sie das Experiment aus. Wenn das Experiment ausgeführt wird, können die Module [Select Columns in Dataset][select-columns] und [Split Data][split] die Spaltendefinitionen an die Module übergeben, die wir als Nächstes hinzufügen.  

3. Um den Lernalgorithmus auszuwählen, erweitern Sie die Kategorie **Machine Learning** in der Modulpalette links vom Experimentbereich, und erweitern Sie anschließend **Modell initialisieren**. Daraufhin werden verschiedene Kategorien von Modulen angezeigt, die zur Initialisierung eines Algorithmus für maschinelles Lernen verwendet werden können. Wählen Sie für dieses Experiment in der Kategorie **Regression** das Modul [Linear Regression][linear-regression] aus, und ziehen Sie es in den Experimentbereich.
(Sie können das Modul auch suchen, indem Sie „linear regression“ in das Suchfeld der Palette eingeben.)

4. Suchen Sie nach dem Modul [Train Model][train-model], und ziehen Sie es ebenfalls in den Experimentbereich. Verbinden Sie die Ausgabe des Moduls [Linear Regression][linear-regression] mit der linken Eingabe des Moduls [Train Model][train-model], und verbinden Sie die Ausgabe der Trainingsdaten (linker Port) des Moduls [Split Data][split] mit der rechten Eingabe des Moduls [Train Model][train-model].

    ![Verbinden Sie das Modul „Train Model“mit den Modulen „Linear Regression“ und „Split Data“.][connect-train-model]
    <br/>
    ***Verbinden Sie das Modul „Train Model“mit den Modulen „Linear Regression“ und „Split Data“.***

5. Klicken Sie auf das Modul [Train Model][train-model], klicken Sie im Bereich **Eigenschaften** auf **Spaltenauswahl starten**, und wählen Sie dann die Spalte **price** aus. Dies ist der Wert, den unser Modell vorhersagen wird.

    Wählen Sie die Spalte **Price** in der Spaltenauswahl aus, indem Sie sie aus der Liste **Available columns** (Verfügbare Spalten) in die Liste **Selected columns** (Ausgewählte Spalten) verschieben.

    ![Wählen Sie die Spalte „Price“ des Moduls „Train Model“ aus.][select-price-column]
    <br/>
    ***Wählen Sie die Spalte „Price“ des Moduls „Train Model“ aus.***

6. Führen Sie das Experiment aus.

Als Ergebnis erhalten Sie ein trainiertes Regressionsmodell, mit dem Sie neue Automobildaten bewerten können, um Preisvorhersagen zu treffen.

![Nach der Ausführung sollte das Experiment in etwa so aussehen.][second-experiment-run]
<br/>
***Nach der Ausführung sollte das Experiment in etwa so aussehen.***

## <a name="step-5-predict-new-automobile-prices"></a>Schritt 5: Erstellen von Preisprognosen für neue Fahrzeuge

Wir haben das Modell nun unter Verwendung von 75 Prozent unserer Daten trainiert und können die restlichen 25 Prozent der Daten dafür aufwenden, zu bewerten, wie gut unser Modell funktioniert.

1. Suchen Sie nach dem Modul [Score Model][score-model] (Modell bewerten), und ziehen Sie es in den Experimentbereich. Verbinden Sie die Ausgabe des Moduls [Train Model][train-model] mit dem linken Eingabeport des Moduls [Score Model][score-model]. Verbinden Sie den Testdatenausgang (rechter Port) des Moduls [Split Data][split] mit dem rechten Eingabeport des Moduls [Score Model][score-model].

    ![Verbinden Sie das Modul „Score Model“mit den Modulen „Train Model“ und „Split Data“.][connect-score-model]
    <br/>
    ***Verbinden Sie das Modul „Score Model“mit den Modulen „Train Model“ und „Split Data“.***

2. Führen Sie das Experiment aus, und zeigen Sie die Ausgabe des Moduls [Score Model][score-model] an. (Klicken Sie auf den Ausgabeport des Moduls [Score Model][score-model], und wählen Sie **Visualize** (Visualisieren).) Die Ausgabe zeigt die vorhergesagten Preiswerte zusammen mit den bekannten Werten aus den Testdaten an.  

    ![Ausgabe des Moduls „Score Model“][score-model-output]
    <br/>
    ***Ausgabe des Moduls „Score Model“***

3. Schließlich testen wir die Qualität der Ergebnisse. Wählen Sie das Modul [Evaluate Model][evaluate-model] (Modell auswerten) aus, und ziehen Sie es in den Experimentbereich. Verbinden Sie die Ausgabe des Moduls [Score Model][score-model] mit der linken Eingabe des Moduls [Evaluate Model][evaluate-model].

    > [!TIP]
    > Es gibt zwei Eingabeports im Modul [Evaluate Model][evaluate-model], da es für den parallelen Vergleich von zwei Modellen verwendet kann. Später können Sie dem Experiment einen weiteren Algorithmus hinzufügen und mithilfe von [Evaluate Model][evaluate-model] herausfinden, welcher bessere Ergebnisse erzielt.

4. Führen Sie das Experiment aus.

Um die Ausgabe des Moduls [Evaluate Model][evaluate-model] anzuzeigen, klicken Sie auf den Ausgabeport und wählen dann **Visualize** (Visualisieren).

![Auswertungsergebnisse für das Experiment][evaluation-results]
<br/>
***Auswertungsergebnisse für das Experiment***

Die folgenden Statistiken werden für unser Modell angezeigt:

- **Mean Absolute Error** (MAE) – der Mittelwert der absoluten Fehler (ein *Fehler* ist die Differenz zwischen vorhergesagtem und tatsächlichem Wert).
- **Root Mean Squared Error** (RMSE) – die Quadratwurzel des Durchschnitts des Quadrats der Vorhersagefehler für das Test-DataSet.
- **Relative Absolute Error**– der Mittelwert der absoluten Fehler relativ zur absoluten Differenz zwischen tatsächlichen Werten und dem Durchschnitt aller tatsächlichen Werte.
- **Relative Squared Error**– der Durchschnitt der quadrierten Fehler relativ zur quadrierten Differenz zwischen tatsächlichen Werten und dem Durchschnitt aller tatsächlichen Werte.
- **Coefficient of Determination** – dieser auch als **R-Quadrat** bekannte Wert ist eine statistische Kenngröße, die angibt, wie gut ein Modell zu den Daten passt.

Für jede Fehlerstatistik sind kleinere Werte besser. Ein kleinerer Wert gibt an, dass die Vorhersagen genauer mit den tatsächlichen Werten übereinstimmen. Für den **Bestimmungskoeffizienten**gilt: Je näher der Bestimmungskoeffizient am Wert eins (1,0) liegt, desto besser die Vorhersage.

## <a name="final-experiment"></a>Endgültiges Experiment

Das endgültige Experiment sollte in etwa wie folgt aussehen:

![Das endgültige Experiment][complete-linear-regression-experiment]
<br/>
***Das endgültige Experiment***

## <a name="next-steps"></a>Nächste Schritte

Da Sie nun das erste Tutorial zu maschinellem Lernen abgeschlossen und das Experiment eingerichtet haben, können Sie fortfahren, um das Modell zu verbessern und es als Prognosewebdienst bereitzustellen.

- **Versuchen Sie, das Modell anhand von Iterationen zu verbessern:** Sie können z.B. die Funktionen ändern, die Sie für Ihre Vorhersage verwenden. Oder Sie können die Eigenschaften des [Linearen Regressionsalgorithmus][linear-regression] ändern oder einen völlig anderen Algorithmus ausprobieren. Sie können Ihrem Ereignis sogar gleichzeitig mehrere Algorithmen für Machine Learning hinzufügen und jeweils zwei davon vergleichen, indem Sie das Modul [Evaluate Model][evaluate-model] verwenden.
Ein Beispiel für den Vergleich mehrerer Modelle in einem einzigen Experiment finden Sie unter [Compare Regressors (Vergleichen von Regressoren)](https://gallery.cortanaintelligence.com/Experiment/Compare-Regressors-5) in der [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com).

    > [!TIP]
    > Mit der Schaltfläche **SPEICHERN UNTER** am unteren Rand der Seite können Sie Kopien der Iterationen Ihres Experiments speichern. Sie können alle Iterationen Ihres Experiments anzeigen, indem Sie am unteren Rand der Seite auf **AUSFÜHRUNGSVERLAUF ANZEIGEN** klicken. Weitere Informationen finden Sie unter [Verwalten von Experimentiterationen in Azure Machine Learning-Studio][runhistory].

[runhistory]: machine-learning-manage-experiment-iterations.md

- **Bereitstellen des Modells als Prognosewebdienst:** Wenn Sie mit Ihrem Modell zufrieden sind, können Sie es als Webdienst bereitstellen, der Automobilpreise anhand neuer Daten vorhersagt. Ausführlichere Informationen finden Sie unter [Bereitstellen eines Azure Machine Learning-Webdiensts][publish].

[publish]: machine-learning-publish-a-machine-learning-web-service.md

Sie möchten mehr erfahren? Eine umfassendere und ausführlichere exemplarische Vorgehensweise für das Erstellen, Trainieren, Bewerten und Bereitstellen eines Modells finden Sie unter [Entwickeln einer Lösung zur Vorhersageanalyse in Azure Machine Learning][walkthrough].

[walkthrough]: machine-learning-walkthrough-develop-predictive-solution.md

<!-- Images -->
[sign-in-to-studio]: ./media/machine-learning-create-experiment/sign-in-to-studio.png
[rename-experiment]: ./media/machine-learning-create-experiment/rename-experiment.png
[visualize-auto-data]:./media/machine-learning-create-experiment/visualize-auto-data.png
[select-visualize]: ./media/machine-learning-create-experiment/select-visualize.png
[showing-excluded-column]:./media/machine-learning-create-experiment/showing-excluded-column.png
[set-remove-entire-row]:./media/machine-learning-create-experiment/set-remove-entire-row.png
[early-experiment-run]:./media/machine-learning-create-experiment/early-experiment-run.png
[select-columns-to-include]:./media/machine-learning-create-experiment/select-columns-to-include.png
[second-experiment-run]:./media/machine-learning-create-experiment/second-experiment-run.png
[connect-score-model]:./media/machine-learning-create-experiment/connect-score-model.png
[evaluation-results]:./media/machine-learning-create-experiment/evaluation-results.png
[complete-linear-regression-experiment]:./media/machine-learning-create-experiment/complete-linear-regression-experiment.png

<!-- temporarily switching GIFs to PNGs to remove animation --> 
[type-automobile]:./media/machine-learning-create-experiment/type-automobile.png
[type-select-columns]:./media/machine-learning-create-experiment/type-select-columns.png
[launch-column-selector]:./media/machine-learning-create-experiment/launch-column-selector.png
[add-comment]:./media/machine-learning-create-experiment/add-comment.png
[connect-clean-to-select]:./media/machine-learning-create-experiment/connect-clean-to-select.png

[set-split-data-percentage]:./media/machine-learning-create-experiment/set-split-data-percentage.png

<!-- temporarily switching GIFs to PNGs to remove animation --> 
[connect-train-model]:./media/machine-learning-create-experiment/connect-train-model.png
[select-price-column]:./media/machine-learning-create-experiment/select-price-column.png

[score-model-output]:./media/machine-learning-create-experiment/score-model-output.png

<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/

