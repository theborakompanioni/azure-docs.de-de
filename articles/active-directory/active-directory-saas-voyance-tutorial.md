---
title: 'Tutorial: Azure Active Directory-Integration mit Voyance | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Voyance konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 539dc1f9-64c9-4dce-b259-2b0b49dcf857
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/09/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: 0746f010e8d001a66f8de0a32ea0147774da657b
ms.lasthandoff: 03/28/2017


---
# <a name="tutorial-azure-active-directory-integration-with-voyance"></a>Tutorial: Azure Active Directory-Integration mit Voyance

In diesem Tutorial erfahren Sie, wie Sie Voyance in Azure Active Directory (Azure AD) integrieren.

Die Integration von Voyance in Azure AD bietet folgende Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Voyance haben soll.
- Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Voyance anzumelden (einmaliges Anmelden, SSO).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Voyance konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Voyance-Abonnement, für das einmaliges Anmelden aktiviert ist

>[!NOTE]
>Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.
>

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Voyance über den Katalog
2. Konfigurieren und Testen des einmaligen Anmeldens (SSO) von Azure AD

## <a name="add-voyance-from-the-gallery"></a>Hinzufügen von Voyance aus dem Katalog
Zum Konfigurieren der Integration von Voyance in Azure AD müssen Sie Voyance über den Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um Voyance über den Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .

    ![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen** .

    ![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.

    ![Anwendungen][4]

6. Geben Sie im Suchfeld die Zeichenfolge **Voyance** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_01.png)

7. Wählen Sie im Ergebnisbereich die Option **Voyance** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_0001.png)


## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden (SSO) von Azure AD mit Voyance basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Voyance als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Voyance muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Benutzernamen****in Azure AD dem Benutzernamen****in Voyance zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Voyance müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Voyance-Testbenutzers](#creating-a-voyance-test-user)**, um in Voyance einen Gegenpart von Britta Simon zu erhalten, der mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt ermöglichen Sie das einmalige Anmelden von Azure AD im klassischen Portal und konfigurieren es in Ihrer Voyance-Anwendung.


**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD mit Voyance die folgenden Schritte aus:**

1. Klicken Sie im klassischen Portal auf der Anwendungsintegrationsseite für **Voyance** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.

    ![Einmaliges Anmelden konfigurieren][6]

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Voyance anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_02.png)

3. Führen Sie auf der Dialogfeldseite **App-Einstellungen konfigurieren** die folgenden Schritte aus, wenn Sie die Anwendung im **IdP-initiierten Modus** konfigurieren möchten, und klicken Sie dann auf **Weiter**:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_03.png)
  1. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<company name>.nyansa.com`.
  2. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<company name>.nyansa.com/saml/create/`.
  3. Klicken Sie auf **Weiter**.

4. Wenn die Anwendung im **SP-initiierten Modus** konfiguriert werden soll, klicken Sie auf der Dialogfeldseite **App-Einstellungen konfigurieren** auf **Zeigen Sie die erweiterten Einstellungen an (optional)**, geben Sie die **Anmelde-URL** ein, und klicken Sie dann auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_04.png)
  1. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<company name>.nyansa.com/`.
  2. Klicken Sie auf **Weiter**.

      >[!NOTE]
      >Sie müssen diese Werte durch die tatsächliche Anmelde-URL, den tatsächlichen Bezeichner und die tatsächliche Antwort-URL ersetzen. Wenden Sie sich an das [Supportteam von Voyance](emaiLto:support@nyansa.com), um diese Werte zu erhalten.
      >

5. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Voyance** auf **Zertifikat herunterladen**, und speichern Sie die Datei auf Ihrem Computer:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_05.png) 

6. Melden Sie sich in einem anderen Webbrowserfenster bei Ihrem Voyance-Mandanten als Administrator an.

7. Klicken Sie in der rechten oberen Ecke der Navigationsleiste auf die Dropdownliste **Acme University**.
    
    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_001.png) 

8. Klicken Sie auf **Administratoreinstellungen**.

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_002.png)

9. Klicken Sie auf die Registerkarte **Benutzerzugriff**.

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_003.png)

10. Klicken Sie auf die Schaltfläche **SSO is disabled** (SSO ist deaktiviert), um Azure AD als IdP mit SAML 2.0 zu konfigurieren.

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_004.png)

11. Navigieren Sie zum Abschnitt **SAML v2**, und führen Sie folgende Schritte aus:

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_005.png)
 1. Wählen Sie **Aktiviert**.
 2. Geben Sie im Textfeld **IdP Login URL** (IdP-Anmelde-URL) den Wert der **SAML-SSO-URL** aus dem Konfigurations-Assistenten der Azure AD-Anwendung ein.
 3. Öffnen Sie das Base64-codierte Zertifikat im Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **IdP Cert** (IdP-Zertifikat) ein.
 4. Klicken Sie auf **Speichern**.

12. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration der einmaligen Anmeldung aus, und klicken Sie dann auf **Weiter**.

    ![Azure AD – einmaliges Anmelden][10]

13. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
  
    ![Azure AD – einmaliges Anmelden][11]


### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im klassischen Azure-Portal.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-voyance-tutorial/create_aaduser_09.png) 

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-voyance-tutorial/create_aaduser_03.png) 

4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-voyance-tutorial/create_aaduser_04.png) 

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-voyance-tutorial/create_aaduser_05.png) 
 1. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.
 2. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
 3. Klicken Sie auf **Weiter**.

6.  Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-voyance-tutorial/create_aaduser_06.png) 
 1. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  
 2. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
 3. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
 4. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
 5. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-voyance-tutorial/create_aaduser_07.png) 

8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-voyance-tutorial/create_aaduser_08.png) 
 1. Notieren Sie den Wert von **Neues Kennwort**.
 2. Klicken Sie auf **Fertig stellen**.   

### <a name="create-a-voyance-test-user"></a>Erstellen eines Voyance-Testbenutzers

In diesem Abschnitt wird in Voyance ein Benutzer namens Britta Simon erstellt. Voyance unterstützt die standardmäßig aktivierte Just-in-Time-Bereitstellung.

Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Ist noch kein Benutzer vorhanden, wird beim Zugreifen auf Voyance ein neuer Benutzer erstellt.

>[!NOTE]
>Setzen Sie sich mit dem [Supportteam von Voyance](emaiLto:support@nyansa.com) in Verbindung, wenn Sie einen Benutzer manuell erstellen müssen.
>

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Voyance gewähren.

![Benutzer zuweisen][200] 

**Führen Sie die folgenden Schritte aus, um die Zuweisung von Britta Simon zu Voyance durchzuführen:**

1. Klicken Sie in der Verzeichnisansicht des klassischen Portals auf der oberen Menüleiste auf **Anwendungen** , um die Anwendungsansicht zu öffnen.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste die Option **Voyance** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_50.png) 

3. Klicken Sie im oberen Menü auf **Benutzer**.

    ![Benutzer zuweisen][203] 

4. Wählen Sie in der Benutzerliste **Britta Simon**aus.

5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
    
    ![Benutzer zuweisen][205]


### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Voyance-Kachel klicken, sollten Sie automatisch bei Ihrer Voyance-Anwendung angemeldet werden.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_205.png

