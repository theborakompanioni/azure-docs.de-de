---
title: Erstellen eines benutzerdefinierten Azure DevTest Labs-Images aus einer VHD-Datei | Microsoft Docs
description: Erfahren Sie, wie Sie mit dem Azure-Portal aus einer VHD-Datei ein benutzerdefiniertes Image in Azure DevTest Labs erstellen.
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: b795bc61-7c28-40e6-82fc-96d629ee0568
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 38d70ed302eeab912ce4fe33272f85e96f1b1eda
ms.openlocfilehash: 9983ea9b847f44ed18a6169a4bdb224b63626a64


---

# <a name="create-a-custom-image-from-a-vhd-file"></a>Erstellen eines benutzerdefinierten Images aus einer VHD-Datei

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a>Schrittweise Anweisungen

Die folgenden Schritte führen Sie durch die Erstellung eines benutzerdefinierten Images aus einer VHD-Datei mithilfe des Azure-Portals:

1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)an.

1. Wählen Sie **Weitere Dienste** und dann in der Liste **DevTest Labs**.

1. Wählen Sie in der Liste der Labs das gewünschte Lab aus.  

1. Wählen Sie auf dem Blatt des Labs **Konfiguration**aus. 

1. Wählen Sie auf dem Blatt **Konfiguration** für das Lab die Option **Benutzerdefinierte Images (VHDs)** aus.

1. Wählen Sie auf dem Blatt **Benutzerdefinierte Images** die Option **+ Hinzufügen** aus.

    ![Hinzufügen des benutzerdefinierten Images](./media/devtest-lab-create-template/add-custom-image.png)

1. Geben Sie den Namen des benutzerdefinierten Images ein. Dieser Name wird in der Liste der Basisimages angezeigt, wenn Sie einen virtuellen Computer erstellen.

1. Geben Sie die Beschreibung des benutzerdefinierten Images ein. Diese Beschreibung wird in der Liste der Basisimages angezeigt, wenn Sie einen virtuellen Computer erstellen.

1. Wählen Sie **VHD** aus.

1. Wählen Sie auf dem Blatt **VHD** die gewünschte VHD-Datei aus.

1. Wählen Sie **OK** aus, um das Blatt **VHD** zu schließen.

1. Wählen Sie **Betriebssystemkonfiguration** aus.

1. Wählen Sie auf der Registerkarte **Betriebssystemkonfiguration** entweder **Windows** oder **Linux** aus.

1. Wenn Sie **Windows** ausgewählt haben, geben Sie über das Kontrollkästchen an, ob *Sysprep* auf dem Computer ausgeführt wurde. 

1. Wählen Sie **OK** aus, um das Blatt **Betriebssystemkonfiguration** zu schließen.

1. Wählen Sie **OK** , um das benutzerdefinierte Image zu erstellen.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Verwandte Blogbeiträge

- [Custom images or formulas? (Benutzerdefinierte Images oder Formeln?)](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Copying Custom Images between Azure DevTest Labs (Kopieren benutzerdefinierter Images zwischen Azure DevTest Labs)](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a>Nächste Schritte

- [Hinzufügen eines virtuellen Computers zum Lab](./devtest-lab-add-vm-with-artifacts.md)



<!--HONumber=Feb17_HO3-->


