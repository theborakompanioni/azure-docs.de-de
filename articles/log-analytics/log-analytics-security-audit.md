<properties
	pageTitle="Sicherheits- und Überwachungslösung in Log Analytics | Microsoft Azure"
	description="Die Sicherheits- und Überwachungslösung in Log Analytics bietet mit integrierten Suchabfragen für relevante Probleme, die Ihre Aufmerksamkeit erfordern, einen umfassenden Einblick in die Lage der IT-Sicherheit Ihres Unternehmens."
	services="log-analytics"
	documentationCenter=""
	authors="bandersmsft"
	manager="jwhit"
	editor=""/>

<tags
	ms.service="log-analytics"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/03/2016"
	ms.author="banders"/>

# Sicherheits- und Überwachungslösung in Log Analytics

Die Sicherheits- und Überwachungslösung in Log Analytics bietet mit integrierten Suchabfragen für relevante Probleme, die Ihre Aufmerksamkeit erfordern, einen umfassenden Einblick in die Lage der IT-Sicherheit Ihres Unternehmens.

## Installieren und Konfigurieren der Lösung
Verwenden Sie die folgenden Informationen zum Installieren und Konfigurieren der Lösung.

- Fügen Sie die Sicherheits- und Überwachungslösung mithilfe des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md) beschriebenen Prozesses Ihrem OMS-Arbeitsbereich hinzu. Es ist keine weitere Konfiguration erforderlich.



## Details zur Datenerfassung mit der Sicherheits- und Überwachungslösung

Die folgende Tabelle enthält die Datenerfassungmethoden und andere Details dazu, wie Daten für die Sicherheits- und Überwachungslösung erfasst werden.

|Datentyp| Plattform | Direkt-Agent | SCOM-Agent | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
|---|---|---|---|---|---|---|---|
|Windows-Sicherheitsereignisprotokolle|Windows|![Ja](./media/log-analytics-security-audit/oms-bullet-green.png)|![Ja](./media/log-analytics-security-audit/oms-bullet-green.png)|![Ja](./media/log-analytics-security-audit/oms-bullet-green.png)| ![Nein](./media/log-analytics-security-audit/oms-bullet-red.png)|![Nein](./media/log-analytics-security-audit/oms-bullet-red.png)| Für Azure-Speicher: 10 Minuten; für Agent: bei Ankunft|
|Windows-Firewallprotokolle|Windows|![Ja](./media/log-analytics-security-audit/oms-bullet-green.png)|![Ja](./media/log-analytics-security-audit/oms-bullet-green.png)|![Nein](./media/log-analytics-security-audit/oms-bullet-red.png)| ![Nein](./media/log-analytics-security-audit/oms-bullet-red.png)|![Nein](./media/log-analytics-security-audit/oms-bullet-red.png)| Bei Ankunft|
|Windows-Ereignisprotokolle|Windows|![Ja](./media/log-analytics-security-audit/oms-bullet-green.png)|![Ja](./media/log-analytics-security-audit/oms-bullet-green.png)|![Ja](./media/log-analytics-security-audit/oms-bullet-green.png)| ![Nein](./media/log-analytics-security-audit/oms-bullet-red.png)|![Ja](./media/log-analytics-security-audit/oms-bullet-green.png)| Für Azure-Speicher: 1 Minute; für Agent: bei Ankunft|


## Bewährte Methoden für die forensische Analyse

**Spurensuche**

Wenn Sie mithilfe der Sicherheits- und Überwachungslösung eine forensische Analyse durchführen, suchen Sie nach Spuren, die potenziell böswillige Benutzer hinterlassen. Unabhängig davon, was Benutzer in ihrer IT-Umgebung tun, generieren viele ihrer Aktivitäten Sicherheitsartefakte. Beachten Sie, dass Angreifer den Inhalt der Protokolle oft löschen. Dies ist meist der erste Schritt, mit dem sie ihre Spur verwischen wollen.

Ganz gleich, ob die Benutzer direkt oder per Remote auf ihre Computer zugreifen, der Zugriff wird in Ereignisprotokollen gespeichert. OMS sammelt diese Artefakte *bei ihrem Auftreten*, noch bevor sie jemand manipulieren kann, und ermöglicht Ihnen, mittels Datenkorrelation über mehrere Computer verschiedene Analysen durchzuführen.

**Grundlegendes zum Konfigurieren und Erfassen von Überwachungsereignissen**

Um die Sicherheits- und Überwachungslösung optimal zu nutzen, sollten Sie mithilfe der folgenden Webressourcen die Ebene der von Ihrer Windows-Umgebung erhobenen Überwachungsereignisse so konfigurieren, dass sie Ihren Anforderungen am besten entspricht.

- [Konfigurieren der Sicherheitsrichtlinieneinstellungen](https://technet.microsoft.com/library/dn135243&#40;v=ws.10&#41;.aspx)
- [Erweiterte Überwachungsrichtlinienkonfiguration](https://technet.microsoft.com/library/jj852202&#40;v=ws.10&#41;.aspx)
- [Empfehlungen zu Überwachungsrichtlinien](https://technet.microsoft.com/library/dn487457.aspx)

**Aktivieren von AppLocker**

Sie sollten auch AppLocker-Ereignisse aktivieren, um umfassende Informationen über Prozessausführungen in Ihrer IT-Umgebung zu erhalten. Weitere Informationen finden Sie unter [Konfigurieren einer AppLocker-Richtlinie ausschließlich zur Überwachung](https://technet.microsoft.com/library/hh994622.aspx).

**Konfigurieren der Überwachungsebene für Windows-Ereignisse**

Die Windows-Umgebung ermöglicht Ihnen, den Umfang der sicherheitsrelevanten Aufzeichnungen selbst zu bestimmen. Beispielsweise können Sie die Umgebung so konfigurieren, dass jedes Mal, wenn ein Benutzer auf eine Datei zugreift, sie öffnet oder liest, ein Ereignis erstellt wird. Die zu erfassende Detailmenge ändert sich ganz nach Ihrem Bedarf. Allerdings bedeutet jede weitere aktivierte Option auch höhere Kosten, da die von Ihnen gesammelten Daten alle gespeichert werden müssen. Daher verzichten viele Unternehmen darauf, Lese- oder Schreibvorgänge bei ihren Objekten zu erfassen. Bei jedem einzelnen Dateizugriff können tausende von Ereignissen generiert werden, von denen etliche nutzlos sind. Für welchen Umfang zu erfassender Daten Sie sich entscheiden, hängt von Ihrer Einschätzung ab.

>[AZURE.NOTE] Bei Verwendung des Direkt-Agents muss der Proxyserver Ihres Unternehmens so konfiguriert werden, dass er dem Agent den Zugriff auf OMS erlaubt. Weitere Informationen finden Sie unter [Konfigurieren von Proxy- und Firewalleinstellungen in Log Analytics](log-analytics-proxy-firewall.md).

## Bewährte Methoden für die Untersuchung von Sicherheitsverletzungsmustern

**Untersuchung ungewöhnlicher Aktivitätsmuster**

Sicherheitsverletzungen sind gewöhnlich auf legitime Anmeldedaten zurückzuführen und erfordern, dass ein böswilliger Benutzer versucht, durch Angriffe erhöhte Rechte zu erlangen. Der Schwerpunkt der Sicherheits- und Überwachungslösung liegt nicht auf der Angriffserkennung, sondern die Funktion „Relevante Probleme“ hilft Ihnen dabei, Muster ungewöhnlicher Aktivitäten zu entdecken und zu untersuchen. So sollten Sie folgende und alle weiteren ungewöhnlichen Aktivitäten unter **Relevante Probleme** untersuchen.

- ungewöhnliche Anmeldungen an einem Computer, den der Benutzer normalerweise nicht verwendet
- ungewöhnliche Netzwerkaufzählung von untypischen Benutzern oder Computern
- Erstellung neuer Benutzerkonten mit Administratorrechten
- Änderungen an den Protokollierungs- oder Sicherheitsrichtlinien
- verdächtige ausführbare Dateien

## Bewährte Methoden für Überwachungsszenarien

Sie müssen wahrscheinlich Computer- und Netzwerkkompatibilitätsrichtlinien und -vorschriften Ihres Unternehmens einhalten, die umfassende Überwachungsaufzeichnungen erfordern. Ein Finanzunternehmen zum Beispiel muss wahrscheinlich Aufzeichnungen machen, mit denen sich zu jedem Zeitpunkt nachweisen lässt, welcher Benutzer einen bestimmten Vorgang im Netzwerk durchgeführt hat. Es könnte auch nötig sein, detaillierte Berichte über die Aktivitäten eines bestimmten Benutzers oder ausgewählter Server nach Bedarf zu erstellen – und das zum Teil über viele Monate, manchmal sogar Jahre zurück.

Verwenden Sie die Sicherheits- und Überwachungslösung, um Überwachungsdaten Ihrer gesamten IT-Umgebung zu erfassen – ob von lokalen Computern oder aus der Cloud. Alle Überwachungsdaten werden gemäß Ihrem Abonnement gespeichert, indiziert und beibehalten. Bei einem Premium-Abonnement von OMS wird eine unbegrenzte Menge an Daten ein Jahr lang gespeichert. Sie können die Überwachungsdaten ansehen, Suchvorgänge ausführen und zwischen verschiedenen Datentypen und Computern korrelieren, um umfassende Ergebnisse für jedes Zeitintervall seit Beginn der Datensammlung zu erhalten.

**Verwenden von Gruppenrichtlinien zur Erfassung von Überwachungsdaten**

Alle Überwachungsdaten, die Sie erfassen und an OMS senden möchten, werden vollständig über die Gruppenrichtlinien verwaltet. Sie verwenden sie, um Sicherheitskonfigurationen als Teil eines Gruppenrichtlinienobjekts (GPO) zu definieren, die zu Active Directory-Containern wie Sites, Domänen und Organisationseinheiten verlinkt sind. Sie helfen bei der Verwaltung der Sicherheitseinstellungen. Richtliniendaten werden protokolliert und später dann an den OMS-Dienst gesendet.

**Verwenden von AppLocker zur Erfassung von Überwachungsdaten**

Wenn Sie zusätzlich zu den lokalen Richtlinieneinstellungen AppLocker zur Erfassung der Überwachungsdaten verwenden, werden diese Daten von OMS erfasst, und Sie können sie anzeigen.

## Einfache Untersuchung einer verdächtigen ausführbaren Datei

1. Melden Sie sich an OMS an.
2. Prüfen Sie auf der Seite **Übersicht** die Informationen in der Kachel **Sicherheit und Überwachung** und klicken Sie sie an. ![Bild der Seite "Übersicht"](./media/log-analytics-security-audit/oms-security-audit-dash02.png)
3. Prüfen Sie auf der Seite **Sicherheit und Überwachung** die Informationen auf dem Blatt **RELEVANTE PROBLEME**. Im Beispielbild sehen Sie 6 relevante Probleme für heute, 2 von gestern. In diesem Beispiel gibt es 1 verdächtige ausführbare Datei. Klicken Sie im Blatt **RELEVANTE PROBLEME** auf **Verdächtige ausführbare Dateien**. ![Bild der Seite "Sicherheit und Überwachung"](./media/log-analytics-security-audit/oms-security-audit-dash03.png)
4. In der Suche werden die Abfrage sowie die Ergebnisse für die verdächtige ausführbare Datei, auf die Sie geklickt haben, angezeigt. Im Beispiel gibt es 1 Ergebnis, der Dateihash wird angezeigt. Klicken Sie auf die **DATEIHASH**-ID. ![Bild der Suchergebnisse "Dateihash"](./media/log-analytics-security-audit/oms-security-audit-search01.png)
5. In der Suche werden nun weitere Informationen über die ausführbare Datei angezeigt, u.a. der Dateipfad und der Prozessname. Klicken Sie auf **Prozess &lt;Dateiname&gt;**. In diesem Beispiel ist das HEXEDIT.EXE. ![Bild der Suchergebnisse "Prozess"](./media/log-analytics-security-audit/oms-security-audit-search02.png)
6. In der Suche wird die Abfrage um den Prozessnamen in Anführungszeichen ergänzt. "**HEXEDIT.EXE"** in diesem Beispiel. ![Bild der Suchabfrage](./media/log-analytics-security-audit/oms-security-audit-search03.png)
7. Lassen Sie im Feld für die Suchabfrage nur den Prozessnamen und die Anführungszeichen stehen und klicken Sie dann auf das Suchsymbol. ![Bild der detaillierten Suchinformationen](./media/log-analytics-security-audit/oms-security-audit-search04.png)
8. In der Suche werden ausführliche Informationen über den Prozess angezeigt, u.a. der oder die Computer, wo der Prozess lief, das entsprechende Benutzerkonto sowie Datum und Uhrzeit, zu der das Ereignis für den Prozess erstellt wurde.
9. Mit den gewonnenen Informationen können Sie nun die gewünschten Maßnahmen ergreifen. Falls Sie zum Beispiel feststellen, dass es sich bei der ausführbaren Datei um Schadsoftware handelt, können Sie sie von allen betroffenen Computersystemen entfernen lassen. Wenn die ausführbare Datei entfernt wurde und OMS aktuelle Protokoll- und Überwachungsereignisse für Ihr Computersystem erhält, ändern sich die Werte im Blatt RELEVANTE PROBLEME am folgenden Tag.

## Untersuchen von schädlichen IP-Adressen

Eine Methode zum Schützen Ihrer Server besteht in der Sicherstellung, dass diese nicht mit bekannten schädlichen oder verdächtigen IP-Adressen kommunizieren. In OMS verfügen Sie über eine zentrale Ansicht mit allen bekannten schädlichen IP-Adressen, mit denen Ihre verwalteten Server und Clients unter Umständen kommunizieren können. In Zusammenarbeit mit dem Microsoft Threat Intelligence Center (MSTIC) erhält OMS stündliche Updates zu den letzten bekannten schädlichen IP-Adressen und informiert Sie darüber, ob Ihre Server ggf. kompromittiert wurden. Das MSTIC-Team arbeitet mit verschiedenen Threat Intelligence-Partnern zusammen, um diese Liste zusammenzustellen und für den OMS-Dienst bereitzustellen.

Die Ansicht mit den schädlichen IP-Adressen finden Sie in der Sicherheits- und Überwachungslösung im OMS-Portal unter **Distinct Suspicious IP Addresses Accessed** (Zugriff auf unterschiedliche verdächtige IP-Adressen).

Sie können die Kachel öffnen und die vollständige Liste mit den einzelnen verdächtigen IP-Adressen anzeigen, mit denen Ihre Geräte unter Umständen kommunizieren. OMS scannt alle Datenquellen, die Sie an den Dienst senden, z.B.:

- Windows-Firewallprotokolle
- IIS-Protokolle
- WireData


In OMS werden viele Felder angezeigt, die sich auf verdächtige IP-Adressen in Ergebnissen von Protokollsuchen beziehen. Zu den wichtigsten Feldern gehören:

- **IndicatorThreatType**: Beispiele hierfür sind „botnet“, „proxy“, „darknet“, „malware command“ und „control node“.
- **Beschreibung**: Der Bedrohungstyp und der Zuverlässigkeitsgrad, dass die IP-Adresse schädlich ist. Dies sind die gleichen Daten, die auch Microsoft für den eigenen Schutz verwendet. Sie werden für OMS-Benutzer zur Verfügung gestellt.

Eine vollständige Liste mit verdächtigen IP-Adressfeldern finden Sie unten unter [Schädliche IP-Adressen – Felder der Protokollsuche](#Malicious-IP-log-search-fields).

### So führen Sie eine Untersuchung auf schädliche IP-Adressen durch

In diesem Beispiel sehen wir uns eine potenziell schädliche IP-Adresse an, indem wir Windows-Firewall-Protokolldaten verwenden.

1. Melden Sie sich an OMS an.
2. Prüfen Sie auf der Seite **Übersicht** die Informationen in der Kachel **Sicherheit und Überwachung** und klicken Sie sie an. ![Bild der Seite "Übersicht"](./media/log-analytics-security-audit/oms-security-audit-dash02.png)  
3. Sehen Sie sich auf der Seite **Security and Audit** (Sicherheit und Überwachung) die Zusammenfassung der Informationen auf der Kachel **Distinct Suspicious IP Addresses Accessed** (Zugriff auf unterschiedliche verdächtige IP-Adressen) an. ![Seite „Sicherheit und Überwachung“ mit verdächtiger IP-Adresse](./media/log-analytics-security-audit/oms-maliciousip-01.png)  
4. In diesem Beispiel sind sechs verdächtige IP-Adressen angegeben. Klicken Sie auf die Kachel**Distinct Suspicious IP Addresses Accessed** (Zugriff auf unterschiedliche verdächtige IP-Adressen).
5. Die Suche zeigt die Abfrage und die Ergebnisse für die gefundenen schädlichen IP-Adressen an. Das Beispiel enthält sechs Ergebnisse, und die IP-Adressen werden entsprechend angezeigt. ![Suchergebnisse mit verdächtigen IP-Adressen](./media/log-analytics-security-audit/oms-maliciousip-02-revised.png)  
6. Beachten Sie in der Abbildung oben die letzte IP-Adresse. In diesem Beispiel ist dies **94.102.56.130**. Klicken Sie auf diese Adresse.
7. Die Suche zeigt ausführliche Ergebnisse und verschiedene Bedrohungsindikatordaten an. Klicken Sie auf **Mehr anzeigen**, um alle Ergebnisse anzuzeigen. ![Suchergebnisse mit ausführlichen Bedrohungsindikatordaten](./media/log-analytics-security-audit/oms-maliciousip-03-revised.png)  
8. Wenn Sie eine Liste mit allen Servern der Umgebung anzeigen möchten, die unter Umständen mit einer schädlichen IP-Adresse kommunizieren, können Sie die folgende Protokollsuchabfrage verwenden.

    ```
    IsActive=True | measure count() by Computer
    ```

### Korrekturmaßnahmen für die Kommunikation mit schädlichen IP-Adressen

Wenn Sie sicher sind, dass eine Komponente oder ein Prozess in Ihrem Netzwerk mit bekannten schädlichen IP-Adressen kommuniziert, indem Ihre Daten gesendet werden, ist es eindeutig, welche Maßnahme Sie treffen sollten:

- Vergewissern Sie sich, dass Ihre Ressource Daten an die schädliche IP-Adresse sendet.
- Versuchen Sie es nach Möglichkeit mit der Entfernung oder Blockierung der Software von dem Computer, von dem Ihre Daten gesendet werden, und verhindern Sie die Kommunikation mit bestimmten IP-Adressen für bestimmte Software.
- Ermitteln Sie, ob die Benutzeranmeldeinformationen, mit denen eine ausführbare Datei oder ein Prozess ausgeführt wird, kompromittiert wurden, indem Sie alle Konten untersuchen, unter denen der fragliche Prozess ausgeführt wird. Ermitteln Sie, auf welche anderen Ressourcen damit ggf. zugegriffen wurde.
- Ermitteln Sie, wie die Software auf dem Computer installiert wurde.
- Lassen Sie die fragwürdige Software vorerst installiert, damit Sie deren Aktivität genau überwachen können. Treffen Sie nach einer eingehenden Bewertung die erforderlichen Korrekturmaßnahmen.


Falls Sie aber *nicht sicher* sind, ob Ihre Daten an eine bekannte schädliche IP-Adresse gesendet werden, oder wenn die *Zuverlässigkeit der Informationen fraglich ist*, ist es nicht so eindeutig, welche Korrekturmaßnahmen zu treffen sind. Meist ist es ratsam, eine Untersuchung anhand von einigen der oben aufgeführten Schritte durchzuführen. In anderen Fällen kann es ratsam sein, nichts zu unternehmen. Letztendlich sind Sie mit der IT-Infrastruktur in Ihrer Organisation am besten vertraut und können daher am besten bestimmen, wie mit potenziellen Bedrohungen umgegangen werden soll.


In Firewallprotokollen wird die Richtung angegeben, in die sich Daten bewegen. Wenn die Firewall so konfiguriert ist, dass die ausgehende Kommunikation („Senden“ in den Protokollen) an bestimmte IP-Adressen blockiert wird, wird die versuchte Kommunikation von potenziell kompromittierter Software mit schädlichen IP-Adressen in den Ergebnissen der Protokollsuche als *Blockiert* angezeigt. Dies ist ein Fall, in dem Sie weitere Maßnahmen treffen sollten. Auch wenn die kompromittierte Software für eine bestimmte IP-Adresse blockiert ist, kann von der Software zu einem späteren Zeitpunkt der Versuch unternommen werden, mit anderen unbekannten und potenziell schädlichen IP-Adressen zu kommunizieren.

In Firewallprotokollen wird auch angezeigt, wann eingehende Daten blockiert werden, und für diesen Fall kann es zu vermehrter Aktivität kommen. Sollten Sie sich deshalb Sorgen machen? Wahrscheinlich eher nicht. Wenn in Ihren Firewallprotokollen blockierte eingehende Daten angezeigt werden, verdeutlicht dies, dass die Firewall ihre Aufgabe erfüllt, nämlich das Schützen Ihrer Infrastruktur.


### Threat Intelligence für schädliche IP-Adressen

Microsoft tauscht Daten mit vielen Organisationen aus, um eine Übersicht mit allen bekannten Indikatoren erstellen zu können, mit denen Daten ggf. kompromittiert werden. Die Übersicht dient zum Identifizieren von Treffern in Datasets und zum Anzeigen erweiterter Bedrohungsdetails als Links, damit leicht weitere Details angezeigt werden können. Sie zeigen diese Threat Intelligence-Daten in den Ergebnissen der Protokollsuche für schädliche IP-Adressen an.

Es gibt drei Methoden, wie Microsoft Bedrohungsdaten erhält, die dann zu einem Threat Intelligence-Dataset zusammengefasst werden. Diese Anbieter lassen sich grob in die folgenden Kategorien unterteilen:

- Kostenpflichtige Abonnements: Unternehmen, mit denen Microsoft einen Vertrag über den Kauf ihrer Daten abgeschlossen hat.
- Partnerdaten: Dies sind Daten von Security Intelligence-Firmen. Sie können sich diese Daten wie „Aufklärungsdaten“ (Signals Intelligence) vorstellen, da sie keine ausführlichen Berichte enthalten. Die Daten werden aber als gültig angesehen.
- Interne Microsoft-Prozesse: Hierbei geht es vor allem um das Microsoft Threat Intelligence Center (MSTIC).

#### Schädliche IP-Adressen – Felder der Protokollsuche

Die folgenden Felder werden in den Ergebnissen der Protokollsuche für schädliche IP-Adressen angezeigt.

|Suchfeld| Beschreibung |
|---|---|
| **IsActive**| Gibt an, dass der Indikator aktiv ist und als gültiger Treffer angesehen werden sollte, wenn er mit einer gefundenen IP-Adresse übereinstimmt. Dieser Indikator wird von MSTIC verwaltet.|
|**Confidence**| Hat einen Wert von 1 bis 100. MSTIC verwaltet diesen Wert und die Algorithmen, mit denen der Wert definiert wird. Dieser Wert eignet sich gut, um die Suchergebnisse danach zu filtern.|
|**TLPLevel**| Die Protokollebene im Verkehrsampel-Stil. Werte: Grün, Gelb und Rot. Diese Werte stehen für eine geringe geschäftliche Auswirkung, mittlere geschäftliche Auswirkung bzw. hohe geschäftliche Auswirkung.|
| **IndicatorThreatType** | Eine kurze Beschreibung des Bedrohungstyps für den Indikator. Beispiele: Botnet, DDoS, Malware, Malicious URL, Malicious IP, Phishing und Spam.|
| **Severity** | Hat einen Wert von 0 bis 5. So wird angegeben, wie schwerwiegend die Bedrohung ist.|
| **MaliciousIP** | Die IP-Adresse des schädlichen Hosts.|
| **CommunicationDirection** | Gibt die Richtung an, in der IP-Datenverkehr für die schädliche IP-Adresse fließt.|

## Nächste Schritte

- Informieren Sie sich über das [Durchsuchen von Protokollen](log-analytics-log-searches.md) und darüber, wie Sie ausführliche Sicherheits- und Überwachungsdaten anzeigen können.

<!---HONumber=AcomDC_0518_2016-->