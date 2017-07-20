---
title: "Planen der Kapazität und Skalierung der Hyper-V-VM-Replikation (ohne VMM) mit Azure Site Recovery in Azure | Microsoft-Dokumentation"
description: "Verwenden Sie diesen Artikel zum Planen der Kapazität und der Skalierung beim Replizieren von Hyper-V-VMs in Azure mit Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 8687af60-5b50-481c-98ee-0750cbbc2c57
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 31ecec607c78da2253fcf16b3638cc716ba3ab89
ms.openlocfilehash: c7891c188c2cecbbf056fa79672a13bb16fa7fcf
ms.contentlocale: de-de
ms.lasthandoff: 06/23/2017

---

# <a name="step-3-plan-capacity-and-scaling-for-hyper-v-to-azure-replication"></a>Schritt 3: Planen der Kapazität und Skalierung für Hyper-V für die Replikation in Azure

Verwenden Sie diesen Artikel, um zu ermitteln, wie die Kapazität und die Skalierung beim Replizieren von lokalen Hyper-V-VMs (ohne System Center VMM) in Azure mit [Azure Site Recovery](site-recovery-overview.md) geplant wird.

Nach der Lektüre dieses Artikels können Sie Kommentare am Ende des Artikels oder technische Fragen im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.


## <a name="how-do-i-start-capacity-planning"></a>Wie beginne ich mit der Kapazitätsplanung?


Sie tragen Informationen zu Ihrer Replikationsumgebung zusammen und planen anschließend die Kapazität mithilfe dieser Informationen, zusammen mit den hervorgehobenen Überlegungen in diesem Artikel.


## <a name="gather-information"></a>Sammeln von Informationen

1. Sammeln Sie Informationen zu Ihrer Replikationsumgebung, z.B. VMs, Datenträger pro VM und Speicher pro Datenträger.
2. Ermitteln Sie Ihre tägliche Änderungsrate für replizierte Daten. Laden Sie das [Tool für die Hyper-V-Kapazitätsplanung](https://www.microsoft.com/download/details.aspx?id=39057) herunter, um die Änderungsrate zu erhalten. Es wird empfohlen, das Tool eine Woche lang auszuführen, um Durchschnittswerte zu erfassen.
 

## <a name="figure-out-capacity"></a>Ermitteln der Kapazität

Auf Grundlage der gesammelten Informationen verwenden Sie das Tool [Site Recovery Capacity Planner](http://aka.ms/asr-capacity-planner-excel), um Ihre Quellumgebung und Workloads zu analysieren, die Bandbreitenanforderungen und Serverressourcen zu ermitteln, die sie am Quellstandort benötigen, und die Ressourcen (virtuelle Computer und Speicher usw.) zu eruieren, die Sie am Zielstandort benötigen. Sie können das Tool in einem von zwei Modi ausführen:

- Schnelle Planung: Führen Sie das Tool in diesem Modus aus, um Netzwerk- und Serverhochrechnungen basierend auf der durchschnittlichen Anzahl/Größe von VMs, Datenträgern, Speicher und der Änderungsrate zu erhalten.
- Detaillierte Planung: Führen Sie das Tool in diesem Modus aus, und geben Sie die Details für jede Workload auf VM-Ebene an. Analysieren Sie die VM-Kompatibilität, und rufen Sie Netzwerk- und Serverhochrechnungen ab.

Führen Sie das Tool nun aus:

1. Laden Sie das [Tool](http://aka.ms/asr-capacity-planner-excel) herunter.
2. Um den Quick Planner auszuführen, befolgen Sie [diese Anweisungen](site-recovery-capacity-planner.md#run-the-quick-planner), und wählen Sie das Szenario **Hyper-V to Azure** (Hyper-V zu Azure) aus.
3. Um den Detailed Planner auszuführen, befolgen Sie [diese Anweisungen](site-recovery-capacity-planner.md#run-the-detailed-planner), und wählen Sie das Szenario **Hyper-V to Azure** (Hyper-V zu Azure) aus.

## <a name="control-network-bandwidth"></a>Steuern der Netzwerkbandbreite

Nachdem Sie die benötigte Bandbreite berechnet haben, stehen Ihnen einige Optionen zur Verfügung, mit denen Sie die für die Replikation verwendete Bandbreitenmenge steuern können:

* **Bandbreite drosseln**: Hyper-V-Datenverkehr, der nach Azure repliziert wird, wird über einen speziellen Hyper-V-Host geleitet. Sie können die Bandbreite auf dem Hostserver drosseln.
* **Bandbreite beeinflussen**: Die für die Replikation genutzte Bandbreite lässt sich mithilfe einiger Registrierungsschlüssel beeinflussen.

### <a name="throttle-bandwidth"></a>Bandbreite drosseln
1. Öffnen Sie das Microsoft Azure Backup-MMC-Snap-In auf dem Hyper-V-Hostserver. Standardmäßig ist auf dem Desktop oder unter „C:\Programme\Microsoft Azure Recovery Services Agent\bin\wabadmin“ eine Verknüpfung für Microsoft Azure Backup verfügbar.
2. Klicken Sie im Snap-In auf **Eigenschaften ändern**.
3. Wählen Sie auf der Registerkarte **Drosselung** die Option **Internet-Bandbreiteneinschränkung für Sicherungsvorgänge aktivieren** aus, und legen Sie die Grenzwerte für Geschäftszeiten und arbeitsfreie Zeiten fest. Der gültige Bereich reicht von 512 KBit/s bis 102 MBit/s.

    ![Bandbreite drosseln](./media/hyper-v-site-walkthrough-capacity/throttle2.png)

Sie können auch das Cmdlet [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) verwenden, um die Drosselung festzulegen. Hier ein Beispiel:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** gibt an, dass keine Drosselung erforderlich ist.

### <a name="influence-network-bandwidth"></a>Netzwerkbandbreite beeinflussen
1. Navigieren Sie in der Registrierung zu **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
   * Um die Bandbreite für den Datenverkehr auf einem replizierenden Datenträger zu beeinflussen, ändern Sie den Wert für **UploadThreadsPerVM**, oder erstellen Sie den Schlüssel, falls dieser nicht vorhanden ist.
   * Um die Bandbreite für den Failbackdatenverkehr von Azure zu beeinflussen, ändern Sie den Wert für **DownloadThreadsPerVM**.
2. Der Standardwert ist 4. In einem absichtlich mit großen Reserven ausgestatteten Netzwerk müssen die Standardwerte dieser Registrierungsschlüssel geändert werden. Der maximale Wert beträgt 32. Überwachen Sie den Datenverkehr, um den Wert zu optimieren.

## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 4: Planen von Netzwerken](hyper-v-site-walkthrough-network.md).

