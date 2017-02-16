---
title: "Azure Multi-Factor Authentication – Häufig gestellte Fragen | Microsoft Docs"
description: "Enthält eine Liste mit häufig gestellten Fragen und Antworten im Zusammenhang mit Azure Multi-Factor Authentication. Multi-Factor Authentication ist eine Methode zum Überprüfen der Identität eines Benutzers, für die Benutzername und Kennwort nicht ausreichen. Sie bietet eine zusätzliche Sicherheitsebene für Benutzeranmeldungen und -transaktionen."
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
ms.date: 01/06/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 9617cd00ed4a5f8f867542238e5008a9a17663c9
ms.openlocfilehash: 1e01bdc099af865e01eb2784cf367e482300ade8


---
# <a name="azure-multi-factor-authentication-faq"></a>Azure Multi-Factor Authentication – Häufig gestellte Fragen
Dieses FAQ beantwortet häufige Fragen zur Azure Multi-Factor Authentication und zur Nutzung des Multi-Factor Authentication-Diensts, einschließlich Fragen zu Abrechnungsmodell und Verwendbarkeit.

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

Das Authentifizierungsergebnis (Erfolg oder Ablehnung) und der Grund für etwaige Ablehnungen werden mit den Authentifizierungsdaten gespeichert. Sie stehen in den Authentifizierungs- und Verwendungsberichten zur Verfügung.

## <a name="billing"></a>Abrechnung
Die meisten Fragen zur Abrechnung werden auf der Seite [Multi-Factor Authentication – Preise](https://azure.microsoft.com/pricing/details/multi-factor-authentication/) beantwortet.

**F: Entstehen durch Telefonanrufe oder Textnachrichten für die Benutzerauthentifizierung zusätzliche Kosten für meine Organisation?**

Organisationen entstehen keine zusätzlichen Kosten für individuelle Telefonanrufe oder Textnachrichten, die über Azure Multi-Factor Authentication an Benutzer gesendet werden. Für Telefonbenutzer können Kosten für die Telefonanrufe oder Textnachrichten, die sie erhalten, entstehen. Dies hängt von ihrem jeweiligen Netzbetreiber ab.

**F: Werden die Kosten für das Abrechnungsmodell „pro Benutzer“ basierend auf der Anzahl der Benutzer ermittelt, die zur Nutzung von Multi-Factor Authentication konfiguriert sind, oder basierend auf der Anzahl der Benutzer, die Überprüfungen durchführen?**

Die Abrechnung basiert auf der Anzahl der Benutzer, die zur Nutzung von Multi-Factor Authentication konfiguriert sind.

**F: Wie funktioniert die Abrechnung für die Multi-Factor Authentication?**

Wenn Sie das Modell „Pro Benutzer“ oder „Pro Authentifizierung“ benutzen, handelt es sich bei Azure MFA um eine auf dem Verbrauch basierende Ressource. Jegliche Kosten werden mit dem Azure-Abonnement des Unternehmens abgerechnet, wie bei virtuellen Computer, Websites, usw.

Bei Verwendung des Lizenzmodells werden Lizenzen für Azure Multi-Factor Authentication erworben und wie bei Office 365 und anderen Abonnementprodukten den Benutzern zugewiesen.

Weitere Informationen zu Ihren Möglichkeiten finden Sie unter [Funktionsweise von Azure Multi-Factor Authentication](multi-factor-authentication-how-it-works.md#how-to-get-azure-multi-factor-authentication).

**F: Gibt es eine kostenlose Version von Azure Multi-Factor Authentication für Administratoren?**

In einigen Fällen schon. Multi-Factor Authentication für Azure-Administratoren stellt kostenlos einen Teil der Funktionen von Azure MFA zur Verfügung. Dieses Angebot gilt für Mitglieder der Gruppe „Azure Global-Administratoren“ in Azure Active Directory-Instanzen, die nicht mit einem verbrauchsbasierten Azure Multi-Factor Authentication-Anbieter verknüpft sind. Bei Verwendung eines Multi-Factor Authentication-Anbieters wird für alle Administratoren und Benutzer in diesem Verzeichnis, die für die Nutzung von Multi-Factor Authentication konfiguriert sind, ein Upgrade auf die Vollversion von Azure Multi-Factor Authentication durchgeführt.

**F: Gibt es eine kostenlose Version von Azure Multi-Factor Authentication für Benutzer von Office 365?**

In einigen Fällen schon. Multi-Factor Authentication für Office 365 stellt kostenlos einen Teil der Funktionen von Azure MFA zur Verfügung. Dieses Angebot gilt für Benutzer, denen eine Office 365-Lizenz zugewiesen ist, wenn mit einem verbrauchsbasierten Azure Multi-Factor Authentication-Anbieter keine entsprechende Instanz von Azure Active Directory verknüpft ist. Bei Verwendung eines Multi-Factor Authentication-Anbieters wird für alle Administratoren und Benutzer in diesem Verzeichnis, die für die Nutzung von Multi-Factor Authentication konfiguriert sind, ein Upgrade auf die Vollversion von Azure Multi-Factor Authentication durchgeführt.

**F: Kann meine Organisation jederzeit zwischen den Verbrauchsmodellen „Pro Benutzer“ und „Pro Authentifizierung“ wechseln?**

Ihre Organisation wählt ein Abrechnungsmodell, wenn sie eine Ressource erstellt. Sie können ein Abrechnungsmodell nach Bereitstellen der Ressource nicht ändern. Sie können jedoch eine andere Multi-Factor Authentication-Ressource erstellen, um die ursprüngliche zu ersetzen. Benutzereinstellungen und Konfigurationsoptionen können nicht auf die neue Ressource übertragen werden.

**F: Kann meine Organisation jederzeit zwischen der verbrauchsbasierten Abrechnung und dem Lizenzmodell wechseln?**

Wenn Lizenzen einem Verzeichnis hinzugefügt werden, für das bereits ein Multi-Factor Authentication-Anbieter für die Abrechnung pro Benutzer eingerichtet wurde, so wird die verbrauchsbasierte Abrechnung um die Anzahl der im Besitz befindlichen Lizenzen reduziert. Wenn allen Benutzer, die für die Verwendung von Multi-Factor Authentication konfiguriert sind, Lizenzen zugewiesen sind, kann der Administrator den Azure Multi-Factor Authentication-Anbieter löschen.

Sie können die Abrechnung pro Authentifizierung nicht mit einem Lizenzmodell kombinieren. Wenn ein Multi-Factor Authentication-Anbieter für die Abrechnung nach Anzahl der Authentifizierungen mit einem Verzeichnis verknüpft ist, werden der Organisation alle Multi-Factor Authentication-Verifizierungsanfragen in Rechnung gestellt. Dies gilt ungeachtet eventuell im Besitz befindlicher Lizenzen.

**F: Muss meine Organisation Identitäten verwenden und synchronisieren, um Azure Multi-Factor Authentication benutzen zu dürfen?**

Bei der Verwendung des verbrauchsbasierten Abrechnungsmodells durch eine Organisation ist Azure Active Directory nicht erforderlich. Es steht Ihnen frei, einen Multi-Factor Authentication-Anbieter mit einem Verzeichnis zu verknüpfen. Wenn Ihre Organisation nicht mit einem Verzeichnis verknüpft ist, kann sie Azure Multi-Factor Authentication-Server oder das Azure Multi-Factor Authentication-SDK lokal bereitstellen.

Azure Active Directory ist für das Lizenzmodell erforderlich, da Lizenzen dem Verzeichnis hinzugefügt werden, wenn Sie sie nach dem Kauf Benutzern im Verzeichnis zuweisen.

## <a name="usability"></a>Benutzerfreundlichkeit
**F: Was tut ein Benutzer, wenn er keine Antwort auf seinem Telefon erhält, oder das Telefon dem Benutzer nicht zur Verfügung steht?**

Wenn der Benutzer ein alternatives Telefon als Ersatzanschluss konfiguriert hat, sollte er es erneut versuchen, und auf der Anmeldeseite bei entsprechender Aufforderung den Ersatzanschluss auswählen. Wenn für den Benutzer keine andere Methode konfiguriert wurde, kann der Administrator der Organisation die Nummer des primären Telefons des Benutzers aktualisieren.

**F: Wie geht der Administrator vor, wenn ein Benutzer über ein Konto mit ihm in Kontakt tritt, auf das der Benutzer nicht mehr zugreifen kann?**

Der Administrator kann das Benutzerkonto zurücksetzen, indem er den Benutzer auffordert, den Registrierungsprozess erneut zu durchlaufen. Weitere Informationen finden Sie unter [Verwalten von Benutzereinstellungen mit Azure Multi-Factor Authentication (MFA) in der Cloud](multi-factor-authentication-manage-users-and-devices.md).

**F: Wie geht der Administrator bei Verlust oder Diebstahl eines Benutzertelefons vor, das App-Kennwörter verwendet?**

Der Administrator kann alle App-Kennwörter des Benutzers löschen, um nicht autorisierte Zugriffe zu verhindern. Sobald der Benutzer ein Ersatzgerät besitzt, kann der Benutzer neue Kennwörter erstellen. Weitere Informationen finden Sie unter [Verwalten von Benutzereinstellungen mit Azure Multi-Factor Authentication (MFA) in der Cloud](multi-factor-authentication-manage-users-and-devices.md).

**F: Was geschieht, wenn der Benutzer sich nicht bei nicht browserbasierten Apps anmelden kann?**

Ein Benutzer, der zur Verwendung von Multi-Factor Authentication konfiguriert ist, benötigt zum Anmelden bei einigen nicht browserbasierten Apps ein App-Kennwort. Der Benutzer muss die Anmeldeinformationen löschen, die App neu starten und sich mit seinem Benutzernamen und dem App-Kennwort anmelden.

Weitere Informationen zum Erstellen von App-Kennwörtern und sonstige Hilfe finden Sie unter [Welchen Zweck erfüllen App-Kennwörter bei Azure Multi-Factor Authentication?](multi-factor-authentication-end-user-app-passwords.md).

> [!NOTE]
> Moderne Authentifizierung für Office 2013-Clients
> 
> Office 2013-Clients (einschließlich Outlook) unterstützen neue Authentifizierungsprotokolle. Sie können Office 2013 zur Unterstützung von Multi-Factor Authentication konfigurieren. Nach der Konfiguration von Office 2013 sind keine App-Kennwörter für Office 2013-Clients erforderlich. Weitere Informationen finden Sie unter [Office 2013 modern authentication public preview announced](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/)(Öffentliche Preview für moderne Authentifizierung in Office 2013).
> 
> 

**F: Was kann ein Benutzer tun, wenn er keine Textnachricht erhält oder beim Antworten auf die Textnachricht ein Timeout auftritt?**

Die Übermittlung von Textnachrichten, und der Empfang von Antwort-SMS im bidirektionalen Verfahren kann nicht garantiert werden, da hier unkontrollierbare Faktoren auftreten können, die die Zuverlässigkeit des Diensts beeinträchtigen können. Unter diese Faktoren fallen das Bestimmungsland, der Mobilfunkanbieter und die Signalstärke.

Benutzer, bei denen Probleme mit dem zuverlässigen Empfang von Textnachrichten auftreten, sollten stattdessen die Benachrichtigung in der mobilen App oder Telefonanrufe als Methode auswählen. Die mobile App kann sowohl über Mobilfunk- als auch WLAN-Verbindungen Benachrichtigungen empfangen. Darüber hinaus kann die mobile App Überprüfungscodes auch dann generieren, wenn das Gerät noch gar kein Signal empfangen hat. Die Microsoft Authenticator-App ist für [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072) und [IOS](http://go.microsoft.com/fwlink/?Linkid=825073) verfügbar.

Wenn Sie Textnachrichten benutzen müssen, sollte nach Möglichkeit die unidirektionale SMS der bidirektionalen SMS vorgezogen werden. Das unidirektionale SMS-Verfahren ist zuverlässiger und verhindert, dass für die Benutzer zusätzliche SMS-Gebühren entstehen, wenn sie z.B. auf eine SMS antworten, die aus einem anderen Land gesendet wurde.

**F: Kann ich Hardwaretoken in Verbindung mit dem Azure Multi-Factor Authentication-Server verwenden?**

Wenn Sie den Azure Multi-Factor Authentication-Server verwenden, können Sie zeitbasierte Token für die offene Authentifizierung (Open Authentication, OATH) mit einmaligen Kennwörtern (Time-based One-time Password Algorithm, TOTP) von Drittanbietern importieren und sie dann für die Authentifizierung in zwei Schritten verwenden.

Sie können ActiveIdentity-Token verwenden, die OATH TOTP-Token sind, wenn Sie die Datei mit dem geheimen Schlüssel in eine CSV-Datei einfügen, die zum Azure Multi-Factor Authentication-Server importiert werden. Sie können OATH-Token mit Active Directory-Verbunddiensten (AD FS) verwenden, sowie mit Remote Authentication Dial-In User Service (RADIUS), wenn das Clientsystem Access-Challenge-Antworten verarbeiten kann. Außerdem können Sie sie mit Authentifizierung, die auf Internet Information Server-Formularen (IIS) basiert, verwenden.

Sie können OATH TOTP-Token von Drittanbietern mit den folgenden Formaten importieren:  

* Portable Symmetric Key Container (PSKC)  
* CSV, wenn die Datei eine Seriennummer, einen geheimen Schlüssel im Base 32-Format, und ein Zeitintervall enthält  

**F: Kann ich den Azure Multi-Factor Authentication-Server verwenden, um Terminaldienste zu sichern?**

Ja, aber wenn Sie Windows Server 2012 R2 oder höher verwenden, können Sie dies nur mithilfe von Remotedesktopgateway (RD-Gateway).

Sicherheitsänderungen in Windows Server 2012 R2 haben die Art geändert, in der sich der Azure Multi-Factor Authentication-Server in Windows Server 2012 und früheren Versionen mit dem Sicherheitspaket der lokalen Sicherheitsautorität (Local Security Authority, LSA) verbindet. Bei der Verwendung von Terminaldiensten unter Windows Server 2012 oder einer älteren Version können Sie [eine Anwendung mit der Windows-Authentifizierung sichern](multi-factor-authentication-get-started-server-windows.md#to-secure-an-application-with-windows-authentication-use-the-following-procedure). Wenn Sie Windows Server 2012 R2 verwenden, benötigen Sie ein RD-Gateway.

**F: Warum könnte ein Benutzer nach dem Einrichten der Anrufer-ID einen Multi-Factor Authentication-Anruf von einem anonymen Aufrufer erhalten?**

Wenn Multi-Factor Authentication-Anrufe über das öffentliche Telefonnetz geleitet werden, laufen sie manchmal über einen Betreiber, der Anrufer-IDs nicht unterstützt. Aus diesem Grund ist die Anrufer-ID nicht garantiert, obwohl das Multi-Factor Authentication-System sie immer sendet.

## <a name="errors"></a>Fehler
**F: Was sollten Benutzer tun, wenn bei der Verwendung von Benachrichtigungen über die mobile App die Fehlermeldung „Authentication request is not for an activated account“ (Authentifizierungsanforderung gilt nicht für ein aktiviertes Konto) angezeigt wird?**

Informieren Sie die Benutzer, dass sie wie Folgt vorgehen sollen, um ihr Konto aus der mobilen App zu löschen, und anschließend wieder hinzuzufügen:

1. Wechseln Sie zu [Ihrem Azure-Portalprofil](https://account.activedirectory.windowsazure.com/profile/) , und melden Sie sich mit Ihrem Unternehmenskonto an.
2. Wählen Sie **Zusätzliche Sicherheitsüberprüfung** aus.
3. Entfernen Sie das vorhandene Konto aus der mobilen App.
4. Klicken Sie auf **Konfigurieren**, und folgen Sie den Anweisungen, um die mobile App neu zu konfigurieren.

**F: Was können Benutzer tun, wenn sie beim Anmelden bei einer nicht auf Browsern basierenden Anwendung die Fehlermeldung „0x800434D4L“ erhalten?**

Derzeit können Benutzer die zusätzliche Sicherheitsüberprüfung nur in Verbindung mit Anwendungen und Diensten nutzen, auf die Benutzer über einen Browser zugreifen können. Anwendungen, die nicht auf Browsern basieren (auch als *Rich Client-Anwendungen* bezeichnet), die auf dem lokalen Computer installiert sind, z.B. Windows PowerShell, können nicht zusammen mit Konten verwendet werden, bei denen eine zusätzliche Sicherheitsüberprüfung erforderlich ist. In diesem Fall kann die Anwendung den Fehler 0x800434D4L generieren.

Als Problemumgehung können Sie separate Benutzerkonten für Administratoraufgaben und Nicht-Administratoraufgaben erstellen. Später können Sie die Postfächer beider Konten miteinander verknüpfen, sodass Sie sich bei Outlook lediglich über ihr Nicht-Administratorkonto anmelden. Weitere Informationen hierzu, und wie ein Administrator die Möglichkeit erhält, den Inhalt des Postfachs eines Benutzers zu öffnen und anzuzeigen, siehe [Verwalten von Berechtigungen für Empfänger](http://help.outlook.com/141/gg709759.aspx?sl=1).

## <a name="next-steps"></a>Nächste Schritte
Wenn Ihre Frage hier nicht beantwortet wurde, schreiben Sie sie bitte in die Kommentare unten auf der Seite. Alternativ finden Sie hier weitere Optionen zur Hilfesuche:

* Durchsuchen Sie die [Microsoft Support Wissensdatenbank](https://www.microsoft.com/en-us/Search/result.aspx?form=mssupport&q=phonefactor&form=mssupport) nach Lösungen für häufige technische Probleme.
* In den [Azure Active Directory-Foren](https://social.msdn.microsoft.com/Forums/azure/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required)können Sie nach technischen Fragen und Antworten aus der Community suchen oder eine eigene Frage stellen.
* Wenn Sie ein älterer PhoneFactor-Kunde sind und Fragen haben oder Hilfe beim Zurücksetzen eines Kennworts benötigen, verwenden Sie den Link zur [Kennwortzurücksetzung](mailto:phonefactorsupport@microsoft.com) , um eine Supportanfrage zu öffnen.
* Wenden Sie sich über[Azure Multi-Factor Authentication-Server (PhoneFactor) – Support](https://support.microsoft.com/oas/default.aspx?prid=14947) an einen Support-Experten. Es ist hilfreich, wenn Sie uns so viele Informationen wie möglich über Ihr Problem geben, wenn Sie sich mit uns in Verbindung setzen. Diese Informationen können die Website umfassen, auf der der Fehler aufgetreten ist, den spezifischen Fehlercode, die spezifische Sitzungs-ID, und die ID des Benutzers, der den Fehler beobachtet hat.




<!--HONumber=Jan17_HO1-->


