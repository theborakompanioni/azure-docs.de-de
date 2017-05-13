---
title: Einmaliges Anmelden bei Apps mit dem Azure AD-Anwendungsproxy | Microsoft-Dokumentation
description: "Aktivieren Sie das einmalige Anmelden für Ihre veröffentlichten lokalen Anwendungen mit dem Azure AD-Anwendungsproxy im Azure-Portal."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: asteen
ms.assetid: d94ac3f4-cd33-4c51-9d19-544a528637d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: kgremban
ms.translationtype: Human Translation
ms.sourcegitcommit: 54b5b8d0040dc30651a98b3f0d02f5374bf2f873
ms.openlocfilehash: b0454ad51a2d0edecb6f7e7748ec3b74eeefa1da
ms.contentlocale: de-de
ms.lasthandoff: 04/28/2017

---


# <a name="provide-single-sign-on-with-azure-ad-application-proxy---public-preview"></a>Ermöglichen des einmaligen Anmeldens mit dem Azure AD-Anwendungsproxy – Public Preview

Mit dem Azure Active Directory-Anwendungsproxy können Sie die Produktivität steigern, indem Sie lokale Anwendungen so veröffentlichen, dass auch Remotemitarbeiter sicher darauf zugreifen können. Im Azure-Portal können Sie auch einmaliges Anmelden (Single Sign-On, SSO) für diese Apps einrichten. Jetzt müssen sich Ihre Benutzer nur bei Azure AD authentifizieren. Dann können sie auf die Unternehmensanwendung zugreifen, ohne sich erneut anmelden zu müssen.

In diesem Artikel nutzen wir das Beispiel einer kennwortbasierten App, um zu zeigen, wie mit Kennworttresoren benutzerfreundliche SSO-Funktionen ermöglicht werden. 

Sie sollten Ihre App bereits mit dem Anwendungsproxy veröffentlicht und getestet haben. Führen Sie andernfalls die Schritte unter [Veröffentlichen von Anwendungen mit dem Azure AD-Anwendungsproxy – Public Preview](application-proxy-publish-azure-portal.md) aus, und fahren Sie dann hier fort. 

Wenn Sie mit dem Anwendungsproxy noch nicht vertraut sind, erfahren Sie im Artikel [Bereitstellen von sicherem Remotezugriff auf lokale Anwendungen](active-directory-application-proxy-get-started.md) mehr über diese Funktion.

## <a name="set-up-password-vaulting-for-your-application"></a>Einrichten von Kennworttresoren für Ihre Anwendung

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Administrator an.
2. Wählen Sie **Azure Active Directory** > **Unternehmensanwendungen** > **Alle Anwendungen** aus.
3. Wählen Sie in der Liste die App aus, für die SSO einrichten möchten. Wenn Sie über viele Apps verfügen, können Sie das Suchfeld verwenden, um die richtige zu finden.  
4. Wählen Sie im Abschnitt „Verwalten“ die Option **Einmaliges Anmelden** aus.

   ![„Einmaliges Anmelden“ auswählen](./media/application-proxy-sso-azure-portal/select-sso.png)

5. Wählen Sie als SSO-Modus **Kennwortbasierte Anmeldung** aus.
6. Geben Sie als Anmelde-URL die URL der Seite ein, auf der die Benutzer ihren Benutzernamen und ihr Kennwort für die Anmeldung bei Ihrer App eingeben. Dies sollte die externe URL sein, die Sie erstellt haben, als Sie die App über den Anwendungsproxy veröffentlicht haben. 

   ![Kennwortbasierte Anmeldung auswählen und URL eingeben](./media/application-proxy-sso-azure-portal/password-sso.png)

7. Wählen Sie **Speichern**aus.

<!-- Need to repro?
7. The page should tell you that a sign-in form was successfully detected at the provided URL. If it doesn't, select **Configure [your app name] Password Single Sign-on Settings** and choose **Manually detect sign-in fields**. Follow the instructions to point out where the sign-in credentials go. 
-->

## <a name="test-your-app"></a>Testen Ihrer App

Wechseln Sie zur [Website mit Ihren Apps](https://myapps.microsoft.com), und wählen Sie die App aus, die Sie gerade konfiguriert haben. Melden Sie sich mit Ihren Anmeldeinformationen für die App an (oder mit den Anmeldeinformationen für das Testkonto, für das Sie Zugriff eingerichtet haben). Nachdem Sie sich erfolgreich angemeldet haben, sollten Sie die App verlassen und wieder aufrufen können, ohne Ihre Anmeldeinformationen erneut eingeben zu müssen. 

## <a name="next-steps"></a>Nächste Schritte

Lernen Sie andere Möglichkeiten zum Implementieren des [einmaligen Anmeldens mit dem Anwendungsproxy](active-directory-application-proxy-sso-using-kcd.md) kennen.

