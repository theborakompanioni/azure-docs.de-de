---
title: 'Lernprogramm: Azure Active Directory-Integration mit SciQuest Spend Director | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und HSciQuest Spend Director konfigurieren.
services: active-directory
documentationcenter: ''
author: jeevansd
manager: femila
editor: ''

ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/01/2016
ms.author: jeedes

---
# Lernprogramm: Azure Active Directory-Integration mit SciQuest Spend Director
Dieses Tutorial soll Ihnen zeigen, wie Sie SciQuest Spend Director in Azure Active Directory (Azure AD) integrieren können. Die Integration von SciQuest Spend Director in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer auf SciQuest Spend Director Zugriff hat.
* Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch für SciQuest Spend Director anzumelden (einmaliges Anmelden).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## Voraussetzungen
Um die Azure AD-Integration mit SciQuest Spend Director konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein SciQuest Spend Director-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.
> 
> 

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.

## Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden von Azure AD in einer Testumgebung zu testen. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von SciQuest Spend Director aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## Hinzufügen von SciQuest Spend Director aus dem Katalog
Zum Konfigurieren der Integration von SciQuest Spend Director in Azure AD müssen Sie SciQuest Spend Director aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

**Um SciQuest Spend Director aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.
   
    ![Anwendungen][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen**.
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]
6. Geben Sie im Suchfeld den Suchbegriff **SciQuest Spend Director** ein.
   
    ![Anwendungen][5]
7. Wählen Sie im Ergebnisbereich **SciQuest Spend Director** aus, und klicken Sie dann auf **Fertigstellen**, um die Anwendung hinzufügen.
   
    ![Anwendungen][6]

## Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt soll veranschaulicht werden, wie basierend auf einem Testbenutzer namens "Britta Simon" das einmalige Anmelden von Azure AD in SciQuest Spend Director konfiguriert und getestet werden kann.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, wer der entsprechende Gegenbenutzer in SciQuest Spend Director zu einem Benutzer in Azure AD ist. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in SciQuest Spend Director muss eine Linkbeziehung eingerichtet werden. Diese Linkbeziehung wird hergestellt, indem Sie den Wert des **Benutzernames** in Azure AD als Wert zum **Benutzernamen** in SciQuest Spend Director zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei SciQuest Spend Director müssen Sie die folgenden Bausteinen ausführen:

1. **[Konfigurieren von Azure AD – einmaliges Anmelden](#configuring-azure-ad-single-single-sign-on)** – um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines SciQuest Spend Director-Testbenutzers](#creating-a-halogen-software-test-user)** – um eine Entsprechung von Britta Simon in SciQuest Spend Director zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)** – um zu überprüfen, ob die Konfiguration funktioniert.

### Konfigurieren der einmaligen Anmeldung in Azure AD
Das Ziel dieses Abschnitts besteht darin, das einmalige Anmelden von Azure AD im klassischen Azure-Portal zu aktivieren und das einmalige Anmelden in Ihrer SciQuest Spend Director-Anwendung zu konfigurieren.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in SciQuest Spend Director die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite **SciQuest Spend Director** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][8]
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei SciQuest Spend Director anmelden** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Azure AD – einmaliges Anmelden][9]
3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:
   
    ![Konfigurieren von App-Einstellungen][10]
   
     3\.1. Geben Sie im Textfeld **Anmelde-URL** die URL ein, die von Ihren Benutzern nach folgendem Muster zur Anmeldung bei der SciQuest Spend Director verwendet wird: *https://.*sciquest.com/.**
   
     3\.2. Geben Sie im Textfeld **Antwort-URL** den gleichen Wert ein, den Sie in das Textfeld **Anmelde-URL** eingetippt haben.
   
     3\.3. Klicken Sie auf **Next**.
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für SciQuest Spend Director** auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei lokal auf Ihrem Computer.
   
    ![Was ist Azure AD Connect?][11]
5. Wenden Sie sich an den SciQuest-Support, um diese Authentifizierungsmethode mithilfe der oben genannten heruntergeladenen Metadaten zu aktivieren.
6. Wählen Sie im klassischen Azure-Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
    ![Was ist Azure AD Connect?][15]
7. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.

### Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt wird im klassischen Azure-Portal eine Testbenutzerin namens Britta Simon erstellt.

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Was ist Azure AD Connect?][100]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie im Menü oben auf **Benutzer**, um die Liste der Benutzer anzuzeigen.
   
    ![Was ist Azure AD Connect?][101]
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Was ist Azure AD Connect?][102]
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
   
    ![Was ist Azure AD Connect?][103]
   
    a. Wählen Sie als **Benutzertyp** die Option **Neuer Benutzer in Ihrer Organisation** aus.
   
    b. Geben Sie in das Textfeld **Benutzername** den Text **BrittaSimon** ein.
   
    c. Klicken Sie auf Weiter.
6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
   
   ![Was ist Azure AD Connect?][104]
   
   a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.
   
   b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
   
   c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
   
   d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
   
   e. Klicken Sie auf **Weiter**.
7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Was ist Azure AD Connect?][105]
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Was ist Azure AD Connect?][106]
   
    a. Notieren Sie den Wert von **Neues Kennwort**.
   
    b. Klicken Sie auf **Fertig stellen**.

### Erstellen eines SciQuest Spend Director-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in SciQuest Spend Director.

Wenden Sie sich an das Supportteam von SciQuest Spend Director, und geben sie dort die Details zu Ihrem Testkonto an, damit es erstellt wird.

Alternativ können Sie auch die Just-in-Time-Bereitstellung nutzen, ein Feature für einmaliges Anmelden, das von SciQuest Spend Director unterstützt wird. Wenn die Just-in-Time-Bereitstellung aktiviert ist, werden Benutzer von SciQuest Spend Director beim Versuch des einmaligen Anmeldens automatisch erstellt, wenn sie nicht vorhanden sind. Mit dieser Funktion ist es nicht mehr erforderlich, Gegenbenutzer für einmaliges Anmelden manuell zu erstellen.

Damit die Just-in-Time-Bereitstellung aktiviert wird, wenden Sie sich an das Supportteam von SciQuest Spend Director.

### Zuweisen des Azure AD-Testbenutzers
Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens von Azure zu ermöglichen, indem sie Zugriff auf SciQuest Spend Director erhält.

![Was ist Azure AD Connect?][200]

**Um Britta Simon SciQuest Spend Director zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Azure-Portal im oberen Menü der Verzeichnisansicht auf **Anwendungen**.
   
    ![Was ist Azure AD Connect?][201]
2. Wählen Sie in der Anwendungsliste **SciQuest Spend Director** aus.
   
    ![Was ist Azure AD Connect?][202]
3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Was ist Azure AD Connect?][203]
4. Wählen Sie in der Benutzerliste **Britta Simon** aus.
   
    ![Was ist Azure AD Connect?][204]
5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Was ist Azure AD Connect?][205]

### Testen der einmaligen Anmeldung
Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich. Wenn Sie auf die Kachel SciQuest Spend Director im Zugriffsbereich klicken, sollten Sie automatisch in Ihrer SciQuest Spend Director-Anwendung angemeldet werden.

## Weitere Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_01.png
[2]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_02.png
[3]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_03.png
[4]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_04.png
[5]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_01.png
[6]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_05.png
[8]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_06.png
[9]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_07.png
[10]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_08.png
[11]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_03.png
[15]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_04.png

[100]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_09.png
[101]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_10.png
[102]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_11.png
[103]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_12.png
[104]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_13.png
[105]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_14.png
[106]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_15.png
[200]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_16.png
[201]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_17.png
[202]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_06.png
[203]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_18.png
[204]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_19.png
[205]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_20.png

<!---HONumber=AcomDC_0907_2016-->