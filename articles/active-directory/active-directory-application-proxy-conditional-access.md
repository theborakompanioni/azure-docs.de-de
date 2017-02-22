---
title: "Bedingter Zugriff auf lokale Apps – Azure AD | Microsoft-Dokumentation"
description: "Stellt Informationen dazu bereit, wie der bedingte Zugriff für veröffentlichte Anwendungen eingerichtet wird, auf die mit dem Azure AD-Anwendungsproxy remote zugegriffen wird."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: 2e97722b-eb4e-4078-b607-9fed210d8a0f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 164e3b0af47cd98f2d20fce7344230608e9fbe8c
ms.openlocfilehash: 72b823998ebd570acc34e6a7ca034f64881db0c4


---
# <a name="working-with-conditional-access-in-azure-ad-application-proxy"></a>Arbeiten mit bedingtem Zugriff im Azure AD-Anwendungsproxy
Sie können Zugriffsregeln konfigurieren, um den bedingten Zugriff auf Anwendungen zu gewähren, die mit dem Anwendungsproxy veröffentlicht werden. Hierdurch wird Folgendes ermöglicht:

* Erfordern einer Multi-Factor Authentication pro Anwendung
* Erfordern einer Multi-Factor Authentication nur dann, wenn die Benutzer nicht am Arbeitsplatz sind
* Blockieren des Zugriffs der Benutzer auf Anwendungen, wenn die Benutzer nicht am Arbeitsplatz sind

Diese Regeln können auf alle Benutzer und Gruppen oder lediglich auf bestimmte Benutzer und Gruppen angewendet werden. Standardmäßig gilt die Regel für alle Benutzer, die Zugriff auf die Anwendung haben. Die Regel kann jedoch auch auf Benutzer beschränkt werden, die Mitglied der angegebenen Sicherheitsgruppen sind.  

Zugriffsregeln werden ausgewertet, wenn ein Benutzer auf eine Verbundanwendung zugreift, die OAuth 2.0, OpenID Connect, SAML oder WS-Federation verwendet. Darüber hinaus werden Zugriffsregeln mit OAuth 2.0 und OpenID Connect ausgewertet, wenn ein Aktualisierungstoken zum Abrufen eines Zugriffstokens verwendet wird.

## <a name="conditional-access-prerequisites"></a>Voraussetzungen für den bedingten Zugriff
* Azure Active Directory Premium-Abonnement
* Ein Verbundmandant oder verwalteter Azure Active Directory-Mandant
* Verbundmandanten erfordern die Aktivierung von Multi-Factor Authentication (MFA)   
    ![Konfigurieren von Zugriffsregeln – Erfordern von Multi-Factor Authentication](./media/active-directory-application-proxy-conditional-access/application-proxy-conditional-access.png)

## <a name="configure-per-application-multi-factor-authentication"></a>Konfigurieren von Multi-Factor Authentication pro Anwendung
1. Melden Sie sich als Administrator beim klassischen Azure-Portal an.
2. Gehen Sie zu Active Directory, und wählen Sie das Verzeichnis aus, in dem Sie den Anwendungsproxy aktivieren möchten.
3. Klicken Sie auf **Anwendungen**, und scrollen Sie nach unten zum Abschnitt **Zugriffsregeln**. Der Abschnitt mit den Zugriffsregeln wird nur für Anwendungen angezeigt, die mit dem Anwendungsproxy veröffentlicht wurden und die Verbundauthentifizierung nutzen.
4. Aktivieren Sie die Regeln, indem Sie **Zugriffsregeln aktivieren** auf **Ein** festlegen.
5. Geben Sie die Benutzer und Gruppen an, auf die die Regeln angewendet werden sollen. Verwenden Sie die Schaltfläche **Gruppe hinzufügen**, um eine oder mehrere Gruppen auszuwählen, für die die Zugriffsregel gelten soll. Dieses Dialogfeld kann auch zum Entfernen ausgewählter Gruppen verwendet werden.  Bei Anwendung der ausgewählten Regeln auf Gruppen werden die Zugriffsregeln nur für Benutzer erzwungen, die einer der angegebenen Sicherheitsgruppen angehören.  

   * Um Sicherheitsgruppen ausdrücklich von der Regel auszuschließen, aktivieren Sie die Option **Ausgenommen** und geben eine oder mehrere Gruppen an. Benutzer, die Mitglieder einer der in der Ausnahmeliste aufgeführten Gruppen sind, müssen keine mehrstufige Authentifizierung durchführen.  
   * Wenn ein Benutzer unter Verwendung der Multi-Factor Authentication-Funktion auf Benutzerebene konfiguriert wurde, hat diese Einstellung Vorrang vor den Multi-Factor Authentication-Regeln auf Anwendungsebene. Dies bedeutet, dass Benutzer, die auf Benutzerebene für die mehrstufige Authentifizierung konfiguriert wurden, auch dann eine mehrstufige Authentifizierung durchlaufen müssen, wenn sie auf Anwendungsebene von den Multi-Factor Authentication-Regeln ausgenommen wurden. Hier erfahren Sie mehr über [Multi-Factor Authentication und benutzerspezifische Einstellungen](../multi-factor-authentication/multi-factor-authentication.md).
6. Wählen Sie die Zugriffsregel, die Sie festlegen möchten:

   * **Erfordert mehrstufige Authentifizierung**: Benutzer, auf die Zugriffsregeln angewendet werden, müssen eine mehrstufige Authentifizierung durchlaufen, bevor sie auf die Anwendung zugreifen können, für die die Regel gilt.
   * **Erfordert mehrstufige Authentifizierung, wenn nicht bei der Arbeit**: Benutzer, die über eine vertrauenswürdige IP-Adresse auf die Anwendung zugreifen, werden nicht zur Ausführung einer mehrstufigen Authentifizierung aufgefordert. Die Bereiche für vertrauenswürdige IP-Adressen können auf den Einstellungsseiten für die mehrstufige Authentifizierung konfiguriert werden.
   * **Zugriff blockieren, wenn nicht gearbeitet wird**: Benutzer, die versuchen, sich von außerhalb des Unternehmensnetzwerks zu verbinden, können nicht auf die Anwendung zugreifen.

## <a name="configuring-mfa-for-federation-services"></a>Konfigurieren von MFA für Verbunddienste
Für Verbundmandanten kann Multi-Factor Authentication (MFA) von Azure Active Directory oder vom lokalen AD FS-Server durchgeführt werden. Standardmäßig wird MFA auf allen Seiten durchgeführt, die von Azure Active Directory gehostet werden. Führen Sie zur lokalen Konfiguration von MFA Windows PowerShell aus, und verwenden Sie die –SupportsMFA-Eigenschaft zum Festlegen des Azure AD-Moduls.

Im folgenden Beispiel wird veranschaulicht, wie lokale MFA mithilfe des [Set-MsolDomainFederationSettings-Cmdlets](https://msdn.microsoft.com/library/azure/dn194088.aspx) auf dem Mandanten „contoso.com“ aktiviert wird: `Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true `

Zusätzlich zum Festlegen dieses Kennzeichens muss die AD FS-Instanz des Verbundmandanten für die Ausführung der mehrstufigen Authentifizierung konfiguriert werden. Folgen Sie hierzu den Anweisungen unter [Lokales Bereitstellen von Microsoft Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## <a name="see-also"></a>Weitere Informationen
* [Arbeiten mit Anwendungen, die Ansprüche unterstützen](active-directory-application-proxy-claims-aware-apps.md)
* [Veröffentlichen von Anwendungen mit dem Anwendungsproxy](active-directory-application-proxy-publish.md)
* [Aktivieren der einmaligen Anmeldung](active-directory-application-proxy-sso-using-kcd.md)
* [Veröffentlichen von Anwendungen mit Ihrem eigenen Domänennamen](active-directory-application-proxy-custom-domains.md)

Aktuelle Neuigkeiten und Updates finden Sie im [Blog zum Anwendungsproxy](http://blogs.technet.com/b/applicationproxyblog/)



<!--HONumber=Jan17_HO4-->


