---
title: 'Tutorial: Azure Active Directory-Integration mit Fuse | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Fuse konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5ef34f58-863a-4b37-875c-e8efa3e18bb3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: afc9719a41e9ecb9a690edc7f16d392450fbb932
ms.contentlocale: de-de
ms.lasthandoff: 04/03/2017


---
# <a name="tutorial-azure-active-directory-integration-with-fuse"></a>Tutorial: Azure Active Directory-Integration mit Fuse

In diesem Tutorial erfahren Sie, wie Sie Fuse in Azure Active Directory (Azure AD) integrieren.

Die Integration von Fuse in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Fuse hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Fuse anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Verwaltungsportal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Fuse konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Fuse-Abonnement, für das einmaliges Anmelden (SSO) aktiviert ist


>[!NOTE]
>Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.
>
>

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden (SSO) von Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Fuse aus dem Katalog
2. Konfigurieren und Testen des einmaligen Anmeldens (SSO) von Azure AD

## <a name="add-fuse-from-the-gallery"></a>Hinzufügen von Fuse aus dem Katalog
Zum Konfigurieren der Integration von Fuse in Azure AD müssen Sie Fuse aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Fuse aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Verwaltungsportals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Hinzufügen**.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld als Suchbegriff **Fuse**ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_001.png)

5. Wählen Sie im Ergebnisbereich die Option **Fuse** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_0001.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden (SSO) von Azure AD bei Fuse basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit einmaliges Anmelden (SSO) funktioniert, muss Azure AD wissen, welcher Benutzer in Fuse als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Fuse muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Benutzernamen**** in Azure AD als Benutzernamen**** in Fuse zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens (SSO) in Azure AD bei Fuse sind die folgenden Bausteine erforderlich:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Fuse-Testbenutzers](#creating-a-fuse-test-user)** , um eine Entsprechung von Britta Simon in Fuse zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden (SSO) mit Azure AD im Azure-Verwaltungsportal und konfigurieren das einmalige Anmelden in Ihrer Fuse-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens (SSO) von Azure AD bei Fuse die folgenden Schritte aus:**

1. Klicken Sie im Azure-Verwaltungsportal auf der Anwendungsintegrationsseite für **Fuse** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_01.png)

3. Führen Sie die folgenden Schritte auf der Seite **Domäne und URLs für Fuse** aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_02.png)
  1. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<tenant name>.fusion-universal.com/`
  2. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<tenant name>.fusion-universal.com`

      >[!NOTE] 
      >Hierbei handelt es sich um Beispielwerte. Die Werte müssen durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner ersetzt werden. Wenden Sie sich an das [Supportteam von Fuse](mailto:support@fusion-universal.com), um diese Werte zu erhalten. 
      >
      >

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Neues Zertifikat erstellen**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_03.png)     

5. Klicken Sie im Dialogfeld **Neues Zertifikat erstellen** auf das Kalendersymbol, und wählen Sie ein **Ablaufdatum** aus. Klicken Sie auf die Schaltfläche **Speichern**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-fuse-tutorial/tutorial_general_300.png)

6. Wählen Sie im Abschnitt **SAML-Signaturzertifikat** die Option **Make new certificate active** (Neues Zertifikat zum aktiven Zertifikat machen), und klicken Sie auf die Schaltfläche **Speichern**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_04.png)

7. Klicken Sie im Popupfenster **Rolloverzertifikat** auf **OK**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-fuse-tutorial/tutorial_general_400.png)

8. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Rohdaten)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_05.png) 

9. Klicken Sie im Abschnitt **Fuse-Konfiguration** auf **Fuse konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_06.png) 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_07.png)

10. Wenden Sie sich an das [Fuse-Supportteam](mailto:support@fusion-universal.com), um SSO (Single Sign-On, einmaliges Anmelden) für Ihre Anwendung konfigurieren zu lassen, und stellen Sie Folgendes bereit: 

    • Die heruntergeladene **Zertifikatsdatei** • Die **SAML-Dienst-URL für einmaliges Anmelde** • Die **SAML-Entitäts-ID** • Die **Abmelde-URL**
  
### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt wird im Azure-Verwaltungsportal eine Testbenutzerin namens Britta Simon erstellt.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Verwaltungsportals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-fuse-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-fuse-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-fuse-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-fuse-tutorial/create_aaduser_04.png) 
  1. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.
  2. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.
  3. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.
  4. Klicken Sie auf **Erstellen**. 

### <a name="create-a-fuse-test-user"></a>Erstellen eines Fuse-Testbenutzers

In diesem Abschnitt erstellen Sie in Fuse einen Benutzer namens Britta Simon. Lassen Sie sich beim Hinzufügen der Benutzer ggf. vom [Fuse-Supportteam](mailto:support@fusion-universal.com) unterstützen.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Fuse gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon Fuse zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Verwaltungsportal die Anwendungsansicht, navigieren Sie dann zur Verzeichnisansicht, wechseln Sie zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste den Eintrag **Fuse**aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_50.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden (SSO) über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Fuse“ klicken, sollten Sie automatisch bei Ihrer Fuse-Anwendung angemeldet werden.


## <a name="additional-resources"></a>Weitere Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_203.png

