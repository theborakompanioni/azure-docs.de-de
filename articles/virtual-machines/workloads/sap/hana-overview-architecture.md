---
title: "Übersicht und Architektur von SAP HANA in Azure (große Instanzen) | Microsoft-Dokumentation"
description: "Architekturübersicht über die Bereitstellung von SAP HANA in Azure (große Instanzen)."
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
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
ms.sourcegitcommit: 54b5b8d0040dc30651a98b3f0d02f5374bf2f873
ms.openlocfilehash: 18501fc0fdb3174b3ec74e3dde3422b27898bf10
ms.contentlocale: de-de
ms.lasthandoff: 04/28/2017


---
# <a name="sap-hana-large-instances-overview-and-architecture-on-azure"></a>Übersicht und Architektur von SAP HANA in Azure (große Instanzen) 
Dies ist ein fünfteiliges Handbuch zur Architektur und technischen Bereitstellung, das Ihnen Informationen zur Bereitstellung von SAP auf dem neuen SAP HANA in Azure (große Instanzen) bietet. Es ist nicht umfassend, und bestimmte Details im Zusammenhang mit dem Setup von SAP-Lösungen werden nicht behandelt. Stattdessen bietet es wertvolle Informationen, um Sie bei Ihrer erstmaligen Bereitstellung und bei laufenden Vorgängen zu unterstützen. Verwenden Sie es nicht als Ersatz für die SAP-Dokumentation zur Installation von SAP HANA (oder für die zahlreichen SAP-Supporthinweise, die sich mit dem Thema befassen). Darüber hinaus enthält es Details zur Installation von SAP HANA in Azure (große Instanzen).


Die fünf Teile dieses Handbuchs behandeln die folgenden Themen:

- [Übersicht und Architektur von SAP HANA in Azure (große Instanzen)](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Infrastruktur und Verbindungen mit SAP HANA in Azure (große Instanzen)](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Installieren und Konfigurieren von SAP HANA in Azure (große Instanzen)](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Hohe Verfügbarkeit und Notfallwiederherstellung für SAP HANA in Azure (große Instanzen)](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Problembehandlung und Überwachung von SAP HANA in Azure (große Instanzen)](troubleshooting-monitoring.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="definitions"></a>Definitionen

Im Handbuch zur Architektur und technischen Bereitstellung werden mehrere allgemeine Definitionen häufig verwendet. Bitte beachten Sie die folgenden Begriffe und ihre Bedeutung:

- **IaaS:** Infrastructure-as-a-Service
- **PaaS:** Platform-as-a-Service
- **SaaS:** Software-as-a-Service
- **SAP-Komponente:** Eine einzelne SAP-Anwendung wie ECC, BW, Solution Manager oder EP. SAP-Komponenten können auf herkömmlichen ABAP- oder Java-Technologien oder auf einer Nicht-NetWeaver-basierten Anwendung wie Business Objects basieren.
- **SAP-Umgebung:** Eine oder mehrere SAP-Komponenten, die logisch gruppiert sind, um eine Geschäftsfunktion wie Entwicklung, QAS, Training, DR oder Produktion auszuführen.
- **SAP-Landschaft:** Dies bezieht sich auf alle SAP-Assets in Ihrer IT-Landschaft. Die SAP-Landschaft umfasst alle Produktions- und anderen Umgebungen.
- **SAP-System:** Die Kombination aus DBMS-Schicht und Anwendungsschicht in einem SAP ERP-Entwicklungssystem, einem SAP BW-Testsystem, einem SAP CRM-Produktionssystem usw. In Azure-Bereitstellungen wird die Aufteilung dieser beiden Schichten zwischen lokalen Systemen und Azure nicht unterstützt. Das bedeutet, dass ein SAP-System entweder lokal oder in Azure bereitgestellt wird. Allerdings können Sie die verschiedenen Systeme einer SAP-Landschaft in Azure oder lokal bereitstellen. Sie könnten z.B. die SAP CRM-Entwicklungs- und Testsysteme in Azure und die SAP CRM-Produktionssysteme lokal bereitstellen. Im Fall von SAP HANA in Azure (große Instanzen) werden die Ausführung der SAP-Anwendungsschicht von SAP-Systemen in Azure-VMs und die Ausführung der HANA-Instanz auf einer Einheit im Stapel für große Instanzen unterstützt.
- **Stapel für große Instanzen:** Ein Hardwareinfrastrukturstapel, der SAP HANA TDI-zertifiziert ist und für die Ausführung von SAP HANA-Instanzen in Azure vorgesehen ist.
- **SAP HANA in Azure (große Instanzen):** Offizieller Name für das Angebot in Azure, HANA-Instanzen auf SAP HANA TDI-zertifizierter Hardware auszuführen, die in Stapeln für große Instanzen in verschiedenen Azure-Regionen bereitgestellt wird. Der zugehörige Begriff **HANA – große Instanzen** ist die Kurzform für SAP HANA in Azure (große Instanzen) und wird in diesem Handbuch für die technische Bereitstellung gemeinhin verwendet.
- **Standortübergreifend:** Beschreibt ein Szenario, in dem virtuelle Computer für ein Azure-Abonnement bereitgestellt werden, das über Site-to-Site-, Multisite- oder ExpressRoute-Konnektivität zwischen den lokalen Rechenzentren und Azure umfasst. In allgemeinen Azure-Dokumentationen werden diese Arten von Bereitstellungen auch als "Cross-Premises"-Szenarien bezeichnet. Durch die Verbindung sollen lokale Domänen, das lokale Active Directory/OpenLDAP und lokales DNS auf Azure erweitert werden. Die lokale Landschaft wird auf die Azure-Ressourcen des Abonnements erweitert. Durch diese Erweiterung können die virtuellen Computer Teil der lokalen Domäne sein. Domänenbenutzer der lokalen Domäne können auf die Server zugreifen und Dienste auf diesen virtuellen Computern ausführen (z.B. DBMS-Dienste). Die Kommunikation und Namensauflösung zwischen lokal bereitgestellten virtuellen Computern und in Azure bereitgestellten virtuellen Computern ist möglich. Dies ist das typische Szenario, in dem die meisten SAP-Assets bereitgestellt werden. Weitere Informationen finden Sie unter [Planung und Entwurf für VPN Gateway](../../../vpn-gateway/vpn-gateway-plan-design.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) und [Erstellen eines VNet mit einer Site-to-Site-Verbindung über das Azure-Portal](../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Es gibt eine Vielzahl zusätzlicher Ressourcen, die zum Thema der Bereitstellung von SAP-Workload in der öffentlichen Microsoft Azure-Cloud veröffentlicht wurden. Es wird dringend empfohlen, dass nur erfahrene Benutzer eine Bereitstellung von SAP HANA in Azure planen, die sich mit den Prinzipalen von Azure IaaS und der Bereitstellung von SAP-Workloads in Azure IaaS auskennen. Die folgenden Ressourcen bieten weitere Informationen und sollten als Referenz hinzugezogen werden, bevor Sie fortfahren:


- [Verwenden von SAP-Lösungen auf virtuellen Microsoft Azure-Computern](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="certification"></a>Zertifizierung

Neben der NetWeaver-Zertifizierung erfordert SAP eine spezielle Zertifizierung für SAP HANA, damit SAP HANA auf bestimmten Infrastrukturen wie Azure IaaS unterstützt wird.

Der wichtigste SAP-Hinweis zu NetWeaver und in gewisser Weise zur SAP HANA-Zertifizierung ist [SAP-Hinweis 1928533 – SAP Applications on Azure: Supported Products and Azure VM types](https://launchpad.support.sap.com/#/notes/1928533) (SAP-Anwendungen in Azure: Unterstützte Produkte und Azure-VM-Typen).

Auch der [SAP-Hinweis 2316233 – SAP HANA on Microsoft Azure (Large Instances)](https://launchpad.support.sap.com/#/notes/2316233/E) (SAP HANA in Microsoft Azure [Große Instanzen]) ist relevant. Darin wird die in diesem Handbuch beschriebene Lösung behandelt. Darüber hinaus werden Sie bei der Ausführung von SAP HANA auf dem Azure-VM-Typ GS5 unterstützt. [Informationen für diesen Fall sind auf der SAP-Website veröffentlicht](http://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html).

Die Lösung „SAP HANA in Azure (große Instanzen)“, auf die in SAP-Hinweis 2316233 Bezug genommen wird, verleiht Microsoft- und SAP-Kunden die Möglichkeit, große SAP Business Suite-, SAP Business Warehouse- (BW), S/4 HANA-, BW/4HANA- oder andere SAP HANA-Workloads in Azure bereitzustellen. Dies beruht auf dem SAP HANA-zertifizierten dedizierten Hardwarestapel ([SAP HANA Tailored Datacenter Integration – TDI](https://scn.sap.com/docs/DOC-63140)). Die Ausführung als SAP-TDI-konfigurierte Lösung gibt Ihnen die Gewissheit, dass alle SAP HANA-basierten Anwendungen funktionieren (einschließlich SAP Business Suite auf SAP HANA, SAP Business Warehouse (BW) auf SAP HANA, S4/HANA und BW4/HANA).

Im Vergleich zur Ausführung von SAP HANA in Azure Virtual Machines bietet diese Lösung einen Vorteil – sie ermöglicht einen wesentlich größeren Arbeitsspeicher. Wenn Sie diese Lösung aktivieren möchten, müssen Ihnen einige wichtige Aspekte klar sein:

- Die SAP-Anwendungsschicht und Nicht-SAP-Anwendungen werden auf virtuellen Azure-Computern ausgeführt, die in den üblichen Azure-Hardwarestapeln gehostet werden.
- Lokale Kundeninfrastruktur, Rechenzentren und Anwendungsbereitstellungen sind über Azure ExpressRoute (empfohlen) oder ein virtuelles privates Netzwerk (VPN) mit der Microsoft Azure-Cloudplattform verbunden. Auch Active Directory (AD) und DNS werden auf Azure erweitert.
- Die SAP HANA-Datenbankinstanz für HANA-Workload wird auf SAP HANA in Azure (große Instanzen) ausgeführt. Der Stapel für große Instanzen ist mit Azure-Netzwerken verbunden, daher kann auf Azure-VMs ausgeführte Software mit der HANA-Instanz interagieren, die in HANA (große Instanzen) ausgeführt wird.
- Hardware von SAP HANA in Azure (große Instanzen) ist dedizierte Hardware, die in einer IaaS (Infrastructure-as-a-Service) mit vorinstalliertem SUSE Linux Enterprise Server oder Red Hat Enterprise Linux bereitgestellt wird. Wie bei Azure Virtual Machines liegen weitere Updates und die Wartung für das Betriebssystem in Ihrer Verantwortung.
- Die Installation von HANA oder zusätzlichen Komponenten, die zum Ausführen von SAP HANA auf Einheiten von HANA (große Instanzen) erforderlich sind, liegt in Ihrer Verantwortung, ebenso wie alle entsprechenden laufenden Vorgänge und Administrationsaufgaben für SAP HANA in Azure.
- Zusätzlich zu den hier beschriebenen Lösungen können Sie andere Komponenten in Ihrem Azure-Abonnement installieren, das eine Verbindung mit SAP HANA in Azure (große Instanzen) herstellt.  Hierzu gehören beispielsweise Komponenten, die die Kommunikation mit und/oder direkt an die SAP HANA-Datenbank ermöglichen (Jumpserver, RDP-Server, SAP HANA Studio, SAP Data Services für SAP BI-Szenarien oder Netzwerküberwachungslösungen).
- Wie in Azure bietet HANA (große Instanzen) unterstützende Funktionen zum Ermöglichen von hoher Verfügbarkeit und Notfallwiederherstellung.

## <a name="architecture"></a>Architektur

Auf allgemeiner Ebene befindet sich die SAP-Anwendungsschicht in der Lösung „SAP HANA auf Azure (große Instanzen)“ auf virtuellen Azure-Computern, und die Datenbankschicht liegt auf mit SAP TDI konfigurierter Hardware, die sich in einem Stapel für große Instanzen in derselben Azure-Region befindet, die mit Azure IaaS verbunden ist.

> [!NOTE]
> Die SAP-Anwendungsschicht muss in derselben Azure-Region wie die SAP-DBMS-Schicht bereitgestellt werden. Dies ist in veröffentlichten Informationen zur SAP-Workload unter Azure umfassend dokumentiert.

Die Gesamtarchitektur von SAP HANA in Azure (große Instanzen) bietet eine SAP TDI-zertifizierte Hardwarekonfiguration (nicht virtualisierter Bare-Metal-Hochleistungsserver für die SAP HANA-Datenbank) sowie die Möglichkeit und Flexibilität von Azure, Ressourcen für die SAP-Anwendungsschicht nach Ihren Anforderungen zu skalieren.

![Architekturübersicht über SAP HANA in Azure (große Instanzen)](./media/hana-overview-architecture/image1-architecture.png)

Die obigen Architektur ist in drei Abschnitte unterteilt:

- **Rechts:** Eine lokale Infrastruktur, in der verschiedene Anwendungen in Rechenzentren ausgeführt werden und Endbenutzer auf branchenspezifische Anwendungen (z.B. SAP) zugreifen. Im Idealfall wird diese lokale Infrastruktur dann über Azure [ExpressRoute](https://azure.microsoft.com/services/expressroute/) mit Azure verbunden.

- **Mitte:** Zeigt Azure IaaS und, in diesem Fall, die Verwendung virtueller Azure-Computer zum Hosten von SAP oder anderen Anwendungen, die SAP HANA als DBMS-System nutzen. Kleinere HANA-Instanzen, die mit dem von Azure-VMs bereitgestellten Arbeitsspeicher arbeiten, werden zusammen mit ihrer Anwendungsschicht auf Azure-VMs bereitgestellt. Weitere Informationen zu [Virtual Machines](https://azure.microsoft.com/services/virtual-machines/).
<br />Azure-Netzwerke werden verwendet, um SAP-Systeme mit anderen Anwendungen in virtuellen Azure-Netzwerken (VNETs) zu gruppieren. Diese VNETs stellen sowohl eine Verbindung mit lokalen Systemen als auch mit SAP HANA in Azure (große Instanzen) her.
<br />Für SAP NetWeaver-Anwendungen und -Datenbanken, deren Ausführung in Microsoft Azure unterstützt wird, finden Sie Informationen in [SAP-Supporthinweis 1928533 – SAP Applications on Azure: Supported Products and Azure VM types](https://launchpad.support.sap.com/#/notes/1928533) (SAP-Anwendungen in Azure: Unterstützte Produkte und Azure-VM-Typen). Eine Dokumentation zur Bereitstellung von SAP-Lösungen unter Azure finden Sie unter:

  -  [Verwenden von SAP auf virtuellen Windows-Computern](../../virtual-machines-windows-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  -  [Verwenden von SAP-Lösungen auf virtuellen Microsoft Azure-Computern](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

- **Links:** Zeigt die SAP HANA TDI-zertifizierte Hardware im Azure-Stapel für große Instanzen. Die Einheiten für HANA (große Instanzen) sind mit den Azure-VNETs Ihres Abonnements verbunden. Dabei wird dieselbe Technologie wie für die Konnektivität zwischen dem lokalen System und Azure verwendet.

Im Azure-Stapel für große Instanzen selbst werden die folgenden Komponenten kombiniert:

- **Computing:** Server, die auf Intel Xeon E7-8890v3-Prozessoren basieren, welche die erforderliche Computingleistung bereitstellen und SAP HANA-zertifiziert sind.
- **Netzwerk:** Ein einheitliches Hochgeschwindigkeitsnetzwerk-Fabric, das die Computing-, Speicher- und LAN-Komponenten miteinander verbindet.
- **Speicher:** Eine Speicherinfrastruktur, auf die über ein einheitliches Netzwerkfabric zugegriffen wird. Je nach der bereitgestellten spezifischen Konfiguration von SAP HANA in Azure (große Instanzen) wird eine bestimmte Speicherkapazität zur Verfügung gestellt. (Weitere Speicherkapazität ist zu zusätzlichen monatlichen Kosten verfügbar.)

Innerhalb der mehrinstanzenfähigen Infrastruktur des Stapels für große Instanzen werden Kunden als isolierte Mandanten bereitgestellt. Diese Mandanten weisen eine 1:1-Beziehung mit dem Azure-Abonnement auf. Dies bedeutet, dass Sie von zwei verschiedenen Azure-Abonnements aus nicht auf dieselbe Instanz von SAP HANA in Azure (große Instanzen) zugreifen können, die in einem Azure-Stapel für große Instanzen ausgeführt wird.

Wie bei Azure-VMs wird SAP HANA in Azure (große Instanzen) in mehreren Azure-Regionen angeboten. Um Funktionen zur Notfallwiederherstellung anzubieten, können Sie diese abonnieren. Die verschiedenen Stapel für große Instanzen in den einzelnen Azure-Regionen sind miteinander verbunden.

Ebenso, wie Sie bei Azure Virtual Machines zwischen verschiedenen VM-Typen auswählen können, können Sie aus verschiedenen SKUs von HANA (große Instanzen) auswählen, die für verschiedene Workloadtypen von SAP HANA zugeschnitten sind. Für variierende Workloads wendet SAP basierend auf den Intel-Prozessorgenerationen ein geeignetes Verhältnis zwischen Arbeitsspeicher und Prozessorsockets an – es werden vier verschiedene SKU-Typen angeboten:

Ab 2016 Dezember ist SAP HANA in Azure (große Instanzen) in sechs Konfigurationen in den Azure-Regionen USA, Westen und USA, Osten verfügbar:

| SAP-Lösung | CPU | RAM | Speicher |
| --- | --- | --- | --- |
| Optimiert für OLAP: SAP BW, BW/4HANA<br /> oder SAP HANA für generische OLAP-Workload | SAP HANA in Azure S72<br /> – 2 x Intel® Xeon®-Prozessor E7-8890 v3 |  768 GB |  3 TB |
| --- | SAP HANA in Azure S144<br /> – 4 x Intel® Xeon®-Prozessor E7-8890 v3 |  1,5 TB |  6 TB |
| --- | SAP HANA in Azure S192<br /> – 4 x Intel® Xeon® Processor E7-8890 v4 |  2,0 TB |  8 TB |
| Optimiert für OLTP: SAP Business Suite<br /> auf SAP HANA oder S/4HANA (OLTP),<br /> generisches OLTP | SAP HANA in Azure S72m<br /> – 2 x Intel® Xeon®-Prozessor E7-8890 v3 |  1,5 TB |  6 TB |
|---| SAP HANA in Azure S144m<br /> – 4 x Intel® Xeon®-Prozessor E7-8890 v3 |  3,0 TB |  12 TB |
|---| SAP HANA in Azure S192m<br /> – 4 x Intel® Xeon® Processor E7-8890 v4 |  4,0 TB |  16 TB |

Die oben genannten unterschiedlichen Konfigurationen werden im [SAP-Supporthinweis 2316233 – SAP HANA on Microsoft Azure (Large Instances)](https://launchpad.support.sap.com/#/notes/2316233/E) (SAP HANA in Microsoft Azure [große Instanzen]) behandelt.

Die jeweils gewählten Konfigurationen hängen von der Workload, den CPU-Ressourcen und dem gewünschten Arbeitsspeicher ab. Für OLTP-Workload ist es möglich, die SKUs zu nutzen, die für die OLAP-Workload optimiert sind. Ebenso können Sie OLTP-SKUs für OLAP HANA-Workload nutzen. Möglicherweise müssen Sie jedoch den von HANA genutzten Arbeitsspeicher auf den Arbeitsspeicher beschränken, der für die Intel E7-Prozessorgeneration zertifiziert ist, wie [auf der SAP-Website](http://global12.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/appliances.html) für den Appliancetyp _Scale up BW on HANA_ aufgeführt.

Beachten Sie unbedingt, dass die vollständige Infrastruktur des Stapels für große Instanzen nicht ausschließlich für die Verwendung durch einen einzelnen Kunden zugeordnet wird. Dies gilt für SAP HANA in Azure (große Instanzen) ebenso wie für Regale mit Compute- und Speicherressourcen, die über ein in Azure bereitgestelltes Netzwerkfabric verbunden sind. Die Infrastruktur von HANA (große Instanzen) stellt wie Azure verschiedene &quot;Kundenmandanten&quot; bereit, die durch Netzwerkisolation voneinander getrennt sind. Daher beeinträchtigen diese Bereitstellungen von HANA (große Instanzen) sich nicht gegenseitig und sind für die jeweils anderen nicht sichtbar. Ein bereitgestellter Mandant im Stapel für große Instanzen wird einem Azure-Abonnement zugewiesen. Wenn Sie ein zweites Azure-Abonnement besitzen, für das HANA (große Instanzen) ebenfalls erforderlich ist, wird dies in einem separaten Mandanten in einem Stapel für große Instanzen mit der gesamten Netzwerkisolation bereitgestellt, welche die direkte Kommunikation zwischen diesen Instanzen verhindert.

Es gibt jedoch bedeutende Unterschiede zwischen der Ausführung von SAP HANA auf HANA (große Instanzen) und SAP HANA, das auf in Azure bereitgestellten Azure-VMs ausgeführt wird:

- Es gibt keine Virtualisierungsschicht für SAP HANA in Azure (große Instanzen). Sie erhalten die Leistung der zugrunde liegenden Bare-Metal-Hardware.
- Im Gegensatz zu Azure ist der Server für SAP HANA in Azure (große Instanzen) für einen bestimmten Kunden vorgesehen. Ein Neustart oder das Herunterfahren des Servers führt nicht dazu, dass das Betriebssystem und SAP HANA auf einem anderen Server bereitgestellt werden. (Die einzige Ausnahme hierbei ist, wenn auf einem Server möglicherweise Probleme auftreten und eine erneute Bereitstellung auf einem anderen Blade erfolgen muss.)
- Im Gegensatz zu Azure, wo die Auswahl der Hostprozessortypen nach dem besten Preis-Leistungs-Verhältnis erfolgt, werden für SAP HANA in Azure (große Instanzen) die Prozessortypen mit der höchsten Leistung der Intel E7v3-Prozessorserie ausgewählt.

Auf der Hardware von SAP HANA in Azure (große Instanzen) führen mehrere Kunden ihre Bereitstellung durch, und jeder wird durch die Bereitstellung im eigenen VLAN von den anderen abgeschirmt. Um eine Verbindung von HANA (große Instanzen) mit einem Azure Virtual Network (VNet) herzustellen, stellen die vorhandenen Netzwerkkomponenten isoliert eine Verbindung der Einheiten für HANA (große Instanzen) der Mandanten mit Azure-VNets des Azure-Abonnements der Mandanten her. 

## <a name="operations-model-and-responsibilities"></a>Funktionsprinzipien und Zuständigkeiten

Der mit SAP HANA in Azure (große Instanzen) bereitgestellte Dienst ist auf Azure-IaaS-Dienste ausgerichtet. Sie erhalten eine Instanz von HANA (große Instanzen) mit einem installiertem Betriebssystem, das für SAP HANA optimiert ist. Wie bei Azure-IaaS-VMs liegen die meisten Aufgaben zum Härten des Betriebssystems, zum Installieren zusätzlich benötigter Software, zum Installieren von HANA, zum Betreiben von Betriebssystem und HANA sowie zum Aktualisieren des Betriebssystems und HANA in Ihrer Verantwortung. Microsoft zwingt Ihnen keine Betriebssystemupdates oder HANA-Updates auf.

![Zuständigkeiten von SAP HANA in Azure (große Instanzen)](./media/hana-overview-architecture/image2-responsibilities.png)

Wie Sie oben in der Abbildung sehen können, ist SAP HANA in Azure (große Instanzen) ein mehrinstanzenfähiges IaaS-Angebot. Daher erfolgt die Trennung der Verantwortungsbereiche zum größten Teil an der Infrastrukturgrenze zum Betriebssystem. Microsoft ist für alle Aspekte des Diensts unterhalb der Linie zum Betriebssystem verantwortlich, und Sie sind oberhalb der Linie, einschließlich des Betriebssystems, zuständig. Die meisten aktuellen lokalen Methoden, die Sie für Konformität, Sicherheit, Anwendungsverwaltung, Basis und Betriebssystemverwaltung einsetzen, können somit weiterhin verwendet werden. Die Systeme werden in jeder Hinsicht so dargestellt, als befänden sie sich in Ihrem Netzwerk.

Dieser Dienst ist jedoch für SAP HANA optimiert, daher gibt es Bereiche, in denen Sie und Microsoft zusammenarbeiten müssen, um die Funktionen der zugrunde liegenden Infrastruktur zu nutzen und optimale Ergebnisse zu erzielen.

Die folgende Liste enthält weitere Details zu den einzelnen Schichten und Ihren Zuständigkeiten:

**Netzwerk:** Alle internen Netzwerke für den Stapel für große Instanzen unter SAP HANA, der Zugriff auf den Speicher, die Konnektivität zwischen den Instanzen (für horizontale Hochskalierung und andere Funktionen), die Konnektivität mit der Landschaft und die Konnektivität mit Azure, wo die SAP-Anwendungsschicht in Azure Virtual Machines gehostet wird. Darüber hinaus umfasst diese Schicht WAN-Konnektivität zwischen Azure-Rechenzentren zur Replikation für die Notfallwiederherstellung. Alle Netzwerke sind nach Mandanten partitioniert und unterliegen QoS.

**Speicher:** Der virtualisierte partitionierte Speicher für alle Volumes, die von den SAP HANA-Servern sowie für Momentaufnahmen benötigt werden.

**Server:** Die dedizierten physischen Server zum Ausführen der den Mandanten zugewiesenen SAP HANA-Datenbanken. Sie sind hardwarevirtualisiert.

**SDDC:** Die Verwaltungssoftware, die zum Verwalten von Rechenzentren als durch Software definierte Entität verwendet wird. Sie ermöglicht Microsoft das Zusammenfassen von Ressourcen in Pools aus Skalierungs-, Verfügbarkeits- und Leistungsgründen.

**Betriebssystem:** Das von Ihnen ausgewählte Betriebssystem (SUSE Linux oder Red Hat Linux), das auf den Servern ausgeführt wird. Die Betriebssystemimages, die Ihnen zur Verfügung gestellt werden, sind die Images, die Microsoft von den einzelnen Linux-Anbietern zum Zweck der Ausführung von SAP HANA bereitgestellt werden. Sie benötigen ein Abonnement des Linux-Anbieters für das jeweilige für SAP HANA optimierte Image. Zu Ihren Zuständigkeiten gehören die Registrierung der Images beim Betriebssystemanbieter. Ab dem Zeitpunkt der Übergabe durch Microsoft sind Sie auch für alle weiteren Patches des Linux-Betriebssystems verantwortlich. Dies gilt für die zusätzlichen Pakete, die ggf. für eine erfolgreiche SAP HANA-Installation erforderlich sind (siehe die Dokumentation zur Installation von SAP HANA und SAP Notes) und die ggf. nicht vom jeweiligen Linux-Anbieter seinen für SAP HANA optimierten Betriebssystemimages hinzugefügt wurden. In die Zuständigkeit des Kunden fällt auch das Patchen des Betriebssystems in Bezug auf Fehlfunktionen und Optimierung und seiner Treiber in Bezug auf bestimmte Serverhardware. Gleiches gilt für das sicherheits- oder funktionsbezogene Patchen des Betriebssystems. Darüber hinaus ist der Kunde für die Überwachung und Kapazitätsplanung der folgenden Elemente zuständig:

- CPU-Ressourcennutzung
- Arbeitsspeichernutzung
- Datenträgervolumes im Zusammenhang mit freiem Speicherplatz, IOPS und Latenz
- Netzwerkvolume-Datenverkehr zwischen HANA (große Instanzen) und SAP-Anwendungsschicht

Die zugrunde liegende Infrastruktur von HANA (große Instanzen) bietet Funktionen zur Sicherung und Wiederherstellung des Betriebssystemvolumes. Auch die Nutzung dieser Funktionalität liegt in Ihrer Verantwortung.

**Middleware:** Hauptsächlich die SAP HANA-Instanz. Verwaltung, Betrieb und Überwachung liegen in Ihrer Verantwortung. Es werden Funktionen bereitgestellt, mit denen Sie Speichermomentaufnahmen zur Sicherung/Wiedherstellung und zur Notfallwiederherstellung verwenden können. Diese Funktionen werden von der Infrastruktur bereitgestellt. Zu Ihren Zuständigkeiten gehören jedoch auch das Entwerfen einer Architektur für hohe Verfügbarkeit oder Notfallwiederherstellung mit diesen Funktionen, die Nutzung dieser Funktionen und die Überwachung der erfolgreichen Ausführung der Speichermomentaufnahmen.

**Daten:** Ihre von SAP HANA verwalteten Daten und andere Daten, z.B. Sicherungsdateien auf Volumes oder Dateifreigaben. Zu Ihren Zuständigkeiten gehören das Überwachen des freien Speicherplatzes und das Verwalten des Inhalts auf den Volumes sowie die Überwachung der erfolgreichen Ausführung von Sicherungen von Datenträgern und Speichermomentaufnahmen. Die erfolgreiche Ausführung der Datenreplikation auf DR-Standorte liegt jedoch in der Verantwortung von Microsoft.

**Anwendungen:** Die SAP-Anwendungsinstanzen oder bei Nicht-SAP-Anwendungen die Anwendungsschicht dieser Anwendungen. Zu Ihren Zuständigkeiten gehören Bereitstellung, Verwaltung, Betrieb und Überwachung dieser Anwendungen im Hinblick auf die Kapazitätsplanung der CPU-Ressourcennutzung, der Speichernutzung, der Nutzung von Azure Storage und der Nutzung der Netzwerkbandbreite innerhalb von Azure-VNETs und von Azure-VNETs auf SAP HANA in Azure (große Instanzen).

**WANs:** Die Verbindungen, die Sie von der lokalen Umgebung aus mit Azure-Bereitstellungen für Workloads herstellen. Wenn die Workloads unternehmenskritisch sind, verwenden Sie die Azure ExpressRoute. Diese Verbindung ist nicht Teil der Lösung „SAP HANA in Azure (große Instanzen)“, daher sind Sie für die Einrichtung dieser Verbindung verantwortlich.

**Archiv:** Möglicherweise möchten Sie Datenkopien mit Ihren eigenen Methoden in Speicherkonten archivieren. Dies erfordert Verwaltungsaufwand, Konformität, Kosten und Vorgänge. Sie sind für die Herstellung von Archivkopien und Sicherungen in Azure sowie für eine konforme Speicherung verantwortlich.

Informationen hierzu finden Sie in der [SLA für SAP HANA in Azure (große Instanzen)](https://azure.microsoft.com/support/legal/sla/sap-hana-large/v1_0/).

## <a name="sizing"></a>Festlegen der Größe

Die Größenanpassung für HANA (große Instanzen) unterscheidet sich nicht von der Größenanpassung für HANA im Allgemeinen. Für vorhandene und bereitgestellte Systeme, die Sie von anderen RDBMS auf HANA umstellen möchten, bietet SAP eine Reihe von Berichten, die auf Ihren vorhandenen SAP-Systemen ausgeführt werden können. Diese überprüfen die Daten und berechnen Arbeitsspeicheranforderungen für Tabellen, wenn die Datenbank nach HANA verschoben wird. Lesen Sie die folgenden SAP-Hinweise, um weitere Informationen zur Ausführung dieser Berichte und zum Abrufen der aktuellen Patches/Versionen zu erhalten:

- [SAP-Hinweis 1793345 – Größenanpassung für die SAP-Suite für HANA](https://launchpad.support.sap.com/#/notes/1793345)
- [SAP-Hinweis 1872170 – Bericht zur Größenanpassung für die Suite für HANA und S/4 HANA](https://launchpad.support.sap.com/#/notes/1872170)
- [SAP-Hinweis 2121330 – Häufig gestellte Fragen: Bericht zur Größenanpassung für SAP BW auf HANA](https://launchpad.support.sap.com/#/notes/2121330)
- [SAP-Hinweis 1736976 – Bericht zur Größenanpassung für BW auf HANA](https://launchpad.support.sap.com/#/notes/1736976)
- [SAP-Hinweis 2296290 – Neuer Bericht zur Größenanpassung für BW auf HANA](https://launchpad.support.sap.com/#/notes/2296290)

Für Greenfield-Implementierungen steht der SAP Quick Sizer zur Verfügung, um Arbeitsspeicheranforderungen für die Implementierung von SAP-Software zusätzlich zu HANA zu berechnen.

Die Arbeitsspeicheranforderungen für HANA steigen mit zunehmender Datenmenge, daher sollten Sie die jetzige Arbeitsspeichernutzung kennen und dazu in der Lage sein, die zukünftige Nutzung zu prognostizieren. Basierend auf den Speicheranforderungen können Sie Ihren Bedarf dann einer der SKUs für HANA (große Instanzen) zuordnen.

## <a name="requirements"></a>Anforderungen

Hierbei handelt es sich um die Anforderungen zum Ausführen von SAP HANA in Azure (große Instanzen).

**Microsoft Azure:**

- Ein Azure-Abonnement, das mit SAP HANA in Azure (große Instanzen) verknüpft werden kann.
- Microsoft Premier Support-Vertrag. Spezifische Informationen im Zusammenhang mit der Ausführung von SAP in Azure finden Sie im [SAP-Supporthinweis 2015553 – SAP on Microsoft Azure: Support Prerequisites](https://launchpad.support.sap.com/#/notes/2015553) (SAP in Microsoft Azure: Supportvoraussetzungen).
- Kenntnis der SKUs für HANA (große Instanzen), die Sie nach dem Ausführen einer Größenanpassung mit SAP benötigen.

**Netzwerkkonnektivität:**

- Azure ExpressRoute zwischen lokaler Umgebung und Azure: Stellen Sie sicher, dass Sie bei Ihrem Internetdienstanbieter mindestens eine 1-GBit/s-Verbindung bestellen, um Ihr lokales Rechenzentrum mit Azure zu verbinden.

**Betriebssystem:**

- Lizenzen für SUSE Linux Enterprise Server 12 für SAP-Anwendungen.

> [!NOTE] 
> Das von Microsoft bereitgestellte Betriebssystem ist nicht bei SUSE registriert und nicht mit einer SMT-Instanz verbunden.

- Bereitstellung von SUSE Linux SMT (Subscription Management Tool) in Azure auf einer Azure-VM. So kann SAP HANA in Azure (große Instanzen) registriert und von SUSE entsprechend aktualisiert werden (da es innerhalb eines Rechenzentrums von HANA (große Instanzen) keinen Internetzugang gibt). 
- Lizenzen für Red Hat Enterprise Linux 6.7 oder 7.2 für SAP HANA.

> [!NOTE]
> Das von Microsoft bereitgestellte Betriebssystem ist nicht bei Red Hat registriert und nicht mit einer Red Hat Subscription Manager-Instanz verbunden.

- Bereitstellung von Red Hat Subscription Manager in Azure auf einer Azure-VM. So kann SAP HANA in Azure (große Instanzen) registriert und von Red Hat entsprechend aktualisiert werden (da es innerhalb des Mandanten, der im Azure-Stapel für große Instanzen bereitgestellt wird, keinen direkten Internetzugang gibt).
- SAP setzt voraus, dass Sie auch einen Support-Vertrag mit Ihrem Linux-Anbieter haben. Diese Anforderung wird nicht durch die Lösung HANA (große Instanzen) oder die Tatsache aufgehoben, dass Sie Linux in Azure ausführen. Im Gegensatz zu einigen der Images im Linux Azure-Katalog ist die Servicegebühr NICHT im Lösungsangebot HANA (große Instanzen) enthalten. Es liegt an Ihnen als Kunde, die Anforderungen von SAP in Bezug auf Support-Verträge mit dem Linux-Distributor zu erfüllen.   
   - Entnehmen Sie die Voraussetzungen für SUSE Linux bitte dem Support-Vertrag in [SAP Note 1984787 – SUSE LINUX Enterprise Server 12: Installation notes](https://launchpad.support.sap.com/#/notes/1984787) (SAP-Hinweis 1984787 – SUSE LINUX Enterprise Server 12: Installationshinweise) und [SAP Note 1056161 – SUSE Priority Support for SAP applications](https://launchpad.support.sap.com/#/notes/1056161) (SAP-Hinweis 1056161 – SUSE-Prioritätssupport für SAP-Anwendungen).
   - Für Red Hat Linux benötigen Sie die richtigen Abonnementebenen, die Support und Dienst einschließen (aktualisiert auf die Betriebssysteme von HANA [große Instanzen]). Red Hat empfiehlt die Beschaffung eines „RHEL for SAP Business Applications“-Abonnements im Hinblick auf Support und Dienste. Bitte entnehmen Sie Näheres den Dokumenten [SAP Note 2002167 – Red Hat Enterprise Linux 7.x: Installation and Upgrade](https://launchpad.support.sap.com/#/notes/2002167) (SAP-Hinweis 2002167 – Red Hat Enterprise Linux 7.x: Installation und Upgrade) und [SAP Note 1496410 – Red Hat Enterprise Linux 6.x: Installation and Upgrade](https://launchpad.support.sap.com/#/notes/1496410) (SAP-Hinweis 1496410 – Red Hat Enterprise Linux 6.x: Installation und Upgrade).

**Datenbank:**

- Lizenzen und Softwareinstallationskomponenten für SAP HANA (Plattform oder Enterprise Edition).

**Anwendungen:**

- Lizenzen und Softwareinstallationskomponenten für alle SAP-Anwendungen, die eine Verbindung mit SAP HANA herstellen, sowie zugehörige SAP-Supportverträge.
- Lizenzen und Softwareinstallationskomponenten für alle Nicht-SAP-Anwendungen, die in Zusammenhang mit der SAP HANA-Umgebung in Azure (große Instanzen) verwendet werden, sowie zugehörige Supportverträge.

**Fähigkeiten:**

- Erfahrung und Kenntnisse zu IaaS und den zugehörigen Komponenten.
- Erfahrung und Kenntnisse zur Bereitstellung von SAP-Workload in Azure.
- Für die SAP HANA-Installation zertifizierte Mitarbeiter.
- Kenntnisse der SAP-Architektur zum Entwerfen von hoher Verfügbarkeit und Notfallwiederherstellung für SAP HANA.


## <a name="storage"></a>Speicher

Das Speicherlayout für SAP HANA in Azure (große Instanzen) wird von der SAP HANA in Azure-Dienstverwaltung gemäß von SAP empfohlenen bewährten Methoden konfiguriert. Siehe das Whitepaper [SAP HANA Storage Requirements](http://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html).

Bei den Einheiten von HANA (große Instanzen) ist das Speichervolume in der Regel 4-mal so groß wie der Arbeitsspeicher. Die Einheiten verfügen über ein Volume zum Speichern von HANA-Protokollsicherungen. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von SAP HANA in Azure (große Instanzen)](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Als Kunde können Sie Speichermomentaufnahmen für die Sicherung/Wiederherstellung und Notfallwiederherstellung nutzen. Weitere Informationen zu diesem Thema finden Sie unter [Hohe Verfügbarkeit und Notfallwiederherstellung für SAP HANA in Azure (große Instanzen)](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="encryption-of-data-at-rest"></a>Verschlüsselung für ruhende Daten
Der für HANA (große Instanzen) genutzte Speicher ermöglicht eine transparente Verschlüsselung der Daten, die auf den Datenträgern gespeichert werden. Sie können optional festlegen, dass diese Art der Verschlüsselung zum Zeitpunkt der Bereitstellung einer Einheit von HANA (große Instanzen) aktiviert ist. Sie können auch den unmittelbaren Wechsel zu den verschlüsselten Volumes nach der Bereitstellung auswählen. Der Schritt von den unverschlüsselten zu den verschlüsselten Volumes ist transparent und erfordert keine Ausfallzeit. 


## <a name="networking"></a>Netzwerk

Die Architektur des Azure-Netzwerks ist eine wichtige Komponente für eine erfolgreiche Bereitstellung von SAP-Anwendungen. In der Regel weisen Bereitstellungen von SAP HANA in Azure (große Instanzen) eine größere SAP-Landschaft mit mehreren verschiedenen SAP-Lösungen mit Datenbanken unterschiedlicher Größe sowie variierender CPU-Ressourcen- und Arbeitsspeichernutzung auf. Sehr wahrscheinlich basieren nur ein oder zwei solcher SAP-Systeme auf SAP HANA, daher ist Ihre SAP-Landschaft wahrscheinlich hybrid aufgebaut und nutzt folgende Komponenten:

- Lokal bereitgestellte SAP-Systeme. Aufgrund ihrer Größe können diese derzeit nicht in Azure gehostet werden. Ein klassisches Beispiel hierfür ist ein SAP ERP-Produktionssystem unter Microsoft SQL Server (wie die Datenbank) mit &gt;100 CPUs und 1 TB Arbeitsspeicher.
- Lokal bereitgestellte SAP HANA-basierte SAP-Systeme (z.B. ein SAP ERP-System, das mindestens 6 TB Arbeitsspeicher für die SAP HANA-Datenbank erfordert).
- In Azure-VMs bereitgestellte SAP-Systeme. Bei diesen Systemen kann es sich um Entwicklungs-, Test-, Sandbox- oder Produktionsinstanzen für die auf SAP NetWeaver basierenden Anwendungen handeln, die basierend auf Ressourcennutzung und Arbeitsspeicherbedarf eine erfolgreiche Bereitstellung in Azure (auf VMs) durchführen können. Diese können auch auf Datenbanken wie SQL Server (siehe [SAP-Supporthinweis 1928533 – SAP Applications on Azure: Supported Products and Azure VM types](https://launchpad.support.sap.com/#/notes/1928533/E) [SAP-Anwendungen in Azure: Unterstützte Produkte und Azure-VM-Typen]) oder SAP HANA basieren (siehe [SAP HANA-zertifizierte IaaS-Plattformen](http://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html)).
- Bereitgestellte SAP-Anwendungsserver in Azure (auf VMs), die SAP HANA in Azure (große Instanzen) in Azure-Stapeln für große Instanzen nutzen.

Auch wenn eine hybride SAP-Landschaft (mit mindestens vier unterschiedlichen Bereitstellungsszenarios) typisch ist, gibt es Instanzen einer vollständigen SAP-Landschaft in Azure, und da Microsoft Azure-VMs immer leistungsfähiger werden, wird die Anzahl von SAP-Lösungen mit Bereitstellung auf Azure-VMs wachsen.

Das Betreiben von Azure-Netzwerken im Kontext von in Azure bereitgestellten SAP-Systemen ist nicht kompliziert. Es basiert auf den folgenden Prinzipien:

- Virtuelle Azure-Netzwerke (VNETs) müssen mit der Azure-ExpressRoute-Verbindung verbunden sein, die eine Verbindung mit dem lokalen Netzwerk herstellt.
- Eine ExpressRoute-Verbindung sollte in der Regel über eine Bandbreite von 1 Gbit/s oder höher verfügen. So wird eine ausreichende Bandbreite zum Übertragen von Daten zwischen lokalen Systemen und Systemen auf virtuellen Azure-Computern gewährleistet (ebenso wie die Verbindung von lokalen Endbenutzern mit Azure-Systemen).
- Alle SAP-Systeme in Azure müssen in Azure-VNETs eingerichtet werden, um miteinander zu kommunizieren.
- Active Directory und lokal gehostetes DNS werden über ExpressRoute vom lokalen Bereich auf Azure ausgedehnt.


> [!NOTE] 
> Ein einziges Azure-Abonnement kann nur mit einem einzigen Mandanten in einem Stapel für große Instanzen in einer bestimmten Azure-Region verknüpft werden, und umgekehrt kann ein einziger Stapelmandant für große Instanzen nur mit einem einzigen Azure-Abonnement verknüpft werden.

Die Bereitstellung von SAP HANA in Azure (große Instanzen) in zwei verschiedenen Azure-Regionen führt dazu, dass ein separater Mandant im Stapel für große Instanzen bereitgestellt wird. Allerdings können Sie beide im gleichen Azure-Abonnement ausführen, solange diese Instanzen zu der gleichen SAP-Landschaft gehören. 

> [!IMPORTANT] 
> Nur die Bereitstellung der Azure-Ressourcenverwaltung wird mit SAP HANA in Azure (große Instanzen) unterstützt.

### <a name="internet-connectivity-of-hana-large-instances"></a>Internetkonnektivität von HANA (große Instanzen)
HANA (große Instanzen) besitzt KEINE direkte Internetkonnektivität. Dies schränkt z.B. Ihre Möglichkeit ein, das Betriebssystemimage direkt beim Betriebssystemanbieter zu registrieren. Daher müssen Sie möglicherweise mit dem lokalen SLES SMT-Server oder RHEL Subscription Manager arbeiten.

### <a name="data-encryption-between-azure-vms-and-hana-large-instances"></a>Verschlüsselung von Daten zwischen Azure-VMs und HANA (große Instanzen)
Zwischen HANA (große Instanzen) und virtuellen Azure-Computern übertragene Daten werden nicht verschlüsselt. Allerdings können Sie ausschließlich für den Austausch zwischen der HANA DBMS-Seite und JDBC/ODBC-basierten Anwendungen Verschlüsselung des Datenverkehrs aktivieren. Bitte lesen Sie [diese Dokumentation von SAP](http://help-legacy.sap.com/saphelp_hanaplatform/helpdata/en/db/d3d887bb571014bf05ca887f897b99/content.htm?frameset=/en/dd/a2ae94bb571014a48fc3b22f8e919e/frameset.htm&current_toc=/en/de/ec02ebbb57101483bdf3194c301d2e/plain.htm&node_id=20&show_children=false).  

### <a name="additional-azure-vnet-information"></a>Zusätzliche Azure-VNET-Informationen

Um ein Azure-VNET mit ExpressRoute zu verbinden, muss ein Azure-Gateway erstellt werden. (Informationen hierzu finden Sie unter [Informationen zu Gateways für virtuelle Netzwerke für ExpressRoute](../../../expressroute/expressroute-about-virtual-network-gateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).) Ein Azure-Gateway kann entweder mit ExpressRoute für eine Infrastruktur außerhalb von Azure (oder für einen Azure-Stapel für große Instanzen) oder für die Verbindung zwischen Azure-VNETs verwendet werden. (Informationen hierzu finden Sie unter [Konfigurieren einer VNet-zu-VNet-Verbindung für Resource Manager mithilfe von PowerShell](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).) Sie können das Azure-Gateway mit maximal vier verschiedenen ExpressRoute-Verbindungen verbinden, solange diese von unterschiedlichen MS Enterprise Edges-Instanzen (MSEE) stammen.  Weitere Informationen finden Sie unter [Infrastruktur und Verbindungen mit SAP HANA in Azure (große Instanzen)](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

> [!NOTE] 
> Der von einem Azure-Gateway bereitgestellte Durchsatz unterscheidet sich für die beiden Anwendungsfälle (siehe [Informationen zu VPN Gateway](../../../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Der maximale Durchsatz, den wir mit einem VNET-Gateway erreichen können, ist 10 Gbit/s über eine ExpressRoute-Verbindung. Beachten Sie, dass beim Kopieren von Dateien zwischen einer Azure-VM, die sich in einem Azure-VNET und einem lokalen System befindet (als einzelner Kopiedatenstrom) nicht der vollständige Durchsatz der anderen Gateway-SKUs erzielt wird. Um die vollständige Bandbreite des VNET-Gateways nutzen zu können, müssen Sie mehrere Datenströme verwenden oder verschiedene Dateien in parallelen Datenströmen einer einzelnen Datei kopieren.

Beachten Sie beim Lesen der oben genannten Artikel die Informationen zur Verfügbarkeit der UltraPerformance-Gateway-SKU in verschiedenen Azure-Regionen.

### <a name="networking-for-sap-hana-on-azure"></a>Netzwerke für SAP HANA in Azure

Für die Verbindung mit SAP HANA in Azure (große Instanzen) muss ein Azure-VNET über sein VNET-Gateway mithilfe von ExpressRoute mit dem lokalen Netzwerk eines Kunden verbunden sein. Darüber hinaus muss es über eine zweite ExpressRoute-Verbindung mit HANA (große Instanzen) verbunden sein, das sich in einem Azure-Stapel für große Instanzen befindet. Das VNET-Gateway weist mindestens zwei ExpressRoute-Verbindungen auf, und beide Verbindungen nutzen die maximale Bandbreite des VNET-Gateways gemeinsam.

> [!IMPORTANT] 
> Bei dem gesamten Netzwerkdatenverkehr zwischen der SAP-Anwendungs- und Datenbankschicht werden zum Herstellen einer Verbindung mit SAP HANA in Azure (große Instanzen) nur die HighPerformance- oder UltraPerformance-Gateway-SKUs für VNETs unterstützt.

![Mit SAP HANA in Azure (große Instanzen) und lokal verbundenes Azure-VNET](./media/hana-overview-architecture/image3-on-premises-infrastructure.png)

### <a name="single-sap-system"></a>Einzelnes SAP-System

Die oben gezeigte lokale Infrastruktur wird über ExpressRoute mit Azure verbunden, und die ExpressRoute-Verbindung stellt eine Verbindung mit einem MSEE-Router (Microsoft Enterprise Edge) her. (Informationen hierzu finden Sie unter [ExpressRoute – technische Übersicht](../../../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).) Sobald sie eingerichtet ist, stellt diese Route eine Verbindung mit dem Microsoft Azure-Backbone her, und alle Azure-Regionen sind verfügbar.

> [!NOTE] 
> Stellen Sie zum Ausführen von SAP-Landschaften in Azure eine Verbindung mit dem MSEE her, der der Azure-Region in der SAP-Landschaft am nächsten liegt. Azure-Stapel für große Instanzen werden über dedizierte MSEE-Geräte verbunden, um die Netzwerklatenz zwischen Azure-VMs in Azure IaaS und Stapeln für große Instanzen zu minimieren.

Das VNET-Gateway für die Azure-VMs, die SAP-Anwendungsinstanzen hosten, ist mit dieser ExpressRoute-Verbindung verbunden, und dasselbe VNET ist mit einem separaten MSEE-Router verbunden, der für das Verbinden mit Stapeln für große Instanzen vorgesehen ist.

Dies ist ein einfaches Beispiel für ein einzelnes SAP-System, in dem die SAP-Anwendungsschicht in Azure gehostet und die SAP HANA-Datenbank unter SAP HANA in Azure (große Instanzen) ausgeführt wird. Es wird davon ausgegangen, dass die VNET-Gatewaybandbreite von 2 Gbit/s oder 10 Gbit/s keinen Engpass darstellt.

### <a name="multiple-sap-systems-or-large-sap-systems"></a>Mehrere SAP-Systeme oder große SAP-Systeme

Wenn mehrere SAP-Systeme oder große SAP-Systeme über eine Verbindung mit SAP HANA in Azure (große Instanzen) bereitgestellt werden, ist es sinnvoll anzunehmen, dass der Durchsatz der HighPerformance-SKU des VNET-Gateways zum Engpass werden kann. Wählen Sie in diesem Fall die UltraPerformance-SKU, sofern sie verfügbar ist. Wenn nur die HighPerformance-SKU verfügbar ist (bis zu 2 Gbit/s) oder die Gefahr besteht, dass die UltraPerformance-SKU (bis zu 10 Gbit/s) nicht ausreicht, müssen Sie die Anwendungsschichten in mehrere Azure-VNETs aufteilen. Es könnte auch ratsam sein, spezielle VNets zu erstellen, die in Fällen wie den folgenden eine Verbindung mit HANA (große Instanzen) herstellen:

- Direktes Ausführen von Sicherungen von den HANA-Instanzen aus in HANA (große Instanzen) auf einem virtuellen Computer in Azure, der NFS-Freigaben hostet.
- Kopieren großer Sicherungen oder anderer Dateien aus Einheiten von HANA (große Instanzen) auf Festplattenspeicher, der in Azure verwaltet wird.

Die Verwendung separater VNets, die virtuelle Computer hosten, die den Speicher verwalten, vermeidet eine Beeinträchtigung durch umfangreiche Datei- oder Datenübertragung aus HANA (große Instanzen) in Azure über das VNet-Gateway, das die virtuellen Computer bedient, auf denen die SAP-Anwendungsschicht ausgeführt wird. 

Gehen Sie für eine stärker skalierbare Netzwerkarchitektur folgendermaßen vor:

- Nutzen Sie mehrere Azure-VNets für eine einzelne, größere SAP-Anwendungsschicht.
- Stellen Sie für jedes bereitgestellte SAP-System ein separates Azure-VNET bereit, statt diese SAP-Systeme in separaten Subnetzen unter demselben VNET zu kombinieren.

 Eine stärker skalierbare Netzwerkarchitektur für SAP HANA in Azure (große Instanzen):

![Bereitstellen einer SAP-Anwendungsschicht über mehrere Azure-VNETs](./media/hana-overview-architecture/image4-networking-architecture.png)

Durch das Bereitstellen der SAP-Anwendungsschicht oder -komponenten in mehreren Azure-VNets (wie oben gezeigt) kann unvermeidbare Latenz entstehen, die während der Kommunikation zwischen den Anwendungen auftritt, die in diesen Azure-VNets gehostet werden. Der Netzwerkdatenverkehr zwischen virtuellen Azure-Computern in verschiedenen VNets erfolgt in dieser Konfiguration standardmäßig über die MSEE-Router. Dies kann jedoch seit September 2016 vermieden und optimiert werden. Die Methode zum Optimieren und Verkürzen der Latenz bei der Kommunikation zwischen zwei VNets ist das Peering von Azure-VNets innerhalb derselben Region, und zwar auch dann, wenn diese sich in verschiedenen Abonnements befinden. Bei einem Peering von Azure-VNets kann für die Kommunikation zwischen zwei Azure-VNets der Azure-Netzwerk-Backbone für die direkte Kommunikation verwendet werden. Dies führt zu einer Latenz wie bei VMs im gleichen VNet. Hingegen wird Datenverkehr an IP-Adressbereiche, die über das Azure VNet-Gateway verbunden sind, über das individuelle VNet-Gateway des VNets geleitet. Informationen zum Peering von Azure-VNets finden Sie im Artikel [VNet-Peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).


### <a name="routing-in-azure"></a>Routing in Azure

Es gibt zwei wichtige Überlegungen zum Netzwerkrouting für SAP HANA in Azure (große Instanzen):

1. SAP HANA in Azure (große Instanzen) ist nur für Azure-VMs in der dedizierten ExpressRoute-Verbindung und nicht direkt vom lokalen System aus zugänglich. Manche Administrationsclients und Anwendungen, die einen direkten Zugriff benötigen (z.B. eine lokale Ausführung von SAP Solution Manager) können nicht auf die SAP HANA-Datenbank zugreifen.

2. Einheiten von SAP HANA (große Instanzen) in Azure haben eine zugewiesene IP-Adresse aus dem Server-IP-Pooladressbereich, den Sie als Kunde übermittelt haben (weitere Informationen finden Sie unter [Infrastruktur und Verbindungen mit SAP HANA in Azure (große Instanzen)](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).  Zugriff auf diese IP-Adresse ist über das Azure-Abonnement und ExpressRoute möglich, womit die Verbindung von Azure VNets mit HANA (große Instanzen) in Azure hergestellt wird. Die aus diesem Server-IP-Pooladressbereich zugewiesene IP-Adresse wird direkt der Hardwareeinheit zugewiesen und NICHT mehr der NAT unterzogen, wie es in den ersten Bereitstellungen dieser Lösung der Fall war. 

> [!NOTE] 
> Wenn Sie eine Verbindung mit SAP HANA in Azure (große Instanzen) in einem _Data Warehouse_-Szenario herstellen müssen, in dem Anwendungen und/oder Endbenutzer eine Verbindung mit der (direkt ausgeführten) SAP HANA-Datenbank herstellen müssen, muss eine andere Netzwerkkomponente verwendet werden: ein Reverseproxy zum Routen von Daten in beide Richtungen. Beispiel: F5 BIG-IP, NGINX mit Traffic Manager, in Azure als virtuelle Firewall-/Datenverkehrsroutinglösung bereitgestellt.

### <a name="leveraging-in-multiple-regions"></a>Nutzen in mehreren Regionen

Möglicherweise haben Sie neben der Notfallwiederherstellung auch andere Gründe für die Bereitstellung von SAP HANA in Azure (große Instanzen) in mehreren Azure-Regionen. Vielleicht möchten Sie von den einzelnen virtuellen Computern, die in den verschiedenen VNETs in den Regionen bereitgestellt werden, auf HANA (große Instanzen) zugreifen. Da die IP-Adressen der verschiedenen Einheiten von HANA (große Instanzen) nicht über die Azure-VNETs (die direkt über ihr Gateway mit den Instanzen verbunden sind) hinaus verteilt werden, gibt es eine geringfügige Änderung am oben vorgestellten VNET-Entwurf: Ein Azure-VNET-Gateway kann vier verschiedene ExpressRoute-Verbindungen aus verschiedenen MSEEs verarbeiten, und jedes VNET, das mit einem der Stapel für große Instanzen verbunden ist, kann mit dem Stapel für große Instanzen in einer anderen Azure-Region verbunden werden.

![Mit Azure-Stapeln für große Instanzen in verschiedenen Azure-Regionen verbundene Azure-VNETs](./media/hana-overview-architecture/image8-multiple-regions.png)

Die obige Abbildung zeigt, wie die verschiedenen Azure-VNETs in beiden Regionen mit zwei verschiedenen ExpressRoute-Verbindungen verbunden sind, welche zur Verbindung mit SAP HANA in Azure (große Instanzen) in beiden Azure-Regionen verwendet werden. Die neu eingeführten Verbindungen werden durch die rechteckigen roten Linien dargestellt. Mit diesen von den Azure-VNETs ausgehenden Verbindungen können die in einem dieser VNETs ausgeführten virtuellen Computer auf jede der verschiedenen HANA-Einheiten (große Instanzen) zugreifen, die in den beiden Regionen bereitgestellt werden, (vorausgesetzt, dass Sie das gleiche Abonnement verwenden). Wie Sie in der oben gezeigten Grafik sehen, wird davon ausgegangen, dass Sie zwei ExpressRoute-Verbindungen vom lokalen System zu den beiden Azure-Regionen verwenden. Dies wird aus Gründen der Notfallwiederherstellung empfohlen.

> [!IMPORTANT] 
> Wenn mehrere ExpressRoute-Verbindungen verwendet werden, sollten die Einstellungen „AS Path prepending“ und „Local Preference BGP“ verwendet werden, um ein ordnungsgemäßes Routing des Datenverkehrs sicherzustellen.



