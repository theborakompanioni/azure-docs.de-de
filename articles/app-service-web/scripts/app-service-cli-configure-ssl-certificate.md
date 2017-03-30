---
title: "Azure CLI-Skriptbeispiel – Binden eines benutzerdefinierten SSL-Zertifikats an eine Web-App | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Binden eines benutzerdefinierten SSL-Zertifikats an eine Web-App"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: eb95d350-81ea-4145-a1e2-6eea3b7469b2
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 115749f29d73efdb81f491d9f31687b097f6d270
ms.lasthandoff: 03/21/2017

---

# <a name="bind-a-custom-ssl-certificate-to-a-web-app"></a>Binden eines benutzerdefinierten SSL-Zertifikats an eine Web-App

Dieses Skriptbeispiel erstellt eine Web-App und die zugehörigen Ressourcen in App Service und bindet dann das SSL-Zertifikat eines benutzerdefinierten Domänennamens an die App. 

Installieren Sie bei Bedarf die Azure-Befehlszeilenschnittstelle anhand der Anleitung im [Azure CLI-Installationshandbuch](https://docs.microsoft.com/cli/azure/install-azure-cli). Stellen Sie darüber hinaus Folgendes sicher:

- Eine Verbindung mit Azure wurde mit dem Befehl `az login` hergestellt.
- Sie haben Zugriff auf die Seite „DNS-Konfiguration“ der Domänenregistrierungsstelle.
- Sie verfügen über eine gültige PFX-Datei und das zugehörige Kennwort für das SSL-Zertifikat, das Sie hochladen und binden möchten.

Dieses Beispiel wird in einer Bash-Shell ausgeführt. Optionen zum Ausführen von Azure CLI-Skripts auf einem Windows-Client finden Sie unter [Verwenden der Azure CLI unter Windows](../../virtual-machines/virtual-machines-windows-cli-options.md).

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Binden eines benutzerdefinierten SSL-Zertifikats an eine Web-App")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Erstellt einen App Service-Plan. |
| [az appservice web create](https://docs.microsoft.com/cli/azure/appservice/web#delete) | Erstellt eine Azure-Web-App. |
| [az appservice web config hostname add](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#add) | Aktualisiert einen App Service-Plan, um den zugehörigen Tarif zu skalieren. |
| [az appservice web config ssl upload](https://docs.microsoft.com/cli/azure/appservice/web/config/ssl#upload) | Hochladen eines benutzerdefinierten SSL-Zertifikats zu einer Web-App |
| [az appservice web config ssl bind](https://docs.microsoft.com/en-us/cli/azure/appservice/web/config/ssl#bind) | Binden eines hochgeladenen SSL-Zertifikats an eine Web-App |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche App Service-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure App Service](../app-service-cli-samples.md).
