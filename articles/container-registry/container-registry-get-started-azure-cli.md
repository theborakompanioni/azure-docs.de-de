---
title: "Erstellen einer privaten Docker-Containerregistrierung – Azure CLI | Microsoft-Dokumentation"
description: Erste Schritte beim Erstellen und Verwalten von privaten Docker-Containerregistrierungen mit der Azure CLI 2.0
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: cristyg
tags: 
keywords: 
ms.assetid: 29e20d75-bf39-4f7d-815f-a2e47209be7d
ms.service: container-registry
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/06/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 80be19618bd02895d953f80e5236d1a69d0811af
ms.openlocfilehash: 99bb3db7cc80e8426e1dca14bc3d733ee6c7342c
ms.contentlocale: de-de
ms.lasthandoff: 06/07/2017

---
# <a name="create-a-private-docker-container-registry-using-the-azure-cli-20"></a>Erstellen einer privaten Docker-Containerregistrierung mit der Azure CLI 2.0
Verwenden Sie Befehle von [Azure-CLI 2.0](https://github.com/Azure/azure-cli), um eine Containerregistrierung zu erstellen und die Einstellungen dafür über Ihren Linux-, Macintosh- oder Windows-Computer zu verwalten. Sie können Containerregistrierungen auch mit dem [Azure-Portal](container-registry-get-started-portal.md) oder programmgesteuert mit der [REST-API](https://go.microsoft.com/fwlink/p/?linkid=834376) für die Containerregistrierung erstellen und verwalten.


* Hintergrundinformationen und Konzepte finden Sie in der [Übersicht](container-registry-intro.md).
* Übergeben Sie den Parameter `-h` für einen beliebigen Befehl, um Hilfe zu CLI-Befehlen für die Containerregistrierung (`az acr`-Befehle) zu erhalten.


## <a name="prerequisites"></a>Voraussetzungen
* **Azure CLI 2.0**: Informationen zur Installation und zu den ersten Schritten mit der CLI 2.0 finden Sie in der [Installationsanleitung](/cli/azure/install-azure-cli). Melden Sie sich an Ihrem Azure-Abonnement an, indem Sie `az login` ausführen. Weitere Informationen finden Sie unter [Get started with CLI 2.0](/cli/azure/get-started-with-azure-cli) (Erste Schritte mit der CLI 2.0).
* **Ressourcengruppe**: Erstellen Sie eine [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md#resource-groups), bevor Sie eine Containerregistrierung erstellen, oder verwenden Sie eine vorhandene Ressourcengruppe. Stellen Sie sicher, dass sich die Ressourcengruppe an einem Standort befindet, für den der Containerregistrierungsdienst [verfügbar](https://azure.microsoft.com/regions/services/) ist. Informationen zum Erstellen einer Ressourcengruppe mit der CLI 2.0 finden Sie in der [Referenz zu CLI 2.0](/cli/azure/group).
* **Speicherkonto** (optional): Erstellen Sie ein standardmäßiges Azure-[Speicherkonto](../storage/storage-introduction.md) zum Sichern der Containerregistrierung an demselben Standort. Wenn Sie beim Erstellen einer Registrierung mit `az acr create` kein Speicherkonto angeben, führt der Befehl dies für Sie durch. Informationen zum Erstellen eines Speicherkontos mit der CLI 2.0 finden Sie in der [Referenz zu CLI 2.0](/cli/azure/storage/account). Storage Premium wird derzeit nicht unterstützt.
* **Dienstprinzipal** (optional): Wenn Sie mit der CLI eine Registrierung erstellen, wird der Zugriff standardmäßig nicht eingerichtet. Je nach Ihren Anforderungen können Sie einen vorhandenen Azure Active Directory-Dienstprinzipal einer Registrierung zuweisen (oder einen neuen erstellen und zuweisen) oder das Administratorbenutzerkonto der Registrierung aktivieren. Informationen hierzu finden Sie in den Abschnitten weiter unten in diesem Artikel. Weitere Informationen zum Zugreifen auf die Registrierung finden Sie unter [Authenticate with the container registry](container-registry-authentication.md) (Authentifizieren bei der Containerregistrierung).

## <a name="create-a-container-registry"></a>Erstellen einer Containerregistrierung
Führen Sie den Befehl `az acr create` aus, um eine Containerregistrierung zu erstellen.

> [!TIP]
> Geben Sie beim Erstellen einer Registrierung einen global eindeutigen Namen einer Domäne der obersten Ebene an, der nur Buchstaben und Zahlen enthält. Der Registrierungsname in den Beispielen lautet `myRegistry1`. Ersetzen Sie ihn durch einen eigenen eindeutigen Namen.
>
>

Im folgenden Befehl werden die minimal erforderlichen Parameter zum Erstellen der Containerregistrierung `myRegistry1` in der Ressourcengruppe `myResourceGroup` mit dem *Basis*-SKU verwendet:

```azurecli
az acr create --name myRegistry1 --resource-group myResourceGroup --sku Basic
```

* `--storage-account-name` ist optional. Wenn nichts angegeben ist, wird in der angegebenen Ressourcengruppe ein Speicherkonto mit einem Namen erstellt, der aus dem Registrierungsnamen und einem Zeitstempel besteht.

Wenn die Registrierung erstellt wird, sieht die Ausgabe etwa wie folgt aus:

```azurecli
{
  "adminUserEnabled": false,
  "creationDate": "2017-06-06T18:36:29.124842+00:00",
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myResourceGroup/providers/Microsoft.ContainerRegistry
/registries/myRegistry1",
  "location": "southcentralus",
  "loginServer": "myregistry1.azurecr.io",
  "name": "myRegistry1",
  "provisioningState": "Succeeded",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "myregistry123456789"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}

```


Beachten Sie besonders Folgendes:

* `id`: Der Bezeichner für die Registrierung in Ihrem Abonnement, den Sie benötigen, wenn Sie einen Dienstprinzipal zuweisen möchten.
* `loginServer`: Der vollqualifizierte Name, den Sie zum [Anmelden an der Registrierung](container-registry-authentication.md) angeben. In diesem Beispiel lautet der Name `myregistry1.exp.azurecr.io` (nur Kleinbuchstaben).

## <a name="assign-a-service-principal"></a>Zuweisen eines Dienstprinzipals
Verwenden Sie die Befehle der CLI 2.0, um einen Azure Active Directory-Dienstprinzipal einer Registrierung zuzuweisen. Dem Dienstprinzipal wird in diesen Beispielen die Rolle „Besitzer“ zugewiesen, aber Sie können bei Bedarf auch [andere Rollen](../active-directory/role-based-access-control-configure.md) zuweisen.

### <a name="create-a-service-principal-and-assign-access-to-the-registry"></a>Erstellen eines Dienstprinzipals und Zuweisen des Zugriffs auf die Registrierung
Im folgenden Befehl wird einem neuen Dienstprinzipal über die Rolle „Besitzer“ der Zugriff auf den Registrierungsbezeichner zugewiesen, der mit dem Parameter `--scopes` übergeben wird. Geben Sie mit dem Parameter `--password` ein sicheres Kennwort an.

```azurecli
az ad sp create-for-rbac --scopes /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry1 --role Owner --password myPassword
```



### <a name="assign-an-existing-service-principal"></a>Zuweisen eines vorhandenen Dienstprinzipals
Wenn Sie bereits über einen Dienstprinzipal verfügen und dafür über die Rolle „Besitzer“ Zugriff auf die Registrierung zuweisen möchten, können Sie einen ähnlichen Befehl wie im folgenden Beispiel ausführen. Sie übergeben die Dienstprinzipal-App-ID mit dem Parameter `--assignee`:

```azurecli
az role assignment create --scope /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry1 --role Owner --assignee myAppId
```



## <a name="manage-admin-credentials"></a>Verwalten von Administratoranmeldeinformationen
Ein Administratorkonto wird automatisch für jede Containerregistrierung erstellt und ist standardmäßig deaktiviert. In den folgenden Beispielen werden CLI-Befehle vom Typ `az acr` verwendet, um die Administratoranmeldeinformationen für Ihre Containerregistrierung zu verwalten.

### <a name="obtain-admin-user-credentials"></a>Abrufen von Administratorbenutzer-Anmeldeinformationen
```azurecli
az acr credential show -n myRegistry1
```

### <a name="enable-admin-user-for-an-existing-registry"></a>Aktivieren des Administratorbenutzers für eine vorhandene Registrierung
```azurecli
az acr update -n myRegistry1 --admin-enabled true
```

### <a name="disable-admin-user-for-an-existing-registry"></a>Deaktivieren des Administratorbenutzers für eine vorhandene Registrierung
```azurecli
az acr update -n myRegistry1 --admin-enabled false
```

## <a name="list-images-and-tags"></a>Auflisten von Images und Tags
Verwenden Sie die CLI-Befehle vom Typ `az acr`, um die Images und Tags in einem Repository abzufragen.

> [!NOTE]
> Derzeit wird der Befehl `docker search` zum Abfragen von Images und Tags für die Containerregistrierung nicht unterstützt.


### <a name="list-repositories"></a>Auflisten von Repositorys
Im folgenden Beispiel sind die Repositorys in einer Registrierung im JSON-Format (JavaScript Object Notation) aufgelistet:

```azurecli
az acr repository list -n myRegistry1 -o json
```

### <a name="list-tags"></a>Auflisten von Tags
Im folgenden Beispiel sind die Tags im Repository **samples/nginx** im JSON-Format aufgelistet:

```azurecli
az acr repository show-tags -n myRegistry1 --repository samples/nginx -o json
```

## <a name="next-steps"></a>Nächste Schritte
* [Freigeben Ihres ersten Image mit der Docker CLI](container-registry-get-started-docker-cli.md)

