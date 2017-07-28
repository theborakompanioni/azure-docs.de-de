---
title: "Erstellen einer Java-HBase-Anwendung für Windows-basierte Azure HDInsight-Cluster | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie mit Apache Maven eine Java-basierte Apache HBase-Anwendung erstellen und anschließend in einem Windows-basiertem Azure HDInsight-Cluster bereitstellen können."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7f4a4e02-45ab-40dd-842b-3ec034f256c9
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: 83aeb23275a86e5d4706a0ad7ea72d11d71f9604
ms.contentlocale: de-de
ms.lasthandoff: 07/08/2017

---
# <a name="use-maven-to-build-java-applications-that-use-hbase-with-windows-based-hdinsight-hadoop"></a>Verwenden von Maven zur Entwicklung von Java-Anwendungen, die HBase mit Windows-basiertem HDInsight (Hadoop) nutzen
Erfahren Sie, wie Sie eine [Apache HBase](http://hbase.apache.org/) -Anwendung in Java mithilfe von Apache Maven erstellen und entwickeln. Setzen Sie anschließend die Anwendung mit Azure HDInsight (Hadoop) ein.

[Maven](http://maven.apache.org/) ist ein Projektmanagement- und Verständnistool, mit dem Sie Software, Dokumentationen und Berichte für Java-Projekte erstellen können. In diesem Artikel erfahren Sie, wie Sie es für das Erstellen einer einfachen Java-Anwendung verwenden, die eine HBase-Tabelle auf einem Azure HDInsight-Cluster erstellt, abfragt und löscht.

> [!IMPORTANT]
> Die Schritte in diesem Dokument erfordern einen HDInsight-Cluster mit Windows. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="requirements"></a>Anforderungen
* [Java-Plattform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 oder höher
* [Maven](http://maven.apache.org/)
* Ein Windows-basierter HDInsight-Cluster mit HBase

    > [!NOTE]
    > Die Schritte in diesem Dokument wurden mit HDInsight-Clustern der Versionen 3.2 und 3.3 getestet. Die Standardwerte in den Beispielen gelten für einen Cluster mit HDInsight 3.3.

## <a name="create-the-project"></a>Erstellen des Projekts
1. Wechseln Sie in der Befehlszeile Ihrer Entwicklungsumgebung zu dem Speicherort, an dem Sie das Projekt erstellen möchten, z.B. `cd code\hdinsight`.
2. Verwenden Sie den Befehl **mvn** , der mit Maven installiert wird, um das Gerüst für das Projekt zu erstellen.

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Mit diesem Befehl wird ein Verzeichnis am aktuellen Speicherort erstellt, das den Namen trägt, der mit dem Parameter **artifactID** (in diesem Beispiel **hbaseapp**) festgelegt wurde. Dieses Verzeichnis enthält die folgenden Elemente:

   * **pom.xml**: Das Projektobjektmodell ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) enthält Informationen und Konfigurationsdetails zum Erstellen des Projekts.
   * **src**: Das Verzeichnis mit dem Verzeichnis **main\java\com\microsoft\examples**, in dem Sie die Anwendung erstellen.
3. Löschen Sie die Datei **src\test\java\com\microsoft\examples\apptest.java**, da sie in diesem Beispiel nicht verwendet wird.

## <a name="update-the-project-object-model"></a>Aktualisieren des Projektobjektmodells
1. Bearbeiten Sie die Datei **pom.xml**, und fügen Sie folgenden Code im Abschnitt `<dependencies>` ein:

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    In diesem Abschnitt wird Maven informiert, dass das Projekt den **hbase-Client** in der Version **1.1.2** benötigt. Beim Kompilieren wird diese Abhängigkeit aus dem Maven-Standardrepository heruntergeladen. Sie können die [Suche im zentralen Maven-Repository](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) verwenden, wenn Sie weitere Informationen zu dieser Abhängigkeit erhalten möchten.

   > [!IMPORTANT]
   > Die Versionsnummer muss mit der Version von HBase übereinstimmen, die mit Ihrem HDInsight-Cluster bereitgestellt wird. Suchen Sie die richtige Versionsnummer mithilfe der folgenden Tabelle.
   >
   >

   | HDInsight-Clusterversion | Zu verwendende HBase-Version |
   | --- | --- |
   | 3.2 |0.98.4-hadoop2 |
   | 3.3 |1.1.2 |

    Weitere Informationen zu HDInsight-Versionen und Komponenten finden Sie unter [Was sind die verschiedenen Hadoop-Komponenten, die in HDInsight verfügbar sind?](hdinsight-component-versioning.md).
2. Wenn Sie einen HDInsight 3.3-Cluster verwenden, müssen Sie dem `<dependencies>` -Abschnitt auch Folgendes hinzufügen:

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>

    Diese Abhängigkeit lädt die Phoenix-Kernkomponenten, die von Hbase-Version 1.1.x verwendet werden.
3. Fügen Sie der Datei **pom.xml** folgenden Code hinzu: Dieser Abschnitt muss sich in der Datei innerhalb der `<project>...</project>`-Tags befinden, z.B. zwischen `</dependencies>` und `</project>`.

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

    Mit dem Abschnitt `<resources>` wird eine Ressource (**conf/hbase-site.xml**) konfiguriert, die die Konfigurationsinformationen für HBase enthält.

   > [!NOTE]
   > Sie können die Konfigurationswerte auch per Code festlegen. Wie Sie hierbei vorgehen, erfahren Sie in den Kommentaren im Beispiel **CreateTable** weiter unten.
   >
   >

    Mit diesem Abschnitt `<plugins>` werden auch das [Maven Compiler-Plug-In](http://maven.apache.org/plugins/maven-compiler-plugin/) und das [Maven Shade-Plug-In](http://maven.apache.org/plugins/maven-shade-plugin/) konfiguriert. Das Compiler-Plug-In wird zum Kompilieren der Topologie verwendet. Das Shade-Plug-In wird verwendet, um die Lizenzduplizierung in dem von Maven erstellten JAR-Paket zu verhindern. Es wird deshalb verwendet, weil die doppelten Lizenzdateien bei der Laufzeit einen Fehler auf dem HDInsight-Cluster verursachen. Wenn das Maven-Shade-Plug-In mit der `ApacheLicenseResourceTransformer` -Implementierung verwendet wird, wird dieser Fehler verhindert.

    Das Maven-Shade-Plug-In erzeugt außerdem ein Uberjar (oder Fatjar), das alle Abhängigkeiten enthält, die von der Anwendung benötigt werden.
4. Speichern Sie die Datei **pom.xml** .
5. Erstellen Sie ein neues Verzeichnis mit dem Namen **conf** im Verzeichnis **hbaseapp**. Erstellen Sie im Verzeichnis **conf** eine Datei mit dem Namen **hbase-site.xml**. Fügen Sie Folgendes als Inhalt der Datei hinzu:

        <?xml version="1.0"?>
        <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
        <!--
        /**
          * Copyright 2010 The Apache Software Foundation
          *
          * Licensed to the Apache Software Foundation (ASF) under one
          * or more contributor license agreements.  See the NOTICE file
          * distributed with this work for additional information
          * regarding copyright ownership.  The ASF licenses this file
          * to you under the Apache License, Version 2.0 (the
          * "License"); you may not use this file except in compliance
          * with the License.  You may obtain a copy of the License at
          *
          *     http://www.apache.org/licenses/LICENSE-2.0
          *
          * Unless required by applicable law or agreed to in writing, software
          * distributed under the License is distributed on an "AS IS" BASIS,
          * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
          * See the License for the specific language governing permissions and
          * limitations under the License.
          */
        -->
        <configuration>
          <property>
            <name>hbase.cluster.distributed</name>
            <value>true</value>
          </property>
          <property>
            <name>hbase.zookeeper.quorum</name>
            <value>zookeeper0,zookeeper1,zookeeper2</value>
          </property>
          <property>
            <name>hbase.zookeeper.property.clientPort</name>
            <value>2181</value>
          </property>
        </configuration>

    Diese Datei wird zum Laden der HBase-Konfiguration für einen HDInsight-Cluster verwendet.

   > [!NOTE]
   > Dies ist eine kompakte Datei vom Typ „hbase-site.xml“, die nur die wichtigsten Einstellungen für den HDInsight-Cluster enthält.

6. Speichern Sie die Datei **hbase-site.xml**.

## <a name="create-the-application"></a>Erstellen der Anwendung
1. Wechseln Sie zum Verzeichnis **hbaseapp\src\main\java\com\microsoft\examples**, und benennen Sie die Datei „app.java“ in **CreateTable.java** um.
2. Öffnen Sie die Datei **CreateTable.java**, und ersetzen Sie die vorhandenen Inhalte durch den folgenden Code:

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
            // The following sets the znode root for Linux-based HDInsight
            //config.set("zookeeper.znode.parent","/hbase-unsecure");

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
4. Erstellen Sie im Verzeichnis **hbaseapp\src\main\java\com\microsoft\examples** eine Datei mit dem Namen **SearchByEmail.java**. Fügen Sie den folgenden Code als Inhalt dieser Datei hinzu:

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
6. Erstellen Sie im Verzeichnis **hbaseapp\src\main\hava\com\microsoft\examples** eine Datei mit dem Namen **DeleteTable.java**. Fügen Sie den folgenden Code als Inhalt dieser Datei hinzu:

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
1. Öffnen Sie eine Eingabeaufforderung, und ändern Sie das Verzeichnis in **hbaseapp**.
2. Verwenden Sie den folgenden Befehl, um eine JAR-Datei zu erstellen, die die Anwendung enthält:

        mvn clean package

    Damit werden etwaige frühere Erstellungsartefakte entfernt, alle noch nicht installierten Abhängigkeiten heruntergeladen und dann die Anwendung erstellt und gepackt.
3. Nachdem dieser Befehl ausgeführt wurde, enthält das Verzeichnis **hbaseapp\target** eine Datei namens **hbaseapp-1.0-SNAPSHOT.jar**.

   > [!NOTE]
   > Die Datei **hbaseapp-1.0-SNAPSHOT.jar** ist ein Uberjar (manchmal auch Fatjar genannt) mit allen Abhängigkeiten, die für das Ausführen der Anwendung erforderlich sind.

## <a name="upload-the-jar-file-and-start-a-job"></a>Hochladen der JAR-Datei und Starten eines Auftrags
Es gibt viele Möglichkeiten, eine Datei in einen HDInsight-Cluster hochzuladen, wie unter [Hochladen von Daten für Hadoop-Aufträge in HDInsight](hdinsight-upload-data.md)beschrieben. Für die folgenden Schritte wird Azure PowerShell verwendet.

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

1. Nachdem Sie Azure PowerShell installiert und konfiguriert haben, erstellen Sie eine neue Datei namens **hbase-runner.psm1**. Fügen Sie Folgendes als Inhalt der Datei hinzu:

        <#
        .SYNOPSIS
        Copies a file to the primary storage of an HDInsight cluster.
        .DESCRIPTION
        Copies a file from a local directory to the blob container for
        the HDInsight cluster.
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.CreateTable"
        -clusterName "MyHDInsightCluster"

        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
        -clusterName "MyHDInsightCluster"
        -emailRegex "contoso.com"

        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
        -clusterName "MyHDInsightCluster"
        -emailRegex "^r" -showErr
        #>

        function Start-HBaseExample {
        [CmdletBinding(SupportsShouldProcess = $true)]
        param(
        #The class to run
        [Parameter(Mandatory = $true)]
        [String]$className,

        #The name of the HDInsight cluster
        [Parameter(Mandatory = $true)]
        [String]$clusterName,

        #Only used when using SearchByEmail
        [Parameter(Mandatory = $false)]
        [String]$emailRegex,

        #Use if you want to see stderr output
        [Parameter(Mandatory = $false)]
        [Switch]$showErr
        )

        Set-StrictMode -Version 3

        # Is the Azure module installed?
        FindAzure

        # Get the login for the HDInsight cluster
        $creds=Get-Credential -Message "Enter the login for the cluster" -UserName "admin"

        # The JAR
        $jarFile = "wasb:///example/jars/hbaseapp-1.0-SNAPSHOT.jar"

        # The job definition
        $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
            -JarFile $jarFile `
            -ClassName $className `
            -Arguments $emailRegex

        # Get the job output
        $job = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $jobDefinition `
            -HttpCredential $creds
        Write-Host "Wait for the job to complete ..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
        if($showErr)
        {
        Write-Host "STDERR"
        Get-AzureRmHDInsightJobOutput `
                    -Clustername $clusterName `
                    -JobId $job.JobId `
                    -HttpCredential $creds `
                    -DisplayOutputType StandardError
        }
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
                    -Clustername $clusterName `
                    -JobId $job.JobId `
                    -HttpCredential $creds
        }

        <#
        .SYNOPSIS
        Copies a file to the primary storage of an HDInsight cluster.
        .DESCRIPTION
        Copies a file from a local directory to the blob container for
        the HDInsight cluster.
        .EXAMPLE
        Add-HDInsightFile -localPath "C:\temp\data.txt"
        -destinationPath "example/data/data.txt"
        -ClusterName "MyHDInsightCluster"
        .EXAMPLE
        Add-HDInsightFile -localPath "C:\temp\data.txt"
        -destinationPath "example/data/data.txt"
        -ClusterName "MyHDInsightCluster"
        -Container "MyContainer"
        #>

        function Add-HDInsightFile {
            [CmdletBinding(SupportsShouldProcess = $true)]
            param(
                #The path to the local file.
                [Parameter(Mandatory = $true)]
                [String]$localPath,

                #The destination path and file name, relative to the root of the container.
                [Parameter(Mandatory = $true)]
                [String]$destinationPath,

                #The name of the HDInsight cluster
                [Parameter(Mandatory = $true)]
                [String]$clusterName,

                #If specified, overwrites existing files without prompting
                [Parameter(Mandatory = $false)]
                [Switch]$force
            )

            Set-StrictMode -Version 3

            # Is the Azure module installed?
            FindAzure

            # Get authentication for the cluster
            $creds=Get-Credential

            # Does the local path exist?
            if (-not (Test-Path $localPath))
            {
                throw "Source path '$localPath' does not exist."
            }

            # Get the primary storage container
            $storage = GetStorage -clusterName $clusterName

            # Upload file to storage, overwriting existing files if -force was used.
            Set-AzureStorageBlobContent -File $localPath `
                -Blob $destinationPath `
                -force:$force `
                -Container $storage.container `
                -Context $storage.context
        }

        function FindAzure {
            # Is there an active Azure subscription?
            $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
            if(-not($sub))
            {
                throw "No active Azure subscription found! If you have a subscription, use the Login-AzureRmAccount cmdlet to login to your subscription."
            }
        }

        function GetStorage {
            param(
                [Parameter(Mandatory = $true)]
                [String]$clusterName
            )
            $hdi = Get-AzureRmHDInsightCluster -ClusterName $clusterName
            # Does the cluster exist?
            if (!$hdi)
            {
                throw "HDInsight cluster '$clusterName' does not exist."
            }
            # Create a return object for context & container
            $return = @{}
            $storageAccounts = @{}

            # Get storage information
            $resourceGroup = $hdi.ResourceGroup
            $storageAccountName=$hdi.DefaultStorageAccount.split('.')[0]
            $container=$hdi.DefaultStorageContainer
            $storageAccountKey=(Get-AzureRmStorageAccountKey `
                -Name $storageAccountName `
            -ResourceGroupName $resourceGroup)[0].Value
            # Get the resource group, in case we need that
            $return.resourceGroup = $resourceGroup
            # Get the storage context, as we can't depend
            # on using the default storage context
            $return.context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
            # Get the container, so we know where to
            # find/store blobs
            $return.container = $container
            # Return storage accounts to support finding all accounts for
            # a cluster
            $return.storageAccount = $storageAccountName
            $return.storageAccountKey = $storageAccountKey

            return $return
        }
        # Only export the verb-phrase things
        export-modulemember *-*

    Diese Datei enthält zwei Module:

   * **Add-HDInsightFile** wird für das Hochladen von Dateien zu HDInsight verwendet.
   * **Start-HBaseExample** wird für das Ausführen der bereits erstellten Klassen verwendet.
2. Speichern Sie die Datei **hbase-runner.psm1**.
3. Öffnen Sie ein neues Azure PowerShell-Fenster, ändern Sie das Verzeichnis in **hbaseapp**, und führen Sie dann den folgenden Befehl aus:

        PS C:\ Import-Module c:\path\to\hbase-runner.psm1

    Legen Sie den Pfad auf den Speicherort der früher erstellten Datei **hbase-runner.psm1** fest. Dadurch wird das Modul für diese Azure PowerShell-Sitzung registriert.
4. Verwenden Sie den folgenden Befehl zu Hochladen von **hbaseapp-1.0-SNAPSHOT.jar** in den HDInsight-Cluster.

        Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername

    Ersetzen Sie **hdinsightclustername** durch den Namen Ihres HDInsight-Clusters. Der Befehl lädt dann **hbaseapp-1.0-SNAPSHOT.jar** in **example/jars** im Hauptspeicher Ihres HDInsight-Clusters hoch.
5. Verwenden Sie nach dem Hochladen der Dateien folgenden Code, um eine Tabelle mithilfe von **hbaseapp** zu erstellen.

        Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername

    Ersetzen Sie **hdinsightclustername** durch den Namen Ihres HDInsight-Clusters.

    Dieser Befehl erstellt eine neue Tabelle namens **people** im HDInsight-Cluster. Bei diesem Befehl erfolgt keine Ausgabe im Konsolenfenster.
6. Verwenden Sie den folgenden Befehl, um nach Einträgen in die Tabelle zu suchen:

        Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com

    Ersetzen Sie **hdinsightclustername** durch den Namen Ihres HDInsight-Clusters.

    Dieser Befehl verwendet die **SearchByEmail**-Klasse, um nach allen Zeilen zu suchen, in denen die Spaltenfamilie **contactinformation** und die Spalte **email** die Zeichenfolge **contoso.com** enthalten. Daraufhin sollte Sie folgende Ergebnisse erhalten:

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    Wenn **fabrikam.com** als `-emailRegex`-Wert verwendet wird, werden Benutzer zurückgegeben, die **fabrikam.com** im Feld „E-Mail“ enthalten. Da diese Suche mithilfe eines auf regulären Ausdrücken basierenden Filters implementiert wurde, können Sie auch reguläre Ausdrücke wie etwa **^r** eingeben, womit Einträge zurückgegeben werden, bei denen die E-Mail-Adresse mit dem Buchstaben „r“ beginnt.

## <a name="delete-the-table"></a>Löschen der Tabelle
Wenn Sie mit dem Beispiel fertig sind, verwenden Sie den folgenden Befehl aus der Azure PowerShell-Sitzung zum Löschen der Tabelle **people**, die in diesem Beispiel verwendet wurde:

    Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername

Ersetzen Sie **hdinsightclustername** durch den Namen Ihres HDInsight-Clusters.

## <a name="troubleshooting"></a>Problembehandlung
### <a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a>Keine oder unerwartete Ergebnisse bei Verwenden von "Start-HBaseExample"
Verwenden Sie den Parameter `-showErr` , wenn Sie den Standardfehler (STDERR) sehen möchten, der während der Ausführung des Auftrags erzeugt wurde.

