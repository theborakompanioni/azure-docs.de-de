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
ms.date: 04/03/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: 26d460a699e31f6c19e3b282fa589ed07ce4a068
ms.openlocfilehash: 3cb94144addabf2371c8a058cebff25def4b6338
ms.lasthandoff: 04/04/2017

---

# <a name="create-a-windows-virtual-machine-with-the-azure-portal"></a>Erstellen einer Windows-VM im Azure-Portal

Virtuelle Azure-Computer können über das Azure-Portal erstellt werden. Diese Methode bietet eine browserbasierte Benutzeroberfläche zum Erstellen und Konfigurieren von virtuellen Computern und alle zugehörigen Ressourcen. In diesem Schnellstart wird beschrieben, wie Sie mit dem Azure-Portal einen virtuellen Computer erstellen. Nach Abschluss der Bereitstellung stellen wir eine Verbindung mit dem Server her und installieren IIS.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich unter „http://portal.azure.com“ beim Azure-Portal an.

## <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers

2. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.

3. Wählen Sie **Berechnen** auf dem Blatt **Neu**, wählen Sie **Windows Server 2016 Datacenter** auf dem Blatt **Berechnen**, und klicken Sie dann auf die Schaltfläche **Erstellen**.

4. Füllen Sie das Formular **Grundlagen** für die VM aus. Der hier eingegebene Benutzername und das Kennwort werden zum Anmelden am virtuellen Computer verwendet. Erstellen Sie für **Ressourcengruppe** eine neue Ressourcengruppe. Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen erstellt und kollektiv verwaltet werden. Klicken Sie zum Abschluss auf **OK**.

    ![Eingeben grundlegender Informationen zu Ihrem virtuellen Computer im Portalblatt](./media/quick-create-portal/create-windows-vm-portal-basic-blade.png)  

5. Wählen Sie eine Größe für den virtuellen Computer, und klicken Sie auf **Auswählen**.

6. Wählen Sie auf dem Einstellungenblatt **Ja** unter **Verwaltete Datenträger verwenden**, behalten Sie die Standardwerte für die übrigen Einstellungen bei, und klicken Sie auf **OK**.

7. Klicken Sie auf der Zusammenfassungsseite auf **OK**, um die Bereitstellung des virtuellen Computers zu starten.

8. Um den Bereitstellungsstatus zu überwachen, klicken Sie auf die VM. Sie finden den virtuellen Computer auf dem Dashboard des Azure-Portals oder durch Auswählen von **Virtuelle Computer** im linken Menü. Nachdem der virtuelle Computer erstellt wurde, ändert sich der Status von **Wird bereitgestellt** in **Wird ausgeführt**.

## <a name="open-port-80-for-web-traffic"></a>Öffnen von Port 80 für Webdatenverkehr 

Um IIS-Datenverkehr zuzulassen, muss Port 80 für Webdatenverkehr geöffnet werden. In diesem Schritt erfahren Sie, wie Sie eine Regel für eine Netzwerksicherheitsgruppe (NSG) erstellen, um eingehende Verbindungen an Port 80 zuzulassen.

1. Klicken Sie auf dem Blatt für den virtuellen Computer im Abschnitt **Zusammenfassung** auf den Namen der **Ressourcengruppe**.
2. Klicken Sie auf dem Blatt für die Ressourcengruppe in der Ressourcenliste auf die **Netzwerksicherheitsgruppe**. Der NSG-Name muss eine Kombination aus dem Namen des virtuellen Computers und dem Suffix „-nsg“ sein.
3. Klicken Sie auf die Überschrift **Eingangssicherheitsregel**, um die Eingangsregelliste zu öffnen. Die Liste müsste bereits eine Regel für RDP enthalten.
4. Klicken Sie auf **+ Hinzufügen**, um das Blatt **Eingangssicherheitsregel hinzufügen** zu öffnen.
5. Geben Sie unter **Name** die Zeichenfolge **IIS** ein, und vergewissern Sie sich, dass **Portbereich** auf „80“ und **Aktion** auf **Zulassen** festgelegt ist. Klicken Sie anschließend auf **OK**.


## <a name="connect-to-virtual-machine"></a>Herstellen der Verbindung mit dem virtuellen Computer

Nachdem die Bereitstellung abgeschlossen ist, stellen Sie eine Remotedesktopverbindung mit dem virtuellen Computer her.

1. Klicken Sie auf dem Blatt des virtuellen Computers auf die Schaltfläche **Verbinden**. Eine Remotedesktopprotokoll-Datei (RDP-Datei) wird erstellt und heruntergeladen.

    ![Portal 9](./media/quick-create-portal/quick-create-portal/portal-quick-start-9.png) 

2. Öffnen Sie die heruntergeladene RDP-Datei, um eine Verbindung mit Ihrem virtuellen Computer herzustellen. Klicken Sie in der angezeigten Aufforderung auf **Verbinden**. Auf einem Macintosh benötigen Sie einen RDP-Client, z.B. diesen [Remotedesktopclient](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) aus dem Mac App Store.

3. Geben Sie den Benutzernamen und das Kennwort ein, den bzw. das Sie beim Erstellen des virtuellen Computers festgelegt haben, und klicken Sie anschließend auf **OK**.

4. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Klicken Sie auf **Ja** bzw. **Weiter**, um mit dem Herstellen der Verbindung fortzufahren.


## <a name="install-iis-using-powershell"></a>Installieren von IIS mithilfe von PowerShell

Nach der Anmeldung bei dem virtuellen Computer können Sie mit einer einzelnen PowerShell-Codezeile IIS installieren und die lokale Firewallregel aktivieren, um Webdatenverkehr zuzulassen.  Öffnen Sie eine PowerShell-Eingabeaufforderung, und führen Sie den folgenden Befehl aus:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-the-iis-welcome-page"></a>Anzeigen der IIS-Willkommensseite

Nachdem Sie IIS installiert und Port 80 auf Ihrem virtuellen Computer für den Zugriff über das Internet geöffnet haben, können Sie nun mit einem Webbrowser Ihrer Wahl die IIS-Standardwillkommensseite anzeigen. Ermitteln Sie auf dem Blatt für den virtuellen Computer die **öffentliche IP-Adresse**, und verwenden Sie sie, um die Standardwebsite aufzurufen. 

![IIS-Standardwebsite](./media/quick-create-powershell/default-iis-website.png) 

## <a name="delete-virtual-machine"></a>Löschen des virtuellen Computers

Wenn Ressourcengruppe, VM und alle zugehörigen Ressourcen nicht mehr benötigt werden, können Sie sie mit dem folgenden Befehl entfernen.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```
## <a name="delete-virtual-machine"></a>Löschen des virtuellen Computers

Wenn Ressourcengruppe, VM und alle zugehörigen Ressourcen nicht mehr benötigt werden, löschen Sie sie. Wählen Sie hierzu die Ressourcengruppe auf dem Blatt für virtuelle Computer aus, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

[Experimentieren mit der Installation einer Rolle auf der Windows-VM](hero-role.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Azure CLI-Beispiele für Windows-VMs](cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

