---
title: "Anpassen von Azure HDInsight Hadoop-Clustern für den Team Data Science-Prozess | Microsoft Docs"
description: "Beliebte Python-Module, die in benutzerdefinierten Azure HDInsight Hadoop-Clustern zur Verfügung gestellt werden."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 0c115dca-2565-4e7a-9536-6002af5c786a
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: hangzh;bradsev
translationtype: Human Translation
ms.sourcegitcommit: 22d7dc81cb2fc44ff4471951cbc482f60a97bb27
ms.openlocfilehash: 9104c45508afdb5682c44db64576a0cdae95d75f
ms.lasthandoff: 02/11/2017


---
# <a name="customize-azure-hdinsight-hadoop-clusters-for-the-team-data-science-process"></a>Anpassen von Azure HDInsight Hadoop-Clustern für den Team Data Science-Prozess
In diesem Artikel wird beschrieben, wie ein HDInsight Hadoop-Cluster durch die Installation der 64-Bit-Version von Anaconda (Python 2.7) auf jedem Knoten angepasst wird, wenn der Cluster als HDInsight-Dienst bereitgestellt wird. Es wird auch gezeigt, wie Sie auf den Hauptknoten zugreifen, um benutzerdefinierte Aufträge an den Cluster zu übermitteln. Diese Anpassung macht viele beliebte Python-Module verfügbar, die Bestandteil von Anaconda sind. Dies geschieht sehr benutzerfreundlich in UDFs (User Defined Function, benutzerdefinierte Funktion), die entwickelt wurden, um Hive-Datensätze im Cluster zu verarbeiten. Anweisungen zu den in diesem Szenario verwendeten Prozeduren finden Sie unter [Gewusst wie: Übermitteln von Hive-Abfragen](machine-learning-data-science-move-hive-tables.md#submit).

Das folgende Menü enthält Links zu Themen, in denen das Einrichten der verschiedenen Data Science-Umgebungen, die vom [Team Data Science-Prozess (TDSP)](data-science-process-overview.md)verwendet werden, beschrieben wird.

[!INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

## <a name="customize"></a>Anpassen von Azure HDInsight Hadoop-Clustern
Zum Erstellen eines angepassten HDInsight Hadoop-Clusters melden Sie sich beim [**klassischen Azure-Portal**](https://manage.windowsazure.com/) an. Klicken Sie dann links unten auf **Neu**, und wählen Sie DATA SERVICES -> HDINSIGHT -> **BENUTZERDEFINIERT ERSTELLEN** aus, um das Fenster **Clusterdetails** zu öffnen. 

![Arbeitsbereich erstellen](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

Geben Sie den Namen des zu erstellenden Clusters auf der 1. Konfigurationsseite ein, und übernehmen Sie die Standardwerte für die anderen Felder. Klicken Sie auf den Pfeil, um zur nächsten Konfigurationsseite zu wechseln. 

![Arbeitsbereich erstellen](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

Geben Sie auf der 2. Konfigurationsseite die Anzahl der **DATENKNOTEN** ein. Wählen Sie eine Option für **REGION/VIRTUAL NETWORK** und dann die Größe für **HAUPTKNOTEN** und **DATENKNOTEN** aus. Klicken Sie auf den Pfeil, um zur nächsten Konfigurationsseite zu wechseln.

> [!NOTE]
> Der Wert für **REGION/VIRTUAL NETWORK** muss mit der Region des Speicherkontos identisch sein, das für den HDInsight Hadoop-Cluster verwendet werden soll. Andernfalls wird auf der vierten Konfigurationsseite das Speicherkonto in der Dropdownliste **KONTONAME** nicht angezeigt.
> 
> 

![Arbeitsbereich erstellen](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img3.png)

Geben Sie auf der 3. Konfigurationsseite einen Benutzernamen und ein Kennwort für den HDInsight Hadoop-Cluster ein. **Wählen Sie nicht** die Option *Hive-/Oozie-Metastore eingeben* aus. Klicken Sie anschließend auf den Pfeil, um zur nächsten Konfigurationsseite zu wechseln. 

![Arbeitsbereich erstellen](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img4.png)

Geben Sie auf der 4. Konfigurationsseite den Speicherkontonamen und den Standardcontainer des HDInsight Hadoop-Clusters an. Wenn Sie in der Dropdownliste **STANDARDCONTAINER** den Eintrag *Standardcontainer erstellen* auswählen, wird ein Container mit demselben Namen wie der Cluster erstellt. Klicken Sie auf den Pfeil, um zur letzten Konfigurationsseite zu wechseln.

![Arbeitsbereich erstellen](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img5.png)

Klicken Sie auf der letzten Konfigurationsseite zu **Skriptaktionen** auf die Schaltfläche **Skriptaktion hinzufügen**, und füllen Sie die Felder mit den folgenden Werten aus.

* **NAME**: eine beliebige Zeichenfolge als Name für diese Skriptaktion
* **KNOTENTYP**: Wählen Sie **Alle Knoten** aus.
* **SKRIPT-URI** - *http://getgoing.blob.core.windows.net/publicscripts/Azure_HDI_Setup_Windows.ps1* 
  * *publicscripts* ist ein öffentlicher Container im Speicherkonto. 
  * *getgoing* wird für die Freigabe der PowerShell-Skriptdateien verwendet, um den Benutzern die Arbeit in Azure zu vereinfachen.
* **PARAMETERS** - (leer lassen)

Klicken Sie abschließend auf das Häkchen, um die Erstellung des benutzerdefinierten HDInsight Hadoop-Clusters zu starten. 

![Arbeitsbereich erstellen](./media/machine-learning-data-science-customize-hadoop-cluster/script-actions.png)

## <a name="headnode"></a> Zugreifen auf den Hauptknoten des Hadoop-Clusters
Sie müssen den Remotezugriff auf den Hadoop-Cluster in Azure aktivieren, damit Benutzer über RDP auf den Hauptknoten des Hadoop-Clusters zugreifen können. 

1. Melden Sie sich beim [**klassischen Azure-Portal**](https://manage.windowsazure.com/) an, und wählen Sie links **HDInsight** aus. Wählen Sie Ihren Hadoop-Cluster aus der Liste der Cluster aus, klicken Sie auf die Registerkarte **KONFIGURATION**, und klicken Sie dann unten auf der Seite auf das Symbol für **REMOTE AKTIVIEREN**.
   
    ![Arbeitsbereich erstellen](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-1.png)
2. Geben Sie im Fenster **Configure Remote Desktop** den BENUTZERNAMEN und das KENNWORT ein, und wählen Sie das Ablaufdatum für den Remotezugriff aus. Klicken Sie anschließend auf das Häkchen, um den Remotezugriff auf den Hauptknoten des Hadoop-Clusters zu aktivieren.
   
    ![Arbeitsbereich erstellen](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-2.png)

> [!NOTE]
> Der Benutzername und das Kennwort für den Remotezugriff sind nicht der Benutzername und das Kennwort, die Sie zum Erstellen des Hadoop-Clusters verwendet haben. Hierbei handelt es sich um andere Anmeldeinformationen. Das Ablaufdatum für den Remotezugriff muss sich zudem innerhalb der nächsten sieben Tage ab dem aktuellen Datum befinden.
> 
> 

Klicken Sie nach dem Aktivieren des Remotezugriffs unten auf der Seite auf **CONNECT** , um remote auf den Hauptknoten zuzugreifen. So melden Sie sich auf dem Hauptknoten des Hadoop-Clusters durch Eingabe der Anmeldeinformationen für den RAS-Benutzer an, die Sie zuvor angegeben haben.

![Arbeitsbereich erstellen](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-3.png)

Die nächsten Schritte des erweiterten Analyseprozesses sind unter [Team Data Science-Prozess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) aufgeführt. Dazu gehören u.a. auch das Verschieben von Daten nach HDInsight sowie das Verarbeiten und Extrahieren von Stichproben, um anschließend mithilfe von Azure Machine Learning Erkenntnisse aus den Daten zu ziehen.

Unter [Übermitteln von Hive-Abfragen](machine-learning-data-science-move-hive-tables.md#submit) finden Sie Anweisungen zum Zugreifen auf die Python-Module in Anaconda vom Hauptknoten des Clusters aus. Dazu werden UDFs (User Defined Function, benutzerdefinierte Funktion) verwendet, um im Cluster gespeicherte Hive-Datensätze zu verarbeiten.


