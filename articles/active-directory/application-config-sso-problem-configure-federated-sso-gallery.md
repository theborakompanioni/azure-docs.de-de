---
title: "Probleme beim Konfigurieren des einmaligen Anmeldens im Verbund für eine Azure AD-Kataloganwendung | Microsoft-Dokumentation"
description: "Hier finden Sie Informationen zu den häufigsten Problemen, die beim Konfigurieren des einmaligen Anmeldens im Verbund mit SAML bei Anwendungen auftreten können, die im Azure AD-Anwendungskatalog aufgeführt sind."
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
ms.date: 04/04/2017
ms.author: asteen
translationtype: Human Translation
ms.sourcegitcommit: 0d6f6fb24f1f01d703104f925dcd03ee1ff46062
ms.openlocfilehash: b4d3d114f6947639ef865861337ce9f2a6452e76
ms.lasthandoff: 04/17/2017


---

# <a name="problem-configuring-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Probleme beim Konfigurieren des einmaligen Anmeldens im Verbund für eine Azure AD-Kataloganwendung

Wenn beim Konfigurieren einer Anwendung ein Problem auftritt: Stellen Sie sicher, dass Sie alle Schritte im Lernprogramm für die Anwendung ausgeführt haben. Die Konfiguration der Anwendung umfasst eine Inlinedokumentation zum Konfigurieren der Anwendung. Darüber hinaus finden Sie in der [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) ausführliche schrittweise Anweisungen.

## <a name="cant-add-another-instance-of-the-application"></a>Es kann keine weitere Instanz der Anwendung hinzugefügt werden.

Wenn Sie eine zweite Instanz einer Anwendung hinzufügen möchten, muss Folgendes möglich sein:

-   Konfigurieren eines eindeutigen Bezeichners für die zweite Instanz. Sie können nicht den Bezeichner konfigurieren, den Sie bereits für die erste Instanz verwendet haben.

-   Konfigurieren eines anderen Zertifikats als des Zertifikats, das für die erste Instanz verwendet wurde.

Wenn die Anwendung keinen der genannten Punkte unterstützt, können Sie keine zweite Instanz konfigurieren.

## <a name="cant-add-the-identifier-or-the-reply-url"></a>Der Bezeichner oder die Antwort-URL kann nicht hinzugefügt werden.

Wenn Sie den Bezeichner oder die Antwort-URL nicht konfigurieren können, vergewissern Sie sich, dass die Werte für den Bezeichner und die Antwort-URL mit den für die Anwendung vorkonfigurierten Mustern übereinstimmen.

Abrufen der für die Anwendung vorkonfigurierten Muster:

1.  Melden Sie sich beim [**Azure-Portal**](https://portal.azure.com/) als **Globaler Administrator** oder **Co-Administrator** an. Fahren Sie mit Schritt 7 fort, Wenn das Blatt für die Anwendungskonfiguration in Azure AD bereits angezeigt wird.

2.  Öffnen Sie die **Azure Active Directory-Erweiterung**, indem Sie unten im Hauptnavigationsmenü auf der linken Seite auf **Weitere Dienste** klicken.

3.  Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und wählen Sie das Element **Azure Active Directory** aus.

4.  Klicken Sie im linken Azure Active Directory-Navigationsmenü auf **Unternehmensanwendungen**.

5.  Klicken Sie auf **Alle Anwendungen**, um eine Liste aller Anwendungen anzuzeigen.

   * Wenn die gewünschte Anwendung nicht angezeigt wird, verwenden Sie das Steuerelement **Filter** oberhalb der Liste **Alle Anwendungen**, und legen Sie die Option **Anzeigen** auf **Alle Anwendungen** fest.

6.  Wählen Sie die Anwendung aus, für die Sie das einmalige Anmelden konfigurieren möchten.

7.  Nachdem die Anwendung geladen wurde, klicken Sie im linken Navigationsmenü der Anwendung auf **Einmaliges Anmelden**.

8.  Wählen Sie **SAML-basierte Anmeldung** im Dropdownmenü **Modus** aus.

9.  Wechseln Sie zum Textfeld **Bezeichner** oder **Antwort-URL** im Abschnitt **Domäne und URLs**.

10. Es gibt drei Möglichkeiten, um die unterstützten Muster für die Anwendung zu ermitteln:

   * Im Textfeld werden die unterstützten Muster als Platzhalter angezeigt. *Beispiel:* <https://contoso.com>

   * Wenn das Muster nicht unterstützt wird, wird ein rotes Ausrufezeichen angezeigt, wenn Sie versuchen, den Wert im Textfeld eingeben. Wenn Sie den Mauszeiger auf das rote Ausrufungszeichen halten, werden die unterstützten Muster angezeigt.

   * Im Lernprogramm für die Anwendung erhalten Sie ebenfalls Informationen über die unterstützten Muster. Wechseln Sie im Abschnitt **Konfigurieren des einmaligen Anmeldens in Azure AD** zum Schritt zum Konfigurieren der Werte im Abschnitt **Domäne und URLs**.

Wenn die Werte nicht mit den in Azure AD vorkonfigurierten Mustern übereinstimmen, haben Sie folgende Möglichkeiten. Sie können:

-   Wenden Sie sich an den Hersteller der Anwendung, um Werte zu erhalten, die den in Azure AD vorkonfigurierten Mustern entsprechen.

-   Alternativ können Sie sich an das Azure AD-Team unter <aadapprequest@microsoft.com> wenden oder einen Kommentar zum Lernprogramm senden, um die Aktualisierung der unterstützten Muster für die Anwendung anzufordern.

## <a name="where-do-i-set-the-entityid-user-identifier-format"></a>Wo wird das Format für EntityID (Benutzer-ID) festgelegt?

Sie können das Format für EntityID (Benutzer-ID), das von Azure AD nach der Benutzerauthentifizierung in der Antwort an die Anwendung gesendet wird, nicht auswählen.

In Azure AD wird das Format für das NameID-Attribut (Benutzer-ID) auf Grundlage des ausgewählten Werts oder des Formats festgelegt, das von der Anwendung in der SAML-Authentifizierungsanforderung angefordert wird. Weitere Informationen finden Sie im Artikel [SAML-Protokoll für einmaliges Anmelden](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) im Abschnitt zu NameIDPolicy.

## <a name="cant-find-the-azure-ad-metadata-to-complete-the-configuration-with-the-application"></a>Ich kann die Azure AD-Metadaten zum Abschließen der Konfiguration in der Anwendung nicht finden.

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

## <a name="next-steps"></a>Nächste Schritte
[Verwalten von Anwendungen mit Azure Active Directory](active-directory-enable-sso-scenario.md)

