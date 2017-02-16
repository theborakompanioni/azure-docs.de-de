---
title: Einrichten der Quellumgebung (VMware in Azure) | Microsoft Docs
description: Dieser Artikel beschreibt, wie Sie Ihre lokale Umgebung einrichten, um mit dem Replizieren von virtuellen VMware-Computern in Azure zu beginnen.
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 1/10/2017
ms.author: anoopkv
translationtype: Human Translation
ms.sourcegitcommit: 9dfcdeb4dc3e84089eb8031272b870f87d90b689
ms.openlocfilehash: 2256ba395e8d5f7a1ce7d4d78168a9cc51d4f074

---

# <a name="set-up-the-source-environment-vmware-to-azure"></a>Einrichten der Quellumgebung (VMware in Azure)
> [!div class="op_single_selector"]
> * [Virtuelle VMware-Computer](./site-recovery-set-up-vmware-to-azure.md)
> * [Physische Server](./site-recovery-set-up-physical-to-azure.md)

Dieser Artikel beschreibt, wie Sie Ihre lokale Umgebung einrichten, um mit dem Replizieren von virtuellen Computern, die unter VMware ausgeführt werden, in Azure zu beginnen.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie Folgendes bereits erstellt haben:
1. Einen Recovery Services-Tresor im [Azure-Portal](http://portal.azure.com "Azure-Portal").
2. Ein dediziertes Konto in VMware vCenter, die für die [automatische Ermittlung](./site-recovery-vmware-to-azure.md#vmware-account-permissions) verwendet werden kann
3. Einen virtuellen Computer, um den Konfigurationsserver zu installieren

### <a name="configuration-server-minimum-requirements"></a>Mindestanforderungen für den Konfigurationsserver
Die Konfigurationsserver-Software muss auf einem **hoch verfügbaren** virtuellen VMware-Computer bereitgestellt werden. Die folgende Tabelle enthält die minimalen Hardware-, Software- und Netzwerkanforderungen für einen Konfigurationsserver.
[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-server-requirements.md)]

> [!NOTE]
> HTTPS-basierte Proxyserver werden vom Konfigurationsserver nicht unterstützt.

## <a name="choose-your-protection-goals"></a>Auswählen der Schutzziele

1. Navigieren Sie im Azure-Portal zum Blatt **Recovery Services-Tresore**, und wählen Sie den Tresor aus.
2. Klicken Sie im Ressourcenmenü des Tresors auf **Erste Schritte** > **Site Recovery** > **Schritt 1: Vorbereiten der Infrastruktur** > **Schutzziel**.

    ![Ziele wählen](./media/site-recovery-set-up-vmware-to-azure/choose-goals.png)
3. Wählen Sie unter **Schutzziel** die Option **To Azure** (Zu Azure) und dann **Yes, with VMware vSphere Hypervisor** (Ja, mit VMware vSphere-Hypervisor) aus. Klicken Sie dann auf **OK**.

    ![Ziele wählen](./media/site-recovery-set-up-vmware-to-azure/choose-goals2.png)

## <a name="set-up-the-source-environment"></a>Einrichten der Quellumgebung
Das Einrichten der Quellumgebung umfasst zwei Hauptaktivitäten

1. Installieren und Registrieren eines Konfigurationsservers beim Site Recovery-Dienst
2. Ermitteln der lokalen virtuellen Computer durch Herstellen einer Verbindung von Azure Site Recovery mit Ihren lokalen Hosts für VMware vCenter oder vSphere EXSi

### <a name="step-1-install--register-a-configuration-server"></a>Schritt 1: Installieren und Registrieren eines Konfigurationsservers

1. Klicken Sie auf **Schritt 1: Bereiten Sie die Infrastruktur vor** > **Quelle**. Falls Sie über keinen Konfigurationsserver verfügen, klicken Sie unter **Quelle vorbereiten** auf **+Konfigurationsserver**, um einen Konfigurationsserver hinzuzufügen.

    ![Quelle einrichten](./media/site-recovery-set-up-vmware-to-azure/set-source1.png)
2. Überprüfen Sie auf dem Blatt **Server hinzufügen**, ob unter **Servertyp** die Option **Konfigurationsserver** angezeigt wird.
4. Laden Sie die Installationsdatei für das einheitliche Setup von Site Recovery herunter.
5. Laden Sie den Tresorregistrierungsschlüssel herunter. Sie benötigen den Registrierungsschlüssel, wenn Sie das einheitliche Setup ausführen. Der Schlüssel ist nach der Erstellung **fünf** Tage lang gültig.

    ![Quelle einrichten](./media/site-recovery-set-up-vmware-to-azure/set-source2.png)
6. Führen Sie auf dem Computer, den Sie als Konfigurationsserver verwenden, das **einheitliche Setup von Azure Site Recovery** aus, um den Konfigurationsserver, den Prozessserver und den Masterzielserver zu installieren.

#### <a name="running-the-azure-site-recovery-unified-setup"></a>Ausführen des einheitlichen Setups für Azure Site Recovery

> [!TIP]
> Die Registrierung des Konfigurationsservers schlägt fehl, wenn die Uhrzeit der Systemuhr auf Ihren Computern um mehr als fünf Minuten von der Ortszeit abweicht. Synchronisieren Sie vor der Installation die Systemuhr mit einem [Zeitserver](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service).

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> Der Konfigurationsserver kann über die Befehlszeile installiert werden. Erfahren Sie mehr über das [Installieren eines Konfigurationsservers mithilfe von Befehlszeilentools](http://aka.ms/installconfigsrv).

#### <a name="add-the-vmware-account-for-automatic-discovery"></a>Fügen Sie das VMware-Konto hinzu, das für die automatische Ermittlung verwendet wird.

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="step-2-add-a-vcenter"></a>Schritt 2: Hinzufügen eines vCenter-Servers
Damit Azure Site Recovery virtuelle Computern in Ihrer lokalen Umgebung ermitteln kann, müssen Sie den VMware vCenter-Server oder die vSphere ESXi-Hosts mit Site Recovery verbinden.

Klicken Sie auf die Schaltfläche „+ vCenter“, um eine Verbindung mit einem VMware vCenter-Server oder VMware vSphere ESXi-Host herzustellen.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]


## <a name="common-issues"></a>Häufige Probleme
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Nächste Schritte
Als Nächstes können Sie [Ihre Zielumgebung](./site-recovery-vmware-to-azure.md#step-3-set-up-the-target-environment) in Azure einrichten.



<!--HONumber=Jan17_HO2-->


