---
title: 'Tutorial: Azure Active Directory-Integration mit NetSuite | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Netsuite konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8a6d3994-ee33-4a6f-b0a2-9d0389467f16
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: 277c393536615fc8bfe8af0bc6d487115f04776c
ms.contentlocale: de-de
ms.lasthandoff: 07/08/2017


---
# <a name="tutorial-configuring-netsuite-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von Netsuite für die automatische Benutzerbereitstellung

In diesem Tutorial werden die Schritte beschrieben, die Sie in Netsuite und Azure AD ausführen müssen, um Benutzerkonten von Azure AD in Netsuite automatisch bereitzustellen bzw. deren Bereitstellung automatisch aufzuheben.

## <a name="prerequisites"></a>Voraussetzungen

Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

*   Einen Azure Active Directory-Mandanten
*   Ein Netsuite-Abonnement, für das einmaliges Anmelden aktiviert ist
*   Ein Benutzerkonto in Netsuite mit Teamadministratorberechtigungen

## <a name="assigning-users-to-netsuite"></a>Zuweisen von Benutzern zu Netsuite

Azure Active Directory ermittelt anhand von Zuweisungen, welche Benutzer Zugriff auf bestimmte Apps erhalten sollen. Im Kontext der automatischen Bereitstellung von Benutzerkonten werden nur die Benutzer und Gruppen synchronisiert, die einer Anwendung in Azure AD zugewiesen wurden.

Vor dem Konfigurieren und Aktivieren des Bereitstellungsdiensts müssen Sie entscheiden, welche Benutzer und/oder Gruppen in Azure AD die Benutzer darstellen, die Zugriff auf Ihre Netsuite-App benötigen. Anschließend können Sie diese Benutzer Ihrer Netsuite-App zuweisen, indem Sie diese Anweisungen befolgen:

[Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-netsuite"></a>Wichtige Tipps zum Zuweisen von Benutzern zu Netsuite

*   Es wird empfohlen, Netsuite einen einzelnen Azure AD-Benutzer zuzuweisen, um die Konfiguration der Bereitstellung zu testen. Später können weitere Benutzer und/oder Gruppen zugewiesen werden.

*   Wenn Sie Netsuite einen Benutzer zuweisen, müssen Sie eine gültige Benutzerrolle auswählen. Die Rolle „Standardzugriff“ funktioniert nicht für die Bereitstellung.

## <a name="enable-user-provisioning"></a>Aktivieren der Benutzerbereitstellung

In diesem Abschnitt wird das Herstellen einer Verbindung von Azure AD mit der Netsuite-API zur Bereitstellung von Benutzerkonten sowie das Konfigurieren des Bereitstellungsdiensts für das Erstellen, Aktualisieren und Deaktivieren zugewiesener Benutzerkonten in Netsuite basierend auf der Benutzer- und Gruppenzuweisung in Azure AD beschrieben.

> [!TIP] 
> Sie können auch das SAML-basierte einmalige Anmelden für Netsuite aktivieren. Befolgen Sie hierzu die Anweisungen im [Azure-Portal](https://portal.azure.com). Einmaliges Anmelden kann unabhängig von der automatischen Bereitstellung konfiguriert werden, obwohl diese beiden Features einander ergänzen.

### <a name="to-configure-user-account-provisioning"></a>Gehen Sie folgt vor, um die Bereitstellung von Benutzerkonten zu konfigurieren:

In diesem Abschnitt wird erläutert, wie Sie die Bereitstellung von Active Directory-Benutzerkonten für Netsuite aktivieren.

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) zum Abschnitt **Azure Active Directory > Unternehmens-Apps > Alle Anwendungen**.

2. Suchen Sie über das Suchfeld nach Ihrer Netsuite-Instanz, wenn Sie Netsuite bereits für einmaliges Anmelden konfiguriert haben. Wählen Sie andernfalls **Hinzufügen**, und suchen Sie im Anwendungskatalog nach **Netsuite**. Wählen Sie Netsuite in den Suchergebnissen aus, und fügen Sie es Ihrer Anwendungsliste hinzu.

3. Wählen Sie Ihre Netsuite-Instanz aus, und wählen Sie dann die Registerkarte **Bereitstellung**.

4. Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest. 

    ![Bereitstellung](./media/active-directory-saas-netsuite-provisioning-tutorial/provisioning.png)

5. Geben Sie im Abschnitt **Administratoranmeldeinformationen** die folgenden Konfigurationseinstellungen an:
   
    a. Geben Sie im Textfeld für den **Benutzernamen des Administrators** den Namen eines Netsuite-Kontos ein, dem das Profil **Systemadministrator** auf „Netsuite.com“ zugewiesen ist.
   
    b. Geben Sie im Feld **Administratorkennwort** das Kennwort für das Konto ein.
      
6. Klicken Sie im Azure-Portal auf **Verbindung testen**, um sicherzustellen, dass Azure AD eine Verbindung mit Ihrer Netsuite-App herstellen kann.

7. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder einer Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das Kontrollkästchen.

8. Klicken Sie auf **Speichern**.

9. Wählen Sie im Abschnitt „Zuordnungen“ die Option **Azure Active Directory-Benutzer mit Netsuite synchronisieren**.

10. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure AD mit Netsuite synchronisiert werden. Beachten Sie, dass die als **übereinstimmende** Eigenschaften ausgewählten Attribute für den Abgleich der Benutzerkonten in Netsuite für Updatevorgänge verwendet werden. Wählen Sie die Schaltfläche „Speichern“, um alle Änderungen zu übernehmen.

11. Um den Azure AD-Bereitstellungsdienst für Netsuite zu aktivieren, ändern Sie den **Bereitstellungsstatus** im Abschnitt „Einstellungen“ in **Ein**.

12. Klicken Sie auf **Speichern**.

Dadurch wird die Erstsynchronisierung aller Benutzer und/oder Gruppen gestartet, die Netsuite im Abschnitt „Benutzer und Gruppen“ zugewiesen sind. Beachten Sie, dass die Erstsynchronisierung länger dauert als nachfolgende Synchronisierungen, die ungefähr alle 20 Minuten erfolgen, solange der Dienst ausgeführt wird. Im Abschnitt **Synchronisierungsdetails** können Sie den Fortschritt überwachen und Links zu Berichten zur Bereitstellungsaktivität aufrufen. Darin sind alle Aktionen aufgeführt, die vom Bereitstellungsdienst in Ihrer Netsuite-App ausgeführt werden.

Sie können nun ein Testkonto erstellen. Warten Sie bis zu 20 Minuten, bevor Sie überprüfen, ob das Konto mit Netsuite synchronisiert wurde.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Tutorial: Azure Active Directory-Integration mit NetSuite](active-directory-saas-netsuite-tutorial.md)
