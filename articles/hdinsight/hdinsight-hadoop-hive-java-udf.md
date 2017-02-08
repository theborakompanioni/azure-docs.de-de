---
title: Verwenden einer benutzerdefinierten Java-Funktion (UDF) mit Hive in HDInsight | Microsoft-Dokumentation
description: Informationen zum Erstellen und Verwenden einer benutzerdefinierten Java-Funktion (UDF) in Hive in HDInsight.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 8d4f8efe-2f01-4a61-8619-651e873c7982
ms.service: hdinsight
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/27/2016
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 240ba2289ededf7a65c71e2f5007cf3a52ca4f84


---
# <a name="use-a-java-udf-with-hive-in-hdinsight"></a>Verwenden einer benutzerdefinierten Java-Funktion in HDInsight
Hive eignet sich besonders für die Arbeit mit Daten in HDInsight. Mitunter wird aber eine allgemeinere Sprache benötigt. Hive ermöglicht Ihnen die Erstellung benutzerdefinierter Funktionen (User Defined Functions, UDF) mithilfe einer Vielzahl von Programmiersprachen. In diesem Dokument erfahren Sie, wie Sie eine Java-UDF in Hive verwenden.

## <a name="requirements"></a>Anforderungen
* Ein Azure-Abonnement
* Ein HDInsight-Cluster (Windows- oder Linux-basiert)
  
  > [!NOTE]
  > Die meisten Schritte in diesem Dokument funktionieren bei beiden Clustertypen. Die Schritte zum Hochladen der kompilierten UDF in den Cluster und ihrer Ausführung sind jedoch spezifisch für Linux-basierte Cluster. Links zu Informationen, die mit Windows-basierten Clustern verwendet werden können, sind angegeben.
  > 
  > 
* [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 7 oder höher (oder eine entsprechende Anwendung wie OpenJDK)
* [Apache Maven](http://maven.apache.org/)
* Ein Text-Editor oder eine Java-IDE
  
  > [!IMPORTANT]
  > Wenn Sie einen Linux-basierten HDInsight-Server verwenden, aber die Python-Dateien auf einem Windows-Client erstellen, müssen Sie einen Editor verwenden, der als Zeilenende LF verwendet. Wenn Sie nicht sicher sind, ob der Editor LF oder CRLF verwendet, finden Sie im Abschnitt [Problembehandlung](#troubleshooting) Schritte für das Entfernen des CR-Zeichens mithilfe von Hilfsprogrammen für den HDInsight-Cluster.
  > 
  > 

## <a name="create-an-example-udf"></a>Erstellen Sie einer Beispiel-UDF
1. Geben Sie an der Befehlszeile Folgendes an, um ein neues Maven-Projekt zu erstellen:
   
        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   
   > [!NOTE]
   > Wenn Sie PowerShell verwenden, müssen Sie die Parameter in Anführungszeichen setzen. Beispiel: `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.
   > 
   > 
   
    Dadurch wird ein neues Verzeichnis namens **exampleudf**erstellt, welches das Maven-Projekt enthält.
2. Nachdem das Projekt erstellt wurde, löschen Sie das Verzeichnis **exampleudf/src/test**, das als Teil des Projekts erstellt wurde. Es wird in diesem Beispiel nicht verwendet.
3. Öffnen Sie die Datei **exampleudf/pom.xml**, und ersetzen Sie den vorhandenen Eintrag `<dependencies>` durch den folgenden Eintrag:
   
        <dependencies>
            <dependency>
                <groupId>org.apache.hadoop</groupId>
                <artifactId>hadoop-client</artifactId>
                <version>2.7.1</version>
                <scope>provided</scope>
            </dependency>
            <dependency>
                <groupId>org.apache.hive</groupId>
                <artifactId>hive-exec</artifactId>
                <version>1.2.1</version>
                <scope>provided</scope>
            </dependency>
        </dependencies>
   
    Diese Einträge geben die Version von Hadoop und Hive an, die in HDInsight 3.3- und 3.4-Clustern enthalten sind. Informationen zu den in HDInsight enthaltenen Versionen von Hadoop und Hive finden Sie im Dokument zu den [Versionen von HDInsight-Komponenten](hdinsight-component-versioning.md) .
   
    Fügen Sie am Ende der Datei den Abschnitt `<build>` der Zeile `</project>` hinzu. Dieser Abschnitt sollte Folgendes enthalten:
   
        <build>
            <plugins>
                <!-- build for Java 1.7, even if you're on a later version -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.3</version>
                    <configuration>
                        <source>1.7</source>
                        <target>1.7</target>
                    </configuration>
                </plugin>
                <!-- build an uber jar -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-shade-plugin</artifactId>
                    <version>2.3</version>
                    <configuration>
                        <!-- Keep us from getting a can't overwrite file error -->
                        <transformers>
                            <transformer
                                    implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                            </transformer>
                            <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer">
                            </transformer>
                        </transformers>
                        <!-- Keep us from getting a bad signature error -->
                        <filters>
                            <filter>
                                <artifact>*:*</artifact>
                                <excludes>
                                    <exclude>META-INF/*.SF</exclude>
                                    <exclude>META-INF/*.DSA</exclude>
                                    <exclude>META-INF/*.RSA</exclude>
                                </excludes>
                            </filter>
                        </filters>
                    </configuration>
                    <executions>
                        <execution>
                            <phase>package</phase>
                            <goals>
                                <goal>shade</goal>
                            </goals>
                        </execution>
                    </executions>
                </plugin>
            </plugins>
        </build>
   
    Diese Einträge definieren, wie das Projekt erstellt werden soll. Insbesondere die Version von Java, die das Projekt verwendet, und wie Sie ein „Uberjar“ für die Bereitstellung im Cluster zu erstellen.
   
    Speichern Sie die Datei, nachdem die Änderungen erfolgt sind.
4. Benennen Sie **exampleudf/src/main/java/com/microsoft/examples/App.java** in **ExampleUDF.java** um, und öffnen Sie dann die Datei im Editor.
5. Ersetzen Sie den Inhalt der Datei **ExampleUDF.java** durch Folgendes, und speichern Sie dann die Datei.
   
        package com.microsoft.examples;
   
        import org.apache.hadoop.hive.ql.exec.Description;
        import org.apache.hadoop.hive.ql.exec.UDF;
        import org.apache.hadoop.io.*;
   
        // Description of the UDF
        @Description(
            name="ExampleUDF",
            value="returns a lower case version of the input string.",
            extended="select ExampleUDF(deviceplatform) from hivesampletable limit 10;"
        )
        public class ExampleUDF extends UDF {
            // Accept a string input
            public String evaluate(String input) {
                // If the value is null, return a null
                if(input == null)
                    return null;
                // Lowercase the input string and return it
                return input.toLowerCase();
            }
        }
   
    Dadurch wird eine UDF implementiert, die einen Zeichenfolgenwert verwendet und die Zeichenfolge in Kleinbuchstaben zurückgibt.

## <a name="build-and-install-the-udf"></a>Erstellen und Installieren der UDF
1. Führen Sie den folgenden Befehl aus, um die UDF zu kompilieren und zu packen:
   
        mvn compile package
   
    Dadurch wird die UDF in **exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar**erstellt und gepackt.
2. Kopieren Sie die Datei mit dem Befehl `scp` in den HDInsight-Cluster.
   
        scp ./target/ExampleUDF-1.0-SNAPSHOT.jar myuser@mycluster-ssh.azurehdinsight
   
    Ersetzen Sie **myuser** durch das SSH-Benutzerkonto für den Cluster. Ersetzen Sie **mycluster** durch den Namen des Clusters. Wenn Sie ein Kennwort zum Schützen des SSH-Kontos verwendet haben, werden Sie zur Eingabe dieses Kennworts aufgefordert. Wenn Sie einen Schlüssel verwendet haben, müssen Sie möglicherweise den Parameter `-i` verwenden, um die Datei mit dem privaten Schlüssel anzugeben.
3. Stellen Sie mithilfe von SSH eine Verbindung zum Cluster her. 
   
        ssh myuser@mycluster-ssh.azurehdinsight.net
   
    Weitere Informationen zur Verwendung von SSH mit HDInsight finden Sie in den folgenden Dokumenten.
   
   * [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
   * [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
4. Kopieren Sie in der SSH-Sitzung die JAR-Datei in HDInsight-Speicher.
   
        hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars

## <a name="use-the-udf-from-hive"></a>Verwenden der UDF in Hive
1. Verwenden Sie den folgenden Befehl, um den Beeline-Client in der SSH-Sitzung zu starten.
   
        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin
   
    Dieser Befehl setzt voraus, dass Sie die Standardeinstellung **admin** für das Anmeldekonto Ihres Clusters verwendet haben.
2. Sobald Sie bei der Eingabeaufforderung `jdbc:hive2://localhost:10001/>` ankommen, geben Sie Folgendes ein, um die UDF zu Hive hinzuzufügen und sie als Funktion verfügbar zu machen.
   
        ADD JAR wasbs:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
        CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';
3. Verwenden Sie die UDF, um aus einer Tabelle abgerufene Werte in Zeichenfolgen mit Kleinbuchstaben zu konvertieren.
   
        SELECT tolower(deviceplatform) FROM hivesampletable LIMIT 10;
   
    Dadurch wird die Geräteplattform (Android, Windows, iOS usw.) in der Tabelle ausgewählt, die Zeichenfolge in Kleinbuchstaben umgewandelt und dann angezeigt. Die Ausgabe sieht in etwa wie folgt aus:
   
        +----------+--+
        |   _c0    |
        +----------+--+
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        +----------+--+

## <a name="next-steps"></a>Nächste Schritte
Andere Möglichkeiten zum Arbeiten mit Hive finden Sie unter [Verwenden von Hive mit HDInsight](hdinsight-use-hive.md).

Weitere Informationen zu benutzerdefinierten Hive-Funktionen finden Sie im Abschnitt [Hive Operators and User-Defined Functions](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) (Hive-Operatoren und benutzerdefinierte Funktionen) des Hive-Wikis unter apache.org.




<!--HONumber=Nov16_HO3-->


