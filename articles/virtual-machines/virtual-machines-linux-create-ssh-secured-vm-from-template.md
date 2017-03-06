---
title: Erstellen einer Linux-VM mit einer Azure-Vorlage | Microsoft Docs
description: Erfahren Sie, wie Sie eine Linux-VM auf Azure mit einer Azure Resource Manager-Vorlage erstellen.
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 721b8378-9e47-411e-842c-ec3276d3256a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: hero-article
ms.date: 10/24/2016
ms.author: v-livech
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: fa842efd99718be7fa9eaf8aac8030c32cbceeec
ms.openlocfilehash: bd0c9fd0044ce8a5de9004c68b1267b8b66df80a
ms.lasthandoff: 03/01/2017


---
# <a name="how-to-create-a-linux-vm-using-an-azure-resourec-manager-template"></a>Erstellen einer Linux-VM mit einer Azure Resource Manager-Vorlage
Dieser Artikel zeigt, wie Sie mithilfe einer Azure-Vorlage schnell einen virtuellen Linux-Computer auf Azure bereitstellen können.  Zum Ausführen der Schritte in diesem Artikel ist Folgendes erforderlich:

* Azure-Konto ([kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/))
* [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md), angemeldet mit `azure login`
* Die Azure-Befehlszeilenschnittstelle *muss* im Azure Resource Manager-Modus `azure config mode arm` ausgeführt werden.

Sie können eine Vorlage für eine Linux-VM auch schnell über das [Azure-Portal](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) bereitstellen.

## <a name="quick-command-summary"></a>Kurze Zusammenfassung der Befehle
```azurecli
azure group create \
    -n myResourceGroup \
    -l westus \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## <a name="detailed-walkthrough"></a>Ausführliche exemplarische Vorgehensweise
Mit Vorlagen können Sie VMs unter Azure mit Einstellungen erstellen, die Sie während des Startprozesses anpassen möchten, z.B. Einstellungen wie Benutzer- und Hostnamen. Für diesen Artikel starten wir eine Azure-Vorlage, indem wir eine Ubuntu-VM mit einer Netzwerksicherheitsgruppe (NSG) mit geöffnetem Port 22 für SSH verwenden.

Azure Resource Manager-Vorlagen sind JSON-Dateien, die für einfache einmalige Aufgaben wie das Starten einer Ubuntu-VM in diesem Artikel verwendet werden können.  Außerdem können Azure-Vorlagen auch zum Erstellen komplexer Azure-Konfigurationen für vollständige Umgebungen eingesetzt werden, z.B. eines Bereitstellungsstapels für Tests, Entwicklung oder Produktion.

## <a name="create-the-linux-vm"></a>Erstellen der Linux-VM
Im folgenden Codebeispiel wird veranschaulicht, wie Sie `azure group create` aufrufen, um eine Ressourcengruppe zu erstellen, und gleichzeitig einen per SSH geschützten virtuellen Linux-Computer bereitstellen, indem Sie [diese Azure Resource Manager-Vorlage](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json)verwenden. Beachten Sie, dass Sie in Ihrem Beispiel Namen verwenden müssen, die für Ihre Umgebung eindeutig sind. In diesem Beispiel wird `myResourceGroup` als Ressourcengruppenname und `myVM` als VM-Name verwendet.

```azurecli
azure group create \
    --name myResourceGroup \
    --location westus \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

Die Ausgabe sollte dem folgenden Ausgabeblock ähneln:

```azurecli
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
info:    Supply values for the following parameters
sshKeyData: ssh-rsa AAAAB3Nza<..ssh public key text..>VQgwjNjQ== myAdminUser@myVM
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<..subid text..>/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

In diesem Beispiel wird eine VM mit dem Parameter `--template-uri` bereitgestellt.  Sie können eine Vorlage auch herunterladen oder lokal erstellen und mit dem Parameter `--template-file` mit einem Pfad zur Vorlagendatei als Argument übergeben. Sie werden von der Azure-Befehlszeilenschnittstelle aufgefordert, die für die Vorlage erforderlichen Parameter anzugeben.

## <a name="next-steps"></a>Nächste Schritte
Im [Katalog mit den Vorlagen](https://azure.microsoft.com/documentation/templates/) können Sie weitere App-Frameworks für die Bereitstellung auswählen.


