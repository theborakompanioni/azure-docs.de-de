---
title: "DNS Analytics-Lösung in Azure Log Analytics | Microsoft-Dokumentation"
description: "Richten Sie die DNS Analytics-Lösung in Log Analytics ein, und verwenden Sie sie, um Erkenntnisse im Zusammenhang mit Sicherheit, Leistung und Betrieb der DNS-Infrastruktur zu sammeln."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: f44a40c4-820a-406e-8c40-70bd8dc67ae7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2017
ms.author: banders
translationtype: Human Translation
ms.sourcegitcommit: 8c4e33a63f39d22c336efd9d77def098bd4fa0df
ms.openlocfilehash: 4473ca68374b96f10c60282135e83f7ec390bb48
ms.lasthandoff: 04/20/2017


---

# <a name="gather-insights-about-your-dns-infrastructure-with-the-dns-analytics-preview-solution"></a>Sammeln von Erkenntnissen zu Ihrer DNS-Infrastruktur mit der DNS Analytics-Lösung (Vorschau)

In diesem Artikel wird beschrieben, wie Sie die DNS Analytics-Lösung in Log Analytics einrichten und verwenden, um Erkenntnisse im Zusammenhang mit Sicherheit, Leistung und Betrieb der DNS-Infrastruktur zu sammeln.

DNS Analytics ermöglicht Ihnen Folgendes:

- Identifizieren von Clients, die versuchen, schädliche Domänennamen aufzulösen
- Identifizieren von veralteten Ressourceneinträgen
- Identifizieren von häufig abgefragten Domänennamen und DNS-Clients mit hoher Aktivität
- Anzeigen der Anforderungslast auf DNS-Servern
- Anzeigen von Fehlern der dynamischen DNS-Registrierung

Die Lösung erfasst, analysiert und korreliert Windows-DNS-Analyseprotokolle und -Überwachungsprotokolle sowie andere ähnliche Daten von Ihren DNS-Servern.

## <a name="connected-sources"></a>Verbundene Quellen

In der folgenden Tabelle sind die verbundenen Quellen beschrieben, die von der Lösung unterstützt werden.

| **Verbundene Quelle** | **Unterstützung** | **Beschreibung** |
| --- | --- | --- |
| [Windows-Agents](log-analytics-windows-agents.md) | Ja | Die Lösung erfasst DNS-Informationen von Windows-Agents. |
| [Linux-Agents](log-analytics-linux-agents.md) | Nein | Die Lösung erfasst keine DNS-Informationen von direkten Linux-Agents. |
| [SCOM-Verwaltungsgruppe](log-analytics-om-agents.md) | Ja | Die Lösung erfasst DNS-Informationen von Agents in einer verbundenen SCOM-Verwaltungsgruppe. Es ist keine direkte Verbindung vom SCOM-Agent mit OMS erforderlich. Daten werden von der Verwaltungsgruppe an das OMS-Repository weitergeleitet. |
| [Azure-Speicherkonto](log-analytics-azure-storage.md) | Nein | Azure-Speicher wird von der Lösung nicht verwendet. |

### <a name="data-collection-details"></a>Details zur Datensammlung

Die Lösung sammelt Daten zum DNS-Inventar und zu DNS-Ereignissen von den DNS-Servern, auf denen ein Log Analytics-Agent installiert ist. Diese Daten werden dann in Log Analytics hochgeladen und im Lösungsdashboard angezeigt. Inventardaten, z.B. die Anzahl der DNS-Server, Zonen und Ressourceneinträge, werden gesammelt, indem die DNS-PowerShell-Cmdlets ausgeführt werden. Die Daten werden einmal alle zwei Tage aktualisiert. Die Ereignisdaten werden nahezu in Echtzeit aus den [Analyse- und Überwachungsprotokollen](https://technet.microsoft.com/library/dn800669.aspx#enhanc) erfasst, die durch die verbesserte DNS-Protokollierung und -Diagnose in Windows Server 2012 R2 bereitgestellt werden.

## <a name="configuration"></a>Konfiguration

Verwenden Sie die folgenden Informationen zum Konfigurieren der Lösung.

- Sie benötigen einen [Windows](log-analytics-windows-agents.md)- oder [Operations Manager](log-analytics-om-agents.md)-Agent auf jedem DNS-Server, den Sie überwachen möchten.
- Fügen Sie mithilfe des [Azure Marketplace](https://aka.ms/dnsanalyticsazuremarketplace) oder des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md) beschriebenen Prozesses Ihrem OMS-Arbeitsbereich die DNS Analytics-Lösung hinzu.

Die Lösung beginnt ohne weitere Konfiguration mit dem Sammeln von Daten. Allerdings können Sie die folgende Konfiguration zum Anpassen der Datensammlung verwenden.

###  <a name="configure-the-solution"></a>Konfigurieren der Projektmappe

Klicken Sie im Lösungsdashboard auf **Konfiguration**, um die DNS Analytics-Konfigurationsseite zu öffnen. Es gibt zwei Arten von Konfigurationsänderungen, die Sie vornehmen können:

- **Domänennamen in der Whitelist**: Die Lösung verarbeitet nicht alle Suchabfragen. Sie verwaltet eine Whitelist mit Domänennamensuffixen. Die Suchabfragen, die in Domänennamen aufgelöst werden, die mit Domänennamensuffixen in dieser Whitelist übereinstimmen, werden von der Lösung nicht verarbeitet. Wenn in der Whitelist enthalte Domänennamen nicht verarbeitet werden, werden die an Log Analytics gesendeten Daten optimiert. Die Standardwhitelist enthält gängige öffentliche Domänennamen, z.B. www.google.com und www.facebook.com. Sie können die vollständige Standardliste mithilfe der Bildlaufleiste anzeigen.

Sie können die Liste ändern, um alle Domänennamensuffixe, zu denen Sie keine Daten anzeigen möchten, hinzuzufügen (oder um Suffixe zu entfernen).

- **Schwellenwert für Clients mit hoher Aktivität**: Die DNS-Clients, die den Schwellenwert für die Anzahl der Suchanforderungen überschreiten, werden auf dem Blatt **DNS-Clients** hervorgehoben. Der Standardschwellenwert beträgt 1000. Sie können den Schwellenwert bearbeiten.

![Domänennamen in der Whitelist](./media/log-analytics-dns/dns-config.png)

## <a name="management-packs"></a>Management Packs

Wenn Sie den Microsoft Monitoring Agent (MMA) für die Verbindung mit Ihrem OMS-Arbeitsbereich verwenden, wird das folgende Management Pack installiert.

- Microsoft DNS Data Collector Intelligence Pack (Microsft.IntelligencePacks.Dns)

Falls Ihre SCOM-Verwaltungsgruppe mit dem OMS-Arbeitsbereich verbunden ist, werden in SCOM die folgenden Management Packs installiert, wenn Sie diese Lösung hinzufügen. Für diese Management Packs ist keine Konfiguration oder Wartung erforderlich.

- Microsoft DNS Data Collector Intelligence Pack (Microsft.IntelligencePacks.Dns)
- Microsoft System Center Advisor DNS Analytics Configuration (Microsoft.IntelligencePack.Dns.Configuration)

Weitere Informationen zur Aktualisierung von Management Packs finden Sie unter [Herstellen einer Verbindung zwischen Operations Manager und Log Analytics](log-analytics-om-agents.md).

## <a name="use-the-solution"></a>Verwenden der Lösung

In diesem Abschnitt werden alle Dashboardfunktionen einschließlich ihrer Verwendung erläutert.

Nachdem Sie die DNS Analytics-Lösung zu Ihrem Arbeitsbereich hinzugefügt haben, enthält die Lösungskachel auf der OMS-Übersichtsseite eine kurze Zusammenfassung der DNS-Infrastruktur. Dazu gehört die Anzahl der DNS-Server, auf denen die Daten gesammelt werden, und die Anzahl der Anforderungen von Clients zur Auflösung schädlicher Domänen in den letzten 24 Stunden. Wenn Sie auf die Kachel klicken, wird das Lösungsdashboard geöffnet.

![DNS Analytics-Kachel](./media/log-analytics-dns/dns-tile.png)

### <a name="solution-dashboard"></a>Lösungsdashboard

Das Lösungsdashboard zeigt zusammengefasste Informationen für die verschiedenen Funktionen der Lösung. Es enthält auch Links zur detaillierten Ansicht für die forensische Analyse und Diagnose. Standardmäßig werden die Daten der letzten sieben Tage angezeigt. Sie können den Datums- und Uhrzeitbereich mit dem Steuerelement für die Datums-/Uhrzeitauswahl ändern, wie in der folgenden Abbildung gezeigt.

![Zeitauswahl-Steuerelement](./media/log-analytics-dns/dns-time.png)

Im Lösungsdashboard werden die folgenden Blätter angezeigt:

**DNS-Sicherheit**: Gibt die DNS-Clients an, die versuchen, mit schädlichen Domänen zu kommunizieren. Mithilfe von Microsoft-Feeds mit Informationen zu Bedrohungen erkennt DNS Analytics Client-IP-Adressen, die versuchen, auf die schädlichen Domänen zuzugreifen. In vielen Fällen &quot;wählen&quot; mit Schadsoftware infizierte Geräte das &quot;Befehls- und Steuerungszentrum&quot; der schädlichen Domäne durch Auflösen des Domänennamens der Schadsoftware an.

![Blatt „DNS-Sicherheit“](./media/log-analytics-dns/dns-security-blade.png)

Wenn Sie auf eine Client-IP-Adresse in der Liste klicken, wird die Protokollsuche geöffnet und zeigt die Details der Suche für die jeweilige Abfrage an. Im folgenden Beispiel hat DNS Analytics erkannt, dass mit einem [IRCbot](https://www.microsoft.com/security/portal/threat/encyclopedia/entry.aspx?Name=Win32/IRCbot) kommuniziert wurde.

![Protokollsuchergebnisse mit IRCbot](./media/log-analytics-dns/ircbot.png)

Mit den Informationen können Sie Folgendes identifizieren:

- Die Client-IP-Adresse, die die Kommunikation initiiert hat
- Den Domänennamen, der in die schädliche IP-Adresse aufgelöst wird
- Die IP-Adressen, in die der Domänenname aufgelöst wird
- Die schädliche IP-Adresse
- Den Schweregrad des Problems
- Den Grund für die Aufnahme der schädlichen IP-Adresse in die Blacklist
- Den Zeitpunkt der Erkennung

**Abgefragte Domänen**: Gibt die am häufigsten von den DNS-Clients in Ihrer Umgebung abgefragten Domänennamen an. Sie können die Liste aller abgefragten Domänennamen und Details von Suchanforderungen für einen bestimmten Domänennamen in der Protokollsuche anzeigen.

![Blatt „Abgefragte Domänen“](./media/log-analytics-dns/domains-queried-blade.png)

**DNS-Clients**: Gibt die Clients an, die **den Schwellenwert** für die Anzahl von Abfragen im gewählten Zeitraum verletzen. Sie können die Liste aller DNS-Clients und die Details der von ihnen vorgenommenen Abfragen in der Protokollsuche anzeigen.

![Blatt „DNS-Clients“](./media/log-analytics-dns/dns-clients-blade.png)

**Dynamische DNS-Registrierungen**: Gibt Fehler bei der Registrierung von Namen an. Alle Registrierungsfehler für [Ressourceneinträge](https://en.wikipedia.org/wiki/List_of_DNS_record_types) von Adressen (Typ A und AAAA) werden zusammen mit den Client-IP-Adressen hervorgehoben, von denen die Registrierungsanforderungen stammen. Mit diesen Informationen können Sie dann die eigentliche Ursache des Registrierungsfehlers finden:

1. Suchen Sie die Zone, die für den Namen autoritativ ist, den der Client aktualisieren möchte.
2. Verwenden Sie die Lösung, um die Inventarinformationen dieser Zone zu überprüfen.
3. Stellen Sie sicher, dass die dynamische Aktualisierung für die Zone aktiviert ist.
4. Überprüfen Sie, ob die Zone für die sichere dynamische Aktualisierung konfiguriert ist.

![Blatt „Dynamische DNS-Registrierungen“](./media/log-analytics-dns/dynamic-dns-reg-blade.png)

**Anforderungen zur Namensregistrierung**: Auf der oberen Kachel wird ein Trend bei der Anzahl von erfolgreichen und fehlgeschlagenen dynamischen DNS-Aktualisierungsanforderungen angezeigt. Auf der unteren Kachel sind die zehn Clients aufgeführt, die die meisten fehlgeschlagenen DNS-Aktualisierungsanforderungen an die DNS-Server gesendet haben, sortiert nach der Anzahl von Fehlern.

![Blatt „Anforderungen zur Namensregistrierung“ ](./media/log-analytics-dns/name-reg-req-blade.png)

**DNS Analytics-Beispielabfragen**: Enthält eine Liste der am häufigsten verwendeten Suchabfragen, mit denen unformatierte Analysedaten direkt abgerufen werden.

![Beispielabfragen](./media/log-analytics-dns/queries.png)

Sie können diese Abfragen als Ausgangspunkt für das Erstellen eigener Abfragen für benutzerdefinierte Berichte verwenden.

- **Serverliste:** Dieser Link öffnet die Seite für die DNS-Protokollsuche, auf der eine Liste aller DNS-Server mit den zugehörigen FQDNs, Domänennamen, Gesamtstrukturnamen und Server-IP-Adressen angezeigt wird.
- **Liste der DNS-Zonen:** Dieser Link öffnet die Seite für die DNS-Protokollsuche, auf der eine Liste aller DNS-Zonen mit den zugehörigen Zonennamen, Angaben zum dynamischen Aktualisierungsstatus, Namenserver und Angaben zum DNSSEC-Signaturstatus angezeigt werden.
- **Nicht verwendete Ressourceneinträge:** Dieser Link öffnet die Seite für die DNS-Protokollsuche und zeigt eine Liste der nicht verwendeten/veralteten Ressourceneinträge an. Diese Liste enthält den Namen und den Typ des Ressourceneintrags, den zugeordneten DNS-Server, den Zeitpunkt der Generierung und den Zonennamen. Sie können mit dieser Liste die DNS-Ressourceneinträge identifizieren, die nicht mehr verwendet werden. Auf der Basis dieser Informationen können Sie dann diese Einträge von den DNS-Servern entfernen.
- **Abfragelast der DNS-Server:** Dieser Link öffnet die Seite für die DNS-Protokollsuche, auf der Sie einen Eindruck von der DNS-Last auf den DNS-Servern erhalten, der Ihnen beim Planen der Kapazität für die Server hilft. Sie können zur Registerkarte **Metriken** wechseln, um die Ansicht in eine grafische Visualisierung zu ändern. Mit dieser Ansicht können Sie nachvollziehen, wie die DNS-Last auf die DNS-Server verteilt wird. Sie zeigt Trends der DNS-Abfragerate für jeden Server.
    ![Protokollsuchergebnisse von Abfragen für DNS-Server](./media/log-analytics-dns/dns-servers-query-load.png)  
- **Abfragelast von DNS-Zonen:** Dieser Link öffnet die Seite für die DNS-Protokollsuche, auf der Sie Statistiken zu den Abfragen pro Sekunde in DNS-Zonen für alle Zonen auf den DNS-Servern sehen, die von der Lösung verwaltet werden. Klicken Sie auf die Registerkarte **Metriken**, um die Ansicht von ausführlichen Datensätzen in eine grafische Visualisierung der Ergebnisse zu ändern.
- **Konfigurationsereignisse:** Dieser Link öffnet die Seite für die DNS-Protokollsuche, auf der Sie alle DNS-Konfigurationsänderungsereignisse und die zugehörigen Meldungen sehen können. Sie können dann diese Ereignisse basierend auf dem Zeitpunkt des Ereignisses, der Ereignis-ID, des DNS-Servers oder der Aufgabenkategorie filtern. Auf diese Weise können Sie Änderungen an bestimmten DNS-Servern zu bestimmten Zeitpunkten überprüfen.
- **DNS-Analyseprotokoll:** Dieser Link öffnet die Seite für die DNS-Protokollsuche, auf der alle Analyseereignisse auf allen DNS-Servern angezeigt werden, die mit der Lösung verwaltet werden. Sie können dann diese Ereignisse basierend auf dem Zeitpunkt des Ereignisses, der Ereignis-ID, des DNS-Servers, der Client-IP-Adresse, von der die Suchabfrage stammt, und der Aufgabenkategorie des Abfragetyps filtern. Mit Analyseereignissen für DNS-Server können Aktivitäten auf dem DNS-Server überwacht werden. Ein Analyseereignis wird jedes Mal protokolliert, wenn der Server DNS-Informationen sendet oder empfängt.

### <a name="dns-log-search"></a>DNS-Protokollsuche

Auf der Seite Search können Sie eine Abfrage erstellen und die Ergebnisse dann bei der Suche anhand von Facetsteuerelementen filtern. Sie können auch erweiterte Abfragen zum Transformieren, Filtern und Berichten für Ihre Ergebnisse erstellen. Sie können mithilfe der folgenden Abfragen beginnen.

Geben Sie im Feld für die Suchabfrage `Type=DnsEvents` ein, um alle DNS-Ereignisse anzuzeigen, die von den DNS-Servern generiert wurden, die von der Lösung verwaltet werden. Die Ergebnisse listen Protokolldaten für alle Ereignisse im Zusammenhang mit Suchabfragen, dynamischen Registrierungen und Konfigurationsänderungen auf.

![DnsEvents-Protokollsuche](./media/log-analytics-dns/log-search-dnsevents.png)  

- Um die Protokolldaten für die Suchabfrage anzuzeigen, filtern Sie im LHS-Facetsteuerelement nach dem Untertyp **LookUpQuery**. Eine Liste/Tabelle mit den Ereignissen zu Suchabfragen für den ausgewählten Zeitraum wird angezeigt.
- Um die Protokolldaten für dynamische Registrierungen anzuzeigen, filtern Sie im LHS-Facetsteuerelement nach dem Untertyp **DynamicRegistration**. Eine Liste/Tabelle mit allen Ereignissen zu dynamischen Registrierungen für den ausgewählten Zeitraum wird angezeigt.
- Um die Protokolldaten für Konfigurationsänderungen anzuzeigen, filtern Sie im LHS-Facetsteuerelement nach dem Untertyp **ConfigurationChange**. Eine Liste/Tabelle mit allen Ereignissen zu Konfigurationsänderungen für den ausgewählten Zeitraum wird angezeigt.

Geben Sie im Feld für die Suchabfrage `Type=DnsInventory` ein, um alle DNS-Inventardaten für die DNS-Server anzuzeigen, die von der Lösung verwaltet werden. Die Ergebnisse listen die Protokolldaten für DNS-Server, DNS-Zonen und Ressourceneinträge auf.
![DnsInventory-Protokollsuche](./media/log-analytics-dns/log-search-dnsinventory.png)

## <a name="feedback"></a>Feedback

Es gibt verschiedene Möglichkeiten, Feedback zu senden:

- **UserVoice**: Teilen Sie Ideen zu wünschenswerten DNS Analytics-Features mit. Besuchen Sie die [UserVoice-Seite für OMS](https://aka.ms/dnsanalyticsuservoice).
- **Treten Sie unserer Gruppe bei**: Wir sind immer an neuen Kunden interessiert, die unseren Gruppen beitreten, um frühen Zugriff auf neue Features zu erhalten und uns bei der weiteren Verbesserung von DNS Analytics zu helfen. Wenn Sie den Gruppen beitreten möchten, füllen Sie diesen [kurzen Fragebogen](https://aka.ms/dnsanalyticssurvey) aus.

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über [Protokollsuchen](log-analytics-log-searches.md) für die Anzeige ausführlicher DNS-Protokolldatensätze.

