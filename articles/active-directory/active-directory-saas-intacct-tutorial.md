---
title: 'Tutorial: Azure Active Directory-Integration mit Intacct | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie Intacct mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 92518e02-a62c-4b1b-a8e9-2803eb2b49ac
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/02/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 20db13c5ba67fcc402db5c692842719dd9a34eb1
ms.openlocfilehash: 0ea77d5f9c5b139ce6f87bc0cdd817166b03c22e
ms.lasthandoff: 03/01/2017

---

# <a name="tutorial-azure-active-directory-integration-with-intacct"></a>Tutorial: Azure Active Directory-Integration mit Intacct
In diesem Tutorial wird die Integration von Azure und Intacct erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Einen Intacct-Mandanten

Nach Abschluss dieses Tutorials können sich die Azure Active Directory (Azure AD)-Benutzer, die Sie Intacct zugewiesen haben, auf Ihrer Intacct-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung) oder den [Zugriffsbereich](active-directory-saas-access-panel-introduction.md) nutzen.

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

* Aktivieren der Anwendungsintegration für Intacct
* Konfigurieren der einmaligen Anmeldung
* Konfigurieren der Benutzerbereitstellung
* Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-intacct-tutorial/IC790030.png "Szenario")

## <a name="enable-the-application-integration-for-intacct"></a>Aktivieren der Anwendungsintegration für Intacct
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Intacct aktivieren.

**Führen Sie zum Aktivieren der Anwendungsintegration für Intacct die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des klassischen Azure-Portals auf das Active Directory-Symbol.

   ![Active Directory](./media/active-directory-saas-intacct-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.

   ![Anwendungen](./media/active-directory-saas-intacct-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .

   ![Anwendung hinzufügen](./media/active-directory-saas-intacct-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie auf der Seite **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.

   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-intacct-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im Suchfeld **Intacct** ein.

   ![Anwendungskatalog](./media/active-directory-saas-intacct-tutorial/IC790031.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **Intacct** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

   ![Intacct](./media/active-directory-saas-intacct-tutorial/IC790032.png "Intacct")

## <a name="configure-single-sign-on"></a>Einmaliges Anmelden konfigurieren

In diesem Abschnitt wird erläutert, wie Sie Benutzern ermöglichen, sich mit ihrem Azure AD-Konto bei Intacct zu authentifizieren. Verbund, der auf dem für diese Authentifizierung verwendeten SAML-Protokoll basiert.  

Im Rahmen dieses Verfahrens müssen Sie eine Base64-codierte Zertifikatsdatei erstellen. Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)(Konvertieren eines binären Zertifikats in eine Textdatei; in englischer Sprache) weitere Informationen.

**So konfigurieren Sie einmaliges Anmelden**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Intacct** auf **Einmaliges Anmelden konfigurieren**, um die Seite **Einmaliges Anmelden konfigurieren** zu öffnen.

   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-intacct-tutorial/IC790033.png "Einmaliges Anmelden konfigurieren")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Intacct anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.

   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-intacct-tutorial/IC790034.png "Einmaliges Anmelden konfigurieren")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Feld **Anmelde-URL** Ihre URL mit dem Muster *https://Intacct.com/company* ein, und klicken Sie dann auf **Weiter**.

   ![App-URL konfigurieren](./media/active-directory-saas-intacct-tutorial/IC790035.png "App-URL konfigurieren")
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Intacct** auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei auf Ihrem Computer.

   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-intacct-tutorial/IC790036.png "Einmaliges Anmelden konfigurieren")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der Intacct-Unternehmenswebsite als Administrator an.
6. Klicken Sie auf die Registerkarte **Unternehmen** und dann auf **Unternehmensinformationen**.

   ![Unternehmen](./media/active-directory-saas-intacct-tutorial/IC790037.png "Unternehmen")
7. Klicken Sie auf die Registerkarte **Sicherheit** und dann auf **Bearbeiten**.

   ![Sicherheit](./media/active-directory-saas-intacct-tutorial/IC790038.png "Sicherheit")
8. Führen Sie im Abschnitt **Einmaliges Anmelden (SSO)** die folgenden Schritte aus:

   ![Einmaliges Anmelden](./media/active-directory-saas-intacct-tutorial/IC790039.png "Einmaliges Anmelden")

   1. Wählen Sie **Einmaliges Anmelden aktivieren**aus.
   2. Wählen Sie für **Identitätsanbietertyp** die Option **SAML 2.0** aus.
   3. Kopieren Sie im klassischen Azure-Portal auf der Seite **Einmaliges Anmelden konfigurieren für Intacct** den Wert für **Aussteller-URL**, und fügen Sie ihn in das Feld **Aussteller-URL** ein.
   4. Kopieren Sie im klassischen Azure-Portal auf der Seite **Einmaliges Anmelden konfigurieren für Intacct** den Wert für **Remoteanmelde-URL**, und fügen Sie ihn in das Feld **Anmelde-URL** ein.
   5. Erstellen Sie eine **Base64-codierte** Datei aus dem heruntergeladenen Zertifikat. Weitere Informationen finden Sie unter [Konvertieren eines binären Zertifikats in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o).      
   6. Öffnen Sie das Base-64-codierte Zertifikat im Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Feld **Zertifikat** ein.
   7. Klicken Sie auf **Speichern**.
9. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um die Seite **Einmaliges Anmelden konfigurieren** zu schließen.

   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-intacct-tutorial/IC790040.png "Einmaliges Anmelden konfigurieren")

## <a name="configure-user-provisioning"></a>Benutzerbereitstellung konfigurieren

Zur Einrichtung von Azure AD-Benutzern, damit Sie sich bei Intacct anmelden können, müssen sie in Intacct bereitgestellt werden. Bei Intacct ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen von Benutzerkonten die folgenden Schritte aus:**

1. Melden Sie sich bei Ihrem **Intacct**-Mandanten an.
2. Klicken Sie auf die Registerkarte **Unternehmen** und dann auf **Benutzer**.

   ![Benutzer](./media/active-directory-saas-intacct-tutorial/IC790041.png "Benutzer")
3. Klicken Sie auf die Registerkarte **Hinzufügen**.

   ![Hinzufügen](./media/active-directory-saas-intacct-tutorial/IC790042.png "Hinzufügen")
4. Führen Sie im Abschnitt **Benutzerinformationen** die folgenden Schritte aus:

   ![Benutzerinformationen](./media/active-directory-saas-intacct-tutorial/IC790043.png "Benutzerinformationen")

   1. Geben Sie im Abschnitt **Benutzerinformationen** die **Benutzer-ID**, den **Nachnamen**, den **Vornamen**, die **E-Mail-Adresse**, den **Titel** und die **Telefonnummer** eines Azure AD-Kontos ein, das Sie bereitstellen möchten.
   2. Wählen Sie die **Administratorrechte** eines Azure AD-Kontos aus, das Sie bereitstellen möchten.
   3. Klicken Sie auf **Speichern**. Der Besitzer des Azure AD-Kontos erhält eine E-Mail mit einem Link zur Bestätigung des Kontos, bevor es aktiv wird.

>[!NOTE]
>Sie können Azure AD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Intacct-Benutzerkonten oder mithilfe von Intacct bereitgestellten APIs erstellen.
>

## <a name="assign-users"></a>Benutzer zuweisen
Zum Testen Ihrer Konfiguration müssen Sie Intacct Azure AD-Benutzer zuweisen. Nachdem ein Benutzer zugewiesen wurde, kann er auf Ihre Anwendung zugreifen.

**Führen Sie zum Zuweisen von Benutzern in Intacct folgende Schritte aus:**

1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Intacct** auf **Benutzer zuweisen**.

   ![Zuweisen von Benutzern](./media/active-directory-saas-intacct-tutorial/IC790044.png "Zuweisen von Benutzern")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.

   ![Ja](./media/active-directory-saas-intacct-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).


### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt richten Sie Britta Simon für die Verwendung des einmaligen Anmeldens von Azure ein, indem Sie ihr Zugriff auf Intacct gewähren.

![Benutzer zuweisen][200]

**Um Britta Simon Intacct zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht. Navigieren Sie zur Verzeichnisansicht und dann zu **Unternehmensanwendungen**, und klicken Sie dann auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **Intacct**aus.

    ![Configure single sign-on](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_50.png)

3. Klicken Sie im Menü **Verwalten** auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**, und wählen Sie dann unter **Zuweisung hinzufügen** die Option **Benutzer und Gruppen**.

    ![Benutzer zuweisen][203]

5. Wählen Sie unter **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie unter **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie unter **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.



### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Intacct“ klicken, sollten Sie automatisch bei Ihrer Intacct-Anwendung angemeldet werden.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_203.png

