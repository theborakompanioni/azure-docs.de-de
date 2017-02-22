---
title: Anmelden bei einem klassischen virtuellen Azure-Computer | Microsoft Docs
description: "Verwenden Sie das klassische Azure-Portal für die Anmeldung bei einem virtuellen Windows-Computer, der mit dem klassischen Bereitstellungsmodell erstellt wurde."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 3c1239ed-07dc-48b8-8b3d-dc8c5f0ab20e
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/28/2016
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: f6537e4ebac76b9f3328223ee30647885ee15d3e
ms.openlocfilehash: f8fb116f14f771947f321f59d9880df4cdfe6859


---
# <a name="log-on-to-a-windows-virtual-machine-using-the-azure-classic-portal"></a>Anmelden bei einem virtuellen Windows-Computer über das klassische Azure-Portal
Im klassischen Azure-Portal können Sie über die Schaltfläche **Verbinden** eine Remotedesktopsitzung starten und sich bei einem virtuellen Windows-Computer anmelden.

Möchten Sie eine Verbindung mit einem virtuellen Linux-Computer herstellen? Siehe [Anmelden bei einem virtuellen Computer, auf dem Linux ausgeführt wird](virtual-machines-linux-mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Erfahren Sie, wie Sie [diese Schritte über das neue Azure-Portal ausführen](virtual-machines-windows-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!IMPORTANT] 
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells. Informationen zum Anmelden bei einem virtuellen Computer im Resource Manager-Modell finden Sie [hier](virtual-machines-windows-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="video-walkthrough"></a>Exemplarische Vorgehensweise per Video
Hier finden Sie ein Video mit einer exemplarischen Vorgehensweise für die Schritte dieses Tutorials. Dabei werden auch Endpunkte sowie öffentliche und private Ports für die Verbindung mit einem virtuellen Computer in Azure behandelt.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Documentation-Shorts/Logging-On-To-VM-Running-Windows-Server-on-Azure/player]



## <a name="connect-to-the-virtual-machine"></a>Herstellen einer Verbindung mit dem virtuellen Computer
1. Melden Sie sich am klassischen Azure-Portal an.
2. Klicken Sie auf **Virtuelle Computer**, und wählen Sie den virtuellen Computer aus.
3. Klicken Sie in der Befehlsleiste am unteren Rand der Seite auf **Verbinden**.
   
    ![Melden Sie sich beim virtuellen Computer an.](./media/virtual-machines-windows-classic-connect-logon/connectwindows.png)

> [!TIP]
> Wenn die Schaltfläche **Verbinden** nicht verfügbar ist, finden Sie weitere Informationen in den Tipps zur Problembehandlung am Ende dieses Artikels.
> 
> 

## <a name="log-on-to-the-virtual-machine"></a>Melden Sie sich beim virtuellen Computer an.
[!INCLUDE [virtual-machines-log-on-win-server](../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a>Nächste Schritte
* Wenn die Schaltfläche **Verbinden** inaktiv ist oder andere Probleme mit der Remotedesktopverbindung auftreten, versuchen Sie, die Konfiguration zurückzusetzen. Klicken Sie auf dem Dashboard des virtuellen Computers unter **Auf einen Blick** auf **Remotekonfiguration zurücksetzen**.
* Wenn Sie Probleme mit Ihrem Kennwort haben, setzen Sie es zurück. Klicken Sie auf dem Dashboard des virtuellen Computers unter **Auf einen Blick** auf **Kennwort zurücksetzen**.

Wenn diese Tipps nicht funktionieren oder nicht das Gesuchte sind, finden Sie weitere Informationen unter [Problembehandlung bei Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer](virtual-machines-windows-troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Dieser Artikel führt Sie durch die Diagnose und Behebung von häufigen Problemen.




<!--HONumber=Feb17_HO3-->


