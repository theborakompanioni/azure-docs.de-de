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
translationtype: Human Translation
ms.sourcegitcommit: 7f469fb309f92b86dbf289d3a0462ba9042af48a
ms.openlocfilehash: 61d191b5b2cc18731caacb6d192b851b6b49b22c
ms.lasthandoff: 04/13/2017


---
# <a name="how-to-install-and-configure-sap-hana-large-instances-on-azure"></a>Installieren und Konfigurieren von SAP HANA in Azure (große Instanzen)

Die Installation von SAP HANA liegt in Ihrer Verantwortung und kann unmittelbar nach der Übergabe eines Servers für SAP HANA in Azure (große Instanzen) erfolgen. Beachten Sie, dass gemäß einer SAP-Richtlinie die Installation von SAP HANA durch einen zertifizierten SAP HANA-Installationsbeauftragen durchgeführt werden muss, also durch eine Person, die die Zertifizierungsprüfung „Certified SAP Technology Associate – SAP HANA Installation“ bestanden hat, oder durch einen von SAP zertifizierten Systemintegrator (SI).

Es gibt bestimmte Konnektivitätsaspekte im Zusammenhang mit SAP HANA (serverseitig) und SAP HANA (clientseitig), die berücksichtigt werden müssen. In vielen Fällen sendet der SAP HANA-Server seine IP-Adresse an den Client, auf dem diese zwischengespeichert und für nachfolgende Verbindungsversuche verwendet wird. Da SAP HANA in Azure (große Instanzen) die interne Server-IP-Adresse, die im Mandantennetzwerk verwendet wird, per NAT in einen IP-Adressbereich übersetzt, der für angegebenen Azure VNets bereitgestellt wird, würde der SAP HANA-Datenbankserver absichtlich den &quot;internen&quot; IP-Adressbereich senden. Für die Hostnamenauflösung wird beispielsweise die zwischengespeicherte IP-Adresse verwendet, anstatt dass SAP HANA die per NAT übersetzte IP-Adresse bereitstellt. Deshalb kann eine Anwendung, die einen SAP HANA-Client (ODBC, JDBC usw.) verwendet, über diese IP-Adresse keine Verbindung herstellen. Um dem SAP HANA-Server anzuweisen, die per NAT übersetzte IP-Adresse an den Client weiterzugeben, muss die globale Systemkonfigurationsdatei (global.ini) von SAP HANA bearbeitet werden.

Fügen Sie Folgendes der Datei „global.ini“ hinzu (entweder direkt oder über SAP HANA Studio):
```
[public\_hostname\_resolution]
use\_default\_route = no
map\_<hostname of SAP HANA on Azure (Large Instances) tenant> = <NAT IP Address (IP address that can be accessed in Azure)>
```
Im Abschnitt _Mapping Host Names for Database Client Access_ im [SAP HANA Administration Guide](http://help.sap.com/hana/sap_hana_administration_guide_en.pdf) finden Sie weitere Details und Beispiele.

Bearbeiten Sie auf der Clientseite (für SAP-Anwendungsserver in Azure) die Datei „/etc/hosts“ (unter Linux) oder „/system32/drivers/etc/hosts“ (unter Windows Server), und fügen Sie einen Eintrag für den Hostnamen des SAP HANA in Azure (große Instanzen)-Mandanten und seine zugehörige per NAT übersetzte IP-Adresse hinzu.

>[!NOTE] 
>Wenn während der Installation des SAP HANA-Datenbankclients beim Herstellen der Verbindung mit SAP HANA in Azure (große Instanzen) Fehler auftreten, verwenden Sie implizit die per NAT übersetzte IP-Adresse des HANA (große Instanzen)-Mandanten anstelle des Hostnamens.

## <a name="storage"></a>Speicher

Das Speicherlayout für SAP HANA in Azure (große Instanzen) wird von der SAP HANA in Azure-Dienstverwaltung gemäß von SAP empfohlenen bewährten Methoden konfiguriert. Siehe das Whitepaper [SAP HANA Storage Requirements](http://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html).

Die genauen Konfigurationen des Speichers für unterschiedliche HANA (große Instanzen)-Modelle sind wie folgt:

| Arbeitsspeichergröße | HANA/data | HANA/log | HANA/shared | HANA/log/backups |
| --- | --- | --- | --- | --- |
| 768 GB | 1.280 GB | 512 GB | 768 GB | 512 GB |
| 1.536 GB | 3.456 GB | 768 GB | 1024 GB | 768 GB |
| 3.072 GB | 7.680 GB | 1.536 GB | 1024 GB | 1.536 GB |

Darüber hinaus können Kunden in Schritten von 1 TB zusätzliche Speicherkapazität erwerben. Dieser zusätzliche Speicher kann HANA (große Instanzen) als neue Volumes hinzugefügt werden.

Der Speichercontroller und die Knoten in den „Große Instanz“-Stapeln werden mit NTP-Servern synchronisiert. Beim Synchronisieren der SAP HANA in Azure (große Instanzen)-Einheiten und Azure-VMs mit einem NTP-Server darf es zwischen der Infrastruktur und den Compute-Einheiten in Azure- oder „Große Instanz“-Stapeln keine signifikante Zeitabweichung geben.

## <a name="operating-system"></a>Betriebssystem

[SUSE Linux Enterprise Server 12 SP1 for SAP Applications](https://www.suse.com/products/sles-for-sap/hana) ist die Distribution von Linux, die für SAP HANA in Azure (große Instanzen) installiert wird. Diese besondere Distribution bietet &quot;standardmäßig&quot; SAP-spezifische Funktionen (einschließlich vorab festgelegter Parameter für die effektive Ausführung von SAP unter SLES).

Auf der SUSE-Website unter [Ressourcen Library/White Papers](https://www.suse.com/products/sles-for-sap/resource-library#white-papers) und unter [SAP on SUSE](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+SUSE) im SAP Community Network (SCN) finden Sie verschiedene nützliche Ressourcen zur Bereitstellung von SAP HANA unter SLES (einschließlich u.a. zur Einrichtung für hohe Verfügbarkeit und Sicherheitshärtung für den SAP-Betrieb).

Zusätzliche und nützliche auf SLES bezogene Links:

- [Website zu SAP HANA unter SUSE Linux](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+SUSE)
- [Best Practice for SAP: Enqueue Replication – SAP NetWeaver on SUSE Linux Enterprise 12](https://www.suse.com/docrepcontent/container.jsp?containerId=9113)
- [ClamSAP – SLES Virus Protection for SAP](http://scn.sap.com/community/linux/blog/2014/04/14/clamsap--suse-linux-enterprise-server-integrates-virus-protection-for-sap) (einschließlich SLES 12 für SAP-Anwendungen)

Anmerkungen des SAP-Supports zur Implementierung von SAP HANA unter SLES 12 SP1:

- [SAP Support Note #1944799 – SAP HANA Guidelines for SLES Operating System Installation](http://go.sap.com/documents/2016/05/e8705aae-717c-0010-82c7-eda71af511fa.html)
- [SAP Support Note #2205917 – SAP HANA DB Recommended OS Settings for SLES 12 for SAP Applications](https://launchpad.support.sap.com/#/notes/2205917/E)
- [SAP Support Note #1984787 – SUSE Linux Enterprise Server 12: Installation Notes](https://launchpad.support.sap.com/#/notes/1984787)
- [SAP Support Note #171356 – SAP Software on Linux: General Information](https://launchpad.support.sap.com/#/notes/1984787)
- [SAP Support Note #1391070 – Linux UUID Solutions](https://launchpad.support.sap.com/#/notes/1391070)

## <a name="time-synchronization"></a>Zeitsynchronisierung

SAP reagiert sehr empfindlich auf Zeitunterschiede bei den verschiedenen Komponenten, die das SAP-System bilden. SAP ABAP-Kurzdumps mit dem Fehlertitel ZDATE\_LARGE\_TIME\_DIFF sind Ihnen wahrscheinlich vertraut, wenn Sie bereits längere Zeit mit SAP Basis arbeiten. Diese Kurzdumps werden nämlich angezeigt, wenn die Systemzeit auf den verschiedenen Servern bzw. VMs zu stark voneinander abweicht.

Bei SAP HANA in Azure (große Instanzen) gilt die in Azure erfolgende Zeitsynchronisierung nicht für die Compute-Einheiten in „Große Instanz“-Stapeln. Dies gilt nicht für die native Ausführung von SAP-Anwendungen in Azure (auf VMs), da Azure sichergestellt, dass die Uhrzeit eines Systems ordnungsgemäß synchronisiert wird. Deshalb muss ein getrennter Zeitserver eingerichtet werden, der von auf Azure-VMs ausgeführten SAP-Anwendungsservern und den SAP HANA-Datenbankinstanzen verwendet werden kann, die in HANA (große Instanzen) ausgeführt werden. Der Speicherinfrastruktur in „Große Instanz“-Stapeln wird mit NTP-Servern zeitlich synchronisiert.



