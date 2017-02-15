---
title: 'Tutorial: Azure Active Directory-Integration mit Thoughtworks Mingle | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie Thoughtworks Mingle mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 69d859d9-b7f7-4c42-bc8c-8036138be586
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/06/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 49352a5e8255468bbc54c02e0cd9242d49002dbd
ms.openlocfilehash: 6a9a1cf616531de1d0b8194bc0e6c186acc27008


---
# <a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a>Tutorial: Azure Active Directory-Integration mit Thoughtworks Mingle
In diesem Tutorial wird die Integration von Azure und Thoughtworks Mingle erläutert.  
Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein Thoughtworks Mingle-Mandant

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Thoughtworks Mingle
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785150.png "Szenario")

## <a name="enabling-the-application-integration-for-thoughtworks-mingle"></a>Aktivieren der Anwendungsintegration für Thoughtworks Mingle
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Thoughtworks Mingle aktivieren.

### <a name="to-enable-the-application-integration-for-thoughtworks-mingle-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Thoughtworks Mingle:
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC700993.png "Active Directory")

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC700994.png "Anwendungen")

4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendung hinzufügen](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC749321.png "Anwendung hinzufügen")

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")

6. Geben Sie in das **Suchfeld** den Wert **thoughtworks mingle** ein.
   
    ![Anwendungskatalog](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785151.png "Anwendungskatalog")

7. Wählen Sie im Ergebnisbereich **Thoughtworks Mingle** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Thoughtworks Mingle](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785152.png "Thoughtworks Mingle")

## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung
In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Thoughtworks Mingle zu authentifizieren.  
Im Rahmen dieses Verfahrens müssen Sie ein Zertifikat auf Thoughtworks Mingle hochladen.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Thoughtworks Mingle** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785153.png "Einmaliges Anmelden konfigurieren")

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Thoughtworks Mingle anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785154.png "Einmaliges Anmelden konfigurieren")

3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld **URL des Thoughtworks Mingle-Mandanten** die URL im Format „*http://company.mingle.thoughtworks.com*“ ein, und klicken Sie dann auf **Weiter**.
   
    ![App-URL konfigurieren](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785155.png "App-URL konfigurieren")

4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Thoughtworks Mingle** auf „Metadaten herunterladen“, und speichern Sie die Daten auf Ihrem Computer.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785156.png "Einmaliges Anmelden konfigurieren")

5. Melden Sie sich bei der **Thoughtworks Mingle** -Unternehmenswebsite als Administrator an.

6. Klicken Sie auf die Registerkarte **Admin**, und klicken Sie dann auf **SSO-Config**.
   
    ![SSO-Config](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785157.png "SSO-Config")

7. Führen Sie im Abschnitt **SSO Config** die folgenden Schritte aus:
   
    ![SSO-Config](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785158.png "SSO-Config")
   
    a. Klicken Sie auf **Datei auswählen**, um die Metadatendatei hochzuladen.
   
    b. Klicken Sie auf **Änderungen speichern**.

8. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785159.png "Einmaliges Anmelden konfigurieren")

## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung
Damit sich AAD-Benutzer anmelden können, müssen sie in der Thoughtworks Mingle-Anwendung unter Verwendung ihrer Azure Active Directory-Benutzernamen bereitgestellt werden.  
Im Fall von Thoughtworks Mingle ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>So konfigurieren Sie die Benutzerbereitstellung
1. Melden Sie sich bei der Thoughtworks Mingle-Unternehmenswebsite als Administrator an.

2. Klicken Sie auf **Profil**.
   
    ![Ihr erstes Projekt](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785160.png "Ihr erstes Projekt")

3. Klicken Sie auf die Registerkarte **Admin**, und klicken Sie dann auf **Benutzer**.
   
    ![Benutzer](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785161.png "Benutzer")

4. Klicken Sie auf **Neuer Benutzer**.
   
    ![Neuer Benutzer](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785162.png "Neuer Benutzer")

5. Führen Sie auf der Dialogfeldseite **Neuer Benutzer** die folgenden Schritte aus:
   
    ![Neuer Benutzer](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785163.png "Neuer Benutzer")
   
    a. Geben Sie **Anmeldenamen**, **Anzeigenamen**, **Kennwort auswählen** und **Kennwort bestätigen** eines gültigen AAD-Kontos, das Sie bereitstellen möchten, in die entsprechenden Textfelder ein.
   
    b. Wählen Sie als **Benutzertyp** die Option **Vollbenutzer**.
   
    c. Klicken Sie auf **Dieses Profil erstellen**.

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Thoughtworks Mingle-Benutzerkonten oder mithilfe der von Thoughtworks Mingle bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-thoughtworks-mingle-perform-the-following-steps"></a>So weisen Sie Thoughtworks Mingle Benutzer zu:
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.

2. Klicken Sie auf der Anwendungsintegrationsseite für **Thoughtworks Mingle** auf **Benutzer zuweisen**.
   
    ![Zuweisen von Benutzern](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785164.png "Zuweisen von Benutzern")

3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
    ![Ja](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Dec16_HO2-->


