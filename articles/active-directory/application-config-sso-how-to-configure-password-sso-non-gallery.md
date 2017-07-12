---
title: "Konfigurieren des einmaligen Anmeldens per Kennwort für eine nicht im Katalog enthaltene Anwendung | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie eine benutzerdefinierte Anwendung für das sichere einmalige Anmelden mit Kennwort konfigurieren, wenn die Anwendung nicht im Azure AD-Anwendungskatalog aufgeführt ist."
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
ms.sourcegitcommit: cc9e81de9bf8a3312da834502fa6ca25e2b5834a
ms.openlocfilehash: 03200aeb1b6ca6f01a78c95e9014f5290c114e6a
ms.contentlocale: de-de
ms.lasthandoff: 04/11/2017

---

<a id="how-to-configure-password-single-sign-on-for-a-non-gallery-application" class="xliff"></a>

# Konfigurieren des einmaligen Anmeldens per Kennwort für eine nicht im Katalog enthaltene Anwendung

Zusätzlich zu den Auswahlmöglichkeiten im Azure AD-Anwendungskatalog können Sie auch eine **nicht im Katalog enthaltene Anwendung** hinzufügen, wenn die gewünschte Anwendung im Katalog nicht aufgeführt wird. Auf diese Weise können Sie Anwendungen hinzufügen, die in Ihrer Organisation bereits vorhanden sind. Sie können auch Anwendungen eines Anbieters hinzufügen, der noch nicht im [Azure AD-Anwendungskatalog](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery) gelistet ist.

Nachdem Sie eine nicht im Katalog enthaltene Anwendung hinzugefügt haben, können Sie die Methode für das einmalige Anmelden konfigurieren, die für diese Anwendung verwendet werden soll. Wählen Sie dazu im [Azure-Portal](https://portal.azure.com/) das Navigationselement **Einmaliges Anmelden** für eine Unternehmensanwendung aus.

Eine der verfügbaren Methoden für das einmalige Anmelden ist die Option [Kennwortbasiertes einmaliges Anmelden](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work). Durch **Hinzufügen einer nicht im Katalog enthaltenen Anwendung** können Sie jede Anwendung integrieren, die HTML-basierte Felder für Benutzername und Kennwort generiert, selbst wenn diese nicht zu den von uns vorab integrierten Anwendungen gehört.

Hierzu wird eine Technologie zum Auslesen von Seiten verwendet, die zur Zugriffsbereichserweiterung gehört. Mit dieser Technologie können wir Felder für die Eingabe von Benutzername und Kennwort automatisch ermitteln und diese Informationen für Ihre spezifische Anwendungsinstanz sicher speichern. Danach können die Felder für Benutzername und Kennwort auf sichere Weise automatisch ausgefüllt werden, wenn ein Benutzer im Anwendungszugriffsbereich zur entsprechenden Anwendung navigiert.

Diese Option eignet sich hervorragend dazu, jede beliebige Anwendung schnell in Azure AD zu integrieren, und ermöglicht Ihnen Folgendes:

-   Integrieren Sie **jede beliebige Anwendung** in Ihren Azure AD-Mandanten, vorausgesetzt, die Anwendung generiert HTML-basierte Eingabefelder für Benutzername und Kennwort.

-   Aktivieren Sie das **einmalige Anmelden für Ihre Benutzer**, indem Sie Benutzernamen und Kennwörter für die in Azure AD integrierte Anwendung sicher speichern und später in der Anwendung automatisch ausfüllen.

-   **Ermitteln Sie automatisch Eingabefelder** für jede beliebige Anwendung. Falls die automatische Ermittlung diese Felder nicht findet, können Sie sie mithilfe der browserbasierten Zugriffsbereichserweiterung manuell suchen.

-   **Unterstützen Sie Anwendungen, die mehrere Anmeldefelder erfordern** für Anwendungen, die über Benutzername und Kennwort hinaus weitere Informationen zur Anmeldung erfordern.

-   **Definieren Sie die Bezeichnungen** der Eingabefelder für Benutzer und Kennwort, die Ihre Benutzer beim Eingeben ihrer Anmeldeinformationen im [Anwendungszugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) sehen.

-   Ermöglichen Sie es Ihren **Benutzern**, für vorhandene Anwendungskonten eigene Benutzernamen und Kennwörter festzulegen und diese manuell im [Anwendungszugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) einzugeben.

-   Ermöglichen Sie es einem **Mitglied der Gruppe „Business“**, mithilfe des Features [Self-Service-Anwendungszugriff](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) die Benutzernamen und Kennwörter festzulegen, die einem Benutzer zugewiesen werden.

-   Ermöglichen Sie es einem **Administrator**, beim [Zuweisen von Benutzern zu einer Anwendung](#_How_to_configure_1) das Feature zum Aktualisieren von Anmeldeinformationen zu verwenden, um die den Benutzern zugewiesenen Benutzernamen und Kennwörter anzugeben.

-   Ermöglichen Sie es einem **Administrator**, beim [Zuweisen von Gruppen zu einer Anwendung](#assign-an-application-to-a-group-directly) das Feature zum Aktualisieren von Anmeldeinformationen zu verwenden, um die von Gruppen von Benutzern gemeinsam genutzten Benutzernamen und Kennwörter anzugeben.

Im Folgenden wird beschrieben, wie Sie das [kennwortbasierte einmalige Anmelden](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) bei einer beliebigen Anwendung aktivieren, die Sie über das Feature zum **Hinzufügen einer nicht im Katalog enthaltenen Anwendung** hinzugefügt haben.

<a id="overview-of-steps-required" class="xliff"></a>

## Übersicht über die erforderlichen Schritte

Um eine Anwendung aus dem Azure AD-Katalog zu konfigurieren, müssen folgende Schritte ausgeführt werden:

-   [Hinzufügen einer nicht im Katalog enthaltenen Anwendung](#add-a-non-gallery-application)

-   [Konfigurieren der Anwendung für das einmalige Anmelden per Kennwort](#configure-the-application-for-password-single-sign-on)

-   [Zuweisen der Anwendung zu einem Benutzer oder einer Gruppe](#assign-the-application-to-a-user-or-a-group)

    -   [Direktes Zuweisen eines Benutzers zu einer Anwendung](#assign-a-user-to-an-application-directly)

    -   [Direktes Zuweisen einer Anwendung zu einer Gruppe](#assign-an-application-to-a-group-directly)

<a id="add-a-non-gallery-application" class="xliff"></a>

## Hinzufügen einer nicht im Katalog enthaltenen Anwendung

Um eine Anwendung aus dem Azure AD-Katalog hinzuzufügen, führen Sie folgende Schritte aus:

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als **Globaler Administrator** oder **Co-Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie oben rechts auf dem Blatt **Unternehmensanwendungen** auf die Schaltfläche **Hinzufügen**.

6.  Klicken Sie auf **Nicht im Katalog enthaltene Anwendung**.

7.  Geben Sie den Namen Ihrer Anwendung im Textfeld **Name** ein. Wählen Sie **Hinzufügen** aus.

Nach kurzer Zeit wird das Konfigurationsblatt der Anwendung angezeigt.

<a id="configure-the-application-for-password-single-sign-on" class="xliff"></a>

## Konfigurieren der Anwendung für das einmalige Anmelden per Kennwort

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

<a id="assign-a-user-to-an-application-directly" class="xliff"></a>

## Direktes Zuweisen eines Benutzers zu einer Anwendung

Um einer Anwendung einen oder mehrere Benutzer direkt zuzuweisen, führen Sie folgende Schritte aus:

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie auf **Alle Anwendungen**, um eine Liste aller Anwendungen anzuzeigen.

  * Wenn die gewünschte Anwendung nicht angezeigt wird, verwenden Sie das Steuerelement **Filter** oberhalb der Liste **Alle Anwendungen**, und legen Sie die Option **Anzeigen** auf **Alle Anwendungen** fest.

6.  Wählen Sie aus der Liste die Anwendung aus, der Sie einen Benutzer zuweisen möchten.

7.  Nachdem die Anwendung geladen wurde, klicken Sie im linken Navigationsmenü der Anwendung auf **Benutzer und Gruppen**.

8.  Klicken Sie oberhalb der Liste **Benutzer und Gruppen** auf die Schaltfläche **Hinzufügen**, um das Blatt **Zuweisung hinzufügen** zu öffnen.

9.  Klicken Sie auf dem Blatt **Zuweisung hinzufügen** auf das Auswahlfeld **Benutzer und Gruppen**.

10. Geben Sie im Suchfeld **Nach Name oder E-Mail-Adresse suchen** den **vollständigen Namen** oder die **E-Mail-Adresse** des gewünschten Benutzers ein.

11. Zeigen Sie auf den **Benutzer** in der Liste, um ein **Kontrollkästchen** anzuzeigen. Klicken Sie auf das Kontrollkästchen neben dem Profilbild oder Logo des Benutzers, um den Benutzer zur Liste **Ausgewählt** hinzuzufügen.

12. **Optional**: Wenn Sie **mehrere Benutzer hinzufügen** möchten, geben Sie im Suchfeld **Nach Name oder E-Mail-Adresse suchen** einen weiteren **vollständigen Namen** oder eine weitere **E-Mail-Adresse** ein, und klicken Sie auf das Kontrollkästchen, um diesen Benutzer zur Liste **Ausgewählt** hinzuzufügen.

13. Wenn Sie alle gewünschten Benutzer ausgewählt haben, klicken Sie auf die Schaltfläche **Auswählen**, um sie zur Liste der Benutzer und Gruppen hinzuzufügen, die der Anwendung zugewiesen werden sollen.

14. **Optional**: Klicken Sie auf dem Blatt **Zuweisung hinzufügen** auf das Auswahlfeld **Rolle auswählen**, um eine Rolle auszuwählen, die den ausgewählten Benutzern zugewiesen werden soll.

15. Klicken Sie auf die Schaltfläche **Zuweisen**, um die Anwendung den ausgewählten Benutzern zuzuweisen.

<a id="assign-an-application-to-a-group-directly" class="xliff"></a>

## Direktes Zuweisen einer Anwendung zu einer Gruppe

Um einer Anwendung eine oder mehrere Gruppen direkt zuzuweisen, führen Sie folgende Schritte aus:

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** an.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie auf **Alle Anwendungen**, um eine Liste aller Anwendungen anzuzeigen.

  * Wenn die gewünschte Anwendung nicht angezeigt wird, verwenden Sie das Steuerelement **Filter** oberhalb der Liste **Alle Anwendungen**, und legen Sie die Option **Anzeigen** auf **Alle Anwendungen** fest.

6.  Wählen Sie aus der Liste die Anwendung aus, der Sie einen Benutzer zuweisen möchten.

7.  Nachdem die Anwendung geladen wurde, klicken Sie im linken Navigationsmenü der Anwendung auf **Benutzer und Gruppen**.

8.  Klicken Sie oberhalb der Liste **Benutzer und Gruppen** auf die Schaltfläche **Hinzufügen**, um das Blatt **Zuweisung hinzufügen** zu öffnen.

9.  Klicken Sie auf dem Blatt **Zuweisung hinzufügen** auf das Auswahlfeld **Benutzer und Gruppen**.

10. Geben Sie im Suchfeld **Nach Name oder E-Mail-Adresse suchen** den **vollständigen Gruppennamen** der gewünschten Gruppe ein.

11. Zeigen Sie auf die **Gruppe** in der Liste, um ein **Kontrollkästchen** anzuzeigen. Klicken Sie auf das Kontrollkästchen neben dem Profilbild oder Logo der Gruppe, um die Gruppe zur Liste **Ausgewählt** hinzuzufügen.

12. **Optional**: Wenn Sie **mehrere Gruppen hinzufügen** möchten, geben Sie im Suchfeld **Nach Name oder E-Mail-Adresse suchen** einen weiteren **vollständigen Gruppennamen** ein, und klicken Sie auf das Kontrollkästchen, um diese Gruppe zur Liste **Ausgewählt** hinzuzufügen.

13. Wenn Sie alle gewünschten Gruppen ausgewählt haben, klicken Sie auf die Schaltfläche **Auswählen**, um sie zur Liste der Benutzer und Gruppen hinzuzufügen, die der Anwendung zugewiesen werden sollen.

14. **Optional**: Klicken Sie auf dem Blatt **Zuweisung hinzufügen** auf das Auswahlfeld **Rolle auswählen**, um eine Rolle auszuwählen, die den ausgewählten Gruppen zugewiesen werden soll.

15. Klicken Sie auf die Schaltfläche **Zuweisen**, um die Anwendung den ausgewählten Gruppen zuzuweisen.

Nach kurzer Zeit können die ausgewählten Benutzer diese Anwendungen im Zugriffsbereich starten.

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
[Bereitstellen von einmaligem Anmelden bei Ihren Apps mit dem Anwendungsproxy](active-directory-application-proxy-sso-using-kcd.md)

