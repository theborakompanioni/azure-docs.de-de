---
title: "Azure CLI-Skriptbeispiel – Binden eines benutzerdefinierten SSL-Zertifikats an eine Funktionen-App | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Binden eines benutzerdefinierten SSL-Zertifikats an eine Funktionen-App in Azure"
services: functions
documentationcenter: 
author: ggailey777
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: eb95d350-81ea-4145-a1e2-6eea3b7469b2
ms.service: functions
ms.workload: na
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 04/10/2017
ms.author: glenga
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: a0d0bd6af950cf7c70560db4c4d81789bb5c12f6
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---
# <a name="bind-a-custom-ssl-certificate-to-a-function-app"></a>Binden eines benutzerdefinierten SSL-Zertifikats an eine Funktionen-App

Dieses Skriptbeispiel erstellt eine Funktionen-App und die zugehörigen Ressourcen in App Service und bindet dann das SSL-Zertifikat eines benutzerdefinierten Domänennamens an die App. Für dieses Beispiel benötigen Sie Folgendes:

* Zugriff auf die Seite „DNS-Konfiguration“ der Domänenregistrierungsstelle.
* Eine gültige PFX-Datei und das zugehörige Kennwort für das SSL-Zertifikat, das Sie hochladen und binden möchten.

Um ein SSL-Zertifikat zu binden, muss Ihre Funktionen-App in einem App Service-Plan und nicht in einem Verbrauchsplan erstellt werden.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/azure-functions/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Binden eines benutzerdefinierten SSL-Zertifikats an eine Web-App")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Erstellt einen App Service-Plan, der zum Binden von SSL-Zertifikaten erforderlich ist. |
| [az functionapp create]() | Erstellt eine Funktionen-App. |
| [az appservice web config hostname add](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#add) | Ordnet eine benutzerdefinierte Domäne zur Funktionen-App zu. |
| [az appservice web config ssl upload](https://docs.microsoft.com/cli/azure/appservice/web/config/ssl#upload) | Lädt ein benutzerdefiniertes SSL-Zertifikat in eine Funktionen-App hoch. |
| [az appservice web config ssl bind](https://docs.microsoft.com/en-us/cli/azure/appservice/web/config/ssl#bind) | Bindet ein hochgeladenes SSL-Zertifikat an eine Funktionen-App. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche App Service-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure App Service]().
