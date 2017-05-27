---
title: Erstellen eines virtuellen Linux-Computers mithilfe einer Azure-Vorlage mit Azure CLI 1.0| Microsoft-Dokumentation
description: Erstellen Sie einen virtuellen Linux-Computer auf Azure mithilfe von Azure CLI 1.0 und einer Azure Resource Manager-Vorlage.
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: v-livech
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 9568210d4df6cfcf5b89ba8154a11ad9322fa9cc
ms.openlocfilehash: 33d4aaa78fcdf3bd9e2e236606f2d3049f464a8a
ms.contentlocale: de-de
ms.lasthandoff: 05/15/2017


---
# <a name="how-to-create-a-linux-vm-using-the-azure-cli-10-an-azure-resource-manager-template"></a>Erstellen eines virtuellen Linux-Computers mithilfe von Azure CLI 1.0 und einer Azure Resource Manager-Vorlage
In diesem Artikel erfahren Sie, wie Sie schnell einen virtuellen Linux-Computer mithilfe von Azure CLI 1.0 und einer Azure Resource Manager-Vorlage erstellen. Zum Ausführen der Schritte in diesem Artikel ist Folgendes erforderlich:

* Azure-Konto ([kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/))
* [Azure CLI 1.0](../../cli-install-nodejs.md), bei `azure login` angemeldet.
* Die Azure-Befehlszeilenschnittstelle *muss* im Azure Resource Manager-Modus `azure config mode arm` ausgeführt werden.

Sie können eine Vorlage für eine Linux-VM auch schnell über das [Azure-Portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) bereitstellen.

## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe
Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

- [Azure-CLI 1.0](#quick-command-summary): Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell (in diesem Artikel)
- [Azure CLI 2.0:](create-ssh-secured-vm-from-template.md) Unsere Befehlszeilenschnittstelle der nächsten Generation für das Resource Manager-Bereitstellungsmodell

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
Im folgenden Codebeispiel wird veranschaulicht, wie Sie `azure group create` aufrufen, um eine Ressourcengruppe zu erstellen, und gleichzeitig einen per SSH geschützten virtuellen Linux-Computer bereitstellen, indem Sie [diese Azure Resource Manager-Vorlage](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json)verwenden. Beachten Sie, dass Sie in Ihrem Beispiel Namen verwenden müssen, die für Ihre Umgebung eindeutig sind. In diesem Beispiel werden *myResourceGroup* als Name der Ressourcengruppe und *myVM* als Name der VM verwendet.

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


