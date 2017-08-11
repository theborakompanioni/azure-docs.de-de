---
title: Bereitstellen in Azure Container Instances aus Azure Container Registry | Azure-Dokumentation
description: Bereitstellen in Azure Container Instances aus Azure Container Registry | Azure-Dokumentation
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: 
ms.devlang: na
ms.topic: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/21/2017
ms.author: seanmck
ms.custom: 
ms.translationtype: HT
ms.sourcegitcommit: a678700884b612cad6281eb8f3b74ce63a0ebb69
ms.openlocfilehash: fcb21785584c3c5fb41f5ceb70346e3e05a30d93
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

---

# <a name="deploy-to-azure-container-instances-from-the-azure-container-registry"></a>Bereitstellen in Azure Container Instances aus Azure Container Registry | Azure-Dokumentation

Azure Container Registry (ACR) ist eine Azure-basierte, private Registrierung für Docker-Containerimages. In diesem Artikel wird beschrieben, wie Containerimages, die in Azure Container Registry gespeichert sind, auf Azure Container Instances bereitgestellt werden können.

## <a name="using-the-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle

Die Azure-CLI enthält Befehle zum Erstellen und Verwalten von Containern in Azure Container Instances. Wenn Sie ein privates Image im `create`-Befehl angeben, können Sie auch das Kennwort für die Image-Registrierung festlegen, das erforderlich ist, um sich bei der Containerregistrierung zu authentifizieren.

```azurecli-interactive
az container create --name myprivatecontainer --image mycontainerregistry.azurecr.io/mycontainerimage:v1 --image-registry-password myRegistryPassword --resource-group myresourcegroup
```

Der `create`-Befehl unterstützt auch die Angabe von `image-registry-login-server` und `image-registry-username`. Allerdings ist standardmäßig der Anmeldeserver für Azure Container Registry einfach *registryname*.azurecr.io, und der Benutzername ist *registryname*, sodass diese Werte aus dem Imagenamen abgeleitet werden, wenn sie nicht explizit bereitgestellt werden.

## <a name="using-an-azure-resource-manager-template"></a>Verwenden einer Azure Resource Manager-Vorlage

Sie können die Eigenschaften von Azure Container Registry in einer Azure Resource Manager-Vorlage angeben. Schließen Sie einfach die `imageRegistryCredentials`-Eigenschaft in der Definition Ihrer Containergruppe ein:

```json
"imageRegistryCredentials": [
  {
    "server": "imageRegistryLoginServer",
    "username": "imageRegistryUsername",
    "password": "imageRegistryPassword"
  }
]
```

Um das Kennwort für Ihre Containerregistrierung nicht direkt in der Vorlage zu speichern, wird empfohlen, dass Sie es als geheimen Schlüssel in [Azure Key Vault](../key-vault/key-vault-manage-with-cli2.md) aufbewahren und in der Vorlage mithilfe der [nativen Integration zwischen Azure Resource Manager und Key Vault](../azure-resource-manager/resource-manager-keyvault-parameter.md) auf darauf verweisen.


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie Container erstellen, sie an eine private Containerregistrierung und an Azure Container Instances weiterleiten, indem Sie das [Tutorial abschließen](container-instances-tutorial-prepare-app.md).
