---
title: 'Tutorial: Azure Active Directory-Integration mit Mimecast Personal Portal | Microsoft Docs'
description: "Erfahren Sie, wie Sie Mimecast Personal Portal mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 345b22be-d87e-45a4-b4c0-70a67eaf9bfd
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/03/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: a5b24d6897fbc6fdf71e4acc3898d640ec974f51
ms.openlocfilehash: 93544a2b907629c18e8abe6405e85d4623995bd7
ms.lasthandoff: 02/17/2017


---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-personal-portal"></a>Tutorial: Azure Active Directory-Integration mit Mimecast Personal Portal
In diesem Tutorial wird die Integration von Azure und Mimecast Personal Portal erläutert. 

Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein Mimecast Personal Portal-Abonnement, für das das einmalige Anmelden (SSO) aktiviert ist

Nach Abschluss dieses Tutorials können sich die Mimecast Personal Portal zugewiesenen Azure AD-Benutzer mittels einmaliger Anmeldung auf der Mimecast Personal Portal-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie auch den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Mimecast Personal Portal
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794991.png "Szenario")

## <a name="enabling-the-application-integration-for-mimecast-personal-portal"></a>Aktivieren der Anwendungsintegration für Mimecast Personal Portal
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Mimecast Personal Portal aktivieren.

**Führen Sie zum Aktivieren der Anwendungsintegration für Mimecast Personal Portal die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** den Begriff **Mimecast Personal Portal** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794992.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **Mimecast Personal Portal** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Mimecast Personal Portal](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794993.png "Mimecast Personal Portal")
   
## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Mimecast Personal Portal zu authentifizieren.  

Im Rahmen dieses Verfahrens müssen Sie eine Base-64-codierte Zertifikatsdatei erstellen.  
Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)(Konvertieren eines binären Zertifikats in eine Textdatei; in englischer Sprache) weitere Informationen.

**So konfigurieren Sie einmaliges Anmelden**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Mimecast Personal Portal** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794994.png "Einmaliges Anmelden konfigurieren")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Mimecast Personal Portal anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794995.png "Einmaliges Anmelden konfigurieren")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld **Mimecast Personal Portal-Anmelde-URL** die von den Benutzern für die Anmeldung bei Mimecast Personal Portal verwendete URL ein (z.B. https://webmail-uk.mimecast.com oder https://webmail-us.mimecast.com), und klicken Sie dann auf **Weiter**.
   
   >[!NOTE]
   >Die Anmelde-URL ist regionsspezifisch. 
   > 
   
   ![App-URL konfigurieren](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794996.png "App-URL konfigurieren")
4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren für Mimecast Personal Portal** auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei lokal auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794997.png "Einmaliges Anmelden konfigurieren")
5. Melden Sie sich in einem anderen Webbrowserfenster bei Mimecast Personal Portal als Administrator an.
6. Wechseln Sie zu **Dienste \> Anwendung**.
   
   ![Anwendungen](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794998.png "Anwendungen")
7. Klicken Sie auf **Authentifizierungsprofile**.
   
   ![Authentifizierungsprofile](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794999.png "Authentifizierungsprofile")
8. Klicken Sie auf **Neues Authentifizierungsprofil**.
   
   ![Neues Authentifizierungsprofil](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795000.png "Neues Authentifizierungsprofil")
9. Führen Sie im Abschnitt **Authentifizierungsprofil** die folgenden Schritte aus:
   
   ![Authentifizierungsprofil](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795001.png "Authentifizierungsprofil")
   
   1. Geben Sie im Textfeld **Beschreibung** einen Namen für die Konfiguration ein.
   2. Wählen Sie **SAML-Authentifizierung für Mimecast Personal Portal aktivieren**aus.
   3. Wählen Sie für **Anbieter** die Option **Azure Active Directory** aus.
   4. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Mimecast Personal Port** den Wert der **Aussteller-URL**, und fügen Sie ihn in das Textfeld **Aussteller-URL** ein.
   5. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Mimecast Personal Port** den Wert der **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **Anmelde-URL** ein.
   6. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Mimecast Personal Port** den Wert der **Remoteabmelde-URL**, und fügen Sie ihn in das Textfeld **Abmelde-URL** ein.  
      >[!NOTE]
      >Der Wert für Anmelde-URL und Abmelde-URL sind bei Mimecast Personal Portal gleich.
      > 
   7. Erstellen Sie eine **Base-64-codierte** Datei aus dem heruntergeladenen Zertifikat.  
      
      >[!TIP]
      >Weitere Informationen finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)(in englischer Sprache).
      >  
   8. Öffnen Sie das Base64-codierte Zertifikat in Editor, entfernen Sie die erste Zeile (*--*) und die letzte Zeile (*--*), und kopieren Sie den übrigen Inhalt in die Zwischenablage. Fügen Sie ihn anschließend in das Textfeld **Identitätsanbieterzertifikat (Metadaten)** ein.
   9. Wählen Sie **Einmaliges Anmelden zulassen**aus.
   10. Klicken Sie auf **Speichern**.
10. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795002.png "Einmaliges Anmelden konfigurieren")
    
## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich Azure AD-Benutzer bei Mimecast Personal Portal anmelden können, müssen Sie in Mimecast Personal Portal bereitgestellt werden. Im Fall von Mimecast Personal Portal ist die Bereitstellung eine manuelle Aufgabe.

* Sie müssen eine Domäne registrieren, bevor Sie Benutzer erstellen können.

**Um die Benutzerbereitstellung zu konfigurieren, führen Sie die folgenden Schritte durch:**

1. Melden Sie sich bei **Mimecast Personal Portal** als Administrator an.
2. Wechseln Sie zu **Verzeichnisse \> Intern**.
   
   ![Verzeichnisse](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795003.png "Verzeichnisse")
3. Klicken Sie auf **Neue Domäne registrieren**.
   
   ![Neue Domäne registrieren](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795004.png "Neue Domäne registrieren")
4. Nachdem die neue Domäne erstellt wurde, klicken Sie auf **Neue Adresse**.
   
   ![Neue Adresse](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795005.png "Neue Adresse")
5. Führen Sie im Dialogfeld „Neue Adresse“ die folgenden Schritte aus:
   
   ![Speichern](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795006.png "Speichern")
   
   1. Geben Sie die Attribute **E-Mail-Adresse**, **Globaler Name**, **Kennwort** und **Kennwort bestätigen** eines gültigen AAD-Kontos, das Sie bereitstellen möchten, in die entsprechenden Textfelder ein.
   2. Klicken Sie auf **Speichern**.

>[!NOTE]
>Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Mimecast Personal Portal-Benutzerkonten oder mithilfe der von Mimecast Personal Portal bereitgestellten APIs erstellen. 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

**Führen Sie zum Zuweisen von Benutzern in Mimecast Personal Portal folgende Schritte aus:**

1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Mimecast Personal Portal** auf **Benutzer zuweisen**.
   
   ![Zuweisen von Benutzern](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795007.png "Zuweisen von Benutzern")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-mimecast-personal-portal-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).


