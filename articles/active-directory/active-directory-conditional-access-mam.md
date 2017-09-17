---
title: Bedingter Zugriff mit Verwaltung mobiler Apps in Azure Active Directory | Microsoft-Dokumentation
description: Hier finden Sie Informationen zur Funktionsweise von bedingtem Zugriff mit Verwaltung mobiler Apps in Azure Active Directory.
services: active-directory
keywords: "bedingter Zugriff auf Apps, bedingter Zugriff mit Azure AD, sicherer Zugriff auf Unternehmensressourcen, Richtlinien für bedingten Zugriff"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/01/2017
ms.author: markvi
ms.reviewer: spunukol
ms.translationtype: HT
ms.sourcegitcommit: ce0189706a3493908422df948c4fe5329ea61a32
ms.openlocfilehash: c6bc39dc151c80ffe1306464da60a029e54cc6b1
ms.contentlocale: de-de
ms.lasthandoff: 09/05/2017

---
# <a name="conditional-access-with-mobile-app-management-in-azure-active-directory"></a>Bedingter Zugriff mit Verwaltung mobiler Apps in Azure Active Directory  

Mit einer Kombination aus bedingtem, auf Azure AD-Apps (Azure Active Directory) basierendem Zugriff im Azure-Portal und Intune-Richtlinien für den App-Schutz können Sie den Zugriff auf Cloud-Apps auf mobile Apps beschränken, die den Intune-App-Schutz unterstützen – beispielsweise, um den Zugriff auf Exchange Online auf die Outlook-App zu beschränken. Dadurch kann auch bei Geräten, die nicht für die Verwaltung durch Intune MDM registriert sind, der Schutz von Unternehmensdaten gewährleistet werden.   

Der bedingte Zugriff der Verwaltung mobiler Apps lässt sich mit anderen Richtlinien wie etwa gerätebasierten Richtlinien zum bedingten Zugriff kombinieren und sorgt so für Flexibilität beim Schutz von Daten für Privat- und Unternehmensgeräte. 

##<a name="before-you-begin"></a>Voraussetzungen

In diesem Thema wird vorausgesetzt, dass Sie mit Folgendem vertraut sind:

- Den grundlegenden Konzepten des [bedingten Zugriffs in Azure Active Directory](active-directory-conditional-access-azure-portal.md)

- Der Vorgehensweise zum [Konfigurieren einer Richtlinie zum bedingten Zugriff](active-directory-conditional-access-azure-portal-get-started.md)


Darüber hinaus empfiehlt es sich unter Umständen, sich mit den [Best Practices für den bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-best-practices.md) vertraut zu machen.  



## <a name="prerequisites"></a>Voraussetzungen

1.  Zum Erstellen einer App-basierten Richtlinie zum bedingten Zugriff benötigen Sie ein Abonnement vom Typ „Enterprise Mobility + Security“ oder „Azure Active Directory Premium“, und die Benutzer müssen für EMS oder Azure AD lizenziert sein. 
2.  Machen Sie sich vor dem Erstellen einer Richtlinie zum bedingten Zugriff mit Verwaltung mobiler Apps zunächst mit den Szenarien und den Migrationsüberlegungen vertraut.

## <a name="supported-platforms"></a>Unterstützte Plattformen

-   iOS

-   Android

## <a name="approved-client-applications"></a>Genehmigte Clientanwendungen 

- Microsoft Outlook

- Microsoft SharePoint

- Microsoft OneDrive

- Microsoft Teams

- Microsoft Word

- Microsoft Excel

- Microsoft PowerPoint


## <a name="exchange-online-policy"></a>Exchange Online-Richtlinie 

Dieses Szenario umfasst eine Richtlinie zum bedingten Zugriff mit Verwaltung mobiler Apps für den Zugriff auf Exchange Online mit genehmigten Apps.


### <a name="scenario-playbook"></a>Szenario-Playbook

Annahmen für den Benutzer in diesem Szenario:

- Er konfiguriert E-Mails mithilfe einer nativen E-Mail-Anwendung unter iOS oder Android, um eine Verbindung mit Exchange herzustellen.

- Er erhält eine E-Mail mit dem Hinweis, dass der Zugriff nur über die Outlook-App möglich ist.

- Er lädt die Anwendung über den Link herunter.

- Er öffnet die Outlook-Anwendung und meldet sich mit den Azure AD-Anmeldeinformationen an.

- Er wird aufgefordert, entweder Authenticor (iOS) oder das Unternehmensportal (Android) zu installieren, um den Vorgang fortzusetzen.

- Er installiert die Anwendung und kann zur Outlook-App zurückkehren, um den Vorgang fortzusetzen.

- Er wird aufgefordert, ein Gerät zu registrieren.

- Er kann auf die E-Mails zugreifen.

Intune-Richtlinien für den App-Schutz werden beim Zugriff auf Unternehmensdaten aktiviert, und der Benutzer wird unter Umständen aufgefordert, die Anwendung neu zu starten, eine zusätzliche PIN zu verwenden oder eine andere Aktion auszuführen (sofern dies für die Anwendung und die Plattform konfiguriert wurde).

### <a name="configuration"></a>Konfiguration 

**Schritt 1: Konfigurieren einer Azure AD-Richtlinie zum bedingten Zugriff für Exchange Online**

Für die Richtlinie zum bedingten Zugriff in diesem Schritt müssen folgende Komponenten konfiguriert werden:

![Bedingter Zugriff](./media/active-directory-conditional-access-mam/01.png)

1. Der **Name** Ihrer Richtlinie zum bedingten Zugriff.

2. **Benutzer und Gruppen:** Für jede Richtlinie zum bedingten Zugriff muss mindestens ein Benutzer oder eine Gruppe ausgewählt werden.

3. **Cloud-Apps:** Wählen Sie als Cloud-Apps **Office 365 Exchange Online** aus.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-mam/07.png)

4. **Bedingungen:** Als **Bedingungen** müssen **Geräteplattformen** und **Clientanwendungen** konfiguriert werden:

    a. Wählen Sie als **Geräteplattformen** die Optionen **Android** und **iOS** aus.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-mam/03.png)

    b. Wählen Sie als **Clientanwendungen** die Option **Mobile Apps und Desktopclients** aus.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-mam/04.png)

5. Als **Zugriffskontrollen** muss **Genehmigte Client-App erforderlich (Vorschau)** ausgewählt werden.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-mam/05.png)
 

**Schritt 2: Konfigurieren einer Azure AD-Richtlinie zum bedingten Zugriff für Exchange Online mit Active Sync (EAS)**

Für die Richtlinie zum bedingten Zugriff in diesem Schritt müssen folgende Komponenten konfiguriert werden:

![Bedingter Zugriff](./media/active-directory-conditional-access-mam/06.png)

1. Der **Name** Ihrer Richtlinie zum bedingten Zugriff.

2. **Benutzer und Gruppen:** Für jede Richtlinie zum bedingten Zugriff muss mindestens ein Benutzer oder eine Gruppe ausgewählt werden.


3. **Cloud-Apps:** Wählen Sie als Cloud-Apps **Office 365 Exchange Online** aus.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-mam/07.png)

4. **Bedingungen:** Als **Bedingungen** müssen **Clientanwendungen** konfiguriert werden. 

    a. Wählen Sie als **Clientanwendungen** die Option **Exchange Active Sync** aus.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-mam/08.png)

    b. Als **Zugriffskontrollen** muss **Genehmigte Client-App erforderlich (Vorschau)** ausgewählt werden.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-mam/05.png)


**Schritt 3: Konfigurieren der Intune-Richtlinie für den App-Schutz für iOS- und Android-Clientanwendungen**


![Bedingter Zugriff](./media/active-directory-conditional-access-mam/09.png)

Weitere Informationen finden Sie unter [Schützen von Apps und Daten mit Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).


## <a name="exchange-online-and-sharepoint-online-policy"></a>Richtlinie für Exchange Online und SharePoint Online

Dieses Szenario umfasst eine Richtlinie zum bedingten Zugriff mit Verwaltung mobiler Apps für den Zugriff auf Exchange Online und SharePoint Online mit genehmigten Apps.

### <a name="scenario-playbook"></a>Szenario-Playbook

Annahmen für den Benutzer in diesem Szenario:

- Er versucht, mit der SharePoint-App eine Verbindung herzustellen und Unternehmenswebsites anzuzeigen.

- Er versucht, sich mit den gleichen Anmeldeinformationen anzumelden, die er auch für die Outlook-App verwendet.

- Er muss sich nicht erneut registrieren und kann auf die Ressourcen zugreifen.


### <a name="configuration"></a>Konfiguration

**Schritt 1: Konfigurieren einer Azure AD-Richtlinie zum bedingten Zugriff für Exchange Online und SharePoint Online**

Für die Richtlinie zum bedingten Zugriff in diesem Schritt müssen folgende Komponenten konfiguriert werden:

![Bedingter Zugriff](./media/active-directory-conditional-access-mam/71.png)

1. Der **Name** Ihrer Richtlinie zum bedingten Zugriff.

2. **Benutzer und Gruppen:** Für jede Richtlinie zum bedingten Zugriff muss mindestens ein Benutzer oder eine Gruppe ausgewählt werden.


3. **Cloud-Apps:** Wählen Sie als Cloud-Apps **Office 365 Exchange Online** und **Office 365 SharePoint Online** aus. 

    ![Bedingter Zugriff](./media/active-directory-conditional-access-mam/02.png)

4. **Bedingungen:** Als **Bedingungen** müssen **Geräteplattformen** und **Clientanwendungen** konfiguriert werden:

    a. Wählen Sie als **Geräteplattformen** die Optionen **Android** und **iOS** aus.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-mam/03.png)

    b. Wählen Sie als **Clientanwendungen** die Option **Mobile Apps und Desktopclients** aus.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-mam/04.png)

5. Als **Zugriffskontrollen** muss **Genehmigte Client-App erforderlich (Vorschau)** ausgewählt werden.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-mam/05.png)




**Schritt 2: Konfigurieren einer Azure AD-Richtlinie zum bedingten Zugriff für Exchange Online mit Active Sync (EAS)**

Für die Richtlinie zum bedingten Zugriff in diesem Schritt müssen folgende Komponenten konfiguriert werden:

![Bedingter Zugriff](./media/active-directory-conditional-access-mam/06.png)

1. Der **Name** Ihrer Richtlinie zum bedingten Zugriff.

2. **Benutzer und Gruppen:** Für jede Richtlinie zum bedingten Zugriff muss mindestens ein Benutzer oder eine Gruppe ausgewählt werden.

3. **Cloud-Apps:** Wählen Sie als Cloud-Apps **Office 365 Exchange Online** aus. Online 

    ![Bedingter Zugriff](./media/active-directory-conditional-access-mam/07.png)

4. **Bedingungen:** Als **Bedingungen** müssen **Clientanwendungen** konfiguriert werden:

    a. Wählen Sie als **Clientanwendungen** die Option **Exchange Active Sync** aus.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-mam/08.png)

    b. Als **Zugriffskontrollen** muss **Genehmigte Client-App erforderlich (Vorschau)** ausgewählt werden.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-mam/05.png)




**Schritt 3: Konfigurieren der Intune-Richtlinie für den App-Schutz für iOS- und Android-Clientanwendungen**


![Bedingter Zugriff](./media/active-directory-conditional-access-mam/09.png)

Weitere Informationen finden Sie unter [Schützen von Apps und Daten mit Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).


## <a name="mobile-application-management-or-compliant-device-policy-for-exchange-online-and-sharepoint-online"></a>Richtlinie für die mobile Anwendungsverwaltung oder für konforme Geräte für Exchange Online und SharePoint Online

Dieses Szenario umfasst eine Richtlinie zum bedingten Zugriff mit Verwaltung mobiler Apps oder konformen Geräten für den Zugriff auf Exchange Online mit genehmigten Apps.


### <a name="scenario-playbook"></a>Szenario-Playbook

Annahmen für dieses Szenario:
 
- Einige Benutzer sind bereits registriert (mit Unternehmensgeräten oder ohne).

- Benutzer, die nicht bei Azure AD registriert sind und eine durch eine App geschützte Anwendung verwenden, müssen ein Gerät registrieren, um auf Ressourcen zugreifen zu können.

- Registrierte Benutzer, die die durch eine App geschützte Anwendung verwenden, müssen das Gerät nicht erneut registrieren.


### <a name="configuration"></a>Konfiguration

**Schritt 1: Konfigurieren einer Azure AD-Richtlinie zum bedingten Zugriff für Exchange Online und SharePoint Online**

Für die Richtlinie zum bedingten Zugriff in diesem Schritt müssen folgende Komponenten konfiguriert werden:

![Bedingter Zugriff](./media/active-directory-conditional-access-mam/62.png)

1. Der **Name** Ihrer Richtlinie zum bedingten Zugriff.

2. **Benutzer und Gruppen:** Für jede Richtlinie zum bedingten Zugriff muss mindestens ein Benutzer oder eine Gruppe ausgewählt werden.

3. **Cloud-Apps:** Wählen Sie als Cloud-Apps **Office 365 Exchange Online** und **Office 365 SharePoint Online** aus. 

     ![Bedingter Zugriff](./media/active-directory-conditional-access-mam/02.png)

4. **Bedingungen:** Als **Bedingungen** müssen **Geräteplattformen** und **Clientanwendungen** konfiguriert werden. 
 
    a. Wählen Sie als **Geräteplattformen** die Optionen **Android** und **iOS** aus.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-mam/03.png)

    b. Wählen Sie als **Clientanwendungen** die Option **Mobile Apps und Desktopclients** aus.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-mam/04.png)

5. Als **Zugriffskontrollen** muss Folgendes ausgewählt werden:

    - **Markieren des Geräts als kompatibel erforderlich**

    - **Genehmigte Client-App erforderlich (Vorschau)**

    - **Eine der ausgewählten Steuerungen anfordern**   
 
    ![Bedingter Zugriff](./media/active-directory-conditional-access-mam/11.png)



**Schritt 2: Konfigurieren einer Azure AD-Richtlinie zum bedingten Zugriff für Exchange Online mit Active Sync (EAS)**

Für die Richtlinie zum bedingten Zugriff in diesem Schritt müssen folgende Komponenten konfiguriert werden:

![Bedingter Zugriff](./media/active-directory-conditional-access-mam/61.png)

1. Der **Name** Ihrer Richtlinie zum bedingten Zugriff.

2. **Benutzer und Gruppen:** Für jede Richtlinie zum bedingten Zugriff muss mindestens ein Benutzer oder eine Gruppe ausgewählt werden.

3. **Cloud-Apps:** Wählen Sie als Cloud-Apps **Office 365 Exchange Online** aus. 

    ![Bedingter Zugriff](./media/active-directory-conditional-access-mam/07.png)

4. **Bedingungen:** Als **Bedingungen** müssen **Clientanwendungen** konfiguriert werden. 

    Wählen Sie als *Clientanwendungen* die Option **Exchange Active Sync** aus.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-mam/08.png)

5. Als **Zugriffskontrollen** muss **Genehmigte Client-App erforderlich (Vorschau)** ausgewählt werden.
 
    ![Bedingter Zugriff](./media/active-directory-conditional-access-mam/11.png)




**Schritt 3: Konfigurieren der Intune-Richtlinie für den App-Schutz für iOS- und Android-Clientanwendungen**


![Bedingter Zugriff](./media/active-directory-conditional-access-mam/09.png)

Weitere Informationen finden Sie unter [Schützen von Apps und Daten mit Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).





## <a name="mobile-application-management-and-compliant-device-policy-for-exchange-online-and-sharepoint-online"></a>Richtlinie für die mobile Anwendungsverwaltung und für konforme Geräte für Exchange Online und SharePoint Online

Dieses Szenario umfasst eine Richtlinie zum bedingten Zugriff mit Verwaltung mobiler Apps und konformen Geräten für den Zugriff auf Exchange Online mit genehmigten Apps.

### <a name="scenario-playbook"></a>Szenario-Playbook

Annahmen für den Benutzer in diesem Szenario:
 
-   Er konfiguriert E-Mails mithilfe einer nativen E-Mail-Anwendung unter iOS oder Android, um eine Verbindung mit Exchange herzustellen.
-   Er erhält eine E-Mail mit dem Hinweis, dass das Gerät für den Zugriff registriert werden muss.
-   Er lädt das Unternehmensportal herunter und meldet sich an.
-   Er ruft seine E-Mails ab und wird zur Verwendung der Outlook-App aufgefordert.
-   Er lädt die Outlook-App herunter.
-   Er öffnet die Outlook-App und gibt die bei der Registrierung verwendeten Anmeldeinformationen ein.
-   Der Benutzer kann auf die E-Mails zugreifen.

Intune-Richtlinien für den App-Schutz werden beim Zugriff auf die Unternehmensdaten aktiviert, und der Benutzer wird unter Umständen aufgefordert, die Anwendung neu zu starten, eine zusätzliche PIN zu verwenden oder eine andere Aktion auszuführen (sofern dies für die Anwendung und die Plattform konfiguriert wurde).


### <a name="configuration"></a>Konfiguration

**Schritt 1: Konfigurieren einer Azure AD-Richtlinie zum bedingten Zugriff für Exchange Online und SharePoint Online**

Für die Richtlinie zum bedingten Zugriff in diesem Schritt müssen folgende Komponenten konfiguriert werden:

![Bedingter Zugriff](./media/active-directory-conditional-access-mam/62.png)

1. Der **Name** Ihrer Richtlinie zum bedingten Zugriff.

2. **Benutzer und Gruppen:** Für jede Richtlinie zum bedingten Zugriff muss mindestens ein Benutzer oder eine Gruppe ausgewählt werden.

3. **Cloud-Apps:** Wählen Sie als Cloud-Apps **Office 365 Exchange Online** und **Office 365 SharePoint Online** aus. 

     ![Bedingter Zugriff](./media/active-directory-conditional-access-mam/02.png)

4. **Bedingungen:** Als **Bedingungen** müssen **Geräteplattformen** und **Clientanwendungen** konfiguriert werden. 
 
    a. Wählen Sie als **Geräteplattformen** die Optionen **Android** und **iOS** aus.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-mam/03.png)

    b. Wählen Sie als **Clientanwendungen** die Option **Mobile Apps und Desktopclients** aus.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-mam/04.png)

5. Als **Zugriffskontrollen** muss Folgendes ausgewählt werden:

    - **Markieren des Geräts als kompatibel erforderlich**

    - **Genehmigte Client-App erforderlich (Vorschau)**

    - **Eine der ausgewählten Steuerungen anfordern**   
 
    ![Bedingter Zugriff](./media/active-directory-conditional-access-mam/11.png)



**Schritt 2: Konfigurieren einer Azure AD-Richtlinie zum bedingten Zugriff für Exchange Online mit Active Sync (EAS)**

Für die Richtlinie zum bedingten Zugriff in diesem Schritt müssen folgende Komponenten konfiguriert werden:

![Bedingter Zugriff](./media/active-directory-conditional-access-mam/61.png)

1. Der **Name** Ihrer Richtlinie zum bedingten Zugriff.

2. **Benutzer und Gruppen:** Für jede Richtlinie zum bedingten Zugriff muss mindestens ein Benutzer oder eine Gruppe ausgewählt werden.

3. **Cloud-Apps:** Wählen Sie als Cloud-Apps **Office 365 Exchange Online** aus. 

    ![Bedingter Zugriff](./media/active-directory-conditional-access-mam/07.png)

4. **Bedingungen:** Als **Bedingungen** müssen **Clientanwendungen** konfiguriert werden. 

    Wählen Sie als **Clientanwendungen** die Option **Exchange Active Sync** aus.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-mam/08.png)

5. Als **Zugriffskontrollen** muss Folgendes ausgewählt werden:

    - **Markieren des Geräts als kompatibel erforderlich**

    - **Genehmigte Client-App erforderlich (Vorschau)**

    - **Alle ausgewählten Kontrollen anfordern**   
 
    ![Bedingter Zugriff](./media/active-directory-conditional-access-mam/64.png)




**Schritt 3: Konfigurieren der Intune-Richtlinie für den App-Schutz für iOS- und Android-Clientanwendungen**


![Bedingter Zugriff](./media/active-directory-conditional-access-mam/09.png)

Weitere Informationen finden Sie unter [Schützen von Apps und Daten mit Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).



## <a name="migration-considerations"></a>Überlegungen zur Migration

Falls Sie Richtlinien im klassischen Azure-Portal konfiguriert haben, sollten Sie diese aus folgenden Gründen migrieren:


- Ein Benutzer, der sowohl in einer Richtlinie des klassischen Azure-Portals als auch in einer Richtlinie des Azure-Portals enthalten ist, muss die Anforderungen beider Richtlinien erfüllen. 

- Wenn Sie Ihre vorhandenen Richtlinien nicht migrieren, können Sie keine Richtlinien für die Zugriffsgewährung implementieren.


## <a name="migration-from-the-azure-classic-portal"></a>Migration aus dem klassischen Azure-Portal

Szenario: 

- In Ihrem [klassischen Azure-Portal](https://manage.windowsazure.com) ist Folgendes konfiguriert:

    - SharePoint Online

    ![Bedingter Zugriff](./media/active-directory-conditional-access-mam/14.png)

    - Eine gerätebasierte Richtlinie zum bedingten Zugriff

    ![Bedingter Zugriff](./media/active-directory-conditional-access-mam/15.png)

- Sie möchten im Azure-Portal eine Richtlinie zum bedingten Zugriff mit mobiler Anwendungsverwaltung konfigurieren. 
 

### <a name="configuration"></a>Konfiguration 

- Überprüfen Sie Ihre gerätebasierten Richtlinien zum bedingten Zugriff.

- Migrieren Sie sie zum Azure-Portal. 

- Fügen Sie Richtlinien zum bedingten Zugriff mit mobiler Anwendungsverwaltung hinzu.


## <a name="migrating-from-intune"></a>Migrieren aus Intune 

Szenario:

- In [Intune](https://portal.azure.com/#blade/Microsoft_Intune/SummaryBlade ) ist eine Richtlinie zum bedingten Zugriff mit mobiler Anwendungsverwaltung für Exchange Online oder SharePoint Online konfiguriert.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-mam/15.png)

- Sie möchten im Azure-Portal zur Verwendung von bedingtem Zugriff mit mobiler Anwendungsverwaltung migrieren.


### <a name="configuration"></a>Konfiguration 
 
- Überprüfen Sie Ihre gerätebasierten Richtlinien zum bedingten Zugriff.

- Migrieren Sie sie zum Azure-Portal. 

- Überprüfen Sie Ihre Richtlinien zum bedingten Zugriff mit mobiler Anwendungsverwaltung, die in Intune für Exchange Online oder SharePoint Online konfiguriert sind.

- Geben Sie zusätzlich zur gerätebasierten Kontrolle an, dass auch genehmigte Anwendungen erforderlich**** sind. 
 

## <a name="migrating-from-the-azure-classic-portal-and-intune"></a>Migrieren aus dem klassischen Azure-Portal und aus Intune

Szenario:

- Folgendes ist konfiguriert:

    - **Klassisches Azure-Portal:** gerätebasierter bedingter Zugriff 

    - **Intune:** Richtlinien zum bedingten Zugriff mit mobiler Anwendungsverwaltung 
    
- Sie möchten beide Richtlinien im Azure-Portal zur Verwendung von Richtlinien zum bedingtem Zugriff mit mobiler Anwendungsverwaltung migrieren.


### <a name="configuration"></a>Konfiguration

- Überprüfen Sie Ihre gerätebasierten Richtlinien zum bedingten Zugriff.

- Migrieren Sie sie zum Azure-Portal. 

- Überprüfen Sie Ihre Richtlinie zum bedingten Zugriff mit mobiler Anwendungsverwaltung, die in Intune für Exchange Online oder SharePoint Online konfiguriert ist.

- Geben Sie zusätzlich zur gerätebasierten Kontrolle an, dass auch genehmigte Anwendungen erforderlich**** sind. 




## <a name="next-steps"></a>Nächste Schritte

Wenn Sie wissen möchten, wie Sie eine Richtlinie für den bedingten Zugriff konfigurieren, helfen Ihnen die Informationen unter [Erste Schritte mit dem bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md) weiter.

Wenn Sie bereit sind, Richtlinien für den bedingten Zugriff für Ihre Umgebung zu konfigurieren, lesen Sie unter [Best Practices für den bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-best-practices.md) nach. 
