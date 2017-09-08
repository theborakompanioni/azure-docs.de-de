---
title: "Verwalten von SSO für den Azure AD-Anwendungsproxy | Microsoft-Dokumentation"
description: "Enthält eine Beschreibung der Grundlagen von SSO (einmaliges Anmelden) mit dem Anwendungsproxy."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: 646886ad82d47162a62835e343fcaa7dadfaa311
ms.openlocfilehash: 1deb3d91049d45fe26791783e13bd23e0a7d9f95
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---

# <a name="how-does-azure-ad-application-proxy-provide-single-sign-on"></a>Wie stellt der Azure AD-Anwendungsproxy das einmalige Anmelden bereit?

Die einmalige Anmeldung ist ein wichtiges Element des Azure AD-Anwendungsproxys.  Sie ermöglicht die höchste Benutzerfreundlichkeit, weil sich Ihre Benutzer nur an Azure Active Directory in der Cloud anmelden müssen. Nach der Authentifizierung bei Azure Active Directory übernimmt der Anwendungsproxy die Authentifizierung bei der lokalen Anwendung. Die Back-End-Anwendung kann keinen Unterschied zwischen einem Remotebenutzer, der sich über den Anwendungsproxy anmeldet, und einem regulären Benutzer auf einem in die Domäne eingebundenen Gerät feststellen. 

Zum Verwenden von Azure Active Directory für das einmalige Anmelden an Ihren Anwendungen müssen Sie **Azure Active Directory** als Methode für die Präauthentifizierung wählen. Wenn Sie **Passthrough** wählen, führen Ihre Benutzer keine Authentifizierung bei Azure Active Directory durch, sondern werden direkt an die Anwendung geleitet. Sie können diese Einstellung konfigurieren, wenn Sie eine Anwendung zum ersten Mal veröffentlichen, oder im Azure-Portal zu Ihrer Anwendung navigieren und die Anwendungsproxyeinstellungen bearbeiten. 

Führen Sie diese Schritte aus, um die Optionen für das einmalige Anmelden anzuzeigen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Navigieren Sie zu **Azure Active Directory** > **Unternehmensanwendungen** > **Alle Anwendungen**.
3. Wählen Sie die App aus, für die Sie die Optionen für das einmalige Anmelden verwalten möchten.
4. Wählen Sie **Einmaliges Anmelden**.

   ![SSO-Dropdownmenü](./media/application-proxy-sso-overview/single-sign-on-mode.png)

Im Dropdownmenü werden für Ihre Anwendung fünf Optionen für das einmalige Anmelden angezeigt:

* Azure AD-SSO deaktiviert
* Kennwortbasierte Anmeldung
* Anmeldung über Link
* Integrierte Windows-Authentifizierung
* Headerbasierte Anmeldung

## <a name="azure-ad-single-sign-on-disabled"></a>Azure AD-SSO deaktiviert

Wählen Sie **Azure AD-SSO deaktiviert**, wenn Sie nicht die Azure Active Directory-Integration für das einmalige Anmelden an Ihrer Anwendung verwenden möchten. Wenn diese Option ausgewählt ist, müssen sich Ihre Benutzer unter Umständen zweimal authentifizieren. Zuerst authentifizieren sie sich gegenüber Azure Active Directory und melden sich dann an der eigentlichen Anwendung an. 

Diese Option ist eine gute Wahl, wenn sich die Benutzer gegenüber Ihrer lokalen Anwendung nicht authentifizieren müssen und Sie Azure Active Directory als Sicherheitsebene für den Remotezugriff hinzufügen möchten. 

## <a name="password-based-sign-on"></a>Kennwortbasierte Anmeldung

Wählen Sie **Kennwortbasierte Anmeldung**, wenn Sie Azure Active Directory als Kennworttresor für Ihre lokalen Anwendungen verwenden möchten. Diese Option ist eine gute Wahl, wenn für Ihre Anwendung zur Authentifizierung keine Zugriffstoken oder Header verwendet werden, sondern eine Kombination aus Benutzername und Kennwort. Bei der kennwortbasierten Anmeldung müssen sich Benutzer an der Anwendung anmelden, wenn sie zum ersten Mal darauf zugreifen. Danach werden der Benutzername und das Kennwort von Azure Active Directory für den Benutzer bereitgestellt. 

Informationen zum Einrichten der kennwortbasierten Anmeldung finden Sie unter [Ermöglichen des einmaligen Anmeldens mit dem Azure AD-Anwendungsproxy – Public Preview](application-proxy-sso-azure-portal.md).

## <a name="linked-sign-on"></a>Anmeldung über Link

Wählen Sie **Anmeldung über Link**, wenn Sie bereits eine SSO-Lösung für Ihre lokalen Identitäten eingerichtet haben. Diese Option ermöglicht Azure Active Directory die Nutzung von vorhandenen SSO-Lösungen, aber Ihre Benutzer haben weiterhin Remotezugriff auf die Anwendung. 

Informationen zur Anmeldung über einen Link (wurde früher als „einmaliges Anmelden“ bezeichnet) finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

## <a name="integrated-windows-authentication"></a>Integrierte Windows-Authentifizierung

Wählen Sie **Integrierte Windows-Authentifizierung**, wenn für Ihre lokalen Anwendungen die integrierte Windows-Authentication (IWA) verwendet wird oder wenn Sie die Eingeschränkte Kerberos-Delegierung (Kerberos Constrained Delegation, KCD) für das einmalige Anmelden nutzen möchten. Mit dieser Option müssen sich Ihre Benutzer nur gegenüber Azure Active Directory authentifizieren. Der Anwendungsproxyconnector nimmt dann die Identität des Benutzers an, um ein Kerberos-Token zu erhalten und die Anmeldung an der Anwendung durchzuführen. 

Informationen zum Einrichten der integrierten Windows-Authentifizierung finden Sie unter [Bereitstellen von einmaligem Anmelden bei Ihren Apps mit dem Anwendungsproxy](active-directory-application-proxy-sso-using-kcd.md).

## <a name="header-based-sign-on"></a>Headerbasierte Anmeldung 

Wählen Sie **Headerbasierte Anmeldung**, wenn in Ihren Anwendungen Header für die Authentifizierung verwendet werden. Mit dieser Option müssen sich Ihre Benutzer nur gegenüber Azure Active Directory authentifizieren. Microsoft verfügt über eine Partnerschaft mit einem Drittanbieter-Authentifizierungsdienst mit dem Namen PingAccess, bei dem das Azure Active Directory-Zugriffstoken in ein Headerformat für die Anwendung übersetzt wird. 

Informationen zum Einrichten der headerbasierten Authentifizierung finden Sie unter [Veröffentlichen von Anwendungen, die die headerbasierte Authentifizierung mit Azure AD-Anwendungsproxy und PingAccess unterstützen](application-proxy-ping-access.md).

## <a name="next-steps"></a>Nächste Schritte

- [Ermöglichen des einmaligen Anmeldens mit dem Azure AD-Anwendungsproxy – Public Preview](application-proxy-sso-azure-portal.md)
- [Bereitstellen von einmaligem Anmelden bei Ihren Apps mit dem Anwendungsproxy](active-directory-application-proxy-sso-using-kcd.md)
- [Veröffentlichen von Anwendungen, die die headerbasierte Authentifizierung mit Azure AD-Anwendungsproxy und PingAccess unterstützen](application-proxy-ping-access.md) 

