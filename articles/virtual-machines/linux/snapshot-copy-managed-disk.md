---
title: "Kopieren eines verwalteten Azure-Datenträgers für die Sicherung | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie eine Kopie eines verwalteten Azure-Datenträgers für die Sicherung oder Behandlung von Datenträgerproblemen erstellen."
documentationcenter: 
author: squillace
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 2/6/2017
ms.author: rasquill
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 22013ec8e5531a2f61d811300bce016fcde5ab86
ms.lasthandoff: 04/03/2017


---
# <a name="create-a-copy-of-a-vhd-stored-as-an-azure-managed-disk-by-using-managed-snapshots"></a>Erstellen einer Kopie einer als verwalteter Azure-Datenträger gespeicherten VHD mithilfe verwalteter Momentaufnahmen
Erstellen Sie eine Momentaufnahme eines verwalteten Datenträgers für die Sicherung oder einen verwalteten Datenträger aus der Momentaufnahme, und fügen Sie ihn an einen virtuellen Testcomputer an, um Probleme zu behandeln. Eine verwaltete Momentaufnahme ist eine vollständige zeitpunktgebundene Kopie eines verwalteten VM-Datenträgers. Es wird eine schreibgeschützte Kopie Ihrer VHD erstellt, die als verwalteter Standard-Datenträger gespeichert wird. 

Informationen zu Preisen finden Sie unter [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/managed-disks/). <!--Add link to topic or blog post that explains managed disks. -->

Sie können eine Momentaufnahme des verwalteten Datenträgers im Azure-Portal oder mithilfe von Azure CLI 2.0 erstellen.

## <a name="use-azure-cli-20-to-take-a-snapshot"></a>Erstellen einer Momentaufnahme mithilfe von Azure CLI 2.0

> [!NOTE] 
> Für das folgende Beispiel muss Azure CLI 2.0 installiert sein, und Sie müssen bei Ihrem Azure-Konto angemeldet sein.

Die folgenden Schritte zeigen, wie Sie mithilfe des Befehls `az snapshot create` mit dem `--source-disk`-Parameter eine Momentaufnahme eines verwalteten Betriebssystemdatenträgers erstellen. Beim folgenden Beispiel wird davon ausgegangen, dass ein virtueller Computer namens `myVM` vorhanden ist, der mit einem verwalteten Betriebssystemdatenträger in der Ressourcengruppe `myResourceGroup` erstellt wurde.

```azure-cli
# take the disk id with which to create a snapshot
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
az snapshot create -g myResourceGroup --source "$osDiskId" --name osDisk-backup
```

Die Ausgabe sollte ungefähr wie folgt aussehen:

```json
{
  "accountType": "Standard_LRS",
  "creationData": {
    "createOption": "Copy",
    "imageReference": null,
    "sourceResourceId": null,
    "sourceUri": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/disks/osdisk_6NexYgkFQU",
    "storageAccountId": null
  },
  "diskSizeGb": 30,
  "encryptionSettings": null,
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/snapshots/osDisk-backup",
  "location": "westus",
  "name": "osDisk-backup",
  "osType": "Linux",
  "ownerId": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "tags": null,
  "timeCreated": "2017-02-06T21:27:10.172980+00:00",
  "type": "Microsoft.Compute/snapshots"
}
```

## <a name="use-azure-portal-to-take-a-snapshot"></a>Erstellen einer Momentaufnahme im Azure-Portal 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie zunächst links oben auf **Neu**, und suchen Sie nach **Momentaufnahme**.
3. Klicken Sie auf dem Blatt „Momentaufnahme“ auf **Erstellen**.
4. Geben Sie einen **Namen** für die Momentaufnahme ein.
5. Wählen Sie eine vorhandene [Ressourcengruppe](../../azure-resource-manager/resource-group-overview.md#resource-groups) aus, oder geben Sie den Namen für eine neue Ressourcengruppe ein. 
6. Wählen Sie den Standort eines Azure-Rechenzentrums aus.  
7. Wählen Sie für **Quelldatenträger** den verwalteten Datenträger aus, für den eine Momentaufnahme erstellt werden soll.
8. Wählen Sie den **Kontotyp** aus, der zum Speichern der Momentaufnahme verwendet werden soll. Wir empfehlen **Standard_LRS**, es sei denn, Sie benötigen eine Speicherung auf einem Hochleistungsdatenträger.
9. Klicken Sie auf **Erstellen**.

Wenn Sie vorhaben, die Momentaufnahme zum Erstellen eines verwalteten Datenträgers zu nutzen und an eine VM anzufügen, die hohe Leistung benötigt, verwenden Sie den Parameter `--sku Premium_LRS` mit dem Cmdlet `az snapshot create`. Dadurch wird die Momentaufnahme so erstellt, dass sie als verwalteter Premium-Datenträger gespeichert wird. Verwaltete Premium-Datenträger bieten eine bessere Leistung, da es sich um SSDs (Solid-State Drives) handelt. Allerdings sind sie auch teurer als Standard-Datenträger (HDDs).



