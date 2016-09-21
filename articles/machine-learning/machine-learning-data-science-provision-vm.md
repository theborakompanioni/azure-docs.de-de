<properties 
	pageTitle="Bereitstellen der Microsoft Data Science Virtual Machine | Microsoft Azure" 
	description="Konfigurieren und erstellen Sie für Analysen und Machine Learning einen virtuellen Computer für Data Science in Azure." 
	services="machine-learning" 
	documentationCenter="" 
	authors="bradsev" 
	manager="paulettm" 
	editor="cgronlun" />

<tags 
	ms.service="machine-learning" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="09/07/2016" 
	ms.author="bradsev" />


# Bereitstellen der Microsoft Data Science Virtual Machine

Die Microsoft Data Science Virtual Machine ist das Image eines virtueller Azure-Computers (VM), das vorinstalliert und mit einigen gängigen Tools konfiguriert ist, die häufig für Datenanalysen und Machine Learning verwendet werden. Die enthaltenen Tools sind:

- Microsoft R Server Developer Edition
- Anaconda Python Distribution
- Jupyter Notebook (mit R, Python-Kernel)
- Visual Studio Community Edition
- Power BI Desktop
- SQL Server 2016 Developer Edition
- Machine Learning-Tools
    - [Computational Network Toolkit (CNTK)](https://github.com/Microsoft/CNTK): Ein Deep Learning-Software-Toolkit von Microsoft Research.
    - [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): Ein schnelles Machine Learning-System, das verschiedene Verfahren unterstützt, wie z.B. Online, Hashing, Allreduce, Reductions, Learning2Search, Active und Interactive Learning.
    - [XGBoost](https://xgboost.readthedocs.org/en/latest/): Ein Tool, das eine schnelle und präzise Boosted Tree-Implementierung ermöglicht.
    - [Rattle](http://rattle.togaware.com/) (R Analytical Tool To Learn Easily, R-Analysetool zum einfachen Lernen): Ein Tool, das die ersten Schritte von Datenanalysen und Machine Learning in R vereinfacht, indem eine GUI-basierte Datenuntersuchung und -modellierung mit automatischer R-Codegenerierung verwendet wird.
    - [mxnet](https://github.com/dmlc/mxnet): Eine für Effizienz und Flexibilität konzipierte Struktur für intensives Lernen.
- Bibliotheken in R und Python zur Verwendung in Azure Machine Learning und anderen Azure-Diensten
- Git einschließlich Git Bash für den Einsatz mit Quellcoderepositorys einschließlich GitHub, Visual Studio Team Services
- Windows-Ports einiger gängiger Linux-Befehlszeilenprogramme (einschließlich awk, sed, perl, grep, find, wget, curl usw.), auf die über die Befehlszeile zugegriffen werden kann.


Data Science umfasst Iterationen einer Sequenz von Aufgaben: Suchen, Laden und Vorverarbeiten von Daten, Erstellen und Testen von Modellen sowie Bereitstellen der Modelle für die Nutzung in intelligenten Anwendungen. Es ist nicht ungewöhnlich, dass Datenanalysten eine Vielzahl von Tools für diese Aufgaben verwenden. Es kann sehr zeitaufwändig sein, die richtigen Versionen der Software zu finden und dann herunterzuladen und zu installieren. Die Microsoft Data Science Virtual Machine kann hier für Erleichterung sorgen, indem sie ein einsatzbereites Image bietet, das mit allen vorinstallierten und konfigurierten gängigen Tools auf Azure bereitgestellt werden kann.

Mit der Microsoft Data Science Virtual Machine können Sie gleich mit Ihrem Analyseprojekt loslegen. Sie können in einer Vielzahl von Sprachen, einschließlich R, Python, SQL und C#, an Aufgaben arbeiten. Visual Studio bietet eine benutzerfreundliche IDE zum Entwickeln und Testen von Code. Mit dem Azure SDK der VM können Sie Ihre Anwendungen mit verschiedenen Diensten auf der Cloudplattform von Microsoft erstellen.

Für dieses Data Science-VM-Image fallen keine Softwaregebühren an. Sie zahlen nur die Azure-Nutzungsgebühren, die von der Größe des virtuellen Computers abhängen, den Sie mit diesem VM-Image bereitstellen. Weitere Informationen zu den Computegebühren finden Sie [hier](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/).


## Voraussetzungen

Bevor Sie eine Microsoft Data Science Virtual Machine erstellen können, benötigen Sie Folgendes:

- **Ein Azure-Abonnement**: Informationen zum Bezug finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

*   **Ein Azure-Speicherkonto**: Um eines zu erstellen, siehe [Erstellen eines Azure-Speicherkontos](storage-create-storage-account.md#create-a-storage-account). Das Speicherkonto kann alternativ beim Erstellen der VM erstellt werden, wenn Sie kein vorhandenes Konto verwenden möchten.


## Erstellen Ihrer Microsoft Data Science Virtual Machine

Es folgen die Schritte zum Erstellen einer Instanz der Microsoft Data Science Virtual Machine:

1.	Wechseln Sie im [Azure-Portal](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm) zur Liste der virtuellen Computer.
2.	 Klicken Sie unten auf die Schaltfläche **Erstellen**, um einen Assistenten aufzurufen. ![configure-data-science-vm](./media/machine-learning-data-science-provision-vm/configure-data-science-virtual-machine.png)
3.	 Der Assistent zum Erstellen der Microsoft Data Science Virtual Machine erfordert **Eingaben** für jeden der **5 Schritte**, die in der Abbildung rechts aufgelistet sind. Die zum Konfigurieren der einzelnen Schritte erforderlichen Eingaben sind:

     **a. Grundlagen**:

   - **Name**: Name des Data Science-Servers, den Sie erstellen.
   - **Benutzername**: ID des Administratoranmeldekontos.
   - **Kennwort**: Kennwort des Administratorkontos.
   - **Abonnement:** Wenn Sie über mehrere Abonnements verfügen, wählen Sie eines aus, über das der Computer erstellt und abgerechnet wird.
   - **Ressourcengruppe**: Sie können eine neue Gruppe erstellen oder eine vorhandene Gruppe verwenden.
   - **Standort**: Wählen Sie das Rechenzentrum aus, das am besten geeignet ist. Normalerweise ist es das Rechenzentrum, in dem der größte Teil Ihrer Daten gespeichert ist oder das Ihrem Standort am nächsten ist, um den schnellsten Netzwerkzugriff zu erreichen.


     **b. Größe**:

   - Wählen Sie einen Servertyp aus, der die funktionalen Anforderungen und den Kostenrahmen erfüllt. Eine größere Auswahl an VM-Größen erhalten Sie durch die Auswahl von „Alle anzeigen“.

     **c. Einstellungen**:


   - **Datenträgertyp**: Wählen Sie „Premium“, wenn Sie Solid-State-Laufwerke (SSD) bevorzugen, andernfalls „Standard“.
  
   - **Speicherkonto**: Sie können ein neues Azure-Speicherkonto in Ihrem Abonnement erstellen oder ein vorhandenes an dem *Standort* verwenden, der im Schritt „Grundlagen“ des Assistenten ausgewählt wurde.
  
   - **Andere Parameter**: In den meisten Fällen verwenden Sie die Standardeinstellungen für die übrigen Parameter. Sie können den Mauszeiger über den Informationslink bewegen, um Hilfe zu einem bestimmten Feld anzuzeigen, falls Sie nicht die Standardeinstellungen verwenden möchten.


     **d. Zusammenfassung**:

   - Stellen Sie sicher, dass alle eingegebenen Informationen richtig sind.


     **e. Kaufen**:

   - Klicken Sie auf **Kaufen**, um die Bereitstellung zu starten. Ein Link zu den Bedingungen der Transaktion wird bereitgestellt. Für die VM gelten keine über die Computekosten für die Servergröße, die Sie im Schritt **Größe** ausgewählt haben, hinausgehenden Kosten.


Die Bereitstellung sollte ungefähr 10 bis 20 Minuten dauern. Der Status der Bereitstellung wird im Azure-Portal angezeigt.

## Zugreifen auf die Microsoft Data Science Virtual Machine

Nach der VM-Erstellung können Sie sich mithilfe von Remotedesktop mit den Anmeldeinformationen des Administratorkontos anmelden, die Sie im Abschnitt „Grundlagen“ von Schritt 4 erstellt haben.

Sobald Ihre VM erstellt und bereitgestellt wurde, können Sie beginnen, die installierten und konfigurierten Tools zu verwenden. Für viele der Tools gibt es Desktopsymbole und Kacheln im Startmenü.

## Erstellen eines sicheren Kennworts für den Jupyter Notebook-Server 

Führen Sie den folgenden Befehl über die Befehlszeile der Data Science Virtual Machine aus, um Ihr eigenes sicheres Kennwort für den darauf installierten Jupyter Notebook-Server zu erstellen.

	c:\anaconda\python.exe -c "import IPython;print IPython.lib.passwd()"

Verwenden Sie ein sicheres Kennwort, sobald Sie dazu aufgefordert werden.

Der Kennworthash wird in der Ausgabe im Format "sha1:xxxxxx" angezeigt. Kopieren Sie diesen Kennworthash, und ersetzen Sie den vorhandenen Hash in Ihrer Notebook-Konfigurationsdatei in diesem Verzeichnis: **C:\\ProgramData\\jupyter\\jupyter\_notebook\_config.py**. Verwenden Sie den Parameternamen ***c.NotebookApp.password***.

Ersetzen Sie nur den vorhandenen Hashwert innerhalb der Anführungszeichen. Die Anführungszeichen und das Präfix ***sha1:*** für den Parameterwert müssen beibehalten werden.

Zum Schluss müssen Sie den Jupyter-Server, der auf dem virtuellen Computer als geplanter Windows-Task mit der Bezeichnung „Start\_IPython\_Notebook“ ausgeführt wird, anhalten und erneut starten. Wenn Ihr neues Kennwort nach dem Neustart dieser Aufgabe nicht akzeptiert wird, versuchen Sie, alle ausgeführten Python-Prozesse im Task-Manager zu beenden und dann die obige geplante Aufgabe neu zu starten, ODER versuchen Sie, den virtuellen Computer neu zu starten.

## In der Microsoft Data Science Virtual Machine installierte Tools

### Microsoft R Server Developer Edition
Für die Durchführung Ihrer Analysen ist auf der VM Microsoft R Server Developer Edition vorinstalliert. Microsoft R Server ist eine unterstützte, sichere und in großem Maße bereitstell- und skalierbare Analyseplattform für Unternehmen, die auf R basiert. Dank vielfältiger Big Data-Statistiken, Vorhersagemodellierung und Machine Learning-Funktionen unterstützt R Server Analysen vollumfänglich - von der Exploration über die Analyse bis hin zur Visualisierung und der Modellierung. Die Verwendung und Erweiterung der Open-Source-Sprache R sorgt für die vollständige Kompatibilität zwischen Microsoft R Server und in R geschriebenen Skripten, Funktionen und CRAN-Paketen für die Datenanalyse in Unternehmen. Mit der Möglichkeit, Daten parallel und in großen Stücken verarbeiten zu können, hebt Microsoft R Server die Speicherbeschränkungen der Open-Source-Sprache R auf und ermöglicht den Benutzern so auch die Analyse von Daten, deren Größe die des Hauptspeichers bei weitem überschreitet. Die auf dem virtuellen Computer befindliche Visual Studio Community Edition enthält die R-Tools für die Visual Studio-Erweiterung, die eine vollständige IDE für das Arbeiten mit R bietet. Sie können auch andere IDEs wie [RStudio](http://www.rstudio.com) herunterladen und verwenden.

### Python
Für die Entwicklung mithilfe von Python wurden Anaconda Python Distribution 2.7 und 3.5 installiert. Diese Distribution enthält die Python-Basisversion sowie etwa 300 der beliebtesten Pakete für Mathematik, Entwicklung und Datenanalysen. Sie können die Python-Tools für Visual Studio (PTVS), die mit der Visual Studio 2015 Community Edition installiert werden, oder eine der mit Anaconda gebündelten IDEs wie IDLE oder Spyder verwenden. Sie können eines dieser Tools über eine Suche auf der Suchleiste starten (**Windows-Taste**+**S**).

>[AZURE.NOTE] Um unter Anaconda Python 2.7 und 3.5 auf die Python-Tools für Visual Studio zu verweisen, müssen Sie für jede Version eine angepasste Umgebung erstellen. Um diese Umgebungspfade in Visual Studio 2015 Community Edition zu erstellen, navigieren Sie zu **Tools** > **Python Tools** > **Python Environments**, und klicken Sie auf **+ Custom**.

Anaconda Python 2.7 wird unter C:\\Anaconda installiert, Anaconda Python 3.5 unter C:\\Anaconda\\envs\\py35. Ausführliche Schritte hierzu finden Sie in der [PTVS-Dokumentation](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it).

### Jupyter Notebook
Zur Anaconda Distribution gehört außerdem ein Jupyter Notebook, eine Umgebung zum Freigeben von Code und Analysen. Es wurde bereits ein Jupyter Notebook-Server mit Python 2, Python 3 und R-Kernels konfiguriert. Es gibt ein Desktopsymbol namens „Jupyter Notebook“, um den Browser für den Zugriff auf den Notebook-Server zu starten. Wenn Sie den virtuellen Computer über Remotedesktop verwenden, können Sie auch [https://localhost:9999/](https://localhost:9999/) für den Zugriff auf den Jupyter Notebook-Server aufrufen, wenn Sie bei der VM angemeldet sind.
 
>[AZURE.NOTE] Fahren Sie fort, falls Sie Zertifikatwarnungen erhalten.

Es sind Beispiele für Notebooks enthalten - eines für Python und eines für R. Der Link zu den Beispielen wird Ihnen auf der Startseite für die Notebooks angezeigt, nachdem Sie sich mit dem im vorherigen Schritt festgelegten Passwort am Jupyter Notebook authentifiziert haben. Beispiele für die Zusammenarbeit von Jupyter Notebooks mit Microsoft R Server, SQL Server 2016 R Services (datenbankinterne Analyse), Python und anderen Azure-Technologien finden Sie, wenn Sie sich bei Jupyter anmelden.

### Visual Studio 2015 Community Edition
Visual Studio Community Edition ist auf der VM installiert. Dies ist eine kostenlose Version der beliebten IDE von Microsoft, die Sie zur Evaluierung und für sehr kleine Teams verwenden können. Die Lizenzbedingungen können Sie [hier](https://www.visualstudio.com/support/legal/mt171547) prüfen. Öffnen Sie Visual Studio durch Doppelklicken auf das Desktopsymbol oder über das **Startmenü**. Sie können auch nach Programmen suchen, indem Sie die **Windows-Taste**+**S** drücken und „Visual Studio“ eingeben. Anschließend können Sie Projekte in Sprachen wie C#, Python, R, node.js erstellen. Zusätzlich werden Sie feststellen, dass bereits Plugins installiert wurden, die Ihnen die Arbeit mit Azure-Diensten wie Azure Data Catalog, Azure, HDInsight (Hadoop, Spark) und Azure Data Lake erleichtern.

>[AZURE.NOTE] Es könnte eine Meldung angezeigt werden, die besagt, dass der Evaluierungszeitraum abgelaufen ist. Sie müssen die Anmeldeinformationen Ihres Microsoft-Kontos eingeben oder ein kostenloses Konto erstellen, um Zugriff auf die Visual Studio Community Edition zu erhalten.

### SQL Server 2016 Developer Edition
Eine Developer-Version von SQL Server 2016 mit R Services für Datenbankanalysen wird auf dem virtuellen Computer bereitgestellt. R Services bietet eine Plattform zum Entwickeln und Bereitstellen von intelligenten Anwendungen, die Ihnen neue Einblicke verschaffen. Sie können die reichhaltige und leistungsfähige R-Sprache sowie die vielen Paketen aus der Community zum Erstellen von Modellen und Generieren von Vorhersagen mithilfe Ihrer SQL Server-Daten nutzen. Da R Services (datenbankintern) die Sprache R in SQL Server integriert, können Sie datennahe Analysen durchführen und damit Kosten ebenso vermeiden wie Sicherheitsrisiken, die mit einer Datenverschiebung verbunden sind. Die Developer-Edition von SQL Server 2016 kann für Entwicklungs- und Testzwecke verwendet werden (Sie benötigen eine Lizenz für die Ausführung in der Produktion).

Sie können auf die SQL Server-Instanz zugreifen, indem Sie **SQL Server Management Studio** starten. Ihr VM-Name wird als Servernamen angegeben. Verwenden Sie die Windows-Authentifizierung, wenn Sie als Administrator unter Windows angemeldet sind. Sobald SQL Server Management Studio gestartet wurde, können Sie andere Benutzer erstellen, Datenbanken erstellen, Daten importieren und SQL-Abfragen ausführen.

Um datenbankinterne Analysen mit Microsoft R zu ermöglichen, müssen Sie den folgenden Befehl einmal nach der Anmeldung als Serveradministrator in SQL Server Management Studio ausführen.

        CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS 
        
        (Please replace the %COMPUTERNAME% with your VM name)


### Azure 
Mehrere Azure-Tools werden auf dem virtuellen Computer installiert:
- Es gibt eine Desktopverknüpfung, um auf die Azure SDK-Dokumentation zuzugreifen.
- **AzCopy** wird zum Verschieben von Daten in Ihr und aus Ihrem Microsoft Azure Storage-Konto verwendet. Geben Sie einfach **Azcopy** auf einer Befehlszeile ein, um die Nutzung anzuzeigen.
- **Microsoft Azure-Speicher-Explorer**: Wird verwendet, um die Objekte zu durchsuchen, die in Ihrem Azure Storage-Konto gespeichert sind, und Daten in Ihren Azure-Speicher und heraus zu übertragen. Sie können **Speicher-Explorer** in das Suchfeld eingeben oder über das Windows-Startmenü auf dieses Tool zugreifen.
- **Adlcopy**: Wird zum Verschieben von Daten in Azure Data Lake verwendet. Geben Sie einfach **adlcopy** in einer Befehlszeile ein, um die Nutzung anzuzeigen.
- **dtui**: Wird zum Verschieben von Daten in und aus Azure DocumentDB verwendet, einer NoSQL-Datenbank in der Cloud. Geben Sie einfach **dtui** an der Eingabeaufforderung ein.
- **Microsoft Data Management Gateway**: Ermöglicht das Verschieben von Daten zwischen lokalen Datenquellen und der Cloud. Es wird in Tools wie Azure Data Factory verwendet.
- **Microsoft Azure PowerShell** ist ein Tool zur Verwaltung Ihrer Azure-Ressourcen in der PowerShell-Skriptsprache und auch auf Ihrem virtuellen Computer installiert.

###Power BI

Um Dashboards und aussagekräftige Visualisierungen zu erstellen, wurde **Power BI Desktop** installiert. Verwenden Sie dieses Tool, um Daten aus verschiedenen Quellen abzurufen, Dashboards und Berichte zu erstellen und diese in der Cloud zu veröffentlichen. Weitere Informationen finden Sie auf der [Power BI](http://powerbi.microsoft.com)-Website. Sie finden den Power BI-Desktop im Startmenü.

>[AZURE.NOTE] Für den Zugriff auf Power BI benötigen Sie ein Office 365-Konto.

## Zusätzliche Microsoft-Entwicklungstools
Der [**Microsoft-Webplattform-Installer**](https://www.microsoft.com/web/downloads/platform.aspx) kann verwendet werden, um weitere Microsoft-Entwicklungstools zu suchen und herunterzuladen. Es gibt auch eine Verknüpfung mit dem Tool auf dem Desktop der Microsoft Data Science Virtual Machine.

## Wichtige Verzeichnisse auf dem virtuellen Computer

| Item | Verzeichnis |
| ------------------------------| ---------------- |
|Jupyter Notebook-Serverkonfigurationen | C:\\ProgramData\\jupyter |
|Basisverzeichnis für Jupyter Notebook-Beispiele| c:\\dsvm\\notebooks|
|Weitere Beispiele | c:\\dsvm\\samples|
| Anaconda (Standard: Python 2.7) | c:\\Anaconda |
| Anaconda Python 3.5-Umgebung | c:\\Anaconda\\envs\\py35|
|R Server (Standalone) – Instanzverzeichnis (standardmäßige R-Instanz) | C:\\Programme\\Microsoft SQL Server\\130\\R\_SERVER |
| R-Server (datenbankintern) – Instanzverzeichnis | C:\\Programme\\Microsoft SQL Server\\MSSQL13.MSSQLSERVER\\R\_SERVICES |
| Verschiedene Tools | c:\\dsvm\\tools|

>[AZURE.NOTE] Bei Instanzen von Microsoft Data Science Virtual Machine, die vor 1.5.0 (vor dem 3. September 2016) erstellt wurden, unterscheidet sich die Verzeichnisstruktur geringfügig gegenüber der obigen.

## Nächste Schritte
Mit den folgenden Schritten können Sie noch mehr lernen und entdecken.

* Verschaffen Sie sich einen Überblick über die verschiedenen Tools, die auf der Data Science-VM installiert sind, indem Sie das Startmenü öffnen und sich die Liste der installierten Tools ansehen.
* Navigieren Sie zu **C:\\Programme\\Microsoft SQL Server\\130\\R\_SERVER\\library\\RevoScaleR\\demoScripts**, um Beispiele der RevoScaleR-Bibliothek in R zu finden, die Datenanalysen auf Unternehmensebene unterstützt.
* Lesen Sie den Artikel [Zehn Dinge, die Sie mit der Data Science Virtual Machine machen können](http://aka.ms/dsvmtenthings).
* Erfahren Sie, wie Sie mithilfe des [Team Data Science-Prozesses](https://azure.microsoft.com/documentation/learning-paths/data-science-process/) vollständige Analyselösungen systematisch erstellen.
* Öffnen Sie den [Cortana Intelligence-Katalog](http://gallery.cortanaintelligence.com), um Beispiele der Cortana Intelligence Suite für die Bereiche Machine Learning und Datenanalyse zu erhalten. Für den einfachen Zugriff haben wir auch ein Symbol auf dem Desktop und im Startmenü der virtuellen Maschine bereitgestellt.

<!---HONumber=AcomDC_0907_2016-->