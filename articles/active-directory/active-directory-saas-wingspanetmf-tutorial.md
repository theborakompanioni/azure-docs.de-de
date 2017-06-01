---
title: 'Tutorial: Azure Active Directory-Integration mit Wingspan eTMF | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Wingspan eTMF konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ace320d3-521c-449c-992f-feabe7538de7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: de674af369080ad7eb608608685e293f2326c8e6
ms.openlocfilehash: 8c76fb64229abcad0cabb910e7c170979a79d839
ms.contentlocale: de-de
ms.lasthandoff: 05/04/2017


---
# <a name="tutorial-azure-active-directory-integration-with-wingspan-etmf"></a>Tutorial: Azure Active Directory-Integration mit Wingspan eTMF

In diesem Tutorial erfahren Sie, wie Sie Wingspan eTMF in Azure Active Directory (Azure AD) integrieren.

Die Integration von Wingspan eTMF in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Wingspan eTMF hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Wingspan eTMF anzumelden (einmaliges Anmelden, SSO).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Wingspan eTMF konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Wingspan eTMF-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Wingspan eTMF aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-wingspan-etmf-from-the-gallery"></a>Hinzufügen von Wingspan eTMF aus dem Katalog
Zum Konfigurieren der Integration von Wingspan eTMF in Azure AD müssen Sie Wingspan eTMF aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Wingspan eTMF aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld **Wingspan eTMF** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_search.png)

5. Wählen Sie im Ergebnisbereich die Option **Wingspan eTMF** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Wingspan eTMF mithilfe eines Testbenutzers mit dem Namen „Britta Simon“.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Wingspan eTMF als Gegenstück für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Wingspan eTMF muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als Wert des **Benutzernamens** in Wingspan eTMF zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Wingspan eTMF müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Wingspan eTMF-Testbenutzers](#creating-a-wingspan-etmf-test-user)**, um eine Entsprechung von „Britta Simon“ in Wingspan eTMF zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt ermöglichen Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Wingspan eTMF-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Wingspan eTMF die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Wingspan eTMF** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_samlbase.png)

3. Führen Sie die folgenden Schritte im Bereich **Domäne und URLs für Wingspan eTMF** aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_url11.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<customer name>.<instance name>.mywingspan.com/saml`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `http://saml.<instance name>.wingspan.com/shibboleth`

    c. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<customer name>.<instance name>.mywingspan.com/`
     
    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Aktualisieren Sie diese Werte mit der tatsächlichen Anmelde-, Bezeichner- und Antwort-URL einschließlich dem tatsächlichen Kunden- und Instanznamen. Wenden Sie sich an das [Supportteam von Wingspan eTMF](http://www.wingspan.com/contact-us/), um diese Werte zu erhalten. 

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_400.png)

6. Zum Konfigurieren des einmaligen Anmeldens bei **Wingspan eTMF** müssen Sie die heruntergeladene **Metadaten-XML**-Datei an das [Wingspan eTMF-Supportteam](http://www.wingspan.com/contact-us/) senden. Es führt die Einrichtung durch, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-wingspanetmf-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-wingspanetmf-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-wingspanetmf-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-wingspanetmf-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-wingspan-etmf-test-user"></a>Erstellen eines Wingspan eTMF-Testbenutzers

In diesem Abschnitt erstellen Sie in Wingspan eTMF einen Benutzer namens „Britta Simon“. Arbeiten Sie mit [Wingspan eTMF-Unterstützung](http://www.wingspan.com/contact-us/), um die Benutzer in der Wingspan eTMF-Anwendung hinzuzufügen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie „Britta Simon“ die Verwendung des einmaligen Anmeldens von Azure, indem Sie Zugriff auf Wingspan eTMF gewähren.

![Benutzer zuweisen][200] 

**Führen Sie die folgenden Schritte aus, um „Britta Simon“ Wingspan eTMF zuzuweisen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Wingspan eTMF**aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich. 

Klicken Sie im Zugriffsbereich auf die Kachel „Wingspan eTMF“. Sie werden auf die Anmeldeseite der Organisation weitergeleitet. Nach erfolgreicher Anmeldung werden Sie auch bei Ihrer Wingspan eTMF-Anwendung angemeldet. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://msdn.microsoft.com/library/dn308586).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_203.png


