---
title: 'Tutorial: Azure Active Directory-Integration in ArcGIS Online | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und ArcGIS Online konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a9e132a4-29e7-48bf-beb9-4148e617c8a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: ef1e603ea7759af76db595d95171cdbe1c995598
ms.openlocfilehash: df72270ca6443b456c079b22425f1660aa522389
ms.contentlocale: de-de
ms.lasthandoff: 06/16/2017


---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-online"></a>Tutorial: Azure Active Directory-Integration in ArcGIS Online

In diesem Tutorial erfahren Sie, wie Sie Azure Active Directory (Azure AD) in ArcGIS Online integrieren.

Die Integration von Azure AD in ArcGIS Online bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf ArcGIS Online Zugriff hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei ArcGIS Online anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

<!--## Overview

To enable single sign-on with ArcGIS Online, it must be configured to use Azure Active Directory as an identity provider. This guide provides information and tips on how to perform this configuration in ArcGIS Online.

>[!Note]: 
>This embedded guide is brand new in the new Azure portal, and we’d love to hear your thoughts. Use the Feedback ? button at the top of the portal to provide feedback. The older guide for using the [Azure classic portal](https://manage.windowsazure.com) to configure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a>Voraussetzungen

Um die Integration von Azure AD in ArcGIS Online konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein ArcGIS Online-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von ArcGIS Online aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-arcgis-online-from-the-gallery"></a>Hinzufügen von ArcGIS Online aus dem Katalog
Zum Konfigurieren der Integration von Azure AD in ArcGIS Online müssen Sie Ihrer Liste der verwalteten SaaS-Apps ArcGIS Online aus dem Katalog hinzufügen.

**Um ArcGIS Online aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte durch:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld als Suchbegriff **ArcGIS Online** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_search.png)

5. Wählen Sie im Ergebnisbereich **ArcGIS Online** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei ArcGIS Online mithilfe eines Testbenutzers namens „Britta Simon“.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in ArcGIS Online als Pendant für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in ArcGIS Online muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** in ArcGIS Online zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD in ArcGIS Online müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines ArcGIS Online-Testbenutzers](#creating-an-arcgis-online-test-user)**, um eine Entsprechung von Britta Simon in ArcGIS Online zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden mit Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer ArcGIS Online-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in ArcGIS Online die folgenden Schritte durch:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **ArcGIS Online** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für ArcGIS Online** die folgenden Schritte durch:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_url.png)

    Geben Sie im Textfeld **Anmelde-URL** den Wert im folgenden Format ein: `https://<company>.maps.arcgis.com`.

    > [!NOTE] 
    > Dieser Wert entspricht nicht dem tatsächlichen Wert. Ersetzen Sie diesen Wert durch die tatsächliche Anmelde-URL. Wenden Sie sich an das [Supportteam für den ArcGIS Online-Client](http://support.esri.com/), um diesen Wert zu erhalten. 

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die XML-Datei dann auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-arcgis-tutorial/tutorial_general_400.png)

6. Melden Sie sich in einem anderen Webbrowserfenster bei der ArcGIS-Unternehmenswebsite als Administrator an.

7. Klicken Sie auf **EINSTELLUNGEN BEARBEITEN**.

    ![Einstellungen bearbeiten](./media/active-directory-saas-arcgis-tutorial/ic784742.png "Einstellungen bearbeiten")

8. Klicken Sie auf **Sicherheit**.

    ![Sicherheit](./media/active-directory-saas-arcgis-tutorial/ic784743.png "Sicherheit")

9. Klicken Sie unter **Unternehmensanmeldungen** auf **IDENTITÄTSANBIETER FESTLEGEN**.

    ![Unternehmensanmeldungen](./media/active-directory-saas-arcgis-tutorial/ic784744.png "Unternehmensanmeldungen")

10. Führen Sie auf der Konfigurationsseite **Set Identity Provider** die folgenden Schritte aus.
   
    ![Identitätsanbieter festlegen](./media/active-directory-saas-arcgis-tutorial/ic784745.png "Identitätsanbieter festlegen")
   
    a. Geben Sie in das Textfeld **Name** den Namen Ihrer Organisation ein.

    b. Wählen Sie für **Metadaten für Unternehmensidentitätsanbieter werden bereitgestellt durch** die Option **eine Datei** aus.

    c. Klicken Sie auf **Datei auswählen**, um die heruntergeladene Metadatendatei hochzuladen.

    d. Klicken Sie auf **IDENTITÄTSANBIETER FESTLEGEN**.

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).


### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-arcgis-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-arcgis-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-arcgis-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-arcgis-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-an-arcgis-online-test-user"></a>Erstellen eines ArcGIS Online-Testbenutzers

Damit sich Azure AD-Benutzer bei ArcGIS Online anmelden können, müssen sie in ArcGIS Online bereitgestellt werden.  
Im Fall von ArcGIS Online ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei Ihrem **ArcGIS** -Mandanten an.

2. Klicken Sie auf **MITGLIEDER EINLADEN**.
   
    ![Mitglieder einladen](./media/active-directory-saas-arcgis-tutorial/ic784747.png "Mitglieder einladen")

3. Wählen Sie **Mitglieder automatisch hinzufügen, ohne eine E-Mail zu senden** aus, und klicken Sie dann auf **WEITER**.
   
    ![Mitglieder automatisch hinzufügen](./media/active-directory-saas-arcgis-tutorial/ic784748.png "Mitglieder automatisch hinzufügen")

4. Führen Sie auf der Dialogfeldseite **Members** die folgenden Schritte aus:
   
     ![Hinzufügen und überprüfen Sie](./media/active-directory-saas-arcgis-tutorial/ic784749.png "hinzufügen und überprüfen")
    
     a. Geben Sie **E-Mail-Adresse**, **Vorname** und **Nachname** eines gültigen AAD-Kontos ein, das Sie bereitstellen möchten.
  
     b. Klicken Sie auf **HINZUFÜGEN UND ÜBERPRÜFEN**.
5. Überprüfen Sie die eingegebenen Daten, und klicken Sie dann auf **MITGLIEDER HINZUFÜGEN**.
   
    ![Mitglied hinzufügen](./media/active-directory-saas-arcgis-tutorial/ic784750.png "Mitglied hinzufügen")
        
    > [!NOTE]
    > Der Besitzer des Azure Active Directory-Kontos erhält eine E-Mail und folgt einem Link zur Bestätigung des Kontos, bevor es aktiv wird.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf ArcGIS Online gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon zu ArcGIS Online zuzuweisen, führen Sie die folgenden Schritte durch:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **ArcGIS Online** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „ArcGIS Online“ klicken, sollten Sie automatisch bei Ihrer ArcGIS Online-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_203.png


