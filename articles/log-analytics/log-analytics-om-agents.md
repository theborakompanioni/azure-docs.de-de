<properties
	pageTitle="Herstellen einer Verbindung zwischen Operations Manager und Log Analytics | Microsoft Azure"
	description="Zur Bewahrung Ihrer bisherigen Investitionen in System Center Operations Manager sowie zur Nutzung erweiterter Funktionen mit Log Analytics können Sie Operations Manager mit Ihrem OMS-Arbeitsbereich verknüpfen."
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
	ms.date="04/28/2016"
	ms.author="banders"/>

# Herstellen einer Verbindung zwischen Operations Manager und Log Analytics

Zur Bewahrung Ihrer bisherigen Investitionen in System Center Operations Manager sowie zur Nutzung erweiterter Funktionen mit Log Analytics können Sie Operations Manager mit Ihrem OMS-Arbeitsbereich verknüpfen. Die Kombination der Möglichkeiten von OMS mit den Vorteilen von Operations Manager ermöglicht Folgendes:

- Weitere Verwendung der Integritätsüberwachung für Ihre IT-Dienste mit Operations Manager
- Verwaltung der Integration mit Ihren ITSM-Lösungen (mit Unterstützung von Incident Management und Problemverwaltung)
- Verwaltung des Lebenszyklus von Agents, die auf lokalen und öffentlichen Cloud-IaaS-VMs bereitgestellt und mit Operations Manager überwacht werden

Die Verknüpfung mit System Center Operations Manager kommt Ihrer Dienstbetriebsstrategie zugute, da Sie dadurch beim Sammeln, Speichern und Analysieren von Daten aus Operations Manager von der Geschwindigkeit und Effizienz von OMS profitieren. OMS ist hilfreich beim Korrelieren und Ermitteln problembedingter Fehler sowie beim Ermitteln von Wiederholungen und unterstützt somit Ihren vorhandenen Problemverwaltungsprozess. Die Flexibilität des Suchmoduls bei der Untersuchung von Leistungs-, Ereignis- und Warnungsdaten sowie umfassende Dashboards und Berichtsfunktionen zur sinnvollen Betrachtung der Daten machen OMS zu einer optimalen Ergänzung von Operations Manager.

Die Agents, die Daten an die Verwaltungsgruppe von Operations Manager melden, sammeln Daten von Ihren Servern auf der Grundlage der Log Analytics-Datenquellen und -Lösungen, die Sie in Ihrem OMS-Abonnement aktiviert haben. Je nach aktivierter Lösung werden Daten aus diesen Lösungen entweder direkt von einem Operations Manager-Verwaltungsserver oder (aufgrund des Umfangs der Daten, die auf dem mit Agent verwalteten System gesammelt werden) direkt vom Agent an den OMS-Webdienst gesendet. Der Verwaltungsserver leitet die OMS-Daten direkt an den OMS-Webdienst weiter; sie werden gar nicht erst in die OperationsManager- oder OperationsManagerDW-Datenbank geschrieben. Sollte die Verbindung zwischen einem Verwaltungsserver und dem OMS-Webdienst getrennt werden, werden die Daten lokal zwischengespeichert, bis die Kommunikation mit OMS wiederhergestellt ist. Ist der Verwaltungsserver aufgrund einer geplanten Wartung oder eines ungeplanten Ausfalls offline, wird die Verbindung mit OMS von einem anderen Verwaltungsserver in der Verwaltungsgruppe übernommen.

Das folgende Diagramm veranschaulicht die Beziehung zwischen System Center Operations Manager und OMS:

![oms-operations-manager-integration-diagram](./media/log-analytics-om-agents/oms-operations-manager-connection.png)

## Systemanforderungen
Vergewissern Sie sich zunächst anhand der folgenden Informationen, dass Sie über die erforderlichen Komponenten verfügen.

- OMS unterstützt nur Operations Manager 2012 SP1 UR6 und Operations Manager 2012 R2 UR2 sowie jeweils höhere Versionen. Proxyunterstützung wurde in Operations Manager 2012 SP1 UR7 und Operations Manager 2012 R2 UR3 hinzugefügt.
- Ein OMS-Abonnement. Weitere Informationen finden Sie unter [Erster Eindruck: Erste Schritte mit der OMS](log-analytics-get-started.md).

## Herstellen einer Verbindung zwischen Operations Manager und OMS
Führen Sie die folgenden Schritte aus, um Ihre Operations Manager-Verwaltungsgruppe mit einem Ihrer OMS-Arbeitsbereiche zu verknüpfen.

1. Wählen Sie in der Operations Manager-Konsole den Arbeitsbereich **Administration** aus.
2. Erweitern Sie den Knoten für Operations Management Suite, und klicken Sie auf **Verbindung**.
3. Klicken Sie auf den Link **Register to Operations Management Suite** (Bei Operations Management Suite registrieren).
4. Geben Sie auf der Seite **Operations Management Suite Onboarding Wizard: Authentication** (Assistent zum Integrieren von Operations Management Suite: Authentifizierung) die E-Mail-Adresse oder Telefonnummer und das Kennwort des mit Ihrem OMS-Abonnement verknüpften Administratorkontos ein, und klicken Sie auf **Anmelden**.
5. Nach erfolgreicher Authentifizierung werden Sie auf der Seite **Operations Management Suite Onboarding Wizard: Select Workspace** (Assistent zum Integrieren von Operations Management Suite: Arbeitsbereich auswählen) zum Auswählen Ihres OMS-Arbeitsbereichs aufgefordert. Falls Sie über mehrere Arbeitsbereiche verfügen, wählen Sie in der Dropdownliste den Arbeitsbereich aus, den Sie in der Operations Manager-Verwaltungsgruppe registrieren möchten, und klicken Sie anschließend auf **Weiter**.
    >[AZURE.NOTE] Operations Manager unterstützt immer nur einen einzelnen OMS-Arbeitsbereich. Die Verbindung und Computer, die bei OMS mit dem vorherigen Arbeitsbereich registriert wurden, werden von OMS entfernt.
6. Überprüfen Sie auf der Seite **Operations Manager Suite Onboarding Wizard: Summary** (Assistent zum Integrieren von Operations Management Suite: Zusammenfassung) Ihre Einstellungen, und klicken Sie auf **Erstellen**, wenn die Einstellungen korrekt sind.
7. Klicken Sie auf der Seite **Operations Management Suite Onboarding Wizard: Finish** (Assistent zum Integrieren von Operations Management Suite: Fertig stellen) auf **Schließen**.

### Hinzufügen von mit Agent verwalteten Computern
Nach dem Konfigurieren der Integration in Ihren OMS-Arbeitsbereich wird lediglich eine Verbindung mit OMS hergestellt, es werden aber keine Daten von den Agents gesammelt, die Daten an Ihre Verwaltungsgruppe melden. Dazu müssen Sie zuerst festlegen, welche mit Agent verwalteten Computer Daten für Log Analytics sammeln sollen. Sie können die Computerobjekte entweder einzeln oder aber eine Gruppe mit Windows-Computerobjekten auswählen. Sie können keine Gruppe auswählen, die Instanzen einer anderen Klasse enthält (etwa logische Datenträger oder SQL-Datenbanken).

1. Öffnen Sie die Operations Manager-Konsole, und wählen Sie den Arbeitsbereich **Administration** aus.
2. Erweitern Sie den Knoten für Operations Management Suite, und klicken Sie auf **Verbindung**.
3. Klicken Sie unter der Überschrift „Aktionen“ (rechts im Bereich) auf den Link **Computer/Gruppe hinzufügen**.
4. Im Dialogfeld **Computersuche** können Sie nach Computern oder Gruppen suchen, die von Operations Manager überwacht werden. Wählen Sie Computer oder Gruppen aus, die in OMS aufgenommen werden sollen, klicken Sie auf **Hinzufügen**, und klicken Sie dann auf **OK**.

Computer und Gruppen, die unter Operations Management Suite zum Sammeln von Daten aus dem Knoten für verwaltete Computer konfiguriert sind, können in der Betriebskonsole im Arbeitsbereich **Administration** angezeigt werden. Hier können Sie Computer und Gruppen nach Bedarf hinzufügen und entfernen.

### Konfigurieren von OMS-Proxyeinstellungen in der Betriebskonsole
Führen Sie die folgenden Schritte aus, wenn sich zwischen Verwaltungsgruppe und OMS-Webdienst ein interner Proxyserver befindet. Diese Einstellungen werden zentral über die Verwaltungsgruppe verwaltet und an mit Agent verwaltete Systeme verteilt, die im Datensammlungsbereich für OMS enthalten sind. Dies ist von Vorteil, wenn bestimmte Lösungen den Verwaltungsserver umgehen und Daten direkt an den OMS-Webdienst senden.

1. Öffnen Sie die Operations Manager-Konsole, und wählen Sie den Arbeitsbereich **Administration** aus.
2. Erweitern Sie „Operations Management Suite“, und klicken Sie auf **Verbindungen**.
3. Klicken Sie in der Ansicht für die OMS-Verbindung auf **Proxyserver konfigurieren**.
4. Wählen Sie auf der Seite **Operations Management Suite Wizard: Proxy Server** (Operations Management Suite-Assistent: Proxyserver) die Option **Proxyserver für Zugriff auf Operations Management Suite verwenden** aus, geben Sie die URL mit der Portnummer ein (beispielsweise http://corpproxy:80), und klicken Sie anschließend auf **Fertig stellen**.

Falls Ihr Proxyserver eine Authentifizierung erfordert, führen Sie die folgenden Schritte aus, um Anmeldeinformationen und Einstellungen zu konfigurieren, die an verwaltete Computer weitergegeben werden müssen, die in der Verwaltungsgruppe Daten an OMS melden.

1. Öffnen Sie die Operations Manager-Konsole, und wählen Sie den Arbeitsbereich **Administration** aus.
2. Wählen Sie unter **RunAs Configuration** die Option **Profile** aus.
3. Öffnen Sie das Profil **System Center Advisor Run As Profile Proxy** (System Center Advisor – ausführendes Profil – Proxy).
4. Klicken Sie im Assistenten für das ausführende Profil auf „Hinzufügen“, um ein ausführendes Konto zu verwenden. Sie können ein neues [ausführendes Konto](https://technet.microsoft.com/library/hh321655.aspx) erstellen oder ein vorhandenes Konto verwenden. Dieses Konto muss über ausreichende Berechtigungen für die Weiterleitung über den Proxyserver verfügen.
5. Wählen Sie zum Festlegen des zu verwaltenden Kontos die Option **Einer bestimmten Klasse oder Gruppe bzw. eines bestimmten Objekts** aus, klicken Sie auf **Auswählen...**, und klicken Sie anschließend auf **Gruppe…**, um das Feld **Gruppensuche** zu öffnen.
6. Suchen Sie nach **Microsoft System Center Advisor-Überwachungsservergruppe**, und wählen Sie sie dann aus. Klicken Sie nach dem Auswählen der Gruppe auf **OK**, um das Feld **Gruppensuche** zu schließen.
7.	Klicken Sie auf **OK**, um das Feld **Ausführendes Konto hinzufügen** zu schließen.
8.	Klicken Sie auf **Speichern**, um den Assistenten abzuschließen und Ihre Änderungen zu speichern.

Nachdem die Verbindung erstellt wurde und Sie die Agents konfiguriert haben, die Daten sammeln und an OMS melden sollen, wird in der Verwaltungsgruppe die folgende Konfiguration angewendet (nicht zwingend in der hier angegebenen Reihenfolge):

- Das ausführende Konto **Microsoft.SystemCenter.Advisor.RunAsAccount.Certificate** wird erstellt. Es wird mit dem ausführenden Profil **Microsoft System Center Advisor Run As Profile Blob** (Microsoft System Center Advisor – ausführendes Profil – Blob) verknüpft und auf zwei Klassen ausgerichtet: **Collection Server** und **Operations Manager Management Group**.
- Zwei Connectors werden erstellt. Der erste heißt **Microsoft.SystemCenter.Advisor.DataConnector** und wird automatisch mit einem Abonnement konfiguriert, das alle Warnungen, die von Instanzen aller Klassen in der Verwaltungsgruppe generiert werden, an OMS Log Analytics weiterleitet. Der zweite Connector heißt **Advisor Connector** und ist für die Kommunikation mit dem OMS-Webdienst und für die Weitergabe von Daten zuständig.
- Agents und Gruppen, die Sie zum Sammeln von Daten in der Verwaltungsgruppe ausgewählt haben, werden der Microsoft System Center Advisor-Überwachungsservergruppe hinzugefügt.

## Management Pack-Updates
Nach Abschluss der Konfiguration stellt die Operations Manager-Verwaltungsgruppe eine Verbindung mit dem OMS-Dienst her. Der Verwaltungsserver wird mit dem Webdienst synchronisiert und erhält aktualisierte Konfigurationsinformationen in Form von Management Packs für die aktivierten Lösungen, die mit Operations Manager verknüpft werden. Operations Manager sucht nach Updates für diese Management Packs. Verfügbare Updates werden automatisch herunterladen und importiert. Dieses Verhalten wird insbesondere durch zwei Regeln gesteuert:

- **Microsoft.SystemCenter.Advisor.MPUpdate**: Aktualisiert die grundlegenden OMS-Management Packs. Wird standardmäßig alle 12 Stunden ausgeführt.
- **Microsoft.SystemCenter.Advisor.Core.GetIntelligencePacksRule**: Aktualisiert lösungsbezogene, in Ihrem Arbeitsbereich aktivierte Management Packs. Wird standardmäßig alle fünf Minuten ausgeführt.

Sie können diese beiden Regeln überschreiben und so entweder das automatische Herunterladen verhindern, indem Sie sie deaktivieren, oder die Häufigkeit ändern, mit der der Verwaltungsserver eine Synchronisierung mit OMS durchführt, um zu ermitteln, ob ein neues Management Pack verfügbar ist und heruntergeladen werden soll. Führen Sie die Schritte unter [How to Override a Rule or Monitor](https://technet.microsoft.com/library/hh212869.aspx) (Gewusst wie: Überschreiben einer Regel oder Überwachung) aus, um einen Sekundenwert für den Häufigkeitsparameter anzugeben und so den Synchronisierungszeitplan zu ändern, oder ändern Sie den Aktivierungsparameter, um die Regeln zu deaktivieren. Verwenden Sie als Ziel für die Überschreibungen alle Objekte der Klasse „Operations Manager Management Group“.

Wenn Sie zur Steuerung von Management Pack-Versionen in Ihrer Produktionsverwaltungsgruppe weiterhin Ihren vorhandenen Änderungssteuerungsprozess verwenden möchten, können Sie die Regeln deaktivieren und sie während bestimmter Zeiten, in denen Updates zulässig sind, wieder aktivieren. Falls Ihre Umgebung eine Entwicklungs- oder QA-Verwaltungsgruppe mit Internetzugriff enthält, können Sie diese Verwaltungsgruppe zur Unterstützung dieses Szenarios mit einem OMS-Arbeitsbereich konfigurieren. Dadurch können Sie die iterativen Versionen der OMS-Management Packs vor der Freigabe für die Produktionsverwaltungsgruppe überprüfen und evaluieren.

## Überprüfen der Operations Manager-Integration für OMS
Es gibt verschiedene Methoden, um zu überprüfen, ob die OMS-Integration in Operations Manager erfolgreich war.

### Überprüfen der Integration über das OMS-Portal

1.	Klicken Sie im OMS-Portal auf die Kachel **Einstellungen**.
2.	Klicken Sie im oberen Menü auf **Connected Sources** (Verbundene Quellen).
3.	Im Abschnitt für System Center Operations Manager sollte der Status **1 MGMT Group Connected** (1 Verwaltungsgruppe verbunden) angezeigt werden. Die Tabelle darunter enthält den Namen der Verwaltungsgruppe sowie die Anzahl von Agents und die Angabe, wann zuletzt Daten empfangen wurden.

### Überprüfen der Integration über die Betriebskonsole

1.	Öffnen Sie die Operations Manager-Konsole, und wählen Sie den Arbeitsbereich **Administration** aus.
2.	Klicken Sie auf den Knoten **Management Packs**, und geben Sie im Textfeld für die Suche die Zeichenfolge **Advisor** oder **Intelligence** ein.
3.	Abhängig von den aktivierten Lösungen wird in den Suchergebnissen ein entsprechendes Management Pack aufgeführt. Wenn Sie also etwa die Lösung „Alert Management“ aktiviert haben, enthält die Liste das Management Pack „Microsoft System Center Advisor Alert Management“.

## Nächste Schritte

- [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md) (Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog) beschreibt das Hinzufügen von Funktionen und das Sammeln von Daten.
- [Configure proxy and firewall settings in Log Analytics](log-analytics-proxy-firewall.md) (Konfigurieren von Proxy- und Firewalleinstellungen in Log Analytics) beschreibt, wie Sie vorgehen müssen, damit die Agents mit dem Log Analytics-Dienst kommunizieren können, wenn Ihre Organisation einen Proxyserver oder eine Firewall verwendet.

<!---HONumber=AcomDC_0504_2016-->