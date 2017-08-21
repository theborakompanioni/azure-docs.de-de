---
title: 'Tutorial: Azure Active Directory-Integration mit Citrix GoToMeeting | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Citrix GoToMeeting konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0f59fedb-2cf8-48d2-a5fb-222ed943ff78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: 1ddfcd991431a11e5c3e306bd5905003d094ac18
ms.contentlocale: de-de
ms.lasthandoff: 07/08/2017


---
# <a name="tutorial-configuring-citrix-gotomeeting-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von GoToMeeting für die automatische Benutzerbereitstellung

Dieses Tutorial zeigt Ihnen die Schritte, die Sie in Citrix GoToMeeting und Azure AD ausführen müssen, um Benutzerkonten von Azure AD an Citrix GoToMeeting automatisch bereitzustellen bzw. deren Bereitstellung automatisch aufzuheben.

## <a name="prerequisites"></a>Voraussetzungen

Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

*   Einen Azure Active Directory-Mandanten
*   Ein Citrix GoToMeeting-Abonnement, für das einmaliges Anmelden aktiviert ist
*   Ein Benutzerkonto in Citrix GoToMeeting mit Teamadministratorberechtigung

## <a name="assigning-users-to-citrix-gotomeeting"></a>Zuweisen von Benutzern zu Citrix GoToMeeting

Azure Active Directory ermittelt anhand von Zuweisungen, welche Benutzer Zugriff auf bestimmte Apps erhalten sollen. Im Kontext der automatischen Bereitstellung von Benutzerkonten werden nur die Benutzer und Gruppen synchronisiert, die einer Anwendung in Azure AD zugewiesen wurden.

Vor dem Konfigurieren und Aktivieren des Bereitstellungsdiensts müssen Sie entscheiden, welche Benutzer und/oder Gruppen in Azure AD die Benutzer darstellen, die Zugriff auf Ihre Citrix GoToMeeting-App benötigen. Anschließend können Sie diese Benutzer Ihrer Citrix GoToMeeting-App zuweisen, indem Sie diese Anweisungen befolgen:

[Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-citrix-gotomeeting"></a>Wichtige Tipps zum Zuweisen von Benutzern zu Citrix GoToMeeting

*   Es wird empfohlen, Citrix GoToMeeting einen einzelnen Azure AD-Benutzer zuzuweisen, um die Konfiguration der Bereitstellung zu testen. Später können weitere Benutzer und/oder Gruppen zugewiesen werden.

*   Wenn Sie Citrix GoToMeeting einen Benutzer zuweisen, müssen Sie eine gültige Benutzerrolle auswählen. Die Rolle „Standardzugriff“ funktioniert nicht für die Bereitstellung.

## <a name="enable-automated-user-provisioning"></a>Aktivieren der automatisierten Benutzerbereitstellung

In diesem Abschnitt wird das Verbinden von Azure AD mit der Citrix GoToMeeting-API zur Bereitstellung von Benutzerkonten sowie das Konfigurieren des Bereitstellungsdiensts für das Erstellen, Aktualisieren und Deaktivieren zugewiesener Benutzerkonten in Citrix GoToMeeting basierend auf der Benutzer- und Gruppenzuweisung in Azure AD beschrieben.

> [!TIP]
> Sie können auch das SAML-basierte einmalige Anmelden für Citrix GoToMeeting aktivieren. Befolgen Sie dazu die Anweisungen im [Azure-Portal](https://portal.azure.com). Einmaliges Anmelden kann unabhängig von der automatischen Bereitstellung konfiguriert werden, obwohl diese beiden Features einander ergänzen.

### <a name="to-configure-automatic-user-account-provisioning"></a>So konfigurieren Sie die automatische Bereitstellung von Benutzerkonten:

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) zum Abschnitt **Azure Active Directory > Unternehmens-Apps > Alle Anwendungen**.

2. Wenn Sie Citrix GoToMeeting bereits für einmaliges Anmelden konfiguriert haben, suchen Sie über das Suchfeld nach Ihrer Citrix GoToMeeting-Instanz. Wählen Sie andernfalls **Hinzufügen** aus, und suchen Sie im Anwendungskatalog nach **Citrix GoToMeeting**. Wählen Sie „Citrix GoToMeeting“ in den Suchergebnissen aus, und fügen Sie es Ihrer Anwendungsliste hinzu.

3. Wählen Sie Ihre Citrix GoToMeeting-Instanz und dann die Registerkarte **Bereitstellung** aus.

4. Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest. 

    ![Bereitstellung](./media/active-directory-saas-citrixgotomeeting-provisioning-tutorial/provisioning.png)

5. Führen Sie im Abschnitt „Administratoranmeldeinformationen“ die folgenden Schritte aus:
   
    a. Geben Sie im Textfeld **Citrix GotoMeeting-Administratorbenutzername** den Benutzernamen eines Administrators ein.

    b. Geben Sie im Textfeld **Citrix GoToMeeting-Administratorkennwort** das Kennwort des Administrators ein.

6. Klicken Sie im Azure-Portal auf **Verbindung testen**, um sicherzustellen, dass Azure AD eine Verbindung zu Ihrer Citrix GoToMeeting-App herstellen kann. Wenn die Verbindung nicht möglich ist, stellen Sie sicher, dass Ihr Citrix GoToMeeting-Konto über Teamadministratorberechtigungen verfügt, und versuchen Sie den Schritt **Administratoranmeldeinformationen** erneut.

7. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder einer Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das Kontrollkästchen.

8. Klicken Sie auf **Speichern**.

9. Wählen Sie im Abschnitt „Zuordnungen“ die Option **Azure Active Directory-Benutzer mit Citrix GoToMeeting synchronisieren** aus.

10. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure AD mit Citrix GoToMeeting synchronisiert werden. Die als **übereinstimmende** Eigenschaften ausgewählten Attribute werden für den Abgleich der Benutzerkonten in Citrix GoToMeeting für Updatevorgänge verwendet. Wählen Sie die Schaltfläche „Speichern“, um alle Änderungen zu übernehmen.

11. Ändern Sie den **Bereitstellungsstatus** im Abschnitt „Einstellungen“ in **Ein**, um den Azure AD-Bereitstellungsdienst für Citrix GoToMeeting zu aktivieren.

12. Klicken Sie auf **Speichern**.

Dadurch wird die Erstsynchronisierung aller Benutzer und/oder Gruppen gestartet, die Citrix GoToMeeting im Abschnitt „Benutzer und Gruppen“ zugewiesen sind. Die Erstsynchronisierung dauert länger als nachfolgende Synchronisierungen, die ungefähr alle 20 Minuten erfolgen, solange der Dienst ausgeführt wird. Im Abschnitt **Synchronisierungsdetails** können Sie den Fortschritt überwachen und Links zu Berichten zur Bereitstellungsaktivität aufrufen. Darin sind alle Aktionen aufgeführt, die vom Bereitstellungsdienst in Ihrer Citrix GoToMeeting-App ausgeführt werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Configure Single Sign-on (Konfigurieren des einmaligen Anmeldens)](active-directory-saas-citrix-gotomeeting-tutorial.md)



