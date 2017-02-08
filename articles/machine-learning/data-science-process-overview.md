---
title: Team Data Science-Prozesslebenszyklus | Microsoft-Dokumentation
description: "Enthält eine Beschreibung der wichtigsten Komponenten des Team Data Science-Prozesslebenszyklus."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: b1f677bb-eef5-4acb-9b3b-8a5819fb0e78
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2016
ms.author: bradsev;hangzh;gokuma
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 725f6d4a209ada154eb9aa90a8226dcaa0859990


---
# <a name="team-data-science-process-lifecycle"></a>Team Data Science-Prozesslebenszyklus
Der Team Data Science-Prozess (TDSP) umfasst einen empfohlenen Lebenszyklus, den Sie verwenden können, um die Entwicklung Ihrer Data Science-Projekte zu strukturieren. Im Lebenszyklus sind die gesamten Schritte aufgeführt, die bei Projekten bei der Ausführung normalerweise durchlaufen werden. Wenn Sie einen anderen Data Science-Lebenszyklus verwenden, z.B. [CRISP-DM](https://wikipedia.org/wiki/Cross_Industry_Standard_Process_for_Data_Mining), [KDD](https://wikipedia.org/wiki/Data_mining#Process) oder einen eigenen benutzerdefinierten Prozess Ihrer Organisation, können Sie den aufgabenbasierten TDSP trotzdem im Kontext dieser Entwicklungslebenszyklen verwenden. 

Dieser Lebenszyklus wurde für Data Science-Projekte entworfen, die im Lieferumfang von intelligenten Anwendungen enthalten sein sollen. Mit diesen Anwendungen werden Modelle vom Typ „Machine Learning“ oder „Künstliche Intelligenz“ für Predictive Analytics bereitgestellt. Explorative Data Science-Projekte und spontane oder unregelmäßige Analyseprojekte können von diesem Prozess ebenfalls profitieren, aber in diesen Fällen sind einige der beschriebenen Schritte unter Umständen nicht erforderlich.    

Hier ist der **Team Data Science-Prozesslebenszyklus** grafisch dargestellt. 

![TDSP-Lebenszyklus](./media/data-science-process-overview/tdsp-lifecycle.png) 

Der TDSP-Lebenszyklus besteht aus fünf Hauptphasen, die iterativ ausgeführt werden. Diese umfassen:

* **Geschäftliche Aspekte**
* **Datenerfassung und -auswertung**
* **Modellierung**
* **Bereitstellung**
* **Kundenakzeptanz**

Für jede Phase werden die folgenden Informationen bereitgestellt:

* **Ziele**: Liste der einzelnen Ziele
* **Vorgehensweise**: Auflistung der spezifischen Aufgaben und Anleitungen zur Durchführung
* **Artefakte**: Aufstellung der Endergebnisse und Unterstützung für ihre Erreichung

## <a name="1-business-understanding"></a>1. Geschäftliche Aspekte
### <a name="goals"></a>Ziele
* Es werden die **Hauptvariablen** angegeben, die als **Modellziele** dienen sollen und deren verwandte Metriken verwendet werden, um den Erfolg des Projekts zu bestimmen.
* Die relevanten **Datenquellen**, auf die für das Unternehmen Zugriff besteht bzw. von denen aus das Unternehmen bei Bedarf zugreift, werden identifiziert.

### <a name="how-to-do-it"></a>Vorgehensweise
In dieser Phase werden zwei Hauptaufgaben durchgeführt: 

* **Definieren von Zielen**: Ermitteln Sie zusammen mit Ihrem Kunden und anderen Projektbeteiligten die geschäftlichen Probleme, um sich damit vertraut zu machen. Formulieren Sie Fragestellungen, mit denen die geschäftlichen Ziele definiert werden und die mit Data Science-Verfahren gelöst werden können.
* **Identifizieren von Datenquellen**: Suchen Sie nach den relevanten Daten, mit denen Sie die Fragen beantworten können, die den Zielen des Projekts zugrunde liegen.

#### <a name="11-define-objectives"></a>1.1 Definieren von Zielen
1. Ein zentrales Ziel dieses Schritts ist die Identifizierung der wichtigsten **geschäftlichen Variablen**, die von der Analyse vorhergesagt werden müssen. Diese Variablen werden als **Modellziele** bezeichnet, und die dazugehörigen Metriken werden verwendet, um den Erfolg des Projekts zu bestimmen. Beispiele für Ziele sind Umsatzprognosen oder die Wahrscheinlichkeit, dass eine Bestellung betrügerischer Art ist.
2. Definieren Sie die **Projektziele**, indem Sie präzise Fragen stellen und verfeinern, die relevant, spezifisch und eindeutig sind. Data Science ist der Prozess zur Verwendung von Namen und Zahlen mit dem Ziel, Antworten auf diese Fragen zu finden. „*Stellen Sie sich beim Formulieren der Frage vor, dass Sie sich an ein Orakel wenden, das auf alles eine Antwort hat, solange es sich um eine Zahl oder einen Namen handelt*.“ Weitere Anleitungen finden Sie im Abschnitt **Stellen einer genauen Frage** des Blogs [How to do Data Science](https://blogs.technet.microsoft.com/machinelearning/2016/03/28/how-to-do-data-science/) (Vorgehensweise bei Data Science).   Data Science/Machine Learning wird normalerweise verwendet, um fünf Arten von Fragen zu beantworten:
   * Wie viel bzw. wie viele? (Regression)
   * Welche Kategorie? (Klassifizierung)
   * Welche Gruppe? (Clustering)
   * Ist dies merkwürdig? (Anomalieerkennung)
   * Welche Option sollte gewählt werden? (Empfehlung)
3. Definieren Sie das **Projektteam**, indem Sie die Rollen und Zuständigkeiten der Mitglieder angeben. Entwickeln Sie einen groben Plan mit Meilensteinen, den Sie abarbeiten, wenn weitere Informationen ermittelt werden.  
4. **Definieren von Erfolgsmetriken**: Beispiel: Erreichung einer Genauigkeit von X% in Bezug auf die Prognose der Kundenabwanderung am Ende dieses dreimonatigen Projekts, damit wir Sonderangebote anbieten können, um die Abwanderung zu reduzieren. Die Metriken sollten **SMART** sein: 
   * **S**pecific (Spezifisch) 
   * **M**easurable (Messbar)
   * **A**chievable (Realistisch) 
   * **R**elevant (Relevant) 
   * **T**ime-bound (Zeitbezogen) 

#### <a name="12-identify-data-sources"></a>1.2 Identifizieren von Datenquellen
Identifizieren Sie Datenquellen, die bekannte Beispiele für Antworten auf Ihre präzisen Fragen enthalten. Suchen Sie nach den folgenden Daten:

* Daten, die für die Frage **relevant** sind. Verfügen wir über Kennzahlen für das Ziel und Features, die sich auf das Ziel beziehen?
* Dies sind Daten, bei denen es sich um **genaue Kennzahlen** unseres Modellziels und die passenden Features handelt.

Es ist z. B. nicht ungewöhnlich festzustellen, dass vorhandene Systeme zusätzliche Arten von Daten sammeln und protokollieren müssen, um das Problem anzugehen und die Projektziele zu erreichen. In diesem Fall kann es ratsam sein, nach externen Datenquellen zu suchen oder die Systeme zu aktualisieren, um neuere Daten zu erfassen.

### <a name="artifacts"></a>Artefakte
Hier sind die Ergebnisse dieser Phase angegeben:

* [**Charta-Dokument**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Project/Charter.md): In der Strukturdefinition des TDSP-Projekts ist eine Standardvorlage angegeben. Dieses Dokument wird während des gesamten Projekts entsprechend aktualisiert, wenn neue Erkenntnisse gewonnen werden und sich geschäftliche Anforderungen ändern. Der Schlüssel ist, dass dieses Dokument immer wieder durchlaufen wird und während des Ermittlungsprozesses weitere Details hinzugefügt werden. Beziehen Sie den Kunden und andere Projektbeteiligte in das Vornehmen der Änderungen ein, und informieren Sie sie eindeutig über die Gründe für die Änderungen.  
* [**Datenquellen**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/Data%20Defintion.md#raw-data-sources): Dies ist Teil des Datenberichts aus der TDSP-Projektstruktur. Darin sind die Quellen für die Rohdaten beschrieben. In späteren Phasen fügen Sie weitere Details ein, z.B. Skripts, um die Daten in die Analyseumgebung zu verschieben.  
* [**Daten**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/DataDictionaries): Dieses Dokument enthält die Beschreibungen und das Schema (Datentypen, Informationen zu Überprüfungsregeln, falls vorhanden) für die Daten zum Beantworten der Frage. Die Diagramme bzw. Beschreibungen zu den Entitätsbeziehungen sind ebenfalls enthalten, falls verfügbar.

## <a name="2-data-acquisition-and-understanding"></a>2. Datenerfassung und -auswertung
### <a name="goals"></a>Ziele
* Ein sauberes Dataset von hoher Qualität, bei dem die Beziehungen zu den Zielvariablen in der Analyseumgebung klar sind und das bereit für die Modellierung ist.
* Eine Lösungsarchitektur der Datenpipeline wurde entwickelt, um Daten regelmäßig aktualisieren und bewerten zu können.

### <a name="how-to-do-it"></a>Vorgehensweise
In dieser Phase werden drei Hauptaufgaben durchgeführt:

* **Erfassen der Daten** in der Zielanalyseumgebung.
* **Untersuchen der Daten**, um zu ermitteln, ob die Datenqualität für die Beantwortung der Frage ausreicht. 
* **Einrichten einer Datenpipeline**, um neue oder regelmäßig aktualisierte Daten bewerten zu können.

#### <a name="21-ingest-the-data"></a>2.1 Erfassen der Daten
Richten Sie den Prozess so ein, dass die Daten von den Quellspeicherorten an die Zielspeicherorte verschoben werden, an denen Analysevorgänge wie das Trainieren oder das Erstellen von Vorhersagen ausgeführt werden. Technische Details und Optionen zur Vorgehensweise mit den verschiedenen Azure-Datendiensten finden Sie unter [Laden von Daten in Speicherumgebungen für Analysen](machine-learning-data-science-ingest-data.md). 

#### <a name="22-explore-the-data"></a>2.2 Untersuchen der Daten
Bevor Sie Modelle trainieren können, müssen Sie ein gutes Verständnis der Daten entwickeln. Datasets sind in der Praxis häufig überladen oder weisen fehlende Werte oder verschiedene andere Diskrepanzen auf. Sie können die Datenzusammenfassung und -visualisierung verwenden, um die Qualität Ihrer Daten zu überprüfen und die Informationen bereitzustellen, die vor der Modellerstellung zum Verarbeiten der Daten erforderlich sind. Eine Anleitung zum Bereinigen der Daten finden Sie unter [Aufgaben zur Vorbereitung von Daten für erweitertes Machine Learning](machine-learning-data-science-prepare-data.md). Dieser Prozess ist häufig iterativ. 

TDSP verfügt über ein automatisiertes Hilfsprogramm mit dem Namen [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils), mit dem Daten visualisiert und Berichte zur Datenzusammenfassung visualisiert werden können. Es wird empfohlen, mit IDEAR zu beginnen, um die Daten zu untersuchen. Auf diese Weise können Sie interaktiv ohne Codierung ein erstes Verständnis der Daten entwickeln und anschließend benutzerdefinierten Code für die Untersuchung und Visualisierung der Daten schreiben. 

Wenn Sie mit der Qualität der bereinigten Daten zufrieden sind, besteht der nächste Schritt darin, die inhärenten Muster der Daten besser zu verstehen. Mit diesen Informationen können Sie eine passendes Vorhersagemodell für Ihr Ziel auswählen und entwickeln. Suchen Sie nach Anhaltspunkten dafür, wie gut die Daten mit dem Ziel verbunden sind und ob ausreichend Daten vorhanden sind, um mit den nächsten Schritten der Modellierung fortfahren zu können. Auch dieser Prozess ist häufig iterativ. Unter Umständen müssen Sie nach neuen Datenquellen mit genaueren bzw. relevanteren Daten suchen, um das Dataset zu erweitern, das im vorherigen Schritt ursprünglich identifiziert wurde.  

#### <a name="23-set-up-a-data-pipeline"></a>2.3 Einrichten einer Datenpipeline
Zusätzlich zur ersten Erfassung und Bereinigung der Daten müssen Sie normalerweise einen Prozess einrichten, um neue Daten zu bewerten oder die Daten im Rahmen des ständigen Lernprozesses regelmäßig zu aktualisieren. Dies kann durch Einrichten einer Datenpipeline oder eines Workflows erfolgen. Hier ist ein [Beispiel](machine-learning-data-science-move-sql-azure-adf.md) dafür angegeben, wie Sie eine Pipeline mit der [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) einrichten. In dieser Phase wird eine Lösungsarchitektur der Datenpipeline entwickelt. Die Pipeline wird außerdem parallel zu den folgenden Phasen des Data Science-Projekts entwickelt. Die Pipeline kann auf Batches basieren oder eine Streaming/Echtzeit- oder Hybridpipeline sein. Dies richtet sich nach Ihren geschäftlichen Anforderungen und den Einschränkungen Ihrer vorhandenen Systeme, in die diese Lösung integriert wird. 

### <a name="artifacts"></a>Artefakte
Hier sind die Ergebnisse dieser Phase angegeben:

* [**Bericht zur Datenqualität**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/DataSummaryReport.md): Dieser Bericht enthält Datenzusammenfassungen, Beziehungen zwischen den einzelnen Attributen und Zielen, die Rangfolge der Variablen usw. Mit dem in TDSP enthaltenen Tool [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils) können Sie diesen Bericht für alle Datasets in Tabellenform schnell erstellen, z.B. eine CSV-Datei oder eine relationale Tabelle. 
* **Lösungsarchitektur**: Dies kann ein Diagramm oder eine Beschreibung Ihrer Datenpipeline sein, die zum Ausführen von Bewertungen oder Vorhersagen für neue Daten nach der Erstellung eines Modells verwendet wird. Die Pipeline zum erneuten Trainieren Ihres Modells basierend auf den neuen Daten ist ebenfalls enthalten. Das Dokument ist in [diesem Verzeichnis](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/Project) gespeichert, wenn Sie die Vorlage für die TDSP-Verzeichnisstruktur verwenden.
* **Prüfpunktentscheidung**: Bevor Sie mit der vollständigen Featureentwicklung und Modellerstellung beginnen, können Sie das Projekt erneut auswerten, um zu ermitteln, ob Daten in ausreichender Menge vorhanden sind. Es kann beispielsweise sein, dass Sie bereit zum Fortfahren sind, mehr Daten erfassen müssen oder das Projekt beenden müssen, weil für die Beantwortung der Frage keine Daten zur Verfügung stehen.

## <a name="3-modeling"></a>3. Modellierung
### <a name="goals"></a>Ziele
* Optimale Datenfeatures für das Machine Learning-Modell.
* Ein informatives ML-Modell, mit dem das Ziel am genauesten vorhergesagt wird.
* Ein ML-Modell, das für die Produktion geeignet ist.

### <a name="how-to-do-it"></a>Vorgehensweise
In dieser Phase werden drei Hauptaufgaben durchgeführt:

* **Featureentwicklung**: Es werden Datenfeatures aus den Rohdaten erstellt, um das Trainieren des Modells zu ermöglichen.
* **Modelltraining**: Es wird nach dem Modell gesucht, mit dem die Frage am genauesten beantwortet wird, indem die Erfolgsmetriken verglichen werden.
* Ermitteln Sie, ob Ihr Modell **für die Produktion geeignet** ist.

#### <a name="31-feature-engineering"></a>3.1 Featureentwicklung
Die Featureentwicklung umfasst die Einbeziehung, Aggregation und Transformation von Rohvariablen zum Erstellen der Features, die in der Analyse verwendet werden. Wenn Sie einen Einblick in die Grundlagen eines Modells erhalten möchten, müssen Sie verstehen, wie Features miteinander in Beziehung stehen und wie die Machine Learning-Algorithmen diese Features nutzen sollen. Dieser Schritt erfordert eine kreative Kombination aus Sachkenntnis und den im Datenuntersuchungsschritt gewonnenen Erkenntnissen. Dies ist ein Balanceakt, weil Variablen mit hohem Informationsgehalt gesucht und eingefügt werden, während gleichzeitig vermieden werden muss, zu viele irrelevante Variablen zu verwenden. Mit informativen Variablen verbessern wir unser Ergebnis, und mit wenig relevanten Variablen überladen wir das Modell unnötig. Sie müssen diese Features auch für alle neuen Daten generieren, die während der Bewertung ermittelt werden. Die Generierung dieser Features kann also nur von Daten abhängen, die zum Zeitpunkt der Bewertung verfügbar sind. Eine technische Anleitung zur Featureentwicklung bei Verwendung verschiedener Azure-Datentechnologien finden Sie unter [Featureentwicklung im Cortana-Analyseprozess](machine-learning-data-science-create-features.md). 

#### <a name="32-model-training"></a>3.2 Modelltraining
Je nach Art der Frage, die beantwortet werden soll, stehen ggf. relativ viele Algorithmen für die Modellierung zur Verfügung. Eine Anleitung zur Auswahl der Algorithmen finden Sie unter [Auswählen von Algorithmen für Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md). Dieser Artikel ist zwar für Azure Machine Learning geschrieben, aber die darin enthaltenen Informationen sind auch für andere ML-Frameworks nützlich. 

Der Prozess für das Modelltraining umfasst die folgenden Schritte: 

* Teilen Sie die Eingabedaten beliebig für die Modellierung eines Trainingsdatasets und eines Testdatasets auf.
* Erstellen Sie die Modelle mit dem Trainingsdataset.
* Werten Sie eine Reihe von konkurrierenden Machine Learning-Algorithmen (Trainings- und Testdataset) zusammen mit den verschiedenen zugehörigen Optimierungsparametern (als Parameter-Sweeping bezeichnet) aus, die auf die Beantwortung der jeweiligen Fragen mit den aktuellen Daten ausgerichtet sind.
* Ermitteln Sie die „beste“ Lösung für die Beantwortung der Frage, indem Sie die Erfolgsmetrik für die alternativen Methoden vergleichen.

> [!NOTE]
> **Vermeiden von Datenlecks**: Datenlecks können dadurch verursacht werden, dass Daten von außerhalb des Trainingsdatasets einbezogen werden, sodass ein Modell oder ein Machine Learning-Algorithmus unrealistisch gute Vorhersagen ergibt. Datenlecks sind häufig der Grund dafür, warum Data Scientists nervös werden, wenn die Vorhersageergebnisse zu schön sind, um wahr zu sein. Es kann schwierig sein, diese Abhängigkeiten zu erkennen. Um dies zu vermeiden, ist häufig das wiederholte Durchlaufen der Schritte zur Erstellung eines Analysedatasets, Erstellung eines Modells und Auswertung der Genauigkeit erforderlich. 
> 
> 

Wir stellen für TDSP ein [Tool für die Automatisierung der Modellierung und Berichterstellung](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/Modeling) bereit, mit dem mehrere Algorithmen und Parameter-Sweeping-Vorgänge durchlaufen werden können, um ein Grundmodell zu erhalten. Außerdem wird ein Basisbericht für die Modellierung erstellt, in dem die Leistung der einzelnen Modell/Parameter-Kombinationen, einschließlich Variablenwichtigkeit, zusammengefasst wird. Dieser Prozess ist ebenfalls iterativ, da er weitere Maßnahmen zur Featureentwicklung nach sich ziehen kann. 

### <a name="artifacts"></a>Artefakte
In dieser Phase werden die folgenden Artefakte produziert:

* [**Funktionssätze**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/Data%20Defintion.md#feature-sets): Die für die Modellierung entwickelten Funktionen werden im Abschnitt „Feature Set“ (Funktionssatz) des Data Definition-Berichts beschrieben. Er enthält Verknüpfungen zum Code, mit dem die Features generiert werden, und eine Beschreibung zur Generierung der Features.
* [**Bericht zur Modellierung**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Model/Model%201/Model%20Report.md): Für jedes Modell, das ausprobiert wird, wird ein Standardbericht anhand einer angegebenen TDSP-Vorlage erstellt.
* **Prüfpunktentscheidung**: Werten Sie aus, ob das Modell gut genug funktioniert, um in einem Produktionssystem bereitgestellt zu werden. Einige wichtige Fragen lauten:
  * Kann die Frage mit dem Modell basierend auf den Testdaten ausreichend beantwortet werden? 
  * Sollten Alternativen ausprobiert werden: Erfassung weiterer Daten, weitere Featureentwicklung oder Experimente mit anderen Algorithmen?

## <a name="4-deployment"></a>4. Bereitstellung
### <a name="goal"></a>Zielsetzung
* Modelle und die Pipeline werden in einer Produktionsumgebung oder produktionsähnlichen Umgebung bereitgestellt, um die endgültige Benutzerakzeptanz zu testen. 

### <a name="how-to-do-it"></a>Vorgehensweise
In dieser Phase geht es um die folgende Hauptaufgabe:

* **Operationalisieren des Modells**: Stellen Sie das Modell und die Pipeline in einer Produktionsumgebung oder produktionsähnlichen Umgebung zur Nutzung der Anwendung bereit.

#### <a name="41-operationalize-a-model"></a>4.1 Operationalisieren eines Modells
Sobald wir eine Gruppe von Modellen mit adäquater Leistung haben, können diese für die Nutzung durch andere Anwendungen operationalisiert werden. Abhängig von den Geschäftsanforderungen erfolgen Vorhersagen entweder in Echtzeit oder auf Batchbasis. Um operationalisiert zu werden, müssen die Modelle mit einer offenen API-Schnittstelle verfügbar gemacht werden, die von verschiedenen Anwendungen ohne Schwierigkeiten genutzt werden kann, z.B. von Onlinewebsites, Kalkulationstabellen, Dashboards bzw. Branchen- oder Back-End-Anwendungen. Beispiele für die Operationalisierung von Modellen mit einem Azure Machine Learning-Webdienst finden Sie unter [Bereitstellen eines Azure Machine Learning-Webdiensts](machine-learning-publish-a-machine-learning-web-service.md). Es ist auch eine gute Idee, in das Produktionsmodell und die bereitgestellte Datenpipeline Telemetriedaten und eine Überwachungsfunktion zu integrieren, um die Erstellung von Statusberichten und die Problembehandlung des Systems zu unterstützen.  

### <a name="artifacts"></a>Artefakte
* Statusdashboard mit Systemintegrität und wichtigen Metriken.
* Endgültiger Modellierungsbericht mit Bereitstellungsdetails.
* Dokument mit der endgültigen Lösungsarchitektur.

## <a name="5-customer-acceptance"></a>5. Kundenakzeptanz
### <a name="goal"></a>Zielsetzung
* **Abschließen der Projektergebnisse**: Vergewissern Sie sich, dass die Pipeline, das Modell und die Bereitstellung in einer Produktionsumgebung die Anforderungen an die Kundenziele erfüllen.

### <a name="how-to-do-it"></a>Vorgehensweise
In dieser Phase werden drei Hauptaufgaben durchgeführt:

* **Systemüberprüfung**: Vergewissern Sie sich, dass das bereitgestellte Modell und die Pipeline die Kundenanforderungen erfüllen.
* **Projektübergabe**: Dies ist die Übergabe an die Entität, die das System in der Produktion ausführt.

Der Kunde überprüft, ob das System seine geschäftlichen Anforderungen erfüllt und ob die Fragen mit akzeptabler Genauigkeit beantwortet werden, damit das System in der Produktion zur Nutzung durch die Clientanwendung bereitgestellt werden kann. Die gesamte Dokumentation wird abgeschlossen und überprüft. Die Übergabe des Projekts an die Entität, die für den Betrieb zuständig ist, wird vollzogen. Dies kann beispielsweise ein IT-Team bzw. Data Science-Team des Kunden oder ein Agent des Kunden sein, das bzw. der für die Ausführung des Systems in der Produktion zuständig ist. 

### <a name="artifacts"></a>Artefakte
Das Hauptartefakt, das in dieser letzten Phase produziert wird, ist der [**Projektabschlussbericht**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Project/Exit%20Report.md). Dies ist der technische Projektbericht mit allen Details des Projekts, die zum Erlernen und Betreiben des Systems hilfreich sind. Im Rahmen von TDSP wird eine [Vorlage](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Project/Exit%20Report.md) bereitgestellt, die unverändert oder nach Anpassung an bestimmte Clientanforderungen verwendet werden kann. 

## <a name="summary"></a>Zusammenfassung
Der [Team Data Science-Prozesslebenszyklus](http://aka.ms/datascienceprocess) wird als Sequenz der durchlaufenen Schritte modelliert, die als Anleitung für die erforderlichen Aufgaben zur Verwendung von Vorhersagemodellen dienen. Diese Modelle können in einer Produktionsumgebung bereitgestellt werden, damit sie zum Erstellen von intelligenten Anwendungen genutzt werden können. Das Ziel dieses Prozesslebenszyklus besteht darin, ein Data Science-Projekt auf dem Weg zu einem eindeutigen Vernetzungsziel voranzubringen. Bei Data Science geht es um die Erforschung und Ermittlung. Indem Sie dies gegenüber Ihrem Team und Ihren Kunden unter Verwendung eines gut definierten Satzes von Artefakten mit standardisierten Vorlagen kommunizieren, können Missverständnisse vermieden und die Chancen für einen erfolgreichen Abschluss eines komplexen Data Science-Projekts erhöht werden.

## <a name="next-steps"></a>Nächste Schritte
Vollständige exemplarische Vorgehensweisen, in denen sämtliche Schritte im Prozess für **bestimmte Szenarien** gezeigt werden, sind ebenfalls verfügbar. Sie sind mit Miniaturansichtsbeschreibungen im Thema [Exemplarische Vorgehensweisen für den Team Data Science-Prozess](data-science-process-walkthroughs.md) aufgeführt und verlinkt.




<!--HONumber=Nov16_HO3-->


