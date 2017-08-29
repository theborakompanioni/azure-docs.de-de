---
title: "Azure CLI-Skriptbeispiel – Erstellen einer Funktionen-App für die serverlose Ausführung | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Erstellen einer Funktionen-App für die serverlose Ausführung"
services: functions
documentationcenter: functions
author: syntaxc4
manager: cfowler
editor: 
tags: azure-service-management
ms.assetid: 0e221db6-ee2d-4e16-9bf6-a456cd05b6e7
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 04/11/2017
ms.author: cfowler
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 4f68f90c3aea337d7b61b43e637bcfda3c98f3ea
ms.openlocfilehash: 37046967bd5ab0d797d1c66690db7200fb4805e2
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017

---

# <a name="create-a-function-app-for-serverless-execution"></a>Erstellen einer Funktionen-App für die serverlose Ausführung

Dieses Beispielskript erstellt eine Azure-Funktionen-App, die einen Container für Ihre Funktionen darstellt. Die Funktionen-App wird mithilfe des [Verbrauchsplans](../functions-scale.md#consumption-plan) erstellt, der sich ideal für ereignisgesteuerte serverlose Workloads eignet.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Thema die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

## <a name="sample-script"></a>Beispielskript

Dieses Skript erstellt eine Azure-Funktionen-App mithilfe des [Verbrauchsplans](../functions-scale.md#consumption-plan).

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-consumption/create-function-app-consumption.sh "Erstellen einer Azure-Funktion in einem Verbrauchsplan")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft. Das Skript verwendet die folgenden Befehle:

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az storage account create](/cli/azure/storage/account#create) | Erstellt ein Azure Storage-Konto. |
| [az functionapp create](https://docs.microsoft.com/cli/azure/functionapp#create) | Erstellt eine Azure-Funktion. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Weitere Azure Functions-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Functions](../functions-cli-samples.md).

