---
title: Erstellen einer HBase-Anwendung mit Maven und Java und Bereitstellen in Linux-basiertem HDInsight | Microsoft Docs
description: "Erfahren Sie, wie Sie mit Apache Maven eine Java-basierte Apache HBase-Anwendung erstellen und anschließend in einem Linux-basierten HDInsight-Cluster in der Azure-Cloud bereitstellen."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: 
ms.assetid: 1d1ed180-e0f4-4d1c-b5ea-72e0eda643bc
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2016
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 0f321065b9c24075837bebb71251cbc5751a1854


---
# <a name="use-maven-to-build-java-applications-that-use-hbase-with-linux-based-hdinsight-hadoop"></a>Verwenden von Maven zur Entwicklung von Java-Anwendungen, die HBase mit Linux-basiertem HDInsight (Hadoop) nutzen
Erfahren Sie, wie Sie eine [Apache HBase](http://hbase.apache.org/) -Anwendung in Java mithilfe von Apache Maven erstellen und entwickeln. Verwenden Sie dann die Anwendung mit einem Linux-basierten HDInsight-Cluster.

[Maven](http://maven.apache.org/) ist ein Projektmanagement- und Verständnistool, mit dem Sie Software, Dokumentationen und Berichte für Java-Projekte erstellen können. In diesem Artikel erfahren Sie, wie Sie das Tool zum Erstellen einer einfachen Java-Anwendung einsetzen, die eine HBase-Tabelle auf einem Linux-basierten HDInsight-Cluster erstellt, abfragt und löscht.

> [!NOTE]
> Die Schritte in diesem Artikel setzen voraus, dass Sie einen Linux-basierten HDInsight-Cluster verwenden. Informationen zur Verwendung eines Windows-basierten HDInsight-Clusters finden Sie unter [Verwenden von Maven zur Entwicklung von Java-Anwendungen, die HBase mit Windows-basiertem HDInsight (Hadoop) nutzen](hdinsight-hbase-build-java-maven.md)
> 
> 

## <a name="requirements"></a>Anforderungen
* [Java-Plattform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 oder höher
* [Maven](http://maven.apache.org/)
* [Linux-basierter Azure HDInsight-Cluster mit HBase](hdinsight-hbase-tutorial-get-started-linux.md#create-hbase-cluster)
  
  > [!NOTE]
  > Die Schritte in diesem Dokument wurden mit HDInsight-Clustern der Versionen 3.2, 3.3 und 3.4 getestet. Die Standardwerte in den Beispielen gelten für einen Cluster mit HDInsight 3.4.
  > 
  > 
* **Erfahrung mit SSH und SCP**. Weitere Informationen zur Verwendung von SSH und SCP mit HDInsight finden Sie in den folgenden Artikeln:
  
  * **Linux-, Unix- oder OS X-Clients**: Siehe [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, OS X oder Unix](hdinsight-hadoop-linux-use-ssh-unix.md)
  * **Windows-Clients**: Siehe [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

## <a name="create-the-project"></a>Erstellen des Projekts
1. Ändern Sie an der Befehlszeile Ihrer Entwicklungsumgebung die Verzeichnisse auf den Speicherort, an dem Sie das Projekt erstellen möchten, beispielsweise `cd code/hdinsight`.
2. Verwenden Sie den Befehl **mvn** , der mit Maven installiert wird, um das Gerüst für das Projekt zu erstellen.
   
        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   
    Damit wird ein neues Verzeichnis im aktuellen erstellt, das den Namen trägt, der vom Parameter **artifactID** (in diesem Beispiel **hbaseapp**) festgelegt wurde. Dieses Verzeichnis enthält die folgenden Elemente:
   
   * **pom.xml**: Das Projektobjektmodell ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) enthält Informationen und Konfigurationsdetails zum Erstellen des Projekts.
   * **src**: Das Verzeichnis mit dem Verzeichnis **main/java/com/microsoft/examples**, in dem Sie die Anwendung erstellen.
3. Löschen Sie die Datei **src/test/java/com/microsoft/examples/apptest.java**, da sie in diesem Beispiel nicht verwendet wird.

## <a name="update-the-project-object-model"></a>Aktualisieren des Projektobjektmodells
1. Bearbeiten Sie die Datei **pom.xml**, und fügen Sie folgenden Code im Abschnitt `<dependencies>` ein:
   
        <dependency>
            <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>
   
    Damit wird Maven informiert, dass das Projekt den **hbase-Client** in der Version **1.1.2** benötigt. Beim Kompilieren wird er aus dem standardmäßigen Maven-Repository heruntergeladen. Sie können die [Suche im zentralen Maven-Repository](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) verwenden, wenn Sie weitere Informationen zu dieser Abhängigkeit erhalten möchten.
   
   > [!IMPORTANT]
   > Die Versionsnummer muss mit der Version von HBase übereinstimmen, die mit Ihrem HDInsight-Cluster bereitgestellt wird. Suchen Sie die richtige Versionsnummer mithilfe der folgenden Tabelle.
   > 
   > 
   
   | HDInsight-Clusterversion | Zu verwendende HBase-Version |
   | --- | --- |
   | 3.2 |0.98.4-hadoop2 |
   | 3.3 und 3.4 |1.1.2 |
   
    Weitere Informationen zu HDInsight-Versionen und Komponenten finden Sie unter [Was sind die verschiedenen Hadoop-Komponenten, die in HDInsight verfügbar sind?](hdinsight-component-versioning.md).
2. Wenn Sie einen HDInsight 3.3- oder 3.4-Cluster verwenden, müssen Sie dem `<dependencies>` -Abschnitt auch Folgendes hinzufügen:
   
        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>
   
    Dies lädt die Phoenix-Kernkomponenten, die Hbase-Version 1.1.x benötigt.
3. Fügen Sie der Datei **pom.xml** folgenden Code hinzu: Dieser muss sich in der Datei innerhalb der `<project>...</project>`-Tags befinden, z. B. zwischen `</dependencies>` und `</project>`.
   
        <build>
          <sourceDirectory>src</sourceDirectory>
          <resources>
            <resource>
              <directory>${basedir}/conf</directory>
              <filtering>false</filtering>
              <includes>
                <include>hbase-site.xml</include>
              </includes>
            </resource>
          </resources>
          <plugins>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-compiler-plugin</artifactId>
                        <version>3.3</version>
              <configuration>
                  <source>1.7</source>
                  <target>1.7</target>
              </configuration>
              </plugin>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-shade-plugin</artifactId>
              <version>2.3</version>
              <configuration>
                <transformers>
                  <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                  </transformer>
                </transformers>
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
   
    Damit wird eine Ressource (**conf/hbase-site.xml**) konfiguriert, die die Konfigurationsinformationen für HBase enthält.
   
   > [!NOTE]
   > Sie können die Konfigurationswerte auch per Code festlegen. Wie Sie hierbei vorgehen, erfahren Sie in den Kommentaren im Beispiel **CreateTable** weiter unten.
   > 
   > 
   
    Damit werden auch das [Maven Compiler-Plug-In](http://maven.apache.org/plugins/maven-compiler-plugin/) und das [Maven Shade-Plug-In](http://maven.apache.org/plugins/maven-shade-plugin/) konfiguriert. Das Compiler-Plug-In wird zum Kompilieren der Topologie verwendet. Das Shade-Plug-In wird verwendet, um die Lizenzduplizierung in dem von Maven erstellten JAR-Paket zu verhindern. Es wird deshalb verwendet, weil die doppelten Lizenzdateien bei der Laufzeit einen Fehler auf dem HDInsight-Cluster verursachen. Wird maven-shade-plugin mit der `ApacheLicenseResourceTransformer` -Implementierung verwendet, so wird dieser Fehler verhindert.
   
    Das maven-shade-plugin erzeugt außerdem ein Uberjar (oder Fatjar), das alle Abhängigkeiten enthält, die von der Anwendung benötigt werden.
4. Speichern Sie die Datei **pom.xml** .
5. Erstellen Sie ein neues Verzeichnis mit dem Namen **conf** im Verzeichnis **hbaseapp**. Dies wird zum Speichern von Konfigurationsinformationen für die HBase-Verbindung verwendet.
6. Verwenden Sie den folgenden Befehl, um die HBase-Konfiguration vom HDInsight-Server in das Verzeichnis **conf** zu kopieren. Ersetzen Sie **USERNAME** durch den Namen der SSH-Anmeldung. Ersetzen Sie **CLUSTERNAME** durch den Namen Ihres HDInsight-Clusters:
   
        scp USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml
   
   > [!NOTE]
   > Wenn Sie ein Kennwort für das SSH-Konto verwendet haben, werden Sie zur Eingabe dieses Kennworts aufgefordert. Wenn Sie einen SSH-Schlüssel mit dem Konto verwendet haben, müssen Sie möglicherweise den `-i` -Parameter verwenden, um den Pfad zur Schlüsseldatei anzugeben. Im folgenden Beispiel wird der private Schlüssel aus `~/.ssh/id_rsa`geladen:
   > 
   > `scp -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml`
   > 
   > 

## <a name="create-the-application"></a>Erstellen der Anwendung
1. Wechseln Sie zum Verzeichnis **hbaseapp/src/main/java/com/microsoft/examples**, und benennen Sie die Datei „app.java“ in **CreateTable.java** um.
2. Öffnen Sie die Datei **CreateTable.java**, und ersetzen Sie die vorhandenen Inhalte wie folgt:
   
        package com.microsoft.examples;
        import java.io.IOException;
   
        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;
        import org.apache.hadoop.hbase.HTableDescriptor;
        import org.apache.hadoop.hbase.TableName;
        import org.apache.hadoop.hbase.HColumnDescriptor;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Put;
        import org.apache.hadoop.hbase.util.Bytes;
   
        public class CreateTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();
   
            // Example of setting zookeeper values for HDInsight
            // in code instead of an hbase-site.xml file
            //
            // config.set("hbase.zookeeper.quorum",
            //            "zookeepernode0,zookeepernode1,zookeepernode2");
            //config.set("hbase.zookeeper.property.clientPort", "2181");
            //config.set("hbase.cluster.distributed", "true");
            //
            //NOTE: Actual zookeeper host names can be found using Ambari:
            //curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts"
   
            //Linux-based HDInsight clusters use /hbase-unsecure as the znode parent
            config.set("zookeeper.znode.parent","/hbase-unsecure");
   
            // create an admin object using the config
            HBaseAdmin admin = new HBaseAdmin(config);
   
            // create the table...
            HTableDescriptor tableDescriptor = new HTableDescriptor(TableName.valueOf("people"));
            // ... with two column families
            tableDescriptor.addFamily(new HColumnDescriptor("name"));
            tableDescriptor.addFamily(new HColumnDescriptor("contactinfo"));
            admin.createTable(tableDescriptor);
   
            // define some people
            String[][] people = {
                { "1", "Marcel", "Haddad", "marcel@fabrikam.com"},
                { "2", "Franklin", "Holtz", "franklin@contoso.com" },
                { "3", "Dwayne", "McKee", "dwayne@fabrikam.com" },
                { "4", "Rae", "Schroeder", "rae@contoso.com" },
                { "5", "Rosalie", "burton", "rosalie@fabrikam.com"},
                { "6", "Gabriela", "Ingram", "gabriela@contoso.com"} };
   
            HTable table = new HTable(config, "people");
   
            // Add each person to the table
            //   Use the `name` column family for the name
            //   Use the `contactinfo` column family for the email
            for (int i = 0; i< people.length; i++) {
              Put person = new Put(Bytes.toBytes(people[i][0]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("first"), Bytes.toBytes(people[i][1]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("last"), Bytes.toBytes(people[i][2]));
              person.add(Bytes.toBytes("contactinfo"), Bytes.toBytes("email"), Bytes.toBytes(people[i][3]));
              table.put(person);
            }
            // flush commits and close the table
            table.flushCommits();
            table.close();
          }
        }
   
    Dies ist die **CreateTable**-Klasse, die eine Tabelle namens **people** erzeugt und sie mit einigen vordefinierten Benutzern füllt.
3. Speichern Sie die Datei **CreateTable.java**.
4. Erstellen Sie im Verzeichnis **hbaseapp/src/main/java/com/microsoft/examples** eine Datei mit dem Namen **SearchByEmail.java**. Fügen Sie Folgendes als Inhalt der Datei hinzu:
   
        package com.microsoft.examples;
        import java.io.IOException;
   
        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Scan;
        import org.apache.hadoop.hbase.client.ResultScanner;
        import org.apache.hadoop.hbase.client.Result;
        import org.apache.hadoop.hbase.filter.RegexStringComparator;
        import org.apache.hadoop.hbase.filter.SingleColumnValueFilter;
        import org.apache.hadoop.hbase.filter.CompareFilter.CompareOp;
        import org.apache.hadoop.hbase.util.Bytes;
        import org.apache.hadoop.util.GenericOptionsParser;
   
        public class SearchByEmail {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();
   
            // Use GenericOptionsParser to get only the parameters to the class
            // and not all the parameters passed (when using WebHCat for example)
            String[] otherArgs = new GenericOptionsParser(config, args).getRemainingArgs();
            if (otherArgs.length != 1) {
              System.out.println("usage: [regular expression]");
              System.exit(-1);
            }
   
            // Open the table
            HTable table = new HTable(config, "people");
   
            // Define the family and qualifiers to be used
            byte[] contactFamily = Bytes.toBytes("contactinfo");
            byte[] emailQualifier = Bytes.toBytes("email");
            byte[] nameFamily = Bytes.toBytes("name");
            byte[] firstNameQualifier = Bytes.toBytes("first");
            byte[] lastNameQualifier = Bytes.toBytes("last");
   
            // Create a new regex filter
            RegexStringComparator emailFilter = new RegexStringComparator(otherArgs[0]);
            // Attach the regex filter to a filter
            //   for the email column
            SingleColumnValueFilter filter = new SingleColumnValueFilter(
              contactFamily,
              emailQualifier,
              CompareOp.EQUAL,
              emailFilter
            );
   
            // Create a scan and set the filter
            Scan scan = new Scan();
            scan.setFilter(filter);
   
            // Get the results
            ResultScanner results = table.getScanner(scan);
            // Iterate over results and print  values
            for (Result result : results ) {
              String id = new String(result.getRow());
              byte[] firstNameObj = result.getValue(nameFamily, firstNameQualifier);
              String firstName = new String(firstNameObj);
              byte[] lastNameObj = result.getValue(nameFamily, lastNameQualifier);
              String lastName = new String(lastNameObj);
              System.out.println(firstName + " " + lastName + " - ID: " + id);
              byte[] emailObj = result.getValue(contactFamily, emailQualifier);
              String email = new String(emailObj);
              System.out.println(firstName + " " + lastName + " - " + email + " - ID: " + id);
            }
            results.close();
            table.close();
          }
        }
   
    Die **SearchByEmail**-Klasse kann für Abfragen von Zeilen nach der E-Mail-Adresse verwendet werden. Da sie einen Filter für reguläre Ausdrücke verwendet, können Sie beim Einsatz der Klasse entweder eine Zeichenfolge angeben oder einen regulären Ausdruck.
5. Speichern Sie die Datei **SearchByEmail.java**.
6. Erstellen Sie im Verzeichnis **hbaseapp/src/main/java/com/microsoft/examples** eine Datei mit dem Namen **DeleteTable.java**. Fügen Sie Folgendes als Inhalt der Datei hinzu:
   
        package com.microsoft.examples;
        import java.io.IOException;
   
        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;
   
        public class DeleteTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();
   
            // Create an admin object using the config
            HBaseAdmin admin = new HBaseAdmin(config);
   
            // Disable, and then delete the table
            admin.disableTable("people");
            admin.deleteTable("people");
          }
        }
   
    Diese Klasse dient zum Aufräumen dieses Beispiels. Sie deaktiviert die von der **CreateTable**-Klasse erstellte Tabelle und verwirft sie.
7. Speichern Sie die Datei **DeleteTable.java**.

## <a name="build-and-package-the-application"></a>Erstellen und Packen der Anwendung
1. Verwenden Sie im Verzeichnis **hbaseapp** den folgenden Befehl, um eine JAR-Datei zu erstellen, die die Anwendung enthält:
   
        mvn clean package
   
    Damit werden etwaige frühere Erstellungsartefakte entfernt, alle noch nicht installierten Abhängigkeiten heruntergeladen und dann die Anwendung erstellt und gepackt.
2. Nachdem dieser Befehl ausgeführt wurde, enthält das Verzeichnis **hbaseapp/target** eine Datei namens **hbaseapp-1.0-SNAPSHOT.jar**.
   
   > [!NOTE]
   > Die Datei **hbaseapp-1.0-SNAPSHOT.jar** ist ein Uberjar (manchmal auch Fatjar genannt) mit allen Abhängigkeiten, die für das Ausführen der Anwendung erforderlich sind.
   > 
   > 

## <a name="upload-the-jar-file-and-run-jobs"></a>Hochladen der JAR-Datei und Ausführen von Aufträgen
1. Verwenden Sie den folgenden Befehl, um die JAR-Datei in den HDInsight-Cluster hochzuladen. Ersetzen Sie **USERNAME** durch den Namen der SSH-Anmeldung. Ersetzen Sie **CLUSTERNAME** durch den Namen Ihres HDInsight-Clusters:
   
        scp ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.
   
    Dadurch wird die Datei in das Basisverzeichnis für Ihr SSH-Benutzerkonto hochgeladen.
   
   > [!NOTE]
   > Wenn Sie ein Kennwort für das SSH-Konto verwendet haben, werden Sie zur Eingabe dieses Kennworts aufgefordert. Wenn Sie einen SSH-Schlüssel mit dem Konto verwendet haben, müssen Sie möglicherweise den `-i` -Parameter verwenden, um den Pfad zur Schlüsseldatei anzugeben. Im folgenden Beispiel wird der private Schlüssel aus `~/.ssh/id_rsa`geladen:
   > 
   > `scp -i ~/.ssh/id_rsa ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`
   > 
   > 
2. Stellen Sie mithilfe von SSH eine Verbindung mit dem HDInsight-Cluster her. Ersetzen Sie **USERNAME** durch den Namen des SSH-Anmeldung. Ersetzen Sie **CLUSTERNAME** durch den Namen Ihres HDInsight-Clusters:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
   > [!NOTE]
   > Wenn Sie ein Kennwort für das SSH-Konto verwendet haben, werden Sie zur Eingabe dieses Kennworts aufgefordert. Wenn Sie einen SSH-Schlüssel mit dem Konto verwendet haben, müssen Sie möglicherweise den `-i` -Parameter verwenden, um den Pfad zur Schlüsseldatei anzugeben. Im folgenden Beispiel wird der private Schlüssel aus `~/.ssh/id_rsa`geladen:
   > 
   > `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`
   > 
   > 
3. Nachdem die Verbindung hergestellt wurde, erstellen Sie mithilfe des folgenden Befehls eine neue HBase-Tabelle mithilfe der Java-Anwendung:
   
        hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.CreateTable
   
    Dadurch entsteht eine neue HBase-Tabelle namens **people**, die mit Daten aufgefüllt wird.
4. Verwenden Sie als Nächstes den folgenden Befehl für die Suche nach E-Mail-Adressen in der Tabelle:
   
        hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.SearchByEmail contoso.com
   
    Daraufhin sollte Sie folgende Ergebnisse erhalten:
   
        Franklin Holtz - ID: 2
        Franklin Holtz - franklin@contoso.com - ID: 2
        Rae Schroeder - ID: 4
        Rae Schroeder - rae@contoso.com - ID: 4
        Gabriela Ingram - ID: 6
        Gabriela Ingram - gabriela@contoso.com - ID: 6

## <a name="delete-the-table"></a>Löschen der Tabelle
Wenn Sie mit dem Beispiel fertig sind, verwenden Sie den folgenden Befehl aus der Azure PowerShell-Sitzung zum Löschen der Tabelle **people**, die in diesem Beispiel verwendet wurde:

    hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.DeleteTable




<!--HONumber=Nov16_HO3-->


