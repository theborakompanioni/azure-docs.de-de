---
title: "Verwaltung des einmaligen Anmeldens für Unternehmens-Apps in Azure Active Directory | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie mithilfe von Azure Active Directory das einmalige Anmelden für Unternehmens-Apps verwalten."
services: active-directory
documentationcenter: 
author: asmalser
manager: femila
editor: 
ms.assetid: bcc954d3-ddbe-4ec2-96cc-3df996cbc899
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2017
ms.author: asmalser
ms.translationtype: Human Translation
ms.sourcegitcommit: 9ae7e129b381d3034433e29ac1f74cb843cb5aa6
ms.openlocfilehash: 118b77064a96585b0d5b951ca56313776021624e
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017


---
# <a name="managing-single-sign-on-for-enterprise-apps"></a>Verwalten des einmaligen Anmeldens für Unternehmens-Apps
> [!div class="op_single_selector"]
> * [Azure-Portal](active-directory-enterprise-apps-manage-sso.md)
> * [klassischen Azure-Portal](active-directory-sso-integrate-saas-apps.md)
> 

Dieser Artikel beschreibt die Verwendung des [Azure-Portals](https://portal.azure.com) zur Verwaltung der Einstellungen für einmaliges Anmelden für Anwendungen, insbesondere für solche, die aus dem [Anwendungskatalog von Azure Active Directory](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery) hinzugefügt wurden. 

## <a name="finding-your-apps"></a>Suchen Ihrer Apps
## <a name="finding-your-apps-in-the-portal"></a>Suchen Ihrer Apps im Portal
Alle Anwendungen, die von einem Verzeichnisadministrator mithilfe des [Azure Active Directory-Anwendungskatalogs](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery) konfiguriert werden, können im [Azure-Portal](https://portal.azure.com) angezeigt und verwaltet werden. Die Anwendungen können im Abschnitt **Weitere Dienste** &gt; **Unternehmensanwendungen** im Portal gefunden werden. Unternehmens-Apps sind Apps, die innerhalb Ihrer Organisation bereitgestellt und verwendet werden.

![Blatt „Unternehmensanwendungen“][1]

Wählen Sie **Alle Anwendungen**, um eine Liste aller konfigurierten Apps anzuzeigen, einschließlich Apps, die aus dem Katalog hinzugefügt wurden. Beim Auswählen einer App wird das Ressourcenblatt die App geladen, in dem Berichte für App angezeigt und eine Reihe von Einstellungen verwaltet werden können.

Zum Verwalten der Einstellungen für einmaliges Anmelden wählen Sie **Einmaliges Anmelden**.

![Blatt „Anwendungsressource“][2]

## <a name="single-sign-on-modes"></a>Modi für einmaliges Anmelden
Das Blatt **Einmaliges Anmelden** beginnt mit einem **Modus**-Menü, mit dem der Modus für einmaliges Anmelden konfiguriert werden kann. Die verfügbaren Optionen umfassen:

* **SAML-basierte Anmeldung:** Diese Option ist verfügbar, wenn die Anwendung die vollständige einmalige Verbundanmeldung mit Azure Active Directory mit dem SAML 2.0-Protokoll unterstützt.
* **Kennwortbasierte Anmeldung**: Diese Option ist verfügbar, wenn Azure AD für diese Anwendung das Ausfüllen eines Kennwortformulars unterstützt.
* **Anmeldung über Link:** Diese Option wurde früher als „Vorhandenes einmaliges Anmelden“ bezeichnet und ermöglicht Administratoren das Platzieren eines Links zu dieser Anwendung im Azure AD-Zugriffsbereich ihres Benutzers oder im Office 365-Anwendungsstartprogramm.

Weitere Informationen zu diesen Modi finden Sie unter [Wie funktioniert das einmalige Anmelden mit Azure Active Directory](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

## <a name="saml-based-sign-on"></a>SAML-basierte Anmeldung
Die Option **SAML-basierte Anmeldung** zeigt ein Blatt, das in vier Abschnitte unterteilt ist:

### <a name="domains-and-urls"></a>Domänen und URLs
Hier werden Ihrem Azure AD-Verzeichnis alle Details zu den Domänen und URLs der Anwendung hinzugefügt. Alle Eingaben, die erforderlich sind, damit das einmalige Anmelden funktioniert, werden direkt auf dem Bildschirm angezeigt, während alle optionalen Eingaben durch Aktivieren des Kontrollkästchens **Erweiterte URL-Einstellungen anzeigen** angezeigt werden können. Die vollständige Liste der unterstützten Eingaben enthält:

* **Anmelde-URL:** Hiermit melden sich Benutzer bei dieser Anwendung an. Sofern die Anwendung für die Durchführung des vom Dienstanbieter initiierten einmaligen Anmeldens konfiguriert ist, passiert Folgendes, wenn ein Benutzer zu dieser URL navigiert: Der Dienstanbieter führt die Umleitung an Azure AD durch, um den Benutzer zu authentifizieren und anzumelden. Wenn dieses Feld ausgefüllt wird, verwendet Azure AD diese URL zum Starten der Anwendung aus Office 365 und über den Azure AD-Zugriffsbereich. Falls Sie dieses Feld leer lassen, führt Azure AD stattdessen eine vom Identitätsanbieter initiierte Anmeldung durch, wenn die App über Office 365, den Azure AD-Zugriffsbereich oder die Azure AD-URL für das einmalige Anmelden gestartet wird.
* **Bezeichner:** Mit diesem URI sollte die Anwendung, für die das einmalige Anmelden konfiguriert wird, eindeutig identifiziert werden. Dies ist der Wert, der von Azure AD als Audience-Parameter des SAML-Tokens zurück an die Anwendung gesendet wird, und von der Anwendung wird erwartet, dass sie ihn überprüft. Dieser Wert ist auch als Entitäts-ID in SAML-Metadaten enthalten, die von der Anwendung bereitgestellt werden.
* **Antwort-URL:** Unter der Antwort-URL erwartet die Anwendung den Empfang des SAML-Tokens. Sie wird auch als „Assertionsverbraucherdienst-URL“ (Assertion Consumer Service, ACS) bezeichnet. Nachdem Sie diese Informationen eingegeben haben, klicken Sie auf „Weiter“, um den nächsten Bildschirm anzuzeigen. Auf diesem Bildschirm erhalten Sie Informationen darüber, was Sie auf Anwendungsseite konfigurieren müssen, damit diese ein SAML-Token von Azure AD akzeptiert.
* **Relaystatus:** Der Relaystatus ist ein optionaler Parameter, mit dessen Hilfe der Anwendung mitgeteilt werden kann, wohin der Benutzer nach Abschluss der Authentifizierung umzuleiten ist. Der Wert ist in der Regel eine gültige URL in der Anwendung, einige Anwendungen nutzen dieses Feld jedoch anders (Einzelheiten finden Sie in der Dokumentation zum einmaligen Anmelden der App). Die Möglichkeit zum Festlegen des Relaystatus ist eine neue Funktion, die nur im neuen Azure-Portal verfügbar ist.

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

