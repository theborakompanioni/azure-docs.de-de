<properties 
	pageTitle="Was ist der Team Data Science-Prozess (TDSP)? | Microsoft Azure" 
	description="Der Team Data Science-Prozess ist eine systematische Methode zum Entwickeln intelligenter Anwendungen, die erweiterte Analysen nutzen." 
	services="machine-learning" 
	documentationCenter="" 
	authors="bradsev"
	manager="jhubbard" 
	editor="cgronlun" />

<tags 
	ms.service="machine-learning" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="09/19/2016" 
	ms.author="bradsev" />


# Was ist der Team Data Science-Prozess (TDSP)?

Der [Team Data Science-Prozess (TDSP)](data-science-process-overview.md) bietet einen systematischen Ansatz für das Erstellen intelligenter Anwendungen, der Data Scientists eine effektive Zusammenarbeit an allen Aktivitäten des gesamten Lebenszyklus ermöglicht, um diese Anwendungen in Produkte umzuwandeln. Der TDSP definiert eine Abfolge von Schritten als **Leitfaden** zum Bestimmen des Problems, Einrichten der benötigten Tools und Umgebungen, Analysieren relevanter Daten, Erstellen und Bewerten von Vorhersagemodellen und anschließenden Bereitstellen dieser Modelle in Unternehmensanwendungen.

Dies sind die Schritte im **Team Data Science-Prozess**:

![CAP-Workflow](./media/machine-learning-data-science-the-cortana-analytics-process/CAP-workflow.png)

Der Prozess ist **iterativ**: das Verständnis neuer und vorhandener Optimierungen im Modell entwickelt sich weiter und erfordert das Überarbeiten von zuvor in der Folge abgeschlossenen Schritten. Vorhandene Organisationsentwicklungs- und Projektplanungsprozesse können **problemlos an die im TDSP definierte Schrittfolge angepasst werden**.

Die Schritte in diesem Prozess sind im [TDSP-Lernpfad](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) abgebildet und verlinkt und werden unten beschrieben.

## Vorbereitungsschritte 

## P1. Planen des Analyseprojekts 

Starten Sie ein Analyseprojekt, indem Sie geschäftliche Ziele und Probleme bestimmen, die als **Geschäftsanforderungen** ausgedrückt werden. Ein zentrales Ziel dieses Schritts ist das Bestimmen der wesentlichen geschäftlichen Variablen (z. B. Umsatzprognose oder Wahrscheinlichkeit eines betrügerischen Auftrags), die die Analyse vorherzusagen hat, um diese Anforderungen zu erfüllen. Zusätzliche Planung ist dann in der Regel wichtig, um sich einen Überblick über die **Datenquellen** zu verschaffen, die benötigt werden, um die Ziel des Projekts aus Analysesicht zu erreichen. Es ist z. B. nicht ungewöhnlich festzustellen, dass vorhandene Systeme zusätzliche Arten von Daten sammeln und protokollieren müssen, um das Problem anzugehen und die Projektziele zu erreichen. Eine Anleitung finden Sie unter [Bestimmen von Szenarien und Planen der Datenverarbeitung für die erweiterte Analyse](machine-learning-data-science-plan-your-environment.md) sowie unter [Szenarien für die erweiterte Analyse in Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md).

## P2. Einrichten der Analyse-Umgebung 

Eine Analyse-Umgebung für den Team Data Science-Prozess umfasst mehrere Komponenten:

- **Datenarbeitsbereiche**, in denen die Daten für die Analyse und Modellierung bereitgestellt werden
- eine **Verarbeitungsinfrastruktur** für die Vorverarbeitung, Untersuchung und Modellierung von Daten
- eine **Laufzeitinfrastruktur** zum Operationalisieren der Analysemodelle und Ausführen intelligenter Clientanwendungen, die die Modelle nutzen.

Die einzurichtende Analyse-Infrastruktur ist häufig Teil einer Umgebung, die von den Hauptsystemen getrennt ist. Doch in der Regel nutzt sie Daten aus mehreren Systemen innerhalb des Unternehmens sowie aus Quellen außerhalb des Unternehmens. Die Analyse-Infrastruktur kann rein cloudbasiert, lokal oder eine Kombination aus beidem sein. Optionen finden Sie unter [Einrichten von Data Science-Umgebungen für die Verwendung im Team Data Science-Prozess](machine-learning-data-science-environment-setup.md).

## Analyseschritte:  

## 1\. Erfassen von Daten in der Analyse-Umgebung 

Der erste Schritt ist das Erfassen der relevanten Daten aus verschiedenen Quellen, entweder innerhalb oder außerhalb des Unternehmens, in einer Analyse-Umgebung, in der die Daten verarbeitet werden können. Das **Format** der Daten in der Quelle kann sich von dem vom Ziel geforderten Format unterscheiden. Daher müssen möglicherweise auch verschiedene Datentransformationen mithilfe der Erfassungstools erfolgen. Optionen finden Sie unter [Laden von Daten in Speicherumgebungen für Analysen](machine-learning-data-science-ingest-data.md).

Zusätzlich zur ersten Erfassung von Daten sind viele intelligente Anwendungen erforderlich, um die Daten im Rahmen eines laufenden Lernprozesses zu aktualisieren. Dies kann durch Einrichten einer **Datenpipeline** oder eines Workflows erfolgen. Das gehört zum iterativen Teil des Prozesses, der das Neuerstellen und Neuauswerten der Analysemodelle umfasst, die von der intelligenten Anwendung verwendet werden, die die Lösung bereitstellt. Siehe beispielsweise [Verschieben von Daten von einem lokalen SQL Server zu SQL Azure mithilfe von Azure Data Factory](machine-learning-data-science-move-sql-azure-adf.md).


## 2\. Untersuchen und Vorverarbeiten von Daten 

Der nächste Schritte besteht darin, sich ein umfassenderes Verständnis der Daten zu verschaffen, indem **zusammenfassende Statistiken** und Beziehungen untersucht und Techniken wie die **Visualisierung** genutzt werden. In dieser Phase werden auch Probleme mit der **Datenqualität** und Integrität, wie z. B. fehlende Werte, Datentypenkonflikte und inkonsistente Datenbeziehungen, behandelt. Vorverarbeitungstransformationen erfolgen, um die Rohdaten zu bereinigen, bevor weitere Analysen und Modellierungen stattfinden. Eine Beschreibung finden Sie unter [Aufgaben zur Vorbereitung von Daten für erweitertes Machine Learning](machine-learning-data-science-prepare-data.md).


## 3\. Entwickeln von Features 

Datenwissenschaftler muss in Zusammenarbeit mit Fachbereichsexperten die Features bestimmen, die die wesentlichen Eigenschaften des Datasets erfassen und am besten zum Vorhersagen der wichtigsten Geschäftsvariablen genutzt werden können, die während der Planung ausgemacht wurden. Diese neuen Features können von vorhandenen Daten abgeleitet werden oder ggf. das Erfassen zusätzlicher Daten erfordern. Dieser Prozess wird als **Feature-Engineering** bezeichnet und ist einer der wichtigsten Schritte beim Entwickeln eines effektiven Predictive Analytics-Systems. Dieser Schritt erfordert eine kreative Kombination von Sachkenntnis mit den im Datenuntersuchungsschritt gewonnenen Erkenntnissen. Eine Anleitung finden Sie unter [Featureentwicklung im Cortana-Analyseprozess](machine-learning-data-science-create-features.md).


## 4\. Erstellen von Vorhersagemodellen 

Datenwissenschaftler erstellen Analysemodelle zum Vorhersagen der Hauptvariablen, die von den Geschäftsanforderungen bestimmt werden. Diese werden wiederum im Planungsschritt anhand von Daten definiert, die bereinigt und Features zugeordnet wurden. Machine Learning-Systeme unterstützen mehrere **Modellierungsalgorithmen**, die für eine Vielzahl von Fällen geeignet sind. Eine Anleitung finden Sie unter [Auswählen von Algorithmen für Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md).

Datenwissenschaftler müssen das am besten geeignete Modell für die jeweilige Vorhersageaufgabe wählen. Es ist nicht ungewöhnlich, dass Ergebnisse aus mehreren Modellen kombiniert werden müssen, um die besten Ergebnisse zu erzielen. Die Eingabedaten für die Modellierung sind in der Regel nach dem Zufallsprinzip in drei Teile unterteilt:

- ein Dataset für das Training
- ein Dataset für die Überprüfung
- ein Datasets für Tests

Die Modelle werden mithilfe des **Datasets für das Training** erstellt. Die optimale Kombination von Modellen (mit optimierten Parametern) wird ausgewählt, indem die Modelle ausgeführt und die Vorhersagefehler für das **Dataset für die Überprüfung** gemessen werden. Schließlich wird das **Dataset für Tests** verwendet, um die Leistung des ausgewählten Modells für unabhängige Daten auswerten, die nicht zum Trainieren oder Überprüfen des Modells verwendet wurden. Verfahren finden Sie unter [Auswerten der Modellleistung in Azure Machine Learning](machine-learning-evaluate-model-performance.md).


## 5\. Bereitstellen und Nutzen von Modellen 

Sobald wir eine Gruppe von Modellen mit adäquater Leistung haben, können diese von anderen Anwendungen **operationalisiert** und genutzt werden. Abhängig von den Geschäftsanforderungen erfolgen Vorhersagen entweder in **Echtzeit** oder auf **Batchbasis**. Um operationalisiert zu werden, müssen die Modelle mit einer **offenen API-Schnittstelle** verfügbar gemacht werden, die von verschiedenen Anwendungen einfach genutzt werden kann, wie z. B. von Onlinewebsites, Kalkulationstabellen, Dashboards bzw. Branchen- oder Back-End-Anwendungen. Siehe [Bereitstellen eines Azure Machine Learning-Webdiensts](machine-learning-publish-a-machine-learning-web-service.md).

## Zusammenfassung und nächste Schritte

Der [Team Data Science-Prozess](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) wird als eine Folge von zu durchlaufenden Schritten modelliert. Diese bieten einen **Leitfaden** für die Aufgaben, die zum Nutzen erweiterter Analysen für das Entwickeln intelligenter Anwendungen ausgeführt werden müssen. Jeder Schritt enthält Details zur Verwendung verschiedener Microsoft-Technologien zum Erledigen der beschriebenen Aufgaben.

Wenngleich der TDSP nicht bestimmte Arten von **Dokumentationsinhalten** vorschreibt, ist es eine bewährte Methode, die Ergebnisse der Datenuntersuchung, Modellierung und Auswertung zu dokumentieren und den dazugehörigen Code zu speichern, damit die Analyse bei Bedarf wiederholt kann. Dies ermöglicht außerdem die Wiederverwendung der Analyseergebnisse bei der Arbeit mit anderen Anwendungen mit ähnlichen Daten und Vorhersageaufgaben.

Vollständige exemplarische Vorgehensweisen, in denen sämtliche Schritte im Prozess für **bestimmte Szenarien** gezeigt werden, sind ebenfalls verfügbar. Siehe z.B.:

- [Der Team Data Science-Prozess in Aktion: Verwenden von SQL Server](machine-learning-data-science-process-sql-walkthrough.md)
- [Der Team Data Science-Prozess in Aktion: Verwenden von HDInsight Hadoop-Clustern](machine-learning-data-science-process-hive-walkthrough.md)
- [Data Science mit Spark in Azure HD.mdnsight](machine-learning-data-science-spark-overview.md)
- [Skalierbare Data Science in Azure Data Lake: lückenlose exemplarische Vorgehensweise](machine-learning-data-science-process-data-lake-walkthrough.md)

 

<!---HONumber=AcomDC_0921_2016-->