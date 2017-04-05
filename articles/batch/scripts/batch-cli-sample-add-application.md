---
title: "Azure CLI-Skriptbeispiel: Hinzufügen einer Anwendung in Batch | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel: Hinzufügen einer Anwendung in Batch"
services: batch
documentationcenter: 
author: annatisch
manager: daryls
editor: tysonn
ms.assetid: 
ms.service: batch
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/20/2017
ms.author: antisch
translationtype: Human Translation
ms.sourcegitcommit: 0bec803e4b49f3ae53f2cc3be6b9cb2d256fe5ea
ms.openlocfilehash: 342cc2c93304f7a8f651769139e28ebf2e30d412
ms.lasthandoff: 03/24/2017

---

# <a name="adding-applications-to-azure-batch-with-azure-cli"></a>Hinzufügen von Anwendungen in Azure Batch mit der Azure-Befehlszeilenschnittstelle

Dieses Skript veranschaulicht die Einrichtung einer Anwendung für die Verwendung mit einem Azure Batch-Pool oder -Task. Für das Einrichten einer Anwendung fassen Sie die ausführbare Datei mit sämtlichen Abhängigkeiten in einer ZIP-Datei zusammen. In diesem Beispiel hat die ausführbare ZIP-Datei den Namen „my-application-exe.zip“.
Bei der Ausführung des Skripts wird davon ausgegangen, dass bereits ein Batch-Konto eingerichtet wurde. Weitere Informationen finden Sie im [Beispielskript für das Erstellen eines Batch-Kontos](./batch-cli-sample-create-account.md).

Installieren Sie bei Bedarf die Azure-Befehlszeilenschnittstelle anhand der Anweisungen im [Azure CLI-Installationshandbuch](https://docs.microsoft.com/cli/azure/install-azure-cli), und führen Sie dann `az login` aus, um sich bei Azure anzumelden.

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/batch/add-application/add-application.sh "Anwendung hinzufügen")]

## <a name="clean-up-application"></a>Bereinigen der Anwendung

Nachdem Sie das oben aufgeführte Beispielskript ausgeführt haben, führen Sie die folgenden Befehle aus, um die Anwendung und alle hochgeladenen Anwendungspakete zu entfernen.

```azurecli
az batch application package delete -g myresourcegroup -n mybatchaccount --application-id myapp --version 1.0 --yes
az batch application delete -g myresourcegroup -n mybatchaccount --application-id myapp --yes
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle für das Erstellen einer Anwendung und das Hochladen eines Anwendungspakets.
Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az batch application create](https://docs.microsoft.com/cli/azure/batch/application#create) | Erstellt eine Anwendung  |
| [az batch application set](https://docs.microsoft.com/cli/azure/batch/application#set) | Aktualisiert die Eigenschaften einer Anwendung  |
| [az batch application package create](https://docs.microsoft.com/cli/azure/batch/application/package#create) | Fügt der angegebenen Anwendung ein Anwendungspaket hinzu  |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche Batch-CLI-Skriptbeispiele finden Sie in der [Dokumentation zur Azure Batch-CLI](../batch-cli-samples.md).

