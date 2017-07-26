---
title: 'Tutorial: Azure Active Directory-Integration mit IdeaScale | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und IdeaScale konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e16dda6b-fdf9-43cc-9bbb-a523f085a8af
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: ef1e603ea7759af76db595d95171cdbe1c995598
ms.openlocfilehash: 88099e942319f16dd721da83e4e69b8fcb836c0d
ms.contentlocale: de-de
ms.lasthandoff: 06/16/2017


---
# <a name="tutorial-azure-active-directory-integration-with-ideascale"></a>Tutorial: Azure Active Directory-Integration mit IdeaScale

In diesem Tutorial erfahren Sie, wie Sie IdeaScale in Azure Active Directory (Azure AD) integrieren.

Die Integration von IdeaScale in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf IdeaScale hat.
- Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei IdeaScale anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit IdeaScale konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SSO-fähiges IdeaScale-Abonnement

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von IdeaScale aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-ideascale-from-the-gallery"></a>Hinzufügen von IdeaScale aus dem Katalog
Zum Konfigurieren der Integration von IdeaScale in Azure AD müssen Sie IdeaScale aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um IdeaScale aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld das Wort **IdeaScale** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_search.png)

5. Wählen Sie im Ergebnisbereich **IdeaScale** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei IdeaScale mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in IdeaScale als Pendant zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in IdeaScale muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in IdeaScale den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei IdeaScale müssen Sie die folgenden Schritte ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines IdeaScale-Testbenutzers](#creating-an-ideascale-test-user)**, um ein Pendant von Britta Simon in IdeaScale zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer IdeaScale-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in IdeaScale die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **IdeaScale** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für IdeaScale** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<companyname>.ideascale.com`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein:
    | |
    |--|
    | `http://<companyname>.ideascale.com`  |
    | `https://<companyname>.ideascale.com` |

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Clientsupportteam von IdeaScale](http://support.ideascale.com/), um diese Werte zu erhalten. 
 
4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ideascale-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **IdeaScale-Konfiguration** auf **IdeaScale konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie **Abmelde-URL und SAML-Entitäts-ID** aus dem **Abschnitt mit der Kurzübersicht**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_configure.png) 

7. Melden Sie sich in einem anderen Webbrowserfenster bei der IdeaScale-Unternehmenswebsite als Administrator an.

8. Navigieren Sie zu **Communityeinstellungen**.
   
    ![Communityeinstellungen](./media/active-directory-saas-ideascale-tutorial/ic790847.png "Communityeinstellungen")

9. Navigieren Sie zu **Sicherheit \> Einstellungen für einmaliges Anmelden**.
   
    ![Einstellungen für einmaliges Anmelden](./media/active-directory-saas-ideascale-tutorial/ic790848.png "Einstellungen für einmaliges Anmelden")

10. Wählen Sie für **SSO-Typ** die Einstellung **SAML 2.0** aus.
   
    ![SSO-Typ](./media/active-directory-saas-ideascale-tutorial/ic790849.png "SSO-Typ")

11. Führen Sie im Dialogfeld **Einstellungen für einmaliges Anmelden** die folgenden Schritte aus:
   
    ![Einstellungen für einmaliges Anmelden](./media/active-directory-saas-ideascale-tutorial/ic790850.png "Einstellungen für einmaliges Anmelden")
   
    a. Fügen Sie in das Textfeld **SAML-IdP-Entitäts-ID** den Wert der **SAML-Entitäts-ID** ein, den Sie aus dem Azure-Portal kopiert haben.

    b. Kopieren Sie den Inhalt der aus dem Azure-Portal heruntergeladenen Metadatendatei, und fügen Sie ihn in das Textfeld **SAML-IdP-Metadaten** ein.

    c. Fügen Sie in das Textfeld **Abmeldeerfolg-URL** den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    d. Klicken Sie auf **Änderungen speichern**.

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-ideascale-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-ideascale-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-ideascale-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-ideascale-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-an-ideascale-test-user"></a>Erstellen eines IdeaScale-Testbenutzers

Damit sich Azure AD-Benutzer bei IdeaScale anmelden können, müssen sie in IdeaScale bereitgestellt werden. Im Fall von IdeaScale ist die Bereitstellung eine manuelle Aufgabe.

**Um die Benutzerbereitstellung zu konfigurieren, führen Sie die folgenden Schritte durch:**

1. Melden Sie sich bei der **IdeaScale** -Unternehmenswebsite als Administrator an.

2. Navigieren Sie zu **Communityeinstellungen**.
   
    ![Communityeinstellungen](./media/active-directory-saas-ideascale-tutorial/ic790847.png "Communityeinstellungen")

3. Navigieren Sie zu **Grundlegende Einstellungen \> Mitgliederverwaltung**.

4. Klicken Sie auf **Mitglied hinzufügen**.
   
    ![Mitgliederverwaltung](./media/active-directory-saas-ideascale-tutorial/ic790852.png "Mitgliederverwaltung")

5. Führen Sie im Abschnitt „Neues Mitglied hinzufügen“ die folgenden Schritte aus:
   
    ![Neues Mitglied hinzufügen](./media/active-directory-saas-ideascale-tutorial/ic790853.png "Neues Mitglied hinzufügen")
   
    a. Geben Sie im Textfeld **E-Mail-Adressen** die E-Mail-Adresse eines gültigen AAD-Benutzerkontos ein, das Sie bereitstellen möchten.
   
    b. Klicken Sie auf **Änderungen speichern**. 
   
    >[!NOTE]
    >Der Besitzer des Azure Active Directory-Kontos erhält eine E-Mail mit einem Link zur Bestätigung des Kontos, bevor es aktiv wird.
      
>[!NOTE]
>Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von IdeaScale-Benutzerkonten oder mithilfe der von IdeaScale bereitgestellten APIs erstellen.
 

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf IdeaScale gewähren.

![Benutzer zuweisen][200] 

**Zum Zuweisen von Britta Simon zu IdeaScale führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **IdeaScale** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung


Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „IdeaScale“ klicken, sollten Sie automatisch bei Ihrer IdeaScale-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_203.png


