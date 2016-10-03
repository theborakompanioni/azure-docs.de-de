<properties 
	pageTitle="Anpassen von Azure HDInsight Hadoop-Clustern für den Team Data Science-Prozess | Microsoft Azure" 
	description="Beliebte Python-Module, die in benutzerdefinierten Azure HDInsight Hadoop-Clustern zur Verfügung gestellt werden."
	services="machine-learning" 
	documentationCenter="" 
	authors="bradsev" 
	manager="jhubbard" 
	editor="cgronlun"  />

<tags 
	ms.service="machine-learning" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="09/19/2016" 
	ms.author="hangzh;bradsev" />

# Anpassen von Azure HDInsight Hadoop-Clustern für den Team Data Science-Prozess 

In diesem Artikel wird beschrieben, wie ein HDInsight Hadoop-Cluster durch die Installation der 64-Bit-Version von Anaconda (Python 2.7) auf jedem Knoten angepasst wird, wenn der Cluster als HDInsight-Dienst bereitgestellt wird. Es wird auch gezeigt, wie Sie auf den Hauptknoten zugreifen, um benutzerdefinierte Aufträge an den Cluster zu übermitteln. Diese Anpassung macht viele beliebte Python-Module verfügbar, die Bestandteil von Anaconda sind. Dies geschieht sehr benutzerfreundlich in UDFs (User Defined Function, benutzerdefinierte Funktion), die entwickelt wurden, um Hive-Datensätze im Cluster zu verarbeiten. Anweisungen zu den in diesem Szenario verwendeten Prozeduren finden Sie unter [Gewusst wie: Übermitteln von Hive-Abfragen](machine-learning-data-science-move-hive-tables.md#submit).

Das nachstehende Menü enthält Links zu Themen, in denen das Einrichten der verschiedenen Data Science-Umgebungen beschrieben wird, die vom [Team Data Science-Prozess (TDSP)](data-science-process-overview.md) verwendet werden.

[AZURE.INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]


## <a name="customize"></a>Anpassen von Azure HDInsight Hadoop-Clustern

Zum Erstellen eines angepassten HDInsight Hadoop-Clusters müssen Sie sich beim [**klassischen Azure-Portal**](https://manage.windowsazure.com/) anmelden. Klicken Sie links unten auf **New** und wählen Sie dann DATA SERVICES -> HDINSIGHT -> **CUSTOM CREATE** aus, um das Fenster **Cluster Details** zu öffnen.

![Arbeitsbereich erstellen](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

Geben Sie den Namen des zu erstellenden Clusters auf der 1. Konfigurationsseite ein, und übernehmen Sie die Standardwerte für die anderen Felder. Klicken Sie auf den Pfeil, um zur nächsten Konfigurationsseite zu wechseln.

![Arbeitsbereich erstellen](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

Geben Sie auf der 2. Konfigurationsseite die Anzahl der **DATA NODES** ein, wählen Sie eine Option für **REGION/VIRTUAL NETWORK** aus und wählen Sie dann die Größe für **HEAD NODE** und **DATA NODE** aus. Klicken Sie auf den Pfeil, um zur nächsten Konfigurationsseite zu wechseln.

>[AZURE.NOTE] Der Wert für **REGION/VIRTUAL NETWORK** muss mit der Region des Speicherkontos identisch sein, das für den HDInsight Hadoop-Cluster verwendet werden soll. Andernfalls wird auf der vierten Konfigurationsseite nicht das gewünschte Speicherkonto in der Dropdownliste **ACCOUNT NAME** angezeigt.

![Arbeitsbereich erstellen](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img3.png)

Geben Sie auf der 3. Konfigurationsseite einen Benutzernamen und ein Kennwort für den HDInsight Hadoop-Cluster ein. **Wählen Sie nicht** _Enter the Hive/Oozie Metastore_ aus. Klicken Sie anschließend auf den Pfeil, um zur nächsten Konfigurationsseite zu wechseln.

![Arbeitsbereich erstellen](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img4.png)

Geben Sie auf der 4. Konfigurationsseite den Speicherkontonamen und den Standardcontainer des HDInsight Hadoop-Clusters an. Wenn Sie in der Dropdownliste **DEFAULT CONTAINER** _Create default container_ auswählen, wird ein Container mit demselben Namen wie der Cluster erstellt. Klicken Sie auf den Pfeil, um zur letzten Konfigurationsseite zu wechseln.

![Arbeitsbereich erstellen](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img5.png)

Klicken Sie auf der letzten Konfigurationsseite zu **Script Actions** auf die Schaltfläche** add script action** und füllen Sie die Felder mit den folgenden Werten aus.
 
* **NAME** - eine beliebige Zeichenfolge als Name für diese Skriptaktion.
* **NODE TYPE** - wählen Sie **All nodes** aus.
* **SCRIPT URI** - *http://getgoing.blob.core.windows.net/publicscripts/Azure_HDI_Setup_Windows.ps1*
	* *publicscripts* ist ein öffentlicher Container im Speicherkonto.
	* *getgoing* wird für die Freigabe der PowerShell-Skriptdateien verwendet, um den Benutzern die Arbeit in Azure zu vereinfachen.
* **PARAMETERS** - (leer lassen)

Klicken Sie abschließend auf das Häkchen, um die Erstellung des benutzerdefinierten HDInsight Hadoop-Clusters zu starten.

![Arbeitsbereich erstellen](./media/machine-learning-data-science-customize-hadoop-cluster/script-actions.png)

## <a name="headnode"></a>Zugreifen auf den Hauptknoten des Hadoop-Clusters

Sie müssen den Remotezugriff auf den Hadoop-Cluster in Azure aktivieren, damit Benutzer über RDP auf den Hauptknoten des Hadoop-Clusters zugreifen können.

1. Melden Sie sich beim [**klassischen Azure-Portal**](https://manage.windowsazure.com/) an, und wählen Sie links **HDInsight** aus. Wählen Sie Ihren Hadoop-Cluster aus der Liste der Cluster aus, klicken Sie auf die Registerkarte **CONFIGURATION**, und klicken Sie dann unten auf der Seite auf das Symbol **ENABLE REMOTE**.
	
	![Arbeitsbereich erstellen](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-1.png)

2. Geben Sie im Fenster **Configure Remote Desktop** den BENUTZERNAMEN und das KENNWORT ein, und wählen Sie das Ablaufdatum für den Remotezugriff aus. Klicken Sie anschließend auf das Häkchen, um den Remotezugriff auf den Hauptknoten des Hadoop-Clusters zu aktivieren.

	![Arbeitsbereich erstellen](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-2.png)
	
>[AZURE.NOTE] Der Benutzername und das Kennwort für den Remotezugriff sind nicht der Benutzername und das Kennwort, die Sie zum Erstellen des Hadoop-Clusters verwendet haben. Hierbei handelt es sich um andere Anmeldeinformationen. Das Ablaufdatum für den Remotezugriff muss sich zudem innerhalb der nächsten sieben Tage ab dem aktuellen Datum befinden.

Klicken Sie nach dem Aktivieren des Remotezugriffs unten auf der Seite auf **CONNECT**, um remote auf den Hauptknoten zuzugreifen. So melden Sie sich auf dem Hauptknoten des Hadoop-Clusters durch Eingabe der Anmeldeinformationen für den RAS-Benutzer an, die Sie zuvor angegeben haben.

![Arbeitsbereich erstellen](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-3.png)

Die nächsten Schritte des erweiterten Analyseprozesses sind unter [Team Data Science-Prozess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) aufgeführt. Dazu gehören u.a. auch das Verschieben von Daten nach HDInsight sowie das Verarbeiten und Extrahieren von Stichproben, um anschließend mithilfe von Azure Machine Learning Erkenntnisse aus den Daten zu ziehen.

Unter [Gewusst wie: Übermitteln von Hive-Abfragen ](machine-learning-data-science-move-hive-tables.md#submit)finden Sie Anweisungen zum Zugreifen auf die Python-Module in Anaconda vom Hauptknoten des Clusters aus. Dazu werden UDFs (User Defined Function, benutzerdefinierte Funktion) verwendet, um im Cluster gespeicherte Hive-Datensätze zu verarbeiten.

 

<!---HONumber=AcomDC_0921_2016-->