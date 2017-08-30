---
title: "Tutorial: Konfigurieren von Workplace by Facebook für die Benutzerbereitstellung | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie Benutzerkonten aus Azure AD in Workplace by Facebook automatisch bereitstellen bzw. die Bereitstellung aufheben.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6341e67e-8ce6-42dc-a4ea-7295904a53ef
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: a347eedbf5511dc83e1bc7721667441cfb87cb59
ms.contentlocale: de-de
ms.lasthandoff: 08/23/2017

---
# <a name="tutorial-configure-workplace-by-facebook-for-user-provisioning"></a>Tutorial: Konfigurieren von Workplace by Facebook für die Benutzerbereitstellung

Dieses Tutorial veranschaulicht die erforderlichen Schritte zur automatischen Bereitstellung (und Aufhebung der Bereitstellung) von Benutzerkonten aus Azure Active Directory (Azure AD) in Workplace by Facebook.

## <a name="prerequisites"></a>Voraussetzungen

Für das Konfigurieren der Azure AD-Integration mit Workplace by Facebook benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Workplace by Facebook-Abonnement mit aktiviertem einmaligen Anmelden

Beachten Sie beim Testen der Schritte in diesem Tutorial die folgenden Empfehlungen:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [eine einmonatige Testversion](https://azure.microsoft.com/pricing/free-trial/) anfordern.

## <a name="assign-users-to-workplace-by-facebook"></a>Zuweisen von Benutzern zu Workplace by Facebook

Azure AD ermittelt anhand von Zuweisungen, welche Benutzer Zugriff auf ausgewählte Apps erhalten sollen. Im Kontext der automatischen Bereitstellung von Benutzerkonten werden nur die Benutzer und Gruppen synchronisiert, die einer Anwendung in Azure AD zugewiesen wurden.

Entscheiden Sie vor dem Konfigurieren und Aktivieren des Bereitstellungsdiensts, welche Benutzer und/oder Gruppen in Azure AD die Benutzer darstellen, die Zugriff auf Ihre Workplace by Facebook-App benötigen. Sie können diese Benutzer anschließend Ihrer Workplace by Facebook-App zuweisen, indem Sie die Anleitung unter [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) befolgen.

>[!IMPORTANT]
>*   Weisen Sie Workplace by Facebook einen einzelnen Azure AD-Benutzer zu, um die Konfiguration der Bereitstellung zu testen. Weisen Sie später weitere Benutzer und Gruppen zu.
>*   Wenn Sie Workplace by Facebook einen Benutzer zuweisen, müssen Sie eine gültige Benutzerrolle auswählen. Die Rolle „Standardzugriff“ funktioniert nicht für die Bereitstellung.

## <a name="enable-automated-user-provisioning"></a>Aktivieren der automatisierten Benutzerbereitstellung

Dieser Abschnitt führt Sie durch das Verbinden Ihres Azure AD mit der API zum Bereitstellen von Benutzerkonten von Workplace by Facebook. Außerdem erfahren Sie, wie Sie den Bereitstellungsdienst zum Erstellen, Aktualisieren und Deaktivieren von in Workplace by Facebook zugewiesenen Benutzerkonten konfigurieren. Dies basiert auf der Benutzer- und Gruppenzuweisung in Azure AD.

>[!Tip]
>Sie können auch das SAML-basierte einmalige Anmelden für Workplace by Facebook aktivieren. Befolgen Sie dazu die Anweisungen im [Azure-Portal](https://portal.azure.com). Einmaliges Anmelden kann unabhängig von der automatischen Bereitstellung konfiguriert werden, obwohl diese beiden Features einander ergänzen.

### <a name="configure-user-account-provisioning-to-workplace-by-facebook-in-azure-ad"></a>Konfigurieren der Benutzerkontenbereitstellung an Workplace by Facebook in Azure AD

Azure AD unterstützt die Möglichkeit, die Kontodetails zugewiesener Benutzer mit Workplace by Facebook zu synchronisieren. Dank dieser automatischen Synchronisierung kann Workplace by Facebook die erforderlichen Daten für die Autorisierung von Benutzern für den Zugriff abrufen, bevor diese sich zum ersten Mal anmelden. Sie hebt zudem die Bereitstellung von Benutzern in Workplace by Facebook auf, wenn der Zugriff in Azure AD widerrufen wurde.

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) zu **Azure Active Directory** > **Unternehmens-Apps** > **Alle Anwendungen**.

2. Wenn Sie Workplace by Facebook bereits für einmaliges Anmelden konfiguriert haben, suchen Sie über das Suchfeld nach Ihrer Workplace by Facebook-Instanz. Klicken Sie andernfalls auf **Hinzufügen**, und suchen Sie im Anwendungskatalog nach **Workplace by Facebook**. Wählen Sie in den Suchergebnissen **Workplace by Facebook** aus, und fügen Sie es Ihrer Anwendungsliste hinzu.

3. Wählen Sie Ihre Workplace by Facebook-Instanz und dann die Registerkarte **Bereitstellung** aus.

4. Legen Sie **Bereitstellungsmodus** auf **Automatisch** fest. 

    ![Screenshot der Workplace by Facebook-Bereitstellungsoptionen](./media/active-directory-saas-facebook-at-work-provisioning-tutorial/provisioning.png)

5. Geben Sie im Abschnitt **Admin Credentials** (Administratoranmeldeinformationen) das **Secret Token** (Geheime Token) und die **Tenant URL** (Mandanten-URL) Ihres Workplace by Facebook-Administrators ein.

6. Klicken Sie im Azure-Portal auf **Verbindung testen**, um sicherzustellen, dass Azure AD eine Verbindung mit Ihrer Workplace by Facebook-App herstellen kann. Wenn die Verbindung nicht hergestellt werden kann, stellen Sie sicher, dass Ihr Workplace by Facebook-Konto über Teamadministratorberechtigungen verfügt.

7. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das Kontrollkästchen.

8. Wählen Sie **Speichern** aus.

9. Wählen Sie im Abschnitt „Zuordnungen“ die Option **Azure Active Directory-Benutzer mit Workplace by Facebook synchronisieren** aus.

10. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure AD mit Workplace by Facebook synchronisiert werden. Die als **übereinstimmende** Eigenschaften ausgewählten Attribute werden für den Abgleich der Benutzerkonten in Workplace by Facebook für Updatevorgänge verwendet. Um alle Änderungen zu speichern, klicken Sie auf **Speichern**.

11. Ändern Sie im Abschnitt **Einstellungen** den **Bereitstellungsstatus** in **Ein**, um den Azure AD-Bereitstellungsdienst für Workplace by Facebook zu aktivieren.

12. Wählen Sie **Speichern** aus.

Weitere Informationen zum Konfigurieren der automatischen Bereitstellung finden Sie in der [Facebook-Dokumentation](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers).

Sie können nun ein Testkonto erstellen. Warten Sie bis zu 20 Minuten, um zu überprüfen, ob das Konto mit Workplace by Facebook synchronisiert wurde.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurieren von einmaligem Anmelden](active-directory-saas-facebook-at-work-tutorial.md)


