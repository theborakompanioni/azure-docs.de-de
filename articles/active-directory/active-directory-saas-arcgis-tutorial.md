---
title: 'Tutorial: Azure Active Directory-Integration mit ArcGIS | Microsoft Docs'
description: "Erfahren Sie, wie Sie ArcGIS mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: a9e132a4-29e7-48bf-beb9-4148e617c8a9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 3a373d1b8ae73f5b978414718dc93906ac5b16b7


---
# <a name="tutorial-azure-active-directory-integration-with-arcgis"></a>Lernprogramm: Azure Active Directory-Integration mit ArcGIS
In diesem Lernprogramm wird die Integration von Azure und ArcGIS erläutert. Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein ArcGIS-Abonnement, für das einmaliges Anmelden aktiviert ist

Nach Abschluss dieses Tutorials können sich die Azure AD-Benutzer, die Sie ArcGIS zugewiesen haben, mittels einmaligen Anmeldens auf Ihrer ArcGIS-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für ArcGIS
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-arcgis-tutorial/IC784735.png "Szenario")

## <a name="enabling-the-application-integration-for-arcgis"></a>Aktivieren der Anwendungsintegration für ArcGIS
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für ArcGIS aktivieren.

### <a name="to-enable-the-application-integration-for-arcgis-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für ArcGIS
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-arcgis-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-arcgis-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-arcgis-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-arcgis-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** als Suchbegriff **ArcGIS** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-arcgis-tutorial/IC784736.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **ArcGIS** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![ArcGIS](./media/active-directory-saas-arcgis-tutorial/IC784737.png "ArcGIS")
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei ArcGIS zu authentifizieren.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **ArcGIS** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-arcgis-tutorial/IC784738.png "Einmaliges Anmelden konfigurieren")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei ArcGIS anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-arcgis-tutorial/IC784739.png "Einmaliges Anmelden konfigurieren")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **ArcGIS-Anmelde-URL** die von Ihren Benutzern für die Anmeldung verwendete URL im Format *https://unternehmen.maps.arcgis.com* ein, und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-arcgis-tutorial/IC784740.png "App-URL konfigurieren")
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für ArcGIS** auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei lokal auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-arcgis-tutorial/IC784741.png "Einmaliges Anmelden konfigurieren")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der ArcGIS-Unternehmenswebsite als Administrator an.
6. Klicken Sie auf **Edit Settings**.
   
   ![Einstellungen bearbeiten](./media/active-directory-saas-arcgis-tutorial/IC784742.png "Einstellungen bearbeiten")
7. Klicken Sie auf **Sicherheit**.
   
   ![Sicherheit](./media/active-directory-saas-arcgis-tutorial/IC784743.png "Sicherheit")
8. Klicken Sie unter **Unternehmensanmeldungen** auf **Identitätsanbieter festlegen**.
   
   ![Unternehmensanmeldungen](./media/active-directory-saas-arcgis-tutorial/IC784744.png "Unternehmensanmeldungen")
9. Führen Sie auf der Konfigurationsseite **Set Identity Provider** die folgenden Schritte aus.
   
   ![Identitätsanbieter festlegen](./media/active-directory-saas-arcgis-tutorial/IC784745.png "Identitätsanbieter festlegen")
   
   1. Geben Sie in das Textfeld "Name" den Namen Ihrer Organisation ein.
   2. Wählen Sie für **Metadaten für Unternehmensidentitätsanbieter werden bereitgestellt durch** die Option **eine Datei** aus.
   3. Klicken Sie auf **Datei auswählen**, um die heruntergeladene Metadatendatei hochzuladen.
   4. Klicken Sie auf **Set Identity Provider**.
10. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-arcgis-tutorial/IC784746.png "Einmaliges Anmelden konfigurieren")
    
    ## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich Azure AD-Benutzer bei ArcGIS anmelden können, müssen sie in ArcGIS bereitgestellt werden.  
Im Fall von ArcGIS ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>So konfigurieren Sie die Benutzerbereitstellung
1. Melden Sie sich bei Ihrem **ArcGIS** -Mandanten an.
2. Klicken Sie auf **Invite Members**.
   
   ![Mitglieder einladen](./media/active-directory-saas-arcgis-tutorial/IC784747.png "Mitglieder einladen")
3. Wählen Sie **Mitglieder automatisch hinzufügen, ohne eine E-Mail zu senden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Mitglieder automatisch hinzufügen](./media/active-directory-saas-arcgis-tutorial/IC784748.png "Mitglieder automatisch hinzufügen")
4. Führen Sie auf der Dialogfeldseite **Members** die folgenden Schritte aus:
   
   ![Hinzufügen und überprüfen Sie](./media/active-directory-saas-arcgis-tutorial/IC784749.png "hinzufügen und überprüfen")
   
   1. Geben Sie den **Vornamen**, den **Nachnamen** und die **E-Mail-Adresse** eines gültigen AAD-Benutzerkontos ein, das Sie bereitstellen möchten.
   2. Klicken Sie auf **Add And Review**.
5. Überprüfen Sie die eingegebenen Daten, und klicken Sie dann auf **Add Members**.
   
   ![Mitglied hinzufügen](./media/active-directory-saas-arcgis-tutorial/IC784750.png "Mitglied hinzufügen")

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe von anderen Tools zum Erstellen von ArcGIS-Benutzerkonten oder mithilfe der von ArcGIS bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-arcgis-perform-the-following-steps"></a>So weisen Sie ArcGIS Benutzer zu
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **ArcGIS** auf **Benutzer zuweisen**.
   
   ![Zuweisen von Benutzern](./media/active-directory-saas-arcgis-tutorial/IC784751.png "Zuweisen von Benutzern")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-arcgis-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Feb17_HO3-->


