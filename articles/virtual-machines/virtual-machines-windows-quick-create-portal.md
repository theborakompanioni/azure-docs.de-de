---
title: "Azure-Schnellstart – Erstellen einer Windows-VM im Portal | Microsoft-Dokumentation"
description: "Azure-Schnellstart – Erstellen einer Windows-VM im Portal"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/21/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: a13ac5ab425ccbbe53d77cb9f5a8ebf02d009370
ms.lasthandoff: 03/22/2017

---

# <a name="create-a-windows-virtual-machine-with-the-azure-portal"></a>Erstellen einer Windows-VM im Azure-Portal

Virtuelle Azure-Computer können über das Azure-Portal erstellt werden. Diese Methode bietet eine browserbasierte Benutzeroberfläche zum Erstellen und Konfigurieren von virtuellen Computern und alle zugehörigen Ressourcen. In diesem Schnellstart wird beschrieben, wie Sie mit dem Azure-Portal einen virtuellen Computer erstellen. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="log-in-to-azure"></a>Anmelden an Azure 

Melden Sie sich unter „http://portal.azure.com“ beim Azure-Portal an.

## <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers

2. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.

3. Wählen Sie **Berechnen** auf dem Blatt **Neu**, wählen Sie **Windows Server 2016 Datacenter** auf dem Blatt **Berechnen**, und klicken Sie dann auf die Schaltfläche **Erstellen**.

4. Füllen Sie das Formular **Grundlagen** für die VM aus. Der hier eingegebene Benutzername und das Kennwort werden zum Anmelden am virtuellen Computer verwendet. Erstellen Sie für **Ressourcengruppe** eine neue Ressourcengruppe. Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen erstellt und kollektiv verwaltet werden. Klicken Sie zum Abschluss auf **OK**.

    ![Eingeben grundlegender Informationen zu Ihrem virtuellen Computer im Portalblatt](./media/virtual-machine-quick-start/create-windows-vm-portal-basic-blade.png)  

5. Wählen Sie eine Größe für den virtuellen Computer, und klicken Sie auf **Auswählen**. 

6. Wählen Sie auf dem Einstellungenblatt **Ja** unter **Verwaltete Datenträger verwenden**, behalten Sie die Standardwerte für die übrigen Einstellungen bei, und klicken Sie auf **OK**.

7. Klicken Sie auf der Zusammenfassungsseite auf **OK**, um die Bereitstellung des virtuellen Computers zu starten.

8. Um den Bereitstellungsstatus zu überwachen, klicken Sie auf die VM. Sie finden den virtuellen Computer auf dem Dashboard des Azure-Portals oder durch Auswählen von **Virtuelle Computer** im linken Menü. Nachdem der virtuelle Computer erstellt wurde, ändert sich der Status von **Wird bereitgestellt** in **Wird ausgeführt**.

## <a name="connect-to-virtual-machine"></a>Herstellen der Verbindung mit dem virtuellen Computer

Nachdem die Bereitstellung abgeschlossen ist, stellen Sie eine Remotedesktopverbindung mit dem virtuellen Computer her.

1. Klicken Sie auf dem Blatt des virtuellen Computers auf die Schaltfläche **Verbinden**. Eine Remotedesktopprotokoll-Datei (RDP-Datei) wird erstellt und heruntergeladen.

    ![Portal 9](./media/virtual-machine-quick-start/portal-quick-start-9.png) 

2. Öffnen Sie die heruntergeladene RDP-Datei, um eine Verbindung mit Ihrem virtuellen Computer herzustellen. Klicken Sie in der angezeigten Aufforderung auf **Verbinden**. Auf einem Macintosh benötigen Sie einen RDP-Client, z.B. diesen [Remotedesktopclient](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) aus dem Mac App Store.

3. Geben Sie den Benutzernamen und das Kennwort ein, den bzw. das Sie beim Erstellen des virtuellen Computers festgelegt haben, und klicken Sie anschließend auf **OK**.

4. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Klicken Sie auf **Ja** bzw. **Weiter**, um mit dem Herstellen der Verbindung fortzufahren.

## <a name="delete-virtual-machine"></a>Löschen des virtuellen Computers

Wenn Ressourcengruppe, VM und alle zugehörigen Ressourcen nicht mehr benötigt werden, löschen Sie sie. Wählen Sie hierzu die Ressourcengruppe auf dem Blatt für virtuelle Computer aus, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

[Experimentieren mit der Installation einer Rolle auf der Windows-VM](./virtual-machines-windows-hero-role.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Azure CLI-Beispiele für Windows-VMs](./virtual-machines-windows-cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
