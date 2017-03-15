---
title: "Hinzufügen eines virtuellen Computers zu einem Lab in Azure DevTest Labs | Microsoft-Dokumentation"
description: "Informationen zum Hinzufügen eines virtuellen Computers zu einem Lab in Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/24/2017
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 7bac59810ee3c1da239716b58d41b97088035133
ms.openlocfilehash: e8de45686102cd6c4671484a05568a10dd22fa67
ms.lasthandoff: 02/27/2017


---
# <a name="add-a-vm-to-a-lab-in-azure-devtest-labs"></a>Hinzufügen eines virtuellen Computers zu einem Lab in Azure DevTest Labs
Sie fügen einem Lab einen virtuellen Computer aus einer *Basis* hinzu, bei der es sich entweder um ein [benutzerdefiniertes Image](devtest-lab-create-template.md), eine [Formel](devtest-lab-manage-formulas.md) oder ein [Marketplace-Image](devtest-lab-configure-marketplace-images.md) handelt. Dieses Tutorial führt Sie durch die Verwendung des Azure-Portals zum Hinzufügen eines virtuellen Computers zu einem Lab in DevTest Labs.

## <a name="steps-to-add-a-vm-to-a-lab-in-azure-devtest-labs"></a>Schritte zum Hinzufügen eines virtuellen Computers zu einem Lab in Azure DevTest Labs
1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)an.
1. Wählen Sie **Weitere Dienste** und dann in der Liste **DevTest Labs** aus.
1. Wählen Sie in der Liste der Labs das Lab aus, in dem Sie den neuen virtuellen Computer (VM, Virtual Machine) erstellen möchten.  
1. Wählen Sie auf dem Blatt **Übersicht** des Labs die Option **+Hinzufügen** aus.  

    ![Schaltfläche zum Hinzufügen eines virtuellen Computers](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. Wählen Sie auf dem Blatt **Basisdesign auswählen** eine Basis für den virtuellen Computer aus.
1. Geben Sie auf dem Blatt **Virtueller Computer** im Textfeld **Name des virtuellen Computers** einen Namen für den neuen virtuellen Computer ein.
   
    ![Blatt „Lab-VM“](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. Geben Sie einen **Benutzernamen** ein, dem Administratorrechte auf dem virtuellen Computer erteilt werden.  
1. Wenn Sie ein Kennwort verwenden möchten, das in Ihrem [geheimen Speicher](https://azure.microsoft.com/en-us/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store) hinterlegt ist, wählen Sie **Gespeichertes Geheimnis verwenden**, und geben Sie einen Schlüsselwert an, der Ihrem Geheimnis (Kennwort) entspricht. Geben Sie andernfalls im Textfeld **Geben Sie einen Wert ein** ein Kennwort ein.
1. Der **Datenträgertyp des virtuellen Computers** bestimmt, welcher Typ von Speicherdatenträgern für die virtuellen Computer im Lab zulässig ist.
1. Wählen Sie **Größe des virtuellen Computers** , und wählen Sie eines der vordefinierten Elemente aus, die die Prozessorkerne, die RAM-Größe und die Größe der Festplatte für den zu erstellenden virtuellen Computer angeben.
1. Wählen Sie **Artefakte** , und wählen Sie aus der Liste der Artefakte die Artefakte aus, die Sie dem Basisimage hinzufügen möchten, und konfigurieren Sie sie. 
    **Hinweis:** Wenn Sie noch nicht mit DevTest Labs oder dem Konfigurieren von Artefakten vertraut sind, lesen Sie den Abschnitt [Hinzufügen eines vorhandenen Artefakts zu einer VM](./devtest-lab-add-vm-with-artifacts.md#add-an-existing-artifact-to-a-vm), und kehren Sie dann hierher zurück.
1. Wählen Sie **Erweiterte Einstellungen**, um die Netzwerkoptionen und Ablaufoptionen des virtuellen Computers zu konfigurieren.
1. Wenn Sie die Azure Resource Manager-Vorlage anzeigen oder kopieren möchten, lesen Sie den Abschnitt [Speichern der Azure Resource Manager-Vorlage](./devtest-lab-add-vm-with-artifacts.md#save-azure-resource-manager-template), und kehren Sie anschließend hierher zurück.
1. Wählen Sie **Erstellen** , um den angegebene virtuellen Computer dem Lab hinzuzufügen.
1. Auf dem Blatt für das Lab wird der Status der VM-Erstellung angezeigt: erst als **Wird erstellt**, dann als **Wird ausgeführt**, nachdem die VM gestartet wurde.

## <a name="next-steps"></a>Nächste Schritte
* Nach der Erstellung des virtuellen Computers können Sie **Verbinden** auf dem Blatt des virtuellen Computers wählen, um eine Verbindung mit dem virtuellen Computer herzustellen.
* Erkunden Sie den [DevTest Labs-Azure Resource Manager-Katalog mit Schnellstartvorlagen](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates).



