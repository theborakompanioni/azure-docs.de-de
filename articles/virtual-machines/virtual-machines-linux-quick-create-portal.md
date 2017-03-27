---
title: "Azure-Schnellstart – Erstellen einer VM mit dem Portal | Microsoft-Dokumentation"
description: "Azure-Schnellstart – Erstellen einer VM mit dem Portal"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/21/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: bcfd830a5e2f39f36460990cae7e84b04d9a5fbb
ms.lasthandoff: 03/22/2017

---

# <a name="create-a-linux-virtual-machine-with-the-azure-portal"></a>Erstellen einer Linux-VM mit dem Azure-Portal

Virtuelle Azure-Computer können über das Azure-Portal erstellt werden. Diese Methode bietet eine browserbasierte Benutzeroberfläche zum Erstellen und Konfigurieren von virtuellen Computern und alle zugehörigen Ressourcen. In diesem Schnellstart wird beschrieben, wie Sie mit dem Azure-Portal einen virtuellen Computer erstellen. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="create-ssh-key-pair"></a>Erstellen eines SSH-Schlüsselpaars

Für diesen Schnellstart benötigen Sie ein SSH-Schlüsselpaar. Sie können diesen Schritt überspringen, wenn Sie bereits über ein vorhandenes SSH-Schlüsselpaar verfügen. Wenn Sie einen Windows-Computer verwenden, finden Sie [hier](./virtual-machines-linux-ssh-from-windows.md) die richtige Anleitung. 

Führen Sie diesen Befehl über eine Bash-Shell aus, und befolgen Sie die Anweisungen auf dem Bildschirm. Die Ausgabe des Befehls enthält den Dateinamen der Datei mit dem öffentlichen Schlüssel. Sie benötigen den Inhalt dieser Datei beim Erstellen des virtuellen Computers.

```bash
ssh-keygen -t rsa -b 2048
```

## <a name="log-in-to-azure"></a>Anmelden an Azure 

Melden Sie sich unter „http://portal.azure.com“ beim Azure-Portal an.

## <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.

2. Wählen Sie **Berechnen** auf dem Blatt **Neu**, wählen Sie **Ubuntu Server 16.04 LTS** auf dem Blatt **Berechnen**, und klicken Sie dann auf die Schaltfläche **Erstellen**.

3. Füllen Sie das Formular **Grundlagen** für die VM aus. Wählen Sie unter **Authentifizierungstyp** die Option **SSH**. Achten Sie beim Einfügen Ihres **Öffentlichen SSH-Schlüssels** darauf, alle führenden oder nachgestellten Leerzeichen zu entfernen. Erstellen Sie für **Ressourcengruppe** eine neue Ressourcengruppe. Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen erstellt und kollektiv verwaltet werden. Klicken Sie zum Abschluss auf **OK**.

    ![Eingeben grundlegender Informationen zu Ihrem virtuellen Computer im Portalblatt](./media/virtual-machine-quick-start/create-vm-portal-basic-blade.png)  

4. Wählen Sie eine Größe für den virtuellen Computer, und klicken Sie auf **Auswählen**. 

    ![Auswählen einer Größe für Ihren virtuellen Computer im Portalblatt](./media/virtual-machine-quick-start/create-vm-portal-size-blade.png)

5. Wählen Sie auf dem Einstellungenblatt **Ja** unter **Verwaltete Datenträger verwenden**, behalten Sie die Standardwerte für die übrigen Einstellungen bei, und klicken Sie auf **OK**.

6. Klicken Sie auf der Zusammenfassungsseite auf **OK**, um die Bereitstellung des virtuellen Computers zu starten.

7. Um den Bereitstellungsstatus zu überwachen, klicken Sie auf die VM. Sie finden den virtuellen Computer auf dem Dashboard des Azure-Portals oder durch Auswählen von **Virtuelle Computer** im linken Menü. Nachdem der virtuelle Computer erstellt wurde, ändert sich der Status von **Wird bereitgestellt** in **Wird ausgeführt**.

## <a name="connect-to-virtual-machine"></a>Herstellen der Verbindung mit dem virtuellen Computer

Nachdem die Bereitstellung abgeschlossen ist, stellen Sie eine SSH-Verbindung mit dem virtuellen Computer her.

1. Klicken Sie auf dem Blatt des virtuellen Computers auf die Schaltfläche **Verbinden**. Die Verbindungsschaltfläche zeigt eine SSH-Verbindungszeichenfolge an, mit der Sie eine Verbindung mit dem virtuellen Computer herstellen können.

    ![Portal 9](./media/virtual-machine-quick-start/portal-quick-start-9.png) 

2. Führen Sie den folgenden Befehl aus, um eine SSH-Sitzung zu erstellen. Ersetzen Sie die Verbindungszeichenfolge mit derjenigen, die Sie aus dem Azure-Portal kopiert haben.

```bash 
ssh <replace with IP address>
```
## <a name="delete-virtual-machine"></a>Löschen des virtuellen Computers

Wenn Ressourcengruppe, VM und alle zugehörigen Ressourcen nicht mehr benötigt werden, löschen Sie sie. Wählen Sie hierzu die Ressourcengruppe auf dem Blatt für virtuelle Computer aus, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer vollständigen Linux-Umgebung mit der Azure CLI&2;.0](./virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Azure CLI-Beispiele für Linux-VMs](./virtual-machines-linux-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

