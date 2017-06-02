---
title: Erstellen einer Azure-Funktion, die eine Verbindung mit einem Azure-Speicher herstellt | Microsoft-Dokumentation
description: 'Azure CLI-Skriptbeispiel: Erstellen einer Azure-Funktion, die eine Verbindung mit Azure Storage herstellt'
services: functions
documentationcenter: functions
author: rachelappel
manager: erikre
editor: 
tags: functions
ms.assetid: 
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 04/20/2017
ms.author: rachelap
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 9568210d4df6cfcf5b89ba8154a11ad9322fa9cc
ms.openlocfilehash: 3925c1a3d2c5a9fc3a82e8823dbdeffabeb38a3a
ms.contentlocale: de-de
ms.lasthandoff: 05/15/2017

---
# <a name="integrate-function-app-into-azure-storage-account"></a>Integrieren der Funktionen-App in ein Azure-Speicherkonto

Dieses Beispielskript erstellt eine Funktionen-App und ein Speicherkonto.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Beispielskript

Dieses Beispiel erstellt eine Azure-Funktionen-App und fügt einer App-Einstellung die Speicher-Verbindungszeichenfolge hinzu.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-storage/create-function-app-connect-to-storage-account.sh "Integrieren der Funktionen-App in ein Azure-Speicherkonto")]


## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe, die App Service-App und alle zugehörigen Ressourcen entfernt werden:

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az login](https://docs.microsoft.com/cli/azure/#login) | Anmelden bei Azure. |
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellen Sie eine Ressourcengruppe mit einem Speicherort. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account) | Erstellen Sie ein Speicherkonto. |
| [az functionapp create](https://docs.microsoft.com/cli/azure/functionapp#create) | Erstellen Sie eine neue Funktionen-App. |
| [az group delete](https://docs.microsoft.com/cli/azure/group#delete) | Bereinigen |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Weitere Azure Functions-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Functions](../functions-cli-samples.md).

