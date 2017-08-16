---
title: "Voraussetzungen für den Zugriff auf die Azure AD-Berichterstellungs-API | Microsoft-Dokumentation"
description: "Erfahren Sie, welche Voraussetzungen für den Zugriff auf die Azure AD-Berichterstellungs-API gelten."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.translationtype: HT
ms.sourcegitcommit: 1dbb1d5aae55a4c926b9d8632b416a740a375684
ms.openlocfilehash: 5fafd83c337e3c73260d89cdad7409a01ce5855b
ms.contentlocale: de-de
ms.lasthandoff: 08/07/2017

---
# <a name="prerequisites-to-access-the-azure-ad-reporting-api"></a>Voraussetzungen zum Zugriff auf die Azure AD-Berichterstellungs-API

Die [Azure AD-Berichterstellungs-APIs](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) bieten Ihnen über einen Satz von REST-basierten APIs programmgesteuerten Zugriff auf die Daten. Sie können diese APIs über verschiedene Programmiersprachen und Tools aufrufen.

Die Berichterstellungs-API verwendet [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) zum Autorisieren des Zugriffs auf die Web-APIs. 

Um mithilfe der API auf die Berichtsdaten zuzugreifen, müssen Ihnen eine der folgenden Rollen zugewiesen sein:

- Sicherheit lesen
- Sicherheitsadministrator
- Globaler Administrator


Um auf die Berichterstellungs-API zugreifen zu können, müssen Sie folgende Schritte ausführen:

1. Registrieren einer Anwendung 
2. Erteilen von Berechtigungen 
3. Erfassen von Konfigurationseinstellungen 

Bei Fragen, Problemen oder zum Senden von Feedback [eröffnen Sie ein Supportticket](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto).

## <a name="register-an-azure-active-directory-application"></a>Registrieren einer Azure Active Directory-Anwendung

Sie müssen Apps auch dann registrieren, wenn Sie mithilfe eines Skripts auf die Berichterstellungs-API zugreifen. Dadurch wird Ihnen eine **Anwendungs-ID** zugewiesen, die für einen Autorisierungsaufruf erforderlich ist und den Empfang von Tokens über Ihren Code ermöglicht.

Um Ihr Verzeichnis für den Zugriff auf die Azure AD-Berichterstellungs-API zu konfigurieren, müssen Sie sich beim Azure-Portal mit dem Konto eines Azure-Administrators anmelden, das auch der Verzeichnisrolle **Globaler Administrator** Ihres Azure AD-Mandanten angehört.

> [!IMPORTANT]
> Clientanwendungen, die wie hier unter Anmeldeinformationen mit Administratorberechtigungen ausgeführt werden, können äußerst einflussreich sein. Sorgen Sie also unbedingt dafür, dass die ID und geheimen Anmeldeinformationen der Anwendung stets gut geschützt sind.
> 


**So registrieren Sie eine Azure Active Directory-Anwendung:**

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) im linken Navigationsbereich auf **Active Directory**.
   
    ![Register application (Anwendung registrieren)](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. Klicken Sie auf dem Blatt **Azure Active Directory** auf **App-Registrierungen**.

    ![Register application (Anwendung registrieren)](./media/active-directory-reporting-api-prerequisites-azure-portal/02.png) 

3. Klicken Sie auf dem Blatt **App-Registrierungen** auf der Symbolleiste am oberen Rand auf **Registrierung einer neuen Anwendung**.

    ![Register application (Anwendung registrieren)](./media/active-directory-reporting-api-prerequisites-azure-portal/03.png)

4. Führen Sie auf dem Blatt **Erstellen** die folgenden Schritte durch:

    ![Register application (Anwendung registrieren)](./media/active-directory-reporting-api-prerequisites-azure-portal/04.png)

    a. Geben Sie im Textfeld **Name** Folgendes ein: `Reporting API application`.

    b. Wählen Sie als **Anwendungstyp** **Web-App/API** aus.

    c. Geben Sie im Textfeld **Anmelde-URL** die URL `https://localhost` ein.

    d. Klicken Sie auf **Erstellen**. 


## <a name="grant-permissions"></a>Erteilen von Berechtigungen 

Durch diesen Schritt soll Ihrer Anwendung die Berechtigung **Verzeichnisdaten lesen** für die **Microsoft Azure Active Directory**-API erteilt werden.

![Register application (Anwendung registrieren)](./media/active-directory-reporting-api-prerequisites-azure-portal/16.png)
 

**So erteilen Sie Ihrer Anwendung die Berechtigung zur Verwendung der API:**

1. Klicken Sie auf dem Blatt **App-Registrierungen** in der App-Liste auf **Berichterstellungs-API-Anwendung**.

2. Klicken Sie auf dem Blatt **Berichterstellungs-API-Anwendung** auf der Symbolleiste am oberen Rand auf **Einstellungen**. 

    ![Register application (Anwendung registrieren)](./media/active-directory-reporting-api-prerequisites-azure-portal/05.png)

3. Klicken Sie auf dem Blatt **Einstellungen** auf **Erforderliche Berechtigungen**. 

    ![Register application (Anwendung registrieren)](./media/active-directory-reporting-api-prerequisites-azure-portal/06.png)

4. Klicken Sie auf dem Blatt **Erforderliche Berechtigungen** in der **API**-Liste auf **Microsoft Azure Active Directory**. 

    ![Register application (Anwendung registrieren)](./media/active-directory-reporting-api-prerequisites-azure-portal/07.png)

5. Wählen Sie auf dem Blatt **Zugriff aktivieren** die Option **Verzeichnisdaten lesen** aus. 

    ![Register application (Anwendung registrieren)](./media/active-directory-reporting-api-prerequisites-azure-portal/08.png)

6. Klicken Sie auf der Symbolleiste am oberen Rand auf **Speichern**.

    ![Register application (Anwendung registrieren)](./media/active-directory-reporting-api-prerequisites-azure-portal/15.png)

## <a name="gather-configuration-settings"></a>Erfassen von Konfigurationseinstellungen 
In diesem Abschnitt wird gezeigt, wie Sie die folgenden Einstellungen aus Ihrem Verzeichnis abrufen:

* Domänenname
* Client-ID
* Geheimer Clientschlüssel

Sie benötigen diese Werte, um Aufrufe an die Berichterstellungs-API zu konfigurieren. 

### <a name="get-your-domain-name"></a>Ermitteln des Domänennamens

**So ermitteln Sie den Domänennamen:**

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) im linken Navigationsbereich auf **Active Directory**.
   
    ![Register application (Anwendung registrieren)](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. Klicken Sie auf dem Blatt **Azure Active Directory** auf **Domänennamen**.

    ![Register application (Anwendung registrieren)](./media/active-directory-reporting-api-prerequisites-azure-portal/09.png) 

3. Kopieren Sie Ihren Domänennamen aus der Liste der Domänen.


### <a name="get-your-applications-client-id"></a>Abrufen der Client-ID der Anwendung

**So rufen Sie die Client-ID der Anwendung ab:**

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) im linken Navigationsbereich auf **Active Directory**.
   
    ![Register application (Anwendung registrieren)](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. Klicken Sie auf dem Blatt **App-Registrierungen** in der App-Liste auf **Berichterstellungs-API-Anwendung**.

3. Klicken Sie auf dem Blatt **Berichterstellungs-API-Anwendung** unter **Anwendungs-ID** auf **Klicken Sie zum Kopieren**.

    ![Register application (Anwendung registrieren)](./media/active-directory-reporting-api-prerequisites-azure-portal/11.png) 



### <a name="get-your-applications-client-secret"></a>Abrufen des geheimen Clientschlüssels der Anwendung
Um den geheimen Clientschlüssel Ihrer Anwendung abzurufen, müssen Sie einen neuen Schlüssel erstellen und dessen Wert beim Speichern des neuen Schlüssels speichern, weil dieser Wert später nicht mehr abgerufen werden kann.

**So rufen Sie den geheimen Clientschlüssel der Anwendung ab:**

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) im linken Navigationsbereich auf **Active Directory**.
   
    ![Register application (Anwendung registrieren)](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. Klicken Sie auf dem Blatt **App-Registrierungen** in der App-Liste auf **Berichterstellungs-API-Anwendung**.


3. Klicken Sie auf dem Blatt **Berichterstellungs-API-Anwendung** auf der Symbolleiste am oberen Rand auf **Einstellungen**. 

    ![Register application (Anwendung registrieren)](./media/active-directory-reporting-api-prerequisites-azure-portal/05.png)

4. Klicken Sie auf dem Blatt **Einstellungen** im Abschnitt **APIR-Zugriff** auf **Schlüssel**. 

    ![Register application (Anwendung registrieren)](./media/active-directory-reporting-api-prerequisites-azure-portal/12.png)


5. Führen Sie auf dem Blatt **Schlüssel** die folgenden Schritte durch:

    ![Register application (Anwendung registrieren)](./media/active-directory-reporting-api-prerequisites-azure-portal/14.png)

    a. Geben Sie im Textfeld **Beschreibung** Folgendes ein: `Reporting API`.

    b. Wählen Sie für **Läuft ab** die Option **In 2 Jahren** aus.

    c. Klicken Sie auf **Speichern**.

    d. Kopieren Sie den Schlüsselwert.


## <a name="next-steps"></a>Nächste Schritte
* Möchten Sie programmgesteuert auf die Daten der Azure AD-Berichterstellungs-API zugreifen? Dann lesen Sie den Artikel [Erste Schritte mit der Berichterstellungs-API von Azure Active Directory](active-directory-reporting-api-getting-started.md).
* Wenn Sie weitere Informationen zur Azure Active Directory-Berichterstellung benötigen, finden Sie diese im [Leitfaden zur Azure Active Directory-Berichterstellung](active-directory-reporting-guide.md).  


