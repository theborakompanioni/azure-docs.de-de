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
ms.service: container-instances
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/02/2017
ms.author: seanmck
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 8b857b4a629618d84f66da28d46f79c2b74171df
ms.openlocfilehash: cdac6362f0d51b3144024efd28af09eb6d97515f
ms.contentlocale: de-de
ms.lasthandoff: 08/04/2017

---

# <a name="deploy-to-azure-container-instances-from-the-azure-container-registry"></a>Bereitstellen in Azure Container Instances aus Azure Container Registry | Azure-Dokumentation

Azure Container Registry (ACR) ist eine Azure-basierte, private Registrierung für Docker-Containerimages. In diesem Artikel wird beschrieben, wie Containerimages, die in Azure Container Registry gespeichert sind, auf Azure Container Instances bereitgestellt werden können.

## <a name="using-the-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle

Die Azure-CLI enthält Befehle zum Erstellen und Verwalten von Containern in Azure Container Instances. Wenn Sie ein privates Image im `create`-Befehl angeben, können Sie auch das Kennwort für die Image-Registrierung festlegen, das erforderlich ist, um sich bei der Containerregistrierung zu authentifizieren.

```azurecli-interactive
az container create --name myprivatecontainer --image mycontainerregistry.azurecr.io/mycontainerimage:v1 --image-registry-password myRegistryPassword --resource-group myresourcegroup
```

Der `create`-Befehl unterstützt auch die Angabe von `image-registry-login-server` und `image-registry-username`. Allerdings ist standardmäßig der Anmeldeserver für Azure Container Registry immer *registryname*.azurecr.io, und der Standardbenutzername ist *registryname*, sodass diese Werte aus dem Imagenamen abgeleitet werden, wenn sie nicht explizit bereitgestellt werden.

## <a name="using-an-azure-resource-manager-template"></a>Verwenden einer Azure Resource Manager-Vorlage

Sie können die Eigenschaften von Azure Container Registry in einer Azure Resource Manager-Vorlage angeben, indem Sie die `imageRegistryCredentials`-Eigenschaft in die Definition der Containergruppe einbeziehen:

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

## <a name="using-the-azure-portal"></a>Verwenden des Azure-Portals

Wenn Sie Containerimages im Azure Container Registry verwalten, können Sie leicht mit dem Azure-Portal einen Container in Azure Container Instances erstellen.

1. Navigieren Sie im Azure-Portal wieder zu Ihrer Containerregistrierung.

2. Auswählen von Repositorys

    ![Das Menü von Azure Container Registry im Azure-Portal][acr-menu]

3. Wählen Sie das Repository aus, aus dem Sie eine Bereitstellung durchführen möchten.

4. Rechtsklicken Sie auf das Tag für das Containerimage, das Sie bereitstellen möchten.

    ![Kontextmenü für den Start von Containern mit Azure Container Instances][acr-runinstance-contextmenu]

5. Geben Sie einen Namen für den Container und einen Namen für die Ressourcengruppe ein. Sie können den Standardwert auch bei Bedarf ändern.

    ![Erstellmenü für Azure Container Instances][acr-create-deeplink]

6. Sobald die Bereitstellung abgeschlossen wurde, können Sie vom Benachrichtigungsbereich aus zur Containergruppe navigieren, um die IP-Adresse und andere Eigenschaften zu sehen.

    ![Detailansicht für eine Containergruppe in Azure Container Instances][aci-detailsview]

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie Container erstellen, sie an eine private Containerregistrierung und an Azure Container Instances weiterleiten, indem Sie das [Tutorial abschließen](container-instances-tutorial-prepare-app.md).

<!-- IMAGES -->
[acr-menu]: ./media/container-instances-using-azure-container-registry/acr-menu.png

[acr-runinstance-contextmenu]: ./media/container-instances-using-azure-container-registry/acr-runinstance-contextmenu.png

[acr-create-deeplink]: ./media/container-instances-using-azure-container-registry/acr-create-deeplink.png

[aci-detailsview]: ./media/container-instances-using-azure-container-registry/aci-detailsview.png
