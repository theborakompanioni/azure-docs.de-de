---
title: "Planen der Kapazität und Skalierung der VMware-Replikation mit Azure Site Recovery | Microsoft-Dokumentation"
description: "Verwenden Sie diesen Artikel zum Planen der Kapazität und der Skalierung beim Replizieren von VMware-VMs in Azure mit Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 0a1cd8eb-a8f7-4228-ab84-9449e0b2887b
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/24/2017
ms.author: rayne
ms.translationtype: Human Translation
ms.sourcegitcommit: a30a90682948b657fb31dd14101172282988cbf0
ms.openlocfilehash: 8b580ac239bfb6d7b633fb03d4cfb91b168b0610
ms.contentlocale: de-de
ms.lasthandoff: 05/25/2017


---
# <a name="plan-capacity-and-scaling-for-vmware-replication-with-azure-site-recovery"></a>Planen der Kapazität und Skalierung der VMware-Replikation mit Azure Site Recovery

Verwenden Sie diesen Artikel, um zu ermitteln, wie die Kapazität und die Skalierung beim Replizieren von lokalen VMware-VMs und physischen Servern in Azure mit [Azure Site Recovery](site-recovery-overview.md) geplant wird.

## <a name="how-do-i-start-capacity-planning"></a>Wie beginne ich mit der Kapazitätsplanung?

Sammeln Sie für die VMware-Replikation mit dem [Azure Site Recovery Deployment Planner](https://aka.ms/asr-deployment-planner-doc) Informationen zu Ihrer Replikationsumgebung. [Hier](site-recovery-deployment-planner.md) finden Sie weitere Informationen zu diesem Tool. Sie sammeln Informationen über kompatible und inkompatible VMs, Datenträger pro VM und Datenänderungen pro Datenträger. Zudem behandelt das Tool Anforderungen an Netzwerkbandbreite und die erforderliche Azure-Infrastruktur für eine erfolgreiche Replikation und ein erfolgreiches Testfailover.

## <a name="capacity-considerations"></a>Überlegungen zur Kapazität

**Komponente** | **Details** |
--- | --- | ---
**Replikation** | **Maximale tägliche Änderungsrate:** Ein geschützter Computer kann nur einen einzigen Prozessserver verwenden, und ein einzelner Prozessserver kann eine tägliche Änderungsrate von bis zu 2 TB verarbeiten. Daher beträgt die maximale Datenänderungsrate pro Tag, die für einen geschützten Computer unterstützt wird, 2 TB.<br/><br/> **Maximaler Durchsatz:** Ein replizierter Computer kann zu einem einzigen Speicherkonto in Azure gehören. Ein Standardspeicherkonto kann maximal 20.000 Anforderungen pro Sekunde verarbeiten, und es wird empfohlen, dass Sie die Anzahl von Eingabe-/Ausgabevorgängen pro Sekunde (IOPS) über einen Quellcomputer hinweg bei 20.000 zu halten. Wenn Ihr Quellcomputer z.B. 5 Datenträger aufweist und jeder Datenträger 120 IOPS (mit einer Größe von 8 KB) für den Quellcomputer generiert, wird das Azure-Limit von 500 IOPS pro Datenträger eingehalten. (Die Anzahl der erforderlichen Speicherkonten entspricht dem IOPS-Gesamtwert der Quellcomputer, geteilt durch 20.000.)
**Konfigurationsserver** | Der Konfigurationsserver muss in der Lage sein, die tägliche Änderungsrate für alle auf geschützten Computern ausgeführten Workloads zu bewältigen, und er muss über genügend Bandbreite verfügen, um Daten kontinuierlich in den Azure-Speicher zu replizieren.<br/><br/> Als bewährte Methode sollten Sie den Konfigurationsserver im selben Netzwerk und LAN-Segment platzieren, in dem sich auch die zu schützenden Computer befinden. Er kann in einem anderen Netzwerk platziert werden, aber die zu schützenden Computer sollten für ihn über Layer 3-Netzwerksichtbarkeit verfügen.<br/><br/> Die empfohlenen Größen für den Konfigurationsserver werden in der Tabelle im folgenden Abschnitt zusammengefasst.
**Prozessserver** | Der erste Prozessserver wird standardmäßig auf dem Konfigurationsserver installiert. Sie können zusätzliche Prozessserver bereitstellen, um Ihre Umgebung zu skalieren. <br/><br/> Der Prozessserver empfängt Replikationsdaten von geschützten Computern und optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung. Anschließend sendet er die Daten an Azure. Der Prozessservercomputer muss über genügend Ressourcen zum Ausführen dieser Aufgaben verfügen.<br/><br/> Der Prozessserver verwendet einen datenträgerbasierten Cache. Verwenden Sie einen separaten Cachedatenträger mit einer Größe von mindestens 600 GB, um Datenänderungen bei einem Netzwerkengpass oder -ausfall zwischenspeichern und verarbeiten zu können.

## <a name="size-recommendations-for-the-configuration-server"></a>Empfohlene Größen für den Konfigurationsserver

**CPU** | **Arbeitsspeicher** | **Größe des Cachedatenträgers** | **Datenänderungsrate** | **Geschützte Computer**
--- | --- | --- | --- | ---
8 vCPUs (2 Sockets * 4 Kerne mit 2,5 GHz) | 16 GB | 300 GB | 500 GB oder weniger | Weniger als 100 Computer replizieren.
12 vCPUs (2 Sockets * 6 Kerne mit 2,5 GHz) | 18 GB | 600 GB | 500 GB bis 1 TB | Zwischen 100 und 150 Computer replizieren.
16 vCPUs (2 Sockets * 8 Kerne mit 2,5 GHz) | 32 GB | 1 TB | 1 TB bis 2 TB | Zwischen 150 und 200 Computer replizieren.
Bereitstellen eines weiteren Prozessservers | | | > 2 TB | Stellen Sie zusätzliche Prozessserver bereit, wenn Sie mehr als 200 Computer replizieren oder wenn die tägliche Änderungsrate 2 TB überschreitet.

Hierbei gilt:

* Jeder Quellcomputer ist mit 3 Datenträgern von jeweils 100 GB konfiguriert.
* Bei den Benchmarkingmessungen für den Cachedatenträger wurde ein Speicher aus 8 SAS-Laufwerken mit 10.000 U/min und RAID 10 verwendet.

## <a name="size-recommendations-for-the-process-server"></a>Empfohlene Größen für den Prozessserver

Wenn Sie mehr als 200 Computer schützen müssen oder die tägliche Änderungsrate größer als 2 TB ist, können Sie Prozessserver zum Verarbeiten der Replikationslast hinzufügen. Zum horizontalen Hochskalieren haben Sie folgende Möglichkeiten:

* Erhöhen Sie die Anzahl von Konfigurationsservern. Beispielsweise können Sie mit zwei Konfigurationsservern bis zu 400 Computer schützen.
* Fügen Sie weitere Prozessserver hinzu, und verwenden Sie diese anstelle des Konfigurationsservers (oder als Ergänzung zu diesem) für die Abwicklung des Datenverkehrs.

Die folgende Tabelle beschreibt dieses Szenario:

* Sie planen nicht, den Konfigurationsserver als Prozessserver zu verwenden.
* Sie haben einen zusätzlichen Prozessserver eingerichtet.
* Sie haben geschützte virtuelle Computer so konfiguriert, dass sie den zusätzlichen Prozessserver verwenden.
* Jeder geschützte Quellcomputer ist mit drei Datenträgern von jeweils 100 GB konfiguriert.

**Konfigurationsserver** | **Zusätzlicher Prozessserver** | **Größe des Cachedatenträgers** | **Datenänderungsrate** | **Geschützte Computer**
--- | --- | --- | --- | ---
8 vCPUs (2 Sockets * 4 Kerne mit 2,5 GHz), 16 GB Arbeitsspeicher | 4 vCPUs (2 Sockets * 2 Kerne mit 2,5 GHz), 8 GB Arbeitsspeicher | 300 GB | 250 GB oder weniger | Maximal 85 Computer replizieren.
8 vCPUs (2 Sockets * 4 Kerne mit 2,5 GHz), 16 GB Arbeitsspeicher | 8 vCPUs (2 Sockets * 4 Kerne mit 2,5 GHz), 12 GB Arbeitsspeicher | 600 GB | 250 GB bis 1 TB | Zwischen 85 und 150 Computer replizieren.
12 vCPUs (2 Sockets * 6 Kerne mit 2,5 GHz), 18 GB Arbeitsspeicher | 12 vCPUs (2 Sockets * 6 Kerne mit 2,5 GHz), 24 GB Arbeitsspeicher | 1 TB | 1 TB bis 2 TB | Zwischen 150 und 225 Computer replizieren.

Wie Sie Ihre Server skalieren, hängt davon ab, ob Sie das zentrale Hochskalieren oder das horizontale Hochskalieren als Modell bevorzugen.  Beim zentralen Hochskalieren stellen Sie wenige besonders leistungsstarke Konfigurations- und Prozessserver bereit. Beim horizontalen Hochskalieren stellen Sie mehr Server mit geringeren Ressourcen bereit. Wenn Sie zum Beispiel 220 Computer schützen müssen, können Sie sich für eine der beiden folgenden Optionen entscheiden:

* Richten Sie den Konfigurationsserver mit 12 vCPUs, 18 GB Arbeitsspeicher, und einen zusätzlichen Prozessserver mit 12 vCPUs, 24 GB Speicher ein. Konfigurieren Sie die geschützten Computer so, dass sie nur den zusätzlichen Prozessserver verwenden.
* Konfigurieren Sie zwei Konfigurationsserver (2 x 8 vCPUs, 16 GB RAM) und zwei zusätzliche Prozessserver (1 x 8 vCPUs und 1 x 4 vCPUs für 135 + 85 (220) Computer). Konfigurieren Sie die geschützten Computer so, dass sie nur die zusätzlichen Prozessserver verwenden.


## <a name="control-network-bandwidth"></a>Steuern der Netzwerkbandbreite

Nachdem Sie mit dem [Deployment Planner-Tool](site-recovery-deployment-planner.md) die Bandbreite berechnet haben, die für die Replikation erforderlich ist (die erste Replikation und anschließende Deltas), können Sie die Menge der für die Replikation verwendeten Bandbreite mit einer Reihe von Optionen steuern:

* **Bandbreite drosseln**: VMware-Datenverkehr, der nach Azure repliziert wird, wird über einen speziellen Prozessserver geleitet. Sie können die Bandbreite auf den Computern, die als Prozessserver ausgeführt werden, einschränken.
* **Bandbreite beeinflussen**: Die für die Replikation genutzte Bandbreite lässt sich mithilfe einiger Registrierungsschlüssel beeinflussen:
  * Der Registrierungswert **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\UploadThreadsPerVM** gibt die Anzahl von Threads an, die für die Datenübertragung (erste Replikation oder Deltareplikation) eines Datenträgers verwendet werden. Bei einem höheren Wert wird die Netzwerkbandbreite für die Replikation erhöht.
  * Der Registrierungswert **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\DownloadThreadsPerVM** gibt die Anzahl von Threads an, die für die Datenübertragung während eines Failbacks verwendet werden.

### <a name="throttle-bandwidth"></a>Bandbreite drosseln

1. Öffnen Sie das Azure Backup-MMC-Snap-In auf dem Computer, der als Prozessserver fungiert. Standardmäßig ist auf dem Desktop oder im Ordner „C:\Programme\Microsoft Azure Recovery Services Agent\bin\wabadmin“ eine Verknüpfung für Backup verfügbar.
2. Klicken Sie im Snap-In auf **Eigenschaften ändern**.

    ![Screenshot der Azure Backup-MMC-Snap-In-Option zum Ändern von Eigenschaften](./media/site-recovery-vmware-to-azure/throttle1.png)
3. Aktivieren Sie auf der Registerkarte **Drosselung** die Option **Internet-Bandbreiteneinschränkung für Sicherungsvorgänge aktivieren**. Legen Sie die Grenzwerte für Arbeitsstunden und arbeitsfreie Stunden fest. Der gültige Bereich reicht von 512 KBit/s bis 102 MBit/s.

    ![Screenshot des Dialogfeld „Eigenschaften von Azure Backup“](./media/site-recovery-vmware-to-azure/throttle2.png)

Sie können auch das Cmdlet [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) verwenden, um die Drosselung festzulegen. Hier ein Beispiel:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** gibt an, dass keine Drosselung erforderlich ist.

### <a name="influence-network-bandwidth-for-a-vm"></a>Beeinflussen der Netzwerkbandbreite für eine VM

1. Navigieren Sie in der Registrierung der VM zu **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
   * Um die Bandbreite für den Datenverkehr auf einem replizierenden Datenträger zu beeinflussen, ändern Sie den Wert von **UploadThreadsPerVM**, oder erstellen Sie den Schlüssel, falls dieser nicht vorhanden ist.
   * Um die Bandbreite für den Failbackdatenverkehr von Azure zu beeinflussen, ändern Sie den Wert von **DownloadThreadsPerVM**.
2. Der Standardwert ist 4. In einem absichtlich mit großen Reserven ausgestatteten Netzwerk müssen die Standardwerte dieser Registrierungsschlüssel geändert werden. Der maximale Wert beträgt 32. Überwachen Sie den Datenverkehr, um den Wert zu optimieren.


## <a name="deploy-additional-process-servers"></a>Bereitstellen zusätzlicher Prozessserver

Wenn Sie Ihre Bereitstellung über 200 Quellcomputer horizontal hochskalieren müssen oder Ihre gesamte tägliche Änderungsrate 2 TB übersteigt, benötigen Sie zusätzliche Prozessserver zur Bewältigung des Datenverkehrsaufkommens. Befolgen Sie diese Anweisungen, um den Prozessserver einzurichten. Nachdem Sie den Prozessserver eingerichtet haben, migrieren Sie Quellcomputer, damit diese ihn verwenden können.

1. Klicken Sie unter **Site Recovery-Server** auf den Konfigurationsserver und anschließend auf **Prozessserver**.

    ![Screenshot der Option „Site Recovery-Server“ zum Hinzufügen eines Prozessservers](./media/site-recovery-vmware-to-azure/migrate-ps1.png)
2. Klicken Sie unter **Servertyp** auf **Prozessserver (lokal)**.

    ![Screenshot des Dialogfelds „Prozessserver“](./media/site-recovery-vmware-to-azure/migrate-ps2.png)
3. Laden Sie die Datei für das einheitliche Setup von Site Recovery herunter, und führen Sie sie aus, um den Prozessserver zu installieren. Damit wird sie auch im Tresor registriert.
4. Wählen Sie unter **Before you begin** die Option **Add additional process servers to scale out deployment** (Weitere Prozessserver zum horizontalen Hochskalieren der Bereitstellung hinzufügen) aus.
5. Schließen Sie den Assistenten genau wie beim [Einrichten](#step-2-set-up-the-source-environment) des Konfigurationsservers ab.

    ![Screenshot des Assistenten für das einheitliche Setup für Azure Site Recovery](./media/site-recovery-vmware-to-azure/add-ps1.png)
6. Geben Sie unter **Configuration Server Details** die IP-Adresse des Konfigurationsservers und die Passphrase an. Um die Passphrase abzurufen, führen Sie auf dem Konfigurationsserver **[Site Recovery-Installationsordner]\home\svsystems\bin\genpassphrase.exe –n** aus.

    ![Screenshot der Seite „Konfigurationsserverdetails“](./media/site-recovery-vmware-to-azure/add-ps2.png)

### <a name="migrate-machines-to-use-the-new-process-server"></a>Migrieren der Computer für die Verwendung des neuen Prozessservers
1. Klicken Sie unter **Einstellungen** > **Site Recovery servers** auf den Konfigurationsserver, und erweitern Sie anschließend **Prozessserver**.

    ![Screenshot des Dialogfelds „Prozessserver“](./media/site-recovery-vmware-to-azure/migrate-ps2.png)
2. Klicken Sie mit der rechten Maustaste auf den aktuell verwendeten Prozessserver, und klicken Sie auf **Wechseln**.

    ![Screenshot des Dialogfelds „Konfigurationsserver“](./media/site-recovery-vmware-to-azure/migrate-ps3.png)
3. Wählen Sie unter **Zielprozessserver auswählen** den neuen Prozessserver aus, den Sie verwenden möchten, und wählen Sie anschließend die virtuellen Computer aus, für die der Server zuständig sein wird. Klicken Sie auf das Informationssymbol, um Informationen über den Server zu erhalten. Der durchschnittliche zum Replizieren jedes ausgewählten virtuellen Computers auf den neuen Prozessserver benötigte Speicherplatz wird angezeigt, um Sie bei Lastenentscheidungen zu unterstützen. Klicken Sie auf das Häkchen, um mit dem Replizieren auf den neuen Prozessserver zu beginnen.


## <a name="next-steps"></a>Nächste Schritte

Laden Sie den [Azure Site Recovery Deployment Planner](https://aka.ms/asr-deployment-planner) herunter, und installieren Sie ihn.

