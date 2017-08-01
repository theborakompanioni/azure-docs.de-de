---
title: "Probleme beim Anmelden bei einer Azure AD-Kataloganwendung, die für einmaliges Anmelden per Kennwort konfiguriert ist | Microsoft-Dokumentation"
description: "Erläuterungen von Problembereichen zur Hilfestellung beim Behandeln von Problemen beim Anmelden bei Azure AD-Kataloganwendungen, die für einmaliges Anmelden per Kennwort konfiguriert sind"
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
ms.openlocfilehash: 4f8ebf5e734cc2e79819593c942f0e55227e522c
ms.contentlocale: de-de
ms.lasthandoff: 04/17/2017

---

# <a name="problems-signing-in-to-an-azure-ad-gallery-application-configured-for-password-single-sign-on"></a>Probleme beim Anmelden bei einer Azure AD-Kataloganwendung, die für einmaliges Anmelden per Kennwort konfiguriert ist

Der Zugriffsbereich ist ein webbasiertes Portal, mit dem Benutzer, die in Azure Active Directory (Azure AD) über ein Geschäfts-, Schul- oder Unikonto verfügen, cloudbasierte Anwendungen anzeigen und starten können, für die der Azure AD-Administrator ihnen Zugriff gewährt hat. Ein Benutzer, der über Azure AD-Editionen verfügt, kann über den Zugriffsbereich auch die Funktionen für die Self-Service-Gruppen- und App-Verwaltung nutzen. Der Zugriffsbereich ist unabhängig vom Azure-Portal und setzt nicht voraus, dass Benutzer über ein Azure-Abonnement verfügen.

Für die Verwendung des einmaligen Anmeldens per Kennwort im Anwendungsbereich muss die Erweiterung für den Zugriffsbereich im Browser des Benutzers installiert sein. Diese Erweiterung wird automatisch heruntergeladen, wenn ein Benutzer eine Anwendung auswählt, die für kennwortbasiertes SSO konfiguriert ist.

## <a name="meeting-browser-requirements-for-the-access-panel"></a>Erfüllen der Browseranforderungen für den Zugriffsbereich

Der Zugriffsbereich erfordert einen Browser, der JavaScript unterstützt und in dem CSS aktiviert ist. Für die Verwendung des einmaligen Anmeldens per Kennwort im Anwendungsbereich muss die Erweiterung für den Zugriffsbereich im Browser des Benutzers installiert sein. Diese Erweiterung wird automatisch heruntergeladen, wenn ein Benutzer eine Anwendung auswählt, die für kennwortbasiertes SSO konfiguriert ist.

Für kennwortbasiertes SSO eignen sich folgende Browser des Endbenutzers:

-   Internet Explorer 8, 9, 10, 11 – unter Windows 7 oder höher

-   Chrome – unter Windows 7 oder höher und MacOS x oder höher

-   Firefox 26.0 oder höher – unter Windows XP SP2 oder höher und Mac OS X 10.6 oder höher

>[!NOTE]
>Die Erweiterung für die kennwortbasierte einmalige Anmeldung steht für Edge unter Windows 10 zur Verfügung, sobald Browsererweiterungen von Edge unterstützt werden.
>
>

## <a name="how-to-install-the-access-panel-browser-extension"></a>Installieren der Browsererweiterung für den Zugriffsbereich

Führen Sie zum Installieren der Browsererweiterung für den Zugriffsbereich die folgenden Schritte aus:

1.  Öffnen Sie in einem der unterstützten Browser den [Zugriffsbereich](https://myapps.microsoft.com), und melden Sie sich in Azure AD als **Benutzer** an.

2.  Klicken Sie im Zugriffsbereich auf eine **für einmaliges Anmelden per Kennwort konfigurierte Anwendung**.

3.  Wählen Sie in der Aufforderung zum Installieren der Software **Jetzt installieren** aus.

4.  Je nach Browser werden Sie zum Downloadlink geleitet. **Fügen** Sie dem Browser die Erweiterung hinzu.

5.  Wenn Sie im Browser zur Auswahl aufgefordert werden, wählen Sie die Option zum **Aktivieren** oder **Zulassen** der Erweiterung aus.

6.  Führen Sie nach der Installation einen **Neustart** Ihrer Browsersitzung durch.

7.  Melden Sie sich beim Zugriffsbereich an, und prüfen Sie, ob Sie Ihre Anwendungen mit einmaligem Anmelden per Kennwort **starten** können.

Sie können die Erweiterung für Chrome und Firefox auch direkt über die folgenden Links herunterladen:

-   [Zugriffsbereichserweiterung für Chrome](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Zugriffsbereichserweiterung für Firefox](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="setting-up-a-group-policy-for-internet-explorer"></a>Einrichten einer Gruppenrichtlinie für Internet Explorer

Sie können eine Gruppenrichtlinie einrichten, um die Zugriffsbereichserweiterung für Internet Explorer per Remotezugriff auf den Computern Ihrer Benutzer zu installieren.

Die folgenden Voraussetzungen müssen erfüllt sein:

-   Sie haben [Active Directory-Domänendienste](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx)eingerichtet und die Computer Ihrer Benutzer Ihrer Domäne hinzugefügt.

-   Zum Bearbeiten des Gruppenrichtlinienobjekts benötigen Sie die Berechtigung „Einstellungen bearbeiten“. Standardmäßig verfügen Mitglieder der folgenden Sicherheitsgruppen über diese Berechtigung: Domänenadministratoren, Organisationsadministratoren und Richtlinien-Ersteller-Besitzer. [Weitere Informationen](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).

Das Tutorial [Bereitstellen der Zugriffsbereichserweiterung für Internet Explorer mit der Gruppenrichtlinie](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-group-policy) bietet Ihnen Schritt-für-Schritt-Anweisungen zum Konfigurieren der Gruppenrichtlinie und Bereitstellen der Gruppenrichtlinie für Benutzer.

## <a name="troubleshoot-the-access-panel-in-internet-explorer"></a>Problembehandlung für den Zugriffsbereich in Internet Explorer

Befolgen Sie die Anleitung [Problembehandlung in der Zugriffsbereichserweiterung für Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-Troubleshoot), um Zugriff auf ein Diagnosetool und Schritt-für-Schritt-Anweisungen zum Konfigurieren der Erweiterung für Internet Explorer zu erhalten.

## <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a>Konfigurieren des einmaligen Anmeldens per Kennwort für eine nicht im Katalog enthaltene Anwendung

Um eine Anwendung aus dem Azure AD-Katalog zu konfigurieren, müssen folgende Schritte ausgeführt werden:

-   [Hinzufügen einer nicht im Katalog enthaltenen Anwendung](#add-a-non-gallery-application)

-   [Konfigurieren der Anwendung für das einmalige Anmelden per Kennwort](#configure-the-application-for-password-single-sign-on)

-   [Zuweisen von Benutzern zur Anwendung](#assign-users-to-the-application)

### <a name="add-a-non-gallery-application"></a>Hinzufügen einer nicht im Katalog enthaltenen Anwendung

Um eine Anwendung aus dem Azure AD-Katalog hinzuzufügen, führen Sie folgende Schritte aus:

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als **Globaler Administrator** oder **Co-Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie rechts oben auf dem Blatt **Unternehmensanwendungen** auf die Schaltfläche **Hinzufügen**.

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

   * Wenn die gewünschte Anwendung nicht angezeigt wird, verwenden Sie das Steuerelement **Filter** oberhalb der Liste **Alle Anwendungen**, und legen Sie die Option **Anzeigen** auf **Alle Anwendungen** fest.

6.  Wählen Sie die Anwendung aus, für die Sie das einmalige Anmelden konfigurieren möchten.

7.  Nachdem die Anwendung geladen wurde, klicken Sie im linken Navigationsmenü der Anwendung auf **Einmaliges Anmelden**.

8.  Wählen Sie den Modus **Kennwortbasiertes Anmelden** aus.

9.  Geben Sie die **Anmelde-URL** ein. Dies ist die URL, unter der Benutzer ihren Benutzernamen und ihr Kennwort eingeben, um sich anzumelden. Stellen Sie sicher, dass die Anmeldefelder auf der Seite angezeigt werden, auf die über die URL zugegriffen wird.

10. Weisen Sie der Anwendung Benutzer zu.

11. Darüber hinaus können Sie Anmeldeinformationen für einen Benutzer angeben, indem Sie die Zeile des Benutzers auswählen, auf **Anmeldeinformationen aktualisieren** klicken und den Benutzernamen und das Kennwort für den Benutzer eingeben. Andernfalls werden Benutzer beim Start der Anwendung aufgefordert, die Anmeldeinformationen selbst einzugeben.

### <a name="assign-users-to-the-application"></a>Zuweisen von Benutzern zur Anwendung

Um einer Anwendung einen oder mehrere Benutzer direkt zuzuweisen, führen Sie folgende Schritte aus:

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

## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a>Wenn das Problem mit diesen Problembehandlungsschritten nicht behoben wird

Öffnen Sie ein Supportticket mit den folgenden Informationen, sofern verfügbar:

-   Fehlerkorrelations-ID

-   UPN (E-Mail-Adresse des Benutzers)

-   Mandanten-ID

-   Browsertyp

-   Zeitzone und Uhrzeit/Zeitraum des Auftretens des Fehlers

-   Fiddler-Ablaufverfolgungen

## <a name="next-steps"></a>Nächste Schritte
[Bereitstellen von einmaligem Anmelden bei Ihren Apps mit dem Anwendungsproxy](active-directory-application-proxy-sso-using-kcd.md)


