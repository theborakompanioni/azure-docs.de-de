---
title: 'Tutorial: Azure Active Directory-Integration mit Gigya | Microsoft-Dokumentation'
description: "Hier erfahren Sie, wie Sie Gigya mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 2c7d200b-9242-44a5-ac8a-ab3214a78e41
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/14/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: bf5588885de9c280eb70712dbf800efe509ee912
ms.openlocfilehash: 96eb66f35a91e769418d179b0997cd32e02cdc97


---
# <a name="tutorial-azure-active-directory-integration-with-gigya"></a>Tutorial: Azure Active Directory-Integration mit Gigya
In diesem Tutorial wird die Integration von Azure und Gigya erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein Gigya-Abonnement, für das einmaliges Anmelden aktiviert ist

Nach Abschluss dieses Tutorials können sich die Azure AD-Benutzer, die Sie Gigya zugewiesen haben, mittels einmaligen Anmeldens auf der Gigya-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Gigya
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-gigya-tutorial/IC789512.png "Einmaliges Anmelden konfigurieren")

## <a name="enabling-the-application-integration-for-gigya"></a>Aktivieren der Anwendungsintegration für Gigya
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Gigya aktivieren.

### <a name="to-enable-the-application-integration-for-gigya-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Gigya
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-gigya-tutorial/IC700993.png "Active Directory")

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen](./media/active-directory-saas-gigya-tutorial/IC700994.png "Anwendungen")

4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendung hinzufügen](./media/active-directory-saas-gigya-tutorial/IC749321.png "Anwendung hinzufügen")

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-gigya-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")

6. Geben Sie im **Suchfeld** das Wort **Gigya** ein.
   
    ![Anwendungskatalog](./media/active-directory-saas-gigya-tutorial/IC789513.png "Anwendungskatalog")

7. Wählen Sie im Ergebnisbereich **Gigya** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Gigya](./media/active-directory-saas-gigya-tutorial/IC789527.png "Gigya")
   
## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Gigya zu authentifizieren.  
Im Rahmen dieses Verfahrens müssen Sie eine Base-64-codierte Zertifikatsdatei erstellen.  
Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)(Konvertieren eines binären Zertifikats in eine Textdatei; in englischer Sprache) weitere Informationen.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Gigya** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-gigya-tutorial/IC789528.png "Einmaliges Anmelden konfigurieren")

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Gigya anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-gigya-tutorial/IC789529.png "Einmaliges Anmelden konfigurieren")

3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **Gigya-Anmelde-URL** Ihre URL im Format *http://Unternehmen.gigya.com* ein, und klicken Sie dann auf **Weiter**.
   
    ![App-URL konfigurieren](./media/active-directory-saas-gigya-tutorial/IC789530.png "App-URL konfigurieren")

4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Gigya** auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei auf Ihrem Computer.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-gigya-tutorial/IC789531.png "Einmaliges Anmelden konfigurieren")

5. Melden Sie sich in einem anderen Webbrowserfenster bei der Gigya-Unternehmenswebsite als Administrator an.

6. Navigieren Sie zu **Einstellungen \> SAML-Anmeldung**, und klicken Sie dann auf die Schaltfläche **Hinzufügen**.
   
    ![SAML-Anmeldung](./media/active-directory-saas-gigya-tutorial/IC789532.png "SAML-Anmeldung")

7. Führen Sie im Abschnitt **SAML-Anmeldung** die folgenden Schritte aus:
   
    ![SAML-Konfiguration](./media/active-directory-saas-gigya-tutorial/IC789533.png "SAML-Konfiguration")
   
    a. Geben Sie im Textfeld **Name** einen Namen für die Konfiguration ein.
   
    b. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Gigya** den Wert für **Aussteller-URL**, und fügen Sie ihn in das Textfeld **Aussteller** ein.
   
    c. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Gigya** den Wert für **Dienst-URL für einmaliges Anmelden**, und fügen Sie ihn in das Textfeld **Dienst-URL für einmaliges Anmelden** ein.
   
    d. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Gigya** den Wert für **Namensbezeichnerformat**, und fügen Sie ihn in das Textfeld **Namensbezeichnerformat** ein.
   
    e. Erstellen Sie eine **Base64-codierte** Datei aus dem heruntergeladenen Zertifikat.
      
    > [!TIP]
    > Weitere Informationen finden Sie unter [Konvertieren eines binären Zertifikats in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)
    > 
    > 
   
    f. Öffnen Sie das Base-64-codierte Zertifikat im Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **X.509-Zertifikat** ein.
   
    g. Klicken Sie auf **Einstellungen speichern**.

8. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-gigya-tutorial/IC789534.png "Einmaliges Anmelden konfigurieren")
   
## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich Azure AD-Benutzer bei Gigya anmelden können, müssen sie in Gigya bereitgestellt werden.  
Im Fall von Gigya ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Führen Sie zum Bereitstellen von Benutzerkonten die folgenden Schritte aus:
1. Melden Sie sich bei der **Gigya** -Unternehmenswebsite als Administrator an.
2. Navigieren Sie zu **Administrator \> Benutzer verwalten**, und klicken Sie dann auf **Benutzer einladen**.
   
    ![Benutzer verwalten](./media/active-directory-saas-gigya-tutorial/IC789535.png "Benutzer verwalten")

3. Führen Sie im Dialogfeld „Benutzer einladen“ die folgenden Schritte aus:
   
    ![Benutzer einladen](./media/active-directory-saas-gigya-tutorial/IC789536.png "Benutzer einladen")
   
    a. Geben Sie im Textfeld **E-Mail** die E-Mail-Adresse eines gültigen Azure Active Directory-Kontos ein, das Sie bereitstellen möchten.
    
    b. Klicken Sie auf **Benutzer einladen**.
      
    > [!NOTE]
    > Der Besitzer des Azure Active Directory-Kontos erhält eine E-Mail mit einem Link zur Bestätigung des Kontos, bevor es aktiv wird.
    > 
    > 

 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-gigya-perform-the-following-steps"></a>So weisen Sie Gigya Benutzer zu
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Gigya** auf **Benutzer zuweisen**.
   
    ![Zuweisen von Benutzern](./media/active-directory-saas-gigya-tutorial/IC789537.png "Zuweisen von Benutzern")

3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
    ![Ja](./media/active-directory-saas-gigya-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Feb17_HO2-->


