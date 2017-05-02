---
title: "Azure Multi-Factor Authentication – Häufig gestellte Fragen | Microsoft Docs"
description: "Häufig gestellte Fragen und Antworten im Zusammenhang mit Azure Multi-Factor Authentication. Multi-Factor Authentication ist eine Methode zum Überprüfen der Identität eines Benutzers, für die Benutzername und Kennwort nicht ausreichen. Sie bietet eine zusätzliche Sicherheitsebene für Benutzeranmeldungen und -transaktionen."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 50bb8ac3-5559-4d8b-a96a-799a74978b14
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/26/2017
ms.author: kgremban
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: 12b67d92fb955d9d20b0e86a38baa47a3717a333
ms.lasthandoff: 03/31/2017

---
# <a name="frequently-asked-questions-about-azure-multi-factor-authentication"></a>Häufig gestellte Fragen zu Azure Multi-Factor Authentication
Dieser Artikel enthält Antworten auf häufig gestellte Fragen zur Azure Multi-Factor Authentication und zur Nutzung des Multi-Factor Authentication-Diensts. Er ist unterteilt in allgemeine Fragen zum Dienst sowie in Fragen zu Abrechnungsmodellen, zur Benutzeroberfläche und zur Problembehandlung. 

## <a name="general"></a>Allgemein
**F: Wie werden Benutzerdaten vom Azure Multi-Factor Authentication-Server behandelt?**

Bei Multi-Factor Authentication-Server werden die Daten eines Benutzers nur auf den lokalen Servern gespeichert. Daten werden nicht dauerhaft in der Cloud gespeichert. Wenn der Benutzer die Authentifizierung in zwei Schritten ausführt, sendet der Multi-Factor Authentication-Server für die Authentifizierung Daten an den Multi-Factor Authentication-Clouddienst. Zur Kommunikation zwischen Multi-Factor Authentication-Server und dem Multi-Factor Authentication-Clouddienst wird Secure Sockets Layer (SSL) oder Transport Layer Security (TLS) über den ausgehenden Port 443 verwendet.

Wenn Authentifizierungsanforderungen an den Clouddienst gesendet werden, werden Daten für Authentifizierungs- und Verwendungsberichte gesammelt. Folgende Datenfelder werden in den Protokollen der Authentifizierung in zwei Schritten berücksichtigt:

* **Eindeutige ID** (entweder Benutzername oder lokale Server-ID der Multi-Factor Authentication)
* **Vor- und Nachname** (optional)
* **E-Mail-Adresse** (optional)
* **Rufnummer** (für eine Anruf- oder SMS-Authentifizierung)
* **Gerätetoken** (für die Authentifizierung einer mobilen App)
* **Authentifizierungsmodus**
* **Authentifizierungsergebnis**
* **Name des Multi-Factor Authentication-Servers**
* **IP des Multi-Factor Authentication-Servers**
* **Client-IP** (falls verfügbar)

Die optionalen Felder können im Multi-Factor Authentication-Server konfiguriert werden.

Das Authentifizierungsergebnis (Erfolg oder Ablehnung) und der Grund für etwaige Ablehnungen werden mit den Authentifizierungsdaten gespeichert. Diese Daten stehen in den Authentifizierungs- und Verwendungsberichten zur Verfügung.

## <a name="billing"></a>Abrechnung
Die meisten Fragen zur Abrechnung werden auf der Seite [Multi-Factor Authentication – Preise](https://azure.microsoft.com/pricing/details/multi-factor-authentication/) und in der Dokumentation zum [Beziehen von Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md) beantwortet.

**F: Werden meiner Organisation die Kosten der für die Authentifizierung verwendeten Telefonanrufe und SMS in Rechnung gestellt?**

Nein. Durch individuelle Telefonanrufe oder SMS, die über Azure Multi-Factor Authentication an Benutzer gesendet werden, entstehen für Sie keine Kosten. Bei Verwendung eines authentifizierungsbasierten MFA-Anbieters werden die Kosten für die einzelnen Authentifizierungen, aber nicht für die verwendete Methode in Rechnung gestellt.

Bei Ihren Benutzern können Kosten für die eingehenden Telefonanrufe oder SMS anfallen. Das hängt allerdings vom jeweiligen Netzbetreiber ab.

**F: Werden bei Verwendung des benutzerbasierten Abrechnungsmodells alle aktivierten Benutzer abgerechnet oder nur die Benutzer, die eine Überprüfung in zwei Schritten durchgeführt haben?**

Die Abrechnung basiert auf der Anzahl von Benutzern, die zur Nutzung von Multi-Factor Authentication konfiguriert sind – unabhängig davon, ob sie im Abrechnungsmonat eine Überprüfung initiiert haben. 

**F: Wie funktioniert die Abrechnung für die Multi-Factor Authentication?**

Wenn Sie Azure Multi-Factor Authentication als eigenständigen Dienst erwerben (indem Sie einen benutzer- oder authentifizierungsbasierten MFA-Anbieter erstellen), wird das Azure-Abonnement Ihrer Organisation monatlich auf der Grundlage der Nutzung abgerechnet. Dieses Abrechnungsmodell ähnelt der Azure-Abrechnung für die Nutzung von virtuellen Computern und Websites.

Wenn Sie ein Abonnement für Azure Multi-Factor Authentication erwerben (entweder als benutzerbasierte Jahreslizenz oder im Rahmen eines Office 365-, Azure AD Premium- oder Enterprise Mobility + Security-Pakets), fällt für Ihre Organisation lediglich die jährliche Lizenzgebühr für die einzelnen Benutzer an.

Weitere Informationen zu Ihren Möglichkeiten finden Sie unter [Beziehen von Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md).

**F: Gibt es eine kostenlose Version von Azure Multi-Factor Authentication?**

In einigen Fällen schon. 

Multi-Factor Authentication für Azure-Administratoren stellt einen Teil der Azure MFA-Features für den Zugriff auf Microsoft-Onlinedienste wie das Azure- und das Office 365-Verwaltungsportal kostenlos zur Verfügung. Dieses Angebot gilt für Azure-Administratoren in Azure Active Directory-Instanzen, die nicht über die Vollversion von Azure MFA (über eine MFA-Lizenz, ein Paket oder einen eigenständigen nutzungsbasierten Anbieter) verfügen. Wenn Ihre Administratoren die kostenlose Version verwenden und Sie einen Multi-Factor Authentication-Anbieter erstellen, erhalten alle Administratoren und Benutzer in diesem Verzeichnis, die für die Nutzung von Multi-Factor Authentication konfiguriert sind, die Vollversion von Azure Multi-Factor Authentication.

Multi-Factor Authentication für Office 365-Benutzer stellt einen Teil der Azure MFA-Features für den Zugriff auf Microsoft-Onlinedienste wie Exchange Online, SharePoint Online und andere Office 365-Dienste kostenlos zur Verfügung. Dieses Angebot gilt für Benutzer, denen eine Office 365-Lizenz zugewiesen ist, wenn die entsprechende Azure Active Directory-Instanz nicht über die Vollversion von Azure MFA (über eine MFA-Lizenz, ein Paket oder einen eigenständigen nutzungsbasierten Anbieter) verfügt. 

**F: Kann meine Organisation jederzeit zwischen den Verbrauchsmodellen „Pro Benutzer“ und „Pro Authentifizierung“ wechseln?**

Wenn Ihre Organisation MFA als eigenständigen Dienst mit nutzungsbasierter Abrechnung erwirbt, wählen Sie bei der Erstellung eines MFA-Anbieters ein Abrechnungsmodell aus. Das Abrechnungsmodell kann nach der Erstellung eines MFA-Anbieters nicht mehr geändert werden. Sie können allerdings den MFA-Anbieter löschen und einen neuen Anbieter mit einem anderen Abrechnungsmodell erstellen. 

Bei der Erstellung können Sie den MFA-Anbieter mit einer Azure Active Directory-Instanz (auch „Azure AD-Mandant“ genannt) verknüpfen. Wenn der aktuelle MFA-Anbieter mit einem Azure AD-Mandanten verknüpft ist, können Sie den MFA-Anbieter problemlos löschen und einen neuen MFA-Anbieter erstellen, der mit dem gleichen Azure AD-Mandanten verknüpft ist. Sofern die Anzahl der für MFA, Azure AD Premium oder Enterprise Mobility + Security (EMS) erworbenen Lizenzen für alle Benutzer ausreicht, für die MFA aktiviert ist, können Sie den MFA-Anbieter auch ganz löschen.

Falls Ihr MFA-Anbieter jedoch nicht mit einem Azure AD-Mandanten verknüpft ist oder Sie den neuen MFA-Anbieter mit einem anderen Azure AD-Mandanten verknüpfen, werden Benutzereinstellungen und Konfigurationsoptionen nicht in den neuen MFA-Anbieter übernommen. Darüber hinaus müssen vorhandene Azure MFA-Server unter Verwendung von Aktivierungsanmeldeinformationen des neuen MFA-Anbieters erneut aktiviert werden. Wenn Sie die MFA-Server erneut aktivieren, um sie mit dem neuen MFA-Anbieter zu verknüpfen, hat das keinerlei Auswirkungen auf die Authentifizierung per Telefonanruf oder SMS, aber Benachrichtigungen der mobilen App funktionieren erst wieder, wenn die Benutzer die mobile App erneut aktivieren.

Weitere Informationen zu MFA-Anbietern finden Sie unter [Erste Schritte mit Azure Multi-Factor Authentication-Anbietern](multi-factor-authentication-get-started-auth-provider.md).

**F: Kann meine Organisation jederzeit zwischen der nutzungsbasierten Abrechnung und Abonnements (lizenzbasiertes Modell) wechseln?**

In einigen Fällen schon. 

Wenn Ihr Verzeichnis über einen *benutzerbasierten* Azure Multi-Factor Authentication-Anbieter verfügt, können Sie MFA-Lizenzen hinzufügen. Benutzer mit Lizenzen fließen nicht in die benutzer-/nutzungsbasierte Abrechnung mit ein. Für Benutzer ohne Lizenz kann MFA weiterhin über den MFA-Anbieter aktiviert werden. Wenn Sie für alle Ihre Benutzer, die für die Verwendung von Multi-Factor Authentication konfiguriert sind, Lizenzen erwerben und zuweisen, können Sie den Azure Multi-Factor Authentication-Anbieter löschen. Sollten später einmal mehr Benutzer als Lizenzen vorhanden sein, können Sie jederzeit einen weiteren benutzerbasierten MFA-Anbieter erstellen. 

Falls Ihr Verzeichnis über einen *authentifizierungsbasierten* Azure Multi-Factor Authentication-Anbieter verfügt, werden Ihnen immer die einzelnen Authentifizierungen in Rechnung gestellt, solange der MFA-Anbieter mit Ihrem Abonnement verknüpft ist. Sie können Benutzern zwar MFA-Lizenzen zuweisen, Ihnen wird jedoch weiterhin jede Anforderung einer Überprüfung in zwei Schritten in Rechnung gestellt. Dabei spielt es keine Rolle, ob dem anfordernden Benutzer eine MFA-Lizenz zugewiesen ist. 

**F: Muss meine Organisation Identitäten verwenden und synchronisieren, um Azure Multi-Factor Authentication benutzen zu dürfen?**

Wenn Ihre Organisation ein nutzungsbasiertes Abrechnungsmodell verwendet, ist Azure Active Directory optional, aber nicht erforderlich. Wenn Ihr MFA-Anbieter nicht mit einem Azure AD-Mandanten verknüpft ist, können Sie den Azure Multi-Factor Authentication-Server oder das Azure Multi-Factor Authentication-SDK nur lokal bereitstellen.

Azure Active Directory ist für das Lizenzmodell erforderlich, da Lizenzen dem Azure AD-Mandanten hinzugefügt werden, wenn Sie sie nach dem Kauf Benutzern im Verzeichnis zuweisen.

## <a name="manage-and-support-user-accounts"></a>Verwalten von Benutzerkonten und Bereitstellen von Support

**F: Was soll ich meinen Benutzern raten, wenn Sie auf ihrem Smartphone keine Antwort erhalten oder ihr Smartphone nicht zur Hand haben?**

Im Idealfall haben alle Ihre Benutzer mehrere Überprüfungsmethoden konfiguriert. Fordern Sie sie auf, sich erneut anzumelden und dabei auf der Anmeldeseite eine andere Überprüfungsmethode auszuwählen. 

Unter [Probleme bei der Überprüfung in zwei Schritten](./end-user/multi-factor-authentication-end-user-troubleshoot.md) finden Ihre Endbenutzer einen Problembehandlungsleitfaden.


**F: Was kann ich tun, wenn ein Benutzer nicht auf sein Konto zugreifen kann?**

Sie können den Benutzer auffordern, den Registrierungsprozess erneut zu durchlaufen, um das Benutzerkonto zurückzusetzen. Weitere Informationen finden Sie unter [Verwalten von Benutzereinstellungen mit Azure Multi-Factor Authentication (MFA) in der Cloud](multi-factor-authentication-manage-users-and-devices.md).

**F: Was kann ich tun, wenn ein Benutzer ein Smartphone mit App-Kennwörtern verliert?**

Löschen Sie alle App-Kennwörter des Benutzers, um nicht autorisierte Zugriffe zu verhindern. Wenn der Benutzer über ein Ersatzgerät verfügt, kann er neue Kennwörter erstellen. Weitere Informationen finden Sie unter [Verwalten von Benutzereinstellungen mit Azure Multi-Factor Authentication (MFA) in der Cloud](multi-factor-authentication-manage-users-and-devices.md).

**F: Was kann ich tun, wenn sich ein Benutzer nicht bei nicht browserbasierten Apps anmelden kann?**

Falls Ihre Organisation noch Legacyclients verwendet und Sie [die Verwendung von App-Kennwörtern zugelassen haben](multi-factor-authentication-whats-next.md#app-passwords), können sich Ihre Benutzer nicht mit ihrem Benutzernamen und Kennwort bei diesen Legacyclients anmelden. Stattdessen müssen sie [App-Kennwörter einrichten](./end-user/multi-factor-authentication-end-user-app-passwords.md). Ihre Benutzer müssen ihre Anmeldeinformationen löschen, die App neu starten und sich dann mit ihrem Benutzernamen und ihrem *App-Kennwort* (anstelle des regulären Kennworts) anmelden. 

Wenn Ihre Organisation keine Legacyclients verwendet, sollten Sie die Erstellung von App-Kennwörtern durch Benutzer nicht zulassen. 

> [!NOTE]
> Moderne Authentifizierung für Office 2013-Clients
> 
> App-Kennwörter werden nur für Apps benötigt, die keine moderne Authentifizierung unterstützen. Office 2013-Clients unterstützen moderne Authentifizierungsprotokolle, müssen aber konfiguriert werden. Neuere Office-Clients unterstützen automatisch moderne Authentifizierungsprotokolle. Weitere Informationen finden Sie unter [Office 2013 modern authentication public preview announced](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/)(Öffentliche Preview für moderne Authentifizierung in Office 2013).

**F: Meine Benutzer erhalten manchmal keine SMS, oder es tritt ein Timeout auf, nachdem sie auf eine bidirektionale SMS geantwortet haben.**

Die Übermittlung von SMS und der Empfang von Antworten auf bidirektionale SMS können nicht garantiert werden, da die Zuverlässigkeit des Diensts durch unkontrollierbare Faktoren beeinträchtigt werden kann. Unter diese Faktoren fallen das Bestimmungsland, der Mobilfunkanbieter und die Signalstärke.

Falls Ihre Benutzer häufig SMS-Empfangsprobleme haben, fordern Sie sie auf, die mobile App oder die Telefonanrufmethode zu verwenden. Die mobile App kann sowohl über Mobilfunk- als auch WLAN-Verbindungen Benachrichtigungen empfangen. Darüber hinaus kann die mobile App Überprüfungscodes auch dann generieren, wenn das Gerät noch gar kein Signal empfangen hat. Die Microsoft Authenticator-App ist für [Android](http://go.microsoft.com/fwlink/?Linkid=825072), [iOS](http://go.microsoft.com/fwlink/?Linkid=825073) und [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071) verfügbar.

Wenn Sie Textnachrichten benutzen müssen, sollte nach Möglichkeit die unidirektionale SMS der bidirektionalen SMS vorgezogen werden. Das unidirektionale SMS-Verfahren ist zuverlässiger und verhindert, dass für die Benutzer zusätzliche SMS-Gebühren entstehen, wenn sie z.B. auf eine SMS antworten, die aus einem anderen Land gesendet wurde.

**F: Kann ich ändern, wie lange meine Benutzer für die Eingabe des Überprüfungscodes aus einer SMS Zeit haben, bevor ein Timeout auftritt?**

In einigen Fällen schon. Die Timeouteinstellung für bidirektionale SMS kann ab Azure MFA Server 7.0 konfiguriert werden. 

Azure MFA Server speichert Einmalkennungen standardmäßig für 300 Sekunden (fünf Minuten). Wenn der Benutzer seinen Code erst nach Ablauf der 300 Sekunden eingibt, wird die Authentifizierung verweigert. Das Timeout kann durch Festlegen eines Registrierungsschlüssels festgelegt werden. 

1. Navigieren Sie zu „HKLM\Software\Wow6432Node\Positive Networks\PhoneFactor“.
2. Erstellen Sie einen DWORD-Registrierungsschlüssel namens **pfsvc_pendingSmsTimeoutSeconds**, und legen Sie fest, wie lange Azure MFA Server Einmalkennungen speichern soll (in Sekunden).

Bei unidirektionalen SMS werden Einmalkennungen von MFA Server 300 Sekunden lang gespeichert. Die cloudbasierte MFA in Azure AD speichert sie für 180 Sekunden. Diese Einstellung ist nicht konfigurierbar. 

**F: Kann ich Hardwaretoken in Verbindung mit dem Azure Multi-Factor Authentication-Server verwenden?**

Wenn Sie den Azure Multi-Factor Authentication-Server verwenden, können Sie zeitbasierte Token für die offene Authentifizierung (Open Authentication, OATH) mit einmaligen Kennwörtern (Time-based One-time Password Algorithm, TOTP) von Drittanbietern importieren und sie dann für die Authentifizierung in zwei Schritten verwenden.

Sie können ActiveIdentity-Token vom Typ „OATH TOTP“ verwenden, wenn Sie den geheimen Schlüssel in eine CSV-Datei einfügen und in den Azure Multi-Factor Authentication-Server importieren. Sie können OATH-Token mit Active Directory Federation Services (AD FS), mit formularbasierter IIS-Authentifizierung (Internet Information Services) sowie mit Remote Authentication Dial-In User Service (RADIUS) verwenden, wenn das Clientsystem Access-Challenge-Antworten verarbeiten kann.

Sie können OATH TOTP-Drittanbietertoken mit folgenden Formaten importieren:  

- Portable Symmetric Key Container (PSKC)  
- CSV, wenn die Datei eine Seriennummer, einen geheimen Schlüssel im Base 32-Format, und ein Zeitintervall enthält  

**F: Kann ich den Azure Multi-Factor Authentication-Server verwenden, um Terminaldienste zu sichern?**

Ja, aber wenn Sie Windows Server 2012 R2 oder höher verwenden, können Sie Terminaldienste nur mithilfe von Remotedesktopgateway (RD-Gateway) schützen.

Aufgrund von Sicherheitsänderungen in Windows Server 2012 R2 hat sich die Verbindungsherstellung zwischen dem Azure Multi-Factor Authentication-Server und dem Sicherheitspaket der lokalen Sicherheitsautorität (Local Security Authority, LSA) in Windows Server 2012 und früheren Versionen geändert. Bei der Verwendung von Terminaldiensten unter Windows Server 2012 oder einer älteren Version können Sie [eine Anwendung mit der Windows-Authentifizierung sichern](multi-factor-authentication-get-started-server-windows.md#to-secure-an-application-with-windows-authentication-use-the-following-procedure). Wenn Sie Windows Server 2012 R2 verwenden, benötigen Sie ein RD-Gateway.

**F: Ich habe in MFA Server die Anrufer-ID konfiguriert, meine Benutzer erhalten von Multi-Factor Authentication aber weiterhin anonyme Anrufe.**

Wenn Multi-Factor Authentication-Anrufe über das öffentliche Telefonnetz geleitet werden, laufen sie manchmal über einen Betreiber, der Anrufer-IDs nicht unterstützt. Aus diesem Grund ist die Anrufer-ID nicht garantiert, obwohl das Multi-Factor Authentication-System sie immer sendet.

**F: Warum werden meine Benutzer zur Registrierung ihrer Sicherheitsinformationen aufgefordert?**
Benutzer können aus mehreren Gründen zur Registrierung ihrer Sicherheitsinformationen aufgefordert werden:

- Unter Umständen hat der Administrator in Azure AD MFA für den Benutzer aktiviert, der Benutzer hat jedoch noch keine Sicherheitsinformationen für sein Konto registriert. 
- Für den Benutzer wurde die Self-Service-Kennwortzurücksetzung in Azure AD aktiviert. Mithilfe der Sicherheitsinformationen kann der Benutzer sein Kennwort zurücksetzen, falls er es einmal vergessen sollte.
- Der Benutzer hat auf eine Anwendung mit einer Richtlinie für bedingten Zugriff zugegriffen, die MFA erfordert, und hat sich bislang noch nicht für MFA registriert.
- Der Benutzer registriert ein Gerät bei Azure AD (einschließlich Azure AD Join), und Ihre Organisation schreibt MFA für die Geräteregistrierung vor, der Benutzer hat sich bislang aber noch nicht für MFA registriert.
- Der Benutzer generiert Windows Hello for Business unter Windows 10 (wofür MFA erforderlich ist) und hat sich bislang noch nicht für MFA registriert.
- Eine von der Organisation erstellte und aktivierte MFA-Registrierungsrichtlinie wurde auf den Benutzer angewendet.
- Der Benutzer hat sich zwar bereits für MFA registriert, dabei aber eine Überprüfungsmethode gewählt, die inzwischen von einem Administrator deaktiviert wurde. Daher muss der Benutzer die MFA-Registrierung erneut durchlaufen und eine neue Standardüberprüfungsmethode auswählen.


## <a name="errors"></a>Fehler
**F: Was sollten Benutzer tun, wenn bei der Verwendung von Benachrichtigungen über die mobile App die Fehlermeldung „Authentication request is not for an activated account“ (Authentifizierungsanforderung gilt nicht für ein aktiviertes Konto) angezeigt wird?**

Informieren Sie die Benutzer, dass sie wie Folgt vorgehen sollen, um ihr Konto aus der mobilen App zu löschen, und anschließend wieder hinzuzufügen:

1. Wechseln Sie zu [Ihrem Azure-Portalprofil](https://account.activedirectory.windowsazure.com/profile/) , und melden Sie sich mit Ihrem Unternehmenskonto an.
2. Wählen Sie **Zusätzliche Sicherheitsüberprüfung** aus.
3. Entfernen Sie das vorhandene Konto aus der mobilen App.
4. Klicken Sie auf **Konfigurieren**, und folgen Sie den Anweisungen, um die mobile App neu zu konfigurieren.

**F: Was können Benutzer tun, wenn sie beim Anmelden bei einer nicht auf Browsern basierenden Anwendung die Fehlermeldung „0x800434D4L“ erhalten?**

Der Fehler „0x800434D4L“ tritt auf, wenn Sie versuchen, sich bei einer nicht browserbasierten, auf einem lokalen Computer installierten Anwendung anzumelden, die nicht mit einem Konto verwendet werden kann, das eine Überprüfung in zwei Schritten erfordert. 

Als Problemumgehung können Sie separate Benutzerkonten für Administratoraufgaben und Nicht-Administratoraufgaben erstellen. Später können Sie die Postfächer beider Konten miteinander verknüpfen, sodass Sie sich bei Outlook lediglich über ihr Nicht-Administratorkonto anmelden. Weitere Informationen hierzu, und wie ein Administrator die Möglichkeit erhält, den Inhalt des Postfachs eines Benutzers zu öffnen und anzuzeigen, siehe [Verwalten von Berechtigungen für Empfänger](http://help.outlook.com/141/gg709759.aspx?sl=1).

## <a name="next-steps"></a>Nächste Schritte
Wenn Ihre Frage hier nicht beantwortet wurde, schreiben Sie sie bitte in die Kommentare unten auf der Seite. Alternativ finden Sie hier weitere Optionen zur Hilfesuche:

* Durchsuchen Sie die [Microsoft Support Wissensdatenbank](https://www.microsoft.com/en-us/Search/result.aspx?form=mssupport&q=phonefactor&form=mssupport) nach Lösungen für häufige technische Probleme.
* In den [Azure Active Directory-Foren](https://social.msdn.microsoft.com/Forums/azure/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required)können Sie nach technischen Fragen und Antworten aus der Community suchen oder eine eigene Frage stellen.
* Wenn Sie ein älterer PhoneFactor-Kunde sind und Fragen haben oder Hilfe beim Zurücksetzen eines Kennworts benötigen, verwenden Sie den Link zur [Kennwortzurücksetzung](mailto:phonefactorsupport@microsoft.com) , um eine Supportanfrage zu öffnen.
* Wenden Sie sich über[Azure Multi-Factor Authentication-Server (PhoneFactor) – Support](https://support.microsoft.com/oas/default.aspx?prid=14947) an einen Support-Experten. Es ist hilfreich, wenn Sie uns so viele Informationen wie möglich über Ihr Problem geben, wenn Sie sich mit uns in Verbindung setzen. Diese Informationen können die Website umfassen, auf der der Fehler aufgetreten ist, den spezifischen Fehlercode, die spezifische Sitzungs-ID, und die ID des Benutzers, der den Fehler beobachtet hat.


