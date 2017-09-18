---
title: "Azure CLI-Skriptbeispiel – Binden eines benutzerdefinierten SSL-Zertifikats an eine Funktionen-App | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Binden eines benutzerdefinierten SSL-Zertifikats an eine Funktionen-App in Azure"
services: functions
documentationcenter: 
author: ggailey777
manager: cfowler
editor: 
tags: azure-service-management
ms.assetid: eb95d350-81ea-4145-a1e2-6eea3b7469b2
ms.service: functions
ms.workload: na
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 04/10/2017
ms.author: glenga
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 190ca4b228434a7d1b30348011c39a979c22edbd
ms.openlocfilehash: 76602f63f8b7fd7adc38353756387484f0af6493
ms.contentlocale: de-de
ms.lasthandoff: 09/09/2017

---
# <a name="bind-a-custom-ssl-certificate-to-a-function-app"></a>Binden eines benutzerdefinierten SSL-Zertifikats an eine Funktionen-App

Dieses Skriptbeispiel erstellt eine Funktionen-App und die zugehörigen Ressourcen in App Service und bindet dann das SSL-Zertifikat eines benutzerdefinierten Domänennamens an die App. Für dieses Beispiel benötigen Sie Folgendes:

* Zugriff auf die Seite „DNS-Konfiguration“ der Domänenregistrierungsstelle.
* Eine gültige PFX-Datei und das zugehörige Kennwort für das SSL-Zertifikat, das Sie hochladen und binden möchten.

Um ein SSL-Zertifikat zu binden, muss Ihre Funktionen-App in einem App Service-Plan und nicht in einem Verbrauchsplan erstellt werden.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Thema die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Binden eines benutzerdefinierten SSL-Zertifikats an eine Web-App")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Erstellt einen App Service-Plan, der zum Binden von SSL-Zertifikaten erforderlich ist. |
| [az functionapp create]() | Erstellt eine Funktionen-App. |
| [az appservice web config hostname add](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#az_appservice_web_config_hostname_add) | Ordnet eine benutzerdefinierte Domäne zur Funktionen-App zu. |
| [az appservice web config ssl upload](https://docs.microsoft.com/cli/azure/appservice/web/config/ssl#az_appservice_web_config_ssl_upload) | Lädt ein benutzerdefiniertes SSL-Zertifikat in eine Funktionen-App hoch. |
| [az appservice web config ssl bind](https://docs.microsoft.com/en-us/cli/azure/appservice/web/config/ssl#az_appservice_web_config_ssl_bind) | Bindet ein hochgeladenes SSL-Zertifikat an eine Funktionen-App. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche App Service-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure App Service]().

