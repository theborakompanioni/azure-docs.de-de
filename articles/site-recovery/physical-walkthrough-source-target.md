---
title: "Einrichten der Quelle und des Ziels für die Replikation physischer Server zu Azure mit Azure Site Recovery | Microsoft-Dokumentation"
description: "Dieser Artikel bietet einen Überblick über die Schritte zum Einrichten von Quell- und Zieleinstellungen für die Replikation von physischen Servern zu Azure Storage mit Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 2e3d03bc-4e53-4590-8a01-f702d3cc9500
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: e89bbf5a2c1d71852e49da43d3106a05ebfc28a8
ms.contentlocale: de-de
ms.lasthandoff: 06/29/2017


---
# <a name="step-7-set-up-the-source-and-target-for-physical-server-replication-to-azure"></a>Schritt 7: Einrichten der Quelle und des Ziels für die Replikation physischer Server zu Azure

In diesem Artikel wird beschrieben, wie Sie die Quell- und Zieleinstellungen beim Replizieren von lokalen physischen Servern zu Azure mit [Azure Site Recovery](site-recovery-overview.md) im Azure-Portal konfigurieren.

Kommentare und Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.


## <a name="set-up-the-source-environment"></a>Einrichten der Quellumgebung

Richten Sie den Konfigurationsserver ein, registrieren Sie ihn im Tresor, und ermitteln Sie die Computer.

1. Klicken Sie auf **Site Recovery** > **Schritt 1: Bereiten Sie die Infrastruktur vor** > **Quelle**.
2. Wenn Sie keinen Konfigurationsserver verwenden, klicken Sie auf **+Konfigurationsserver**.
3. Überprüfen Sie unter **Server hinzufügen**, ob unter **Servertyp** die Option **Konfigurationsserver** angezeigt wird.
4. Laden Sie die Installationsdatei für das einheitliche Setup von Site Recovery herunter.
5. Laden Sie den Tresorregistrierungsschlüssel herunter. Sie benötigen diesen beim Ausführen des einheitlichen Setups. Der Schlüssel ist nach der Erstellung fünf Tage lang gültig.

   ![Quelle einrichten](./media/vmware-walkthrough-source-target/set-source2.png)


## <a name="register-the-configuration-server-in-the-vault"></a>Registrieren des Konfigurationsservers im Tresor

Erledigen Sie das Folgende, bevor Sie beginnen, und führen Sie dann das einheitliche Setup aus, um den Konfigurationsserver, den Prozessserver und den Masterzielserver zu installieren. Das Video beschreibt das Einrichten der Komponenten für die Replikation einer VMware-VM zu Azure. Der gleiche Prozess für die Replikation physischer Server zu Azure gültig.

- Stellen Sie auf der Konfigurationsserver-VM sicher, dass die Systemuhr mit einem [Zeitserver](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) synchronisiert ist. Die Zeiten sollten übereinstimmen. Falls der Unterschied 15 Minuten beträgt, ist das Setup unter Umständen nicht erfolgreich.
- Führen Sie das Setup als lokaler Administrator auf dem Konfigurationsservercomputer aus.
- Stellen Sie sicher, dass TLS 1.0 auf der VM aktiviert ist.


[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> Der Konfigurationsserver kann auch [über die Befehlszeile](http://aka.ms/installconfigsrv) installiert werden.




## <a name="set-up-the-target-environment"></a>Einrichten der Zielumgebung

Stellen Sie vor dem Einrichten der Zielumgebung sicher, dass Sie über ein Azure-Speicherkonto und -Netzwerk verfügen.

1. Klicken Sie auf **Infrastruktur vorbereiten** > **Ziel**, und wählen Sie das gewünschte Azure-Abonnement aus.
2. Geben Sie an, ob Ihr Zielbereitstellungsmodell vom Typ „Resource Manager“ oder „klassisch“ ist.
3. Site Recovery prüft, ob Sie über ein oder mehrere kompatible Azure-Speicherkonten und -Netzwerke verfügen.

   ![Ziel](./media/physical-walkthrough-source-target/gs-target.png)

4. Gehen Sie wie folgt vor, falls Sie kein Speicherkonto oder Netzwerk erstellt haben: Klicken Sie auf **+Speicherkonto** oder **+Netzwerk**, um „inline“ ein Resource Manager-Konto oder Netzwerk zu erstellen.

## <a name="next-steps"></a>Nächste Schritte

[Step 8: Set up a replication policy](physical-walkthrough-replication.md) (Schritt 8: Einrichten einer Replikationsrichtlinie)

