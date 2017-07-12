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
ms.translationtype: Human Translation
ms.sourcegitcommit: a1ba750d2be1969bfcd4085a24b0469f72a357ad
ms.openlocfilehash: e66d1e245d74c11e8bf36e307009910126551b0b
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017


---
<a id="how-to-install-and-configure-sap-hana-large-instances-on-azure" class="xliff"></a>

# Installieren und Konfigurieren von SAP HANA in Azure (große Instanzen)

Die Installation von SAP HANA liegt in Ihrer Verantwortung, und Sie können damit beginnen, sobald ein neuer Server für SAP HANA in Azure (große Instanzen) übergeben und die Verbindung zwischen Ihren Azure-VNets und den SAP HANA (große Instanzen)-Einheiten hergestellt wurde. Beachten Sie, dass gemäß einer SAP-Richtlinie die Installation von SAP HANA durch eine Person durchgeführt werden muss, die dazu zertifiziert ist, also eine Person, die die Zertifizierungsprüfung „Certified SAP Technology Associate – SAP HANA Installation“ bestanden hat, oder durch einen von SAP zertifizierten Systemintegrator (SI).

Führen Sie unter [SAP Support Note #2235581 - SAP HANA: Supported Operating Systems](https://launchpad.support.sap.com/#/notes/2235581/E) (SAP-Supporthinweis #2235581 – SAP HANA: Unterstützte Betriebssysteme) erneut eine Prüfung durch, vor allem bei einer geplanten Installation von HANA 2.0, um sicherzustellen, dass das Betriebssystem von der SAP HANA-Version unterstützt wird, die Sie installiert haben. Sie werden bemerken, dass das unterstützte Betriebssystem für HANA 2.0 stärker eingeschränkt als das Betriebssystem ist, das für HANA 1.0 unterstützt wird. 

<a id="first-steps-after-receiving-the-hana-large-instance-units" class="xliff"></a>

## Erste Schritte nach dem Empfang der SAP HANA (große Instanzen)-Einheit(en)

Der **erste Schritt** nach Empfang der SAP HANA (große Instanzen)-Einheit(en) und Einrichten von Zugriff auf und Verbindung mit den Instanzen besteht darin, das Betriebssystem der Instanz bei Ihrem Betriebssystemanbieter zu registrieren. Dazu gehört das Registrieren Ihres SUSE Linux-Betriebssystems in einer Instanz von SUSE SMT, die Sie auf einem VM in Azure bereitgestellt haben müssen, mit dem die Einheit der großen SAP HANA-Instanz eine Verbindung herstellen kann. Alternativ muss Ihr RedHat-Betriebssystem bei dem Red Hat Subscription Manager registriert werden, mit dem eine Verbindung herstellen müssen. Lesen Sie auch die Anmerkungen in diesem [Dokument](https://docs.microsoft.com/azure/virtual-machines/linux/sap-hana-overview-architecture?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Dieser Schritt ist auch erforderlich, damit das Betriebssystem zukünftig gepatcht werden kann. Eine Aufgabe, die in der Verantwortung des Kunden liegt. Die Dokumentation zum Installieren und Konfigurieren von SMT für SUSE finden Sie [hier](https://www.suse.com/documentation/sles-12/book_smt/data/smt_installation.html).

Der **zweite Schritt** besteht darin, nach neuen Patches und Fixes der speziellen Betriebssystemversion zu suchen. Überprüfen Sie, ob die Patchebene der SAP HANA (große Instanz) den neuesten Stand hat. Je nach Zeitplanung der Betriebssystempatches/-versionen und Änderungen am Image, das Microsoft bereitstellen kann, gibt es möglicherweise Fälle, in denen die neuesten Patches nicht enthalten sind. Daher ist es, nachdem eine SAP HANA (große Instanzen)-Einheit übernommen und die Betriebssysteminstallation bei dem Linux-Distributor registriert wurde, zwingend erforderlich zu prüfen, ob der jeweilige Linux-Anbieter in der Zwischenzeit Patches freigegeben hat, die für die Sicherheit, Funktionalität, Verfügbarkeit und Leistung relevant sind und angewendet werden müssen.

Der **dritte Schritt** besteht darin, die zutreffenden SAP-Hinweise (SAP Notes) zum Installieren und Konfigurieren von SAP HANA unter der speziellen Betriebssystemversion zu lesen. Wegen sich ändernden Empfehlungen oder Änderungen an SAP-Hinweisen oder -Konfigurationen, die von einzelnen Installationsszenarien abhängen, ist Microsoft nicht immer in der Lage, eine HANA (große Instanzen)-Einheit perfekt zu konfigurieren. Daher ist es für Sie als Kunde unverzichtbar, die SAP-Hinweise zu lesen (mindestens die nachstehend aufgeführten), die erforderlichen Konfigurationen der Betriebssystemversion zu prüfen und die erforderlichen Konfigurationseinstellungen anzuwenden, sofern dies noch nicht geschehen ist.

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

Der **vierte Schritt** besteht darin, die Systemzeit Ihrer HANA (große Instanzen)-Einheit zu prüfen. Die Instanzen werden mit einer Systemzeitzone bereitgestellt, die dem Standort der Azure-Region entspricht, in der sich der HANA (große Instanzen)-Stempel befindet. Sie können die Systemzeit oder Zeitzone der Instanzen ändern, deren Besitzer Sie sind. Wenn Sie dies tun und weitere Instanzen in Ihrem Mandanten bestellen, müssen Sie auch die Zeitzone der neu bereitgestellten Instanzen anpassen. Microsoft-Vorgänge haben keinen Einblick in die Systemzeitzone, die Sie nach der Übergabe für die Instanzen eingerichtet haben. Daher kann es sein, dass neu bereitgestellte Instanzen nicht mit der Zeitzone eingerichtet sind, zu der Sie gewechselt haben. Folglich liegt es in Ihrer Verantwortung als Kunde, die Zeitzone der übergebenen Instanzen zu prüfen und ggf. anzupassen. 

Der **fünfte Schritt** besteht darin, „etc/hosts“ zu prüfen. Wenn die Blades übergeben werden, haben sie unterschiedliche IP-Adressen, die für unterschiedliche Zwecke zugeordnet sind (siehe nächster Abschnitt). Prüfen Sie „etc/hosts“. In Fällen, in denen Einheiten in einem vorhandenen Mandanten hinzugefügt werden, dürfen Sie nicht erwarten, dass „etc/hosts“ der neu bereitgestellten Systeme ordnungsgemäß mit den IP-Adressen von früher bereitgestellten Systemen gewartet wird. Daher müssen Sie als Kunde die richtigen Einstellungen vornehmen, damit eine neu bereitgestellte Instanz die Namen von früher bereitgestellten Einheiten in Ihrem Mandanten auflösen kann. 

<a id="networking" class="xliff"></a>

## Netzwerk
Es wird davon ausgegangen, dass Sie beim Entwerfen Ihrer Azure-VNets und Verbinden dieser VNets mit den HANA (große Instanzen)-Einheiten den Empfehlungen gefolgt sind, die in diesen Dokumenten beschrieben sind:

- [Übersicht und Architektur von SAP HANA in Azure (große Instanzen)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)
- [Infrastruktur und Verbindungen mit SAP HANA in Azure (große Instanzen)](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Es gibt einige Details, die zu dem Netzwerkbetrieb der einzelnen Einheiten bekannt sein sollten. Jede HANA (große Instanzen)-Einheit wird mit zwei oder drei IP-Adressen geliefert, die zwei oder drei Netzwerkkartenports der Einheit zugewiesen sind. Drei IP-Adressen werden in horizontal skalierten HANA-Konfigurationen und im Szenario für HANA-Systemreplikation verwendet. Eine der IP-Adressen, die der Netzwerkkarte der Einheit zugewiesen sind, stammt aus dem Server-IP-Pool, der in [Übersicht und Architektur von SAP HANA in Azure (große Instanzen)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) beschrieben ist.

Die Verteilung für Einheiten mit zwei zugewiesenen IP-Adressen sollte wie folgt aussehen:

- „eth0.xx“ muss eine zugewiesene IP-Adresse haben, die aus dem Server-IP-Pool-Adressbereich stammt, den Sie an Microsoft übermittelt haben. Diese IP-Adresse muss zur Verwaltung in „/etc/hosts“ des Betriebssystems verwendet werden.
- „eth1.xx“ muss eine zugewiesene IP-Adresse haben, die für die Kommunikation mit NFS verwendet wird. Daher muss diese Adresse **NICHT** in „etc/hosts“ verwaltet werden, um Instanz-zu-Instanz-Datenverkehr im Mandanten zu ermöglichen.

Für Bereitstellungsfälle von HANA-Systemreplikation oder horizontal skaliertem HANA ist eine Bladekonfiguration mit zwei IP-Adressen nicht geeignet. Wenn Sie nur zwei zugewiesene IP-Adressen haben, aber eine solche Konfiguration bereitstellen möchten, wenden Sie sich an Microsoft Service Management, um eine dritte IP-Adresse zu erhalten, die in einem dritten VLAN zugewiesen ist. Für HANA (große Instanzen)-Einheiten mit drei zugewiesenen IP-Adressen auf drei Netzwerkkartenports gilt Folgendes:

- „eth0.xx“ muss eine zugewiesene IP-Adresse haben, die aus dem Server-IP-Pool-Adressbereich stammt, den Sie an Microsoft übermittelt haben. Daher darf diese IP-Adresse nicht zur Verwaltung in „/etc/hosts“ des Betriebssystems verwendet werden.
- „eth1.xx“ muss eine zugewiesene IP-Adresse haben, die für die Kommunikation mit NFS-Speicher verwendet wird. Daher darf dieser Typ von Adressen nicht in „etc/hosts“ verwaltet werden.
- „eth2.xx“ muss ausschließlich verwendet werden, um in „etc/hosts“ für die Kommunikation zwischen den verschiedenen Instanzen verwaltet zu werden. Diese Adressen wären auch die IP-Adressen, die in horizontal skalierten HANA-Konfigurationen als IP-Adressen verwaltet werden müssen, die HANA für die Konfiguration zwischen Knoten verwendet.



<a id="storage" class="xliff"></a>

## Speicher

Das Speicherlayout für SAP HANA in Azure (große Instanzen) wird von der SAP HANA in Azure-Dienstverwaltung gemäß von SAP empfohlenen bewährten Methoden konfiguriert. Siehe das Whitepaper [SAP HANA Storage Requirements](http://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html). Während Sie das Dokument lesen und sich eine HANA (große Instanzen)-Einheit ansehen, werden Sie feststellen, dass die Einheiten mit ziemlich großzügigem Datenträgervolume für „HANA/data“ bereitgestellt werden und dass es das Volume „HANA/log/backup“ gibt. Der Grund, warum „HANA/data“ so großzügig dimensioniert ist, liegt darin, dass für die Speichermomentaufnahmen, die Ihnen als Kunde angeboten werden, genau dasselbe Datenträgervolume verwendet wird. Das bedeutet, je mehr Storagemomentaufnahmen Sie ausführen, desto mehr Speicherplatz ist für diese erforderlich. Das Volume „HANA/log/backup“ ist nicht als das Volume vorgesehen, in dem Datenbanksicherungen abgelegt werden, sondern soll als Sicherungsvolume für das HANA-Transaktionsprotokoll genutzt werden. Für zukünftige Versionen des Speichermomentaufnahmen-Self-Service ist für dieses spezielle Volume geplant, häufigere Momentaufnahmen und damit häufigere Replikationen zum Notfallwiederherstellungsstandort zu haben, wenn Sie die Notfallwiederherstellungsfunktionalität nutzen möchten, die von der HANA (große Instanzen)-Infrastruktur bereitgestellt wird. Ausführlichere Informationen finden Sie unter [Hohe Verfügbarkeit und Notfallwiederherstellung für SAP HANA in Azure (große Instanzen)](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Zusätzlich zu dem bereitgestellten Speicher können Sie weitere Speicherkapazität in Schritten von 1 TB erwerben. Dieser zusätzliche Speicher kann HANA (große Instanzen) als neue Volumes hinzugefügt werden.

Während des Onboardings mit Microsoft Service Management gibt der Kunde eine Benutzer-ID (UID) und Gruppen-ID (GID) für den <SID>-ADM-Benutzer und die „sapsys“-Gruppe an (Beispiel: 1000,500). Es ist erforderlich, dass während der Installation des SAP HANA-Systems genau diese Werte verwendet werden.

Der Speichercontroller und die Knoten in den „Große Instanz“-Stapeln werden mit NTP-Servern synchronisiert. Beim Synchronisieren der SAP HANA in Azure (große Instanzen)-Einheiten und Azure-VMs mit einem NTP-Server darf es zwischen der Infrastruktur und den Compute-Einheiten in Azure- oder „Große Instanz“-Stapeln keine signifikante Zeitabweichung geben.

Um SAP HANA für den darunter verwendeten Speicher zu optimieren, sollten Sie auch die folgenden SAP HANA-Konfigurationsparameter festlegen:

- max_parallel_io_requests 128
- async_read_submit on
- async_write_submit_active on
- async_write_submit_blocks all
 
Für SAP HANA 1.0-Versionen bis SPS12 können diese Parameter während der Installation der SAP HANA-Datenbank festgelegt werden, wie dies in [SAP Note #2267798 - Configuration of the SAP HANA Database](https://launchpad.support.sap.com/#/notes/2267798) beschrieben ist.

Sie können die Parameter auch nach der Installation der SAP HANA-Datenbank konfigurieren, indem Sie das hdbparam-Framework verwenden. 

Seit SAP HANA Version 2.0 ist das hdbparam-Framework veraltet. Daher müssen die Parameter nun über SQL-Befehle festgelegt werden. Ausführliche Informationen finden Sie in [SAP Note #2399079: Elimination of hdbparam in HANA 2](https://launchpad.support.sap.com/#/notes/2399079).


<a id="operating-system" class="xliff"></a>

## Betriebssystem

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

<a id="time-synchronization" class="xliff"></a>

## Zeitsynchronisierung

SAP reagiert sehr empfindlich auf Zeitunterschiede bei den verschiedenen Komponenten, die das SAP-System bilden. SAP ABAP-Kurzdumps mit dem Fehlertitel ZDATE\_LARGE\_TIME\_DIFF sind Ihnen wahrscheinlich vertraut, wenn Sie bereits längere Zeit mit SAP Basis arbeiten. Diese Kurzdumps werden nämlich angezeigt, wenn die Systemzeit auf den verschiedenen Servern bzw. VMs zu stark voneinander abweicht.

Bei SAP HANA in Azure (große Instanzen) gilt die in Azure erfolgende Zeitsynchronisierung nicht für die Compute-Einheiten in „Große Instanz“-Stapeln. Dies gilt nicht für die native Ausführung von SAP-Anwendungen in Azure (auf VMs), da Azure sichergestellt, dass die Uhrzeit eines Systems ordnungsgemäß synchronisiert wird. Deshalb muss ein getrennter Zeitserver eingerichtet werden, der von auf Azure-VMs ausgeführten SAP-Anwendungsservern und den SAP HANA-Datenbankinstanzen verwendet werden kann, die in HANA (große Instanzen) ausgeführt werden. Der Speicherinfrastruktur in „Große Instanz“-Stapeln wird mit NTP-Servern zeitlich synchronisiert.





