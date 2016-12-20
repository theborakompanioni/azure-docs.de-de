---
title: Erstellen einer Containerregistrierung mit der Azure-Befehlszeilenschnittstelle | Microsoft Docs
description: Erste Schritte beim Erstellen und Verwalten von Azure-Containerregistrierungen mit der Azure-CLI 2.0 (Vorschau)
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: dlepow
tags: 
keywords: 
ms.assetid: 29e20d75-bf39-4f7d-815f-a2e47209be7d
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/14/2016
ms.author: stevelas
translationtype: Human Translation
ms.sourcegitcommit: aa4b960ed75b5a4702317bf557b4588e7a54fa0e
ms.openlocfilehash: d806564056390ea2db74eebbbe89e1b20a350f33

---
# <a name="create-a-container-registry-using-the-azure-cli"></a>Erstellen einer Containerregistrierung mit der Azure-Befehlszeilenschnittstelle
Verwenden Sie Befehle der [Azure-CLI 2.0 (Vorschau)](https://github.com/Azure/azure-cli), um eine Containerregistrierung zu erstellen und die Einstellungen dafür über Ihren Linux-, Macintosh- oder Windows-Computer zu verwalten. Sie können Containerregistrierungen auch mit dem [Azure-Portal](container-registry-get-started-portal.md) oder programmgesteuert mit der [REST-API](https://go.microsoft.com/fwlink/p/?linkid=834376) für die Containerregistrierung erstellen und verwalten.


* Hintergrundinformationen und Konzepte finden Sie unter [Was ist die Azure-Containerregistrierung?](container-registry-intro.md).
* Übergeben Sie den Parameter `-h` für einen beliebigen Befehl, um Hilfe zu CLI-Befehlen für die Containerregistrierung (`az acr`-Befehle) zu erhalten.

> [!NOTE]
> Die Containerregistrierung befindet sich derzeit in der Vorschauphase.
> 
> 

## <a name="prerequisites"></a>Voraussetzungen
* **Azure-CLI 2.0 (Vorschau)**: Informationen zur Installation und den ersten Schritten mit der CLI 2.0 (Vorschau) finden Sie in der [Installationsanleitung](https://github.com/Azure/azure-cli/blob/master/README.rst). Melden Sie sich an Ihrem Azure-Abonnement an, indem Sie `az login` ausführen.
* **Ressourcengruppe**: Erstellen Sie eine [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md#resource-groups), bevor Sie eine Containerregistrierung erstellen, oder verwenden Sie eine vorhandene Ressourcengruppe. Stellen Sie sicher, dass sich die Ressourcengruppe an einem Standort befindet, für den der Containerregistrierungsdienst [verfügbar](https://azure.microsoft.com/regions/services/) ist. Informationen zum Erstellen einer Ressourcengruppe mit der CLI 2.0 (Vorschau) finden Sie in den [Beispielen zur CLI 2.0 (Vorschau)](https://github.com/Azure/azure-cli-samples/tree/master/arm). 
* **Speicherkonto** (optional): Erstellen Sie ein standardmäßiges Azure-[Speicherkonto](../storage/storage-introduction.md) zum Sichern der Containerregistrierung an demselben Standort. Wenn Sie beim Erstellen einer Registrierung mit `az acr create` kein Speicherkonto angeben, führt der Befehl dies für Sie durch. Informationen zum Erstellen eines Speicherkontos mit der CLI 2.0 (Vorschau) finden Sie in den [Beispielen zur CLI 2.0 (Vorschau)](https://github.com/Azure/azure-cli-samples/tree/master/storage).
* **Dienstprinzipal** (optional): Wenn Sie mit der CLI eine Registrierung erstellen, wird der Zugriff standardmäßig nicht eingerichtet. Je nach Ihren Anforderungen können Sie einen vorhandenen Azure Active Directory-Dienstprinzipal einer Registrierung zuweisen (oder einen neuen erstellen und zuweisen) oder das Administratorbenutzerkonto der Registrierung aktivieren. Informationen hierzu finden Sie in den Abschnitten weiter unten in diesem Artikel. Weitere Informationen zum Zugreifen auf die Registrierung finden Sie unter [Authenticate with the container registry](container-registry-authentication.md) (Authentifizieren bei der Containerregistrierung). 

## <a name="create-a-container-registry"></a>Erstellen einer Containerregistrierung
Führen Sie den Befehl `az acr create` aus, um eine Containerregistrierung zu erstellen. 

> [!TIP]
> Geben Sie beim Erstellen einer Registrierung einen global eindeutigen Namen einer Domäne der obersten Ebene an, der nur Buchstaben und Zahlen enthält. Der Registrierungsname in den Beispielen lautet `myRegistry`. Ersetzen Sie ihn durch einen eigenen eindeutigen Namen. 
> 
> 

Im folgenden Befehl werden die minimal erforderlichen Parameter zum Erstellen der Containerregistrierung `myRegistry` in der Ressourcengruppe `myResourceGroup` für den Standort „USA, Süden-Mitte“ verwendet:

```azurecli
az acr create -n myRegistry -g myResourceGroup -l southcentralus
```

* `--storage-account-name` oder `-s` sind optional. Wenn nichts angegeben ist, wird ein Speicherkonto mit einem zufälligen Namen in der angegebenen Ressourcengruppe erstellt.

Die Ausgabe sieht in etwa wie folgt aus:

![az acr create output](./media/container-registry-get-started-azure-cli/acr_create.png)


Beachten Sie besonders Folgendes:

* `id`: Der Bezeichner für die Registrierung in Ihrem Abonnement, den Sie benötigen, wenn Sie einen Dienstprinzipal zuweisen möchten. 
* `loginServer`: Der vollqualifizierte Name, den Sie zum [Anmelden an der Registrierung](container-registry-authentication.md) angeben. In diesem Beispiel lautet der Name `myregistry-contoso.exp.azurecr.io` (nur Kleinbuchstaben).

## <a name="assign-a-service-principal"></a>Zuweisen eines Dienstprinzipals
Verwenden Sie die Befehle der CLI 2.0 (Vorschau), um einen Azure Active Directory-Dienstprinzipal einer Registrierung zuzuweisen. Dem Dienstprinzipal wird in diesen Beispielen die Rolle „Besitzer“ zugewiesen, aber Sie können bei Bedarf auch [andere Rollen](../active-directory/role-based-access-control-configure.md) zuweisen.

### <a name="create-a-service-principal-and-assign-access-to-the-registry"></a>Erstellen eines Dienstprinzipals und Zuweisen des Zugriffs auf die Registrierung
Im folgenden Befehl wird einem neuen Dienstprinzipal über die Rolle „Besitzer“ der Zugriff auf den Registrierungsbezeichner zugewiesen, der mit dem Parameter `--scopes` übergeben wird. Geben Sie mit dem Parameter `--password` ein sicheres Kennwort an.

```azurecli
az ad sp create-for-rbac --scopes /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry --role Owner --password myPassword
```



### <a name="assign-an-existing-service-principal"></a>Zuweisen eines vorhandenen Dienstprinzipals
Wenn Sie bereits über einen Dienstprinzipal verfügen und dafür über die Rolle „Besitzer“ Zugriff auf die Registrierung zuweisen möchten, können Sie einen ähnlichen Befehl wie im folgenden Beispiel ausführen. Sie übergeben die Dienstprinzipal-App-ID mit dem Parameter `--assignee`:

```azurecli
az role assignment create --scope /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry --role Owner --assignee myAppId
```



## <a name="manage-admin-credentials"></a>Verwalten von Administratoranmeldeinformationen
Ein Administratorkonto wird automatisch für jede Containerregistrierung erstellt und ist standardmäßig deaktiviert. In den folgenden Beispielen werden CLI-Befehle vom Typ `az acr` verwendet, um die Administratoranmeldeinformationen für Ihre Containerregistrierung zu verwalten.

### <a name="obtain-admin-user-credentials"></a>Abrufen von Administratorbenutzer-Anmeldeinformationen
```azurecli
az acr credential show -n myRegistry
```

### <a name="enable-admin-user-for-an-existing-registry"></a>Aktivieren des Administratorbenutzers für eine vorhandene Registrierung
```azurecli
az acr update -n myRegistry --admin-enabled true
```

### <a name="disable-admin-user-for-an-existing-registry"></a>Deaktivieren des Administratorbenutzers für eine vorhandene Registrierung
```azurecli
az acr update -n myRegistry --admin-enabled false
```

## <a name="list-images-and-tags"></a>Auflisten von Images und Tags
Verwenden Sie die CLI-Befehle vom Typ `az acr`, um die Images und Tags in einem Repository abzufragen. 

> [!NOTE]
> Derzeit wird der Befehl `docker search` zum Abfragen von Images und Tags für die Containerregistrierung nicht unterstützt.


### <a name="list-repositories"></a>Auflisten von Repositorys
Im folgenden Beispiel sind die Repositorys in einer Registrierung im JSON-Format (JavaScript Object Notation) aufgelistet:

```azurecli
az acr repository list -n myRegistry -o json
```

### <a name="list-tags"></a>Auflisten von Tags
Im folgenden Beispiel sind die Tags im Repository **samples/nginx** im JSON-Format aufgelistet:

```azurecli
az acr repository show-tags -n myRegistry --repository samples/nginx -o json
```

## <a name="next-steps"></a>Nächste Schritte
* [Freigeben Ihres ersten Image mit der Docker CLI](container-registry-get-started-docker-cli.md)




<!--HONumber=Nov16_HO3-->


