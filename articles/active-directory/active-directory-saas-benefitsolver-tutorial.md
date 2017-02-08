---
title: 'Tutorial: Azure Active Directory-Integration mit Benefitsolver | Microsoft Docs'
description: "Erfahren Sie, wie Sie Benefitsolver mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: cf4529b1-3fb6-4475-82b7-2ceedcb70b3c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/10/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: eb7dce1c0664e34c4946bb08f5b412e1e01c715d


---
# <a name="tutorial-azure-active-directory-integration-with-benefitsolver"></a>Lernprogramm: Azure Active Directory-Integration mit Benefitsolver
In diesem Lernprogramm wird die Integration von Azure und Benefitsolver erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein Benefitsolver-Abonnement, für das einmaliges Anmelden aktiviert ist

Nach Abschluss dieses Lernprogramms können sich die Azure AD-Benutzer, die Sie Benefitsolver zugewiesen haben, wie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)beschrieben, mittels einmaligen Anmeldens bei der Anwendung anmelden.

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Benefitsolver
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-benefitsolver-tutorial/IC804820.png "Szenario")

## <a name="enabling-the-application-integration-for-benefitsolver"></a>Aktivieren der Anwendungsintegration für Benefitsolver
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Benefitsolver aktivieren.

### <a name="to-enable-the-application-integration-for-benefitsolver-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Benefitsolver
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-benefitsolver-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-benefitsolver-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-benefitsolver-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-benefitsolver-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** als Suchbegriff **Benefitsolver** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-benefitsolver-tutorial/IC804821.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **Benefitsolver** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Benefitssolver](./media/active-directory-saas-benefitsolver-tutorial/IC804822.png "Benefitssolver")
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Benefitsolver zu authentifizieren.  
Die Benefitsolver-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der **SAML-Tokenattribute** benutzerdefinierte Attributzuordnungen hinzufügen.  
Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:

![Attribute](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Attribute")

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Benefitsolver** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-benefitsolver-tutorial/IC804824.png "Einmaliges Anmelden konfigurieren")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Benefitsolver anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden aus**, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-benefitsolver-tutorial/IC804825.png "Einmaliges Anmelden konfigurieren")
3. Führen Sie auf der Seite **App-Einstellungen konfigurieren** die folgenden Schritte aus:
   
   ![App-Einstellungen konfigurieren](./media/active-directory-saas-benefitsolver-tutorial/IC804826.png "App-Einstellungen konfigurieren")
   
   1. Geben Sie im Textfeld **Anmelde-URL** die URL **http://azure.benefitsolver.com** ein.
   2. Geben Sie im Textfeld **Antwort-URL** die URL **https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml** ein.  
   3. Klicken Sie auf **Weiter**.
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Benefitsolver** auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei lokal auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-benefitsolver-tutorial/IC804827.png "Einmaliges Anmelden konfigurieren")
5. Senden Sie die heruntergeladene Metadatendatei an das Supportteam von Benefitsolver.
   
   > [!NOTE]
   > Die eigentliche SSO-Konfiguration muss vom Supportteam von Benefitsolver durchgeführt werden.
   > Sie erhalten eine Benachrichtigung, wenn SSO für Ihr Abonnement aktiviert wurde.
   > 
   > 
6. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-benefitsolver-tutorial/IC804828.png "Einmaliges Anmelden konfigurieren")
7. Klicken Sie oben im Menü auf **Attribute** to open the **SAML Token Attribute** zu öffnen.
   
   ![Attribute](./media/active-directory-saas-benefitsolver-tutorial/IC795920.png "Attribute")
8. So fügen Sie die erforderlichen Attributzuordnungen hinzu:
   
   ![Attribute](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Attribute")
   
   | Attributname | Attributwert |
   | --- | --- |
   | ClientID |Sie müssen diesen Wert beim Supportteam von Benefitsolver anfragen. |
   | ClientKey |Sie müssen diesen Wert beim Supportteam von Benefitsolver anfragen. |
   | LogoutURL |Sie müssen diesen Wert beim Supportteam von Benefitsolver anfragen. |
   | EmployeeID |Sie müssen diesen Wert beim Supportteam von Benefitsolver anfragen. |
   
   1. Klicken Sie für jede Datenzeile in der obigen Tabelle auf **add user attribute**.
   2. Geben Sie im Textfeld **Attribute Name** den für die Zeile angezeigten Attributnamen ein.
   3. Geben Sie im Textfeld **Attribute Value** den für die Zeile angezeigten Attributwert ein.
   4. Klicken Sie auf **Fertig stellen**.
9. Klicken Sie auf **Apply Changes**.

## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung
Damit sich Azure AD-Benutzer bei Benefitsolver anmelden können, müssen sie in Benefitsolver bereitgestellt werden.  
Im Fall von Benefitsolver werden die Mitarbeiterdaten in Ihrer Anwendung (in der Regel nachts) über eine Erhebungsdatei aus Ihrem HRIS-System ausgefüllt.  

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Benefitsolver-Benutzerkonten oder mithilfe der von Benefitsolver bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-benefitsolver-perform-the-following-steps"></a>So weisen Sie Benefitsolver Benutzer zu
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Benefitsolver** auf **Benutzer zuweisen**.
   
   ![Zuweisen von Benutzern](./media/active-directory-saas-benefitsolver-tutorial/IC804829.png "Zuweisen von Benutzern")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-benefitsolver-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Dec16_HO4-->


