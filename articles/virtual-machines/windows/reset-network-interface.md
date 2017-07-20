---
title: "Zurücksetzen der Netzwerkschnittstelle für virtuelle Windows-Computer in Azure | Microsoft-Dokumentation"
description: "Zeigt, wie Sie die Netzwerkschnittstelle für virtuelle Windows-Computer in Azure zurücksetzen"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: genlin
manager: willchen
editor: 
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: genli
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: 220e426be20086841854d89831f6c9d67529867f
ms.contentlocale: de-de
ms.lasthandoff: 06/28/2017


---
# <a name="how-to-reset-network-interface-for-azure-windows-vm"></a>Die Netzwerkschnittstelle für den virtuellen Windows-Computer in Azure zurücksetzen 

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Sie können nach dem Deaktivieren der Netzwerkschnittstelle (NIC) keine Verbindung zu dem virtuellen Windows-Computer (VM) in Azure herstellen oder manuell eine statische IP-Adresse für die NIC erstellen. Dieser Artikel zeigt, wie Sie die Netzwerkschnittstelle für die Windows-VM in Azure zurücksetzen, wodurch das Problem mit der Remoteverbindung gelöst wird.

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]
## <a name="reset-network-interface"></a>Zurücksetzen der Netzwerkschnittstelle

### <a name="for-classic-vms"></a>Für klassische virtuelle Computer

Um die Netzwerkschnittstelle zurückzusetzen, gehen Sie folgendermaßen vor:

1.  Öffnen Sie das [Azure-Portal]( https://ms.portal.azure.com).
2.  Wählen Sie **Virtuelle Computer (klassisch)** aus.
3.  Wählen Sie den betroffenen virtuellen Computer aus.
4.  Wählen Sie die **IP-Adressen** aus.
5.  Wenn die **Private IP-Zuweisung** nicht **Statisch** ist, ändern Sie diese in **Statisch**.
6.  Ändern Sie die **IP-Adresse** zu einer anderen IP-Adresse, die im Subnetz verfügbar ist.
7.  Wählen Sie Speichern aus.
8.  Der virtuelle Computer wird neu gestartet, um die neue Netzwerkkarte mit dem System zu initialisieren.
9.  Versuchen Sie RDP auf Ihrem Computer auszuführen. Wenn Sie möchten, können Sie bei erfolgreicher Ausführung die private IP-Adresse zurück zur ursprünglichen ändern. Andernfalls können Sie sie speichern. 

### <a name="for-vms-deployed-in-resource-group-model"></a>Für im Ressourcengruppenmodell bereitgestellte virtuelle Computer

1.  Öffnen Sie das [Azure-Portal]( https://ms.portal.azure.com).
2.  Wählen Sie den betroffenen virtuellen Computer aus.
3.  Wählen Sie **Netzwerkschnittstellen** aus.
4.  Wählen Sie die Netzwerkschnittstelle, die mit dem Computer verknüpft ist, aus
5.  Wählen Sie die **IP-Konfigurationen** aus.
6.  Wählen Sie die IP aus. 
7.  Wenn die **Private IP-Zuweisung** nicht **Statisch** ist, ändern Sie diese in **Statisch**.
8.  Ändern Sie die **IP-Adresse** zu einer anderen IP-Adresse, die im Subnetz verfügbar ist.
9. Der virtuelle Computer wird neu gestartet, um die neue Netzwerkkarte mit dem System zu initialisieren.
10. Versuchen Sie RDP auf Ihrem Computer auszuführen. Wenn Sie möchten, können Sie bei erfolgreicher Ausführung die private IP-Adresse zurück zur ursprünglichen ändern. Andernfalls können Sie sie speichern. 

## <a name="delete-the-unavailable-nics"></a>Löschen Sie die nicht verfügbaren NICs
Nachdem Sie eine Remotedesktopverbindung mit dem Computer ausführen können, müssen Sie die alten NICs löschen, um potenzielle Probleme zu vermeiden:

1.  Öffnen Sie den Geräte-Manager.
2.  Wählen Sie die **Ansicht** > **Ausgeblendete Geräte anzeigen** aus.
3.  Wählen Sie die **Netzwerkadapter** aus. 
4.  Suchen Sie nach Adaptern mit dem Namen „Microsoft Hyper-V-Netzwerkadapter“.
5.  Möglicherweise sehen Sie einen nicht verfügbaren, abgeblendeten Adapter. Klicken Sie mit der rechten Maustaste auf den Adapter, und klicken Sie dann auf Deinstallieren.

    ![die Abbildung der Netzwerkschnittstellenkarte](media/reset-network-interface/nicpage.png)

    > [!NOTE]
    > Deinstallieren Sie nur die nicht verfügbaren Adapter mit dem Namen „Microsoft Hyper-V-Netzwerkadapter“. Wenn Sie einen der anderen ausgeblendeten Adapter deinstallieren, können zusätzliche Probleme auftreten.
    >
    >

6.  Jetzt sollten alle nicht verfügbaren Adapter aus dem System gelöscht worden sein.
