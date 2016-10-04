<properties
	pageTitle="Tutorial – Erste Schritte mit dem Azure Batch-Python-Client | Microsoft Azure"
	description="Erfahren Sie mehr über die grundlegenden Konzepte von Azure Batch, und lesen Sie, wie Sie den Batch-Dienst in einem einfachen Szenario für die Entwicklung einsetzen."
	services="batch"
	documentationCenter="python"
	authors="mmacy"
	manager="timlt"
	editor=""/>

<tags
	ms.service="batch"
	ms.devlang="python"
	ms.topic="hero-article"
	ms.tgt_pltfrm="na"
	ms.workload="big-compute"
	ms.date="09/27/2016"
	ms.author="marsma"/>

# Erste Schritte mit dem Azure Batch-Python-Client

> [AZURE.SELECTOR]
- [.NET](batch-dotnet-get-started.md)
- [Python](batch-python-tutorial.md)

Lernen Sie die Grundlagen von [Azure Batch][azure_batch] und [Batch-Python][py_azure_sdk]-Clients mithilfe einer kleinen in Python erstellten Batch-Anwendung kennen. Wir sehen uns an, wie zwei Beispielskripts den Batch-Dienst nutzen, um eine parallele Workload auf virtuellen Linux-Computern in der Cloud zu verarbeiten, und wie sie mit [Azure Storage](./../storage/storage-introduction.md) in Bezug auf das Bereitstellen und Abrufen von Dateien interagieren. Sie lernen häufig verwendete Batch-Anwendungsworkflows kennen und erhalten ein besseres Verständnis in Bezug auf die Hauptkomponenten von Batch, wie etwa Aufträge, Aufgaben, Pools und Computeknoten.

![Batch-Lösungsworkflow (einfach)][11]<br/>

## Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie über Python-Kenntnisse verfügen und mit Linux vertraut sind. Außerdem wird vorausgesetzt, dass Sie die Anforderungen an die Kontoerstellung erfüllen können, die unten für Azure und die Batch- und Storage-Dienste angegeben sind.

### Konten

- **Azure-Konto**: Wenn Sie nicht bereits über ein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto erstellen][azure_free_account].
- **Batch-Konto**: Wenn Sie über ein Azure-Abonnement verfügen, können Sie ein [Azure Batch-Konto erstellen](batch-account-create-portal.md).
- **Storage-Konto:** Weitere Informationen finden Sie unter [Informationen zu Azure-Speicherkonten](../storage/storage-create-storage-account.md) im Abschnitt [Erstellen eines Speicherkontos](../storage/storage-create-storage-account.md#create-a-storage-account).

### Codebeispiel

Das [Codebeispiel][github_article_samples] des Python-Tutorials ist eines der vielen Batch-Codebeispiele im Repository [azure-batch-samples][github_samples] auf GitHub. Sie können alle Beispiele herunterladen, indem Sie auf der Seite des Repositorys auf **Clone or download > Download ZIP** klicken. Oder klicken Sie zum direkten Herunterladen auf den Link [azure-batch-samples-master.zip][github_samples_zip]. Wenn Sie den Inhalt der ZIP-Datei extrahiert haben, befinden sich die beiden Skripts für dieses Tutorial im Verzeichnis `article_samples`:

`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_client.py`<br/> `/azure-batch-samples/Python/Batch/article_samples/python_tutorial_task.py`

### Python-Umgebung

Damit Sie das Beispielskript *python\_tutorial\_client.py* auf der lokalen Arbeitsstation verwenden können, benötigen Sie einen mit Version **2.7** oder **3.3+** kompatiblen **Python-Interpreter**. Das Skript wurde auf Linux sowie auf Windows getestet.

### Kryptografieabhängigkeiten

Sie müssen die Abhängigkeiten für die [Kryptografie][crypto]bibliothek installieren, die für die `azure-batch`- und `azure-storage`-Python-Pakete benötigt werden. Führen Sie eines der folgenden Verfahren durch, das für Ihre Plattform geeignet ist, oder informieren Sie sich über die Details zur [Kryptografieinstallation][crypto_install]\:

* Ubuntu

    `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython-dev python-dev`

* CentOS

    `yum update && yum install -y gcc openssl-dev libffi-devel python-devel`

* SLES/OpenSUSE

    `zypper ref && zypper -n in libopenssl-dev libffi48-devel python-devel`

* Windows

    `pip install cryptography`

>[AZURE.NOTE] Verwenden Sie beim Installieren für Python 3.3+ unter Linux die python3-Entsprechungen für die Python-Abhängigkeiten. Beispielsweise für Ubuntu: `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython3-dev python3-dev`

### Azure-Pakete

Als Nächstes installieren Sie die **Azure Batch**- und **Azure Storage**-Python-Pakete. Hierfür können Sie **pip** und die Datei *requirements.txt* verwenden, die Sie hier finden:

`/azure-batch-samples/Python/Batch/requirements.txt`

Führen Sie den folgenden **pip**-Befehl aus, um die Batch- und Storage-Pakete zu installieren:

`pip install -r requirements.txt`

Sie können die [azure-batch][pypi_batch]- und [azure-storage][pypi_storage]-Python-Pakete auch manuell installieren:

`pip install azure-batch`<br/> `pip install azure-storage`

> [AZURE.TIP] Unter Umständen müssen Sie Ihren Befehlen das Präfix `sudo` voranstellen, wenn Sie ein nicht privilegiertes Konto verwenden. Beispiel: `sudo pip install -r requirements.txt`. Weitere Informationen zur Installation von Python-Paketen finden Sie unter [Installing Packages][pypi_install] (Installieren von Paketen) auf „readthedocs.io“.

## Python-Tutorial für Batch – Codebeispiel

Das Codebeispiel für das Python-Tutorial für Batch besteht aus zwei Python-Skripts und einigen Datendateien.

- **python\_tutorial\_client.py**: Interagiert mit den Diensten Batch und Storage, um auf Computeknoten (virtuellen Computern) eine parallele Workload auszuführen. Das Skript *python\_tutorial\_client.py* wird auf Ihrer lokalen Arbeitsstation ausgeführt.

- **python\_tutorial\_task.py**: Das Skript, das auf Computeknoten in Azure ausgeführt wird, um die eigentliche Arbeit zu erledigen. Im Beispiel analysiert *python\_tutorial\_task.py* den Text in einer Datei, die aus Azure Storage (der Eingabedatei) heruntergeladen wird. Anschließend wird eine Textdatei produziert (Ausgabedatei), die eine Liste mit den drei wichtigsten Wörtern aus der Eingabedatei enthält. Nach dem Erstellen der Ausgabedatei lädt *python\_tutorial\_task.py* die Datei nach Azure Storage hoch. Dadurch steht es dem auf Ihrer Arbeitsstation ausgeführten Clientskript zum Download zur Verfügung. Das Skript *python\_tutorial\_task.py* wird im Batch-Dienst parallel auf mehreren Computeknoten ausgeführt.

- **./data/taskdata*.txt**: Diese drei Textdateien stellen die Eingabe für die auf den Computeknoten ausgeführten Aufgaben bereit.

Das folgende Diagramm veranschaulicht die primären Vorgänge, die von den Client- und Aufgabenskripts ausgeführt werden. Dieser grundlegende Workflow ist typisch für viele Computelösungen, die mit Batch erstellt werden. Er veranschaulicht zwar nicht alle im Batch-Dienst verfügbaren Features, aber fast jedes Batch-Szenario enthält Teile dieses Workflows.

![Batch-Beispielworkflow][8]<br/>

[**Schritt 1:**](#step-1-create-storage-containers) Erstellen von **Containern** in Azure Blob Storage<br/> [**Schritt 2:**](#step-2-upload-task-script-and-data-files) Hochladen von Aufgabenskripts und Eingabedateien in Container<br/> [**Schritt 3:**](#step-3-create-batch-pool) Erstellen eines Batch-**Pools**<br/> &nbsp;&nbsp;&nbsp;&nbsp;**3a.** Die Aufgabe **StartTask** des Pools lädt das Aufgabenskript (python\_tutorial\_task.py) auf Knoten herunter, wenn diese dem Pool beitreten.<br/> [**Schritt 4:**](#step-4-create-batch-job) Erstellen eines Batch-**Auftrags**<br/> [**Schritt 5:**](#step-5-add-tasks-to-job) Hinzufügen von **Aufgaben** zum Auftrag<br/> &nbsp;&nbsp;&nbsp;&nbsp;**5a.** Die Aufgaben werden für die Ausführung auf Knoten geplant.<br/> &nbsp;&nbsp;&nbsp;&nbsp;**5b.** Jede Aufgabe lädt ihre Eingabedaten aus Azure Storage und beginnt dann mit der Ausführung.<br/> [**Schritt 6:**](#step-6-monitor-tasks) Überwachen von Aufgaben<br/> &nbsp;&nbsp;&nbsp;&nbsp;**6a.** Nach Abschluss der Aufgaben werden die zugehörigen Ausgabedaten in Azure Storage hochgeladen.<br/> [**Schritt 7:**](#step-7-download-task-output) Herunterladen der Aufgabenausgabe aus Storage

Wie bereits erwähnt, werden nicht von jeder Batch-Lösung genau diese Schritte ausgeführt, und es können auch erheblich mehr Schritte enthalten sein. Im Beispiel werden jedoch die Prozesse veranschaulicht, die in einer Batch-Lösung häufig vorkommen.

## Vorbereiten von Clientskripts

Fügen Sie vor dem Ausführen des Beispiels *python\_tutorial\_client.py* Ihre Kontoanmeldeinformationen für Batch und Storage hinzu. Öffnen Sie die Datei in einem Editor und aktualisieren Sie die folgenden Zeilen mit Ihren Anmeldeinformationen (sofern Sie dies noch nicht getan haben).

```python
# Update the Batch and Storage account credential strings below with the values
# unique to your accounts. These are used when constructing connection strings
# for the Batch and Storage client objects.

# Batch account credentials
batch_account_name = "";
batch_account_key  = "";
batch_account_url  = "";

# Storage account credentials
storage_account_name = "";
storage_account_key  = "";
```

Die Kontoanmeldeinformationen für Batch und Storage finden Sie im [Azure-Portal][azure_portal] auf dem Kontoblatt des jeweiligen Diensts:

![Batch-Anmeldeinformationen im Portal][9] ![Storage-Anmeldeinformationen im Portal][10]<br/>

In den folgenden Abschnitten analysieren wir die Schritte, die von den Skripts verwendet werden, um eine Workload im Batch-Dienst zu verarbeiten. Werfen Sie regelmäßig einen Blick auf die Skripts in Ihrem Editor, während Sie den Rest dieses Artikels lesen.

Navigieren Sie zur Zeile **python\_tutorial\_client.py**, um mit Schritt 1 zu beginnen:

```python
if __name__ == '__main__':
```

## Schritt 1: Erstellen von Storage-Containern

![Container in Azure Storage erstellen][1] <br/>

Batch enthält integrierte Unterstützung für die Interaktion mit Azure Storage. Über Container in Ihrem Storage-Konto werden die Dateien angegeben, die für die in Ihrem Batch-Konto ausgeführten Aufgaben erforderlich sind. Die Container stellen auch einen Ort zum Speichern von Ausgabedaten dar, die von den Aufgaben produziert werden. Als Erstes erstellt das Skript *python\_tutorial\_client.py* drei Container in [Azure Blob Storage](../storage/storage-introduction.md#blob-storage):

- **application:** Dieser Container speichert das von den Aufgaben ausgeführte Python-Skript *python\_tutorial\_task.py*.
- **input:** Die zu verarbeitenden Datendateien werden von den Aufgaben aus dem Container *input* heruntergeladen.
- **output:** Nach Abschluss der Verarbeitung der Eingabedateien werden die Ergebnisse in den Container *output* hochgeladen.

Für die Interaktion mit einem Storage-Konto und die Containererstellung verwenden wir das Paket [azure-storage][pypi_storage], um ein [BlockBlobService][py_blockblobservice]-Objekt zu erstellen – den „Blob-Client“. Anschließend erstellen wir mithilfe des Blob-Clients im Storage-Konto drei Container.

```python
 # Create the blob client, for use in obtaining references to
 # blob storage containers and uploading files to containers.
 blob_client = azureblob.BlockBlobService(
     account_name=_STORAGE_ACCOUNT_NAME,
     account_key=_STORAGE_ACCOUNT_KEY)

 # Use the blob client to create the containers in Azure Storage if they
 # don't yet exist.
 app_container_name = 'application'
 input_container_name = 'input'
 output_container_name = 'output'
 blob_client.create_container(app_container_name, fail_on_exist=False)
 blob_client.create_container(input_container_name, fail_on_exist=False)
 blob_client.create_container(output_container_name, fail_on_exist=False)
```

Nachdem die Container erstellt wurden, kann die Anwendung die Dateien hochladen, die von den Aufgaben verwendet werden.

> [AZURE.TIP] [How to use Azure Blob storage from Python](../storage/storage-python-how-to-use-blob-storage.md) ermöglicht eine gute Übersicht über die Verwendung von Azure Storage-Containern und -Blobs. Es ist ratsam, sich diese Informationen zu Beginn der Nutzung von Batch durchzulesen.

## Schritt 2: Hochladen der Aufgabenskripts und Datendateien

![Aufgabenanwendungs- und Eingabe(daten)dateien in Container hochladen][2] <br/>

Beim Vorgang für den Dateiupload definiert *python\_tutorial\_client.py* zuerst die Sammlungen mit den Dateipfaden für **application** und **input** auf Grundlage der auf dem lokalen Computer vorhandenen Dateipfade. Diese Dateien werden dann in die Container hochgeladen, die Sie im vorherigen Schritt erstellt haben.

```python
 # Paths to the task script. This script will be executed by the tasks that
 # run on the compute nodes.
 application_file_paths = [os.path.realpath('python_tutorial_task.py')]

 # The collection of data files that are to be processed by the tasks.
 input_file_paths = [os.path.realpath('./data/taskdata1.txt'),
                     os.path.realpath('./data/taskdata2.txt'),
                     os.path.realpath('./data/taskdata3.txt')]

 # Upload the application script to Azure Storage. This is the script that
 # will process the data files, and is executed by each of the tasks on the
 # compute nodes.
 application_files = [
     upload_file_to_container(blob_client, app_container_name, file_path)
     for file_path in application_file_paths]

 # Upload the data files. This is the data that will be processed by each of
 # the tasks executed on the compute nodes in the pool.
 input_files = [
     upload_file_to_container(blob_client, input_container_name, file_path)
     for file_path in input_file_paths]
```

Mithilfe der List Comprehensions wird die Funktion `upload_file_to_container` für jede Datei in den Sammlungen aufgerufen, und zwei [ResourceFile][py_resource_file]-Sammlungen werden gefüllt. Die Funktion `upload_file_to_container` wird unten angezeigt:

```
def upload_file_to_container(block_blob_client, container_name, file_path):
    """
    Uploads a local file to an Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param str container_name: The name of the Azure Blob storage container.
    :param str file_path: The local path to the file.
    :rtype: `azure.batch.models.ResourceFile`
    :return: A ResourceFile initialized with a SAS URL appropriate for Batch
    tasks.
    """
    blob_name = os.path.basename(file_path)

    print('Uploading file {} to container [{}]...'.format(file_path,
                                                          container_name))

    block_blob_client.create_blob_from_path(container_name,
                                            blob_name,
                                            file_path)

    sas_token = block_blob_client.generate_blob_shared_access_signature(
        container_name,
        blob_name,
        permission=azureblob.BlobPermissions.READ,
        expiry=datetime.datetime.utcnow() + datetime.timedelta(hours=2))

    sas_url = block_blob_client.make_blob_url(container_name,
                                              blob_name,
                                              sas_token=sas_token)

    return batchmodels.ResourceFile(file_path=blob_name,
                                    blob_source=sas_url)
```

### ResourceFiles

Ein [ResourceFile][py_resource_file]-Objekt stellt für Aufgaben in Batch die URL zu einer Datei in Azure Storage bereit, die vor dem Ausführen der Aufgabe auf einen Computeknoten heruntergeladen wird. Mit der [ResourceFile][py_resource_file].**blob\_source**-Eigenschaft wird die vollständige URL der Datei so angegeben, wie sie in Azure Storage vorhanden ist. Die URL kann auch eine Shared Access Signature (SAS) enthalten, die sicheren Zugriff auf die Datei gewährt. Die meisten Aufgabentypen in Batch enthalten eine *ResourceFiles*-Eigenschaft. Hierzu zählen:

- [CloudTask][py_task]
- [StartTask][py_starttask]
- [JobPreparationTask][py_jobpreptask]
- [JobReleaseTask][py_jobreltask]

In diesem Beispiel werden die Aufgabentypen „JobPreparationTask“ und „JobReleaseTask“ nicht verwendet. Weitere Informationen hierzu finden Sie bei Bedarf unter [Ausführen von Auftragsvorbereitungs- und Auftragsabschlussaufgaben auf Azure Batch-Computeknoten](batch-job-prep-release.md).

### Shared Access Signature (SAS)

Bei Shared Access Signatures handelt es sich um Zeichenfolgen, die den sicheren Zugriff auf Container und Blobs in Azure Storage ermöglichen. Das Skript *python\_tutorial\_client.py* nutzt Shared Access Signatures sowohl von Blobs als auch von Containern und veranschaulicht, wie diese Shared Access Signature-Zeichenfolgen aus dem Storage-Dienst abgerufen werden.

- **Blob-Shared Access Signatures**: In der StartTask-Aufgabe des Pools werden Blob-Shared Access Signatures verwendet, wenn das Aufgabenskript und Eingabedatendateien aus Storage heruntergeladen werden (siehe [Schritt 3](#step-3-create-batch-pool) weiter unten). Die `upload_file_to_container`-Funktion in *python\_tutorial\_client.py* enthält den Code, mit dem die Shared Access Signature der einzelnen Blobs abgerufen wird. Dies erfolgt durch das Aufrufen von [BlockBlobService.make\_blob\_url][py_make_blob_url] im Storage-Modul.

- **Container-Shared Access Signatures**: Wenn die Arbeit einer Aufgabe auf dem Computeknoten abgeschlossen ist, wird die Ausgabedatei in Azure Storage in den Container *output* hochgeladen. Dafür verwendet *python\_tutorial\_task.py* eine Container-Shared Access Signature, die Schreibzugriff auf den Container bietet. Die Funktion `get_container_sas_token` in *python\_tutorial\_client.py* bezieht die Shared Access Signature des Containers, die daraufhin als Befehlszeilenargument an die Aufgaben übergeben wird. In Schritt 5, [Hinzufügen von Aufgaben zu Aufträgen](#step-5-add-tasks-to-job), wird die Verwendung von Container-SAS beschrieben.

> [AZURE.TIP] Sehen Sie sich die zweiteilige Reihe zu Shared Access Signatures an: [Teil 1: Grundlagen zum SAS-Modell](../storage/storage-dotnet-shared-access-signature-part-1.md) und [Teil 2: Erstellen und Verwenden einer SAS mit dem Blob-Dienst](../storage/storage-dotnet-shared-access-signature-part-2.md). Sie erhalten darin weitere Informationen zur Bereitstellung des sicheren Zugriffs auf Daten in Ihrem Storage-Konto.

## Schritt 3: Erstellen eines Batch-Pools

![Batch-Pool erstellen][3] <br/>

Ein Batch-**Pool** ist eine Sammlung von Computeknoten (virtuellen Computern), auf denen Batch die Aufgaben eines Auftrags ausführt.

Nach dem Hochladen des Aufgabenskripts und der Datendateien in das Storage-Konto beginnt *python\_tutorial\_client.py* unter Verwendung des Batch Python-Moduls die Interaktion mit dem Batch-Dienst. Dafür wird ein [BatchServiceClient][py_batchserviceclient] erstellt:

```python
 # Create a Batch service client. We'll now be interacting with the Batch
 # service in addition to Storage.
 credentials = batchauth.SharedKeyCredentials(_BATCH_ACCOUNT_NAME,
                                              _BATCH_ACCOUNT_KEY)

 batch_client = batch.BatchServiceClient(
     credentials,
     base_url=_BATCH_ACCOUNT_URL)
```

Als Nächstes wird mit einem Aufruf von `create_pool` im Batch-Konto ein Pool mit Computeknoten erstellt.

```python
def create_pool(batch_service_client, pool_id,
                resource_files, publisher, offer, sku):
    """
    Creates a pool of compute nodes with the specified OS settings.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str pool_id: An ID for the new pool.
    :param list resource_files: A collection of resource files for the pool's
    start task.
    :param str publisher: Marketplace image publisher
    :param str offer: Marketplace image offer
    :param str sku: Marketplace image sku
    """
    print('Creating pool [{}]...'.format(pool_id))

    # Create a new pool of Linux compute nodes using an Azure Virtual Machines
    # Marketplace image. For more information about creating pools of Linux
    # nodes, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/

    # Specify the commands for the pool's start task. The start task is run
    # on each node as it joins the pool, and when it's rebooted or re-imaged.
    # We use the start task to prep the node for running our task script.
    task_commands = [
        # Copy the python_tutorial_task.py script to the "shared" directory
        # that all tasks that run on the node have access to.
        'cp -r $AZ_BATCH_TASK_WORKING_DIR/* $AZ_BATCH_NODE_SHARED_DIR',
        # Install pip and the dependencies for cryptography
        'apt-get update',
        'apt-get -y install python-pip',
        'apt-get -y install build-essential libssl-dev libffi-dev python-dev',
        # Install the azure-storage module so that the task script can access
        # Azure Blob storage
        'pip install azure-storage']

    # Get the node agent SKU and image reference for the virtual machine
    # configuration.
    # For more information about the virtual machine configuration, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/
    sku_to_use, image_ref_to_use = \
        common.helpers.select_latest_verified_vm_image_with_node_agent_sku(
            batch_service_client, publisher, offer, sku)

    new_pool = batch.models.PoolAddParameter(
        id=pool_id,
        virtual_machine_configuration=batchmodels.VirtualMachineConfiguration(
            image_reference=image_ref_to_use,
            node_agent_sku_id=sku_to_use),
        vm_size=_POOL_VM_SIZE,
        target_dedicated=_POOL_NODE_COUNT,
        start_task=batch.models.StartTask(
            command_line=
            common.helpers.wrap_commands_in_shell('linux', task_commands),
            run_elevated=True,
            wait_for_success=True,
            resource_files=resource_files),
        )

    try:
        batch_service_client.pool.add(new_pool)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

Wenn Sie einen Pool erstellen, definieren Sie mit [PoolAddParameter][py_pooladdparam] verschiedene Eigenschaften für den Pool:

- **ID** des Pools (*id* – erforderlich)<p/>Wie die meisten Entitäten in Batch muss der neue Pool über eine eindeutige ID in Ihrem Batch-Konto verfügen. Ihr Code verweist mit seiner ID auf diesen Pool. So wird der Pool im Azure-[Portal][azure_portal] identifiziert.

- **Anzahl von Computeknoten** (*target\_dedicated* – erforderlich)<p/>Diese Eigenschaft gibt an, wie viele virtuelle Computer im Pool bereitgestellt werden sollen. Beachten Sie, dass alle Batch-Konten über ein Standard**kontingent** verfügen, das die Anzahl von **Kernen** (und somit auch von Computeknoten) in einem Batch-Konto begrenzt. Die Standardkontingente und eine Anleitung zum [Erhöhen des Kontingents](batch-quota-limit.md#increase-a-quota) (wie etwa der Anzahl von Kernen in Ihrem Batch-Konto) finden Sie unter [Kontingente und Limits für den Azure Batch-Dienst](batch-quota-limit.md). Wenn Sie sich z.B. die Frage stellen, weshalb Ihr Pool nur eine bestimmte Anzahl von Knoten erreicht, liegt dies möglicherweise am Kernkontingent.

- **Betriebssystem** für Knoten (*virtual\_machine\_configuration* **oder** *cloud\_service\_configuration* – erforderlich)<p/>In *python\_tutorial\_client.py* haben wir einen Pool mit Linux-Knoten mithilfe eines [VirtualMachineConfiguration][py_vm_config]-Elements erstellt. Mit der Funktion `select_latest_verified_vm_image_with_node_agent_sku` in `common.helpers` wird die Verwendung von Images des [Azure Virtual Machines Marketplace][vm_marketplace] vereinfacht. Unter [Bereitstellen von Linux-Computeknoten in Azure Batch-Pools](batch-linux-nodes.md) finden Sie weitere Informationen zur Verwendung von Marketplace-Images.

- **Größe der Computeknoten** (*vm\_size* – erforderlich)<p/>Da für [VirtualMachineConfiguration][py_vm_config] Linux-Knoten angegeben werden, legen wir die Größe des virtuellen Computers fest (in diesem Beispiel `STANDARD_A1`), wie unter [Größen für virtuelle Computer in Azure](../virtual-machines/virtual-machines-linux-sizes.md) beschrieben. Unter [Bereitstellen von Linux-Computeknoten in Azure Batch-Pools](batch-linux-nodes.md) finden Sie ebenfalls wieder weitere Informationen.

- **Startaufgabe** (*start\_task* – nicht erforderlich)<p/>Zusammen mit den o.g. Eigenschaften des physischen Knotens können Sie auch eine [StartTask][py_starttask]-Aufgabe für den Pool festlegen (nicht erforderlich). Die StartTask wird auf jedem Knoten ausgeführt, wenn dieser dem Pool hinzugefügt wird, sowie bei jedem Neustart eines Knotens. Die StartTask-Aufgabe ist insbesondere hilfreich, um Computeknoten auf die Ausführung von Aufgaben vorzubereiten, z.B. die Installation von Anwendungen, die von Ihren Aufgaben ausgeführt werden.<p/>In dieser Beispielanwendung kopiert die StartTask-Aufgabe die Dateien, die aus Storage heruntergeladen werden (und die mit der **resource\_files**-Eigenschaft von StartTask angegeben werden), aus dem StartTask-*Arbeitsverzeichnis* in das *freigegebene* Verzeichnis. Auf dieses Verzeichnis können alle auf dem Knoten ausgeführten Aufgaben zugreifen. Im Wesentlichen wird dadurch `python_tutorial_task.py` in das freigegebene Verzeichnis jedes Knotens kopiert, wenn der Knoten dem Pool beitritt. So können alle Aufgaben, die auf dem Knoten ausgeführt werden, darauf zugreifen.

Wie Sie vielleicht sehen, wird die Hilfsfunktion `wrap_commands_in_shell` aufgerufen. Diese Funktion erstellt mit einer Sammlung separater Befehle eine einzelne Befehlszeile, die für die Befehlszeileneigenschaft einer Aufgabe geeignet ist.

Beachten Sie im obigen Codeausschnitt auch die Verwendung von zwei Umgebungsvariablen in der **command\_line**-Eigenschaft von StartTask: `AZ_BATCH_TASK_WORKING_DIR` und `AZ_BATCH_NODE_SHARED_DIR`. Jeder Computeknoten in einem Batch-Pool wird automatisch mit verschiedenen Umgebungsvariablen konfiguriert, die speziell für Batch gelten. Für alle Prozesse, die von einer Aufgabe ausgeführt werden, besteht Zugriff auf diese Umgebungsvariablen.

> [AZURE.TIP] Weitere Informationen zu den Umgebungsvariablen, die auf Computeknoten in einem Batch-Pool verfügbar sind, sowie Informationen zu Arbeitsverzeichnissen von Aufgaben finden Sie unter **Umgebungseinstellungen für Aufgaben** und **Dateien und Verzeichnisse** in der [Übersicht über Azure Batch-Features](batch-api-basics.md).

## Schritt 4: Erstellen eines Batch-Auftrags

![Batch-Auftrag erstellen][4]<br/>

Ein Batch-**Auftrag** ist im Wesentlichen eine Sammlung von Aufgaben, die einem Pool mit Computeknoten zugeordnet sind. Die Aufgaben in einem Auftrag werden auf den Computeknoten des zugeordneten Pools ausgeführt.

Mit einem Auftrag wird nicht nur das Organisieren und Nachverfolgen von Aufgaben in zusammengehörenden Workloads ermöglicht, sondern es können auch bestimmte Beschränkungen festgelegt werden. Beispiele hierfür sind die maximale Laufzeit für den Auftrag (und somit auch seiner Aufgaben) und die Auftragspriorität in Bezug auf andere Aufträge im Batch-Konto. In diesem Beispiel ist die Aufgabe aber nur dem Pool zugeordnet, der in Schritt 3 erstellt wurde. Es werden keine zusätzlichen Eigenschaften konfiguriert.

Alle Batch-Aufträge sind einem bestimmten Pool zugeordnet. Diese Zuordnung gibt an, auf welchen Knoten die Aufgaben des Auftrags ausgeführt werden. Sie geben den Pool mit der Eigenschaft [PoolInformation][py_poolinfo] an (siehe Codeausschnitt weiter unten).

```python
def create_job(batch_service_client, job_id, pool_id):
    """
    Creates a job with the specified ID, associated with the specified pool.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The ID for the job.
    :param str pool_id: The ID for the pool.
    """
    print('Creating job [{}]...'.format(job_id))

    job = batch.models.JobAddParameter(
        job_id,
        batch.models.PoolInformation(pool_id=pool_id))

    try:
        batch_service_client.job.add(job)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

Nachdem ein Auftrag erstellt wurde, werden Aufgaben zum Durchführen der Arbeitsschritte hinzugefügt.

## Schritt 5: Hinzufügen von Aufgaben zum Auftrag

![Aufgaben zu Auftrag hinzufügen][5]<br/> *(1) Die Aufgaben werden dem Auftrag hinzugefügt, (2) die Aufgaben werden für die Ausführung auf Knoten eingeplant, und (3) für die Aufgaben werden die zu verarbeitenden Datendateien heruntergeladen.*

Batch-**Aufgaben** sind die einzelnen Arbeitseinheiten, die auf den Computeknoten ausgeführt werden. Eine Aufgabe verfügt über eine Befehlszeile und führt die Skripts oder ausführbaren Dateien aus, die Sie in der Befehlszeile festlegen.

Zum eigentlichen Ausführen der Arbeitsschritte müssen die Aufgaben einem Auftrag hinzugefügt werden. Jede [CloudTask][py_task]-Aufgabe wird mit einer Befehlszeileneigenschaft und [ResourceFiles][py_resource_file] (wie bei der StartTask-Aufgabe des Pools) konfiguriert, die von der Aufgabe auf den Knoten heruntergeladen werden, bevor die Befehlszeile automatisch ausgeführt wird. Im Beispiel verarbeitet jede Aufgabe nur eine Datei. Daher enthält die ResourceFiles-Sammlung in diesem Fall ein einzelnes Element.

```python
def add_tasks(batch_service_client, job_id, input_files,
              output_container_name, output_container_sas_token):
    """
    Adds a task for each input file in the collection to the specified job.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The ID of the job to which to add the tasks.
    :param list input_files: A collection of input files. One task will be
     created for each input file.
    :param output_container_name: The ID of an Azure Blob storage container to
    which the tasks will upload their results.
    :param output_container_sas_token: A SAS token granting write access to
    the specified Azure Blob storage container.
    """

    print('Adding {} tasks to job [{}]...'.format(len(input_files), job_id))

    tasks = list()

    for input_file in input_files:

        command = ['python $AZ_BATCH_NODE_SHARED_DIR/python_tutorial_task.py '
                   '--filepath {} --numwords {} --storageaccount {} '
                   '--storagecontainer {} --sastoken "{}"'.format(
                    input_file.file_path,
                    '3',
                    _STORAGE_ACCOUNT_NAME,
                    output_container_name,
                    output_container_sas_token)]

        tasks.append(batch.models.TaskAddParameter(
                'topNtask{}'.format(input_files.index(input_file)),
                wrap_commands_in_shell('linux', command),
                resource_files=[input_file]
                )
        )

    batch_service_client.task.add_collection(job_id, tasks)
```

> [AZURE.IMPORTANT] Beim Zugreifen auf Umgebungsvariablen (beispielsweise `$AZ_BATCH_NODE_SHARED_DIR`) oder beim Ausführen einer Anwendung, die nicht im `PATH` des Knotens enthalten ist, müssen Aufgabenbefehlszeilen die Shell explizit aufrufen, z.B. mit `/bin/sh -c MyTaskApplication $MY_ENV_VAR`. Diese Anforderung ist nicht relevant, falls Ihre Aufgaben eine Anwendung im `PATH` des Knotens ausführen und nicht auf Umgebungsvariablen verweisen.

In der `for`-Schleife im obigen Codeausschnitt sehen Sie, dass die Befehlszeile für die Aufgabe mit fünf Befehlszeilenargumenten erstellt wurde, die an *python\_tutorial\_task.py* übergeben werden:

1. **filepath**: Dies ist der lokale Pfad zu der Datei, wie sie auf dem Knoten vorhanden ist. Bei der Erstellung des ResourceFile-Objekts in `upload_file_to_container` in Schritt 2 oben wurde der Dateiname für diese Eigenschaft verwendet (der Parameter `file_path` für den ResourceFile-Konstruktor). Dies deutet darauf hin, dass sich die Datei auf dem Knoten im gleichen Verzeichnis wie *python\_tutorial\_task.py* befindet.

2. **numwords**: Die obersten *N* Wörter sollen in die Ausgabedatei geschrieben werden.

3. **storageaccount**: Der Name des Storage-Kontos, das als Besitzer des Containers angegeben ist, in den die Aufgabenausgabe hochgeladen werden soll.

4. **storagecontainer**: Der Name des Storage-Containers, in den die Ausgabedateien hochgeladen werden sollen.

5. **sastoken**: Die Shared Access Signature (SAS), die Schreibzugriff auf den Container **output** in Azure Storage gewährt. Das Skript *python\_tutorial\_task.py* verwendet diese Shared Access Signature beim Erstellen des BlockBlobService-Verweises. Dadurch wird Schreibzugriff auf den Container gewährt, ohne dass eine Tastenkombination für das Storage-Konto erforderlich ist.

```python
# NOTE: Taken from python_tutorial_task.py

# Create the blob client using the container's SAS token.
# This allows us to create a client that provides write
# access only to the container.
blob_client = azureblob.BlockBlobService(account_name=args.storageaccount,
                                         sas_token=args.sastoken)
```

## Schritt 6: Überwachen von Aufgaben

![Aufgaben überwachen][6]<br/> *Das Skript (1) überwacht für die Aufgaben den Abschlussstatus, und (2) die Aufgaben laden Ergebnisdaten in Azure Storage hoch.*

Wenn einem Auftrag Aufgaben hinzugefügt werden, werden sie automatisch in die Warteschlange eingereiht und für die Ausführung auf Computeknoten in dem Pool eingeplant, der dem Auftrag zugeordnet ist. Basierend auf den Einstellungen, die Sie angeben, führt Batch das Einreihen, Planen und erneute Ausführen sowie andere Schritte der Aufgabenverwaltung für Sie durch.

Es gibt viele Ansätze für die Überwachung der Aufgabenausführung. Die Funktion `wait_for_tasks_to_complete` in *python\_tutorial\_client.py* bietet ein einfaches Beispiel für das Überwachen von Aufgaben für einen bestimmten Status. In diesem Fall ist dies [completed][py_taskstate] (abgeschlossen).

```python
def wait_for_tasks_to_complete(batch_service_client, job_id, timeout):
    """
    Returns when all tasks in the specified job reach the Completed state.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The id of the job whose tasks should be to monitored.
    :param timedelta timeout: The duration to wait for task completion. If all
    tasks in the specified job do not reach Completed state within this time
    period, an exception will be raised.
    """
    timeout_expiration = datetime.datetime.now() + timeout

    print("Monitoring all tasks for 'Completed' state, timeout in {}..."
          .format(timeout), end='')

    while datetime.datetime.now() < timeout_expiration:
        print('.', end='')
        sys.stdout.flush()
        tasks = batch_service_client.task.list(job_id)

        incomplete_tasks = [task for task in tasks if
                            task.state != batchmodels.TaskState.completed]
        if not incomplete_tasks:
            print()
            return True
        else:
            time.sleep(1)

    print()
    raise RuntimeError("ERROR: Tasks did not reach 'Completed' state within "
                       "timeout period of " + str(timeout))
```

## Schritt 7: Herunterladen der Aufgabenausgabe

![Aufgabenausgabe aus Storage herunterladen][7]<br/>

Nachdem der Auftrag abgeschlossen wurde, kann die Ausgabe der Aufgaben aus Azure Storage heruntergeladen werden. Hierzu wird `download_blobs_from_container` in *python\_tutorial\_client.py* aufgerufen:

```python
def download_blobs_from_container(block_blob_client,
                                  container_name, directory_path):
    """
    Downloads all blobs from the specified Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param container_name: The Azure Blob storage container from which to
     download files.
    :param directory_path: The local directory to which to download the files.
    """
    print('Downloading all files from container [{}]...'.format(
        container_name))

    container_blobs = block_blob_client.list_blobs(container_name)

    for blob in container_blobs.items:
        destination_file_path = os.path.join(directory_path, blob.name)

        block_blob_client.get_blob_to_path(container_name,
                                           blob.name,
                                           destination_file_path)

        print('  Downloaded blob [{}] from container [{}] to {}'.format(
            blob.name,
            container_name,
            destination_file_path))

    print('  Download complete!')
```

> [AZURE.NOTE] Der Aufruf von `download_blobs_from_container` in *python\_tutorial\_client.py* gibt an, dass die Dateien in Ihr Stammverzeichnis heruntergeladen werden sollen. Sie können diesen Ausgabespeicherort ändern.

## Schritt 8: Löschen von Containern

Da Ihnen Daten berechnet werden, die sich in Azure Storage befinden, ist Folgendes ratsam: Entfernen Sie alle Blobs, die für Ihre Batch-Aufträge nicht mehr benötigt werden. In *python\_tutorial\_client.py* erfolgt dies mit drei Aufrufen von [BlockBlobService.delete\_container][py_delete_container]\:

```
# Clean up storage resources
print('Deleting containers...')
blob_client.delete_container(app_container_name)
blob_client.delete_container(input_container_name)
blob_client.delete_container(output_container_name)
```

## Schritt 9: Löschen des Auftrags und des Pools

Im letzten Schritt werden Sie aufgefordert, den Auftrag und den Pool zu löschen, die vom Skript *python\_tutorial\_client.py* erstellt wurden. Für die Aufträge und Aufgaben fallen zwar keine Kosten an, *für Computeknoten dagegen schon*. Daher empfehlen wir Ihnen, Knoten nur bei Bedarf zuzuordnen. Sie können das Löschen von nicht verwendeten Pools beispielsweise im Rahmen Ihres Wartungsprozesses durchführen.

[JobOperations][py_job] und [PoolOperations][py_pool] von BatchServiceClient verfügen jeweils über entsprechende Löschmethoden, die aufgerufen werden, wenn Sie das Löschen bestätigen:

```python
# Clean up Batch resources (if the user so chooses).
if query_yes_no('Delete job?') == 'yes':
    batch_client.job.delete(_JOB_ID)

if query_yes_no('Delete pool?') == 'yes':
    batch_client.pool.delete(_POOL_ID)
```

> [AZURE.IMPORTANT] Denken Sie daran, dass für Computeressourcen Kosten anfallen und dass Sie durch das Löschen nicht verwendeter Pools Kosten sparen können. Beachten Sie außerdem, dass beim Löschen eines Pools alle Computeknoten dieses Pools gelöscht werden und dass alle Daten auf den Knoten nicht mehr wiederhergestellt werden können, nachdem der Pool gelöscht wurde.

## Ausführen des Beispiel-Skripts

Wenn Sie das Skript *python\_tutorial\_client.py* aus dem [Codebeispiel][github_article_samples] des Tutorials ausführen, sieht die Konsolenausgabe in etwa wie folgt aus. Bei `Monitoring all tasks for 'Completed' state, timeout in 0:20:00...` kommt es zu einer Pause, wenn der Pool mit Computeknoten erstellt und gestartet wird und die Befehle in der Startaufgabe (Starttask) des Pools ausgeführt werden. Verwenden Sie das [Azure-Portal][azure_portal], um den Pool, die Computeknoten, den Auftrag und die Aufgaben während und nach der Ausführung zu überwachen. Verwenden Sie das [Azure-Portal][azure_portal] oder den [Microsoft Azure-Speicher-Explorer][storage_explorer], um die von der Anwendung erstellten Speicherressourcen (Container und Blobs) anzuzeigen.

>[AZURE.TIP] Führen Sie das Skript *python\_tutorial\_client.py* im Verzeichnis `azure-batch-samples/Python/Batch/article_samples` aus. Hierbei wird ein relativer Pfad für den `common.helpers`-Modulimport verwendet. Unter Umständen wird also `ImportError: No module named 'common'` angezeigt, wenn Sie das Skript nicht in diesem Verzeichnis ausführen.

Die normale Ausführungsdauer beträgt **ca. 5 bis 7 Minuten**, wenn Sie das Beispiel in der Standardkonfiguration ausführen.

```
Sample start: 2016-05-20 22:47:10

Uploading file /home/user/py_tutorial/python_tutorial_task.py to container [application]...
Uploading file /home/user/py_tutorial/data/taskdata1.txt to container [input]...
Uploading file /home/user/py_tutorial/data/taskdata2.txt to container [input]...
Uploading file /home/user/py_tutorial/data/taskdata3.txt to container [input]...
Creating pool [PythonTutorialPool]...
Creating job [PythonTutorialJob]...
Adding 3 tasks to job [PythonTutorialJob]...
Monitoring all tasks for 'Completed' state, timeout in 0:20:00..........................................................................
  Success! All tasks reached the 'Completed' state within the specified timeout period.
Downloading all files from container [output]...
  Downloaded blob [taskdata1_OUTPUT.txt] from container [output] to /home/user/taskdata1_OUTPUT.txt
  Downloaded blob [taskdata2_OUTPUT.txt] from container [output] to /home/user/taskdata2_OUTPUT.txt
  Downloaded blob [taskdata3_OUTPUT.txt] from container [output] to /home/user/taskdata3_OUTPUT.txt
  Download complete!
Deleting containers...

Sample end: 2016-05-20 22:53:12
Elapsed time: 0:06:02

Delete job? [Y/n]
Delete pool? [Y/n]

Press ENTER to exit...
```

## Nächste Schritte

Nehmen Sie Änderungen an *python\_tutorial\_client.py* und *python\_tutorial\_task.py* vor, um mit verschiedenen Computeszenarien zu experimentieren. Versuchen Sie beispielsweise, *python\_tutorial\_task.py* eine Ausführungsverzögerung hinzuzufügen, um Aufgaben mit langer Ausführungsdauer zu simulieren und im Portal zu überwachen. Versuchen Sie, weitere Aufgaben hinzuzufügen oder die Anzahl von Computeknoten anzupassen. Fügen Sie logische Abläufe hinzu, um einen bestehenden Pool zu suchen und dessen Verwendung zur Beschleunigung der Ausführungszeit zuzulassen.

Nachdem Sie sich jetzt mit dem grundlegenden Workflow einer Batch-Lösung vertraut gemacht haben, können wir uns mit den zusätzlichen Features des Batch-Diensts beschäftigen.

- Falls Sie noch keine Erfahrung mit dem Dienst haben, helfen Ihnen die Informationen im Artikel [Übersicht über Azure Batch-Features](batch-api-basics.md) weiter.
- Beginnen Sie mit den anderen Artikeln zur Batch-Entwicklung unter **Entwicklung im Detail** im [Lernpfad für Batch][batch_learning_path].
- Sehen Sie sich eine andere Implementierung einer Verarbeitung der Workload „Oberste N Wörter“ mit Batch im Beispiel [TopNWords][github_topnwords] an.

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[blog_linux]: http://blogs.technet.com/b/windowshpc/archive/2016/03/30/introducing-linux-support-on-azure-batch.aspx
[crypto]: https://cryptography.io/en/latest/
[crypto_install]: https://cryptography.io/en/latest/installation/
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[github_article_samples]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/article_samples

[nuget_packagemgr]: https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build

[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_batchserviceclient]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html#azure.batch.BatchServiceClient
[py_blockblobservice]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.blockblobservice.html#azure.storage.blob.blockblobservice.BlockBlobService
[py_cloudtask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_cs_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudServiceConfiguration
[py_delete_container]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.delete_container
[py_gen_blob_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_blob_shared_access_signature
[py_gen_container_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_container_shared_access_signature
[py_image_ref]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_job]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.JobOperations
[py_jobpreptask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobPreparationTask
[py_jobreltask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobReleaseTask
[py_list_skus]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[py_make_blob_url]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.make_blob_url
[py_pool]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.PoolOperations
[py_pooladdparam]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolAddParameter
[py_poolinfo]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolInformation
[py_resource_file]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ResourceFile
[py_samples_github]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_task]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_taskstate]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.TaskState
[py_vm_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.VirtualMachineConfiguration
[pypi_batch]: https://pypi.python.org/pypi/azure-batch
[pypi_storage]: https://pypi.python.org/pypi/azure-storage

[pypi_install]: http://python-packaging-user-guide.readthedocs.io/en/latest/installing/
[storage_explorer]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/products/vs-2015-product-editions
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/

[1]: ./media/batch-python-tutorial/batch_workflow_01_sm.png "Container in Azure Storage erstellen"
[2]: ./media/batch-python-tutorial/batch_workflow_02_sm.png "Aufgabenanwendungs- und Eingabe(daten)dateien in Container hochladen"
[3]: ./media/batch-python-tutorial/batch_workflow_03_sm.png "Batch-Pool erstellen"
[4]: ./media/batch-python-tutorial/batch_workflow_04_sm.png "Batch-Auftrag erstellen"
[5]: ./media/batch-python-tutorial/batch_workflow_05_sm.png "Aufgaben zu Auftrag hinzufügen"
[6]: ./media/batch-python-tutorial/batch_workflow_06_sm.png "Aufgaben überwachen"
[7]: ./media/batch-python-tutorial/batch_workflow_07_sm.png "Aufgabenausgabe aus Storage herunterladen"
[8]: ./media/batch-python-tutorial/batch_workflow_sm.png "Batch-Lösungsworkflow (vollständiges Diagramm)"
[9]: ./media/batch-python-tutorial/credentials_batch_sm.png "Batch-Anmeldeinformationen im Portal"
[10]: ./media/batch-python-tutorial/credentials_storage_sm.png "Storage-Anmeldeinformationen im Portal"
[11]: ./media/batch-python-tutorial/batch_workflow_minimal_sm.png "Batch-Lösungsworkflow (reduziertes Diagramm)"

<!---HONumber=AcomDC_0928_2016-->