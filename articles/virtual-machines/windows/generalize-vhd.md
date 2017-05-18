---
title: "Generalisieren eines virtuellen Windows-Computers für die Verwendung in Azure | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie Sysprep verwenden, um eine Windows-VM zu verallgemeinern, um sie mit dem Resource Manager-Bereitstellungsmodell zu verwenden.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: a745d400-c8be-48b4-a891-4a18495ef3fd
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: cynthn
ms.translationtype: Human Translation
ms.sourcegitcommit: c308183ffe6a01f4d4bf6f5817945629cbcedc92
ms.openlocfilehash: 3be9805bfd1a71df1ff761531406adcbb7bbe612
ms.contentlocale: de-de
ms.lasthandoff: 05/17/2017


---
# <a name="generalize-a-windows-virtual-machine-using-sysprep"></a>Verallgemeinern eines virtuellen Windows-Computers mit Sysprep
In diesem Abschnitt wird erläutert, wie Sie den virtuellen Windows-Computer für die Verwendung als Image verallgemeinern. Sysprep entfernt unter anderem alle persönlichen Kontoinformationen, und bereitet den Computer darauf vor, als Image verwendet zu werden. Weitere Informationen zu Sysprep finden Sie unter [How to Use Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx)(in englischer Sprache).

Stellen Sie sicher, dass die auf dem Computer ausgeführten Serverrollen von Sysprep unterstützt werden. Weitere Informationen finden Sie unter [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

> [!IMPORTANT]
> Wenn Sie Sysprep vor dem Hochladen der virtuellen Festplatte in Azure zum ersten Mal ausführen, stellen Sie sicher, dass Sie vor dem Ausführen von Sysprep [Ihren virtuellen Computer vorbereitet](prepare-for-upload-vhd-image.md) haben. 
> 
> 

1. Melden Sie sich bei dem virtuellen Windows-Computer an.
2. Öffnen Sie das Eingabeaufforderungsfenster als Administrator. Wechseln Sie in das Verzeichnis **%windir%\system32\sysprep**, und führen Sie anschließend `sysprep.exe` aus.
3. Wählen Sie unter **Systemvorbereitungsprogramm** die Option **Out-of-Box-Experience (OOBE) für System aktivieren**, und vergewissern Sie sich, dass das Kontrollkästchen **Verallgemeinern** aktiviert ist.
4. Wählen Sie unter **Optionen für Herunterfahren** die Option **Herunterfahren**.
5. Klicken Sie auf **OK**.
   
    ![Starten von Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. Nach Abschluss von Sysprep wird die virtuelle Maschine heruntergefahren. 

> [!IMPORTANT]
> Starten Sie den virtuellen Computer nicht neu, bis Sie mit dem Hochladen der VHD in Azure oder dem Erstellen eines Images des virtuellen Computers fertig sind. Wenn der virtuelle Computer versehentlich neu gestartet wird, führen Sie Sysprep aus, um ihn erneut zu generalisieren.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
* Wenn der virtuelle Computer lokal ist, können Sie jetzt [Hochladen der VHD in Azure](upload-image.md) ausführen.
* Wenn der virtuelle Computer bereits in Azure ist, können Sie jetzt [Erstellen Sie ein Abbild von der allgemeinen VM](capture-image.md) ausführen.


