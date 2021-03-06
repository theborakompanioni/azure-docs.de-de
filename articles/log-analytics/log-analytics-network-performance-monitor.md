---
title: "Netzwerkleistungsmonitor-Lösung in Azure Log Analytics | Microsoft-Dokumentation"
description: "Mit dem Netzwerkleistungsmonitor in Azure Log Analytics können Sie die Leistung Ihrer Netzwerke quasi in Echtzeit überwachen, um Leistungsengpässe im Netzwerk zu erkennen und zu lokalisieren."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: banders
ms.translationtype: HT
ms.sourcegitcommit: 80fd9ee9b9de5c7547b9f840ac78a60d52153a5a
ms.openlocfilehash: c6568e491429f6046ab164ab5eacd0ae5846e201
ms.contentlocale: de-de
ms.lasthandoff: 08/14/2017

---
# <a name="network-performance-monitor-solution-in-log-analytics"></a>Netzwerkleistungsmonitor-Lösung in Azure Log Analytics

![Symbol des Netzwerkleistungsmonitors](./media/log-analytics-network-performance-monitor/npm-symbol.png)

In diesem Dokument wird das Einrichten und Verwenden der Netzwerkleistungsmonitor-Lösung in Log Analytics beschrieben, mit der Sie die Leistung Ihrer Netzwerke quasi in Echtzeit überwachen und Leistungsengpässe im Netzwerk erkennen und lokalisieren können. Mit der Netzwerkleistungsmonitor-Lösung können Sie den Paketverlust und die Latenz zwischen zwei Netzwerken, Subnetzen oder Servern überwachen. Der Netzwerkleistungsmonitor erkennt Netzwerkprobleme wie ins Nichts führenden Datenverkehr (Blackholing), Routingfehler und Probleme, die mit herkömmlichen Netzwerküberwachungsmethoden nicht erkannt werden können. Der Netzwerkleistungsmonitor generiert Warnungen und gibt eine Benachrichtigung aus, sobald ein Schwellenwert für eine Netzwerkverbindung überschritten wird. Diese Schwellenwerte können automatisch vom System erlernt werden, oder Sie können die Werte so konfigurieren, dass benutzerdefinierte Warnungsregeln verwendet werden. Der Netzwerkleistungsmonitor gewährleistet das rechtzeitige Erkennen von Leistungsproblemen im Netzwerk und ordnet die Ursache des Problems einem bestimmten Netzwerksegment oder Gerät zu.

Sie können Netzwerkprobleme mit dem Lösungsdashboard ermitteln, auf dem zusammengefasste Informationen zu Ihrem Netzwerk angezeigt werden, einschließlich aktueller Netzwerkintegritätsereignisse, fehlerhafter Netzwerkverbindungen und Subnetzwerkverbindungen, bei denen es zu hohen Paketverlusten und Latenzen kommt. Sie können einen Drilldown in eine Netzwerkverbindung ausführen, um den aktuellen Integritätsstatus von Subnetzwerkverbindungen und von Knoten-zu-Knoten-Verbindungen zu prüfen. Sie können außerdem den historischen Trend der Verluste und der Latenz auf Netzwerk-, Subnetzwerk- und Knoten-zu-Knoten-Ebene anzeigen. Sie können vorübergehende Netzwerkprobleme erkennen, indem Sie sich historische Trenddiagramme zu Paketverlusten und zur Latenz anzeigen lassen und Netzwerkengpässe in einer Topologiekarte lokalisieren. Mit dem interaktiven Topologiediagramm können Sie die Hop-by-Hop-Netzwerkrouten visualisieren und die Ursache des Problems ermitteln. Wie bei anderen Lösungen können Sie die Protokollsuche für verschiedene Analyseanforderungen verwenden, um auf Basis der vom Netzwerkleistungsmonitor erfassten Daten benutzerdefinierte Berichte zu erstellen.

Die Lösung verwendet synthetische Transaktionen als primären Mechanismus zum Erkennen von Netzwerkfehlern. Daher können Sie die Lösung ungeachtet des Netzwerkgeräteherstellers oder -modells verwenden. Sie funktioniert lokal, in der Cloud (IaaS) und in hybriden Umgebungen. Die Lösung ermittelt automatisch die Netzwerktopologie und verschiedene Routen in Ihrem Netzwerk.

Typische Netzwerküberwachungsprodukte konzentrieren sich darauf, die Integrität von Netzwerkgeräten (Router, Switches usw.) zu überwachen. Anders als der Netzwerkleistungsmonitor geben sie jedoch keinen Einblick in die tatsächliche Qualität der Netzwerkkonnektivität zwischen zwei Punkten.

### <a name="using-the-solution-standalone"></a>Verwenden als eigenständige Lösung
Wenn Sie die Qualität der Netzwerkverbindungen zwischen ihren kritischen Workloads, Netzwerken, Rechenzentren oder Bürostandorten überwachen möchten, können Sie die Netzwerkleistungsmonitor-Lösung als eigenständige Lösung verwenden, um die Integrität der Konnektivität zwischen folgenden Punkten zu überwachen:

* mehreren Rechenzentren oder Bürostandorten, die über ein öffentliches oder privates Netzwerk miteinander verbunden sind
* kritischen Workloads, auf denen Branchenanwendungen ausgeführt werden
* öffentlichen Cloud-Diensten wie Microsoft Azure oder Amazon Web Services (AWS) und lokalen Netzwerken, wenn IaaS (VM) verfügbar ist und Ihre Gateways so konfiguriert sind, dass eine Kommunikation zwischen lokalen Netzwerken und Cloudnetzwerken zulässig ist
* Azure und lokalen Netzwerken bei Verwendung von Express Route

### <a name="using-the-solution-with-other-networking-tools"></a>Verwenden der Lösung mit anderen Netzwerktools
Wenn Sie eine Branchenanwendung überwachen möchten, können Sie die Netzwerkleistungsmonitor-Lösung als begleitende Lösung zu anderen Netzwerktools verwenden. Ein langsames Netzwerk kann zu langsamen Anwendungen führen, und der Netzwerkleistungsmonitor kann Sie dabei unterstützen, Leistungsprobleme von Anwendungen zu untersuchen, die durch zugrunde liegende Netzwerkprobleme verursacht werden. Da die Lösung keinen Zugriff auf Netzwerkgeräte erfordert, benötigt der Anwendungsadministrator kein Netzwerkteam, das ihm Informationen darüber liefert, wie sich das Netzwerk auf die Anwendungen auswirkt.

Selbst wenn Sie bereits in andere Netzwerküberwachungstools investiert haben, kann die Lösung diese Tools ergänzen, da die meisten herkömmlichen Netzwerküberwachungslösungen keinen Einblick in End-to-End-Netzwerkleistungsmetriken wie Verlust und Latenz geben.  Die Netzwerkleistungsmonitor-Lösung kann helfen, diese Lücke zu schließen.

## <a name="installing-and-configuring-agents-for-the-solution"></a>Installieren und Konfigurieren von Agents für die Lösung
Verwenden Sie zum Installieren von Agents die grundlegenden Prozesse, die unter [Verbinden von Windows-Computern mit Log Analytics](log-analytics-windows-agents.md) und [Herstellen einer Verbindung zwischen Operations Manager und Log Analytics](log-analytics-om-agents.md) beschrieben werden.

> [!NOTE]
> Sie müssen mindestens zwei Agents installieren, damit genügend Daten zum Ermitteln und Überwachen Ihrer Netzwerkressourcen vorhanden sind. Andernfalls verharrt die Lösung im Konfigurierungsstatus, bis Sie weitere Agents installieren und konfigurieren.
>
>

### <a name="where-to-install-the-agents"></a>Bestimmen des Installationsorts für die Agents
Bevor Sie Agents installieren, sollten Sie sich Gedanken über die Topologie Ihres Netzwerks und die zu überwachenden Bereiche des Netzwerks machen. Es wird empfohlen, für jedes zu überwachende Subnetz mehrere Agents zu installieren. Für jedes Subnetz, das Sie überwachen möchten, wählen Sie also mindestens zwei Server oder VMs aus, auf denen Sie einen Agent installieren.

Wenn Sie sich bezüglich der Topologie Ihres Netzwerks unsicher sind, installieren Sie die Agents auf Servern mit kritischen Workloads, für die Sie die Netzwerkleistung überwachen möchten. Beispiel: Sie möchten eine Netzwerkverbindung zwischen einem Webserver und einem Server mit SQL Server verfolgen. In diesem Beispiel würden Sie auf beiden Servern einen Agent installieren.

Agents überwachen die Netzwerkkonnektivität (Verknüpfungen) zwischen Hosts – und nicht die eigentlichen Hosts. Um eine Netzwerkverbindung zu überwachen, müssen Sie also an beiden Endpunkten der Verbindung Agents installieren.

### <a name="configure-agents"></a>Konfigurieren von Agents

Wenn Sie das ICMP-Protokoll für synthetische Transaktionen verwenden möchten, müssen Sie die folgenden Firewallregeln für die zuverlässige Verwendung von ICMP aktivieren:

```
netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action=allow
```


Wenn Sie das TCP-Protokoll verwenden möchten, müssen Sie Firewallports für diese Computer öffnen, um sicherzustellen, dass Agents kommunizieren können. Sie müssen das [PowerShell-Skript EnableRules.ps1](https://gallery.technet.microsoft.com/OMS-Network-Performance-04a66634) herunterladen und ohne Parameter in einem PowerShell-Fenster mit Administratorrechten ausführen.

Das Skript erstellt vom Netzwerkleistungsmonitor benötigte Registrierungsschlüssel und Windows-Firewallregeln, die es Agents erlauben, untereinander TCP-Verbindungen herzustellen. Die vom Skript erstellten Registrierungsschlüssel geben außerdem an, ob die Debugprotokolle und der Pfad zur Protokolldatei protokolliert werden sollen. Ferner definieren sie den für die Kommunikation verwendeten Agent-TCP-Port. Die Werte für diese Schlüssel werden automatisch durch das Skript festgelegt, daher sollten Sie diese Schlüssel nicht manuell ändern.

Standardmäßig wird Port 8084 geöffnet. Sie können einen benutzerdefinierten Port verwenden, indem Sie im Skript den Parameter `portNumber` angeben. Allerdings sollte auf allen Computern, auf denen das Skript ausgeführt wird, derselbe Port verwendet werden.

> [!NOTE]
> Das Skript „EnableRules.ps1“ konfiguriert die Windows-Firewallregeln nur auf dem Computer, auf dem das Skript ausgeführt wird. Bei Verwendung einer Netzwerkfirewall sollten Sie sicherstellen, dass diese den Datenverkehr zum TCP-Port erlaubt, der vom Netzwerkleistungsmonitor verwendet wird.
>
>

## <a name="configuring-the-solution"></a>Konfigurieren der Lösung
Verwenden Sie die folgenden Informationen zum Installieren und Konfigurieren der Lösung.

1. Die Netzwerkleistungsmonitor-Lösung erhält Daten von Computern unter Windows Server 2008 SP1 oder höher bzw. Windows 7 SP1 oder höher, die dieselben Anforderungen erfüllen wie der Microsoft Monitoring Agent (MMA). NPM-Agents können auch unter Windows-Desktop/Client-Betriebssystemen (Windows 10, Windows 8.1, Windows 8 und Windows 7) ausgeführt werden.
    >[!NOTE]
    >Die Agents für Windows-Serverbetriebssysteme unterstützen sowohl TCP-als auch ICMP-Protokolle für synthetische Transaktionen. Die Agents für Windows-Clientbetriebssysteme unterstützen jedoch nur ICMP als Protokolle für synthetische Transaktionen.

2. Fügen Sie mithilfe des [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.NetworkMonitoringOMS?tab=Overview) oder des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md) beschriebenen Prozesses Ihrem Arbeitsbereich die Netzwerkleistungsmonitor-Lösung hinzu.  
   ![Symbol des Netzwerkleistungsmonitors](./media/log-analytics-network-performance-monitor/npm-symbol.png)
3. Im OMS-Portal sehen Sie eine neue Kachel mit der Bezeichnung **Netzwerkleistungsmonitor** und der Meldung *Für die Lösung ist eine weitere Konfiguration erforderlich*. Sie müssen die Lösung konfigurieren, um Netzwerke basierend auf von Agents ermittelten Subnetzwerken und Knoten hinzuzufügen. Klicken Sie auf **Netzwerkleistungsmonitor**, um mit dem Konfigurieren des Standardnetzwerks zu beginnen.  
   ![Für die Lösung ist eine weitere Konfiguration erforderlich](./media/log-analytics-network-performance-monitor/npm-config.png)

### <a name="configure-the-solution-with-a-default-network"></a>Konfigurieren der Lösung mit einem Standardnetzwerk
Auf der Konfigurationsseite sehen Sie ein einziges Netzwerk, dieses hat den Namen **Default** (Standard). Wenn Sie noch keine Netzwerke definiert haben, sind alle automatisch ermittelten Subnetze im Standardnetzwerk enthalten.

Immer dann, wenn Sie ein Netzwerk erstellen, fügen Sie diesem ein Subnetz hinzu, und dieses Subnetz wird aus dem Standardnetzwerk entfernt. Wenn Sie ein Netzwerk löschen, werden alle seine Subnetze automatisch an das Standardnetzwerk zurückgegeben.

Das Standardnetzwerk ist also der Container für alle Subnetze, die nicht in einem benutzerdefinierten Netzwerk enthalten sind. Sie können das Standardnetzwerk weder bearbeiten noch löschen. Es verbleibt stets im System. Sie können jedoch beliebig viele Netzwerke erstellen.

In den meisten Fällen sind die Subnetze in Ihrer Organisation in mehreren Netzwerken angeordnet, und Sie sollten ein oder mehrere Netzwerke erstellen, um Ihre Subnetze logisch zu gruppieren.

### <a name="create-new-networks"></a>Erstellen neuer Netzwerke
Ein Netzwerk im Netzwerkleistungsmonitor ist ein Container für Subnetze. Sie können ein Netzwerk mit einem beliebigen Namen erstellen und Subnetze zum Netzwerk hinzufügen. Sie können z.B. ein Netzwerk namens *Gebäude1* erstellen und anschließend Subnetze hinzufügen, oder Sie können ein Netzwerk namens *DMZ* erstellen und anschließend alle der demilitarisierten Zone angehörenden Subnetze zu diesem Netzwerk hinzufügen.

#### <a name="to-create-a-new-network"></a>So erstellen Sie ein neues Netzwerk
1. Klicken Sie auf **Netzwerk hinzufügen**, und geben Sie den Netzwerknamen und eine Beschreibung ein.
2. Wählen Sie ein oder mehrere Subnetze aus, und klicken Sie anschließend auf **Hinzufügen**.
3. Klicken Sie zum Speichern der Konfiguration auf **Speichern**.  
   ![Netzwerk hinzufügen](./media/log-analytics-network-performance-monitor/npm-add-network.png)

### <a name="wait-for-data-aggregation"></a>Warten auf die Datenaggregation
Nachdem Sie die Konfiguration erstmals gespeichert haben, beginnt die Lösung, Informationen zu Netzwerkpaketverlusten und zur Latenz zwischen den Knoten zu sammeln, auf denen Agents installiert sind. Dieser Prozess kann eine gewisse Zeit in Anspruch nehmen, gelegentlich sogar mehr als 30 Minuten. Während dieses Zustands zeigt die Kachel des Netzwerkleistungsmonitors auf der Übersichtsseite die Meldung *Datenaggregation wird durchgeführt* an.

![Datenaggregation wird durchgeführt](./media/log-analytics-network-performance-monitor/npm-aggregation.png)

Nachdem die Daten hochgeladen wurden, wird die Kachel des Netzwerkleistungsmonitors aktualisiert und zeigt Daten an.

![Kachel des Netzwerkleistungsmonitors](./media/log-analytics-network-performance-monitor/npm-tile.png)

Klicken Sie auf die Kachel, um das Dashboard des Netzwerkleistungsmonitors anzuzeigen.

![Dashboard des Netzwerkleistungsmonitors](./media/log-analytics-network-performance-monitor/npm-dash01.png)

### <a name="edit-monitoring-settings-for-subnets"></a>Bearbeiten von Überwachungseinstellungen für Subnetze
Alle Subnetze, in denen mindestens ein Agent installiert wurde, werden auf der Registerkarte **Subnetzwerke** der Konfigurationsseite aufgeführt.

#### <a name="to-enable-or-disable-monitoring-for-particular-subnetworks"></a>So aktivieren bzw. deaktivieren Sie die Überwachung für bestimmte Subnetzwerke
1. Aktivieren bzw. deaktivieren Sie das Kontrollkästchen neben der **Subnetzwerk-ID**, und stellen Sie dann sicher, dass je nach Bedarf **Zur Überwachung verwenden** aktiviert oder deaktiviert ist. Sie können mehrere Subnetze aktivieren oder deaktivieren. Deaktivierte Subnetzwerke werden nicht überwacht, da die Agents dahingehend aktualisiert werden, dass sie keine Pings anderer Agents mehr ausführen.
2. Wählen Sie die Knoten aus, die Sie für ein bestimmtes Subnetzwerk überwachen möchten, indem Sie das Subnetzwerk aus der Liste auswählen und die erforderlichen Knoten von der Liste mit den nicht überwachten Knoten in die Liste mit den überwachten Knoten verschieben.
   Falls gewünscht können Sie eine benutzerdefinierte **Beschreibung** des Subnetzwerks hinzufügen.
3. Klicken Sie zum Speichern der Konfiguration auf **Speichern**.  
   ![Subnetz bearbeiten](./media/log-analytics-network-performance-monitor/npm-edit-subnet.png)

### <a name="choose-nodes-to-monitor"></a>Auswählen zu überwachender Knoten
Alle Knoten, auf denen ein Agent installiert ist, werden auf der Registerkarte **Knoten** aufgeführt.

#### <a name="to-enable-or-disable-monitoring-for-nodes"></a>So aktivieren bzw. deaktivieren Sie die Überwachung für Knoten
1. Aktivieren bzw. deaktivieren Sie die Knoten, die überwacht werden sollen bzw. deren Überwachung beendet werden soll.
2. Aktivieren bzw. deaktivieren Sie nach Bedarf **Zur Überwachung verwenden**.
3. Klicken Sie auf **Speichern**.  
   ![Knotenüberwachung aktivieren](./media/log-analytics-network-performance-monitor/npm-enable-node-monitor.png)

### <a name="set-monitoring-rules"></a>Festlegen von Überwachungsregeln
Wenn ein Schwellenwert überschritten wird, generiert der Netzwerkleistungsmonitor Integritätsereignisse bezüglich der Konnektivität zwischen einem Knotenpaar oder einem Subnetzwerk- oder Netzwerkverbindungspaar. Diese Schwellenwerte können automatisch vom System erlernt werden, oder Sie können die Werte so konfigurieren, dass benutzerdefinierte Warnungsregeln verwendet werden.

Die *Standardregel* wird vom System erstellt. Sie generiert immer dann ein Integritätsereignis, wenn der Verlust oder die Latenz zwischen einem Netzwerk- oder Subnetzwerkverbindungspaar den vom System erlernten Schwellenwert überschreitet. Sie können die Standardregel deaktivieren und benutzerdefinierte Überwachungsregeln erstellen.

#### <a name="to-create-custom-monitoring-rules"></a>So erstellen Sie benutzerdefinierte Überwachungsregeln
1. Klicken Sie auf der Registerkarte **Überwachen** auf **Regel hinzufügen**, und geben Sie den Regelnamen und eine Beschreibung ein.
2. Wählen Sie das zu überwachende Netzwerk- oder Subnetzwerkverbindungspaar aus den Listen aus.
3. Wählen Sie zunächst aus der Dropdownliste „Netzwerk“ das Netzwerk aus, in dem das erste relevante Subnetz bzw. die ersten relevanten Subnetze enthalten ist/sind. Wählen Sie anschließend das Subnetzwerk bzw. die Subnetzwerke in der entsprechenden Dropdownliste „Subnetzwerke“ aus.
   Wählen Sie **Alle Subnetzwerke** aus, wenn alle Subnetzwerke in einer Netzwerkverbindung überwacht werden sollen. Wählen Sie analog die übrigen relevanten Subnetzwerke aus. Ferner können Sie auf **Ausnahme hinzufügen** klicken, um die Überwachung bestimmter Subnetzwerkverbindungen aus Ihrer Auswahl zu entfernen.
4. Wählen Sie zwischen für die Ausführung synthetischer Transaktionen zwischen den Protokollen ICMP und TCP.
5. Wenn keine Integritätsereignisse für die von Ihnen ausgewählten Elemente erstellt werden sollen, deaktivieren Sie das Kontrollkästchen **Integritätsüberwachung für die mit dieser Regel abgedeckten Verbindungen aktivieren**.
6. Wählen Sie Überwachungsbedingungen aus.
   Sie können benutzerdefinierte Schwellenwerte für das Generieren von Integritätsereignissen festlegen, indem Sie Schwellenwerte eingeben. Sobald der Wert einer Bedingung den für Sie festgelegten Schwellenwert für das ausgewählte Netzwerk-/Subnetzwerkpaar überschreitet, wird ein Integritätsereignis generiert.
7. Klicken Sie zum Speichern der Konfiguration auf **Speichern**.  
   ![Benutzerdefinierte Überwachungsregel erstellen](./media/log-analytics-network-performance-monitor/npm-monitor-rule.png)

Nachdem Sie eine Überwachungsregel gespeichert haben, können Sie diese Regel in die Warnungsverwaltung integrieren, indem Sie auf **Warnung erstellen** klicken. Beim Erstellen einer Warnungsregel werden die Suchabfrage und andere erforderliche Parameter automatisch ausgefüllt. Mithilfe einer Warnungsregel können Sie neben den vorhandenen Warnungen in NPM auch E-Mail-basierte Warnungen erhalten. Warnungen können auch Problembehandlungsaktionen mit Runbooks auslösen oder mithilfe von Webhooks in vorhandene Dienstverwaltungslösungen integriert werden. Zum Bearbeiten der Warnungseinstellungen klicken Sie auf die Option zum **Verwalten von Warnungen**.

### <a name="choose-the-right-protocol-icmp-or-tcp"></a>Wählen des richtigen Protokolls: ICMP oder TCP

Die Funktion „Netzwerkleistungsüberwachung“ (Network Performance Monitor, NPM) nutzt synthetische Transaktionen zum Berechnen von Netzwerkleistungmetriken wie Paketverlust und Linklatenz. Um dies besser zu verstehen, sollten Sie einen NPM-Agent mit einem Ende einer Netzwerkverbindung verbinden. Dieser NPM-Agent sendet Testpakete an einen zweiten NPM-Agent, der mit einem anderen Ende des Netzwerks verbunden ist. Der zweite Agent antwortet mit Antwortpaketen. Dieser Prozess wird einige Male wiederholt. Durch Messen der Anzahl von Antworten und Empfangszeit jeder Antwort kann der erste NPM-Agent die Linklatenz und Paketverluste messen.

Format, Größe und Reihenfolge dieser Pakete richten sich nach dem Protokoll, das Sie bei der Erstellung von Überwachungsregeln auswählen. Je nach Protokoll der Pakete können die zwischengeschalteten Netzwerkgeräte (Router, Switches usw.) diese Pakete möglicherweise unterschiedlich verarbeiten. Folglich wirkt sich Ihre Protokollauswahl auf die Genauigkeit der Ergebnisse aus. Zudem bestimmt Ihre Wahl des Protokolls auch, ob Sie nach Bereitstellen der NPM-Lösung manuelle Schritte ausführen müssen.

NPM bietet Ihnen für die Ausführung synthetischer Transaktionen die Wahl zwischen den Protokollen ICMP und TCP.
Wenn Sie beim Erstellen einer synthetischen Transaktionsregel ICMP wählen, verwenden die NPM-Agents ICMP ECHO-Nachrichten zum Berechnen von Netzwerklatenz und Paketverlust. ICMP ECHO verwendet dieselbe Nachricht, die vom gängigen Hilfsprogramm Ping gesendet wird. Bei Verwenden von TCP als Protokoll senden NPM-Agents ein TCP SYN-Paket über das Netzwerk. Darauf folgt ein TCP-Handshake bis zum Abschluss und das anschließende Entfernen der Verbindung mithilfe von RST-Paketen.

#### <a name="points-to-consider-before-choosing-the-protocol"></a>Bei Wahl des Protokolls zu beachtende Aspekte
Ehe Sie ein Protokoll wählen, sollten Sie die folgenden Informationen berücksichtigen:

##### <a name="discovering-multiple-network-routes"></a>Ermitteln mehrerer Netzwerkrouten
TCP ist genauer, wenn mehrere Routen ermitteln werden, und benötigt weniger Agents in jedem Subnetz. Beispielsweise können ein oder zwei Agents, die TCP verwenden, alle redundanten Pfade zwischen Subnetzen ermitteln. Bei Verwenden von ICMP benötigen Sie mehr Agents, um dasselbe Resultat zu erzielen. Wenn Sie bei Verwenden von ICMP über *N* Routen zwischen zwei Subnetzen verfügen, benötigen Sie im jeweiligen Quell- oder Zielsubnetz mehr als *5* Agents.

##### <a name="accuracy-of-results"></a>Genauigkeit der Ergebnisse
Router und Switches weisen ICMP ECHO-Paketen im Vergleich mit TCP-Paketen tendenziell eine niedrigere Priorität zu. Wenn Netzwerkgeräte in bestimmten Situationen stark ausgelastet sind, geben die von TCP abgerufenen Daten den Paketverlust und die Latenz von Anwendungen präziser zurück. Dies erfolgt, da der meiste Datenverkehr über TCP übertragen wird. In solchen Fällen bietet ICMP im Vergleich mit TCP ungenauere Ergebnisse.

##### <a name="firewall-configuration"></a>Firewall-Konfiguration
Das TCP-Protokoll erfordert, dass TCP-Pakete an einen Zielport gesendet werden. Der von NPM-Agents verwendete Standardport ist 8084, den Sie beim Konfigurieren von Agents jedoch ändern können. Sie müssen daher sicherstellen, dass Ihre Netzwerkfirewalls bzw. NSG-Regeln (Netzwerksicherheitsgruppen) Datenverkehr an diesem Port zulassen. Sie müssen auch sicherstellen, dass die lokale Firewall auf den Computern, auf denen Agents installiert sind, für das Zulassen von Datenverkehr an diesem Port konfiguriert ist.

Sie können zum Konfigurieren von Firewallregeln auf Ihren Computern Windows PowerShell-Skripts verwenden. Die Netzwerkfirewall müssen Sie allerdings manuell konfigurieren.

Im Gegensatz dazu arbeitet ICMP nicht mit einem Port. In den meisten Unternehmensumgebungen ist ICMP-Datenverkehr durch die Firewalls zulässig, damit Sie Netzwerkdiagnosetools wie das Hilfsprogramm Ping einsetzen können. Wenn Sie also einen Computer von einem anderen aus pingen können, lässt sich das ICMP-Protokoll nutzen, ohne dass Firewalls manuell konfiguriert werden müssen.

> [!NOTE]
> Falls Sie nicht sicher sind, welches Protokoll Sie verwenden sollen, wählen Sie zum Einstieg ICMP. Wenn Sie mit den Ergebnissen nicht zufrieden sind, können Sie später immer noch zu TCP wechseln.


#### <a name="how-to-switch-the-protocol"></a>Informationen zum Wechseln des Protokolls

Wenn Sie sich während der Bereitstellung für ICMP entscheiden, können Sie jederzeit zu TCP wechseln, indem Sie die Standardüberwachungsregel bearbeiten.

##### <a name="to-edit-the-default-monitoring-rule"></a>So bearbeiten Sie die Standardüberwachungsregel
1.  Navigieren Sie zu **Netzwerkleistung** > **Monitor** > **Konfigurieren** > **Monitor**, und klicken Sie dann auf **Standardregel**.
2.  Scrollen Sie zum Abschnitt **Protokoll**, und wählen Sie das Protokoll, das Sie verwenden möchten.
3.  Klicken Sie auf **Speichern**, um die Einstellung zu übernehmen.

Auch wenn die Standardregel ein bestimmtes Protokoll angibt, können Sie neue Regeln mit einem anderen Protokoll erstellen. Sie können sogar eine Kombination aus Regeln erstellen, bei der einige der Regeln ICMP und andere TCP verwenden.




## <a name="data-collection-details"></a>Details zur Datensammlung
Der Netzwerkleistungsmonitor verwendet TCP SYN-SYNACK-ACK-Handshakepakete, wenn „TCP“ als Protokoll zum Erfassen von Informationen zu Verlusten und zur Latenz ausgewählt ist, und „ICMP ECHO ICMP ECHO REPLY“, wenn „ICMP“ ausgewählt ist. Außerdem wird Traceroute zum Abrufen von Topologieinformationen verwendet.

Die folgende Tabelle zeigt Datensammlungsmethoden und andere Details, wie Daten für den Netzwerkleistungsmonitor gesammelt werden, an.

| Plattform | Direkt-Agent | SCOM-Agent | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- |
| Windows | &#8226; | &#8226; |  |  |  |TCP-Handshakes/ICMP ECHO-Nachrichten werden alle fünf Sekunden gesendet, Daten alle drei Minuten |

Die Lösung nutzt synthetische Transaktionen, um die Integrität des Netzwerks zu bewerten. OMS-Agents, die an verschiedenen Punkten im Netzwerk installiert sind, tauschen TCP- oder ICMP Echo-Pakete (je nach dem für die Überwachung ausgewählten Protokoll) miteinander aus. Dabei erhalten Agents Informationen zur Roundtripzeit und ggf. zu Paketverlust. Jeder Agent führt zudem in regelmäßigen Abständen eine Routenverfolgung (Traceroute) zu anderen Agents aus, um alle zu testenden Routen im Netzwerk zu ermitteln. Anhand dieser Daten können die Agents die Netzwerklatenz und die Paketverluste herleiten. Die Tests werden alle fünf Sekunden wiederholt, und die Daten werden von den Agents vor dem Hochladen zum Log Analytics-Dienst für einen Zeitraum von drei Minuten aggregiert.

> [!NOTE]
> Obwohl die Agents häufig miteinander kommunizieren, generieren sie bei den Tests nur wenig Netzwerkdatenverkehr. Agents bedienen sich lediglich der TCP SYN-SYNACK-ACK-Handshakepakete, um die Datenverluste und die Latenz zu ermitteln. Sie tauschen keine Datenpakete aus. Bei diesem Vorgang kommunizieren die Agents nur bei Bedarf miteinander, und die Kommunikationstopologie der Agents ist optimiert, um den Netzwerkdatenverkehr zu reduzieren.
>
>

## <a name="using-the-solution"></a>Verwenden der Lösung
In diesem Abschnitt werden alle Dashboardfunktionen einschließlich ihrer Verwendung erläutert.

### <a name="solution-overview-tile"></a>Kachel der Lösungsübersicht
Nachdem Sie die Netzwerkleistungsmonitor-Lösung aktiviert haben, wird auf der Kachel der Lösung auf der OMS-Übersichtsseite eine kurze Übersicht über die Integrität des Netzwerks angezeigt. Ein Ringdiagramm zeigt die Anzahl der fehlerfreien und fehlerhaften Subnetzwerkverbindungen an. Wenn Sie auf die Kachel klicken, wird das Lösungsdashboard geöffnet.

![Kachel des Netzwerkleistungsmonitors](./media/log-analytics-network-performance-monitor/npm-tile.png)

### <a name="network-performance-monitor-solution-dashboard"></a>Dashboard der Netzwerkleistungsmonitor-Lösung
Auf dem Blatt **Netzwerkzusammenfassung** wird eine Zusammenfassung der Netzwerke einschließlich ihrer relativen Größe angezeigt. Hierauf folgen Kacheln, auf denen die Gesamtzahl der Netzwerkverbindungen, Subnetzwerkverbindungen und Pfade im System angezeigt wird. (Ein Pfad besteht aus den IP-Adressen zweier Hosts mit Agents sowie allen Hops zwischen ihnen.)

Das Blatt **Top-Netzwerkintegritätsereignisse** enthält eine Liste der aktuellen Integritätsereignisse und Warnungen im System, einschließlich einer Angabe, seit wann das Ereignis aktiv ist. Ein Integritätsereignis oder eine Warnung wird generiert, wenn der Paketverlust oder die Latenz einer Netzwerk- oder Subnetzwerkverbindung einen Schwellenwert überschreitet.

Auf dem Blatt **Top-Netzwerkverbindungen mit Fehlern** wird eine Liste der fehlerhaften Netzwerkverbindungen angezeigt. Hierbei handelt es sich um die Netzwerkverbindungen, bei denen gegenwärtig mindestens ein Integritätsereignis v‎orliegt.

Auf den Blättern **Top Subnetwork Links with Most Loss** (Top-Subnetzwerkverbindungen mit den höchsten Verlusten) und **Subnetwork Links with Most Latency** (Subnetzwerkverbindungen mit der höchsten Latenz) werden die Subnetzwerkverbindungen mit den höchsten Paketverlusten bzw. der höchsten Latenz angezeigt. Bei bestimmten Netzwerkverbindungen ist mit einer hohen Latenz bzw. beträchtlichen Paketverlusten zu rechnen. Derartige Verbindungen werden in den Top-10-Listen angezeigt, sind jedoch nicht als fehlerhaft gekennzeichnet.

Das Blatt **Allgemeine Abfragen** enthält einen Satz von Suchabfragen, mit denen Netzwerk-Überwachungsrohdaten direkt abgerufen werden. Sie können diese Abfragen als Ausgangspunkt für das Erstellen eigener Abfragen für benutzerdefinierte Berichte verwenden.

![Dashboard des Netzwerkleistungsmonitors](./media/log-analytics-network-performance-monitor/npm-dash01.png)

### <a name="drill-down-for-depth"></a>Drilldown für mehr Tiefe
Sie können auf dem Lösungsdashboard auf verschiedene Verbindungen klicken, um zu den für Sie relevanten Bereichen einen Drilldown auszuführen. Wenn beispielsweise auf dem Dashboard eine Warnung oder eine fehlerhafte Netzwerkverbindung angezeigt wird, können Sie darauf klicken, um diesen Umstand näher zu untersuchen. Sie werden zu einer Seite weitergeleitet, auf der alle Subnetzwerkverbindungen für die betreffende Netzwerkverbindung aufgeführt werden. Sie sehen den Verlust, die Latenz und den Integritätsstatus aller Subnetzwerkverbindungen und können schnell feststellen, welche Subnetzwerkverbindungen das Problem verursachen. Klicken Sie anschließend auf **Knotenverbindungen anzeigen**, um alle Knotenverbindungen für die fehlerhafte Subnetzverbindung anzuzeigen. Nun sehen Sie die einzelnen Knoten-zu-Knoten-Verbindungen und können die fehlerhaften Knotenverbindungen ausfindig machen.

Klicken Sie auf **Topologie anzeigen**, um die Hop-by-Hop-Topologie der Routen zwischen den Quell- und den Zielknoten anzuzeigen. Die fehlerhafte Routen bzw. Hops werden rot angezeigt, sodass Sie das Problem schnell einem bestimmten Teil des Netzwerks zuordnen können.

![Datendrilldown](./media/log-analytics-network-performance-monitor/npm-drill.png)

### <a name="network-state-recorder"></a>Network State Recorder

Jede Ansicht zeigt eine Momentaufnahme der Netzwerkintegrität zu einem bestimmten Zeitpunkt. Standardmäßig wird der aktuelle Status angezeigt. Die Leiste am oberen Rand der Seite zeigt den Zeitpunkt, für den der Status angezeigt wird. Sie können in der Zeit zurückgehen und die Momentaufnahme der Netzwerkintegrität anzeigen, indem Sie auf der Leiste auf **Aktionen** klicken. Sie können auch die automatische Aktualisierung für eine beliebige Seite aktivieren oder deaktivieren, während Sie den aktuellen Status anzeigen.

![Netzwerkstatus](./media/log-analytics-network-performance-monitor/network-state.png)

#### <a name="trend-charts"></a>Trenddiagramme
Auf jeder Ebene, zu der Sie einen Drilldown ausführen, sehen Sie den Trend der Verluste und der Latenz für eine Netzwerkverbindung. Außerdem stehen Trenddiagramme auch für Subnetzwerk- und Knotenverbindungen zur Verfügung. Mit dem Zeitsteuerelement am oberen Rand des Diagramms können Sie das Zeitintervall ändern, für das das Diagramm erstellt werden soll.

Trenddiagramme zeigen die Leistung einer Netzwerkverbindung im historischen Kontext an. Einige Netzwerkprobleme sind vorübergehender Natur und lassen sich durch einen Blick auf den aktuellen Status des Netzwerks nur schwer erkennen. Derartige Probleme können kurzfristig auftreten und wieder verschwinden, bevor sie bemerkt werden, um dann zu einem späteren Zeitpunkt erneut aufzutreten. Solche vorübergehenden Probleme können auch Anwendungsadministratoren Schwierigkeiten bereiten, da sie sich häufig in einem unerklärlichen Anstieg der Reaktionszeit einer Anwendung niederschlagen, obgleich alle Anwendungskomponenten einwandfrei zu funktionieren scheinen.

Sie können derartige Probleme unverzüglich erkennen, wenn Sie sich ein Trenddiagramm anschauen, in dem das Problem als plötzliche Spitze bei der Netzwerklatenz oder dem Paketverlust zu sehen ist.

![Trenddiagramm](./media/log-analytics-network-performance-monitor/npm-trend.png)

#### <a name="hop-by-hop-topology-map"></a>Hop-by-Hop-Topologiekarte
Im Netzwerkleistungsmonitor wird die Hop-by-Hop-Topologie der Routen zwischen zwei Knoten in einer interaktiven Topologiekarte angezeigt. Sie können die Topologiekarte anzeigen, indem Sie eine Knotenverbindung auswählen und anschließend auf **Topologie anzeigen** klicken. Sie können die Topologiekarte ferner anzeigen, indem Sie im Dashboard auf die Kachel **Pfade** klicken. Beim Klicken auf **Pfade** im Dashboard müssen Sie den Quell- und den Zielknoten aus dem linken Bereich auswählen und anschließend auf **Zeichnen** klicken, damit die Routen zwischen diesen beiden Knoten dargestellt werden.

Die Topologiekarte zeigt an, wie viele Routen zwischen den beiden Knoten existieren und welche Pfade die Datenpakete verwenden. Leistungsengpässe im Netzwerk werden in der Topologiekarte rot gekennzeichnet. Sie finden eine fehlerhafte Netzwerkverbindung bzw. ein fehlerhaftes Netzwerkgerät, indem Sie in der Topologiekarte nach rot gekennzeichneten Elementen suchen.

Wenn Sie in der Topologiekarte auf einen Knoten klicken oder auf ihn zeigen, sehen Sie die Eigenschaften des Knotens wie FQDN und die IP-Adresse. Klicken Sie auf einen Hop, um seine IP-Adresse anzuzeigen. Sie können bestimmte Routen mithilfe der Filter im reduzierbaren Aktionsbereich filtern. Und Sie können auch die Netzwerktopologien vereinfachen, indem die mithilfe des Schiebereglers im Aktionsbereich die zwischenzeitlichen Hops ausblenden. Mit dem Mausrad können Sie die Topologiekarte vergrößern oder verkleinern.

Beachten Sie, dass es sich bei der in der Karte gezeigten Topologie um eine Layer 3-Topologie handelt, die keine Layer 2-Geräte und -Verbindungen enthält.

![Hop-by-Hop-Topologiekarte](./media/log-analytics-network-performance-monitor/npm-topology.png)

#### <a name="fault-localization"></a>Lokalisieren von Fehlern
Der Netzwerkleistungsmonitor findet Netzwerkengpässe, ohne eine Verbindung zu den Netzwerkgeräten herzustellen. Der Netzwerkleistungsmonitor erstellt anhand der über das Netzwerk gesammelten Daten und durch Anwenden erweiterter Algorithmen auf das Netzwerkdiagramm eine probabilistische Schätzung derjenigen Teile des Netzwerks, die höchstwahrscheinlich für das Problem verantwortlich sind.

Dieser Ansatz ist hilfreich, um die Netzwerkengpässe zu ermitteln, wenn kein Zugriff auf Hops verfügbar ist, da hierzu keine Daten von den Netzwerkgeräten wie Routern und Switches erfasst werden müssen. Dies ist auch nützlich, wenn die Hops zwischen zwei Knoten nicht Ihrer administrativen Kontrolle unterliegen. Bei den Hops kann es sich beispielsweise um ISP-Router handeln.

### <a name="log-analytics-search"></a>Log Analytics-Suche
Alle auf dem Dashboard des Netzwerkleistungsmonitors und auf den Drilldownseiten grafisch dargestellten Daten sind auch nativ über die Log Analytics-Suche verfügbar. Sie können die Daten mithilfe der Suchabfragesprache abrufen und benutzerdefinierte Berichte erstellen, indem Sie die Daten nach Excel oder PowerBI exportieren. Das Blatt **Allgemeine Abfragen** im Dashboard enthält einige nützliche Abfragen, die Sie als Ausgangspunkt für das Erstellen eigener Abfragen und Berichte verwenden können.

![Suchabfragen](./media/log-analytics-network-performance-monitor/npm-queries.png)

## <a name="investigate-the-root-cause-of-a-health-alert"></a>Untersuchen der Ursache einer Integritätswarnung
Nachdem Sie nun mit dem Netzwerkleistungsmonitor vertraut sind, erfahren Sie in diesem Abschnitt anhand eines Beispiels, wie Sie auf einfache Weise die Ursachen eines Integritätsereignisses untersuchen.

1. Auf der Übersichtsseite erhalten Sie mithilfe der Kachel **Netzwerkleistungsmonitor** einen unverzüglichen Überblick über die Integrität Ihres Netzwerks. Beachten Sie, dass von den 6 überwachten Subnetzwerkverbindungen 2 Verbindungen fehlerhaft sind. Dies macht eine Untersuchung erforderlich. Klicken Sie auf die Kachel, um das Lösungsdashboard anzuzeigen.  
   ![Kachel des Netzwerkleistungsmonitors](./media/log-analytics-network-performance-monitor/npm-investigation01.png)
2. Im folgenden Beispielbild sehen Sie, dass ein Integritätsereignis vorhanden ist – ein fehlerhafter Netzwerklink. Sie möchten dieses Problem untersuchen und klicken dazu auf die Netzwerkverbindung **DMZ2-DMZ1**, um die Ursache des Problems herauszufinden.  
   ![Beispiel für eine fehlerhafte Netzwerkverbindung](./media/log-analytics-network-performance-monitor/npm-investigation02.png)
3. Auf der Drilldownseite werden alle Subnetzwerkverbindungen der Netzwerkverbindung **DMZ2-DMZ1** angezeigt. Sie sehen, dass die Latenz beider Subnetzwerkverbindungen den Schwellenwert überschritten hat und die Netzwerkverbindung somit fehlerhaft ist. Sie können auch die Latenztrends der beiden Subnetzwerkverbindungen anzeigen. Mit dem Zeitauswahl-Steuerelement im Diagramm können Sie sich auf den gewünschten Zeitraum konzentrieren. Sie können sehen, zu welcher Tageszeit die Latenz ihren Höchstwert erreicht hat. Sie können anschließend die Protokolle für diesen Zeitraum durchgehen, um das Problem zu untersuchen. Klicken Sie auf **Knotenverbindungen anzeigen**, um einen weiteren Drilldown auszuführen.  
   ![Beispiel für fehlerhafte Subnetzverbindungen](./media/log-analytics-network-performance-monitor/npm-investigation03.png)
4. Ähnlich wie bei der vorherigen Seite werden auf der Drilldownseite der Subnetzwerkverbindung die enthaltenen Knotenverbindungen aufgeführt. Sie können ähnliche Aktionen wie im vorherigen Schritt ausführen. Klicken Sie auf **Topologie anzeigen**, um die Topologie zwischen den beiden Knoten anzuzeigen.  
   ![Beispiel für fehlerhafte Knotenverbindungen](./media/log-analytics-network-performance-monitor/npm-investigation04.png)
5. Alle Pfade zwischen den beiden ausgewählten Knoten werden in die Topologiekarte gezeichnet. Sie können die Hop-by-Hop-Topologie der Routen zwischen zwei Knoten in der Topologiekarte grafisch darstellen. So wissen Sie genau, wie viele Routen es zwischen den beiden Knoten gibt und welche Pfade die Datenpakete verwenden. Leistungsengpässe im Netzwerk werden rot gekennzeichnet. Sie finden eine fehlerhafte Netzwerkverbindung bzw. ein fehlerhaftes Netzwerkgerät, indem Sie in der Topologiekarte nach rot gekennzeichneten Elementen suchen.  
   ![Beispiel für eine Ansicht mit einer fehlerhaften Topologie](./media/log-analytics-network-performance-monitor/npm-investigation05.png)
6. Verlust, Latenz und Anzahl der Hops in jedem Pfad können im Bereich **Aktion** überprüft werden. Verwenden Sie die Bildlaufleiste, um die Details dieser fehlerhaften Pfade anzuzeigen.  Verwenden Sie die Filter, um die Pfade mit dem fehlerhaften Hop auszuwählen, sodass die Topologie nur für die ausgewählten Pfade dargestellt wird. Mit dem Mausrad können Sie die Topologiekarte vergrößern oder verkleinern.

   In der Abbildung unten ist anhand der rot gekennzeichneten Pfade und Hops deutlich erkennbar, wo die Ursache für die Problembereiche im betreffenden Abschnitt des Netzwerks liegt. Durch Klicken auf einen Knoten in der Topologiekarte werden die Eigenschaften des Knotens einschließlich FQDN und IP-Adresse angezeigt. Durch Klicken auf einen Hop wird die IP-Adresse des Hops angezeigt.  
   ![Fehlerhafte Topologie – Beispiel für Pfaddetails](./media/log-analytics-network-performance-monitor/npm-investigation06.png)

## <a name="provide-feedback"></a>Feedback geben

- **UserVoice** – Sie können Ihre Ideen zu Funktionen des Netzwerkleistungsmonitors veröffentlichen, an denen wir arbeiten sollen. Besuchen Sie unsere [UserVoice-Seite](https://feedback.azure.com/forums/267889-log-analytics/category/188146-network-monitoring).
- **Treten Sie unserer Gruppe bei** – wir sind immer an neuen Kunden interessiert, die unserer Gruppe beitreten. Als Teil der Gruppe erhalten Sie vorab Zugriff auf neue Funktionen und helfen uns dabei, den Netzwerkleistungsmonitor zu verbessern. Wenn Sie beitreten möchten, füllen Sie diesen [kurzen Fragebogen](https://aka.ms/npmcohort) aus.

## <a name="next-steps"></a>Nächste Schritte
* Mit [Protokollsuchen](log-analytics-log-searches.md) können Sie Detaildatensätze mit Netzwerkleistungsdaten anzeigen.

