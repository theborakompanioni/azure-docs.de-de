---
title: "Hinzufügen eines anforderbaren virtuellen Computers zu einem Lab in Azure DevTest Labs | Microsoft Docs"
description: "Erfahren Sie, wie Sie einem Lab in Azure DevTest Labs einen anforderbaren virtuellen Computer hinzufügen."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: f671e66e-9630-4e30-a131-a6bad9ed9c11
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 538f282b28e5f43f43bf6ef28af20a4d8daea369
ms.openlocfilehash: 1b6e826426ebafe10abdf33f9bcfecc1c6315cfc
ms.lasthandoff: 04/07/2017


---
# <a name="add-a-claimable-vm-to-a-lab-in-azure-devtest-labs"></a>Hinzufügen eines anforderbaren virtuellen Computers zu einem Lab in Azure DevTest Labs
Einen anforderbaren virtuellen Computer fügen Sie einem Lab in ähnlicher Weise hinzu wie [einen virtuellen Standardcomputer](devtest-lab-add-vm.md), d.h. über eine *Basis*, also entweder über ein [benutzerdefiniertes Image](devtest-lab-create-template.md), eine [Formel](devtest-lab-manage-formulas.md) oder ein [Marketplace-Image](devtest-lab-configure-marketplace-images.md). In diesem Tutorial wird erläutert, wie Sie über das Azure-Portal einem Lab in DevTest Labs einen anforderbaren virtuellen Computer hinzufügen. Zudem wird der Vorgang beschrieben, mit dem ein Benutzer den virtuellen Computer anfordert.

> [!NOTE]
> Wenn Sie virtuelle Computer im Lab über [Azure Resource Manager-Vorlagen](devtest-lab-create-environment-from-arm.md) bereitstellen, können Sie anforderbare virtuelle Computer erstellen, indem Sie die Eigenschaft **allowClaim** im Abschnitt „Eigenschaften“ auf „true“ festlegen.
>
>

## <a name="steps-to-add-a-claimable-vm-to-a-lab-in-azure-devtest-labs"></a>Schritte zum Hinzufügen eines anforderbaren virtuellen Computers zu einem Lab in Azure DevTest Labs
1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)an.
1. Wählen Sie **Weitere Dienste** und dann in der Liste **DevTest Labs** aus.
1. Wählen Sie in der Liste der Labs das Lab aus, in dem Sie den neuen virtuellen Computer (VM, Virtual Machine) erstellen möchten.  
1. Wählen Sie auf dem Blatt **Übersicht** des Labs die Option **+Hinzufügen** aus.  

    ![Schaltfläche zum Hinzufügen eines virtuellen Computers](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. Wählen Sie auf dem Blatt **Basisdesign auswählen** eine Basis für den virtuellen Computer aus.
1. Geben Sie auf dem Blatt **Virtueller Computer** im Textfeld **Name des virtuellen Computers** einen Namen für den neuen virtuellen Computer ein.

    ![Blatt „Lab-VM“](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. Geben Sie einen **Benutzernamen** ein, dem Administratorrechte auf dem virtuellen Computer erteilt werden.  
1. Wenn Sie ein Kennwort verwenden möchten, das in Ihrem [geheimen Speicher](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store) hinterlegt ist, wählen Sie **Gespeichertes Geheimnis verwenden**, und geben Sie einen Schlüsselwert an, der Ihrem Geheimnis (Kennwort) entspricht. Geben Sie andernfalls im Textfeld **Geben Sie einen Wert ein** ein Kennwort ein.
1. Der **Datenträgertyp des virtuellen Computers** bestimmt, welcher Typ von Speicherdatenträgern für die virtuellen Computer im Lab zulässig ist.
1. Wählen Sie **Größe des virtuellen Computers** , und wählen Sie eines der vordefinierten Elemente aus, die die Prozessorkerne, die RAM-Größe und die Größe der Festplatte für den zu erstellenden virtuellen Computer angeben.
1. Wählen Sie **Artefakte** , und wählen Sie aus der Liste der Artefakte die Artefakte aus, die Sie dem Basisimage hinzufügen möchten, und konfigurieren Sie sie.
    **Hinweis:** Wenn Sie noch nicht mit DevTest Labs oder dem Konfigurieren von Artefakten vertraut sind, lesen Sie den Abschnitt [Hinzufügen eines vorhandenen Artefakts zu einer VM](./devtest-lab-add-vm-with-artifacts.md#add-an-existing-artifact-to-a-vm), und kehren Sie dann hierher zurück.
1. Wählen Sie **Erweiterte Einstellungen**, um die Netzwerkoptionen und Ablaufoptionen des virtuellen Computers zu konfigurieren. Wählen Sie unter **Claim options** (Anforderungsoptionen) die Option **Ja** aus, um den Computer als anforderbar festzulegen.

  ![Legen Sie den virtuellen Computer als anforderbar fest.](./media/devtest-lab-add-vm/devtestlab-claim-VM-option.png)

1. Wenn Sie die Azure Resource Manager-Vorlage anzeigen oder kopieren möchten, lesen Sie den Abschnitt [Speichern der Azure Resource Manager-Vorlage](./devtest-lab-add-vm-with-artifacts.md#save-azure-resource-manager-template), und kehren Sie anschließend hierher zurück.
1. Wählen Sie **Erstellen** , um den angegebene virtuellen Computer dem Lab hinzuzufügen.
1. Auf dem Blatt für das Lab wird der Status der VM-Erstellung angezeigt: erst als **Wird erstellt**, dann als **Wird ausgeführt**, nachdem die VM gestartet wurde.


## <a name="using-a-claimable-vm"></a>Verwenden eines anforderbaren virtuellen Computers

Ein Benutzer kann mit einem der folgenden Schritte jeden virtuellen Computer aus der Liste „Claimable virtual machines“ (Anforderbare virtuelle Computer) anfordern:

* Klicken Sie in der Liste „Claimable virtual machines“ (Anforderbare virtuelle Computer) unten auf dem Blatt „Übersicht“ des Labs mit der rechten Maustaste auf einen der virtuellen Computer in der Liste, und wählen Sie **Claim machine** (Computer anfordern) aus.

 ![Anfordern eines bestimmten anforderbaren virtuellen Computers.](./media/devtest-lab-add-vm/devtestlab-claim-VM.png)


* Wählen Sie oben auf dem Blatt **Übersicht** die Option **Claim any** (Beliebigen Computer anfordern) aus. Aus der Liste der anforderbaren virtuellen Computer wird ein virtueller Computer zufällig zugewiesen.

 ![Fordern Sie einen beliebigen anforderbaren virtuellen Computer an.](./media/devtest-lab-add-vm/devtestlab-claim-any.png)


Nachdem ein Benutzer einen virtuellen Computer angefordert hat, wird dieser in die Liste „My virtual machines“ (Meine virtuellen Computer) des Benutzers verschoben und kann von keinem anderen Benutzer mehr angefordert werden.

## <a name="next-steps"></a>Nächste Schritte
* Nach der Erstellung des virtuellen Computers können Sie **Verbinden** auf dem Blatt des virtuellen Computers wählen, um eine Verbindung mit dem virtuellen Computer herzustellen.
* Erkunden Sie den [DevTest Labs-Azure Resource Manager-Katalog mit Schnellstartvorlagen](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates).

