<properties
	pageTitle="Bedingter Zugriff unter Azure für SaaS-Apps | Microsoft Azure"
	description="Der bedingte Zugriff in Azure AD ermöglicht Ihnen die anwendungsspezifische Konfiguration von Zugriffsregeln für die mehrstufige Authentifizierung und das Blockieren des Zugriffs für Benutzer, die nicht zu einem vertrauenswürdigen Netzwerk gehören. "
	services="active-directory"
	documentationCenter=""
	authors="markusvi"
	manager="femila"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/14/2016"
	ms.author="markvi"/>

# Erste Schritte mit dem bedingten Zugriff in Azure Active Directory

Der bedingte Zugriff mit Azure Active Directory für [SaaS](https://azure.microsoft.com/overview/what-is-saas/)-Apps und verbundene Azure AD-Apps ermöglicht den bedingten Zugriff basierend auf Gruppe, Standort und Anwendungsvertraulichkeit.

Mit dem bedingten Zugriff auf Grundlage der Anwendungsvertraulichkeit können Sie anwendungsspezifische MFA-Zugriffsregeln (Multi-Factor Authentication, mehrstufige Authentifizierung) festlegen. Mithilfe der anwendungsspezifischen MFA-Zugriffsregeln können Sie den Zugriff für Benutzer blockieren, die sich nicht in einem vertrauenswürdigen Netzwerk befinden. Die MFA-Regeln können auf alle Benutzer angewendet werden, die einer Anwendung zugewiesen sind, oder nur auf Benutzer in angegebenen Sicherheitsgruppen. Benutzer können von der Pflicht zur mehrstufigen Authentifizierung ausgenommen werden, wenn sie von einer IP-Adresse innerhalb des Netzwerks der Organisation auf die Anwendung zugreifen.

Diese Funktionen stehen Kunden zur Verfügung, die eine Azure Active Directory Premium-Lizenz erworben haben.

## Voraussetzungen für das Szenario
* Lizenz für Azure Active Directory Premium

* Verbundbasierter oder verwalteter Azure Active Directory-Mandant

* Verbundmandanten erfordern die Aktivierung der mehrstufigen Authentifizierung.

## Konfigurieren anwendungsspezifischer Zugriffsregeln

Dieser Abschnitt beschreibt die Konfiguration anwendungsspezifischer Zugriffsregeln.

1. Melden Sie sich beim klassischen Azure-Portal mit einem Konto an, das ein globaler Administrator für Azure AD ist.
2. Wählen Sie im linken Bereich **Active Directory** aus.
3. Wählen Sie auf der Registerkarte "Verzeichnis" Ihr Verzeichnis aus.
4. Wählen Sie die Registerkarte **Anwendungen** aus.
5. Wählen Sie die Anwendung, für die Sie die Regel festlegen.
6. Wählen Sie die Registerkarte **Konfigurieren** aus.
7. Führen Sie einen Bildlauf nach unten zum Zugriffsregelabschnitt aus. Wählen Sie die gewünschte Zugriffsregel aus.
8. Geben Sie die Benutzer an, für die die Richtlinie gilt.
9. Aktivieren Sie die Richtlinie, indem Sie **"Aktiviert" für "Ein"** auswählen.

##Grundlegendes zu Zugriffsregeln

Dieser Abschnitt enthält eine ausführliche Beschreibung der Zugriffsregeln, die in der Azure-Funktion für bedingten Anwendungszugriff unterstützt werden.

### Angeben der Benutzer, für die die Zugriffsregeln gelten

Standardmäßig gilt die Richtlinie für alle Benutzer, die auf die Anwendung zugreifen können. Allerdings kann die Richtlinie auch auf Benutzer beschränkt werden, die den angegebenen Sicherheitsgruppen angehören. Über die Schaltfläche **Gruppe hinzufügen** können Sie eine oder mehrere Gruppen im Dialogfeld zur Gruppenauswahl auswählen, auf die dann die Zugriffsregel angewendet wird. Dieses Dialogfeld kann auch zum Entfernen ausgewählter Gruppen verwendet werden. Wenn die Regeln auf Gruppen angewendet werden sollen, werden die Zugriffsregeln nur für Benutzer erzwungen, die einer der angegebenen Sicherheitsgruppen angehören.

Sicherheitsgruppen können auch explizit von der Richtlinie ausgeschlossen werden, indem Sie die Ausnahmeoption aktivieren und mindestens eine Gruppe angeben. Benutzer, die Mitglied einer Gruppe in der Ausnahmenliste sind, unterliegen nicht der Pflicht zur mehrstufigen Authentifizierung, selbst wenn sie einer Gruppe angehören, für die diese Regel gilt. Die unten gezeigte Zugriffsregel verlangt von allen Benutzern in der Manager-Gruppe, dass sie über die mehrstufige Authentifizierung auf die Anwendung zugreifen.

![Festlegen bedingter Zugriffsregeln mit MFA](./media/active-directory-conditional-access-azuread-connected-apps/conditionalaccess-saas-apps.png)

## Regeln für den bedingten Zugriff mit MFA
Wenn ein Benutzer mit der benutzerspezifischen mehrstufigen Authentifizierung konfiguriert wurde, wird diese Einstellung für den Benutzer mit den Regeln der App für die mehrstufige Authentifizierung kombiniert. Dies bedeutet, dass ein Benutzer, für den die benutzerspezifische mehrstufige Authentifizierung konfiguriert wurde, auch dann die mehrstufige Authentifizierung ausführen muss, wenn er aus den anwendungsspezifischen Regeln für die mehrstufige Authentifizierung ausgenommen wurde. Hier erfahren Sie mehr über die mehrstufige Authentifizierung und benutzerspezifische Einstellungen.

### Zugriffsregeloptionen
Die folgenden Optionen werden unterstützt:

* **Erfordert mehrstufige Authentifizierung**: Benutzer, für die die Zugriffsregeln gelten, müssen die mehrstufige Authentifizierung durchlaufen, um auf die Anwendung zugreifen zu können, für die die Richtlinie gilt.

* **Erfordert mehrstufige Authentifizierung, wenn nicht bei der Arbeit**: Ein Benutzer, der von einer vertrauenswürdigen IP-Adresse aus zugreift, muss keine mehrstufige Authentifizierung durchlaufen. Die Bereiche für vertrauenswürdige IP-Adressen können auf den Einstellungsseiten für die mehrstufige Authentifizierung konfiguriert werden.

* **Zugriff blockieren, wenn nicht gearbeitet wird**: Benutzer, die nicht von einer vertrauenswürdigen IP-Adresse aus zugreifen, werden blockiert. Die Bereiche für vertrauenswürdige IP-Adressen können auf den Einstellungsseiten für die mehrstufige Authentifizierung konfiguriert werden.

### Festlegen des Regelstatus
Über den Zugriffsregelstatus können die Regeln aktiviert oder deaktiviert werden. Wenn die Zugriffsregeln deaktiviert sind, wird die Anforderung zur mehrstufigen Authentifizierung nicht erzwungen.

### Zugriffsregelauswertung

Zugriffsregeln werden ausgewertet, wenn ein Benutzer auf eine Verbundanwendung zugreift, die OAuth 2.0, OpenID Connect, SAML oder WS-Federation verwendet. Darüber hinaus werden Zugriffsregeln ausgewertet, wenn von OAuth 2.0 und OpenID Connect ein Aktualisierungstoken zum Abrufen eines Zugriffstokens verwendet wird. Wenn die Richtlinienauswertung bei Verwendung eines Aktualisierungstokens nicht erfolgreich ist, wird der Fehler**invalid\_grant** zurückgegeben. Dies bedeutet, dass sich der Benutzer erneut beim Client authentifizieren muss.

###Konfigurieren von Verbunddiensten für die Bereitstellung der mehrstufigen Authentifizierung

Für Verbundmandanten kann MFA von Azure Active Directory oder vom lokalen AD FS-Server durchgeführt werden.

Standardmäßig erfolgt die MFA auf einer Seite, die von Azure Active Directory gehostet wird. Um die MFA lokal zu konfigurieren, muss die Eigenschaft**–SupportsMFA** in Azure Active Directory mithilfe des Azure AD-Moduls für Windows PowerShell auf **true** festgelegt werden.

Im folgenden Beispiel wird veranschaulicht, wie lokale MFA mithilfe des [Set-MsolDomainFederationSettings-Cmdlets](https://msdn.microsoft.com/library/azure/dn194088.aspx) auf dem Mandanten "contoso.com" aktiviert wird:

    Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true

Zusätzlich zum Festlegen dieses Kennzeichens muss die AD FS-Instanz des Verbundmandanten für die Ausführung der mehrstufigen Authentifizierung konfiguriert werden. Befolgen Sie die Anweisungen für das[lokale Bereitstellen von Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## Verwandte Artikel

- [Sichern des Zugriffs auf Office 365 und andere mit Azure Active Directory verbundene Apps](active-directory-conditional-access.md)
- [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)

<!---HONumber=AcomDC_0914_2016-->