---
title: "Einschränken des HDInsight-Zugriffs auf Daten mithilfe von Shared Access Signatures"
description: "Erfahren Sie, wie Sie mit Shared Access Signatures den HDInsight-Zugriff auf Daten in Azure-Speicherblobs einschränken."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 7bcad2dd-edea-467c-9130-44cffc005ff3
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/17/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: ccd1dffda19718a434fc09bb74a536714799740a
ms.openlocfilehash: 6187106a9aa98107d89e65fe4c7a0e8a27befa87


---
# <a name="use-azure-storage-shared-access-signatures-to-restrict-access-to-data-with-hdinsight"></a>Verwenden von Azure Storage Shared Access Signatures zum Einschränken des Zugriffs auf Daten mit HDInsight
HDInsight verwendet zur Datenspeicherung Azure-Speicherblobs. Während HDInsight Vollzugriff auf das Blob benötigt, das als Standardspeicher des Clusters verwendet wird, können Sie Berechtigungen für Daten beschränken, die in anderen vom Cluster verwendeten Blobs gespeichert sind. Sie können z. B. bestimmte Daten mit Schreibschutz versehen. Nutzen Sie dazu Shared Access Signatures.

Shared Access Signatures (SAS) sind ein Feature von Azure Storage-Konten, das das Einschränken des Zugriffs auf Daten ermöglicht. Sie können beispielsweise einen schreibgeschützten Zugriff auf Daten bieten. In diesem Dokument erfahren Sie, wie Sie in HDInsight mithilfe von SAS einen Lese- und Auflistungszugriff auf einen Blobcontainer aktivieren.

## <a name="requirements"></a>Anforderungen
* Ein Azure-Abonnement
* C# oder Python. C#-Beispielcode wird als Visual Studio-Projektmappe bereitgestellt.
  
  * Die Visual Studio-Version 2013 oder 2015 ist erforderlich.
  * Die Python-Version 2.7 oder höher ist erforderlich.
* Einen Linux-basierten HDInsight-Cluster ODER [Azure PowerShell][powershell] – Wenn Sie bereits über einen Linux-basierten Cluster verfügen, können mit Ambari eine Shared Access Signature zum Cluster hinzuzufügen. Falls nicht, können Sie mit Azure PowerShell einen neuen Cluster erstellen und eine Shared Access Signature während der Clustererstellung hinzufügen.

    > [!IMPORTANT]
    > Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

* Die Beispieldateien aus [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature). Dieses Repository enthält Folgendes:
  
  * Ein Visual Studio-Projekt, in dem ein Speichercontainer, eine gespeicherte Richtlinie und SAS für die Verwendung mit HDInsight erstellt werden kann.
  * Ein Python-Skript zum Erstellen eines Speichercontainers, einer gespeicherten Richtlinie und einer SAS für die Verwendung mit HDInsight.
  * Ein PowerShell-Skript zum Erstellen eines neuen HDInsight-Clusters und seiner Konfiguration für die Verwendung von SAS.

## <a name="shared-access-signatures"></a>Shared Access Signatures
Es gibt zwei Arten von Shared Access Signatures:

* Ad hoc: Startzeit, Ablaufzeit und Berechtigungen für die SAS werden direkt im SAS-URI angegeben (bzw. impliziert, falls die Startzeit ausgelassen wird).
* Gespeicherte Zugriffsrichtlinie: Gespeicherte Zugriffsrichtlinien werden für Ressourcencontainer (Blobcontainer, Tabellen, Warteschlangen oder Dateifreigaben) definiert und dienen zur Verwaltung von Einschränkungen für eine oder mehrere Shared Access Signatures. Wenn Sie eine SAS mit einer gespeicherten Zugriffsrichtlinie verknüpfen, erbt die SAS die Einschränkungen (Startzeit, Ablaufzeit und Berechtigungen) dieser gespeicherten Zugriffsrichtlinie.

Der Unterschied zwischen diesen beiden Formen ist wichtig für ein Schlüsselszenario: Widerruf. Eine SAS ist eine URL und kann daher von beliebiger Stelle verwendet werden, unabhängig davon, wer die SAS ursprünglich angefordert hatte. Wenn eine SAS veröffentlicht wird, kann diese von beliebiger Stelle weltweit verwendet werden. Auf diese Weise verteilte SAS sind gültig, bis eines von vier Ereignissen eintritt:

1. Die Ablaufzeit der SAS wird erreicht.
2. Die Ablaufzeit der von der SAS referenzierten gespeicherten Zugriffsrichtlinie wird erreicht (falls eine gespeicherte Zugriffsrichtlinie referenziert wurde und diese eine Ablaufzeit definiert). Dies geschieht entweder, wenn das Zeitintervall abläuft, oder wenn Sie die Ablaufzeit der gespeicherten Zugriffsrichtlinie auf einen Zeitpunkt in der Vergangenheit gesetzt haben, um die SAS zu widerrufen.
3. Die von der SAS referenzierte gespeicherte Zugriffsrichtlinie wird gelöscht, wodurch die SAS ebenfalls widerrufen wird. Wenn Sie die gespeicherte Zugriffsrichtlinie mit demselben Namen erneut erstellen, sind alle existierenden SAS-Tokens gemäß der Berechtigungen der gespeicherten Zugriffsrichtlinie wieder gültig (sofern die Ablaufzeit der SAS noch nicht erreicht wurde). Falls Sie die SAS widerrufen möchten, müssen Sie einen anderen Namen verwenden, wenn Sie die gespeicherte Zugriffsrichtlinie mit einer Ablaufzeit in der Zukunft erneut erstellen.
4. Der Kontoschlüssel, mit dem die SAS erstellt wurde, wird erneut generiert. Dies führt dazu, dass die Authentifizierung aller Anwendungskomponenten mit diesem Kontoschlüssel misslingt, bis diese entweder mit dem anderen gültigen Kontoschlüssel oder dem neu generierten Kontoschlüssel aktualisiert werden.

> [!IMPORTANT]
> Ein SAS-URI wird dem Kontoschlüssel, mit dem die Signatur erstellt wurde, und der zugehörigen gespeicherten Zugriffsrichtlinie (sofern vorhanden) zugeordnet. Wenn keine gespeicherte Zugriffsrichtlinie angegeben wird, kann eine SAS nur durch Änderung des Kontoschlüssels aufgehoben werden. 
> 
> 

Es wird empfohlen, stets gespeicherte Zugriffsrichtlinien zu verwenden, sodass Sie Signaturen entweder aufheben oder bei Bedarf das Ablaufdatum verlängern können. In den Schritten in diesem Dokument werden gespeicherte Zugriffsrichtlinien zum Generieren von SAS verwendet.

Weitere Informationen zu Shared Access Signatures finden Sie unter [Grundlagen zum SAS-Modell](../storage/storage-dotnet-shared-access-signature-part-1.md).

## <a name="create-a-stored-policy-and-generate-a-sas"></a>Erstellen einer gespeicherte Richtlinie und Generieren einer SAS
Derzeit müssen Sie eine gespeicherte Richtlinie programmgesteuert erstellen. Das C#- und Python-Beispiel zum Erstellen einer gespeicherten Richtlinie und SAS finden Sie unter [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature).

### <a name="create-a-stored-policy-and-sas-using-c"></a>Erstellen einer gespeicherte Richtlinie und SAS mit C\
1. Öffnen Sie die Projektmappe in Visual Studio.
2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **SASToken**, und wählen Sie **Eigenschaften** aus.
3. Wählen Sie **Einstellungen** aus, und fügen Sie Werte für die folgenden Einträge hinzu:
   
   * StorageConnectionString: Die Verbindungszeichenfolge für das Speicherkonto, für das eine gespeicherte Richtlinie und SAS erstellt werden soll. Das Format muss `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` sein, wobei `myaccount` der Name Ihres Speicherkontos und `mykey` der Schlüssel des Speicherkontos ist.
   * ContainerName: Der Container im Speicherkonto, auf den Sie den Zugriff beschränken möchten.
   * SASPolicyName: Der Name der gespeicherten Richtlinie, die erstellt wird.
   * FileToUpload: Der Pfad zu einer Datei, die in den Container hochgeladen wird.
4. Führen Sie das Projekt aus. Ein Konsolenfenster wird angezeigt, und Informationen ähnlich den folgenden werden angezeigt, nachdem die SAS generiert wurde:
   
        Container SAS token using stored access policy: sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14
   
    Speichern Sie das SAS-Richtlinientoken, da Sie es benötigen, wenn Sie das Speicherkonto Ihrem HDInsight-Cluster zuordnen. Sie benötigen auch den Namen des Speicherkontos und den Containernamen.

### <a name="create-a-stored-policy-and-sas-using-python"></a>Erstellen einer gespeicherte Richtlinie und SAS mit Python
1. Öffnen Sie die Datei „SASToken.py“, und ändern Sie die folgenden Werte:
   
   * policy\_name: Der Name der gespeicherten Richtlinie, die erstellt wird.
   * storage\_account\_name: Der Name Ihres Speicherkontos.
   * storage\_account\_key: Der Schlüssel für das Speicherkonto.
   * storage\_container\_name: Der Container im Speicherkonto, für den Sie den Zugriff beschränken möchten.
   * example\_file\_path: Der Pfad zu einer Datei, die in den Container hochgeladen wird.
2. Führen Sie das Skript aus. Nach Abschluss des Skripts wird ein SAS-Token ähnlich dem folgenden angezeigt:
   
        sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14
   
    Speichern Sie das SAS-Richtlinientoken, da Sie es benötigen, wenn Sie das Speicherkonto Ihrem HDInsight-Cluster zuordnen. Sie benötigen auch den Namen des Speicherkontos und den Containernamen.

## <a name="use-the-sas-with-hdinsight"></a>Verwenden der SAS mit HDInsight
Wenn Sie einen HDInsight-Cluster erstellen, müssen Sie ein primäres Speicherkonto angeben. Optional können Sie zusätzliche Speicherkonten angeben. Beide Methoden zum Hinzufügen von Speicher benötigen Vollzugriff auf die Speicherkonten und Container, die verwendet werden.

Um eine Shared Access Signature zum Begrenzen des Zugriffs auf einen Container zu verwenden, müssen Sie der Konfiguration von **core-site** einen benutzerdefinierten Eintrag hinzufügen.

* Für **Windows-basierte** oder **Linux-basierte** HDInsight-Cluster kann dies während der Erstellung des Clusters mithilfe von PowerShell erfolgen.
* Für **Linux-basierte** HDInsight-Cluster ändern Sie die Konfiguration nach der Erstellung des Clusters mit Ambari.

### <a name="create-a-new-cluster-that-uses-the-sas"></a>Erstellen eines neuen Clusters, der die SAS verwendet
Ein Beispiel zum Erstellen eines HDInsight-Clusters, der die SAS verwendet, befindet sich im Verzeichnis `CreateCluster` des Repositorys. Führen Sie zu dessen Nutzung die folgenden Schritte aus:

1. Öffnen Sie die Datei `CreateCluster\HDInsightSAS.ps1` in einem Text-Editor, und ändern Sie am Anfang des Dokuments die folgenden Werte.
   
        # Replace 'mycluster' with the name of the cluster to be created
        $clusterName = 'mycluster'
        # Valid values are 'Linux' and 'Windows'
        $osType = 'Linux'
        # Replace 'myresourcegroup' with the name of the group to be created
        $resourceGroupName = 'myresourcegroup'
        # Replace with the Azure data center you want to the cluster to live in
        $location = 'North Europe'
        # Replace with the name of the default storage account to be created
        $defaultStorageAccountName = 'mystorageaccount'
        # Replace with the name of the SAS container created earlier
        $SASContainerName = 'sascontainer'
        # Replace with the name of the SAS storage account created earlier
        $SASStorageAccountName = 'sasaccount'
        # Replace with the SAS token generated earlier
        $SASToken = 'sastoken'
        # Set the number of worker nodes in the cluster
        $clusterSizeInNodes = 2
   
    Ändern Sie z. B. `'mycluster'` in den Namen des Clusters, den Sie erstellen möchten. Die SAS-Werte müssen den Werten in den vorherigen Schritten zum Erstellen eines Speicherkontos und SAS-Tokens entsprechen.
   
    Nachdem Sie die Werte geändert haben, speichern Sie die Datei.
2. Öffnen Sie eine neue Azure PowerShell-Eingabeaufforderung. Wenn Sie mit Azure PowerShell nicht vertraut sind oder dieses Feature nicht installiert haben, lesen Sie unter [Installieren und Konfigurieren von Azure PowerShell][powershell] nach.
3. Führen Sie an der Eingabeaufforderung Folgendes aus, um sich bei Ihrem Azure-Abonnement zu authentifizieren:
   
        Login-AzureRmAccount
   
    Melden Sie sich bei Aufforderung mit dem Konto Ihres Azure-Abonnements an.
   
    Wenn Ihre Anmeldung mehreren Azure-Abonnements zugeordnet ist, müssen Sie möglicherweise `Select-AzureRmSubscription` zur Auswahl des gewünschten Abonnements verwenden.
4. Wechseln Sie an der Eingabeaufforderung zum Verzeichnis `CreateCluster` , das die Datei „HDInsightSAS.ps1“ enthält. Geben Sie Folgendes zum Ausführen des Skripts an:
   
        .\HDInsightSAS.ps1
   
    Bei der Ausführung des Skripts zum Erstellen der Ressourcengruppe und Speicherkonten wird die Ausgabe an der PowerShell-Eingabeaufforderung protokolliert. Es fordert Sie dann auf, den HTTP-Benutzer für den HDInsight-Cluster einzugeben. Dies ist das Benutzerkonto, das verwendet wird, um den HTTP/S-Zugriff auf den Cluster zu schützen.
   
    Wenn Sie einen Linux-basierten Cluster erstellen, werden Sie auch zur Angabe des Namens und Kennworts eines SSH-Benutzerkontos aufgefordert. Dieses wird für eine Remoteanmeldung am Cluster verwendet.
   
   > [!IMPORTANT]
   > Wenn Sie zur Angabe des HTTP/S- oder SSH-Benutzernamens und Kennworts aufgefordert werden, müssen Sie ein Kennwort angeben, das die folgenden Kriterien erfüllt:
   > 
   > * Mindestens 10 Zeichen
   > * Mindestens eine Ziffer
   > * Mindestens ein nicht alphanumerisches Zeichen
   > * Mindestens ein Groß- oder Kleinbuchstaben
   > 
   > 

Die Ausführung dieses Skripts dauert meist ca. 15 Minuten. Wenn das Skript ohne Fehler abgeschlossen wird, wurde der Cluster erstellt.

### <a name="update-an-existing-cluster-to-use-the-sas"></a>Aktualisieren einen vorhandenen Clusters für die SAS-Verwendung
Wenn Sie bereits einen Linux-basierten Cluster haben, können Sie die SAS der **core-site** -Konfiguration mithilfe der folgenden Schritte hinzufügen:

1. Öffnen Sie die Ambari-Webbenutzeroberfläche für den Cluster. Die Adresse dieser Seite lautet: https://IHRCLUSTERNAME.azurehdinsight.net Authentifizieren Sie sich bei Aufforderung am Cluster mithilfe des Administratornamens (admin) und des Kennworts, das Sie beim Erstellen des Clusters verwendet haben.
2. Wählen Sie links auf der Ambari-Webbenutzeroberfläche **HDFS** und dann in der Mitte der Seite die Registerkarte **Configs** aus.
3. Wählen Sie die Registerkarte **Advanced** aus, und scrollen Sie zum Abschnitt **Custom core-site**.
4. Erweitern Sie den Abschnitt **Custom core-site**, scrollen Sie zum Seitenende, und klicken Sie auf den Link **Add property...**. Verwenden Sie für die Felder **Key** und **Value** die folgenden Werte:
   
   * **Key**: fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net
   * **Value**: Die SAS, die von der zuvor ausgeführten C#- oder Python-Anwendung zurückgegeben wurde.
     
     Ersetzen Sie **CONTAINERNAME** durch den Containernamen, den Sie mit der C#- oder SAS-Anwendung verwendet haben. Ersetzen Sie **STORAGEACCOUNTNAME** durch den Namen des verwendeten Speicherkontos.
5. Klicken Sie auf die Schaltfläche **Add**, um diesen Schlüssel und Wert zu speichern. Klicken Sie dann auf die Schaltfläche **Save**, um die Konfigurationsänderungen zu speichern. Fügen Sie bei Aufforderung eine Beschreibung der Änderung hinzu (z. B. „Hinzufügen des SAS-Speicherzugriffs“), und klicken Sie dann auf **Save**.
   
    Klicken Sie auf **OK** , wenn die Änderungen abgeschlossen sind.
   
   > [!IMPORTANT]
   > Dies speichert die geänderte Konfiguration, doch Sie müssen mehrere Dienste neu starten, damit die Änderung wirksam wird.
   > 
   > 
6. Wählen Sie auf der Ambari-Webbenutzeroberfläche in der Liste auf der linken Seite **HDFS** und dann in der Dropdownliste **Service Actions** auf der rechten Seite **Restart All** aus. Wählen Sie bei Aufforderung **Turn on maintenance mode** und dann „Conform Restart All“ aus.
   
    Wiederholen Sie diesen Vorgang für die Einträge „MapReduce2“ und „YARN“ in der Liste auf der linken Seite.
7. Sobald diese neu gestartet wurden, wählen Sie sie nacheinander aus und deaktivieren den Wartungsmodus in der Dropdownliste **Service Actions** .

## <a name="test-restricted-access"></a>Testen des eingeschränkten Zugriffs
Um sicherzustellen, dass der Zugriff eingeschränkt wird, verwenden Sie die folgenden Methoden:

* Nutzen Sie für **Windows-basierte** -Cluster Remotedesktop zur Verbindung mit dem Cluster. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit HDInsight mithilfe von RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp) .
  
    Nachdem die Verbindung aufgebaut wurde, verwenden Sie das Symbol **Hadoop-Befehlszeile** auf dem Desktop, um eine Eingabeaufforderung zu öffnen.
* Verwenden Sie für **Linux-basierte** -Cluster SSH zur Verbindung mit dem Cluster. Informationen zur Verwendung von SSH mit Linux-basierten Clustern finden Sie in den folgenden Artikeln:
  
  * [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, OS X und Unix](hdinsight-hadoop-linux-use-ssh-unix.md)
  * [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

Nachdem die Verbindung mit dem Cluster hergestellt wurde, befolgen Sie die folgenden Schritte zum Überprüfen, ob Sie Elemente im SAS-Speicherkonto ausschließlich lesen und auflisten können:

1. Geben Sie an der Eingabeaufforderung den folgenden Befehl ein. Ersetzen Sie **SASCONTAINER** durch den Namen des Containers, den Sie für das SAS-Speicherkonto erstellt haben. Ersetzen Sie **SASACCOUNTNAME** durch den Namen des Speicherkontos, das für die SAS verwendet wird:
   
        hdfs dfs -ls wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/
   
    Dadurch wird der Inhalt des Containers aufgelistet, der die Datei enthalten sollte, die bei der Erstellung des Containers und der SAS hochgeladen wurde.
2. Geben Sie Folgendes an, um sicherzustellen, dass Sie den Inhalt der Datei lesen können. Ersetzen Sie **SASCONTAINER** und **SASACCOUNTNAME** wie im vorigen Schritt. Ersetzen Sie **FILENAME** durch den Namen der Datei, die im vorherigen Befehl angezeigt wurde:
   
        hdfs dfs -text wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME
   
    Dadurch wird der Inhalt der Datei aufgelistet.
3. Geben Sie Folgendes an, um die Datei in das lokale Dateisystem herunterzuladen:
   
        hdfs dfs -get wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME testfile.txt
   
    Damit wird die Datei in eine lokale Datei mit dem Namen **testfile.txt**heruntergeladen.
4. Gehen Sie zum Hochladen der lokalen Datei in eine neue Datei mit dem Namen **testupload.txt** in den SAS-Speicher Folgendes an:
   
        hdfs dfs -put testfile.txt wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/testupload.txt
   
    Sie erhalten eine Meldung wie die folgende:
   
        put: java.io.IOException
   
    Dieser Fehler tritt auf, weil der Speicherort nur einen Lese- und Auflistungszugriff zulässt. Geben Sie Folgendes an, um die Daten im Standardspeicher des Clusters abzulegen, der Schreibzugriff zulässt:
   
        hdfs dfs -put testfile.txt wasbs:///testupload.txt
   
    Dieses Mal sollte der Vorgang erfolgreich abgeschlossen werden.

## <a name="troubleshooting"></a>Problembehandlung
### <a name="a-task-was-canceled"></a>Eine Aufgabe wurde abgebrochen.
**Symptome**: Beim Erstellen eines Clusters mithilfe des PowerShell-Skripts können Sie die folgende Fehlermeldung erhalten:

    New-AzureRmHDInsightCluster : A task was canceled.
    At C:\Users\larryfr\Documents\GitHub\hdinsight-azure-storage-sas\CreateCluster\HDInsightSAS.ps1:62 char:5
    +     New-AzureRmHDInsightCluster `
    +     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (:) [New-AzureRmHDInsightCluster], CloudException
        + FullyQualifiedErrorId : Hyak.Common.CloudException,Microsoft.Azure.Commands.HDInsight.NewAzureHDInsightClusterCommand

**Ursache**: Dieser Fehler kann auftreten, wenn Sie ein Kennwort für den Administrator/HTTP-Benutzer für den Cluster oder (bei Linux-basierten Clustern) für den SSH-Benutzer verwenden.

**Lösung**: Verwenden Sie ein Kennwort ein, das die folgenden Kriterien erfüllt:

* Mindestens 10 Zeichen
* Mindestens eine Ziffer
* Mindestens ein nicht alphanumerisches Zeichen
* Mindestens ein Groß- oder Kleinbuchstaben

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie erfahren haben, wie Sie Ihrem HDInsight-Cluster Speicher mit eingeschränktem Zugriff hinzufügen, können Sie sich mit anderen Möglichkeiten des Arbeitens mit Daten in Ihrem Cluster vertraut machen:

* [Verwenden von Hive mit HDInsight](hdinsight-use-hive.md)
* [Verwenden von Pig mit HDInsight](hdinsight-use-pig.md)
* [Verwenden von MapReduce mit HDInsight](hdinsight-use-mapreduce.md)

[powershell]: /powershell/azureps-cmdlets-docs



<!--HONumber=Feb17_HO3-->


