---
title: Bereitstellen der Linux Data Science Virtual Machine | Microsoft Docs
description: Konfigurieren und erstellen Sie eine Linux Data Science Virtual Machine in Azure, um Analysen und Machine Learning-Vorgänge durchzuführen.
services: machine-learning
documentationcenter: ''
author: bradsev
manager: jhubbard
editor: cgronlun

ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2016
ms.author: bradsev

---
# <a name="provision-the-linux-data-science-virtual-machine"></a>Bereitstellen der Linux Data Science Virtual Machine
Die Linux Data Science Virtual Machine ist ein virtueller Azure-Computer, der eine Sammlung von vorinstallierten Tools enthält. Diese Tools werden häufig für Datenanalysen und Machine Learning-Aufgaben verwendet. Die wichtigsten Softwarekomponenten sind:

* Microsoft R Server Developer Edition
* Anaconda Python-Distribution (Versionen 2.7 und 3.5), einschließlich gängiger Bibliotheken für die Datenanalyse
* JupyterHub: Jupyter Notebook-Server für mehrere Benutzer mit Unterstützung von R, Python, Julia-Kernels
* Azure-Speicher-Explorer
* Azure-Befehlszeilenschnittstelle für die Verwaltung von Azure-Ressourcen
* PostgreSQL-Datenbank
* Machine Learning-Tools
  * [Computational Network Toolkit (CNTK)](https://github.com/Microsoft/CNTK): Ein Deep Learning-Software-Toolkit von Microsoft Research.
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): Ein schnelles Machine Learning-System, das verschiedene Verfahren unterstützt, wie z.B. Online, Hashing, Allreduce, Reductions, Learning2Search, Active und Interactive Learning.
  * [XGBoost](https://xgboost.readthedocs.org/en/latest/): Ein Tool, das eine schnelle und präzise Boosted Tree-Implementierung ermöglicht.
  * [Rattle](http://rattle.togaware.com/) (R Analytical Tool To Learn Easily, R-Analysetool zum einfachen Lernen): Ein Tool, das die ersten Schritte von Datenanalysen und Machine Learning in R vereinfacht, indem eine GUI-basierte Datenuntersuchung und -modellierung mit automatischer R-Codegenerierung verwendet wird.
* Azure SDK in Java, Python, node.js, Ruby, PHP
* Bibliotheken in R und Python zur Verwendung in Azure Machine Learning und anderen Azure-Diensten
* Entwicklungstools und -Editoren (Eclipse, Emacs, gedit, vi)

Die Durchführung des Data Science-Vorgangs umfasst eine Aufgabensequenz:

1. Suchen, Laden und Vorverarbeiten von Daten
2. Erstellen und Testen von Modellen
3. Bereitstellen der Modelle für die Nutzung in intelligenten Anwendungen

Datenanalysten verwenden verschiedene Tools zum Ausführen dieser Aufgaben. Es kann sehr zeitaufwändig sein, die richtigen Versionen der Software zu finden und diese dann herunterzuladen, zu kompilieren und zu installieren.

Mit der Linux Data Science Virtual Machine können Sie diesen Aufwand erheblich reduzieren. Nutzen Sie sie, um Ihr Analyseprojekt entscheidend voranzubringen. Sie können in verschiedenen Sprachen arbeiten, z.B. R, Python, SQL, Java und C++. Eclipse verfügt über eine benutzerfreundliche IDE zum Entwickeln und Testen von Code. Mit dem in die VM integrierten Azure SDK können Sie Ihre Anwendungen mit verschiedenen Diensten unter Linux für die Microsoft-Cloudplattform erstellen. Außerdem haben Sie Zugriff auf andere Sprachen wie Ruby, Perl, PHP und node.js, die ebenfalls vorinstalliert sind.

Für dieses Data Science VM-Image fallen keine Softwaregebühren an. Sie zahlen nur die Gebühren für die Azure-Hardwarenutzung, die basierend auf der Größe des virtuellen Computers berechnet werden, den Sie mit dem VM-Image bereitstellen. Weitere Informationen zu Computegebühren finden Sie im [Azure Marketplace auf der Seite mit den VM-Preisen](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/).

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie eine Linux Data Science Virtual Machine erstellen können, benötigen Sie Folgendes:

* **Ein Azure-Abonnement**: Informationen zum Bezug finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/free/).
* **Ein Azure-Speicherkonto**: Informationen zur Erstellung finden Sie unter [Erstellen eines Azure-Speicherkontos](../storage/storage-create-storage-account.md#create-a-storage-account). Alternativ dazu kann das Speicherkonto im Rahmen des Erstellungsprozesses der VM erstellt werden, wenn Sie kein vorhandenes Konto verwenden möchten.

## <a name="create-your-linux-data-science-virtual-machine"></a>Erstellen Ihrer Linux Data Science Virtual Machine
Es folgen die Schritte zum Erstellen einer Instanz der Linux Data Science Virtual Machine:

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com/#create/microsoft-ads.linux-data-science-vmlinuxdsvm)zur Auflistung der virtuellen Computer.
2. Klicken Sie (unten auf der Seite) auf **Erstellen**, um den Assistenten aufzurufen.![configure-data-science-vm](./media/machine-learning-data-science-linux-dsvm-intro/configure-linux-data-science-virtual-machine.png)
3. Die folgenden Abschnitte enthalten die Eingaben für jeden Schritt des Assistenten (im rechten Teil der obigen Abbildung aufgelistet), mit dem die Microsoft Data Science Virtual Machine erstellt wird. Die zum Konfigurieren der einzelnen Schritte erforderlichen Eingaben sind:
   
   a. **Grundlagen**:
   
   * **Name**: Name des Data Science-Servers, den Sie erstellen.
   * **Benutzername**: Die ID für die erste Kontoanmeldung.
   * **Kennwort**: Das erste Kontokennwort (Sie können anstelle eines Kennworts einen öffentlichen SSH-Schlüssel verwenden).
   * **Abonnement:**Wenn Sie über mehrere Abonnements verfügen, wählen Sie eines aus, über das der Computer erstellt und abgerechnet wird. Sie müssen für dieses Abonnement über Berechtigungen zum Erstellen von Ressourcen verfügen.
   * **Ressourcengruppe**: Sie können eine neue Gruppe erstellen oder eine vorhandene Gruppe verwenden.
   * **Standort**: Wählen Sie das Rechenzentrum aus, das am besten geeignet ist. Normalerweise handelt es sich dabei um das Rechenzentrum, in dem der größte Teil Ihrer Daten gespeichert ist oder das Ihrem physischen Standort am nächsten ist, um den schnellsten Netzwerkzugriff zu erreichen.
   
   b. **Größe**:
   
   * Wählen Sie einen Servertyp aus, der die funktionalen Anforderungen und den Kostenrahmen erfüllt. Wählen Sie **Alle anzeigen** aus, um weitere Optionen für VM-Größen anzuzeigen.
   
   c. **Einstellungen**:
   
   * **Datenträgertyp**: Wählen Sie **Premium** aus, wenn Sie ein Festkörperlaufwerk (Solid State Drive, SSD) bevorzugen. Andernfalls wählen Sie **Standard**aus.
   * **Speicherkonto**: Sie können ein neues Azure-Speicherkonto in Ihrem Abonnement erstellen oder ein vorhandenes Konto an dem Standort verwenden, der im Schritt **Grundlagen** des Assistenten ausgewählt wurde.
   * **Andere Parameter**: In den meisten Fällen verwenden Sie einfach die Standardeinstellungen. Bewegen Sie den Mauszeiger über den jeweiligen Informationslink, um Hilfe zu bestimmten Feldern anzuzeigen, falls Sie auch nicht standardmäßige Einstellungen verwenden möchten.
   
   d. **Zusammenfassung**:
   
   * Stellen Sie sicher, dass alle eingegebenen Informationen richtig sind.
   
   e. **Kaufen**:
   
   * Klicken Sie auf **Kaufen**, um die Bereitstellung zu starten. Ein Link zu den Bedingungen der Transaktion wird bereitgestellt. Für die VM gelten keine über die Computekosten für die Servergröße, die Sie im Schritt **Größe** ausgewählt haben, hinausgehenden Kosten.

Die Bereitstellung sollte ungefähr 10 bis 20 Minuten dauern. Der Status der Bereitstellung wird im Azure-Portal angezeigt.

## <a name="how-to-access-the-linux-data-science-virtual-machine"></a>Zugreifen auf die Linux Data Science Virtual Machine
Nachdem die VM erstellt wurde, können Sie sich mithilfe von SSH an der VM anmelden. Verwenden Sie dabei die Kontoanmeldeinformationen, die Sie im Abschnitt **Grundlagen** von Schritt 3 für die Textshell-Schnittstelle erstellt haben. Unter Windows können Sie ein SSH-Clienttool wie [PuTTY](http://www.putty.org)herunterladen. Falls Sie einen grafischen Desktop bevorzugen (X Windows System), können Sie die X11-Weiterleitung von PuTTY verwenden oder den X2Go-Client installieren.

> [!NOTE]
> Der X2Go-Client hat in Tests eine erheblich bessere Leistung als X11 erzielt. Es wird empfohlen, den X2Go-Client als grafische Desktop-Benutzeroberfläche zu nutzen.
> 
> 

## <a name="installing-and-configuring-x2go-client"></a>Installieren und Konfigurieren des X2Go-Clients
Die Linux-VM wird mit X2Go-Server bereits bereitgestellt und ist zum Akzeptieren von Clientverbindungen bereit. Um eine Verbindung mit dem grafischen Linux-VM-Desktop herzustellen, führen Sie auf dem Client Folgendes durch:

1. Laden Sie den X2Go-Client für Ihre Clientplattform von [X2Go](http://wiki.x2go.org/doku.php/doc:installation:x2goclient)herunter, und installieren Sie ihn.    
2. Führen Sie den X2Go-Client aus, und wählen Sie die Option **Neue Sitzung**aus. Es wird ein Konfigurationsfenster mit mehreren Registerkarten geöffnet. Geben Sie die folgenden Konfigurationsparameter ein:
   * **Registerkarte „Sitzung“:**
     * **Host:**Hostname oder IP-Adresse Ihrer Linux Data Science VM.
     * **Anmeldung:**Benutzername für die Linux-VM.
     * **SSH-Port:**Übernehmen Sie den Standardwert 22.
     * **Sitzungstyp:**Ändern Sie den Wert in „XFCE“. Derzeit unterstützt die Linux-VM nur den XFCE-Desktop.
   * **Registerkarte „Medien“:**Sie können die Soundunterstützung und das Clientdrucken deaktivieren, wenn Sie diese Funktionen nicht benötigen.
   * **Freigegebene Ordner:**Wenn Verzeichnisse von Ihren Clientcomputern auf der Linux-VM bereitgestellt werden sollen, fügen Sie auf dieser Registerkarte die Clientcomputerverzeichnisse hinzu, die Sie für die VM freigeben möchten.

Nachdem Sie sich bei der VM angemeldet haben, indem Sie entweder den SSH-Client oder den grafischen XFCE-Desktop über den X2Go-Client nutzen, können Sie die Tools verwenden, die auf der VM installiert und konfiguriert sind. Auf dem XFCE-Desktop können Sie Anwendungsmenü-Tastenkombinationen und Desktopsymbole für viele Tools anzeigen.

## <a name="tools-installed-on-the-linux-data-science-virtual-machine"></a>In der Linux Data Science Virtual Machine installierte Tools
### <a name="microsoft-r-open"></a>Microsoft R Open
R ist eine der beliebtesten Sprachen für Datenanalyse und Machine Learning. Wenn Sie R für Ihre Analysen verwenden möchten, können Sie Microsoft R Open (MRO) mit der Math Kernel Library (MKL) auf der VM nutzen. Mit der MKL werden die mathematischen Vorgänge in Analysealgorithmen optimiert. MRO ist vollständig kompatibel mit CRAN-R, und alle in CRAN veröffentlichten R-Bibliotheken können auf der MRO-Plattform installiert werden. Sie können Ihre R-Programme in einem Standard-Editor wie vi, Emacs oder gedit bearbeiten. Sie können auch andere IDEs wie [RStudio](http://www.rstudio.com)herunterladen und verwenden. Der Einfachheit halber ist ein einfaches Skript (installRStudio.sh) im Verzeichnis **/dsvm/tools** enthalten, mit dem RStudio installiert wird. Wenn Sie den Emacs-Editor verwenden, beachten Sie, dass das ESS-Paket für Emacs (Emacs Speaks Statistics), mit dem die Verwendung von R-Dateien im Emacs-Editor vereinfacht wird, vorinstalliert ist.

Um R zu starten, geben Sie in der Shell einfach **R** ein. Sie gelangen zu einer interaktiven Umgebung. Zum Entwickeln Ihres R-Programms verwenden Sie normalerweise einen Editor wie Emacs, vi oder gedit und führen dann die Skripts in R aus. Wenn Sie RStudio installieren, verfügen Sie über eine vollständige grafische IDE zum Entwickeln Ihres R-Programms.

Es ist auch ein R-Skript vorhanden, mit dem Sie bei Bedarf die [20 beliebtesten R-Pakete](http://www.kdnuggets.com/2015/06/top-20-r-packages.html) installieren können. Dieses Skript kann ausgeführt werden, wenn Sie sich auf der interaktiven R-Benutzeroberfläche befinden. Sie können (wie bereits erwähnt) darauf zugreifen, indem Sie in der Shell **R** eingeben.  

### <a name="python"></a>Python
Für die Entwicklung mithilfe von Python wurden Anaconda Python Distribution 2.7 und 3.5 installiert. Diese Distribution enthält die Python-Basisversion sowie etwa 300 der beliebtesten Pakete für Mathematik, Entwicklung und Datenanalysen. Sie können die standardmäßigen Text-Editoren verwenden. Außerdem können Sie Spyder nutzen, eine Python-IDE, die als Bündel mit Anaconda Python-Distributionen bereitgestellt wird. Für Spyder wird ein grafischer Desktop oder die X11-Weiterleitung benötigt. Eine Verknüpfung mit Spyder wird auf dem grafischen Desktop bereitgestellt.

Da wir sowohl über Python 2.7 als auch Python 3.5 verfügen, müssen Sie die gewünschte Python-Version, mit der Sie in der aktuellen Sitzung arbeiten möchten, speziell aktivieren. Beim Aktivierungsprozess wird die PATH-Variable auf die gewünschte Version von Python festgelegt.

Führen Sie Folgendes über die Shell aus, um Python 2.7 zu aktivieren:

    source /anaconda/bin/activate root

Python 2.7 wird unter */anaconda/bin*installiert.

Führen Sie Folgendes über die Shell aus, um Python 3.5 zu aktivieren:

    source /anaconda/bin/activate py35


Python 3.5 wird unter */anaconda/envs/py35/bin*installiert.

Geben Sie zum Aufrufen einer interaktiven Python-Sitzung in der Shell einfach **python** ein. Wenn Sie sich auf einer grafischen Benutzeroberfläche befinden oder die X11-Weiterleitung eingerichtet haben, können Sie **spyder** eingeben, um die Python-IDE zu starten.

### <a name="jupyter-notebook"></a>Jupyter Notebook
Zur Anaconda-Distribution gehört außerdem Jupyter Notebook, eine Umgebung zum Freigeben von Code und Analysen. Auf Jupyter Notebook wird über JupyterHub zugegriffen. Sie melden sich mit Ihrem lokalen Linux-Benutzernamen und dem dazugehörigen Kennwort an.

Der Jupyter Notebook-Server wurde bereits mit Python 2-, Python 3- und R-Kernels vorkonfiguriert. Es gibt ein Desktopsymbol namens „Jupyter Notebook“, um den Browser für den Zugriff auf den Notebook-Server zu starten. Wenn Sie sich per SSH oder X2Go-Client bei der VM angemeldet haben, können Sie auch [https://localhost:8000/](https://localhost:8000/) besuchen, um auf den Jupyter Notebook-Server zuzugreifen.

> [!NOTE]
> Fahren Sie fort, falls Sie Zertifikatwarnungen erhalten.
> 
> 

Sie können von jedem Host aus auf den Jupyter Notebook-Server zugreifen. Geben Sie einfach *https://\<VM-DNS-Name oder -IP-Adresse\>:8000/* ein.

> [!NOTE]
> Port 8000 wird in der Firewall standardmäßig geöffnet, wenn der virtuelle Computer bereitgestellt wird.
> 
> 

Wir haben einige Beispiel-Notebooks zusammengestellt – eines für Python und eines für R. Der Link zu den Beispielen wird Ihnen auf der Startseite für die Notebooks angezeigt, nachdem Sie sich mit Ihrem lokalen Linux-Benutzernamen und dem dazugehörigen Kennwort bei dem Jupyter Notebook authentifiziert haben. Sie können ein neues Notebook erstellen, indem Sie **Neu** und dann den entsprechenden Sprachkernel wählen. Wenn die Schaltfläche **Neu** nicht angezeigt wird, klicken Sie oben links auf das **Jupyter**-Symbol, um die Startseite des Notebook-Servers zu öffnen.

### <a name="ides-and-editors"></a>IDEs und Editoren
Sie können zwischen mehreren Code-Editoren wählen. Beispiele hierfür sind vi/VIM, Emacs, gEdit und Eclipse. gEdit und Eclipse sind grafische Editoren, und Sie müssen bei einem grafischen Desktop angemeldet sein, um sie verwenden zu können. Diese Editoren verfügen über Verknüpfungen auf dem Desktop und im Anwendungsmenü, mit denen sie gestartet werden können.

**VIM** und **Emacs** sind textbasierte Editoren. Unter Emacs haben wir ein Add-On-Paket mit dem Namen Emacs Speaks Statistics (ESS) installiert, um die Nutzung von R im Emacs-Editor zu vereinfachen. Weitere Informationen finden Sie unter [ESS](http://ess.r-project.org/).

**Eclipse** ist eine erweiterbare Open-Source-IDE, die mehrere Sprachen unterstützt. Die Java-Entwickler-Edition ist die auf dem virtuellen Computer installierte Instanz. Es sind Plug-Ins für mehrere beliebte Sprachen vorhanden, die installiert werden können, um die Eclipse-Umgebung zu erweitern. In Eclipse ist auch ein Plug-In mit dem Namen **Azure-Toolkit für Eclipse**installiert. Damit können Sie Azure-Anwendungen mithilfe der Eclipse-Entwicklungsumgebung, die Sprachen wie Java unterstützt, erstellen, entwickeln, testen und bereitstellen. Es ist auch ein **Azure SDK für Java** vorhanden, das den Zugriff auf unterschiedliche Azure-Dienste aus einer Java-Umgebung ermöglicht. Weitere Informationen zum Azure-Toolkit für Eclipse finden Sie unter [Azure-Toolkit für Eclipse](../azure-toolkit-for-eclipse.md).

**LaTeX** wird über das texlive-Paket zusammen mit einem [aucte](https://www.gnu.org/software/auctex/manual/auctex/auctex.html)x-Paket als Emacs-Add-On installiert. Es dient zum Vereinfachen der Erstellung Ihrer LaTeX-Dokumente in Emacs.  

### <a name="databases"></a>Datenbanken
#### <a name="postgres"></a>Postgres
Die Open Source-Datenbank **Postgres** ist auf der VM verfügbar. Die Dienste werden ausgeführt, und initdb ist bereits abgeschlossen. Sie müssen trotzdem noch Datenbanken und Benutzer erstellen. Weitere Informationen finden Sie in der [Postgres-Dokumentation](https://www.postgresql.org/docs/).  

#### <a name="graphical-sql-client"></a>Grafischer SQL-Client
**SQuirrel SQL**, ein grafischer SQL-Client, wurde bereitgestellt, damit Verbindungen mit verschiedenen Datenbanken (z.B. Microsoft SQL Server, Postgres und MySQL) hergestellt und SQL-Abfragen ausgeführt werden können. Sie können dieses Tool über eine Sitzung des grafischen Desktops ausführen (z.B. mit dem X2Go-Client). Um SQuirrel SQL aufzurufen, können Sie entweder das Symbol auf dem Desktop verwenden oder den folgenden Befehl in der Shell ausführen.

    /usr/local/squirrel-sql-3.7/squirrel-sql.sh

Vor der ersten Verwendung müssen Sie Ihre Treiber und Datenbankaliase einrichten. Die JDBC-Treiber befinden sich am folgenden Speicherort:

*/usr/share/java/jdbcdrivers*

Weitere Informationen finden Sie unter [SQuirrel SQL](http://squirrel-sql.sourceforge.net/index.php?page=screenshots).

#### <a name="command-line-tools-for-accessing-microsoft-sql-server"></a>Befehlszeilentools für den Zugriff auf Microsoft SQL Server
Das ODBC-Treiberpaket für SQL Server verfügt auch über zwei Befehlszeilentools:

**bcp:**Mit diesem Hilfsprogramm werden Daten per Massenkopiervorgang zwischen einer Instanz von Microsoft SQL Server und einer Datendatei in einem vom Benutzer angegebenen Format kopiert. Das Hilfsprogramm bcp kann zum Importieren großer Mengen an neuen Zeilen in SQL Server-Tabellen oder zum Exportieren von Daten aus Tabellen in Datendateien verwendet werden. Zum Importieren von Daten in eine Tabelle müssen Sie entweder eine Formatdatei verwenden, die für diese Tabelle erstellt wurde, oder die Struktur der Tabelle und die Arten von Daten verstehen, die für die Tabellenspalten gültig sind.

Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit bcp](https://msdn.microsoft.com/library/hh568446.aspx).

**sqlcmd:**Mit dem Hilfsprogramm sqlcmd können Sie Transact-SQL-Anweisungen, Systemprozeduren und Skriptdateien an der Eingabeaufforderung eingeben. Für dieses Hilfsprogramm wird ODBC verwendet, um Transact-SQL-Batches auszuführen.

Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit sqlcmd](https://msdn.microsoft.com/library/hh568447.aspx).

> [!NOTE]
> Bei diesem Hilfsprogramm gibt es einige Unterschiede zwischen Linux- und Windows-Plattformen. Ausführliche Informationen dazu finden Sie in der -Dokumentation.
> 
> 

#### <a name="database-access-libraries"></a>Bibliotheken für den Datenbankzugriff
In R und Python sind Bibliotheken für den Zugriff auf Datenbanken verfügbar.

* In R können Sie mit den Paketen **RODBC** und **dplyr** SQL-Anweisungen auf dem Datenbankserver abfragen oder ausführen.
* In Python ermöglicht die Bibliothek **pyodbc** den Datenbankzugriff mit ODBC als zugrunde liegender Schicht.  

So greifen Sie auf **Postgres**zu

* Aus R: Verwenden Sie das Paket **RPostgreSQL**.
* Aus Python: Verwenden Sie die Bibliothek **psycopg2** .

### <a name="azure-tools"></a>Azure-Tools
Die folgenden Azure-Tools werden auf dem virtuellen Computer installiert:

* **Azure-Befehlszeilenschnittstelle:**Mit der Azure-Befehlszeilenschnittstelle können Sie Azure-Ressourcen über Shellbefehle erstellen und verwalten. Geben Sie zum Aufrufen der Azure-Tools einfach **azure help**ein. Weitere Informationen finden Sie auf der [Dokumentationsseite zur Azure-Befehlszeilenschnittstelle](../virtual-machines-command-line-tools.md).
* **Microsoft Azure-Speicher-Explorer:**Der Microsoft Azure-Speicher-Explorer ist ein grafisches Tool zum Navigieren durch die Objekte, die Sie in Ihrem Azure-Speicherkonto gespeichert haben, und zum Hoch- und Herunterladen der Daten in und aus Azure-Blobs. Sie können über das Symbol der Desktopverknüpfung auf den Speicher-Explorer zugreifen. Sie können ihn auch über eine Eingabeaufforderung der Shell aufrufen, indem Sie **StorageExplorer**eingeben. Sie müssen über einen X2Go-Client angemeldet sein oder die X11-Weiterleitung eingerichtet haben.
* **Azure-Bibliotheken**: Im Folgenden finden Sie einige der vorinstallierten Bibliotheken.
  
  * **Python**: Die installierten zu Azure gehörenden Bibliotheken in Python sind **azure**, **azureml**, **pydocumentdb** und **pyodbc**. Mit den ersten drei Bibliotheken können Sie auf Azure-Speicherdienste, Azure Machine Learning und Azure DocumentDB (eine NoSQL-Datenbank in Azure) zugreifen. Mit der vierten Bibliothek, pyodbc (zusammen mit dem Microsoft ODBC-Treiber für SQL Server), können Sie unter Verwendung einer ODBC-Schnittstelle über Python auf SQL Server, Azure SQL-Datenbank und Azure SQL Data Warehouse zugreifen. Geben Sie **pip list** ein, um alle aufgeführten Bibliotheken anzuzeigen. Achten Sie darauf, dass dieser Befehl sowohl in der Python 2.7- als auch in der Python 3.5-Umgebung ausgeführt wird.
  * **R**: Die installierten zu Azure gehörenden Bibliotheken in R sind **AzureML** und **RODBC**.
  * **Java**: Sie finden die Liste mit den Azure Java-Bibliotheken im Verzeichnis **/dsvm/sdk/AzureSDKJava** auf der VM. Die wichtigsten Bibliotheken sind Azure-Speicher- und -Verwaltungs-APIs, DocumentDB und JDBC-Treiber für SQL Server.  

Sie können über den vorinstallierten Firefox-Browser auf das [Azure-Portal](https://portal.azure.com) zugreifen. Im Azure-Portal können Sie Azure-Ressourcen erstellen, verwalten und überwachen.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Azure Machine Learning ist ein vollständig verwalteter Clouddienst, mit dem Sie Predictive Analytics-Lösungen erstellen, bereitstellen und freigeben können. Sie erstellen Ihre Experimente und Modelle mit Azure Machine Learning Studio. Sie können mit einem Webbrowser auf der Data Science Virtual Machine darauf zugreifen, indem Sie [Microsoft Azure Machine Learning](https://studio.azureml.net)besuchen.

Nachdem Sie sich bei Azure Machine Learning Studio angemeldet haben, haben Sie Zugriff auf einen Zeichenbereich zum Experimentieren, in dem Sie einen logischen Ablauf für die Machine Learning-Algorithmen erstellen können. Außerdem haben Sie Zugriff auf ein Jupyter Notebook, das in Azure Machine Learning gehostet wird und nahtlos für die Experimente in Machine Learning Studio genutzt werden kann. Operationalisieren Sie die von Ihnen erstellten Machine Learning-Modelle, indem Sie sie mit einer Webdienst-Schnittstelle umschließen. So können Clients, die in einer beliebigen Sprache geschrieben sind, Vorhersagen aus den Machine Learning-Modellen aufrufen. Weitere Informationen finden Sie in der [Dokumentation zu Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/).

Sie können Ihre Modelle auch in R oder Python auf der VM erstellen und diese dann in der Produktion in Azure Machine Learning bereitstellen. Wir haben Bibliotheken in R (**AzureML**) und Python (**azureml**) installiert, um diese Funktionalität zu ermöglichen.

Informationen zum Bereitstellen von Modellen in R und Python in Azure Machine Learning finden Sie im Artikel [Zehn Dinge, die Sie mit der Data Science Virtual Machine machen können](machine-learning-data-science-vm-do-ten-things.md) (hier insbesondere im Abschnitt „Erstellen von Modellen mit R oder Python und Operationalisieren dieser Modelle durch Verwenden von Azure Machine Learning“).

> [!NOTE]
> Diese Anweisungen wurden für die Windows-Version von Data Science VM geschrieben. Die Informationen zum Bereitstellen von Modellen für Azure Machine Learning gelten allerdings für die Linux-VM.
> 
> 

### <a name="machine-learning-tools"></a>Machine Learning-Tools
Die VM enthält einige Machine Learning-Tools und -Algorithmen, die vorkompiliert und lokal vorinstalliert wurden. Diese umfassen:

* **CNTK** (Computational Network Toolkit von Microsoft Research): Ein Deep Learning-Toolkit.
* **Vowpal Wabbit**: Ein Algorithmus für schnelles Onlinelernen.
* **xgboost**: Tool mit optimierten Boosted Tree-Algorithmen.
* **Python**: Anaconda Python wird als Paket mit Machine Learning-Algorithmen für Bibliotheken wie Scikit-learn bereitgestellt. Sie können andere Bibliotheken installieren, indem Sie den `pip install` -Befehl verwenden.
* **R**: Für R ist eine umfassende Bibliothek mit Machine Learning-Funktionen verfügbar. Einige Bibliotheken sind vorinstalliert, z.B. lm, glm, randomForest und rpart. Sie können andere Bibliotheken installieren, indem Sie Folgendes ausführen:
  
        install.packages(<lib name>)

Im Folgenden finden Sie einige zusätzliche Informationen zu den ersten drei Machine Learning-Tools in der Liste.

#### <a name="cntk"></a>CNTK
Dies ist ein Open-Source-Deep Learning-Toolkit. Es handelt sich um ein Befehlszeilentool (cntk), das sich bereits im Pfad (PATH) befindet.

Um ein einfaches Beispiel auszuführen, führen Sie die folgenden Befehle in der Shell aus:

    # Copy samples to your home directory and execute cntk
    cp -r /dsvm/tools/CNTK-2016-02-08-Linux-64bit-CPU-Only/Examples/Other/Simple2d cntkdemo
    cd cntkdemo/Data
    cntk configFile=../Config/Simple.cntk

Sie finden die Modellausgabe unter *~/cntkdemo/Output/Models*.

Weitere Informationen finden Sie auf [GitHub](https://github.com/Microsoft/CNTK) im Abschnitt zu CNTK und im [CNTK-Wiki](https://github.com/Microsoft/CNTK/wiki).

#### <a name="vowpal-wabbit"></a>Vowpal Wabbit
Vowpal Wabbit ist ein Machine Learning-System, das verschiedene Verfahren einsetzt, wie z.B. Online, Hashing, Allreduce, Reductions, Learning2Search, Active und Interactive Learning.

Gehen Sie wie folgt vor, um das Tool für ein sehr einfaches Beispiel auszuführen:

    cp -r /dsvm/tools/VowpalWabbit/demo vwdemo
    cd vwdemo
    vw house_dataset

In diesem Verzeichnis sind noch weitere, größere Demos enthalten. Informationen zu Vowpal Wabbit finden Sie [in diesem Abschnitt auf GitHub](https://github.com/JohnLangford/vowpal_wabbit) und im [Vowpal Wabbit-Wiki](https://github.com/JohnLangford/vowpal_wabbit/wiki).

#### <a name="xgboost"></a>XGBoost
Dies ist eine Bibliothek, die für Boosted (Tree)-Algorithmen entworfen und optimiert wurde. Das Ziel dieser Bibliothek besteht darin, die Rechenleistung von Computern erheblich zu verbessern, um Struktur-Boosting-Vorgänge in großem Umfang zu ermöglichen, die skalierbar, portabel und präzise sind.

Die Bibliothek wird als Befehlszeilentool und als R-Bibliothek bereitgestellt.

Um diese Bibliothek in R zu verwenden, können Sie eine interaktive R-Sitzung starten (indem Sie in der Shell einfach **R** eingeben) und die Bibliothek laden.

Hier ist ein einfaches Beispiel angegeben, das Sie an der Eingabeaufforderung von R ausführen können:

    library(xgboost)

    data(agaricus.train, package='xgboost')
    data(agaricus.test, package='xgboost')
    train <- agaricus.train
    test <- agaricus.test
    bst <- xgboost(data = train$data, label = train$label, max.depth = 2,
                    eta = 1, nthread = 2, nround = 2, objective = "binary:logistic")
    pred <- predict(bst, test$data)

Hier sind die Befehle für die Shell zum Ausführen der xgboost-Befehlszeile angegeben:

    cp -r /dsvm/tools/xgboost/demo/binary_classification/ xgboostdemo
    cd xgboostdemo
    xgboost mushroom.conf


Eine MODEL-Datei wird in das angegebene Verzeichnis geschrieben. Informationen zu diesem Demonstrationsbeispiel finden Sie [auf GitHub](https://github.com/dmlc/xgboost/tree/master/demo/binary_classification).

Weitere Informationen zu xgboost finden Sie auf der [xgboost-Dokumentationsseite](https://xgboost.readthedocs.org/en/latest/) und im zugehörigen [GitHub-Repository](https://github.com/dmlc/xgboost).

#### <a name="rattle"></a>Rattle
Rattle (**R** **A**nalytical **T**ool **T**o **L**earn **E**asily) verwendet GUI-basierte Durchsuchungs- und Modellierungsvorgänge für Daten. Mit dem Tool werden statistische und visuelle Zusammenfassungen von Daten dargestellt, Daten transformiert, die leicht modelliert werden können, nicht überwachte und überwachte Modelle aus den Daten erstellt, Leistungsdaten von Modellen grafisch dargestellt und neue Datasets bewertet. Außerdem wird R-Code generiert, mit dem die Vorgänge in der UI repliziert werden, die direkt in R ausgeführt oder als Ausgangspunkt für weitere Analysen verwendet werden können.

Um Rattle ausführen zu können, müssen Sie in einer grafischen Desktopsitzung angemeldet sein. Geben Sie am Terminal ```R``` ein, um die R-Umgebung aufzurufen. Geben Sie an der R-Eingabeaufforderung die folgenden Befehle ein:

    library(rattle)
    rattle()

Eine grafische Benutzeroberfläche mit einer Reihe von Registerkarten wird geöffnet. Im Folgenden finden Sie die Schnellstartschritte, die in Rattle erforderlich sind, um ein Beispiel-Wetterdataset zu verwenden und ein Modell zu erstellen. In einigen der folgenden Schritte werden Sie zum automatischen Installieren und Laden einiger erforderlicher R-Pakete aufgefordert, die sich nicht bereits im System befinden.

> [!NOTE]
> Falls Sie für die Installation des Pakets im Systemverzeichnis (Standardeinstellung) keinen Zugriff haben, wird unter Umständen in Ihrem R-Konsolenfenster eine Aufforderung mit der Frage angezeigt, ob Pakete in Ihrer persönlichen Bibliothek installiert werden sollen. Wählen Sie *y* , wenn diese Aufforderungen angezeigt werden.
> 
> 

1. Klicken Sie auf **Ausführen**.
2. Ein Dialogfeld wird geöffnet, in dem Sie gefragt werden, ob Sie das Beispiel-Wetterdataset verwenden möchten. Klicken Sie auf **Yes** , um das Beispiel zu laden.
3. Klicken Sie auf die Registerkarte **Model** .
4. Klicken Sie auf **Execute** , um eine Entscheidungsstruktur zu erstellen.
5. Klicken Sie auf **Draw** , um die Entscheidungsstruktur anzuzeigen.
6. Klicken Sie auf das Optionsfeld **Forest**, und klicken Sie auf **Execute**, um eine Random Forest zu erstellen.
7. Klicken Sie auf die Registerkarte **Evaluate** .
8. Klicken Sie auf das Optionsfeld **Risk**, und klicken Sie auf **Execute**, um zwei Leistungsdarstellungen (Risk [Cumulative]) anzuzeigen.
9. Klicken Sie auf die Registerkarte **Log** , um den generierten R-Code für die obigen Vorgänge anzuzeigen.
   (Aufgrund eines Fehlers in der aktuellen Version von Rattle müssen Sie im Text des Protokolls vor *Export this log ...* ein *#*-Zeichen eingeben.)
10. Klicken Sie auf die Schaltfläche **Export**, um die R-Skriptdatei *weather_script.R* im Basisordner zu speichern.

Sie können Rattle und R jetzt beenden. Als Nächstes können Sie das generierte R-Skript ändern, oder Sie können es unverändert verwenden und bei Bedarf ausführen, um die auf der Rattle-Benutzeroberfläche ausgeführten Schritte zu wiederholen. Dies ist besonders für R-Anfänger eine einfache Möglichkeit, auf einer einfachen grafischen Benutzeroberfläche schnell Analysen und Machine Learning-Vorgänge durchzuführen, während automatisch Code in R zum Ändern bzw. Lernen generiert wird.

## <a name="next-steps"></a>Nächste Schritte
Informieren Sie sich weiter:

* Die exemplarische Vorgehensweise [Data science on the Linux Data Science Virtual Machine](machine-learning-data-science-linux-dsvm-walkthrough.md) (Data Science auf der Linux Data Science Virtual Machine) zeigt Ihnen, wie Sie mehrere allgemeine Data Science-Aufgaben mit der hier bereitgestellten Linux Data Science VM ausführen. 
* Informieren Sie sich über die unterschiedlichen Data Science-Tools auf der Data Science VM, indem Sie die in diesem Artikel beschriebenen Tools ausprobieren. Sie können auch *dsvm-more-info* in der Shell auf dem virtuellen Computer ausführen, um eine grundlegende Einführung und Hinweise auf weitere Informationen zu den Tools auf der VM zu erhalten.  
* Erfahren Sie, wie Sie mithilfe des [Team Data Science-Prozesses](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/)systematisch vollständige Analyselösungen erstellen.
* Öffnen Sie den [Cortana Analytics-Katalog](http://gallery.cortanaanalytics.com) , um Anwendungsbeispiele der Cortana Analytics Suite für die Bereiche Machine Learning und Datenanalysen zu erhalten.

<!--HONumber=Oct16_HO2-->


