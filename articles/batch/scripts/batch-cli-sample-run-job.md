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
ms.date: 03/20/2017
ms.author: antisch
translationtype: Human Translation
ms.sourcegitcommit: 0bec803e4b49f3ae53f2cc3be6b9cb2d256fe5ea
ms.openlocfilehash: d5ef87e6e7092820a65c5736c1942fd5cec57462
ms.lasthandoff: 03/24/2017

---

# <a name="running-jobs-on-azure-batch-with-azure-cli"></a>Ausführen von Aufträgen in Azure Batch mit der Azure-CLI

Dieses Skript erstellt einen Batch-Auftrag und fügt eine Reihe von Aufgaben zum Auftrag hinzu. Außerdem wird die Überwachung eines Auftrags und seiner Aufgaben veranschaulicht.
Beim Ausführen des Skripts wird davon ausgegangen, dass bereits ein Batch-Konto eingerichtet wurde und sowohl ein Pool als auch eine Anwendung konfiguriert wurden. Weitere Informationen finden Sie in den [Beispielskripts](../batch-cli-samples.md) zu den einzelnen Themen.

Installieren Sie bei Bedarf die Azure-Befehlszeilenschnittstelle anhand der Anweisungen im [Azure CLI-Installationshandbuch](https://docs.microsoft.com/cli/azure/install-azure-cli), und führen Sie dann `az login` aus, um sich bei Azure anzumelden.

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/batch/run-job/run-job.sh "Ausführen eines Auftrags")]

## <a name="clean-up-job"></a>Bereinigungsauftrag

Nachdem Sie das oben aufgeführte Beispielskript ausgeführt haben, entfernen Sie den Auftrag und alle seine Aufgaben mit dem folgenden Befehl. Beachten Sie, dass der Pool separat gelöscht werden muss. Informationen hierzu finden Sie unter der [Tutorial zum Verwalten von Pools](./batch-cli-sample-manage-pool.md).

```azurecli
az batch job delete --job-id myjob
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Erstellen eines Batch-Auftrags und der zugehörigen Aufgaben. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az batch account login](https://docs.microsoft.com/cli/azure/batch/account#login) | Authentifizierung für ein Batch-Konto.  |
| [az batch job create](https://docs.microsoft.com/cli/azure/batch/job#create) | Erstellt einen Batch-Auftrag.  |
| [az batch job set](https://docs.microsoft.com/cli/azure/batch/job#set) | Aktualisiert die Eigenschaften eines Batch-Auftrags.  |
| [az batch job show](https://docs.microsoft.com/cli/azure/batch/job#show) | Ruft Details eines angegebenen Batch-Auftrags ab.  |
| [az batch task create](https://docs.microsoft.com/cli/azure/batch/task#create) | Fügt eine Aufgabe zum angegebenen Batch-Auftrag hinzu.  |
| [az batch task show](https://docs.microsoft.com/cli/azure/batch/task#show) | Ruft die Details einer Aufgabe aus dem angegebenen Batch-Auftrag ab.  |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche Batch-CLI-Skriptbeispiele finden Sie in der [Dokumentation zur Azure Batch-CLI](../batch-cli-samples.md).

