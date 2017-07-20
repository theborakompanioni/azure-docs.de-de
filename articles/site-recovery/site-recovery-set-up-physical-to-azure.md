---
title: Einrichten der Quellumgebung (physische Server in Azure) | Microsoft-Dokumentation
description: Dieser Artikel beschreibt, wie Sie Ihre lokale Umgebung einrichten, um mit dem Replizieren von physischen Servern unter Windows oder Linux in Azure zu beginnen.
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
ms.workload: storage-backup-recovery
ms.date: 06/29/2017
ms.author: anoopkv
ms.translationtype: Human Translation
ms.sourcegitcommit: 3716c7699732ad31970778fdfa116f8aee3da70b
ms.openlocfilehash: 49b9d2e21dbcb612828a25f21ed4382327d6f64c
ms.contentlocale: de-de
ms.lasthandoff: 06/30/2017

---

# <a name="set-up-the-source-environment-physical-server-to-azure"></a>Einrichten der Quellumgebung (physische Server in Azure)
> [!div class="op_single_selector"]
> * [VMware zu Azure](./site-recovery-set-up-vmware-to-azure.md)
> * [Physisch nach Azure](./site-recovery-set-up-physical-to-azure.md)

Dieser Artikel beschreibt, wie Sie Ihre lokale Umgebung einrichten, um mit dem Replizieren von physischen Servern unter Windows oder Linux in Azure zu beginnen.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie über Folgendes bereits verfügen:
1. Einen Recovery Services-Tresor im [Azure-Portal](http://portal.azure.com "Azure-Portal").
3. Einen physischen Computer, um den Konfigurationsserver zu installieren.

### <a name="configuration-server-minimum-requirements"></a>Mindestanforderungen für den Konfigurationsserver
Die folgende Tabelle enthält die minimalen Hardware-, Software- und Netzwerkanforderungen für einen Konfigurationsserver.
[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

> [!NOTE]
> HTTPS-basierte Proxyserver werden vom Konfigurationsserver nicht unterstützt.

## <a name="choose-your-protection-goals"></a>Auswählen der Schutzziele

1. Navigieren Sie im Azure-Portal zum Blatt **Recovery Services-Tresore**, und wählen Sie den Tresor aus.
2. Klicken Sie im Menü **Ressource** des Tresors auf **Erste Schritte** > **Site Recovery** > **Schritt 1: Vorbereiten der Infrastruktur** > **Schutzziel**.

    ![Ziele wählen](./media/site-recovery-set-up-physical-to-azure/choose-goals.png)
3. Wählen Sie unter **Schutzziel** die Option **In Azure** und dann **Nicht virtualisiert/Andere** aus. Klicken Sie anschließend auf **OK**.

    ![Ziele wählen](./media/site-recovery-set-up-physical-to-azure/physical-protection-goal.PNG)

## <a name="set-up-the-source-environment"></a>Einrichten der Quellumgebung

1. Falls Sie über keinen Konfigurationsserver verfügen, klicken Sie unter **Quelle vorbereiten** auf **+Konfigurationsserver**, um einen Konfigurationsserver hinzuzufügen.

  ![Quelle einrichten](./media/site-recovery-set-up-physical-to-azure/plus-config-srv.png)
2. Überprüfen Sie auf dem Blatt **Server hinzufügen**, ob unter **Servertyp** die Option **Konfigurationsserver** angezeigt wird.
4. Laden Sie die Installationsdatei für das einheitliche Setup von Site Recovery herunter.
5. Laden Sie den Tresorregistrierungsschlüssel herunter. Sie benötigen den Registrierungsschlüssel, wenn Sie das einheitliche Setup ausführen. Der Schlüssel ist nach der Erstellung fünf Tage lang gültig.

    ![Quelle einrichten](./media/site-recovery-set-up-physical-to-azure/set-source2.png)
6. Führen Sie auf dem Computer, den Sie als Konfigurationsserver verwenden, das **einheitliche Setup von Azure Site Recovery** aus, um den Konfigurationsserver, den Prozessserver und den Masterzielserver zu installieren.

#### <a name="run-azure-site-recovery-unified-setup"></a>Ausführen des einheitlichen Setups von Azure Site Recovery

> [!TIP]
> Die Registrierung des Konfigurationsservers schlägt fehl, wenn die Systemuhr auf Ihrem Computer um mehr als fünf Minuten von der Ortszeit abweicht. Synchronisieren Sie vor der Installation die Systemuhr mit einem [Zeitserver](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service).

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> Der Konfigurationsserver kann über eine Befehlszeile installiert werden. Weitere Informationen finden Sie im Thema zum [Installieren eines Konfigurationsservers mithilfe von Befehlszeilentools](http://aka.ms/installconfigsrv).


## <a name="common-issues"></a>Häufige Probleme

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Nächste Schritte

Als Nächstes können Sie [Ihre Zielumgebung](./site-recovery-prepare-target-physical-to-azure.md) in Azure einrichten.

