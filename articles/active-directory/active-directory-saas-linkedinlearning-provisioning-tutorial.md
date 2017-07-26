---
title: "Tutorial: Konfigurieren von LinkedIn Learning für die automatische Benutzerbereitstellung in Azure Active Directory | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Azure Active Directory für das automatische Bereitstellen und Aufheben der Bereitstellung von Benutzerkonten in LinkedIn Learning konfigurieren."
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
ms.date: 04/15/2017
ms.author: asmalser-msft
ms.translationtype: Human Translation
ms.sourcegitcommit: a643f139be40b9b11f865d528622bafbe7dec939
ms.openlocfilehash: 5eb2b1594eedb2a135d7b8cd501a33d8264e136b
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017

---

# <a name="tutorial-configuring-linkedin-learning-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von LinkedIn Learning für die automatische Benutzerbereitstellung


Dieses Tutorial zeigt Ihnen die Schritte, die Sie in LinkedIn Learning und Azure AD ausführen müssen, um Benutzerkonten von Azure AD in LinkedIn Learning automatisch bereitzustellen bzw. deren Bereitstellung automatisch aufzuheben. 

## <a name="prerequisites"></a>Voraussetzungen

Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

*   Azure Active Directory-Mandant
*   Einen LinkedIn Learning-Mandanten 
*   Ein Administratorkonto in LinkedIn Learning mit Zugriff auf das LinkedIn Account Center

> [!NOTE]
> Die Integration von Azure Active Directory mit LinkedIn Learning erfolgt über das [SCIM](http://www.simplecloud.info/)-Protokoll.

## <a name="assigning-users-to-linkedin-learning"></a>Zuweisen von Benutzern zu LinkedIn Learning

Azure Active Directory ermittelt anhand von Zuweisungen, welche Benutzer Zugriff auf bestimmte Apps erhalten sollen. Im Kontext der automatischen Bereitstellung von Benutzerkonten werden nur die Benutzer und Gruppen synchronisiert, die einer Anwendung in Azure AD zugewiesen wurden. 

Vor dem Konfigurieren und Aktivieren des Bereitstellungsdiensts müssen Sie entscheiden, welche Benutzer und/oder Gruppen in Azure AD die Benutzer darstellen, die Zugriff auf LinkedIn Learning benötigen. Anschließend können Sie diese Benutzer LinkedIn Learning anhand der folgenden Anweisungen zuweisen:

[Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-linkedin-learning"></a>Wichtige Tipps zum Zuweisen von Benutzern zu LinkedIn Learning

*    Es wird empfohlen, LinkedIn Learning einen einzelnen Azure AD-Benutzer zuzuweisen, um die Konfiguration der Bereitstellung zu testen. Später können weitere Benutzer und/oder Gruppen zugewiesen werden.

*    Beim Zuweisen eines Benutzers zu LinkedIn Learning müssen Sie im Dialogfeld „Zuweisung“ die Rolle **Benutzer** auswählen. Die Rolle „Standardzugriff“ funktioniert nicht für die Bereitstellung.


## <a name="configuring-user-provisioning-to-linkedin-learning"></a>Konfigurieren der Benutzerbereitstellung in LinkedIn Learning

Dieser Abschnitt führt Sie durch das Herstellen einer Verbindung von Azure AD mit der API zur Bereitstellung von SCIM-Benutzerkonten von LinkedIn Learning sowie durch das Konfigurieren des Bereitstellungsdiensts für das Erstellen, Aktualisieren und Deaktivieren zugewiesener Benutzerkonten in LinkedIn Learning basierend auf der Benutzer- und Gruppenzuweisung in Azure AD.

> [!TIP]
> Sie können auch das SAML-basierte einmalige Anmelden für LinkedIn Learning aktivieren. Befolgen Sie dazu die Anweisungen im [Azure-Portal](https://portal.azure.com). Einmaliges Anmelden kann unabhängig von der automatischen Bereitstellung konfiguriert werden, obwohl diese beiden Features einander ergänzen.


### <a name="to-configure-automatic-user-account-provisioning-to-linkedin-learning-in-azure-ad"></a>So konfigurieren Sie die automatische Bereitstellung von Benutzerkonten für LinkedIn Learning in Azure AD


Der erste Schritt besteht aus dem Abrufen Ihres LinkedIn-Zugriffstokens. Wenn Sie ein Enterprise-Administrator sind, können Sie das Zugriffstoken selbst bereitstellen. Gehen Sie in Ihrem Kontocenter auf **Einstellungen &gt; Globale Einstellungen**, und öffnen Sie den Bereich **SCIM Setup**.

> [!NOTE]
> Wenn Sie direkt auf das Kontocenter zugreifen und nicht über einen Link, können Sie es über die folgenden Schritte erreichen.

1)  Melden Sie sich am Kontocenter an.

2)  Wählen Sie **Administrator &gt; Administratoreinstellungen**.

3)  Klicken Sie auf **Advanced Integrations** (Erweiterte Integrationen) auf der linken Seitenleiste. Sie werden zum Kontocenter weitergeleitet.

4)  Klicken Sie auf **+ Add new SCIM configuration** (+ Neue SCIM-Konfiguration hinzufügen), und befolgen Sie das Verfahren, indem Sie jedes Feld ausführen.

> Wenn die automatische Zuweisung von Lizenzen nicht aktiviert ist, bedeutet das, dass nur Benutzerdaten synchronisiert werden.

![LinkedIn Learning-Bereitstellung](./media/active-directory-saas-linkedinlearning-provisioning-tutorial/linkedin_1.PNG)

> Wenn die automatische Zuweisung von Lizenzen aktiviert ist, müssen Sie die Anwendungsinstanz und den Lizenztyp notieren. Lizenzen werden nach dem Prinzip „Wer zuerst kommt, malt zuerst“ zugewiesen, bis alle Lizenzen ausgeführt sind.

![LinkedIn Learning-Bereitstellung](./media/active-directory-saas-linkedinlearning-provisioning-tutorial/linkedin_2.PNG)

5)  Klicken Sie dann auf **Token generieren**. Ihr Zugriffstoken sollte unter dem Feld **Zugriffstoken** angezeigt werden.

6)  Speichern Sie Ihren Zugriffstoken in Ihrer Zwischenablage oder auf dem Computer, bevor Sie diese Seite verlassen.

7) Wechseln Sie als Nächstes im [Azure-Portal](https://portal.azure.com) zum Abschnitt **Azure Active Directory > Unternehmens-Apps > Alle Anwendungen**.

8) Wenn Sie LinkedIn Learning bereits für einmaliges Anmelden konfiguriert haben, suchen Sie über das Suchfeld nach Ihrer LinkedIn Learning-Instanz. Wählen Sie andernfalls **Hinzufügen** aus, und suchen Sie im Anwendungskatalog nach **LinkedIn Learning**. Wählen Sie „LinkedIn Learning“ in den Suchergebnissen aus, und fügen Sie es Ihrer Anwendungsliste hinzu.

9)    Wählen Sie Ihre LinkedIn Learning-Instanz und dann die Registerkarte **Bereitstellung** aus.

10)    Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest.

![LinkedIn Learning-Bereitstellung](./media/active-directory-saas-linkedinlearning-provisioning-tutorial/linkedin_3.PNG)

11)  Füllen Sie die folgenden Felder unter **Administratoranmeldeinformationen** aus:

* Geben Sie im Feld **Mandanten-URL** https://api.linkedin.com ein.

* Geben Sie im Feld **Geheimes Token** den im Schritt 1 generierten Zugriffstoken ein, und klicken Sie auf **Verbindung testen**.

* Nun sollten Sie oben rechts im Portal eine Benachrichtigung über die erfolgreiche Ausführung sehen.

12) Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder einer Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das unten gezeigte Kontrollkästchen.

13) Klicken Sie auf **Speichern**. 

14) Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzer- und Gruppenattribute, die von Azure AD mit LinkedIn Learning synchronisiert werden. Beachten Sie, dass die als **übereinstimmende** Eigenschaften ausgewählten Attribute für den Abgleich der Benutzerkonten und Gruppen in LinkedIn Learning für Updatevorgänge verwendet werden. Wählen Sie die Schaltfläche „Speichern“, um alle Änderungen zu übernehmen.

![LinkedIn Learning-Bereitstellung](./media/active-directory-saas-linkedinlearning-provisioning-tutorial/linkedin_4.PNG)

15) Um den Azure AD-Bereitstellungsdienst für LinkedIn Learning zu aktivieren, ändern Sie den **Bereitstellungsstatus** im Abschnitt **Einstellungen** in **Ein**.

16) Klicken Sie auf **Speichern**. 

Dadurch wird die Erstsynchronisierung aller Benutzer und/oder Gruppen gestartet, die LinkedIn Learning im Abschnitt „Benutzer und Gruppen“ zugewiesen sind. Beachten Sie, dass die Erstsynchronisierung länger dauert als nachfolgende Synchronisierungen, die ungefähr alle 20 Minuten erfolgen, solange der Dienst ausgeführt wird. Im Abschnitt **Synchronisierungsdetails** können Sie den Fortschritt überwachen und Links zu Berichten zur Bereitstellungsaktivität aufrufen. Darin sind alle Aktionen aufgeführt, die vom Bereitstellungsdienst in Ihrer LinkedIn Learning-App ausgeführt werden.


## <a name="additional-resources"></a>Weitere Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](active-directory-enterprise-apps-manage-provisioning.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

