---
title: Eine zugewiesene Anwendung wird im Zugriffsbereich nicht angezeigt | Microsoft-Dokumentation
description: Beheben des Problems, dass eine Anwendung nicht im Zugriffsbereich angezeigt wird
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
ms.openlocfilehash: 605e68dd86e5188f8a872745164ac879c40a1278
ms.contentlocale: de-de
ms.lasthandoff: 04/17/2017

---

<a id="an-assigned-application-is-not-appearing-on-the-access-panel" class="xliff"></a>

# Eine zugewiesene Anwendung wird im Zugriffsbereich nicht angezeigt.

Der Zugriffsbereich ist ein webbasiertes Portal, mit dem Benutzer mit einem Geschäfts-, Schul- oder Unikonto in Azure Active Directory (Azure AD) cloudbasierte Anwendungen anzeigen und starten können, für die der Azure AD-Administrator ihnen Zugriff gewährt hat. Diese Anwendungen werden im Namen des Benutzers im Azure AD-Portal konfiguriert. Die Anwendung muss ordnungsgemäß konfiguriert und dem Benutzer oder einer Gruppe, in der der Benutzer Mitglied ist, zugewiesen sein, damit sie im Zugriffsbereich angezeigt wird.

Es gibt drei Kategorien von Apps, die für einen Benutzer angezeigt werden können:

-   Office 365-Anwendungen

-   Anwendungen von Microsoft und Drittanbietern, für die verbundbasiertes SSO konfiguriert ist

-   Anwendungen mit kennwortbasiertem SSO

-   Anwendungen mit vorhandenen SSO-Lösungen

<a id="general-issues-to-check-first" class="xliff"></a>

## Allgemeine Probleme, die zuerst überprüft werden sollten

-   Wenn eine Anwendung gerade einem Benutzer hinzugefügt wurde, melden Sie sich im Zugriffsbereich des Benutzers nach einigen Minuten an und wieder ab, um zu überprüfen, ob die Anwendung hinzugefügt wurde.

-   Wenn gerade eine Lizenz von einem Benutzer oder einer Gruppe, in der der Benutzer Mitglied ist, entfernt wurde, kann es je nach Größe und Komplexität der Gruppe lange dauern, bis die Änderungen durchgeführt wurden. Lassen Sie zusätzliche Zeit verstreichen, bevor Sie sich beim Zugriffsbereich anmelden.

<a id="problems-related-to-application-configuration" class="xliff"></a>

## Probleme in Verbindung mit der Anwendungskonfiguration

Eine Anwendung wird möglicherweise nicht im Zugriffsbereich des Benutzers angezeigt, weil sie nicht ordnungsgemäß konfiguriert ist. Nachfolgend werden einige Methoden beschrieben, mit denen Sie Probleme bei der Anwendungskonfiguration beheben können:

-   [Konfigurieren von einmaligem Anmelden im Verbund für eine Azure AD-Kataloganwendung](#how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application)

-   [Konfigurieren von einmaligem Anmelden im Verbund für eine nicht im Katalog enthaltene Anwendung](#how-to-configure-federated-single-sign-on-for-a-non-gallery-application)

-   [Konfigurieren einer Anwendung mit einmaligem Anmelden per Kennwort für eine Azure AD-Kataloganwendung](#how-to-configure-password-single-sign-on-for-a-non-gallery-application)

-   [Konfigurieren einer Anwendung mit einmaligem Anmelden per Kennwort für eine nicht im Katalog enthaltene Anwendung](#how-to-configure-password-single-sign-on-for-a-non-gallery-application)

<a id="how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application" class="xliff"></a>

### Konfigurieren von einmaligem Anmelden im Verbund für eine Azure AD-Kataloganwendung

Für alle Anwendungen im Azure AD-Katalog mit Enterprise Single Sign-On ist ein Schritt-für-Schritt-Tutorial verfügbar. In der [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) finden Sie ausführliche schrittweise Anweisungen.

Um eine Anwendung aus dem Azure AD-Katalog zu konfigurieren, müssen folgende Schritte ausgeführt werden:

-   [Hinzufügen einer Anwendung aus dem Azure AD-Katalog](#add-an-application-from-the-azure-ad-gallery)

-   [Konfigurieren der Metadatenwerte für die Anwendung in Azure AD (Anmelde-URL, ID, Antwort-URL)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Auswählen der Benutzer-ID und Hinzufügen von Benutzerattributen, die an die Anwendung gesendet werden](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Abrufen der Azure AD-Metadaten und des Zertifikats](#download-the-azure-ad-metadata-or-certificate)

-   [Konfigurieren von Azure AD-Metadatenwerten in der Anwendung (Anmelde-URL, Aussteller, Abmelde-URL und Zertifikat)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

<a id="add-an-application-from-the-azure-ad-gallery" class="xliff"></a>

#### Hinzufügen einer Anwendung aus dem Azure AD-Katalog

Um eine Anwendung aus dem Azure AD-Katalog hinzuzufügen, führen Sie folgende Schritte aus:

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als **Globaler Administrator** oder **Co-Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie rechts oben auf dem Blatt **Unternehmensanwendungen** auf die Schaltfläche **Hinzufügen**.

6.  Geben Sie im Abschnitt **Aus Katalog hinzufügen** im Textfeld **Namen eingeben** den Namen der Anwendung ein.

7.  Wählen Sie die Anwendung aus, die Sie für das einmalige Anmelden konfigurieren möchten.

8.  Bevor Sie die Anwendung hinzufügen, können Sie im Textfeld **Name** den Namen der Anwendung ändern.

9.  Klicken Sie auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

Nach kurzer Zeit wird das Konfigurationsblatt der Anwendung angezeigt.

<a id="configure-single-sign-on-for-an-application-from-the-azure-ad-gallery" class="xliff"></a>

#### Konfigurieren von einmaligem Anmelden für eine Anwendung aus dem Azure AD-Katalog

Um das einmalige Anmelden für eine Anwendung zu konfigurieren, führen Sie folgende Schritte aus:

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** oder **Co-Administrator** an.

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

13. Klicken Sie auf **&lt;Anwendungsname&gt; konfigurieren**, um auf die Dokumentation zum Konfigurieren des einmaligen Anmeldens in der Anwendung zuzugreifen. Sie enthält außerdem die Metadaten-URLs und das Zertifikat, die zum Einrichten von SSO mit der Anwendung erforderlich sind.

14. Klicken Sie zum Speichern der Konfiguration auf **Speichern**.

15. Weisen Sie der Anwendung Benutzer zu.

<a id="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application" class="xliff"></a>

#### Auswählen der Benutzer-ID und Hinzufügen von Benutzerattributen, die an die Anwendung gesendet werden

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

#### Herunterladen der Azure AD-Metadaten oder des Zertifikats

Führen Sie zum Herunterladen der Metadaten für die Anwendung oder des Zertifikats aus Azure AD die folgenden Schritte aus:

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** oder **Co-Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie auf **Alle Anwendungen**, um eine Liste aller Anwendungen anzuzeigen.

  * Wenn die gewünschte Anwendung nicht angezeigt wird, verwenden Sie das Steuerelement **Filter** oberhalb der Liste **Alle Anwendungen**, und legen Sie die Option **Anzeigen** auf **Alle Anwendungen** fest.

6.  Wählen Sie die Anwendung aus, für die Sie das einmalige Anmelden konfiguriert haben.

7.  Nachdem die Anwendung geladen wurde, klicken Sie im linken Navigationsmenü der Anwendung auf **Einmaliges Anmelden**.

8.  Wechseln Sie zum Abschnitt **SAML-Signaturzertifikat**, und klicken Sie auf den Spaltenwert **Herunterladen**. Abhängig davon, welche Werte die Anwendung für die Konfiguration des einmaligen Anmeldens benötigt, wird die Option zum Herunterladen des Metadaten-XML oder des Zertifikats angezeigt.

    Azure AD stellt keine URL zum Abrufen der Metadaten bereit. Die Metadaten können nur als XML-Datei abgerufen werden.

<a id="how-to-configure-federated-single-sign-on-for-a-non-gallery-application" class="xliff"></a>

### Konfigurieren des einmaligen Anmeldens im Verbund für eine nicht im Katalog enthaltene Anwendung

Zum Konfigurieren einer Anwendung, die nicht im Katalog enthalten ist, benötigen Sie Azure AD Premium, und die Anwendung muss SAML 2.0 unterstützen. Weitere Informationen zu Azure AD-Versionen finden Sie in der [Preisübersicht für Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).

-   [Konfigurieren der Metadatenwerte für die Anwendung in Azure AD (Anmelde-URL, ID, Antwort-URL)](#configuring-single-sign-on)

-   [Auswählen der Benutzer-ID und Hinzufügen von Benutzerattributen, die an die Anwendung gesendet werden](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Abrufen der Azure AD-Metadaten und des Zertifikats](#download-the-azure-ad-metadata-or-certificate)

-   [Konfigurieren von Azure AD-Metadatenwerten in der Anwendung (Anmelde-URL, Aussteller, Abmelde-URL und Zertifikat)](#configuring-single-sign-on)

<a id="configure-the-applications-metadata-values-in-azure-ad-sign-on-url-identifier-reply-url" class="xliff"></a>

#### Konfigurieren der Metadatenwerte für die Anwendung in Azure AD (Anmelde-URL, ID, Antwort-URL)

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden für eine nicht im Azure AD-Katalog aufgeführte Anwendung zu konfigurieren:

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** oder **Co-Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie rechts oben auf dem Blatt **Unternehmensanwendungen** auf die Schaltfläche **Hinzufügen**.

6.  Klicken Sie im Abschnitt **Eigene App hinzufügen** auf **Nicht-Kataloganwendung**.

7.  Geben Sie den Namen der Anwendung im Textfeld **Name** ein.

8.  Klicken Sie auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

9.  Nachdem die Anwendung geladen wurde, klicken Sie im linken Navigationsmenü der Anwendung auf **Einmaliges Anmelden**.

10. Wählen Sie **SAML-basierte Anmeldung** im Dropdownmenü **Modus** aus.

11. Geben Sie unter **Domäne und URLs** die erforderlichen Werte ein. Sie sollten diese Werte vom Hersteller der Anwendung erhalten.

   1. Geben Sie zum Konfigurieren der Anwendung als IdP-initiierte SSO die Antwort-URL und die ID ein.

   2.  **Optional:** Zum Konfigurieren der Anwendung als SP-initiierte SSO ist die Anmelde-URL ein erforderlicher Wert.

12. Wählen Sie unter **Benutzerattribute** in der Dropdownliste **Benutzer-ID** den eindeutigen Bezeichner für die Benutzer aus.

13. **Optional:** Klicken Sie auf **Alle weiteren Benutzerattribute anzeigen und bearbeiten**, um die Attribute zu bearbeiten, die bei der Anmeldung durch die Benutzer im SAML-Token an die Anwendung gesendet werden sollen.

   So fügen Sie ein Attribut hinzu

   1. Klicken Sie auf **Attribut hinzufügen**. Geben Sie einen **Namen** ein, und wählen Sie in der Dropdownliste einen **Wert** aus.

   2. Klicken Sie auf **Speichern**. Das neue Attribut wird in der Tabelle angezeigt.

14. Klicken Sie auf **&lt;Anwendungsname&gt; konfigurieren**, um auf die Dokumentation zum Konfigurieren des einmaligen Anmeldens in der Anwendung zuzugreifen. Außerdem sind Azure AD-URLs und das Zertifikat für die Anwendung erforderlich.

<a id="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application" class="xliff"></a>

#### Auswählen der Benutzer-ID und Hinzufügen von Benutzerattributen, die an die Anwendung gesendet werden

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

#### Herunterladen der Azure AD-Metadaten oder des Zertifikats

Führen Sie zum Herunterladen der Metadaten für die Anwendung oder des Zertifikats aus Azure AD die folgenden Schritte aus:

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** oder **Co-Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie auf **Alle Anwendungen**, um eine Liste aller Anwendungen anzuzeigen.

   * Wenn die gewünschte Anwendung nicht angezeigt wird, verwenden Sie das Steuerelement **Filter** oberhalb der Liste **Alle Anwendungen**, und legen Sie die Option **Anzeigen** auf **Alle Anwendungen** fest.

6.  Wählen Sie die Anwendung aus, für die Sie das einmalige Anmelden konfiguriert haben.

7.  Nachdem die Anwendung geladen wurde, klicken Sie im linken Navigationsmenü der Anwendung auf **Einmaliges Anmelden**.

8.  Wechseln Sie zum Abschnitt **SAML-Signaturzertifikat**, und klicken Sie auf den Spaltenwert **Herunterladen**. Abhängig davon, welche Werte die Anwendung für die Konfiguration des einmaligen Anmeldens benötigt, wird die Option zum Herunterladen des Metadaten-XML oder des Zertifikats angezeigt.

Azure AD stellt keine URL zum Abrufen der Metadaten bereit. Die Metadaten können nur als XML-Datei abgerufen werden.

<a id="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application" class="xliff"></a>

### Konfigurieren des einmaligen Anmeldens per Kennwort für eine Azure AD-Kataloganwendung

Um eine Anwendung aus dem Azure AD-Katalog zu konfigurieren, müssen folgende Schritte ausgeführt werden:

-   [Hinzufügen einer Anwendung aus dem Azure AD-Katalog](#add-an-application-from-the-azure-ad-gallery)

-   [Konfigurieren der Anwendung für das einmalige Anmelden per Kennwort](#configure-the-application-for-password-single-sign-on)

<a id="add-an-application-from-the-azure-ad-gallery" class="xliff"></a>

#### Hinzufügen einer Anwendung aus dem Azure AD-Katalog

Um eine Anwendung aus dem Azure AD-Katalog hinzuzufügen, führen Sie folgende Schritte aus:

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als **Globaler Administrator** oder **Co-Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie rechts oben auf dem Blatt **Unternehmensanwendungen** auf die Schaltfläche **Hinzufügen**.

6.  Geben Sie im Abschnitt **Aus Katalog hinzufügen** im Textfeld **Namen eingeben** den Namen der Anwendung ein.

7.  Wählen Sie die Anwendung aus, die Sie für das einmalige Anmelden konfigurieren möchten.

8.  Bevor Sie die Anwendung hinzufügen, können Sie im Textfeld **Name** den Namen der Anwendung ändern.

9.  Klicken Sie auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

Nach kurzer Zeit wird das Konfigurationsblatt der Anwendung angezeigt.

<a id="configure-the-application-for-password-single-sign-on" class="xliff"></a>

#### Konfigurieren der Anwendung für das einmalige Anmelden per Kennwort

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

<a id="how-to-configure-password-single-sign-on-for-a-non-gallery-application" class="xliff"></a>

### Konfigurieren des einmaligen Anmeldens per Kennwort für eine nicht im Katalog enthaltene Anwendung

Um eine Anwendung aus dem Azure AD-Katalog zu konfigurieren, müssen folgende Schritte ausgeführt werden:

-   [Hinzufügen einer nicht im Katalog enthaltenen Anwendung](#add-a-non-gallery-application)

-   [Konfigurieren der Anwendung für das einmalige Anmelden per Kennwort](#configure-the-application-for-password-single-sign-on)

<a id="add-a-non-gallery-application" class="xliff"></a>

#### Hinzufügen einer nicht im Katalog enthaltenen Anwendung

Um eine Anwendung aus dem Azure AD-Katalog hinzuzufügen, führen Sie folgende Schritte aus:

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als **Globaler Administrator** oder **Co-Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie rechts oben auf dem Blatt **Unternehmensanwendungen** auf die Schaltfläche **Hinzufügen**.

6.  Klicken Sie auf **Nicht-Kataloganwendung**.

7.  Geben Sie den Namen Ihrer Anwendung im Textfeld **Name** ein. Wählen Sie **Hinzufügen** aus.

Nach kurzer Zeit wird das Konfigurationsblatt der Anwendung angezeigt.

<a id="configure-the-application-for-password-single-sign-on" class="xliff"></a>

#### Konfigurieren der Anwendung für das einmalige Anmelden per Kennwort

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

10. [Weisen Sie der Anwendung Benutzer zu](#how-to-assign-a-user-to-an-application-directly).

11. Darüber hinaus können Sie Anmeldeinformationen für einen Benutzer angeben, indem Sie die Zeile des Benutzers auswählen, auf **Anmeldeinformationen aktualisieren** klicken und den Benutzernamen und das Kennwort für den Benutzer eingeben. Andernfalls werden die Benutzer beim Start der Anwendung aufgefordert, die Anmeldeinformationen selbst einzugeben.

<a id="problems-related-to-assigning-applications-to-users" class="xliff"></a>

## Probleme beim Zuweisen von Anwendungen zu Benutzern

Möglicherweise wird im Zugriffsbereich eines Benutzers eine Anwendung nicht für diesen angezeigt, da der Benutzer nicht der Anwendung zugewiesen ist. Nachfolgend sind einige Möglichkeiten aufgeführt, dies zu überprüfen:

-   [Überprüfen, ob der Anwendung ein Benutzer zugewiesen ist](#check-if-a-user-is-assigned-to-the-application)

-   [Direktes Zuweisen eines Benutzers zu einer Anwendung](#how-to-assign-a-user-to-an-application-directly)

-   [Überprüfen, ob ein Benutzer einer Lizenz für die Anwendung zugewiesen ist](#check-if-a-user-is-under-a-license-related-to-the-application)

-   [Zuweisen einer Lizenz zu einem Benutzer](#how-to-assign-a-user-a-license)

<a id="check-if-a-user-is-assigned-to-the-application" class="xliff"></a>

### Überprüfen, ob der Anwendung ein Benutzer zugewiesen ist

Führen Sie die folgenden Schritte aus, um zu überprüfen, ob der Anwendung ein Benutzer zugewiesen ist:

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie auf **Alle Anwendungen**, um eine Liste aller Anwendungen anzuzeigen.

6.  **Suchen** Sie den Namen der betreffenden Anwendung.

7.  Klicken Sie auf **Benutzer und Gruppen**.

8.  Überprüfen Sie, ob der Benutzer der Anwendung zugewiesen ist.

   * Wenn dies nicht der Fall ist, führen Sie die Schritte in „Direktes Zuweisen eines Benutzers zu einer Anwendung“ aus.

<a id="how-to-assign-a-user-to-an-application-directly" class="xliff"></a>

### Direktes Zuweisen eines Benutzers zu einer Anwendung

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

<a id="check-if-a-user-is-under-a-license-related-to-the-application" class="xliff"></a>

### Überprüfen, ob ein Benutzer einer Lizenz für die Anwendung zugewiesen ist

Führen Sie die folgenden Schritte aus, um die einem Benutzer zugewiesenen Lizenzen zu überprüfen:

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im Navigationsmenü auf **Benutzer und Gruppen**.

5.  Klicken Sie auf **Alle Benutzer**.

6.  **Suchen** Sie nach dem Benutzer, an dem Sie interessiert sind, und **klicken Sie auf die Zeile**, um ihn auszuwählen.

7.  Klicken Sie auf **Lizenzen**, um anzuzeigen, welche Lizenzen dem Benutzer derzeit zugewiesen sind.

  * Wenn der Benutzer einer Office-Lizenz zugewiesen ist, können im Zugriffsbereich des Benutzers Erstanbieter-Office-Anwendungen angezeigt werden.

<a id="how-to-assign-a-user-a-license" class="xliff"></a>

### Zuweisen einer Lizenz zu einem Benutzer 

Führen Sie die folgenden Schritte aus, um einem Benutzer eine Lizenz zuzuweisen:

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im Navigationsmenü auf **Benutzer und Gruppen**.

5.  Klicken Sie auf **Alle Benutzer**.

6.  **Suchen** Sie nach dem Benutzer, an dem Sie interessiert sind, und **klicken Sie auf die Zeile**, um ihn auszuwählen.

7.  Klicken Sie auf **Lizenzen**, um anzuzeigen, welche Lizenzen dem Benutzer derzeit zugewiesen sind.

8.  Klicken Sie auf die Schaltfläche **Zuweisen**.

9.  Wählen Sie in der Liste der verfügbaren Produkte **ein oder mehrere Produkte** aus.

10. **Optional:** Klicken Sie auf die Option für **Zuweisungsoptionen**, um Produkte abgestuft zuzuweisen. Klicken Sie auf **OK**, wenn Sie fertig sind.

11. Klicken Sie auf die Schaltfläche **Zuweisen**, um dem Benutzer diese Lizenzen zuzuweisen.

<a id="problems-related-to-assigning-applications-to-groups" class="xliff"></a>

## Probleme beim Zuweisen von Anwendungen zu Gruppen

Möglicherweise wird im Zugriffsbereich eines Benutzers eine Anwendung für diesen angezeigt, da der Benutzer Mitglied einer Gruppe ist, der die Anwendung zugewiesen ist. Nachfolgend sind einige Möglichkeiten aufgeführt, dies zu überprüfen:

-   [Überprüfen der Gruppenmitgliedschaften eines Benutzers](#check-a-users-group-memberships)

-   [Direktes Zuweisen einer Anwendung zu einer Gruppe](#how-to-assign-an-application-to-a-group-directly)

-   [Überprüfen, ob ein Benutzer einer Gruppe angehört, die einer Lizenz zugewiesen ist](#check-if-a-user-is-part-of-group-assigned-to-a-license)

-   [Zuweisen einer Lizenz zu einer Gruppe](#how-to-assign-a-license-to-a-group)

<a id="check-a-users-group-memberships" class="xliff"></a>

### Überprüfen der Gruppenmitgliedschaften eines Benutzers

Führen Sie die folgenden Schritte aus, um die Mitgliedschaft einer Gruppe zu überprüfen:

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im Navigationsmenü auf **Benutzer und Gruppen**.

5.  Klicken Sie auf **Alle Benutzer**.

6.  **Suchen** Sie nach dem Benutzer, an dem Sie interessiert sind, und **klicken Sie auf die Zeile**, um ihn auszuwählen.

7.  Klicken Sie auf **Gruppen**.

8.  Überprüfen Sie, ob der Benutzer einer Gruppe angehört, die der Anwendung zugewiesen ist.

  * Wenn Sie den Benutzer aus der Gruppe entfernen möchten, **klicken Sie auf die Zeile** der Gruppe, und wählen Sie „Löschen“ aus.

<a id="how-to-assign-an-application-to-a-group-directly" class="xliff"></a>

### Direktes Zuweisen einer Anwendung zu einer Gruppe

Führen Sie die folgenden Schritte aus, um einer Anwendung eine oder mehrere Gruppen direkt zuzuweisen:

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

10. Geben Sie im Suchfeld **Nach Name oder E-Mail-Adresse suchen** den **vollständigen Gruppennamen** der gewünschten Gruppe ein.

11. Zeigen Sie auf die **Gruppe** in der Liste, um ein **Kontrollkästchen** anzuzeigen. Klicken Sie auf das Kontrollkästchen neben dem Profilbild oder Logo der Gruppe, um den Benutzer zur Liste **Ausgewählt** hinzuzufügen.

12. **Optional**: Wenn Sie **mehrere Gruppen hinzufügen** möchten, geben Sie im Suchfeld **Nach Name oder E-Mail-Adresse suchen** einen weiteren **vollständigen Gruppennamen** ein, und klicken Sie auf das Kontrollkästchen, um diese Gruppe der Liste **Ausgewählt** hinzuzufügen.

13. Wenn Sie alle gewünschten Gruppen ausgewählt haben, klicken Sie auf die Schaltfläche **Auswählen**, um sie der Liste der Benutzer und Gruppen hinzuzufügen, die der Anwendung zugewiesen werden sollen.

14. **Optional**: Klicken Sie auf dem Blatt **Zuweisung hinzufügen** auf das Auswahlfeld **Rolle auswählen**, um eine Rolle auszuwählen, die den ausgewählten Gruppen zugewiesen werden soll.

15. Klicken Sie auf die Schaltfläche **Zuweisen**, um die Anwendung den ausgewählten Gruppen zuzuweisen.

Nach kurzer Zeit können die ausgewählten Benutzer diese Anwendungen im Zugriffsbereich starten.

<a id="check-if-a-user-is-part-of-group-assigned-to-a-license" class="xliff"></a>

### Überprüfen, ob ein Benutzer einer Gruppe angehört, die einer Lizenz zugewiesen ist

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im Navigationsmenü auf **Benutzer und Gruppen**.

5.  Klicken Sie auf **Alle Benutzer**.

6.  **Suchen** Sie nach dem Benutzer, an dem Sie interessiert sind, und **klicken Sie auf die Zeile**, um ihn auszuwählen.

7.  Klicken Sie auf **Gruppen**.

8.  Klicken Sie auf die Zeile für eine bestimmte Gruppe.

9.  Klicken Sie auf **Lizenzen**, um anzuzeigen, welche Lizenzen der Gruppe zugewiesen sind.

   * Wenn die Gruppe einer Office-Lizenz zugewiesen ist, können im Zugriffsbereich des Benutzers bestimmte Erstanbieter-Office-Anwendungen angezeigt werden.

<a id="how-to-assign-a-license-to-a-group" class="xliff"></a>

### Zuweisen einer Lizenz zu einer Gruppe

Führen Sie die folgenden Schritte aus, um einer Gruppe eine Lizenz zuzuweisen:

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im Navigationsmenü auf **Benutzer und Gruppen**.

5.  Klicken sie auf **Alle Gruppen**.

6.  **Suchen** Sie nach der Gruppe, an der Sie interessiert sind, und **klicken Sie auf die Zeile**, um sie auszuwählen.

7.  Klicken Sie auf **Lizenzen**, um anzuzeigen, welche Lizenzen der Gruppe derzeit zugewiesen sind.

8.  Klicken Sie auf die Schaltfläche **Zuweisen**.

9.  Wählen Sie in der Liste der verfügbaren Produkte **ein oder mehrere Produkte** aus.

10. **Optional:** Klicken Sie auf die Option für **Zuweisungsoptionen**, um Produkte abgestuft zuzuweisen. Klicken Sie auf **OK**, wenn Sie fertig sind.

11. Klicken Sie auf die Schaltfläche **Zuweisen**, um der Gruppe diese Lizenzen zuzuweisen. Dies kann je nach der Größe und Komplexität der Gruppe sehr lange dauern.

>[!NOTE]
>Um die Zeit zu verkürzen, sollten Sie in Erwägung ziehen, dem Benutzer vorübergehend direkt eine Lizenz zuzuweisen. 
>
>

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
[Hinzufügen neuer Benutzer zu Azure Active Directory](active-directory-users-create-azure-portal.md)


