---
title: "Verwenden des Azure-Toolkit für IntelliJ mit Hortonworks Sandbox | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie HDInsight-Tools im Azure-Toolkit für IntelliJ mit Hortonworks Sandbox verwenden."
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
ms.date: 01/07/2017
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: bb700c7de96712666bc4be1f8e430a2e94761f69
ms.openlocfilehash: b52e5bcf443afd05bf8cc37ce436cb80ab41412f
ms.lasthandoff: 01/24/2017


---
#<a name="use-hdinsight-tools-for-intellij-with-hortonworks-sandbox"></a>Verwenden von HDInsight-Tools für IntelliJ mit Hortonworks Sandbox

Es wird beschrieben, wie Sie die HDInsight-Tools für IntelliJ verwenden, um Apache Scala-Anwendungen zu entwickeln und die Anwendungen in [Hortonworks Sandbox](http://hortonworks.com/products/sandbox/) auf Ihrer Arbeitsstation zu testen. [IntelliJ IDEA](https://www.jetbrains.com/idea/) ist eine integrierte Java-Entwicklungsumgebung zum Entwickeln von Computersoftware. Nachdem Sie Ihre Anwendungen in Hortonworks Sandbox entwickelt und getestet haben, können Sie sie in [Azure HDInsight](hdinsight-hadoop-introduction.md) verschieben.

##<a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen können, benötigen Sie Folgendes:

- HDP 2.4 in Hortonworks Sandbox in Ihrer lokalen Umgebung. Informationen zur Konfiguration finden Sie unter [Erste Schritte im Hadoop-Ökosystem mit einer Hadoop-Sandbox auf einem virtuellen Computer](hdinsight-hadoop-emulator-get-started.md). Beachten Sie, dass HDInsight-Tools für IntelliJ nur mit HDP 2.4 getestet wurde. Erweitern Sie auf der [Website für den Hortonworks Sandbox-Download](http://hortonworks.com/downloads/#sandbox) die Option **Hortonworks Sandbox Archive**, um das Herunterladen durchzuführen.
- [Java Developer Kit (JDK) Version 1.8 oder höher](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html). JDK ist für das Azure-Toolkit für IntelliJ erforderlich.
- [IntelliJ IDEA Community Edition](https://www.jetbrains.com/idea/download) mit dem [Scala](https://plugins.jetbrains.com/idea/plugin/1347-scala)-Plug-In und dem [Azure-Toolkit für IntelliJ](../azure-toolkit-for-intellij.md)-Plug-In. Die HDInsight-Tools für IntelliJ sind als Teil des Azure-Toolkits für IntelliJ verfügbar. 

  **So installieren Sie die Plug-Ins**

  1. Öffnen Sie IntelliJ IDEA.
  2. Klicken Sie auf dem Willkommensbildschirm auf **Configure** (Konfigurieren) und dann auf **Plugins** (Plug-Ins).
  3. Klicken Sie im nächsten Bildschirm links unten auf **Install JetBrains plugin** (JetBrains-Plug-In installieren).
  4. Verwenden Sie die Suchfunktion, um nach **Scala** zu suchen, und klicken Sie dann auf **Install** (Installieren).
  5. Klicken Sie auf **Restart IntelliJ IDEA** (IntelliJ IDEA neu starten), um die Installation abzuschließen.
  6. Wiederholen Sie die Schritte 4 und 5, um das **Azure-Toolkit für IntelliJ** zu installieren. Weitere Informationen finden Sie unter [Installieren des Azure-Toolkits für IntelliJ](../azure-toolkit-for-intellij-installation.md).

## <a name="create-a-spark-scala-application"></a>Erstellen einer Spark Scala-Anwendung

In diesem Abschnitt erstellen Sie ein Scala-Beispielprojekt mit IntelliJ IDEA. Im nächsten Abschnitt verknüpfen Sie IntelliJ IDEA mit Hortonworks Sandbox (Emulator), bevor Sie das Projekt übermitteln.

1. Öffnen Sie IntelliJ IDEA auf Ihrer Arbeitsstation.
2. Klicken Sie auf **Create New Project** (Neues Projekt erstellen).
3. Klicken Sie im linken Bereich auf **HDInsight** und dann im rechten Bereich auf **Spark on HDInsight(Scala)** (Spark für HDInsight (Scala)). Klicken Sie anschließend auf **Next** (Weiter).

    ![Erstellen eines IntelliJ Scala-Projekts](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project.png)
4. Geben Sie Folgendes ein:

    ![Erstellen von IntelliJ Scala-Projekteigenschaften](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project-properties.png)

    - **Project name** (Projektname): Geben Sie einen Projektnamen an.
    - **Project location** (Projektspeicherort): Geben Sie einen Projektspeicherort an.
    - **Project SDK** (Projekt-SDK): Klicken Sie auf **New** (Neu) und **JDK**, und geben Sie dann den Ordner mit Java JDK-Version 7 oder höher an.  Der Standardspeicherort ist „C:\Programme\Java\jdk1.8.x_xxx“.
    - **Scala SDK**: Klicken Sie auf **Select** (Auswählen), wählen Sie Version **2.10.6**, und klicken Sie dann auf **OK**. Gehen Sie wie folgt vor, falls die Version nicht aufgeführt ist: Klicken Sie auf **Download** (Herunterladen), wählen Sie die **Scala-Version**, und klicken Sie dann auf **OK**. Achten Sie darauf, dass Sie nicht Version 2.11.x verwenden. In diesem Artikel wird Version 2.10.6 verwendet.
    - **Spark SDK**: Laden Sie das [SDK](http://go.microsoft.com/fwlink/?LinkID=723585&clcid=0x409) herunter. Sie können stattdessen auch das Spark Maven-Repository verwenden. Stellen Sie aber sicher, dass die richtige Maven-Repository-Version für die Entwicklung Ihrer Spark-Anwendungen installiert ist. (Wenn Sie z. B. Spark Streaming verwenden, müssen Sie sicherstellen, dass Sie den Teil für Spark Streaming installiert haben. Vergewissern Sie sich auch, dass Sie das Repository für Scala 2.10 verwenden. Verwenden Sie nicht das Repository für Scala 2.11.)
5. Klicken Sie auf **Fertig stellen**.
6. Drücken Sie **[ALT]+1**, um die Projektansicht zu öffnen, falls sie noch nicht angezeigt wird.
7. Erweitern Sie im **Projektexplorer** das Projekt, und klicken Sie auf **src**.
8. Klicken Sie mit der rechten Maustaste auf **src**, zeigen Sie auf **New** (Neu), und klicken Sie auf **Scala class** (Scala-Klasse).
9. Geben Sie einen Namen ein, wählen Sie unter **Kind** (Art) die Option **Object** (Objekt), und klicken Sie dann auf **OK**.

    ![Erstellen einer neuen IntelliJ Scala-Klasse](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-new-scala-class.png)
10. Fügen Sie den folgenden Code in die SCALA-Datei ein:

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

11. Klicken Sie im Menü **Build** auf **Projekt erstellen**. Stellen Sie sicher, dass die Kompilierung erfolgreich abgeschlossen wurde.


## <a name="link-to-the-hortonworks-sandbox"></a>Link zu Hortonworks Sandbox

Sie müssen über eine vorhandene IntelliJ-Anwendung verfügen, bevor Sie einen Link zu Hortonworks Sandbox (Emulator) einrichten können.

**So richten Sie einen Link zu einem Emulator ein**

1. Öffnen Sie das Projekt in IntelliJ, falls es noch nicht geöffnet ist.
2. Klicken Sie im Menü **Ansicht** auf **Toolfenster** und dann auf **Azure Explorer**.
3. Erweitern Sie **Azure**, klicken Sie mit der rechten Maustaste auf **HDInsight**, und klicken Sie dann auf **Link an Emulator** (Emulator verknüpfen).
4. Geben Sie das Kennwort ein, das Sie für das Stammkonto von Hortonworks Sandbox konfiguriert haben, und geben Sie die restlichen Werte ähnlich wie im folgenden Screenshot ein. Klicken Sie anschließend auf **OK**. 

  ![Verknüpfen eines Emulators mit IntelliJ](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-link-an-emulator.png)

5. Klicken Sie auf **Ja**, um den Emulator zu konfigurieren.

  Wenn die Verbindung erfolgreich hergestellt wird, sehen Sie, dass der Emulator (Hortonworks Sandbox) unter dem HDInsight-Knoten aufgeführt ist.

## <a name="submit-the-spark-scala-application-to-the-sandbox"></a>Übermitteln der Spark Scala-Anwendung an die Sandbox

Nachdem Sie IntelliJ IDEA mit dem Emulator verlinkt haben, können Sie Ihr Projekt übermitteln.

**So übermitteln Sie ein Projekt an einen Emulator**

1. Klicken Sie im **Projektexplorer** mit der rechten Maustaste auf das Projekt, und klicken Sie dann auf **Submit Spark Application to HDInsight** (Spark-Anwendung an HDInsight senden).
2. Füllen Sie die folgenden Felder aus:

    - **Spark cluster (Linux only)** (Spark-Cluster (nur Linux)): Wählen Sie Ihre lokale Hortonworks Sandbox-Instanz aus.
    - **Main class name** (Name der Hauptklasse): Wählen Sie den Namen der Hauptklasse aus, oder geben Sie ihn ein.  Für dieses Tutorial lautet der Name **GroupByTest**.
3. Klicken Sie auf **Senden**. Die Protokolle der Auftragsübermittlungen werden im Fenster des Spark-Übermittlungstools angezeigt.

##<a name="next-steps"></a>Nächste Schritte:

- Informationen dazu, wie Sie Spark-Anwendungen für HDInsight mit HDInsight-Tools für IntelliJ erstellen, finden Sie unter [Verwenden der HDInsight-Tools im Azure-Toolkit für IntelliJ zum Erstellen von Spark-Anwendungen für HDInsight Spark-Cluster unter Linux](hdinsight-apache-spark-intellij-tool-plugin.md).
- Ein Video zu HDInsight-Tools für IntelliJ finden Sie unter [Introduce HDInsight Tools for IntelliJ for Spark Development](https://mix.office.com/watch/1nqkqjt5xonza) (Einführung in HDInsight-Tools für IntelliJ für die Spark-Entwicklung).
- Informationen dazu, wie Sie Spark-Anwendungen mit dem Toolkit per Remoteverbindung über HDInsight debuggen, finden Sie unter [Verwenden der HDInsight-Tools im Azure-Toolkit für IntelliJ zum Remotedebuggen von Spark-Anwendungen im HDInsight Spark-Cluster unter Linux](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md).
- Informationen dazu, wie Sie HDInsight-Tools für Eclipse zum Erstellen von Spark-Anwendungen verwenden, finden Sie unter [Verwenden der HDInsight-Tools im Azure-Toolkit für Eclipse zum Erstellen von Spark-Anwendungen](hdinsight-apache-spark-eclipse-tool-plugin.md).
- Ein Video zu HDInsight-Tools für Eclipse finden Sie unter [Use HDInsight Tool for Eclipse to create Spark applications](https://mix.office.com/watch/1rau2mopb6fha) (Verwenden von HDInsight-Tools für Eclipse zum Erstellen von Spark-Anwendungen).
