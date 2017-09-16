---
title: "Verwenden des Azure-Toolkits für IntelliJ mit Hortonworks Sandbox | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie HDInsight-Tools im Azure-Toolkit für IntelliJ mit Hortonworks Sandbox verwenden."
keywords: "Hadoop-Tools,Hive-Abfrage,IntelliJ,Hortonworks Sandbox,Azure-Toolkit für IntelliJ"
services: HDInsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: b587cc9b-a41a-49ac-998f-b54d6c0bdfe0
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/06/2017
ms.author: jgao
ms.translationtype: HT
ms.sourcegitcommit: eeed445631885093a8e1799a8a5e1bcc69214fe6
ms.openlocfilehash: 5c42b3e519781fa25fe92b286fa65898a7aac173
ms.contentlocale: de-de
ms.lasthandoff: 09/07/2017

---
# <a name="use-hdinsight-tools-for-intellij-with-hortonworks-sandbox"></a>Verwenden von HDInsight-Tools für IntelliJ mit Hortonworks Sandbox

Erfahren Sie, wie Sie die HDInsight-Tools für IntelliJ verwenden, um Apache Scala-Anwendungen zu entwickeln und die Anwendungen in [Hortonworks Sandbox](http://hortonworks.com/products/sandbox/) auf Ihrer Arbeitsstation zu testen. 

[IntelliJ IDEA](https://www.jetbrains.com/idea/) ist eine integrierte Java-Entwicklungsumgebung zum Entwickeln von Computersoftware. Nachdem Sie Ihre Anwendungen in Hortonworks Sandbox entwickelt und getestet haben, können Sie sie in [Azure HDInsight](hdinsight-hadoop-introduction.md) verschieben.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen können, benötigen Sie Folgendes:

- Hortonworks Data Platform 2.4 (HDP) in Hortonworks Sandbox in Ihrer lokalen Umgebung. Informationen zur Konfiguration von HDP finden Sie unter [Erste Schritte im Hadoop-Ökosystem mit einer Hadoop-Sandbox auf einem virtuellen Computer](hdinsight-hadoop-emulator-get-started.md). 
    >[!NOTE]
    >HDInsight-Tools für IntelliJ wurden nur mit HDP 2.4 getestet. Um HDP 2.4 zu erhalten, erweitern Sie auf der **Website für den Download von Hortonworks Sandbox** die Option [Hortonworks Sandbox Archive](http://hortonworks.com/downloads/#sandbox).

- [Java Developer Kit (JDK) Version 1.8 oder höher](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html). JDK ist für das Azure-Toolkit für IntelliJ erforderlich.

- [IntelliJ IDEA Community Edition](https://www.jetbrains.com/idea/download) mit dem [Scala](https://plugins.jetbrains.com/idea/plugin/1347-scala)-Plug-In und [Azure-Toolkit für IntelliJ](../azure-toolkit-for-intellij.md)-Plug-In. Die HDInsight-Tools für IntelliJ sind als Teil des Azure-Toolkits für IntelliJ verfügbar. 

  Um die Plug-Ins zu installieren, führen Sie folgende Schritte aus:

  1. Öffnen Sie IntelliJ IDEA.
  2. Klicken Sie auf dem **Begrüßungsbildschirm** auf **Configure** (Konfigurieren) und dann auf **Plugins** (Plug-Ins).
  3. Klicken Sie links unten auf **Install JetBrains plugin** (JetBrains-Plug-In installieren).
  4. Verwenden Sie die Suchfunktion, um nach **Scala** zu suchen, und klicken Sie dann auf **Install** (Installieren).
  5. Klicken Sie auf **Restart IntelliJ IDEA** (IntelliJ IDEA neu starten), um die Installation abzuschließen.
  6. Wiederholen Sie die Schritte 4 und 5, um das **Azure-Toolkit für IntelliJ** zu installieren. Weitere Informationen finden Sie unter [Installieren des Azure-Toolkits für IntelliJ](../azure-toolkit-for-intellij-installation.md).

## <a name="create-a-spark-scala-application"></a>Erstellen einer Spark Scala-Anwendung

In diesem Abschnitt erstellen Sie mit IntelliJ IDEA ein Scala-Beispielprojekt. Im nächsten Abschnitt verknüpfen Sie IntelliJ IDEA mit Hortonworks Sandbox (Emulator), bevor Sie das Projekt übermitteln.

1. Öffnen Sie IntelliJ IDEA auf Ihrer Arbeitsstation. Führen Sie im Dialogfeld **Neues Projekt** folgende Schritte aus:

   a. Wählen Sie **HDInsight** > **Spark auf HDInsight (Scala)** aus.

   b. Wählen Sie in der Liste **Build-Tool** je nach Ihren Anforderungen einen der folgenden Einträge aus:

    * **Maven** für die Unterstützung des Scala-Projekterstellungs-Assistenten
    * **SBT** zum Verwalten von Abhängigkeiten und Erstellen für das Scala-Projekt

   ![Dialogfeld „Neues Projekt“](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project.png)

2. Klicken Sie auf **Weiter**.

3. Führen Sie im nächsten Dialogfeld **New Project** folgende Schritte aus:

    ![Erstellen von IntelliJ Scala-Projekteigenschaften](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project-properties.png)

    a. Geben Sie in das Feld **Project name** einen Projektnamen ein.

    b. Geben Sie in das Feld **Project location** (Projektspeicherort) einen Projektspeicherort ein.

    c. Klicken Sie neben der Dropdownliste **Project SDK** auf **New** (Neu). Wählen Sie **JDK** aus, und geben Sie dann den Ordner der Java JDK-Version 1.7 oder höher an. Wählen Sie für Spark 2.x-Cluster **Java 1.8** oder für Spark 1.x-Cluster **Java 1.7** aus. Der Standardspeicherort ist „C:\Programme\Java\jdk1.8.x_xxx“.

    d. In der Dropdownliste **Spark-Version** fügt der Scala-Projekterstellungs-Assistent die richtige Version von Spark-SDK und Scala-SDK ein. Wenn die Spark-Clusterversion niedriger als 2.0 ist, wählen Sie **Spark 1.x** aus. Wählen Sie andernfalls **Spark 2.x** aus. In diesem Beispiel wird Spark 1.6.2 (Scala 2.10.5) verwendet. Stellen Sie sicher, dass Sie das Repository „Scala 2.10.x“ verwenden. Verwenden Sie nicht das Repository „Scala 2.11.x“.

4. Wählen Sie **Fertig stellen** aus.

5. Wenn die Ansicht **Project** (Projekt) nicht bereits geöffnet ist, drücken Sie **ALT+1**, um sie zu öffnen.

6. Erweitern Sie im **Project Explorer** (Projekt-Explorer) das Projekt, und wählen Sie anschließend **src** aus.

7. Klicken Sie mit der rechten Maustaste auf **src**, zeigen Sie auf **New** (Neu), und klicken Sie auf **Scala class** (Scala-Klasse).

8. Geben Sie in das Feld **Name** einen Namen ein. Wählen Sie im Feld **Kind** (Art) **Object** (Objekt) aus, und klicken Sie auf **OK**.

    ![Das Fenster „Create New Scala Class“ (Neue Scala-Klasse erstellen)](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-new-scala-class.png)

9. Fügen Sie den folgenden Code in die SCALA-Datei ein:

        import java.util.Random
        import org.apache.spark.{SparkConf, SparkContext}
        import org.apache.spark.SparkContext._

        /**
        * Usage: GroupByTest [numMappers] [numKVPairs] [valSize] [numReducers]
        */
        object GroupByTest {
            def main(args: Array[String]) {
                val sparkConf = new SparkConf().setAppName("GroupBy Test")
                var numMappers = 3
                var numKVPairs = 10
                var valSize = 10
                var numReducers = 2

                val sc = new SparkContext(sparkConf)

                val pairs1 = sc.parallelize(0 until numMappers, numMappers).flatMap { p =>
                val ranGen = new Random
                var arr1 = new Array[(Int, Array[Byte])](numKVPairs)
                for (i <- 0 until numKVPairs) {
                    val byteArr = new Array[Byte](valSize)
                    ranGen.nextBytes(byteArr)
                    arr1(i) = (ranGen.nextInt(Int.MaxValue), byteArr)
                }
                arr1
                }.cache
                // Enforce that everything has been calculated and in cache
                pairs1.count

                println(pairs1.groupByKey(numReducers).count)
            }
        }

10. Klicken Sie im Menü **Build** auf **Build project** (Projekt erstellen). Stellen Sie sicher, dass die Kompilierung erfolgreich abgeschlossen wurde.


## <a name="link-to-the-hortonworks-sandbox"></a>Link zu Hortonworks Sandbox

Sie müssen über eine vorhandene IntelliJ-Anwendung verfügen, bevor Sie einen Link zu Hortonworks Sandbox (Emulator) einrichten können.

Führen Sie folgende Schritte aus, um die Verknüpfung mit einem Emulator herzustellen:

1. Öffnen Sie das Projekt in IntelliJ.

2. Wählen Sie im Menü **View**  (Ansicht) die Option **Tool Windows** (Toolfenster) und dann **Azure Explorer** aus.

3. Erweitern Sie **Azure**, klicken Sie mit der rechten Maustaste auf **HDInsight**, und klicken Sie dann auf **Link an Emulator** (Mit einem Emulator verknüpfen).

4. Geben Sie in das Fenster **Link A New Emulator** (Verknüpfung mit neuem Emulator herstellen) das Kennwort ein, das Sie für das Konto „root“ von Hortonworks Sandbox konfiguriert haben. Geben Sie dem folgenden Screenshot entsprechende Werte ein, und klicken Sie dann auf **OK**. 

   ![Das Fenster „Link a New Emulator“ (Verknüpfung mit neuem Emulator herstellen)](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-link-an-emulator.png)

5. Klicken Sie auf **Ja**, um den Emulator zu konfigurieren.

Wenn die Verbindung mit dem Emulator erfolgreich hergestellt wurde, sehen Sie, dass der Emulator (Hortonworks Sandbox) unter dem Knoten „HDInsight“ aufgeführt ist.

## <a name="submit-the-spark-scala-application-to-the-hortonworks-sandbox"></a>Übermitteln der Spark Scala-Anwendung an die Hortonworks Sandbox

Nachdem Sie IntelliJ IDEA mit dem Emulator verknüpft haben, können Sie Ihr Projekt übermitteln.

Um ein Projekt an einen Emulator zu übermitteln, gehen Sie wie folgt vor:

1. Klicken Sie im **Projektexplorer** mit der rechten Maustaste auf das Projekt, und wählen Sie dann **Submit Spark Application to HDInsight** (Spark-Anwendung an HDInsight senden).

2. Gehen Sie wie folgt vor:

    a. Wählen Sie in der Dropdownliste **Spark cluster (Linux only)** (Spark-Cluster [nur Linux]) Ihre lokale Hortonworks Sandbox-Instanz aus.

    b. Wählen Sie im Feld **Main class name** (Name der Hauptklasse) den Namen der Hauptklasse aus, oder geben Sie ihn ein. Für dieses Tutorial lautet der Name **GroupByTest**.

3. Klicken Sie auf **Submit** (Senden). Die Protokolle der Auftragsübermittlungen werden im Fenster des Spark-Übermittlungstools angezeigt.

## <a name="next-steps"></a>Nächste Schritte

- Informationen dazu, wie Sie Spark-Anwendungen für HDInsight mit HDInsight-Tools für IntelliJ erstellen, finden Sie unter [Verwenden der HDInsight-Tools im Azure-Toolkit für IntelliJ zum Erstellen von Spark-Anwendungen für HDInsight Spark-Cluster unter Linux](hdinsight-apache-spark-intellij-tool-plugin.md).

- Ein Video zu HDInsight-Tools für IntelliJ finden Sie unter [Introduce HDInsight Tools for IntelliJ for Spark Development](https://www.youtube.com/watch?v=YTZzYVgut6c) (Einführung in HDInsight-Tools für IntelliJ für die Spark-Entwicklung).

- Wie Sie Spark-Anwendungen mit dem Toolkit per Remoteverbindung über SSH in HDInsight debuggen, erfahren Sie unter [Remotely debug Spark applications on an HDInsight cluster with Azure Toolkit for IntelliJ through SSH](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md) (Remotedebuggen von Spark-Anwendungen auf einem HDInsight-Cluster mit Azure-Toolkit für IntelliJ über SSH).

- Wie Sie Spark-Anwendungen mit dem Toolkit per Remoteverbindung über VPN auf HDInsight debuggen, erfahren Sie unter [Verwenden der HDInsight-Tools im Azure-Toolkit für IntelliJ zum Remotedebuggen von Spark-Anwendungen im HDInsight Spark-Cluster unter Linux](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md).

- Informationen dazu, wie Sie HDInsight-Tools für Eclipse zum Erstellen von Spark-Anwendungen verwenden, finden Sie unter [Verwenden der HDInsight-Tools im Azure-Toolkit für Eclipse zum Erstellen von Spark-Anwendungen](hdinsight-apache-spark-eclipse-tool-plugin.md).

- Ein Video zu HDInsight-Tools für Eclipse finden Sie unter [Use HDInsight Tool for Eclipse to create Spark applications](https://mix.office.com/watch/1rau2mopb6fha) (Verwenden von HDInsight-Tools für Eclipse zum Erstellen von Spark-Anwendungen).

