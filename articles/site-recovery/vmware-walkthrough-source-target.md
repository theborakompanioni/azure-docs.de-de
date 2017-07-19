---
title: "Einrichten der Quelle und des Ziel für die VMware-Replikation in Azure mit Azure Site Recovery | Microsoft-Dokumentation"
description: "In diesem Artikel werden die Schritte zum Einrichten der Quell- und Zieleinstellungen für die Replikation von VMware-VMs im Azure-Speicher mit Azure Site Recovery zusammengefasst."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d99e422e-daf7-4fa8-af3c-af2340340136
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: 94b629a62c3a54eee69ee397b2f27e3f20b753d5
ms.contentlocale: de-de
ms.lasthandoff: 06/29/2017


---
# <a name="step-8-set-up-the-source-and-target-for-vmware-replication-to-azure"></a>Schritt 8: Einrichten der Quelle und des Ziel der VMware-Replikation in Azure

In diesem Artikel wird beschrieben, wie Sie die Quell- und Zieleinstellungen beim Replizieren von lokalen VMware-VMs in Azure mit dem [Azure Site Recovery](site-recovery-overview.md)-Dienst im Azure-Portal konfigurieren können.

Kommentare und Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.


## <a name="set-up-the-source-environment"></a>Einrichten der Quellumgebung

Richten Sie den Konfigurationsserver ein, registrieren Sie ihn im Tresor, und ermitteln Sie die VMs.

1. Klicken Sie auf **Site Recovery** > **Schritt 1: Bereiten Sie die Infrastruktur vor** > **Quelle**.
2. Wenn Sie keinen Konfigurationsserver verwenden, klicken Sie auf **+Konfigurationsserver**.
3. Überprüfen Sie unter **Server hinzufügen**, ob unter **Servertyp** die Option **Konfigurationsserver** angezeigt wird.
4. Laden Sie die Installationsdatei für das einheitliche Setup von Site Recovery herunter.
5. Laden Sie den Tresorregistrierungsschlüssel herunter. Sie benötigen diesen beim Ausführen des einheitlichen Setups. Der Schlüssel ist nach der Erstellung fünf Tage lang gültig.

   ![Quelle einrichten](./media/vmware-walkthrough-source-target/set-source2.png)


## <a name="register-the-configuration-server-in-the-vault"></a>Registrieren des Konfigurationsservers im Tresor

Erledigen Sie das Folgende, bevor Sie beginnen, und führen Sie dann das einheitliche Setup aus, um den Konfigurationsserver, den Prozessserver und den Masterzielserver zu installieren.
    - Verschaffen Sie sich schnell einen Überblick per Video

        > [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video1-Source-Infrastructure-Setup/player]

    - Stellen Sie auf der Konfigurationsserver-VM sicher, dass die Systemuhr mit einem [Zeitserver](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) synchronisiert ist. Die Zeiten sollten übereinstimmen. Falls der Unterschied 15 Minuten beträgt, ist das Setup unter Umständen nicht erfolgreich.
    - Führen Sie das Setup als lokaler Administrator auf der Konfigurationsserver-VM aus.
    - Stellen Sie sicher, dass TLS 1.0 auf der VM aktiviert ist.


[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> Der Konfigurationsserver kann auch [über die Befehlszeile](http://aka.ms/installconfigsrv) installiert werden.



## <a name="connect-to-vmware-servers"></a>Herstellen der Verbindung mit VMware-Servern

Damit Azure Site Recovery virtuelle Computern in Ihrer lokalen Umgebung ermitteln kann, müssen Sie den VMware vCenter Server- oder vSphere ESXi-Host mit Site Recovery verbinden. Beachten Sie zunächst Folgendes:

- Wenn Sie den vCenter-Server oder vSphere-Hosts in Site Recovery mit einem Konto ohne Administratorrechte auf dem Server hinzufügen, müssen für das Konto die folgenden Berechtigungen aktiviert sein:
    - Datencenter, Datenspeicher, Ordner, Host, Netzwerk, Ressource, Virtueller Computer, vSphere Distributed Switch.
    - Der vCenter-Server erfordert Anzeigeberechtigungen für den Speicher.
- Beim Hinzufügen von VMware-Servern in Site Recovery kann es 15 Minuten oder auch länger dauern, bis sie im Portal angezeigt werden.

### <a name="add-the-account-for-automatic-discovery"></a>Hinzufügen des Kontos für die automatische Ermittlung

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="set-up-a-connection"></a>Herstellen einer Verbindung

Sie können wie folgt eine Verbindung mit Servern herstellen:

1. Klicken Sie auf **+vCenter**, um eine Verbindung mit einem VMware vCenter Server- oder VMware vSphere ESXi-Host herzustellen.
2. Geben Sie unter **Add vCenter** (vCenter hinzufügen) einen Anzeigenamen für den vSphere-Host oder den vCenter-Server an, und geben Sie anschließend die IP-Adresse oder den FQDN des Servers an.
3. Belassen Sie den Port als Port 443, es sei denn Ihre VMware-Server sind so konfiguriert, dass sie auf Anforderungen auf einem anderen Port lauschen können. Wählen Sie das Konto aus, unter dem die Verbindung mit dem VMware vCenter- oder vSphere ESXi-Server hergestellt werden soll. Klicken Sie auf **OK**.
4. Site Recovery stellt mithilfe der angegebenen Einstellungen eine Verbindung mit VMware-Servern her und ermittelt VMs.

> [!NOTE]
> Wenn Sie einen Server oder Host mit einem Konto hinzufügen, das auf dem vCenter- oder Hostserver keine Administratorrechte besitzt, müssen Sie sicherstellen, dass für das Konto folgende Berechtigungen aktiviert sind: Rechenzentrum, Datenspeicher, Ordner, Host, Netzwerk, Ressource, virtueller Computer und vSphere Distributed Switch. Außerdem benötigt der vCenter-Server von VMware die Anzeigeberechtigung für den Speicher.


## <a name="set-up-the-target-environment"></a>Einrichten der Zielumgebung

Stellen Sie vor dem Einrichten der Zielumgebung sicher, dass Sie über ein Azure-Speicherkonto und -Netzwerk verfügen.

1. Klicken Sie auf **Infrastruktur vorbereiten** > **Ziel**, und wählen Sie das gewünschte Azure-Abonnement aus.
2. Geben Sie an, ob Ihr Zielbereitstellungsmodell vom Typ „Resource Manager“ oder „klassisch“ ist.
3. Site Recovery prüft, ob Sie über ein oder mehrere kompatible Azure-Speicherkonten und -Netzwerke verfügen.

   ![Ziel](./media/vmware-walkthrough-source-target/gs-target.png)
4. Gehen Sie wie folgt vor, falls Sie kein Speicherkonto oder Netzwerk erstellt haben: Klicken Sie auf **+Speicherkonto** oder **+Netzwerk**, um „inline“ ein Resource Manager-Konto oder Netzwerk zu erstellen.

## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 9: Aufstellen einer Replikationsrichtlinie](vmware-walkthrough-replication.md).

