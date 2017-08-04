---
title: "Tutorial: Konfigurieren von Google Apps für die automatische Benutzerbereitstellung in Azure | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie Benutzerkonten aus Azure AD für Google Apps automatisch bereitstellen bzw. die Bereitstellung aufheben."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6dbd50b5-589f-4132-b9eb-a53a318a64e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: 818f7756189ed4ceefdac9114a0b89ef9ee8fb7a
ms.openlocfilehash: b061f0ddad70be4a5ca48d48d1a737d6af8afa8d
ms.contentlocale: de-de
ms.lasthandoff: 07/14/2017

---
# <a name="tutorial-configuring-google-apps-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von Google Apps für die automatische Benutzerbereitstellung

In diesem Tutorial werden die Schritte beschrieben, die Sie in Google Apps und Azure AD ausführen müssen, um Benutzerkonten aus Azure AD in Google Apps automatisch bereitzustellen bzw. deren Bereitstellung automatisch aufzuheben.

## <a name="prerequisites"></a>Voraussetzungen

Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

*   Einen Azure Active Directory-Mandanten.
*   Sie benötigen einen gültigen Mandanten für Google Apps for Work oder Google Apps for Education. Sie können ein kostenloses Testkonto für einen der Dienste verwenden.
*   Ein Benutzerkonto in Google Apps mit Teamadministratorberechtigung.

## <a name="assigning-users-to-google-apps"></a>Zuweisen von Benutzern zu Google Apps

Azure Active Directory ermittelt anhand von Zuweisungen, welche Benutzer Zugriff auf bestimmte Apps erhalten sollen. Im Kontext der automatischen Bereitstellung von Benutzerkonten werden nur die Benutzer und Gruppen synchronisiert, die einer Anwendung in Azure AD zugewiesen wurden.

Vor dem Konfigurieren und Aktivieren des Bereitstellungsdiensts müssen Sie entscheiden, welche Benutzer und/oder Gruppen in Azure AD die Benutzer darstellen, die Zugriff auf Ihre Google Apps-App benötigen. Nachdem die Entscheidung getroffen wurde, können Sie diese Benutzer Ihrer Google Apps-App zuweisen, indem Sie die hier angegebene Anleitung befolgen: [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

> [!IMPORTANT]
>*   Es wird empfohlen, Google Apps einen einzelnen Azure AD-Benutzer zuzuweisen, um die Konfiguration der Bereitstellung zu testen. Später können weitere Benutzer und/oder Gruppen zugewiesen werden.
>*   Beim Zuweisen eines Benutzers zu Google Apps müssen Sie im Dialogfeld „Zuweisung“ die Rolle „Benutzer“ oder „Gruppe“ auswählen. Die Rolle „Standardzugriff“ funktioniert nicht für die Bereitstellung.

## <a name="enable-automated-user-provisioning"></a>Aktivieren der automatisierten Benutzerbereitstellung

In diesem Abschnitt wird das Herstellen einer Verbindung von Azure AD mit der Google Apps-API zur Bereitstellung von Benutzerkonten sowie das Konfigurieren des Bereitstellungsdiensts für das Erstellen, Aktualisieren und Deaktivieren zugewiesener Benutzerkonten in Google Apps basierend auf der Benutzer- und Gruppenzuweisung in Azure AD beschrieben.

>[!Tip]
>Sie können auch das SAML-basierte einmalige Anmelden für Google Apps aktivieren. Befolgen Sie hierzu die Anweisungen im [Azure-Portal](https://portal.azure.com). Einmaliges Anmelden kann unabhängig von der automatischen Bereitstellung konfiguriert werden, obwohl diese beiden Features einander ergänzen.

### <a name="configure-automatic-user-account-provisioning"></a>Konfigurieren der automatischen Bereitstellung von Benutzerkonten

> [!NOTE]
> Eine weitere Option für die Automatisierung der Benutzerbereitstellung für Google Apps ist die Verwendung von [Google Apps Directory Sync (GADS)](https://support.google.com/a/answer/106368?hl=en) , um Ihre lokalen Active Directory-Identitäten für Google Apps bereitzustellen. Im Gegensatz dazu stellt die Lösung in diesem Tutorial Ihre Azure Active Directory-Benutzer (Cloud) und E-Mail-aktivierten Gruppen für Google Apps bereit. 

1. Melden Sie sich bei der [Google Apps-Verwaltungskonsole](http://admin.google.com/) mit Ihrem Administratorkonto an, und klicken Sie auf **Sicherheit**. Wenn der Link nicht angezeigt wird, kann er unter dem Menü **Weitere Steuerelemente** im unteren Bereich des Bildschirms versteckt sein.
   
    ![Klicken Sie auf "Sicherheit".][10]

2. Klicken Sie auf der Seite **Sicherheit** auf **API-Referenz**.
   
    ![Klicken Sie auf "API-Referenz".][15]

3. Wählen Sie **API-Zugriff aktivieren**aus.
   
    ![Klicken Sie auf "API-Referenz".][16]

    > [!IMPORTANT]
    > Für jeden Benutzer, den Sie für Google Apps bereitstellen möchten, *muss* der Benutzername in Azure Active Directory an eine benutzerdefinierte Domäne gebunden werden. Beispielsweise werden Benutzernamen, die bob@contoso.onmicrosoft.com lauten, von Google Apps nicht akzeptiert, während bob@contoso.com zulässig ist. Sie können die Domäne eines vorhandenen Benutzers ändern, indem Sie seine Eigenschaften in Azure AD bearbeiten. Eine Anleitung zum Festlegen einer benutzerdefinierten Domäne für Azure Active Directory und Google Apps ist in den folgenden Schritten enthalten.
      
4. Führen Sie diese Schritte aus, wenn Sie Ihrem Azure Active Directory noch keinen benutzerdefinierten Domänennamen hinzugefügt haben:
  
    a. Klicken Sie im [Azure-Portal](https://portal.azure.com) im linken Navigationsbereich auf **Active Directory**. Wählen Sie in der Verzeichnisliste Ihr Verzeichnis aus. 

    b. Klicken Sie im linken Navigationsbereich auf **Domains name** (Domänenname) und dann auf **Hinzufügen**.
     
     ![Domäne](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_1.png)

     ![Hinzufügen von Domänen](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_2.png)

    c. Geben Sie Ihren Domänennamen in das Feld **Domänenname** ein. Dieser Domänenname sollte der gleiche Domänenname sein, den Sie für Google Apps verwenden möchten. Wenn Sie fertig sind, klicken Sie auf die Schaltfläche **Domäne hinzufügen**.
     
     ![Domänenname](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_3.png)

    d. Klicken Sie auf **Weiter** , um zur Überprüfungsseite zu wechseln. Um sicherzustellen, dass Sie diese Domäne besitzen, müssen Sie die DNS-Einträge der Domäne gemäß der Werte bearbeiten, die auf dieser Seite bereitstehen. Sie können auch wählen, ob Sie zur Überprüfung **MX-Einträge** oder **TXT-Einträge** verwenden möchten. Dies hängt von Ihrer Auswahl für die Option **Datensatztyp** ab. Ausführlichere Anweisungen zum Überprüfen von Domänennamen mit Azure AD finden Sie unter [Hinzufügen eigener Domänennamen zu Azure AD](https://go.microsoft.com/fwLink/?LinkID=278919&clcid=0x409).
     
     ![Domäne](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_4.png)

    e. Wiederholen Sie die obigen Schritte für alle Domänen, die Sie Ihrem Verzeichnis hinzufügen möchten.

5. Nachdem Sie Ihre Domänen mit Azure AD überprüft haben, müssen Sie sie mit Google Apps erneut überprüfen. Führen Sie für jede Domäne, die noch nicht für Google Apps registriert ist, die folgenden Schritte aus:
   
    a. Klicken Sie in der [Google Apps-Verwaltungskonsole](http://admin.google.com/) auf **Domänen**.
     
     ![Klicken Sie auf "Domänen"][20]

    b. Klicken Sie auf **Domäne oder Domänenalias hinzufügen**.
     
     ![Hinzufügen einer neuen Domäne][21]

    c. Wählen Sie **Andere Domäne hinzufügen**, und geben Sie den Namen der Domäne ein, die Sie hinzufügen möchten.
     
     ![Geben Sie Ihren Domänennamen ein][22]

    d. Klicken Sie auf **Continue and verify domain ownership** (Fortfahren und Domänenbesitz überprüfen). Führen Sie dann die Schritte aus, mit denen Sie prüfen, ob Sie den Domänennamen besitzen. Eine ausführliche Anleitung zum Überprüfen Ihrer Domäne mit Google Apps finden Sie unter [Verify your site ownership with Google Apps](https://support.google.com/webmasters/answer/35179) (Überprüfen des Websitebesitzes mit Google Apps).

    e. Wiederholen Sie die obigen Schritte für alle weiteren Domänen, die Sie Google Apps hinzufügen möchten.
     
     > [!WARNING]
     > Wenn Sie die primäre Domäne für Ihren Google Apps-Mandanten ändern und das einmalige Anmelden mit Azure AD bereits konfiguriert haben, müssen Sie Schritt 3 unter [Schritt 2: Aktivieren der einmaligen Anmeldung](#step-two-enable-single-sign-on) wiederholen.
       
6. Klicken Sie in der [Google Apps-Verwaltungskonsole](http://admin.google.com/) auf **Administratorrollen**.
   
     ![Klicken Sie auf "Google Apps".][26]

7. Ermitteln Sie, welches Administratorkonto Sie zum Verwalten der Benutzerbereitstellung verwenden möchten. Bearbeiten Sie für die **Administratorrolle** des Kontos die **Berechtigungen** für diese Rolle. Stellen Sie sicher, dass alle **Admin-API-Berechtigungen** aktiviert sind, sodass dieses Konto für die Bereitstellung verwendet werden kann.
   
     ![Klicken Sie auf "Google Apps".][27]
   
    > [!NOTE]
    > Wenn Sie eine Produktionsumgebung konfigurieren, besteht die bewährte Methode darin, speziell für diesen Schritt ein Administratorkonto in Google Apps zu erstellen. Diesem Konto muss eine Administratorrolle zugeordnet sein, die über die erforderlichen API-Berechtigungen verfügt.
     
8. Wechseln Sie im [Azure-Portal](https://portal.azure.com) zum Abschnitt **Azure Active Directory > Unternehmens-Apps > Alle Anwendungen**.

9. Wenn Sie Google Apps bereits für einmaliges Anmelden konfiguriert haben, können Sie über das Suchfeld nach Ihrer Google Apps-Instanz suchen. Wählen Sie andernfalls **Hinzufügen**, und suchen Sie im Anwendungskatalog nach **Google Apps**. Wählen Sie „Google Apps“ in den Suchergebnissen aus, und fügen Sie die Anwendung Ihrer Anwendungsliste hinzu.

10. Wählen Sie Ihre Google Apps-Instanz aus, und wählen Sie dann die Registerkarte **Bereitstellung**.

11. Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest. 

     ![Bereitstellung](./media/active-directory-saas-google-apps-provisioning-tutorial/provisioning.png)

12. Klicken Sie im Abschnitt **Administratoranmeldeinformationen** auf **Autorisieren**. In einem neuen Browserfenster wird ein Dialogfeld für die Google Apps-Autorisierung geöffnet.

13. Bestätigen Sie, dass Sie Azure Active Directory die Berechtigung erteilen möchten, Änderungen an Ihrem Google Apps-Mandanten vorzunehmen. Klicken Sie auf **Annehmen**.
    
     ![Überprüfen Sie die Berechtigungen.][28]

14. Klicken Sie im Azure-Portal auf **Verbindung testen**, um sicherzustellen, dass Azure AD eine Verbindung mit Ihrer Google Apps-App herstellen kann. Wenn die Verbindung nicht möglich ist, stellen Sie sicher, dass Ihr Google Apps-Konto über Teamadministratorberechtigungen verfügt, und wiederholen Sie den Schritt zum **Autorisieren**.

15. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder einer Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das Kontrollkästchen.

16. Klicken Sie auf **Speichern**.

17. Wählen Sie im Abschnitt „Zuordnungen“ die Option **Azure Active Directory-Benutzer mit Google Apps synchronisieren** aus.

18. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure AD mit Google Apps synchronisiert werden. Beachten Sie, dass die als **übereinstimmende** Eigenschaften ausgewählten Attribute für den Abgleich der Benutzerkonten in Google Apps für Updatevorgänge verwendet werden. Wählen Sie die Schaltfläche „Speichern“, um alle Änderungen zu übernehmen.

19. Um den Azure AD-Bereitstellungsdienst für Google Apps zu aktivieren, ändern Sie den **Bereitstellungsstatus** im Abschnitt „Einstellungen“ in **Ein**.

20. Klicken Sie auf **Speichern**.

Dadurch wird die Erstsynchronisierung aller Benutzer bzw. Gruppen gestartet, die Google Apps im Abschnitt „Benutzer und Gruppen“ zugewiesen sind. Die Erstsynchronisierung dauert länger als nachfolgende Synchronisierungen, die ungefähr alle 20 Minuten erfolgen, solange der Dienst ausgeführt wird. Im Abschnitt **Synchronisierungsdetails** können Sie den Fortschritt überwachen und Links zu Berichten zur Bereitstellungsaktivität aufrufen. Darin sind alle Aktionen aufgeführt, die vom Bereitstellungsdienst in Ihrer Google Apps-App ausgeführt werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurieren des einmaligen Anmeldens](active-directory-saas-google-apps-tutorial.md)



<!--Image references-->

[10]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-security.png
[15]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-api.png
[16]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-api-enabled.png
[20]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-domains.png
[21]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-add-domain.png
[22]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-add-another.png
[24]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-provisioning.png
[25]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-provisioning-auth.png
[26]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-admin.png
[27]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-admin-privileges.png
[28]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-auth.png
