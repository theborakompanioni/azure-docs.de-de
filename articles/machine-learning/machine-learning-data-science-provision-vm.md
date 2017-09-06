---
title: Bereitstellen der Microsoft Data Science Virtual Machine | Microsoft-Dokumentation
description: "Konfigurieren und erstellen Sie für Analysen und Machine Learning einen virtuellen Computer für Data Science in Azure."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: e1467c0f-497b-48f7-96a0-7f806a7bec0b
ms.service: machine-learning
ms.workload: data-services
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: bradsev
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 76cd54cd234dfe43e8f0d61f0b66f0ed0c09e8b7
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="provision-the-microsoft-data-science-virtual-machine"></a>Bereitstellen der Microsoft Data Science Virtual Machine
Der virtuelle Microsoft Data Science-Computer ist das Image eines virtuellen Windows Azure-Computers (VM), das vorinstalliert und mit einigen gängigen Tools konfiguriert ist, die häufig für Datenanalysen und Machine Learning verwendet werden. Die enthaltenen Tools sind:

* Microsoft R Server Developer Edition
* Anaconda Python Distribution
* Jupyter Notebook (mit R, Python-Kernel)
* Visual Studio Community Edition
* Power BI Desktop
* SQL Server 2016 Developer Edition
* Machine Learning und Datenanalysetools
  * [Computational Network Toolkit (CNTK)](https://github.com/Microsoft/CNTK): Ein Deep Learning-Software-Toolkit von Microsoft Research.
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): Ein schnelles Machine Learning-System, das verschiedene Verfahren unterstützt, wie z.B. Online, Hashing, Allreduce, Reductions, Learning2Search, Active und Interactive Learning.
  * [XGBoost](https://xgboost.readthedocs.org/en/latest/): Ein Tool, das eine schnelle und präzise Boosted Tree-Implementierung ermöglicht.
  * [Rattle](http://rattle.togaware.com/) (R Analytical Tool To Learn Easily, R-Analysetool zum einfachen Lernen): Ein Tool, das die ersten Schritte von Datenanalysen und Machine Learning in R vereinfacht, indem eine GUI-basierte Datenuntersuchung und -modellierung mit automatischer R-Codegenerierung verwendet wird.
  * [mxnet](https://github.com/dmlc/mxnet): Eine für Effizienz und Flexibilität konzipierte Struktur für intensives Lernen.
  * [Weka](http://www.cs.waikato.ac.nz/ml/weka/): Eine Software für visuelles Data Mining und Machine Learning in Java.
  * [Apache Drill](https://drill.apache.org/): Ein schemaloses SQL-Abfragemodul für Hadoop, NoSQL und Cloud Storage.  Unterstützt ODBC- und JDBC-Schnittstellen, um NoSQL- und Dateiabfragen mit BI-Standardtools wie PowerBI, Excel und Tableau zu ermöglichen.
* Bibliotheken in R und Python zur Verwendung in Azure Machine Learning und anderen Azure-Diensten
* Git einschließlich Git Bash für den Einsatz mit Quellcoderepositorys einschließlich GitHub, Visual Studio Team Services
* Windows-Ports einiger gängiger Linux-Befehlszeilenprogramme (einschließlich awk, sed, perl, grep, find, wget, curl usw.), auf die über die Befehlszeile zugegriffen werden kann. 

Die Durchführung des Data Science-Vorgangs umfasst eine Aufgabensequenz:

1. Suchen, Laden und Vorverarbeiten von Daten
2. Erstellen und Testen von Modellen
3. Bereitstellen der Modelle für die Nutzung in intelligenten Anwendungen

Datenanalysten verwenden verschiedene Tools zum Ausführen dieser Aufgaben. Es kann sehr zeitaufwändig sein, die richtigen Versionen der Software zu finden und dann herunterzuladen und zu installieren. Die Microsoft Data Science Virtual Machine kann hier für Erleichterung sorgen, indem sie ein einsatzbereites Image bietet, das mit allen vorinstallierten und konfigurierten gängigen Tools auf Azure bereitgestellt werden kann. 

Mit der Microsoft Data Science Virtual Machine können Sie gleich mit Ihrem Analyseprojekt loslegen. Sie können in verschiedenen Sprachen arbeiten, z.B. R, Python, SQL und C++. Visual Studio bietet eine benutzerfreundliche IDE zum Entwickeln und Testen von Code. Mit dem Azure SDK der VM können Sie Ihre Anwendungen mit verschiedenen Diensten auf der Cloudplattform von Microsoft erstellen. 

Für dieses Data Science VM-Image fallen keine Softwaregebühren an. Sie zahlen nur die Azure-Nutzungsgebühren, die von der Größe des virtuellen Computers abhängen, die Sie bereitstellen. Weitere Informationen zu den Compute-Gebühren finden Sie auf der Seite [Data Science Virtual Machine](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) im Abschnitt „Preisübersicht“. 

## <a name="other-versions-of-the-data-science-virtual-machine"></a>Andere Versionen der Data Science Virtual Machine
Ein [CentOS](machine-learning-data-science-linux-dsvm-intro.md)-Image ist ebenfalls verfügbar, das viele gleiche Tools wie das Windows-Image bietet. Ein [Ubuntu](machine-learning-data-science-dsvm-ubuntu-intro.md)-Image ist ebenfalls verfügbar, das viele ähnliche Tools sowie umfangreiche Lerngerüste bietet.

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie eine Microsoft Data Science Virtual Machine erstellen können, benötigen Sie Folgendes:

* **Ein Azure-Abonnement**: Informationen zum Bezug finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Ein Azure-Speicherkonto**: Informationen zur Erstellung finden Sie unter [Erstellen eines Azure-Speicherkontos](../storage/common/storage-create-storage-account.md#create-a-storage-account). Alternativ dazu kann das Speicherkonto im Rahmen des Erstellungsprozesses der VM erstellt werden, wenn Sie kein vorhandenes Konto verwenden möchten.

## <a name="create-your-microsoft-data-science-virtual-machine"></a>Erstellen Ihrer Microsoft Data Science Virtual Machine
Es folgen die Schritte zum Erstellen einer Instanz der Microsoft Data Science Virtual Machine:

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm)zur Auflistung der virtuellen Computer.
2. Wählen Sie am unteren Seitenrand die Schaltfläche **Erstellen**, um einen Assistenten aufzurufen.![configure-data-science-vm](./media/machine-learning-data-science-provision-vm/configure-data-science-virtual-machine.png)
3. Der Assistent zum Erstellen der Microsoft Data Science Virtual Machine erfordert **Eingaben** für jeden der **fünf Schritte**, die in der Abbildung rechts aufgelistet sind. Die zum Konfigurieren der einzelnen Schritte erforderlichen Eingaben sind:
   
   1. **Grundlagen**
      
      1. **Name**: Name des Data Science-Servers, den Sie erstellen.
      2. **Benutzername**: ID des Administratoranmeldekontos.
      3. **Kennwort**: Kennwort des Administratorkontos.
      4. **Abonnement:**Wenn Sie über mehrere Abonnements verfügen, wählen Sie eines aus, über das der Computer erstellt und abgerechnet wird.
      5. **Ressourcengruppe**: Sie können eine neue Gruppe erstellen oder eine vorhandene Gruppe verwenden.
      6. **Standort**: Wählen Sie das Rechenzentrum aus, das am besten geeignet ist. Normalerweise ist es das Rechenzentrum, in dem der größte Teil Ihrer Daten gespeichert ist oder das Ihrem Standort am nächsten ist, um den schnellsten Netzwerkzugriff zu erreichen.
   2. **Größe**: Wählen Sie einen Servertyp aus, der die funktionalen Anforderungen und den Kostenrahmen erfüllt. Eine größere Auswahl an VM-Größen erhalten Sie durch die Auswahl von „Alle anzeigen“.
   3. **Einstellungen**:
      
      1. **Datenträgertyp**: Wählen Sie „Premium“, wenn Sie Solid-State-Laufwerke (SSD) bevorzugen, andernfalls „Standard“.
      2. **Speicherkonto**: Sie können ein neues Azure-Speicherkonto in Ihrem Abonnement erstellen oder ein vorhandenes Konto an dem *Standort* verwenden, der im Schritt **Grundlagen** des Assistenten ausgewählt wurde.
      3. **Andere Parameter**: In der Regel verwenden Sie einfach die Standardeinstellungen. Sie können den Mauszeiger über den Informationslink bewegen, um Hilfe zu einem bestimmten Feld anzuzeigen, falls Sie nicht die Standardeinstellungen verwenden möchten.
   4. **Zusammenfassung**: Stellen Sie sicher, dass alle eingegebenen Informationen richtig sind.
   5. **Kaufen**: Klicken Sie auf **Kaufen**, um die Bereitstellung zu starten. Ein Link zu den Bedingungen der Transaktion wird bereitgestellt. Für die VM gelten keine über die Computekosten für die Servergröße, die Sie im Schritt **Größe** ausgewählt haben, hinausgehenden Kosten. 

> [!NOTE]
> Die Bereitstellung sollte ungefähr 10 bis 20 Minuten dauern. Der Status der Bereitstellung wird im Azure-Portal angezeigt.
> 
> 

## <a name="how-to-access-the-microsoft-data-science-virtual-machine"></a>Zugreifen auf die Microsoft Data Science Virtual Machine
Nach der VM-Erstellung können Sie sich mithilfe von Remotedesktop mit den Anmeldeinformationen des Administratorkontos anmelden, die Sie zuvor im Abschnitt **Grundlagen** erstellt haben. 

Sobald Ihre VM erstellt und bereitgestellt wurde, können Sie beginnen, die installierten und konfigurierten Tools zu verwenden. Für viele der Tools gibt es Desktopsymbole und Kacheln im Startmenü. 

## <a name="how-to-create-a-strong-password-for-jupyter-and-start-the-notebook-server"></a>Erstellen eines sicheren Kennworts für Jupyter und Starten des Notebook-Servers
Standardmäßig ist der Jupyter Notebook-Server vorkonfiguriert und auf der VM deaktiviert, bis Sie ein Jupyter-Kennwort festlegen. Führen Sie zum Erstellen eines sicheren Kennworts für den auf dem Computer installierten Jupyter Notebook-Server den folgenden Befehl über eine Befehlseingabeaufforderung auf dem virtuellen Computer für Data Science aus, ODER doppelklicken Sie unter einem lokalen VM-Administratorkonto auf die bereitgestellte Desktopverknüpfung **Jupyter Set Password & Start** (Jupyter: Kennwort festlegen und starten).

    C:\dsvm\tools\setup\JupyterSetPasswordAndStart.cmd

Führen Sie erforderlichen Schritte aus, und wählen Sie ein sicheres Kennwort, wenn Sie dazu aufgefordert werden.

Mit dem vorherigen Skript wird ein Kennworthash erstellt und in der Jupyter-Konfigurationsdatei am Speicherort **C:\ProgramData\jupyter\jupyter_notebook_config.py** unter dem Parameternamen ***c.NotebookApp.password*** gespeichert.

Außerdem wird mit dem Skript der Jupyter-Server im Hintergrund aktiviert und ausgeführt. Der Jupyter-Server wird in der Windows-Aufgabenplanung als Windows-Task unter dem Namen **Start_IPython_Notebook** erstellt.  Unter Umständen müssen Sie nach dem Festlegen des Kennworts einige Sekunden warten, bevor Sie das Notebook in Ihrem Browser öffnen. Der Abschnitt **Jupyter Notebook** weiter unten enthält Informationen zum Zugriff auf den Jupyter Notebook-Server. 


## <a name="tools-installed-on-the-microsoft-data-science-virtual-machine"></a>In der Microsoft Data Science Virtual Machine installierte Tools

### <a name="microsoft-r-server-developer-edition"></a>Microsoft R Server Developer Edition
Für die Durchführung Ihrer Analysen ist auf der VM Microsoft R Server Developer Edition vorinstalliert. Microsoft R Server ist eine unterstützte, sichere und in großem Maße bereitstell- und skalierbare Analyseplattform für Unternehmen, die auf R basiert. Dank vielfältiger Big Data-Statistiken, Vorhersagemodellierung und Machine Learning-Funktionen unterstützt R Server Analysen vollumfänglich - von der Exploration über die Analyse bis hin zur Visualisierung und der Modellierung. Die Verwendung und Erweiterung der Open-Source-Sprache R sorgt für die vollständige Kompatibilität zwischen Microsoft R Server und in R geschriebenen Skripten, Funktionen und CRAN-Paketen für die Datenanalyse in Unternehmen. Mit der Möglichkeit, Daten parallel und in großen Stücken verarbeiten zu können, hebt Microsoft R Server die Speicherbeschränkungen der Open-Source-Sprache R auf. Dies ermöglicht Ihnen auch die Analyse von Daten, deren Größe die des Hauptspeichers bei weitem überschreitet.  Die auf dem virtuellen Computer befindliche Visual Studio Community Edition enthält die R-Tools für die Visual Studio-Erweiterung, die eine vollständige IDE für das Arbeiten mit R bietet. Sie können auch andere IDEs wie [RStudio](http://www.rstudio.com) herunterladen und verwenden. 

### <a name="python"></a>Python
Für die Entwicklung mithilfe von Python wurden Anaconda Python Distribution 2.7 und 3.5 installiert. Diese Distribution enthält die Python-Basisversion sowie etwa 300 der beliebtesten Pakete für Mathematik, Entwicklung und Datenanalysen. Sie können die Python-Tools für Visual Studio (PTVS), die mit der Visual Studio 2015 Community Edition installiert werden, oder eine der mit Anaconda gebündelten IDEs wie IDLE oder Spyder verwenden. Sie können eines dieser Tools über eine Suche auf der Suchleiste starten (**Windows-Taste** + **S**).

> [!NOTE]
> Um unter Anaconda Python 2.7 und 3.5 auf die Python-Tools für Visual Studio zu verweisen, müssen Sie für jede Version eine angepasste Umgebung erstellen. Um diese Umgebungspfade in Visual Studio 2015 Community Edition zu erstellen, navigieren Sie zu **Tools** -> **Python Tools** -> **Python Environments** und klicken auf **+ Custom**. 
> 
> 

Anaconda Python 2.7 wird unter C:\Anaconda installiert, Anaconda Python 3.5 unter C:\Anaconda\envs\py35. Ausführliche Schritte hierzu finden Sie in der [PTVS-Dokumentation](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) . 

### <a name="jupyter-notebook"></a>Jupyter Notebook
Zur Anaconda-Distribution gehört außerdem Jupyter Notebook, eine Umgebung zum Freigeben von Code und Analysen. Es wurde bereits ein Jupyter Notebook-Server mit Python 2.7, Python 3.4, Python 3.5 und R-Kernels konfiguriert. Es gibt ein Desktopsymbol namens „Jupyter Notebook“, um den Browser für den Zugriff auf den Notebook-Server zu starten. Wenn Sie den virtuellen Computer über Remotedesktop verwenden, können Sie auch [https://localhost:9999/](https://localhost:9999/) für den Zugriff auf den Jupyter Notebook-Server aufrufen, wenn Sie bei der VM angemeldet sind.

> [!NOTE]
> Fahren Sie fort, falls Sie Zertifikatwarnungen erhalten. 
> 
> 

Es sind mehrere Beispiele für Notebooks enthalten – eines für Python und eines für R. Beispiele für die Zusammenarbeit von Jupyter Notebooks mit Microsoft R Server, SQL Server 2016 R Services (datenbankinterne Analyse), Python, Microsoft Cognitive ToolKit (CNTK) für Deep Learning und anderen Azure-Technologien finden Sie, wenn Sie sich bei Jupyter anmelden. Der Link zu den Beispielen wird Ihnen auf der Startseite für die Notebooks angezeigt, nachdem Sie sich mit dem im vorherigen Schritt festgelegten Passwort am Jupyter Notebook authentifiziert haben. 

### <a name="visual-studio-2015-community-edition"></a>Visual Studio 2015 Community Edition
Visual Studio Community Edition ist auf der VM installiert. Dies ist eine kostenlose Version der beliebten IDE von Microsoft, die Sie zur Evaluierung und für kleine Teams verwenden können. Die Lizenzbedingungen können Sie [hier](https://www.visualstudio.com/support/legal/mt171547)prüfen.  Öffnen Sie Visual Studio durch Doppelklicken auf das Desktopsymbol oder über das **Startmenü** . Sie können auch nach Programmen suchen, indem Sie **Windows-Taste** + **S** drücken und „Visual Studio“ eingeben. Anschließend können Sie Projekte in Sprachen wie C#, Python, R, node.js erstellen. Es wurden bereits Plugins installiert, die Ihnen die Arbeit mit Azure-Diensten wie Azure Data Catalog, Azure, HDInsight (Hadoop, Spark) und Azure Data Lake erleichtern. 

> [!NOTE]
> Es könnte eine Meldung angezeigt werden, die besagt, dass der Evaluierungszeitraum abgelaufen ist. Sie müssen die Anmeldeinformationen Ihres Microsoft-Kontos eingeben oder ein neues kostenloses Konto erstellen, um Zugriff auf die Visual Studio Community Edition zu erhalten. 
> 
> 

### <a name="sql-server-2016-developer-edition"></a>SQL Server 2016 Developer Edition
Eine Developer-Version von SQL Server 2016 mit R Services für Datenbankanalysen wird auf dem virtuellen Computer bereitgestellt. R Services bietet eine Plattform zum Entwickeln und Bereitstellen von intelligenten Anwendungen. Sie können die reichhaltige und leistungsfähige R-Sprache sowie die vielen Pakete aus der Community zum Erstellen von Modellen und Generieren von Vorhersagen mithilfe Ihrer SQL Server-Daten nutzen. Da R Services (datenbankintern) die Sprache R in SQL Server integriert, können Sie datennahe Analysen durchführen. Dadurch vermeiden Sie die Kosten und Sicherheitsrisiken, die mit einer Datenverschiebung verbunden sind.

> [!NOTE]
> Die Developer-Edition von SQL Server 2016 kann für Entwicklungs- und Testzwecke verwendet werden. Sie benötigen eine Lizenz für die Ausführung in der Produktion. 
> 
> 

Sie können auf die SQL Server-Instanz zugreifen, indem Sie **SQL Server Management Studio**starten. Ihr VM-Name wird als Servername angegeben. Verwenden Sie die Windows-Authentifizierung, wenn Sie als Administrator unter Windows angemeldet sind. Sobald SQL Server Management Studio gestartet wurde, können Sie andere Benutzer erstellen, Datenbanken erstellen, Daten importieren und SQL-Abfragen ausführen. 

Um datenbankinterne Analysen mit Microsoft R zu ermöglichen, führen Sie den folgenden Befehl einmal nach der Anmeldung als Serveradministrator in SQL Server Management Studio aus. 

        CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS 

        (Please replace the %COMPUTERNAME% with your VM name)


### <a name="azure"></a>Azure
Mehrere Azure-Tools werden auf dem virtuellen Computer installiert:

* Es gibt eine Desktopverknüpfung, um auf die Azure SDK-Dokumentation zuzugreifen. 
* **AzCopy**wird zum Verschieben von Daten in Ihr und aus Ihrem Microsoft Azure Storage-Konto verwendet. Geben Sie **Azcopy** in einer Befehlszeile ein, um die Nutzung anzuzeigen. 
* **Microsoft Azure-Speicher-Explorer**: Wird verwendet, um die Objekte zu durchsuchen, die in Ihrem Azure Storage-Konto gespeichert sind, und Daten in Ihren Azure-Speicher und heraus zu übertragen. Sie können **Speicher-Explorer** in das Suchfeld eingeben oder über das Windows-Startmenü auf dieses Tool zugreifen. 
* **Adlcopy**: Wird zum Verschieben von Daten in Azure Data Lake verwendet. Geben Sie **adlcopy** in einer Befehlszeile ein, um die Nutzung anzuzeigen. 
* **dtui**: Wird zum Verschieben von Daten in und aus Azure Cosmos DB verwendet, einer NoSQL-Datenbank in der Cloud. Geben Sie **dtui** in eine Befehlszeile ein. 
* **Microsoft-Datenverwaltungsgateway**: Ermöglicht das Verschieben von Daten zwischen lokalen Datenquellen und der Cloud. Es wird in Tools wie Azure Data Factory verwendet. 
* **Microsoft Azure PowerShell**: Ein Tool zur Verwaltung Ihrer Azure-Ressourcen in der PowerShell-Skriptsprache, das auch auf Ihrem virtuellen Computer installiert ist. 

### <a name="power-bi"></a>Power BI
Um Dashboards und aussagekräftige Visualisierungen zu erstellen, wurde **Power BI Desktop** installiert. Verwenden Sie dieses Tool, um Daten aus verschiedenen Quellen abzurufen, Dashboards und Berichte zu erstellen und diese in der Cloud zu veröffentlichen. Weitere Informationen finden Sie auf der [Power BI](http://powerbi.microsoft.com) -Website. Sie finden den Power BI-Desktop im Startmenü. 

> [!NOTE]
> Für den Zugriff auf Power BI benötigen Sie ein Office 365-Konto. 
> 
> 

## <a name="additional-microsoft-development-tools"></a>Zusätzliche Microsoft-Entwicklungstools
Der [**Microsoft-Webplattform-Installer**](https://www.microsoft.com/web/downloads/platform.aspx) kann verwendet werden, um weitere Microsoft-Entwicklungstools zu suchen und herunterzuladen. Es gibt auch eine Verknüpfung mit dem Tool auf dem Desktop der Microsoft Data Science Virtual Machine.  

## <a name="important-directories-on-the-vm"></a>Wichtige Verzeichnisse auf dem virtuellen Computer
| Item | Verzeichnis |
| --- | --- |
| Jupyter Notebook-Serverkonfigurationen |C:\ProgramData\jupyter |
| Basisverzeichnis für Jupyter Notebook-Beispiele |c:\dsvm\notebooks |
| Weitere Beispiele |c:\dsvm\samples |
| Anaconda (Standard: Python 2.7) |c:\Anaconda |
| Anaconda Python 3.5-Umgebung |c:\Anaconda\envs\py35 |
| R Server (Standalone) – Instanzverzeichnis (standardmäßige R-Instanz basierend auf R3.2.2) |C:\Programme\Microsoft SQL Server\130\R_SERVER |
| R Server (datenbankintern) – Instanzverzeichnis (R3.2.2) |C:\Programme\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES |
| Microsoft R Open-Instanzverzeichnis (R3.3.1) |C:\Programme\Microsoft\MRO-3.3.1 |
| Verschiedene Tools |c:\dsvm\tools |

> [!NOTE]
> Bei Instanzen von Microsoft Data Science Virtual Machine, die vor 1.5.0 (vor dem 3. September 2016) erstellt wurden, unterscheidet sich die Verzeichnisstruktur geringfügig von der vorherigen Tabelle. 
> 
> 

## <a name="next-steps"></a>Nächste Schritte
Mit den folgenden Schritten können Sie noch mehr lernen und entdecken. 

* Verschaffen Sie sich einen Überblick über die verschiedenen Tools, die auf der Data Science-VM installiert sind, indem Sie das Startmenü öffnen und sich die Liste der installierten Tools ansehen.
* Navigieren Sie zu **C:\Programme\Microsoft SQL Server\130\R_SERVER\library\RevoScaleR\demoScripts**, um Beispiele der RevoScaleR-Bibliothek in R zu finden, die Datenanalysen auf Unternehmensebene unterstützt.  
* Lesen Sie den Artikel [Zehn Dinge, die Sie mit der Data Science Virtual Machine machen können](http://aka.ms/dsvmtenthings)
* Erfahren Sie, wie Sie mithilfe des [Team Data Science-Prozesses](https://azure.microsoft.com/documentation/learning-paths/data-science-process/)vollständige Analyselösungen systematisch erstellen.
* Öffnen Sie den [Cortana Intelligence-Katalog](http://gallery.cortanaintelligence.com) , um Beispiele der Cortana Intelligence Suite für die Bereiche Machine Learning und Datenanalyse zu erhalten. Wir haben auch ein Symbol im **Startmenü** und auf dem Desktop der virtuellen Maschine dieser Galerie bereitgestellt.


