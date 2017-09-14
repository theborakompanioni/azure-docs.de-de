---
title: "Verwaltung des einmaligen Anmeldens für Unternehmens-Apps in Azure Active Directory | Microsoft-Dokumentation"
description: "Verwalten der Einstellungen für einmaliges Anmelden für Unternehmens-Apps in Ihrer Organisation über den Azure Active Directory-Anwendungskatalog"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: bcc954d3-ddbe-4ec2-96cc-3df996cbc899
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/05/2017
ms.author: curtand
ms.reviewer: asmalser
ms.translationtype: HT
ms.sourcegitcommit: 763bc597bdfc40395511cdd9d797e5c7aaad0fdf
ms.openlocfilehash: 73c0917702e2c222f3dc09ddfa2d6d54cf005abf
ms.contentlocale: de-de
ms.lasthandoff: 09/06/2017

---
# <a name="managing-single-sign-on-for-enterprise-apps"></a>Verwalten des einmaligen Anmeldens für Unternehmens-Apps

Dieser Artikel beschreibt die Verwendung des [Azure-Portals](https://portal.azure.com) zum Verwalten der Einstellungen für das einmalige Anmelden für Unternehmensanwendungen. Unternehmens-Apps sind Apps, die innerhalb Ihrer Organisation bereitgestellt und verwendet werden. Dieser Artikel gilt insbesondere für Apps, die aus dem [Azure Active Directory-Anwendungskatalog](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery) hinzugefügt wurden. 

## <a name="finding-your-apps-in-the-portal"></a>Suchen Ihrer Apps im Portal
Alle Unternehmens-Apps, die für das einmalige Anmelden eingerichtet sind, können im Azure-Portal angezeigt und verwaltet werden. Die Anwendungen können im Abschnitt **Weitere Dienste** &gt; **Unternehmensanwendungen** im Portal gefunden werden. 

![Blatt „Unternehmensanwendungen“][1]

Wählen Sie **Alle Anwendungen** aus, um eine Liste aller konfigurierten Apps anzuzeigen. Wenn Sie eine App auswählen, werden die Ressourcen für diese App angezeigt. Hier können Sie Berichte für die App anzeigen und eine Reihe von Einstellungen verwalten.

Zum Verwalten der Einstellungen für einmaliges Anmelden wählen Sie **Einmaliges Anmelden**.

![Blatt „Anwendungsressource“][2]

## <a name="single-sign-on-modes"></a>Modi für einmaliges Anmelden
**Einmaliges Anmelden** beginnt mit einem Menü zum Konfigurieren des **Modus** für einmaliges Anmelden. Die verfügbaren Optionen umfassen:

* **SAML-basierte Anmeldung:** Diese Option ist verfügbar, wenn die Anwendung die vollständige einmalige Verbundanmeldung mit Azure Active Directory unter Verwendung des SAML 2.0-Protokolls unterstützt.
* **Kennwortbasierte Anmeldung:** Diese Option ist verfügbar, wenn Azure AD für diese Anwendung das Ausfüllen eines Kennwortformulars unterstützt.
* **Anmeldung über Link:** Diese Option wurde früher als „Vorhandenes einmaliges Anmelden“ bezeichnet und ermöglicht Administratoren das Platzieren eines Links zu dieser Anwendung im Azure AD-Zugriffsbereich ihres Benutzers oder im Office 365-Anwendungsstartprogramm.

Weitere Informationen zu diesen Modi finden Sie unter [Wie funktioniert das einmalige Anmelden mit Azure Active Directory](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

## <a name="saml-based-sign-on"></a>SAML-basierte Anmeldung
Die Option **SAML-basierte Anmeldung** ist in vier Abschnitte unterteilt:

### <a name="domains-and-urls"></a>Domänen und URLs
Hier werden Ihrem Azure AD-Verzeichnis alle Details zu den Domänen und URLs der Anwendung hinzugefügt. Alle Eingaben, die erforderlich sind, damit das einmalige Anmelden funktioniert, werden direkt auf dem Bildschirm angezeigt, während alle optionalen Eingaben durch Aktivieren des Kontrollkästchens **Erweiterte URL-Einstellungen anzeigen** angezeigt werden können. Die vollständige Liste der unterstützten Eingaben enthält:

* **Anmelde-URL:** Hier melden sich Benutzer bei der Anwendung an. Wenn die Anwendung für das vom Dienstanbieter initiierte einmalige Anmelden konfiguriert ist und ein Benutzer diese URL aufruft, leitet der Dienstanbieter den Benutzer zu Azure AD um, um ihn zu authentifizieren und anzumelden. 
  * Ist dieses Feld ausgefüllt, verwendet Azure AD die URL, um die Anwendung aus Office 365 und über den Azure AD-Zugriffsbereich zu starten.
  * Ist das Feld leer, führt Azure AD stattdessen eine vom Identitätsanbieter initiierte Anmeldung durch, wenn die App über Office 365, den Azure AD-Zugriffsbereich oder die Azure AD-URL für einmaliges Anmelden gestartet wird.
* **Bezeichner:** Mit diesem URI muss die Anwendung, für die das einmalige Anmelden konfiguriert wird, eindeutig identifiziert werden. Dies ist der Wert, der von Azure AD als Audience-Parameter des SAML-Tokens zurück an die Anwendung gesendet wird, und von der Anwendung wird erwartet, dass sie ihn überprüft. Dieser Wert ist auch als Entitäts-ID in SAML-Metadaten enthalten, die von der Anwendung bereitgestellt werden.
* **Antwort-URL:** Unter der Antwort-URL erwartet die Anwendung den Empfang des SAML-Tokens. Sie wird auch als „Assertionsverbraucherdienst-URL“ (Assertion Consumer Service, ACS) bezeichnet. Nachdem Sie diese Informationen eingegeben haben, klicken Sie auf „Weiter“, um den nächsten Bildschirm anzuzeigen. Auf diesem Bildschirm erhalten Sie Informationen darüber, was Sie auf Anwendungsseite konfigurieren müssen, damit diese ein SAML-Token von Azure AD akzeptiert.
* **Relaystatus:** Der Relaystatus ist ein optionaler Parameter, mit dessen Hilfe der Anwendung mitgeteilt werden kann, wohin der Benutzer nach Abschluss der Authentifizierung umzuleiten ist. Der Wert ist in der Regel eine gültige URL für die Anwendung. Einige Anwendungen nutzen dieses Feld jedoch anders. (Einzelheiten finden Sie in der Dokumentation zum einmaligen Anmelden der App.) Die Möglichkeit zum Festlegen des Relaystatus ist eine neue Funktion, die nur im neuen Azure-Portal verfügbar ist.

### <a name="user-attributes"></a>Benutzerattribute
Hier können Administratoren die Attribute anzeigen und bearbeiten, die im SAML-Token gesendet werden, das Azure AD für die Anwendung immer dann ausstellt, wenn sich Benutzer anmelden.

Die **Benutzer-ID** ist das einzige unterstützte Attribut, das bearbeitet werden kann. Der Wert dieses Attributs ist das Feld in Azure AD, das jeden Benutzer innerhalb der Anwendung eindeutig identifiziert. Wenn die Anwendung beispielsweise mit der E-Mail-Adresse als Benutzername und eindeutiger Bezeichner bereitgestellt wurde, wird der Wert auf das Feld „user.mail“ in Azure AD festgelegt.

### <a name="saml-signing-certificate"></a>SAML-Signaturzertifikat
Dieser Abschnitt zeigt die Details des Zertifikats, das Azure AD zum Signieren der SAML-Token verwendet, die immer dann an die Anwendung ausgegeben werden, wenn sich der Benutzer authentifiziert. Dort können die Eigenschaften des aktuellen Zertifikats einschließlich des Ablaufdatums überprüft werden.

### <a name="application-configuration"></a>Anwendungskonfiguration
Der letzte Abschnitt enthält die Dokumentation und/oder die erforderlichen Steuerelemente zum Konfigurieren der Anwendung selbst für die Verwendung von Azure Active Directory als Identitätsanbieter.

Das Flyoutmenü **Anwendung konfigurieren** enthält neue präzise, eingebettete Anweisungen für die Konfiguration der Anwendung. Dies ist ein weiteres neues Feature, das nur im neuen Azure-Portal verfügbar ist.

> [!NOTE]
> Ein vollständiges Beispiel für eine eingebettete Dokumentation finden Sie in der Salesforce.com-Anwendung. Die Dokumentation für zusätzliche Apps wird fortlaufend ergänzt.
> 
> 

![Eingebettete Dokumente][3]

## <a name="password-based-sign-on"></a>Kennwortbasierte Anmeldung
Wenn der Modus für das kennwortbasierte einmalige Anmelden von der Anwendung unterstützt wird, können Sie ihn auswählen und **Speichern** wählen, um sofort die kennwortbasierte einmalige Anmeldung zu konfigurieren. Weitere Informationen zur Bereitstellung der kennwortbasierten einmaligen Anmeldung finden Sie unter [Wie funktioniert das einmalige Anmelden mit Azure Active Directory](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![Kennwortbasierte Anmeldung][4]

## <a name="linked-sign-on"></a>Anmeldung über Link
Wenn der Modus für die Anmeldung über einen Link von der Anwendung unterstützt wird, können Sie ihn auswählen und die URL eingeben, an die Benutzer von vom Azure AD-Zugriffsbereich und Office 365 umgeleitet werden sollen, wenn sie auf diese App klicken. Weitere Informationen zur Anmeldung über einen Link (früher als „Vorhandenes einmaliges Anmelden“ bezeichnet) finden Sie unter [Wie funktioniert das einmalige Anmelden mit Azure Active Directory](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![Verknüpfte Anmeldung][5]

##<a name="feedback"></a>Feedback

Wir hoffen, Ihnen gefällt die verbesserte Azure AD-Benutzeroberfläche. Es wäre schön, wenn Sie uns weiter Feedback senden würden! Feedback und Verbesserungsvorschläge können Sie uns im [Feedbackforum](https://feedback.azure.com/forums/169401-azure-active-directory/category/162510-admin-portal) über den Abschnitt **Verwaltungsportal** zukommen lassen.  Wir haben großen Spaß daran, jeden Tag neue Dinge zu entwickeln, und Ihr Feedback hilft uns, die nächsten Ziele anzugehen und zu definieren.

[1]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade.PNG
[2]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-sso-blade.PNG
[3]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-embedded-docs.PNG
[4]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-password-sso.PNG
[5]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-linked-sso.PNG

