---
title: "Planen der Kapazität und Skalierung der VMware-Replikation in Azure | Microsoft-Dokumentation"
description: "Verwenden Sie diesen Artikel zum Planen der Kapazität und der Skalierung beim Replizieren von VMware-VMs in Azure"
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
ms.date: 02/05/2017
ms.author: rayne
translationtype: Human Translation
ms.sourcegitcommit: 6521cada7adeacd98fae46e5119ceffa0351e9b5
ms.openlocfilehash: a5c6759d9826084ae339dd291140f8383b55b6db


---
# <a name="plan-capacity-and-scaling-for-vmware-replication-with-azure-site-recovery"></a>Planen der Kapazität und Skalierung der VMware-Replikation mit Azure Site Recovery

Verwenden Sie diesen Artikel, um zu ermitteln, wie die Kapazität und die Skalierung beim Replizieren von lokalen VMware-VMs und physischen Servern in Azure mit [Azure Site Recovery](site-recovery-overview.md) geplant wird.

## <a name="how-do-i-start-capacity-planning"></a>Wie beginne ich mit der Kapazitätsplanung?

1. Tragen Sie mit Azure Site Recovery Capacity Planner Informationen zu Ihrer Replikationsumgebung zusammen. Dazu zählen Informationen zu virtuellen Computern, Datenträger pro virtuellem Computer und Speicher pro Datenträger.
2. Schätzen Sie die tägliche Änderungsrate der replizierten Daten in Ihrer Umgebung.


## <a name="gather-information"></a>Sammeln von Informationen

1. Laden Sie [Capacity Planner[(https://gallery.technet.microsoft.com/Azure-Recovery-Capacity-d01dc40e)] herunter, und führen Sie das Tool aus.
2. [Rufen Sie Anweisungen](site-recovery-capacity-planner.md) für die Ausführung des Tools ab.


## <a name="estimate-the-daily-churn-rate"></a>Schätzen der täglichen Änderungsrate

In Site Recovery Capacity Planner müssen Sie eine durchschnittliche tägliche Datenänderungsrate als Prozentsatz eingeben. Derzeit können Sie diese Informationen mithilfe der [Appliance für die vSphere-Kapazitätsplanung](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance) sammeln.

In diesem Tool können Sie den Prozentsatz berechnen, indem Sie das vSphere-Planungstools auf alle Quell-VMs verweisen, sodass Sie alle täglichen Änderungen erhalten. Dies ist im Wesentlichen der Netzwerkdatenverkehr. [Erfahren Sie mehr](https://blogs.vmware.com/vsphere/2014/04/vsphere-replication-capacity-planning-appliance.html) über die Ausführung des Tools.


## <a name="capacity-considerations"></a>Überlegungen zur Kapazität

**Komponente** | **Details** |
--- | --- | ---
**Replikation** | **Maximale tägliche Änderungsrate:** Ein geschützter Computer kann nur einen einzigen Prozessserver verwenden, und ein einzelner Prozessserver kann eine tägliche Änderungsrate von bis zu 2 TB verarbeiten. Daher beträgt die maximale Datenänderungsrate pro Tag, die für einen geschützten Computer unterstützt wird, 2 TB.<br/><br/> **Maximaler Durchsatz:** Ein replizierter Computer kann zu einem einzigen Speicherkonto in Azure gehören. Ein Standardspeicherkonto kann maximal 20.000 Anforderungen pro Sekunde verarbeiten, und es wird empfohlen, dass Sie die Anzahl von IOPS über einen Quellcomputer hinweg bei 20.000 halten. Wenn Ihr Quellcomputer z.B. 5 Datenträger hat und jeder Datenträger 120 IOPS (mit einer Größe von 8 KB) für die Quelle generiert, wird das Azure-Limit von 500 IOPS pro Datenträger eingehalten. Erforderliche Anzahl von Speicherkonten = Quell-IOPS insgesamt/20000.
**Konfigurationsserver** | Der Konfigurationsserver muss in der Lage sein, die tägliche Änderungsrate für alle auf geschützten Computern ausgeführten Workloads zu bewältigen, und er muss über genügend Bandbreite verfügen, um Daten kontinuierlich in den Azure-Speicher zu replizieren.<br/><br/> Als bewährte Methode wird empfohlen, den Konfigurationsserver im selben Netzwerk und LAN-Segment zu platzieren, in dem sich auch die zu schützenden Computer befinden. Er kann in einem anderen Netzwerk platziert werden, aber die zu schützenden Computer sollten für ihn über L3-Netzwerksichtbarkeit verfügen.<br/><br/> Die empfohlenen Größen für den Konfigurationsserver werden in der nachstehenden Tabelle zusammengefasst.
**Prozessserver** | Der erste Prozessserver wird standardmäßig auf dem Konfigurationsserver installiert. Sie können zusätzliche Prozessserver bereitstellen, um Ihre Umgebung zu skalieren. Beachten Sie Folgendes:<br/><br/> Der Prozessserver empfängt Replikationsdaten von geschützten Computern und optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung, bevor er sie an Azure sendet. Der Prozessservercomputer muss über genügend Ressourcen zum Ausführen dieser Aufgaben verfügen.<br/><br/> Der Prozessserver verwendet einen datenträgerbasierten Cache. Wir empfehlen die Verwendung eines separaten Cachedatenträgers mit einer Größe von mindestens 600 GB, um Datenänderungen bei einem Netzwerkengpass oder -ausfall zwischenspeichern und verarbeiten zu können.

## <a name="size-recommendations-for-the-configuration-server"></a>Empfohlene Größen für den Konfigurationsserver

**CPU** | **Arbeitsspeicher** | **Größe des Cachedatenträgers** | **Datenänderungsrate** | **Geschützte Computer**
--- | --- | --- | --- | ---
8 vCPUs (2 Sockets * 4 Kerne @ 2,5 GHz) | 16 GB | 300 GB | 500 GB oder weniger | Weniger als 100 Computer replizieren.
12 vCPUs (2 Sockets * 6 Kerne @ 2,5 GHz) | 18 GB | 600 GB | 500 GB bis 1 TB | Zwischen 100 und 150 Computer replizieren.
16 vCPUs (2 Sockets * 8 Kerne @ 2,5 GHz) | 32 GB | 1 TB | 1 TB bis 2 TB | Zwischen 150 und 200 Computer replizieren.
Bereitstellen eines weiteren Prozessservers | | | > 2 TB | Stellen Sie zusätzliche Prozessserver bereit, wenn Sie mehr als 200 Computer replizieren oder wenn die tägliche Änderungsrate 2 TB überschreitet.

Hierbei gilt:

* Jeder Quellcomputer ist mit 3 Datenträgern von jeweils 100 GB konfiguriert.
* Bei den Benchmarkingmessungen für den Cachedatenträger wurde ein Speicher aus 8 SAS-Laufwerken mit 10.000 U/min und RAID 10 verwendet.

## <a name="size-recommendations-for-the-process-server"></a>Empfohlene Größen für den Prozessserver

Wenn Sie mehr als 200 Computer schützen müssen oder die tägliche Änderungsrate größer als 2 TB ist, können Sie zusätzliche Prozessserver zum Verarbeiten der Replikationslast hinzufügen. Zum horizontalen Hochskalieren haben Sie folgende Möglichkeiten:

* Erhöhen Sie die Anzahl von Konfigurationsservern. Beispielsweise können Sie mit zwei Konfigurationsservern bis zu 400 Computer schützen.
* Fügen Sie zusätzliche Prozessserver hinzu, und verwenden Sie diese anstelle des Konfigurationsservers (oder als Ergänzung zu diesem) für die Abwicklung des Datenverkehrs.

Die folgende Tabelle beschreibt dieses Szenario:

* Sie planen nicht, den Konfigurationsserver als Prozessserver zu verwenden.
* Sie haben einen zusätzlichen Prozessserver eingerichtet.
* Sie haben geschützte virtuelle Computer so konfiguriert, dass sie den zusätzlichen Prozessserver verwenden.
* Jeder geschützte Quellcomputer ist mit drei Datenträgern von jeweils 100 GB konfiguriert.

**Konfigurationsserver** | **Zusätzlicher Prozessserver** | **Größe des Cachedatenträgers** | **Datenänderungsrate** | **Geschützte Computer**
--- | --- | --- | --- | ---
8 vCPUs (2 Sockets * 4 Kerne @ 2,5 GHz), 16 GB Speicher | 4 vCPUs (2 Sockets * 2 Kerne @ 2,5 GHz), 8 GB Speicher | 300 GB | 250 GB oder weniger | Bis zu 85 Computer replizieren.
8 vCPUs (2 Sockets * 4 Kerne @ 2,5 GHz), 16 GB Speicher | 8 vCPUs (2 Sockets * 4 Kerne @ 2,5 GHz), 12 GB Speicher | 600 GB | 250 GB bis 1 TB | Zwischen 85 und 150 Computer replizieren.
12 vCPUs (2 Sockets * 6 Kerne @ 2,5 GHz), 18 GB Speicher | 12 vCPUs (2 Sockets * 6 Kerne @ 2,5 GHz), 24 GB Speicher | 1 TB | 1 TB bis 2 TB | Zwischen 150 und 225 Computer replizieren.

Wie Sie Ihre Server skalieren, hängt davon ab, ob Sie das zentrale Hochskalieren oder das horizontale Hochskalieren als Modell bevorzugen.  Beim zentralen Hochskalieren stellen Sie wenige besonders leistungsstarke Konfigurations- und Prozessserver bereit. Beim horizontalen Hochskalieren stellen Sie mehr Server mit geringeren Ressourcen bereit. Wenn Sie zum Beispiel 220 Computer schützen müssen, können Sie sich für eine der beiden folgenden Optionen entscheiden:

* Richten Sie den Konfigurationsserver mit 12 vCPU und 18 GB Arbeitsspeicher und einen zusätzlichen Prozessserver mit 12 vCPU und 24 GB Arbeitsspeicher ein, und konfigurieren Sie die geschützten Computer so, dass sie nur den zusätzlichen Prozessserver verwenden.
* Alternativ können Sie zwei Konfigurationsserver (2 x 8vCPU, 16 GB RAM) und zwei zusätzliche Prozessserver (1 x 8vCPU und 1 x 4vCPU zur Handhabung von 135 + 85 (220) Computern) konfigurieren und geschützte Computer so konfigurieren, dass sie nur die zusätzlichen Prozessserver verwenden.


## <a name="control-network-bandwidth"></a>Steuern der Netzwerkbandbreite

Sie können den Capacity Planner verwenden, um die Bandbreite zu berechnen, die Sie für die Replikation benötigen (erste Replikation und dann Deltareplikation). Zur Steuerung der für die Replikation verwendeten Bandbreite stehen Ihnen verschiedene Optionen zur Verfügung:

* **Bandbreite drosseln**: VMware-Datenverkehr, der nach Azure repliziert wird, wird über einen speziellen Prozessserver geleitet. Sie können die Bandbreite auf den Computern, die als Prozessserver ausgeführt werden, einschränken.
* **Bandbreite beeinflussen**: Die für die Replikation genutzte Bandbreite lässt sich mithilfe einiger Registrierungsschlüssel beeinflussen:
  * Der Registrierungswert **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\UploadThreadsPerVM** gibt die Anzahl von Threads an, die für die Datenübertragung (erste Replikation oder Deltareplikation) eines Datenträgers verwendet werden. Bei einem höheren Wert wird die Netzwerkbandbreite für die Replikation erhöht.
  * Der Registrierungswert **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\DownloadThreadsPerVM** gibt die Anzahl von Threads an, die für die Datenübertragung während eines Failbacks verwendet werden.

### <a name="throttle-bandwidth"></a>Bandbreite drosseln
1. Öffnen Sie das Microsoft Azure Backup-MMC-Snap-In auf dem Computer, der als Prozessserver fungiert. Standardmäßig ist auf dem Desktop oder unter „C:\Programme\Microsoft Azure Recovery Services Agent\bin\wabadmin“ eine Verknüpfung für Microsoft Azure Backup verfügbar.
2. Klicken Sie im Snap-In auf **Eigenschaften ändern**.

    ![Bandbreite einschränken](./media/site-recovery-vmware-to-azure/throttle1.png)
3. Wählen Sie auf der Registerkarte **Drosselung** die Option **Internet-Bandbreiteneinschränkung für Sicherungsvorgänge aktivieren** aus, und legen Sie die Grenzwerte für Geschäftszeiten und arbeitsfreie Zeiten fest. Der gültige Bereich reicht von 512 KBit/s bis 102 MBit/s.

    ![Bandbreite drosseln](./media/site-recovery-vmware-to-azure/throttle2.png)

Sie können auch das Cmdlet [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) verwenden, um die Drosselung festzulegen. Hier ein Beispiel:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** gibt an, dass keine Drosselung erforderlich ist.

#### <a name="influence-network-bandwidth-for-a-vm"></a>Beeinflussen der Netzwerkbandbreite für eine VM

1. Navigieren Sie in der Registrierung der VM zu **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
   * Um die Bandbreite für den Datenverkehr auf einem replizierenden Datenträger zu beeinflussen, ändern Sie den Wert für **UploadThreadsPerVM**, oder erstellen Sie den Schlüssel, falls dieser nicht vorhanden ist.
   * Um die Bandbreite für den Failbackdatenverkehr von Azure zu beeinflussen, ändern Sie den Wert für **DownloadThreadsPerVM**.
2. Der Standardwert ist 4. In einem absichtlich mit großen Reserven ausgestatteten Netzwerk müssen die Standardwerte dieser Registrierungsschlüssel geändert werden. Der maximale Wert beträgt 32. Überwachen Sie den Datenverkehr, um den Wert zu optimieren.


## <a name="deploy-additional-process-servers"></a>Bereitstellen zusätzlicher Prozessserver

Wenn Sie Ihre Bereitstellung über 200 Quellcomputer oder eine gesamte tägliche Änderungsrate von 2 TB hinaus horizontal hochskalieren müssen, benötigen Sie zusätzliche Prozessserver zur Bewältigung des Datenverkehrsaufkommens. Befolgen Sie diese Anweisungen, um den Prozessserver einzurichten. Nachdem Sie den Prozessserver eingerichtet haben, migrieren Sie Quellcomputer, damit diese ihn verwenden können.

1. Klicken Sie unter **Site Recovery-Server** auf den Konfigurationsserver und anschließend auf **Prozessserver**.

    ![Prozessserver hinzufügen](./media/site-recovery-vmware-to-azure/migrate-ps1.png)
2. Klicken Sie unter **Servertyp** auf **Process server (on-premises)** (Prozessserver (lokal)).

    ![Prozessserver hinzufügen](./media/site-recovery-vmware-to-azure/migrate-ps2.png)
3. Laden Sie die Datei für das einheitliche Setup von Site Recovery herunter, führen Sie sie zum Installieren des Prozessservers aus, und registrieren Sie sie im Tresor.
4. Wählen Sie unter **Before you begin** die Option **Add additional process servers to scale out deployment** (Weitere Prozessserver zum horizontalen Hochskalieren der Bereitstellung hinzufügen) aus.
5. Schließen Sie den Assistenten genau wie beim [Einrichten](#step-2-set-up-the-source-environment) des Konfigurationsservers ab.

    ![Prozessserver hinzufügen](./media/site-recovery-vmware-to-azure/add-ps1.png)
6. Geben Sie unter **Configuration Server Details** die IP-Adresse des Konfigurationsservers und die Passphrase an. Führen Sie **<SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe –n** auf dem Konfigurationsserver aus, um die Passphrase abzurufen.

    ![Prozessserver hinzufügen](./media/site-recovery-vmware-to-azure/add-ps2.png)

### <a name="migrate-machines-to-use-the-new-process-server"></a>Migrieren der Computer für die Verwendung des neuen Prozessservers
1. Klicken Sie unter **Einstellungen** > **Site Recovery servers** auf den Konfigurationsserver, und erweitern Sie anschließend **Prozessserver**.

    ![Prozessserver aktualisieren](./media/site-recovery-vmware-to-azure/migrate-ps2.png)
2. Klicken Sie mit der rechten Maustaste auf den aktuell verwendeten Prozessserver, und klicken Sie auf **Wechseln**.

    ![Prozessserver aktualisieren](./media/site-recovery-vmware-to-azure/migrate-ps3.png)
3. Wählen Sie unter **Zielprozessserver auswählen** den neuen Prozessserver aus, den Sie verwenden möchten, und wählen Sie anschließend die virtuellen Computer aus, für die der neue Prozessserver zuständig sein wird. Klicken Sie auf das Informationssymbol, um Informationen über den Server zu erhalten. Der durchschnittliche zum Replizieren jedes ausgewählten virtuellen Computers auf den neuen Prozessserver benötigte Speicherplatz wird angezeigt, um Sie bei Lastenentscheidungen zu unterstützen. Klicken Sie auf das Häkchen, um mit dem Replizieren auf den neuen Prozessserver zu beginnen.










<!--HONumber=Feb17_HO2-->


