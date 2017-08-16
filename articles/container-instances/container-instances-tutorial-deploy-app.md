---
title: "Tutorial für Azure Container Instances – Bereitstellungs-App | Microsoft-Dokumentation"
description: "Tutorial für Azure Container Instances – Bereitstellungs-App"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: seanmck
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: f5c887487ab74934cb65f9f3fa512baeb5dcaf2f
ms.openlocfilehash: d8c2056734bc1fdea71543157debd089a9ca743d
ms.contentlocale: de-de
ms.lasthandoff: 08/08/2017

---

# <a name="deploy-a-container-to-azure-container-instances"></a>Bereitstellen eines Containers für Azure Container Instances

Dies ist der letzte Teil eines dreiteiligen Tutorials. In vorhergehenden Abschnitten wurde [ein Containerimage erstellt](container-instances-tutorial-prepare-app.md) und [per Push an eine Azure Container Registry übermittelt](container-instances-tutorial-prepare-acr.md). Mit diesem Abschnitt wird das Tutorial abgeschlossen, indem der Container in Azure Container Instances bereitgestellt wird. Folgende Schritte werden ausgeführt:

> [!div class="checklist"]
> * Definieren einer Containergruppe mit einer Azure Resource Manager-Vorlage
> * Bereitstellen einer Containergruppe mithilfe der Azure-Befehlszeilenschnittstelle
> * Anzeigen von Containerprotokollen

## <a name="deploy-the-container-using-the-azure-cli"></a>Bereitstellen des Containers mithilfe der Azure-Befehlszeilenschnittstelle

Mithilfe der Azure-Befehlszeilenschnittstelle kann ein Container mit einem einzelnen Befehl in Azure Container Instances bereitgestellt werden. Da das Containerimage in der privaten Azure Container Registry gehostet wird, müssen Sie die erforderlichen Anmeldeinformationen einschließen, um darauf zugreifen zu können. Bei Bedarf können Sie diese abfragen wie weiter unten dargestellt.

Anmeldeserver für die Containerregistrierung (durch den Namen Ihrer Registrierung ersetzen):

```azurecli-interactive
az acr show --name <acrName> --query loginServer
```

Kennwort der Containerregistrierung:

```azurecli-interactive
az acr credential show --name <acrName> --query "passwords[0].value"
```

Um das Containerimage aus der Containerregistrierung mit einer Ressourcenanforderung von 1 CPU-Kern und 1 GB Speicher bereitzustellen, führen Sie den folgenden Befehl aus:

```azurecli-interactive
az container create --name aci-tutorial-app --image <acrLoginServer>/aci-tutorial-app:v1 --cpu 1 --memory 1 --registry-password <acrPassword> --ip-address public -g myResourceGroup
```

Innerhalb weniger Sekunden erhalten Sie eine erste Antwort von Azure Resource Manager. Verwenden Sie zum Anzeigen des Status der Bereitstellung Folgendes:

```azurecli-interactive
az container show --name aci-tutorial-app --resource-group myResourceGroup --query state
```

Sie können den Befehl weiter ausführen, bis sich der Status von *Ausstehend* in *Wird ausgeführt* ändert. Dann können Sie fortfahren.

## <a name="view-the-application-and-container-logs"></a>Anzeigen der Anwendungs- und Containerprotokolle

Öffnen Sie nach der erfolgreichen Bereitstellung im Browser die IP-Adresse, die in der Ausgabe des folgenden Befehls angezeigt wird:

```bash
az container show --name aci-tutorial-app --resource-group myResourceGroup --query ipAddress.ip
```

```json
"13.88.176.27"
```

![„Hello World“-Anwendung im Browser][aci-app-browser]

Sie können auch die Protokollausgabe des Containers anzeigen:

```azurecli-interactive
az container logs --name aci-tutorial-app -g myResourceGroup
```

Ausgabe:

```bash
listening on port 80
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://13.88.176.27/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Ihre Container in Azure Container Instances bereitgestellt. Die folgenden Schritte wurden ausgeführt:

> [!div class="checklist"]
> * Bereitstellen des Containers aus der Azure Container Registry mithilfe der Azure-Befehlszeilenschnittstelle
> * Anzeigen der Anwendung im Browser
> * Anzeigen der Containerprotokolle

<!-- LINKS -->
[prepare-app]: ./container-instances-tutorial-prepare-app.md

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png

