---
title: SAP HANA-Sicherung mit Azure Backup auf Dateiebene | Microsoft-Dokumentation
description: "Für SAP HANA auf virtuellen Azure-Computern stehen grundsätzlich zwei Sicherungsoptionen zur Verfügung. In diesem Artikel wird die SAP HANA-Sicherung mit Azure Backup auf Dateiebene behandelt."
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ums.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 3/13/2017
ms.author: rclaus
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: f2449ffda80ec7cb7a73a6eac229d09c3a4a96cc
ms.lasthandoff: 04/03/2017


---

# <a name="sap-hana-azure-backup-on-file-level"></a>SAP HANA-Sicherung mit Azure Backup auf Dateiebene

## <a name="introduction"></a>Einführung

Dieser Artikel ist Teil einer dreiteiligen Reihe zur SAP HANA-Sicherung. [Backup guide for SAP HANA on Azure Virtual Machines](./sap-hana-backup-guide.md) (Sicherungshandbuch für SAP HANA in Azure Virtual Machines) enthält eine Übersicht und Informationen zu den ersten Schritten, und unter [SAP HANA backup based on storage snapshots](./sap-hana-backup-storage-snapshots.md) (SAP HANA-Sicherung auf der Grundlage von Speichermomentaufnahmen) wird die auf Speichermomentaufnahmen basierende Sicherungsoption behandelt.

Ein Blick auf die Azure-VM-Größen zeigt, dass bei GS5 64 angefügte Datenträger zulässig sind. Bei umfangreichen SAP HANA-Systemen wird ein Großteil der Datenträger unter Umständen bereits für Daten und Protokolldateien benötigt – möglicherweise in Kombination mit Software-RAID für optimalen Datenträger-E/A-Durchsatz. Stellt sich die Frage: Wo sollen die SAP HANA-Sicherungsdateien gespeichert werden, die mit der Zeit die angefügten Datenträger füllen können? Die Größentabellen für virtuelle Azure-Computer finden Sie unter [Größen für virtuelle Linux-Computer in Azure](../../linux/sizes.md).

Momentan steht für den Azure Backup-Dienst keine SAP HANA-Sicherungsintegration zur Verfügung. Die Sicherung/Wiederherstellung auf Dateiebene wird standardmäßig mit einer dateibasierten Sicherung über SAP HANA Studio oder über SAP HANA-SQL-Anweisungen verwaltet. Weitere Informationen finden Sie unter [SAP HANA SQL and System Views Reference](https://help.sap.com/hana/SAP_HANA_SQL_and_System_Views_Reference_en.pdf) (Referenz zu SAP HANA SQL und zu Systemansichten).

![Diese Abbildung zeigt das Dialogfeld des Sicherungsmenüelements in SAP HANA-Studio.](media/sap-hana-backup-file-level/image022.png)

Diese Abbildung zeigt das Dialogfeld des Sicherungsmenüelements in SAP HANA-Studio. Bei Verwendung des Typs &quot;Datei&quot; muss ein Pfad im Dateisystem angegeben werden, an den SAP HANA die Sicherungsdateien schreibt. Die Wiederherstellung funktioniert auf die gleiche Weise.

Diese Option klingt zwar recht einfach und unkompliziert, es sind jedoch ein paar Punkte zu berücksichtigen. Wie bereits erwähnt kann an einen virtuellen Azure-Computer nur eine begrenzte Anzahl von Datenträgern angefügt werden. Unter Umständen reicht die Dateisystemkapazität des virtuellen Computers nicht aus, um SAP HANA-Sicherungsdateien zu speichern. Das hängt von der Größe der Datenbank und von den Anforderungen für den Datenträgerdurchsatz (ggf. mit Software-RAID und datenträgerübergreifendem Striping) ab. Weiter unten in diesem Artikel finden Sie verschiedene Optionen zum Verschieben dieser Sicherungsdateien sowie zum Verwalten der Dateigrößenbeschränkungen und der Leistung beim Verarbeiten großer Datenmengen.

Eine weitere Option mit größerer Freiheit bei der Gesamtkapazität ist Azure Blob Storage. Ein einzelnes Blob ist zwar ebenfalls auf 1 TB beschränkt, die Gesamtkapazität eines einzelnen Blobcontainers liegt momentan jedoch bei 500 TB. Darüber hinaus haben Kunden die Möglichkeit, so genannten &quot;kalten&quot; Blobspeicher auszuwählen und dadurch Kosten zu sparen. Ausführliche Informationen zu kaltem Blobspeicher finden Sie unter [Azure Blob Storage: „Heiße“ und „kalte“ Speicherebenen](../../../storage/storage-blob-storage-tiers.md).

Die SAP HANA-Sicherungen können auch in einem georeplizierten Speicherkonto gespeichert werden, um die Sicherheit weiter zu erhöhen. Ausführliche Informationen zur Speicherkontoreplikation finden Sie unter [Azure Storage-Replikation](../../../storage/storage-redundancy.md).

Sie können dedizierte VHDs für SAP HANA-Sicherungen in einem dedizierten Sicherungsspeicherkonto mit Georeplizierung platzieren. Alternativ können Sie die VHDs mit den SAP HANA-Sicherungen in ein georepliziertes Speicherkonto oder in ein Speicherkonto in einer anderen Region kopieren.

## <a name="azure-backup-agent"></a>Azure Backup-Agent

Mit Azure Backup können Sie über den Backup-Agent nicht nur vollständige virtuelle Computer, sondern auch Dateien und Verzeichnisse sichern. Hierzu muss der Agent unter dem Gastbetriebssystem installiert sein. Seit Dezember 2016 wird dieser Agent allerdings nur noch unter Windows unterstützt. (Weitere Informationen finden Sie unter [Sichern eines Windows-Servers oder -Clients in Azure unter Verwendung des Resource Manager-Bereitstellungsmodells](../../../backup/backup-configure-vault.md).)

Eine Möglichkeit zur Umgehung dieses Problems besteht darin, SAP HANA-Sicherungsdateien zuerst auf einen virtuellen Windows-Computer in Azure zu kopieren (beispielsweise über eine SAMBA-Freigabe) und den Azure Backup-Agent dann von dort aus zu verwenden. Das ist zwar technisch möglich, würde aber die Komplexität erhöhen und den Sicherungs- oder Wiederherstellungsprozess durch den Kopiervorgang zwischen dem virtuellen Linux-Computer und dem virtuellen Windows-Computer erheblich verlangsamen. Daher raten wir von dieser Vorgehensweise ab.

## <a name="azure-blobxfer-utility-details"></a>Details zum Azure-Hilfsprogramm „blobxfer“

Wenn Sie Verzeichnisse und Dateien in Azure Storage speichern möchten, können Sie dazu entweder die Befehlszeilenschnittstelle oder PowerShell verwenden oder ein Tool mit einem der [Azure SDKs](https://azure.microsoft.com/downloads/) entwickeln. Zum Kopieren von Daten in Azure Storage steht zwar das verwendungsbereite Hilfsprogramm AzCopy zur Verfügung, dieses wird jedoch nur unter Windows unterstützt. (Weitere Informationen finden Sie unter [Übertragen von Daten mit dem Befehlszeilenprogramm AzCopy](../../../storage/storage-use-azcopy.md).)

Aus diesem Grund wurde „blobxfer“ zum Kopieren von SAP HANA-Sicherungsdateien verwendet. Dieses Open Source-Programm wird von vielen Kunden in Produktionsumgebungen genutzt und ist über [GitHub](https://github.com/Azure/blobxfer) erhältlich. Mit dem Tool können Daten direkt in Azure Blob Storage oder in eine Azure-Dateifreigabe kopiert werden. Darüber hinaus bietet es eine Reihe nützlicher Features wie MD5-Hash oder automatische Parallelität beim Kopieren eines Verzeichnisses mit mehreren Dateien.

## <a name="sap-hana-backup-performance"></a>SAP HANA-Sicherungsleistung

![Dieser Screenshot zeigt die SAP HANA-Sicherungskonsole in SAP HANA Studio.](media/sap-hana-backup-file-level/image023.png)

Dieser Screenshot zeigt die SAP HANA-Sicherungskonsole in SAP HANA Studio. Es hat etwa 42 Minuten gedauert, die 230 GB auf einem einzelnen Azure-Standardspeicherdatenträger zu sichern, der an den virtuellen HANA-Computer mit XFS-Dateisystem angefügt ist.

![Dieser Screenshot zeigt YaST auf dem virtuellen SAP HANA-Testcomputer.](media/sap-hana-backup-file-level/image024.png)

Dieser Screenshot zeigt YaST auf dem virtuellen SAP HANA-Testcomputer. Hier sehen Sie den bereits erwähnten einzelnen 1-TB-Datenträger für die SAP HANA-Sicherung. Das Sichern von 230 GB dauerte etwa 42 Minuten. Darüber hinaus wurden fünf 200-GB-Datenträger angefügt, und Software-RAID „md0“ wurde erstellt – mit Striping für die fünf Azure-Datenträger.

![Wiederholen der gleichen Sicherung mit Software-RAID und übergreifendem Striping für fünf angefügte Azure Standardspeicherdatenträger](media/sap-hana-backup-file-level/image025.png)

Bei der Wiederholung der gleichen Sicherung mit Software-RAID und übergreifendem Striping für fünf angefügte Azure Standardspeicherdatenträger konnte die Sicherungsdauer von 42 Minuten auf zehn Minuten verkürzt werden. Die Datenträger wurden ohne Zwischenspeicherung an den virtuellen Computer angefügt. Sie sehen also, wie wichtig der Schreibdurchsatz des Datenträgers für die Sicherungsdauer ist. Durch einen Wechsel zu Azure Storage Premium lässt sich der Prozess weiter beschleunigen, um eine optimale Leistung zu erzielen. Für Produktionssysteme sollte grundsätzlich Azure Storage Premium verwendet werden.

## <a name="copy-sap-hana-backup-files-to-azure-blob-storage"></a>Kopieren von SAP HANA-Sicherungsdateien in Azure Blob Storage

Seit Dezember 2016 ist Azure Blob Storage die beste Option zur schnellen Speicherung von SAP HANA-Sicherungsdateien. Ein einzelner Blobcontainer ist auf 500 TB beschränkt. Das sollte für die meisten SAP HANA-Systeme auf einem virtuellen GS5-Computer in Azure ausreichend sein. Kunden haben die Wahl zwischen &quot;heißem&quot; und &quot;kaltem&quot; Blobspeicher. (Weitere Informationen finden Sie unter [Azure Blob Storage: „Heiße“ und „kalte“ Speicherebenen](../../../storage/storage-blob-storage-tiers.md).)

Mit dem blobxfer-Tool lassen sich die SAP HANA-Sicherungsdateien komfortabel direkt in Azure Blob Storage kopieren.

![Hier sehen Sie die Dateien einer vollständigen SAP HANA-Dateisicherung.](media/sap-hana-backup-file-level/image026.png)

Hier sehen Sie die Dateien einer vollständigen SAP HANA-Dateisicherung. Sie umfasst vier Dateien, und die größte ist etwa 230 GB groß.

![Es hat ungefähr 3.000 Sekunden gedauert, die 230 GB in einen Blobcontainer eines Azure-Standardspeicherkontos zu kopieren.](media/sap-hana-backup-file-level/image027.png)

Ohne Verwendung des MD5-Hashs hat es beim ersten Test ungefähr 3.000 Sekunden gedauert, die 230 GB in einen Blobcontainer eines Azure-Standardspeicherkontos zu kopieren.

![Dieser Screenshot zeigt die Darstellung im Azure-Portal.](media/sap-hana-backup-file-level/image028.png)

Dieser Screenshot zeigt die Darstellung im Azure-Portal. Ein Blobcontainer namens &quot;sap-hana-backups&quot; wurde erstellt. Er enthält die vier Blobs, die die SAP HANA-Sicherungsdateien darstellen. Eine der Dateien hat eine Größe von etwa 230 GB.

Über die Sicherungskonsole von HANA Studio können Sie die maximal zulässige Größe für HANA-Sicherungsdateien festlegen. In der Beispielumgebung hat sich dadurch die Leistung verbessert, da anstelle einer großen Datei mit 230 GB mehrere kleinere Sicherungsdateien verwendet werden konnten.

![Das Festlegen eines HANA-seitigen Grenzwerts für die Sicherungsdateigröße führt nicht zu einer Verkürzung der Sicherungsdauer.](media/sap-hana-backup-file-level/image029.png)

Das Festlegen eines HANA-seitigen Grenzwerts für die Sicherungsdateigröße führt nicht zu einer Verkürzung der Sicherungsdauer, da die Dateien sequenziell geschrieben werden, wie in dieser Abbildung zu sehen. Der Grenzwert für die Dateigröße wurde auf 60 GB festgelegt, weshalb bei der Sicherung anstelle einer einzelnen 230-GB-Datei vier große Datendateien erstellt wurden.

![Anschließend wurde die maximale Dateigröße für HANA-Sicherungen auf 15 GB festgelegt, um die Parallelität des blobxfer-Tools zu testen.](media/sap-hana-backup-file-level/image030.png)

Anschließend wurde die maximale Dateigröße für HANA-Sicherungen auf 15 GB festgelegt, um die Parallelität des blobxfer-Tools zu testen. Das führte zu 19 Sicherungsdateien. Durch diese Konfiguration konnte die Zeit, die blobxfer benötigt, um die 230 GB in Azure Blob Storage zu kopieren, von 3.000 Sekunden auf 875 Sekunden gesenkt werden.

Das liegt daran, dass beim Schreiben eines Azure-Blobs ein Grenzwert von 60 MB/Sekunde gilt. Die Parallelität mit mehreren Blobs beseitigt zwar den Engpass, hat aber auch einen Nachteil: Die Erhöhung der Leistung des blobxfer-Tools, um alle HANA-Sicherungsdateien in Azure Blob Storage zu kopieren, ist mit zusätzlicher Last für den virtuellen HANA-Computer und das Netzwerk verbunden. Das beeinträchtigt den Betrieb des HANA-Systems.

## <a name="blob-copy-of-dedicated-azure-data-disks-in-backup-software-raid"></a>Ausführen eines Blobkopiervorgangs für dedizierte Azure-Datenträger in Software-RAID für die Sicherung

Im Gegensatz zur manuellen VM-Datenträgersicherung werden bei diesem Ansatz nicht alle Datenträger eines virtuellen Computers gesichert, um die gesamte SAP-Installation (einschließlich HANA-Daten, HANA-Protokolldateien und Konfigurationsdateien) zu speichern. Stattdessen wird eine dedizierte Software-RAID-Instanz mit übergreifendem Striping für mehrere Azure-VHDs verwendet, um eine vollständige SAP HANA-Dateisicherung zu speichern. Sie kopieren also nur diese Datenträger, auf denen sich die SAP HANA-Sicherung befindet. Die Datenträger können problemlos in einem dedizierten Speicherkonto für die HANA-Sicherung gespeichert oder zur weiteren Verarbeitung an einen dedizierten virtuellen Computer für die Sicherungsverwaltung angefügt werden.

![Alle beteiligten VHDs wurden mithilfe des PowerShell-Befehls **start-azurestorageblobcopy** kopiert.](media/sap-hana-backup-file-level/image031.png)

Nach Abschluss der Sicherung in der lokalen Software-RAID-Instanz wurden alle beteiligten VHDs mithilfe des PowerShell-Befehls **start-azurestorageblobcopy** kopiert. (Weitere Informationen finden Sie unter [Start-AzureStorageBlobCopy](/powershell/storage/azure.storage/v2.1.0/start-azurestorageblobcopy).) Da die Auswirkungen auf das dedizierte Dateisystem zur Aufbewahrung der Sicherungsdateien beschränkt sind, müssen Sie sich keine Gedanken über die Konsistenz der SAP HANA-Daten oder -Protokolldatei auf dem Datenträger machen. Ein Vorteil dieses Befehls besteht darin, dass der virtuelle Computer während der Ausführung des Befehls online bleiben kann. Um sicherzustellen, dass kein Prozess in das Stripeset der Sicherung schreibt, heben Sie vor dem Blobkopiervorgang die Bereitstellung auf, und binden Sie es anschließend wieder ein. Alternativ kann das Dateisystem auch mit einer geeigneten Methode „eingefroren“ werden. Ein Beispiel für eine solche Methode wäre etwa „xfs\_freeze“ für das XFS-Dateisystem.

![Dieser Screenshot zeigt die Liste mit Blobs im Container „vhds“ im Azure-Portal.](media/sap-hana-backup-file-level/image032.png)

Dieser Screenshot zeigt die Liste mit Blobs im Container „vhds“ im Azure-Portal. Der Screenshot zeigt die fünf VHDs, die an den virtuellen SAP HANA-Servercomputer angefügt wurden und als Software-RAID für die Aufbewahrung von SAP HANA-Sicherungsdateien fungieren. Außerdem zeigt er die fünf Kopien, die mithilfe des Blobkopierbefehls erstellt wurden.

![Zu Testzwecken wurden die Kopien der Software-RAID-Datenträger für die SAP HANA-Sicherung an den virtuellen App-Servercomputer angefügt.](media/sap-hana-backup-file-level/image033.png)

Zu Testzwecken wurden die Kopien der Software-RAID-Datenträger für die SAP HANA-Sicherung an den virtuellen App-Servercomputer angefügt.

![Der virtuelle App-Servercomputer wurde zum Anfügen der Datenträgerkopien heruntergefahren.](media/sap-hana-backup-file-level/image034.png)

Der virtuelle App-Servercomputer wurde zum Anfügen der Datenträgerkopien heruntergefahren. Nach dem Start des virtuellen Computers wurden Datenträger und RAID korrekt erkannt (eingebunden über UUID). Nur der Bereitstellungspunkt fehlte. Dieser wurde über den YaST-Partitionierer erstellt. Danach wurden die SAP HANA-Sicherungsdateikopien auf der Betriebssystemebene sichtbar.

## <a name="copy-sap-hana-backup-files-to-nfs-share"></a>Kopieren von SAP HANA-Sicherungsdateien in eine NFS-Freigabe

Manche Benutzer ziehen unter Umständen die Speicherung der SAP HANA-Sicherungsdateien auf einer NFS-Freigabe in Erwägung, um die potenziellen Auswirkungen auf die Leistung oder den Speicherplatz des SAP HANA-Systems zu verringern. Das ist zwar technisch möglich, bedeutet aber, dass Sie einen zweiten virtuellen Azure-Computer als Host für die NFS-Freigabe benötigen. Aufgrund der Netzwerkbandbreite des virtuellen Computers darf dabei keine kleine VM-Größe gewählt werden. Somit wäre es in diesem Fall sinnvoll, den virtuellen Sicherungscomputer herunterzufahren und nur für die SAP HANA-Sicherung zu aktivieren. Das Schreiben auf eine NFS-Freigabe belastet das Netzwerk und beeinträchtigt das SAP HANA-System. Die nachträgliche Verwaltung der Sicherungsdateien auf dem virtuellen Sicherungscomputer hätte hingegen keinerlei Auswirkungen auf das SAP HANA-System.

![Auf dem virtuellen SAP HANA-Servercomputer wurde eine NFS-Freigabe eines anderen virtuellen Azure-Computers eingebunden.](media/sap-hana-backup-file-level/image035.png)

Zur Überprüfung des NFS-Anwendungsfalls wurde auf dem virtuellen SAP HANA-Servercomputer eine NFS-Freigabe eines anderen virtuellen Azure-Computers eingebunden. Es wurde keine spezielle NFS-Optimierung angewendet.

![Die direkte Sicherung dauerte eine Stunde und 46 Minuten.](media/sap-hana-backup-file-level/image036.png)

Bei der NFS-Freigabe handelte es sich um ein schnelles Stripeset (wie das auf dem SAP HANA-Server). Dennoch dauerte die direkte Sicherung auf die NFS-Freigabe eine Stunde und 46 Minuten anstatt der zehn Minuten beim Schreiben in ein lokales Stripeset.

![Die Alternative war mit einer Stunde und 43 Minuten nicht viel schneller.](media/sap-hana-backup-file-level/image037.png)

Die Alternative (Sicherung in ein lokales Stripeset und anschließendes Kopieren auf die NFS-Freigabe auf der Betriebssystemebene – ein einfacher Befehl vom Typ **cp -avr**) war nicht viel schneller. Dieser Vorgang dauerte eine Stunde und 43 Minuten.

Es funktioniert also, aber die Leistung war bei dem Sicherungstest mit 230 GB nicht gut. Bei einer Datenmenge von mehreren Terabytes fällt das Ergebnis noch schlechter aus.

## <a name="copy-sap-hana-backup-files-to-azure-file-service"></a>Kopieren von SAP HANA-Sicherungsdateien in den Azure-Dateidienst

Sie können eine Azure-Dateifreigabe innerhalb eines virtuellen Azure-Linux-Computers einbinden. Ausführliche Informationen zur Vorgehensweise finden Sie im Artikel [Verwenden des Azure-Dateispeichers unter Linux](../../../storage/storage-how-to-use-files-linux.md). Vergessen Sie nicht, dass momentan eine 5-TB-Kontingentgrenze für eine einzelne Azure-Dateifreigabe und pro Datei ein Größenlimit von 1 TB gilt. Informationen zu Speichergrenzwerten finden Sie unter [Skalierbarkeits- und Leistungsziele für Azure Storage](../../../storage/storage-scalability-targets.md).

Tests haben jedoch gezeigt, dass die SAP HANA-Sicherung derzeit nicht direkt mit dieser Art von CIFS-Einbindung verwendet werden kann. Im [SAP-Hinweis 1820529](https://launchpad.support.sap.com/#/notes/1820529) wird außerdem von der Verwendung von CIFS abgeraten.

![Diese Abbildung zeigt einen Fehler im Sicherungsdialogfeld von SAP HANA-Studio.](media/sap-hana-backup-file-level/image038.png)

Diese Abbildung zeigt einen Fehler im Sicherungsdialogfeld von SAP HANA-Studio. Der Fehler tritt auf, wenn versucht wird, eine direkte Sicherung auf einer per CIFS eingebundenen Azure-Dateifreigabe auszuführen. Sie müssen also zunächst eine SAP HANA-Standardsicherung in einem VM-Dateisystem ausführen und dann die Sicherungsdateien von dort in den Azure-Dateidienst kopieren.

![Diese Abbildung zeigt, dass das Kopieren von 19 SAP HANA-Sicherungsdateien ungefähr 929 Sekunden gedauert hat.](media/sap-hana-backup-file-level/image039.png)

Diese Abbildung zeigt, dass es ungefähr 929 Sekunden gedauert hat, die 19 SAP HANA-Sicherungsdateien mit einer Gesamtgröße von etwa 230 GB in die Azure-Dateifreigabe zu kopieren.

![Die Quellverzeichnisstruktur auf dem virtuellen SAP HANA-Computer wurde in die Azure-Dateifreigabe kopiert.](media/sap-hana-backup-file-level/image040.png)

Auf diesem Screenshot ist zu sehen, dass die Quellverzeichnisstruktur auf dem virtuellen SAP HANA-Computer in die Azure-Dateifreigabe kopiert wurde: ein Verzeichnis (hana\_backup\_fsl\_15gb) und 19 einzelne Sicherungsdateien.

Das Speichern von SAP HANA-Sicherungsdateien in Azure-Dateien wird möglicherweise interessant, wenn dies von SAP HANA-Dateisicherungen direkt unterstützt wird. Interessant wird es auch, wenn Azure-Dateien über NFS eingebunden werden können und die Kontingentgrenze deutlich mehr als 5 TB beträgt.

## <a name="next-steps"></a>Nächste Schritte
* [Backup guide for SAP HANA on Azure Virtual Machines](sap-hana-backup-guide.md) (Sicherungshandbuch für SAP HANA in Azure Virtual Machines) enthält eine Übersicht und Informationen zu den ersten Schritten.
* In [SAP HANA backup based on storage snapshots](sap-hana-backup-storage-snapshots.md) (SAP HANA-Sicherung auf der Grundlage von Speichermomentaufnahmen) wird die auf Speichermomentaufnahmen basierende Sicherungsoption behandelt.
* Informationen zur Erzielung von hoher Verfügbarkeit und zur Planung der Notfallwiederherstellung für SAP HANA in Azure (große Instanzen) finden Sie unter [Hohe Verfügbarkeit und Notfallwiederherstellung für SAP HANA in Azure (große Instanzen)](hana-overview-high-availability-disaster-recovery.md).

