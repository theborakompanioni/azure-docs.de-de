---
title: "Was ist ein virtueller Computer für Data Science? | Microsoft Docs"
description: "Erfahren Sie die wichtigsten Szenarios, Features und ersten Schritte zu virtuellen Computern für Data Science – Umgebung und Toolkit, bereit für Analysen."
keywords: "Data Science-Tools, virtuelle Computer für Data Science, Tools für Data Science, Linux Data Science"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: d4f91270-dbd2-4290-ab2b-b7bfad0b2703
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2016
ms.author: bradsev
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 9d4df15fb4514817d9233ab4e5bd0eb65e5a95dc


---
# <a name="introduction-to-the-cloud-based-data-science-virtual-machine-for-linux-and-windows"></a>Einführung in den Cloud-basierten virtuellen Computer für Data Science für Linux und Windows
Der virtuelle Computer für Data Science ist ein benutzerdefiniertes VM-Image in der Microsoft Azure-Cloud, das speziell für Data Science konfiguriert wurde. Es hat viele beliebte Data Science und andere Tools vorinstalliert und vorkonfiguriert, damit Sie sofort intelligente Anwendungen für die erweiterte Analyse erstellen können. Es ist unter Windows Server 2012 oder OpenLogic 7.2 CentOS-basierten Linux-Versionen verfügbar. 

In diesem Thema wird erläutert, was Sie mit der Data Science-VM tun können, außerdem einige der wichtigsten Szenarios für die Verwendung des virtuellen Computers, sowie die wichtigsten Features für die Windows- und Linux-Versionen, und es enthält Anweisungen zu deren Verwendung.

## <a name="what-can-i-do-with-the-data-science-virtual-machine"></a>Was kann ich mit dem virtuellen Computer für Data Science tun?
Das Ziel der virtuellen Computer für Data Science ist, Daten-Experten aller Fähigkeiten und Rollen eine reibungslose Data Science-Umgebung bereitzustellen. Dieser virtuelle Computer spart Ihnen viel Zeit, die Sie aufwenden müssten, wenn Sie eine vergleichbare Umgebung selbst einrichten möchten. Stattdessen starten Sie Ihr Data Science-Projekt direkt in einer neu erstellten VM-Instanz. 

Die Data Science-VM ist für die Arbeit mit einer Vielzahl von Verwendungsszenarios entwickelt und konfiguriert. Sie können Ihre Umgebung nach oben oder unten skalieren, wie es das Projekt erfordert. Sie können Ihre bevorzugte Sprache zur Programmierung von Data Science-Aufgaben verwenden. Sie können andere Tools installieren und das System für Ihre Bedürfnisse exakt anpassen.

## <a name="key-scenarios"></a>Wichtige Szenarios
In diesem Abschnitt werden einige wichtige Szenarios genannt, für die Data Science-VM bereitgestellt werden kann.

### <a name="preconfigured-analytics-desktop-in-the-cloud"></a>Vorkonfigurierter Analyse-Desktop in der Cloud
Die Data Science-VM stellt eine Basiskonfiguration für Data Science-Teams bereit, die ihre lokalen Desktops mit einem verwalteten Cloud-Desktop ersetzen möchten. Diese Grundlage stellt sicher, dass alle Datenanalysten in einem Team ein konsistentes Setup zum Überprüfen von Experimenten und besserer Zusammenarbeit haben. Sie verringert auch Kosten durch Reduzieren der Sysadmin-Last und der notwendigen Zeit zum Auswerten, Installieren und Pflegen der verschiedenen Softwarepakete, die für erweiterte Analysen erforderlich sind.  

### <a name="data-science-training-and-education"></a>Data Science-Schulung und -Ausbildung
Unternehmens-Trainer und Lehrer, die Data Science Klassen unterrichten, stellen normalerweise ein Image eines virtuellen Computers bereit, um sicherzustellen, dass ihre Schüler eine konsistente Umgebung eingerichtet haben, und dass die Beispiele erwartungsgemäß funktionieren. Die Data Science-VM erstellt eine bedarfsgerechte Umgebung mit einem konsistenten Setup, das den Support erleichtert und Inkompatibilitäts-Probleme vermeidet. Wenn diese Umgebungen häufig bereitgestellt werden müssen, insbesondere für kürzere Schulungen, bringt dies erhebliche Vorteile.

### <a name="on-demand-elastic-capacity-for-large-scale-projects"></a>Bei Bedarf flexible Kapazität für umfangreiche Projekte
Data Science Hackathons/Wettbewerbe oder umfangreiche Datenmodelle und Auswertungen erfordern skalierte Hardwarekapazität, in der Regel für kurze Zeit. Die Data Science-VM kann dazu beitragen, die Data Science-Umgebung bei Bedarf schnell auf skalierten Servern zu replizieren, auf denen Experimente ausgeführt werden können, die leistungsstarke Computing-Ressourcen erfordern.

### <a name="short-term-experimentation-and-evaluation"></a>Kurzfristige Experimente und Auswertungen
Die Data Science-VM kann zur Auswertung oder zum Lernen von Tools wie Microsoft R Server, SQL Server, Visual Studio Tools, Jupyter, vertieftes Lernen/ML-Toolkits und neue Tools, die in der Community beliebt sind, mit minimalem Einrichtaufwand verwendet werden. Da die Data Science-VM schnell eingerichtet werden kann, kann sie in anderen kurzfristigen Szenarios wie z.B. der Replikation veröffentlichter Experimente, Ausführen von Demos, dem Folgen exemplarischer Vorgehensweisen in Online-Sitzungen oder für Konferenz-Demos verwendet werden.

## <a name="whats-included-in-the-data-science-vm"></a>Was ist in der Data Science-VM enthalten?
Der virtuelle Computer für Data Science hat viele beliebte Data Science-Tools bereits installiert und konfiguriert. Darüber hinaus enthält er Tools, die die Arbeit mit verschiedenen Azure-Daten und Analyse-Produkten erleichtern. Sie können Vorhersagemodelle für umfangreiche Datasets mithilfe von Microsoft R Server oder SQL Server 2016 untersuchen und erstellen. Eine Reihe von anderen Tools der Open-Source-Community und von Microsoft sind ebenfalls enthalten, sowie Beispiel-Code und Notebooks. Die folgende Tabelle enthält eine Aufzählung und einen Vergleich der wichtigsten Komponenten Windows- und Linux-Editionen des virtuellen Computers für Data Science.

| **Windows-Edition** | **Linux-Edition** |
| --- | --- |
| Microsoft R Server Developer Edition |Microsoft R Server Developer Edition |
| Anaconda Python 2.7, 3.5 |Anaconda Python 2.7, 3.5 |
| Jupyter Notebook Server (R, Python) |JupyterHub: Multiuser Jupyter Notebooks (R, Python, Julia) |
| SQL Server 2016 Developer Edition: Skalierbare In-Database-Analyse mit R Services |Postgres, SQuirreL SQL (Datenbanktool), SQL Server-Treiber und Befehlszeile (bcp, sqlcmd) |
| Visual Studio Community Edition 2015 (IDE) </br> - Azure HDInsight (Hadoop), Data Lake, SQL Server Data Tools </br> - Node.js, Python, und R Tools für Visual Studio |IDEs und Editoren </br> - Eclipse mit Azure Toolkit Plugin </br> - Emacs (mit ESS, auctex) gedit |
| Power BI Desktop |-- |
| Machine Learning-Tools </br> - Integration mit Azure Machine Learning </br> - CNTK (vertieftes Lernen/AI) </br> - Xgboost (beliebtes ML-Tool in Data Science Wettbewerben) </br> - Vowpal Wabbit (schnelles Online-Lernsystem) </br> - Rattle (visuelles Schnellstart-Daten- und Analysetool) </br> - Mxnet (vertieftes Lernen/AI) |Machine Learning-Tools </br> - Integrationen mit Azure Machine Learning </br> - CNTK (vertieftes Lernen/AI) </br> - Xgboost (beliebtes ML-Tool in Data Science Wettbewerben) </br> - Vowpal Wabbit (schnelles Online-Lernsystem) </br> - Rattle (visuelles Schnellstart-Daten- und Analysetool) |
| SDKs zum Zugriff auf Azure und Cortana Intelligence Sammlung von Diensten |SDKs zum Zugriff auf Azure und Cortana Intelligence Sammlung von Diensten |
| Tools zum Verschieben von Daten und Verwalten von Azure- und Big Data-Ressourcen: Azure-Speicher-Explorer, CLI, PowerShell, AdlCopy (Azure Data Lake), AzCopy, dtui (für DocumentDB), Microsoft Data Management Gateway |Tools zum Verschieben von Daten und Verwaltung von Azure und Big Data-Ressourcen: Azure-Speicher-Explorer, CLI |
| Git, Visual Studio Team Services Plugin |Git |
| Windows-Port der gängigsten Linux/Unix-Befehlszeilenprogramme, die über GitBash/Eingabeaufforderung zugänglich sind |-- |

## <a name="how-to-get-started-with-the-windows-data-science-vm"></a>Erste Schritte mit der Windows Data Science-VM
* Erstellen Sie eine Instanz des virtuellen Computers unter Windows auf [dieser Seite](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) und wählen Sie die grüne Schaltfläche **Virtuellen Computer erstellen**.
* Melden Sie sich an dem virtuellen Computer aus Ihrem Remotedesktop unter Verwendung der Anmeldeinformationen an, die Sie beim Erstellen des virtuellen Computers angegeben haben.
* Die verfügbaren Tools sehen Sie durch Anklicken des **Start**-Menüs und können sie dort starten.

## <a name="get-started-with-the-linux-data-science-vm"></a>Erste Schritte mit der Linux Data Science-VM
* Erstellen Sie eine Instanz des virtuellen Computers unter Linux (OpenLogic CentOS-basiert) auf [dieser Seite](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/) und wählen Sie die Schaltfläche **Virtuellen Computer erstellen**.
* Melden Sie sich an dem virtuellen Computer von einem SSH-Client wie Putty oder SSH Command unter Verwendung der Anmeldeinformationen an, die Sie beim Erstellen des virtuellen Computers angegeben haben.
* Geben Sie in der Shell-Aufforderung „dsvm-more-info“ ein.
* Für einen grafischen Desktop laden Sie den X2Go-Client für die Clientplattform [hier](http://wiki.x2go.org/doku.php/doc:installation:x2goclient) herunter und befolgen Sie die Anweisungen im Linux Data Science VM Dokument [Bereitstellen der Linux Data Science Virtual Machine](machine-learning-data-science-linux-dsvm-intro.md#installing-and-configuring-x2go-client).

## <a name="next-steps"></a>Nächste Schritte
### <a name="for-the-windows-data-science-vm"></a>Für die Windows Data Science-VM
* Weitere Informationen zum Ausführen von bestimmten verfügbaren Tools in der Windows-Version finden Sie unter [Bereitstellen der Microsoft Data Science Virtual Machine](machine-learning-data-science-provision-vm.md).
* Weitere Informationen zum Ausführen verschiedener Aufgaben, die für Ihr Data Science-Projekt auf der Windows-VM erforderlich sind, finden Sie unter [Zehn Dinge, die Sie mit der Data Science Virtual Machine machen können](machine-learning-data-science-vm-do-ten-things.md).

### <a name="for-the-linux-data-science-vm"></a>Für die Linux Data Science-VM
* Weitere Informationen zum Ausführen von bestimmten verfügbaren Tools in der Linux-Version finden Sie unter [Bereitstellen der Linux Data Science Virtual Machine](machine-learning-data-science-linux-dsvm-intro.md).
* Eine exemplarische Vorgehensweise, wie Sie mehrere allgemeine Data Science-Aufgaben mit der Linux-VM ausführen, zeigt [Data Science auf der Linux Data Science Virtual Machine](machine-learning-data-science-linux-dsvm-walkthrough.md).




<!--HONumber=Nov16_HO3-->


