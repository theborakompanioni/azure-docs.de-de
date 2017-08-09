---
title: "Nahtloses Ausführen von Azure Batch-Aufträgen ohne das Schreiben von Codes (Vorschau) | Microsoft-Dokumentation"
description: "Erstellen Sie Vorlagendateien für die Azure CLI, um Batch-Pools, -Aufträge und -Aufgaben zu erstellen."
services: batch
author: mscurrell
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: na
ms.topic: article
ms.workload: big-compute
ms.date: 07/20/2017
ms.author: markscu
ms.translationtype: HT
ms.sourcegitcommit: 22aa82e5cbce5b00f733f72209318c901079b665
ms.openlocfilehash: 6b91466da46d1f4ca9f25bf1718be783603efc58
ms.contentlocale: de-de
ms.lasthandoff: 07/24/2017

---

# <a name="use-azure-batch-cli-templates-and-file-transfer-preview"></a>Verwenden von Azure Batch-CLI-Vorlagen und Dateiübertragung (Vorschau)

Mithilfe der Azure CLI können Batch-Aufträge ausgeführt werden, ohne Code zu schreiben.

Mit der Azure CLI können Vorlagendateien erstellt und verwendet werden, die die Erstellung von Batch-Pools, -Aufträgen und -Aufgaben ermöglichen. Auftragseingabedateien können problemlos in das Speicherkonto hochgeladen werden, das dem Batch-Konto und den heruntergeladenen Auftragsausgabedateien zugeordnet ist.

## <a name="overview"></a>Übersicht

Durch eine Erweiterung für die Azure CLI kann Batch nahtlos von Benutzern verwendet werden, die keine Entwickler sind. So können Pools erstellt, Eingabedaten hochgeladen, Aufträge und zugehörige Aufgaben erstellt und die resultierenden Ausgabedaten heruntergeladen werden – und das ohne Code, da die CLI direkt verwendet oder in Skripts integriert wird.

Batch-Vorlagen basieren auf der [vorhandenen Unterstützung für Batch in der Azure CLI](https://docs.microsoft.com/azure/batch/batch-cli-get-started#json-files-for-resource-creation), durch die mithilfe von JSON-Dateien Eigenschaftenwerte für die Erstellung von Pools, Aufträgen, Aufgaben und anderen Elementen angegeben werden können. Durch Batch-Vorlagen werden neben den bestehenden Funktionen bei JSON-Dateien folgende Funktionen hinzugefügt:

-   Parameter können definiert werden. Bei der Verwendung von Vorlagen werden nur die Parameterwerte für die Erstellung des Elements angegeben, während die anderen Elementeigenschaftenwerte im Vorlagentext angegeben werden. Ein Benutzer, der mit Batch und den von Batch ausgeführten Anwendungen vertraut ist, kann Vorlagen erstellen, die Eigenschaftenwerte für Pools, Aufträge und Aufgaben angeben. Ein Benutzer, dem Batch und/oder die Anwendungen weniger vertraut ist, muss lediglich die Werte für die definierten Parameter angeben.

-   Durch Auftragsaufgabenfactorys können mehrere Aufgaben für einen Auftrag erstellt werden, sodass keine zahlreichen Aufgabendefinitionen erstellt werden müssen und die Auftragsübermittlung drastisch vereinfacht wird.


Es müssen Eingabedatendateien für Aufträge bereitgestellt, und es werden häufig Ausgabedatendateien erzeugt. Ein Speicherkonto ist standardmäßig jedem Batch-Konto zugeordnet, und Dateien können mithilfe der CLI problemlos an dieses bzw. aus diesem Speicherkonto übertragen werden, ohne dass Code oder Speicheranmeldeinformationen erforderlich sind.

Eine gängige Anwendung für die Verarbeitung von Audio- und Videodateien ist beispielsweise [ffmpeg](http://ffmpeg.org/). Mit der Azure Batch-CLI kann ffmpeg aufgerufen werden, um Videoquelldateien in anderen Auflösungen zu transcodieren.

-   Eine Poolvorlage wird erstellt. Der Benutzer, der die Vorlage erstellt, weiß, wie die ffmpeg-Anwendung und deren Anforderungen aufgerufen werden. Der Benutzer gibt das entsprechende BS, die VM-Größe, die Methode für die Installation von ffmpeg (z.B. über ein Anwendungspaket oder mithilfe eines Paket-Managers) und andere Pooleigenschaftenwerte an. Parameter werden bei der Verwendung der Vorlage erstellt, nur die Pool-ID und die Anzahl der VMs müssen angegeben werden.

-   Eine Auftragsvorlage wird erstellt. Der Benutzer, der die Vorlage erstellt, weiß, wie ffmpeg aufgerufen werden muss, um ein Quellvideo in einer anderen Auflösung zu transcodieren, und gibt die Taskbefehlszeile an. Zudem ist ihm bekannt, dass ein Ordner mit den Quellvideodateien vorhanden ist, für die eine Aufgabe pro Eingabedatei erforderlich ist.

-   Ein Endbenutzer, der einen Satz von Videodateien transcodieren muss, erstellt zunächst einen Pool anhand der Poolvorlage, die nur die Pool-ID und die Anzahl der erforderlichen VMs angibt. Danach kann der Benutzer die zu transcodierenden Quelldateien hochladen. Anschließend kann ein Auftrag mithilfe der Auftragsvorlage angegeben werden, die nur die Pool-ID und den Speicherort der hochgeladenen Quelldateien angibt. Der Batch-Auftrag wird erstellt, wobei pro Eingabedatei eine Aufgabe generiert wird. Schließlich können die transcodierten Ausgabedateien heruntergeladen werden.

## <a name="installation"></a>Installation

Für die Vorlagen- und Dateiübertragungsfunktionen muss keine Erweiterung installiert werden.

Anweisungen zum Installieren der Azure CLI finden Sie unter [Installieren von Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).

Nachdem die Azure CLI installiert wurde, kann die Batch-Erweiterung mit dem folgenden CLI-Befehl installiert werden:

```azurecli
az component update --add batch-extensions --allow-third-party
```

Weitere Informationen über die Batch-Erweiterung finden Sie unter [Microsoft Azure Batch-CLI-Erweiterungen für Windows, Mac und Linux](https://github.com/Azure/azure-batch-cli-extensions#microsoft-azure-batch-cli-extensions-for-windows-mac-and-linux).

## <a name="templates"></a>Vorlagen

Mithilfe der Azure Batch-CLI können Elemente wie Pools, Aufträge und Aufgaben erstellt werden, indem eine JSON-Datei mit Eigenschaftennamen und -werten angegeben wird. Beispiel:

```azurecli
az batch pool create –-json-file AppPool.json
```

Azure Batch-Vorlagen sind Azure Resource Manager-Vorlagen in Bezug auf Funktionalität und Syntax ähnlich. Dabei handelt es sich um JSON-Dateien, die Elementeigenschaftennamen und -werte enthalten, jedoch folgende zusätzliche Hauptkonzepte aufweisen:

-   **Parameter**

    -   Sie ermöglichen die Angabe von Eigenschaftenwerten in einem Textabschnitt, wobei Parameterwerte nur bei der Verwendung der Vorlage angegeben werden müssen. Beispielsweise könnte die vollständige Definition für einen Pool in den Text eingefügt und nur ein Parameter für die Pool-ID definiert werden. Für die Erstellung eines Pools müsste dann nur eine Pool-ID-Zeichenfolge angegeben werden.
        
    -   Der Vorlagentext kann von einem Benutzer mit Kenntnissen zu Batch und den von Batch ausgeführten Anwendungen erstellt werden. Bei der Verwendung der Vorlage müssen nur Werte für die vom Autor definierten Parameter angegeben werden. Daher kann ein Benutzer ohne fundierte Kenntnisse zu Batch und/oder den Anwendungen die Vorlagen verwenden.

-   **Variablen**

    -   Durch Variablen können einfache oder komplexe Parameterwerte an einer Stelle angegeben und an einer oder mehreren Stellen im Vorlagentext verwendet werden. Durch Variablen können Vorlagen vereinfacht, deren Größe reduziert und die Verwaltung vereinfacht werden, da Eigenschaften im Falle einer Änderung der Werte nur an einem Ort geändert werden müssen.

-   **Konstrukte auf höheren Ebenen**

    -   Einige in der Vorlage vorhandenen Konstrukte auf höheren Ebenen sind noch nicht in den Batch-APIs verfügbar. Beispielsweise kann eine Aufgabenfactory in einer Auftragsvorlage definiert werden, die mittels einer allgemeinen Aufgabendefinition mehrere Aufgaben für den Auftrag erstellt. Diese Konstrukte machen eine Codierung hinfällig, sodass mehrere JSON-Dateien dynamisch erstellt werden können (z.B. eine Datei pro Aufgabe). Zudem können Skriptdateien für die Installation von Anwendungen beispielsweise über einen Paket-Manager erstellt werden.

    -   Diese Konstrukte können gegebenenfalls dem Batch-Dienst hinzugefügt werden und sind in den Batch-APIs, auf den Benutzeroberflächen usw. verfügbar.

### <a name="pool-templates"></a>Poolvorlagen

Neben den standardmäßigen Vorlagenfunktionen der Parameter und Variablen werden die folgenden Konstrukte auf höherer Ebene von der Poolvorlage unterstützt:

-   **Paketverweise**

    -   Optional kann mithilfe des Paket-Managers Software auf Poolknoten kopiert werden. Der Paket-Manager und die Paket-ID werden angegeben. Dank der Möglichkeit, ein oder mehrere Pakete deklarieren zu können, müssen keine Skripts zum Abrufen der erforderlichen Pakete erstellt, Skripts installiert und Skripts auf jedem Poolknoten ausgeführt werden.

Im Folgenden wird ein Beispiel für eine Vorlage vorgestellt, die einen Pool von Linux-VMs mit installierter ffmpeg-Anwendung erstellt und nur eine Pool-ID-Zeichenfolge sowie die Anzahl der für die Verwendung bereitzustellenden VMs erfordert:

```json
{
    "parameters": {
        "nodeCount": {
            "type": "int",
            "metadata": {
                "description": "The number of pool nodes"
            }
        },
        "poolId": {
            "type": "string",
            "metadata": {
                "description": "The pool id "
            }
        }
    },
    "pool": {
        "type": "Microsoft.Batch/batchAccounts/pools",
        "apiVersion": "2016-12-01",
        "properties": {
            "id": "[parameters('poolId')]",
            "virtualMachineConfiguration": {
                "imageReference": {
                    "publisher": "Canonical",
                    "offer": "UbuntuServer",
                    "sku": "16.04.0-LTS",
                    "version": "latest"
                },
                "nodeAgentSKUId": "batch.node.ubuntu 16.04"
            },
            "vmSize": "STANDARD_D3_V2",
            "targetDedicatedNodes": "[parameters('nodeCount')]",
            "enableAutoScale": false,
            "maxTasksPerNode": 1,
            "packageReferences": [
                {
                    "type": "aptPackage",
                    "id": "ffmpeg"
                }
            ]
        }
    }
}
```

Wenn die Vorlagendatei unter dem Namen _pool-ffmpeg.json_ gespeichert wurde, würde die Vorlage wie folgt aufgerufen werden:

```azurecli
az batch pool create --template pool-ffmpeg.json
```

### <a name="job-templates"></a>Auftragsvorlagen

Neben den standardmäßigen Vorlagenfunktionen der Parameter und Variablen werden die folgenden Konstrukte auf höherer Ebene von der Auftragsvorlage unterstützt:

-   **Aufgabenfactory**

    -   Erstellt mehrere Aufgaben aus einer Aufgabendefinition. Es werden drei Arten von Aufgabenfactorys unterstützt – parametrische Sweeps, Aufgaben pro Datei und Aufgabenauflistung.

Im Folgenden wird ein Beispiel für eine Vorlage vorgestellt, die einen Auftrag zum Transcodieren von MP4-Videodateien in eine von zwei niedrigeren Auflösungen durch ffmpeg erstellt (dabei wird pro Videoquelldatei eine Aufgabe erstellt):

```json
{
    "parameters": {
        "poolId": {
            "type": "string",
            "metadata": {
                "description": "The name of Azure Batch pool which runs the job"
            }
        },
        "jobId": {
            "type": "string",
            "metadata": {
                "description": "The name of Azure Batch job"
            }
        },
        "resolution": {
            "type": "string",
            "defaultValue": "428x240",
            "allowedValues": [
                "428x240",
                "854x480"
            ],
            "metadata": {
                "description": "Target video resolution"
            }
        }
    },
    "job": {
        "type": "Microsoft.Batch/batchAccounts/jobs",
        "apiVersion": "2016-12-01",
        "properties": {
            "id": "[parameters('jobId')]",
            "constraints": {
                "maxWallClockTime": "PT5H",
                "maxTaskRetryCount": 1
            },
            "poolInfo": {
                "poolId": "[parameters('poolId')]"
            },
            "taskFactory": {
                "type": "taskPerFile",
                "source": { 
                    "fileGroup": "ffmpeg-input"
                },
                "repeatTask": {
                    "commandLine": "ffmpeg -i {fileName} -y -s [parameters('resolution')] -strict -2 {fileNameWithoutExtension}_[parameters('resolution')].mp4",
                    "resourceFiles": [
                        {
                            "blobSource": "{url}",
                            "filePath": "{fileName}"
                        }
                    ],
                    "outputFiles": [
                        {
                            "filePattern": "{fileNameWithoutExtension}_[parameters('resolution')].mp4",
                            "destination": {
                                "autoStorage": {
                                    "path": "{fileNameWithoutExtension}_[parameters('resolution')].mp4",
                                    "fileGroup": "ffmpeg-output"
                                }
                            },
                            "uploadOptions": {
                                "uploadCondition": "TaskSuccess"
                            }
                        }
                    ]
                }
            },
            "onAllTasksComplete": "terminatejob"
        }
    }
}
```

Wenn die Vorlagendatei unter dem Namen _job-ffmpeg.json_ gespeichert wurde, würde die Vorlage wie folgt aufgerufen werden:

```azurecli
az batch job create --template job-ffmpeg.json
```

## <a name="file-groups-and-file-transfer"></a>Dateigruppen und Dateiübertragung

Die meisten Aufträge und Aufgaben erfordern Eingabedateien und erzeugen Ausgabedateien. Eingabe- und Ausgabedateien müssen in der Regel vom Client zum Knoten oder vom Knoten zum Client übertragen werden. Die Azure Batch-CLI-Erweiterung abstrahiert die Dateiübertragung und nutzt das Speicherkonto, das standardmäßig für jedes Batch-Konto erstellt wird.

Eine Dateigruppe entspricht einem im Azure Storage-Konto erstellten Container. Die Dateigruppe kann Unterordner enthalten.

Die Batch CLI-Erweiterung unterstützt Befehle, mit denen Dateien von einem Client in eine bestimmte Dateigruppe hochgeladen und Dateien aus der angegebenen Dateigruppe auf einen Client heruntergeladen werden können.

```azurecli
az batch file upload --local-path c:\source_videos\*.mp4 
    --file-group ffmpeg-input

az batch file download --file-group ffmpeg-output --local-path
    c:\output_lowres_videos
```

Mit Pool- und Auftragsvorlagen können in Dateigruppen gespeicherte Dateien angegeben werden, die auf Poolknoten oder aus Poolknoten wieder in eine Dateigruppe kopiert werden sollen. In der zuvor angegebenen Auftragsvorlage wird z.B. die Dateigruppe „ffmpeg-input“ für die Aufgabenfactory als Speicherort der Quellvideodateien angegeben, die zum Transcodieren auf den Knoten kopiert werden. Die Dateigruppe „ffmpeg-output“ wird dagegen als Speicherort verwendet, in den die transcodierten Ausgabedateien von dem für jede Aufgabe ausgeführten Knoten kopiert werden.

## <a name="summary"></a>Zusammenfassung

Unterstützung für Vorlagen und die Dateiübertragung wurde derzeit nur zur Azure CLI hinzugefügt. Das Ziel besteht darin, die Zielgruppe, die Batch verwenden kann, auf Benutzer zu erweitern, die keine Codes mithilfe der Batch-APIs entwickeln müssen, wie u.a. Forscher und IT-Benutzer. So können Benutzer mit Kenntnissen in Azure, Batch und den von Batch auszuführenden Anwendungen ohne Codierung Vorlagen für die Erstellung von Pools und Aufträgen erstellen. Mit Vorlagenparametern können Benutzer ohne fundierte Kenntnisse in Batch und den Anwendungen Vorlagen verwenden.

Testen Sie die Batch-Erweiterung für die Azure CLI, und teilen Sie uns Ihr Feedback und Ihre Vorschläge mit. Dies können Sie entweder über die Kommentarfunktion dieses Artikels oder das [Azure Batch-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch) tun.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie im Blogbeitrag zu Batch-Vorlagen: [Ausführen von Azure Batch-Aufträgen mit der Azure CLI – kein Code erforderlich](https://azure.microsoft.com/en-us/blog/running-azure-batch-jobs-using-the-azure-cli-no-code-required/).
- Ausführliche Dokumente, Beispiele und Quellcodes zur Installation und Nutzung finden Sie im [Azure GitHub-Repository](https://github.com/Azure/azure-batch-cli-extensions).

