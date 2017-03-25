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
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/10/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: c1cd1450d5921cf51f720017b746ff9498e85537
ms.openlocfilehash: 348407b57bbb3329d7d27a6f38623e052aecc58b
ms.lasthandoff: 03/14/2017

---

# <a name="create-a-linux-virtual-machine-with-the-azure-portal"></a>Erstellen einer Linux-VM mit dem Azure-Portal

Virtuelle Azure-Computer können über das Azure-Portal erstellt werden. Diese Methode bietet eine browserbasierte Benutzeroberfläche zum Erstellen und Konfigurieren von virtuellen Computern und alle zugehörigen Azure-Ressourcen.

Bevor Sie beginnen, benötigen Sie sowohl einen privaten als auch einen öffentlichen SSH-Schlüssel. Ausführliche Informationen zum Erstellen von SSH-Schlüsseln für Azure finden Sie unter [Erstellen eines öffentlich-privaten SSH-Schlüsselpaars für virtuelle Linux-Computer](./virtual-machines-linux-mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="log-in-to-azure"></a>Anmelden an Azure 

Melden Sie sich unter „http://portal.azure.com“ beim Azure-Portal an.

## <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers

2. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.

3. Wählen Sie **Berechnen** auf dem Blatt **Neu**, wählen Sie **Ubuntu Server 16.04 LTS** auf dem Blatt **Berechnen**, und klicken Sie dann auf die Schaltfläche **Erstellen**.

4. Füllen Sie das Formular **Grundlagen** für die VM aus. Für **Authentifizierungstyp** wird „SSH“ empfohlen. Achten Sie beim Einfügen Ihres **Öffentlichen SSH-Schlüssels** darauf, alle führenden oder nachgestellten Leerzeichen zu entfernen. Erstellen Sie für **Ressourcengruppe** eine neue Ressourcengruppe. Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen erstellt und kollektiv verwaltet werden. Klicken Sie zum Abschluss auf **OK**.

    ![Eingeben grundlegender Informationen zu Ihrem virtuellen Computer im Portalblatt](./media/virtual-machine-quick-start/create-vm-portal-basic-blade.png)  

5. Wählen Sie eine Größe für den virtuellen Computer, und klicken Sie auf **Auswählen**. 

    ![Auswählen einer Größe für Ihren virtuellen Computer im Portalblatt](./media/virtual-machine-quick-start/create-vm-portal-size-blade.png)

6. Wählen Sie auf dem Einstellungenblatt **Ja** unter **Verwaltete Datenträger verwenden**, behalten Sie die Standardwerte für die übrigen Einstellungen bei, und klicken Sie auf **OK**.

7. Klicken Sie auf der Zusammenfassungsseite auf **OK**, um die Bereitstellung des virtuellen Computers zu starten.

## <a name="connect-to-virtual-machine"></a>Herstellen der Verbindung mit dem virtuellen Computer

Nachdem die Bereitstellung abgeschlossen ist, stellen Sie eine SSH-Verbindung mit dem virtuellen Computer her.

1. Klicken Sie auf den virtuellen Computer. Sie finden den virtuellen Computer auf dem Startbildschirm des Azure-Portals oder durch Auswählen von **Virtuelle Computer** im linken Menü.

2. Klicken Sie auf die Schaltfläche **Verbinden** . Die Verbindungsschaltfläche zeigt eine SSH-Verbindungszeichenfolge an, mit der Sie eine Verbindung mit dem virtuellen Computer herstellen können.

    ![Portal 9](./media/virtual-machine-quick-start/portal-quick-start-9.png) 

3. Führen Sie den folgenden Befehl aus, um eine SSH-Sitzung zu erstellen. Ersetzen Sie die Verbindungszeichenfolge mit derjenigen, die Sie aus dem Azure-Portal kopiert haben.

```bash 
ssh <replace with IP address>
```
## <a name="delete-virtual-machine"></a>Löschen des virtuellen Computers

Wenn Ressourcengruppe, VM und alle zugehörigen Ressourcen nicht mehr benötigt werden, löschen Sie sie. Wählen Sie hierzu die Ressourcengruppe auf dem Blatt für virtuelle Computer aus, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer vollständigen Linux-Umgebung mit der Azure CLI&2;.0](./virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Azure CLI-Beispiele für Linux-VMs](./virtual-machines-linux-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

