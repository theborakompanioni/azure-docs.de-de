---
title: Erste Schritte mit Azure Data Lake Store unter Verwendung der Azure-Befehlszeilenschnittstelle 2.0 | Microsoft-Dokumentation
description: "Erstellen Sie mithilfe der plattformübergreifenden Befehlszeile 2.0 ein Data Lake Store-Konto, und führen Sie einfache Vorgänge aus."
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 4ffa0f4a-1cca-46ac-803d-1fc8538c685b
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/04/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 988e7fe2ae9f837b661b0c11cf30a90644085e16
ms.openlocfilehash: c9d5fdc2ff27454b2492751034b43658ee9d46c5
ms.lasthandoff: 04/06/2017


---
# <a name="get-started-with-azure-data-lake-store-using-azure-cli-20-preview"></a>Erste Schritte mit Azure Data Lake Store unter Verwendung von Azure CLI 2.0 (Vorschau)
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST-API](data-lake-store-get-started-rest-api.md)
> * [Azure-Befehlszeilenschnittstelle](data-lake-store-get-started-cli.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Hier erfahren Sie, wie Sie mit Azure CLI 2.0 ein Azure Data Lake Store-Konto erstellen und einfache Vorgänge wie das Erstellen von Ordnern, das Hoch- und Herunterladen von Dateien oder das Löschen Ihres Kontos ausführen. Weitere Informationen zu Data Lake Store finden Sie unter [Übersicht über Data Lake Store](data-lake-store-overview.md).

Azure CLI 2.0 ist die neue Befehlszeilenumgebung von Azure und dient zum Verwalten von Azure-Ressourcen. Sie kann unter macOS, Linux und Windows verwendet werden. Weitere Informationen finden Sie in der [Übersicht über Azure CLI 2.0](https://docs.microsoft.com/cli/azure/overview). In der [Referenz für Azure Data Lake Store CLI 2.0](https://docs.microsoft.com/cli/azure/dls) finden Sie außerdem eine umfassende Liste mit Befehlen und Syntax.


## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Artikel beginnen können, benötigen Sie Folgendes:

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

* **Azure CLI 2.0:** Eine Installationsanleitung finden Sie unter [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (Installieren von Azure CLI 2.0).

## <a name="authentication"></a>Authentifizierung

In diesem Artikel wird ein einfacheres Authentifizierungskonzept mit Data Lake Store verwendet, bei dem Sie sich als Endbenutzer anmelden. Die Zugriffsebene für das Data Lake Store-Konto und das Dateisystem hängt dann von der Zugriffsebene des angemeldeten Benutzers ab. Für die Authentifizierung mit Data Lake Store stehen mit **Endbenutzerauthentifizierung** und **Dienst-zu-Dienst-Authentifizierung** aber auch noch andere Konzepte zur Verfügung. Anweisungen und weitere Informationen zur Authentifizierung finden Sie unter [Authenticate with Data Lake Store using Azure Active Directory](data-lake-store-authenticate-using-active-directory.md)(Authentifizieren bei Data Lake Store mithilfe von Azure Active Directory).

## <a name="enable-data-lake-store-preview-in-azure-cli-20"></a>Aktivieren von Data Lake Store (Vorschau) in Azure CLI 2.0

Data Lake Store CLI 2.0 befindet sich derzeit in der Vorschauphase und wird bei der Installation von Azure CLI 2.0 standardmäßig nicht aktiviert. Führen Sie den folgenden Befehl aus, um Data Lake Store CLI 2.0 zu aktivieren:

```azurecli
az component update --add dls
```


## <a name="log-in-to-your-azure-subscription"></a>Anmelden bei Ihrem Azure-Abonnement

1. Melden Sie sich bei Ihrem Azure-Abonnement an.

    ```azurecli
    az login
    ```

    Sie erhalten einen Code, den Sie im nächsten Schritt benötigen. Öffnen Sie „https://aka.ms/devicelogin“ in einem Webbrowser, und geben Sie zur Authentifizierung den Code ein. Sie werden aufgefordert, sich mit Ihren Anmeldeinformationen anzumelden.

2. Nach der Anmeldung werden in dem Fenster alle mit Ihrem Konto verknüpften Azure-Abonnements aufgelistet. Verwenden Sie den folgenden Befehl, um ein bestimmtes Abonnement zu verwenden:
   
    ```azurecli
    az account set --subscription <subscription id> 
    ```

## <a name="create-an-azure-data-lake-store-account"></a>Erstellen eines Azure Data Lake-Speicherkontos

1. Erstellen Sie eine neue Ressourcengruppe. Geben Sie für den folgenden Befehl die Parameterwerte ein, die Sie verwenden möchten. Wenn der Name des Standorts Leerzeichen enthält, setzen Sie ihn in doppelte Anführungszeichen. Beispiel: „USA (Ost) 2“. 
   
    ```azurecli
    az group create --location "East US 2" --name myresourcegroup
    ```

2. Erstellen Sie das Data Lake-Speicherkonto.
   
    ```azurecli
    az dls account create --account mydatalakestore --resource-group myresourcegroup
    ```

## <a name="create-folders-in-a-data-lake-store-account"></a>Erstellen von Ordnern in einem Data Lake-Speicherkonto

Sie können in Ihrem Azure Data Lake-Speicherkonto Ordner zum Verwalten und Speichern von Daten erstellen. Verwenden Sie den folgenden Befehl, um im Stammverzeichnis von Data Lake Store den Ordner **mynewfolder** zu erstellen.

```azurecli
az dls fs create --account mydatalakestore --path /mynewfolder --folder
```

> [!NOTE]
> Der Parameter `--folder` stellt sicher, dass der Befehl einen Ordner erstellt. Ohne diesen Parameter erstellt der Befehl eine leere Datei namens „mynewfolder“ im Stammverzeichnis des Data Lake Store-Kontos.
> 
>

## <a name="upload-data-to-a-data-lake-store-account"></a>Hochladen von Daten in ein Data Lake Store-Konto

Sie können Daten direkt auf die Stammebene von Data Lake Store oder in einen unter dem Konto erstellten Ordner hochladen. Die folgenden Codeausschnitte veranschaulichen das Hochladen von Beispieldaten in den im vorigen Abschnitt erstellten Ordner (**mynewfolder**).

Wenn Sie Beispieldaten zum Hochladen verwenden möchten, können Sie den Ordner **Ambulance Data** aus dem [Azure Data Lake-Git-Repository](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)herunterladen. Laden Sie die Datei herunter, und speichern Sie sie in ein lokales Verzeichnis auf dem Computer, z. B. „C:\sampledata\.“.

```azurecli
az dls fs upload --account mydatalakestore --source-path "C:\SampleData\AmbulanceData\vehicle1_09142014.csv" --destination-path "/mynewfolder/vehicle1_09142014.csv"
```

> [!NOTE]
> Als Ziel müssen Sie den vollständigen Pfad einschließlich des Dateinamens angeben.
> 
>


## <a name="list-files-in-a-data-lake-store-account"></a>Auflisten der Dateien in einem Data Lake Store-Konto

Verwenden Sie den folgenden Befehl, um die Dateien in einem Data Lake-Speicherkonto aufzulisten.

```azurecli
az dls fs list --account mydatalakestore --path /mynewfolder
```

Die Ausgabe sollte in etwa wie folgt aussehen:

    [
        {
            "accessTime": 1491323529542,
            "aclBit": false,
            "blockSize": 268435456,
            "group": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
            "length": 1589881,
            "modificationTime": 1491323531638,
            "msExpirationTime": 0,
            "name": "mynewfolder/vehicle1_09142014.csv",
            "owner": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
            "pathSuffix": "vehicle1_09142014.csv",
            "permission": "770",
            "replication": 1,
            "type": "FILE"
        }
    ]

## <a name="rename-download-and-delete-data-from-a-data-lake-store-account"></a>Umbenennen, Herunterladen und Löschen von Daten in einem Data Lake Store-Konto 

* Verwenden Sie zum **Umbenennen einer Datei** den folgenden Befehl:
  
    ```azurecli
    az dls fs move --account mydatalakestore --source-path /mynewfolder/vehicle1_09142014.csv --destination-path /mynewfolder/vehicle1_09142014_copy.csv
    ```

* **Verwenden Sie zum Herunterladen einer Datei** den folgenden Befehl: Stellen Sie sicher, dass der angegebene Zielpfad bereits vorhanden ist.
  
    ```azurecli        
    az dls fs download --account mydatalakestore --source-path /mynewfolder/vehicle1_09142014_copy.csv --destination-path "C:\mysampledata\vehicle1_09142014_copy.csv"
    ```

    > [!NOTE]
    > Falls der Zielordner nicht vorhanden ist, wird er erstellt.
    > 
    >

* Verwenden Sie zum **Löschen einer Datei** den folgenden Befehl:
  
    ```azurecli
    az dls fs delete --account mydatalakestore --path /mynewfolder/vehicle1_09142014_copy.csv
    ```

    Wenn Sie den Ordner **mynewfolder** und die Datei **vehicle1_09142014_copy.csv** mit einem einzelnen Befehl löschen möchten, verwenden Sie den Parameter „--recurse“.

    ```azurecli
    az dls fs delete --account mydatalakestore --path /mynewfolder --recurse
    ```

## <a name="work-with-permissions-and-acls-for-a-data-lake-store-account"></a>Verwenden von Berechtigungen und ACLs für ein Data Lake Store-Konto

In diesem Abschnitt erfahren Sie, wie Sie ACLs und Berechtigungen mithilfe von Azure CLI 2.0 verwalten. Ausführliche Informationen zur Implementierung von ACLs in Azure Data Lake Store finden Sie unter [Zugriffssteuerung in Azure Data Lake Store](data-lake-store-access-control.md).

* Befehl zum **Aktualisieren des Besitzers einer Datei bzw. eines Ordners**:

    ```azurecli
    az dls fs access set-owner --account mydatalakestore --path /mynewfolder/vehicle1_09142014.csv --group 80a3ed5f-959e-4696-ba3c-d3c8b2db6766 --owner 6361e05d-c381-4275-a932-5535806bb323
    ```

* Befehl zum **Aktualisieren der Berechtigungen für eine Datei bzw. einen Ordner**:

    ```azurecli
    az dls fs access set-permission --account mydatalakestore --path /mynewfolder/vehicle1_09142014.csv --permission 777
    ```
    
* Befehl zum **Abrufen der ACLs für einen bestimmten Pfad**:

    ```azurecli
    az dls fs access show --account mydatalakestore --path /mynewfolder/vehicle1_09142014.csv
    ```

    Die Ausgabe sollte in etwa wie folgt aussehen:

        {
            "entries": [
            "user::rwx",
            "group::rwx",
            "other::---"
          ],
          "group": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
          "owner": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
          "permission": "770",
          "stickyBit": false
        }

* Befehl zum **Festlegen eines Eintrags für eine ACL**:

    ```azurecli
    az dls fs access set-entry --account mydatalakestore --path /mynewfolder --acl-spec user:6360e05d-c381-4275-a932-5535806bb323:-w-
    ```

* Befehl zum **Entfernen eines Eintrags für eine ACL**:

    ```azurecli
    az dls fs access remove-entry --account mydatalakestore --path /mynewfolder --acl-spec user:6360e05d-c381-4275-a932-5535806bb323
    ```

* Befehl zum **Entfernen einer vollständigen Standard-ACL**:

    ```azurecli
    az dls fs access remove-all --account mydatalakestore --path /mynewfolder --default-acl
    ```

* Befehl zum **Entfernen einer vollständigen nicht standardmäßigen ACL**:

    ```azurecli
    az dls fs access remove-all --account mydatalakestore --path /mynewfolder
    ```
    
## <a name="delete-a-data-lake-store-account"></a>Löschen eines Data Lake-Speicherkontos
Verwenden Sie zum Löschen eines Data Lake-Speicherkontos den folgenden Befehl.

```azurecli
az dls account delete --account mydatalakestore
```

Geben Sie nach entsprechender Aufforderung **Y** ein, um das Konto zu löschen.

## <a name="next-steps"></a>Nächste Schritte

* [Referenz für Azure Data Lake Store CLI 2.0](https://docs.microsoft.com/cli/azure/dls)
* [Sichern von Daten in Data Lake Store](data-lake-store-secure-data.md)
* [Verwenden von Azure Data Lake Analytics mit Data Lake-Speicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Verwenden von Azure HDInsight mit Data Lake-Speicher](data-lake-store-hdinsight-hadoop-use-portal.md)

[azure-command-line-tools]: ../xplat-cli-install.md

