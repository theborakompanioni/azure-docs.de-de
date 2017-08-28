---
title: Entwerfen und Implementieren einer Oracle-Datenbank in Azure | Microsoft-Dokumentation
description: Entwerfen und implementieren Sie eine Oracle-Datenbank in Ihrer Azure-Umgebung.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: v-shiuma
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 6/22/2017
ms.author: rclaus
ms.translationtype: HT
ms.sourcegitcommit: b6c65c53d96f4adb8719c27ed270e973b5a7ff23
ms.openlocfilehash: 9bd9ff35cebc2aeaf0558dcf1a37887050682f0a
ms.contentlocale: de-de
ms.lasthandoff: 08/17/2017

---


# <a name="design-and-implement-an-oracle-database-in-azure"></a>Entwerfen und Implementieren einer Oracle-Datenbank in Azure

## <a name="assumptions"></a>Annahmen

- Sie planen die Migration einer Oracle-Datenbank von einem lokalen Standort zu Azure.
- Sie haben den Überblick über verschiedene Metriken von Oracle-AWR-Berichten.
- Sie besitzen Grundkenntnisse über Anwendungsleistung und Plattformnutzung.

## <a name="goals"></a>Ziele

- Sie möchten wissen, wie Sie Ihre Oracle-Bereitstellung in Azure optimieren können.
- Entdecken der Optionen zur Leistungsoptimierung einer Oracle-Datenbank in einer Azure-Umgebung.

## <a name="the-differences-between-an-on-premises-and-azure-implementation"></a>Die Unterschiede zwischen einer lokalen und Azure-Implementierung 

Im Folgenden sind einige wichtige Aspekte aufgeführt, die Sie beachten müssen, wenn Sie lokale Anwendungen in Azure migrieren. 

Ein wichtiger Unterschied besteht darin, dass in einer Azure-Implementierung Ressourcen wie virtuelle Computer, Festplatten und virtuelle Netzwerke für andere Clients freigegeben werden. Darüber hinaus können Ressourcen je nach Anforderungen gedrosselt werden. Anstatt sich auf das Vermeiden von Ausfällen zu konzentrieren (MTBF), ist Azure eher darauf ausgerichtet, Ausfälle zu überstehen (MTTR).

Die folgende Tabelle enthält einige der Unterschiede zwischen einer lokalen Implementierung und einer Azure-Implementierung von Oracle-Datenbanken.

> 
> |  | **Lokale Implementierung** | **Azure-Implementierung** |
> | --- | --- | --- |
> | **Netzwerk** |LAN/WAN  |SDN (Software-Defined Networking)|
> | **Sicherheitsgruppe** |Tools für IP/Port-Einschränkungen |[Netzwerksicherheitsgruppe (NSG)](https://azure.microsoft.com/blog/network-security-groups) |
> | **Resilienz** |MTBF (Mean Time Between Failure, mittlere Betriebsdauer zwischen Ausfällen) |MTTR (Mean Time To Recover, mittlere Reparaturzeit)|
> | **Geplante Wartung** |Patchen/Upgrades|[Verfügbarkeitsgruppen](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) (Patchen/Upgrades werden von Azure verwaltet) |
> | **Ressource** |Dediziert  |Für andere Clients freigegeben|
> | **Regionen** |Datencenter |[Regionspaare](https://docs.microsoft.com/azure/virtual-machines/windows/regions-and-availability)|
> | **Speicher** |SAN-/Physische Datenträger |[Von Azure verwalteter Speicher](https://azure.microsoft.com/pricing/details/managed-disks/?v=17.23h)|
> | **Skalieren** |Vertikale Skalierung |Horizontale Skalierung|


### <a name="requirements"></a>Anforderungen

- Bestimmen Sie Größe und Wachstumsrate der Datenbank.
- Bestimmen Sie die IOPS-Anforderungen, die Sie anhand des Oracle-AWR-Berichts oder anderer Netzwerküberwachungstools einschätzen können.

## <a name="configuration-options"></a>Konfigurationsoptionen

Es gibt vier mögliche Bereiche, die Sie optimieren können, um die Leistung in einer Azure-Umgebung zu verbessern:

- Größe des virtuellen Computers
- Netzwerkdurchsatz
- Datenträgertypen und -konfigurationen
- Cacheeinstellungen von Datenträgern

### <a name="generate-an-awr-report"></a>Generieren eines AWR-Berichts

Wenn Sie über eine Oracle-Datenbank verfügen und die Migration in Azure planen, können Sie zwischen mehreren Optionen wählen. Sie können den Oracle-AWR Bericht zum Abrufen der Metriken (IOPS, Mbit/s, GiB/Sek. usw.) ausführen. Wählen Sie dann die VM auf Basis der Metriken aus, die Sie erfasst haben. Sie können sich aber auch an Ihr Infrastrukturteam wenden, um ähnliche Informationen zu erhalten.

Sie können den AWR-Bericht auch während der regulären und der maximalen Workload ausführen, sodass Sie die Ergebnisse vergleichen können. Anhand dieser Berichte können Sie die Größe der VMs dann entweder nach der durchschnittlichen oder maximalen Workload auswählen.

Im folgenden Beispiel wird gezeigt, wie ein AWR-Bericht generiert wird:

```bash
$ sqlplus / as sysdba
SQL> EXEC DBMS_WORKLOAD_REPOSITORY.CREATE_SNAPSHOT;
SQL> @?/rdbms/admin/awrrpt.sql
```

### <a name="key-metrics"></a>Die wichtigsten Metriken

Folgende Metriken können Sie aus dem AWR-Bericht abrufen:

- Gesamtanzahl von Kernen
- CPU-Taktfrequenz
- Gesamter Speicher in GB
- CPU-Auslastung
- Maximale Datenübertragungsrate
- Rate der E/A-Änderungen (Lesen/Schreiben)
- Redo-Protokollrate (MBit/s)
- Netzwerkdurchsatz
- Netzwerklatenzrate (niedrig/hoch)
- Datenbankgröße in GB
- Bytes von/an Client über SQL*Net

### <a name="virtual-machine-size"></a>Größe des virtuellen Computers

#### <a name="1-estimate-vm-size-based-on-cpu-memory-and-io-usage-from-the-awr-report"></a>1. Schätzen der Größe der VM basierend auf CPU-, Speicher- und E/A-Auslastung gemäß AWR-Bericht

Sie können sich unter anderem die fünf wichtigsten zeitgesteuerten Vordergrundereignisse ansehen, die auf die Engpässe im System hinweisen.

In der folgenden Abbildung ist die Protokolldateisynchronisierung z.B. oben. Die Anzahl von Wartevorgängen wird angezeigt, die erforderlich sind, bevor LGWR den Protokollpuffer in die Redo-Protokolldatei schreibt. Diese Ergebnisse zeigen, dass ein leistungsfähigerer Speicher oder leistungsfähigere Datenträger erforderlich sind. Darüber hinaus werden auch die Anzahl von CPUs (Kernen) und die Speichermenge im Diagramm angezeigt.

![Screenshot der AWR-Berichtsseite](./media/oracle-design/cpu_memory_info.png)

Die folgenden Diagramme zeigen das gesamte E/A-Volumen der Lese- und Schreibvorgänge. Während der Berichtszeit wurden 59 GB gelesen und 247,3 GB geschrieben.

![Screenshot der AWR-Berichtsseite](./media/oracle-design/io_info.png)

#### <a name="2-choose-a-vm"></a>2. Auswählen einer VM

Basierend auf den Informationen aus dem AWR-Bericht wählen Sie im nächsten Schritt eine VM mit ähnlicher Größe aus, die Ihren Anforderungen entspricht. Eine Liste der verfügbaren VMs finden Sie im Artikel [Memory optimized](https://docs.microsoft.com/azure/virtual-machinFine tune es/virtual-machines-windows-sizes-memory) (Optimierter Speicher).

#### <a name="3-fine-tune-the-vm-sizing-with-a-similar-vm-series-based-on-the-acu"></a>3. Optimieren der VM-Größe mit ähnlicher, auf der ACU basierender VM-Serie

Sobald Sie die VM ausgewählt haben, sollten Sie auf die Azure Compute-Einheit (Azure Compute Unit, ACU) für die VM achten. Sie können sich anhand des ACU-Werts auch für eine andere VM entscheiden, die Ihre Anforderungen möglicherweise besser erfüllt. Weitere Informationen finden Sie unter [Azure-Compute-Einheit (ACU)](https://docs.microsoft.com/azure/virtual-machines/windows/acu).

![Screenshot der Seite der ACUs](./media/oracle-design/acu_units.png)

### <a name="network-throughput"></a>Netzwerkdurchsatz

Die folgende Abbildung zeigt die Beziehung zwischen Durchsatz und IOPS:

![Screenshot des Durchsatzes](./media/oracle-design/throughput.png)

Der gesamte Netzwerkdurchsatz wird anhand der folgenden Informationen geschätzt:
- SQL*Net-Datenverkehr
- MBit/s x Anzahl von Servern (ausgehender Datenstrom wie z.B. Oracle Data Guard)
- Andere Faktoren wie z.B. Anwendungsreplikation

![Screenshot des SQL*Net-Durchsatzes](./media/oracle-design/sqlnet_info.png)

Je nach Ihren Anforderungen an die Netzwerkbandbreite können Sie aus verschiedenen Gatewaytypen wählen. Dazu gehören Basic, VpnGw und Azure ExpressRoute. Weitere Informationen finden Sie auf der Seite [VPN Gateway – Preise](https://azure.microsoft.com/en-us/pricing/details/vpn-gateway/?v=17.23h).

**Empfehlungen**

- Die Netzwerklatenz ist höher als bei einer lokalen Bereitstellung. Eine Verringerung der Netzwerkroundtrips kann die Leistung deutlich verbessern.
- Um Roundtrips zu reduzieren, konsolidieren Sie Anwendungen mit hohen Transaktionen oder auf dem gleichen virtuellen Computer häufig ausgeführte Apps.

### <a name="disk-types-and-configurations"></a>Datenträgertypen und -konfigurationen

- *Standard-Betriebssystemdatenträger*: Diese Datenträgertypen bieten persistente Daten und Zwischenspeichern. Sie sind für den Betriebssystemzugriff beim Systemstart optimiert und nicht für Transaktionsworkloads oder Data Warehouse-Workloads (analytisch) ausgelegt.

- *Nicht verwaltete Datenträger*: Mit diesen Datenträgertypen verwalten Sie die Speicherkonten, die die Dateien der virtuellen Festplatte (Virtual Hard Disk, VHD) speichern, die Ihren VM-Datenträgern entsprechen. Die VHD-Dateien werden als Seitenblobs in Azure-Speicherkonten gespeichert.

- *Verwaltete Datenträger*: Azure verwaltet die Speicherkonten, die Sie für Ihre VM-Datenträger verwenden. Sie geben den Datenträgertyp (Premium oder Standard) und die benötigte Datenträgergröße an. Azure erstellt und verwaltet den Datenträger für Sie.

- *Storage Premium-Datenträger*: Diese Datenträgertypen sind am besten für Produktionsworkloads geeignet. Storage Premium unterstützt VM-Datenträger, die an VMs einer bestimmten Größenserie wie DS, DSv2, GS und F angefügt werden können. Der Premium-Datenträger ist in unterschiedlichen Größen von 32 GB bis 4.096 GB erhältlich. Für jede Datenträgergröße gelten eigene Leistungsspezifikationen. Je nach Anwendungsanforderung können Sie einen oder mehrere Datenträger an Ihre VM anfügen.

Wenn Sie einen neuen verwalteten Datenträger aus dem Portal erstellen, können Sie den **Kontotyp** des Datenträgers angeben, den Sie verwenden möchten. Beachten Sie, dass nicht alle verfügbaren Datenträger im Dropdownmenü angezeigt werden. Nachdem Sie eine bestimmte VM-Größe ausgewählt haben, zeigt das Menü nur die verfügbaren Storage Premium-SKUs an, die auf dieser VM-Größe basieren.

![Screenshot der Seite „Verwaltete Datenträger“](./media/oracle-design/premium_disk01.png)

Weitere Informationen finden Sie unter [Storage Premium-Hochleistungsspeicher und verwaltete Datenträger für VMs](https://docs.microsoft.com/azure/storage/storage-premium-storage).

Sobald Sie Ihren Speicher auf einer VM konfiguriert haben, möchten Sie eventuell einen Auslastungstest für die Datenträger ausführen, bevor Sie eine Datenbank erstellen. Wenn Sie die E/A-Rate im Hinblick auf Latenz und Durchsatz kennen, können Sie damit besser bestimmen, ob die VMs den erwarteten Durchsatz mit Latenzzielen unterstützen.

Es gibt eine Reihe von Tools für den Auslastungstest einer Anwendung wie z.B. Oracle Orion, Sysbench und Fio.

Führen Sie den Auslastungstest erneut aus, nachdem Sie eine Oracle-Datenbank bereitgestellt haben. Starten Sie Ihre reguläre und maximale Workload, und die Ergebnisse zeigen Ihnen die Baseline Ihrer Umgebung.

Es könnte wichtiger sein, den Speicher anhand der IOPS-Rate anstatt der Speichergröße zu bemessen. Wenn z.B. die erforderliche IOPS-Rate 5.000 beträgt, Sie aber nur 200 GB benötigen, erhalten Sie möglicherweise immer noch den Premium-Datenträger der P30-Klasse, obwohl er mehr als 200 GB Speicher bietet.

Die IOPS-Rate kann aus dem AWR-Bericht abgerufen werden. Sie wird durch das Redo-Protokoll sowie die Rate physischer Lese- und Schreibvorgänge bestimmt.

![Screenshot der AWR-Berichtsseite](./media/oracle-design/awr_report.png)

Beispiel: Die Größe des Redo-Protokolls beträgt 12.200.000 Bytes pro Sekunde, was 11,63 MBit/s entspricht.
Die IOPS-Rate beträgt 12.200.000/2.358 = 5,174.

Sobald Sie eine genaue Vorstellung von den E/A-Anforderungen haben, können Sie eine Kombination von Laufwerken auswählen, die für diese Anforderungen am besten geeignet ist.

**Empfehlungen**

- Verteilen Sie die E/A-Workload für den DATA-Tabellenbereich mit verwaltetem Speicher oder Oracle ASM auf mehrere Datenträger.
- Wenn die E/A-Blöcke bei lese- und schreibintensiven Vorgängen größer werden, fügen Sie weitere Datenträger hinzu.
- Erhöhen Sie die Blockgröße für hohe sequenzielle Verarbeitung.
- Reduzieren Sie die E/A mit Datenkomprimierung (für Daten und Indizes).
- Trennen Sie Redo-Protokolle, SYSTEM- und TEMP- sowie UNDO-Tabellenbereiche durch separate Datenträger.
- Speichern Sie keine Anwendungsdateien auf dem standardmäßigen Betriebssystemdatenträger (/dev/sda). Diese Datenträger sind für schnelle VM-Startzeiten optimiert und erbringen für Ihre Anwendung möglicherweise keine gute Leistung.

### <a name="disk-cache-settings"></a>Cacheeinstellungen von Datenträgern

Es gibt drei Möglichkeiten für das Hostzwischenspeichern:

- *Schreibgeschützt*: Alle Anfragen werden für zukünftige Lesevorgänge zwischengespeichert. Alle Schreibvorgänge werden direkt in Azure Blob Storage gespeichert.

- *Lese-/Schreibzugriff*: Dies ist ein „Vorauslese“-Algorithmus. Die Lese- und Schreibvorgänge werden für zukünftige Lesevorgänge zwischengespeichert. Schreibvorgänge ohne Durchschreiben werden zuerst im lokalen Cache gespeichert. Für SQL Server werden Schreibvorgänge in Azure Storage gespeichert, da Durchschreiben verwendet wird. Darüber hinaus bietet dies die niedrigste Datenträgerlatenz für leichte Workloads.

- *Keine* (deaktiviert): Mit dieser Option können Sie den Cache umgehen. Alle Daten werden auf den Datenträger übertragen und in Azure Storage gespeichert. Diese Methode bietet Ihnen die höchste E/A-Rate für E/A-intensive Workloads. Sie müssen auch die „Transaktionskosten“ berücksichtigen.

**Empfehlungen**

Zur Maximierung des Durchsatzes sollten Sie für das Hostzwischenspeichern mit **Kein** beginnen. Beachten Sie bei Storage Premium, dass Sie die „Barrieren“ deaktivieren müssen, wenn Sie das Dateisystem mit der Option **Schreibgeschützt** oder **Kein** bereitstellen. Aktualisieren Sie die Datei „/etc/fstab“ mit der UUID auf die Datenträger.

Weitere Informationen finden Sie unter [Storage Premium für virtuelle Linux-Computer](https://docs.microsoft.com/azure/storage/storage-premium-storage#premium-storage-for-linux-vms).

![Screenshot der Seite „Verwaltete Datenträger“](./media/oracle-design/premium_disk02.png)

- Verwenden Sie für Betriebssystem-Datenträger das Standardzwischenspeichern mit **Lese-/Schreibzugriff**.
- Wählen Sie für SYSTEM, TEMP und UNDO bei Zwischenspeichern **Kein** aus.
- Verwenden Sie für DATA bei Zwischenspeichern **Kein**. Wenn Ihre Datenbank aber schreibgeschützt oder leseintensiv ist, verwenden Sie für Zwischenspeichern die Option **Schreibgeschützt**.

Nachdem Ihre Einstellung für den Datenträger gespeichert wurde, können Sie die Einstellung für das Hostzwischenspeichern nicht ändern, bis Sie die Bereitstellung des Laufwerks auf Betriebssystemebene aufheben und es nach der Änderung wieder bereitstellen.


## <a name="security"></a>Sicherheit

Nachdem Sie Ihre Azure-Umgebung eingerichtet und konfiguriert haben, besteht der nächste Schritt im Sichern des Netzwerks. Hier sind einige Empfehlungen dafür:

- *NSG-Richtlinie*: NSG kann von einem Subnetz oder NIC definiert werden. Es ist einfacher, den Zugriff auf der Ebene des Subnetzes zu steuern, sowohl für die Sicherheit als auch für das Erzwingen von Routing wie z.B. bei der Firewall einer Anwendung.

- *Jumpbox*: Damit der Zugriff noch sicherer wird, sollten Administratoren keine direkte Verbindung zum Anwendungsdienst oder zur Datenbank herstellen. Eine Jumpbox wird als Medium zwischen dem Administratorcomputer und Azure-Ressourcen verwendet.
![Screenshot der Jumpbox-Topologieseite](./media/oracle-design/jumpbox.png)

    Der Administratorcomputer sollte nur IP-beschränkten Zugriff auf die Jumpbox bieten. Die Jumpbox sollte dann Zugriff auf Anwendung und Datenbank haben.

- *Privates Netzwerk* (Subnetze): Sie sollten Anwendungsdienst und Datenbank in separaten Subnetzen unterbringen, damit eine bessere Steuerung durch die NSG-Richtlinie festgelegt werden kann.


## <a name="additional-reading"></a>Zusätzliche Lektüre

- [Konfigurieren von Oracle ASM](configure-oracle-asm.md)
- [Konfigurieren von Oracle Data Guard](configure-oracle-dataguard.md)
- [Konfigurieren von Oracle Golden Gate](configure-oracle-golden-gate.md)
- [Sichern und Wiederherstellen einer Oracle-Datenbank](oracle-backup-recovery.md)

## <a name="next-steps"></a>Nächste Schritte

- [Tutorial: Erstellen eines hoch verfügbaren virtuellen Computers](../../linux/create-cli-complete.md)
- [Erkunden der Azure-CLI-Beispiele für die Bereitstellung virtueller Computer](../../linux/cli-samples.md)

