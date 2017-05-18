---
title: "Azure CLI-Skriptbeispiel – Zuordnen einer benutzerdefinierten Domäne zu einer Web-App | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Zuordnen einer benutzerdefinierten Domäne zu einer Web-App"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 5ac4a680-cc73-4578-bcd6-8668c08802c2
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 04/09/2017
ms.author: cephalin
ms.translationtype: Human Translation
ms.sourcegitcommit: 9568210d4df6cfcf5b89ba8154a11ad9322fa9cc
ms.openlocfilehash: f3c8b50f14e21dfef6fec9aa34244f425808f1bc
ms.contentlocale: de-de
ms.lasthandoff: 05/15/2017

---
# <a name="map-a-custom-domain-to-a-web-app"></a>Zuordnen einer benutzerdefinierten Domäne zu einer Web-App

Dieses Beispielskript erstellt eine Web-App in App Service mit den zugehörigen Ressourcen und ordnet ihr dann `www.<yourdomain>` zu.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/configure-custom-domain/configure-custom-domain.sh?highlight=3 "Zuordnen einer benutzerdefinierten Domäne zu einer Web-App")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Erstellt einen App Service-Plan. |
| [az appservice web create](https://docs.microsoft.com/cli/azure/appservice/web#delete) | Erstellt die Web-App. |
| [az appservice web config hostname add](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#add) | Zuordnen einer benutzerdefinierten Domäne zu einer Web-App. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche App Service-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure App Service](../app-service-cli-samples.md).

