<properties
	pageTitle="Azure Machine Learning – häufig gestellte Fragen (FAQ) | Microsoft Azure"
	description="Einführung in Azure Machine Learning: häufig gestellte Fragen (FAQ) zu Abrechnung, Funktionen und Einschränkungen von Clouddiensten für die optimierte Vorhersagemodellierung."
	keywords="Einführung in maschinelles Lernen,Vorhersagemodellierung,was ist maschinelles Lernen"
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
	ms.topic="get-started-article"
	ms.date="07/14/2016"
	ms.author="garye"/>

# Häufig gestellte Fragen zu Azure Machine Learning (FAQ): Abrechnung, Funktionen, Einschränkungen und Support

In diesem Dokument werden häufig gestellte Fragen zu Azure Machine Learning beantwortet. Dabei handelt es sich um einen Clouddienst zum Entwickeln von Vorhersagemodellen und Lösungen zur Operationalisierung über Webdienste. Dazu gehören Fragen zur Verwendung des Diensts, einschließlich Abrechnungsmodell, Funktionen, Einschränkungen und Unterstützung.

## Allgemeine Fragen

**Was ist Azure Machine Learning?**

Azure Machine Learning ist ein vollständig verwalteter Dienst, mit dem Sie Lösungen zu Vorhersageanalyse in der Cloud erstellen, testen, betreiben und verwalten können. Mit einem Browser können Sie sich anmelden, Daten hochladen und sofort mit Experimenten im Bereich Machine Learning beginnen. Dank der Vorhersagemodellierung per Drag & Drop, einer umfangreichen Modulpalette und einer Bibliothek mit Ausgangsvorlagen können Sie gängige Aufgaben im Bereich Machine Learning schnell und einfach ausführen. Weitere Informationen finden Sie auf der Übersichtsseite zu [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/). Eine Einführung in Machine Learning mit wichtigen Begriffen und Konzepten finden Sie unter [Einführung in Azure Machine Learning](machine-learning-what-is-machine-learning.md).

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**Was ist Machine Learning Studio?**

Machine Learning Studio ist eine Workbench-Umgebung, auf die Sie über einen Webbrowser zugreifen. Machine Learning Studio enthält eine Palette von Modulen mit einer visuellen Kompositionsoberfläche, in der Sie durchgängige Datenworkflows in Form von Experimenten erstellen können.

Weitere Informationen zu Machine Learning Studio finden Sie unter [Was ist Machine Learning Studio?](machine-learning-what-is-ml-studio.md).

**Was ist der Machine Learning-API-Dienst?**

Mit dem Machine Learning-API-Dienst können Sie Ihre Vorhersagemodelle, z.B. in Machine Learning Studio erstellte Modelle, als skalierbare, fehlertolerante Webdienste bereitstellen. Die vom Machine Learning-API-Dienst erstellten Webdienste sind REST-APIs. Sie bieten eine Schnittstelle für die Kommunikation zwischen externen Anwendungen und Ihren Predictive Analytics-Modellen.

Weitere Informationen finden Sie unter [Verbinden mit einem Machine Learning-Webdienst](machine-learning-connect-to-azure-machine-learning-web-service.md).

**Wo sind meine klassischen Webdienste aufgeführt? Wo sind meine neuen Azure Resource Manager-basierten Webdienste aufgeführt?**

Klassische Webdienste finden Sie in [Machine Learning Studio](http://studio.azureml.net) auf der Registerkarte „Webdienste“. Neue Azure Resource Manager-basierte Webdienste befinden sich im [Microsoft Azure Machine Learning-Webdienste](https://services.azureml.net/)-Portal. Es ist keine übergreifende Auflistung vorhanden.

## Fragen zu Microsoft Azure Machine Learning-Webdiensten

**Was sind Azure ML-Webdienste?**

Mit dem Azure Machine Learning-Webdienst kommunizieren externe Anwendungen in Echtzeit mit einem Machine Learning-Workflow-Bewertungsmodell. Ein Machine Learning-Webdienstaufruf gibt Vorhersageergebnisse an eine externe Anwendung zurück. Zur Durchführung eines Machine Learning-Webdienstaufrufs übergeben Sie einen API-Schlüssel, der beim Bereitstellen des Webdiensts erstellt wurde. Der Machine Learning-Webdienst basiert auf REST, einer verbreiteten Architektur für Webprogrammierungsprojekte.

Azure Machine Learning verfügt über zwei Arten von Diensten:

* Antwort-/Anfrage-Dienst (Request-Response Service, RRS): ein hochskalierbarer Webdienst mit niedriger Latenz, der eine Schnittstelle für zustandslose Modelle bereitstellt, die in Machine Learning Studio erstellt und bereitgestellt wurden.
* Batch Execution Service (BES): ein asynchroner Dienst für die Bewertung eines Stapels für Datensätze.

Es gibt mehrere Möglichkeiten, die REST-API zu nutzen und auf den Webdienst zuzugreifen. Beispielsweise können Sie eine Anwendung in C#, R oder Python schreiben und dabei den Beispielcode verwenden, der für Sie beim Bereitstellen des Webdiensts generiert wurde.

Der Beispielcode ist hier verfügbar: auf der Seite „Consume“ für den Webdienst im Azure Machine Learning-Webdienste-Portal und auf der API-Hilfeseite im Webdienstdashboard in Machine Learning Studio.

Sie können aber auch die Microsoft Excel-Beispielarbeitsmappe verwenden, die für Sie erstellt wurde (auch im Webdienstdashboard in Studio verfügbar).

**Was sind die wichtigsten Updates für die neuen Azure ML-Webdienste?**

Weitere Informationen zu den neuen Azure Machine Learning-Webdiensten finden Sie in der [zugehörigen Dokumentation](machine-learning-whats-new.md).

## Fragen zu Machine Learning Studio

### Erstellen von Experimenten

**Gibt es Versionskontrolle oder eine Git-Integration für Experimentdiagramme?**

Nein. In Machine Learning Studio wird aber jede Experimentiteration beibehalten und kann von anderen Benutzern nicht geändert werden. Weitere Informationen finden Sie unter [Verwalten von Experimentiterationen in Machine Learning-Studio](machine-learning-manage-experiment-iterations.md).


### Bereitstellen eines Experiments

**Kann ich ein Vorhersageexperiment als neuen (Azure Resource Manager-basierten) Webdienst bereitstellen, wenn ich ihn bereits als klassischen Webdienst bereitgestellt habe?**

Nein. Sie können kein Experiment bereitstellen, das zuvor als klassischer Webdienst bereitgestellt wurde. Sie müssen ein neues Vorhersageexperiment erstellen und stattdessen bereitstellen.


### Importieren und Exportieren von Daten für Machine Learning

**Welche Datenquellen unterstützt Machine Learning?**

Daten können auf drei Arten in ein Machine Learning Studio-Experiment geladen werden: durch das Hochladen einer lokalen Datei als Dataset, durch das Verwenden eines Moduls zum Importieren von Daten aus Clouddatendiensten oder durch das Importieren eines Datasets, das für ein anderes Experiment gespeichert wurde. Weitere Informationen zu den unterstützten Dateiformaten finden Sie unter [Importieren von Trainingsdaten in Machine Learning Studio](machine-learning-data-science-import-data.md).


#### <a id="ModuleLimit"></a>Wie groß können DataSets für meine Module sein?

Module in Machine Learning Studio unterstützen in normalen Anwendungsfällen DataSets bis zu einer Größe von 10 GB an dichten numerischen Daten. Wenn ein Modul mehrere Eingaben akzeptiert, entsprechen 10 GB der Summe aller Eingabegrößen. Sie können über Hive- oder Azure SQL-Datenbank-Abfragen oder durch Vorverarbeitung per Lernen nach Anzahl auch Teile größerer DataSets übernehmen.

Die folgenden Typen von Daten können während der Featurenormalisierung in größere DataSets erweitert werden und sind auf weniger als 10 GB beschränkt:

- Platzsparend
- Kategorisch
- Zeichenfolgen
- Binärdaten

Die folgenden Bereiche sind auf DataSets mit einer Größe unter 10 GB beschränkt:

- Empfohlene Module
- SMOTE-Modul
- Skripting-Module: R, Python, SQL
- Module, bei denen die Größe der Ausgabedaten die der Eingabedaten überschreiten kann, z. B. Join oder Feature-Hashing.
- Kreuzvalidierung, Tune Model Hyperparameters, Ordinal Regression und One-vs-All Multiclass, wenn eine sehr große Anzahl von Iterationen durchgeführt wird.

Für DataSets größer als einige GB sollten Sie die Daten in Azure Storage oder Azure SQL-Datenbank laden oder HDInsight verwenden, anstatt die Daten direkt aus lokalen Dateien hochzuladen.


####<a id="UploadLimit"></a>Was sind die Limits für Datenuploads?
Laden Sie für DataSets größer als einige GB die Daten in Azure Storage oder Azure SQL-Datenbank, oder verwenden Sie HDInsight, anstatt die Daten direkt aus lokalen Dateien hochzuladen.

**Können Daten von Amazon S3 gelesen werden?**

Wenn Ihre Daten nicht sehr umfangreich sind und Sie diese Daten über eine HTTP-URL verfügbar machen möchten, können Sie das Modul zum [Importieren von Daten][import-data] verwenden. Größere Datenmengen sollten Sie zunächst in Azure Storage übertragen und anschließend mit dem Modul zum [Importieren von Daten][import-data] in das Experiment übernehmen.
<!--
<SEE CLOUD DS PROCESS>
-->

**Gibt es eine integrierte Bildeingabefunktion?**

Informationen zur Bildeingabefunktion finden Sie in der Referenz unter [Importieren von Bildern][image-reader].

### Module

**Der gewünschte Algorithmus, das Datenformat oder die Datentransformation ist nicht in Azure Machine Learning Studio enthalten. Welche Optionen habe ich?**

Sie können das [Forum für Benutzerfeedback](http://go.microsoft.com/fwlink/?LinkId=404231) besuchen, um herauszufinden, welche Featureanfragen wir momentan verfolgen. Stimmen Sie für eine entsprechende Anfrage ab, wenn ein von Ihnen gewünschtes Feature bereits angefordert wurde. Falls das gewünschte Feature nicht vorhanden ist, können Sie eine neue Anfrage erstellen. Sie können den Status Ihrer Anfrage ebenfalls in diesem Forum verfolgen. Wir verfolgen diese Liste regelmäßig und aktualisieren den Verfügbarkeitsstatus von Features häufig. Mit der integrierten Unterstützung für R und Python können außerdem benutzerdefinierte Transformationen nach Bedarf erstellt werden.


**Kann ich meinen vorhandenen Code in Machine Learning Studio verwenden?**

Ja. Sie können Ihren vorhandenen R- oder Python-Code in Machine Learning Studio importieren, zusammen im gleichen Experiment mit den Lernmodulen in Azure Machine Learning ausführen und die Lösung als Webdienst über Azure Machine Learning bereitstellen. Weitere Informationen finden Sie unter [Erweitern Ihres Experiments mit R](machine-learning-extend-your-experiment-with-r.md) und [Ausführen von Python-Machine Learning-Skripts in Azure Machine Learning Studio](machine-learning-execute-python-scripts.md).

**Ist es möglich, z. B. mit [PMML](http://en.wikipedia.org/wiki/Predictive_Model_Markup_Language) ein Modell zu definieren?**

Nein, dies wird nicht unterstützt. Benutzerdefinierter R- und Python-Code kann jedoch zum Definieren eines Moduls verwendet werden.

**Wie viele Module kann ich parallel in meinem Experiment ausführen?**

Sie können bis zu vier Module in einem Experiment parallel ausführen.


### Datenverarbeitung

**Gibt es eine Möglichkeit zum interaktiven Anzeigen von Daten (neben R-Visualisierungen) im Experiment?**

Durch Klicken auf die Ausgabe eines Moduls können Sie die Daten visualisieren und Statistiken abrufen.

**Bei der Vorschau von Ergebnissen oder Daten im Browser ist die Anzahl der Zeilen und Spalten beschränkt. Warum?**

Da die an den Browser übertragenen Daten umfangreich sein können, ist die Datengröße beschränkt, damit Machine Learning Studio nicht verlangsamt wird. Es ist besser, alle Daten herunterzuladen und Excel oder ein anderes Tool zu verwenden, um die gesamten Daten bzw. Ergebnisse zu visualisieren.

### Algorithmen

**Welche vorhandenen Algorithmen werden in Machine Learning Studio unterstützt?**

Machine Learning Studio unterstützt moderne Algorithmen, z.B. skalierbare Boosted Decision-Strukturen, Bayessche Empfehlungssysteme, tiefe neuronale Netze und die von Microsoft Research entwickelten „Entscheidungsdschungel“. Skalierbare Open Source-Pakete für Machine Learning wie z. B. Vowpal Wabbit sind ebenfalls enthalten. Machine Learning Studio unterstützt Algorithmen für Machine Learning für mehrklassige und binäre Klassifizierung, Regression und Clustering. Weitere Informationen finden Sie in der vollständigen Liste der [Machine Learning-Module][machine-learning-modules].

**Wird automatisch der richtige Machine Learning-Algorithmus für meine Daten vorgeschlagen?**

Nein. Es gibt aber in Machine Learning Studio verschiedene Möglichkeiten zum Vergleichen der Ergebnisse jedes Algorithmus, um die richtige Wahl für das Problem zu treffen.

**Gibt es Richtlinien zum Auswählen eines Algorithmus für die bereitgestellten Algorithmen?** Informationen hierzu finden Sie unter [Auswählen eines Algorithmus](machine-learning-algorithm-choice.md).

**Wurden die bereitgestellten Algorithmen in R oder Python geschrieben?**

Diese Algorithmen wurden größtenteils in kompilierten Sprachen geschrieben, um eine höhere Leistung zu erzielen.

**Gibt es nähere Informationen zu den Algorithmen?**

Die Dokumentation enthält Informationen zu den Algorithmen. Darin werden auch die Parameter zur Feinabstimmung beschrieben, damit Sie den Algorithmus für Ihre Zwecke optimieren können.

**Gibt es Unterstützung für das Onlinelernen?**

Nein, derzeit wird nur programmgesteuertes erneutes Trainieren unterstützt.

**Können die Ebenen eines Modells für ein neuronales Netz mit dem integrierten Modul visualisiert werden?**

Nr.

**Kann ich eigene Module in C# oder einer anderen Sprache erstellen?**

Derzeit können neue benutzerdefinierte Module nur in R erstellt werden.

### R-Modul

**Welche R-Pakete sind in Machine Learning Studio verfügbar?**

Machine Learning Studio unterstützt bereits über 400 CRAN R-Pakete. [Hier finden Sie die aktuelle Liste](http://az754797.vo.msecnd.net/docs/RPackages.xlsx) aller enthaltenen Pakete. Lesen Sie auch die Anweisungen zum Abrufen dieser Liste unter [Erweitern des Experiments mit R](machine-learning-extend-your-experiment-with-r.md). Falls das gewünschte Paket nicht in der Liste enthalten ist, können Sie den Namen des Pakets im [Forum für Benutzer-Feedback](http://go.microsoft.com/fwlink/?LinkId=404231) hinterlassen.

**Ist es möglich, benutzerdefinierte R-Module zu erstellen?**

Ja. Weitere Informationen finden Sie unter [Erstellen von benutzerdefinierten R-Modulen in Azure Machine Learning](machine-learning-custom-r-modules.md).

**Gibt es eine REPL-Umgebung für R?**

Nein, es gibt keine REPL-Umgebung für R in Machine Learning Studio.

### Python-Modul

**Ist es möglich, benutzerdefinierte Python-Module zu erstellen?**

Derzeit nicht. Sie können aber ein oder mehrere Module vom Typ [Python-Skript ausführen][python] verwenden, um das gleiche Ergebnis zu erzielen.

**Gibt es eine REPL-Umgebung für Python?**

Sie können die „Jupyter Notebooks“ in Machine Learning Studio verwenden. Weitere Informationen finden Sie unter [Introducing Jupyter Notebooks in Azure ML Studio](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx) (Einführung in Jupyter Notebooks in Azure Machine Learning Studio).

## Webdienst

###Programmgesteuertes erneutes Trainieren von Modellen

**Wie kann ich Azure Machine Learning-Modelle programmgesteuert neu trainieren?**

Verwenden Sie die APIs für das erneute Trainieren. Weitere Informationen finden Sie unter [Programmgesteuertes erneutes Trainieren von Machine Learning-Modellen](machine-learning-retrain-models-programmatically.md). Außerdem ist Beispielcode unter [Microsoft Azure Machine Learning Retraining Demo](https://azuremlretrain.codeplex.com/) (Microsoft Azure Machine Learning – Demo für erneutes Trainieren) verfügbar.

### Erstellen

**Kann ich das Modell lokal oder in einer Anwendung ohne Internetverbindung bereitstellen?**

Nr.


**Gibt es eine Grundlatenz, die für alle Webdienste erwartet wird?**

Informationen hierzu finden Sie unter [Einschränkungen für Azure-Abonnements](../azure-subscription-service-limits.md).

### Verwenden Sie

**Wann sollte ich mein Vorhersagemodell als Stapelausführungsdienst oder als Anfrage-Antwort-Dienst ausführen?**

Der Anfrage-Antwort-Dienst (Request Response Service, RRS) ist ein hoch skalierbarer Webdienst mit niedriger Latenz, der eine Schnittstelle für zustandslose Modelle bereitstellt, die in der Experimentumgebung erstellt und bereitgestellt wurden. Der Stapelausführungsdienst (Batch Execution Service, BES) dient zur asynchronen Bewertung eines Stapels von Datensätzen. Die Eingaben für RRS und BES sind einander sehr ähnlich. BES liest im Gegensatz zu RRS einen Block von Einträgen aus einer Vielzahl von Quellen wie z. B. dem Blobdienst und dem Tabellenspeicherdienst in Azure, Azure SQL-Datenbank, HDInsight (Hive-Abfrage) und HTTP-Quellen. Weitere Informationen finden Sie unter [Verwenden von Machine Learning-Webdiensten](machine-learning-consume-web-services.md).

**Wie aktualisiere ich das Modell für den bereitgestellten Webdienst?**

Sie können das Vorhersagemodell für einen bereitgestellten Dienst einfach ändern, indem Sie das Experiment bearbeiten und erneut ausführen, das Sie beim Erstellen und Speichern des trainierten Modells verwendet haben. Sobald die neue Version des trainierten Modells verfügbar ist, werden Sie von Machine Learning Studio gefragt, ob Sie Ihren Webdienst aktualisieren möchten. Ausführliche Informationen zum Aktualisieren eines bereitgestellten Webdiensts finden Sie unter [Bereitstellen von Machine Learning-Webdiensten](machine-learning-publish-a-machine-learning-web-service.md).

Sie können auch die APIs zum erneuten Trainieren verwenden. Weitere Informationen finden Sie unter [Programmgesteuertes erneutes Trainieren von Machine Learning-Modellen](machine-learning-retrain-models-programmatically.md). Außerdem ist Beispielcode unter [Microsoft Azure Machine Learning Retraining Demo](https://azuremlretrain.codeplex.com/) (Microsoft Azure Machine Learning – Demo für erneutes Trainieren) verfügbar.

**Wie überwache ich meinen Webdienst in der Produktionsumgebung?**

Sobald ein Vorhersagemodell bereitgestellt wurde, können Sie es im klassischen Azure-Portal überwachen. Jeder bereitgestellte Dienst hat ein eigenes Dashboard mit Überwachungsinformationen für den jeweiligen Dienst. Weitere Informationen zum Verwalten der bereitgestellten Webdienste finden Sie unter [Verwalten eines Azure Machine Learning-Arbeitsbereichs](machine-learning-manage-workspace.md).

**Kann ich die Ausgabe meines RRS/BES an einer Stelle einsehen?**

Für RRS wird das Ergebnis normalerweise in der Antwort des Webdiensts ausgegeben. Sie können es auch in den Azure-Blobspeicher schreiben. Bei BES wird die Ausgabe standardmäßig in ein Blob geschrieben. Sie können die Ausgabe außerdem mit dem Modul zum [Exportieren von Daten][export-data] in eine Datenbank oder Tabelle schreiben.

**Können Webdienste nur aus Modellen erstellt werden, die in Machine Learning Studio erstellt wurden?**

Nein. Webdienste können auch direkt aus Jupyter Notebooks und RStudio erstellt werden.

**Wo finde ich Informationen zu Fehlercodes?**

 ** Wo finde ich Informationen zu Fehlercodes? Fehlercodes werden [hier](https://msdn.microsoft.com/library/azure/dn905910.aspx) beschrieben.

## Skalierbarkeit

**Wie skalierbar ist der Webdienst?**

Derzeit wird der Standardendpunkt mit 20 gleichzeitigen RRS-Anforderungen pro Endpunkt bereitgestellt. Sie können dies auf 200 gleichzeitige Anforderungen pro Endpunkt skalieren, und jeder Webdienst kann auf 10.000 Endpunkte pro Webdienst skaliert werden, wie unter [Skalieren von API-Endpunkten](machine-learning-scaling-endpoints.md) beschrieben. Bei BES lässt jeder Endpunkt die gleichzeitige Verarbeitung von 40 Anforderungen zu, und weitere Anforderungen werden in eine Warteschlange eingereiht. Diese Anforderungen in der Warteschlange werden automatisch ausgeführt, wenn die Warteschlange geleert wird.


**Werden R-Aufträge auf die Knoten verteilt?**

Nein.


**Wie viele Daten kann ich für das Training verwenden?**

Module in Machine Learning Studio unterstützen in normalen Anwendungsfällen DataSets bis zu einer Größe von 10 GB an dichten numerischen Daten. Wenn für ein Modul mehr als eine Eingabe verwendet wird, beträgt die Gesamtgröße für alle Eingaben zusammen 10 GB. Sie können über Hive- oder Azure SQL-Datenbank-Abfragen oder per Vorverarbeitung durch Module vom Typ „Lernen nach Anzahl“ ([Datentransformation/Lernen mit Zahlen][counts]) auch Teile größerer Datasets übernehmen.

Die folgenden Typen von Daten können während der Featurenormalisierung in größere DataSets erweitert werden und sind auf weniger als 10 GB beschränkt:

- Mit geringer Dichte
- Kategorisch
- Zeichenfolgen
- Binärdaten

Die folgenden Module sind auf Datasets mit einer Größe von unter 10 GB beschränkt:

- Empfohlene Module
- SMOTE-Modul
- Skripting-Module: R, Python, SQL
- Module, bei denen die Größe der Ausgabedaten die der Eingabedaten überschreiten kann, z. B. Join oder Feature-Hashing.
- Cross-Validate, Tune Model Hyperparameters, Ordinal Regression und One-vs-All Multiclass, wenn eine sehr große Anzahl von Iterationen durchgeführt wird.

Für Datasets, die größer als einige GB sind, sollten Sie die Daten in Azure Storage oder Azure SQL-Datenbank laden oder HDInsight verwenden, anstatt die Daten direkt aus einer lokalen Datei hochzuladen.


**Gibt es Vektorgrößenbeschränkungen?**

Zeilen und Spalten sind jeweils auf die .NET-Einschränkung für Ganzzahlen beschränkt: 2.147.483.647.

**Kann die Größe des virtuellen Computers, der zum Ausführen des Webdiensts verwendet wird, angepasst werden?**

Nein.

## Sicherheit und Verfügbarkeit

**Wer hat standardmäßig Zugriff auf den HTTP-Endpunkt für den Webdienst? Wie kann ich den Zugriff auf den Endpunkt einschränken?**

Nach der Bereitstellung eines Webdiensts wird ein Standardendpunkt für den Dienst erstellt. Der Standardendpunkt kann über seinen API-Schlüssel aufgerufen werden. Zusätzliche Endpunkte können im klassischen Azure-Portal oder programmgesteuert mithilfe der APIs der Webdienstverwaltung unter Verwendung ihrer eigenen Schlüssel hinzugefügt werden. Zugriffsschlüssel sind erforderlich, um Aufrufe für den Webdienst durchzuführen. Weitere Informationen finden Sie unter [Verbinden mit einem Machine Learning-Webdienst](machine-learning-connect-to-azure-machine-learning-web-service.md).


**Was passiert, wenn mein Azure-Speicherkonto nicht gefunden werden kann?**

In Machine Learning Studio ist ein vom Benutzer bereitgestelltes Azure-Speicherkonto zum Speichern von temporären Daten beim Ausführen des Workflows erforderlich. Dieses Speicherkonto wird für Machine Learning Studio beim Erstellen des Arbeitsbereichs zur Verfügung gestellt. Wenn das Speicherkonto nach dem Erstellen des Arbeitsbereichs gelöscht wird, funktioniert der Arbeitsbereich nicht mehr, und alle darin enthaltenen Experimente schlagen fehl.

Wenn Sie das Speicherkonto versehentlich löschen, können Sie es mit identischem Namen und in derselben Region neu erstellen. Führen Sie anschließend die erneute Synchronisierung des Zugriffsschlüssels durch.


**Was geschieht, wenn mein Speicherkonto-Zugriffsschlüssel nicht mehr synchronisiert ist?**

In Machine Learning Studio ist ein vom Benutzer bereitgestelltes Azure-Speicherkonto zum Speichern von temporären Daten beim Ausführen des Workflows erforderlich. Dieses Speicherkonto wird für Machine Learning Studio beim Erstellen des Arbeitsbereichs zur Verfügung gestellt, und die Zugriffsschlüssel werden diesem Arbeitsbereich zugewiesen. Wenn Zugriffsschlüssel nach dem Erstellen des Arbeitsbereichs geändert werden, kann dieser Arbeitsbereich nicht mehr auf das Speicherkonto zugreifen. Er funktioniert nicht mehr, und alle in diesem Arbeitsbereich enthaltenen Experimente schlagen fehl.

Wenn Sie Zugriffsschlüssel für Speicherkonten geändert haben, müssen Sie die Zugriffsschlüssel im Arbeitsbereich über das klassische Azure-Portal neu synchronisieren.


## Azure Marketplace

Weitere Informationen finden Sie unter [FAQ zum Veröffentlichen und Verwenden von Apps im Machine Learning Marketplace](machine-learning-marketplace-faq.md).

## Support und Lernmaterial

**Wo erhalte ich Lernmaterial für Azure Machine Learning?**

Im [Azure Machine Learning Documentation Center](https://azure.microsoft.com/services/machine-learning/) finden Sie Videoanleitungen und Leitfäden. Diese ausführlichen Leitfäden enthalten eine Einführung in die Dienste und beschreiben den Lebenszyklus der Daten vom Import und der Bereinigung der Daten bis hin zur Erstellung von Vorhersagemodellen und der Bereitstellung der Modelle in der Produktionsumgebung mit Azure Machine Learning.

Wir fügen dem Machine Learning Center fortlaufend neues Material hinzu. Sie können jederzeit zusätzliches Material für das Machine Learning Center im [Forum für Benutzerfeedback](https://windowsazure.uservoice.com/forums/257792-machine-learning) anfordern.

Es werden auch Schulungen unter [Microsoft Virtual Academy](http://www.microsoftvirtualacademy.com/training-courses/getting-started-with-microsoft-azure-machine-learning) angeboten.

**Wo erhalte ich Support für Azure Machine Learning?**

Technischen Support für Azure Machine Learning erhalten Sie, indem Sie den [Azure-Support](/support/options/) besuchen und **Machine Learning** auswählen.

Für Azure Machine Learning gibt es außerdem ein Community-Forum auf MSDN, in dem Sie Fragen zu Azure Machine Learning stellen können. Das Forum wird vom Azure Machine Learning-Team moderiert. [Azure Forum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) besuchen.

## Fragen zur Abrechnung

**Wie funktioniert die Abrechnung von Machine Learning?**

Der Azure Machine Learning-Dienst umfasst zwei Komponenten: Machine Learning Studio und Machine Learning-Webdienste.

Zum Evaluieren von Machine Learning Studio können Sie den kostenlosen Free-Tarif nutzen. Im Free-Tarif können Sie auch einen klassischen Webdienst mit begrenzter Kapazität bereitstellen.

Wenn Azure Machine Learning Ihre Anforderungen erfüllt, können Sie sich für den Standard-Tarif registrieren. Für die Registrierung müssen Sie über ein Microsoft Azure-Abonnement verfügen.

Im Standard-Tarif wird Ihnen jeder Arbeitsbereich, den Sie in Machine Learning Studio definieren, monatlich berechnet. Wenn Sie in Studio ein Experiment ausführen, werden Ihnen die Computeressourcen berechnet, die beim Ausführen des Experiments genutzt werden. Beim Bereitstellen eines klassischen Webdiensts fallen Kosten für Transaktionen und Computestunden (auch als „Berechnungsstunden“ bezeichnet) basierend auf der nutzungsbasierten Abrechnung an.

Mit den neuen Machine Learning-Webdiensten werden Abrechnungspläne eingeführt, mit denen die Kosten besser vorhergesagt werden können. Im Rahmen der gestaffelten Preise werden für Kunden, die eine große Menge an Kapazität benötigen, Rabatte angeboten.

Wenn Sie einen Plan erstellen, akzeptieren Sie feste Kosten, die mit einer enthaltenen Menge von API-Computestunden und API-Transaktionen verbunden sind. Falls Sie höhere enthaltene Mengen benötigen, können Sie Ihrem Plan zusätzliche Instanzen hinzufügen. Wenn es um höhere Mengen geht, können Sie einen Plan mit höherem Tarif wählen, der über deutlich höhere enthaltene Mengen und einen besseren Rabatt verfügt.

Nachdem die enthaltenen Mengen in den vorhandenen Instanzen aufgebraucht sind, wird für die weitere Nutzung die Überschreitungsrate des Abrechnungsplantarifs berechnet.

Hinweis: Die enthaltenen Mengen werden alle 30 Tage neu zugeordnet, und nicht genutzte Mengen werden nicht auf den nächsten Zeitraum übertragen.

Weitere Informationen zur Abrechnung und zu Preisen finden Sie unter [Machine Learning Preise](https://azure.microsoft.com/pricing/details/machine-learning/).

**Gibt es eine kostenlose Testversion von Machine Learning?**

 Azure Machine Learning verfügt über die Abonnementoption „Free“ (Details unter [Machine Learning – Preise](https://azure.microsoft.com/pricing/details/machine-learning/)), und für Machine Learning Studio wird eine acht Stunden gültige Testversion für die Schnellevaluierung angeboten (melden Sie sich hierzu bei [Machine Learning Studio](https://studio.azureml.net/?selectAccess=true&o=2) an).
 
 Wenn Sie sich für die kostenlose Azure-Testversion anmelden, können Sie außerdem die Azure-Dienste einen Monat lang ausprobieren. Weitere Informationen zur kostenlosen Azure-Testversion finden Sie in den [häufig gestellten Fragen zur kostenlosen Testversion von Azure](/pricing/free-trial-faq/).

**Was ist eine Transaktion?**

Eine Transaktion stellt einen API-Aufruf dar, auf den Azure Machine Learning reagiert. Transaktionen der Aufrufe vom Typ Anforderung/Antwort-Dienst (Request-Response Service, RRS) und Stapelausführungsdienst (Batch Execution Service, BES) werden aggregiert und basierend auf Ihrem Abrechnungsplan berechnet.

**Kann ich die enthaltenen Transaktionsmengen in einem Plan sowohl für RRS-Transaktionen als auch für BES-Transaktionen verwenden?**

Ja. Ihre Transaktionen für RRS und BES werden aggregiert und basierend auf Ihrem Abrechnungsplan berechnet.

**Was ist eine API-Computestunde?**

Eine API-Computestunde ist die Abrechnungseinheit für die Dauer von API-Aufrufen mit Verwendung von ML-Computeressourcen. Alle Aufrufe werden zu Abrechnungszwecken aggregiert (also zusammengefasst).

**Wie lange dauert ein typischer Produktions-API-Aufruf?**

Die Produktions-API-Aufrufzeiten können erheblich variieren und reichen im Allgemeinen von Hundertstel Millisekunden bis zu einigen Sekunden, können aber auch mehrere Minuten betragen. Dies richtet sich jeweils nach der Komplexität der Datenverarbeitung und des Machine Learning-Modells. Die beste Möglichkeit zur Einschätzung der Produktions-API-Aufrufzeiten besteht darin, basierend auf dem Machine Learning-Dienst einen Benchmark für ein Modell zu erstellen.

**Was ist eine Studio-Computestunde?**

Eine Studio-Computestunde ist die Abrechnungseinheit für den gesamten Zeitraum, in dem für Experimente Computeressourcen in Studio genutzt werden.

**Welche Bedeutung hat die Dev/Test-Ebene in den neuen Webdiensten?**

Die neuen Azure ML-Webdienste verfügen über mehrere Ebenen (Tarife), die Sie zum Bereitstellen Ihres Abrechnungsplans verwenden können. Die Dev/Test-Ebene ist eine Ebene mit begrenzten enthaltenen Mengen. So können Sie Ihr Experiment als neuen Webdienst testen, ohne dass Kosten anfallen. Sie können einen Testlauf durchführen, um die Funktionsweise zu testen.

**Gibt es separate Speichergebühren?**

Der Free-Tarif von Machine Learning erfordert keinen separaten Speicher und lässt diesen nicht zu. Für den Standard-Tarif von Machine Learning benötigen Benutzer ein Azure-Speicherkonto. Azure-Speicher [wird separat abgerechnet](https://azure.microsoft.com/pricing/details/storage/).

**Wie unterstützt Machine Learning hoch verfügbares Arbeiten?**

Die Produktions-API-Aufrufzeiten können erheblich variieren und reichen im Allgemeinen von Hundertstel Millisekunden bis zu einigen Sekunden, können aber auch mehrere Minuten betragen. Dies richtet sich jeweils nach der Komplexität der Datenverarbeitung und des Machine Learning-Modells. Die beste Möglichkeit zur Einschätzung der Produktions-API-Aufrufzeiten besteht darin, basierend auf dem Machine Learning-Dienst einen Benchmark für ein Modell zu erstellen.

**Auf welcher Art von Computeressourcen werden meine Produktions-API-Aufrufe ausgeführt?**

Der Machine Learning-Dienst ist ein mehrinstanzenfähiger Dienst, und die tatsächlich im Back-End verwendeten Computeressourcen variieren und werden im Hinblick auf Leistung und Vorhersagbarkeit optimiert.

### Verwaltung der neuen Webdienste 

**Was passiert, wenn ich meinen Plan lösche?**

Der Plan wird aus dem Abonnement entfernt, und die Nutzung wird Ihnen anteilig berechnet.

Hinweis: Es ist nicht möglich, einen Plan zu löschen, der von einem Webdienst verwendet wird. Um den Plan zu löschen, müssen Sie dem Webdienst entweder einen neuen Plan zuweisen oder den Webdienst löschen.

**Was ist eine Planinstanz?**

Eine Planinstanz ist eine Einheit von enthaltenen Mengen, die Sie Ihrem Abrechnungsplan hinzufügen können. Wenn Sie für den Abrechnungsplan einen Abrechnungstarif wählen, ist darin eine Instanz enthalten. Falls Sie mehr enthaltene Mengen benötigen, können Sie dem Plan Instanzen des ausgewählten Abrechnungstarifs hinzufügen.

**Wie viele Planinstanzen kann ich hinzufügen?**

Ein Abonnement kann über eine Instanz der Dev/Test-Ebene verfügen.

Für die Tarife S1, S2 und S3 können Sie beliebig viele Instanzen hinzufügen.

Hinweis: Je nach voraussichtlicher Nutzung kann es unter Umständen kostengünstiger sein, ein Upgrade auf einen höheren Tarif mit enthaltenen Mengen durchzuführen, anstatt dem aktuellen Tarif Instanzen hinzuzufügen.

**Was passiert, wenn ich Plantarife ändere (Upgrade/Downgrade)?**

Der alte Plan wird gelöscht, und die aktuelle Nutzung wird anteilig berechnet. Für den restlichen Zeitraum wird ein neuer Plan mit allen enthaltenen Mengen des Tarifs erstellt, für den das Upgrade/Downgrade durchgeführt wurde.

Hinweis: Enthaltene Mengen werden pro Zeitraum zugeordnet, und nicht genutzte Mengen werden nicht übertragen.

**Was passiert, wenn ich die Anzahl von Instanzen in einem Plan erhöhe?**

Mengen werden anteilig einbezogen, und es kann bis zu 24 Stunden dauern, bis sie wirksam werden.

**Was passiert, wenn ich eine Instanz eines Plans lösche?**

Die Instanz wird aus dem Abonnement entfernt, und die Nutzung wird Ihnen anteilig berechnet.


### Registrieren für neue Webdienstpläne

**Wie kann ich mich für einen Plan registrieren?**

Sie haben zwei Möglichkeiten, Abrechnungspläne zu erstellen.

Bei der ersten Bereitstellung eines neuen Webdiensts können Sie einen vorhandenen Plan wählen oder einen neuen Plan erstellen.

Pläne, die auf diese Weise erstellt werden, befinden sich in Ihrer Standardregion, und Ihr Webdienst wird in dieser Region bereitgestellt.

Wenn Sie Dienste in anderen Regionen als der Standardregion bereitstellen möchten, kann es ratsam sein, Ihre Abrechnungspläne vor dem Bereitstellen des Diensts zu definieren.

In diesem Fall können Sie sich am Azure Machine Learning-Webdienste-Portal anmelden und zur Seite mit den Plänen navigieren. Auf dieser Seite können Sie Pläne hinzufügen und löschen und vorhandene Pläne ändern.

**Mit welchem Plan soll ich beginnen?**

Wir empfehlen Ihnen, mit dem Standard-Tarif S1 zu beginnen und die Nutzung Ihres Diensts zu überwachen. Wenn Sie feststellen, dass Sie Ihre enthaltenen Mengen schnell verbrauchen, können Sie Instanzen hinzufügen oder zu einem höheren Tarif wechseln und bessere Rabatte in Anspruch nehmen. Sie können Ihren Abrechnungsplan je nach Bedarf während des Abrechnungszyklus anpassen.

**In welchen Regionen sind die neuen Pläne verfügbar?**

Die neuen Abrechnungspläne sind in den drei Produktionsregionen verfügbar, in denen wir die neuen Webdienste unterstützen:

* USA (Mitte/Süden)
* Westeuropa
* Südostasien

**Ich verfüge über Webdienste in mehreren Regionen. Benötige ich einen Plan für jede Region?**

Ja. Die Preise der Pläne variieren je nach Region. Wenn Sie einen Webdienst in einer anderen Region bereitstellen, müssen Sie dafür einen regionsspezifischen Plan zuweisen.

### Neue Webdienste – Überschreitungen

**Wie kann ich überprüfen, ob die Nutzung meines Webdiensts überschritten wurde?**

Sie können die Nutzung Ihrer Pläne im Azure Machine Learning-Webdienste-Portal auf der Seite „Pläne“ anzeigen. Melden Sie sich am Portal an, und klicken Sie auf die Menüoption „Pläne“.

In den Spalten „Transaktionen“ und „Compute“ der Tabelle werden die enthaltenen Mengen des Plans und die Nutzung in Prozent angezeigt.

**Was passiert, wenn ich die enthaltenen Mengen der Dev/Test-Ebene aufgebraucht habe?**

Dienste, denen eine Dev/Test-Ebene zugewiesen wurde, werden beendet, bis der nächste Zeitraum beginnt oder bis Sie sie in einen kostenpflichtigen Tarif verschieben.

**Wie werden bei klassischen Webdiensten und Überschreitungen von neuen Webdiensten die Preise für Workloads vom Typ Anforderung/Antwort (RRS) und Batch (BES) berechnet?**

Bei einer RSS-Workload zahlen Sie für jeden API-Transaktionsaufruf, den Sie vornehmen, und für die Computezeit im Zusammenhang mit diesen Anforderungen. Zur Berechnung der Kosten für RRS-Produktions-API-Transaktionen wird die Gesamtzahl Ihrer durchgeführten API-Aufrufe mit dem Preis pro 1.000 Transaktionen (anteilsmäßig für die einzelnen Transaktionen) multipliziert. Zur Berechnung der Kosten für Ihre RSS-API-Produktions-API-Berechnungsstunden wird die Summe der für jeden API-Aufruf benötigten Zeit mit der Gesamtzahl an API-Transaktionen und dem Preis pro Produktions-API-Berechnungsstunde multipliziert.

Beispiel: Für eine Standard S1-Überschreitung lautet das Ergebnis für 1.000.000 API-Transaktionen mit einer Ausführungszeit von je 0,72 Sekunden für Produktions-API-Transaktionen „500 USD“ (1.000.000 · 0,50 USD/1.000 API-Transaktionen) und für Produktions-API-Berechnungsstunden „400 USD“ (1.000.000 · 0,72 Sek. · 2 USD/Std.). Dies ergibt eine Gesamtsumme von 900 USD.

Bei BES-Workloads erfolgt die Abrechnung nach dem gleichen Prinzip. Allerdings stehen die API-Transaktionskosten hier für die Anzahl von initiierten Batchaufträgen, und die Kosten für Computestunden stellen die Computezeit im Zusammenhang mit diesen Batchaufträgen dar. Zur Berechnung der Kosten für BES-Produktions-API-Transaktionen wird die Gesamtzahl der initiierten Aufträge mit dem Preis pro 1.000 Transaktionen (anteilsmäßig für die einzelnen Transaktionen) multipliziert. Zur Berechnung der Kosten für BES-Produktions-API-Berechnungsstunden wird die Summe der für die Ausführung der einzelnen Zeilen in Ihrem Auftrag benötigten Zeit mit der Gesamtzahl an Zeilen in Ihrem Auftrag, mit der Gesamtzahl an Aufträgen und mit dem Preis pro Produktions-API-Berechnungsstunde multipliziert. Bei Verwendung des Machine Learning-Rechners zeigt der Transaktionszähler die Anzahl von Aufträgen an, die ausgeführt werden sollen, und im Feld für die Zeit pro Transaktion wird die Gesamtzeit angezeigt, die zur Ausführung aller Zeilen in den einzelnen Aufträgen benötigt wird.

Beispiel für eine Standard S1-Überschreitung: Wenn Sie 100 Aufträge pro Tag initiieren, die jeweils 500 Zeilen umfassen und deren Ausführung jeweils 0,72 Sekunden dauert, betragen Ihre monatlichen Kosten für Produktions-API-Transaktionen 1,55 USD (100 Aufträge pro Tag = 3.100 Aufträge/Monat · 0,50 USD je 1.000 API-Transaktionen). Die Kosten für Produktions-API-Berechnungsstunden liegen bei 620 USD (500 Zeilen · 0,72 Sek. · 3.100 Aufträge · 2 USD/Std.). Dies ergibt eine Gesamtsumme von 621,55 USD.

### Klassische Azure ML-Webdienste

**Ist die nutzungsbasierte Bezahlung noch verfügbar?** Ja. Klassische Webdienste sind in Azure Machine Learning weiterhin verfügbar.

### Azure Machine Learning – Free- und Standard-Tarif

**Was ist im Free-Tarif von Azure Machine Learning inbegriffen?**

Der Free-Tarif von Azure Machine Learning soll Ihnen eine detaillierte Einführung in Azure Machine Learning Studio ermöglichen. Hierzu benötigen Sie für die Anmeldung nur ein Microsoft-Konto. Der Free-Tarif umfasst den kostenlosen Zugriff auf einen Azure Machine Learning Studio-Arbeitsbereich pro [Microsoft-Konto](https://www.microsoft.com/account/default.aspx). Dazu gehört auch die Möglichkeit, bis zu 10 GB Speicher zu nutzen und Modelle als Staging-APIs zu operationalisieren. Workloads werden im Free-Tarif nicht durch eine SLA abgedeckt und sind lediglich für Entwicklungszwecke und die private Nutzung vorgesehen. Workloads im Free-Tarif können nicht auf Daten zugreifen, indem sie eine Verbindung mit einer lokalen SQL Server-Instanz herstellen.

**Was ist im Standard-Tarif und den Plänen von Azure Machine Learning inbegriffen?**

Der Standard-Tarif von Azure Machine Learning ist eine kostenpflichtige Produktionsversion von Azure Machine Learning Studio. Die Monatsgebühr für den Azure ML Studio-Dienst wird pro Arbeitsbereich und Monat in Rechnung gestellt. Angefangene Monate werden anteilig abgerechnet. Azure ML Studio-Versuchstunden werden pro Computestunde für aktive Versuche (Experimente) in Rechnung gestellt. Angefangene Stunden werden anteilig abgerechnet.

Die Abrechnung des Azure ML-API-Diensts hängt davon ab, ob es sich um einen klassischen oder einen neuen Webdienst handelt.

Die unten angegebenen Kosten werden pro Arbeitsbereich für Ihr Abonnement aggregiert.

* Machine Learning-Arbeitsbereich-Abonnement: Das Machine Learning-Arbeitsbereich-Abonnement ist eine Monatsgebühr, die Zugriff auf einen ML Studio-Arbeitsbereich ermöglicht und zum Ausführen von Experimenten in Studio sowie für die Nutzung der Produktions-APIs erforderlich ist.
* Studio-Versuchstunden: Hierbei werden alle Computegebühren zusammengefasst, die durch laufende Experimente in ML Studio und laufende Produktions-API-Aufrufe in der Stagingumgebung angefallen sind.
* Greifen Sie auf Daten zu, indem Sie eine Verbindung mit einer lokalen SQL Server-Instanz in Ihren Modellen für Training und Bewertung herstellen.
* Für klassische Webdienste:
	* Produktions-API-Berechnungsstunden: Dieser Zähler umfasst Computegebühren, die durch in der Produktion ausgeführte Webdienste anfallen.
	* Produktions-API-Transaktionen (in 1000): Dieser Zähler umfasst die Gebühren, die pro Aufruf Ihres Produktionswebdiensts anfallen.

Abgesehen von den oben genannten Gebühren werden die Gebühren für neue Webdienste basierend auf dem ausgewählten Plan aggregiert:

* API-Plan Standard-S1/S2/S3 (Einheiten): Steht für den Typ der Instanz, der für neue Webdienste ausgewählt wurde.
* Überschreitung API-Computestunden Standard-S1/S2/S3 (Einheiten): Enthält Computegebühren, die für die in der Produktion ausgeführten neuen Webdienste anfallen, nachdem die enthaltenen Mengen in vorhandenen Instanzen aufgebraucht sind. Die zusätzliche Nutzung wird anhand der Überschreitungsrate berechnet, die dem Tarif des S1/S2/S3-Plans zugeordnet ist.
* Überschreitung API-Transaktionen Standard-S1/S2/S3 (in 1.000): Enthält Gebühren, die pro Aufruf Ihres neuen Webdiensts für die Produktion anfallen, nachdem die enthaltenen Mengen in vorhandenen Instanzen aufgebraucht sind. Die zusätzliche Nutzung wird anhand der Überschreitungsrate berechnet, die dem Tarif des S1/S2/S3-Plans zugeordnet ist.
* Enthaltene Menge an API-Computestunden: Umfasst bei den neuen Webdiensten die enthaltene Menge von API-Computestunden.
* Enthaltene Menge an API-Transaktionen (in 1.000): Umfasst bei den neuen Webdiensten die enthaltene Menge von API-Transaktionen.


**Wie registriere ich mich für den Free-Tarif von Azure ML?**

Dazu benötigen Sie nur ein Microsoft-Konto. Wechseln Sie zur [Azure Machine Learning-Startseite](https://azure.microsoft.com/services/machine-learning/), und klicken Sie auf **Jetzt starten**. Melden Sie sich mit Ihrem Microsoft-Konto an. Daraufhin wird für Sie im Free-Tarif ein Arbeitsbereich erstellt. Sie können Machine Learning sofort erkunden und Experimente erstellen.

**Wie registriere ich mich für den Standard-Tarif von Azure ML?**

Zunächst benötigen Sie Zugriff auf ein Azure-Abonnement, um einen ML Standard-Arbeitsbereich erstellen zu können. Sie können sich für ein 30-tägiges, kostenloses Azure-Testabonnement registrieren und später ein Upgrade auf ein kostenpflichtiges Azure-Abonnement durchführen oder direkt ein kostenpflichtiges Azure-Abonnement erwerben. Sobald Sie Zugriff auf das Abonnement haben, können Sie über das klassische Microsoft Azure-Portal einen Machine Learning-Arbeitsbereich erstellen. Lesen Sie die [Schritt-für-Schritt-Anleitungen](https://azure.microsoft.com/trial/get-started-machine-learning-b/).

Alternativ können Sie auch von einem Standard-ML-Arbeitsbereichbesitzer eingeladen werden, auf den Arbeitsbereich des Besitzers zuzugreifen.

**Kann ich meinen eigenen Azure-Blobspeicher angeben, der im Free-Tarif genutzt werden kann?**

Nein. Der Standard-Tarif entspricht der Version des Machine Learning-Diensts, die vor der Einführung der Tarife verfügbar war.

**Kann ich meine Machine Learning-Modelle im Free-Tarif als APIs bereitstellen?**

Ja. Sie können Machine Learning-Modelle im Rahmen des Free-Tarifs mit Staging-API-Diensten operationalisieren. Um den Staging-API-Dienst in die Produktion zu übergeben und einen Produktionsendpunkt für den standardisierten Dienst abzurufen, müssen Sie den Standard-Tarif verwenden.

**Worin besteht der Unterschied zwischen der kostenlosen Testversion von Azure und dem Free-Tarif von Azure Machine Learning?**

Die [kostenlose Testversion von Microsoft Azure](https://azure.microsoft.com/free/) bietet Gutschriften, die einen Monat lang für jeden Azure-Dienst genutzt werden können. Der Free-Tarif von Azure Machine Learning hingegen stellt vor allem einen kontinuierlichen Zugriff auf den Azure Machine Learning-Dienst für produktionsfremde Workloads sicher.

**Wie verschiebe ich ein Experiment vom Free-Tarif in den Standard-Tarif?**

Gehen Sie wie folgt vor, um Ihre Experimente aus dem Free-Tarif in den Standard-Tarif zu kopieren:

1.	Melden Sie sich bei Azure Machine Learning Studio an, und vergewissern Sie sich, dass Ihnen in der Arbeitsbereichauswahl in der oberen Navigationsleiste der Free- und der Standard-Arbeitsbereich angezeigt werden.
2.	Wechseln Sie zum Free-Arbeitsbereich, wenn Sie sich im Standard-Arbeitsbereich befinden.
3.	Wählen Sie in der Experimentlistenansicht ein zu kopierendes Experiment aus, und klicken Sie dann auf die Befehlsschaltfläche „Kopieren“.
4.	Wählen Sie im Popupdialogfeld den Standard-Arbeitsbereich aus, und klicken Sie dann auf die Schaltfläche „Kopieren“. Alle zugehörigen Datasets, Trainingsmodelle usw. werden zusammen mit dem Experiment in den Standard-Arbeitsbereich kopiert.
6.	Sie müssen das Experiment erneut ausführen und Ihren Webdienst erneut im Standard-Arbeitsbereich veröffentlichen.

### Studio-Arbeitsbereich

**Bekomme ich verschiedene Rechnungen für unterschiedliche Arbeitsbereiche?**

Die Gebühren für den Arbeitsbereich werden separat für jede anwendbare Messgröße auf einer Gesamtrechnung ausgewiesen.

**Auf welcher Art von Computeressourcen werden meine Experimente ausgeführt?**

Der Machine Learning-Dienst ist ein mehrinstanzenfähiger Dienst, und die tatsächlich im Back-End verwendeten Computeressourcen variieren und werden im Hinblick auf Leistung und Vorhersagbarkeit optimiert.

### Gastzugriff

**Was umfasst der Gastzugriff für Azure Machine Learning Studio?**

Über den Gastzugriff können Sie Azure Machine Learning Studio in begrenztem Umfang testen und die Umgebung kostenlos und ohne Authentifizierung nutzen, um Experimente zu erstellen und auszuführen. Gastsitzungen sind nicht persistent (können nicht gespeichert werden) und auf acht Stunden begrenzt. Außerdem werden R und Python nicht unterstützt, es können keine Staging-APIs verwendet werden, und sowohl die Größe von Datasets als auch die Speicherkapazität sind beschränkt. Im Gegensatz dazu haben Benutzer, die sich mit einem Microsoft-Konto anmelden, vollen Zugriff auf den oben beschriebenen Funktionsumfang von Machine Learning Studio im Free-Tarif, der einen persistenten Arbeitsbereich und umfangreichere Optionen bietet. Wenn Sie Machine Learning kostenlos nutzen möchten, klicken Sie unter [https://studio.azureml.net](https://studio.azureml.net) einfach auf **Getting started** und wählen entweder den Gastzugriff oder die Anmeldung über ein Microsoft-Konto.

<!-- Module References -->
[image-reader]: https://msdn.microsoft.com/library/azure/893f8c57-1d36-456d-a47b-d29ae67f5d84/
[join]: https://msdn.microsoft.com/library/azure/124865f7-e901-4656-adac-f4cb08248099/
[machine-learning-modules]: https://msdn.microsoft.com/library/azure/6d9e2516-1343-4859-a3dc-9673ccec9edc/
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[python]: https://msdn.microsoft.com/library/azure/CDB56F95-7F4C-404D-BDE7-5BB972E6F232
[counts]: https://msdn.microsoft.com/library/azure/dn913056.aspx

<!---HONumber=AcomDC_0928_2016-->