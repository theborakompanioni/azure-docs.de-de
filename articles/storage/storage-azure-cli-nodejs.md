---
title: Verwenden der Azure-Befehlszeilenschnittstelle 1.0 mit Azure Storage | Microsoft Docs
description: "Erfahren Sie, wie Sie die Azure-Befehlszeilenschnittstelle 1.0 (Azure CLI) mit Azure Storage verwenden, um Speicherkonten zu erstellen und zu verwalten sowie mit Azure-Blobs und -Dateien zu arbeiten. Die Azure-Befehlszeilenschnittstelle ist ein plattformübergreifendes Tool."
services: storage
documentationcenter: na
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: b502232a-e8f6-4d6c-befd-3476592e0e35
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2017
ms.author: seguler
translationtype: Human Translation
ms.sourcegitcommit: 36fa9cd757b27347c08f80657bab8a06789a3c2f
ms.openlocfilehash: 289bf7ab1ba91bd9edc6288af93e06170d8f2def
ms.lasthandoff: 02/27/2017


---
# <a name="using-the-azure-cli-10-with-azure-storage"></a>Verwenden der Azure-Befehlszeilenschnittstelle 1.0 mit Azure Storage

## <a name="overview"></a>Übersicht

Die Azure-Befehlszeilenschnittstelle stellt eine Reihe von plattformübergreifenden Open Source-Befehlen für die Arbeit mit der Azure-Plattform bereit. Sie bietet im Wesentlichen die gleiche Funktionalität wie das [Azure-Portal](https://portal.azure.com) sowie umfangreiche Datenzugriffsfunktionen.

Diese Anleitung enthält Informationen zur Verwendung der [Azure-Befehlszeilenschnittstelle (Azure CLI)](../xplat-cli-install.md) zum Ausführen einer Vielzahl von Entwicklungs- und Verwaltungsaufgaben mit Azure Storage. Sie sollten die neueste Azure-Befehlszeilenschnittstelle herunterladen und installieren bzw. ein Upgrade durchführen, bevor Sie diese Anleitung verwenden.

Diese Anleitung setzt voraus, dass Sie die grundlegenden Konzepte von Azure-Speicher verstehen. Die Anleitung bietet eine Reihe von Skripts, um die Verwendung der Azure-Befehlszeilenschnittstelle mit Azure-Speicher zu veranschaulichen. Sie müssen die Skriptvariablen auf Basis Ihrer Konfiguration aktualisieren, bevor Sie die jeweiligen Skripts ausführen.

> [!NOTE]
> Die Anleitung enthält die Befehle der Azure-Befehlszeilenschnittstelle und Skriptbeispiele für klassische Speicherkonten. Befehle der Azure-Befehlszeilenschnittstelle für Resource Manager-Speicherkonten finden Sie unter [Verwenden der Azure-Befehlszeilenschnittstelle für Mac, Linux und Windows mit der Azure-Ressourcenverwaltung](../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects) .
>
>

[!INCLUDE [storage-cli-versions](../../includes/storage-cli-versions.md)]

## <a name="get-started-with-azure-storage-and-the-azure-cli-in-5-minutes"></a>Erste Schritte mit Azure-Speicher und der Azure-Befehlszeilenschnittstelle in 5 Minuten
Diese Anleitung verwendet für Beispiele Ubuntu, andere Betriebssystemplattformen funktionieren jedoch auf ähnliche Weise.

**Neu in Azure:** Holen Sie Sich ein Microsoft Azure-Abonnement und ein Microsoft-Konto für dieses Abonnement. Informationen zu Azure-Kaufoptionen finden Sie unter [Kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/), [Kaufoptionen](https://azure.microsoft.com/pricing/purchase-options/) und [Angebote für Mitglieder](https://azure.microsoft.com/pricing/member-offers/) (für Mitglieder von MSDN, Microsoft Partner Network, BizSpark und anderen Microsoft-Programmen).

Unter [Zuweisen von Administratorrollen in Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) finden Sie weitere Informationen zu Azure-Abonnements.

**Nach der Erstellung eines Microsoft Azure-Abonnements und eines Kontos:**

1. Laden Sie die Azure-Befehlszeilenschnittstelle herunterladen und installieren Sie sie gemäß den Anweisungen unter [Installieren der Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md).
2. Sobald die Befehlszeilenschnittstelle installiert ist, können Sie den Befehl "azure" in Ihrer Befehlszeilenschnittstelle (Bash, Terminal, Eingabeaufforderung) verwenden, um auf die Befehle der Azure-Befehlszeilenschnittstelle zuzugreifen. Wenn Sie den Befehl _azure_ eingeben, sollte die folgende Ausgabe angezeigt werden.

    ![Azure-Befehlsausgabe:][Image1]
3. Geben Sie in der Befehlszeilenschnittstelle `azure storage` ein, um alle Azure Storage-Befehle aufzulisten und einen ersten Eindruck vom Funktionsumfang der Azure-Befehlszeilenschnittstelle zu erhalten. Sie können Befehlsnamen mit einem **-h**-Parameter (z. B. `azure storage share create -h`) eingeben , um Details der Befehlssyntax anzuzeigen.
4. In einem einfachen Skript werden nun grundlegende Befehle der Azure-Befehlszeilenschnittstelle für den Zugriff auf Azure Storage dargestellt. In dem Skript werden Sie als erstes dazu aufgefordert, zwei Variablen für Ihr Speicherkonto und Ihren Speicherschlüssel festzulegen. Als Nächstes erstellt das Skript in dem neuen Speicherkonto einen neuen Container und lädt eine vorhandene Image-Datei (Blob) in diesen Container. Nachdem das Skript alle Blobs in diesem Container aufgelistet hat, lädt es die Image-Datei in ein Zielverzeichnis auf dem lokalen Computer herunter.

    ```azurecli
    #!/bin/bash
    # A simple Azure storage example

    export AZURE_STORAGE_ACCOUNT=<storage_account_name>
    export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

    export container_name=<container_name>
    export blob_name=<blob_name>
    export image_to_upload=<image_to_upload>
    export destination_folder=<destination_folder>

    echo "Creating the container..."
    azure storage container create $container_name

    echo "Uploading the image..."
    azure storage blob upload $image_to_upload $container_name $blob_name

    echo "Listing the blobs..."
    azure storage blob list $container_name

    echo "Downloading the image..."
    azure storage blob download $container_name $blob_name $destination_folder

    echo "Done"
    ```

5. Öffnen Sie auf Ihrem lokalen Computer Ihren bevorzugten Texteditor (z. B. Vim). Geben Sie das obige Skript in den Texteditor ein.
6. Nun müssen Sie die Skriptvariablen auf Basis der Konfigurationseinstellungen aktualisieren.

   * **<storage_account_name>**: Verwenden Sie den im Skript angegebenen Namen, oder geben Sie einen neuen Namen für Ihr Speicherkonto ein. **Wichtig:** Der Name des Speicherkontos muss in Azure eindeutig sein. Er darf außerdem nur aus Kleinbuchstaben bestehen!
   * **<storage_account_key>**: Der Zugriffsschlüssel für Ihr Speicherkonto.
   * **<container_name>**: Verwenden Sie den im Skript angegebenen Namen, oder geben Sie einen neuen Namen für Ihren Container ein.
   * **<image_to_upload>**: Geben Sie einen Pfad zu einem Bild auf dem lokalen Computer ein. Beispiel: „~/images/HelloWorld.png“.
   * **<destination_folder>**: Geben Sie den Pfad zu einem lokalen Verzeichnis zum Speichern von Dateien ein, die aus Azure Storage heruntergeladen werden. Beispiel: „~/downloadImages“.
7. Nachdem Sie die erforderlichen Variablen in Vim aktualisiert haben, drücken Sie die Tastenkombinationen `ESC`, `:`, `wq!`, um das Skript zu speichern.
8. Zum Ausführen des Skripts geben Sie einfach den Namen der Skripdatei in der Bash-Konsole ein. Nachdem das Skript ausgeführt wird, sollten Sie über einen lokalen Zielordner verfügen, der die heruntergeladene Image-Datei enthält. Der folgende Screenshot zeigt eine Beispielausgabe:

Nachdem das Skript ausgeführt wird, sollten Sie über einen lokalen Ordner verfügen, der die heruntergeladene Datei enthält.

## <a name="manage-storage-accounts-with-the-azure-cli"></a>Verwalten von Speicherkonten mit der Azure-Befehlszeilenschnittstelle
### <a name="connect-to-your-azure-subscription"></a>Verbinden mit Ihrem Azure-Abonnement
Obwohl die meisten Speicherbefehle ohne Azure-Abonnement funktionieren, empfehlen wir Ihnen, eine Verbindung zu Ihrem Abonnement über die Azure-Befehlszeilenschnittstelle herzustellen. Zum Konfigurieren der Azure-Befehlszeilenschnittstelle für die Verwendung mit Ihrem Abonnement führen Sie die Schritte unter [Herstellen einer Verbindung mit einem Azure-Abonnement über die Azure-Befehlszeilenschnittstelle](../xplat-cli-connect.md)aus.

### <a name="create-a-new-storage-account"></a>Erstellen eines neuen Speicherkontos
Für die Verwendung von Azure-Speicher benötigen Sie ein Speicherkonto. Nachdem Sie Ihren Computer für die Verbindung mit Ihrem Abonnement konfiguriert haben, können Sie ein neues Azure-Speicherkonto erstellen.

```azurecli
azure storage account create <account_name>
```

Der Name Ihres Speicherkontos muss zwischen 3 und 24 Zeichen lang sein und darf nur Zahlen und Kleinbuchstaben enthalten.

### <a name="set-a-default-azure-storage-account-in-environment-variables"></a>Festlegen eines Azure-Standardspeicherkontos in Umgebungsvariablen
Sie können mehrere Speicherkonten in Ihrem Abonnement verwenden. Sie können ein Speicherkonto auswählen und es in den Umgebungsvariablen als Standardspeicherkonto für alle Speicherbefehle in einer Sitzung festlegen. Dadurch können Sie die Speicherbefehle der Azure-Befehlszeilenschnittstelle ohne explizite Angabe des Speicherkontos und des Speicherschlüssels ausführen.

```azurecli
export AZURE_STORAGE_ACCOUNT=<account_name>
export AZURE_STORAGE_ACCESS_KEY=<key>
```

Eine weitere Möglichkeit zum Festlegen eines Standardspeicherkontos ist die Verwendung einer Verbindungszeichenfolge. Rufen Sie zuerst die Verbindungszeichenfolge per Befehl ab:

```azurecli
azure storage account connectionstring show <account_name>
```

Kopieren Sie dann die Ausgabeverbindungszeichenfolge und legen Sie sie auf die Umgebungsvariable fest:

```azurecli
export AZURE_STORAGE_CONNECTION_STRING=<connection_string>
```

## <a name="create-and-manage-blobs"></a>Erstellen und Verwalten von Blobs
Der Azure-Blob-Speicher ist ein Dienst zur Speicherung großer Mengen unstrukturierter Daten, beispielsweise Text- oder Binärdaten, auf die von überall auf der Welt über HTTP oder HTTPS zugegriffen werden kann. Dieser Abschnitt setzt voraus, dass Sie mit den Konzepten des Azure-Blob-Speichers bereits vertraut sind. Ausführliche Informationen finden Sie unter [Erste Schritte mit Azure Blob Storage mit .NET](storage-dotnet-how-to-use-blobs.md) und [Konzepte des Blob-Diensts](http://msdn.microsoft.com/library/azure/dd179376.aspx).

### <a name="create-a-container"></a>Erstellen eines Containers
Jeder Blob im Azure-Speicher muss sich in einem Container befinden. Mit dem Befehl `azure storage container create` können Sie einen privaten Container erstellen:

```azurecli
azure storage container create mycontainer
```

> [!NOTE]
> Es gibt drei Stufen des anonymen Lesezugriffs: **Off**, **Blob** und **Container**. Legen Sie für den Parameter "Permission" den Wert **Off**fest, um den anonymen Zugriff auf Blobs zu verhindern. Der neue Container ist standardmäßig privat, und der Zugriff ist ausschließlich dem Kontobesitzer gestattet. Um den anonymen öffentlichen Lesezugriff auf Blob-Ressourcen, jedoch nicht auf Containermetadaten oder die Liste der im Container enthaltenen Blobs zuzulassen, legen Sie für den Parameter "Permission" den Wert **Blob**fest. Um den vollständigen öffentlichen Lesezugriff auf Blob-Ressourcen, Containermetadaten und die Liste der im Container enthaltenen Blobs zuzulassen, legen Sie für den Parameter "Permission" den Wert **Container**fest. Weitere Informationen finden Sie unter [Verwalten des anonymen Lesezugriffs auf Container und Blobs](storage-manage-access-to-resources.md).
>
>

### <a name="upload-a-blob-into-a-container"></a>Hochladen eines Blobs in einen Container
Azure Blob-Speicher unterstützt Blockblobs und Seitenblobs. Weitere Informationen finden Sie unter [Grundlagen zu Blockblobs, Anfügeblobs und Seitenblobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).

Um Blobs in einen Container hochzuladen, können Sie den Befehl `azure storage blob upload`verwenden. Dieser Befehl lädt standardmäßig die lokalen Dateien in einen Blockblob hoch. Um den Blob-Typ anzugeben, können Sie den Parameter `--blobtype` verwenden.

```azurecli
azure storage blob upload '~/images/HelloWorld.png' mycontainer myBlockBlob
```

### <a name="download-blobs-from-a-container"></a>Herunterladen von Blobs aus einem Container
Im folgenden Beispiel wird veranschaulicht, wie Blobs aus einem Container heruntergeladen werden.

```azurecli
azure storage blob download mycontainer myBlockBlob '~/downloadImages/downloaded.png'
```

### <a name="copy-blobs"></a>Kopieren von Blobs
Sie können Blobs innerhalb oder zwischen Speicherkonten und Regionen asynchron kopieren.

Im folgenden Beispiel wird veranschaulicht, wie Sie Blobs von einem Speicherkonto in ein anderes kopieren. In diesem Beispiel erstellen wir einen Container, in dem Blobs öffentlich sind und anonym darauf zugegriffen werden kann.

```azurecli
azure storage container create mycontainer2 -a <accountName2> -k <accountKey2> -p Blob

azure storage blob upload '~/Images/HelloWorld.png' mycontainer2 myBlockBlob2 -a <accountName2> -k <accountKey2>

azure storage blob copy start 'https://<accountname2>.blob.core.windows.net/mycontainer2/myBlockBlob2' mycontainer
```

In diesem Beispiel wird ein asynchroner Kopiervorgang ausgeführt. Sie können den Status jedes Kopiervorgangs überwachen, indem Sie den Vorgang `azure storage blob copy show` ausführen.

Beachten Sie, dass die für den Kopiervorgang bereitgestellte Quell-URL entweder öffentlich zugänglich sein muss oder ein SAS-Token (Token für die freigegebene Zugriffssignatur) enthalten muss.

### <a name="delete-a-blob"></a>Löschen eines BLOBs
Verwenden Sie den nachfolgenden Befehl, um ein Blob zu löschen.

```azurecli
azure storage blob delete mycontainer myBlockBlob2
```

## <a name="create-and-manage-file-shares"></a>Erstellen und Verwalten von Dateifreigaben
Der Azure-Dateispeicher bietet einen gemeinsam genutzten Speicher für Anwendungen und verwendet dabei das SMB-Protokoll. Microsoft Azure Virtual Machines und Clouddienste können wie lokale Anwendungen Dateidaten mithilfe bereitgestellter Freigaben teilen. Dateifreigaben und Dateidaten können über die Azure-Befehlszeilenschnittstelle verwaltet werden. Weitere Informationen zum Azure-Dateispeicher finden Sie unter [Erste Schritte mit Azure File Storage unter Windows](storage-dotnet-how-to-use-files.md) und [Verwenden des Azure-Dateispeichers unter Linux](storage-how-to-use-files-linux.md).

### <a name="create-a-file-share"></a>Erstellen einer Dateifreigabe
Eine Azure-Dateifreigabe ist eine SMB-Dateifreigabe in Azure. Alle Verzeichnisse und Dateien müssen in einer Dateifreigabe erstellt werden. Ein Konto kann eine unbegrenzte Anzahl von Freigaben enthalten, und eine Freigabe kann eine unbegrenzte Anzahl von Dateien speichern, bis die Kapazitätsgrenze des Speicherkontos erreicht ist. Im folgenden Beispiel wird eine Dateifreigabe namens **myshare**erstellt.

```azurecli
azure storage share create myshare
```

### <a name="create-a-directory"></a>Erstellen eines Verzeichnisses
Ein Verzeichnis enthält eine optionale hierarchische Struktur für eine Azure-Dateifreigabe. Im folgenden Beispiel wird ein Verzeichnis namens **myDir** in der Dateifreigabe erstellt.

```azurecli
azure storage directory create myshare myDir
```

Beachten Sie, dass dieser Verzeichnispfad mehrere Ebenen enthalten kann, *z.B.*, **a/b**. Sie müssen allerdings sicherstellen, dass alle übergeordneten Verzeichnisse vorhanden sind. Zum Beispiel müssen Sie für Pfad **a/b**, zuerst Verzeichnis **a** und anschließend Verzeichnis **b** erstellen.

### <a name="upload-a-local-file-to-directory"></a>Hochladen einer lokalen Datei in das Verzeichnis
Im folgenden Beispiel wird eine Datei aus **~/temp/samplefile.txt** in das Verzeichnis **myDir** hochgeladen. Bearbeiten Sie den Dateipfad so, dass er auf eine gültige Datei auf Ihrem lokalen Computer verweist:

```azurecli
azure storage file upload '~/temp/samplefile.txt' myshare myDir
```

Beachten Sie, dass eine Datei in der Freigabe bis zu 1 TB groß sein kann.

### <a name="list-the-files-in-the-share-root-or-directory"></a>Auflisten der Dateien im Freigabestamm oder Verzeichnis
Sie können Sie die Dateien und Unterverzeichnisse in einem Freigabestamm oder einem Verzeichnis mit dem folgenden Befehl auflisten:

```azurecli
azure storage file list myshare myDir
```

Beachten Sie, dass der Name des Verzeichnisses für den Auflistungsvorgang optional ist. Wenn kein Namen angegeben wird, listet der Befehl den Inhalt des Stammverzeichnisses der Freigabe auf.

### <a name="copy-files"></a>Kopieren von Dateien
Ab Version 0.9.8 der Azure-CLI können Sie eine Datei in eine andere Datei, eine Datei in ein Blob oder ein Blob in eine Datei kopieren. Im Folgenden wird demonstriert, wie diese Kopiervorgänge mithilfe von CLI-Befehlen ausgeführt werden können. So kopieren Sie eine Datei in das neue Verzeichnis:

```azurecli
azure storage file copy start --source-share srcshare --source-path srcdir/hello.txt --dest-share destshare
    --dest-path destdir/hellocopy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString
```

So kopieren Sie ein Blob in ein Dateiverzeichnis:

```azurecli
azure storage file copy start --source-container srcctn --source-blob hello2.txt --dest-share hello
    --dest-path hellodir/hello2copy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString
```

## <a name="next-steps"></a>Nächste Schritte

Sie finden die Befehlsreferenz zur Azure-Befehlszeilenschnittstelle 1.0 für die Arbeit mit Speicherressourcen hier:

* [Befehle der Azure-Befehlszeilenschnittstelle im Resource Manager-Modus](../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects)
* [Befehle der Azure-Befehlszeilenschnittstelle im Modus „Azure-Dienstverwaltung“](../xplat-cli-install.md)

Sie können auch die [Azure CLI 2.0](storage-azure-cli.md) testen. Dies ist eine in Python geschriebene Befehlszeilenschnittstelle (Command Line Interface, CLI) der nächsten Generation zur Verwendung mit dem Resource Manager-Bereitstellungsmodell.

[Image1]: ./media/storage-azure-cli/azure_command.png

