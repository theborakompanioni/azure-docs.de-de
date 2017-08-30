---
title: "Verbinden von Excel über den Hive ODBC-Treiber mit Hadoop – Azure HDInsight | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie den Microsoft Hive ODBC-Treiber für Excel einrichten und zum Abfragen von Daten in HDInsight-Clustern von Microsoft Excel verwenden können."
keywords: Hadoop, Excel, Hive Excel, Hive ODBC
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
tags: azure-portal
editor: cgronlun
ms.assetid: a7665a14-0211-4521-b3e7-3b26e8029cc0
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/22/2017
ms.author: jgao
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: 7818093e42c34ee671a035cde783a6622fb2a798
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="connect-excel-to-hadoop-in-azure-hdinsight-with-the-microsoft-hive-odbc-driver"></a>Verbinden von Excel mit Hadoop in Azure HDInsight mithilfe des Microsoft Hive ODBC-Treibers

[!INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

Die Big Data-Lösung von Microsoft integriert Microsoft Business Intelligence (BI)-Komponenten in Apache Hadoop-Cluster, die von Azure HDInsight bereitgestellt wurden. Ein Beispiel für diese Integration ist die Möglichkeit, Excel mit dem Hive-Data Warehouse eines Hadoop-Clusters in HDInsight über den Microsoft Hive Open Database Connectivity (ODBC) Driver zu verbinden.

Es ist ebenfalls möglich, die zu einem HDInsight-Cluster gehörigen Daten und andere Datenquellen, einschließlich anderer Hadoop-Cluster (nicht aus HDInsight), zu verbinden. Hierbei verwenden Sie in Excel das Add-In Microsoft Power Query für Excel. Informationen zur Installation und Verwendung von Power Query finden Sie unter [Verbinden von Excel mit HDInsight über Power Query][hdinsight-power-query].

> [!NOTE]
> Auch wenn die in diesem Artikel beschriebenen Schritte für Linux- und Windows-basierte HDInsight-Cluster gelten, ist für den Client in jedem Fall eine Windows-Arbeitsstation erforderlich.
> 
> 

**Voraussetzungen**:

Bevor Sie mit diesem Artikel beginnen können, benötigen Sie Folgendes:

* **Einen HDInsight-Cluster**. Hinweise zum Erstellen finden Sie unter [Erste Schritte mit Azure HDInsight][hdinsight-get-started].
* **Eine Arbeitsstation** mit Office 2013 Professional Plus, Office 365 Pro Plus, Excel 2013 Standalone oder Office 2010 Professional Plus.

## <a name="install-microsoft-hive-odbc-driver"></a>Installieren des Microsoft Hive ODBC-Treibers
Laden Sie aus dem [Download Center][hive-odbc-driver-download] den Microsoft Hive ODBC-Treiber herunter, und installieren Sie ihn.

Dieser Treiber kann unter 32- oder 64-Bit-Versionen von Windows 7, Windows 8, Windows 10, Windows Server 2008 R2 und Windows Server 2012 installiert werden. Er erlaubt eine Verbindung zu Azure HDInsight (Version 1.6 und höher) und Azure HDInsight Emulator (Version 1.0.0.0 und höher). Installieren Sie die Version, die der Version der Anwendung entspricht, in der Sie den ODBC-Treiber verwenden. In diesem Lernprogramm wird der Treiber aus Office Excel verwendet.

## <a name="create-hive-odbc-data-source"></a>Erstellen einer Hive ODBC-Datenquelle
Die folgenden Schritte zeigen Ihnen, wie Sie eine Hive-ODBC-Datenquelle erstellen können.

1. Drücken Sie unter Windows 8 oder Windows 10 die Windows-Taste, um den Startbildschirm zu öffnen, und geben Sie dann **Datenquellen**ein.
2. Klicken Sie auf **ODBC-Datenquellen einrichten (32-Bit)** oder **ODBC-Datenquellen einrichten (64-Bit)**, je nach verwendeter Office-Version. Wenn Sie Windows 7 verwenden, wählen Sie die **ODBC-Datenquellen (32-Bit)** oder **ODBC-Datenquellen (64-Bit)** unter **Verwaltungstools** aus. Hierdurch wird das Dialogfeld **ODBC-Datenquellen-Administrator** aufgerufen.
   
    ![ODBC-Datenquellen-Administrator](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png "Konfigurieren eines DSN mithilfe des ODBC-Datenquellen-Administrators")

3. Klicken Sie aus Benutzer-DNS auf **Hinzufügen**, um den Assistenten **Neue Datenquelle erstellen** zu öffnen.
4. Wählen Sie **Microsoft Hive ODBC-Treiber**, und klicken Sie dann auf **Fertig stellen**. Hierdurch wird das DNS-Setup-Dialogfeld **Microsoft Hive ODBC-Treiber** geöffnet.
5. Geben Sie folgende Werte ein bzw. wählen diese aus:
   
   | Eigenschaft | Beschreibung |
   | --- | --- |
   |  Name der Datenquelle |Geben Sie einen Namen für die Datenquelle an. |
   |  Host |Geben Sie „&lt;HDInsightClusterName>.azurehdinsight.net“ ein. Beispiel: myHDICluster.azurehdinsight.net |
   |  Port |Verwenden Sie <strong>443</strong>. (Dieser Port wurde von 563 in 443 geändert.) |
   |  Datenbank |Verwenden Sie <strong>Standard</strong>. |
   |  Mechanismus |Wählen Sie <strong>Azure HDInsight Service</strong> aus. |
   |  Benutzername |Geben Sie Ihren HTTP-Benutzernamen für den HDInsight-Cluster an. Der Standard-Benutzername lautet <strong>admin</strong>. |
   |  Kennwort |Geben Sie Ihr Benutzerkennwort für den HDInsight-Cluster an. |
   
    </table>
   
    Sie müssen einige wichtige Parameter berücksichtigen, wenn Sie auf **Erweiterte Optionen**klicken:
   
   | Parameter | Beschreibung |
   | --- | --- |
   |  Use Native Query |Wenn diese Option ausgewählt ist, versucht der ODBC-Treiber NICHT, TSQL in HiveQL zu konvertieren. Verwenden Sie diese Option nur, wenn Sie sich absolut sicher sind, dass Sie reine HiveQL-Anweisungen absenden. Wenn Sie eine Verbindung zu SQL Server oder Azure SQL Database herstellen, sollten Sie die Option nicht aktivieren. |
   |  Rows fetched per block |Wenn Sie viele Datensätze abrufen, ist es möglicherweise erforderlich, diesen Parameter zu optimieren, um optimale Leistung zu garantieren. |
   |  Default string column length, Binary column length, Decimal column scale |Längen und Genauigkeiten der Datentypen können beeinflussen, wie die Daten zurückgegeben werden. Bei zu geringer Genauigkeit und/oder Abschneiden werden falsche Informationen zurückgegeben. |

    ![Erweiterte Optionen](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png "Erweiterte DSN-Konfigurationsoptionen")

1. Klicken Sie auf **Test** , um die Datenquelle zu testen. Wenn die Datenquelle korrekt konfiguriert wurde, wird *TESTS ERFOLGREICH ABGESCHLOSSEN.*angezeigt.
2. Klicken Sie auf **OK** , um den Testdialog zu schließen. Die neue Datenquelle wird im **ODBC-Datenquellen-Administrator** aufgeführt.
3. Klicken Sie auf **OK** , um den Assistenten zu beenden.

## <a name="import-data-into-excel-from-hdinsight"></a>Importieren von Daten aus HDInsight in Excel
In den folgenden Schritten wird beschrieben, wie Sie mithilfe der ODBC-Datenquelle, die Sie im vorangegangenen Abschnitt erstellt haben, Daten aus einer Hive-Tabelle in eine Excel-Arbeitsmappe importieren.

1. Öffnen Sie eine neue oder bereits vorhandene Arbeitsmappe in Excel.
2. Klicken Sie auf der Registerkarte **Daten** auf **Daten abrufen**, anschließend auf **Aus anderen Quellen** und dann auf **Aus ODBC**, um den **Datenverbindungs-Assistenten** zu starten.
   
    ![Öffnen des Datenverbindungs-Assistenten](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png "Öffnen des Datenverbindungs-Assistenten")
4. Wählen Sie den im letzten Abschnitt erstellten Datenquellennamen aus, und klicken Sie dann auf **OK**.
5. Geben Sie den Hadoop-Benutzernamen (der Standardname lautet „admin“) und das zugehörige Kennwort ein, und klicken Sie dann auf **Verbinden**.
6. Erweitern Sie im Navigator die Option **HIVE**, erweitern Sie **Standard**, klicken Sie auf **hivesampletable**, und klicken Sie dann auf **Laden**. Es dauert einige Zeit, bis die Daten in Excel importiert werden.

    ![HDInsight-Hive-ODBC-Navigator](./media/hdinsight-connect-excel-hive-ODBC-driver/hdinsight.hive.odbc.navigator.png "Öffnen des Datenverbindungs-Assistenten")


## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie erfahren, wie Sie den Microsoft Hive ODBC-Treiber verwenden, um Daten aus dem HDInsight-Dienst nach Excel zu übertragen. Ebenso können Sie Daten aus dem HDInsight-Dienst in eine SQL-Datenbank übertragen. Es ist außerdem möglich, Daten in einen HDInsight-Dienst hochzuladen. Weitere Informationen finden Sie unter:

* [Analysieren von Daten zu Flugverspätungen mit HDInsight][hdinsight-analyze-flight-data]
* [Hochladen von Daten in HDInsight][hdinsight-upload-data]
* [Verwenden von Sqoop mit HDInsight][hdinsight-use-sqoop]

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-get-started]: hdinsight-hadoop-tutorial-get-started-windows.md

[hive-odbc-driver-download]: http://go.microsoft.com/fwlink/?LinkID=286698



