---
title: 'Tutorial: Azure Active Directory-Integration mit Salesforce | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Salesforce konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 49384b8b-3836-4eb1-b438-1c46bb9baf6f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: 31ecec607c78da2253fcf16b3638cc716ba3ab89
ms.openlocfilehash: a573a7ef79e28c50ae0923849a88f88af40f21be
ms.contentlocale: de-de
ms.lasthandoff: 06/23/2017


---
# <a name="tutorial-configuring-salesforce-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von Salesforce für die automatische Benutzerbereitstellung

Dieses Tutorial zeigt Ihnen die Schritte, die Sie in Salesforce und Azure AD ausführen müssen, um Benutzerkonten von Azure AD in Salesforce automatisch bereitzustellen bzw. deren Bereitstellung automatisch aufzuheben.

## <a name="prerequisites"></a>Voraussetzungen

Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

*   Einen Azure Active Directory-Mandanten.
*   Sie benötigen einen gültigen Mandanten für Salesforce for Work oder Salesforce for Education. Sie können ein kostenloses Testkonto für einen der Dienste verwenden.
*   Ein Benutzerkonto in Salesforce mit Teamadministratorberechtigung.

## <a name="assigning-users-to-salesforce"></a>Zuweisen von Benutzern zu Salesforce

Azure Active Directory ermittelt anhand von Zuweisungen, welche Benutzer Zugriff auf bestimmte Apps erhalten sollen. Im Kontext der automatischen Bereitstellung von Benutzerkonten werden nur die Benutzer und Gruppen synchronisiert, die einer Anwendung in Azure AD zugewiesen wurden.

Vor dem Konfigurieren und Aktivieren des Bereitstellungsdiensts müssen Sie entscheiden, welche Benutzer und/oder Gruppen in Azure AD die Benutzer darstellen, die Zugriff auf Ihre Salesforce-App benötigen. Anschließend können Sie diese Benutzer Ihrer Salesforce-App zuweisen, indem Sie diese Anweisungen befolgen:

[Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-salesforce"></a>Wichtige Tipps zum Zuweisen von Benutzern zu Salesforce

*   Es wird empfohlen, Salesforce einen einzelnen Azure AD-Benutzer zuzuweisen, um die Konfiguration der Bereitstellung zu testen. Später können weitere Benutzer und/oder Gruppen zugewiesen werden.

*  Wenn Sie einen Benutzer zu Salesforce zuweisen, müssen Sie eine gültige Benutzerrolle auswählen. Die Rolle „Standardzugriff“ funktioniert nicht für die Bereitstellung.

    > [!NOTE]
    > Diese App importiert als Teil des Bereitstellungsprozesses benutzerdefinierte Rollen aus Salesforce, die der Kunde beim Zuweisen von Benutzern auswählen kann.

## <a name="enable-automated-user-provisioning"></a>Aktivieren der automatisierten Benutzerbereitstellung

Dieser Abschnitt führt Sie durch das Herstellen einer Verbindung von Azure AD mit der Salesforce-API zur Bereitstellung von Benutzerkonten sowie durch das Konfigurieren des Bereitstellungsdiensts für das Erstellen, Aktualisieren und Deaktivieren zugewiesener Benutzerkonten in Salesforce basierend auf der Benutzer- und Gruppenzuweisung in Azure AD.

>[!Tip]
>Sie können auch das SAML-basierte einmalige Anmelden für Salesforce aktivieren. Befolgen Sie dazu die Anweisungen im [Azure-Portal](https://portal.azure.com). Einmaliges Anmelden kann unabhängig von der automatischen Bereitstellung konfiguriert werden, obwohl diese beiden Features einander ergänzen.

### <a name="to-configure-automatic-user-account-provisioning"></a>So konfigurieren Sie die automatische Bereitstellung von Benutzerkonten

In diesem Abschnitt wird erläutert, wie Sie die Bereitstellung von Active Directory-Benutzerkonten für Salesforce aktivieren.

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) zum Abschnitt **Azure Active Directory > Unternehmens-Apps > Alle Anwendungen**.

2. Wenn Sie Salesforce bereits für einmaliges Anmelden konfiguriert haben, suchen Sie über das Suchfeld nach Ihrer Salesforce-Instanz. Wählen Sie andernfalls **Hinzufügen** aus, und suchen Sie im Anwendungskatalog nach **Salesforce**. Wählen Sie „Salesforce“ in den Suchergebnissen aus, und fügen Sie die Anwendung Ihrer Anwendungsliste hinzu.

3. Wählen Sie Ihre Salesforce-Instanz aus, und wählen Sie dann die Registerkarte **Bereitstellung** aus.

4. Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest. 
![Bereitstellung](./media/active-directory-saas-salesforce-provisioning-tutorial/provisioning.png)

5. Geben Sie im Abschnitt **Administratoranmeldeinformationen** die folgenden Konfigurationseinstellungen an:
   
    a. Geben Sie im Textfeld **Administratorbenutzername** den Namen eines Salesforce-Kontos ein, dem das Profil **Systemadministrator** in „Salesforce.com“ zugewiesen ist.
   
    b. Geben Sie im Feld **Administratorkennwort** das Kennwort für das Konto ein.

6. Um Ihr Salesforce-Sicherheitstoken abzurufen, öffnen Sie eine neue Registerkarte, und melden Sie sich mit dem gleichen Salesforce-Administratorkonto an. Klicken Sie in der oberen rechten Ecke der Seite auf Ihren Namen und dann auf **Meine Einstellungen**.

     ![Automatische Benutzerbereitstellung aktivieren](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-my-settings.png "Automatische Benutzerbereitstellung aktivieren")
7. Klicken Sie im linken Navigationsbereich auf **Persönlich**, um den entsprechenden Abschnitt zu erweitern, und dann auf **Mein Sicherheitstoken zurücksetzen**.
  
    ![Automatische Benutzerbereitstellung aktivieren](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-personal-reset.png "Automatische Benutzerbereitstellung aktivieren")
8. Klicken Sie auf der Seite **Mein Sicherheitstoken zurücksetzen** auf die Schaltfläche **Sicherheitstoken zurücksetzen**.

    ![Automatische Benutzerbereitstellung aktivieren](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-reset-token.png "Automatische Benutzerbereitstellung aktivieren")
9. Überprüfen Sie den E-Mail-Posteingang dieses Administratorkontos. Achten Sie auf eine E-Mail von Salesforce.com, die das neue Sicherheitstoken enthält.
10. Kopieren Sie das Token, wechseln Sie zu Ihrem Azure AD-Fenster, und fügen Sie es in das Feld **Sockettoken** ein.

11. Klicken Sie im Azure-Portal auf **Verbindung testen**, um sicherzustellen, dass Azure AD eine Verbindung mit Ihrer Salesforce-App herstellen kann.

12. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder einer Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das unten gezeigte Kontrollkästchen.

13. Klicken Sie auf **Speichern**.  
    
14.  Wählen Sie im Abschnitt „Zuordnungen“ die Option **Azure Active Directory-Benutzer mit Salesforce synchronisieren** aus.

15. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure AD mit Salesforce synchronisiert werden. Beachten Sie, dass die als **übereinstimmende** Eigenschaften ausgewählten Attribute für den Abgleich der Benutzerkonten in Salesforce für Updatevorgänge verwendet werden. Wählen Sie die Schaltfläche „Speichern“, um alle Änderungen zu übernehmen.

16. Um den Azure AD-Bereitstellungsdienst für Salesforce zu aktivieren, ändern Sie den **Bereitstellungsstatus** im Abschnitt „Einstellungen“ in **Ein**.

17. Klicken Sie auf **Speichern**.

Dadurch wird die Erstsynchronisierung aller Benutzer und/oder Gruppen gestartet, die Salesforce im Abschnitt „Benutzer und Gruppen“ zugewiesen sind. Beachten Sie, dass die Erstsynchronisierung länger dauert als nachfolgende Synchronisierungen, die ungefähr alle 20 Minuten erfolgen, solange der Dienst ausgeführt wird. Im Abschnitt **Synchronisierungsdetails** können Sie den Fortschritt überwachen und Links zu Berichten zur Bereitstellungsaktivität aufrufen. Darin sind alle Aktionen aufgeführt, die vom Bereitstellungsdienst in Ihrer Salesforce-App ausgeführt werden.

Sie können nun ein Testkonto erstellen. Warten Sie bis zu 20 Minuten, um zu überprüfen, ob das Konto mit Salesforce synchronisiert wurde.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurieren des einmaligen Anmeldens](active-directory-saas-salesforce-tutorial.md)
