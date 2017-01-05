---
title: "Planen der Kapazität für den Schutz virtueller Computer und physischer Server in Azure Site Recovery | Microsoft Docs"
description: "Planen der Replikationskapazität für Azure Site Recovery"
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
ms.date: 11/15/2016
ms.author: nisoneji
translationtype: Human Translation
ms.sourcegitcommit: 8fca992488746bc50a7342644de62b862a9aaea7
ms.openlocfilehash: a427969378d2454246217d48bafe6f360e93df0f


---
# <a name="plan-capacity-for-protecting-virtual-machines-and-physical-servers-in-azure-site-recovery"></a>Planen der Kapazität für den Schutz virtueller Computer und physischer Server in Azure Site Recovery

Mit dem Tool „Azure Site Recovery Capacity Planner“ können Sie ermitteln, welche Kapazitätsanforderungen für die Replikation von Hyper-V-VMs, VMware-VMs und physischen Windows-/Linux-Servern mit Azure Site Recovery gelten.

Verwenden Sie Site Recovery Capacity Planner, um Ihre Quellumgebung und Workloads zu analysieren, die Bandbreitenanforderungen und Serverressourcen zu ermitteln, die sie am Quellstandort benötigen, und die Ressourcen (virtuelle Computer und Speicher usw.) zu eruieren, die Sie am Zielstandort benötigen.

Sie können das Tool in einem von zwei Modi ausführen:

* **Schnelle Planung**: Führen Sie das Tool in diesem Modus aus, um Netzwerk- und Serverhochrechnungen basierend auf der durchschnittlichen Anzahl/Größe von VMs, Datenträgern, Speicher und der Änderungsrate zu erhalten.
* **Detaillierte Planung**: Führen Sie das Tool in diesem Modus aus, und geben Sie die Details für jede Workload auf VM-Ebene an. Analysieren Sie die VM-Kompatibilität, und rufen Sie Netzwerk- und Serverhochrechnungen ab.

## <a name="before-you-start"></a>Vorbereitung


1. Sammeln Sie Informationen zu Ihrer Umgebung, z. B. VMs, Festplatten pro VM, Speicher pro Datenträger.
2. Ermitteln Sie Ihre tägliche Änderungsrate für replizierte Daten. Gehen Sie dazu folgendermaßen vor:

   * Laden Sie beim Replizieren von Hyper-V-VMs das [Tool für die Hyper-V-Kapazitätsplanung](https://www.microsoft.com/download/details.aspx?id=39057) herunter, um die Änderungsrate zu erhalten. [Hier](site-recovery-capacity-planning-for-hyper-v-replication.md) finden Sie weitere Informationen zu diesem Tool. Es wird empfohlen, das Tool eine Woche lang auszuführen, um Durchschnittswerte zu erfassen.
   * Verwenden Sie beim Replizieren von virtuellen VMware-Maschinen die [Appliance für die vSphere-Kapazitätsplanung](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance) , um die Änderungsrate zu ermitteln.
   * Wenn Sie physische Server replizieren, müssen Sie eine manuelle Schätzung erstellen.

## <a name="run-the-quick-planner"></a>Ausführen des Quick Planner-Tools
1. Laden Sie das Tool [Azure Site Recovery Capacity Planner](http://aka.ms/asr-capacity-planner-excel) herunter, und öffnen Sie es. Sie müssen Makros ausführen. Aktivieren Sie also die Bearbeitung und den Inhalt, wenn Sie dazu aufgefordert werden.
2. Wählen Sie unter **Select a planner type** (Planertyp auswählen) im Listenfeld die Option **Quick Planner** (Schnellplaner) aus.

   ![Erste Schritte](./media/site-recovery-capacity-planner/getting-started.png)
3. Geben Sie im Arbeitsblatt **Capacity Planner** (Kapazitätsplaner) die erforderlichen Informationen ein. Sie müssen alle Felder ausfüllen, die unten im Screenshot rot eingekreist sind.

   * Wählen Sie unter **Select your scenario** (Ihr Szenario auswählen) die Option **Hyper-V to Azure** (Hyper-V zu Azure) oder **VMware/Physical to Azure** (VMware/Physisch zu Azure) aus.
   * Geben Sie unter **Average daily data change rate (%)** (Durchschnittliche tägliche Änderungsrate (%)) die Informationen ein, die Sie mit dem [Tool für die Hyper-V-Kapazitätsplanung](site-recovery-capacity-planning-for-hyper-v-replication.md) oder der [Appliance für die vSphere-Kapazitätsplanung](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance) erfasst haben.  
   * **Compression** gilt nur für die Komprimierung, die beim Replizieren von VMware-VMs oder physischen Servern zu Azure angeboten wird. Wir schätzen 30 % oder mehr, aber Sie können die Einstellung nach Bedarf ändern. Sie können zum Replizieren von Hyper-V-VMs zur Azure-Komprimierung ein Drittanbietergerät verwenden, z.B. Riverbed.
   * Geben Sie unter **Retention Inputs** (Aufbewahrungseingaben) an, wie lange Replikate aufbewahrt werden sollen. Geben Sie beim Replizieren von VMware- oder physischen Servern den Wert in Tagen ein. Geben Sie die Dauer in Stunden ein, wenn Sie eine Hyper-V-Replikation durchführen.
   * Geben Sie unter **Number of hours in which initial replication for the batch of virtual machines should complete** (Anzahl der Stunden für den Abschluss der anfänglichen Replikation für den Batch virtueller Computer) und **Number of virtual machines per initial replication batch** (Anzahl virtueller Computer pro anfänglichem Replikationsbatch) die Einstellungen ein, die zum Berechnen der Anforderungen für die erste Replikation verwendet werden.  Bei der Bereitstellung von Site Recovery sollte das gesamte Anfangsdataset hochgeladen werden.

   ![Eingaben](./media/site-recovery-capacity-planner/inputs.png)
4. Nachdem Sie die Werte für die Quellumgebung eingegeben haben, enthält die angezeigte Ausgabe Folgendes:

   * **Bandwidth required for delta replication** (Erforderliche Bandbreite für die Deltareplikation) (MB/Sek). Die Netzwerkbandbreite für die Deltareplikation wird basierend auf der durchschnittlichen täglichen Datenänderungsrate berechnet.
   * **Bandwidth required for initial replication** (Erforderliche Bandbreite für die erste Replikation) (MB/Sek). Die Netzwerkbandbreite für die erste Replikation wird basierend auf den Werten für die erste Replikation berechnet, die Sie eingeben.
   * **Erforderlicher Speicher (in GB)** : Gibt den insgesamt erforderlichen Azure-Speicher an.
   * **Gesamt-IOPS für Standardspeicherkonten** : Wird basierend auf der Einheitengröße 8K IOPS für alle Standardspeicherkonten berechnet.  Für den Quick Planner wird die Zahl basierend auf allen VM-Quelldatenträgern und der täglichen Änderungsrate berechnet. Für den Detailed Planner (Detaillierter Planer) wird die Zahl basierend auf der Gesamtzahl der VMs, die standardmäßigen Azure-VMs zugeordnet sind, und der Datenänderungsrate auf diesen VMs berechnet.
   * **Anzahl von Standard-Speicherkonten** : Gibt die Gesamtzahl von Standard-Speicherkonten an, die zum Schützen der VMs benötigt werden. Beachten Sie, dass ein Standardspeicherkonto bis zu 20.000 IOPS über alle VMs in einem Standardspeicher hinweg mit einer maximalen Unterstützung von 500 IOPS pro Datenträger leisten kann.
   * **Anzahl erforderlicher Blobdatenträger** : Gibt die Anzahl von Datenträgern an, die im Azure-Speicher erstellt werden.
   * **Anzahl erforderlicher Storage Premium-Konten** : Gibt die Gesamtzahl von Storage Premium-Konten an, die zum Schützen der VMs benötigt werden. Beachten Sie, dass für eine Quell-VM mit hohem IOPS-Wert (höher als 20.000) ein Premium-Speicherkonto erforderlich ist. Ein Premium-Speicherkonto kann bis zu 80.000 IOPS leisten.
   * **Gesamt-IOPS für Storage Premium** : Wird basierend auf der Einheitengröße 256K IOPS für alle Storage Premium-Konten berechnet.  Für den Quick Planner wird die Zahl basierend auf allen VM-Quelldatenträgern und der täglichen Änderungsrate berechnet. Für den Detailed Planner wird die Zahl basierend auf der Gesamtzahl der VMs, die Azure-Premium-VMs (DS- und GS-Reihe) zugeordnet sind, und der Datenänderungsrate auf diesen VMs berechnet.
   * **Number of configuration servers required** (Anzahl der erforderlichen Konfigurationsserver) zeigt, wie viele Konfigurationsserver für die Bereitstellung erforderlich sind. 
   * **Number of additional process servers required** (Anzahl der erforderlichen zusätzlichen Prozessserver) zeigt, ob weitere Prozessserver außer dem Prozessserver erforderlich sind, der standardmäßig auf dem Konfigurationsserver ausgeführt wird.
   * **100 % zusätzlicher Speicher für die Quelle** zeigt an, ob am Quellspeicherort zusätzlicher Speicher erforderlich ist.

   ![Ausgabe](./media/site-recovery-capacity-planner/output.png)

## <a name="run-the-detailed-planner"></a>Ausführen des Detailed Planner-Tools

1. Laden Sie das Tool [Azure Site Recovery Capacity Planner](http://aka.ms/asr-capacity-planner-excel) herunter, und öffnen Sie es. Sie müssen Makros ausführen. Aktivieren Sie also die Bearbeitung und den Inhalt, wenn Sie dazu aufgefordert werden.
2. Wählen Sie unter **Select a planner type** im Listenfeld die Option **Detailed Planner** aus.

   ![Erste Schritte](./media/site-recovery-capacity-planner/getting-started-2.png)
3. Geben Sie im Arbeitsblatt **Workload Qualification** (Workload-Qualifikation) die erforderlichen Informationen ein. Sie müssen alle markierten Felder ausfüllen.

   * Geben Sie unter **Processor cores** (Prozessorkerne) die Gesamtzahl der Kerne auf einem Quellserver an.
   * Geben Sie unter **Memory allocation in MB** (Speicherbelegung in MB) die RAM-Größe eines Quellservers an.
   * Geben Sie unter **Number of NICs** (Anzahl der Netzwerkkarten) die Anzahl an Netzwerkadaptern auf einem Quellserver an.
   * Geben Sie unter **Total storage (in GB)** (Gesamtspeicher (in GB)) die Gesamtgröße des VM-Speichers an. Wenn der Quellserver beispielsweise über drei Datenträger mit jeweils 500 GB verfügt, beträgt die Gesamtgröße 1.500 GB.
   * Geben Sie unter **Number of disks attached** (Anzahl der angefügten Datenträger) die Gesamtzahl der Datenträger eines Quellservers an.
   * Geben Sie unter **Disk capacity utilization** (Auslastung der Datenträgerkapazität) die durchschnittliche Auslastung an.
   * Geben Sie unter **Daily change rate (%)** (Tägliche Änderungsrate (%)) die tägliche Änderungsrate eines Quellservers an.
   * Geben Sie unter **Mapping Azure size** (Zuordnung der Azure-Größe) die Größe der Azure-VM ein, die Sie zuordnen möchten. Wenn Sie keine manuelle Eingabe durchführen möchten, klicken Sie auf **Compute IaaS VMs** (IaaS-VMs berechnen).Wenn Sie eine manuelle Einstellung eingeben und dann auf „Compute IaaS VMs“ klicken, könnte die manuelle Einstellung überschrieben werden, da der Berechnungsprozesses automatisch die beste Übereinstimmung mit der Azure-VM-Größe erkennt.

   ![Workload-Qualifikation](./media/site-recovery-capacity-planner/workload-qualification.png)
4. Wenn Sie auf **IaaS-VMs berechnen** klicken, wird Folgendes durchgeführt:

   * Die obligatorischen Eingaben werden überprüft.
   * Der IOPS-Wert wird berechnet, und es wird die beste Azure-VM-Größenübereinstimmung für jede VM ermittelt, die für die Replikation zu Azure geeignet ist. Falls keine passende Größe einer Azure-VM erkannt werden kann, wird ein Fehler ausgegeben. Wenn die Anzahl der angefügten Datenträger beispielsweise 65 beträgt, wird ein Fehler ausgegeben, da der höchste zulässige Wert für die Azure-VM-Größe 64 beträgt.
   * Es wird ein Speicherkonto vorgeschlagen, das für eine Azure-VM verwendet werden kann.
   * Es wird berechnet, wie viele Standard-Speicherkonten und wie viele Premium-Speicherkonten für die Workload insgesamt erforderlich sind. Scrollen Sie nach unten, um den Azure-Speichertyp und das Speicherkonto anzuzeigen, die für einen Quellserver verwendet werden können.
   * Der Rest der Tabelle wird basierend auf dem erforderlichen Speichertyp (Standard oder Premium), der einer VM zugewiesen ist, und der Anzahl von Datenträgern ausgefüllt und sortiert. Für alle VMs, die die Anforderungen für Azure erfüllen, wird in der Spalte **Is VM qualified?** (VM qualifiziert?) **Yes** (Ja) angezeigt. Wenn eine VM nicht unter Azure gesichert werden kann, wird ein Fehler angezeigt.

Die Spalten AA bis AE werden ausgegeben und enthalten Informationen für jede VM.

![Workload-Qualifikation](./media/site-recovery-capacity-planner/workload-qualification-2.png)

### <a name="example"></a>Beispiel
Beispielsweise berechnet das Tool für sechs VMs mit den in der Tabelle angezeigten Werten die beste Azure-VM-Übereinstimmung und die Azure-Speicheranforderungen und weist sie zu.

![Workload-Qualifikation](./media/site-recovery-capacity-planner/workload-qualification-3.png)

* Beachten Sie Folgendes in der Beispielausgabe:

  * Die erste Spalte enthält die Validierung für die VMS, die Datenträger und die Wechselrate.
  * Zwei Standard-Speicherkonten und ein Premium-Speicherkonto werden für fünf VMs benötigt.
  * VM3 erfüllt die Qualifikation für den Schutz nicht, da mindestens ein Datenträger größer als 1 TB ist.
  * Für VM1 und VM2 kann das erste Standard-Speicherkonto verwendet werden.
  * Für VM4 kann das zweite Standard-Speicherkonto verwendet werden.
  * VM5 und VM6 benötigen ein Premium-Speicherkonto, und beide können ein einzelnes Konto verwenden.

    > [!NOTE]
    > Der IOPS-Wert für Standard- und Premium-Speicher wird auf VM-Ebene und nicht auf Datenträgerebene berechnet. Ein standardmäßiger virtueller Computer kann bis zu 500 IOPS pro Datenträger verarbeiten. Wenn der IOPS-Wert für einen Datenträger mehr als 500 beträgt, benötigen Sie einen Premium-Speicher. Wenn der IOPS-Wert jedoch für einen Datenträger über 500 liegt, aber der IOPS-Wert für alle VM-Datenträger innerhalb der standardmäßigen Unterstützungsgrenzen (VM-Größe, Anzahl der Datenträger, Anzahl der Adapter, CPU, Arbeitsspeicher) für Azure-VMs liegt, wählt der Planer statt der DS- oder GS-Reihe eine Standard-VM aus. Sie müssen die Zuordnung der Azure-Größenzelle manuell mit einer geeigneten VM aus der DS- oder GS-Reihe aktualisieren.
   

1. Klicken Sie nach dem Einrichten aller Details auf **Submit data to the planner tool** (Daten an das Planungstool senden), um den **Capacity Planner** zu öffnen. Die Workloads sind hervorgehoben, um anzuzeigen, ob sie für den Schutz geeignet sind oder nicht.

### <a name="submit-data-in-the-capacity-planner"></a>Senden von Daten im Capacity Planner
1. Beim Öffnen wird das Arbeitsblatt **Capacity Planner** basierend auf den Einstellungen, die Sie angegeben haben, gefüllt. In der Zelle **Infra inputs source** (Infrastruktureingabenquelle) wird „Workload“ angezeigt, um zu zeigen, dass die Eingabe das Arbeitsblatt **Workload Qualification** ist.
2. Änderungen müssen Sie ggf. im Arbeitsblatt **Workload Qualification** vornehmen und erneut auf **Submit data to the planner tool** klicken.  

   ![Capacity Planner](./media/site-recovery-capacity-planner/capacity-planner.png)



<!--HONumber=Nov16_HO3-->


