---
title: "Hohe Verfügbarkeit und Notfallwiederherstellung für SAP HANA in Azure (große Instanzen) | Microsoft-Dokumentation"
description: "Sorgen für Sie für hohe Verfügbarkeit, und planen Sie die Notfallwiederherstellung für SAP HANA in Azure (große Instanzen)."
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
ms.translationtype: HT
ms.sourcegitcommit: 48dfc0fa4c9ad28c4c64c96ae2fc8a16cd63865c
ms.openlocfilehash: 4356e35609fa7a9727d8ad4f20fd18df01a05e84
ms.contentlocale: de-de
ms.lasthandoff: 08/30/2017

---
# <a name="sap-hana-large-instances-high-availability-and-disaster-recovery-on-azure"></a>Hohe Verfügbarkeit und Notfallwiederherstellung für SAP HANA in Azure (große Instanzen) 

Hohe Verfügbarkeit und Notfallwiederherstellung sind wichtige Aspekte bei der Ausführung Ihrer unternehmenskritischen Server mit SAP HANA in Azure (große Instanzen). Es ist wichtig, dass Sie mit SAP, Ihrem Systemintegrator oder Microsoft zusammenarbeiten, um die richtige Strategie für hohe Verfügbarkeit bzw. die Notfallwiederherstellung zu implementieren. Außerdem ist es wichtig, die Werte für RPO (Recovery Point Objective) und die RTO (Recovery Time Objective) zu berücksichtigen, die für Ihre Umgebung gelten.

Microsoft unterstützt mit HANA (große Instanzen) standardmäßig einige SAP HANA-Methoden für hohe Verfügbarkeit. Diese umfassen:

- **Speicherreplikation:** Die Fähigkeit des Speichersystems, alle Daten an einen anderen Stapel für HANA (große Instanzen) in einer anderen Azure-Region zu replizieren. SAP HANA wird unabhängig von dieser Methode betrieben.
- **HANA-Systemreplikation:** Die Replikation aller Daten in SAP HANA in ein anderes SAP HANA-System. RTO (Recovery Time Objective) wird durch die Datenreplikation in regelmäßigen Abständen verringert. SAP HANA unterstützt asynchrone, synchrone In-Memory- und synchrone Modi (nur empfohlen für SAP HANA-Systeme innerhalb des gleichen Datencenters oder mit einer Entfernung von weniger als 100 km zueinander). Beim aktuellen Entwurf mit HANA-„Stapeln“ mit großen Instanzen kann die HANA-Systemreplikation nur für hohe Verfügbarkeit verwendet werden. Aufgrund des aktuellen Netzwerkdesigns kann die HANA-Systemreplikation nicht ohne Drittanbieter-Reverseproxykomponente für Notfallwiederherstellungskonfigurationen mit einer anderen Azure-Region als Ziel verwendet werden. 
- **Automatisches Hostfailover:** Eine lokale Wiederherstellungslösung für SAP HANA, die alternativ zur HANA-Systemreplikation verwendet werden kann. Wenn der Masterknoten nicht mehr verfügbar ist, wird mindestens ein SAP HANA-Standbyknoten im Modus „Horizontal hochskalieren“ konfiguriert, und SAP HANA führt automatisch ein Failover auf einen Standbyknoten durch.

SAP HANA in Azure (große Instanzen) wird in zwei Azure-Regionen in mittlerweile drei verschiedenen geopolitischen Regionen (USA, Australien und Europa) angeboten. Zwei unterschiedliche Regionen, die Stapel für HANA (große Instanzen) hosten, sind mit separaten dedizierten Netzwerkverbindungen verbunden, die zum Replizieren von Speichermomentaufnahmen verwendet werden, um Notfallwiederherstellungsmethoden bereitzustellen. Die Replikation ist standardmäßig nicht eingerichtet. Sie wird für Kunden eingerichtet, die die Notfallwiederherstellungsfunktion angefordert haben. Die Speicherreplikation setzt die Verwendung von Speichermomentaufnahmen für HANA (große Instanzen) voraus. Darüber hinaus ist es nicht möglich, eine Azure-Region in einer anderen geopolitischen Region als Notfallwiederherstellungsregion auszuwählen. 

Die folgende Tabelle gibt Aufschluss über die derzeit unterstützten Hochverfügbarkeits- und Notfallwiederherstellungsmethoden sowie über mögliche Kombinationen:

| In HANA (große Instanzen) unterstütztes Szenario | Hohe Verfügbarkeit | Notfallwiederherstellung | Kommentare |
| --- | --- | --- | --- |
| Einzelner Knoten | Nicht verfügbar. | Dedizierte Notfallwiederherstellungseinrichtung<br /> Mehrzweck-Notfallwiederherstellungseinrichtung | |
| Automatisches Hostfailover: N + M<br /> einschließlich 1 + 1 | Möglich durch Übernahme der aktiven Rolle durch Standbyknoten<br /> Steuerung des Rollenwechsels durch HANA | Dedizierte Notfallwiederherstellungseinrichtung<br /> Mehrzweck-Notfallwiederherstellungseinrichtung<br /> Synchronisierung der Notfallwiederherstellung mithilfe der Speicherreplikation | An alle Knoten (N + M) angefügte HANA-Volumesätze<br /> Standort für die Notfallwiederherstellung muss über gleiche Anzahl von Knoten verfügen |
| HANA-Systemreplikation | Möglich mit Primär/Sekundär-Einrichtung<br /> Bei einem Failover übernimmt das sekundäre Replikat die Rolle des primären Replikats.<br /> Failoversteuerung durch HANA-Systemreplikation und Betriebssystem | Dedizierte Notfallwiederherstellungseinrichtung<br /> Mehrzweck-Notfallwiederherstellungseinrichtung<br /> Synchronisierung der Notfallwiederherstellung mithilfe der Speicherreplikation<br /> Die Notfallwiederherstellung mithilfe der HANA-Systemreplikation ist noch nicht ohne Drittanbieterkomponenten möglich. | Separater Satz angefügter Datenträgervolumes für die einzelnen Knoten<br /> Nur Datenträgervolumes des sekundären Replikats am Produktionsstandort werden am Standort für die Notfallwiederherstellung repliziert.<br /> Am Standort für die Notfallwiederherstellung wird ein einzelner Volumesatz benötigt. | 

Als dedizierte Notfallwiederherstellungseinrichtung bezeichnen wir eine Einrichtung, bei der die große HANA-Instanzeinheit am Standort für die Notfallwiederherstellung nicht zum Ausführen einer anderen Workload oder eines produktionsfremden Systems verwendet wird. Die Einheit ist passiv und wird nur bereitgestellt, um im Falle eines Notfallfailovers einspringen zu können. Bislang haben wir noch keinen Kunden mit einer solchen Konfiguration.

Als Mehrzweck-Notfallwiederherstellungseinrichtung bezeichnen wir eine Einrichtung am Standort für die Notfallwiederherstellung, bei der die große HANA-Instanzeinheit eine produktionsfremde Workload ausführt. Bei einem Notfall wird das produktionsfremde System heruntergefahren, die speicherreplizierten (zusätzlichen) Volumesätze werden eingebunden, und die HANA Produktionsinstanz wird gestartet. Bislang kommt bei allen Kunden, die die Notfallwiederherstellung von HANA (große Instanzen) nutzen, diese Konfigurationsalternative zum Einsatz. 


Weitere Informationen zur hohen Verfügbarkeit von SAP HANA finden Sie in den folgenden SAP-Artikeln: 

- [Whitepaper zur hohen Verfügbarkeit von SAP HANA](http://go.sap.com/documents/2016/05/f8e5eeba-737c-0010-82c7-eda71af511fa.html)
- [SAP HANA-Administratorhandbuch](http://help.sap.com/hana/SAP_HANA_Administration_Guide_en.pdf)
- [SAP Academy-Video zur SAP HANA-Systemreplikation](http://scn.sap.com/community/hana-in-memory/blog/2015/05/19/sap-hana-system-replication)
- [SAP Support Note #1999880 – FAQ on SAP HANA System Replication](https://bcs.wdf.sap.corp/sap/support/notes/1999880)
- [SAP Support Note #2165547 – SAP HANA Backup and Restore within SAP HANA System Replication Environment](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3231363535343726)
- [SAP Support Note #1984882 – Using SAP HANA System Replication for Hardware Exchange with Minimum/Zero Downtime](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3139383438383226)

## <a name="network-considerations-for-disaster-recovery-with-hana-large-instances"></a>Netzwerküberlegungen für die Notfallwiederherstellung mit HANA (große Instanzen)

Um die Notfallwiederherstellung von HANA (große Instanzen) nutzen zu können, müssen Sie die Netzwerkverbindung zwischen den beiden Azure-Regionen einrichten. Hierzu benötigen Sie eine Azure ExpressRoute-Verbindung von Ihrem lokalen Standort in Ihrer Azure-Hauptregion und eine andere Verbindung von Ihrem lokalen Standort mit der Region für die Notfallwiederherstellung. Diese Maßnahme eignet sich für Situationen, in denen ein Problem für eine vollständige Azure-Region samt MSEE-Standort (Microsoft Enterprise-Edgerouter) auftritt.

Als zweite Maßnahme können Sie alle Azure VNets, die in einer der Regionen mit SAP HANA in Azure (große Instanzen) verbunden sind, mit der ExpressRoute-Verbindung verbinden, die große HANA-Instanzen in der anderen Region verbindet. Mit dieser Querverbindung können Dienste, die in einem Azure VNet in Region 1 ausgeführt werden, eine Verbindung mit Einheiten von HANA (große Instanzen) in Region 2 herstellen (und umgekehrt). Die ist eine Maßnahme für den Fall, in dem nur einer der MSEE-Standorte, der mit Ihrem lokalen Standort über Azure verbunden ist, ausfällt.

Die folgende Grafik veranschaulicht eine solche robuste Konfiguration für die Notfallwiederherstellung:

![Optimale Konfiguration für die Notfallwiederherstellung](./media/hana-overview-high-availability-disaster-recovery/image1-optimal-configuration.png)



## <a name="other-requirements-for-using-hana-large-instances-storage-replication-for-disaster-recovery"></a>Weitere Anforderungen für die Verwendung der Speicherreplikation von HANA (große Instanzen) für die Notfallwiederherstellung

Für eine Notfallwiederherstellungseinrichtung mit HANA (große Instanzen) müssen zusätzlich folgende Anforderungen erfüllt sein:

- Es ist erforderlich, SKUs vom Typ „SAP HANA für Azure (große Instanzen)“ mit derselben Größe wie die Produktions-SKUs anzufordern und in der Region für die Notfallwiederherstellung bereitzustellen. In den bisherigen Kundenbereitstellungen werden diese Instanzen zum Ausführen produktionsfremder HANA-Instanzen verwendet. Wir bezeichnen sie als Mehrzweck-Notfallwiederherstellungseinrichtungen.   
- Für jede SKU vom Typ „SAP HANA in Azure (große Instanzen)“, die Sie am Standort für die Notfallwiederherstellung wiederherstellen möchten, muss bei Bedarf zusätzlicher Speicher am Standort für die Notfallwiederherstellung angefordert werden. Dies hat die Bereitstellung von Speichervolumes zur Folge, die als Ziel für die Speicherreplikation aus Ihrer Azure-Produktionsregion in der Azure-Notfallwiederherstellungsregion fungieren.

Ausführlichere Informationen zur Notfallwiederherstellung finden Sie in den letzten Kapiteln des Dokuments.
 

## <a name="backup-and-restore"></a>Sichern und Wiederherstellen

Einer der wichtigsten Aspekte beim Betreiben von Datenbanken ist das Sicherstellen, dass die Datenbank vor verschiedenen Katastrophenfällen geschützt ist. Diese Ereignisse können sowohl durch Naturkatastrophen als auch einfache Benutzerfehler verursacht werden.

Das Sichern einer Datenbank mit der Möglichkeit einer beliebigen Zeitpunktwiederherstellung (z.B. vor dem Zeitpunkt des Löschens kritischer Daten) lässt das Wiederherstellen eines Status zu, der möglichst genau dem Status vor der Störung entspricht.

Für optimale Ergebnisse müssen zwei Arten von Sicherungen erfolgen:

- Datenbanksicherungen (vollständig, inkrementell oder differenziell)
- Transaktionsprotokollsicherungen

Zusätzlich zu vollständigen Datenbanksicherungen auf Anwendungsebene können Sie Sicherungen mit Speichermomentaufnahmen erstellen. Speichermomentaufnahmen sind jedoch kein Ersatz für Transaktionsprotokollsicherungen. Transaktionsprotokollsicherungen werden auch weiterhin für Point-in-Time-Datenbankwiederherstellungen sowie zum Leeren der Protokolle bereits committeter Transaktionen benötigt. Speichermomentaufnahmen können allerdings die Wiederherstellung durch schnelles Bereitstellen eines Rollforward-Images der Datenbank beschleunigen. 

Es gibt zwei Sicherungs- und Wiederherstellungsoptionen für SAP HANA in Azure (große Instanzen):

- Eigenständig. Berechnen Sie zunächst, ob genügend Speicherplatz auf dem Datenträger zur Verfügung steht. Führen Sie dann mithilfe von Datenträger-Sicherungsmethoden vollständige Datenbank- und Protokollsicherungen durch – entweder direkt auf Volumes, die an die Einheiten von HANA (große Instanzen) angefügt sind, oder auf NFS-Freigaben, die auf einem virtuellen Azure-Computer eingerichtet sind. In letzterem Fall richten Kunden einen virtuellen Linux-Computer in Azure ein, fügen Azure Storage an den virtuellen Computer an und geben den Speicher über einen konfigurierten NFS-Server auf diesem virtuellen Computer frei. Wenn die Sicherung mit Volumes durchgeführt wird, die direkt an Einheiten von HANA (große Instanzen) angefügt sind, müssen die Sicherungen (nach Einrichtung eines virtuellen Azure-Computers, der NFS-Freigaben auf der Grundlage von Azure Storage exportiert) in ein Azure Storage-Konto kopiert werden, oder es muss ein Azure Backup-Tresor oder ein Azure Cold Storage verwendet werden. Alternativ können die Sicherungen nach dem Kopieren in ein Azure Storage-Konto mithilfe eines Drittanbieter-Datenschutztools gespeichert werden. Die eigenständige Sicherungsoption kann auch bei Daten erforderlich sein, die zu Compliance- oder Auditzwecken über einen längeren Zeitraum gespeichert werden müssen. In allen Fällen werden die Sicherungen in NFS-Freigaben kopiert, die durch einen virtuellen Computer und Azure Storage dargestellt werden.
- Verwenden Sie die Sicherungs- und Wiederherstellungsfunktionalität, die die zugrunde liegende Infrastruktur von SAP HANA in Azure (große Instanzen) bereitstellt. Diese Option sorgt für die nötigen Sicherungen und ermöglicht schnelle Wiederherstellungen. Im weiteren Verlauf dieses Abschnitts werden die Sicherungs- und Wiederherstellungsfunktionen behandelt, die durch HANA (große Instanzen) bereitgestellt werden. Des Weiteren wird die Beziehung zur Notfallwiederherstellungsfunktion von HANA (große Instanzen) erläutert.

> [!NOTE]
> Die Momentaufnahmetechnologie, die von der zugrunde liegenden Infrastruktur von HANA (große Instanzen) verwendet wird, ist von SAP HANA-Momentaufnahmen abhängig. SAP HANA-Momentaufnahmen können bislang nicht in Verbindung mit mehreren Mandanten von SAP HANA-Datenbankcontainern mit mehreren Mandanten verwendet werden. Daher kann diese Sicherungsmethode nicht verwendet werden, wenn Sie mehrere Mandanten in SAP HANA-Datenbankcontainern mit mehreren Mandanten bereitgestellt haben. Falls Sie hingegen lediglich einen einzelnen Mandanten bereitgestellt haben, kann die Methode verwendet werden.

### <a name="using-storage-snapshots-of-sap-hana-on-azure-large-instances"></a>Verwenden von Speichermomentaufnahmen von SAP HANA in Azure (große Instanzen)

Die zugrunde liegende Speicherinfrastruktur von SAP HANA in Azure (große Instanzen) unterstützt das Konzept einer Speichermomentaufnahme von Volumes. Sicherung und Wiederherstellung von Volumes werden unterstützt, dabei ist jedoch Folgendes zu beachten:

- Anstelle von vollständigen Datenbanksicherungen werden in regelmäßigen Abständen Speichervolume-Momentaufnahmen erstellt.
- Beim Auslösen einer Momentaufnahme für die Volumes „/hana/data“, „hana/log“ und „/hana/shared“ (einschließlich „/usr/sap“) initiiert die Speichermomentaufnahme vor dem Erstellen der Speichermomentaufnahme eine SAP HANA-Momentaufnahme. Diese SAP HANA-Momentaufnahme ist der Einrichtungspunkt für nachfolgende Wiederherstellungen von Protokollen, nachdem die Speichermomentaufnahme wiederhergestellt wurde.
- Nach erfolgreicher Erstellung der Speichermomentaufnahme wird die SAP HANA-Momentaufnahme gelöscht.
- Transaktionsprotokollsicherungen werden in regelmäßigen Abständen erstellt und auf dem Volume „/hana/logbackups“ oder in Azure gespeichert. Für das Volume „/hana/logbackups“ mit den Transaktionsprotokollsicherungen kann eine separate Momentaufnahmeerstellung ausgelöst werden. In diesem Fall muss keine HANA-Momentaufnahme erstellt werden.
- Wenn die Datenbank bezogen auf einen bestimmten Zeitpunkt wiederhergestellt werden muss, wird eine Anforderung an den Microsoft Azure-Support (Produktionsausfall) oder das SAP HANA in Azure-Dienstverwaltungsteam gesendet, um eine Wiederherstellung bis zu einer bestimmten Speichermomentaufnahme durchführen zu lassen (z.B. eine geplante Wiederherstellung eines Sandboxsystems in seinem ursprünglichen Zustand).
- Die in der Speichermomentaufnahme enthaltene SAP HANA-Momentaufnahme ist ein Ausgangspunkt für die Anwendung von Transaktionsprotokollsicherungen, die nach Erstellung der Speichermomentaufnahme erstellt und gespeichert wurden.
- Diese Transaktionsprotokollsicherungen werden erstellt, um eine Point-in-Time-Wiederherstellung der Datenbank zu ermöglichen.

Als Kunde können Sie Speichermomentaufnahmen für drei verschiedene Volumeklassen erstellen:

- Kombinierte Momentaufnahme für „/hana/data“, „/hana/log“ und „/hana/shared“ (einschließlich „/usr/sap“). Diese Momentaufnahme erfordert die Erstellung einer SAP HANA-Momentaufnahme.
- Separate Momentaufnahme für „/hana/logbackups“
- Betriebssystempartition (nur für Typ I von HANA (große Instanzen))


### <a name="storage-snapshot-considerations"></a>Aspekte von Speichermomentaufnahmen

>[!NOTE]
>Speichermomentaufnahmen beanspruchen Speicherplatz, der den Einheiten von HANA (große Instanzen) zugeteilt wurde. Berücksichtigen Sie daher bei der Planung von Speichermomentaufnahmen und bei der Aufbewahrung mehrerer Speichermomentaufnahmen die folgenden Aspekte. 

Zur besonderen Funktionsweise von Speichermomentaufnahmen von SAP HANA in Azure (große Instanzen) gehört Folgendes:

- Eine individuelle Speichermomentaufnahme (zum Zeitpunkt der Erstellung) beansprucht nur wenig Speicherplatz.
- Wenn sich Dateninhalte und der Inhalt in SAP HANA-Datendateien auf dem Speichervolume ändern, muss die Momentaufnahme den ursprünglichen Blockinhalt speichern.
- Die Speichermomentaufnahme vergrößert sich. Je länger die Momentaufnahme vorhanden ist, desto größer wird die Speichermomentaufnahme.
- Je mehr Änderungen am SAP HANA-Datenbankvolume während der Lebensdauer einer Speichermomentaufnahme erfolgen, desto mehr Speicherplatz belegt die Speichermomentaufnahme.

SAP HANA in Azure (große Instanzen) bietet standardmäßig feste Volumegrößen für das SAP HANA-Daten- und Protokollvolume. Die Erstellung von Momentaufnahmen dieser Volumes verringert Ihren Volumespeicherplatz. Sie müssen daher Speichermomentaufnahmen planen, den Speicherplatzbedarf von Speichervolumes überwachen und die Anzahl gespeicherter Momentaufnahmen verwalten. Sie können Speichermomentaufnahmen jedoch später deaktivieren, wenn Sie Unmengen von Daten importieren oder andere wichtige Änderungen an der HANA-Datenbank vornehmen. 


Die folgenden Abschnitte enthalten Informationen zum Erstellen dieser Momentaufnahmen sowie allgemeine Empfehlungen:

- Obwohl die Hardware 255 Momentaufnahmen pro Volume unterstützen kann, wird dringend empfohlen, deutlich unter diesem Wert zu bleiben.
- Überwachen und verfolgen Sie den freien Speicherplatz, bevor Sie Speichermomentaufnahmen erstellen.
- Verringern Sie die Anzahl von Speichermomentaufnahmen basierend auf dem freien Speicherplatz. Unter Umständen müssen Sie die Anzahl von erstellten Momentaufnahmen verringern, oder Sie müssen die Volumes erweitern. (Sie können zusätzlichen Speicher in 1-TB-Schritten anfordern.)
- Bei Aktivitäten wie dem Verschieben von Daten in SAP HANA mit SAP-Plattformmigrationstools (R3load) oder dem Wiederherstellen von SAP HANA-Datenbanken aus Sicherungen müssen Sie Speichermomentaufnahmen für das Volume „/hana/data“ deaktivieren. 
- Bei größeren Reorganisationen von SAP HANA-Tabellen sollten Speichermomentaufnahmen nach Möglichkeit vermieden werden.
- Speichermomentaufnahmen sind eine Voraussetzung für die Nutzung der Notfallwiederherstellungsfunktionen von SAP HANA in Azure (große Instanzen).

### <a name="setting-up-storage-snapshots"></a>Einrichten von Speichermomentaufnahmen

Zum Einrichten von Speichermomentaufnahmen mit HANA (große Instanzen) sind allgemein folgende Schritte erforderlich:
1. Stellen Sie sicher, dass Perl im Linux-Betriebssystem auf dem Server mit HANA (große Instanzen) installiert ist.
2. Fügen Sie „/etc/ssh/ssh\_config“ die Zeile _MACs hmac-sha1_ hinzu.
3. Erstellen Sie ein SAP HANA-Sicherungsbenutzerkonto auf dem Masterknoten für jede SAP HANA-Instanz, die Sie ausführen (falls zutreffend).
4. Der SAP HANA HDB-Client muss auf allen Servern mit SAP HANA (große Instanzen) installiert werden.
5. Auf dem ersten Server mit SAP HANA (große Instanzen) in jeder Region muss ein öffentlicher Schlüssel erstellt werden, um auf die zugrunde liegende Speicherinfrastruktur zuzugreifen, die die Erstellung von Momentaufnahmen steuert.
6. Kopieren Sie die Skripts und die Konfigurationsdatei von [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts) an den Speicherort **hdbsql** der SAP HANA-Installation.
7. Ändern Sie bei Bedarf die Datei „HANABackupDetails.txt“ gemäß den jeweiligen Kundenvorgaben.

### <a name="step-1-install-sap-hana-hdbclient"></a>Schritt 1: Installieren des SAP HANA HDB-Clients

Das für SAP HANA in Azure (große Instanzen) installierte Linux-Betriebssystem enthält die Ordner und Skripts, die benötigt werden, um die SAP HANA-Speichermomentaufnahmen für die Sicherung und Notfallwiederherstellung zu erstellen. Überprüfen Sie, ob auf [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts) neuere Versionen zur Verfügung stehen. Die neueste Skriptversion ist 2.0.
Sie müssen allerdings bei der Installation von SAP HANA den SAP HANA HDB-Client für die Einheiten von HANA (große Instanzen) installieren. (Microsoft installiert weder den HDB-Client noch SAP HANA.)

### <a name="step-2-change-etcsshsshconfig"></a>Schritt 2: Ändern von „/etc/ssh/ssh\_config“

Ändern Sie „/etc/ssh/ssh\_config“ wie folgt durch das Hinzufügen der Zeile _MACs hmac-sha1_:
```
#   RhostsRSAAuthentication no
#   RSAAuthentication yes
#   PasswordAuthentication yes
#   HostbasedAuthentication no
#   GSSAPIAuthentication no
#   GSSAPIDelegateCredentials no
#   GSSAPIKeyExchange no
#   GSSAPITrustDNS no
#   BatchMode no
#   CheckHostIP yes
#   AddressFamily any
#   ConnectTimeout 0
#   StrictHostKeyChecking ask
#   IdentityFile ~/.ssh/identity
#   IdentityFile ~/.ssh/id_rsa
#   IdentityFile ~/.ssh/id_dsa
#   Port 22
Protocol 2
#   Cipher 3des
#   Ciphers aes128-ctr,aes192-ctr,aes256-ctr,arcfour256,arcfour128,aes128-cbc,3des-cbc
#   MACs hmac-md5,hmac-sha1,umac-64@openssh.com,hmac-ripemd160
MACs hmac-sha1
#   EscapeChar ~
#   Tunnel no
#   TunnelDevice any:any
#   PermitLocalCommand no
#   VisualHostKey no
#   ProxyCommand ssh -q -W %h:%p gateway.example.com
```

### <a name="step-3-create-a-public-key"></a>Schritt 3: Erstellen eines öffentlichen Schlüssels

Um den Zugriff auf die Speichermomentaufnahme-Schnittstellen Ihres HANA-Mandanten (große Instanzen) zu ermöglichen, müssen Sie eine Anmeldung einrichten, die auf einem öffentlichen Schlüssel basiert. Erstellen Sie in Ihrem Mandanten auf dem ersten Server mit SAP HANA in Azure (große Instanzen) einen öffentlichen Schlüssel, der für den Zugriff auf die Speicherinfrastruktur verwendet wird, um die Erstellung von Momentaufnahmen zu ermöglichen. Der öffentliche Schlüssel sorgt dafür, dass für die Anmeldung bei den Speichermomentaufnahme-Schnittstellen kein Kennwort erforderlich ist und dass Kennwortanmeldeinformationen nicht gespeichert werden. Führen Sie unter Linux auf dem Server mit SAP HANA (große Instanzen) den folgenden Befehl aus, um den öffentlichen Schlüssel zu generieren:
```
  ssh-keygen –t dsa –b 1024
```
Der neue Speicherort ist „_/root/.ssh/id\_dsa.pub“. Geben Sie kein Kennwort ein, da dies sonst bei jeder Anmeldung erforderlich ist. Drücken Sie stattdessen zweimal die**** EINGABETASTE, um die Kennworteingabeaufforderung bei der Anmeldung zu entfernen.

Vergewissern Sie sich, dass der öffentliche Schlüssel wie erwartet korrigiert wurde, indem Sie zu „/root/.ssh/“ navigieren und dann den Befehl **ls** ausführen. Wenn der Schlüssel vorhanden ist, können Sie ihn mit dem folgenden Befehl kopieren:

![Der öffentliche Schlüssel wird durch Ausführen dieses Befehls kopiert](./media/hana-overview-high-availability-disaster-recovery/image2-public-key.png)

Wenden Sie sich als Nächstes an das Dienstverwaltungsteam für SAP HANA in Azure, und geben Sie den öffentlichen Schlüssel an. Der Servicemitarbeiter verwendet den öffentlichen Schlüssel für die Registrierung in der zugrunde liegenden Speicherinfrastruktur, die für Ihren HANA-Mandanten (große Instanzen) eingerichtet ist.

### <a name="step-4-create-an-sap-hana-user-account"></a>Schritt 4: Erstellen eines SAP HANA-Benutzerkontos

Um die Erstellung von SAP HANA-Momentaufnahmen initiieren zu können, müssen Sie in SAP HANA einen Benutzer erstellen, der von den Speichermomentaufnahmeskripts verwendet werden kann. Erstellen Sie hierzu in SAP HANA Studio ein SAP HANA-Benutzerkonto. Dieses Konto benötigt die folgenden Berechtigungen: _Backup Admin_ und _Catalog Read_. In diesem Beispiel wird der Benutzername SCADMIN erstellt. Beim Namen des in HANA Studio erstellten Benutzerkontos wird die Groß-/Kleinschreibung beachtet.  Klicken Sie bei der Frage, ob Benutzer bei der nächsten Anmeldung ihr Kennwort ändern müssen, auf „Nein“.

![Erstellen eines Benutzers in HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image3-creating-user.png)

### <a name="step-5-authorize-the-sap-hana-user-account"></a>Schritt 5: Autorisieren des SAP HANA-Benutzerkontos

In diesem Schritt müssen Sie das von Ihnen erstellte SAP HANA-Benutzerkonto autorisieren, damit die Skripts zur Laufzeit keine Kennwörter übermitteln müssen. Mit dem SAP HANA-Befehl `hdbuserstore` können Sie einen SAP HANA-Benutzerschlüssel erstellen, der auf mindestens einem SAP HANA-Knoten gespeichert wird. Der Benutzerschlüssel ermöglicht dem Benutzer den Zugriff auf SAP HANA ohne Kennwortverwaltung im Rahmen des weiter unten beschriebenen Skriptprozesses.

>[!IMPORTANT]
>Führen Sie den folgenden Befehl als `root` aus. Andernfalls funktioniert das Skript nicht richtig.

Geben Sie den Befehl `hdbuserstore` wie folgt ein:

![Eingeben des Befehls „hdbuserstore“](./media/hana-overview-high-availability-disaster-recovery/image4-hdbuserstore-command.png)

Im folgenden Beispiel mit dem Benutzer „SCADMIN01“, dem Hostnamen „lhanad01“ und der Instanznummer 01 lautet der Befehl wie folgt:
```
hdbuserstore set SCADMIN01 lhanad01:30115 <backup username> <password>
```
Bei einer SAP HANA-Konfiguration mit horizontalem Hochskalieren müssen sämtliche Skripts über einen einzelnen Server verwaltet werden. In diesem Beispiel muss der SAP HANA-Schlüssel SCADMIN01 für jeden Host so angepasst werden, dass der zum Schlüssel passende Host widergespiegelt wird. Hierzu wird das SAP HANA-Sicherungskonto mit der Instanznummer der HANA-Datenbank angepasst. Der Schlüssel benötigt Administratorrechte auf dem Host, dem er zugewiesen ist. Der Sicherungsbenutzer (für horizontale Skalierung) benötigt Zugriffsrechte für alle SAP HANA-Instanzen. Wenn die drei Knoten für horizontales Hochskalieren „lhanad01“, „lhanad02“ und „lhanad03“ heißen, ergibt sich folgende Befehlsfolge:

```
hdbuserstore set SCADMIN01 lhanad01:30115 SCADMIN <password>
hdbuserstore set SCADMIN02 lhanad02:30215 SCADMIN <password>
hdbuserstore set SCADMIN03 lhanad03:30315 SCADMIN <password>
```

### <a name="step-6-get-snapshot-scripts-configure-snapshots-and-test-configuration-and-connectivity"></a>Schritt 6: Abrufen von Momentaufnahmeskripts, Konfigurieren von Momentaufnahmen und Testen von Konfiguration und Konnektivität

Laden Sie [hier](https://github.com/Azure/hana-large-instances-self-service-scripts) die neueste Version der Skripts herunter. Kopieren Sie die heruntergeladenen Skripts und die Textdatei in das Arbeitsverzeichnis für **hdbsql**. Bei aktuellen HANA-Installationen lautet dieses Verzeichnis in etwa „/hana/shared/D01/exe/linuxx86\_64/hdb“.
```
azure\_hana\_backup.pl
azure\_hana\_replication\_status.pl
azure\_hana\_snapshot\_details.pl
azure\_hana\_snapshot\_delelte.pl
testHANAConnection.pl
testStorageSnapshotConnection.pl
removeTestStorageSnapshot.pl
HANABackupCustomerDetails.txt
```

Zweck der einzelnen Skripts und Dateien:

- **azure\_hana\_backup.pl**: Dieses Skript muss mit Cron geplant werden, damit Speichermomentaufnahmen entweder für HANA-Daten-/Protokoll-/Freigabevolumes, für das Volume „/hana/logbackups“ oder für das Betriebssystem (oder Typ I-SKUs großer HANA-Instanzen) erstellt werden.
- **azure\_hana\_replication\_status.pl**: Dieses Skript dient dazu, grundlegende Replikationsstatusdetails des Produktionsstandorts für den Notfallwiederherstellungsstandort bereitzustellen.  Das Skript wird verwendet, um sicherzustellen, dass die Replikation stattfindet, und um die Größe der replizierten Elemente anzuzeigen.  Darüber hinaus liefert es Tipps für den Fall, dass eine Replikation zu lange dauert oder der Link möglicherweise ausgefallen ist.
- **azure\_hana\_snapshot\_details.pl**: Dieses Skript liefert dem Kunden eine Liste mit grundlegenden Details zu allen Momentaufnahmen pro Volume, die in Ihrer Umgebung vorhanden sind. Das Skript kann auf dem primären Server oder in einer Servereinheit am Standort für die Notfallwiederherstellung ausgeführt werden. Es liefert folgende Informationen (aufgeschlüsselt nach den einzelnen Volumes mit Momentaufnahmen): die Größe der gesamten Momentaufnahmen auf einem Volume sowie für jede Momentaufnahme auf dem jeweiligen Volume den Namen der Momentaufnahme, die Erstellungszeit, die Größe der Momentaufnahme, die Momentaufnahmehäufigkeit und die zugeordnete HANA-Sicherungs-ID (sofern relevant).
- **azure\_hana\_snapshot\_delelte.pl**: Dieses Skript dient zum Löschen einer Speichermomentaufnahme oder einer Gruppe von Momentaufnahmen – entweder anhand der SAP HANA-Sicherungs-ID aus HANA Studio oder anhand des Namens der Speichermomentaufnahme.  Die Sicherungs-ID ist derzeit nur mit den Momentaufnahmen verknüpft, die für die HANA-Daten-/Protokoll-/Freigabevolumes erstellt werden.  Ansonsten gilt: Wenn die Momentaufnahme-ID eingegeben wird, werden alle Momentaufnahmen gesucht, die der eingegebenen Momentaufnahme-ID entsprechen.  
- **testHANAConnection.pl**: Dieses Skript testet die Verbindung mit der SAP HANA-Instanz und wird zum Einrichten der Speichermomentaufnahmen benötigt.
- **testStorageSnapshotConnection.pl**: Dieses Skript hat zwei Aufgaben. Es stellt zum einen sicher, dass die Einheit von HANA (große Instanzen), die die Skripts ausführt, Zugriff auf den zugewiesenen virtuellen Speichercomputer und damit auf die Speichermomentaufnahmen-Schnittstelle Ihrer großen HANA-Instanzen hat. Zum anderen erstellt es eine temporäre Momentaufnahme der HANA-Instanz, die Sie testen. Dieses Skript muss für jede HANA-Instanz auf einem Server ausgeführt werden, um sicherzustellen, dass die Sicherungsskripts wie erwartet funktionieren.
- **removeTestStorageSnapshot.pl**: Dieses Skript dient zum Löschen der Testmomentaufnahme, die mit dem Skript „testStorageSnapshotConnection.pl“ erstellt wurde. 
- **HANABackupCustomerDetails.txt**: Hierbei handelt es sich um eine bearbeitbare Konfigurationsdatei, die Sie an Ihre SAP HANA-Konfiguration anpassen müssen.

 
Die Datei „HANABackupCustomerDetails.txt“ ist die Steuerungs- und Konfigurationsdatei für das Skript, das die Speichermomentaufnahmen erstellt. Passen Sie die Datei für Ihre Zwecke und Einstellungen an. Die Angaben für _Storage Backup Name_ (Name der Speichersicherung) und _Storage IP Address_ (IP-Adresse des Speichers) sollten Sie im Zuge der Bereitstellung Ihrer Instanzen vom Dienstverwaltungsteam für SAP HANA in Azure erhalten haben. Reihenfolge, Sortierung und Abstand der Variablen in dieser Datei dürfen nicht verändert werden. Andernfalls werden die Skripts nicht ordnungsgemäß ausgeführt. Darüber hinaus haben Sie vom Dienstverwaltungsteam für SAP HANA in Azure die IP-Adresse des Knotens für zentrales Hochskalieren oder die IP-Adresse des Masterknotens (bei horizontalem Hochskalieren) erhalten. Die HANA-Instanznummer ist nach der Installation von SAP HANA bekannt. Außerdem müssen Sie der Konfigurationsdatei noch einen Sicherungsnamen hinzufügen.

Bei einer Bereitstellung mit zentralem oder horizontalem Hochskalieren sieht die Konfigurationsdatei nach Angabe des Namens der Speichersicherung und der IP-Adresse des Speichers wie weiter unten gezeigt aus. In der Konfigurationsdatei müssen außerdem die IP-Adresse des Einzel- oder Masterknotens, die HANA-Instanznummer und ein frei wählbarer Sicherungsname angegeben werden:
```
#Provided by Microsoft Service Management
Storage Backup Name: client1hm3backup
Storage IP Address: 10.240.20.31
#Node IP addresses, instance numbers, and HANA backup name
#provided by customer.  HANA backup name created using
#hdbuserstore utility.
Node 1 IP Address: 
Node 1 HANA instance number:
Node 1 HANA Backup Name:
```

>[!NOTE]
>Derzeit werden nur die Details von Knoten 1 im eigentlichen Skript der HANA-Speichermomentaufnahme genutzt. Wir empfehlen Ihnen, den Zugriff auf bzw. von allen HANA-Knoten zu testen. Sollte sich der Masterknoten für die Sicherungen dann einmal ändern, haben Sie bereits sichergestellt, dass ein anderer Knoten seinen Platz einnehmen kann, indem die Details auf Knoten 1 geändert werden.

Nachdem Sie alle Konfigurationsdaten in die Datei „HANABackupCustomerDetails.txt“ eingegeben haben, müssen Sie sich mithilfe des Skripts „testHANAConnection.pl“ vergewissern, dass die Konfigurationen im Hinblick auf die HANA-Instanzdaten korrekt sind. Für dieses Skript spielt es keine Rolle, ob es sich um eine SAP HANA-Konfiguration mit zentralem oder mit horizontalem Hochskalieren handelt.

```
testHANAConnection.pl
```

Bei einer SAP HANA-Konfiguration mit horizontalem Hochskalieren benötigt die HANA-Masterinstanz Zugriff auf alle erforderlichen HANA-Server und -Instanzen. Für das Testskript sind zwar keine Parameter verfügbar, Sie müssen jedoch alle erforderlichen Daten in die Konfigurationsdatei „HANABackupCustomerDetails.txt“ eingeben, damit das Skript ordnungsgemäß ausgeführt wird. Da nur die Fehlercodes des Shell-Befehls zurückgegeben werden, ist es für das Skript nicht möglich, jede Instanz auf Fehler zu prüfen. Mit dem Skript werden trotzdem einige hilfreiche Kommentare bereitgestellt, die Sie überprüfen können.

So führen Sie das Skript aus
```
 ./testHANAConnection.pl
```
Wenn das Skript den Status der HANA-Instanz erfolgreich abruft, wird eine Nachricht mit dem Hinweis angezeigt, dass die HANA-Verbindung hergestellt wurde.


Im nächsten Schritt wird anhand der Daten, die Sie in der Konfigurationsdatei „HANABackupCustomerDetails.txt“ angegeben haben, die Verbindung mit dem Speicher überprüft und eine Testmomentaufnahme erstellt.  Dieser Test muss vor der Ausführung des Skripts „azure\_hana\_backup.pl“ durchgeführt werden. Falls ein Volume keine Momentaufnahmen enthält, kann nicht bestimmt werden, ob das Volume leer ist oder ob ein SSH-Fehler vorliegt und keine Momentaufnahmedetails abgerufen werden können. Aus diesem Grund werden mit dem Skript zwei Schritte ausgeführt:

- Es überprüft, ob die Skripts auf den virtuellen Speichercomputer des Mandanten und auf die Schnittstellen zugreifen können, um Momentaufnahmen zu erstellen.
- Anhand der HANA-Instanz wird für jedes Volume ein Test, ein Dummy oder eine Momentaufnahme erstellt.

Aus diesem Grund ist die HANA-Instanz als ein Argument enthalten. Sollte die Ausführung nicht erfolgreich sein, kann keine Fehlerüberprüfung für die Speicherverbindung durchgeführt werden. Das Skript stellt in solchen Fällen jedoch hilfreiche Hinweise bereit.

Das Skript wird ausgeführt als:
```
 ./testStorageSnapshotConnection.pl <HANA SID>
```
Als Nächstes versucht das Skript, sich unter Verwendung des öffentlichen Schlüssels aus den Einrichtungsschritten und den konfigurierten Daten aus „HANABackupCustomerDetails.txt“ beim Speicher anzumelden. Im Falle einer erfolgreichen Anmeldung wird Folgendes angezeigt:

```
**********************Checking access to Storage**********************
Storage Access successful!!!!!!!!!!!!!!
```

Sollten beim Herstellen der Verbindung mit der Speicherkonsole Probleme auftreten, sieht die Ausgabe wie folgt aus:

```
**********************Checking access to Storage**********************
WARNING: Storage check status command 'volume show -type RW -fields volume' failed: 65280
WARNING: Please check the following:
WARNING: Was publickey sent to Microsoft Service Team?
WARNING: If passphrase entered while using tool, publickey must be re-created and passphrase must be left blank for both entries
WARNING: Ensure correct IP address was entered in HANABackupCustomerDetails.txt
WARNING: Ensure correct Storage backup name was entered in HANABackupCustomerDetails.txt
WARNING: Ensure that no modification in format HANABackupCustomerDetails.txt like additional lines, line numbers or spacing
WARNING: ******************Exiting Script*******************************
```

Nach erfolgreicher Anmeldung bei den Schnittstellen des virtuellen Speichercomputers fährt das Skript mit Phase 2 fort und erstellt eine Testmomentaufnahme, wie hier für eine SAP HANA-Konfiguration mit drei Knoten und horizontalem Hochskalieren gezeigt:

```
**********************Creating Storage snapshot**********************
Taking snapshot testStorage.recent for hana_data_hm3_mnt00001_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00001_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00002_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00002_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00003_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00003_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_backups_hm3_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_backups_hm3_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_hm3_mnt00001_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_hm3_mnt00002_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_hm3_mnt00003_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_shared_hm3_t020_vol ...
Snapshot created successfully.
```

Wenn die Testmomentaufnahme erfolgreich mit dem Skript erstellt wurde, können Sie mit dem Konfigurieren der eigentlichen Speichermomentaufnahmen fortfahren. Untersuchen Sie andernfalls zunächst die Probleme. Bewahren Sie die Testmomentaufnahme auf, bis die ersten echten Momentaufnahmen erstellt wurden.



### <a name="step-7-perform-snapshots"></a>Schritt 7: Erstellen von Momentaufnahmen

Nach Abschluss der Vorbereitungsschritte können Sie damit beginnen, die eigentlichen Speichermomentaufnahmen zu konfigurieren. Das zu planende Skript kann für SAP HANA-Konfigurationen mit zentralem und horizontalem Hochskalieren verwendet werden. Die Planung der Skriptausführung erfolgt über Cron. 

Es können drei Arten von Momentaufnahmesicherungen erstellt werden:
- HANA: kombinierte Momentaufnahmesicherung, bei der die Volumes mit „/hana/data“, „/hana/log“ und „/hana/shared“ (einschließlich „/usr/sap“) durch die koordinierte Momentaufnahme abgedeckt sind. Auf der Grundlage dieser Momentaufnahme können einzelne Dateien wiederhergestellt werden.
- Protokolle: Momentaufnahmesicherung des Volumes „/hana/logbackups“. Zur Erstellung dieser Speichermomentaufnahme wird keine HANA-Momentaufnahme ausgelöst. Dieses Speichervolume ist für die Speicherung der SAP HANA-Transaktionsprotokollsicherungen vorgesehen, welche in kürzeren Abständen durchgeführt werden, um dem Protokollwachstum und potenziellen Datenverlusten entgegenzuwirken. Auf der Grundlage dieser Momentaufnahme können einzelne Dateien wiederhergestellt werden. Die Häufigkeit sollte mindestens fünf Minuten betragen.
- Start: Momentaufnahme des Volumes mit der Start-LUN der großen HANA-Instanz. Diese Momentaufnahmesicherung steht nur für Typ I-SKUs großer HANA-Instanzen zur Verfügung. Auf der Grundlage der Momentaufnahme des Volumes mit der Start-LUN können keine einzelnen Dateien wiederhergestellt werden.  


Aufrufsyntax für die drei unterschiedlichen Arten von Momentaufnahmen:
```
HANA backup covering /hana/data, /hana/log, /hana/shared (includes/usr/sap)
./azure_hana_backup.pl hana <HANA SID> manual 30

For /hana/logbackups snapshot
./azure_hana_backup.pl logs <HANA SID> manual 30

For snapshot of the volume storing the boot LUN
./azure_hana_backup.pl boot manual 30

```

Festzulegende Parameter:

- Der erste erforderliche Parameter gibt die Art der Momentaufnahmesicherung an. Zulässige Werte: „hana“, „logs“ und „boot“
- Der zweite Wert ist die HANA-SID (beispielsweise „HM3“). Bei einer Sicherung des Startvolumes wird dieser Parameter nicht benötigt.
- Beim dritten Parameter handelt es sich um die Sicherungsbezeichnung für die Art der Momentaufnahme. Es wird erwartet, dass er für geplante Momentaufnahmen eines bestimmten Typs oder einer bestimmten Häufigkeit unverändert bleibt.
- Der vierte Parameter definiert, wie viele Momentaufnahmen mit dem gleichen Momentaufnahmepräfix (Bezeichnung) gespeichert werden sollen – und damit indirekt die Aufbewahrung der Momentaufnahmen. Dieser Parameter ist für die geplante Ausführung über Cron wichtig. 

Bei Verwendung von horizontalem Hochskalieren führt das Skript einige zusätzliche Prüfungen durch, um den Zugriff auf alle HANA-Server sicherzustellen, und alle HANA-Instanzen geben den entsprechenden Status der Instanzen zurück, ehe mit der Erstellung der SAP HANA-Momentaufnahme und anschließend einer Speichermomentaufnahme fortgefahren wird.

Die Erstellung der Speichermomentaufnahme durch das Skript „azure\_hana\_backup.pl“ umfasst drei individuelle Phasen:

- Erstellen einer SAP HANA-Momentaufnahme
- Erstellen einer Speicher-Momentaufnahme
- Entfernen der SAP HANA-Momentaufnahme, die vor der Erstellung der Speichermomentaufnahme erstellt wurde

Führen Sie das Skript aus, indem Sie es im ausführbaren HDB-Ordner aufrufen, in den es kopiert wurde. 

Die Aufbewahrungsdauer richtet sich nach der Anzahl von Momentaufnahmen, die bei der Skriptausführung als Parameter übergeben wird (im obigen Beispiel: 30). Der von Speichermomentaufnahmen abgedeckte Zeitraum ergibt sich also aus dem Erstellungszeitraum und der Anzahl von Momentaufnahmen, die bei der Skriptausführung als Parameter angegeben wird. Wenn die Anzahl gespeicherter Momentaufnahmen die Anzahl übersteigt, die beim Skriptaufruf als Parameter angegeben wurde, wird vor der Erstellung einer neuen Momentaufnahme die älteste Speichermomentaufnahme mit der gleichen Bezeichnung (im obigen Fall: _manual_) gelöscht. Die Zahl, die Sie als letzten Parameter des Aufrufs angeben, steuert also die Anzahl gespeicherter Momentaufnahmen. So können Sie indirekt auch den von Momentaufnahmen beanspruchten Speicherplatz steuern. 

>Sobald Sie die Bezeichnung ändern, beginnt der Zählvorgang erneut. Überlegen Sie sich also vorher genau, welche Bezeichnungen Sie verwenden.

### <a name="snapshot-strategies"></a>Strategien für Momentaufnahmen
Die Momentaufnahmehäufigkeit für die unterschiedlichen Typen ist abhängig davon, ob Sie die Notfallwiederherstellung von HANA (große Instanzen) verwenden. Da die Notfallwiederherstellung von HANA (große Instanzen) auf Speichermomentaufnahmen basiert, gelten unter Umständen einige spezielle Empfehlungen in Bezug auf die Häufigkeit und Erstellungszeiträume von Speichermomentaufnahmen. In den folgenden Überlegungen und Empfehlungen wird davon ausgegangen, dass Sie die Notfallwiederherstellung von HANA (große Instanzen) NICHT verwenden. Stattdessen verwenden Sie die Speichermomentaufnahmen, um über Sicherungen zu verfügen und Point-in-Time-Wiederherstellungen für die letzten 30 Tage zu ermöglichen. Angesichts der Einschränkungen hinsichtlich der Anzahl von Momentaufnahmen und des Speicherplatzes wurden bei Kundenbereitstellungen folgende Arten von Anforderungen berücksichtigt:

- Anforderungen in Bezug auf die RTO (Recovery Time Objective) für die Zeitpunktwiederherstellung.
- Speicherplatzbelegung.
- Anforderungen in Bezug auf die RPO (Recovery Point Objective) und RTO (Recovery Time Objective) für eine potenzielle Notfallwiederherstellung.
- Tatsächliche Erstellung vollständiger HANA-Datenbanksicherungen für Datenträger. Immer wenn eine vollständige Datenbanksicherung für Datenträger (Schnittstelle _backint_) erfolgt, schlägt die Erstellung von Speichermomentaufnahmen fehl. Wenn Sie zusätzlich zu Speichermomentaufnahmen vollständige Datenbanksicherungen ausführen möchten, sollten Sie sicherstellen, dass die Erstellung von Speichermomentaufnahmen während dieses Zeitraums deaktiviert ist.
- Die Anzahl von Momentaufnahmen ist pro Volume auf 255 beschränkt.


Bei Kunden, die die Notfallwiederherstellung von HANA (große Instanzen) nicht verwenden, ist der Momentaufnahmezeitraum kürzer. In solchen Fällen erstellen Kunden die kombinierten Momentaufnahmen für „/hana/data“, „/hana/log“ und „/hana/shared“ (einschließlich „/usr/sap“) in einem Intervall von 12 oder 24 Stunden und speichern diese Momentaufnahmen, um einen ganzen Monat abzudecken. Gleiches gilt für die Momentaufnahmen des Protokollsicherungsvolumes. Im Gegensatz dazu werden SAP HANA-Transaktionsprotokollsicherungen für das Protokollsicherungsvolume in einem Intervall von fünf bis 15 Minuten ausgeführt.

Wir raten Ihnen, mit Cron geplante Speichermomentaufnahmen durchzuführen. Außerdem wird empfohlen, für alle Sicherungs- und Notfallwiederherstellungsaufgaben dasselbe Skript zu verwenden (indem Sie die Skripteingaben ändern, um es an die unterschiedlichen Zeiten für angeforderte Sicherungen anzupassen). Diese Momentaufnahmen werden in Cron abhängig von ihrer Ausführungszeit jeweils unterschiedlich geplant: stündlich, alle 12 Stunden, täglich oder wöchentlich. 

Im Anschluss sehen Sie ein Beispiel für einen Cron-Zeitplan in „/etc/crontab“:
```
00 1-23 * * * ./azure_hana_backup.pl hana HM3 hourlyhana 46
10 00 * * *  ./azure_hana_backup.pl hana HM3 dailyhana 28
00,05,10,15,20,25,30,35,40,45,50,55 * * * *  Perform SAP HANA transaction log backup
22 12 * * *  ./azure_hana_backup.pl log HM3 dailylogback 28
30 00 * * *  ./azure_hana_backup.pl boot dailyboot 28
```
Das obige Beispiel enthält eine stündliche kombinierte Momentaufnahme, die die Volumes mit den Speicherorten „/hana/data“, „/hana/log“ und „/hana/shared“ (einschließlich „/usr/sap“) abdeckt. Diese Art von Momentaufnahme wird für eine schnellere Point-in-Time-Wiederherstellung innerhalb von zwei Tagen verwendet. Darüber hinaus wird eine tägliche Momentaufnahme für diese Volumes erstellt. Somit verfügen Sie über stündliche Momentaufnahmen für einen Zeitraum von zwei Tagen sowie über tägliche Momentaufnahmen für einen Zeitraum von vier Wochen. Das Volume für die Transaktionsprotokollsicherung wird außerdem täglich gesichert. Diese Sicherungen werden ebenfalls vier Wochen aufbewahrt. In der dritten Zeile von „crontab“ sehen Sie, dass für die Sicherung des HANA-Transaktionsprotokolls ein Ausführungsintervall von fünf Minuten geplant ist. Außerdem sehen Sie, dass die Startminuten der verschiedenen Cron-Aufträge zur Erstellung von Speichermomentaufnahmen gestaffelt sind, damit die Momentaufnahmen nicht alle gleichzeitig zu einem bestimmten Zeitpunkt erstellt werden. 

Im folgenden Beispiel wird stündlich eine kombinierte Momentaufnahme erstellt, die die Volumes mit den Speicherorten „/hana/data“, „/hana/log“ und „/hana/shared“ (einschließlich „/usr/sap“) abdeckt. Diese Momentaufnahmen werden zwei Tage lang aufbewahrt. Die Momentaufnahmen der Volumes mit der Transaktionsprotokollsicherung werden alle fünf Minuten erstellt und vier Stunden lang aufbewahrt. Das Erstellungsintervall für die Sicherung der HANA-Transaktionsprotokolldatei ist wie zuvor auf fünf Minuten festgelegt. Die Momentaufnahme des Volumes mit der Transaktionsprotokollsicherung wird jeweils zwei Minuten nach dem Start der Transaktionsprotokollsicherung erstellt. Nach diesen zwei Minuten sollte die Sicherung des SAP HANA-Transaktionsprotokolls für gewöhnlich abgeschlossen sein. Die Volume mit der Start-LUN wird wie zuvor jeweils einmal pro Tag durch eine Speichermomentaufnahme gesichert und vier Wochen lang aufbewahrt.

```
10 0-23 * * * ./azure_hana_backup.pl hana HM3 hourlyhana 48
0,5,10,15,20,25,30,35,40,45,50,55 * * * *  Perform SAP HANA transaction log backup
2,7,12,17,22,27,32,37,42,47,52,57 * * * *  ./azure_hana_backup.pl log HM3 logback 48
30 00 * * *  ./azure_hana_backup.pl boot dailyboot 28
```

Die folgende Grafik zeigt die Abläufe aus dem obigen Beispiel (mit Ausnahme der Start-LUN):

![Beziehung zwischen Sicherungen und Momentaufnahmen](./media/hana-overview-high-availability-disaster-recovery/backup_snapshot.PNG)

SAP HANA führt regelmäßig Schreibvorgänge für das Volume „/hana/log“ aus, um die committeten Datenbankänderungen zu dokumentieren. In regelmäßigen Abständen schreibt SAP HANA einen Sicherungspunkt auf das Volume „/hana/data“. Wie in „crontab“ angegeben wird alle fünf Minuten eine SAP HANA-Transaktionsprotokollsicherung erstellt. Aufgrund der Auslösung einer kombinierten Speichermomentaufnahme für die Volumes „/hana/data“, „/hana/log“ und „/hana/shared“ wird zudem stündlich eine SAP HANA-Momentaufnahme erstellt. Nach erfolgreicher Erstellung der HANA-Momentaufnahme wird die kombinierte Speichermomentaufnahme erstellt. Gemäß Angabe in „crontab“ wird alle fünf Minuten (etwa zwei Minuten nach der HANA-Transaktionsprotokollsicherung) die Speichermomentaufnahme des Volumes „/hana/logbackup“ erstellt.


>[!IMPORTANT]
> Die Verwendung von Speichermomentaufnahmen für SAP HANA-Sicherungen ist nur sinnvoll, wenn diese in Verbindung mit SAP HANA-Transaktionsprotokollsicherungen erstellt werden. Diese Transaktionsprotokollsicherungen müssen die Zeiträume zwischen den Speichermomentaufnahmen abdecken. 

Sie benötigen Folgendes, wenn Sie für Benutzer eine Zeitpunktwiederherstellung für einen Zeitraum von 30 Tagen zugesagt haben:

- Zugriff auf eine kombinierte Speichermomentaufnahme für „/hana/data“, „/hana/log“ und „/hana/shared“, die maximal 30 Tage alt ist.
- Lückenlose Transaktionsprotokollsicherungen, die die Zeit zwischen den kombinierten Speichermomentaufnahmen abdecken. Die älteste Momentaufnahme des Volumes mit der Transaktionsprotokollsicherung muss also 30 Tage alt sein. Es sei denn, Sie haben die Transaktionsprotokollsicherungen in der Zwischenzeit auf eine andere NFS-Freigabe in Azure Storage kopiert. In diesem Fall können Sie ältere Transaktionsprotokollsicherungen von dieser NFS-Freigabe abrufen.

Um die Vorteile der Speichermomentaufnahmen und letztendlich der Speicherreplikation von Transaktionsprotokollsicherungen nutzen zu können, müssen Sie den Speicherort ändern, an den SAP HANA die Transaktionsprotokollsicherungen schreibt. Diese Änderung kann in HANA Studio durchgeführt werden, wie weiter unten dargestellt. SAP HANA sichert zwar automatisch vollständige Protokollsegmente, trotzdem sollten Sie ein deterministisches Protokollsicherungsintervall angeben. Die Erstellung von Protokollsicherungen mit einem deterministischen Intervall empfiehlt sich insbesondere bei Verwendung der Notfallwiederherstellungsoption. Im folgenden Beispiel wird ein 15-minütiges Protokollsicherungsintervall verwendet:

![Planen von SAP HANA-Sicherungsprotokollen in SAP HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image5-schedule-backup.png)

Sie können Sicherungen wählen, die häufiger als alle 15 Minuten erstellt werden. Dies empfiehlt sich insbesondere in Verbindung mit der Notfallwiederherstellung. Einige Kunden erstellen Transaktionsprotokollsicherungen alle fünf Minuten.  

Falls die Datenbank noch nie zuvor gesichert wurde, wird im letzten Schritt eine dateibasierte Datenbanksicherung erstellt, um einen einzelnen Sicherungseintrag zu erstellen, der im Sicherungskatalog vorhanden sein muss. Andernfalls kann SAP HANA Ihre angegebenen Protokollsicherungen nicht initiieren.

![Erstellen einer dateibasierten Sicherung zum Hinzufügen eines einzelnen Sicherungseintrags](./media/hana-overview-high-availability-disaster-recovery/image6-make-backup.png)


Nach erfolgreicher Erstellung der ersten Speichermomentaufnahmen können Sie die in Schritt 6 erstellte Testmomentaufnahme löschen. Führen Sie hierzu das Skript „removeTestStorageSnapshot.pl“ aus, wie im Anschluss gezeigt:
```
./removeTestStorageSnapshot.pl <hana instance>
```

### <a name="monitoring-the-number-and-size-of-snapshots-on-the-disk-volume"></a>Überwachen der Anzahl und Größe von Momentaufnahmen auf dem Datenträgervolume

Sie können die Anzahl von Momentaufnahmen und deren Speicherbedarf auf einem bestimmten Speichervolume überwachen. Mit dem Befehl `ls` werden keine Momentaufnahmenverzeichnisse oder -dateien angezeigt. Der Befehl `du` des Linux-Betriebssystems liefert jedoch Details zu den Speichermomentaufnahmen, da sie auf den gleichen Volumes gespeichert sind. Für den Befehl stehen folgende Optionen zur Verfügung:

- `du –sh .snapshot`: Gibt die Summe aller Momentaufnahmen im Verzeichnis für die Momentaufnahmen an.
- `du –sh --max-depth=1`: Listet alle Momentaufnahmen, die im Ordner „.snapshot“ gespeichert sind, und die Größe der einzelnen Momentaufnahmen auf.
- `du –hc`: Gibt die Gesamtgröße des Speicherplatzes an, der von allen Momentaufnahmen genutzt wird.

Verwenden Sie diese Befehle, um sicherzustellen, dass die erstellten und gespeicherten Momentaufnahmen nicht den gesamten Speicher auf den Volumes belegen.

>[!NOTE]
>Die Momentaufnahmen der Start-LUN werden bei Verwendung der obigen Befehle nicht angezeigt.

### <a name="getting-details-of-snapshots"></a>Abrufen von Details zu Momentaufnahmen
Ausführlichere Informationen zu Momentaufnahmen können mithilfe des Skripts „azure\_hana\_snapshot\_details.pl“ abgerufen werden. Dieses Skript kann an beiden Standorten ausgeführt werden, sofern am Standort für die Notfallwiederherstellung ein aktiver Server vorhanden ist. Es liefert folgende Informationen (aufgeschlüsselt nach den einzelnen Volumes mit Momentaufnahmen): die Größe der gesamten Momentaufnahmen auf einem Volume sowie die einzelnen Momentaufnahmen auf dem jeweiligen Volume mit dem Namen der Momentaufnahme, der Erstellungszeit, der Größe der Momentaufnahme, der Momentaufnahmehäufigkeit und der zugeordneten HANA-Sicherungs-ID (sofern relevant). Ausführungssyntax des Skripts:

```
./azure_hana_snapshot_details.pl 
```

Da das Skript versucht, die HANA-Sicherungs-ID abzurufen, muss es eine Verbindung mit der SAP HANA-Instanz herstellen. Dazu muss die Konfigurationsdatei „HANABackupCustomerDetails.txt“ korrekt konfiguriert sein. Eine Ausgabe zweier Momentaufnahmen eines Volumes kann beispielsweise wie folgt aussehen:

```
**********************************************************
****Volume: hana_shared_SAPTSTHDB100_t020_vol       ***********
**********************************************************
Total Snapshot Size:  411.8MB
----------------------------------------------------------
Snapshot:   customer.2016-09-20_1404.0
Create Time:   "Tue Sep 20 18:08:35 2016"
Size:   2.10MB
Frequency:   customer 
HANA Backup ID:   
----------------------------------------------------------
Snapshot:   customer2.2016-09-20_1532.0
Create Time:   "Tue Sep 20 19:36:21 2016"
Size:   2.37MB
Frequency:   customer2
HANA Backup ID:   
```


### <a name="file-level-restore-from-storage-snapshot"></a>Wiederherstellung auf Dateiebene auf der Grundlage einer Speichermomentaufnahme
Bei den Momentaufnahmetypen „hana“ und „logs“ können Sie direkt auf den Volumes im Verzeichnis „.snapshot“ auf die Momentaufnahmen zugreifen. Dort finden Sie jeweils ein Unterverzeichnis für die einzelnen Momentaufnahmen. Die einzelnen Dateien aus der Momentaufnahme können in dem Zustand, den sie zum Zeitpunkt der Momentaufnahme hatten, aus dem Unterverzeichnis in die eigentliche Verzeichnisstruktur kopiert werden.

>[!NOTE]
>Bei Momentaufnahmen der Start-LUN können keine einzelnen Dateien wiederhergestellt werden. Das Verzeichnis „.snapshot“ wird in der Start-LUN nicht verfügbar gemacht. 


### <a name="reducing-the-number-of-snapshots-on-a-server"></a>Verringern der Anzahl von Momentaufnahmen auf einem Server

Wie bereits beschrieben, können Sie die Anzahl von bestimmten Bezeichnungen von Momentaufnahmen reduzieren, die Sie speichern. Die beiden letzten Parameter des Befehls zum Erstellen einer Momentaufnahme sind eine Bezeichnung und die Anzahl von Momentaufnahmen, die Sie beibehalten möchten.

```
./azure_hana_backup.pl hana HM3 hanadaily 30
```

Im obigen Beispiel lautet die Bezeichnung der Momentaufnahme _customer_, und die Anzahl zu speichernder Momentaufnahmen mit dieser Bezeichnung ist auf _30_ festgelegt. Als Reaktion auf die Speicherplatzbelegung können Sie die Anzahl von gespeicherten Momentaufnahmen reduzieren. Die Anzahl von Momentaufnahmen kann ganz einfach auf beispielsweise 15 verringert werden, indem der letzte Parameter bei der Skriptausführung auf „15“ festgelegt wird:

```
./azure_hana_backup.pl hana HM3 hanadaily 15
```

Nachdem das Skript mit dieser Einstellung ausgeführt wurde, beträgt die Anzahl von Momentaufnahmen _15_ (einschließlich der neuen Speichermomentaufnahme). Die 15 neuesten Momentaufnahmen werden gespeichert, und die 15 älteren Momentaufnahmen werden gelöscht.

 >[!NOTE]
 > Die Anzahl von Momentaufnahmen verringert sich durch dieses Skript jedoch nur, wenn Momentaufnahme vorhanden sind, die älter als eine Stunde sind. Mit dem Skript werden Momentaufnahmen, die weniger als eine Stunde alt sind, also nicht gelöscht. Diese Einschränkungen beziehen sich auf die bereitgestellten optionalen Funktionen für die Notfallwiederherstellung.

Falls Sie eine Gruppe von Momentaufnahmen mit einer bestimmten Sicherungsbezeichnung (in diesem Beispiel: hanadaily) nicht mehr benötigen, können Sie das Skript mit _0_ als zu speichernde Anzahl ausführen, um alle Momentaufnahmen mit dieser Bezeichnung zu entfernen. Das Entfernen aller Momentaufnahmen kann sich aber auf die Funktionen für die Notfallwiederherstellung auswirken.

Alternativ können Sie bestimmte Momentaufnahmen auch mithilfe des Skripts „azure\_hana\_snapshot\_delelte.pl“ löschen. Dieses Skript dient zum Löschen einer Momentaufnahme oder einer Gruppe von Momentaufnahmen – entweder anhand der HANA-Sicherungs-ID aus HANA Studio oder anhand des Namens der Momentaufnahme.  Die Sicherungs-ID ist derzeit nur mit den Momentaufnahmen verknüpft, die für den Momentaufnahmetyp „hana“ erstellt werden. Bei Momentaufnahmesicherungen vom Typ „logs“ und „boot“ wird keine SAP HANA-Momentaufnahme erstellt. Daher ist bei diesen Momentaufnahmen auch keine Sicherungs-ID vorhanden. Bei Eingabe des Namens der Momentaufnahme wird auf den verschiedenen Volumes nach allen Momentaufnahmen gesucht, die dem eingegebenen Namen entsprechen. Aufrufsyntax für das Skript:

```
./azure_hana_snapshot_delete.pl 

```

Das Skript muss als Benutzer vom Typ _root_ ausgeführt werden.

Wenn Sie eine Momentaufnahme auswählen, können Sie jede Momentaufnahme einzeln löschen. Sie werden zunächst zur Angabe des Volumes mit der Momentaufnahme und anschließend zur Angabe des Namens der eigentlichen Momentaufnahme aufgefordert. Wenn die Momentaufnahme auf dem Volume vorhanden und über eine Stunde alt ist, wird sie gelöscht. Die Namen der Volumes und Momentaufnahmen können Sie durch Ausführen des Skripts „azure_hana_snapshot_details“ ermitteln. 

>[!IMPORTANT]
>Daten, die ausschließlich in der gelöschten Momentaufnahme enthalten sind, sind nach Ausführung des Löschvorgangs unwiederbringlich verloren.

   

### <a name="recovering-to-the-most-recent-hana-snapshot"></a>Wiederherstellen der letzten HANA-Momentaufnahme

Bei einem Ausfall der Produktionsumgebung kann die Wiederherstellung auf der Grundlage einer Speichermomentaufnahme als Kundenvorfall über den Microsoft Azure-Support initiiert werden. Ein unerwartetes Szenario dieser Art ist eine äußerst dringliche Angelegenheit, wenn Daten in einem Produktionssystem gelöscht wurden und die einzige Möglichkeit zum Wiedererlangen der Daten in der Wiederherstellung der Produktionsdatenbank besteht.

Eine Point-in-Time-Wiederherstellung kann dagegen eine niedrigere Dringlichkeit haben und Tage im Voraus geplant werden. Diese Wiederherstellung können Sie mit dem SAP HANA in Azure-Dienstverwaltungsteam planen, anstatt ein Problem mit hoher Priorität zu melden. Es kann beispielsweise sein, dass Sie ein Upgrade der SAP-Software planen, indem Sie ein neues Enhancement Package einspielen und anschließend zu einer Momentaufnahme zurückkehren müssen, die den Zustand vor dem Enhancement Package-Upgrade darstellt.

Vor dem Ausführen der Anforderung sind einige vorbereitende Schritte erforderlich. Das SAP HANA in Azure-Dienstverwaltungsteam kann die Anforderung dann bearbeiten und die wiederhergestellten Volumes bereitstellen. Anschließend ist es Ihre Aufgabe, die HANA-Datenbank basierend auf den Momentaufnahmen wiederherzustellen. Sie bereiten sich wie folgt auf die Anforderung vor:

>[!NOTE]
>Ihre Benutzeroberfläche kann von den folgenden Screenshots abweichen. Dies richtet sich nach der von Ihnen verwendeten SAP HANA-Version.

1. Entscheiden Sie, welche Momentaufnahme wiederhergestellt werden soll. Falls nicht anders angegeben, wird nur das Volume „hana/data“ wiederhergestellt. 

2. Fahren Sie die HANA-Instanz herunter.

 ![Herunterfahren der HANA-Instanz](./media/hana-overview-high-availability-disaster-recovery/image7-shutdown-hana.png)

3. Heben Sie die Bereitstellung der Datenvolumes auf jedem Knoten der HANA-Datenbank auf. Wenn die Datenvolumes noch in das Betriebssystem eingebunden sind, ist die Wiederherstellung der Momentaufnahme nicht erfolgreich.
 ![Heben Sie die Bereitstellung der Datenvolumes auf jedem Knoten der HANA-Datenbank auf](./media/hana-overview-high-availability-disaster-recovery/image8-unmount-data-volumes.png)

4. Erstellen Sie eine Azure-Supportanforderung, in der die Wiederherstellung einer bestimmten Momentaufnahme angefordert wird.

 - Während der Wiederherstellung: Unter Umständen werden Sie vom Dienstverwaltungsteam für SAP HANA in Azure zu Koordinierungs-, Überprüfungs- und Bestätigungszwecken gebeten, an einer Telefonkonferenz teilzunehmen, um die Wiederherstellung der korrekten Speichermomentaufnahme sicherzustellen. 

 - Nach der Wiederherstellung: Sie werden von SAP HANA unter der Azure-Dienstverwaltung benachrichtigt, wenn die Speichermomentaufnahme wiederhergestellt wurde.

5. Stellen Sie nach Abschluss des Wiederherstellungsvorgangs alle Datenvolumes wieder bereit.

 ![Erneutes Bereitstellen aller Datenvolumes](./media/hana-overview-high-availability-disaster-recovery/image9-remount-data-volumes.png)

6. Wählen Sie Wiederherstellungsoptionen in SAP HANA Studio aus, sofern diese nicht automatisch beim Wiederherstellen der Verbindung mit der HANA-Datenbank über SAP HANA Studio angezeigt werden. Das folgende Beispiel zeigt eine Wiederherstellung der letzten HANA-Momentaufnahme. Eine HANA-Momentaufnahme ist in eine Speichermomentaufnahme eingebettet. Wenn Sie die letzte Speichermomentaufnahme wiederherstellen, sollte dies die neueste HANA-Momentaufnahme sein. (Wenn Sie ältere Speichermomentaufnahmen wiederherstellen, müssen Sie die HANA-Momentaufnahme basierend auf dem Zeitpunkt der Erstellung der Speichermomentaufnahme bestimmen.)

 ![Auswählen von Wiederherstellungsoptionen in SAP HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image10-recover-options-a.png)

7. Wählen Sie **Recover the database to a specific data backup or storage snapshot** (Datenbank in eine bestimmte Datensicherung oder Speichermomentaufnahme wiederherstellen) aus.

 ![Fenster „Specify Recovery Type“ (Wiederherstellungstyp angeben)](./media/hana-overview-high-availability-disaster-recovery/image11-recover-options-b.png)

8. Wählen Sie **Specify backup without catalog** (Sicherung ohne Katalog angeben).

 ![Fenster „Specify Backup Location“ (Sicherungsspeicherort angeben)](./media/hana-overview-high-availability-disaster-recovery/image12-recover-options-c.png)

9. Wählen Sie in der Liste **Destination Type** (Zieltyp) die Option **Snapshot** (Momentaufnahme).

 ![Fenster „Specify the Backup to Recover“ (Wiederherzustellende Sicherung angeben)](./media/hana-overview-high-availability-disaster-recovery/image13-recover-options-d.png)

10. Klicken Sie auf **Finish** (Fertig stellen), um den Wiederherstellungsprozess zu starten.

 ![Klicken auf „Finish“ (Fertig stellen), um den Wiederherstellungsprozess zu starten](./media/hana-overview-high-availability-disaster-recovery/image14-recover-options-e.png)

11. Die HANA-Datenbank wird mithilfe der HANA-Momentaufnahme wiederhergestellt, die in der Speichermomentaufnahme enthalten ist.

 ![HANA-Datenbank wird über die HANA-Momentaufnahme wiederhergestellt](./media/hana-overview-high-availability-disaster-recovery/image15-recover-options-f.png)

### <a name="recovering-to-the-most-recent-state"></a>Wiederherstellen des letzten Zustands

Mit dem folgenden Prozess wird die HANA-Momentaufnahme wiederhergestellt, die in der Speichermomentaufnahme enthalten ist. Anschließend wird für die Transaktionsprotokollsicherungen der letzte Zustand der Datenbank wiederhergestellt, bevor die Speichermomentaufnahme wiederhergestellt wird.

>[!IMPORTANT]
>Stellen Sie sicher, dass Sie über eine vollständige und zusammenhängende Kette von Transaktionsprotokollsicherungen verfügen, bevor Sie fortfahren. Ohne diese Sicherungen können Sie den aktuellen Zustand der Datenbank nicht wiederherstellen.

1. Führen Sie die Schritte 1 bis 6 des obigen Verfahrens unter „Wiederherstellen der letzten HANA-Momentaufnahme“ aus.

2. Wählen Sie die Option **Recover the database to its most recent state** (Letzten Zustand der Datenbank wiederherstellen).

 ![Auswählen von „Recover the database to its most recent state“ (Letzten Zustand der Datenbank wiederherstellen)](./media/hana-overview-high-availability-disaster-recovery/image16-recover-database-a.png)

3. Geben Sie den Speicherort der letzten HANA-Protokollsicherungen an. Der Speicherort muss alle HANA-Transaktionsprotokollsicherungen aus der HANA-Momentaufnahme bis zum neuesten Zustand enthalten.

 ![Angeben des Speicherorts der letzten HANA-Protokollsicherungen](./media/hana-overview-high-availability-disaster-recovery/image17-recover-database-b.png)

4. Wählen Sie eine Sicherung als Grundlage für die Wiederherstellung der Datenbank aus. In unserem Beispiel ist die HANA-Momentaufnahme auf dem Screenshot die HANA-Momentaufnahme, die in der Speichermomentaufnahme enthalten war. 

 ![Auswählen einer Sicherung als Grundlage zum Wiederherstellen der Datenbank](./media/hana-overview-high-availability-disaster-recovery/image18-recover-database-c.png)

5. Deaktivieren Sie das Kontrollkästchen **Use Delta Backups** (Deltasicherungen verwenden), sofern diese nicht zwischen dem Zeitpunkt der HANA-Momentaufnahme und dem letzten Zustand vorhanden waren.

 ![Deaktivieren des Kontrollkästchens „Use Delta Backups“ (Deltasicherungen verwenden), falls keine Deltas vorhanden sind](./media/hana-overview-high-availability-disaster-recovery/image19-recover-database-d.png)

6. Klicken Sie auf dem Bildschirm mit der Zusammenfassung auf **Finish** (Fertig stellen), um den Wiederherstellungsvorgang zu starten.

 ![Klicken auf „Finish“ (Fertig stellen) auf dem Bildschirm mit der Zusammenfassung](./media/hana-overview-high-availability-disaster-recovery/image20-recover-database-e.png)

### <a name="recovering-to-another-point-in-time"></a>Wiederherstellen eines anderen Zeitpunkts
Gehen Sie zum Durchführen einer Zeitpunktwiederherstellung zwischen der HANA-Momentaufnahme (die in der Speichermomentaufnahme enthalten ist) und einer späteren Zeitpunktwiederherstellung der HANA-Momentaufnahme wie folgt vor:

1. Stellen Sie sicher, dass Sie über alle Transaktionsprotokollsicherungen in der HANA-Momentaufnahme bis zum Zeitpunkt verfügen, den Sie wiederherstellen möchten.
2. Beginnen Sie mit dem Verfahren unter „Wiederherstellen des letzten Zustands“.
3. Wählen Sie in Schritt 2 des Verfahrens im Fenster **Specify Recovery Type** (Wiederherstellungstyp angeben) die Option **Recover the database to the following point in time** (Datenbank für den folgenden Zeitpunkt wiederherstellen), geben Sie den Zeitpunkt an, und führen Sie die Schritte 3 bis 6 aus.

### <a name="monitoring-the-execution-of-snapshots"></a>Überwachen der Ausführung von Momentaufnahmen

Bei Verwendung von Speichermomentaufnahmen von HANA (große Instanzen) müssen Sie auch die Erstellung dieser Speichermomentaufnahmen überwachen. Das Skript, das eine Speichermomentaufnahme ausführt, schreibt die Ausgabe in eine Datei und speichert sie an demselben Speicherort wie die Perl-Skripts. Für jede Speichermomentaufnahme wird eine separate Datei geschrieben. Die Ausgabe jeder Datei zeigt eindeutig die verschiedenen Phasen, die vom Momentaufnahmenskript ausgeführt werden:

- Suchen der Volumes, für die eine Momentaufnahme erstellt werden muss
- Suchen der Momentaufnahmen, die von diesen Volumes erstellt wurden
- Löschen von eventuell vorhandenen Momentaufnahmen übereinstimmend mit der Anzahl von Momentaufnahmen, die Sie angegeben haben
- Erstellen einer SAP HANA-Momentaufnahme
- Erstellen der Speichermomentaufnahme über alle Volumes hinweg
- Löschen der SAP HANA-Momentaufnahme
- Umbenennen der neuesten Momentaufnahme in **.0**

Der wichtigste Teil der Skript-CAB-Datei ist folgender:
```
**********************Creating HANA snapshot**********************
Creating the HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data create snapshot"" ...
HANA snapshot created successfully.
**********************Creating Storage snapshot**********************
Taking snapshot hourly.recent for hana_data_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_log_backup_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_log_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_shared_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for sapmnt_lhanad01_t020_vol ...
Snapshot created successfully.
**********************Deleting HANA snapshot**********************
Deleting the HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data drop snapshot"" ...
HANA snapshot deletion successfully.
```
Sie können anhand dieses Beispiels erkennen, wie das Skript die Erstellung der HANA-Momentaufnahme aufzeichnet. Bei horizontaler Skalierung wird dieser Prozess auf dem Masterknoten ausgelöst. Der Masterknoten initiiert die synchrone Erstellung der SAP HANA-Momentaufnahmen auf den einzelnen Workerknoten. Dann wird die Speichermomentaufnahme erstellt. Nach erfolgreicher Erstellung der Speichermomentaufnahmen wird die HANA-Momentaufnahme gelöscht. Dies wird erneut vom Masterknoten initiiert.


## <a name="disaster-recovery-principles"></a>Prinzipien für die Notfallwiederherstellung
Mit HANA (große Instanzen) bieten wir eine Notfallwiederherstellungsfunktion zwischen Stapeln für HANA (große Instanzen) in verschiedenen Azure-Regionen an. Wenn Sie also Einheiten von HANA (große Instanzen) in der Azure-Region „USA, Westen“ bereitstellen, können Sie Einheiten von HANA (große Instanzen) in der Region „USA, Osten“ als Notfallwiederherstellungseinheiten verwenden. Die Notfallwiederherstellung wird wie bereits erwähnt nicht automatisch konfiguriert, da sie eine weitere kostenpflichtige Einheit von HANA (große Instanzen) in der Notfallwiederherstellungsregion erfordert. Die Notfallwiederherstellungseinrichtung kann mit zentralem und mit horizontalem Hochskalieren verwendet werden. 

In den bislang bereitgestellten Szenarien haben Kunden die Einheit in der Notfallwiederherstellungsregion zur Ausführung produktionsfremder Systeme mit einer installierten HANA-Instanz verwendet. Die Einheit von HANA (große Instanzen) muss der gleichen SKU angehören wie die für die Produktion verwendete SKU. Anschließend müssen Sie einige weitere Datenträgervolumes anfordern, damit diese am Standort für die Notfallwiederherstellung als Ziele für die Speicherreplikation aus dem Produktionssystem verwendet werden können. Die tatsächlich am Standort für die Notfallwiederherstellung replizierten Volumes decken Folgendes ab:

- /hana/data
- /hana/log
- /hana/shared
- /hana/log/backup (einschließlich „/usr/sap“)


Die angebotene Notfallwiederherstellungsfunktion basiert auf der Speicherreplikationsfunktion der Infrastruktur für HANA (große Instanzen). Die speicherseitig verwendete Funktion ist kein konstanter Datenstrom mit Änderungen, die asynchron repliziert werden, wenn Änderungen am Speicher auftreten. Stattdessen basiert der Mechanismus darauf, dass in regelmäßigen Abständen Momentaufnahmen der Volumes erstellt werden. Anschließend werden die Veränderungen zwischen einer bereits replizierten und einer neuen, noch nicht replizierten Momentaufnahme an Zieldatenträgervolumes am Standort für die Notfallwiederherstellung übertragen. (Die Zieldatenträgervolumes haben dabei die gleiche Größe wie die Produktionsvolumes.) Beim ersten Mal müssen die vollständigen Daten eines Volumes übertragen werden. Danach nimmt der Umfang der Daten ab, da nur noch die Veränderungen zwischen Momentaufnahmen übertragen werden müssen. Somit enthalten die Volumes am Standort für die Notfallwiederherstellung sämtliche Volumemomentaufnahmen, die am Produktionsstandort erstellt wurden. Dadurch können Sie mithilfe des Notfallwiederherstellungssystems letztendlich zu einem früheren Status zurückkehren, um verloren gegangene Daten ohne Rollback des Produktionssystems wiederherzustellen.

Wenn die HANA-Systemreplikation am Produktionsstandort als Hochverfügbarkeitsfunktion genutzt wird, werden nur die Volumes der Instanz der zweiten Ebene (oder der Replikatinstanz) repliziert. Diese Konfiguration kann zu einer Verzögerung bei der Speicherreplikation am Standort für die Notfallwiederherstellung führen, wenn Sie die sekundäre Replikatservereinheit (Ebene 2) oder die SAP HANA-Instanz in dieser Einheit warten oder herunterfahren. 

>[!IMPORTANT]
>Genau wie bei der mehrschichtigen HANA-Systemreplikation wird durch Herunterfahren der HANA-Instanz (Ebene 2) oder der Servereinheit die Replikation am Standort für die Notfallwiederherstellung blockiert, wenn Sie die Notfallwiederherstellung von HANA (große Instanzen) nutzen.


>[!NOTE]
>Die Speicherreplikationsfunktion von HANA (große Instanzen) spiegelt/repliziert Speichermomentaufnahmen. Wenn Sie also keine Speichermomentaufnahmen erstellen, wie in diesem Dokument im Abschnitt zur Sicherung erläutert, ist keine Replikation am Standort für die Notfallwiederherstellung möglich. Die Erstellung von Speichermomentaufnahmen ist eine Voraussetzung für die Speicherreplikation am Standort für die Notfallwiederherstellung.

Zur Minimierung der Recovery Point Objective werden im Dienst für große HANA-Instanzen folgende Replikationsintervalle festgelegt:
- Die durch die kombinierte Momentaufnahme abgedeckten Volumes (Momentaufnahmetyp „hana“) werden alle 15 Minuten an die entsprechenden Speichervolumeziele am Standort für die Notfallwiederherstellung repliziert.
- Das Volume für die Transaktionsprotokollsicherung (Momentaufnahmetyp „log“) wird alle drei Minuten an die entsprechenden Speichervolumeziele am Standort für die Notfallwiederherstellung repliziert.

Zur Minimierung der Recovery Point Objective muss Folgendes eingerichtet werden:
- Erstellen Sie Speichermomentaufnahmen vom Typ „hana“ (siehe Schritt 7: Erstellen von Momentaufnahmen) alle 30 bis 60 Minuten.
- Erstellen Sie SAP HANA-Transaktionsprotokollsicherungen alle fünf Minuten.
- Erstellen Sie Speichermomentaufnahmen vom Typ „logs“ alle fünf bis 15 Minuten. Damit sollte eine RPO von etwa 15 bis 25 Minuten möglich sein.

Die folgende Grafik zeigt eine mögliche Sequenz der Transaktionsprotokollsicherungen und Speichermomentaufnahmen sowie der Replikation des Volumes mit der HANA-Transaktionsprotokollsicherung und von „/hana/data“, „/hana/log“ und „/hana/shared“ (einschließlich „/usr/sap“) bei Verwendung dieser Einstellungen:

 ![Beziehung zwischen Momentaufnahme der Transaktionsprotokollsicherung und Momentaufnahmespiegelung im Zeitverlauf](./media/hana-overview-high-availability-disaster-recovery/snapmirror.PNG)

Um bei der Notfallwiederherstellung eine noch bessere Recovery Point Objective zu erzielen, können Sie die HANA-Transaktionsprotokollsicherungen aus SAP HANA in Azure (große Instanzen) in die andere Azure-Region kopieren. Zur Erzielung dieser weiteren Senkung der Recovery Point Objective müssen folgende Schritte ausgeführt werden:

1. Sichern Sie das HANA-Transaktionsprotokoll so häufig wie möglich unter „/hana/logbackups“.
2. Kopieren Sie die abgeschlossenen Transaktionsprotokollsicherungen mithilfe von rsync auf virtuelle Azure-Computer, die auf einer NFS-Freigabe gehostet werden und sich in virtuellen Azure-Netzwerken in der Azure-Produktionsregion und in den Regionen für die Notfallwiederherstellung befinden. Beide virtuellen Azure-Netzwerke müssen mit der ExpressRoute-Verbindung verbunden werden, die die großen HANA-Produktionsinstanzen mit Azure verbindet (wie in diesem Dokument in der Grafik im Abschnitt mit den Notfallwiederherstellungsanforderungen dargestellt). 
3. Belassen Sie die Transaktionsprotokollsicherungen in dieser Region im angefügten exportierten NFS-Speicher des virtuellen Computers.
4. Ergänzen Sie bei der Notfallwiederherstellung die Transaktionsprotokollsicherungen aus dem Volume „/hana/logbackups“ mit neueren Transaktionsprotokollsicherungen aus der NFS-Freigabe am Standort für die Notfallwiederherstellung. 
5. Nun können Sie eine Wiederherstellung der Transaktionsprotokollsicherung auf die neueste Sicherung starten, die in der Region für die Notfallwiederherstellung gespeichert werden konnte.

## <a name="disaster-recovery-fail-over-procedure"></a>Failoverprozedur für die Notfallwiederherstellung
Bei einem Failover auf den Standort für die Notfallwiederherstellung müssen Sie mit SAP HANA in Azure-Vorgängen interagieren. Der Prozess sieht im Groben wie folgt aus:

- Da Sie mit der Notfallwiederherstellungseinheit von HANA (große Instanzen) eine produktionsfremde Instanz von HANA ausführen, müssen Sie diese Instanz herunterfahren. Wir gehen davon aus, dass eine ruhende HANA Produktionsinstanz vorinstalliert ist.
- Sie müssen sicherstellen, dass keine SAP HANA-Prozesse mehr ausgeführt werden. Die entsprechende Überprüfung kann mithilfe des folgenden Befehls durchgeführt werden: /usr/sap/hostctrl/exe/sapcontrol –nr <HANA instance number> - function GetProcessList. In der Ausgabe muss der Prozess „hdbdaemon“ den Zustand „Beendet“ aufweisen, und es dürfen keine anderen HANA-Prozesse mit dem Zustand „Gestartet“ vorhanden sein.
- Als Nächstes müssen Sie überprüfen, auf welchen Momentaufnahmenamen bzw. auf welche SAP HANA-Sicherungs-ID der Standort für die Notfallwiederherstellung wiederhergestellt werden soll. In der Praxis handelt es sich dabei in der Regel um die neueste Momentaufnahme. Es gibt jedoch Situationen, in denen Sie möglicherweise verloren gegangene Daten wiederherstellen möchten. In einem solchen Fall müsste dann eine ältere Momentaufnahme gewählt werden.
- Sie müssen sich über eine Supportanfrage mit hoher Priorität an das Dienstverwaltungsteam für SAP HANA in Azure wenden und die Wiederherstellung der Momentaufnahme/HANA-Sicherungs-ID am Standort für die Notfallwiederherstellung anfordern. Auf der Vorgangsseite werden folgende Schritte ausgeführt:
    - Die Replikation von Momentaufnahmen aus dem Produktionsvolume in Notfallwiederherstellungsvolumes wird beendet (sofern dies nicht ohnehin bereits durch den Ausfall des Produktionsstandorts verursacht wurde).
    - Die von Ihnen ausgewählte Speichermomentaufnahme/Sicherungs-ID wird auf den Notfallwiederherstellungsvolumes wiederhergestellt.
    - Nach der Wiederherstellung können die Notfallwiederherstellungsvolumes in die Einheiten von HANA (große Instanzen) in der Notfallwiederherstellungsregion eingebunden werden.
- Auf der Kundenseite müssen Sie als Nächstes die Notfallwiederherstellungsvolumes in die Einheit von HANA (große Instanzen) am Standort für die Notfallwiederherstellung einbinden. 
- Anschließend können Sie die bislang ruhende SAP HANA-Produktionsinstanz starten.
- Wenn Sie zur Verringerung der RPO-Zeit zusätzlich Protokolle der Transaktionsprotokollsicherung kopiert haben, müssen Sie diese Transaktionsprotokollsicherungen mit dem neu eingebundenen Notfallwiederherstellungsverzeichnis „hana/logbackups“ zusammenführen. Überschreiben Sie bereits vorhandene Sicherungen nicht, sondern kopieren Sie einfach neuere Sicherungen, die vom Speicher noch nicht repliziert wurden.

Die nächsten Schritte dienen zur Wiederherstellung der SAP HANA-Produktionsinstanz auf der Grundlage der wiederhergestellten Speichermomentaufnahme und der verfügbaren Transaktionsprotokollsicherungen. Hierzu sind folgende Schritte erforderlich:

Ändern Sie den Sicherungsspeicherort mithilfe von SAP HANA Studio in „hana/logbackups“, wie hier zu sehen: ![Ändern des Sicherungsspeicherorts für die Notfallwiederherstellung](./media/hana-overview-high-availability-disaster-recovery/change_backup_location_dr1.png)

SAP HANA überprüft alle Sicherungsdateispeicherorte und schlägt für die Wiederherstellung die neueste Transaktionsprotokollsicherung vor. Die Überprüfung kann einige Minuten dauern. Danach wird ein Bildschirm wie der folgende angezeigt: ![Liste mit Transaktionsprotokollsicherungen die Notfallwiederherstellung](./media/hana-overview-high-availability-disaster-recovery/backup_list_dr2.PNG)

Im nächsten Schritt müssen einige Standardeinstellungen angepasst werden:

- Deaktivieren Sie das Kontrollkästchen **Use Delta Backups** (Deltasicherungen verwenden).
- Aktivieren Sie das Kontrollkästchen **Initialize Log Area** (Protokollbereich initialisieren), wie im Anschluss gezeigt:

 ![Festlegen der Initialisierung des Protokollbereichs](./media/hana-overview-high-availability-disaster-recovery/initialize_log_dr3.PNG)

Klicken Sie auf dem nächsten Bildschirm auf **Fertig stellen**, wie im Anschluss gezeigt:

 ![Fertigstellen der Notfallwiederherstellung](./media/hana-overview-high-availability-disaster-recovery/finish_dr4.PNG)

Daraufhin sollte ein Statusfenster wie das folgende erscheinen. Beachten Sie, dass es sich bei diesem Beispiel um eine Notfallwiederherstellung einer SAP HANA-Konfiguration mit drei Knoten und horizontalem Hochskalieren handelt.

 ![Status der Wiederherstellung](./media/hana-overview-high-availability-disaster-recovery/restore_progress_dr5.PNG)

Falls die Wiederherstellung auf dem Fertigstellungsbildschirm anscheinend nicht fortgesetzt und der Statusbildschirm nicht angezeigt wird, überprüfen Sie, ob alle SAP HANA-Instanzen auf den Workerknoten ausgeführt werden. Starten Sie die SAP HANA-Instanzen bei Bedarf manuell.


### <a name="fail-back-from-dr-to-production-site"></a>Failback vom Standort für die Notfallwiederherstellung auf den Produktionsstandort
Wir gehen davon aus, dass das Failover auf den Standort für die Notfallwiederherstellung auf Probleme in der Azure-Produktionsregion zurückzuführen ist (und nicht darauf, dass Sie verloren gegangene Daten wiederherstellen möchten). Das bedeutet, dass Sie den Notfallwiederherstellungsstandort eine Weile für die Produktion verwendet haben. Nachdem die Probleme am Produktionsstandort behoben wurden, möchten Sie ein Failback auf den Produktionsstandort durchführen. Da hierbei keine Daten verloren gehen dürfen, umfasst der Wechsel zum Produktionsstandort mehrere Schritte und eine enge Zusammenarbeit mit SAP HANA für Azure-Vorgänge. Nach der Behebung der Probleme muss der Kunde die Vorgänge für die Rücksynchronisierung mit dem Produktionsstandort initiieren.

Ablauf:

- SAP HANA für Azure-Vorgänge ruft den Trigger zum Synchronisieren der Produktionsspeichervolumes aus den Speichervolumes für die Notfallwiederherstellung ab, die jetzt den Produktionszustand darstellen. In diesem Zustand wird die Einheit von HANA (große Instanzen) am Produktionsstandort heruntergefahren.
- SAP HANA für Azure-Vorgänge überwacht die Replikation und stellt sicher, dass die Daten auf dem neuesten Stand sind, bevor Sie (der Kunde) informiert werden.
- Sie müssen die Anwendungen herunterfahren, die die HANA-Produktionsinstanz am Standort für die Notfallwiederherstellung verwenden. Erstellen Sie eine HANA-Transaktionsprotokollsicherung, und beenden Sie anschließend die HANA-Instanz, die am Standort für die Notfallwiederherstellung in Einheiten von HANA (große Instanzen) ausgeführt wird.
- Nach dem Herunterfahren der HANA-Instanz, die am Standort für die Notfallwiederherstellung in der Einheit von HANA (große Instanzen) ausgeführt wurde, müssen die Datenträgervolumes manuell synchronisiert werden.
- SAP HANA für Azure-Vorgänge startet die Einheit von HANA (große Instanzen) am Produktionsstandort wieder und übergibt sie an Sie. Vergewissern Sie sich, dass sich die SAP HANA-Instanz beim Start der Einheit von HANA (große Instanzen) im Zustand „Heruntergefahren“ befindet.
- Als Nächstes müssen Sie die gleichen Datenbankwiederherstellungsschritte ausführen wie zuvor beim Failover auf den Standort für die Notfallwiederherstellung.

### <a name="monitoring-disaster-recovery-replication"></a>Überwachen der Replikation für die Notfallwiederherstellung

Sie können den Status der Speicherreplikation mithilfe des Skripts „azure\_hana\_replication\_status.pl“ überwachen. Dieses Skript muss über eine Einheit ausgeführt werden, die am Standort für die Notfallwiederherstellung ausgeführt wird, da es andernfalls nicht ordnungsgemäß funktioniert. Für die Ausführung des Skripts spielt es keine Rolle, ob die Replikation aktiv ist. Das Skript kann für jede Einheit von HANA (große Instanzen) Ihres Mandanten am Standort für die Notfallwiederherstellung ausgeführt werden. Es kann nicht verwendet werden, um Details zum Startvolume abzurufen.

Rufen Sie das Skript wie folgt auf:
```
./replication_status.pl <HANA SID>
```

Die Ausgabe wird nach Volumes in folgende Abschnitte unterteilt:  

- Linkstatus
- Aktuelle Replikationsaktivität
- Neueste replizierte Momentaufnahme 
- Größe der neuesten Momentaufnahme
- Aktueller Zeitabstand zwischen der zuletzt abgeschlossen Momentaufnahme und dem aktuellen Zeitpunkt  

Der Linkstatus lautet „Active“ (Aktiv) – es sei denn, die Verbindung zwischen Standorten ist unterbrochen oder es findet gerade ein Failoverereignis statt. Die Replikationsaktivität gibt Aufschluss darüber, ob momentan Daten repliziert werden, ob sich die Aktivität im Leerlauf befindet oder ob gerade andere Aktivitäten für den Link stattfinden. Für die zuletzt replizierte Momentaufnahme sollte nur „snapmirror...“ angezeigt werden. Anschließend folgt die Größe der letzten Momentaufnahme. Zuletzt wird der Zeitabstand angezeigt. Der Zeitabstand ist die Zeit zwischen der geplanten Replikationszeit und dem Abschluss der Replikation. Der Wert für den Zeitabstand kann bei der Datenreplikation mehr als eine Stunde betragen, obwohl die Replikation gestartet wurde. Dies gilt insbesondere bei der ersten Replikation. Der Zeitabstand nimmt bis zum Abschluss der laufenden Replikation immer weiter zu.

Beispielausgabe:

```
hana_data_hm3_mnt00002_t020_dp
-------------------------------------------------
Link Status: Broken-Off
Current Replication Activity: Idle
Latest Snapshot Replicated: snapmirror.c169b434-75c0-11e6-9903-00a098a13ceb_2154095454.2017-04-21_051515
Size of Latest Snapshot Replicated: 244KB
Current Lag Time between snapshots: -   ***Less than 90 minutes is acceptable***
```












