---
title: "Probleme beim Anmelden bei einer Anwendung über den Zugriffsbereich | Microsoft-Dokumentation"
description: Beheben von Problemen beim Zugreifen auf eine Anwendung aus dem Microsoft Azure AD-Zugriffsbereich unter myapps.microsoft.com
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
ms.openlocfilehash: b89569b818b0e2c79934a842fdc428011674c000
ms.contentlocale: de-de
ms.lasthandoff: 04/17/2017

---

<a id="problems-signing-in-to-an-application-from-the-access-panel" class="xliff"></a>

# Probleme beim Anmelden bei einer Anwendung über den Zugriffsbereich

Der Zugriffsbereich ist ein webbasiertes Portal, in dem Benutzer mit einem Geschäfts-, Schul- oder Unikonto in Azure Active Directory (Azure AD) cloudbasierte Anwendungen anzeigen und starten können, für die der Azure AD-Administrator ihnen Zugriff gewährt hat. 

Diese Anwendungen werden im Namen des Benutzers im Azure AD-Portal konfiguriert. Die Anwendung muss ordnungsgemäß konfiguriert und dem Benutzer oder einer Gruppe, in der der Benutzer Mitglied ist, zugewiesen sein, damit sie im Zugriffsbereich angezeigt wird.

Es gibt drei Kategorien von Apps, die für einen Benutzer angezeigt werden können:

-   Office 365-Anwendungen

-   Anwendungen von Microsoft und Drittanbietern, für die verbundbasiertes SSO konfiguriert ist

-   Anwendungen mit kennwortbasiertem SSO

-   Anwendungen mit vorhandenen SSO-Lösungen

<a id="general-issues-to-check-first" class="xliff"></a>

## Allgemeine Probleme, die zuerst überprüft werden sollten

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

<a id="meeting-browser-requirements-for-the-access-panel" class="xliff"></a>

## Erfüllen der Browseranforderungen für den Zugriffsbereich

Der Zugriffsbereich erfordert einen Browser, der JavaScript unterstützt und in dem CSS aktiviert ist. Für die Verwendung des einmaligen Anmeldens per Kennwort im Anwendungsbereich muss die Erweiterung für den Zugriffsbereich im Browser des Benutzers installiert sein. Diese Erweiterung wird automatisch heruntergeladen, wenn ein Benutzer eine Anwendung auswählt, die für kennwortbasiertes SSO konfiguriert ist.

Für kennwortbasiertes SSO eignen sich folgende Browser des Endbenutzers:

-   Internet Explorer 8, 9, 10, 11 – unter Windows 7 oder höher

-   Chrome – unter Windows 7 oder höher und MacOS x oder höher

-   Firefox 26.0 oder höher – unter Windows XP SP2 oder höher und Mac OS X 10.6 oder höher

**Hinweis**: Die Erweiterung für kennwortbasiertes SSO steht für Edge unter Windows 10 zur Verfügung, sobald Browsererweiterungen von Edge unterstützt werden.

<a id="how-to-install-the-access-panel-browser-extension" class="xliff"></a>

## Installieren der Browsererweiterung für den Zugriffsbereich

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

<a id="how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application" class="xliff"></a>

## Konfigurieren von einmaligem Anmelden im Verbund für eine Azure AD-Kataloganwendung

Für alle Anwendungen im Azure AD-Katalog mit Enterprise Single Sign-On ist ein Schritt-für-Schritt-Tutorial verfügbar. In der [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) finden Sie ausführliche schrittweise Anweisungen.

Um eine Anwendung aus dem Azure AD-Katalog zu konfigurieren, müssen folgende Schritte ausgeführt werden:

-   [Hinzufügen einer Anwendung aus dem Azure AD-Katalog](#add-an-application)

-   [Konfigurieren der Metadatenwerte für die Anwendung in Azure AD (Anmelde-URL, ID, Antwort-URL)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Auswählen der Benutzer-ID und Hinzufügen von Benutzerattributen, die an die Anwendung gesendet werden](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Abrufen der Azure AD-Metadaten und des Zertifikats](#download-the-azure-ad-metadata-or-certificate)

-   [Konfigurieren von Azure AD-Metadatenwerten in der Anwendung (Anmelde-URL, Aussteller, Abmelde-URL und Zertifikat)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Zuweisen von Benutzern zur Anwendung](#assign-users-to-the-application)

<a id="add-an-application-from-the-azure-ad-gallery" class="xliff"></a>

### Hinzufügen einer Anwendung aus dem Azure AD-Katalog

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

<a id="configure-single-sign-on-for-an-application-from-the-azure-ad-gallery" class="xliff"></a>

### Konfigurieren von einmaligem Anmelden für eine Anwendung aus dem Azure AD-Katalog

Um das einmalige Anmelden für eine Anwendung zu konfigurieren, führen Sie folgende Schritte aus:

1.  <span id="_Hlk477187909" class="anchor"><span id="_Hlk477001983" class="anchor"></span></span>Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** oder **Co-Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie auf **Alle Anwendungen**, um eine Liste aller Anwendungen anzuzeigen.

  * Wenn die gewünschte Anwendung nicht angezeigt wird, verwenden Sie das Steuerelement **Filter** oberhalb der Liste **Alle Anwendungen**, und legen Sie die Option **Anzeigen** auf **Alle Anwendungen** fest.

6.  Wählen Sie die Anwendung aus, für die Sie das einmalige Anmelden konfigurieren möchten.

7.  Nachdem die Anwendung geladen wurde, klicken Sie im linken Navigationsmenü der Anwendung auf **Einmaliges Anmelden**.

8.  Wählen Sie **SAML-basierte Anmeldung** im Dropdownmenü **Modus** aus.

9.  Geben Sie unter **Domäne und URLs** die erforderlichen Werte ein. Sie sollten diese Werte vom Hersteller der Anwendung erhalten.

   1. Zum Konfigurieren der Anwendung als SP-initiierte SSO ist die Anmelde-URL ein erforderlicher Wert. Bei einigen Anwendungen ist auch der Bezeichner ein erforderlicher Wert.

   2. Zum Konfigurieren der Anwendung als IdP-initiierte SSO ist die Antwort-URL ein erforderlicher Wert. Bei einigen Anwendungen ist auch der Bezeichner ein erforderlicher Wert.

10. **Optional:** Klicken Sie auf **Erweiterte URL-Einstellungen anzeigen**, wenn die nicht erforderlichen Werte angezeigt werden sollen.

11. Wählen Sie unter **Benutzerattribute** in der Dropdownliste **Benutzer-ID** den eindeutigen Bezeichner für die Benutzer aus.

12. **Optional:** Klicken Sie auf **Alle weiteren Benutzerattribute anzeigen und bearbeiten**, um die Attribute zu bearbeiten, die bei der Anmeldung durch die Benutzer im SAML-Token an die Anwendung gesendet werden sollen.

   So fügen Sie ein Attribut hinzu

   1. Klicken Sie auf **Attribut hinzufügen**. Geben Sie einen **Namen** ein, und wählen Sie in der Dropdownliste einen **Wert** aus.

   2. Klicken Sie auf **Speichern**. Das neue Attribut wird in der Tabelle angezeigt.

13. Klicken Sie auf **&lt;Anwendungsname&gt; konfigurieren** , um auf die Dokumentation zum Konfigurieren des einmaligen Anmeldens in der Anwendung zuzugreifen. Sie enthält außerdem die Metadaten-URLs und das Zertifikat, die zum Einrichten von SSO mit der Anwendung erforderlich sind.

14. Klicken Sie zum Speichern der Konfiguration auf **Speichern**.

15. Weisen Sie der Anwendung Benutzer zu.

<a id="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application" class="xliff"></a>

### Auswählen der Benutzer-ID und Hinzufügen von Benutzerattributen, die an die Anwendung gesendet werden

Führen Sie die folgenden Schritte aus, um die Benutzer-ID auszuwählen oder Benutzerattribute hinzuzufügen:

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** oder **Co-Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie auf **Alle Anwendungen**, um eine Liste aller Anwendungen anzuzeigen.

  * Wenn die gewünschte Anwendung nicht angezeigt wird, verwenden Sie das Steuerelement **Filter** oberhalb der Liste **Alle Anwendungen**, und legen Sie die Option **Anzeigen** auf **Alle Anwendungen** fest.

6.  Wählen Sie die Anwendung aus, für die Sie das einmalige Anmelden konfiguriert haben.

7.  Nachdem die Anwendung geladen wurde, klicken Sie im linken Navigationsmenü der Anwendung auf **Einmaliges Anmelden**.

8.  Wählen Sie im Abschnitt **Benutzerattribute** in der Dropdownliste **Benutzer-ID** den eindeutigen Bezeichner für die Benutzer aus. Die ausgewählte Option muss mit dem erwarteten Wert in der Anwendung übereinstimmen, um den Benutzer zu authentifizieren.

    >[!NOTE]
    >In Azure AD wird das Format für das NameID-Attribut (Benutzer-ID) auf Grundlage des ausgewählten Werts oder des Formats festgelegt, das von der Anwendung in der SAML-Authentifizierungsanforderung angefordert wird. Weitere Informationen finden Sie im Artikel [SAML-Protokoll für einmaliges Anmelden](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) im Abschnitt zu NameIDPolicy.
    >
    >

9.  Klicken Sie zum Hinzufügen von Benutzerattributen auf **Alle weiteren Benutzerattribute anzeigen und bearbeiten**, um die Attribute zu bearbeiten, die bei der Anmeldung durch die Benutzer im SAML-Token an die Anwendung gesendet werden sollen.

   So fügen Sie ein Attribut hinzu

   1. Klicken Sie auf **Attribut hinzufügen**. Geben Sie einen **Namen** ein, und wählen Sie in der Dropdownliste einen **Wert** aus.

   2. Klicken Sie auf **Speichern**. Das neue Attribut wird in der Tabelle angezeigt.

<a id="download-the-azure-ad-metadata-or-certificate" class="xliff"></a>

### Herunterladen der Azure AD-Metadaten oder des Zertifikats

Führen Sie zum Herunterladen der Metadaten für die Anwendung oder des Zertifikats aus Azure AD die folgenden Schritte aus:

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** oder **Co-Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie auf **Alle Anwendungen**, um eine Liste aller Anwendungen anzuzeigen.

  * Wenn die gewünschte Anwendung nicht angezeigt wird, verwenden Sie das Steuerelement **Filter** oberhalb der Liste **Alle Anwendungen**, und legen Sie die Option **Anzeigen** auf **Alle Anwendungen** fest.

6.  Wählen Sie die Anwendung aus, für die Sie das einmalige Anmelden konfiguriert haben.

7.  Nachdem die Anwendung geladen wurde, klicken Sie im linken Navigationsmenü der Anwendung auf **Einmaliges Anmelden**.

8.  Wechseln Sie zum Abschnitt **SAML-Signaturzertifikat**, und klicken Sie auf den Spaltenwert **Herunterladen**. Abhängig davon, welche Werte die Anwendung für die Konfiguration des einmaligen Anmeldens benötigt, wird die Option zum Herunterladen des Metadaten-XML-Dokuments oder des Zertifikats angezeigt.

    Azure AD stellt keine URL zum Abrufen der Metadaten bereit. Die Metadaten können nur als XML-Datei abgerufen werden.

<a id="how-to-configure-federated-single-sign-on-for-a-non-gallery-application" class="xliff"></a>

## Konfigurieren des einmaligen Anmeldens im Verbund für eine nicht im Katalog enthaltene Anwendung

Zum Konfigurieren einer Anwendung, die nicht im Katalog enthalten ist, benötigen Sie Azure AD Premium, und die Anwendung muss SAML 2.0 unterstützen. Weitere Informationen zu Azure AD-Versionen finden Sie in der [Preisübersicht für Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).

-   [Konfigurieren der Metadatenwerte für die Anwendung in Azure AD (Anmelde-URL, ID, Antwort-URL)](#configuring-single-sign-on)

-   [Auswählen der Benutzer-ID und Hinzufügen von Benutzerattributen, die an die Anwendung gesendet werden](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Abrufen der Azure AD-Metadaten und des Zertifikats](#download-the-azure-ad-metadata-or-certificate)

-   [Konfigurieren von Azure AD-Metadatenwerten in der Anwendung (Anmelde-URL, Aussteller, Abmelde-URL und Zertifikat)](#configuring-single-sign-on)

<a id="configure-the-applications-metadata-values-in-azure-ad-sign-on-url-identifier-reply-url" class="xliff"></a>

### Konfigurieren der Metadatenwerte für die Anwendung in Azure AD (Anmelde-URL, ID, Antwort-URL)

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden für eine nicht im Azure AD-Katalog aufgeführte Anwendung zu konfigurieren:

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** oder **Co-Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie oben rechts auf dem Blatt **Unternehmensanwendungen** auf die Schaltfläche **Hinzufügen**.

6.  Klicken Sie auf **Nicht-Kataloganwendung** im Abschnitt **Eigene App hinzufügen**.

7.  Geben Sie den Namen der Anwendung im Textfeld **Name** ein.

8.  Klicken Sie auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

9.  Nachdem die Anwendung geladen wurde, klicken Sie im linken Navigationsmenü der Anwendung auf **Einmaliges Anmelden**.

10. Wählen Sie **SAML-basierte Anmeldung** im Dropdownmenü **Modus** aus.

11. Geben Sie unter **Domäne und URLs** die erforderlichen Werte ein. Sie sollten diese Werte vom Hersteller der Anwendung erhalten.

  1. Geben Sie zum Konfigurieren der Anwendung als IdP-initiierte SSO die Antwort-URL und die ID ein.

  2. **Optional:** Zum Konfigurieren der Anwendung als SP-initiierte SSO ist die Anmelde-URL ein erforderlicher Wert.

12. Wählen Sie unter **Benutzerattribute** in der Dropdownliste **Benutzer-ID** den eindeutigen Bezeichner für die Benutzer aus.

13. **Optional:** Klicken Sie auf **Alle weiteren Benutzerattribute anzeigen und bearbeiten**, um die Attribute zu bearbeiten, die bei der Anmeldung durch die Benutzer im SAML-Token an die Anwendung gesendet werden sollen.

   So fügen Sie ein Attribut hinzu

   1. Klicken Sie auf **Attribut hinzufügen**. Geben Sie einen **Namen** ein, und wählen Sie in der Dropdownliste einen **Wert** aus.

   2. Klicken Sie auf **Speichern**. Das neue Attribut wird in der Tabelle angezeigt.

14. Klicken Sie auf **&lt;Anwendungsname&gt; konfigurieren** , um auf die Dokumentation zum Konfigurieren des einmaligen Anmeldens in der Anwendung zuzugreifen. Außerdem sind Azure AD-URLs und das Zertifikat für die Anwendung erforderlich.

<a id="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application" class="xliff"></a>

### Auswählen der Benutzer-ID und Hinzufügen von Benutzerattributen, die an die Anwendung gesendet werden

Führen Sie die folgenden Schritte aus, um die Benutzer-ID auszuwählen oder Benutzerattribute hinzuzufügen:

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** oder **Co-Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie auf **Alle Anwendungen**, um eine Liste aller Anwendungen anzuzeigen.

  * Wenn die gewünschte Anwendung nicht angezeigt wird, verwenden Sie das Steuerelement **Filter** oberhalb der Liste **Alle Anwendungen**, und legen Sie die Option **Anzeigen** auf **Alle Anwendungen** fest.

6.  Wählen Sie die Anwendung aus, für die Sie das einmalige Anmelden konfiguriert haben.

7.  Nachdem die Anwendung geladen wurde, klicken Sie im linken Navigationsmenü der Anwendung auf **Einmaliges Anmelden**.

8.  Wählen Sie im Abschnitt **Benutzerattribute** in der Dropdownliste **Benutzer-ID** den eindeutigen Bezeichner für die Benutzer aus. Die ausgewählte Option muss mit dem erwarteten Wert in der Anwendung übereinstimmen, um den Benutzer zu authentifizieren.

   >[!NOTE]
   >In Azure AD wird das Format für das NameID-Attribut (Benutzer-ID) auf Grundlage des ausgewählten Werts oder des Formats festgelegt, das von der Anwendung in der SAML-Authentifizierungsanforderung angefordert wird. Weitere Informationen finden Sie im Artikel [SAML-Protokoll für einmaliges Anmelden](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) im Abschnitt zu NameIDPolicy.
   >
   >

9.  Klicken Sie zum Hinzufügen von Benutzerattributen auf **Alle weiteren Benutzerattribute anzeigen und bearbeiten**, um die Attribute zu bearbeiten, die bei der Anmeldung durch die Benutzer im SAML-Token an die Anwendung gesendet werden sollen.

   So fügen Sie ein Attribut hinzu

   1. Klicken Sie auf **Attribut hinzufügen**. Geben Sie einen **Namen** ein, und wählen Sie in der Dropdownliste einen **Wert** aus.

   2. Klicken Sie auf **Speichern**. Das neue Attribut wird in der Tabelle angezeigt.

<a id="download-the-azure-ad-metadata-or-certificate" class="xliff"></a>

### Herunterladen der Azure AD-Metadaten oder des Zertifikats

Führen Sie zum Herunterladen der Metadaten für die Anwendung oder des Zertifikats aus Azure AD die folgenden Schritte aus:

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** oder **Co-Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie auf **Alle Anwendungen**, um eine Liste aller Anwendungen anzuzeigen.

   * Wenn die gewünschte Anwendung nicht angezeigt wird, verwenden Sie das Steuerelement **Filter** oberhalb der Liste **Alle Anwendungen**, und legen Sie die Option **Anzeigen** auf **Alle Anwendungen** fest.

6.  Wählen Sie die Anwendung aus, für die Sie das einmalige Anmelden konfiguriert haben.

7.  Nachdem die Anwendung geladen wurde, klicken Sie im linken Navigationsmenü der Anwendung auf **Einmaliges Anmelden**.

8.  Wechseln Sie zum Abschnitt **SAML-Signaturzertifikat**, und klicken Sie auf den Spaltenwert **Herunterladen**. Abhängig davon, welche Werte die Anwendung für die Konfiguration des einmaligen Anmeldens benötigt, wird die Option zum Herunterladen des Metadaten-XML-Dokuments oder des Zertifikats angezeigt.

    Azure AD stellt keine URL zum Abrufen der Metadaten bereit. Die Metadaten können nur als XML-Datei abgerufen werden.

<a id="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application" class="xliff"></a>

## Konfigurieren des einmaligen Anmeldens per Kennwort für eine Azure AD-Kataloganwendung

Um eine Anwendung aus dem Azure AD-Katalog zu konfigurieren, müssen folgende Schritte ausgeführt werden:

-   [Hinzufügen einer Anwendung aus dem Azure AD-Katalog](#add-an-application)

-   [Konfigurieren der Anwendung für das einmalige Anmelden per Kennwort](#configure-the-application)

<a id="add-an-application-from-the-azure-ad-gallery" class="xliff"></a>

### Hinzufügen einer Anwendung aus dem Azure AD-Katalog

Um eine Anwendung aus dem Azure AD-Katalog hinzuzufügen, führen Sie folgende Schritte aus:

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als **Globaler Administrator** oder **Co-Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie oben rechts auf dem Blatt **Unternehmensanwendungen** auf die Schaltfläche **Hinzufügen**.

6.  Geben Sie im Abschnitt **Aus dem Katalog hinzufügen** im Textfeld **Namen eingeben** den Namen der Anwendung ein.

7.  Wählen Sie die Anwendung aus, die Sie für das einmalige Anmelden konfigurieren möchten.

8.  Bevor Sie die Anwendung hinzufügen, können Sie im Textfeld **Name** den Namen der Anwendung ändern.

9.  Klicken Sie auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

Nach kurzer Zeit wird das Konfigurationsblatt der Anwendung angezeigt.

<a id="configure-the-application-for-password-single-sign-on" class="xliff"></a>

### Konfigurieren der Anwendung für das einmalige Anmelden per Kennwort

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

9.  Weisen Sie der Anwendung Benutzer zu.

10. Darüber hinaus können Sie Anmeldeinformationen für einen Benutzer angeben, indem Sie die Zeile des Benutzers auswählen, auf **Anmeldeinformationen aktualisieren** klicken und den Benutzernamen und das Kennwort für den Benutzer eingeben. Andernfalls werden die Benutzer beim Start der Anwendung aufgefordert, die Anmeldeinformationen selbst einzugeben.

<a id="how-to-configure-password-single-sign-on-for-a-non-gallery-application" class="xliff"></a>

## Konfigurieren des einmaligen Anmeldens per Kennwort für eine nicht im Katalog enthaltene Anwendung

Um eine Anwendung aus dem Azure AD-Katalog zu konfigurieren, müssen folgende Schritte ausgeführt werden:

-   [Hinzufügen einer nicht im Katalog enthaltenen Anwendung](#add-a-non-gallery-application)

-   [Konfigurieren der Anwendung für das einmalige Anmelden per Kennwort](#configure-the-application-for-password-single-sign-on)

<a id="add-a-non-gallery-application" class="xliff"></a>

### Hinzufügen einer nicht im Katalog enthaltenen Anwendung

Um eine Anwendung aus dem Azure AD-Katalog hinzuzufügen, führen Sie folgende Schritte aus:

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als **Globaler Administrator** oder **Co-Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie oben rechts auf dem Blatt **Unternehmensanwendungen** auf die Schaltfläche **Hinzufügen**.

6.  Klicken Sie auf **Nicht-Kataloganwendung**.

7.  Geben Sie den Namen Ihrer Anwendung im Textfeld **Name** ein. Wählen Sie **Hinzufügen** aus.

Nach kurzer Zeit wird das Konfigurationsblatt der Anwendung angezeigt.

<a id="configure-the-application-for-password-single-sign-on" class="xliff"></a>

### Konfigurieren der Anwendung für das einmalige Anmelden per Kennwort

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

11. Darüber hinaus können Sie Anmeldeinformationen für einen Benutzer angeben, indem Sie die Zeile des Benutzers auswählen, auf **Anmeldeinformationen aktualisieren** klicken und den Benutzernamen und das Kennwort für den Benutzer eingeben. Andernfalls werden die Benutzer beim Start der Anwendung aufgefordert, die Anmeldeinformationen selbst einzugeben.

<a id="how-to-assign-a-user-to-an-application-directly" class="xliff"></a>

## Direktes Zuweisen eines Benutzers zu einer Anwendung

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

<a id="if-these-troubleshooting-steps-do-not-the-resolve-the-issue" class="xliff"></a>

## Wenn das Problem mit diesen Problembehandlungsschritten nicht behoben wird

Öffnen Sie ein Supportticket mit den folgenden Informationen, sofern verfügbar:

-   Fehlerkorrelations-ID

-   UPN (E-Mail-Adresse des Benutzers)

-   Mandanten-ID

-   Browsertyp

-   Zeitzone und Uhrzeit/Zeitraum des Auftretens des Fehlers

-   Fiddler-Ablaufverfolgungen

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
[Bereitstellen von einmaligem Anmelden bei Ihren Apps mit dem Anwendungsproxy](active-directory-application-proxy-sso-using-kcd.md)


