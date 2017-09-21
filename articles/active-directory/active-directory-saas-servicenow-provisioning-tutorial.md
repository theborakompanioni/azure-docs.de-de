---
title: "Tutorial: Konfigurieren von ServiceNow für die automatische Benutzerbereitstellung in Azure Active Directory | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie Benutzerkonten aus Azure AD für ServiceNow automatisch bereitstellen bzw. die Bereitstellung aufheben."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4d6f06dd-a798-4c22-b84f-8a11f1b8592a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: e338804a0c5fcb09dfdfd9bf18ae4d0466cd9a82
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---
# <a name="tutorial-configuring-servicenow-for-automatic-user-provisioning-with-azure-active-directory"></a>Tutorial: Konfigurieren von ServiceNow für die automatische Benutzerbereitstellung in Azure Active Directory

Dieses Tutorial zeigt Ihnen die Schritte, die Sie in ServiceNow und Azure AD ausführen müssen, um Benutzerkonten von Azure AD in ServiceNow automatisch bereitzustellen bzw. deren Bereitstellung automatisch aufzuheben.

## <a name="prerequisites"></a>Voraussetzungen

Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

*   Einen Azure Active Directory-Mandanten
*   Sie benötigen einen gültigen Mandanten für ServiceNow for Work oder ServiceNow for Education. Sie können ein kostenloses Testkonto für einen der Dienste verwenden.
*   Ein Benutzerkonto in ServiceNow mit Teamadministratorberechtigung

## <a name="assigning-users-to-servicenow"></a>Zuweisen von Benutzern zu ServiceNow

Azure Active Directory ermittelt anhand von Zuweisungen, welche Benutzer Zugriff auf bestimmte Apps erhalten sollen. Im Kontext der automatischen Bereitstellung von Benutzerkonten werden nur die Benutzer und Gruppen synchronisiert, die einer Anwendung in Azure AD zugewiesen wurden.

Vor dem Konfigurieren und Aktivieren des Bereitstellungsdiensts müssen Sie entscheiden, welche Benutzer und/oder Gruppen in Azure AD die Benutzer darstellen, die Zugriff auf Ihre ServiceNow-App benötigen. Nachdem die Entscheidung getroffen wurde, können Sie diese Benutzer Ihrer ServiceNow-App zuweisen, indem Sie die Anleitung in diesem Artikel befolgen: [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).


> [!IMPORTANT]
>*   Es wird empfohlen, ServiceNow einen einzelnen Azure AD-Benutzer zuzuweisen, um die Konfiguration der Bereitstellung zu testen. Später können weitere Benutzer und/oder Gruppen zugewiesen werden.
>*   Wenn Sie ServiceNow einen Benutzer zuweisen, müssen Sie eine gültige Benutzerrolle auswählen. Die Rolle „Standardzugriff“ funktioniert nicht für die Bereitstellung.

## <a name="enable-automated-user-provisioning"></a>Aktivieren der automatisierten Benutzerbereitstellung

Dieser Abschnitt führt Sie durch das Herstellen einer Verbindung von Azure AD mit der ServiceNow-API zur Bereitstellung von Benutzerkonten sowie durch das Konfigurieren des Bereitstellungsdiensts für das Erstellen, Aktualisieren und Deaktivieren zugewiesener Benutzerkonten in ServiceNow basierend auf der Benutzer- und Gruppenzuweisung in Azure AD.

> [!TIP]
>Sie können auch das SAML-basierte einmalige Anmelden für ServiceNow aktivieren. Befolgen Sie dazu die Anweisungen im [Azure-Portal](https://portal.azure.com). Einmaliges Anmelden kann unabhängig von der automatischen Bereitstellung konfiguriert werden, obwohl diese beiden Features einander ergänzen.

### <a name="configure-automatic-user-account-provisioning"></a>Konfigurieren der automatischen Bereitstellung von Benutzerkonten

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) zum Abschnitt **Azure Active Directory > Unternehmens-Apps > Alle Anwendungen**.

2. Wenn Sie ServiceNow bereits für einmaliges Anmelden konfiguriert haben, suchen Sie über das Suchfeld nach Ihrer ServiceNow-Instanz. Wählen Sie andernfalls **Hinzufügen** aus, und suchen Sie im Anwendungskatalog nach **ServiceNow**. Wählen Sie ServiceNow in den Suchergebnissen aus, und fügen Sie die Anwendung Ihrer Anwendungsliste hinzu.

3. Wählen Sie Ihre ServiceNow-Instanz aus, und wählen Sie dann die Registerkarte **Bereitstellung**.

4. Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest. 

    ![Bereitstellung](./media/active-directory-saas-servicenow-provisioning-tutorial/provisioning.png)

5. Führen Sie im Abschnitt „Administratoranmeldeinformationen“ die folgenden Schritte aus:
   
    a. Geben Sie im Textfeld **ServiceNow-Instanzname** den Namen des ServiceNow-Instanz ein.

    b. Geben Sie im Textfeld **ServiceNow-Administratorbenutzername** den Benutzernamen eines Administrators ein.

    c. Geben Sie im Textfeld **ServiceNow-Administratorkennwort** das Kennwort des Administrators ein.

6. Klicken Sie im Azure-Portal auf **Verbindung testen**, um sicherzustellen, dass Azure AD eine Verbindung mit Ihrer ServiceNow-App herstellen kann. Wenn die Verbindung nicht möglich ist, stellen Sie sicher, dass Ihr ServiceNow-Konto über Teamadministratorberechtigungen verfügt, und führen Sie den Schritt **Administratoranmeldeinformationen** erneut aus.

7. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder einer Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das Kontrollkästchen.

8. Klicken Sie auf **Speichern**.

9. Wählen Sie im Abschnitt „Zuordnungen“ die Option **Azure Active Directory-Benutzer mit ServiceNow synchronisieren** aus.

10. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure AD mit ServiceNow synchronisiert werden. Beachten Sie, dass die als **übereinstimmende** Eigenschaften ausgewählten Attribute zum Abgleichen der Benutzerkonten in ServiceNow bei Updatevorgängen verwendet werden. Wählen Sie die Schaltfläche „Speichern“, um alle Änderungen zu übernehmen.

11. Um den Azure AD-Bereitstellungsdienst für ServiceNow zu aktivieren, ändern Sie den **Bereitstellungsstatus** im Abschnitt „Einstellungen“ in **Ein**.

12. Klicken Sie auf **Speichern**.

Dadurch wird die Erstsynchronisierung aller Benutzer und/oder Gruppen gestartet, die ServiceNow im Abschnitt „Benutzer und Gruppen“ zugewiesen sind. Die Erstsynchronisierung dauert länger als nachfolgende Synchronisierungen, die ungefähr alle 20 Minuten erfolgen, solange der Dienst ausgeführt wird. Im Abschnitt **Synchronisierungsdetails** können Sie den Fortschritt überwachen und Links zu Berichten über Bereitstellungsaktivitäten aufrufen. Darin sind alle Aktionen aufgeführt, die vom Bereitstellungsdienst in Ihrer ServiceNow-App ausgeführt werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurieren des einmaligen Anmeldens](active-directory-saas-servicenow-tutorial.md)



