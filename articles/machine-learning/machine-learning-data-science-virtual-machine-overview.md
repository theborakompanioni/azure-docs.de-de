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
ms.date: 04/13/2017
ms.author: gokuma
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: 77b4f634c2a30d0cbec92d34a7f83866541d7d84
ms.lasthandoff: 04/15/2017


---
# <a name="introduction-to-the-cloud-based-data-science-virtual-machine-for-linux-and-windows"></a>Einführung in den Cloud-basierten virtuellen Computer für Data Science für Linux und Windows
Der virtuelle Computer für Data Science (DSVM) ist ein benutzerdefiniertes VM-Image in der Microsoft Azure-Cloud, das speziell für Data Science konfiguriert wurde. Es hat viele beliebte Data Science und andere Tools vorinstalliert und vorkonfiguriert, damit Sie sofort intelligente Anwendungen für die erweiterte Analyse erstellen können. Er ist unter Windows Server 2012 und unter Linux verfügbar. Wir bieten eine Linux-Edition des DSVM in Linux-Distributionen auf der Basis von Ubuntu 16.04 LTS oder OpenLogic 7.2 CentOS an. 

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

### <a name="deep-learning"></a>Deep Learning
Der virtuelle Computer für Data Science kann zum Modelltraining mit Deep Learning-Algorithmen auf Basis von GPU-Hardware (Grafikprozessoren) verwendet werden. DSVM unterstützt Sie bei Bedarf bei der Verwendung von GPU-basierter Hardware in der Cloud, wenn Sie große Modelle trainieren oder Berechnungen in hoher Geschwindigkeit ausführen müssen, die GPU-Leistung nutzen.  Unter Windows bieten wir derzeit das [Deep Learning-Toolkit für DSVM](http://aka.ms/dsvm/deeplearning) als ein separates Add-On für DSVM an. Mit diesem Add-On werden automatisch GPU-Treiber, Frameworks und die GPU-Version der Deep Learning-Algorithmen beim Erstellen der VM-Instanz installiert. Unter Linux ist Deep Learning für GPUs nur in der Edition [Data Science Virtual Machine für Linux (Ubuntu)](http://aka.ms/dsvm/ubuntu) aktiviert. Sie können die Ubuntu-Edition der Data Science-VM in virtuellen Computern ohne GPU-Aktivierung in Azure bereitstellen. In diesem Fall werden jedoch die Deep Learning-Frameworks auf den CPU-Modus zurückgesetzt. Die DSVM-Edition für Linux auf CentOS-Basis enthält nur die CPU-Builds von einigen Deep Learning-Tools (CNTK, Tensorflow, MXNet). Diese sind jedoch nicht mit dem GPU-Treiber und den Frameworks vorinstalliert. 

## <a name="whats-included-in-the-data-science-vm"></a>Was ist in der Data Science-VM enthalten?
Der virtuelle Computer für Data Science hat viele beliebte Data Science- und Deep Learning-Tools bereits installiert und konfiguriert. Darüber hinaus enthält er Tools, die die Arbeit mit verschiedenen Azure-Daten und Analyse-Produkten erleichtern. Sie können Vorhersagemodelle für umfangreiche Datasets mithilfe von Microsoft R Server oder SQL Server 2016 untersuchen und erstellen. Eine Reihe von anderen Tools der Open-Source-Community und von Microsoft sind ebenfalls enthalten, sowie Beispiel-Code und Notebooks. Die folgende Tabelle enthält eine Aufzählung und einen Vergleich der wichtigsten Komponenten Windows- und Linux-Editionen des virtuellen Computers für Data Science.


| **Tool**                                                           | **Windows-Edition** | **Linux-Edition** |
| :------------------------------------------------------------------ |:-------------------:|:------------------:|
| [Microsoft R Open](https://mran.microsoft.com/open/) mit verbreiteten vorinstallierten Paketen   |J                      | J             |
| [Microsoft R Server](https://msdn.microsoft.com/microsoft-r/) Developer Edition enthält: <br />  &nbsp;&nbsp;&nbsp;&nbsp;* [ScaleR](https://msdn.microsoft.com/microsoft-r/scaler-getting-started) – paralleles und verteiltes Hochleistungs-R-Framework<br />  &nbsp;&nbsp;&nbsp;&nbsp;* [MicrosoftML](https://msdn.microsoft.com/microsoft-r/microsoftml-introduction) – neue moderne ML-Algorithmen von Microsoft <br />  &nbsp;&nbsp;&nbsp;&nbsp;* [Operationalisierung von R](https://msdn.microsoft.com/microsoft-r/operationalize/about)                                            |J                      | J <br/> (MicrosoftML noch nicht verfügbar)|
| [Anaconda Python](https://www.continuum.io/) 2.7 und 3.5 mit beliebten vorinstallierten Paketen    |J                      |J              |
| [JuliaPro](https://juliacomputing.com/products/juliapro.html) mit beliebten vorinstallierten Paketen                         |J                      |J              |
| Relationale Datenbanken                                                            | [SQL Server 2016 SP1](https://www.microsoft.com/sql-server/sql-server-2016) <br/> Developer Edition| [PostgreSQL](https://www.postgresql.org/) |
| Datenbanktools                                                       | * SQL Server Management Studio <br/>* SQL Server Integration Services<br/>* [bcp, sqlcmd](https://docs.microsoft.com/sql/tools/command-prompt-utility-reference-database-engine)<br /> * ODBC/JDBC-Treiber| * [SQuirreL SQL](http://squirrel-sql.sourceforge.net/) (Abfrage-Tool), <br /> * bcp, sqlcmd <br /> * ODBC/JDBC-Treiber|
| Skalierbare In-Database-Analyse mit SQL Server R Services | J     |N              |
| **[Jupyter-Notebook-Server](http://jupyter.org/) mit folgenden Kernels,**                                  | J     | J |
|     &nbsp;&nbsp;&nbsp;&nbsp;* R | J | J |
|     &nbsp;&nbsp;&nbsp;&nbsp;* Python 2.7 & 3.5 | J | J |
|     &nbsp;&nbsp;&nbsp;&nbsp;* Julia | J | J |
|     &nbsp;&nbsp;&nbsp;&nbsp;* PySpark | N | J |
|     &nbsp;&nbsp;&nbsp;&nbsp;* [Sparkmagic](https://github.com/jupyter-incubator/sparkmagic) | N | Y (nur Ubuntu) |
|     &nbsp;&nbsp;&nbsp;&nbsp;* SparkR     | N | J |
| JupyterHub (Notebook-Server für mehrere Benutzer)| N | J |
| **Entwicklungstools, IDEs und Code-Editoren**| | |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Visual Studio 2015 (Community Edition)](https://www.visualstudio.com/community/) mit Git-Plug-In, Azure HDInsight (Hadoop), Data Lake, SQL Server Data Tools, [Node.js](https://github.com/Microsoft/nodejstools), [Python](http://aka.ms/ptvs) und [R-Tools für Visual Studio (RTVS)](http://microsoft.github.io/RTVS-docs/) | J | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Visual Studio Code](https://code.visualstudio.com/) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [RStudio Desktop](https://www.rstudio.com/products/rstudio/#Desktop) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [RStudio Server](https://www.rstudio.com/products/rstudio/#Server) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [PyCharm](https://www.jetbrains.com/pycharm/) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Atom](https://atom.io/) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Juno (Julia IDE)](http://junolab.org/)| J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* Vim und Emacs | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* Git und GitBash | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* OpenJDK | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* .NET Framework | J | N |
| Power BI Desktop | J | N |
| SDKs zum Zugriff auf Azure und Cortana Intelligence Sammlung von Diensten | J | J |
| **Datenverschiebungs- und -verwaltungstools** | | |
| &nbsp;&nbsp;&nbsp;&nbsp;* Azure-Speicher-Explorer | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/overview) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* Azure PowerShell | J | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Azcopy](https://docs.microsoft.com/azure/storage/storage-use-azcopy) | J | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Adlcopy (Azure Data Lake Storage)](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob) | J | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [DocDB-Datenmigrationstool](https://docs.microsoft.com/azure/documentdb/documentdb-import-data) | J | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Microsoft-Datenverwaltungsgateway:](https://msdn.microsoft.com/library/dn879362.aspx) Verschieben von Daten zwischen lokalen Speicherorten und der Cloud | J | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* Unix/Linux-Befehlszeilenprogramme | J | J |
| [Apache Drill](http://drill.apache.org) für das Durchsuchen von Daten | J | J |
| **Tools für maschinelles Lernen** |||
| &nbsp;&nbsp;&nbsp;&nbsp;* Integration mit [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) (R, Python) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Xgboost](https://github.com/dmlc/xgboost) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Weka](http://www.cs.waikato.ac.nz/ml/weka/) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Rattle](http://rattle.togaware.com/) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [LightGBM](https://github.com/Microsoft/LightGBM) | N | Y (nur Ubuntu) |
| &nbsp;&nbsp;&nbsp;&nbsp;* [H2O](https://www.h2o.ai/h2o/) | N | Y (nur Ubuntu) |
| **GPU-basierte Deep Learning-Tools** |[Deep Learning-Toolkit für DSVM](http://aka.ms/dsvm/deeplearning) verwenden |Nur Ubuntu-Edition|
| &nbsp;&nbsp;&nbsp;&nbsp;* [Microsoft Cognitive Toolkit (CNTK)](http://cntk.ai) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Tensorflow](https://www.tensorflow.org/) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [MXNet](http://mxnet.io/) | J | J|
| &nbsp;&nbsp;&nbsp;&nbsp;* [Caffe & Caffe2](https://github.com/caffe2/caffe2) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Torch](http://torch.ch/) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Theano](https://github.com/Theano/Theano) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Keras](https://keras.io/)| N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Nvidia Digits](https://github.com/NVIDIA/DIGITS) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* [CUDA, CUDNN, Nvidia-Treiber](https://developer.nvidia.com/cuda-toolkit) | J | J |
| **Big Data Platform (nur Devtest)**|||
| &nbsp;&nbsp;&nbsp;&nbsp;* Lokale [Spark](http://spark.apache.org/)-Instanz | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp;* Lokale [Hadoop](http://hadoop.apache.org/) (HDFS, YARN) | N | J |



## <a name="how-to-get-started-with-the-windows-data-science-vm"></a>Erste Schritte mit der Windows Data Science-VM
* Erstellen Sie eine Instanz des virtuellen Computers unter Windows auf [dieser Seite](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) und wählen Sie die grüne Schaltfläche **Virtuellen Computer erstellen**.
* Melden Sie sich an dem virtuellen Computer aus Ihrem Remotedesktop unter Verwendung der Anmeldeinformationen an, die Sie beim Erstellen des virtuellen Computers angegeben haben.
* Die verfügbaren Tools sehen Sie durch Anklicken des **Start**-Menüs und können sie dort starten.

## <a name="get-started-with-the-linux-data-science-vm"></a>Erste Schritte mit der Linux Data Science-VM
* Erstellen einer Instanz des virtuellen Computers unter Linux
  * Navigieren Sie für die OpenLogic CentOS-basierte Edition zu [dieser Seite](http://aka.ms/dsvm/centos), und wählen Sie die Schaltfläche **Jetzt anfordern** aus.
  * Navigieren Sie für die Ubuntu-Edition zu [dieser Seite](http://aka.ms/dsvm/ubuntu), und wählen Sie die Schaltfläche **Jetzt anfordern** aus.
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


