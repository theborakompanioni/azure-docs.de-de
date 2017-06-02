---
title: "Azure CLI-Skriptbeispiel – Erstellen einer Funktionen-App in einem App Service-Plan | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Erstellen einer Funktionen-App in einem App Service-Plan"
services: functions
documentationcenter: functions
author: syntaxc4
manager: erikre
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
ms.sourcegitcommit: 9568210d4df6cfcf5b89ba8154a11ad9322fa9cc
ms.openlocfilehash: 8c98e392c1c735458184bb782ed9c42f468f33de
ms.contentlocale: de-de
ms.lasthandoff: 05/15/2017

---
# <a name="create-a-function-app-in-an-app-service-plan"></a>Erstellen einer Funktionen-App in einem App Service-Plan

Dieses Beispielskript erstellt eine Azure-Funktionen-App, die einen Container für Ihre Funktionen darstellt. Die Funktionen-App wird mithilfe eines dedizierten App Service-Plans erstellt. Die Serverressourcen sind also immer aktiv.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Beispielskript

Dieses Skript erstellt eine Azure-Funktionen-App mithilfe eines dedizierten [App Service-Plans](../functions-scale.md#app-service-plan).

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-app-service-plan/create-function-app-app-service-plan.sh "Erstellen einer Azure-Funktion in einem App Service-Plan")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft. Das Skript verwendet die folgenden Befehle:

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#create) | Erstellt ein Azure Storage-Konto. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appserviceplan#create) | Erstellt einen App Service-Plan. |
| [az functionapp create](https://docs.microsoft.com/cli/azure/functionapp#delete) | Erstellt eine Azure-Funktionen-App. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Weitere Azure Functions-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Functions](../functions-cli-samples.md).

