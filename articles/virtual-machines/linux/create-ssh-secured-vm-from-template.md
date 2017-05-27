---
title: Erstellen einer Linux-VM in Azure mithilfe einer Vorlage | Microsoft-Dokumentation
description: Verwenden der Azure-CLI-2.0 zum Erstellen einer Linux-VM aus einer Ressourcen-Manager-Vorlage
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 721b8378-9e47-411e-842c-ec3276d3256a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 05/12/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 9568210d4df6cfcf5b89ba8154a11ad9322fa9cc
ms.openlocfilehash: 908a8a0c82b2d21fb25c9b33dbd714570d1ac272
ms.contentlocale: de-de
ms.lasthandoff: 05/15/2017


---
# <a name="how-to-create-a-linux-virtual-machine-with-azure-resource-manager-templates"></a>Erstellen eines virtuellen Linux-Computers mithilfe von Azure Resource Manager-Vorlagen
In diesem Artikel erfahren Sie, wie Sie schnell einen virtuellen Linux-Computer (VM) mithilfe von Azure Resource Manager-Vorlagen und der Azure CLI 2.0 bereitstellen. Sie können diese Schritte auch mit [Azure CLI 1.0](create-ssh-secured-vm-from-template-nodejs.md) ausführen.


## <a name="templates-overview"></a>Übersicht über Vorlagen
Azure Resource Manager-Vorlagen sind JSON-Dateien, die die Infrastruktur und Konfiguration Ihrer Azure-Lösung definieren. Mit einer Vorlage können Sie die Lösung während ihres Lebenszyklus wiederholt bereitstellen und sicher sein, dass Ihre Ressourcen einheitlich bereitgestellt werden. Weitere Informationen zum Format der Vorlage und zur Erstellung finden Sie unter [Erstellen Ihrer ersten Azure Resource Manager-Vorlage](../../azure-resource-manager/resource-manager-create-first-template.md). Die JSON-Syntax für Ressourcentypen finden Sie unter [Define resources in Azure Resource Manager templates](/azure/templates/) (Definieren von Ressourcen in Azure Resource Manager-Vorlagen).


## <a name="create-resource-group"></a>Ressourcengruppe erstellen
Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Vor dem virtuellen Computer muss eine Ressourcengruppe erstellt werden. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroupVM* in der Region *eastus* erstellt:

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers
Im folgenden Beispiel wird eine VM aus [dieser Azure Resource Manager-Vorlage](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) mit [az group deployment create](/cli/azure/group/deployment#create) erstellt. Geben Sie den Wert Ihres eigenen öffentlichen SSH-Schlüssels ein, etwa den Inhalt von *~/.ssh/id_rsa.pub*. Informationen zum Erstellen eines SSH-Schlüsselpaars finden Sie unter [Erstellen und Verwenden eines SSH-Schlüsselpaars für Linux-VMs in Azure](mac-create-ssh-keys.md).

```azurecli
az group deployment create --resource-group myResourceGroup \
  --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
  --parameters '{"sshKeyData": {"value": "ssh-rsa AAAAB3N{snip}B9eIgoZ"}}'
```

In diesem Beispiel haben Sie eine in GitHub gespeicherte Vorlage angegeben. Sie können eine Vorlage außerdem herunterladen oder erstellen und den lokalen Pfad mithilfe des gleichen Parameters `--template-file` angeben.

Rufen Sie die öffentliche IP-Adresse mit [az network public-ip show](/cli/azure/network/public-ip#show), um Ihre VM mit SSH zu schützen:

```azurecli
az network public-ip show \
    --resource-group myResourceGroup \
    --name sshPublicIP \
    --query [ipAddress] \
    --output tsv
```

Anschließend können Sie SSH in gewohnter Weise auf Ihre VM anwenden:

```bash
ssh azureuser@<ipAddress>
```

## <a name="next-steps"></a>Nächste Schritte
In diesem Beispiel haben Sie eine einfache Linux-VM erstellt. Weitere Resource Manager-Vorlagen, die Anwendungsframeworks enthalten oder komplexere Umgebungen erstellen, finden Sie mithilfe einer Suche im [Azure-Schnellstartvorlagen-Katalog](https://azure.microsoft.com/documentation/templates/).
