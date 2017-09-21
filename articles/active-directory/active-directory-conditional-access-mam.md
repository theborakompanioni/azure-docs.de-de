---
title: App-basierter bedingter Zugriff mit Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie, wie der App-basierte bedingte Zugriff mit Azure Active Directory funktioniert.
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
ms.date: 09/07/2017
ms.author: markvi
ms.reviewer: spunukol
ms.translationtype: HT
ms.sourcegitcommit: f2ac16c2f514aaa7e3f90fdf0d0b6d2912ef8485
ms.openlocfilehash: 48c9f55e2296b88acc697ab818f13787695643a5
ms.contentlocale: de-de
ms.lasthandoff: 09/08/2017

---
# <a name="azure-active-directory-app-based-conditional-access"></a>App-basierter bedingter Zugriff mit Azure Active Directory  

Ihre Mitarbeiter verwenden mobile Geräte sowohl für private als auch für berufliche Zwecke. Während Sie einerseits die Produktivität der Mitarbeiter sicherstellen möchten, sollten Sie andererseits Datenverluste vermeiden. Mit dem App-basierten bedingten Zugriff mit Azure Active Directory (Azure AD) können Sie den Zugriff auf Ihre Cloud-Apps auf Client-Apps beschränken, die Ihre Unternehmensdaten schützen können.  

In diesem Thema wird erläutert, wie Sie den App-basierten bedingten Zugriff mit Azure AD konfigurieren.

## <a name="overview"></a>Übersicht

Mit dem [bedingten Zugriff von Azure AD](active-directory-conditional-access-azure-portal.md) können Sie präzise steuern, wie autorisierte Benutzer auf Ihre Ressourcen zugreifen können. Sie können z.B. den Zugriff auf Ihre Cloud-Apps auf vertrauenswürdige Geräte beschränken.

Zum Schutz Ihrer Unternehmensdaten können Sie [Intune-Richtlinien für den App-Schutz](https://docs.microsoft.com/intune/app-protection-policy) verwenden. Da die Intune-Richtlinien für den App-Schutz keine MDM-Lösung (Mobile-Device Management, Verwaltung mobiler Geräte) erfordern, sind Ihre Unternehmensdaten unabhängig davon geschützt, ob Sie die Geräte in einer Lösung für die Geräteverwaltung registrieren oder nicht.

Der App-basierte bedingte Zugriff mit Azure Active Directory ermöglicht es Ihnen, den Zugriff auf Ihre Cloud-Apps auf Client-Apps zu beschränken, die Intune-Richtlinien für den App-Schutz unterstützen. Sie können zum Beispiel den Zugriff auf Exchange Online für die Outlook-App einschränken.

Im Kontext des bedingten Zugriffs werden diese Client-Apps als **genehmigte Client-Apps** bezeichnet.  


![Bedingter Zugriff](./media/active-directory-conditional-access-mam/05.png)


Eine Liste der genehmigten Client-Apps finden Sie unter [Genehmigte Client-App als Voraussetzung](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement).


App-basierte bedingte Zugriffsrichtlinien lassen sich mit anderen Richtlinien wie etwa [gerätebasierten Richtlinien zum bedingten Zugriff](active-directory-conditional-access-policy-connected-applications.md) kombinieren und sorgen so für Flexibilität beim Schutz von Daten für Privat- und Unternehmensgeräte.

 


##<a name="before-you-begin"></a>Voraussetzungen

In diesem Thema wird vorausgesetzt, dass Sie mit Folgendem vertraut sind:

- Der technischen Referenz [Genehmigte Client-App als Voraussetzung](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement)


- Den grundlegenden Konzepten des [bedingten Zugriffs in Azure Active Directory](active-directory-conditional-access-azure-portal.md)

- Der Vorgehensweise zum [Konfigurieren einer Richtlinie zum bedingten Zugriff](active-directory-conditional-access-azure-portal-get-started.md)

- Der [Migration von Richtlinien für bedingten Zugriff](active-directory-conditional-access-best-practices.md#policy-migration)
 

## <a name="prerequisites"></a>Voraussetzungen

Zum Erstellen einer Richtlinie zum App-basierten bedingten Zugriff benötigen Sie ein Abonnement vom Typ „Enterprise Mobility + Security“ oder „Azure Active Directory Premium“, und die Benutzer müssen für EMS oder Azure AD lizenziert sein. 


## <a name="exchange-online-policy"></a>Exchange Online-Richtlinie 

Dieses Szenario besteht aus einer Richtlinie zum App-basierten bedingten Zugriff für den Zugriff auf Exchange Online.


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


## <a name="app-based-or-compliant-device-policy-for-exchange-online-and-sharepoint-online"></a>App-basierte Richtlinie oder Richtlinie für konforme Geräte für Exchange Online und SharePoint Online

Dieses Szenario besteht aus einer Richtlinie zum App-basierten bedingten Zugriff oder einer Richtlinie für den bedingten Zugriff für konforme Geräte für den Zugriff auf Exchange Online.


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





## <a name="app-based-and-compliant-device-policy-for-exchange-online-and-sharepoint-online"></a>App-basierte Richtlinie und Richtlinie für konforme Geräte für Exchange Online und SharePoint Online

Dieses Szenario besteht aus einer Richtlinie zum App-basierten bedingten Zugriff und einer Richtlinie für den bedingten Zugriff für konforme Geräte für den Zugriff auf Exchange Online.


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






## <a name="next-steps"></a>Nächste Schritte

Wenn Sie wissen möchten, wie Sie eine Richtlinie für den bedingten Zugriff konfigurieren, helfen Ihnen die Informationen unter [Erste Schritte mit dem bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md) weiter.

Wenn Sie bereit sind, Richtlinien für den bedingten Zugriff für Ihre Umgebung zu konfigurieren, lesen Sie unter [Best Practices für den bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-best-practices.md) nach. 
