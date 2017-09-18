---
title: "Azure CLI-Skriptbeispiel: Ausführen eines Auftrags mit Batch | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel: Ausführen eines Auftrags mit Batch"
services: batch
documentationcenter: 
author: annatisch
manager: daryls
editor: tysonn
ms.assetid: 
ms.service: batch
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/02/2017
ms.author: antisch
ms.translationtype: HT
ms.sourcegitcommit: 190ca4b228434a7d1b30348011c39a979c22edbd
ms.openlocfilehash: 73d93622d418359be421e043d0af4e4befc6f4b4
ms.contentlocale: de-de
ms.lasthandoff: 09/09/2017

---

# <a name="running-jobs-on-azure-batch-with-azure-cli"></a>Ausführen von Aufträgen in Azure Batch mit der Azure-CLI

Dieses Skript erstellt einen Batch-Auftrag und fügt eine Reihe von Aufgaben zum Auftrag hinzu. Außerdem wird die Überwachung eines Auftrags und seiner Aufgaben veranschaulicht. Schließlich erfahren Sie, wie Sie den Batch-Dienst effizient abfragen, um Informationen über die Aufgaben von Aufträgen zu erhalten.

## <a name="prerequisites"></a>Voraussetzungen

- Installieren Sie Azure CLI gemäß den Anweisungen im [Azure CLI-Installationshandbuch](https://docs.microsoft.com/cli/azure/install-azure-cli), wenn dies noch nicht erfolgt ist.
- Erstellen Sie ein Batch-Konto, falls Sie noch keins besitzen. Ein Beispielskript zum Erstellen eines Kontos finden Sie unter [Erstellen eines Batch-Kontos mit der Azure-Befehlszeilenschnittstelle (CLI)](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account).
- Konfigurieren Sie eine Anwendung für die Ausführung aus einer Startaufgabe, wenn dies noch nicht geschehen ist. Unter [Hinzufügen von Anwendungen in Azure Batch mit der Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) finden Sie ein Beispielskript, das eine Anwendung erstellt und ein Anwendungspaket auf Azure hochlädt.
- Konfigurieren Sie einen Pool, in dem der Auftrag ausgeführt wird. Unter [Verwalten von Azure Batch-Pools mit der Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/azure/batch/batch-cli-sample-manage-pool) finden Sie ein Beispielskript, das einen Pool entweder mit einer Clouddienstkonfiguration oder einer virtuellen Computerkonfiguration erstellt.

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/batch/run-job/run-job.sh "Ausführen eines Auftrags")]

## <a name="clean-up-job"></a>Bereinigungsauftrag

Nachdem Sie das oben aufgeführte Beispielskript ausgeführt haben, entfernen Sie den Auftrag und alle seine Aufgaben mit dem folgenden Befehl. Beachten Sie, dass der Pool separat gelöscht werden muss. Weitere Informationen zum Erstellen und Löschen von Pools finden Sie unter [Verwalten von Azure Batch-Pools mit der Azure-Befehlszeilenschnittstelle](./batch-cli-sample-manage-pool.md).

```azurecli
az batch job delete --job-id myjob
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Erstellen eines Batch-Auftrags und der zugehörigen Aufgaben. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az batch account login](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_login) | Authentifizierung für ein Batch-Konto.  |
| [az batch job create](https://docs.microsoft.com/cli/azure/batch/job#az_batch_job_create) | Erstellt einen Batch-Auftrag.  |
| [az batch job set](https://docs.microsoft.com/cli/azure/batch/job#az_batch_job_set) | Aktualisiert die Eigenschaften eines Batch-Auftrags.  |
| [az batch job show](https://docs.microsoft.com/cli/azure/batch/job#az_batch_job_show) | Ruft Details eines angegebenen Batch-Auftrags ab.  |
| [az batch task create](https://docs.microsoft.com/cli/azure/batch/task#az_batch_task_create) | Fügt eine Aufgabe zum angegebenen Batch-Auftrag hinzu.  |
| [az batch task show](https://docs.microsoft.com/cli/azure/batch/task#az_batch_task_show) | Ruft die Details einer Aufgabe aus dem angegebenen Batch-Auftrag ab.  |
| [az batch task list](https://docs.microsoft.com/cli/azure/batch/task#az_batch_task_list) | Listet die dem angegebenen Auftrag zugeordneten Aufgaben auf.  |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche Batch-CLI-Skriptbeispiele finden Sie in der [Dokumentation zur Azure Batch-CLI](../batch-cli-samples.md).

