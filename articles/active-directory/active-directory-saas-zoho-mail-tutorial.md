<properties 
    pageTitle="Tutorial: Azure Active Directory-Integration mit Zoho Mail | Microsoft Azure" 
    description="Hier erfahren Sie, wie Sie Zoho Mail mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen." 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="09/09/2016" 
    ms.author="markvi" />

#Tutorial: Azure Active Directory-Integration mit Zoho Mail
  
In diesem Tutorial wird die Integration von Azure und Zoho Mail erläutert. Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

-   Ein gültiges Azure-Abonnement
-   Einen Zoho Mail-Mandanten
  
Nach Abschluss dieses Tutorials können sich die Zoho Mail zugewiesenen Azure AD-Benutzer mittels einmaliger Anmeldung auf Ihrer Zoho Mail-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).
  
Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1.  Aktivieren der Anwendungsintegration für Zoho Mail
2.  Konfigurieren der einmaligen Anmeldung
3.  Konfigurieren der Benutzerbereitstellung
4.  Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-zoho-mail-tutorial/IC789600.png "Szenario")

##Aktivieren der Anwendungsintegration für Zoho Mail
  
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Zoho Mail aktivieren.

###So aktivieren Sie die Anwendungsintegration für Zoho Mail:

1.  Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory](./media/active-directory-saas-zoho-mail-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.

    ![Anwendungen](./media/active-directory-saas-zoho-mail-tutorial/IC700994.png "Anwendungen")

4.  Klicken Sie unten auf der Seite auf **Hinzufügen**.

    ![Anwendung hinzufügen](./media/active-directory-saas-zoho-mail-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.

    ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-zoho-mail-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")

6.  Geben Sie im **Suchfeld** **Zoho Mail** ein.

    ![Anwendungskatalog](./media/active-directory-saas-zoho-mail-tutorial/IC789601.png "Anwendungskatalog")

7.  Wählen Sie im Ergebnisbereich **Zoho Mail** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

    ![Zoho Mail](./media/active-directory-saas-zoho-mail-tutorial/IC789602.png "Zoho Mail")

##Konfigurieren der einmaligen Anmeldung
  
In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Zoho Mail zu authentifizieren. Im Rahmen dieses Verfahrens müssen Sie eine Base-64-codierte Zertifikatsdatei erstellen. Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o) (Konvertieren eines binären Zertifikats in eine Textdatei; in englischer Sprache) weitere Informationen.

###So konfigurieren Sie einmaliges Anmelden

1.  Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Zoho Mail** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zoho-mail-tutorial/IC789603.png "Einmaliges Anmelden konfigurieren")

2.  Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Zoho Mail anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zoho-mail-tutorial/IC789604.png "Einmaliges Anmelden konfigurieren")

3.  Führen Sie auf der Seite **App-URL konfigurieren** die folgenden Schritte aus:

    ![App-URL konfigurieren](./media/active-directory-saas-zoho-mail-tutorial/IC789605.png "App-URL konfigurieren")

	a. Geben Sie im Textfeld **Zoho Mail-Anmelde-URL** die URL in folgendem Format ein: `http://<company name>.ZohoMail.com`

	b. Klicken Sie auf **Next**.


4.  Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Zoho Mail** auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zoho-mail-tutorial/IC789606.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie sich in einem anderen Webbrowserfenster bei der Zoho Mail-Unternehmenswebsite als Administrator an.

6.  Wechseln Sie zur **Systemsteuerung**.

    ![Systemsteuerung](./media/active-directory-saas-zoho-mail-tutorial/IC789607.png "Systemsteuerung")

7.  Klicken Sie auf die Registerkarte **SAML-Authentifizierung**.

    ![SAML-Authentifizierung](./media/active-directory-saas-zoho-mail-tutorial/IC789608.png "SAML-Authentifizierung")

8.  Führen Sie im Abschnitt für die **SAML-Authentifizierungsdetails** die folgenden Schritte aus:

    ![SAML-Authentifizierungsdetails](./media/active-directory-saas-zoho-mail-tutorial/IC789609.png "SAML-Authentifizierungsdetails")

    1.  Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Zoho Mail** den Wert für **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **Anmelde-URL** ein.
    2.  Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Zoho Mail** den Wert für **Remoteabmelde-URL**, und fügen Sie ihn in das Textfeld **Abmelde-URL** ein.
    3.  Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Zoho Mail** den Wert für **Kennwort-URL ändern**, und fügen Sie ihn in das Textfeld **Kennwort-URL ändern** ein.
    4.  Erstellen Sie eine **Base64-codierte** Datei aus dem heruntergeladenen Zertifikat.

        >[AZURE.TIP] Weitere Informationen finden Sie unter [Konvertieren eines binären Zertifikats in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o).

    5.  Öffnen Sie das Base-64-codierte Zertifikat im Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **Öffentlicher Schlüssel** ein.
    6.  Wählen Sie als **Algorithmus** **RSA**.
    7.  Klicken Sie auf **OK**.

9.  Wählen Sie im klassischen Azure-Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zoho-mail-tutorial/IC789610.png "Einmaliges Anmelden konfigurieren")

##Konfigurieren der Benutzerbereitstellung
  
Damit sich Azure AD-Benutzer bei Zoho Mail anmelden können, müssen sie in Zoho Mail bereitgestellt werden. Im Fall von Zoho Mail ist die Bereitstellung eine manuelle Aufgabe.

###Führen Sie zum Bereitstellen von Benutzerkonten die folgenden Schritte aus:

1.  Melden Sie sich bei der **Zoho Mail**-Unternehmenswebsite als Administrator an.

2.  Wechseln Sie zu **Systemsteuerung > E-Mails und Dokumente**.

3.  Wechseln Sie zu **Benutzerdetails > Benutzer hinzufügen**.

    ![Benutzer hinzufügen](./media/active-directory-saas-zoho-mail-tutorial/IC789611.png "Benutzer hinzufügen")

4.  Führen Sie im Dialogfeld **Benutzer hinzufügen** die folgenden Schritte aus:

    ![Benutzer hinzufügen](./media/active-directory-saas-zoho-mail-tutorial/IC789612.png "Benutzer hinzufügen")

    1.  Geben Sie **Vorname**, **Nachname**, **E-Mail-ID** und **Kennwort** eines gültigen Azure Active Directory-Kontos, das Sie bereitstellen möchten, in die entsprechenden Textfelder ein.
    2.  Klicken Sie auf **OK**.

        >[AZURE.NOTE] Der Besitzer des Azure Active Directory-Kontos erhält eine E-Mail mit einem Link zur Bestätigung des Kontos, bevor es aktiv wird.

>[AZURE.NOTE] Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Zoho Mail-Benutzerkonten oder mithilfe der von Zoho Mail bereitgestellten APIs erstellen.

##Zuweisen von Benutzern
  
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

###So weisen Sie Zoho Mail Benutzer zu:

1.  Erstellen Sie im klassischen Azure-Portal ein Testkonto.

2.  Klicken Sie auf der Anwendungsintegrationsseite für **Zoho Mai**l auf **Benutzer zuweisen**.

    ![Benutzer zuweisen](./media/active-directory-saas-zoho-mail-tutorial/IC789613.png "Benutzer zuweisen")

3.  Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.

    ![Ja](./media/active-directory-saas-zoho-mail-tutorial/IC767830.png "Ja")
  
Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

<!---HONumber=AcomDC_0914_2016-->