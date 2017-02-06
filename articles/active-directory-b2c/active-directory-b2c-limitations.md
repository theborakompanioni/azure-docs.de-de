---
title: "Azure Active Directory B2C: Einschränkungen | Microsoft Docs"
description: "Enthält eine Liste der Einschränkungen in Bezug auf Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 04ec3310-98bb-4bb1-856d-ddc66913b390
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
translationtype: Human Translation
ms.sourcegitcommit: 3ff8fba42e6455b33103c931da731b0affa8a0fb
ms.openlocfilehash: b5fbd15729da2674b34a227861e65b89548dad39


---
# <a name="azure-active-directory-b2c-limitations-and-restrictions"></a>Azure Active Directory B2C: Einschränkungen
Es gibt mehrere Features und Funktionen von Azure Active Directory (Azure AD) B2C, die noch nicht unterstützt werden. Viele dieser bekannten Probleme und Einschränkungen sollen in der Zukunft beseitigt werden, aber Sie sollten sich darüber bewusst sein, wenn Sie Anwendungen für Endkunden mit Azure AD B2C erstellen.

## <a name="issues-during-the-creation-of-azure-ad-b2c-tenants"></a>Probleme bei der Erstellung von Azure AD B2C-Mandanten
Wenn während der [Erstellung eines Azure AD B2C-Mandanten](active-directory-b2c-get-started.md) Probleme auftreten, hilft Ihnen die Anleitung unter [Erstellen eines Azure Active Directory (Azure AD)-Mandanten oder eines Azure AD B2C-Mandanten – Probleme und Lösungen](active-directory-b2c-support-create-directory.md) weiter.

Beachten Sie, dass beim Löschen eines vorhandenen B2C-Mandanten und erneuten Erstellen mit demselben Domänennamen bekannte Probleme auftreten. Sie müssen einen B2C-Mandanten mit einem anderen Domänennamen erstellen.

## <a name="note-about-b2c-tenant-quotas"></a>Hinweis zu B2C-Mandantenkontingenten
Standardmäßig ist die Anzahl von Benutzern in einem B2C-Mandanten auf 50.000 Benutzer begrenzt. Wenn Sie das Kontingent für den B2C-Mandanten erhöhen möchten, sollten Sie sich an den Support wenden.

## <a name="branding-issues-on-verification-email"></a>Probleme beim Branding in der Bestätigungs-E-Mail
Die Standard-Bestätigungs-E-Mail ist mit dem Microsoft-Branding versehen. Dieses wird künftig entfernt. Vorläufig können Sie es mit dem [Feature für Unternehmensbranding](../active-directory/active-directory-add-company-branding.md)entfernen.

## <a name="branding-issues-on-local-account-sign-in-page-in-a-sign-in-policy"></a>Probleme beim Branding auf lokalen Kontoanmeldeseite in einer Anmelderichtlinie
Die lokale Kontoanmeldeseite in einer Anmelderichtlinie kann nur mit dem [Feature für Unternehmensbranding](../active-directory/active-directory-add-company-branding.md) angepasst werden, mit dem [hier](active-directory-b2c-reference-ui-customization.md) beschriebenen Anpassungsfeature für die Seiten-UI ist dies nicht möglich. Darüber hinaus sind keine Bezeichnungen oder der Platzhalter für die Felder „Benutzername“ und „Kennwort“ verfügbar. Um dieses Problem zu umgehen, wird empfohlen, stattdessen die vollständig anpassbare „Registrierungs- oder Anmelderichtlinie“ zu verwenden. Wenn Sie daran interessiert sind, die lokale Kontoanmeldeseite in einer Anmelderichtlinie vollständig anzupassen, stimmen Sie auf [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/13062033-b2c-fully-customizable-sign-in-page) für diese Funktion.

## <a name="restrictions-on-applications"></a>Einschränkungen für Anwendungen
Die unten angegebenen Arten von Anwendungen werden in Azure AD B2C derzeit nicht unterstützt. Eine Beschreibung der unterstützten Anwendungstypen finden Sie unter [Azure AD B2C: Anwendungstypen](active-directory-b2c-apps.md).

### <a name="daemons--server-side-applications"></a>Daemons/serverseitige Anwendungen
Anwendungen, die lang andauernde Prozesse enthalten oder ohne Benutzereingriff arbeiten, benötigen auch die Möglichkeit, auf sichere Ressourcen wie Web-APIs zuzugreifen. Diese Anwendungen können mithilfe der App-Identität (anstelle der delegierten Endkundenidentität) im [OAuth 2.0-Ablauf für Clientanmeldeinformationen](active-directory-b2c-reference-protocols.md) die Authentifizierung durchführen und Token abrufen. Dieser Ablauf ist in Azure AD B2C noch nicht verfügbar. Vorläufig können Anwendungen erst Token erhalten, nachdem ein interaktiver Endkunden-Anmeldeablauf stattgefunden hat.

### <a name="standalone-web-apis"></a>Eigenständige Web-APIs
In Azure AD B2C können Sie eine [Web-API erstellen, die mit OAuth 2.0-Token geschützt wird](active-directory-b2c-apps.md#web-apis). Allerdings kann die Web-API nur Token von einem Client empfangen, der die gleiche Anwendungs-ID aufweist. Das Erstellen einer Web-API, auf die von mehreren unterschiedlichen Clients zugegriffen werden kann, wird nicht unterstützt.

### <a name="web-api-chains-on-behalf-of"></a>Web-API-Ketten (Im Auftrag von)
Viele Architekturen umfassen eine Web-API, von der eine andere Downstream-Web-API aufgerufen werden muss, wobei beide durch Azure AD B2C gesichert sind. Dieses Szenario kommt häufig bei nativen Clients mit Web-API-Back-End vor, das wiederum einen Microsoft Online-Dienst aufruft, z. B. die Azure AD Graph-API.

Dieses Szenario der verketteten Web-API kann mithilfe der Berechtigung für Anmeldeinformationen über den OAuth 2.0-JWT-Bearer unterstützt werden, auch bekannt als „Im Auftrag von“-Ablauf. Der „Im Auftrag von“-Ablauf ist in Azure AD B2C derzeit noch nicht implementiert.

## <a name="restriction-on-libraries-and-sdks"></a>Einschränkungen für Bibliotheken und SDKs
Die Gruppe der von Microsoft unterstützten Bibliotheken, die mit Azure AD B2C verwendet werden können, ist zurzeit noch sehr beschränkt. Es ist Unterstützung für .NET-basierte Web-Apps und Dienste sowie für NodeJS-Web-Apps und -Dienste verfügbar.  Außerdem ist eine Vorschauversion der .NET-Clientbibliothek (MSAL) vorhanden, die mit Azure AD B2C in Windows-Apps und anderen .NET-Apps verwendet werden kann.

Derzeit werden noch keine Bibliotheken für andere Sprachen oder Plattformen unterstützt, z.B. iOS und Android.  Wenn Sie die Erstellung auf einer anderen Plattform als den oben beschriebenen durchführen möchten, empfehlen wir Ihnen die Verwendung eines Open Source-SDKs. Bei Bedarf finden Sie hierzu weitere Informationen unter [OAuth 2.0- und OpenID Connect-Protokollreferenz](active-directory-b2c-reference-protocols.md).  Azure AD B2C implementiert OAuth und OpenID Connect, um es zu ermöglichen, dass eine generische OAuth- oder OpenID Connect-Bibliothek für die Integration verwendet wird.

In unseren iOS- und Android-Schnellstart-Tutorials werden Open-Source-Bibliotheken genutzt, die wir auf Kompatibilität mit Azure AD B2C getestet haben.  Alle Schnellstarttutorials stehen im Abschnitt [Erste Schritte](active-directory-b2c-overview.md#get-started) zur Verfügung.

## <a name="restriction-on-protocols"></a>Einschränkung für Protokolle
Azure AD B2C unterstützt OpenID Connect und OAuth 2.0. Allerdings wurden nicht alle Features und Funktionen der einzelnen Protokolle implementiert. Lesen Sie zum besseren Verständnis des Umfangs der unterstützten Protokollfunktionen in Azure AD B2C die [OpenID Connect- und OAuth 2.0-Protokollreferenz](active-directory-b2c-reference-protocols.md). Die Protokolle SAML (Security Assertion Markup Language) und WS-Fed (WS-Federation) werden nicht unterstützt.

## <a name="restriction-on-tokens"></a>Einschränkungen für Token
Viele der von Azure AD B2C ausgestellten Token werden als JSON-Webtoken (JWTs) implementiert. Aber nicht alle Informationen, die in JWTs enthalten sind (als „Ansprüche“ bezeichnet), sind richtig, oder sie fehlen sogar. Beispiele hierfür sind die Ansprüche „sub“ und „preferred_username“.  Da sich die Werte, das Format oder die Bedeutung von Ansprüchen im Laufe der Zeit ändern, bleiben Token für Ihre vorhandenen Richtlinien davon unberührt. Sie können sich auf die Werte in den Produktions-Apps verlassen.  Wenn sich Werte ändern, erhalten Sie die Möglichkeit, diese Änderungen für jede Richtlinie zu konfigurieren.  Lesen Sie sich die [Token-Referenz](active-directory-b2c-reference-tokens.md) durch, um ein besseres Verständnis für die Token zu entwickeln, die vom Azure AD B2C-Dienst derzeit ausgegeben werden.

## <a name="restriction-on-nested-groups"></a>Einschränkung für geschachtelte Gruppen
Geschachtelte Gruppenmitgliedschaften werden in Azure AD B2C-Mandanten nicht unterstützt. Wir planen nicht, diese Funktion hinzuzufügen.

## <a name="restriction-on-differential-query-feature-on-azure-ad-graph-api"></a>Einschränkung der Funktion „Differenzielle Abfrage“ in der Azure AD Graph-API
Die [Funktion „Differenzielle Abfrage“ in der Azure AD Graph-API](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-differential-query) wird in Azure AD B2C-Mandanten nicht unterstützt. Dies ist unsere langfristige Roadmap.

## <a name="issues-with-user-management-on-the-azure-classic-portal"></a>Probleme mit der Benutzerverwaltung im klassischen Azure-Portal
Sie können über das Azure-Portal auf B2C-Features zugreifen. Sie können jedoch das klassische Azure-Portal verwenden, um auf andere Mandantenfeatures zuzugreifen, z. B. auf die Benutzerverwaltung. Derzeit sind für das klassische Azure-Portal einige Probleme mit der Benutzerverwaltung (Registerkarte **Benutzer**) bekannt:

* Für einen Benutzer mit lokalem Konto (also ein Endkunde, der sich mit E-Mail-Adresse und Kennwort oder Benutzername und Kennwort registriert) entspricht das Feld **Benutzername** nicht dem Anmeldebezeichner (E-Mail-Adresse oder Benutzername), der während der Registrierung verwendet wurde. Dies liegt daran, dass das im klassischen Azure-Portal angezeigte Feld eigentlich der Benutzerprinzipalname ist, der in B2C-Szenarien nicht verwendet wird. Wenn Sie den Anmeldebezeichner des lokalen Kontos anzeigen möchten, suchen Sie das Benutzerobjekt im [Graph-Tester](https://graphexplorer.cloudapp.net/). Auf das gleiche Problem stoßen Sie bei einem Benutzer mit einem Konto bei einem sozialen Netzwerk (also ein Consumer, der sich mit Facebook, Google+ usw. anmeldet), aber in diesem Fall ist kein eigentlicher Anmeldebezeichner vorhanden.
  
    ![Lokales Konto – UPN](./media/active-directory-b2c-limitations/limitations-user-mgmt.png)
* Bei einem Benutzer mit lokalem Konto können Sie keine Felder bearbeiten und auf der Registerkarte **Profil** keine Änderungen speichern.

## <a name="issues-with-admin-initiated-password-reset-on-the-azure-classic-portal"></a>Probleme bei vom Administrator initiierten Kennwortzurücksetzungen im klassischen Azure-Portal
Wenn Sie das Kennwort für einen Endkunden mit einem lokalen Konto im klassischen Azure-Portal (mit dem Befehl **Kennwort zurücksetzen** auf der Registerkarte **Benutzer**) zurücksetzen, kann der Kunde bei der nächsten Anmeldung sein Kennwort nicht ändern, falls Sie eine Registrierungs- oder Anmelderichtlinie verwenden, und wird für Ihre Anwendungen gesperrt. Verwenden Sie zur Umgehung des Problems die [Azure AD Graph-API](active-directory-b2c-devquickstarts-graph-dotnet.md) , um das Kundenkennwort zurückzusetzen (ohne Kennwortablauf), oder verwenden Sie statt einer Registrierungs- oder Anmelderichtlinie eine Anmelderichtlinie.

## <a name="issues-with-creating-a-custom-attribute"></a>Probleme beim Erstellen eines benutzerdefinierten Attributs
Ein [über das Azure-Portal hinzugefügtes benutzerdefiniertes Attribut](active-directory-b2c-reference-custom-attr.md) wird nicht sofort in Ihrem B2C-Mandanten erstellt. Das benutzerdefinierte Attribut muss in mindestens einer Ihrer Richtlinien verwendet werden, damit es in Ihrem B2C-Mandanten erstellt und über die Graph-API verfügbar wird.

## <a name="issues-with-verifying-a-domain-on-the-azure-classic-portal"></a>Probleme beim Überprüfen einer Domäne im klassischen Azure-Portal
Derzeit können Domänen im [klassischen Azure-Portal](https://manage.windowsazure.com/)nicht erfolgreich überprüft werden.

## <a name="issues-with-sign-in-with-mfa-policy-on-safari-browsers"></a>Probleme beim Anmelden mit der MFA-Richtlinie in Safari-Browsern
Anforderungen an Anmelderichtlinien (bei aktiviertem MFA) führen in Safari-Browsern zeitweise zu HTTP 400-Fehlern (ungültige Anforderung). Dies liegt am niedrigen Größenlimit für Cookies in Safari. Für dieses Problem gibt es mehrere Umgehungen:

* Verwenden Sie die anstelle der Anmelderichtlinie die Richtlinie für Registrierung oder Anmeldung.
* Verringern Sie die Anzahl der in Ihrer Richtlinie angeforderten **Anwendungsansprüche** .




<!--HONumber=Dec16_HO4-->


