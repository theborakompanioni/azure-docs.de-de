---
title: Erstellen und Verwalten virtueller Computer in DevTest Labs mit der Azure-Befehlszeilenschnittstelle | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit Azure DevTest Labs virtuelle Computer mit Azure CLI 2.0 erstellen und verwalten.
services: devtest-lab,virtual-machines
documentationcenter: na
author: lisawong19
manager: douge
editor: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: liwong
ms.translationtype: HT
ms.sourcegitcommit: 190ca4b228434a7d1b30348011c39a979c22edbd
ms.openlocfilehash: a3af12ba0598d60b55b3714ae1690fe3e5b54a42
ms.contentlocale: de-de
ms.lasthandoff: 09/09/2017

---
# <a name="create-and-manage-virtual-machines-with-devtest-labs-using-the-azure-cli"></a>Erstellen und Verwalten virtueller Computer in DevTest Labs mit der Azure-Befehlszeilenschnittstelle
Dieser Schnellstart führt Sie durch das Erstellen, Starten, Verbinden, Aktualisieren und Bereinigen von Entwicklungscomputern im Lab. 

Vorbereitungen

* Falls Sie noch kein Lab erstellt haben, finden Sie [hier](devtest-lab-create-lab.md) Anweisungen dazu.

* [Installieren Sie CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). Führen Sie zum Starten „az login“ aus, um eine Verbindung mit Azure herzustellen. 

## <a name="create-and-verify-the-virtual-machine"></a>Erstellen und Überprüfen von virtuellen Computern 
Erstellen Sie einen virtuellen Computer aus einem Marketplace-Image mit SSH-Authentifizierung.
```azurecli
az lab vm create --lab-name sampleLabName --resource-group sampleLabResourceGroup --name sampleVMName --image "Ubuntu Server 16.04 LTS" --image-type gallery --size Standard_DS1_v2 --authentication-type  ssh --generate-ssh-keys --ip-configuration public 
```
> [!NOTE]
> Fügen Sie den Namen der **Lab-Ressourcengruppe** in den Parameter --resource-group ein.
>

Wenn Sie einen virtuellen Computer mit einer Formel erstellen möchten, verwenden Sie in [az lab vm create](https://docs.microsoft.com/cli/azure/lab/vm#az_lab_vm_create) den Parameter --formula.


Überprüfen Sie, ob der virtuelle Computer verfügbar ist.
```azurecli
az lab vm show --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup --expand 'properties($expand=ComputeVm,NetworkInterface)' --query '{status: computeVm.statuses[0].displayStatus, fqdn: fqdn, ipAddress: networkInterface.publicIpAddress}'
```
```json
{
  "fqdn": "lisalabvm.southcentralus.cloudapp.azure.com",
  "ipAddress": "13.85.228.112",
  "status": "Provisioning succeeded"
}
```

## <a name="start-and-connect-to-the-virtual-machine"></a>Starten und Verbinden virtueller Computer
Starten Sie einen virtuellen Computer.
```azurecli
az lab vm start --lab-name sampleLabName --name sampleVMName --resource-group sampleLabResourceGroup
```
> [!NOTE]
> Fügen Sie den Namen der **Lab-Ressourcengruppe** in den Parameter --resource-group ein.
>

Stellen Sie eine Verbindung mit einem virtuellen Computer her: [SSH](../virtual-machines/linux/mac-create-ssh-keys.md) oder [Remotedesktop](../virtual-machines/windows/connect-logon.md).
```bash
ssh userName@ipAddressOrfqdn 
```

## <a name="update-the-virtual-machine"></a>Aktualisieren virtueller Computer
Wenden Sie Artefakte auf einen virtuellen Computer an.
```azurecli
az lab vm apply-artifacts --lab-name  sampleLabName --name sampleVMName  --resource-group sampleResourceGroup  --artifacts @/artifacts.json
```

```json
[
  {
    "artifactId": "/artifactSources/public repo/artifacts/linux-java",
    "parameters": []
  },
  {
    "artifactId": "/artifactSources/public repo/artifacts/linux-install-nodejs",
    "parameters": []
  },
  {
    "artifactId": "/artifactSources/public repo/artifacts/linux-apt-package",
    "parameters": [
      {
        "name": "packages",
        "value": "abcd"
      },
      {
        "name": "update",
        "value": "true"
      },
      {
        "name": "options",
        "value": ""
      }
    ]
  } 
]
```

Listen Sie die Artefakte im Lab auf.
```azurecli
az lab vm show --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup --expand "properties(\$expand=artifacts)" --query 'artifacts[].{artifactId: artifactId, status: status}'
```
```json
{
  "artifactId": "/subscriptions/abcdeftgh1213123/resourceGroups/lisalab123RG822645/providers/Microsoft.DevTestLab/labs/lisalab123/artifactSources/public repo/artifacts/linux-install-nodejs",
  "status": "Succeeded"
}
```

## <a name="stop-and-delete-the-virtual-machine"></a>Beenden und Löschen virtueller Computer    
Beenden Sie einen virtuellen Computer.
```azurecli
az lab vm stop --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

Löschen Sie einen virtuellen Computer.
```azurecli
az lab vm delete --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]
