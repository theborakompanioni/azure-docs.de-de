---
title: Erstellen eines benutzerdefinierten Azure DevTest Labs-Images aus einem virtuellen Computer | Microsoft Docs
description: Erfahren Sie, wie Sie mit dem Azure-Portal aus einem bereitgestellten virtuellen Computer ein benutzerdefiniertes Image in Azure DevTest Labs erstellen.
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
ms.date: 01/10/2017
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 38d70ed302eeab912ce4fe33272f85e96f1b1eda
ms.openlocfilehash: 9d2dcf7164985508d691e8a0c123efaf3b8aa19a


---

# <a name="create-a-custom-image-from-a-vm"></a>Erstellen eines benutzerdefinierten Images von einem virtuellen Computer

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

## <a name="step-by-step-instructions"></a>Schrittweise Anweisungen

Sie können ein benutzerdefiniertes Image aus einem bereitgestellten virtuellen Computer erstellen und es anschließend zum Erstellen identischer virtuelle Computer verwenden. Die folgenden Schritte beschreiben, wie Sie ein benutzerdefiniertes Image von einem virtuellen Computer erstellen:

1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)an.

1. Wählen Sie **Weitere Dienste** und dann in der Liste **DevTest Labs**.

1. Wählen Sie in der Liste der Labs das gewünschte Lab aus.  

1. Wählen Sie auf dem Blatt des Labs die Option **My virtual machines**(Meine virtuellen Computer) aus.
 
1. Wählen Sie auf dem Blatt **My virtual machines** (Meine virtuellen Computer) den virtuellen Computer aus, von dem Sie das benutzerdefinierte Image erstellen möchten.

1. Wählen Sie auf dem Blatt des virtuellen Computers **Create custom image (VHD)**(Benutzerdefiniertes Image erstellen (VHD)).

    ![Menüelement: Benutzerdefiniertes Bild erstellen](./media/devtest-lab-create-template/create-custom-image.png)

1. Geben Sie auf dem Blatt **Create image** (Image erstellen) einen Namen und eine Beschreibung für Ihr benutzerdefiniertes Image ein. Diese Informationen werden in der Liste der Basisimages angezeigt, wenn Sie einen virtuellen Computer erstellen.

    ![Blatt: Benutzerdefiniertes Bild erstellen](./media/devtest-lab-create-template/create-custom-image-blade.png)

1. Wählen Sie aus, ob „sysprep“ auf dem virtuellen Computer ausgeführt wurde. Wenn „sysprep“ auf dem virtuellen Computer nicht ausgeführt wurde, geben Sie an, ob „sysprep“ ausgeführt werden soll, wenn Sie einen virtuellen Computer anhand dieses benutzerdefinierten Images erstellen.

1. Wählen Sie **OK** , wenn Sie mit der Erstellung des benutzerdefinierten Images fertig sind.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Verwandte Blogbeiträge

- [Custom images or formulas? (Benutzerdefinierte Images oder Formeln?)](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Copying Custom Images between Azure DevTest Labs (Kopieren benutzerdefinierter Images zwischen Azure DevTest Labs)](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a>Nächste Schritte

- [Hinzufügen eines virtuellen Computers zum Lab](./devtest-lab-add-vm-with-artifacts.md)



<!--HONumber=Jan17_HO2-->


