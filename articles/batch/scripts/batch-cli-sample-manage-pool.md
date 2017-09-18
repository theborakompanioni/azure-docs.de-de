---
title: 'Azure CLI-Skriptbeispiel: Verwalten von Pools in Batch | Microsoft-Dokumentation'
description: 'Azure CLI-Skriptbeispiel: Verwalten von Pools in Batch'
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
ms.openlocfilehash: ae7eab97c1da1113b0248b74a9dd67de8ce49e36
ms.contentlocale: de-de
ms.lasthandoff: 09/09/2017

---

# <a name="managing-azure-batch-pools-with-azure-cli"></a>Verwalten von Azure Batch-Pools mit der Azure-Befehlszeilenschnittstelle

Diese Skripts veranschaulichen einige der in der Azure-CLI verfügbaren Tools zum Erstellen und Verwalten von Computeknotenpools im Azure Batch-Dienst.

> [!NOTE]
> Mit den Befehlen in diesem Beispiel werden virtuelle Computer in Azure erstellt. Durch das Ausführen von virtuellen Computern fallen Gebühren für Ihr Konto an. Um diese Gebühren zu minimieren, löschen Sie den virtuellen Computer, sobald Sie das Beispiel ausgeführt haben. Weitere Informationen finden Sie unter [Bereinigen von Pools](#clean-up-pools).

Batch-Pools können auf zwei Arten konfiguriert werden: entweder als Cloud Services- (nur Windows) oder VM-Konfiguration (Windows und Linux) . Die Beispielskripts unten veranschaulichen das Erstellen von Pools mit beiden Konfigurationen.

## <a name="prerequisites"></a>Voraussetzungen

- Installieren Sie Azure CLI gemäß den Anweisungen im [Azure CLI-Installationshandbuch](https://docs.microsoft.com/cli/azure/install-azure-cli), wenn dies noch nicht erfolgt ist.
- Erstellen Sie ein Batch-Konto, falls Sie noch keins besitzen. Ein Beispielskript zum Erstellen eines Kontos finden Sie unter [Erstellen eines Batch-Kontos mit der Azure-Befehlszeilenschnittstelle (CLI)](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account).
- Konfigurieren Sie eine Anwendung für die Ausführung aus einer Startaufgabe, wenn dies noch nicht geschehen ist. Unter [Hinzufügen von Anwendungen in Azure Batch mit der Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) finden Sie ein Beispielskript, das eine Anwendung erstellt und ein Anwendungspaket auf Azure hochlädt.

## <a name="pool-with-cloud-service-configuration-sample-script"></a>Beispielskript für einen Pool mit Cloud Services-Konfiguration

[!code-azurecli[main](../../../cli_scripts/batch/manage-pool/manage-pool-windows.sh "Verwalten von Cloud Services-Pools")]

## <a name="pool-with-virtual-machine-configuration-sample-script"></a>Beispielskript für einen Pool mit VM-Konfiguration

[!code-azurecli[main](../../../cli_scripts/batch/manage-pool/manage-pool-linux.sh "Verwalten von VM-Pools")]

## <a name="clean-up-pools"></a>Bereinigen von Pools

Nachdem Sie das oben aufgeführte Beispielskript ausgeführt haben, löschen Sie die Pools mit dem folgenden Befehl.
```azurecli
az batch pool delete --pool-id mypool-windows
az batch pool delete --pool-id mypool-linux
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Erstellen und Bearbeiten von Batch-Pools.
Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az batch account login](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_login) | Authentifizierung für ein Batch-Konto.  |
| [az batch application summary list](https://docs.microsoft.com/cli/azure/batch/application/summary#az_batch_application_summary_list) | Listet die verfügbaren Anwendungen im Batch-Konto auf.  |
| [az batch pool create](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_create) | Erstellt einen Pool von virtuellen Computern.  |
| [az batch pool set](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_set) | Aktualisiert die Eigenschaften eines Pools.  |
| [az batch pool node-agent-skus list](https://docs.microsoft.com/cli/azure/batch/pool/node-agent-skus#az_batch_pool_node_agent_skus_list) | Listet verfügbaren Knoten-Agent-SKUs und Imageinformationen auf.  |
| [az batch pool resize](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_resize) | Ändern Sie die Anzahl der ausgeführten virtuellen Computer im angegebenen Pool.  |
| [az batch pool show](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_show) | Zeigt die Eigenschaften eines Pools an.  |
| [az batch pool delete](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_delete) | Löscht den angegebenen Pool.  |
| [az batch pool autoscale enable](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#az_batch_pool_autoscale_enable) | Aktiviert die automatische Skalierung für einen Pool, und wendet eine Formel an.  |
| [az batch pool autoscale disable](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#az_batch_pool_autoscale_disable) | Deaktiviert die automatische Skalierung für einen Pool.  |
| [az batch node list](https://docs.microsoft.com/cli/azure/batch/node#az_batch_node_list) | Listet die Computeknoten im angegebenen Pool auf.  |
| [az batch node reboot](https://docs.microsoft.com/cli/azure/batch/node#az_batch_node_reboot) | Startet den angegebenen Computeknoten neu.  |
| [az batch node delete](https://docs.microsoft.com/cli/azure/batch/node#az_batch_node_delete) | Löscht die aufgelisteten Knoten aus dem angegebenen Pool.  |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche Batch-CLI-Skriptbeispiele finden Sie in der [Dokumentation zur Azure Batch-CLI](../batch-cli-samples.md).


