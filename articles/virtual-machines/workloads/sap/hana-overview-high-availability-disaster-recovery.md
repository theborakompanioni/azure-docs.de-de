---
title: "Hochverfügbarkeit und Notfallwiederherstellung für SAP HANA in Azure (große Instanzen) | Microsoft-Dokumentation"
description: "Bereitstellen von Hochverfügbarkeit und Planen der Notfallwiederherstellung für SAP HANA in Azure (große Instanzen)"
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
ms.date: 09/11/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 2c6cf0eff812b12ad852e1434e7adf42c5eb7422
ms.openlocfilehash: 87ea8b808c0b7e5fe79a5bee038a3d34ed59a1e6
ms.contentlocale: de-de
ms.lasthandoff: 09/13/2017

---
# <a name="sap-hana-large-instances-high-availability-and-disaster-recovery-on-azure"></a>Hochverfügbarkeit und Notfallwiederherstellung für SAP HANA (große Instanzen) in Azure 

Hochverfügbarkeit und Notfallwiederherstellung sind wichtige Aspekte bei der Ausführung Ihrer unternehmenskritischen Server mit SAP HANA in Azure (große Instanzen). Es ist wichtig, dass Sie mit SAP, Ihrem Systemintegrator oder Microsoft zusammenarbeiten, um die richtige Strategie für Hochverfügbarkeit und die Notfallwiederherstellung zu implementieren. Außerdem ist es wichtig, die Werte für die RPO (Recovery Point Objective) und die RTO (Recovery Time Objective) zu berücksichtigen, die für Ihre Umgebung gelten.

Microsoft unterstützt mit HANA (große Instanzen) standardmäßig einige SAP HANA-Methoden für Hochverfügbarkeit. Diese Funktionen umfassen:

- **Speicherreplikation**: Die Fähigkeit des Speichersystems, alle Daten an einen anderen Stapel für HANA (große Instanzen) in einer anderen Azure-Region zu replizieren. SAP HANA wird unabhängig von dieser Methode betrieben.
- **HANA-Systemreplikation**: Die Replikation aller Daten in SAP HANA in ein anderes SAP HANA-System. RTO (Recovery Time Objective) wird durch die Datenreplikation in regelmäßigen Abständen verringert. SAP HANA unterstützt den asynchronen, synchronen In-Memory- und den synchronen Modus. Der synchrone Modus wird nur für SAP HANA-Systeme innerhalb des gleichen Rechenzentrums oder mit einer Entfernung von weniger als 100 km zueinander empfohlen. Beim aktuellen Entwurf mit HANA-„Stapeln“ mit großen Instanzen kann die HANA-Systemreplikation nur für hohe Verfügbarkeit verwendet werden. Die HANA-Systemreplikation muss gegenwärtig mit einer Drittanbieter-Reverseproxykomponente für Notfallwiederherstellungskonfigurationen mit einer anderen Azure-Region als Ziel verwendet werden. 
- **Automatisches Hostfailover**: Eine lokale Wiederherstellungslösung für SAP HANA, die alternativ zur HANA-Systemreplikation verwendet werden kann. Wenn der Masterknoten nicht mehr verfügbar ist, konfigurieren Sie mindestens einen SAP HANA-Standbyknoten im Modus „Horizontal hochskalieren“, und SAP HANA führt automatisch ein Failover auf einen Standbyknoten durch.

SAP HANA in Azure (große Instanzen) wird in zwei Azure-Regionen in drei verschiedenen geopolitischen Regionen (USA, Australien und Europa) angeboten. Zwei unterschiedliche Regionen, die Stapel für HANA (große Instanzen) hosten, sind mit separaten dedizierten Netzwerkverbindungen verbunden, die zum Replizieren von Speichermomentaufnahmen verwendet werden, um Notfallwiederherstellungsmethoden bereitzustellen. Die Replikation ist standardmäßig nicht eingerichtet. Sie wird für Kunden eingerichtet, die die Notfallwiederherstellungsfunktion angefordert haben. Die Speicherreplikation setzt die Verwendung von Speichermomentaufnahmen für HANA (große Instanzen) voraus. Darüber hinaus ist es nicht möglich, eine Azure-Region in einer anderen geopolitischen Region als Notfallwiederherstellungsregion auszuwählen. 

Die folgende Tabelle gibt Aufschluss über die derzeit unterstützten Hochverfügbarkeits- und Notfallwiederherstellungsmethoden sowie über mögliche Kombinationen:

| In HANA (große Instanzen) unterstütztes Szenario | Hochverfügbarkeitsoption | Notfallwiederherstellungsoption | Kommentare |
| --- | --- | --- | --- |
| Einzelner Knoten | Nicht verfügbar. | Dedizierte Notfallwiederherstellungseinrichtung.<br /> Mehrzweck-Notfallwiederherstellungseinrichtung. | |
| Automatisches Hostfailover: N + M<br /> einschließlich 1 + 1 | Möglich durch Übernahme der aktiven Rolle durch den Standbyknoten.<br /> Steuerung des Rollenwechsels durch HANA. | Dedizierte Notfallwiederherstellungseinrichtung.<br /> Mehrzweck-Notfallwiederherstellungseinrichtung.<br /> Synchronisierung der Notfallwiederherstellung mithilfe der Speicherreplikation. | An alle Knoten (N + M) angefügte HANA-Volumesätze.<br /> Standort für die Notfallwiederherstellung muss über gleiche Anzahl von Knoten verfügen. |
| HANA-Systemreplikation | Möglich mit Primär-/Sekundäreinrichtung.<br /> Bei einem Failover übernimmt das sekundäre Replikat die Rolle des primären Replikats.<br /> Failoversteuerung durch HANA-Systemreplikation und Betriebssystem. | Dedizierte Notfallwiederherstellungseinrichtung.<br /> Mehrzweck-Notfallwiederherstellungseinrichtung.<br /> Synchronisierung der Notfallwiederherstellung mithilfe der Speicherreplikation.<br /> Die Notfallwiederherstellung mithilfe der HANA-Systemreplikation ist noch nicht ohne Drittanbieterkomponenten möglich. | Separater Satz angefügter Datenträgervolumes für die einzelnen Knoten.<br /> Nur Datenträgervolumes des sekundären Replikats am Produktionsstandort werden am Standort für die Notfallwiederherstellung repliziert.<br /> Am Standort für die Notfallwiederherstellung wird ein einzelner Volumesatz benötigt. | 

Eine dedizierte Notfallwiederherstellungseinrichtung ist eine Einrichtung, bei der die große HANA-Instanzeinheit am Standort für die Notfallwiederherstellung nicht zum Ausführen einer anderen Workload oder eines produktionsfremden Systems verwendet wird. Die Einheit ist passiv und wird nur dann bereitgestellt, wenn eine Notfallwiederherstellung ausgeführt wird. Bislang haben wir noch keinen Kunden mit dieser Konfiguration.

Eine Mehrzweck-Notfallwiederherstellungseinrichtung ist eine Einrichtung, bei der die große HANA-Instanzeinheit am Standort für die Notfallwiederherstellung eine produktionsfremde Workload ausführt. Bei einem Notfall fahren Sie das produktionsfremde System herunter, binden die speicherreplizierten (zusätzlichen) Volumesätze ein und starten dann die HANA-Produktionsinstanz. Bislang kommt bei allen Kunden, die die Notfallwiederherstellung von HANA (große Instanzen) nutzen, diese Konfigurationsalternative zum Einsatz. 


Weitere Informationen zur Hochverfügbarkeit von SAP HANA finden Sie in den folgenden SAP-Artikeln: 

- [Whitepaper zur Hochverfügbarkeit von SAP HANA](http://go.sap.com/documents/2016/05/f8e5eeba-737c-0010-82c7-eda71af511fa.html)
- [SAP HANA-Administratorhandbuch](http://help.sap.com/hana/SAP_HANA_Administration_Guide_en.pdf)
- [SAP Academy-Video zur SAP HANA-Systemreplikation](http://scn.sap.com/community/hana-in-memory/blog/2015/05/19/sap-hana-system-replication)
- [SAP Support Note #1999880 – FAQ on SAP HANA System Replication](https://bcs.wdf.sap.corp/sap/support/notes/1999880)
- [Hinweis zur SAP-Unterstützung #2165547 – Sichern und Wiederherstellen mit SAP HANA in einer SAP HANA-Systemreplikationsumgebung](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3231363535343726)
- [SAP Support Note #1984882 – Using SAP HANA System Replication for Hardware Exchange with Minimum/Zero Downtime](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3139383438383226)

## <a name="network-considerations-for-disaster-recovery-with-hana-large-instances"></a>Überlegungen zu Netzwerken für die Notfallwiederherstellung mit HANA (große Instanzen)

Um die Notfallwiederherstellung von HANA (große Instanzen) nutzen zu können, müssen Sie die Netzwerkkonnektivität zwischen den beiden Azure-Regionen einrichten. Sie benötigen eine Azure ExpressRoute-Verbindung von Ihrem lokalen Standort in Ihrer Azure-Hauptregion und eine andere Verbindung von Ihrem lokalen Standort mit der Region für die Notfallwiederherstellung. Diese Maßnahme eignet sich für Situationen, in denen ein Problem in einer Azure-Region samt MSEE-Standort (Microsoft Enterprise-Edgerouter) auftritt.

Als zweite Maßnahme können Sie alle virtuellen Azure-Netzwerke, die in einer der Regionen mit SAP HANA in Azure (große Instanzen) verbunden sind, mit einer ExpressRoute-Verbindung verbinden, die große HANA-Instanzen in der anderen Region verbindet. Mit dieser *Querverbindung* können Dienste, die in einem virtuellen Azure-Netzwerk in Region 1 ausgeführt werden, eine Verbindung mit Einheiten von HANA (große Instanzen) in Region 2 herstellen (und umgekehrt). Dies ist eine Maßnahme für den Fall, in dem nur einer der MSEE-Standorte, der mit Ihrem lokalen Standort über Azure verbunden ist, ausfällt.

Die folgende Grafik veranschaulicht eine robuste Konfiguration für die Notfallwiederherstellung:

![Optimale Konfiguration für die Notfallwiederherstellung](./media/hana-overview-high-availability-disaster-recovery/image1-optimal-configuration.png)



## <a name="other-requirements-when-you-use-hana-large-instances-storage-replication-for-disaster-recovery"></a>Weitere Anforderungen für die Verwendung der Speicherreplikation von HANA (große Instanzen) für die Notfallwiederherstellung

Für eine Notfallwiederherstellungseinrichtung mit HANA (große Instanzen) müssen zusätzlich folgende Anforderungen erfüllt sein:

- Es ist erforderlich, SKUs vom Typ „SAP HANA für Azure (große Instanzen)“ mit derselben Größe wie die Produktions-SKUs anzufordern und in der Region für die Notfallwiederherstellung bereitzustellen. In den gegenwärtigen Kundenbereitstellungen werden diese Instanzen zum Ausführen produktionsfremder HANA-Instanzen verwendet. Wir bezeichnen sie als *Mehrzweck-Notfallwiederherstellungseinrichtungen*.   
- Für jede SKU vom Typ „SAP HANA in Azure (große Instanzen)“, die Sie am Standort für die Notfallwiederherstellung wiederherstellen möchten, muss zusätzlicher Speicher am Standort für die Notfallwiederherstellung angefordert werden. Für die Zuordnung von Speichervolumes kann zusätzlicher Speicher erworben werden. Sie können Volumes zuordnen, die als Ziel für die Speicherreplikation aus Ihrer Azure-Produktionsregion in der Azure-Notfallwiederherstellungsregion fungieren.
 

## <a name="backup-and-restore"></a>Sichern und Wiederherstellen

Einer der wichtigsten Aspekte beim Betrieb von Datenbanken ist der Schutz dieser Datenbanken vor verschiedenen Katastrophenfällen. Diese Ereignisse können sowohl durch Naturkatastrophen als auch einfache Benutzerfehler verursacht werden.

Durch das Sichern einer Datenbank mit der Möglichkeit einer Wiederherstellung zu einem beliebigen Zeitpunkt (z.B. vor dem Zeitpunkt des Löschens kritischer Daten) kann ein Status wiederhergestellt werden, der möglichst genau dem Status vor der Störung entspricht.

Für optimale Ergebnisse müssen zwei Arten von Sicherungen erfolgen:

- Datenbanksicherungen: vollständig, inkrementell oder differenziell
- Transaktionsprotokollsicherungen

Zusätzlich zu vollständigen Datenbanksicherungen auf Anwendungsebene können Sie Sicherungen mit Speichermomentaufnahmen erstellen. Speichermomentaufnahmen sind jedoch kein Ersatz für Transaktionsprotokollsicherungen. Transaktionsprotokollsicherungen werden auch weiterhin für Point-in-Time-Datenbankwiederherstellungen sowie zum Leeren der Protokolle von Transaktionen, für die bereits ein Commit ausgeführt wurde, benötigt. Speichermomentaufnahmen können allerdings die Wiederherstellung durch schnelles Bereitstellen eines Rollforwardimages der Datenbank beschleunigen. 

Es gibt zwei Sicherungs- und Wiederherstellungsoptionen für SAP HANA in Azure (große Instanzen):

- Eigenständig. Nachdem genügend vorhandener Speicherplatz berechnet wurde, führen Sie vollständige Datenbank- und Protokollsicherungen mithilfe von Datenträger-Sicherungsmethoden durch. Sie können entweder direkt auf Volumes sichern, die an die HANA-Einheiten (große Instanz) angefügt sind, oder auf Netzwerkdateifreigaben (Network File Share, NFS), die auf einem virtuellen Azure-Computer (Virtual Machine, VM) eingerichtet sind. In letzterem Fall richten Kunden eine Linux-VM in Azure ein, fügen Azure Storage an die VM an und geben den Speicher über einen konfigurierten NFS-Server auf dieser VM frei. Wenn Sie die Sicherung mit Volumes durchführen, die direkt an Einheiten von HANA (große Instanzen) angefügt sind, müssen Sie die Sicherungen (nach Einrichtung einer Azure-VM, die NFS-Freigaben auf der Grundlage von Azure Storage exportiert) in ein Azure Storage-Konto kopieren. Alternativ können Sie einen Azure Backup-Tresor oder einen Azure Cold Storage verwenden. 

   Alternativ können die Sicherungen nach dem Kopieren in ein Azure Storage-Konto mithilfe eines Drittanbieter-Datenschutztools gespeichert werden. Die eigenständige Sicherungsoption kann auch bei Daten erforderlich sein, die zu Konformitäts- oder Auditzwecken über einen längeren Zeitraum gespeichert werden müssen. In allen Fällen werden die Sicherungen in NFS-Freigaben kopiert, die durch eine VM und Azure Storage dargestellt werden.

- Verwenden Sie die Sicherungs- und Wiederherstellungsfunktionalität, die die zugrunde liegende Infrastruktur von SAP HANA in Azure (große Instanzen) bereitstellt. Diese Option sorgt für die nötigen Sicherungen und ermöglicht schnelle Wiederherstellungen. Im restlichen Teil dieses Abschnitts werden die Sicherungs- und Wiederherstellungsfunktionen behandelt, die von HANA (große Instanzen) bereitgestellt werden. Dieser Abschnitt behandelt auch die Beziehung der Sicherung und Wiederherstellung zur Notfallwiederherstellungsfunktionalität von HANA (große Instanzen).

> [!NOTE]
> Die Momentaufnahmetechnologie, die von der zugrunde liegenden Infrastruktur von HANA (große Instanzen) verwendet wird, ist von SAP HANA-Momentaufnahmen abhängig. An dieser Stelle können SAP HANA-Momentaufnahmen nicht in Verbindung mit mehreren Mandanten von mehrinstanzenfähigen SAP HANA-Datenbankcontainern verwendet werden. Daher kann diese Sicherungsmethode nicht verwendet werden, wenn Sie mehrere Mandanten in mehrinstanzenfähigen SAP HANA-Datenbankcontainern bereitgestellt haben. SAP HANA-Momentaufnahmen funktionieren jedoch, wenn nur ein Mandant bereitgestellt wird.

### <a name="using-storage-snapshots-of-sap-hana-on-azure-large-instances"></a>Verwenden von Speicher-Snapshots von SAP HANA in Azure (große Instanzen)

Die zugrunde liegende Speicherinfrastruktur von SAP HANA in Azure (große Instanzen) unterstützt Speichermomentaufnahmen von Volumes. Sicherung und Wiederherstellung von Volumes werden unterstützt, dabei ist jedoch Folgendes zu beachten:

- Anstelle von vollständigen Datenbanksicherungen werden in regelmäßigen Abständen Speichervolume-Momentaufnahmen erstellt.
- Beim Auslösen einer Momentaufnahme für die Volumes „/hana/data“, „hana/log“ und „/hana/shared“ (einschließlich „/usr/sap“) initiiert die Speichermomentaufnahme vor dem Ausführen der Speichermomentaufnahme eine SAP HANA-Momentaufnahme. Diese SAP HANA-Momentaufnahme ist der Einrichtungspunkt für nachfolgende Wiederherstellungen von Protokollen, nachdem die Speichermomentaufnahme wiederhergestellt wurde.
- Nach erfolgreicher Erstellung der Speichermomentaufnahme wird die SAP HANA-Momentaufnahme gelöscht.
- Transaktionsprotokollsicherungen werden in regelmäßigen Abständen erstellt und auf dem Volume „/hana/logbackups“ oder in Azure gespeichert. Sie können für das Volume „/hana/logbackups“ mit den Transaktionsprotokollsicherungen eine separate Momentaufnahmeerstellung auslösen. In diesem Fall müssen Sie keine HANA-Momentaufnahme ausführen.
- Wenn Sie eine Datenbank zu einem bestimmten Zeitpunkt wiederherstellen müssen, fordern Sie beim Microsoft Azure-Support (bei einem Produktionsausfall) oder beim Dienstverwaltungsteam für SAP HANA in Azure die Wiederherstellung einer bestimmten Speichermomentaufnahme an. Ein Beispiel ist eine geplante Wiederherstellung eines Sandkastensystems in seinen ursprünglichen Zustand.
- Die in der Speichermomentaufnahme enthaltene SAP HANA-Momentaufnahme ist ein Ausgangspunkt für die Anwendung von Transaktionsprotokollsicherungen, die nach Erstellung der Speichermomentaufnahme ausgeführt und gespeichert wurden.
- Diese Transaktionsprotokollsicherungen werden erstellt, um eine Zeitpunktwiederherstellung der Datenbank zu ermöglichen.

Sie können Speichermomentaufnahmen für drei verschiedene Volumeklassen durchführen:

- Eine kombinierte Momentaufnahme für „/hana/data“, „/hana/log“ und „/hana/shared“ (einschließlich „/usr/sap“). Für diese Momentaufnahme ist die Ausführung einer SAP HANA-Momentaufnahme erforderlich.
- Eine separate Momentaufnahme für „/hana/logbackups“
- Eine Betriebssystempartition (nur für Typ I von HANA (große Instanzen))


### <a name="storage-snapshot-considerations"></a>Aspekte von Speichermomentaufnahmen

>[!NOTE]
>Speichermomentaufnahmen beanspruchen Speicherplatz, der den Einheiten von HANA (große Instanzen) zugeteilt wurde. Daher müssen Sie bei der Planung von Speichermomentaufnahmen und der Festlegung der Anzahl von Speichermomentaufnahmen die folgenden Aspekte berücksichtigen. 

Zur besonderen Funktionsweise von Speichermomentaufnahmen von SAP HANA in Azure (große Instanzen) gehört Folgendes:

- Eine individuelle Speichermomentaufnahme (zum Zeitpunkt der Erstellung) beansprucht nur wenig Speicherplatz.
- Wenn sich Dateninhalte und der Inhalt in SAP HANA-Datendateien auf dem Speichervolume ändern, muss die Momentaufnahme den ursprünglichen Blockinhalt speichern und die Daten ändern.
- Die Speichermomentaufnahme vergrößert sich infolgedessen. Je länger die Momentaufnahme vorhanden ist, desto größer wird die Speichermomentaufnahme.
- Je mehr Änderungen am SAP HANA-Datenbankvolume während der Lebensdauer einer Speichermomentaufnahme erfolgen, desto mehr Speicherplatz belegt die Speichermomentaufnahme.

SAP HANA in Azure (große Instanzen) bietet standardmäßig feste Volumegrößen für die SAP HANA-Daten- und Protokollvolumes. Die Erstellung von Momentaufnahmen dieser Volumes verringert Ihren Volumespeicherplatz. Sie müssen festlegen, wann geplante Speichermomentaufnahmen erstellt werden sollen. Darüber hinaus müssen Sie auch den Speicherplatzverbrauch der Speichervolumes überwachen und die Anzahl von Momentaufnahmen, die Sie speichern, verwalten. Sie können Speichermomentaufnahmen deaktivieren, wenn Sie Unmengen von Daten importieren oder andere wichtige Änderungen an der HANA-Datenbank vornehmen. 


Die folgenden Abschnitte enthalten Informationen zum Erstellen dieser Momentaufnahmen sowie allgemeine Empfehlungen:

- Obwohl die Hardware 255 Momentaufnahmen pro Volume unterstützen kann, wird dringend empfohlen, deutlich unter diesem Wert zu bleiben.
- Überwachen und verfolgen Sie den freien Speicherplatz, bevor Sie Speichermomentaufnahmen erstellen.
- Verringern Sie die Anzahl von Speichermomentaufnahmen basierend auf dem freien Speicherplatz. Sie können die Anzahl der gespeicherten Momentaufnahmen verringern oder die Volumes erweitern. Sie können zusätzlichen Speicher in 1-TB-Schritten anfordern.
- Deaktivieren Sie Speichermomentaufnahmen für das Volume „/hana/data“ bei Aktivitäten wie dem Verschieben von Daten in SAP HANA mit SAP-Plattformmigrationstools (R3load) oder dem Wiederherstellen von SAP HANA-Datenbanken aus Sicherungen. 
- Bei größeren Neuorganisationen von SAP HANA-Tabellen sollten Speichermomentaufnahmen nach Möglichkeit vermieden werden.
- Speichermomentaufnahmen sind eine Voraussetzung, um von den Notfallwiederherstellungsfunktionen von SAP HANA in Azure (große Instanzen) profitieren zu können.

### <a name="setting-up-storage-snapshots"></a>Einrichten von Speichermomentaufnahmen

Zum Einrichten von Speichermomentaufnahmen mit HANA (große Instanzen) sind folgende Schritte erforderlich:
1. Stellen Sie sicher, dass Perl im Linux-Betriebssystem auf dem Server mit HANA (große Instanzen) installiert ist.
2. Fügen Sie „/etc/ssh/ssh\_config“ die Zeile _MACs hmac-sha1_ hinzu.
3. Erstellen Sie ein SAP HANA-Sicherungsbenutzerkonto auf dem Masterknoten für jede SAP HANA-Instanz, die Sie ausführen (falls zutreffend).
4. Installieren Sie den SAP HANA HDB-Client auf allen Servern mit SAP HANA (große Instanzen).
5. Erstellen Sie auf dem ersten Server mit SAP HANA (große Instanzen) in jeder Region einen öffentlichen Schlüssel, um auf die zugrunde liegende Speicherinfrastruktur zuzugreifen, die die Erstellung von Momentaufnahmen steuert.
6. Kopieren Sie die Skripts und die Konfigurationsdatei von [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts) im Speicherort **hdbsql** der SAP HANA-Installation.
7. Ändern Sie bei Bedarf die Datei „HANABackupDetails.txt“ gemäß den jeweiligen Kundenvorgaben.

### <a name="step-1-install-the-sap-hana-hdb-client"></a>Schritt 1: Installieren des SAP HANA HDB-Clients

Das für SAP HANA in Azure (große Instanzen) installierte Linux-Betriebssystem enthält die Ordner und Skripts, die benötigt werden, um die SAP HANA-Speichermomentaufnahmen für die Sicherung und Notfallwiederherstellung auszuführen. Überprüfen Sie, ob auf [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts) neuere Versionen zur Verfügung stehen. Die neueste Skriptversion ist 2.0.
Sie müssen allerdings bei der Installation von SAP HANA den SAP HANA HDB-Client für die Einheiten von HANA (große Instanzen) installieren. (Microsoft installiert weder den HDB-Client noch SAP HANA.)

### <a name="step-2-change-the-etcsshsshconfig"></a>Schritt 2: Ändern von „/etc/ssh/ssh\_config“

Ändern Sie „/etc/ssh/ssh`/etc/ssh/ssh_config`config“ wie folgt durch Hinzufügen der Zeile _MACs hmac-sha1_:
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

Um den Zugriff auf die Speichermomentaufnahme-Schnittstellen Ihres HANA-Mandanten (große Instanzen) zu ermöglichen, müssen Sie eine Anmeldung einrichten, die auf einem öffentlichen Schlüssel basiert. Erstellen Sie in Ihrem Mandanten auf dem ersten Server mit SAP HANA in Azure (große Instanzen) einen öffentlichen Schlüssel, der für den Zugriff auf die Speicherinfrastruktur verwendet wird, um die Erstellung von Momentaufnahmen zu ermöglichen. Der öffentliche Schlüssel sorgt dafür, dass für die Anmeldung bei den Speichermomentaufnahme-Schnittstellen kein Kennwort erforderlich ist. Die Erstellung eines öffentlichen Schlüssels bedeutet auch, dass Sie keine Kennwortanmeldeinformationen verwalten müssen. Führen Sie unter Linux auf dem Server mit SAP HANA (große Instanzen) den folgenden Befehl aus, um den öffentlichen Schlüssel zu generieren:
```
  ssh-keygen –t dsa –b 1024
```
Der neue Speicherort ist **_/root/.ssh/id\_dsa.pub**. Geben Sie kein Kennwort ein, da dies sonst bei jeder Anmeldung erforderlich ist. Drücken Sie stattdessen zweimal die **EINGABETASTE**, um die Kennworteingabeaufforderung bei der Anmeldung zu entfernen.

Vergewissern Sie sich, dass der öffentliche Schlüssel wie erwartet korrigiert wurde, indem Sie zu den Ordnern unter **/root/.ssh/** navigieren und dann den `ls`-Befehl ausführen. Wenn der Schlüssel vorhanden ist, können Sie ihn mit dem folgenden Befehl kopieren:

![Der öffentliche Schlüssel wird durch Ausführen dieses Befehls kopiert.](./media/hana-overview-high-availability-disaster-recovery/image2-public-key.png)

Wenden Sie sich als Nächstes an das Dienstverwaltungsteam für SAP HANA in Azure, und geben Sie den öffentlichen Schlüssel an. Der Servicemitarbeiter verwendet den öffentlichen Schlüssel für die Registrierung in der zugrunde liegenden Speicherinfrastruktur, die für Ihren HANA-Mandanten (große Instanzen) eingerichtet ist.

### <a name="step-4-create-an-sap-hana-user-account"></a>Schritt 4: Erstellen eines SAP HANA-Benutzerkontos

Um die Erstellung von SAP HANA-Momentaufnahmen zu initiieren, müssen Sie in SAP HANA ein Benutzerkonto erstellen, das die Speichermomentaufnahmeskripts verwenden können. Erstellen Sie hierzu in SAP HANA Studio ein SAP HANA-Benutzerkonto. Dieses Konto benötigt die folgenden Berechtigungen: **Backup Admin** und **Catalog Read**. In diesem Beispiel lautet der Benutzername **SCADMIN**. Beim Namen des in HANA Studio erstellten Benutzerkontos wird die Groß-/Kleinschreibung beachtet. Stellen Sie sicher, dass für die Aufforderung des Benutzers zum Ändern des Kennworts bei der nächsten Anmeldung **Nein** ausgewählt ist.

![Erstellen eines Benutzers in HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image3-creating-user.png)

### <a name="step-5-authorize-the-sap-hana-user-account"></a>Schritt 5: Autorisieren des SAP HANA-Benutzerkontos

In diesem Schritt autorisieren Sie das von Ihnen erstellte SAP HANA-Benutzerkonto, damit die Skripts zur Laufzeit keine Kennwörter übermitteln müssen. Mit dem SAP HANA-Befehl `hdbuserstore` können Sie einen SAP HANA-Benutzerschlüssel erstellen, der auf mindestens einem SAP HANA-Knoten gespeichert wird. Der Benutzerschlüssel ermöglicht dem Benutzer den Zugriff auf SAP HANA ohne Kennwortverwaltung im Rahmen des Skriptprozesses. Der Skriptprozess wird weiter unten erläutert.

>[!IMPORTANT]
>Führen Sie den folgenden Befehl als `root` aus. Andernfalls funktioniert das Skript nicht richtig.

Geben Sie den Befehl `hdbuserstore` wie folgt ein:

![Eingeben des Befehls „hdbuserstore“](./media/hana-overview-high-availability-disaster-recovery/image4-hdbuserstore-command.png)

Im folgenden Beispiel lautet der Benutzer **SCADMIN01**, der Hostname **lhanad01** und die Instanznummer **01**:
```
hdbuserstore set SCADMIN01 lhanad01:30115 <backup username> <password>
```
Bei einer SAP HANA-Konfiguration für horizontales Hochskalieren müssen sämtliche Skripts über einen einzelnen Server verwaltet werden. In diesem Beispiel muss der SAP HANA-Schlüssel **SCADMIN01** für jeden Host so angepasst werden, dass widergespiegelt wird, welcher Host zum Schlüssel passt. Passen Sie das SAP HANA-Sicherungskonto mit der Instanznummer der HANA-Datenbank an. Der Schlüssel benötigt Administratorrechte auf dem Host, dem er zugewiesen ist. Der Sicherungsbenutzer für Konfigurationen für horizontales Hochskalieren benötigt Zugriffsrechte für alle SAP HANA-Instanzen. Wenn die drei Knoten für horizontales Hochskalieren **lhanad01**, **lhanad02** und **lhanad03** heißen, ergibt sich folgende Befehlsfolge:

```
hdbuserstore set SCADMIN01 lhanad01:30115 SCADMIN <password>
hdbuserstore set SCADMIN02 lhanad02:30215 SCADMIN <password>
hdbuserstore set SCADMIN03 lhanad03:30315 SCADMIN <password>
```

### <a name="step-6-get-the-snapshot-scripts-configure-the-snapshots-and-test-the-configuration-and-connectivity"></a>Schritt 6: Abrufen der Momentaufnahmeskripts, Konfigurieren der Momentaufnahmen und Testen der Konfiguration und Konnektivität

Laden Sie von [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts) die neueste Version der Skripts herunter. Kopieren Sie die heruntergeladenen Skripts und die Textdatei in das Arbeitsverzeichnis für **hdbsql**. Bei aktuellen HANA-Installationen lautet dieses Verzeichnis in etwa „/hana/shared/D01/exe/linuxx86\_64/hdb“. 
``` 
azure_hana_backup.pl 
azure_hana_replication_status.pl 
azure_hana_snapshot_details.pl 
azure_hana_snapshot_delete.pl 
testHANAConnection.pl 
testStorageSnapshotConnection.pl 
removeTestStorageSnapshot.pl 
HANABackupCustomerDetails.txt 
``` 


Zweck der einzelnen Skripts und Dateien ist Folgender:

- **azure\_hana\_backup.pl**: Planen Sie das Skript mit Cron, damit Speichermomentaufnahmen entweder für HANA-Daten-/Protokoll-/Freigabevolumes, für das Volume „/hana/logbackups“ oder für das Betriebssystem (für Typ I-SKUs großer HANA-Instanzen) ausgeführt werden.
- **azure\_hana\_replication\_status.pl**: Dieses Skript stellt grundlegende Replikationsstatusdetails des Produktionsstandorts für den Notfallwiederherstellungsstandort bereit. Das Skript dient zur Überwachung, um sicherzustellen, dass die Replikation stattfindet, und um die Größe der replizierten Elemente anzuzeigen. Darüber hinaus liefert es Anweisungen für den Fall, dass eine Replikation zu lange dauert oder der Link ausgefallen ist.
- **azure\_hana\_snapshot\_details.pl**: Dieses Skript liefert eine Liste mit grundlegenden Details zu allen Momentaufnahmen pro Volume, die in Ihrer Umgebung vorhanden sind. Das Skript kann auf dem primären Server oder in einer Servereinheit am Standort für die Notfallwiederherstellung ausgeführt werden. Das Skript enthält die folgenden Informationen, die nach den einzelnen Volumes mit Momentaufnahmen aufgeschlüsselt sind:
   * Größe der gesamten Momentaufnahmen in einem Volume
   * Jede Momentaufnahme in diesem Volume enthält die folgenden Details: 
      - Name der Momentaufnahme 
      - Erstellungszeitpunkt 
      - Größe der Momentaufnahme
      - Häufigkeit der Momentaufnahme
      - HANA-Sicherungs-ID im Zusammenhang mit dieser Momentaufnahme, sofern relevant
- **azure\_hana\_snapshot\_delete.pl**: Dieses Skript löscht eine Speichermomentaufnahme oder eine Gruppe von Momentaufnahmen. Sie können die SAP HANA-Sicherungs-ID in HANA Studio oder den Namen der Speichermomentaufnahme verwenden. Die Sicherungs-ID ist derzeit nur mit den Momentaufnahmen verknüpft, die für die HANA-Daten-/Protokoll-/Freigabevolumes erstellt werden. Ansonsten gilt: Wenn die Momentaufnahme-ID eingegeben wird, werden alle Momentaufnahmen gesucht, die der eingegebenen Momentaufnahme-ID entsprechen.  
- **testHANAConnection.pl**: Dieses Skript testet die Verbindung mit der SAP HANA-Instanz und wird zum Einrichten der Speichermomentaufnahmen benötigt.
- **testStorageSnapshotConnection.pl**: Dieses Skript hat zwei Aufgaben. Es stellt zum einen sicher, dass die Einheit von HANA (große Instanzen), die die Skripts ausführt, Zugriff auf den zugewiesenen virtuellen Speichercomputer und auf die Speichermomentaufnahmen-Schnittstelle Ihrer großen HANA-Instanzen hat. Zum anderen erstellt es eine temporäre Momentaufnahme der HANA-Instanz, die Sie testen. Dieses Skript muss für jede HANA-Instanz auf einem Server ausgeführt werden, um sicherzustellen, dass die Sicherungsskripts wie erwartet funktionieren.
- **removeTestStorageSnapshot.pl**: Dieses Skript löscht die Testmomentaufnahme, die mit dem Skript **testStorageSnapshotConnection.pl** erstellt wurde. 
- **HANABackupCustomerDetails.txt**: Bei dieser Datei handelt es sich um eine bearbeitbare Konfigurationsdatei, die Sie an Ihre SAP HANA-Konfiguration anpassen müssen.

 
Die Datei „HANABackupCustomerDetails.txt“ ist die Steuerungs- und Konfigurationsdatei für das Skript, das die Speichermomentaufnahmen erstellt. Passen Sie die Datei für Ihre Zwecke und Einrichtung an. Die Angaben für **Storage Backup Name** und **Storage IP Address** sollten Sie vom SAP HANA in Azure-Dienstverwaltungsteam erhalten haben, als Ihre Instanzen bereitgestellt wurden. Reihenfolge, Sortierung und Abstand der Variablen in dieser Datei dürfen nicht verändert werden. Andernfalls werden die Skripts nicht ordnungsgemäß ausgeführt. Darüber hinaus haben Sie vom Dienstverwaltungsteam für SAP HANA in Azure die IP-Adresse des Knotens für zentrales Hochskalieren oder die IP-Adresse des Masterknotens (bei horizontalem Hochskalieren) erhalten. Zudem ist Ihnen auch die Anzahl der HANA-Instanzen bekannt, die Sie während der Installation von SAP HANA erhalten haben. Nun müssen Sie einen Sicherungsnamen zur Konfigurationsdatei hinzufügen.

Bei einer Bereitstellung für zentrales oder horizontales Skalieren sieht die Konfigurationsdatei nach Angabe des Namens der Speichersicherung und der IP-Adresse des Speichers wie im folgenden Beispiel gezeigt aus. Außerdem müssen Sie in der Konfigurationsdatei die folgenden Daten eingeben:
- IP-Adresse eines einzelnen Knotens oder Masterknotens
- HANA-Instanznummer
- Sicherungsname 
    
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

Nachdem Sie alle Konfigurationsdaten in die Datei „HANABackupCustomerDetails.txt“ eingegeben haben, müssen Sie sich vergewissern, dass die Konfigurationen im Hinblick auf die HANA-Instanzdaten korrekt sind. Verwenden Sie das Skript `testHANAConnection.pl`. Für dieses Skript spielt es keine Rolle, ob es sich um eine SAP HANA-Konfiguration für zentrales oder horizontales Hochskalieren handelt.

```
testHANAConnection.pl
```

Bei einer SAP HANA-Konfiguration für horizontales Hochskalieren benötigt die HANA-Masterinstanz Zugriff auf alle erforderlichen HANA-Server und -Instanzen. Für das Testskript sind zwar keine Parameter verfügbar, Sie müssen jedoch Ihre Daten in die Konfigurationsdatei „HANABackupCustomerDetails.txt“ eingeben, damit das Skript ordnungsgemäß ausgeführt wird. Nur die Fehlercodes des Shellbefehls werden zurückgegeben. Daher ist es für das Skript nicht möglich, jede Instanz auf Fehler zu prüfen. Mit dem Skript werden trotzdem einige hilfreiche Kommentare bereitgestellt, die Sie überprüfen können.

Geben Sie den folgenden Befehl ein, um das Skript auszuführen:
```
 ./testHANAConnection.pl
```
Wenn das Skript den Status der HANA-Instanz erfolgreich abruft, wird eine Nachricht mit dem Hinweis angezeigt, dass die HANA-Verbindung hergestellt wurde.


Im nächsten Schritt wird anhand der Daten, die Sie in der Konfigurationsdatei „HANABackupCustomerDetails.txt“ angegeben haben, die Konnektivität mit dem Speicher überprüft und dann eine Testmomentaufnahme ausgeführt. Dieser Test muss vor der Ausführung des Skripts `azure_hana_backup.pl` ausgeführt werden. Falls ein Volume keine Momentaufnahmen enthält, kann nicht bestimmt werden, ob das Volume leer ist oder ob ein SSH-Fehler vorliegt und keine Momentaufnahmedetails abgerufen werden können. Aus diesem Grund werden mit dem Skript zwei Schritte ausgeführt:

- Es überprüft, ob die Skripts auf den virtuellen Speichercomputer des Mandanten und auf die Schnittstellen zugreifen können, um Momentaufnahmen auszuführen.
- Anhand der HANA-Instanz wird für jedes Volume ein Test, ein Dummy oder eine Momentaufnahme erstellt.

Aus diesem Grund ist die HANA-Instanz als ein Argument enthalten. Sollte die Ausführung nicht erfolgreich sein, kann keine Fehlerüberprüfung für die Speicherverbindung durchgeführt werden. Auch wenn keine Fehlerüberprüfung durchgeführt wird, bietet das Skript hilfreiche Hinweise.

Das Skript wird ausgeführt als:
```
 ./testStorageSnapshotConnection.pl <HANA SID>
```
Als Nächstes versucht das Skript, sich unter Verwendung des öffentlichen Schlüssels aus den vorherigen Einrichtungsschritten und der konfigurierten Daten aus der Datei „HANABackupCustomerDetails.txt“ beim Speicher anzumelden. Im Falle einer erfolgreichen Anmeldung wird Folgendes angezeigt:

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

Nach einer erfolgreichen Anmeldung bei den Schnittstellen der virtuellen Speichercomputer fährt das Skript mit Phase 2 fort und erstellt eine Testmomentaufnahme. Die folgende Ausgabe zeigt eine SAP HANA-Konfiguration mit drei Knoten für horizontales Hochskalieren:

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

Wenn die Testmomentaufnahme erfolgreich mit dem Skript erstellt wurde, können Sie mit dem Konfigurieren der eigentlichen Speichermomentaufnahmen fortfahren. Wenn die Anmeldung nicht erfolgreich ist, untersuchen Sie die Probleme, bevor Sie fortfahren. Bewahren Sie die Testmomentaufnahme, bis die ersten echten Momentaufnahmen fertig gestellt sind.


### <a name="step-7-perform-snapshots"></a>Schritt 7: Erstellen von Momentaufnahmen

Nach Abschluss der Vorbereitungsschritte können Sie damit beginnen, die eigentlichen Speichermomentaufnahmen zu konfigurieren. Das zu planende Skript kann für SAP HANA-Konfigurationen mit zentralem und horizontalem Hochskalieren verwendet werden. Die Planung der Skriptausführung sollte über Cron erfolgen. 

Es können drei Typen von Momentaufnahmesicherungen erstellt werden:
- **HANA**: Kombinierte Momentaufnahmesicherung, bei der die Volumes mit „/hana/data“, „/hana/log“ und „/hana/shared“ (einschließlich „/usr/sap“) durch die koordinierte Momentaufnahme abgedeckt sind. Auf der Grundlage dieser Momentaufnahme können einzelne Dateien wiederhergestellt werden.
- **Protokolle**: Momentaufnahmesicherung des Volumes „/hana/logbackups“. Zur Erstellung dieser Speichermomentaufnahme wird keine HANA-Momentaufnahme ausgelöst. Dieses Speichervolume ist das Volume, das die SAP HANA-Transaktionsprotokollsicherungen enthalten soll. SAP HANA-Transaktionsprotokollsicherungen werden häufiger ausgeführt, um die Protokollvergrößerung einzuschränken und potenzielle Datenverluste zu verhindern. Auf der Grundlage dieser Momentaufnahme können einzelne Dateien wiederhergestellt werden. Die Häufigkeit sollte mindestens fünf Minuten betragen.
- **Start**: Momentaufnahme des Volumes mit der Start-LUN (Logical Unit Number, logische Gerätenummer) der großen HANA-Instanz. Diese Momentaufnahmesicherung steht nur für Typ I-SKUs großer HANA-Instanzen zur Verfügung. Auf der Grundlage der Momentaufnahme des Volumes mit der Start-LUN können keine einzelnen Dateien wiederhergestellt werden.  


Die Aufrufsyntax für die drei unterschiedlichen Arten von Momentaufnahmen sieht wie folgt aus:
```
HANA backup covering /hana/data, /hana/log, /hana/shared (includes/usr/sap)
./azure_hana_backup.pl hana <HANA SID> manual 30

For /hana/logbackups snapshot
./azure_hana_backup.pl logs <HANA SID> manual 30

For snapshot of the volume storing the boot LUN
./azure_hana_backup.pl boot manual 30

```

Festzulegende Parameter:

- Der erste Parameter gibt die Art der Momentaufnahmesicherung an. Die zulässige Werte lauten **hana**, **logs** und **boot**. 
- Der zweite Wert ist die HANA-SID (beispielsweise „HM3“). Bei einer Sicherung des Startvolumes wird dieser Parameter nicht benötigt.
- Beim dritten Parameter handelt es sich um eine Momentaufnahme- oder Sicherungsbezeichnung für die Art der Momentaufnahme. Sie hat zwei Funktionen. Zum einen dient sie als Namensvorschlag, damit Sie wissen, worum es sich bei diesen Momentaufnahmen handelt. Zum anderen dient sie dazu, dem Skript „azure\_hana\_backup.pl“ die Ermittlung der Anzahl von Speichermomentaufnahmen zu ermöglichen, die unter dieser bestimmten Bezeichnung beibehalten werden. Wenn Sie zwei Speichermomentaufnahme-Sicherungen desselben Typs (z.B. **hana**) mit zwei unterschiedlichen Bezeichnungen planen und definieren, dass 30 Momentaufnahmen pro Sicherung beibehalten werden sollen, erhalten Sie letztlich 60 Speichermomentaufnahmen der betroffenen Volumes. 
- Der vierte Parameter definiert, wie viele Momentaufnahmen mit dem gleichen Momentaufnahmepräfix (Bezeichnung) gespeichert werden sollen – und damit indirekt die Aufbewahrung der Momentaufnahmen. Dieser Parameter ist für die geplante Ausführung über Cron wichtig. 

Beim horizontalen Hochskalieren führt das Skript einige zusätzliche Überprüfungen durch, um sicherzustellen, dass Sie auf alle HANA-Server zugreifen können. Das Skript überprüft außerdem, ob alle HANA-Instanzen den entsprechenden Status der Instanzen zurückgeben, bevor es eine SAP HANA-Momentaufnahme erstellt. Nach der SAP HANA-Momentaufnahme folgt eine Speichermomentaufnahme.

Bei Ausführung des Skripts `azure_hana_backup.pl` wird die Speichermomentaufnahme in den folgenden drei Phasen erstellt:

1. Ausführen einer SAP HANA-Momentaufnahme
2. Ausführen einer Speichermomentaufnahme
3. Entfernen der SAP HANA-Momentaufnahme, die vor der Ausführung der Speichermomentaufnahme erstellt wurde

Um das Skript auszuführen, rufen Sie es im ausführbaren HDB-Ordner auf, in den es kopiert wurde. 

Die Aufbewahrungsdauer richtet sich nach der Anzahl von Momentaufnahmen, die bei der Skriptausführung als Parameter übergeben wird (im obigen Beispiel: **30**). Der von Speichermomentaufnahmen abgedeckte Zeitraum ergibt sich also aus zwei Faktoren: dem Ausführungszeitraum und der Anzahl von Momentaufnahmen, die bei der Skriptausführung als Parameter angegeben wird. Wenn die Anzahl gespeicherter Momentaufnahmen die Anzahl übersteigt, die beim Skriptaufruf als Parameter angegeben wurde, wird vor der Erstellung einer neuen Momentaufnahme die älteste Speichermomentaufnahme mit der gleichen Bezeichnung (im obigen Fall: **manual**) gelöscht. Die Anzahl, die Sie als letzten Parameter des Aufrufs angeben, ist die Anzahl der beibehaltenen Momentaufnahmen, die Sie steuern können. Mit dieser Anzahl können Sie indirekt auch den von Momentaufnahmen beanspruchten Speicherplatz steuern. 

> [!NOTE]
>Sobald Sie die Bezeichnung ändern, beginnt der Zählvorgang erneut. Dementsprechend müssen Sie bei der Bezeichnung genau vorgehen, damit Ihre Momentaufnahmen nicht versehentlich gelöscht werden.

### <a name="snapshot-strategies"></a>Strategien für Momentaufnahmen
Die Momentaufnahmenhäufigkeit für die unterschiedlichen Typen hängt davon ab, ob Sie die Notfallwiederherstellung von HANA (große Instanzen) verwenden. Die Notfallwiederherstellungsfunktionalität von HANA (große Instanzen) hängt von Speichermomentaufnahmen ab. Für die Verwendung von Speichermomentaufnahmen sind eventuell besondere Empfehlungen in Bezug auf die Häufigkeit und Ausführungszeiträume der Speichermomentaufnahmen erforderlich. 

Bei den folgenden Überlegungen und Empfehlungen wird davon ausgegangen, dass Sie die Notfallwiederherstellung von HANA (große Instanzen) *nicht* verwenden. Stattdessen verwenden Sie die Speichermomentaufnahmen, um über Sicherungen zu verfügen und Zeitpunktwiederherstellungen für die letzten 30 Tage bereitstellen zu können. Angesichts der Einschränkungen hinsichtlich der Anzahl von Momentaufnahmen und des Speicherplatzes haben Kunden folgende Anforderungen berücksichtigt:

- Zeitraum für Zeitpunktwiederherstellung
- Belegter Speicherplatz
- RPO (Recovery Point Objective) und RTO (Recovery Time Objective) für eine potenzielle Notfallwiederherstellung
- Tatsächliche Ausführung vollständiger HANA-Datenbanksicherungen für Datenträger. Immer wenn eine vollständige Datenbanksicherung für Datenträger oder die Schnittstelle **backint** erfolgt, tritt bei der Ausführung von Speichermomentaufnahmen ein Fehler auf. Wenn Sie zusätzlich zu Speichermomentaufnahmen vollständige Datenbanksicherungen ausführen möchten, sollten Sie sicherstellen, dass die Ausführung von Speichermomentaufnahmen während dieses Zeitraums deaktiviert ist.
- Die Anzahl von Momentaufnahmen ist pro Volume auf 255 beschränkt.


Bei Kunden, die die Notfallwiederherstellung von HANA (große Instanzen) nicht verwenden, ist der Momentaufnahmezeitraum kürzer. In solchen Fällen führen Kunden die kombinierten Momentaufnahmen für „/hana/data“, „/hana/log“ und „/hana/shared“ (einschließlich „/usr/sap“) in einem Intervall von 12 oder 24 Stunden durch und speichern diese Momentaufnahmen, um einen ganzen Monat abzudecken. Gleiches gilt für die Momentaufnahmen des Protokollsicherungsvolumes. Im Gegensatz dazu werden SAP HANA-Transaktionsprotokollsicherungen für das Protokollsicherungsvolume in einem Intervall von fünf bis 15 Minuten ausgeführt.

Es wird empfohlen, geplante Speichermomentaufnahmen mithilfe von Cron durchzuführen. Außerdem wird empfohlen, dasselbe Skript für alle Sicherungs- und Notfallwiederherstellungsanforderungen zu verwenden. Sie müssen die Skripteingaben so ändern, dass sie den verschiedenen angeforderten Sicherungszeiten entsprechen. Diese Momentaufnahmen werden in Cron abhängig von ihrer Ausführungszeit jeweils unterschiedlich geplant: stündlich, alle 12 Stunden, täglich oder wöchentlich. 

Im Anschluss sehen Sie ein Beispiel für einen Cron-Zeitplan in „/etc/crontab“:
```
00 1-23 * * * ./azure_hana_backup.pl hana HM3 hourlyhana 46
10 00 * * *  ./azure_hana_backup.pl hana HM3 dailyhana 28
00,05,10,15,20,25,30,35,40,45,50,55 * * * *  Perform SAP HANA transaction log backup
22 12 * * *  ./azure_hana_backup.pl log HM3 dailylogback 28
30 00 * * *  ./azure_hana_backup.pl boot dailyboot 28
```
Das vorherige Beispiel enthält eine stündliche kombinierte Momentaufnahme, die die Volumes mit den Speicherorten „/hana/data“, „/hana/log“ und „/hana/shared“ (einschließlich „/usr/sap“) abdeckt. Diese Art von Momentaufnahme wird für eine schnellere Zeitpunktwiederherstellung der letzten zwei Tage verwendet. Darüber hinaus wird eine tägliche Momentaufnahme für diese Volumes erstellt. Somit verfügen Sie über stündliche Momentaufnahmen für einen Zeitraum von zwei Tagen sowie über tägliche Momentaufnahmen für einen Zeitraum von vier Wochen. Das Volume für die Transaktionsprotokollsicherung wird außerdem täglich gesichert. Diese Sicherungen werden ebenfalls vier Wochen aufbewahrt. In der dritten Zeile von „crontab“ sehen Sie, dass für die Sicherung des HANA-Transaktionsprotokolls ein Ausführungsintervall von fünf Minuten geplant ist. Die Startminuten der verschiedenen Cron-Aufträge zur Erstellung von Speichermomentaufnahmen sind gestaffelt, damit die Momentaufnahmen nicht alle gleichzeitig zu einem bestimmten Zeitpunkt ausgeführt werden. 

Im folgenden Beispiel führen Sie stündlich eine kombinierte Momentaufnahme durch, die die Volumes mit den Speicherorten „/hana/data“, „/hana/log“ und „/hana/shared“ (einschließlich „/usr/sap“) abdeckt. Diese Momentaufnahmen werden zwei Tage lang aufbewahrt. Die Momentaufnahmen der Volumes mit der Transaktionsprotokollsicherung werden alle fünf Minuten ausgeführt und vier Stunden lang aufbewahrt. Das Ausführungsintervall für die Sicherung der HANA-Transaktionsprotokolldatei ist wie zuvor auf fünf Minuten festgelegt. Die Momentaufnahme des Volumes mit der Transaktionsprotokollsicherung wird jeweils zwei Minuten nach dem Start der Transaktionsprotokollsicherung erstellt. Nach diesen zwei Minuten sollte die Sicherung des SAP HANA-Transaktionsprotokolls für gewöhnlich abgeschlossen sein. Das Volume mit der Start-LUN wird wie zuvor jeweils einmal pro Tag durch eine Speichermomentaufnahme gesichert und vier Wochen lang aufbewahrt.

```
10 0-23 * * * ./azure_hana_backup.pl hana HM3 hourlyhana 48
0,5,10,15,20,25,30,35,40,45,50,55 * * * *  Perform SAP HANA transaction log backup
2,7,12,17,22,27,32,37,42,47,52,57 * * * *  ./azure_hana_backup.pl log HM3 logback 48
30 00 * * *  ./azure_hana_backup.pl boot dailyboot 28
```

Die folgende Grafik veranschaulicht die Sequenzen des vorherigen Beispiels ausgenommen der Start-LUN:

![Beziehung zwischen Sicherungen und Momentaufnahmen](./media/hana-overview-high-availability-disaster-recovery/backup_snapshot.PNG)

SAP HANA führt regelmäßig Schreibvorgänge für das Volume „/hana/log“ aus, um die Datenbankänderungen, für die ein Commit ausgeführt wurde, zu dokumentieren. In regelmäßigen Abständen schreibt SAP HANA einen Sicherungspunkt auf das Volume „/hana/data“. Wie in „crontab“ angegeben wird alle fünf Minuten eine SAP HANA-Transaktionsprotokollsicherung ausgeführt. Aufgrund der Auslösung einer kombinierten Speichermomentaufnahme für die Volumes „/hana/data“, „/hana/log“ und „/hana/shared“ wird zudem stündlich eine SAP HANA-Momentaufnahme ausgeführt. Nach erfolgreicher Ausführung der HANA-Momentaufnahme wird die kombinierte Speichermomentaufnahme ausgeführt. Gemäß Angabe in „crontab“ wird alle fünf Minuten (etwa zwei Minuten nach der HANA-Transaktionsprotokollsicherung) die Speichermomentaufnahme des Volumes „/hana/logbackup“ ausgeführt.


>[!IMPORTANT]
> Die Verwendung von Speichermomentaufnahmen für SAP HANA-Sicherungen ist nur sinnvoll, wenn diese in Verbindung mit SAP HANA-Transaktionsprotokollsicherungen durchgeführt werden. Diese Transaktionsprotokollsicherungen müssen die Zeiträume zwischen den Speichermomentaufnahmen abdecken. 

Sie benötigen Folgendes, wenn Sie für Benutzer eine Zeitpunktwiederherstellung für einen Zeitraum von 30 Tagen zugesagt haben:

- In Extremfällen müssen Sie über die Möglichkeit verfügen, auf eine kombinierte Speichermomentaufnahme für „/hana/data“, „/hana/log“ und „/hana/shared“ zuzugreifen, die 30 Tage alt ist.
- Sie benötigen lückenlose Transaktionsprotokollsicherungen, die die Zeit zwischen den kombinierten Speichermomentaufnahmen abdecken. Die älteste Momentaufnahme des Volumes mit der Transaktionsprotokollsicherung muss also 30 Tage alt sein. Dies gilt nicht, wenn Sie die Transaktionsprotokollsicherungen in eine andere NFS-Freigabe in Azure Storage kopieren. In diesem Fall können Sie ältere Transaktionsprotokollsicherungen von dieser NFS-Freigabe abrufen.

Um die Vorteile der Speichermomentaufnahmen und letztendlich der Speicherreplikation von Transaktionsprotokollsicherungen nutzen zu können, müssen Sie den Speicherort ändern, in den SAP HANA die Transaktionsprotokollsicherungen schreibt. Sie können diese Änderung in HANA Studio vornehmen. SAP HANA sichert zwar automatisch vollständige Protokollsegmente, trotzdem sollten Sie ein deterministisches Protokollsicherungsintervall angeben. Dies empfiehlt sich insbesondere bei Verwendung der Notfallwiederherstellungsoption, da Sie in der Regel Protokollsicherungen mit einem deterministischen Intervall ausführen. Im folgenden Fall wurde ein 15-minütiges Protokollsicherungsintervall verwendet.

![Planen von SAP HANA-Sicherungsprotokollen in SAP HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image5-schedule-backup.png)

Sie können Sicherungen wählen, die häufiger als alle 15 Minuten erstellt werden. Dies ist häufig in Verbindung mit der Notfallwiederherstellung der Fall. Einige Kunden führen Transaktionsprotokollsicherungen alle fünf Minuten durch.  

Falls die Datenbank noch nie gesichert wurde, wird im letzten Schritt eine dateibasierte Datenbanksicherung durchgeführt, um einen einzelnen Sicherungseintrag zu erstellen, der im Sicherungskatalog vorhanden sein muss. Andernfalls kann SAP HANA Ihre angegebenen Protokollsicherungen nicht initiieren.

![Erstellen einer dateibasierten Sicherung zum Hinzufügen eines einzelnen Sicherungseintrags](./media/hana-overview-high-availability-disaster-recovery/image6-make-backup.png)


Nach erfolgreicher Ausführung der ersten Speichermomentaufnahmen können Sie die in Schritt 6 ausgeführte Testmomentaufnahme löschen. Führen Sie hierzu das Skript `removeTestStorageSnapshot.pl` aus:
```
./removeTestStorageSnapshot.pl <hana instance>
```

### <a name="monitoring-the-number-and-size-of-snapshots-on-the-disk-volume"></a>Überwachen der Anzahl und Größe von Momentaufnahmen auf dem Datenträgervolume

Sie können die Anzahl von Momentaufnahmen und deren Speicherbedarf auf einem bestimmten Speichervolume überwachen. Mit dem Befehl `ls` werden keine Momentaufnahmenverzeichnisse oder -dateien angezeigt. Der Befehl `du` des Linux-Betriebssystems liefert jedoch Details zu den Speichermomentaufnahmen, da sie auf den gleichen Volumes gespeichert sind. Für den Befehl stehen folgende Optionen zur Verfügung:

- `du –sh .snapshot`: Gibt die Summe aller Momentaufnahmen im Verzeichnis für die Momentaufnahmen an.
- `du –sh --max-depth=1`: Listet alle Momentaufnahmen, die im Ordner **.snapshot** gespeichert sind, und die Größe der einzelnen Momentaufnahmen auf.
- `du –hc`: Gibt die Gesamtgröße des Speicherplatzes an, der von allen Momentaufnahmen genutzt wird.

Verwenden Sie diese Befehle, um sicherzustellen, dass die erstellten und gespeicherten Momentaufnahmen nicht den gesamten Speicher auf den Volumes belegen.

>[!NOTE]
>Die Momentaufnahmen der Start-LUN werden bei Verwendung der vorherigen Befehle nicht angezeigt.

### <a name="getting-details-of-snapshots"></a>Abrufen von Details zu Momentaufnahmen
Um weitere Informationen zu Momentaufnahmen zu erhalten, können Sie auch das Skript `azure_hana_snapshot_details.pl` verwenden. Dieses Skript kann an beiden Standorten ausgeführt werden, sofern am Standort für die Notfallwiederherstellung ein aktiver Server vorhanden ist. Das Skript enthält die folgende Ausgabe, die nach den einzelnen Volumes mit Momentaufnahmen aufgeschlüsselt sind: 
   * Größe der gesamten Momentaufnahmen in einem Volume
   * Jede Momentaufnahme in diesem Volume enthält die folgenden Details: 
      - Name der Momentaufnahme 
      - Erstellungszeitpunkt 
      - Größe der Momentaufnahme
      - Häufigkeit der Momentaufnahme
      - HANA-Sicherungs-ID im Zusammenhang mit dieser Momentaufnahme, sofern relevant

Die Ausführungssyntax des Skripts sieht wie folgt aus:

```
./azure_hana_snapshot_details.pl 
```

Da das Skript versucht, die HANA-Sicherungs-ID abzurufen, muss es eine Verbindung mit der SAP HANA-Instanz herstellen. Für diese Verbindung muss die Konfigurationsdatei „HANABackupCustomerDetails.txt“ richtig festgelegt sein. Eine Ausgabe zweier Momentaufnahmen eines Volumes kann beispielsweise wie folgt aussehen:

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


### <a name="file-level-restore-from-a-storage-snapshot"></a>Wiederherstellung auf Dateiebene basierend auf einer Speichermomentaufnahme
Bei den Momentaufnahmetypen „hana“ und „logs“ können Sie direkt auf den Volumes im Verzeichnis **.snapshot** auf die Momentaufnahmen zugreifen. Dort finden Sie jeweils ein Unterverzeichnis für die einzelnen Momentaufnahmen. Die einzelnen Dateien aus der Momentaufnahme können in dem Zustand, den sie zum Zeitpunkt der Momentaufnahme hatten, aus dem Unterverzeichnis in die eigentliche Verzeichnisstruktur kopiert werden.

>[!NOTE]
>Bei Momentaufnahmen der Start-LUN können keine einzelnen Dateien wiederhergestellt werden. Das Verzeichnis **.snapshot** wird in der Start-LUN nicht verfügbar gemacht. 


### <a name="reducing-the-number-of-snapshots-on-a-server"></a>Verringern der Anzahl von Momentaufnahmen auf einem Server

Wie bereits beschrieben, können Sie die Anzahl von bestimmten Bezeichnungen von Momentaufnahmen reduzieren, die Sie speichern. Die beiden letzten Parameter des Befehls zum Initiieren einer Momentaufnahme sind die Bezeichnung und die Anzahl von Momentaufnahmen, die Sie beibehalten möchten.

```
./azure_hana_backup.pl hana HM3 hanadaily 30
```

Im obigen Beispiel lautet die Bezeichnung der Momentaufnahme **customer**, und die Anzahl zu speichernder Momentaufnahmen mit dieser Bezeichnung ist auf **30** festgelegt. Als Reaktion auf die Speicherplatzbelegung können Sie die Anzahl von gespeicherten Momentaufnahmen reduzieren. Die Anzahl von Momentaufnahmen kann ganz einfach auf beispielsweise 15 verringert werden, indem der letzte Parameter bei der Skriptausführung auf **15** festgelegt wird:

```
./azure_hana_backup.pl hana HM3 hanadaily 15
```

Wenn Sie das Skript mit dieser Einstellung ausführen, beträgt die Anzahl von Momentaufnahmen „15“ (einschließlich der neuen Speichermomentaufnahme). Die 15 neuesten Momentaufnahmen werden gespeichert, und die 15 älteren Momentaufnahmen werden gelöscht.

 >[!NOTE]
 > Die Anzahl von Momentaufnahmen verringert sich durch dieses Skript jedoch nur, wenn Momentaufnahmen vorhanden sind, die älter als eine Stunde sind. Mit dem Skript werden Momentaufnahmen, die weniger als eine Stunde alt sind, also nicht gelöscht. Diese Einschränkungen beziehen sich auf die bereitgestellten optionalen Funktionen für die Notfallwiederherstellung.

Falls Sie eine Gruppe von Momentaufnahmen mit einer bestimmten Sicherungsbezeichnung (in diesen Syntaxbeispielen: **hanadaily**) nicht mehr benötigen, können Sie das Skript mit **0** als zu speichernde Anzahl ausführen. Hierdurch werden alle Momentaufnahmen mit dieser Bezeichnung entfernt. Das Entfernen aller Momentaufnahmen kann sich aber auf die Funktionen für die Notfallwiederherstellung auswirken.

Alternativ können Sie bestimmte Momentaufnahmen auch mithilfe des Skripts `azure_hana_snapshot_delete.pl` löschen. Dieses Skript dient zum Löschen einer Momentaufnahme oder einer Gruppe von Momentaufnahmen – entweder anhand der HANA-Sicherungs-ID aus HANA Studio oder anhand des Namens der Momentaufnahme. Die Sicherungs-ID ist derzeit nur mit den Momentaufnahmen verknüpft, die für den Momentaufnahmetyp **hana** erstellt werden. Bei Momentaufnahmesicherungen vom Typ **logs** und **boot** wird keine SAP HANA-Momentaufnahme erstellt. Daher ist bei diesen Momentaufnahmen auch keine Sicherungs-ID vorhanden. Bei Eingabe des Namens der Momentaufnahme wird auf den verschiedenen Volumes nach allen Momentaufnahmen gesucht, die dem eingegebenen Namen entsprechen. Aufrufsyntax für das Skript:

```
./azure_hana_snapshot_delete.pl 

```

Führen Sie das Skript als Benutzer vom Typ **root** aus.

Wenn Sie eine Momentaufnahme auswählen, können Sie jede Momentaufnahme einzeln löschen. Zunächst geben Sie das Volume mit der Momentaufnahme an und anschließend den Namen der eigentlichen Momentaufnahme. Wenn die Momentaufnahme auf dem Volume vorhanden und über eine Stunde alt ist, wird sie gelöscht. Die Namen der Volumes und Momentaufnahmen können Sie durch Ausführen des Skripts `azure_hana_snapshot_details` ermitteln. 

>[!IMPORTANT]
>Daten, die ausschließlich in der gelöschten Momentaufnahme enthalten sind, sind nach Ausführung des Löschvorgangs unwiederbringlich verloren.

   

### <a name="recovering-to-the-most-recent-hana-snapshot"></a>Wiederherstellen der letzten HANA-Momentaufnahme

Bei einem Ausfall der Produktionsumgebung kann die Wiederherstellung auf der Grundlage einer Speichermomentaufnahme als Kundenvorfall über den Microsoft Azure-Support initiiert werden. Eine äußerst dringliche Angelegenheit stellt das Szenario dar, wenn Daten in einem Produktionssystem gelöscht wurden und die einzige Möglichkeit zum Wiedererlangen der Daten in der Wiederherstellung der Produktionsdatenbank besteht.

Eine Zeitpunktwiederherstellung kann dagegen eine niedrigere Dringlichkeit haben und Tage im Voraus geplant werden. Diese Wiederherstellung können Sie mit dem Dienstverwaltungsteam für SAP HANA in Azure planen, anstatt ein Problem mit hoher Priorität zu melden. Beispielsweise planen Sie eventuell die Durchführung eines Upgrades für die SAP-Software, indem Sie ein neues Erweiterungspaket anwenden. Sie müssen dann eine Momentaufnahme wiederherstellen, die den Zustand vor dem Upgrade mit dem Erweiterungspaket darstellt.

Vor dem Senden der Anforderung müssen Sie diese vorbereiten. Das Dienstverwaltungsteam für SAP HANA in Azure kann die Anforderung dann bearbeiten und die wiederhergestellten Volumes bereitstellen. Anschließend stellen Sie die HANA-Datenbank basierend auf den Momentaufnahmen wieder her. Sie bereiten sich wie folgt auf die Anforderung vor:

>[!NOTE]
>Ihre Benutzeroberfläche kann von den folgenden Screenshots abweichen. Dies richtet sich nach der von Ihnen verwendeten SAP HANA-Version.

1. Entscheiden Sie, welche Momentaufnahme wiederhergestellt werden soll. Falls nicht anders angegeben, wird nur das Volume „hana/data“ wiederhergestellt. 

2. Fahren Sie die HANA-Instanz herunter.

 ![Herunterfahren der HANA-Instanz](./media/hana-overview-high-availability-disaster-recovery/image7-shutdown-hana.png)

3. Heben Sie die Bereitstellung der Datenvolumes auf jedem Knoten der HANA-Datenbank auf. Wenn die Datenvolumes noch in das Betriebssystem eingebunden sind, tritt bei der Wiederherstellung der Momentaufnahme ein Fehler auf.
 ![Heben Sie die Bereitstellung der Datenvolumes auf jedem Knoten der HANA-Datenbank auf](./media/hana-overview-high-availability-disaster-recovery/image8-unmount-data-volumes.png)

4. Erstellen Sie eine Azure-Supportanfrage, um die Wiederherstellung einer bestimmten Momentaufnahme anzufordern.

 - Während der Wiederherstellung: Unter Umständen werden Sie vom Dienstverwaltungsteam für SAP HANA in Azure zu Koordinierungs-, Überprüfungs- und Bestätigungszwecken gebeten, an einer Telefonkonferenz teilzunehmen, um die Wiederherstellung der korrekten Speichermomentaufnahme sicherzustellen. 

 - Nach der Wiederherstellung: Sie werden von SAP HANA unter der Azure-Dienstverwaltung benachrichtigt, wenn die Speichermomentaufnahme wiederhergestellt wurde.

5. Stellen Sie nach Abschluss des Wiederherstellungsvorgangs alle Datenvolumes wieder bereit.

 ![Erneutes Bereitstellen aller Datenvolumes](./media/hana-overview-high-availability-disaster-recovery/image9-remount-data-volumes.png)

6. Wählen Sie die Wiederherstellungsoptionen in SAP HANA Studio aus, sofern diese nicht automatisch beim Wiederherstellen der Verbindung mit der HANA-Datenbank über SAP HANA Studio angezeigt werden. Das folgende Beispiel zeigt eine Wiederherstellung der letzten HANA-Momentaufnahme. In einer Speichermomentaufnahme ist eine HANA-Momentaufnahme eingebettet. Wenn Sie die letzte Speichermomentaufnahme wiederherstellen, sollte es sich dabei um die neueste HANA-Momentaufnahme handeln. (Wenn Sie eine ältere Speichermomentaufnahme wiederherstellen, müssen Sie die HANA-Momentaufnahme basierend auf dem Zeitpunkt der Erstellung der Speichermomentaufnahme bestimmen.)

 ![Auswählen von Wiederherstellungsoptionen in SAP HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image10-recover-options-a.png)

7. Wählen Sie **Recover the database to a specific data backup or storage snapshot** (Datenbank in eine bestimmte Datensicherung oder Speichermomentaufnahme wiederherstellen) aus.

 ![Fenster „Wiederherstellungstyp angeben“](./media/hana-overview-high-availability-disaster-recovery/image11-recover-options-b.png)

8. Wählen Sie **Specify backup without catalog** (Sicherung ohne Katalog angeben).

 ![Fenster „Sicherungsspeicherort angeben“](./media/hana-overview-high-availability-disaster-recovery/image12-recover-options-c.png)

9. Wählen Sie in der Liste **Destination Type** (Zieltyp) die Option **Snapshot** (Momentaufnahme).

 ![Fenster „Wiederherzustellende Sicherung angeben“](./media/hana-overview-high-availability-disaster-recovery/image13-recover-options-d.png)

10. Wählen Sie **Fertig stellen**, um den Wiederherstellungsprozess zu starten.

 ![Wählen Sie „Fertig stellen“, um den Wiederherstellungsprozess zu starten.](./media/hana-overview-high-availability-disaster-recovery/image14-recover-options-e.png)

11. Die HANA-Datenbank wird mithilfe der HANA-Momentaufnahme wiederhergestellt, die in der Speichermomentaufnahme enthalten ist.

 ![HANA-Datenbank wird über die HANA-Momentaufnahme wiederhergestellt](./media/hana-overview-high-availability-disaster-recovery/image15-recover-options-f.png)

### <a name="recovering-to-the-most-recent-state"></a>Wiederherstellen des letzten Zustands

Mit dem folgenden Prozess wird die HANA-Momentaufnahme wiederhergestellt, die in der Speichermomentaufnahme enthalten ist. Anschließend wird für die Transaktionsprotokollsicherungen der letzte Zustand der Datenbank wiederhergestellt, bevor die Speichermomentaufnahme wiederhergestellt wird.

>[!IMPORTANT]
>Stellen Sie sicher, dass Sie über eine vollständige und zusammenhängende Kette von Transaktionsprotokollsicherungen verfügen, bevor Sie fortfahren. Ohne diese Sicherungen können Sie den aktuellen Zustand der Datenbank nicht wiederherstellen.

1. Führen Sie die Schritte 1-6 unter [Wiederherstellen der letzten HANA-Momentaufnahme](#recovering-to-the-most-recent-hana-snapshot) durch.

2. Wählen Sie die Option **Recover the database to its most recent state** (Letzten Zustand der Datenbank wiederherstellen).

 ![Auswählen von „Recover the database to its most recent state“ (Letzten Zustand der Datenbank wiederherstellen)](./media/hana-overview-high-availability-disaster-recovery/image16-recover-database-a.png)

3. Geben Sie den Speicherort der letzten HANA-Protokollsicherungen an. Der Speicherort muss alle HANA-Transaktionsprotokollsicherungen aus der HANA-Momentaufnahme bis zum neuesten Zustand enthalten.

 ![Angeben des Speicherorts der letzten HANA-Protokollsicherungen](./media/hana-overview-high-availability-disaster-recovery/image17-recover-database-b.png)

4. Wählen Sie eine Sicherung als Grundlage für die Wiederherstellung der Datenbank aus. In unserem Beispiel ist die HANA-Momentaufnahme auf dem Screenshot die HANA-Momentaufnahme, die in der Speichermomentaufnahme enthalten war. 

 ![Auswählen einer Sicherung als Grundlage zum Wiederherstellen der Datenbank](./media/hana-overview-high-availability-disaster-recovery/image18-recover-database-c.png)

5. Deaktivieren Sie das Kontrollkästchen **Use Delta Backups** (Deltasicherungen verwenden), sofern diese nicht zwischen dem Zeitpunkt der HANA-Momentaufnahme und dem letzten Zustand vorhanden waren.

 ![Deaktivieren des Kontrollkästchens „Use Delta Backups“ (Deltasicherungen verwenden), falls keine Deltas vorhanden sind](./media/hana-overview-high-availability-disaster-recovery/image19-recover-database-d.png)

6. Wählen Sie auf dem Bildschirm mit der Zusammenfassung **Fertig stellen**, um den Wiederherstellungsvorgang zu starten.

 ![Klicken auf „Finish“ (Fertig stellen) auf dem Bildschirm mit der Zusammenfassung](./media/hana-overview-high-availability-disaster-recovery/image20-recover-database-e.png)

### <a name="recovering-to-another-point-in-time"></a>Wiederherstellen eines anderen Zeitpunkts
Gehen Sie zum Durchführen einer Zeitpunktwiederherstellung zwischen der HANA-Momentaufnahme (die in der Speichermomentaufnahme enthalten ist) und einer späteren Zeitpunktwiederherstellung der HANA-Momentaufnahme wie folgt vor:

1. Stellen Sie sicher, dass Sie über alle Transaktionsprotokollsicherungen in der HANA-Momentaufnahme bis zum Zeitpunkt verfügen, den Sie wiederherstellen möchten.
2. Beginnen Sie mit dem Verfahren unter [Wiederherstellen des letzten Zustands](#recovering-to-the-most-recent-state).
3. Wählen Sie in Schritt 2 des Verfahrens im Fenster **Wiederherstellungstyp angeben** die Option **Datenbank für den folgenden Zeitpunkt wiederherstellen**, und geben Sie den Zeitpunkt an. Führen Sie anschließend die Schritte 3-6 durch.

### <a name="monitoring-the-execution-of-snapshots"></a>Überwachen der Ausführung von Momentaufnahmen

Bei Verwendung von Speichermomentaufnahmen von HANA (große Instanzen) müssen Sie auch die Erstellung dieser Speichermomentaufnahmen überwachen. Das Skript, das eine Speichermomentaufnahme ausführt, schreibt die Ausgabe in eine Datei und speichert sie an demselben Speicherort wie die Perl-Skripts. Für jede Speichermomentaufnahme wird eine separate Datei geschrieben. Die Ausgabe jeder Datei zeigt eindeutig die verschiedenen Phasen, die vom Momentaufnahmenskript ausgeführt werden:

1. Suchen nach den Volumes, für die eine Momentaufnahme erstellt werden muss
2. Suchen nach den Momentaufnahmen, die basierend auf diesen Volumes erstellt wurden
3. Löschen von eventuell vorhandenen Momentaufnahmen entsprechend der von Ihnen angegebenen Anzahl von Momentaufnahmen
4. Erstellen einer SAP HANA-Momentaufnahme
5. Volumeübergreifendes Erstellen der Speichermomentaufnahme
6. Löschen der SAP HANA-Momentaufnahme
7. Umbenennen der neuesten Momentaufnahme in **.0**

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
Sie können anhand dieses Beispiels erkennen, wie das Skript die Erstellung der HANA-Momentaufnahme aufzeichnet. Bei horizontaler Skalierung wird dieser Prozess auf dem Masterknoten ausgelöst. Der Masterknoten initiiert die synchrone Erstellung der SAP HANA-Momentaufnahmen auf den einzelnen Workerknoten. Dann wird die Speichermomentaufnahme erstellt. Nach erfolgreicher Erstellung der Speichermomentaufnahmen wird die HANA-Momentaufnahme gelöscht. Das Löschen der HANA-Momentaufnahme wird basierend auf dem Masterknoten initiiert.


## <a name="disaster-recovery-principles"></a>Prinzipien für die Notfallwiederherstellung
Mit HANA (große Instanzen) bieten wir eine Notfallwiederherstellungsfunktion zwischen Stapeln für HANA (große Instanzen) in verschiedenen Azure-Regionen an. Wenn Sie beispielsweise Einheiten von HANA (große Instanzen) in der Azure-Region „USA, Westen“ bereitstellen, können Sie Einheiten von HANA (große Instanzen) in der Region „USA, Osten“ als Notfallwiederherstellungseinheiten verwenden. Die Notfallwiederherstellung wird wie bereits erwähnt nicht automatisch konfiguriert, da sie eine weitere kostenpflichtige Einheit von HANA (große Instanzen) in der Notfallwiederherstellungsregion erfordert. Die Notfallwiederherstellungseinrichtung kann mit zentralem und horizontalem Hochskalieren verwendet werden. 

In den bislang bereitgestellten Szenarien haben unsere Kunden die Einheit in der Notfallwiederherstellungsregion zur Ausführung produktionsfremder Systeme mit einer installierten HANA-Instanz verwendet. Die Einheit von HANA (große Instanzen) muss der gleichen SKU angehören wie die für die Produktion verwendete SKU. Die Datenträgerkonfiguration zwischen der Servereinheit in der Azure-Produktionsregion und der Region für die Notfallwiederherstellung sieht wie folgt aus:

![Konfiguration mit Notfallwiederherstellungseinrichtung aus Datenträgersicht](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_setup.PNG)

Wie in dieser Übersichtsgrafik dargestellt, müssen Sie anschließend eine zweite Gruppe von Datenträgervolumes anfordern. Die Zieldatenträgervolumes weisen dieselbe Größe wie die Produktionsvolumes für die Produktionsinstanz in den Notfallwiederherstellungseinheiten auf. Diese Datenträgervolumes sind mit der HANA-Servereinheit (große Instanz) am Standort der Notfallwiederherstellung verknüpft. Die folgenden Volumes werden von der Produktionsregion auf den Standort für die Notfallwiederherstellung repliziert:

- /hana/data
- /hana/logbackups 
- /hana/shared (einschließlich „/usr/sap“)

Das Volume „/hana/log“ wird nicht repliziert, da das SAP HANA-Transaktionsprotokoll für ein solches Wiederherstellungsverfahren basierend auf diesen Volumes nicht erforderlich ist. 

Die bereitgestellte Notfallwiederherstellungsfunktion basiert auf der Speicherreplikationsfunktion der Infrastruktur für HANA (große Instanzen). Die speicherseitig verwendete Funktion ist kein konstanter Datenstrom mit Änderungen, die asynchron repliziert werden, wenn Änderungen am Speichervolume auftreten. Stattdessen basiert der Mechanismus darauf, dass in regelmäßigen Abständen Momentaufnahmen dieser Volumes erstellt werden. Anschließend werden die Veränderungen zwischen einer bereits replizierten und einer neuen, noch nicht replizierten Momentaufnahme an Zieldatenträgervolumes am Standort für die Notfallwiederherstellung übertragen.  Diese Momentaufnahmen werden in den Volumes gespeichert und müssen im Falle einer Notfallwiederherstellung in diesen Volumes wiederhergestellt werden.  

Die erste Übertragung der gesamten Daten des Volumes sollte durchgeführt werden, bevor die Menge der Daten kleiner wird als die Veränderungen zwischen den Momentaufnahmen. Somit enthalten die Volumes am Standort für die Notfallwiederherstellung sämtliche Volumemomentaufnahmen, die am Produktionsstandort durchgeführt wurden. Dadurch können Sie mithilfe des Notfallwiederherstellungssystems letztendlich zu einem früheren Status zurückkehren, um verloren gegangene Daten ohne Rollback des Produktionssystems wiederherzustellen.

Wenn die HANA-Systemreplikation am Produktionsstandort als Hochverfügbarkeitsfunktion genutzt wird, werden nur die Volumes der Instanz zweiter Ebene (oder der Replikatinstanz) repliziert. Diese Konfiguration kann zu einer Verzögerung bei der Speicherreplikation am Standort für die Notfallwiederherstellung führen, wenn Sie die sekundäre Replikatservereinheit (Ebene 2) oder die SAP HANA-Instanz in dieser Einheit warten oder herunterfahren. 

>[!IMPORTANT]
>Genau wie bei der mehrschichtigen HANA-Systemreplikation wird durch Herunterfahren der HANA-Instanz (Ebene 2) oder der Servereinheit die Replikation am Standort für die Notfallwiederherstellung blockiert, wenn Sie die Notfallwiederherstellungsfunktion von HANA (große Instanzen) nutzen.


>[!NOTE]
>Die Speicherreplikationsfunktion von HANA (große Instanzen) spiegelt und repliziert Speichermomentaufnahmen. Wenn Sie also keine Speichermomentaufnahmen durchführen, wie in diesem Dokument im Abschnitt zur Sicherung erläutert, ist keine Replikation am Standort für die Notfallwiederherstellung möglich. Die Ausführung von Speichermomentaufnahmen ist eine Voraussetzung für die Speicherreplikation am Standort für die Notfallwiederherstellung.



## <a name="preparation-of-the-disaster-recovery-scenario"></a>Vorbereitung für das Notfallwiederherstellungsszenario
Es wird davon ausgegangen, dass in der Azure-Produktionsregion ein Produktionssystem in HANA (große Instanzen) ausgeführt wird. Im Rahmen der folgenden Dokumentation gehen wir davon aus, dass die SID des HANA-Systems „PRD“ lautet. Darüber hinaus wird davon ausgegangen, dass Sie über ein produktionsfremdes System verfügen, das auf großen HANA-Instanzen in der Azure-Region für die Notfallwiederherstellung ausgeführt wird. Im Rahmen der Dokumentation gehen wir davon aus, dass die SID „TST“ lautet. Die Konfiguration sieht daher wie folgt aus:

![Starten der Notfallwiederherstellungseinrichtung](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start1.PNG)

Wenn die Serverinstanz noch nicht mit dem zusätzlichen Speichervolumesatz angefordert wurde, fügt das Dienstverwaltungsteam für SAP HANA in Azure den zusätzlichen Volumesatz als Ziel für das Produktionsreplikat an die HANA-Einheit (große Instanz) an, auf der die TST HANA-Instanz ausgeführt wird. Zu diesem Zweck müssen Sie die SID Ihrer HANA-Produktionsinstanz angeben. Nachdem das Dienstverwaltungsteam für SAP HANA in Azure sichergestellt hat, dass diese Volumes angefügt wurden, müssen Sie die Volumes in die HANA-Einheit (große Instanz) einbinden.

![Nächster Schritt: Notfallwiederherstellungseinrichtung](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start2.PNG)

Als Nächstes installieren Sie in der Azure-Region für die Notfallwiederherstellung die zweite SAP HANA-Instanz auf der HANA-Einheit (große Instanz), in der Sie die HANA-Instanz „TST“ ausführen. Die neu installierte SAP HANA-Instanz muss dieselbe SID aufweisen. Die erstellten Benutzer müssen dieselbe UID und Gruppen-ID wie die Produktionsinstanz aufweisen. Wenn die Installation erfolgreich war, müssen Sie Folgendes tun:
- Beenden Sie die neu installierte SAP HANA-Instanz auf der HANA-Einheit (große Instanz) in der Azure-Region für die Notfallwiederherstellung.
- Heben Sie die Bereitstellung dieser PRD-Volumes auf, und wenden Sie sich an das Dienstverwaltungsteam für SAP HANA in Azure. Die Volumes können nicht mehr für die Einheit bereitgestellt werden, da nicht auf diese zugegriffen werden kann, wenn sie als Speicherreplikationsziel fungiert.  

![Schritt zur Notfallwiederherstellungseinrichtung vor der Einrichtung der Replikation](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start3.PNG)

Das Betriebsteam stellt die Replikationsbeziehung zwischen den PRD-Volumes in der Azure-Produktionsregion und den PRD-Volumes in der Azure-Region für die Notfallwiederherstellung her.

>[!IMPORTANT]
>Das Volume „/hana/log“ wird nicht repliziert, da es nicht erforderlich ist, die replizierte SAP HANA-Datenbank am Notfallwiederherstellungsstandort in einem konsistenten Zustand wiederherzustellen.

Der nächste Schritt besteht darin, den Sicherungszeitplan für Speichermomentaufnahmen einzurichten oder anzupassen, um Ihre RTO und RPO im Falle eines Notfalls zu ermitteln. Legen Sie zur Minimierung der Recovery Point Objective folgende Replikationsintervalle im HANA-Dienst (große Instanz) fest:
- Die durch die kombinierte Momentaufnahme abgedeckten Volumes (Momentaufnahmetyp **hana**) werden alle 15 Minuten an die entsprechenden Speichervolumeziele am Standort für die Notfallwiederherstellung repliziert.
- Das Volume für die Transaktionsprotokollsicherung (Momentaufnahmetyp **logs**) wird alle drei Minuten an die entsprechenden Speichervolumeziele am Standort für die Notfallwiederherstellung repliziert.

Richten Sie zur Minimierung der Recovery Point Objective Folgendes ein:
- Führen Sie alle 30 bis 60 Minuten eine Speichermomentaufnahme vom Typ **hana** (siehe „Schritt 7: Durchführen von Momentaufnahmen“) durch.
- Führen Sie alle fünf Minuten SAP HANA-Transaktionsprotokollsicherungen durch.
- Führen Sie alle fünf bis 15 Minuten Speichermomentaufnahmen vom Typ **logs** durch. Mit diesem Intervall sollte eine RPO von etwa 15 bis 25 Minuten möglich sein.

Die folgende Grafik zeigt eine mögliche Sequenz der Transaktionsprotokollsicherungen und Speichermomentaufnahmen sowie der Replikation des Volumes mit der HANA-Transaktionsprotokollsicherung und von „/hana/data“ und „/hana/shared“ (einschließlich „/usr/sap“) bei Verwendung dieser Einstellungen:

 ![Beziehung zwischen Momentaufnahme der Transaktionsprotokollsicherung und Momentaufnahmespiegelung im Zeitverlauf](./media/hana-overview-high-availability-disaster-recovery/snapmirror.PNG)

Um bei der Notfallwiederherstellung eine noch bessere RPO zu erzielen, können Sie die HANA-Transaktionsprotokollsicherungen aus SAP HANA in Azure (große Instanzen) in die andere Azure-Region kopieren. Zur weiteren Senkung der RPO müssen folgende Schritte ausgeführt werden:

1. Sichern Sie das HANA-Transaktionsprotokoll so häufig wie möglich unter „/hana/logbackups“.
2. Kopieren Sie mithilfe von rsync die Transaktionsprotokollsicherungen auf die virtuellen Azure-Computer, die mit der NFS-Freigabe gehostet werden. Die VMs befinden sich in virtuellen Azure-Netzwerken in der Azure-Produktionsregion und den Regionen für die Notfallwiederherstellung. Sie müssen beide virtuelle Azure-Netzwerke mit den großen HANA-Produktionsinstanzen in Azure verbinden. Weitere Informationen finden Sie in der Grafik im Abschnitt [Überlegungen zu Netzwerken für die Notfallwiederherstellung mit HANA (große Instanzen)](#Network-considerations-for-disaster-recovery-with-HANA-Large-Instances). 
3. Belassen Sie die Transaktionsprotokollsicherungen in der Region auf der VM, die an den exportierten NFS-Speicher angefügt ist.
4. Ergänzen Sie bei der Notfallwiederherstellung die Transaktionsprotokollsicherungen aus dem Volume „/hana/logbackups“ mit neueren Transaktionsprotokollsicherungen aus der NFS-Freigabe am Standort für die Notfallwiederherstellung. 
5. Nun können Sie eine Wiederherstellung der Transaktionsprotokollsicherung auf die neueste Sicherung starten, die in der Region für die Notfallwiederherstellung gespeichert werden konnte.

Sobald das Betriebsteam für HANA (große Instanz) sichergestellt hat, dass die Replikationsbeziehung eingerichtet ist, und Sie die Speichermomentaufnahmesicherungen für die Ausführung starten, werden die Daten repliziert.

![Schritt zur Notfallwiederherstellungseinrichtung vor der Einrichtung der Replikation](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start4.PNG)

Bei der Replikation werden die Momentaufnahmen nicht auf den PRD-Volumes in den Azure-Regionen für die Notfallwiederherstellung wiederhergestellt. Sie werden lediglich gespeichert. Wenn die Volumes in solch einem Zustand bereitgestellt werden, stellen sie den Status dar, in dem Sie die Bereitstellung dieser Volumes aufgehoben haben, nachdem die PRD SAP HANA-Instanz in der Servereinheit in der Azure-Region für die Notfallwiederherstellung installiert wurde. Darüber hinaus bezeichnen sie die Speichersicherungen, die noch nicht wiederhergestellt wurden.

Bei einem Failover können Sie anstelle der neuesten Speichermomentaufnahme auch eine ältere Speichermomentaufnahme wiederherstellen.

## <a name="disaster-recovery-failover-procedure"></a>Verfahren zur Notfallwiederherstellung
Bei einem Failover auf den Standort für die Notfallwiederherstellung müssen Sie mit dem Betriebsteam von SAP HANA in Azure interagieren. Der Prozess sieht im Groben wie folgt aus:

1. Da Sie mit der Notfallwiederherstellungseinheit von HANA (große Instanzen) eine produktionsfremde Instanz von HANA ausführen, müssen Sie diese Instanz herunterfahren. Wir gehen davon aus, dass eine ruhende HANA Produktionsinstanz vorinstalliert ist.
2. Stellen Sie sicher, dass keine SAP HANA-Prozesse ausgeführt werden. Führen Sie für diese Überprüfung den folgenden Befehl aus: `/usr/sap/hostctrl/exe/sapcontrol –nr <HANA instance number> - function GetProcessList`. In der Ausgabe muss der Prozess **hdbdaemon** den Zustand „Beendet“ aufweisen, und es dürfen keine anderen HANA-Prozesse mit dem Zustand „Gestartet“ vorhanden sein.
3. Überprüfen Sie, unter welchen Momentaufnahmenamen bzw. welcher SAP HANA-Sicherungs-ID der Standort für die Notfallwiederherstellung wiederhergestellt werden soll. In der Praxis handelt es sich dabei in der Regel um die neueste Momentaufnahme. Wenn Sie verlorene Daten wiederherstellen müssen, wählen Sie eine ältere Momentaufnahme.
4. Wenden Sie sich über eine Supportanfrage mit hoher Priorität an den Azure-Support, und fordern Sie die Wiederherstellung der Momentaufnahme (Name und Datum der Momentaufnahme) oder HANA-Sicherungs-ID am Standort für die Notfallwiederherstellung an. Standardmäßig stellt das Betriebsteam lediglich das Volume „/hana/data“ her. Wenn auch die Volumes „/hana/logbackups“ wiederhergestellt werden sollen, müssen Sie dies gesondert angeben. *Es wird davon abgeraten, das Volume „/hana/shared“ wiederherzustellen.* Stattdessen sollten Sie bestimmte Dateien wie etwa „global.ini“ aus dem Verzeichnis **.snapshot** und den zugehörigen Unterverzeichnissen auswählen, nachdem Sie erneut das Volume „/hana/shared“ für das PRD-Volume bereitgestellt haben. Auf der Betriebsseite werden folgende Schritte durchgeführt: a. Die Replikation von Momentaufnahmen aus dem Produktionsvolume auf den Notfallwiederherstellungsvolumes wird beendet. Dies könnte bereits der Fall sein, wenn ein Ausfall am Produktionsstandort der Grund dafür ist, dass Sie eine Notfallwiederherstellung durchführen müssen.
    b. Der Name der Speichermomentaufnahme oder die Momentaufnahme mit der Sicherungs-ID, den bzw. die Sie auswählen, wird auf den Notfallwiederherstellungsvolumes wiederhergestellt.
    c. Nach der Wiederherstellung können die Notfallwiederherstellungsvolumes in die Einheiten von HANA (große Instanzen) in der Notfallwiederherstellungsregion eingebunden werden.
5. Binden Sie die Notfallwiederherstellungsvolumes in die Einheit von HANA (große Instanzen) am Standort für die Notfallwiederherstellung ein. 
6. Starten Sie die bislang ruhende SAP HANA-Produktionsinstanz.
7. Wenn Sie zur Verringerung der RPO-Zeit zusätzlich Protokolle der Transaktionsprotokollsicherung kopiert haben, müssen Sie diese Transaktionsprotokollsicherungen mit dem neu eingebundenen Notfallwiederherstellungsverzeichnis „hana/logbackups“ zusammenführen. Überschreiben Sie keine vorhandenen Sicherungen. Kopieren Sie einfach neuere Sicherungen, die nicht mit der neuesten Replikation einer Speichermomentaufnahme repliziert wurden.
8. Sie können auch einzelne Dateien über die Momentaufnahme wiederherstellen, die auf dem Volume „/hana/shared/PRD“ in der Azure-Region für die Notfallwiederherstellung repliziert wurden.

Die nächsten Schritte dienen zur Wiederherstellung der SAP HANA-Produktionsinstanz auf der Grundlage der wiederhergestellten Speichermomentaufnahme und der verfügbaren Transaktionsprotokollsicherungen. Hierzu sind folgende Schritte erforderlich:

1. Ändern Sie mithilfe von SAP HANA Studio das Sicherungsverzeichnis in **/hana/logbackups**.
   ![Ändern des Sicherungsspeicherorts für die Notfallwiederherstellung](./media/hana-overview-high-availability-disaster-recovery/change_backup_location_dr1.png)

2. SAP HANA überprüft alle Sicherungsdateispeicherorte und schlägt für die Wiederherstellung die neueste Transaktionsprotokollsicherung vor. Die Überprüfung kann einige Minuten dauern. Danach wird ein Bildschirm wie der folgende angezeigt: ![Liste mit Transaktionsprotokollsicherungen für die Notfallwiederherstellung](./media/hana-overview-high-availability-disaster-recovery/backup_list_dr2.PNG).

3. Passen Sie einige Standardeinstellungen an:

      - Deaktivieren Sie **Deltasicherungen verwenden**.
      - Wählen Sie **Protokollbereich initialisieren**.

   ![Aktivieren der Option „Protokollbereich initialisieren“](./media/hana-overview-high-availability-disaster-recovery/initialize_log_dr3.PNG)

4. Wählen Sie **Fertig stellen** aus.

   ![Fertigstellen der Notfallwiederherstellung](./media/hana-overview-high-availability-disaster-recovery/finish_dr4.PNG)

Daraufhin sollte ein Statusfenster wie das folgende erscheinen. Beachten Sie, dass es sich bei diesem Beispiel um eine Notfallwiederherstellung einer SAP HANA-Konfiguration mit drei Knoten für horizontales Hochskalieren handelt.

![Status der Wiederherstellung](./media/hana-overview-high-availability-disaster-recovery/restore_progress_dr5.PNG)

Falls die Wiederherstellung auf dem Bildschirm **Fertig stellen** anscheinend nicht fortgesetzt und der Statusbildschirm nicht angezeigt wird, überprüfen Sie, ob alle SAP HANA-Instanzen auf den Workerknoten ausgeführt werden. Starten Sie die SAP HANA-Instanzen bei Bedarf manuell.


### <a name="failback-from-dr-to-a-production-site"></a>Failback vom Standort für die Notfallwiederherstellung auf den Produktionsstandort
Sie können ein Failback von einem Notfallwiederherstellungsstandort auf einen Produktionsstandort durchführen. Gehen wir davon aus, dass das Failover auf den Standort für die Notfallwiederherstellung auf Probleme in der Azure-Produktionsregion zurückzuführen ist (und nicht darauf, dass Sie verloren gegangene Daten wiederherstellen müssen). Dies bedeutet, dass Sie Ihre SAP-Produktionsworkload für einen gewissen Zeitraum am Standort für die Notfallwiederherstellung ausgeführt haben. Nachdem die Probleme am Produktionsstandort behoben wurden, sollten Sie ein Failback auf Ihren Produktionsstandort durchführen. Da hierbei keine Daten verloren gehen dürfen, umfasst der Wechsel zum Produktionsstandort mehrere Schritte und eine enge Zusammenarbeit mit dem Betriebsteam für SAP HANA in Azure. Nach der Behebung der Probleme müssen Sie sich mit dem Betriebsteam in Verbindung setzen, um die Rücksynchronisierung mit dem Produktionsstandort anhand eines Triggers zu initiieren.

Die Abfolge der Schritte sieht folgendermaßen aus:

1. Das Betriebsteam für SAP HANA in Azure ruft den Trigger zum Synchronisieren der Produktionsspeichervolumes aus den Speichervolumes für die Notfallwiederherstellung ab, die jetzt den Produktionszustand darstellen. In diesem Zustand wird die Einheit von HANA (große Instanzen) am Produktionsstandort heruntergefahren.
2. Das Betriebsteam für SAP HANA in Azure überwacht die Replikation und stellt sicher, dass die Daten auf dem neuesten Stand sind, bevor Sie (der Kunde) informiert werden.
3. Fahren Sie die Anwendungen herunter, die die HANA-Produktionsinstanz am Standort für die Notfallwiederherstellung verwenden. Führen Sie dann eine HANA- Transaktionsprotokollsicherung durch. Beenden Sie anschließend die HANA-Instanz, die am Standort für die Notfallwiederherstellung in Einheiten von HANA (große Instanzen) ausgeführt wird.
4. Nach dem Herunterfahren der HANA-Instanz, die am Standort für die Notfallwiederherstellung in der Einheit von HANA (große Instanzen) ausgeführt wurde, synchronisiert das Betriebsteam erneut manuell die Datenträgervolumes.
5. Das Betriebsteam für SAP HANA in Azure startet die Einheit von HANA (große Instanzen) am Produktionsstandort wieder und übergibt sie an Sie. Vergewissern Sie sich, dass sich die SAP HANA-Instanz beim Start der Einheit von HANA (große Instanzen) im Zustand „Heruntergefahren“ befindet.
6. Führen Sie die gleichen Datenbankwiederherstellungsschritte durch wie zuvor beim Failover auf den Standort für die Notfallwiederherstellung.

### <a name="monitoring-disaster-recovery-replication"></a>Überwachen der Replikation für die Notfallwiederherstellung

Sie können den Status der Speicherreplikation mithilfe des Skripts `azure_hana_replication_status.pl` überwachen. Dieses Skript muss über eine Einheit ausgeführt werden, die am Standort für die Notfallwiederherstellung ausgeführt wird. Andernfalls funktioniert es nicht ordnungsgemäß. Für die Ausführung des Skripts spielt es keine Rolle, ob die Replikation aktiv ist. Das Skript kann für jede HANA-Einheit (große Instanzen) Ihres Mandanten am Standort für die Notfallwiederherstellung ausgeführt werden. Es kann nicht verwendet werden, um Details zum Startvolume abzurufen.

Rufen Sie das Skript wie folgt auf:
```
./replication_status.pl <HANA SID>
```

Die Ausgabe wird nach Volume in folgende Abschnitte unterteilt:  

- Linkstatus
- Aktuelle Replikationsaktivität
- Neueste replizierte Momentaufnahme 
- Größe der neuesten Momentaufnahme
- Aktueller Zeitabstand zwischen Momentaufnahmen (der zuletzt abgeschlossenen und der aktuellen Momentaufnahmereplikation)  

Der Linkstatus lautet **Aktiv** – es sei denn, die Verbindung zwischen Standorten ist unterbrochen oder es findet gerade ein Failoverereignis statt. Die Replikationsaktivität gibt Aufschluss darüber, ob momentan Daten repliziert werden, ob sich die Aktivität im Leerlauf befindet oder ob gerade andere Aktivitäten für den Link stattfinden. Für die zuletzt replizierte Momentaufnahme sollte nur `snapmirror…` angezeigt werden. Anschließend folgt die Größe der letzten Momentaufnahme. Zuletzt wird der Zeitabstand angezeigt. Der Zeitabstand ist die Zeit zwischen der geplanten Replikationszeit und dem Abschluss der Replikation. Der Wert für den Zeitabstand kann bei der Datenreplikation mehr als eine Stunde betragen, obwohl die Replikation gestartet wurde. Dies gilt insbesondere bei der ersten Replikation. Der Zeitabstand nimmt bis zum Abschluss der laufenden Replikation immer weiter zu.

Eine Ausgabe kann beispielsweise etwa wie folgt aussehen:

```
hana_data_hm3_mnt00002_t020_dp
-------------------------------------------------
Link Status: Broken-Off
Current Replication Activity: Idle
Latest Snapshot Replicated: snapmirror.c169b434-75c0-11e6-9903-00a098a13ceb_2154095454.2017-04-21_051515
Size of Latest Snapshot Replicated: 244KB
Current Lag Time between snapshots: -   ***Less than 90 minutes is acceptable***
```













