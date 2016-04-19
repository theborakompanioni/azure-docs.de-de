<properties
    pageTitle="Erstellen einer Linux-VM mit dem Azure-Portal | Microsoft Azure"
    description="Erstellen Sie eine Linux-VM mit dem Azure-Portal."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"
/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="04/05/2016"
    ms.author="v-livech"
/>

# Erstellen einer Linux-VM mit dem Azure-Portal

In diesem Artikel wird beschrieben, wie Sie das [Azure-Portal](https://portal.azure.com/) zum sofortigen Erstellen einer Linux-VM ohne weitere Installation verwenden. Die einzigen Voraussetzungen sind [ein Azure-Konto](https://azure.microsoft.com/pricing/free-trial/) und [Dateien mit öffentlichen und privaten Schlüsseln](virtual-machines-linux-mac-create-ssh-keys.md).


1. Melden Sie sich mit Ihrer Azure-Kontoidentität am Azure-Portal an, und klicken Sie oben links auf **+ Neu**:

    ![Bildschirm 1](../media/virtual-machines-linux-quick-create-portal/screen1.png)

2. Klicken Sie im **Marketplace** auf **Virtual Machines** und dann in der Imageliste **Ausgewählte Apps** auf **Ubuntu Server 14.04 LTS**. Der folgende Bildschirm wird angezeigt:

    ![Bildschirm 2](../media/virtual-machines-linux-quick-create-portal/screen2.png)

3. Prüfen Sie im unteren Bereich, ob als Bereitstellungsmodelltyp `Resource Manager` verwendet wird, und klicken Sie dann auf **Erstellen**.

4. Gehen Sie auf der Seite **Grundlagen** wie folgt vor:
    - Geben Sie einen Namen für den virtuellen Computer ein.
    - Geben Sie einen Benutzernamen für den Administratorbenutzer ein.
    - Legen Sie den Authentifizierungstyp auf **Öffentlicher SSH-Schlüssel** fest.
    - Verwenden Sie Ihren öffentlichen SSH-Schlüssel als Zeichenfolge (standardmäßig aus dem Verzeichnis `~/.ssh/`).
    - Geben Sie einen Ressourcengruppennamen ein (um eine neue Bereitstellungsgruppe zu erstellen), oder wählen Sie eine vorhandene Gruppe aus.

    Klicken Sie auf **OK**, um fortzufahren und die VM-Größe auszuwählen. Folgendes sollte angezeigt werden:

    ![Bildschirm 3](../media/virtual-machines-linux-quick-create-portal/screen3.png)

5. Wählen Sie die Größe **DS1** aus, bei der Ubuntu auf einem Premium-SSD installiert wird, und klicken Sie auf **Auswählen**, um die Einstellungen zu konfigurieren.

    ![Bildschirm 4](../media/virtual-machines-linux-quick-create-portal/screen4.png)

6. Behalten Sie unter **Einstellungen** die Standardwerte für Speicher und Netzwerk bei, und klicken Sie auf **OK**, um die Zusammenfassung anzuzeigen.

    ![Bildschirm 5](../media/virtual-machines-linux-quick-create-portal/screen5.png)

7. Bestätigen Sie die Einstellungen für die neue Ubuntu-VM, und klicken Sie auf **OK**.

    ![Bildschirm 6](../media/virtual-machines-linux-quick-create-portal/screen6.png)

8. Öffnen Sie das Portal-Dashboard, und wählen Sie unter **Netzwerkschnittstellen** Ihre Netzwerkschnittstelle aus.

    ![Bildschirm 7](../media/virtual-machines-linux-quick-create-portal/screen7.png)

9. Öffnen Sie in den NIC-Einstellungen das Menü „Öffentliche IP-Adresse“.

    ![Bildschirm 8](../media/virtual-machines-linux-quick-create-portal/screen8.png)

10. SSH-Verbindung über die öffentliche IP-Adresse mit Ihrem öffentlichen SSH-Schlüssel

```
user@slackware$ ssh -i ~/.ssh/azure_id_rsa ubuntu@13.91.99.206
```

## Nächste Schritte

Wenn Sie möchten, können Sie auch einen [Datenträger hinzufügen](virtual-machines-linux-add-disk.md).

<!---HONumber=AcomDC_0413_2016-->