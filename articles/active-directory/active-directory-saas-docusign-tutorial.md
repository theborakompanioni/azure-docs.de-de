---
title: 'Tutorial: Azure Active Directory-Integration mit DocuSign | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und DocuSign konfigurieren.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: a691288b-84c1-40fb-84bd-5b06878865f0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: ac53d0a2c1c0a257e86fe74e10c8781aa6305975
ms.openlocfilehash: 662e853ca31d426a71121d22733256556ca56fc6
ms.contentlocale: de-de
ms.lasthandoff: 02/23/2017


---
# <a name="tutorial-azure-active-directory-integration-with-docusign"></a>Tutorial: Azure Active Directory-Integration mit DocuSign
In diesem Tutorial wird die Integration von Azure und DocuSign erläutert.
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Einen Mandanten in DocuSign

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. [Aktivieren der Anwendungsintegration für DocuSign](#enabling-the-application-integration-for-docusign) 
2. [Konfigurieren der einmaligen Anmeldung](#configuring-single-sign-on) 
3. [Konfigurieren der Kontobereitstellung](#configuring-account-provisioning) 
4. [Zuweisen von Benutzern](#assigning-users) 
   
![Konfigurieren der einmaligen Anmeldung][0]

## <a name="enabling-the-application-integration-for-docusign"></a>Aktivieren der Anwendungsintegration für DocuSign
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für DocuSign aktivieren.

### <a name="to-enable-the-application-integration-for-docusign-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, um die Anwendungsintegration für DocuSign zu aktivieren:
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
    ![Konfigurieren der einmaligen Anmeldung][1]
2. Wählen Sie in der Liste Verzeichnis das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Konfigurieren der einmaligen Anmeldung][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld „Was möchten Sie tun?“ auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Konfigurieren der einmaligen Anmeldung][4]
6. Geben Sie im Suchfeld das Wort **DocuSign**ein.
   
    ![Konfigurieren der einmaligen Anmeldung][5]
7. Wählen Sie im Ergebnisbereich **DocuSign** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Konfigurieren der einmaligen Anmeldung][6]

## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung
In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei DocuSign zu authentifizieren.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **DocuSign** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld „Einmaliges Anmelden konfigurieren“ zu öffnen.
   
    ![Konfigurieren der einmaligen Anmeldung][7]
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei DocuSign anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf „Weiter“.
   
    ![Konfigurieren der einmaligen Anmeldung][8]
3. Führen Sie auf der Seite **App-Einstellungen konfigurieren** die folgenden Schritte aus:
   
    ![Konfigurieren der einmaligen Anmeldung][61]
   
    a. Geben Sie im Textfeld **Anmelde-URL** die `https://account.docusign.com/*` ein.  
   
    b. Geben Sie im Textfeld **Bezeichner** Folgendes ein: `https://account.docusign.com/*`.  
   
    c. Klicken Sie auf **Next**. 

    > [!TIP] 
    > Die Werte für die Anmelde-URL und den Bezeichner sind nur Platzhalter. Anweisungen zum Abrufen der tatsächlichen Werte für Ihre Umgebung finden Sie weiter unten in diesem Thema.


1. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren** für DocuSign auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei lokal auf Ihrem Computer.
   
    ![Konfigurieren der einmaligen Anmeldung][10]
2. Melden Sie sich in einem anderen Webbrowserfenster im **DocuSign-Verwaltungsportal** als Administrator an.
3. Klicken Sie im Navigationsmenü auf der linken Seite auf **Domänen**.
   
    ![Konfigurieren der einmaligen Anmeldung][51]
4. Klicken Sie im rechten Bereich auf **Claim Domain**(Domäne anfordern).
   
    ![Konfigurieren der einmaligen Anmeldung][52]
5. Geben Sie im Dialogfeld **Claim a domain** (Domäne anfordern) im Textfeld **Domänenname** Ihre Unternehmensdomäne ein, und klicken Sie auf **Claim** (Anfordern). Achten Sie darauf, die Domäne zu überprüfen, und vergewissern Sie sich, dass der Status „Aktiv“ lautet.
   
    ![Konfigurieren der einmaligen Anmeldung][53]
6. Klicken Sie im Menü auf der linken Seite auf **Identitätsanbieter**  
   
    ![Konfigurieren der einmaligen Anmeldung][54]
7. Klicken Sie im rechten Bereich auf **Add Identity Provider**(Identitätsanbieter hinzufügen). 
   
    ![Konfigurieren der einmaligen Anmeldung][55]
8. Führen Sie auf der Seite mit den **Identitätsanbietereinstellungen** die folgenden Schritte aus:
   
    ![Konfigurieren der einmaligen Anmeldung][56]

    a. Geben Sie im Textfeld **Name** einen eindeutigen Namen für die Konfiguration ein. Verwenden Sie keine Leerzeichen.

    b. Kopieren Sie im klassischen Azure-Portal die Aussteller-URL und fügen Sie sie in das Textfeld **Aussteller des Identitätsanbieters** ein.

    c. Kopieren Sie im klassischen Azure-Portal die **Remoteanmelde-URL**, und fügen Sie sie in das Textfeld **Anmelde-URL für Identitätsanbieter** ein.

    d. Kopieren Sie im klassischen Azure-Portal die **Remoteabmelde-URL**, und fügen Sie sie in das Textfeld **Abmelde-URL des Identitätsanbieters** ein.

    e. Wählen Sie **Sign AuthN Request**(Authentifizierungsanforderung signieren).

    f. Wählen Sie für **Send AuthN request by** (Authentifizierungsanforderung senden per) die Option **POST** aus.

    g. Wählen Sie für **Send logout request by** (Abmeldeanforderung senden per) die Option **POST** aus. 


1. Wählen Sie im Abschnitt **Benutzerdefinierte Attributzuordnung** das Feld aus, das Sie dem Azure AD-Anspruch zuordnen möchten. In diesem Beispiel wird der Anforderung **emailaddress** der Wert von **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress** zugeordnet. Dies ist der Standard-Anspruchsname aus Azure AD für den E-Mail-Anspruch. 
   
    > [!NOTE]
    > Verwenden Sie den entsprechenden **Benutzerbezeichner** , um den Benutzer aus Azure AD der DocuSign-Benutzerzuordnung zuzuordnen. Wählen Sie das richtige Feld, und geben Sie den entsprechenden Wert basierend auf den Einstellungen Ihrer Organisation ein.
    > 
    > 
   
    ![Konfigurieren der einmaligen Anmeldung][57]
2. Klicken Sie im Abschnitt **Zertifikat des Identitätsanbieters** auf **Zertifikat hinzufügen**, und laden Sie anschließend das aus dem klassischen Azure AD-Portal heruntergeladene Zertifikat hoch.   
   
    ![Konfigurieren der einmaligen Anmeldung][58]
3. Klicken Sie auf **Speichern**.
4. Klicken Sie im Abschnitt **Identitätsanbieter** auf **Aktionen** und dann auf **Endpunkte**.   
   
    ![Konfigurieren der einmaligen Anmeldung][59]
5. Navigieren Sie im klassischen Azure-Portal zurück zur Seite **App-Einstellungen konfigurieren** . 
6. Führen Sie im **DocuSign-Verwaltungsportal** im Abschnitt **SAML 2.0-Endpunkte anzeigen** die folgenden Schritte aus:
   
    ![Konfigurieren der einmaligen Anmeldung][60]
   
    a. Kopieren Sie **Service Provider Issuer URL** (Service Provider-Aussteller-URL), und fügen Sie diese im klassischen Azure-Portal ins Textfeld **Bezeichner** ein.
   
    b. Kopieren Sie **Service Provider Login URL** (Service Provider-Anmelde-URL), und fügen Sie sie im klassischen Azure-Portal ins Textfeld **Anmelde-URL** ein.
   
    c.  Klicken Sie unten auf der Seite auf **Schließen**  
7. Klicken Sie im klassischen Azure-Portal auf **Weiter**. 
8. Wählen Sie im klassischen Azure-Portal die **Bestätigung zur Konfiguration des einmaligen Anmeldens** aus, und klicken Sie dann auf **Weiter**.
   
    ![Anwendungen][14]
9. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.
   
    ![Anwendungen][15]

## <a name="configuring-account-provisioning"></a>Konfigurieren der Kontobereitstellung
In diesem Abschnitt wird erläutert, wie Sie die Bereitstellung von Active Directory-Benutzerkonten für DocuSign aktivieren.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>So konfigurieren Sie die Benutzerbereitstellung
1. Klicken Sie im **klassischen Azure-Portal** auf der **Anwendungsintegrationsseite für DocuSign** auf **Kontobereitstellung konfigurieren**, um das Dialogfeld „Benutzerbereitstellung konfigurieren“ zu öffnen.
   
    ![Konfigurieren der Kontobereitstellung][30]
2. Geben Sie auf der Seite **Einstellungen und Administrator-Anmeldeinformationen** zum Aktivieren der automatischen Benutzerbereitstellung die Anmeldeinformationen eines DocuSign-Kontos mit ausreichenden Rechten ein, und klicken Sie auf **Weiter**. 
   
    ![Konfigurieren der Kontobereitstellung][31]
3. Klicken Sie im Dialogfeld **Verbindung testen** auf **Test starten**, und klicken Sie auf **Weiter**, wenn der Test erfolgreich ist.
   
    ![Konfigurieren der Kontobereitstellung][32]
4. Klicken Sie auf der **Bestätigung**-Seite auf **Abschließen**.
   
    ![Konfigurieren der Kontobereitstellung][33]

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-docusign-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, um DocuSign Benutzer zuzuweisen:
1. Erstellen Sie im **klassischen Azure-Portal**ein Testkonto.
2. Klicken Sie auf der **Anwendungsintegrationsseite für DocuSign** auf **Benutzer zuweisen**.
   
    ![Zuweisen von Benutzern][40]
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
    ![Zuweisen von Benutzern][41]

Nach 10 Minuten können Sie überprüfen, ob das Konto mit DocuSign synchronisiert wurde.

Als ersten Überprüfungsschritt können Sie den Bereitstellungsstatus überprüfen, indem Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für DocuSign auf „Dashboard“ klicken.

![Zuweisen von Benutzern][42]

Ein erfolgreich abgeschlossener Benutzerbereitstellungszyklus wird durch einen entsprechenden Status angezeigt:

![Zuweisen von Benutzern][43]

Wenn Sie die Einstellungen für einmaliges Anmelden testen möchten, öffnen Sie den Zugriffsbereich.

Weitere Informationen zum Zugriffsbereich finden Sie unter „Einführung in den Zugriffsbereich“.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[0]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_00.png
[1]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_01.png
[6]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_02.png
[7]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_03.png
[8]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_04.png
[9]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_05.png
[10]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_06.png

[14]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_10.png
[15]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_11.png

[30]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_400.png
[31]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_12.png
[32]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_13.png
[33]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_14.png



[40]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_15.png
[41]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_16.png
[42]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_17.png
[43]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_18.png

[51]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_29.png

