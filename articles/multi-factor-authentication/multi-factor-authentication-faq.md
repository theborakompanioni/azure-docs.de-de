<properties
	pageTitle="Azure Multi-Factor Authentication – Häufig gestellte Fragen"
	description="Enthält eine Liste mit häufig gestellten Fragen und Antworten im Zusammenhang mit Azure Multi-Factor Authentication. Multi-Factor Authentication ist eine Methode zum Überprüfen der Identität eines Benutzers, für die Benutzername und Kennwort nicht ausreichen. Sie bietet eine zusätzliche Sicherheitsebene für Benutzeranmeldungen und -transaktionen."
	services="multi-factor-authentication"
	documentationCenter=""
	authors="kgremban"
	manager="femila"
	editor="curtand"/>

<tags
	ms.service="multi-factor-authentication"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/22/2016"
	ms.author="kgremban"/>

# Azure Multi-Factor Authentication – Häufig gestellte Fragen


Dieses FAQ beantwortet häufige Fragen zur Azure Multi-Factor Authentication und zur Nutzung des Multi-Factor Authentication-Diensts, einschließlich Fragen zu Abrechnungsmodell und Verwendbarkeit.

## Allgemein

**Wo erhalte ich Hilfe zur Azure Multi-Factor Authentication?**

- [Durchsuchen der Microsoft Support Knowledge Base](https://www.microsoft.com/de-DE/Search/result.aspx?form=mssupport&q=phonefactor&form=mssupport)

  Durchsuchen Sie die Wissensdatenbank nach Lösungen für häufige technische Probleme.

- [Microsoft Azure Active Directory-Foren](https://social.msdn.microsoft.com/Forums/azure/home?forum=WindowsAzureAD)

  In den [Azure Active Directory-Foren](https://social.msdn.microsoft.com/Forums/azure/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required) können Sie nach technischen Fragen und Antworten aus der Community suchen oder eine eigene Frage stellen.

- [Zurücksetzen des Kennworts](mailto:phonefactorsupport@microsoft.com)

  Wenn Sie ein älterer PhoneFactor-Kunde sind und Fragen haben oder Hilfe beim Zurücksetzen eines Kennworts benötigen, verwenden Sie den Link zur [Kennwortzurücksetzung](mailto:phonefactorsupport@microsoft.com), um eine Supportanfrage zu öffnen.

- [Azure Multi-Factor Authentication-Server (PhoneFactor) – Support](https://support.microsoft.com/oas/default.aspx?prid=14947)

  Verwenden Sie diesen Link, wenn Sie mit einem Microsoft-Supportmitarbeiter Kontakt aufnehmen möchten. Ihnen werden einige Fragen gestellt, um Ihnen zu helfen, Ihre Supportoptionen zu bestimmen, wobei E-Mail, Onlineübermittlung oder Telefonsupport infrage kommen.



**F: Wie werden Benutzerdaten vom Azure Multi-Factor Authentication-Server behandelt?**

Bei Verwendung des Multi-Factor Authentication-Servers werden die Daten eines Benutzers nur auf den lokalen Servern gespeichert. Daten werden nicht dauerhaft in der Cloud gespeichert. Wenn der Benutzer die zweistufige Authentifizierung ausführt, sendet der Multi-Factor Authentication-Server für die Authentifizierung Daten an den Multi-Factor Authentication-Clouddienst. Wenn Authentifizierungsanforderungen an den Clouddienst gesendet werden, werden in der Anforderung und den Protokollen die Daten aus den Feldern der folgenden Liste gesendet, damit sie in den Authentifizierungs- und Verwendungsberichten des Kunden verfügbar sind. Einige der Felder sind optional, und Sie können sie im Multi-Factor Authentication-Server konfigurieren. Zur Kommunikation des Multi-Factor Authentication-Servers mit dem Multi-Factor Authentication-Clouddienst wird Secure Sockets Layer (SSL) oder Transport Layer Security (TLS) über den ausgehenden Port 443 verwendet. Folgende Datenfelder werden in den Protokollen der zweistufigen Authentifizierung berücksichtigt:

- **Eindeutige ID** (entweder Benutzername oder lokale Server-ID der Multi-Factor Authentication)
- **Vor- und Nachname** (optional)
- **E-Mail-Adresse** (optional)
- **Rufnummer** (für eine Anruf- oder SMS-Authentifizierung)
- **Gerätetoken** (für die Authentifizierung einer mobilen App)
- **Authentifizierungsmodus**
- **Authentifizierungsergebnis**
- **Name des Multi-Factor Authentication-Servers**
- **IP des Multi-Factor Authentication-Servers**
- **Client-IP** (falls verfügbar)



Zusätzlich zu diesen Feldern werden auch das Authentifizierungsergebnis (Erfolg oder Ablehnung) und der Grund für etwaige Ablehnungen mit den Authentifizierungsdaten gespeichert. Sie stehen in den Authentifizierungs- und Verwendungsberichten zur Verfügung.




## Abrechnung

**F: Entstehen durch Telefonanrufe oder Textnachrichten für die Benutzerauthentifizierung zusätzliche Kosten für meine Organisation?**

Diese Gebühren werden vollständig durch die Kosten abgedeckt, die pro Benutzer bzw. pro Authentifizierung für den Dienst erhoben werden. Einer Organisation entstehen keine zusätzlichen Kosten für individuelle Telefonanrufe oder Textnachrichten, die über Azure Multi-Factor Authentication an Benutzer gesendet werden. Für die Telefonbesitzer können Roaming-Gebühren oder andere Kosten anfallen, die vom jeweiligen Netzbetreiber für empfangene Telefonanrufe oder Textnachrichten erhoben werden.

**F: Wie werden der Organisation die Kosten für Azure Multi-Factor Authentication in Rechnung gestellt?**

Azure Multi-Factor Authentication ist als eigenständiger Dienst mit Abrechnungsoptionen „Pro Benutzer“ und „Pro Authentifizierung“ oder im Paket mit Azure Active Directory Premium, Enterprise Mobility Suite oder Enterprise Cloud Suite verfügbar. Der eigenständige Dienst wird basierend auf der Nutzung abgerechnet. Die Abrechnung erfolgt entweder monatlich über die Azure-Zahlungsverpflichtungen einer Organisation oder über eine jährliche Lizenz pro Benutzer, die im Rahmen eines Microsoft Enterprise Agreement, des Microsoft Open License-Programms, des Microsoft Cloud-Lösungsanbieter-Programms (Cloud Solution Provider, CSP) oder auch im Direktbezug erhältlich ist.

>[AZURE.IMPORTANT]
Die Regionen in Australien sind für Kunden mit Geschäftspräsenz in Australien und Neuseeland verfügbar.

Abrechnungsmodell | Preis
------------- | ------------- |
Abrechnung pro Benutzer (Azure-Zahlungsverpflichtung)| 1,40 US-Dollar pro Monat (unbegrenzte Authentifizierungen)
Abrechnung pro Authentifizierung (Azure-Zahlungsverpflichtung)|1,40 US-Dollar für 10 Authentifizierungen
Jahreslizenz für einen Benutzer (Direktbezug)|1,40 US-Dollar pro Monat (unbegrenzte Authentifizierungen)
Jahreslizenz für einen Benutzer (Volumenlizenzprogramm)|Wenden Sie sich an Ihren [Enterprise Agreement-Vertreter](https://www.microsoft.com/de-DE/licensing/licensing-programs/enterprise.aspx).

**F: Werden die Kosten für das Abrechnungsmodell „pro Benutzer“ basierend auf der Anzahl der Benutzer ermittelt, die zur Nutzung von Multi-Factor Authentication konfiguriert sind, oder basierend auf der Anzahl der Benutzer, die Überprüfungen durchführen?**

Die Abrechnung basiert auf der Anzahl der Benutzer, die zur Nutzung von Multi-Factor Authentication konfiguriert sind.

**F: Wie funktioniert die Abrechnung für die Multi-Factor Authentication?**

Wenn eine Organisation das Pro-Benutzer- oder Pro-Authentifizierung-Abrechnungs- und Nutzungsmodell verwendet, wählt der Administrator der Organisation den Nutzungstyp aus, wenn er einen Multi-Factor Authentication-Anbieter im klassischen Azure-Portal erstellt. Hierbei handelt es sich um eine Ressource, die wie virtuelle Computer, Websites etc. mit dem Azure-Abonnement des Unternehmens abgerechnet wird. Bei Verwendung des Lizenzmodells werden Lizenzen für Azure Multi-Factor Authentication erworben und wie bei Office 365 und anderen Abonnementprodukten den Benutzern zugewiesen.

**F: Gibt es eine kostenlose Version von Azure Multi-Factor Authentication für Administratoren?**

Ein Teil der Funktionen von Azure Multi-Factor Authentication, „Multi-Factor Authentication für Azure-Administratoren“ genannt, wird Azure Global-Administratorengruppen kostenlos zur Verfügung gestellt, wenn mit der entsprechenden Instanz von Azure Active Directory kein Azure Multi-Factor Authentication-Anbieter für verbrauchsbasierende Abrechnung verknüpft wurde. Bei Verwendung eines Multi-Factor Authentication-Anbieters wird für alle Administratoren und Benutzer in diesem Verzeichnis, die für die Nutzung von Multi-Factor Authentication konfiguriert sind, ein Upgrade auf die Vollversion von Azure Multi-Factor Authentication durchgeführt.

**F: Gibt es eine kostenlose Version von Azure Multi-Factor Authentication für Benutzer von Office 365?**

Ein Teil der Funktionen von Azure Multi-Factor Authentication, „Multi-Factor Authentication für Office 365“ genannt, wird Benutzern, denen eine Office 365-Lizenz zugewiesen ist, kostenlos zur Verfügung gestellt, wenn mit der entsprechenden Instanz von Azure Active Directory kein Azure Multi-Factor Authentication-Anbieter für verbrauchsbasierende Abrechnung verknüpft wurde. Bei Verwendung eines Multi-Factor Authentication-Anbieters wird für alle Administratoren und Benutzer in diesem Verzeichnis, die für die Nutzung von Multi-Factor Authentication konfiguriert sind, ein Upgrade auf die Vollversion von Azure Multi-Factor Authentication durchgeführt.

**F: Kann meine Organisation jederzeit zwischen den Verbrauchsmodellen „Pro Benutzer“ und „Pro Authentifizierung“ wechseln?**

Ihre Organisation wählt ein Abrechnungsmodell, wenn sie eine Ressource erstellt. Sie können ein Abrechnungsmodell nach Bereitstellen der Ressource nicht ändern. Sie können jedoch eine neue Multi-Factor Authentication-Ressource erstellen, um die ursprüngliche zu ersetzen. Beachten Sie, dass die Benutzereinstellungen und Konfigurationsoptionen nicht auf die neue Ressource übertragen werden können.

**F: Kann meine Organisation jederzeit zwischen der verbrauchsbasierten Abrechnung und dem Lizenzmodell wechseln?**

Ihre Organisation kann jederzeit Lizenzen für Azure Multi-Factor Authentication, Azure Active Directory Premium , Enterprise Mobility Suite und Enterprise Cloud Suite erwerben. Wenn Lizenzen einem Verzeichnis hinzugefügt werden, für das bereits ein Multi-Factor Authentication-Anbieter für die Abrechnung pro Benutzer eingerichtet wurde, so wird die verbrauchsbasierte Abrechnung um die Anzahl der im Besitz befindlichen Lizenzen reduziert. Wenn allen Benutzer, die für die Verwendung von Multi-Factor Authentication konfiguriert sind, Lizenzen zugewiesen sind, kann der Administrator den Azure Multi-Factor Authentication-Anbieter löschen. Eine Organisation kann die Abrechnung pro Authentifizierung nicht mit einem Lizenzmodell kombinieren. Wenn ein Multi-Factor Authentication-Anbieter für die Abrechnung nach Anzahl der Authentifizierungen mit einem Verzeichnis verknüpft ist, werden der Organisation alle Multi-Factor Authentication-Verifizierungsanfragen in Rechnung gestellt. Dies gilt ungeachtet eventuell im Besitz befindlicher Lizenzen.

**F: Muss meine Organisation Identitäten verwenden und synchronisieren, um Azure Multi-Factor Authentication benutzen zu dürfen? **

Bei der Verwendung des verbrauchsbasierten Abrechnungsmodells durch eine Organisation ist Azure Active Directory nicht erforderlich. Es steht Ihnen frei, einen Multi-Factor Authentication-Anbieter mit einem Verzeichnis zu verknüpfen. Wenn Ihre Organisation nicht mit einem Verzeichnis verknüpft ist, kann sie Azure Multi-Factor Authentication-Server oder das Azure Multi-Factor Authentication-SDK lokal bereitstellen. Azure Active Directory ist für das Lizenzmodell erforderlich, da Lizenzen dem Verzeichnis hinzugefügt werden, wenn Sie sie nach dem Kauf Benutzern im Verzeichnis zuweisen.


## Benutzerfreundlichkeit

**F: Was tut ein Benutzer, wenn er keine Antwort auf seinem Telefon erhält, oder das Telefon dem Benutzer nicht zur Verfügung steht?**

Wenn für den Benutzer zuvor ein alternatives Telefon als Ersatzanschluss konfiguriert wurde, sollte er es erneut versuchen und auf der Anmeldeseite bei entsprechender Aufforderung den Ersatzanschluss auswählen. Wenn für den Benutzer keine andere Methode konfiguriert wurde, sollte er den Administrator der Organisation bitten, die Nummer des primären Mobil- oder Festnetztelefons, die dem Benutzer zugewiesen wurde, zu aktualisieren.


**F: Wie geht der Administrator vor, wenn ein Benutzer über ein Konto mit ihm in Kontakt tritt, auf das der Benutzer nicht mehr zugreifen kann?**

Der Administrator kann das Benutzerkonto zurücksetzen, indem er den Benutzer auffordert, den Registrierungsprozess erneut zu durchlaufen. Weitere Informationen finden Sie unter [Verwalten von Benutzereinstellungen mit Azure Multi-Factor Authentication (MFA) in der Cloud](multi-factor-authentication-manage-users-and-devices.md).

**F: Wie geht der Administrator bei Verlust oder Diebstahl eines Benutzertelefons vor, das App-Kennwörter verwendet?**

Der Administrator kann alle App-Kennwörter des Benutzers löschen, um nicht autorisierte Zugriffe zu verhindern. Sobald der Benutzer ein Ersatzgerät besitzt, kann der Benutzer neue Kennwörter erstellen. Weitere Informationen finden Sie unter [Verwalten von Benutzereinstellungen mit Azure Multi-Factor Authentication (MFA) in der Cloud](multi-factor-authentication-manage-users-and-devices.md).

**F: Was geschieht, wenn der Benutzer sich nicht bei nicht browserbasierten Apps anmelden kann?**

- Ein Benutzer, der zur Verwendung von Multi-Factor Authentication konfiguriert ist, benötigt zum Anmelden bei einigen nicht browserbasierten Apps ein App-Kennwort.
- Der Benutzer muss die Anmeldeinformationen löschen, die App neu starten und sich mit seinem Benutzernamen und dem App-Kennwort anmelden.

Weitere Informationen zum Erstellen von App-Kennwörtern und sonstige Hilfe finden Sie unter [Welchen Zweck erfüllen App-Kennwörter bei Azure Multi-Factor Authentication?](multi-factor-authentication-end-user-app-passwords.md).


>[AZURE.NOTE] Moderne Authentifizierung für Office 2013-Clients
>
> Office 2013-Clients (einschließlich Outlook) unterstützen neue Authentifizierungsprotokolle. Sie können Office 2013 zur Unterstützung von Multi-Factor Authentication konfigurieren. Nach der Konfiguration von Office 2013 sind keine App-Kennwörter für Office 2013-Clients erforderlich. Weitere Informationen finden Sie unter [Office 2013 modern authentication public preview announced](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/) (Öffentliche Preview für moderne Authentifizierung in Office 2013).

**F: Was kann ein Benutzer tun, wenn er keine Textnachricht erhält oder beim Antworten auf die Textnachricht ein Timeout auftritt?**

Der Azure Multi-Factor Authentication-Dienst sendet Textnachrichten über SMS-Aggregatoren. Viele Faktoren können die Zuverlässigkeit der Nachrichtenzustellung und der Empfangsbestätigung beeinflussen, z.B. der verwendete Aggregator, das Bestimmungsland, der Mobilfunkanbieter und die Signalstärke. Aus diesem Grund kann nicht gewährleistet werden, dass Textnachricht und Antwort-SMS beim Verwenden dieses bidirektionalen Verfahrens immer zuverlässig übermittelt bzw. empfangen werden. Nach Möglichkeit sollte die unidirektionale SMS der bidirektionalen SMS vorgezogen werden. Das unidirektionale SMS-Verfahren ist zuverlässiger und verhindert, dass für die Benutzer zusätzliche SMS-Gebühren entstehen, wenn sie z.B. auf eine SMS antworten, die aus einem anderen Land gesendet wurde.

Außerdem ist die Überprüfung per Textnachricht in einigen Ländern oder Regionen zuverlässiger, z.B. in den USA oder Kanada. Benutzer, bei denen Probleme mit dem zuverlässigen Empfang der Textnachrichten für Azure Multi-Factor Authentication auftreten, sollten stattdessen die Benachrichtigung in der mobilen App oder Telefonanrufe als Methode auswählen. Die Authentifizierung über die mobile App eignet sich hervorragend, da der Benutzer Benachrichtigungen über die mobile App sowohl per Mobilfunk als auch über WLAN-Verbindungen erhalten kann. Darüber hinaus wird die Kennung der mobilen Anwendung auch dann angezeigt, wenn das Gerät noch gar kein Signal empfangen hat. Die Microsoft Authenticator-App ist für [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072) und [iOS](http://go.microsoft.com/fwlink/?Linkid=825073) verfügbar.

**F: Kann ich Hardwaretoken in Verbindung mit dem Azure Multi-Factor Authentication-Server verwenden?**

Wenn Sie den Azure Multi-Factor Authentication-Server verwenden, können Sie zeitbasierte Token für die offene Authentifizierung (Open Authentication, OATH) mit einmaligen Kennwörtern (Time-based One-time Password Algorithm, TOTP) von Drittanbietern importieren und sie dann für die Multi-Factor Authentication verwenden. Wir unterstützen den Import von OATH TOTP-Token in einem älteren PSKC-Format (Portable Symmetric Key Container), das Gemalto für seine Token generieren kann, und unterstützen das Importieren von Token im CSV-Format. Beim Importieren von Token im CSV-Format muss die CSV-Datei eine Seriennummer, einen geheimen Schlüssel im Base32-Format und ein Zeitintervall (in der Regel 30 Sekunden) enthalten.

Sie können ActiveIdentity-Token verwenden, die OATH TOTP-Token sind, wenn Sie die Datei mit dem geheimen Schlüssel in eine CSV-Datei einfügen können, die zum Azure Multi-Factor Authentication-Server importiert werden kann. Sie können OATH-Token mit Active Directory-Verbunddiensten (ADFS) verwenden; mit Remote Authentication Dial-In User Service (RADIUS), wenn das Clientsystem Access-Challenge-Antworten verarbeiten kann; und mit Authentifizierung, die auf Internet Information Server-Formularen (IIS) basiert.

**F: Kann ich den Azure Multi-Factor Authentication-Server verwenden, um Terminaldienste zu sichern?**

Ja, aber wenn Sie Windows Server 2012 R2 oder höher verwenden, können Sie dies nur mithilfe von Remotedesktopgateway (RD-Gateway).

Sicherheitsänderungen in Windows Server 2012 R2 haben die Art geändert, in der sich der Azure Multi-Factor Authentication-Server in Windows Server 2012 und früheren Versionen mit dem Sicherheitspaket der lokalen Sicherheitsautorität (Local Security Authority, LSA) verbindet. Bei der Verwendung von Terminaldiensten unter Windows Server 2012 oder einer älteren Version können Sie [eine Anwendung einfach mit der Windows-Authentifizierung sichern](multi-factor-authentication-get-started-server-windows.md#to-secure-an-application-with-windows-authentication-use-the-following-procedure). Wenn Sie Windows Server 2012 R2 verwenden, benötigen Sie ein RD-Gateway.

**F: Warum könnte ein Benutzer nach dem Einrichten der Anrufer-ID einen Multi-Factor Authentication-Anruf von einem anonymen Aufrufer erhalten?**

Wenn Multi-Factor Authentication-Anrufe über das öffentliche Telefonnetz geleitet werden, laufen sie manchmal über einen Betreiber, der Anrufer-IDs nicht unterstützt. Aus diesem Grund ist die Anrufer-ID nicht garantiert, obwohl das Multi-Factor Authentication-System sie immer sendet.


## Fehler

**F: Wie geht ein Benutzer vor, wenn er die Fehlermeldung „Authentifizierungsanforderung gilt nicht für ein aktiviertes Konto“ erhält, wenn der Benutzer sich mithilfe von Benachrichtigungen über die mobile App authentifiziert?**

Gehen Sie folgendermaßen vor:

1. Wechseln Sie zu [Ihrem Azure-Portalprofil](https://account.activedirectory.windowsazure.com/profile/), und melden Sie sich mit Ihrem Unternehmenskonto an.
2. Falls erforderlich, klicken Sie auf **Weitere Überprüfungsoptionen**, und klicken Sie auf eine andere Option für die Kontoüberprüfung.
3. Klicken Sie auf **Zusätzliche Sicherheitsüberprüfung**.
4. Entfernen Sie das vorhandene Konto aus der mobilen App.
5. Klicken Sie auf **Konfigurieren**, und folgen Sie den Anweisungen, um die mobile App neu zu konfigurieren.




**F: Wie geht ein Benutzer vor, wenn eine 0x800434D4L-Fehlermeldung angezeigt wird, wenn der Benutzer versucht, sich mithilfe einer nicht browserbasierten Anwendung anzumelden?**

Derzeit können Benutzer die zusätzliche Sicherheitsüberprüfung nur in Verbindung mit Anwendungen und Diensten nutzen, auf die Benutzer über ihre Browser zugreifen können. Nicht browserbasierte Anwendungen (auch als *Rich Client-Anwendungen* bezeichnet), die auf dem lokalen Computer installiert sind, z.B. Windows PowerShell, können nicht zusammen mit Konten verwendet werden, bei denen eine zusätzliche Sicherheitsüberprüfung erforderlich ist. In diesem Fall kann die Anwendung den Fehler 0x800434D4L generieren.

Als Problemumgehung können Sie separate Benutzerkonten für Administratoraufgaben und Nicht-Administratoraufgaben erstellen. Später können Sie die Postfächer beider Konten miteinander verknüpfen, sodass Sie sich bei Outlook lediglich über ihr Nicht-Administratorkonto anmelden. Weitere Informationen hierzu, und wie ein Administrator die Möglichkeit erhält, den Inhalt des Postfachs eines Benutzers zu öffnen und anzuzeigen, siehe [Verwalten von Berechtigungen für Empfänger](http://help.outlook.com/141/gg709759.aspx?sl=1).

<!---HONumber=AcomDC_0921_2016-->