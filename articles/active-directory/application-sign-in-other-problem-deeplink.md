---
title: "Probleme beim Anmelden bei einer Anwendung über einen Deeplink | Microsoft-Dokumentation"
description: "Beheben von Problemen beim Zugreifen auf eine Anwendung über eine Deeplink-URL mit Azure AD"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.translationtype: Human Translation
ms.sourcegitcommit: 0d6f6fb24f1f01d703104f925dcd03ee1ff46062
ms.openlocfilehash: 7e7c19173229586cc788dbb5a6a01a9b075d33e9
ms.contentlocale: de-de
ms.lasthandoff: 04/17/2017

---

# <a name="problems-signing-in-to-an-application-using-a-deeplink"></a>Probleme beim Anmelden bei einer Anwendung über einen Deeplink

Der Zugriffsbereich ist ein webbasiertes Portal, in dem Benutzer mit einem Geschäfts-, Schul- oder Unikonto in Azure Active Directory (Azure AD) cloudbasierte Anwendungen anzeigen und starten können, für die der Azure AD-Administrator ihnen Zugriff gewährt hat. 

Diese Anwendungen werden im Namen des Benutzers im Azure AD-Portal konfiguriert. Die Anwendung muss ordnungsgemäß konfiguriert und dem Benutzer oder einer Gruppe, in der der Benutzer Mitglied ist, zugewiesen sein, damit sie im Zugriffsbereich angezeigt wird.

Deeplinks oder Benutzerzugriffs-URLs sind Links, mit deren Hilfe Ihre Benutzer direkt über die URL-Leiste ihrer Browser auf ihre Kennwort-SSO-Anwendungen zugreifen können. Bei der Navigation zu diesem Link werden die Benutzer automatisch bei der Anwendung angemeldet, ohne zuerst in den Zugriffsbereich wechseln zu müssen. Es handelt sich um denselben Link, mit dem Benutzer über den Office 365-Anwendungsstarter auf diese Anwendungen zugreifen.

## <a name="general-issues-to-check-first"></a>Allgemeine Probleme, die zuerst überprüft werden sollten

-   Stellen Sie sicher, dass Sie einen **Browser** verwenden, der die Mindestanforderungen für den Zugriffsbereich erfüllt.

-   Stellen Sie sicher, dass die URL der Anwendung den **vertrauenswürdigen Sites** im Browser des Benutzers hinzugefügt wurde.

-   Stellen Sie sicher, dass die Anwendung ordnungsgemäß **konfiguriert** ist.

-   Stellen Sie sicher, dass das Konto des Benutzers für Anmeldungen **aktiviert** ist.

-   Stellen Sie sicher, dass das Konto des Benutzers **nicht gesperrt ist**.

-   Stellen Sie sicher, dass **Kennwort des Benutzers nicht abgelaufen ist oder vergessen wurde**.

-   Stellen Sie sicher, dass Benutzerzugriff nicht durch **Multi-Factor Authentication** blockiert wird.

-   Stellen Sie sicher, dass der Benutzerzugriff nicht durch eine **Richtlinie für bedingten Zugriff** oder eine **Identity Protection**-Richtlinie blockiert wird.

-   Stellen Sie sicher, dass die **Kontaktinformationen für die Authentifizierung** des Benutzers aktuell sind, sodass Multi-Factor Authentication-Richtlinien oder Richtlinien für bedingten Zugriff erzwungen werden können.

-   Löschen Sie auch die Cookies in Ihrem Browser, und versuchen Sie sich erneut anzumelden.

## <a name="checking-the-deeplink"></a>Überprüfen des Deeplinks

Um festzustellen, ob Sie über den richtigen Deeplink verfügen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** oder **Co-Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie auf **Alle Anwendungen**, um eine Liste aller Anwendungen anzuzeigen.

  * Wenn die gewünschte Anwendung nicht angezeigt wird, verwenden Sie das Steuerelement **Filter** oberhalb der Liste **Alle Anwendungen**, und legen Sie die Option **Anzeigen** auf **Alle Anwendungen** fest.

6.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** oder **Co-Administrator** an.

7.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

8.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

9.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

10. Klicken Sie auf **Alle Anwendungen**, um eine Liste aller Anwendungen anzuzeigen.

   * Wenn die gewünschte Anwendung nicht angezeigt wird, verwenden Sie das Steuerelement **Filter** oberhalb der Liste **Alle Anwendungen**, und legen Sie die Option **Anzeigen** auf **Alle Anwendungen** fest.

11. Wählen Sie die Anwendung aus, für die Sie den Deeplink überprüfen möchten.

12. Suchen Sie die Bezeichnung **URL für den Benutzerzugriff**. Ihr Deeplink sollte mit dieser URL übereinstimmen.

## <a name="how-to-install-the-access-panel-browser-extension"></a>Installieren der Browsererweiterung für den Zugriffsbereich

Führen Sie zum Installieren der Browsererweiterung für den Zugriffsbereich die folgenden Schritte aus:

1.  Öffnen Sie in einem der unterstützten Browser den [Zugriffsbereich](https://myapps.microsoft.com), und melden Sie sich in Azure AD als **Benutzer** an.

2.  Klicken Sie im Zugriffsbereich auf eine **für kennwortbasiertes SSO konfigurierte Anwendung**.

3.  Wählen Sie in der Aufforderung zum Installieren der Software **Jetzt installieren** aus.

4.  Sie werden basierend auf Ihrem Browser zum Downloadlink weitergeleitet. **Fügen** Sie die Erweiterung Ihrem Browser hinzu.

5.  Wenn Sie im Browser zur Auswahl aufgefordert werden, wählen Sie die Option zum **Aktivieren** oder **Zulassen** der Erweiterung aus.

6.  Führen Sie nach der Installation einen **Neustart** der Browsersitzung durch.

7.  Melden Sie sich beim Zugriffsbereich an, und überprüfen Sie, ob Sie die für kennwortbasiertes SSO konfigurierten Anwendungen **starten** können.

Sie können die Erweiterung für Chrome und Firefox auch direkt über die folgenden Links herunterladen:

-   [Zugriffsbereichserweiterung für Chrome](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Zugriffsbereichserweiterung für Firefox](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Konfigurieren des einmaligen Anmeldens per Kennwort für eine Azure AD-Kataloganwendung

Um eine Anwendung aus dem Azure AD-Katalog zu konfigurieren, müssen folgende Schritte ausgeführt werden:

-   [Hinzufügen einer Anwendung aus dem Azure AD-Katalog](#add-an-application-from-the-Azure-AD-gallery)

-   [Konfigurieren der Anwendung für das einmalige Anmelden per Kennwort](#configure-the-application-for-password-single-sign-on)

### <a name="add-an-application-from-the-azure-ad-gallery"></a>Hinzufügen einer Anwendung aus dem Azure AD-Katalog

Um eine Anwendung aus dem Azure AD-Katalog hinzuzufügen, führen Sie folgende Schritte aus:

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als **Globaler Administrator** oder **Co-Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie oben rechts auf dem Blatt **Unternehmensanwendungen** auf die Schaltfläche **Hinzufügen**.

6.  Geben Sie im Abschnitt **Aus Katalog hinzufügen** im Textfeld **Namen eingeben** den Namen der Anwendung ein.

7.  Wählen Sie die Anwendung aus, die Sie für das einmalige Anmelden konfigurieren möchten.

8.  Bevor Sie die Anwendung hinzufügen, können Sie im Textfeld **Name** den Namen der Anwendung ändern.

9.  Klicken Sie auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

Nach kurzer Zeit wird das Konfigurationsblatt der Anwendung angezeigt.

### <a name="configure-the-application-for-password-single-sign-on"></a>Konfigurieren der Anwendung für das einmalige Anmelden per Kennwort

Um das einmalige Anmelden für eine Anwendung zu konfigurieren, führen Sie folgende Schritte aus:

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** oder **Co-Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie auf **Alle Anwendungen**, um eine Liste aller Anwendungen anzuzeigen.

  * Wenn die gewünschte Anwendung nicht angezeigt wird, verwenden Sie das Steuerelement **Filter** oberhalb der Liste **Alle Anwendungen**, und legen Sie die Option **Anzeigen** auf **Alle Anwendungen** fest.

6.  Wählen Sie die Anwendung aus, für die Sie das einmalige Anmelden konfigurieren möchten.

7.  Nachdem die Anwendung geladen wurde, klicken Sie im linken Navigationsmenü der Anwendung auf **Einmaliges Anmelden**.

8.  Wählen Sie den Modus **Kennwortbasiertes Anmelden** aus.

9.  [Weisen Sie der Anwendung Benutzer zu](#how-to-assign-a-user-to-an-application-directly).

10. Darüber hinaus können Sie Anmeldeinformationen für einen Benutzer angeben, indem Sie die Zeile des Benutzers auswählen, auf **Anmeldeinformationen aktualisieren** klicken und den Benutzernamen und das Kennwort für den Benutzer eingeben. Andernfalls werden die Benutzer beim Start der Anwendung aufgefordert, die Anmeldeinformationen selbst einzugeben.

## <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a>Konfigurieren des einmaligen Anmeldens per Kennwort für eine nicht im Katalog enthaltene Anwendung

Um eine Anwendung aus dem Azure AD-Katalog zu konfigurieren, müssen folgende Schritte ausgeführt werden:

-   [Hinzufügen einer nicht im Katalog enthaltenen Anwendung](#add-a-non-gallery-application)

-   [Konfigurieren der Anwendung für das einmalige Anmelden per Kennwort](#configure-the-application-for-password-single-sign-on)

### <a name="add-a-non-gallery-application"></a>Hinzufügen einer nicht im Katalog enthaltenen Anwendung

Um eine Anwendung aus dem Azure AD-Katalog hinzuzufügen, führen Sie folgende Schritte aus:

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als **Globaler Administrator** oder **Co-Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie oben rechts auf dem Blatt **Unternehmensanwendungen** auf die Schaltfläche **Hinzufügen**.

6.  Klicken Sie auf **Nicht-Kataloganwendung**.

7.  Geben Sie den Namen Ihrer Anwendung im Textfeld **Name** ein. Wählen Sie **Hinzufügen** aus.

Nach kurzer Zeit wird das Konfigurationsblatt der Anwendung angezeigt.

### <a name="configure-the-application-for-password-single-sign-on"></a>Konfigurieren der Anwendung für das einmalige Anmelden per Kennwort

Um das einmalige Anmelden für eine Anwendung zu konfigurieren, führen Sie folgende Schritte aus:

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** oder **Co-Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie auf **Alle Anwendungen**, um eine Liste aller Anwendungen anzuzeigen.

    1.  Wenn die gewünschte Anwendung nicht angezeigt wird, verwenden Sie das Steuerelement **Filter** oberhalb der Liste **Alle Anwendungen**, und legen Sie die Option **Anzeigen** auf **Alle Anwendungen** fest.

6.  Wählen Sie die Anwendung aus, für die Sie das einmalige Anmelden konfigurieren möchten.

7.  Nachdem die Anwendung geladen wurde, klicken Sie im linken Navigationsmenü der Anwendung auf **Einmaliges Anmelden**.

8.  Wählen Sie den Modus **Kennwortbasiertes Anmelden** aus.

9.  Geben Sie die **Anmelde-URL** ein. Dies ist die URL, unter der Benutzer ihren Benutzernamen und ihr Kennwort eingeben, um sich anzumelden. Stellen Sie sicher, dass die Anmeldefelder auf der Seite angezeigt werden, auf die über die URL zugegriffen wird.

10. Weisen Sie der Anwendung Benutzer zu.

11. Darüber hinaus können Sie Anmeldeinformationen für einen Benutzer angeben, indem Sie die Zeile des Benutzers auswählen, auf **Anmeldeinformationen aktualisieren** klicken und den Benutzernamen und das Kennwort für den Benutzer eingeben. Andernfalls werden die Benutzer beim Start der Anwendung aufgefordert, die Anmeldeinformationen selbst einzugeben.

## <a name="how-to-assign-a-user-to-an-application-directly"></a>Direktes Zuweisen eines Benutzers zu einer Anwendung

Um einer Anwendung Benutzer direkt zuzuweisen, führen Sie folgende Schritte aus:

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie auf **Alle Anwendungen**, um eine Liste aller Anwendungen anzuzeigen.

  * Wenn die gewünschte Anwendung nicht angezeigt wird, verwenden Sie das Steuerelement **Filter** oberhalb der Liste **Alle Anwendungen**, und legen Sie die Option **Anzeigen** auf **Alle Anwendungen** fest.

6.  Wählen Sie in der Liste die Anwendung aus, der Sie einen Benutzer zuweisen möchten.

7.  Nachdem die Anwendung geladen wurde, klicken Sie im linken Navigationsmenü der Anwendung auf **Benutzer und Gruppen**.

8.  Klicken Sie oberhalb der Liste **Benutzer und Gruppen** auf die Schaltfläche **Hinzufügen**, um das Blatt **Zuweisung hinzufügen** zu öffnen.

9.  Klicken Sie auf dem Blatt **Zuweisung hinzufügen** auf das Auswahlfeld **Benutzer und Gruppen**.

10. Geben Sie im Suchfeld **Nach Name oder E-Mail-Adresse suchen** den **vollständigen Namen** oder die **E-Mail-Adresse** des gewünschten Benutzers ein.

11. Zeigen Sie auf den **Benutzer** in der Liste, um ein **Kontrollkästchen** anzuzeigen. Klicken Sie auf das Kontrollkästchen neben dem Profilbild oder Logo des Benutzers, um den Benutzer der Liste **Ausgewählt** hinzuzufügen.

12. **Optional:** Wenn Sie **mehrere Benutzer hinzufügen** möchten, geben Sie im Suchfeld **Nach Name oder E-Mail-Adresse suchen** einen weiteren **vollständigen Namen** oder eine weitere **E-Mail-Adresse** ein, und klicken Sie auf das Kontrollkästchen, um diesen Benutzer der Liste **Ausgewählt** hinzuzufügen.

13. Wenn Sie alle gewünschten Benutzer ausgewählt haben, klicken Sie auf die Schaltfläche **Auswählen**, um sie der Liste der Benutzer und Gruppen hinzuzufügen, die der Anwendung zugewiesen werden sollen.

14. **Optional:** Klicken Sie auf dem Blatt **Zuweisung hinzufügen** auf das Auswahlfeld **Rolle auswählen**, um eine Rolle auszuwählen, die den ausgewählten Benutzern zugewiesen werden soll.

15. Klicken Sie auf die Schaltfläche **Zuweisen**, um die Anwendung den ausgewählten Benutzern zuzuweisen.

Nach kurzer Zeit können die ausgewählten Benutzer diese Anwendungen im Zugriffsbereich starten.

## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a>Wenn das Problem mit diesen Problembehandlungsschritten nicht behoben wird, 

öffnen Sie ein Supportticket mit den folgenden Informationen, sofern verfügbar:

-   Fehlerkorrelations-ID

-   UPN (E-Mail-Adresse des Benutzers)

-   Mandanten-ID

-   Browsertyp

-   Zeitzone und Uhrzeit/Zeitraum des Auftretens des Fehlers

-   Fiddler-Ablaufverfolgungen

## <a name="next-steps"></a>Nächste Schritte
[Bereitstellen von einmaligem Anmelden bei Ihren Apps mit dem Anwendungsproxy](active-directory-application-proxy-sso-using-kcd.md)

