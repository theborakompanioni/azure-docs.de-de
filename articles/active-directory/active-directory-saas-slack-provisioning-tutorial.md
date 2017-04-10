---
title: "Tutorial: Konfigurieren von Slack für die automatische Benutzerbereitstellung in Azure Active Directory | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Azure Active Directory für das automatische Bereitstellen und Aufheben der Bereitstellung von Benutzerkonten in Slack konfigurieren."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: stevenpo
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: asmalser-msft
translationtype: Human Translation
ms.sourcegitcommit: a3c339de98250f43c71f7dd9706888c6715ca2ef
ms.openlocfilehash: e39f2202e83cdf66b85f1cc02a9320ec9e31472e
ms.lasthandoff: 02/08/2017

---

# <a name="tutorial-configuring-slack-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von Slack für die automatische Benutzerbereitstellung


Dieses Tutorial zeigt Ihnen die Schritte, die Sie in Slack und Azure AD ausführen müssen, um Benutzerkonten von Azure AD in Slack automatisch bereitzustellen bzw. deren Bereitstellung automatisch aufzuheben. 

## <a name="prerequisites"></a>Voraussetzungen

Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

*   Einen Azure Active Directory-Mandanten
*   Einen aktivierten Slack-Mandanten mit dem [Plus-Tarif](https://aadsyncfabric.slack.com/pricing) oder höher 
*   Ein Benutzerkonto in Slack mit Teamadministratorberechtigung 

Hinweis: Die Integration der Azure AD-Bereitstellung basiert auf der [Slack-SCIM-API](https://api.slack.com/scim), die für Slack-Teams mit dem Plus-Tarif oder einem höheren Tarif zur Verfügung steht.

## <a name="assigning-users-to-slack"></a>Zuweisen von Benutzern zu Slack

Azure Active Directory ermittelt anhand von Zuweisungen, welche Benutzer Zugriff auf bestimmte Apps erhalten sollen. Im Kontext der automatischen Bereitstellung von Benutzerkonten werden nur die Benutzer und Gruppen synchronisiert, die einer Anwendung in Azure AD zugewiesen wurden. 

Vor dem Konfigurieren und Aktivieren des Bereitstellungsdiensts müssen Sie entscheiden, welche Benutzer und/oder Gruppen in Azure AD die Benutzer darstellen, die Zugriff auf Ihre Slack-App benötigen. Anschließend können Sie diese Benutzer Ihrer Slack-App zuweisen, indem Sie diese Anweisungen befolgen:

[Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-slack"></a>Wichtige Tipps zum Zuweisen von Benutzern zu Slack

*    Es wird empfohlen, Slack einen einzelnen Azure AD-Benutzer zuzuweisen, um die Konfiguration der Bereitstellung zu testen. Später können weitere Benutzer und/oder Gruppen zugewiesen werden.

*    Beim Zuweisen eines Benutzers zu Slack müssen Sie im Dialogfeld „Zuweisung“ die Rolle **Benutzer** oder „Gruppe“ auswählen. Die Rolle „Standardzugriff“ funktioniert nicht für die Bereitstellung.


## <a name="configuring-user-provisioning-to-slack"></a>Konfigurieren der Benutzerbereitstellung in Slack 

Dieser Abschnitt führt Sie durch das Herstellen einer Verbindung von Azure AD mit der Slack-API zur Bereitstellung von Benutzerkonten sowie durch das Konfigurieren des Bereitstellungsdiensts für das Erstellen, Aktualisieren und Deaktivieren zugewiesener Benutzerkonten in Slack basierend auf der Benutzer- und Gruppenzuweisung in Azure AD.

**Tipp:** Optional können Sie das SAML-basierte einmalige Anmelden für Slack aktivieren. Befolgen Sie dazu die Anweisungen im (Azure-Portal)[https://portal.azure.com]. Einmaliges Anmelden kann unabhängig von der automatischen Bereitstellung konfiguriert werden, obwohl diese beiden Features einander ergänzen.


### <a name="to-configure-automatic-user-account-provisioning-to-slack-in-azure-ad"></a>So konfigurieren Sie die automatische Bereitstellung von Benutzerkonten für Slack in Azure AD:


1)    Wechseln Sie im [Azure-Portal](https://portal.azure.com) zum Abschnitt **Azure Active Directory > Unternehmens-Apps > Alle Anwendungen**.

2) Wenn Sie Slack bereits für einmaliges Anmelden konfiguriert haben, suchen Sie über das Suchfeld nach Ihrer Slack-Instanz. Wählen Sie andernfalls **Hinzufügen**, und suchen Sie im Anwendungskatalog nach **Slack**. Wählen Sie „Slack“ in den Suchergebnissen aus, und fügen Sie es Ihrer Anwendungsliste hinzu.

3)    Wählen Sie Ihre Slack-Instanz aus, und wählen Sie dann die Registerkarte **Bereitstellung**.

4)    Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest.

![Slack-Bereitstellung](./media/active-directory-saas-slack-provisioning-tutorial/Slack1.PNG)

5)    Klicken Sie im Abschnitt **Administratoranmeldeinformationen** auf **Autorisieren**. Dadurch wird ein Slack-Autorisierungsdialogfeld in einem neuen Browserfenster geöffnet. 

6) Melden Sie sich im neuen Fenster mit Ihrem Teamadministratorkonto bei Slack an. Wählen Sie im daraufhin angezeigten Autorisierungsdialogfeld das Slack-Team aus, für das Sie die Bereitstellung aktivieren möchten, und wählen Sie dann **Autorisieren**. Kehren Sie anschließend zum Azure-Portal zurück, um die Konfiguration der Bereitstellung abzuschließen.

![Autorisierungsdialogfeld](./media/active-directory-saas-slack-provisioning-tutorial/Slack3.PNG)

7) Klicken Sie im Azure-Portal auf **Verbindung testen**, um sicherzustellen, dass Azure AD eine Verbindung mit Ihrer Slack-App herstellen kann. Wenn die Verbindung nicht möglich ist, stellen Sie sicher, dass Ihr Slack-Konto über Teamadministratorberechtigungen verfügt, und wiederholen Sie den Schritt zum Autorisieren.

8) Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder einer Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das unten gezeigte Kontrollkästchen.

9) Klicken Sie auf **Speichern**. 

10) Wählen Sie im Abschnitt „Zuordnungen“ die Option **Azure Active Directory-Benutzer mit Slack synchronisieren**.

11) Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure AD mit Slack synchronisiert werden. Beachten Sie, dass die als **übereinstimmende** Eigenschaften ausgewählten Attribute für den Abgleich der Benutzerkonten in Slack für Updatevorgänge verwendet werden. Wählen Sie die Schaltfläche „Speichern“, um alle Änderungen zu übernehmen.

12) Um den Azure AD-Bereitstellungsdienst für Slack zu aktivieren, ändern Sie den **Bereitstellungsstatus** im Abschnitt **Einstellungen** in **Ein**.

13) Klicken Sie auf **Speichern**. 

Dadurch wird die Erstsynchronisierung aller Benutzer und/oder Gruppen gestartet, die Slack im Abschnitt „Benutzer und Gruppen“ zugewiesen sind. Beachten Sie, dass die Erstsynchronisierung länger dauert als nachfolgende Synchronisierungen, die ungefähr alle 10 Minuten erfolgen, solange der Dienst ausgeführt wird. Im Abschnitt **Synchronisierungsdetails** können Sie den Fortschritt überwachen und Links zu Berichten zur Bereitstellungsaktivität aufrufen. Darin sind alle Aktionen aufgeführt, die vom Bereitstellungsdienst in Ihrer Slack-App ausgeführt werden.

## <a name="optional-configuring-group-object-provisioning-to-slack"></a>[Optional] Konfigurieren der Gruppenobjektbereitstellung in Slack 

Optional können Sie die Bereitstellung von Gruppenobjekten von Azure AD in Slack aktivieren. Dies unterscheidet sich von der Zuweisung von Benutzergruppen insofern, als dass das eigentliche Gruppenobjekt zusätzlich zu seinen Mitgliedern von Azure AD in Slack repliziert wird. Wenn Sie in Azure AD beispielsweise eine Gruppe namens „Meine Gruppe“ verwenden, wird eine identische Gruppe namens „Meine Gruppe“ in Slack erstellt.

### <a name="to-enable-provisioning-of-group-objects"></a>So aktivieren Sie die Bereitstellung von Gruppenobjekten

1) Wählen Sie im Abschnitt „Zuordnungen“ die Option **Azure Active Directory-Gruppen mit Slack synchronisieren**.

2) Legen Sie auf dem Blatt „Attributzuordnung“ die Option „Aktiviert“ auf „Ja“ fest.

3) Überprüfen Sie im Abschnitt **Attributzuordnungen** die Gruppenattribute, die von Azure AD mit Slack synchronisiert werden. Beachten Sie, dass die als **übereinstimmende** Eigenschaften ausgewählten Attribute für den Abgleich der Gruppen in Slack für Updatevorgänge verwendet werden. 

4) Klicken Sie auf **Speichern**.

So werden alle Gruppenobjekte, die Slack im Abschnitt **Benutzer und Gruppen** zugewiesen sind, vollständig von Azure AD nach Slack synchronisiert. Im Abschnitt **Synchronisierungsdetails** können Sie den Fortschritt überwachen und Links zu Berichten zur Bereitstellungsaktivität aufrufen. Darin sind alle Aktionen aufgeführt, die vom Bereitstellungsdienst in Ihrer Slack-App ausgeführt werden.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](active-directory-enterprise-apps-manage-provisioning.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

