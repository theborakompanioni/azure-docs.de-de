---
title: "Häufig gestellte Fragen zu Azure Active Directory B2C | Microsoft Docs"
description: "Häufig gestellte Fragen zu Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: ed33c2ca-76d0-442a-abb1-8b7b7bb92d6a
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2017
ms.author: swkrish
ms.translationtype: Human Translation
ms.sourcegitcommit: 9ae7e129b381d3034433e29ac1f74cb843cb5aa6
ms.openlocfilehash: bc0b0b652312f6bcc27a981da766e19cded7bd6c
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017


---
# <a name="azure-active-directory-b2c-faqs"></a>Häufig gestellte Fragen zu Azure Active Directory B2C
Auf dieser Seite werden häufig gestellte Fragen zu Azure Active Directory (Azure AD) B2C beantwortet. Die Seite wird bei Bedarf aktualisiert.

### <a name="can-i-use-azure-ad-b2c-features-in-my-existing-employee-based-azure-ad-tenant"></a>Kann ich Azure AD B2C-Features in meinem vorhandenen mitarbeiterbasierten Azure AD Directory-Mandanten nutzen?
Derzeit können Azure AD B2C-Features in Ihrem vorhandenen Azure AD-Mandanten noch nicht aktiviert werden. Es wird empfohlen, einen separaten Mandanten zu erstellen, um Ihre Endkunden mit Azure AD B2C-Features zu verwalten.

### <a name="can-i-use-azure-ad-b2c-to-provide-social-login-facebook-and-google-into-office-365"></a>Kann ich Azure AD B2C zum Bereitstellen der Anmeldung für soziale Netzwerke (Facebook und Google+) in Office 365 verwenden?
Azure AD B2C kann nicht mit Microsoft Office 365 verwendet werden. Es kann generell nicht für die Bereitstellung der Authentifizierung für SaaS-Apps (Office 365, Salesforce, Workday usw.) verwendet werden. Es ermöglicht nur die Identitäts- und Zugriffsverwaltung für endkundenorientierte Webanwendungen und mobile Anwendungen und ist nicht für Mitarbeiter- oder Partnerszenarien ausgelegt.

### <a name="what-are-local-accounts-in-azure-ad-b2c-how-are-they-different-from-work-or-school-accounts-in-azure-ad"></a>Was sind lokale Konten in Azure AD B2C? Wie unterscheiden sie sich von Geschäfts-, Schul- oder Unikonten in Azure AD?
In einem Azure AD-Mandanten meldet sich jeder Benutzer im Mandanten (mit Ausnahme von Benutzern mit vorhandenen Microsoft-Konten) mit einer E-Mail-Adresse im Format `<xyz>@<tenant domain>` an. Hierbei ist `<tenant domain>` eine der überprüften Domänen im Mandanten oder die ursprüngliche `<...>.onmicrosoft.com`-Domäne. Dieser Kontotyp ist ein Geschäfts-, Schul- oder Unikonto.

Bei einem Azure AD B2C-Mandanten kann sich der Benutzer in den meisten Apps mit einer beliebigen E-Mail-Adresse anmelden (also beispielsweise mit joe@comcast.net, bob@gmail.com, sarah@contoso.com oder jim@live.com). Diese Art von Konto ist ein lokales Konto. Jetzt werden auch beliebige Benutzernamen (einfache Zeichenfolgen) als lokale Konten unterstützt (z. B. joe, bob, sarah oder jim). Sie können eine dieser beiden lokalen Kontotypen im Azure AD B2C-Dienst auswählen.

### <a name="which-social-identity-providers-do-you-support-now-which-ones-do-you-plan-to-support-in-the-future"></a>Welche Identitätsanbieter aus sozialen Netzwerken werden derzeit unterstützt? Welche sollen in Zukunft unterstützt werden?
Gegenwärtig werden Facebook, Google+, LinkedIn und Amazon unterstützt. Die Unterstützung für andere beliebte soziale Identitätsanbieter wird je nach Kundennachfrage hinzugefügt.

### <a name="can-i-configure-scopes-to-gather-more-information-about-consumers-from-various-social-identity-providers"></a>Kann ich Bereiche konfigurieren, um von verschiedenen Identitätsanbietern aus sozialen Netzwerken mehr Informationen zu Endkunden zu erhalten?
Nein, aber dieses Feature ist geplant. Die Standardbereiche für unsere unterstützten sozialen Identitätsanbieter sind:

* Facebook: email
* Google+: email
* Microsoft-Konto: openid email profile
* Amazon: profile
* LinkedIn: r_emailaddress, r_basicprofile

### <a name="does-my-application-have-to-be-run-on-azure-for-it-work-with-azure-ad-b2c"></a>Muss meine Anwendung unter Azure ausgeführt werden, damit sie mit Azure AD B2C funktioniert?
Nein, Sie können Ihre Anwendung überall hosten (in der Cloud oder lokal). Für die Interaktion mit Azure AD B2C muss nur die Fähigkeit zum Senden und Empfangen von HTTP-Anforderungen über öffentlich zugängliche Endpunkte möglich sein.

### <a name="i-have-multiple-azure-ad-b2c-tenants-how-can-i-manage-them-on-the-azure-portal"></a>Ich habe mehrere Azure AD B2C-Mandanten. Wie kann ich sie im Azure-Portal verwalten?
Jeder Azure AD B2C-Mandant verfügt im Azure-Portal über ein eigenes Blatt mit B2C-Funktionen. Informationen dazu, wie Sie im Azure-Portal zum Blatt mit den B2C-Funktionen eines bestimmten Mandanten navigieren, finden Sie unter [Azure AD B2C: Registrieren Ihrer Anwendung](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) . Beim Wechseln zwischen Azure AD B2C-Verzeichnissen im Azure-Portal bleibt das Blatt mit den B2C-Funktionen in den meisten Browsern nicht geöffnet.

### <a name="how-do-i-customize-verification-emails-the-content-and-the-from-field-sent-by-azure-ad-b2c"></a>Wie kann ich Bestätigungs-E-Mails anpassen (Inhalt und das Feld „Von:“), die von Azure AD B2C gesendet werden?
Sie können das [Feature für Unternehmensbranding](../active-directory/active-directory-add-company-branding.md) verwenden, um den Inhalt von Bestätigungs-E-Mails anzupassen. Insbesondere diese beiden Elemente einer E-Mail können angepasst werden:

* **Bannerlogo**: Wird unten rechts angezeigt.
* **Hintergrundfarbe**: Wird oben angezeigt.
  
    ![Screenshot einer angepassten Bestätigungs-E-Mail](./media/active-directory-b2c-faqs/company-branded-verification-email.png)

Die E-Mail-Signatur enthält den Namen des B2C-Mandanten, den Sie bei der ursprünglichen Erstellung des B2C-Mandanten angegeben haben. Sie können den Namen mit folgenden Schritten ändern:

* Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com/) als Abonnementadministrator an.
* Navigieren Sie zu Ihrem B2C-Mandanten.
* Klicken Sie auf die Registerkarte **Konfigurieren** .
* Ändern Sie im Abschnitt **Verzeichniseigenschaften** das Feld **Name**.
* Klicken Sie unten auf der Seite auf **Speichern** .

Zurzeit besteht keine Möglichkeit, das Feld „Von:“ der E-Mail zu ändern. Wenn Sie an dieser Funktionalität und einer vollständigen Bearbeitbarkeit des Texts der Bestätigungs-E-Mail interessiert sind, stimmen Sie unter [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/15334335-fully-customizable-verification-emails)für dieses Feature ab.

### <a name="how-can-i-migrate-my-existing-user-names-passwords-and-profiles-from-my-database-to-azure-ad-b2c"></a>Wie kann ich meine vorhandenen Benutzernamen, Kennwörter und Profile von meiner Datenbank zu Azure AD B2C migrieren?
Sie können Ihr Migrationstool mithilfe der Azure AD Graph-API schreiben. Einzelheiten finden Sie im [Graph-API-Beispiel](active-directory-b2c-devquickstarts-graph-dotnet.md) . In Zukunft werden verschiedene Migrationsoptionen und Tools standardmäßig vorhanden sein.

### <a name="what-password-policy-is-used-for-local-accounts-in-azure-ad-b2c"></a>Welche Kennwortrichtlinie wird für lokale Konten in Azure AD B2C verwendet?
Die Kennwortrichtlinie von Azure AD B2C für lokale Konten basiert auf der Kennwortrichtlinie von Azure AD. Die Azure AD B2C-Richtlinien zur Registrierung, Anmeldung und Kennwortrücksetzung verwenden sichere Kennwörter, und die Kennwörter laufen nicht ab. Weitere Informationen finden Sie in der [Kennwortrichtlinie von Azure AD](https://msdn.microsoft.com/library/azure/jj943764.aspx) .

### <a name="can-i-use-azure-ad-connect-to-migrate-consumer-identities-that-are-stored-on-my-on-premises-active-directory-to-azure-ad-b2c"></a>Kann ich Azure AD Connect zum Migrieren von Endkundenidentitäten, die in meinem lokalen Active Directory gespeichert sind, zu Azure AD B2C verwenden?
Nein. Azure AD Connect ist nicht für die Verwendung mit Azure AD B2C ausgelegt. In Zukunft werden verschiedene Migrationsoptionen und Tools standardmäßig vorhanden sein.

### <a name="can-my-app-open-up-azure-ad-b2c-pages-within-an-iframe"></a>Kann meine App Azure AD B2C-Seiten in einem iFrame öffnen?
Nein. Aus Sicherheitsgründen können Azure AD B2C-Seiten nicht in einem iFrame geöffnet werden.  Unser Dienst kommuniziert mit dem Browser, um dies zu unterbinden.  In allgemeinen Sicherheitskreisen und in der OAUTH2-Spezifikation wird davon abgeraten, iFrames für Identitätszwecke zu verwenden, weil dies mit einem Clickjacking-Risiko verbunden ist.

### <a name="does-azure-ad-b2c-work-with-crm-systems-such-as-microsoft-dynamics"></a>Funktioniert Azure AD B2C in Verbindung mit CRM-Systemen wie Microsoft Dynamics?
Derzeit ist dies nicht möglich. Die Integration dieser Systeme ist aber geplant.

### <a name="does-azure-ad-b2c-work-with-sharepoint-on-premises-2016-or-earlier"></a>Funktioniert Azure AD B2C in Verbindung mit SharePoint On-Premises 2016 oder früher?
Derzeit ist dies nicht möglich. Azure AD B2C bietet keine Unterstützung für SAML 1.1-Token, die von Portalen und E-Commerce-Anwendungen benötigt werden, die auf SharePoint On-Premises beruhen. Beachten Sie, dass Azure AD B2C nicht für das SharePoint-Freigabeszenario für externe Partner vorgesehen ist. Informieren Sie sich dazu unter [Azure AD B2B](http://blogs.technet.com/b/ad/archive/2015/09/15/learn-all-about-the-azure-ad-b2b-collaboration-preview.aspx).

### <a name="should-i-use-azure-ad-b2c-or-b2b-to-manage-external-identities"></a>Sollte ich Azure AD B2C oder B2B zum Verwalten externer Identitäten verwenden?
Lesen Sie diesen Artikel über [externe Identitäten](../active-directory/active-directory-b2b-compare-external-identities.md) , um mehr über das Anwenden der entsprechenden Features in Ihren Szenarien mit externen Identitäten zu erfahren.

### <a name="what-reporting-and-auditing-features-does-azure-ad-b2c-provide-are-they-the-same-as-in-azure-ad-premium"></a>Welche Funktionen für Berichterstellung und Überwachung bietet Azure AD B2C? Sind die Funktionen mit denen in Azure AD Premium identisch?
Nein. Azure AD B2C unterstützt nicht den gleichen Berichtssatz wie Azure AD Premium. Es gibt aber viele Gemeinsamkeiten.  
* Die Anmeldeberichte enthalten einen Eintrag für jede Anmeldung mit reduzierten Details.  
* Überwachungsberichte sind im Azure-Portal unter „Azure Active Directory“ > „ACTIVITY-Audit logs“ (ACTIVITY-Überwachungsprotokolle) verfügbar. Wählen Sie die Option „B2C“, und wenden Sie je nach Bedarf Filter an. Sowohl die Administratoraktivität als auch die Anwendungsaktivität sind abgedeckt. 
* Ein Nutzungsbericht mit Angaben zur Anzahl von Benutzern, zur Anzahl von Anmeldungen und zum MFA-Umfang ist unter [Zugriff auf Verwendungsberichte in Azure AD B2C über die Berichterstattungs-API](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-usage-reporting-api) verfügbar.

### <a name="can-i-localize-the-ui-of-pages-served-by-azure-ad-b2c-what-languages-are-supported"></a>Kann ich die Benutzeroberfläche von Seiten lokalisieren, die von Azure AD B2C bereitgestellt werden? Welche Sprachen werden unterstützt?
Derzeit ist Azure AD B2C nur für Englisch optimiert. Es ist geplant, die Lokalisierungsfeatures so bald wie möglich bereitzustellen.

### <a name="can-i-use-my-own-urls-on-my-sign-up-and-sign-in-pages-that-are-served-by-azure-ad-b2c-for-instance-can-i-change-the-url-from-loginmicrosoftonlinecom-to-logincontosocom"></a>Kann ich auf meinen von Azure AD B2C bereitgestellten Seiten für die Registrierung und Anmeldung meine eigenen URLs verwenden? Kann ich beispielsweise die URL von „login.microsoftonline.com“ in „login.contoso.com“ ändern?
Derzeit ist dies nicht möglich. Dieses Feature ist aber geplant. Beachten Sie auch, dass dies nicht durch Überprüfen Ihrer Domäne auf der Registerkarte **Domänen** für den Mandanten im klassischen Azure-Portal erfolgen kann.

### <a name="how-do-i-delete-my-azure-ad-b2c-tenant"></a>Wie lösche ich meinen Azure AD B2C-Mandanten?
Führen Sie die folgenden Schritte aus, um Ihren Azure AD B2C-Mandanten zu löschen:

* Führen Sie die folgenden Schritte aus, um im Azure-Portal [zum Blatt „B2C-Funktionen“ zu navigieren](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) .
* Navigieren Sie zu den Blättern **Anwendungen**, **Identitätsanbieter** und **Alle Richtlinien**, und löschen Sie jeweils alle Einträge.
* Melden Sie sich jetzt beim [klassischen Azure-Portal](https://manage.windowsazure.com/) als Abonnementadministrator an. (Dies ist dasselbe Geschäfts-, Schul- oder Unikonto bzw. dasselbe Microsoft-Konto, mit dem Sie sich bei Azure registriert haben.)
* Navigieren Sie links zur Active Directory-Erweiterung, und klicken Sie auf Ihren B2C-Mandanten.
* Klicken Sie auf die Registerkarte **Benutzer** .
* Wählen Sie nacheinander die einzelnen Benutzer aus – mit Ausnahme des Benutzers, als der Sie gerade angemeldet sind (Abonnementadministrator). Klicken Sie im unteren Bereich der Seite auf **Löschen**, und klicken Sie auf **Ja**, wenn Sie zur Bestätigung aufgefordert werden.
* Klicken Sie auf die Registerkarte **Anwendungen** .
* Wählen Sie im Dropdownfeld **Anzeigen** die Einstellung **Anwendungen im Besitz meines Unternehmens** aus, und klicken Sie auf das Häkchen.
* Eine Anwendung namens **b2c-extensions-app** wird angezeigt. Klicken Sie im unteren Bereich der Seite auf **Löschen**, und klicken Sie auf **Ja**, wenn Sie zur Bestätigung aufgefordert werden.
* Navigieren Sie erneut zur Active Directory-Erweiterung, und wählen Sie Ihren B2C-Mandanten aus.
* Klicken Sie unten auf der Seite auf **Löschen** . Folgen Sie den Anweisungen auf dem Bildschirm, um den Vorgang abzuschließen.

### <a name="can-i-get-azure-ad-b2c-as-part-of-enterprise-mobility-suite"></a>Kann ich Azure AD B2C als Teil der Enterprise Mobility Suite erwerben?
Nein. Azure AD B2C ist ein Azure-Dienst mit nutzungsbasierter Bezahlung und nicht Teil der Enterprise Mobility Suite.

### <a name="how-do-i-report-issues-with-azure-ad-b2c"></a>Wie melde ich Probleme mit Azure AD B2C?
Siehe [Senden von Supportanfragen für Azure Active Directory B2C](active-directory-b2c-support.md).

## <a name="more-information"></a>Weitere Informationen
Informieren Sie sich auch über die aktuellen [Einschränkungen des Diensts](active-directory-b2c-limitations.md).


