---
title: Voraussetzungen zum Zugriff auf die Azure AD-Berichterstellungs-API | Microsoft Docs
description: "Erfahren Sie, welche Voraussetzungen für den Zugriff auf die Azure AD-Berichterstellungs-API gelten."
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/25/2016
ms.author: dhanyahk;markvi
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: b0f23cc578fb946247d492b9dee92cf7a9daafa1


---
# <a name="prerequisites-to-access-the-azure-ad-reporting-api"></a>Voraussetzungen zum Zugriff auf die Azure AD-Berichterstellungs-API
Die [Azure AD-Berichterstellungs-APIs](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) bieten Ihnen über einen Satz von REST-basierten APIs programmgesteuerten Zugriff auf die Daten. Sie können diese APIs über verschiedene Programmiersprachen und Tools aufrufen.

Die Berichterstellungs-API verwendet [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) zum Autorisieren des Zugriffs auf die Web-APIs. 

Um auf die Berichterstellungs-API zugreifen zu können, müssen Sie folgende Schritte ausführen:

1. Erstellen Sie eine Anwendung in Ihrem Azure AD-Mandanten. 
2. Gewähren Sie der Anwendung geeignete Berechtigungen zum Zugriff auf Ihre Azure AD-Daten.
3. Rufen Sie Konfigurationseinstellungen aus Ihrem Verzeichnis ab.

Bei Fragen, Problemen oder zum Senden von Feedback wenden Sie sich an das [Hilfeteam für die AAD-Berichterstellung](mailto:aadreportinghelp@microsoft.com).

## <a name="create-an-azure-ad-application"></a>Erstellen einer Azur AD-Anwendung
Um Ihr Verzeichnis für den Zugriff auf die Azure AD-Berichterstellungs-API zu konfigurieren, müssen Sie sich beim klassischen Azure-Portal mit dem Konto eines Azure-Abonnementadministrators anmelden, das auch der Verzeichnisrolle „Globaler Administrator“ Ihres Azure AD-Mandanten angehört.

> [!IMPORTANT]
> Clientanwendungen, die wie hier unter Anmeldeinformationen mit Administratorberechtigungen ausgeführt werden, können äußerst einflussreich sein. Sorgen Sie also unbedingt dafür, dass die ID und geheimen Anmeldeinformationen der Anwendung stets gut geschützt sind.
> 
> 

1. Klicken Sie im linken Navigationsbereich des [klassischen Azure-Portals](https://manage.windowsazure.com)auf **Active Directory**.
   
    ![Registrieren einer Anwendung](./media/active-directory-reporting-api-prerequisites/01.png) 
2. Wählen Sie in der Liste **Active Directory** Ihr Verzeichnis aus.
3. Klicken Sie im Menü oben auf **Anwendungen**.
   
    ![Registrieren einer Anwendung](./media/active-directory-reporting-api-prerequisites/02.png) 
4. Klicken Sie auf der unteren Leiste auf **Hinzufügen**.
   
    ![Registrieren einer Anwendung](./media/active-directory-reporting-api-prerequisites/03.png) 
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Eine von meinem Unternehmen entwickelte Anwendung hinzufügen**. 
   
    ![Registrieren einer Anwendung](./media/active-directory-reporting-api-prerequisites/04.png) 
6. Führen Sie im Dialogfeld **Geben Sie uns Informationen zu Ihrer Anwendung.** die folgenden Schritte aus: 
   
    ![Registrieren einer Anwendung](./media/active-directory-reporting-api-prerequisites/05.png) 
   
    a. Geben Sie im Textfeld **Name** einen Namen ein (z.B. „Anwendung für die Berichterstellungs-API“).
   
    b. Wählen Sie **Webanwendung und/oder Web-API**.
   
    c. Klicken Sie auf **Weiter**.
7. Führen Sie im Dialogfeld **App-Eigenschaften** folgende Schritte aus: 
   
    ![Registrieren einer Anwendung](./media/active-directory-reporting-api-prerequisites/06.png) 
   
    a. Geben Sie im Textfeld **Anmelde-URL** die URL `https://localhost` ein.
   
    b. Geben Sie im Textfeld **App-ID-URI** den URI ```https://localhost/ReportingApiApp``` ein.
   
    c. Klicken Sie auf **Fertig stellen**.

## <a name="grant-your-application-permission-to-use-the-api"></a>Gewähren von Anwendungsberechtigungen zum Verwenden der API
1. Klicken Sie im linken Navigationsbereich des [klassischen Azure-Portals](https://manage.windowsazure.com/)auf **Active Directory**.
   
    ![Registrieren einer Anwendung](./media/active-directory-reporting-api-prerequisites/01.png) 
2. Wählen Sie in der Liste **Active Directory** Ihr Verzeichnis aus.
3. Klicken Sie im Menü oben auf **Anwendungen**.
   
    ![Registrieren einer Anwendung](./media/active-directory-reporting-api-prerequisites/02.png)
4. Wählen Sie in der Anwendungsliste Ihre neu erstellte Anwendung aus.
   
    ![Registrieren einer Anwendung](./media/active-directory-reporting-api-prerequisites/07.png)
5. Klicken Sie oben im Menü auf **Konfigurieren**.
   
    ![Registrieren einer Anwendung](./media/active-directory-reporting-api-prerequisites/08.png)
6. Klicken Sie im Abschnitt **Berechtigungen für andere Anwendungen** für die Ressource **Azure Active Directory** auf die Dropdownliste **Anwendungsberechtigungen**, und wählen Sie **Verzeichnisdaten lesen** aus.
   
    ![Registrieren einer Anwendung](./media/active-directory-reporting-api-prerequisites/09.png)
7. Klicken Sie auf der unteren Leiste auf **Speichern**.
   
    ![Registrieren einer Anwendung](./media/active-directory-reporting-api-prerequisites/10.png)

## <a name="gather-configuration-settings-from-your-directory"></a>Rufen Sie Konfigurationseinstellungen aus Ihrem Verzeichnis ab.
In diesem Abschnitt wird gezeigt, wie Sie die folgenden Einstellungen aus Ihrem Verzeichnis abrufen:

* Domänenname
* Client-ID
* Geheimer Clientschlüssel

Sie benötigen diese Werte, um Aufrufe an die Berichterstellungs-API zu konfigurieren. 

### <a name="get-your-domain-name"></a>Ermitteln des Domänennamens
1. Klicken Sie im linken Navigationsbereich des [klassischen Azure-Portals](https://manage.windowsazure.com)auf **Active Directory**.
   
    ![Registrieren einer Anwendung](./media/active-directory-reporting-api-prerequisites/01.png) 
2. Wählen Sie in der Liste **Active Directory** Ihr Verzeichnis aus.
3. Klicken Sie im oberen Menü auf **Domänen**.
   
    ![Registrieren einer Anwendung](./media/active-directory-reporting-api-prerequisites/11.png) 
4. Kopieren Sie Ihren Domänennamen in der Spalte **Domänenname** .
   
    ![Registrieren einer Anwendung](./media/active-directory-reporting-api-prerequisites/12.png) 

### <a name="get-the-applications-client-id"></a>Abrufen der Client-ID der Anwendung
1. Klicken Sie im linken Navigationsbereich des [klassischen Azure-Portals](https://manage.windowsazure.com)auf **Active Directory**.
   
    ![Registrieren einer Anwendung](./media/active-directory-reporting-api-prerequisites/01.png) 
2. Wählen Sie in der Liste **Active Directory** Ihr Verzeichnis aus.
3. Klicken Sie im Menü oben auf **Anwendungen**.
   
    ![Registrieren einer Anwendung](./media/active-directory-reporting-api-prerequisites/02.png) 
4. Wählen Sie in der Anwendungsliste Ihre neu erstellte Anwendung aus.
   
    ![Registrieren einer Anwendung](./media/active-directory-reporting-api-prerequisites/07.png)
5. Klicken Sie oben im Menü auf **Konfigurieren**.
   
    ![Registrieren einer Anwendung](./media/active-directory-reporting-api-prerequisites/08.png)
6. Kopieren Sie Ihre **Client-ID**.
   
    ![Registrieren einer Anwendung](./media/active-directory-reporting-api-prerequisites/13.png)

### <a name="get-the-applications-client-secret"></a>Abrufen des geheimen Clientschlüssels der Anwendung
Um den geheimen Clientschlüssel Ihrer Anwendung abzurufen, müssen Sie einen neuen Schlüssel erstellen und dessen Wert beim Speichern des neuen Schlüssels speichern, weil dieser Wert später nicht mehr abgerufen werden kann.

1. Klicken Sie im linken Navigationsbereich des [klassischen Azure-Portals](https://manage.windowsazure.com)auf **Active Directory**.
   
    ![Registrieren einer Anwendung](./media/active-directory-reporting-api-prerequisites/01.png) 
2. Wählen Sie in der Liste **Active Directory** Ihr Verzeichnis aus.
3. Klicken Sie im Menü oben auf **Anwendungen**.
   
    ![Registrieren einer Anwendung](./media/active-directory-reporting-api-prerequisites/02.png) 
4. Wählen Sie in der Anwendungsliste Ihre neu erstellte Anwendung aus.
   
    ![Registrieren einer Anwendung](./media/active-directory-reporting-api-prerequisites/07.png)
5. Klicken Sie oben im Menü auf **Konfigurieren**.
   
    ![Registrieren einer Anwendung](./media/active-directory-reporting-api-prerequisites/08.png)
6. Führen Sie im Abschnitt **Schlüssel** die folgenden Schritte aus: 
   
    ![Registrieren einer Anwendung](./media/active-directory-reporting-api-prerequisites/14.png)
   
    a. Wählen Sie aus der Liste für die Dauer eine Dauer aus.
   
    b. Klicken Sie auf der unteren Leiste auf **Speichern**.
   
    ![Registrieren einer Anwendung](./media/active-directory-reporting-api-prerequisites/10.png)
   
    c. Kopieren Sie den Schlüsselwert.

## <a name="next-steps"></a>Nächste Schritte
* Möchten Sie programmgesteuert auf die Daten der Azure AD-Berichterstellungs-API zugreifen? Dann lesen Sie den Artikel [Erste Schritte mit der Berichterstellungs-API von Azure Active Directory](active-directory-reporting-api-getting-started.md).
* Wenn Sie weitere Informationen zur Azure Active Directory-Berichterstellung benötigen, finden Sie diese im [Leitfaden zur Azure Active Directory-Berichterstellung](active-directory-reporting-guide.md).  




<!--HONumber=Dec16_HO4-->


