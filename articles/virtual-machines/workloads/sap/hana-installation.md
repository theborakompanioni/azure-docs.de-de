---
title: "Installieren von SAP HANA in Azure (große Instanzen) | Microsoft-Dokumentation"
description: "Vorgehensweise zum Installieren von SAP HANA in Azure (große Instanzen)."
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/01/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 0425da20f3f0abcfa3ed5c04cec32184210546bb
ms.openlocfilehash: 280001f9057825b9dcd98c5180340a54e2e239cf
ms.contentlocale: de-de
ms.lasthandoff: 07/20/2017

---
# <a name="how-to-install-and-configure-sap-hana-large-instances-on-azure"></a>Installieren und Konfigurieren von SAP HANA in Azure (große Instanzen)

Hier sind einige wichtige Definitionen angegeben, die Sie kennen sollten, bevor Sie diesen Leitfaden lesen. In [Übersicht und Architektur von SAP HANA in Azure (große Instanzen)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) wurden zwei unterschiedliche Klassen von HANA-Einheiten (große Instanzen) eingeführt:

- S72, S72m, S144, S144m, S192 und S192m, die wir als „Typ I“-Klassen von SKUs bezeichnen.
- S384, S384m, S384xm, S576, S768 und S960, die wir als „Typ II“-Klassen“ von SKUs bezeichnen.

Der Klassenspezifizierer wird in der gesamten Dokumentation zu HANA (große Instanzen) verwendet, um auf unterschiedliche Funktionen und Anforderungen basierend auf SKUs für HANA (große Instanzen) zu verweisen.

Weitere häufig verwendete Definitionen sind:
- **Stapel für große Instanzen:** Ein Hardwareinfrastrukturstapel, der SAP HANA TDI-zertifiziert ist und für die Ausführung von SAP HANA-Instanzen in Azure vorgesehen ist.
- **SAP HANA in Azure (große Instanzen):** Offizieller Name für das Angebot in Azure, HANA-Instanzen auf SAP HANA TDI-zertifizierter Hardware auszuführen, die in Stapeln für große Instanzen in verschiedenen Azure-Regionen bereitgestellt wird. Der zugehörige Begriff **HANA (große Instanzen)** ist die Kurzform für SAP HANA in Azure (große Instanzen) und wird in diesem Handbuch für die technische Bereitstellung gemeinhin verwendet.


Die Installation von SAP HANA liegt in Ihrer Verantwortung, und Sie können die Aktivität nach der Übergabe eines Servers für SAP HANA in Azure (große Instanzen) starten. (Und nachdem die Konnektivität zwischen Ihren Azure VNets und der bzw. den Einheiten von HANA (große Instanzen) hergestellt wurde.) 

> [!Note]
> Gemäß SAP-Richtlinie muss die Installation von SAP HANA von einer Person durchgeführt werden, die für die Durchführung von SAP HANA-Installationen zertifiziert ist. Dies ist eine Person, die die Certified SAP Technology Associate-Prüfung, die SAP HANA Installation-Zertifizierungsprüfung oder die Prüfung durch einen SAP-zertifizierten Systemintegrator (SI) bestanden hat.

Führen Sie unter [SAP Support Note #2235581 - SAP HANA: Supported Operating Systems](https://launchpad.support.sap.com/#/notes/2235581/E) (SAP-Supporthinweis #2235581 – SAP HANA: Unterstützte Betriebssysteme) erneut eine Prüfung durch, vor allem bei einer geplanten Installation von HANA 2.0, um sicherzustellen, dass das Betriebssystem von der SAP HANA-Version unterstützt wird, die Sie installiert haben. Sie sehen, dass das unterstützte Betriebssystem für HANA 2.0 stärker eingeschränkt als das Betriebssystem ist, das für HANA 1.0 unterstützt wird. 

## <a name="first-steps-after-receiving-the-hana-large-instance-units"></a>Erste Schritte nach dem Empfang der SAP HANA (große Instanzen)-Einheit(en)

Der **erste Schritt** nach dem Empfang der Einheiten von SAP HANA (große Instanzen) und der Einrichtung des entsprechenden Zugriffs und der Verbindung besteht darin, das Betriebssystem der Instanz bei Ihrem Betriebssystemanbieter zu registrieren. Zu diesem Schritt gehört das Registrieren Ihres SUSE Linux-Betriebssystems in einer Instanz von SUSE SMT, die Sie auf einer VM in Azure bereitstellen müssen. Für die Einheit von HANA (große Instanzen) kann eine Verbindung mit dieser SMT-Instanz hergestellt werden (siehe weiter unten in dieser Dokumentation). Alternativ muss Ihr RedHat-Betriebssystem bei dem Red Hat Subscription Manager registriert werden, mit dem eine Verbindung herstellen müssen. Lesen Sie auch die Anmerkungen in diesem [Dokument](https://docs.microsoft.com/azure/virtual-machines/linux/sap-hana-overview-architecture?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Dieser Schritt ist auch erforderlich, damit das Betriebssystem gepatcht werden kann. Eine Aufgabe, die in der Verantwortung des Kunden liegt. Die Dokumentation zum Installieren und Konfigurieren von SMT für SUSE finden Sie [hier](https://www.suse.com/documentation/sles-12/book_smt/data/smt_installation.html).

Der **zweite Schritt** besteht darin, nach neuen Patches und Fixes der speziellen Betriebssystemversion zu suchen. Überprüfen Sie, ob die Patchebene der SAP HANA (große Instanz) den neuesten Stand hat. Je nach Zeitplanung der Betriebssystempatches/-versionen und Änderungen am Image, das Microsoft bereitstellen kann, gibt es unter Umständen Fälle, in denen die neuesten Patches nicht enthalten sind. Daher muss, nachdem eine Einheit von SAP HANA (große Instanzen) übernommen wurde, geprüft werden, ob der jeweilige Linux-Anbieter in der Zwischenzeit Patches freigegeben hat, die für die Sicherheit, Funktionalität, Verfügbarkeit und Leistung relevant sind und angewendet werden müssen.

Der **dritte Schritt** besteht darin, die zutreffenden SAP-Hinweise (SAP Notes) zum Installieren und Konfigurieren von SAP HANA unter der speziellen Betriebssystemversion zu lesen. Wegen sich ändernden Empfehlungen oder Änderungen an SAP-Hinweisen oder -Konfigurationen, die von einzelnen Installationsszenarien abhängen, ist Microsoft nicht immer in der Lage, eine HANA (große Instanzen)-Einheit perfekt zu konfigurieren. Für Sie als Kunde ist es daher obligatorisch, dass Sie die SAP-Hinweise zu SAP HANA lesen, die exakt für Ihre Linux-Version gelten. Prüfen Sie außerdem die Konfigurationen der erforderlichen Betriebssystemversion, und wenden Sie die Konfigurationseinstellungen an, falls dies nicht bereits geschehen ist.

Prüfen Sie insbesondere die folgenden Parameter, und passen Sie diese ggf. an:

- net.core.rmem_max = 16777216
- net.core.wmem_max = 16777216
- net.core.rmem_default = 16777216
- net.core.wmem_default = 16777216
- net.core.optmem_max = 16777216
- net.ipv4.tcp_rmem = 65536 16777216 16777216
- net.ipv4.tcp_wmem = 65536 16777216 16777216

Seit SLES12 SP1 und RHEL 7.2 müssen diese Parameter in einer Konfigurationsdatei festgelegt sein, die sich im Verzeichnis „/etc/sysctl.d“ befindet. Beispielsweise muss eine Konfigurationsdatei namens „91-NetApp-HANA.conf“ erstellt werden. Für ältere SLES- und RHEL-Versionen müssen diese Parameter in „/etc/sysctl.conf“ festgelegt sein.

Für alle RHEL-Versionen und ab SLES12 muss der Parameter 
- sunrpc.tcp_slot_table_entries = 128

in „/etc/modprobe.d/sunrpc-local.conf“ festgelegt sein. Wenn die Datei nicht vorhanden ist, muss sie zunächst erstellt werden, indem der folgende Eintrag hinzugefügt wird: 
- options sunrpc tcp_max_slot_table_entries=128

Der **vierte Schritt** besteht darin, die Systemzeit Ihrer HANA (große Instanzen)-Einheit zu prüfen. Die Instanzen werden mit einer Systemzeitzone bereitgestellt, die dem Standort der Azure-Region entspricht, in der sich der Stempel für HANA (große Instanzen) befindet. Sie können die Systemzeit oder Zeitzone der Instanzen ändern, deren Besitzer Sie sind. Wenn Sie dies tun und weitere Instanzen in Ihrem Mandanten bestellen, müssen Sie auch die Zeitzone der neu bereitgestellten Instanzen anpassen. Microsoft-Vorgänge haben keinen Einblick in die Systemzeitzone, die Sie nach der Übergabe für die Instanzen eingerichtet haben. Daher kann es sein, dass neu bereitgestellte Instanzen nicht mit der Zeitzone eingerichtet sind, zu der Sie gewechselt haben. Folglich liegt es in Ihrer Verantwortung als Kunde, die Zeitzone der übergebenen Instanzen zu prüfen und ggf. anzupassen. 

Der **fünfte Schritt** besteht darin, „etc/hosts“ zu prüfen. Wenn die Blades übergeben werden, haben sie unterschiedliche IP-Adressen, die für unterschiedliche Zwecke zugeordnet sind (siehe nächster Abschnitt). Überprüfen Sie die Datei „etc/hosts“. In Fällen, in denen Einheiten in einem vorhandenen Mandanten hinzugefügt werden, dürfen Sie nicht erwarten, dass „etc/hosts“ der neu bereitgestellten Systeme ordnungsgemäß mit den IP-Adressen von früher bereitgestellten Systemen gewartet wird. Daher müssen Sie als Kunde die richtigen Einstellungen vornehmen, damit eine neu bereitgestellte Instanz die Namen von früher bereitgestellten Einheiten in Ihrem Mandanten auflösen kann. 

## <a name="networking"></a>Netzwerk
Es wird davon ausgegangen, dass Sie beim Entwerfen Ihrer Azure-VNets und Verbinden dieser VNets mit den Einheiten von HANA (große Instanzen) den Empfehlungen gefolgt sind, die in diesen Dokumenten beschrieben sind:

- [Übersicht und Architektur von SAP HANA in Azure (große Instanzen)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)
- [Infrastruktur und Verbindungen mit SAP HANA in Azure (große Instanzen)](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Es gibt einige Details, die zu dem Netzwerkbetrieb der einzelnen Einheiten bekannt sein sollten. Jede HANA (große Instanzen)-Einheit wird mit zwei oder drei IP-Adressen geliefert, die zwei oder drei Netzwerkkartenports der Einheit zugewiesen sind. Drei IP-Adressen werden in horizontal skalierten HANA-Konfigurationen und im Szenario für HANA-Systemreplikation verwendet. Eine der IP-Adressen, die der Netzwerkkarte der Einheit zugewiesen sind, stammt aus dem Server-IP-Pool, der in [Übersicht und Architektur von SAP HANA in Azure (große Instanzen)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) beschrieben ist.

Die Verteilung für Einheiten mit zwei zugewiesenen IP-Adressen sollte wie folgt aussehen:

- „eth0.xx“ muss eine zugewiesene IP-Adresse haben, die aus dem Server-IP-Pool-Adressbereich stammt, den Sie an Microsoft übermittelt haben. Diese IP-Adresse muss zur Verwaltung in „/etc/hosts“ des Betriebssystems verwendet werden.
- „eth1.xx“ muss eine zugewiesene IP-Adresse haben, die für die Kommunikation mit NFS verwendet wird. Daher muss diese Adresse **NICHT** in „etc/hosts“ verwaltet werden, um Instanz-zu-Instanz-Datenverkehr im Mandanten zu ermöglichen.

Für Bereitstellungsfälle von HANA-Systemreplikation oder horizontal skaliertem HANA ist eine Bladekonfiguration mit zwei IP-Adressen nicht geeignet. Wenn Sie nur zwei zugewiesene IP-Adressen haben, aber eine solche Konfiguration bereitstellen möchten, wenden Sie sich an das SAP HANA in Azure-Dienstverwaltungsteam, um eine dritte IP-Adresse zu erhalten, die in einem dritten VLAN zugewiesen ist. Für Einheiten von HANA (große Instanzen) mit drei zugewiesenen IP-Adressen auf drei Netzwerkkartenports gelten die folgenden Verwendungsregeln:

- „eth0.xx“ muss eine zugewiesene IP-Adresse haben, die aus dem Server-IP-Pool-Adressbereich stammt, den Sie an Microsoft übermittelt haben. Daher darf diese IP-Adresse nicht zur Verwaltung in „/etc/hosts“ des Betriebssystems verwendet werden.
- „eth1.xx“ muss eine zugewiesene IP-Adresse haben, die für die Kommunikation mit NFS-Speicher verwendet wird. Daher darf dieser Typ von Adressen nicht in „etc/hosts“ verwaltet werden.
- „eth2.xx“ muss ausschließlich verwendet werden, um in „etc/hosts“ für die Kommunikation zwischen den verschiedenen Instanzen verwaltet zu werden. Diese Adressen wären auch die IP-Adressen, die in horizontal skalierten HANA-Konfigurationen als IP-Adressen verwaltet werden müssen, die HANA für die Konfiguration zwischen Knoten verwendet.



## <a name="storage"></a>Speicher

Die Speicheranordnung für SAP HANA in Azure (große Instanzen) wird von der SAP HANA in Azure-Dienstverwaltung gemäß den von SAP empfohlenen Richtlinien konfiguriert. Diese Richtlinien sind im Whitepaper [SAP HANA Storage Requirements](http://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) (SAP HANA-Speicheranforderungen) dokumentiert. Die allgemeinen Größen der unterschiedlichen Volumes mit den einzelnen SKUs für HANA (große Instanzen) sind unter [Übersicht und Architektur von SAP HANA in Azure (große Instanzen)](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) dokumentiert.

Die Benennungskonventionen der Speichervolumes sind in der folgenden Tabelle aufgeführt:

| Speicherverwendung | Bereitstellungsname | Volumename | 
| --- | --- | ---|
| HANA-Daten | /hana/data/SID/mnt0000<m> | Speicher-IP: /hana_data_SID_mnt00001_tenant_vol |
| HANA-Protokoll | /hana/log/SID/mnt0000<m> | Speicher-IP: /hana_log_SID_mnt00001_tenant_vol |
| HANA-Protokollsicherung | /hana/log/backups | Speicher-IP: /hana_log_backups_SID_mnt00001_tenant_vol |
| HANA-Freigabe | /hana/shared/SID | Speicher-IP: /hana_shared_SID_mnt00001_tenant_vol/shared |
| usr/sap | /usr/sap/SID | Speicher-IP: /hana_shared_SID_mnt00001_tenant_vol/usr_sap |

SID = System-ID der HANA-Instanz 

tenant = interne Enumeration von Vorgängen beim Bereitstellen eines Mandanten

Sie sehen, dass für die HANA-Freigabe und „usr/sap“ dasselbe Volume verwendet wird. Die Nomenklatur der Bereitstellungspunkte enthält sowohl die System-ID der HANA-Instanzen als auch die Bereitstellungsnummer. Bei Bereitstellungen für zentrales Skalieren ist nur eine Bereitstellung (Mount) vorhanden, z.B. „mnt00001“. Bei der Bereitstellung für horizontales Skalieren werden so viele „Mounts“ verwendet, wie Worker und Masterknoten vorhanden sind. Für die Umgebung für horizontales Skalieren werden Daten-, Protokoll- und Protokollsicherungsvolumes gemeinsam genutzt und jeweils an die Knoten der Konfiguration für horizontales Skalieren angefügt. Für Konfigurationen mit mehreren SAP-Instanzen wird ein anderer Satz mit Volumes erstellt und an die Einheit von HANA (große Instanzen) angefügt.

Während Sie das Dokument lesen und sich eine Einheit von HANA (große Instanzen) ansehen, werden Sie feststellen, dass die Einheiten mit einem ziemlich großzügigen Datenträgervolume für „HANA/data“ bereitgestellt werden und dass es das Volume „HANA/log/backup“ gibt. Wir haben „HANA/data“ so großzügig dimensioniert, weil für die Speichermomentaufnahmen, die Ihnen als Kunde angeboten werden, dasselbe Datenträgervolume verwendet wird. Dies bedeutet Folgendes: Je mehr Speichermomentaufnahmen Sie erstellen, desto mehr Speicherplatz wird von Momentaufnahmen in Ihren zugewiesenen Speichervolumes verbraucht. Das Volume „HANA/log/backup“ ist nicht für die Speicherung von Datenbanksicherungen vorgesehen. Seine Größe ist so ausgelegt, dass es als Sicherungsvolume für Sicherungen von HANA-Transaktionsprotokollen verwendet werden kann. Für zukünftige Versionen des Speichermomentaufnahmen-Self-Service ist für dieses spezielle Volume geplant, häufigere Momentaufnahmen zu verwenden. Hieraus ergeben sich auch häufigere Replikationen zum Notfallwiederherstellungsstandort, wenn Sie die Notfallwiederherstellungsfunktionalität nutzen möchten, die von der HANA (große Instanzen)-Infrastruktur bereitgestellt wird. Ausführlichere Informationen finden Sie unter [Hohe Verfügbarkeit und Notfallwiederherstellung für SAP HANA in Azure (große Instanzen)](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Zusätzlich zu dem bereitgestellten Speicher können Sie weitere Speicherkapazität in Schritten von 1 TB erwerben. Dieser zusätzliche Speicher kann HANA (große Instanzen) als neue Volumes hinzugefügt werden.

Während des Onboardings mit SAP HANA in Azure-Dienstverwaltungsteam gibt der Kunde eine Benutzer-ID (UID) und Gruppen-ID (GID) für den sidadm-Benutzer und die „sapsys“-Gruppe an (Beispiel: 1000,500). Es ist erforderlich, dass während der Installation des SAP HANA-Systems genau diese Werte verwendet werden. Da Sie mehrere HANA-Instanzen in einer Einheit bereitstellen möchten, erhalten Sie mehrere Gruppen von Volumes (eine Gruppe für jede Instanz). Daher müssen Sie zur Bereitstellungszeit Folgendes definieren:

- Die SID der unterschiedlichen HANA-Instanzen (sidadm wird daraus abgeleitet).
- Arbeitsspeichergrößen der unterschiedlichen HANA-Instanzen. Die Arbeitsspeichergröße pro Instanz definiert die Größe der Volumes in den einzelnen Volumesätzen.

Basierend auf den Empfehlungen von Speicheranbietern werden für alle bereitgestellten Volumes (mit Ausnahme der Start-LUN) die folgenden Bereitstellungsoptionen konfiguriert:

- nfs    rw, vers=4, hard, timeo=600, rsize=1048576, wsize=1048576, intr, noatime, lock 0 0

Diese Bereitstellungspunkte werden unter „/etc/fstab“ konfiguriert, wie in den folgenden Grafiken dargestellt:

![fstab von bereitgestellten Volumes in Einheit von HANA (große Instanzen)](./media/hana-installation/image1_fstab.PNG)

Die Ausgabe des Befehls df -h in einer S72m-Einheit von HANA (große Instanzen) sieht wie folgt aus:

![fstab von bereitgestellten Volumes in Einheit von HANA (große Instanzen)](./media/hana-installation/image2_df_output.PNG)


Der Speichercontroller und die Knoten in den „Große Instanz“-Stapeln werden mit NTP-Servern synchronisiert. Beim Synchronisieren der SAP HANA in Azure (große Instanzen)-Einheiten und Azure-VMs mit einem NTP-Server darf es zwischen der Infrastruktur und den Compute-Einheiten in Azure- oder „Große Instanz“-Stapeln keine signifikante Zeitabweichung geben.

Um SAP HANA für den darunter verwendeten Speicher zu optimieren, sollten Sie auch die folgenden SAP HANA-Konfigurationsparameter festlegen:

- max_parallel_io_requests 128
- async_read_submit on
- async_write_submit_active on
- async_write_submit_blocks all
 
Für SAP HANA 1.0-Versionen bis SPS12 können diese Parameter während der Installation der SAP HANA-Datenbank festgelegt werden, wie dies in [SAP Note #2267798 - Configuration of the SAP HANA Database](https://launchpad.support.sap.com/#/notes/2267798) beschrieben ist.

Sie können die Parameter auch nach der Installation der SAP HANA-Datenbank konfigurieren, indem Sie das hdbparam-Framework verwenden. 

Seit SAP HANA Version 2.0 ist das hdbparam-Framework veraltet. Daher müssen die Parameter nun über SQL-Befehle festgelegt werden. Ausführliche Informationen finden Sie in [SAP Note #2399079: Elimination of hdbparam in HANA 2](https://launchpad.support.sap.com/#/notes/2399079).


## <a name="operating-system"></a>Betriebssystem

Der Auslagerungsbereich des bereitgestellten Betriebssystemimage ist gemäß [SAP Support Note #1999997 - FAQ: SAP HANA Memory](https://launchpad.support.sap.com/#/notes/1999997/E) (SAP-Supporthinweis #1999997 – FAQ: SAP HANA-Arbeitsspeicher) auf 2 GB festgelegt. Alle anderen gewünschten Einstellungen müssen von Ihnen als Kunde festgelegt werden.

[SUSE Linux Enterprise Server 12 SP1 for SAP Applications](https://www.suse.com/products/sles-for-sap/hana) ist die Distribution von Linux, die für SAP HANA in Azure (große Instanzen) installiert wird. Diese besondere Distribution bietet &quot;standardmäßig&quot; SAP-spezifische Funktionen (einschließlich vorab festgelegter Parameter für die effektive Ausführung von SAP unter SLES).

Auf der SUSE-Website unter [Ressourcen Library/White Papers](https://www.suse.com/products/sles-for-sap/resource-library#white-papers) und unter [SAP on SUSE](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+SUSE) im SAP Community Network (SCN) finden Sie verschiedene nützliche Ressourcen zur Bereitstellung von SAP HANA unter SLES (einschließlich u.a. zur Einrichtung für hohe Verfügbarkeit und Sicherheitshärtung für den SAP-Betrieb).

Zusätzliche und nützliche Links zu SAP unter SUSE:

- [Website zu SAP HANA unter SUSE Linux](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+SUSE)
- [Best Practice for SAP: Enqueue Replication - SAP NetWeaver on SUSE Linux Enterprise 12](https://www.suse.com/docrepcontent/container.jsp?containerId=9113) (Bewährte Methode für SAP: Enqueue-Replikation – SAP NetWeaver unter SUSE Linux Enterprise 12)
- [ClamSAP - SLES Virus Protection for SAP](http://scn.sap.com/community/linux/blog/2014/04/14/clamsap--suse-linux-enterprise-server-integrates-virus-protection-for-sap) (ClamSAP – SLES-Virenschutz für SAP) (einschließlich SLES 12 für SAP-Anwendungen)

SAP-Supporthinweise zur Implementierung von SAP HANA unter SLES 12:

- [SAP Support Note #1944799 – SAP HANA Guidelines for SLES Operating System Installation](http://go.sap.com/documents/2016/05/e8705aae-717c-0010-82c7-eda71af511fa.html) (SAP-Supporthinweis #1944799 – SAP HANA-Richtlinien für die Installation des SLES-Betriebssystems)
- [SAP Support Note #2205917 – SAP HANA DB Recommended OS Settings for SLES 12 for SAP Applications](https://launchpad.support.sap.com/#/notes/2205917/E) (SAP-Supporthinweis #2205917 – SAP HANA DB – Empfohlene Betriebssystemeinstellungen für SLES 12 für SAP-Anwendungen)
- [SAP Support Note #1984787 – SUSE Linux Enterprise Server 12: Installation Notes](https://launchpad.support.sap.com/#/notes/1984787) (SAP-Supporthinweis #1984787 – SUSE Linux Enterprise Server 12: Installationshinweise)
- [SAP Support Note #171356 – SAP Software on Linux: General Information](https://launchpad.support.sap.com/#/notes/1984787) (SAP-Supporthinweis #171356 – SAP-Software unter Linux: Allgemeine Informationen)
- [SAP Support Note #1391070 – Linux UUID Solutions](https://launchpad.support.sap.com/#/notes/1391070) (SAP-Supporthinweis #1391070 – Linux UUID-Lösungen)

[Red Hat Enterprise Linux für SAP HANA](https://www.redhat.com/en/resources/red-hat-enterprise-linux-sap-hana) ist ein weiteres Angebot zum Ausführen von SAP HANA auf großen SAP HANA-Instanzen. Versionen von RHEL 6.7 und 7.2 sind verfügbar. 

Zusätzliche und nützliche Links zu SAP unter Red Hat:
- [SAP HANA on Red Hat Linux Site](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+Red+Hat) (Website zu SAP HANA unter Red Hat)

SAP-Supporthinweise zur Implementierung von SAP HANA unter Red Hat:

- [SAP Support Note #2009879 – SAP HANA Guidelines for Red Hat Enterprise Linux (RHEL) Operating System](https://launchpad.support.sap.com/#/notes/2009879/E) (SAP-Supporthinweis #2009879 – SAP HANA-Richtlinien für das RHEL-Betriebssystem (Red Hat Enterprise Linux))
- [SAP Support Note #2292690 - SAP HANA DB: Recommended OS Settings for RHEL 7](https://launchpad.support.sap.com/#/notes/2292690) (SAP-Supporthinweis #2292690 – SAP HANA DB: Empfohlene Betriebssystemeinstellungen für RHEL 7)
- [SAP Support Note #2247020 - SAP HANA DB: Recommended OS settings for RHEL 6.7](https://launchpad.support.sap.com/#/notes/2247020) (SAP-Supporthinweis #2247020 – SAP HANA DB: Empfohlene Betriebssystemeinstellungen für RHEL 6.7)
- [SAP Support Note #1391070 – Linux UUID Solutions](https://launchpad.support.sap.com/#/notes/1391070) (SAP-Supporthinweis #1391070 – Linux UUID-Lösungen)
- [SAP Support Note #2228351 - Linux: SAP HANA Database SPS 11 revision 110 (or higher) on RHEL 6 or SLES 11](https://launchpad.support.sap.com/#/notes/2228351) (SAP-Supporthinweis #2228351 – Linux: SAP HANA-Datenbank SPS 11 Revision 110 (oder höher) unter RHEL 6 oder SLES 11)
- [SAP Support Note #2397039 - FAQ: SAP on RHEL](https://launchpad.support.sap.com/#/notes/2397039) (SAP-Supporthinweis #2397039 – FAQ: SAP unter RHEL)
- [SAP Support Note #1496410 - Red Hat Enterprise Linux 6.x: Installation and Upgrade](https://launchpad.support.sap.com/#/notes/1496410) (SAP-Supporthinweis #1496410 – Red Hat Enterprise Linux 6.x: Installation und Upgrade)
- [SAP Support Note #2002167 - Red Hat Enterprise Linux 7.x: Installation and Upgrade](https://launchpad.support.sap.com/#/notes/2002167) (SAP-Supporthinweis #2002167 – Red Hat Enterprise Linux 7.x: Installation und Upgrade)

## <a name="time-synchronization"></a>Zeitsynchronisierung

SAP-Anwendungen, die basierend auf der SAP NetWeaver-Architektur erstellt werden, reagieren sehr empfindlich auf Zeitunterschiede bei den verschiedenen Komponenten, aus denen das SAP-System besteht. SAP ABAP-Kurzdumps mit dem Fehlertitel ZDATE\_LARGE\_TIME\_DIFF sind Ihnen wahrscheinlich vertraut. Diese Kurzdumps werden nämlich angezeigt, wenn die Systemzeit auf den verschiedenen Servern bzw. VMs zu stark voneinander abweicht.

Bei SAP HANA in Azure (große Instanzen) gilt die in Azure erfolgende Zeitsynchronisierung nicht für die Compute-Einheiten in „Große Instanz“-Stapeln. Diese Synchronisierung gilt nicht für die native Ausführung von SAP-Anwendungen auf nativen Azure-VMs, da in Azure sichergestellt wird, dass die Uhrzeit eines Systems richtig synchronisiert wird. Deshalb muss ein getrennter Zeitserver eingerichtet werden, der von auf Azure-VMs ausgeführten SAP-Anwendungsservern und den SAP HANA-Datenbankinstanzen verwendet werden kann, die in HANA (große Instanzen) ausgeführt werden. Der Speicherinfrastruktur in „Große Instanz“-Stapeln wird mit NTP-Servern zeitlich synchronisiert.

## <a name="setting-up-smt-server-for-suse-linux"></a>Einrichten eines SMT-Servers für SUSE Linux
Für SAP HANA (große Instanzen) besteht keine direkte Verbindung mit dem Internet. Daher ist das Registrieren der Einheit beim Betriebssystemanbieter und das Herunterladen und Anwenden von Patches ein etwas aufwändigerer Vorgang. Für SUSE Linux ist eine mögliche Lösung ggf. die Einrichtung eines SMT-Servers auf einer Azure-VM. Die Azure-VM muss dagegen in einem Azure VNet gehostet werden, das mit der Einheit von HANA (große Instanzen) verbunden ist. Mit einem SMT-Server dieser Art ist die Einheit von HANA (große Instanzen) dazu in der Lage, Patches zu registrieren und herunterzuladen. 

Für SUSE ist unter [Subscription Management Tool for SLES 12 SP2](https://www.suse.com/documentation/sles-12/pdfdoc/book_smt/book_smt.pdf) (Subscription Management Tool für SLES 12 SP2) ein umfangreicherer Leitfaden verfügbar. 

Als Voraussetzung für die Installation eines SMT-Servers, der diese Aufgabe für HANA (große Instanzen) übernimmt, benötigen Sie Folgendes:

- Ein Azure VNet, das mit der ER-Verbindung von HANA (große Instanzen) verbunden ist.
- Ein SUSE-Konto, das einer Organisation zugeordnet ist. Für die Organisation ist ein gültiges SUSE-Abonnement erforderlich.

### <a name="installation-of-smt-server-on-azure-vm"></a>Installation des SMT-Servers auf einer Azure-VM

In diesem Schritt installieren Sie den SMT-Server auf einer Azure-VM. Die erste Maßnahme ist die Anmeldung im [SUSE Customer Center](https://scc.suse.com/).

Navigieren Sie nach dem Anmelden zu „Organization“ > „Organization Credentials“. In diesem Abschnitt sind die Anmeldeinformationen enthalten, die zum Einrichten des SMT-Servers erforderlich sind.

Im dritten Schritt installieren Sie eine SUSE Linux-VM im Azure VNet. Verwenden Sie zum Bereitstellen der VM ein SLES 12 SP2-Katalogimage von Azure. Definieren Sie während des Bereitstellungsprozesses keinen DNS-Namen, und verwenden Sie auch keine statischen IP-Adressen (siehe Screenshot).

![VM-Bereitstellung für SMT-Server](./media/hana-installation/image3_vm_deployment.png)

Die bereitgestellte VM war eine kleinere VM und hat im Azure VNet die interne IP-Adresse 10.34.1.4 erhalten. Der Name der VM lautet „smtserver“. Nach der Installation wurde die Konnektivität mit den Einheiten von HANA (große Instanzen) überprüft. Je nachdem, wie Sie die Namensauflösung organisiert haben, müssen Sie unter Umständen die Auflösung der Einheiten von HANA (große Instanzen) auf der Azure-VM unter „etc/hosts“ konfigurieren. Fügen Sie der VM einen zusätzlichen Datenträger hinzu, der für Patches verwendet werden soll. Der Startdatenträger selbst ist unter Umständen zu klein. Im gezeigten Fall wurde der Datenträger unter „/srv/www/htdocs“ bereitgestellt. Dies ist im folgenden Screenshot dargestellt. Eine Datenträger mit 100 GB sollte hierfür ausreichen.

![VM-Bereitstellung für SMT-Server](./media/hana-installation/image4_additional_disk_on_smtserver.PNG)

Melden Sie sich an der bzw. den Einheiten von HANA (große Instanzen) an, verwalten Sie „/etc/hosts“, und überprüfen Sie, ob Sie die Azure-VM erreichen können, mit der der SMT-Server über das Netzwerk ausgeführt werden soll.

Nachdem diese Überprüfung erfolgreich abgeschlossen wurde, müssen Sie sich an der Azure-VM anmelden, auf der der SMT-Server ausgeführt werden soll. Wenn Sie zum Anmelden an der VM PuTTY verwenden, müssen Sie diese Befehlssequenz in Ihrem Bash-Fenster ausführen:

```
cd ~
echo "export NCURSES_NO_UTF8_ACS=1" >> .bashrc
```

Starten Sie nach dem Ausführen dieser Befehle Ihre Bash-Instanz neu, um die Einstellungen zu aktivieren. Starten Sie anschließend YAST.

Navigieren Sie in YAST zu „Software Maintenance“, und suchen Sie nach „smt“. Wählen Sie „smt“ aus, um automatisch zu „yast2-smt“ zu wechseln. Dies ist unten dargestellt.

![SMT in YAST](./media/hana-installation/image5_smt_in_yast.PNG)


Übernehmen Sie die Auswahl für die Installation von smtserver. Navigieren Sie nach Abschluss der Installation zur SMT-Serverkonfiguration, und geben Sie die Anmeldeinformationen für die Organisation aus dem SUSE Customer Center ein, die Sie weiter oben abgerufen haben. Geben Sie außerdem Ihren Azure-VM-Hostnamen als SMT Server-URL ein. In dieser Demonstration war dies „https://smtserver“, wie in den nächsten Grafiken dargestellt.

![Konfiguration des SMT-Servers](./media/hana-installation/image6_configuration_of_smtserver1.png)

Im nächsten Schritt testen Sie, ob die Verbindung mit dem SUSE Customer Center funktioniert. Wie in den folgenden Grafiken zu sehen ist, wurde die Verbindung für die Demonstration erfolgreich hergestellt.

![Testen der Verbindung mit dem SUSE Customer Center](./media/hana-installation/image7_test_connect.png)

Nach dem Start des SMT-Setups müssen Sie ein Datenbankkennwort angeben. Da es sich um eine neue Installation handelt, müssen Sie dieses Kennwort wie in den nächsten Grafiken definieren.

![Definieren des Kennworts für die Datenbank](./media/hana-installation/image8_define_db_passwd.PNG)

Die nächste Interaktion ist erforderlich, wenn ein Zertifikat erstellt wird. Navigieren Sie wie unten gezeigt durch das Dialogfeld, um den Schritt auszuführen.

![Erstellen eines Zertifikats für den SMT-Server](./media/hana-installation/image9_certificate_creation.PNG)

Es kann sein, dass der Schritt „Run synchronization check“ am Ende der Konfiguration einige Minuten dauert. Nach der Installation und Konfiguration des SMT-Servers sollten sich das Verzeichnisrepository unter dem Bereitstellungspunkt „/srv/www/htdocs/“ und einige Unterverzeichnisse unter „repo“ befinden. 

Starten Sie den SMT-Server und die dazugehörigen Dienste mit diesen Befehlen neu.

```
rcsmt restart
systemctl restart smt.service
systemctl restart apache2
```

### <a name="download-of-packages-onto-smt-server"></a>Herunterladen von Paketen auf den SMT-Server

Wählen Sie nach dem Neustart aller Dienste unter SMT Management per Yast die entsprechenden Pakete aus. Die Paketauswahl hängt vom Betriebssystemimage des Servers für HANA (große Instanzen) und nicht vom SLES-Release bzw. der -Version der VM ab, auf der der SMT-Server ausgeführt wird. Unten ist ein Beispiel für den Auswahlbildschirm angegeben.

![Auswählen von Paketen](./media/hana-installation/image10_select_packages.PNG)

Nachdem Sie die Paketauswahl abgeschlossen haben, müssen Sie den ersten Kopiervorgang für die ausgewählten Pakete auf den eingerichteten SMT-Server starten. Dieser Kopiervorgang wird in der Shell ausgelöst, indem Sie wie unten gezeigt den Befehl „smt-mirror“ verwenden.


![Herunterladen von Paketen auf den SMT-Server](./media/hana-installation/image11_download_packages.PNG)

Wie oben dargestellt, sollten die Pakete in die Verzeichnisse kopiert werden, die unter dem Bereitstellungspunkt „/srv/www/htdocs“ erstellt wurden. Dieser Vorgang kann eine Weile dauern. Je nach Anzahl von ausgewählten Paketen kann eine Stunde oder mehr Zeit benötigt werden.
Nachdem dieser Vorgang abgeschlossen ist, führen Sie als Nächstes das Setup für den SMT-Client durch. 

### <a name="set-up-the-smt-client-on-hana-large-instance-units"></a>Einrichten des SMT-Clients auf Einheiten von HANA (große Instanzen)

Die Clients sind in diesem Fall die Einheiten von HANA (große Instanzen). Beim Setup für den SMT-Server wurde das Skript „clientSetup4SMT.sh“ auf die Azure-VM kopiert. Kopieren Sie dieses Skript auf die Einheit von HANA (große Instanzen), für die Sie eine Verbindung mit Ihrem SMT-Server herstellen möchten. Starten Sie das Skript mit der Option „-h“, und verwenden Sie den Namen Ihres SMT-Servers als Parameter. In diesem Beispiel ist dies „smtserver“.

![Konfigurieren des SMT-Clients](./media/hana-installation/image12_configure_client.PNG)

Es kann zu einem Szenario kommen, bei dem das Laden des Zertifikats vom Server durch den Client erfolgreich ist, für die Registrierung aber ein Fehler auftritt. Dies ist unten dargestellt.

![Fehler bei der Registrierung des Clients](./media/hana-installation/image13_registration_failed.PNG)

Lesen Sie dieses [Dokument des SUSE-Supports](https://www.suse.com/de-de/support/kb/doc/?id=7006024), und führen Sie die hier beschriebenen Schritte aus, falls für die Registrierung ein Fehler auftritt.

> [!IMPORTANT] 
> Als Servername müssen Sie den Namen der VM (hier „smtserver“) ohne den vollqualifizierten Domänennamen angeben. Es funktioniert, wenn Sie nur den VM-Namen angeben. 

Nachdem diese Schritte ausgeführt wurden, müssen Sie den folgenden Befehl auf der Einheit von HANA (große Instanzen) ausführen:

```
SUSEConnect –cleanup
```

> [!Note] 
> Bei unseren Tests mussten wir nach diesem Schritt immer einige Minuten warten. Die sofortige Ausführung von „clientSetup4SMT.sh“ laut den Korrekturmaßnahmen im SUSE-Artikel hat zu Meldungen geführt, dass das Zertifikat noch nicht gültig ist. Nach einer Wartezeit von fünf bis zehn Minuten und der Ausführung von „clientSetup4SMT.sh“ ist die Clientkonfiguration aber normalerweise erfolgreich.

Falls ein Problem auftritt, bei dem Sie gemäß den Schritten im SUSE-Artikel eine Korrektur durchführen müssen, müssen Sie „clientSetup4SMT.sh“ für die Einheit von HANA (große Instanzen) noch einmal neu starten. Der Vorgang sollte jetzt wie unten dargestellt erfolgreich abgeschlossen werden können.

![Clientregistrierung erfolgreich](./media/hana-installation/image14_finish_client_config.PNG)

Mit diesem Schritt haben Sie den SMT-Client für die Einheit von HANA (große Instanzen) konfiguriert, und die Verbindung mit dem auf der Azure-VM installierten SMT-Server kann hergestellt werden. Sie können nun „zypper up“ oder „zypper in“ verwenden, um Betriebssystempatches für HANA (große Instanzen) oder weitere Pakete zu installieren. Hierbei ist klar, dass Sie nur Patches nutzen können, die Sie vorher auf dem SMT-Server heruntergeladen haben.


## <a name="example-of-an-sap-hana-installation-on-hana-large-instances"></a>Beispiel für eine SAP HANA-Installation für HANA (große Instanzen)
In diesem Abschnitt wird veranschaulicht, wie Sie SAP HANA auf einer Einheit von HANA (große Instanzen) installieren. Wir verfügen über den folgenden Anfangszustand:

- Sie haben Microsoft alle Daten geliefert, die auf einer Einheit von SAP HANA (große Instanzen) bereitgestellt werden sollen.
- Sie haben von Microsoft die Einheit von SAP HANA (große Instanzen) erhalten.
- Sie haben ein Azure VNet erstellt, das mit Ihrem lokalen Netzwerk verbunden ist.
- Sie haben die ExpressRoute-Verbindung für HANA (große Instanzen) mit demselben Azure VNet hergestellt.
- Sie haben eine Azure-VM installiert, die Sie als Jumpbox für HANA (große Instanzen) verwenden.
- Sie haben sichergestellt, dass Sie eine Verbindung von der Jumpbox zur Einheit von HANA (große Instanzen) und umgekehrt herstellen können.
- Sie haben überprüft, ob alle erforderlichen Pakete und Patches installiert sind.
- Sie haben die SAP-Hinweise und die Dokumentation zur HANA-Installation auf dem von Ihnen verwendeten Betriebssystem gelesen und sich vergewissert, dass das gewünschte HANA-Release für die Betriebssystemversion unterstützt wird.

In den nächsten Sequenzen wird das Herunterladen der HANA-Installationspakete auf die Jumpbox-VM (hier unter einem Windows-Betriebssystem), das Kopieren der Pakete auf die Einheit von HANA (große Instanzen) und die Setup-Sequenz veranschaulicht.

### <a name="download-of-the-sap-hana-installation-bits"></a>Herunterladen der Daten für die SAP HANA-Installation
Da für die Einheiten von HANA (große Instanzen) keine direkte Verbindung mit dem Internet besteht, können Sie die Installationspakete nicht direkt von SAP auf die VM mit HANA (große Instanzen) herunterladen. Sie benötigen die Jumpbox, um das Fehlen der direkten Internetverbindung auszugleichen. Sie laden die Pakete auf die Jumpbox-VM herunter.

Zum Herunterladen der HANA-Installationspakete benötigen Sie einen SAP S-Benutzer oder einen anderen Benutzer, mit dem Sie auf den SAP Marketplace zugreifen können. Durchlaufen Sie nach dem Anmelden die folgende Bildschirmsequenz:

Navigieren Sie zu [SAP Service Marketplace](https://support.sap.com/en/index.html). Klicken Sie auf „Download Software“ > „Installations and Upgrade“ > „By Alphabetical Index“ > „Under H – SAP HANA Platform Edition“ > „SAP HANA Platform Edition 2.0“ > „Installation“, und laden Sie die folgenden Dateien herunter:

![Herunterladen der HANA-Installation](./media/hana-installation/image16_download_hana.PNG)

Für die Demonstration haben wir SAP HANA 2.0-Installationspakete heruntergeladen. Auf der Azure-Jumpbox-VM erweitern Sie die selbstextrahierenden Archive wie unten gezeigt in das Verzeichnis.

![Extrahieren der HANA-Installation](./media/hana-installation/image17_extract_hana.PNG)

Kopieren Sie nach dem Extrahieren der Archive das Verzeichnis, das bei der Extraktion extrahiert wurde (im obigen Fall „51052030“), auf die Einheit von HANA (große Instanzen) im Volume „/hana/shared“ des von Ihnen erstellten Verzeichnisses.

> [!Important]
> Kopieren Sie die Installationspakete NICHT in die Stamm- oder Start-LUN, da der Speicherplatz begrenzt ist und auch von anderen Prozessen genutzt werden muss.


### <a name="install-sap-hana-on-the-hana-large-instance-unit"></a>Installieren von SAP HANA auf der Einheit von HANA (große Instanzen)
Für die Installation von SAP HANA müssen Sie sich als Stammbenutzer (root) anmelden. Nur der Stammbenutzer verfügt über ausreichende Berechtigungen zum Installieren von SAP HANA.
Als Erstes müssen Sie Berechtigungen für das Verzeichnis festlegen, das Sie in „/hana/shared“ kopiert haben. Legen Sie die Berechtigungen wie folgt fest:

```
chmod –R 744 <Installation bits folder>
```

Wenn Sie SAP HANA per grafischem Setup installieren möchten, muss das Paket gtk2 auf der Einheit von HANA (große Instanzen) installiert werden. Überprüfen Sie mit dem folgenden Befehl, ob es installiert ist:

```
rpm –qa | grep gtk2
```

In den weiteren Schritten wird das SAP HANA-Setup mit der grafischen Benutzeroberfläche veranschaulicht. Im nächsten Schritt navigieren Sie zum Installationsverzeichnis und dann in das Unterverzeichnis „HDB_LCM_LINUX_X86_64“. Starten Sie

```
./hdblcmgui 
```
in diesem Verzeichnis. Sie werden nun durch eine Bildschirmsequenz geführt, in der Sie die Daten für die Installation angeben müssen. Im demonstrierten Fall installieren wir den SAP HANA-Datenbankserver und die SAP HANA-Clientkomponenten. Daher wählen wir „SAP HANA Database“ aus, wie unten gezeigt.

![Auswählen von HANA bei der Installation](./media/hana-installation/image18_hana_selection.PNG)

Wählen Sie auf dem nächsten Bildschirm die Option „Install New System“.

![Auswählen der neuen HANA-Installation](./media/hana-installation/image19_select_new.PNG)

Nach diesem Schritt müssen Sie zwischen mehreren weiteren Komponenten wählen, die zusätzlich zum SAP HANA-Datenbankserver installiert werden können.

![Auswählen zusätzlicher HANA-Komponenten](./media/hana-installation/image20_select_components.PNG)

Im Rahmen dieser Dokumentation haben wir den SAP HANA-Client und SAP HANA Studio ausgewählt. Außerdem haben wir eine Instanz für das zentrale Hochskalieren installiert. Wählen Sie im nächsten Bildschirmfenster also „Single-Host System“. 

![Auswählen der Installation für zentrales Hochskalieren](./media/hana-installation/image21_single_host.PNG)

Im nächsten Bildschirmfenster müssen Sie einige Daten angeben.

![Bereitstellen der SAP HANA-SID](./media/hana-installation/image22_provide_sid.PNG)

> [!Important]
> Als HANA-SID (System-ID) müssen Sie die gleiche SID angeben, die Sie für Microsoft bereitgestellt haben, als Sie die Bereitstellung von HANA (große Instanzen) bestellt haben. Wenn Sie eine andere SID wählen, tritt für die Installation ein Fehler auf, da sich für die verschiedenen Volumes Probleme mit der Zugriffsberechtigung ergeben.

Als Installationsverzeichnis verwenden Sie das Verzeichnis „/hana/shared“. Im nächsten Schritt geben Sie die Speicherorte für die HANA-Datendateien und die HANA-Protokolldateien an.


![Angeben des HANA-Protokollspeicherorts](./media/hana-installation/image23_provide_log.PNG)

> [!Note]
> Für die Daten- und Protokolldateien sollten Sie die Volumes angeben, die bereits Teil der Bereitstellungspunkte waren. Diese enthalten die SID, die Sie im Bildschirmfenster weiter oben ausgewählt haben. Falls die SID nicht mit der oben eingegebenen SID übereinstimmt, können Sie zurückgehen und die SID an den Wert anpassen, der in den Bereitstellungspunkten verwendet wird.

Überprüfen Sie im nächsten Schritt den Hostnamen, und korrigieren Sie ihn bei Bedarf. 

![Überprüfen des Hostnamens](./media/hana-installation/image24_review_host_name.PNG)

Im nächsten Schritt müssen Sie auch Daten abrufen, die Sie für Microsoft bereitgestellt haben, als Sie die Bereitstellung von HANA (große Instanzen) bestellt haben. 

![Angeben von UID und GID für Systembenutzer](./media/hana-installation/image25_provide_guid.PNG)

> [!Important]
> Sie müssen die gleiche Systembenutzer-ID und Benutzergruppen-ID angeben, die Sie gegenüber Microsoft beim Bestellen der Einheitenbereitstellung angegeben haben. Wenn die IDs nicht identisch sind, tritt für die Installation von SAP HANA auf der Einheit von HANA (große Instanzen) ein Fehler auf.

In den nächsten beiden Bildschirmfenstern, die wir in dieser Dokumentation nicht aufführen, müssen Sie das Kennwort für den SYSTEM-Benutzer der SAP HANA-Datenbank und das Kennwort für den Benutzer „sapadm“ angeben. Das zweite Kennwort wird für den SAP-Host-Agent verwendet, der im Rahmen der SAP HANA-Datenbankinstanz installiert wird.

Nach dem Definieren des Kennworts wird ein Bestätigungsfenster angezeigt. Überprüfen Sie alle aufgeführten Daten, und fahren Sie mit der Installation fort. Sie gelangen zu einem Statusfenster, auf dem der Installationsstatus angezeigt wird (siehe unten).

![Überprüfen des Installationsstatus](./media/hana-installation/image27_show_progress.PNG)

Nach Abschluss der Installation sollte etwa Folgendes angezeigt werden:

![Abgeschlossene Installation](./media/hana-installation/image28_install_finished.PNG)

Die SAP HANA-Instanz sollte jetzt ausgeführt werden und betriebsbereit sein. Sie sollten von SAP HANA Studio aus eine Verbindung damit herstellen können. Stellen Sie auch sicher, dass Sie eine Prüfung auf die neuesten Patches von SAP HANA durchführen und diese dann anwenden.
























































 







 





