---
title: Optimieren der Leistung von MySQL unter Linux | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie MySQL bei Ausführung auf einem virtuellen Azure-Computer unter Linux optimieren können."
services: virtual-machines-linux
documentationcenter: 
author: NingKuang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 0c1c7fc5-a528-4d84-b65d-2df225f2233f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: ningk
ms.translationtype: Human Translation
ms.sourcegitcommit: 43aab8d52e854636f7ea2ff3aae50d7827735cc7
ms.openlocfilehash: 8f2ec884fa98e989448ac11675e71f39aa21fa7f
ms.contentlocale: de-de
ms.lasthandoff: 06/03/2017


---
# <a name="optimize-mysql-performance-on-azure-linux-vms"></a>Optimieren der MySQL-Leistung auf virtuellen Azure Linux-Computern
Es gibt viele Faktoren, die die MySQL-Leistung unter Azure beeinträchtigen, sowohl in Bezug auf die Auswahl der virtuellen Hardware als auch auf die Softwarekonfiguration. Dieser Artikel befasst sich mit der Leistungsoptimierung über die Speicher-, System- und Datenbankkonfiguration.

> [!IMPORTANT]
> Azure bietet zwei Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Azure Resource Manager](../../../resource-manager-deployment-model.md) und klassisch. Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells. Informationen zu Linux-VM-Optimierungen mit dem Resource Manager-Modell finden Sie unter [Optimieren virtueller Linux-Computer in Azure](../optimization.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="utilize-raid-on-an-azure-virtual-machine"></a>Verwenden von RAID auf einem virtuellen Azure-Computer
Speicher ist der wichtigste Faktor, der sich auf die Leistung der Datenbank in Cloudumgebungen auswirkt. Im Vergleich zu einem einzelnen Datenträger kann RAID schnelleren Zugriff über Parallelität bereitstellen. Weitere Informationen finden Sie unter [Standard-RAID-Stufen](http://en.wikipedia.org/wiki/Standard_RAID_levels).   

Datenträger-E/A-Durchsatz und E/A-Antwortzeiten in Azure können durch RAID verbessert werden. Unsere Labortests zeigen, dass der Datenträger-E/A-Durchsatz bei Verdoppelung der Anzahl der RAID-Datenträger (von 2 auf 4, von 4 auf 8 usw.) durchschnittlich verdoppelt und die E/A-Antwortzeiten auf die Hälfte reduziert werden können. In [Anhang A](#AppendixA) finden Sie nähere Informationen.  

Zusätzlich zur Datenträger-E/A verbessert sich die MySQL-Leistung, wenn Sie die RAID-Stufe erhöhen.  In [Anhang B](#AppendixB) finden Sie nähere Informationen.  

Sie sollten auch die Segmentgröße berücksichtigen. Im Allgemeinen erzielen Sie mit größeren Segmenten einen niedrigeren Verarbeitungsaufwand, insbesondere bei großen Schreibvorgängen. Eine zu große Segmentgröße kann jedoch mit zusätzlichem Verarbeitungsaufwand verbunden sein, sodass Sie die Vorteile von RAID nicht nutzen können. Die aktuelle Standardgröße beträgt 512 KB, die sich für die meisten allgemeinen Produktionsumgebungen als optimal erwiesen hat. In [Anhang C](#AppendixC) finden Sie nähere Informationen.   

Hinsichtlich der Anzahl der Datenträger, die Sie für verschiedene Arten von virtuellen Computern hinzufügen können, gelten Beschränkungen. Ausführliche Informationen zu diesen Grenzen finden Sie unter [Größen virtueller Computer und Clouddienste für Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx). Im RAID-Beispiel in diesem Artikel benötigen Sie vier angefügte Datenträger. Sie können RAID jedoch auch mit weniger Datenträgern einrichten.  

In diesem Artikel wird angenommen, dass Sie bereits einen virtuellen Linux-Computer erstellt und MySQL installiert und konfiguriert haben. Weitere Informationen zu den ersten Schritten finden Sie unter „Installieren von MySQL in Azure“.  

### <a name="set-up-raid-on-azure"></a>Einrichten von RAID in Azure
Die folgenden Schritte beschreiben, wie Sie RAID mithilfe des Azure-Portals in Azure erstellen können. Sie können RAID auch mithilfe von Windows PowerShell-Skripts einrichten.
In diesem Beispiel wird RAID 0 mit 4 Datenträgern konfiguriert.  

#### <a name="add-a-data-disk-to-your-virtual-machine"></a>Hinzufügen eines Datenträgers zum virtuellen Computer
Rufen Sie im Azure-Portal das Dashboard auf, und wählen Sie den virtuellen Computer aus, dem Sie einen Datenträger hinzufügen möchten. In diesem Beispiel wird der virtuelle Computer "mysqlnode1" verwendet.  

<!--![Virtual machines][1]-->

Klicken Sie auf **Datenträger** und dann auf **Neuen anfügen**.

![Virtuelle Computer – Datenträger hinzufügen](media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-Disks-option.png)

Erstellen Sie einen neuen 500-GB-Datenträger. Stellen Sie sicher, dass für **Hostcacheeinstellungen** die Einstellung **Keine** festgelegt ist.  Klicken Sie abschließend auf **OK**.

![Schließen Sie einen leeren Datenträger an.](media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-attach-empty-disk.png)


Dadurch wird Ihrem virtuellen Computer ein leerer Datenträger hinzugefügt. Wiederholen Sie diesen Schritt drei weitere Male, bis Sie über vier Datenträger für RAID verfügen.  

Sie sehen die hinzugefügten Laufwerke auf dem virtuellen Computer im Kernel-Message-Protokoll. Verwenden Sie hierzu unter Ubuntu beispielsweise den folgenden Befehl:  

    sudo grep SCSI /var/log/dmesg

#### <a name="create-raid-with-the-additional-disks"></a>Erstellen von RAID mit den zusätzlichen Datenträgern
In den folgenden Schritte wird beschrieben, wie [Software-RAID unter Linux konfiguriert wird](../configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

> [!NOTE]
> Führen Sie bei Verwendung des Dateisystems XFS nach der RAID-Erstellung die nachfolgend aufgeführten Schritte aus:
>
>

Verwenden Sie den folgenden Befehl, um XFS für Debian, Ubuntu oder Linux Mint zu installieren:  

    apt-get -y install xfsprogs  

Verwenden Sie den folgenden Befehl, um XFS für Fedora, CentOS oder RHEL zu installieren:  

    yum -y install xfsprogs  xfsdump


#### <a name="set-up-a-new-storage-path"></a>Einrichten eines neuen Speicherpfads
Verwenden Sie den folgenden Befehl, um einen neuen Speicherpfad einzurichten:  

    root@mysqlnode1:~# mkdir -p /RAID0/mysql

#### <a name="copy-the-original-data-to-the-new-storage-path"></a>Kopieren der ursprünglichen Daten an den neuen Speicherpfad
Verwenden Sie den folgenden Befehl, um Daten in den neuen Speicherpfad zu kopieren:  

    root@mysqlnode1:~# cp -rp /var/lib/mysql/* /RAID0/mysql/

#### <a name="modify-permissions-so-mysql-can-access-read-and-write-the-data-disk"></a>Ändern der Berechtigungen, sodass MySQL Lese- und Schreibzugriff auf den Datenträger erhält
Geben Sie den folgenden Befehl zum Ändern von Berechtigungen ein:  

    root@mysqlnode1:~# chown -R mysql.mysql /RAID0/mysql && chmod -R 755 /RAID0/mysql


## <a name="adjust-the-disk-io-scheduling-algorithm"></a>Anpassen des E/A-Scheduling-Algorithmus
Linux implementiert vier Arten von E/A-Scheduling-Algorithmen:  

* NOOP-Algorithmus (Kein Vorgang)
* Deadline-Algorithmus (Frist)
* Completely Fair Queuing-Algorithmus (CFQ)
* Budget Period-Algorithmus (Budgetzeitraum, antizipiert)  

Sie können verschiedene E/A-Scheduler in verschiedenen Szenarien auswählen, um die Leistung zu optimieren. In einer vollständig wahlfreien Zugriffsumgebung unterscheidet sich die Leistung des CFQ- und des Deadline-Algorithmus nicht wesentlich. Für die MySQL-Datenbankumgebung wird der Stabilität halber die Einstellung „Deadline“ empfohlen. Bei viel sequenzieller E/A kann die Datenträger-E/A-Leistung durch CFQ beeinträchtigt werden.   

SSD- und andere Geräte erzielen mit NOOP oder Deadline eine bessere Leistung als der Standardscheduler.   

Bis Kernel 2.5 ist der Standard-E/A-Scheduling-Algorithmus „Deadline“. CFQ wurde ab Kernel 2.6.18 zum Standard-E/A-Scheduling-Algorithmus.  Sie können diese Einstellung zur Startzeit des Kernels angeben oder sie bei laufendem System dynamisch ändern.  

Im folgenden Beispiel wird veranschaulicht, wie der Scheduler überprüft und der NOOP-Algorithmus als Standardscheduler festgelegt wird.  

### <a name="view-the-current-io-scheduler"></a>Anzeigen des aktuellen E/A-Schedulers
Führen Sie den folgenden Befehl aus, um den Scheduler anzuzeigen:  

    root@mysqlnode1:~# cat /sys/block/sda/queue/scheduler

Sie sehen die folgende Ausgabe, die den aktuellen Scheduler angibt:  

    noop [deadline] cfq


### <a name="change-the-current-device-devsda-of-the-io-scheduling-algorithm"></a>Ändern des aktuellen Geräts (/dev/sda) des E/A-Scheduling-Algorithmus
Führen Sie den folgenden Befehl aus, um das aktuelle Gerät zu ändern:  

    azureuser@mysqlnode1:~$ sudo su -
    root@mysqlnode1:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mysqlnode1:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mysqlnode1:~# update-grub

> [!NOTE]
> Nur für /dev/sda ist diese Einstellung nicht sinnvoll. Sie muss auf allen Datenträgern festgelegt werden, auf denen sich die Datenbank befindet.  
>
>

Die folgende Ausgabe zeigt an, dass „grub.cfg“ erfolgreich neu erstellt wurde und dass der Standardscheduler in NOOP geändert wurde:  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

Bei Redhat-Distributionen müssen Sie nur den folgenden Befehl ausführen:

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

## <a name="configure-system-file-operations-settings"></a>Konfigurieren der Einstellungen für Systemdateivorgänge
Eine bewährte Methode ist, das Protokollierungsfeature *atime* für das Dateisystem zu deaktivieren. "Atime" ist die Zeit des letzten Dateizugriffs. Wenn auf eine Datei zugegriffen wird, erfasst das Dateisystem den Zeitstempel im Protokoll. Diese Informationen werden jedoch nur selten verwendet. Sie können sie deaktivieren, wenn Sie sie nicht benötigen. Hierdurch verringert sich die Datenträger-Zugriffszeit insgesamt.  

Für die Deaktivierung der atime-Protokollierung müssen Sie die Dateisystemkonfigurationsdatei "/etc/fstab" ändern und die Option **noatime** hinzufügen.  

Bearbeiten Sie z.B. die Datei „vim/etc/fstab“, indem Sie „noatime“ hinzufügen (siehe das folgende Beispiel):  

    # CLOUD_IMG: This file was created/modified by the Cloud Image build process
    UUID=3cc98c06-d649-432d-81df-6dcd2a584d41       /        ext4   defaults,discard        0 0
    #Add the “noatime” option below to disable atime logging
    UUID="431b1e78-8226-43ec-9460-514a9adf060e"     /RAID0   xfs   defaults,nobootwait, noatime 0 0
    /dev/sdb1       /mnt    auto    defaults,nobootwait,comment=cloudconfig 0       2

Stellen Sie dann das Dateisystem mit dem folgenden Befehl erneut bereit:  

    mount -o remount /RAID0

Testen Sie das geänderte Ergebnis. Wenn Sie die Testdatei ändern, wird die Zugriffszeit nicht geändert. Die folgenden Beispiele zeigen, wie der Code vor und nach der Änderung aussieht.

Vorher:        

![Code vor der Zugriffsänderung][5]

Nachher:

![Code nach der Zugriffsänderung][6]

## <a name="increase-the-maximum-number-of-system-handles-for-high-concurrency"></a>Erhöhen der maximalen Anzahl von Systemhandles für hohe Parallelität
MySQL ist eine Datenbank mit hoher Parallelität. Die Standardzahl von gleichzeitigen Handles beträgt 1024 für Linux. Dies ist nicht immer ausreichend. Gehen Sie folgendermaßen vor, um die Zahl von maximalen gleichzeitigen Handles des Systems zur Unterstützung hoher Parallelität von MySQL zu erhöhen.

### <a name="modify-the-limitsconf-file"></a>Ändern der Datei "limits.conf"
Fügen Sie die folgenden vier Zeilen in die Datei „/etc/security/limits.conf“ ein, um die Anzahl der maximal zulässigen gleichzeitigen Handles zu erhöhen. Beachten Sie, dass 65536 die maximale Anzahl ist, die das System unterstützen kann.   

    * soft nofile 65536
    * hard nofile 65536
    * soft nproc 65536
    * hard nproc 65536

### <a name="update-the-system-for-the-new-limits"></a>Aktualisieren des Systems mit den neuen Grenzen
Um das System zu aktualisieren, führen Sie die folgenden Befehle aus:  

    ulimit -SHn 65536
    ulimit -SHu 65536

### <a name="ensure-that-the-limits-are-updated-at-boot-time"></a>Sicherstellen, dass die Grenzwerte beim Systemstart aktualisiert werden
Fügen Sie die folgenden Startbefehle in die Datei „/etc/rc.local“ ein, damit diese beim Systemstart wirksam werden.  

    echo “ulimit -SHn 65536” >>/etc/rc.local
    echo “ulimit -SHu 65536” >>/etc/rc.local

## <a name="mysql-database-optimization"></a>MySQL-Datenbankoptimierung
Sie können die gleiche Leistungsoptimierungsstrategie für die Konfiguration von MySQL in Azure wie für einen lokalen Computer verwenden.  

Die wichtigsten E/A-Optimierungsregeln sind:   

* Erhöhen Sie die Cachegröße.
* Verringern Sie die E/A-Antwortzeit.  

Um MySQL-Server-Einstellungen zu optimieren, können Sie die Datei "my.cnf" aktualisieren. Dabei handelt es sich um die Standardkonfigurationsdatei für Server und Clientcomputer.  

Die folgenden Konfigurationselemente sind die Hauptfaktoren, die die MySQL-Leistung beeinflussen:  

* **innodb_buffer_pool_size**: Der Pufferpool enthält gepufferte Daten und den Index. Dies ist normalerweise auf 70 % des physischen Arbeitsspeichers festgelegt.
* **innodb_log_file_size**: Dies ist die Größe des Redo-Protokolls. Sie verwenden die redo-Protokolle, um sicherzustellen, dass die Schreibvorgänge nach einem Systemabsturz schnell, zuverlässig und wiederherstellbar sind. Dies ist auf 512 MB festgelegt. Damit verfügen Sie über ausreichend Speicherplatz für die Protokollierung von Schreibvorgängen.
* **max_connections**: Manchmal schließen Anwendungen Verbindungen nicht ordnungsgemäß. Mit einem größeren Wert erhält der Server mehr Zeit für die Wiederverwendung von Leerlaufverbindungen. Es sind maximal 10.000 Verbindungen möglich, doch das empfohlene Maximum sind 5.000 Verbindungen.
* **Innodb_file_per_table**: Mit dieser Einstellung wird die Fähigkeit von InnoDB zur Speicherung von Tabellen in separaten Dateien aktiviert bzw. deaktiviert. Durch Aktivieren der Option wird sichergestellt, dass mehrere erweiterte Verwaltungsvorgänge effizient angewendet werden können. Vom Leistungsstandpunkt aus betrachtet kann die Übermittlung von Tabellenspeicherplatz hierdurch beschleunigt und die Leistung beim Umgang mit Verschwendung optimiert werden. Daher wird empfohlen, diese Option auf ON festzulegen.</br></br>
Ab MySQL 5.6 ist die Standardeinstellung ON, sodass keine Aktion erforderlich ist. Für frühere Versionen ist die Standardeinstellung OFF. Die Einstellung muss vor dem Laden neuer Daten geändert werden, weil sie sich nur auf neu erstellte Tabellen auswirkt.
* **Innodb_flush_log_at_trx_commit**: Der Standardwert ist 1, wobei der Gültigkeitsbereich auf 0~2 gesetzt ist. Der Standardwert ist die am besten geeignete Option für eine eigenständige MySQL-DB. Die Einstellung 2 ermöglicht die höchste Datenintegrität und eignet sich für den Master im MySQL-Cluster. Die Einstellung 0 lässt Datenverluste zu. Dies kann die Zuverlässigkeit beeinträchtigen. In einigen Fällen wird die Leistung verbessert. Daher eignet sich die Einstellung für „Slave“ im MySQL-Cluster.
* **Innodb_log_buffer_size**: Der Protokollpuffer lässt zu, dass Transaktionen ausgeführt werden, ohne dass das Protokoll vor dem Ausführen der Transaktionen auf den Datenträger übertragen werden muss. Wenn jedoch ein großes binäres Objekt oder Textfeld vorhanden ist, wird der Zwischenspeicher schnell belegt. Dadurch wird eine häufige Datenträger-E/A ausgelöst. Es ist besser, die Puffergröße zu erhöhen, wenn die Zustandsvariable "Innodb_log_waits" nicht 0 ist.
* **query_cache_size**: Die beste Möglichkeit ist, diese Einstellung von Anfang an zu deaktivieren. Legen Sie „query_cache_size“ auf 0 fest (in MySQL 5.6 ist dies die Standardeinstellung), und verwenden Sie andere Methoden zur Beschleunigung von Abfragen.  

In [Anhang D](#AppendixD) finden Sie einen Vergleich der Leistung vor und nach der Optimierung.

## <a name="turn-on-the-mysql-slow-query-log-for-analyzing-the-performance-bottleneck"></a>Aktivieren des Protokolls für langsame MySQL-Abfragen zur Analyse des Leistungsengpasses
Das Protokoll für langsame MySQL-Abfragen kann Ihnen dabei helfen, langsame MySQL-Abfragen zu identifizieren. Nach der Aktivierung des Protokolls für langsame MySQL-Abfragen können Sie MySQL-Tools wie **mysqldumpslow** zur Identifizierung von Leistungsengpässen verwenden.  

Dieses ist standardmäßig nicht aktiviert. Die Aktivierung des Protokolls für langsame Abfragen kann einige CPU-Ressourcen beanspruchen. Es wird empfohlen, dass Sie diese Option nur vorübergehend zur Problembehebung bei Leistungsengpässen aktivieren. So aktivieren Sie das Protokoll für langsame Abfragen

1. Ändern Sie die Datei „My.cnf“, indem Sie die folgenden Zeilen am Ende hinzufügen:

        long_query_time = 2
        slow_query_log = 1
        slow_query_log_file = /RAID0/mysql/mysql-slow.log

2. Starten Sie den MySQL-Server neu.

        service  mysql  restart

3. Überprüfen Sie, ob die Einstellung wirksam ist, indem Sie den Befehl **show** aufrufen.

![Slow-query-log ON][7]   

![Slow-query-log results][8]

In diesem Beispiel sehen Sie, dass die Funktion für langsame Abfragen aktiviert wurde. Anschließend können Sie mit dem Tool **mysqldumpslow** Leistungsengpässe ermitteln und die Leistung optimieren (z.B. das Hinzufügen von Indizes).

## <a name="appendices"></a>Anhänge
Es folgen beispielhafte Leistungstestdaten, die in einer Ziellaborumgebung erstellt wurden. Sie bieten allgemeine Informationen zum Leistungsdatentrend bei verschiedenen Leistungsoptimierungsansätzen. Die Ergebnisse können bei verschiedenen Umgebungen oder Produktversionen abweichen.

### <a name="AppendixA"></a>Anhang A  
**Datenträgerleistung (IOPS) mit verschiedenen RAID-Stufen**

![Datenträger-IOPS mit verschiedenen RAID-Stufen][9]

**Testbefehle**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=5G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite

> [!NOTE]
> Die Workload dieses Tests verwendet 64 Threads, die die Obergrenze des RAID erreichen sollen.
>
>

### <a name="AppendixB"></a>Anhang B  
**MySQL-Leistungsvergleich (Durchsatz) bei verschiedenen RAID-Stufen**   
(XFS-Dateisystem)

![MySQL-Leistungsvergleich bei verschiedenen RAID-Stufen][10]  
![MySQL-Leistungsvergleich bei verschiedenen RAID-Stufen][11]

**Testbefehle**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb

**MySQL-Leistungsvergleich (OLTP) bei verschiedenen RAID-Stufen**  
![MySQL-Leistungsvergleich (OLTP) bei verschiedenen RAID-Stufen][12]

**Testbefehle**

    time sysbench --test=oltp --db-driver=mysql --mysql-user=root --mysql-password=0ps.123  --mysql-table-engine=innodb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-socket=/var/run/mysqld/mysqld.sock --mysql-db=test --oltp-table-size=1000000 prepare

### <a name="AppendixC"></a>Anhang C   
**Vergleich der Datenträgerleistung (IOPS) bei verschiedenen Blockgrößen**  
(XFS-Dateisystem)

![][13]

**Testbefehle**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=30G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite
    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=1G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite  

Für diesen Test wird eine Dateigröße von 30 GB bzw. 1 GB mit einem XFS-Dateisystem des Typs „RAID 0 (4 Datenträger)“ verwendet.

### <a name="AppendixD"></a>Anhang D  
**Vergleich der MySQL-Leistung (Durchsatz) vor und nach der Optimierung**  
(XFS-Dateisystem)

![Vergleich der MySQL-Leistung (Durchsatz) vor und nach der Optimierung][14]

**Testbefehle**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb,misam

**Die Konfigurationseinstellung für Standard und Optimierung lautet wie folgt:**

| Parameter | Standard | Optimierung |
| --- | --- | --- |
| **innodb_buffer_pool_size** |Keine |7 GB |
| **innodb_log_file_size** |5 MB |512 MB |
| **max_connections** |100 |5.000 |
| **innodb_file_per_table** |0 |1 |
| **innodb_flush_log_at_trx_commit** |1 |2 |
| **innodb_log_buffer_size** |8 MB |128 MB |
| **query_cache_size** |16 MB |0 |

Weitere und detailliertere [Konfigurationsparameter für die Optimierung](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html) finden Sie in den [offiziellen MySQL-Anweisungen](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method).  

  **Testumgebung**  

| Hardware | Details |
| --- | --- |
| CPU |AMD Opteron(tm) Processor 4171 HE/4 Kerne |
| Arbeitsspeicher |14 GB |
| Datenträger |10 GB/Datenträger |
| Betriebssystem |Ubuntu 14.04.1 LTS |

[1]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-01.png
[2]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-02.png
[3]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-03.png
[4]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-04.png
[5]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-05.png
[6]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-06.png
[7]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-07.png
[8]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-08.png
[9]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-09.png
[10]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-10.png
[11]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-11.png
[12]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-12.png
[13]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-13.png
[14]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-14.png


