---
title: Entwickeln von Anwendungen in Azure Data Lake Store per Java SDK | Microsoft-Dokumentation
description: "Verwenden des Azure Data Lake Store Java SDK zum Erstellen eines Data Lake Store-Kontos und Ausführen grundlegender Vorgänge im Data Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: d10e09db-5232-4e84-bb50-52efc2c21887
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/23/2016
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 091fadce064086d82b833f8e44edfbba125d3e6b
ms.openlocfilehash: cb5babdd8fea3615d8aa27f05a07c3b489f3faa4


---
# <a name="get-started-with-azure-data-lake-store-using-java"></a>Erste Schritte mit Azure Data Lake-Speicher mit Java
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST-API](data-lake-store-get-started-rest-api.md)
> * [Azure-Befehlszeilenschnittstelle](data-lake-store-get-started-cli.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
> 

Hier erfahren Sie, wie Sie mit dem Azure Data Lake Store Java SDK einfache Vorgänge ausführen (also beispielsweise Ordner erstellen oder Datendateien hoch- und herunterladen). Weitere Informationen zu Data Lake finden Sie unter [Übersicht über Azure Data Lake Store](data-lake-store-overview.md).

Die Java SDK-API-Dokumente für Azure Data Lake Store finden Sie [hier](https://azure.github.io/azure-data-lake-store-java/javadoc/).

## <a name="prerequisites"></a>Voraussetzungen
* Java Development Kit (JDK 7 oder höher mit Java-Version 1.7 oder höher)
* Azure Data Lake Store-Konto. Führen Sie die Schritte der Anleitung unter [Erste Schritte mit dem Azure Data Lake-Speicher mithilfe des Azure-Portals](data-lake-store-get-started-portal.md)aus.
* [Maven](https://maven.apache.org/install.html). Dieses Tutorial verwendet Maven für die Erstellung und für Projektabhängigkeiten. Die Erstellung ist zwar auch ohne ein Buildsystem wie Maven oder Gradle möglich, mit einem solchen System lassen sich Abhängigkeiten jedoch deutlich einfacher verwalten.
* (Optional) Eine IDE wie [IntelliJ IDEA](https://www.jetbrains.com/idea/download/) oder [Eclipse](https://www.eclipse.org/downloads/).

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Wie authentifiziere ich mich mithilfe von Azure Active Directory?
In diesem Tutorial verwenden wir einen geheimen Clientschlüssel der Azure AD-Anwendung, um ein Azure Active Directory-Token abzurufen (Dienst-zu-Dienst-Authentifizierung). Mithilfe dieses Tokens erstellen wir dann ein Data Lake Store-Clientobjekt für Datei- und Verzeichnisvorgänge. Zur Authentifizierung bei Azure Data Lake Store unter Verwendung des geheimen Clientschlüssels führen wir die folgenden allgemeinen Schritte aus:

1. Erstellen einer Azure AD-Webanwendung
2. Abrufen der Client-ID, des geheimen Schlüssels und des Token-Endpunkts für die Azure AD-Webanwendung
3. Konfigurieren des Zugriffs für die Azure AD-Webanwendung auf die Data Lake Store-Datei bzw. den Data Lake Store-Ordner, auf die bzw. auf den Sie über die zu erstellende Java-Anwendung zugreifen möchten.

Eine entsprechende Anleitung finden Sie unter [Erstellen einer Active Directory-Anwendung](data-lake-store-authenticate-using-active-directory.md#create-an-active-directory-application).

Token können mit Azure Active Directory auch auf andere Weise abgerufen werden. So können Sie einen Authentifizierungsmechanismus wählen, der für Ihr Szenario geeignet ist. Dabei kann es sich beispielsweise um eine in einem Browser ausgeführte Anwendung, um eine als Desktopanwendung verteilte Anwendung oder um eine Serveranwendung handeln, die lokal oder auf einem virtuellen Azure-Computer ausgeführt wird. Außerdem haben Sie die Wahl zwischen verschiedenen Arten von Anmeldeinformationen. Hierzu zählen etwa Kennwörter, Zertifikate und die zweistufige Authentifizierung. Darüber hinaus können Sie mit Azure Active Directory Ihre lokalen Active Directory-Benutzer mit der Cloud synchronisieren. Ausführlichere Informationen finden Sie unter [Authentifizierungsszenarien für Azure Active Directory](../active-directory/active-directory-authentication-scenarios.md). 

## <a name="create-a-java-application"></a>Erstellen einer Java-Anwendung
Das auf [GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/) verfügbare Codebeispiel veranschaulicht Schritt für Schritt den Prozess zum Erstellen der Dateien im Speicher, zum Verketten von Dateien, zum Herunterladen einer Datei und zum Löschen einiger Dateien aus dem Speicher. Dieser Abschnitt des Artikels geht auf die wichtigsten Teile des Codes ein.

1. Erstellen Sie ein Maven-Projekt. Verwenden Sie hierzu [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) an der Befehlszeile oder eine IDE. Eine Anleitung zum Erstellen eines Java-Projekts mit IntelliJ finden Sie [hier](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html). Eine Anleitung zum Erstellen eines Projekts mit Eclipse finden Sie [hier](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm). 
2. Fügen Sie Ihrer Maven-Datei **pom.xml** die folgenden Abhängigkeiten hinzu. Fügen Sie zwischen dem Tag **\</version>** und dem Tag **\</project>** den folgenden Textausschnitt hinzu:
   
        <dependencies>
          <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-data-lake-store-sdk</artifactId>
            <version>2.1.4</version>
          </dependency>
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-nop</artifactId>
            <version>1.7.21</version>
          </dependency>
        </dependencies>
   
    Die erste Abhängigkeit ist die Verwendung des Data Lake Store SDK (`azure-data-lake-store-sdk`) aus dem Maven-Repository. Die zweite Abhängigkeit (`slf4j-nop`) ist die Angabe des Protokollierungsframeworks für diese Anwendung. Das Data Lake Store SDK verwendet die Protokollierungsfassade [slf4j](http://www.slf4j.org/), bei der Sie aus einer Reihe gängiger Protokollierungsframeworks wie log4j, Java-Protokollierung oder Logback wählen oder die Protokollierung deaktivieren können. Da wir im vorliegenden Beispiel die Protokollierung deaktivieren möchten, verwenden wir die Bindung **slf4j-nop**. Informationen zur Verwendung anderer Protokollierungsoptionen für Ihre App finden Sie [hier](http://www.slf4j.org/manual.html#projectDep).

### <a name="add-the-application-code"></a>Hinzufügen des Anwendungscodes
Der Code setzt sich aus drei Hauptkomponenten zusammen:

1. Abrufen des Azure Active Directory-Tokens
2. Erstellen eines Data Lake Store-Clients mithilfe des Tokens
3. Ausführen von Vorgängen mithilfe des Data Lake Store-Clients

#### <a name="step-1-obtain-an-azure-active-directory-token"></a>Schritt 1: Abrufen eines Azure Active Directory-Tokens
Das Data Lake Store SDK ermöglicht die komfortable Verwaltung der Sicherheitstoken, die für die Kommunikation mit dem Data Lake Store-Konto benötigt werden. Das SDK ermöglicht jedoch auch die Verwendung anderer Methoden. Token können daher auch auf andere Weise abgerufen werden – etwa mit dem [Azure Active Directory SDK](https://github.com/AzureAD/azure-activedirectory-library-for-java) oder mit eigenem benutzerdefiniertem Code.

Wenn Sie mit dem Data Lake Store SDK Token für die zuvor erstellte Active Directory-Webanwendung abrufen möchten, verwenden Sie eine der Unterklassen von `AccessTokenProvider`. (Im Beispiel weiter unten wird `ClientCredsTokenProvider` verwendet.) Der Tokenanbieter speichert die verwendeten Anmeldeinformationen zwischen, um das Token im Arbeitsspeicher abzurufen, und erneuert das Token automatisch, bevor es abläuft. Sie können zwar auch eigene Unterklassen von `AccessTokenProvider` erstellen, damit Token durch Ihren Kundencode abgerufen werden, wir verwenden jedoch fürs Erste die Unterklasse aus dem SDK.

Ersetzen Sie **FILL-IN-HERE** durch die Werte für die Azure Active Directory-Webanwendung.

    private static String clientId = "FILL-IN-HERE";
    private static String authTokenEndpoint = "FILL-IN-HERE";
    private static String clientKey = "FILL-IN-HERE";

    AccessTokenProvider provider = new ClientCredsTokenProvider(authTokenEndpoint, clientId, clientKey);

#### <a name="step-2-create-an-azure-data-lake-store-client-adlstoreclient-object"></a>Schritt 2: Erstellen eines Azure Data Lake Store-Clientobjekts (ADLStoreClient)
Wen Sie ein Objekt vom Typ [ADLStoreClient](https://azure.github.io/azure-data-lake-store-java/javadoc/) erstellen, müssen Sie den Namen des Data Lake Store-Kontos und den Tokenanbieter aus dem vorherigen Schritt angeben. Bei dem Namen des Data Lake Store-Kontos muss es sich um einen vollqualifizierten Domänennamen handeln. Ersetzen Sie **FILL-IN-HERE** also beispielsweise durch **mydatalakestore.azuredatalakestore.net**.

    private static String accountFQDN = "FILL-IN-HERE";  // full account FQDN, not just the account name
    ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, provider);

### <a name="step-3-use-the-adlstoreclient-to-perform-file-and-directory-operations"></a>Schritt 3: Ausführen von Datei- und Verzeichnisvorgängen mithilfe des ADLStoreClient-Objekts
Der folgende Code enthält Beispielausschnitte für einige häufig verwendete Vorgänge. Weitere Vorgänge finden Sie in den vollständigen [API-Dokumenten für das Data Lake Store Java SDK](https://azure.github.io/azure-data-lake-store-java/javadoc/) des **ADLStoreClient**-Objekts.

Für dateibezogene Lese- und Schreibvorgänge werden standardmäßige Java-Datenströme verwendet. Es können also beliebige Java-Datenströme mit den Data Lake Store-Datenströmen kombiniert werden, um Java-Standardfunktionen zu nutzen. Hierzu zählen etwa Ausgabedatenströme für formatierte Ausgaben oder Komprimierungs- oder Verschlüsselungsdatenströme zur Verwendung von Zusatzfunktionen.

     // create file and write some content
     String filename = "/a/b/c.txt";
     OutputStream stream = client.createFile(filename, IfExists.OVERWRITE  );
     PrintStream out = new PrintStream(stream);
     for (int i = 1; i <= 10; i++) {
         out.println("This is line #" + i);
         out.format("This is the same line (%d), but using formatted output. %n", i);
     }
     out.close();
    
    // set file permission
    client.setPermission(filename, "744");

    // append to file
    stream = client.getAppendStream(filename);
    stream.write(getSampleContent());
    stream.close();

    // Read File
    InputStream in = client.getReadStream(filename);
    byte[] b = new byte[64000];
    while (in.read(b) != -1) {
        System.out.write(b);
    }
    in.close();

    // concatenate the two files into one
    List<String> fileList = Arrays.asList("/a/b/c.txt", "/a/b/d.txt");
    client.concatenateFiles("/a/b/f.txt", fileList);

    //rename the file
    client.rename("/a/b/f.txt", "/a/b/g.txt");

    // list directory contents
    List<DirectoryEntry> list = client.enumerateDirectory("/a/b", 2000);
    System.out.println("Directory listing for directory /a/b:");
    for (DirectoryEntry entry : list) {
        printDirectoryInfo(entry);
    }

    // delete directory along with all the subdirectories and files in it
    client.deleteRecursive("/a");

#### <a name="step-4-build-and-run-the-application"></a>Schritt 4: Erstellen und Ausführen der Anwendung
1. Verwenden Sie in einer IDE die Schaltfläche **Ausführen**, um die Anwendung auszuführen. Verwenden Sie in Maven den Befehl [exec:exec](http://www.mojohaus.org/exec-maven-plugin/exec-mojo.html), um die Anwendung auszuführen.
2. Wenn Sie eine eigenständige, über die Befehlszeile ausführbare JAR-Datei generieren möchten, erstellen Sie mithilfe des [Maven-Assembly-Plug-Ins](http://maven.apache.org/plugins/maven-assembly-plugin/usage.html) eine JAR-Datei mit sämtlichen Abhängigkeiten. Ein Beispiel für die erforderliche Vorgehensweise finden Sie in der Datei „pom.xml“ aus dem [Beispielquellcode auf GitHub](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started/blob/master/pom.xml).

## <a name="next-steps"></a>Nächste Schritte
* [Kennenlernen von JavaDoc für das Java SDK](https://azure.github.io/azure-data-lake-store-java/javadoc/)
* [Sichern von Daten in Data Lake-Speicher](data-lake-store-secure-data.md)
* [Verwenden von Azure Data Lake Analytics mit Data Lake-Speicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Verwenden von Azure HDInsight mit Data Lake-Speicher](data-lake-store-hdinsight-hadoop-use-portal.md)




<!--HONumber=Jan17_HO5-->


