---
title: "Azure CLI-Skriptbeispiel – Erstellen eines Batch-Kontos | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Erstellen eines Batch-Kontos"
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
ms.openlocfilehash: fd2f4682a04c557b69bbfce115f41c54a96d462c
ms.contentlocale: de-de
ms.lasthandoff: 09/09/2017

---

# <a name="create-a-batch-account-with-the-azure-cli"></a>Erstellen eines Batch-Kontos mit der Azure-Befehlszeilenschnittstelle (CLI)

Dieses Skript erstellt ein Azure Batch-Konto und zeigt, wie verschiedene Eigenschaften des Kontos abgefragt und aktualisiert werden können.

## <a name="prerequisites"></a>Voraussetzungen

Installieren Sie Azure CLI mithilfe der Anweisungen im [Azure CLI-Installationshandbuch](https://docs.microsoft.com/cli/azure/install-azure-cli), wenn dies noch nicht erfolgt ist.

## <a name="batch-account-sample-script"></a>Beispielskript für ein Batch-Konto

Wenn Sie ein Batch-Konto erstellen, werden die Computeknoten standardmäßig intern vom Batch-Dienst zugewiesen. Zugeordnete Computeknoten unterliegen einem separaten Kernkontingent, und das Konto kann entweder über Anmeldeinformationen eines gemeinsam verwendeten Schlüssels oder ein Azure Active Dirctory-Token authentifiziert werden.

[!code-azurecli[main](../../../cli_scripts/batch/create-account/create-account.sh "Erstellen eines Kontos")]

## <a name="batch-account-using-user-subscription-sample-script"></a>Beispielskript für ein Batch-Konto mit Benutzerabonnement

Sie können die Computeknoten auch von Batch selbst in Ihrem eigenen Azure-Abonnement erstellen lassen.
Konten, die Computeknoten in Ihrem Abonnement zuordnen, müssen über ein Azure Active Directory-Token authentifiziert werden, und die zugeordneten Computeknoten werden auf Ihr Abonnementkontingent angerechnet. Um ein Konto in diesem Modus zu erstellen, muss eine Schlüsseltresorreferenz angegeben werden.

[!code-azurecli[main](../../../cli_scripts/batch/create-account/create-account-user-subscription.sh  "Erstellen eines Kontos mit Benutzerabonnement")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Nachdem Sie eines der oben gezeigten Beispielskripts ausgeführt haben, entfernen Sie mit dem folgenden Befehl die Ressourcengruppe und alle zugehörigen Ressourcen (einschließlich Batch-Konten, Azure-Speicherkonten und Azure-Schlüsseltresore).

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, ein Batch-Konto und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az batch account create](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_create) | Erstellt das Batch-Konto.  |
| [az batch account set](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_set) | Aktualisiert die Eigenschaften des Batch-Kontos.  |
| [az batch account show](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_show) | Ruft Details zum angegebenen Batch-Konto ab.  |
| [az batch account keys list](https://docs.microsoft.com/cli/azure/batch/account/keys#az_batch_account_keys_list) | Ruft die Zugriffsschlüssel zum angegebenen Batch-Konto ab.  |
| [az batch account login](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_login) | Authentifiziert das angegebene Batch-Konto zur weiteren CLI-Interaktion.  |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_create) | Erstellt ein Speicherkonto. |
| [az keyvault create](https://docs.microsoft.com/cli/azure/keyvault#az_keyvault_create) | Erstellt einen Schlüsseltresor. |
| [az keyvault set-policy](https://docs.microsoft.com/cli/azure/keyvault#az_keyvault_set_policy) | Aktualisiert die Sicherheitsrichtlinie des angegebenen Schlüsseltresors. |
| [az group delete](https://docs.microsoft.com/cli/azure/group#az_group_delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche Batch-CLI-Skriptbeispiele finden Sie in der [Dokumentation zur Azure Batch-CLI](../batch-cli-samples.md).

