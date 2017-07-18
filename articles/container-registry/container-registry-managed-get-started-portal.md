---
title: "Erstellen einer privaten Docker-Registrierung – Azure-Portal | Microsoft-Dokumentation"
description: Erste Schritte beim Erstellen und Verwalten von privaten Docker-Containerregistrierungen mit dem Azure-Portal
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: na
tags: 
keywords: 
ms.assetid: 53a3b3cb-ab4b-4560-bc00-366e2759f1a1
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: nepeters
ms.custom: na
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: 560aee42b0c5a61c37c594d7937f833ab7183d49
ms.contentlocale: de-de
ms.lasthandoff: 07/12/2017

---

# <a name="create-a-managed-container-registry-using-the-azure-portal"></a>Erstellen einer verwalteten Containerregistrierung über das Azure-Portal

Azure Container Registry ist ein verwalteter Docker-Containerregistrierungsdienst zum Speichern privater Docker-Containerimages. In diesem Leitfaden erfahren Sie, wie Sie über das Azure-Portal eine verwaltete Azure Container Registry-Instanz erstellen.

Verwaltete Azure-Containerregistrierungen befinden sich derzeit in der Vorschauphase und stehen nicht in allen Regionen zur Verfügung.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich unter „http://portal.azure.com“ beim Azure-Portal an.

## <a name="create-a-container-registry"></a>Erstellen einer Containerregistrierung

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.

2. Durchsuchen Sie den Marketplace nach **Azure Container Registry**, und wählen Sie es aus.

3. Klicken Sie auf **Erstellen**. Daraufhin öffnet sich das Blatt für die ACR-Erstellung.

    ![Einstellungen der Containerregistrierung](./media/container-registry-get-started-portal/managed-container-registry-settings.png)

4. Geben Sie auf dem Blatt **Azure-Containerregistrierung** die folgenden Informationen ein. Klicken Sie auf **Erstellen**, wenn Sie fertig sind.

    a. **Registrierungsname**: Ein global eindeutiger Name einer Domäne der obersten Ebene für Ihre jeweilige Registrierung. In diesem Beispiel lautet der Registrierungsname *myAzureContainerRegistry1*. Ersetzen Sie ihn durch einen eigenen eindeutigen Namen. Der Name darf nur Buchstaben und Ziffern enthalten.

    b. **Ressourcengruppe**: Wählen Sie eine vorhandene [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md#resource-groups) aus, oder geben Sie den Namen für eine neue Ressourcengruppe ein.

    c. **Standort**: Wählen Sie den Standort eines Azure-Datencenters aus, für den der Dienst [verfügbar](https://azure.microsoft.com/regions/services/) ist, z.B. **USA, Süden-Mitte**.

    d. **Administratorbenutzer**: Bei Bedarf können Sie einem Administratorbenutzer Zugriff auf die Registrierung gewähren. Sie können diese Einstellung ändern, nachdem Sie die Registrierung erstellt haben.

    e. **Verwaltete Registrierung verwenden**: Wählen Sie „Ja“ aus, damit ACR automatisch den Registrierungsspeicher verwaltet, Webhooks verwendet und die AAD-Authentifizierung verwendet.

    f. **Tarif**: Wählen Sie einen Tarif aus. Weitere Informationen finden Sie im Abschnitt mit den ACR-Preisen.

## <a name="log-in-to-acr-instance"></a>Anmelden bei der ACR-Instanz

Bevor Sie Push- und Pullvorgänge für Containerimages ausführen können, müssen Sie sich bei der ACR-Instanz anmelden. 

Verwenden Sie hierzu Azure CLI 2.0. Melden Sie sich bei Bedarf zunächst mit dem Befehl [az login](/cli/azure/#login) bei Azure an. 

```azurecli
az login
```

Verwenden Sie als Nächstes den Befehl [az acr login](/cli/azure/acr#login), um sich bei Azure Container Registry anzumelden.

```azurecli-interactive
az acr login --name myAzureContainerRegistry1
```

## <a name="use-azure-container-registry"></a>Verwenden von Azure Container Registry

### <a name="list-container-images"></a>Auflisten von Containerimages

Verwenden Sie die CLI-Befehle vom Typ `az acr`, um die Images und Tags in einem Repository abzufragen.

> [!NOTE]
> Derzeit wird der Befehl `docker search` zum Abfragen von Images und Tags für die Containerregistrierung nicht unterstützt.

### <a name="list-repositories"></a>Auflisten von Repositorys

Im folgenden Beispiel sind die Repositorys in einer Registrierung im JSON-Format (JavaScript Object Notation) aufgelistet:

```azurecli
az acr repository list -n myContainerRegistry1 -o json
```

### <a name="list-tags"></a>Auflisten von Tags

Im folgenden Beispiel sind die Tags im Repository **samples/nginx** im JSON-Format aufgelistet:

```azurecli
az acr repository show-tags -n myContainerRegistry1 --repository samples/nginx -o json
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie über das Azure-Portal eine verwaltete Azure Container Registry-Instanz erstellt.

> [!div class="nextstepaction"]
> [Freigeben Ihres ersten Image mit der Docker CLI](container-registry-get-started-docker-cli.md)
