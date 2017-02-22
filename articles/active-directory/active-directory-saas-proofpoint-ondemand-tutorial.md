---
title: 'Tutorial: Azure Active Directory-Integration mit Proofpoint on Demand | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Proofpoint on Demand konfigurieren.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 773e7f7d-ec31-411b-860d-6a6633335d43
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/05/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 7aa746b8c6f4b747a7dca0e9bf75e61b2707b7e1


---
# <a name="tutorial-azure-active-directory-integration-with-proofpoint-on-demand"></a>Tutorial: Azure Active Directory-Integration mit Proofpoint on Demand
In diesem Tutorial erfahren Sie, wie Sie Proofpoint on Demand in Azure Active Directory (Azure AD) integrieren.

Die Integration von Proofpoint on Demand in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf Proofpoint on Demand hat.
* Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Proofpoint on Demand anzumelden (einmaliges Anmelden, SSO).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit Proofpoint on Demand konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein Proofpoint on Demand-Abonnement, für das einmaliges Anmelden aktiviert ist

Beachten Sie beim Testen der Schritte in diesem Tutorial die folgenden Empfehlungen:

* Verwenden Sie keine Produktionsumgebung, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung.

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Proofpoint on Demand aus dem Katalog
2. Konfigurieren und testen Sie das einmalige Anmelden in Azure AD.

## <a name="add-proofpoint-on-demand-from-the-gallery"></a>Hinzufügen von Proofpoint on Demand aus dem Katalog
Um die Integration von Proofpoint on Demand in Azure AD zu konfigurieren, müssen Sie Proofpoint on Demand über den Katalog Ihrer Liste mit den verwalteten SaaS-Apps hinzufügen.

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
    ![Active Directory-Symbol][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht oben im Menü auf **ANWENDUNGEN**.
   
    ![Menüelement „ANWENDUNGEN“][2]
4. Klicken Sie unten auf der Seite auf **HINZUFÜGEN**.
   
    ![Schaltfläche „HINZUFÜGEN“][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Auswahl: Hinzufügen einer Anwendung aus dem Katalog][4]
6. Geben Sie im Suchfeld als Suchbegriff **Proofpoint on Demand** ein.
   
    ![Feld zum Eingeben von „Proofpoint on Demand“](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_01.png)
7. Wählen Sie im Ergebnisbereich **Proofpoint on Demand** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD
In diesem Abschnitt konfigurieren und testen Sie anhand eines Testbenutzers namens Britta Simon das einmalige Anmelden von Azure AD mit Proofpoint on Demand.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Proofpoint on Demand als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Sie müssen zwischen einem Azure AD-Benutzer und dem dazugehörigen Benutzer in Proofpoint on Demand eine Linkbeziehung einrichten.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD dem **Benutzernamen** in Proofpoint on Demand zuweisen.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden in Azure AD mit Proofpoint on Demand zu konfigurieren und zu testen:

1. [Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on), um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. [Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user), um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. [Erstellen eines Proofpoint on Demand-Testbenutzers](#creating-a-proofpoint-ondemand-test-user), um eine Entsprechung von Britta Simon in Proofpoint on Demand zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. [Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user), um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. [Testen des einmaligen Anmeldens](#testing-single-sign-on), um sicherzustellen, dass die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD
In diesem Abschnitt ermöglichen Sie das einmalige Anmelden von Azure AD im klassischen Portal und konfigurieren das einmalige Anmelden in Ihrer Proofpoint on Demand-Anwendung.

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Proofpoint on Demand** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Schaltfläche „Einmaliges Anmelden konfigurieren“][6]
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Proofpoint on Demand anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Optionsfeld „Microsoft Azure AD – einmaliges Anmelden“](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_03.png)
3. Führen Sie auf der Seite **App-Einstellungen konfigurieren** die folgenden Schritte aus:
   
    ![Seite „App-Einstellungen konfigurieren“ mit ausgefüllten Feldern](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_04.png)
   
    a. Geben Sie im Feld **ANMELDE-URL** die URL ein, unter der sich Benutzer an Ihrer Proofpoint on Demand-Anwendung anmelden. Verwenden Sie das folgende Muster: **https://\<hostname\>.pphosted.com/ppssamlsp_hostname**.
   
    b. Geben Sie im Feld **BEZEICHNER** die URL im folgenden Format ein: **https://\<hostname/>.pphosted.com/ppssamlsp**.
   
    c. Geben Sie im Feld **ANTWORT-URL** die URL im folgenden Format ein: **https://\<hostname/>.pphosted.com:portnummer/v1/samlauth/samlconsumer**.
   
    d. Klicken Sie auf **Weiter**.
4. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für Proofpoint on Demand** die folgenden Schritte aus:
   
    ![Seite „Einmaliges Anmelden konfigurieren für Proofpoint on Demand“ mit der Schaltfläche „Zertifikat herunterladen“](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_05.png)
   
    a. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.
   
    b. Klicken Sie auf **Weiter**.
5. Wenden Sie sich an das Supportteam von Proofpoint on Demand, um SSO (Single Sign-On, einmaliges Anmelden) für Ihre Anwendung konfigurieren zu lassen, und stellen Sie dazu Folgendes bereit:
   
    • Das heruntergeladene Zertifikat
   
    • Die Entitäts-ID
   
    • Die SAML-SSO-URL
6. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**.
   
    ![Kontrollkästchen zur Bestätigung, dass Sie das einmalige Anmelden konfiguriert haben][10]
7. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
   
    ![Bestätigungsseite][11]

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt erstellen Sie im klassischen Portal einen Testbenutzer mit dem Namen Britta Simon.

![Testbenutzer in der Liste mit den Benutzern][20]

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
    ![Active Directory-Symbol](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_09.png)
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie im Menü oben auf **BENUTZER**, um die Liste mit den Benutzern anzuzeigen.
   
    ![Menüelement „BENUTZER“](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_03.png)
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie unten in der Symbolleiste auf **BENUTZER HINZUFÜGEN**.
   
    ![Schaltfläche „BENUTZER HINZUFÜGEN“](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_04.png)
5. Führen Sie auf der Seite **Informationen über diesen Benutzer** die folgenden Schritte aus:  ![Seite „Informationen über diesen Benutzer“ mit ausgefüllten Feldern](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_05.png)
   
    a. Wählen Sie im Feld **Art des Benutzers** die Option **Neuer Benutzer in Ihrem Unternehmen**.
   
    b. Geben Sie in das Feld **BENUTZERNAME** den Namen **BrittaSimon** ein.
   
    c. Klicken Sie auf **Weiter**.
6. Führen Sie auf der Seite **Benutzerprofil** die folgenden Schritte aus: ![Seite „Benutzerprofil“ mit ausgefüllten Feldern](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_06.png)
   
   a. Geben Sie in das Feld **VORNAME** den Namen **Britta** ein.  
   
   b. Geben Sie in das Feld **NACHNAME** den Namen **Simon** ein.
   
   c. Geben Sie in das Feld **ANZEIGENAME** den Namen **Britta Simon** ein.
   
   d. Wählen Sie in der Liste **ROLLE** die Option **Benutzer** aus.
   
   e. Klicken Sie auf **Weiter**.
7. Klicken Sie auf der Seite **Temporäres Kennwort abrufen** auf **Erstellen**.
   
    ![Schaltfläche zum Erstellen eines temporären Kennworts](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_07.png)
8. Führen Sie auf der Seite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Seite „Vorübergehendes Kennwort abrufen“ mit Kennwortinformationen](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_08.png)
   
    a. Notieren Sie den Wert des Felds **NEUES KENNWORT**.
   
    b. Klicken Sie auf **Fertig stellen**.   

### <a name="create-a-proofpoint-on-demand-test-user"></a>Erstellen eines Proofpoint on Demand-Testbenutzers
In diesem Abschnitt erstellen Sie in Proofpoint on Demand einen Benutzer namens Britta Simon. Wenden Sie sich an das Supportteam von Proofpoint on Demand, um Benutzer der Proofpoint on Demand-Plattform hinzufügen zu lassen.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
In diesem Abschnitt ermöglichen Sie Britta Simon das einmalige Anmelden bei Azure, indem Sie ihr Zugriff auf Proofpoint on Demand gewähren.

![Benutzerinformationen: mit direkter Methode aktivierter Zugriff][200]

1. Klicken Sie in der Verzeichnisansicht des klassischen Portals oben im Menü auf **ANWENDUNGEN**, um die Anwendungsansicht zu öffnen.
   
    ![Menüelement „ANWENDUNGEN“][201]
2. Wählen Sie in der Liste mit den Anwendungen die Option **Proofpoint on Demand**.
   
    ![Anwendungsliste mit Auswahl von Proofpoint on Demand](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_50.png)
3. Klicken Sie oben im Menü auf **BENUTZER**.
   
    ![Menüelement „BENUTZER“][203]
4. Wählen Sie in der Liste mit den Benutzern den Eintrag **Britta Simon**.
5. Klicken Sie unten in der Symbolleiste auf **ZUWEISEN**.
   
    ![Schaltfläche „Zuweisen“][205]

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens
In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel **Proofpoint on Demand** klicken, sollten Sie automatisch an Ihrer Proofpoint on Demand-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_205.png



<!--HONumber=Dec16_HO2-->


