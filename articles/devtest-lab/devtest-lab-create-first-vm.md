---
title: Erstellen Ihres ersten virtuellen Computers in einem Lab in Azure DevTest Labs | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihren ersten virtuellen Computer in einem Lab in Azure DevTest Labs erstellen.
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: fbc5a438-6e02-4952-b654-b8fa7322ae5f
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/24/2017
ms.author: tarcher
ms.translationtype: Human Translation
ms.sourcegitcommit: a3ca1527eee068e952f81f6629d7160803b3f45a
ms.openlocfilehash: aa6b60b799e1e98815cf288d5612f98cd77cc00e
ms.contentlocale: de-de
ms.lasthandoff: 04/27/2017


---
# <a name="create-your-first-vm-in-a-lab-in-azure-devtest-labs"></a>Erstellen Ihres ersten virtuellen Computers in einem Lab in Azure DevTest Labs

Wenn Sie erstmals auf DevTest Labs zugreifen und Ihren ersten virtuellen Computer erstellen möchten, verwenden Sie dazu wahrscheinlich ein vorkonfiguriertes [Basis-Marketplace-Image](devtest-lab-configure-marketplace-images.md). Später können auch aus einem [benutzerdefinierten Image und einer Formel](devtest-lab-add-vm.md) auswählen, wenn Sie weitere virtuelle Computer erstellen. 

Dieses Tutorial führt Sie durch die Verwendung des Azure-Portals zum Hinzufügen Ihres ersten virtuellen Computers zu einem Lab in DevTest Labs.

## <a name="steps-to-add-your-first-vm-to-a-lab-in-azure-devtest-labs"></a>Schritte zum Hinzufügen Ihres ersten virtuellen Computers zu einem Lab in Azure DevTest Labs
1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)an.
1. Wählen Sie **Weitere Dienste** und dann in der Liste **DevTest Labs** aus.
1. Wählen Sie in der Liste der Labs das Lab aus, in dem Sie den neuen virtuellen Computer (VM, Virtual Machine) erstellen möchten.  
1. Wählen Sie auf dem Blatt **Übersicht** des Labs die Option **+Hinzufügen** aus.  

    ![Schaltfläche zum Hinzufügen eines virtuellen Computers](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. Wählen Sie auf dem Blatt **Basis auswählen** ein Marketplace-Image für den virtuellen Computer aus.
1. Geben Sie auf dem Blatt **Virtueller Computer** im Textfeld **Name des virtuellen Computers** einen Namen für den neuen virtuellen Computer ein.

    ![Blatt „Lab-VM“](./media/devtest-lab-add-vm/devtestlab-lab-add-first-vm.png)

1. Geben Sie einen **Benutzernamen** ein, dem Administratorrechte auf dem virtuellen Computer erteilt werden.  
1. Geben Sie im Textfeld **Geben Sie einen Wert ein** ein Kennwort ein.
1. Der **Datenträgertyp des virtuellen Computers** bestimmt, welcher Typ von Speicherdatenträgern für die virtuellen Computer im Lab zulässig ist.
1. Wählen Sie **Größe des virtuellen Computers** , und wählen Sie eines der vordefinierten Elemente aus, die die Prozessorkerne, die RAM-Größe und die Größe der Festplatte für den zu erstellenden virtuellen Computer angeben.
1. Wählen Sie **Artefakte** , und wählen Sie aus der Liste der Artefakte die Artefakte aus, die Sie dem Basisimage hinzufügen möchten, und konfigurieren Sie sie.
    **Hinweis:** Wenn Sie noch nicht mit DevTest Labs oder dem Konfigurieren von Artefakten vertraut sind, lesen Sie den Abschnitt [Hinzufügen eines vorhandenen Artefakts zu einer VM](./devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm), und kehren Sie dann hierher zurück.
1. Wählen Sie **Erstellen** , um den angegebene virtuellen Computer dem Lab hinzuzufügen.

   Auf dem Blatt für das Lab wird der Status der VM-Erstellung angezeigt: erst als **Wird erstellt**, dann als **Wird ausgeführt**, nachdem die VM gestartet wurde.

## <a name="next-steps"></a>Nächste Schritte
* Nach der Erstellung des virtuellen Computers können Sie **Verbinden** auf dem Blatt des virtuellen Computers wählen, um eine Verbindung mit dem virtuellen Computer herzustellen.
* Unter [Hinzufügen eines virtuellen Computers zu einem Lab](devtest-lab-add-vm.md) finden Sie ausführlichere Informationen zum Hinzufügen weiterer virtueller Computer in Ihrem Lab.
* Erkunden Sie den [DevTest Labs-Azure Resource Manager-Katalog mit Schnellstartvorlagen](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates).

