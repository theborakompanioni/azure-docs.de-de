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
ms.sourcegitcommit: 4eb426b14ec72aaa79268840f23a39b15fee8982
ms.openlocfilehash: e8ca7fdacf8361570d88260b3c359ee6e2fd3e17
ms.contentlocale: de-de
ms.lasthandoff: 09/06/2017

---
# <a name="tutorial-configure-google-apps-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von Google Apps für die automatische Benutzerbereitstellung

In diesem Tutorial erfahren Sie, wie Sie Benutzerkonten aus Azure Active Directory (Azure AD) automatisch in Google Apps bereitstellen bzw. deren Bereitstellung aufheben.

## <a name="prerequisites"></a>Voraussetzungen

Das in diesem Tutorial beschriebene Szenario setzt voraus, dass Sie bereits über Folgendes verfügen:

*   Einen Azure Active Directory-Mandanten
*   Einen gültigen Mandanten für Google Apps for Work oder Google Apps for Education. Sie können ein kostenloses Testkonto für einen der Dienste verwenden.
*   Ein Benutzerkonto in Google Apps mit Teamadministratorberechtigung.

## <a name="assign-users-to-google-apps"></a>Zuweisen von Benutzern zu Google Apps

Azure Active Directory ermittelt anhand von Zuweisungen, welche Benutzer Zugriff auf bestimmte Apps erhalten sollen. Im Kontext der automatischen Bereitstellung von Benutzerkonten werden nur die Benutzer und Gruppen synchronisiert, die einer Anwendung in Azure AD zugewiesen wurden.

Vor dem Konfigurieren und Aktivieren des Bereitstellungsdiensts müssen Sie entscheiden, welche Benutzer oder Gruppen aus Azure AD Zugriff auf Ihre Google Apps-App benötigen. Im Anschluss an diese Entscheidung können Sie die entsprechenden Benutzer gemäß der Anleitung unter [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) Ihrer Google Apps-App zuweisen.

> [!IMPORTANT]
> Es empfiehlt sich, Google Apps einen einzelnen Azure AD-Benutzer zuzuweisen, um die Konfiguration der Bereitstellung zu testen. Später können Sie dann weitere Benutzer und Gruppen zuweisen.

> Wählen Sie im Zuweisungsdialogfeld die Rolle **Benutzer** oder **Gruppe** aus, wenn Sie Google Apps einen Benutzer zuweisen. Die Rolle **Standardzugriff** ist für Bereitstellungen nicht geeignet.

## <a name="enable-automated-user-provisioning"></a>Aktivieren der automatisierten Benutzerbereitstellung

In diesem Abschnitt erfahren Sie, wie Sie Ihr Azure AD mit der API für die Benutzerkontenbereitstellung von Google Apps verbinden. Außerdem erfahren Sie, wie Sie den Bereitstellungsdienst zum Erstellen, Aktualisieren und Deaktivieren zugewiesener Benutzerkonten in Google Apps auf der Grundlage der Benutzer- und Gruppenzuweisung in Azure AD konfigurieren.

>[!TIP]
>Sie können auch das SAML-basierte einmalige Anmelden für Google Apps aktivieren. Befolgen Sie hierzu die Anweisungen im [Azure-Portal](https://portal.azure.com). Einmaliges Anmelden kann unabhängig von der automatischen Bereitstellung konfiguriert werden, obwohl diese beiden Features einander ergänzen.

### <a name="configure-automatic-user-account-provisioning"></a>Konfigurieren der automatischen Bereitstellung von Benutzerkonten

> [!NOTE]
> Eine weitere Option für die Automatisierung der Benutzerbereitstellung für Google Apps ist die Verwendung von [Google Apps Directory Sync (GADS)](https://support.google.com/a/answer/106368?hl=en). GADS stellt Ihre lokalen Active Directory-Identitäten für Google Apps bereit. Im Gegensatz dazu stellt die Lösung in diesem Tutorial Ihre Azure Active Directory-Benutzer (Cloud) und E-Mail-fähigen Gruppen für Google Apps bereit. 

1. Melden Sie sich bei der [Google Apps-Verwaltungskonsole](http://admin.google.com/) mit Ihrem Administratorkonto an, und wählen Sie **Sicherheit** aus. Sollte der Link nicht zu sehen sein, befindet er sich möglicherweise unter dem Menü **Weitere Steuerelemente** am unteren Bildschirmrand.
   
    ![Wählen Sie „Sicherheit“ aus.][10]

2. Wählen Sie auf der Seite **Sicherheit** die Option **API-Referenz** aus.
   
    ![Wählen Sie „API-Referenz“ aus.][15]

3. Wählen Sie **API-Zugriff aktivieren**aus.
   
    ![Wählen Sie „API-Referenz“ aus.][16]

    > [!IMPORTANT]
    > Für jeden Benutzer, den Sie für Google Apps bereitstellen möchten, *muss* der Benutzername in Azure Active Directory an eine benutzerdefinierte Domäne gebunden werden. Beispielsweise werden Benutzernamen wie bob@contoso.onmicrosoft.com von Google Apps nicht akzeptiert. bob@contoso.com ist hingegen zulässig. Sie können die Domäne eines vorhandenen Benutzers ändern, indem Sie seine Eigenschaften in Azure AD bearbeiten. Die folgenden Schritte enthalten eine Anleitung zum Festlegen einer benutzerdefinierten Domäne für Azure Active Directory und Google Apps.
      
4. Führen Sie die folgenden Schritte aus, wenn Sie Ihrem Azure Active Directory noch keinen benutzerdefinierten Domänennamen hinzugefügt haben:
  
    a. Wählen Sie im [Azure-Portal](https://portal.azure.com) im linken Navigationsbereich die Option **Active Directory** aus. Wählen Sie in der Verzeichnisliste Ihr Verzeichnis aus. 

    b. Wählen Sie im linken Navigationsbereich die Option **Domänenname** und anschließend **Hinzufügen** aus.
     
     ![Domäne](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_1.png)

     ![Hinzufügen der Domäne](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_2.png)

    c. Geben Sie Ihren Domänennamen in das Feld **Domänenname** ein. Dieser Domänenname sollte der gleiche Domänenname sein, den Sie für Google Apps verwenden möchten. Wählen Sie als Nächstes die Schaltfläche **Domäne hinzufügen** aus.
     
     ![Domänenname](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_3.png)

    d. Wählen Sie **Weiter** aus, um zur Überprüfungsseite zu gelangen. Bearbeiten Sie die DNS-Einträge der Domäne gemäß den Werten auf dieser Seite, um zu bestätigen, dass Ihnen die Domäne gehört. Sie können wählen, ob Sie zur Bestätigung **MX-Einträge** oder **TXT-Einträge** verwenden möchten. Dies hängt von der Auswahl für die Option **Eintragstyp** ab. 
    
    Eine ausführlichere Anleitung zur Bestätigung von Domänennamen mit Azure AD finden Sie unter [Hinzufügen eigener Domänennamen zu Azure AD](https://go.microsoft.com/fwLink/?LinkID=278919&clcid=0x409).
     
     ![Domäne](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_4.png)

    e. Wiederholen Sie die obigen Schritte für alle Domänen, die Sie Ihrem Verzeichnis hinzufügen möchten.

5. Nachdem Sie alle Ihre Domänen mit Azure AD überprüft haben, müssen Sie sie mit Google Apps erneut überprüfen. Führen Sie für jede Domäne, die noch nicht für Google Apps registriert ist, die folgenden Schritte aus:
   
    a. Wählen Sie in der [Google Apps-Verwaltungskonsole](http://admin.google.com/) die Option **Domänen** aus.
     
     ![Auswählen von „Domänen“][20]

    b. Wählen Sie **Domäne oder Domänenalias hinzufügen** aus.
     
     ![Hinzufügen einer neuen Domäne][21]

    c. Wählen Sie **Andere Domäne hinzufügen** aus, und geben Sie dann den Namen der Domäne ein, die Sie hinzufügen möchten.
     
     ![Geben Sie Ihren Domänennamen ein][22]

    d. Wählen Sie **Weiter und Domänenbesitzrecht überprüfen** aus. Führen Sie dann die Schritte aus, mit denen Sie prüfen, ob Sie den Domänennamen besitzen. Umfassende Anweisungen zum Überprüfen Ihrer Domäne mit Google Apps finden Sie unter [Überprüfen des Websitebesitzrechts mit Google Apps](https://support.google.com/webmasters/answer/35179).

    e. Wiederholen Sie die obigen Schritte für alle weiteren Domänen, die Sie Google Apps hinzufügen möchten.
     
     > [!WARNING]
     > Wenn Sie die primäre Domäne für Ihren Google Apps-Mandanten ändern und das einmalige Anmelden mit Azure AD bereits konfiguriert haben, müssen Sie Schritt 3 unter [Schritt 2: Aktivieren der einmaligen Anmeldung](#step-two-enable-single-sign-on) wiederholen.
       
6. Wählen Sie in der [Google Apps-Verwaltungskonsole](http://admin.google.com/) die Option **Administratorrollen** aus.
   
     ![Auswählen von „Google Apps“][26]

7. Bestimmen Sie, welches Administratorkonto Sie zum Verwalten der Benutzerbereitstellung verwenden möchten. Bearbeiten Sie für die **Administratorrolle** des Kontos die **Berechtigungen** für diese Rolle. Aktivieren Sie alle **Admin-API-Berechtigungen**, damit dieses Konto für die Bereitstellung verwendet werden kann.
   
     ![Auswählen von „Google Apps“][27]
   
    > [!NOTE]
    > Wenn Sie eine Produktionsumgebung konfigurieren, besteht die bewährte Methode darin, speziell für diesen Schritt ein Administratorkonto in Google Apps zu erstellen. Diesen Konten muss eine Administratorrolle zugeordnet sein, die über die erforderlichen API-Berechtigungen verfügt.
     
8. Wechseln Sie im [Azure-Portal](https://portal.azure.com) zum Abschnitt **Azure Active Directory** > **Unternehmens-Apps** > **Alle Anwendungen**.

9. Wenn Sie Google Apps bereits für einmaliges Anmelden konfiguriert haben, können Sie über das Suchfeld nach Ihrer Google Apps-Instanz suchen. Wählen Sie andernfalls **Hinzufügen** aus, und suchen Sie im Anwendungskatalog nach **Google Apps**. Wählen Sie in den Suchergebnissen **Google Apps** aus, und fügen Sie die Anwendung Ihrer Anwendungsliste hinzu.

10. Wählen Sie Ihre Google Apps-Instanz und anschließend die Registerkarte **Bereitstellung** aus.

11. Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest. 

     ![Bereitstellung](./media/active-directory-saas-google-apps-provisioning-tutorial/provisioning.png)

12. Wählen Sie im Abschnitt **Administratoranmeldeinformationen** die Option **Autorisieren** aus. In einem neuen Browserfenster wird ein Dialogfeld für die Google Apps-Autorisierung geöffnet.

13. Bestätigen Sie, dass Sie Azure Active Directory die Berechtigung erteilen möchten, Änderungen an Ihrem Google Apps-Mandanten vorzunehmen. Wählen Sie **Akzeptieren** aus.
    
     ![Überprüfen Sie die Berechtigungen.][28]

14. Wählen Sie im Azure-Portal die Option **Verbindung testen** aus, um sich zu vergewissern, dass Azure AD eine Verbindung mit Ihrer Google Apps-App herstellen kann. Falls die Verbindung nicht hergestellt werden kann, vergewissern Sie sich, dass Ihr Google Apps-Konto über Teamadministratorberechtigungen verfügt. Führen Sie dann erneut den Schritt **Autorisieren** aus.

15. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder einer Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll. Aktivieren Sie dann das Kontrollkästchen.

16. Wählen Sie **Speichern** aus.

17. Wählen Sie im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Benutzer mit Google Apps synchronisieren** aus.

18. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure AD mit Google Apps synchronisiert werden. Die Attribute, bei denen es sich um **übereinstimmende** Eigenschaften handelt, werden zum Abgleich der Benutzerkonten in Google Apps für Aktualisierungsvorgänge verwendet. Wählen Sie **Speichern** aus, um Ihre Änderungen zu committen.

19. Ändern Sie den **Bereitstellungsstatus** unter **Einstellungen** in **Ein**, um den Azure AD-Bereitstellungsdienst für Google Apps zu aktivieren.

20. Wählen Sie **Speichern** aus.

Daraufhin wird die Erstsynchronisierung aller Benutzer oder Gruppen gestartet, die Google Apps im Abschnitt „Benutzer und Gruppen“ zugewiesen sind. Die Erstsynchronisierung dauert länger als nachfolgende Synchronisierungen, die während der Dienstausführung etwa alle 20 Minuten erfolgen. 

Im Abschnitt **Synchronisierungsdetails** können Sie den Fortschritt überwachen und über Links zu Bereitstellungsaktivitätsberichten navigieren. In diesen Berichten werden sämtliche Aktionen beschrieben, die vom Bereitstellungsdienst für Ihre Google Apps-App ausgeführt werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurieren von einmaligem Anmelden](active-directory-saas-google-apps-tutorial.md)



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

