---
title: "Java-HBase-Client – Azure HDInsight | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie mit Apache Maven eine Java-basierte Apache HBase-Anwendung erstellen und anschließend in HBase unter Azure HDInsight bereitstellen können."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: 
ms.assetid: 1d1ed180-e0f4-4d1c-b5ea-72e0eda643bc
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: larryfr
ms.translationtype: HT
ms.sourcegitcommit: caaf10d385c8df8f09a076d0a392ca0d5df64ed2
ms.openlocfilehash: d6ef6c988533f27338a61a587b3ce5174d8fa806
ms.contentlocale: de-de
ms.lasthandoff: 08/08/2017

---
# <a name="build-java-applications-for-apache-hbase"></a>Erstellen von Java-Anwendungen für die Apache-HBase

Erfahren Sie, wie Sie eine [Apache HBase](http://hbase.apache.org/)-Anwendung in Java erstellen. Setzen Sie die Anwendung anschließend mit HBase unter Azure HDInsight ein.

Die Schritte in diesem Dokument verwenden [Maven](http://maven.apache.org/) zum Erstellen des Projekts. Maven ist ein Projektmanagement- und Verständnistool, mit dem Sie Software, Dokumentationen und Berichte für Java-Projekte erstellen können.

> [!NOTE]
> Die Schritte in diesem Dokument wurden zuletzt mit HDInsight 3.6 getestet.

> [!IMPORTANT]
> Die Schritte in diesem Dokument erfordern einen HDInsight-Cluster mit Linux. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="requirements"></a>Anforderungen

* [Java Platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 8 oder höher

    > [!NOTE]
    > Für HDInsight 3.5 und höhere Versionen ist Java 8 erforderlich. Für frühere Versionen von HDInsight ist Java 7 erforderlich.

* [Maven](http://maven.apache.org/)

* [Linux-basierter Azure HDInsight-Cluster mit HBase](hdinsight-hbase-tutorial-get-started-linux.md#create-hbase-cluster)

  > [!NOTE]
  > Die Schritte in diesem Dokument wurden mit HDInsight-Clustern der Versionen 3.4 und 3.5 getestet. Die Standardwerte in den Beispielen gelten für einen Cluster mit HDInsight 3.5.

## <a name="create-the-project"></a>Erstellen des Projekts

1. Wechseln Sie in der Befehlszeile Ihrer Entwicklungsumgebung zu dem Speicherort, an dem Sie das Projekt erstellen möchten, z.B. `cd code\hbase`.

2. Verwenden Sie den Befehl **mvn** , der mit Maven installiert wird, um das Gerüst für das Projekt zu erstellen.

    ```bash
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

    > [!NOTE]
    > Wenn Sie PowerShell verwenden, müssen Sie die `-D`-Parameter in doppelte Anführungszeichen setzen.
    >
    > `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=hbaseapp" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`

    Mit diesem Befehl wird ein Verzeichnis mit dem gleichen Namen wie der Parameter **artifactID** erstellt (in diesem Beispiel **hbaseapp**). Dieses Verzeichnis enthält die folgenden Elemente:

   * **pom.xml**: Das Projektobjektmodell ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) enthält Informationen und Konfigurationsdetails zum Erstellen des Projekts.
   * **src**: Das Verzeichnis mit dem Verzeichnis **main/java/com/microsoft/examples**, in dem Sie die Anwendung erstellen.

3. Löschen Sie die Datei `src/test/java/com/microsoft/examples/apptest.java`. Sie wird in diesem Beispiel nicht verwendet.

## <a name="update-the-project-object-model"></a>Aktualisieren des Projektobjektmodells

1. Bearbeiten Sie die Datei `pom.xml`, und fügen Sie folgenden Code im Abschnitt `<dependencies>` ein:

   ```xml
    <dependency>
        <groupId>org.apache.hbase</groupId>
        <artifactId>hbase-client</artifactId>
        <version>1.1.2</version>
    </dependency>
    <dependency>
        <groupId>org.apache.phoenix</groupId>
        <artifactId>phoenix-core</artifactId>
        <version>4.4.0-HBase-1.1</version>
    </dependency>
   ```

    In diesem Abschnitt ist angegeben, dass für das Projekt die Komponenten **hbase-client** und **phoenix-core** erforderlich sind. Beim Kompilieren werden diese Abhängigkeiten aus dem Maven-Standardrepository heruntergeladen. Sie können die [Suche im zentralen Maven-Repository](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) verwenden, wenn Sie weitere Informationen zu dieser Abhängigkeit erhalten möchten.

   > [!IMPORTANT]
   > Die Versionsnummer der hbase-client-Komponente muss mit der Version von HBase übereinstimmen, die mit Ihrem HDInsight-Cluster bereitgestellt wird. Suchen Sie die richtige Versionsnummer mithilfe der folgenden Tabelle.

   | HDInsight-Clusterversion | Zu verwendende HBase-Version |
   | --- | --- |
   | 3.2 |0.98.4-hadoop2 |
   | 3.3, 3.4, 3.5 und 3.6 |1.1.2 |

    Weitere Informationen zu HDInsight-Versionen und Komponenten finden Sie unter [Was sind die verschiedenen Hadoop-Komponenten, die in HDInsight verfügbar sind?](hdinsight-component-versioning.md).

3. Fügen Sie der Datei **pom.xml** folgenden Code hinzu: Dieser Text muss sich in der Datei innerhalb der `<project>...</project>`-Tags befinden, z.B. zwischen `</dependencies>` und `</project>`.

   ```xml
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
                <source>1.8</source>
                <target>1.8</target>
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
   ```

    Mit diesem Abschnitt wird eine Ressource (`conf/hbase-site.xml`) konfiguriert, die die Konfigurationsinformationen für HBase enthält.

   > [!NOTE]
   > Sie können die Konfigurationswerte auch per Code festlegen. Siehe dazu die Kommentare im Beispiel `CreateTable`.

    Mit diesem Abschnitt werden auch das [Maven Compiler-Plug-In](http://maven.apache.org/plugins/maven-compiler-plugin/) und das [Maven Shade-Plug-In](http://maven.apache.org/plugins/maven-shade-plugin/) konfiguriert. Das Compiler-Plug-In wird zum Kompilieren der Topologie verwendet. Das Shade-Plug-In wird verwendet, um die Lizenzduplizierung in dem von Maven erstellten JAR-Paket zu verhindern. Dieses Plug-In wird verwendet, um einen Fehler des Typs „Doppelte Lizenzdateien“ zur Laufzeit im HDInsight-Cluster zu vermeiden. Wenn das Maven-Shade-Plug-In mit der `ApacheLicenseResourceTransformer`-Implementierung verwendet wird, wird der Fehler vermieden.

    Das maven-shade-plugin generiert außerdem ein Uberjar, das alle Abhängigkeiten enthält, die in der Anwendung benötigt werden.

4. Speichern Sie die Datei `pom.xml`.

5. Erstellen Sie ein Verzeichnis mit dem Namen `conf` im Verzeichnis `hbaseapp`. Dieses Verzeichnis wird zum Speichern von Konfigurationsinformationen für die HBase-Verbindung verwendet.

6. Verwenden Sie den folgenden Befehl, um die HBase-Konfiguration aus dem HBase-Cluster an den Spoeicherort `conf` zu kopieren. Ersetzen Sie `USERNAME` durch den Namen Ihrer SSH-Anmeldung. Ersetzen Sie `CLUSTERNAME` durch den Namen Ihres HDInsight-Clusters:

    ```bash
    scp USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml
    ```

   Weitere Informationen zur Verwendung von `ssh` und `scp` finden Sie unter [Verwenden von SSH mit HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="create-the-application"></a>Erstellen der Anwendung

1. Wechseln Sie zum Verzeichnis `hbaseapp/src/main/java/com/microsoft/examples`, und benennen Sie die Datei „app.java“ in `CreateTable.java` um.

2. Öffnen Sie die Datei `CreateTable.java`, und ersetzen Sie die vorhandenen Inhalte durch den folgenden Text:

   ```java
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
   ```

    Dieser Code ist die **CreateTable**-Klasse, die eine Tabelle namens **people** erzeugt und sie mit einigen vordefinierten Benutzern füllt.

3. Speichern Sie die Datei `CreateTable.java`.

4. Erstellen Sie im Verzeichnis `hbaseapp/src/main/java/com/microsoft/examples` eine Datei namens `SearchByEmail.java`. Verwenden Sie als Inhalt der Datei den folgenden Text:

   ```java
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

        // Create a regex filter
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
   ```

    Die **SearchByEmail**-Klasse kann für Abfragen von Zeilen nach der E-Mail-Adresse verwendet werden. Da sie einen Filter für reguläre Ausdrücke verwendet, können Sie beim Einsatz der Klasse entweder eine Zeichenfolge angeben oder einen regulären Ausdruck.

5. Speichern Sie die Datei `SearchByEmail.java`.

6. Erstellen Sie im Verzeichnis `hbaseapp/src/main/hava/com/microsoft/examples` eine Datei namens `DeleteTable.java`. Verwenden Sie als Inhalt der Datei den folgenden Text:

   ```java
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
   ```

    Mit dieser Klasse werden die in diesem Beispiel erstellten HBase-Tabellen bereinigt. Dabei wird die mit der `CreateTable`-Klasse erstellte Tabelle deaktiviert und gelöscht.

7. Speichern Sie die Datei `DeleteTable.java`.

## <a name="build-and-package-the-application"></a>Erstellen und Packen der Anwendung

1. Verwenden Sie im Verzeichnis `hbaseapp` den folgenden Befehl, um eine JAR-Datei zu erstellen, die die Anwendung enthält:

    ```bash
    mvn clean package
    ```

    Dieser Befehl erstellt und verpackt die Anwendung in eine JAR-Datei.

2. Sobald der Befehl abgeschlossen ist, enthält das Verzeichnis `hbaseapp/target` eine Datei mit dem Namen `hbaseapp-1.0-SNAPSHOT.jar`.

   > [!NOTE]
   > Die Datei `hbaseapp-1.0-SNAPSHOT.jar` ist eine Uber-JAR-Datei. Sie enthält alle Abhängigkeiten, die zum Ausführen der Anwendung erforderlich sind.


## <a name="upload-the-jar-and-run-jobs-ssh"></a>Hochladen der JAR-Datei und Ausführen von Aufträgen (SSH)

In den folgenden Schritten wird `scp` zum Kopieren der JAR-Datei auf den primären Hauptknoten von HBase im HDInsight-Cluster verwendet. Der Befehl `ssh` dient dann zum Herstellen einer Verbindung mit dem Cluster und direkten Ausführen des Beispiels auf dem Hauptknoten.

1. Verwenden Sie den folgenden Befehl, um die JAR-Datei in den Cluster hochzuladen:

    ```bash
    scp ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:hbaseapp-1.0-SNAPSHOT.jar
    ```

    Ersetzen Sie `USERNAME` durch den Namen Ihrer SSH-Anmeldung. Ersetzen Sie `CLUSTERNAME` durch den Namen Ihres HDInsight-Clusters.

2. Stellen Sie mit dem folgenden Befehl eine Verbindung zum HBase-Cluster her:

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Ersetzen Sie `USERNAME` durch den Namen Ihrer SSH-Anmeldung. Ersetzen Sie `CLUSTERNAME` durch den Namen Ihres HDInsight-Clusters.

3. Verwenden Sie den folgenden Befehl, um eine HBase-Tabelle mithilfe der Java-Anwendung zu erstellen:

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.CreateTable
    ```

    Mit diesem Befehl wird eine HBase-Tabelle mit dem Namen **people** erstellt, die dann mit Daten aufgefüllt wird.

4. Suchen Sie mit dem folgenden Befehl nach E-Mail-Adressen in der Tabelle:

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.SearchByEmail contoso.com
    ```

    Sie erhalten folgende Ergebnisse:

        Franklin Holtz - ID: 2
        Franklin Holtz - franklin@contoso.com - ID: 2
        Rae Schroeder - ID: 4
        Rae Schroeder - rae@contoso.com - ID: 4
        Gabriela Ingram - ID: 6
        Gabriela Ingram - gabriela@contoso.com - ID: 6

5. Verwenden Sie zum Löschen der Tabelle den folgenden Befehl:

    

## <a name="upload-the-jar-and-run-jobs-powershell"></a>Hochladen der JAR-Datei und Ausführen von Aufträgen (PowerShell)

In den folgenden Schritte wird Azure PowerShell zum Hochladen der JAR-Datei in den Standardspeicher Ihres HBase-Clusters verwendet. HDInsight-Cmdlets werden dann zur Remoteausführung der Beispiele verwendet.

1. Nachdem Sie Azure PowerShell installiert und konfiguriert haben, erstellen Sie eine Datei mit dem Namen `hbase-runner.psm1`. Verwenden Sie als Inhalt der Datei den folgenden Text:

   ```powershell
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
   ```

    Diese Datei enthält zwei Module:

   * **Add-HDInsightFile** wird für das Hochladen von Dateien in den Cluster verwendet.
   * **Start-HBaseExample** wird für das Ausführen der bereits erstellten Klassen verwendet.

2. Speichern Sie die Datei `hbase-runner.psm1`.

3. Öffnen Sie ein neues Azure PowerShell-Fenster, wechseln Sie in das Verzeichnis `hbaseapp`, und führen Sie dann den folgenden Befehl aus:

    ```powershell
    PS C:\ Import-Module c:\path\to\hbase-runner.psm1
    ```

    Legen Sie den Pfad auf den Speicherort der früher erstellten Datei `hbase-runner.psm1` fest. Mit diesem Befehl wird das Modul bei Azure PowerShell registriert.

4. Verwenden Sie den folgenden Befehl zum Hochladen von `hbaseapp-1.0-SNAPSHOT.jar` in Ihren Cluster.

    ```powershell
    Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername
    ```

    Ersetzen Sie `hdinsightclustername` durch den Namen Ihres Clusters. Der Befehl lädt `hbaseapp-1.0-SNAPSHOT.jar` an den Speicherort `example/jars` im primären Speicher für Ihren Cluster hoch.

5. Verwenden Sie den folgenden Befehl, um eine Tabelle mithilfe von `hbaseapp` zu erstellen:

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername
    ```

    Ersetzen Sie `hdinsightclustername` durch den Namen Ihres Clusters.

    Dieser Befehl erstellt eine Tabelle namens **people** in HBase in Ihrem HDInsight-Cluster. Bei diesem Befehl erfolgt keine Ausgabe im Konsolenfenster.

6. Verwenden Sie den folgenden Befehl, um nach Einträgen in die Tabelle zu suchen:

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com
    ```

    Ersetzen Sie `hdinsightclustername` durch den Namen Ihres Clusters.

    Dieser Befehl verwendet die Klasse `SearchByEmail`, um nach allen Zeilen zu suchen, in denen die Spaltenfamilie `contactinformation` und die Spalte `email` die Zeichenfolge `contoso.com` enthalten. Daraufhin sollte Sie folgende Ergebnisse erhalten:

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    Wenn **fabrikam.com** als `-emailRegex`-Wert verwendet wird, werden Benutzer zurückgegeben, die **fabrikam.com** im Feld „E-Mail“ enthalten. Sie können als Suchbegriffe auch reguläre Ausdrücke verwenden. Mit **^r** werden beispielsweise E-Mail-Adressen zurückgegeben, die mit dem Buchstaben „r“ beginnen.

### <a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a>Keine oder unerwartete Ergebnisse bei Verwenden von "Start-HBaseExample"

Verwenden Sie den Parameter `-showErr`, wenn Sie den Standardfehler (STDERR) sehen möchten, der während der Ausführung des Auftrags erzeugt wurde.

## <a name="delete-the-table"></a>Löschen der Tabelle

Wenn Sie mit dem Beispiel fertig sind, verwenden Sie den folgenden Befehl zum Löschen der Tabelle **people**, die in diesem Beispiel verwendet wurde:

__Über eine `ssh`-Sitzung__:

`yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.DeleteTable`

__In Azure PowerShell__

`Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername`

## <a name="next-steps"></a>Nächste Schritte

[Informationen zum Verwenden von SQuirreL SQL mit HBase](hdinsight-hbase-phoenix-squirrel-linux.md)

