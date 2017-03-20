---
title: 'Azure Active Directory B2C: Registrierung einer Anwendung | Microsoft Docs'
description: Registrieren Ihrer Anwendung bei Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: bryanla
ms.assetid: 20e92275-b25d-45dd-9090-181a60c99f69
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 3/13/2017
ms.author: parakhj
translationtype: Human Translation
ms.sourcegitcommit: c1cd1450d5921cf51f720017b746ff9498e85537
ms.openlocfilehash: 541849501335fb25d96cffa81b8119adc158cdd7
ms.lasthandoff: 03/14/2017


---
# <a name="azure-active-directory-b2c-register-your-application"></a>Azure Active Directory B2C: Registrieren der Anwendung

> [!IMPORTANT]
> Anwendungen, die im Azure-Portal auf dem Blatt „Azure AD B2C“ erstellt wurden, müssen am selben Speicherort verwaltet werden. Mit PowerShell oder über ein anderes Portal bearbeitete B2C-Anwendungen werden nicht mehr unterstützt und können voraussichtlich nicht mehr mit Azure Active Directory B2C verwendet werden.
> 
> 

## <a name="prerequisite"></a>Voraussetzung
Zum Erstellen einer Anwendung, die Registrierungen und Anmeldungen von Kunden akzeptiert, müssen Sie die Anwendung zunächst bei einem Azure Active Directory B2C-Mandanten registrieren. Erstellen Sie einen eigenen Mandanten mithilfe der unter [Erstellen eines Azure AD B2C-Mandanten](active-directory-b2c-get-started.md)beschriebenen Schritte. Nachdem Sie alle Schritte in diesem Artikel ausgeführt haben, ist das Blatt „B2C-Funktionen“ an Ihr Startmenü angeheftet.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="navigate-to-the-b2c-features-blade"></a>Navigieren zum Blatt „B2C-Funktionen“
Wenn das Blatt „B2C-Funktionen“ an Ihr Startmenü angeheftet ist, sehen Sie das Blatt, sobald Sie sich als globaler Administrator des B2C-Mandanten beim [Azure-Portal](https://portal.azure.com/) anmelden.

Sie können auf das Blatt auch zugreifen, indem Sie im [Azure-Portal](https://portal.azure.com/) im linken Navigationsbereich auf **Weitere Dienste** klicken und dann nach **Azure AD B2C** suchen.

> [!IMPORTANT]
> Sie müssen als globaler Administrator des B2C-Mandanten festgelegt sein, um auf das Blade mit den B2C-Features zugreifen zu können. Globale Administratoren anderer Mandanten oder Benutzer von Mandanten haben keinen Zugriff.  Mit dem Mandantenumschalter oben rechts im Azure-Portals können Sie zu Ihrem B2C-Mandanten wechseln.
> 
> 

## <a name="register-a-web-application"></a>Registrieren einer Webanwendung
1. Klicken Sie auf dem Blatt „B2C-Funktionen“ im Azure-Portal auf **Anwendungen**.
2. Klicken Sie oben auf dem Blatt auf **+Hinzufügen** .
3. Geben Sie einen **Namen** für die Anwendung ein, der die Funktion der Anwendung für Kunden beschreibt. Sie könnten z. B. „Contoso B2C-App“ eingeben.
4. Stellen Sie den Schalter **Web-App/Web-API einschließen** auf **Ja**. Die **Antwort-URLs** sind Endpunkte, an denen Azure AD B2C von Ihrer Anwendung angeforderte Token zurückgibt. Geben Sie z. B. Folgendes ein: `https://localhost:44316/`.
5. Klicken Sie auf **Speichern**, um Ihre Anwendung zu registrieren.
6. Klicken Sie auf die soeben erstellte Anwendung, und notieren Sie sich die global eindeutige **Anwendungsclient-ID** zur späteren Verwendung in Ihrem Code.


## <a name="register-a-web-api"></a>Registrieren einer Web-API
1. Klicken Sie auf dem Blatt „B2C-Funktionen“ im Azure-Portal auf **Anwendungen**.
2. Klicken Sie oben auf dem Blatt auf **+Hinzufügen** .
3. Geben Sie einen **Namen** für die Anwendung ein, der die Funktion der Anwendung für Kunden beschreibt. Sie könnten z.B. „Contoso B2C-API“ eingeben.
4. Stellen Sie den Schalter **Web-App/Web-API einschließen** auf **Ja**. Die **Antwort-URLs** sind Endpunkte, an denen Azure AD B2C von Ihrer Anwendung angeforderte Token zurückgibt. Geben Sie z. B. Folgendes ein: `https://localhost:44316/`.
5. Klicken Sie auf **Speichern**, um Ihre Anwendung zu registrieren.
6. Klicken Sie auf die soeben erstellte Anwendung, und notieren Sie sich die global eindeutige **Anwendungsclient-ID** zur späteren Verwendung in Ihrem Code.


## <a name="register-a-mobilenative-application"></a>Registrieren einer mobilen/systemeigenen Anwendung
1. Klicken Sie auf dem Blatt „B2C-Funktionen“ im Azure-Portal auf **Anwendungen**.
2. Klicken Sie oben auf dem Blatt auf **+Hinzufügen** .
3. Geben Sie einen **Namen** für die Anwendung ein, der die Funktion der Anwendung für Kunden beschreibt. Sie könnten z. B. „Contoso B2C-App“ eingeben.
4. Stellen Sie den Schalter **Nativen Client einschließen** auf **Ja**.
5. Geben Sie einen **Umleitungs-URI** mit einem benutzerdefinierten Schema ein. Beispielsweise „com.onmicrosoft.contoso.appname://redirect/path“. Achten Sie darauf, einen [funktionierenden Umleitungs-URI](#choosing-a-redirect-uri) zu wählen.
6. Klicken Sie auf **Speichern**, um Ihre Anwendung zu registrieren.
7. Klicken Sie auf die soeben erstellte Anwendung, und notieren Sie sich die global eindeutige **Anwendungsclient-ID** zur späteren Verwendung in Ihrem Code.

### <a name="choosing-a-redirect-uri"></a>Wählen eines Umleitungs-URIs
Bei der Auswahl eines Umleitungs-URIs für mobile/systemeigene Anwendungen sind zwei Aspekte zu berücksichtigen: 
* **Eindeutigkeit**: Das Schema des Umleitungs-URIS muss für jede Anwendung eindeutig sein. In unserem Beispiel (com.onmicrosoft.contoso.appname://redirect/path) verwenden wir „com.onmicrosoft.contoso.appname“ als Schema. Es wird empfohlen, diesem Muster zu folgen. Wenn zwei Anwendungen dasselbe Schema verwenden, wird den Benutzer das Dialogfeld „App auswählen“ angezeigt. Wenn der Benutzer die falsche Auswahl trifft, schlägt die Anmeldung fehl. 
* **Vollständigkeit**: Der Umleitungs-URI muss ein Schema und einen Pfad aufweisen. Der Pfad muss nach der Domäne mindestens ein Schrägstrich enthalten (Beispiel: „//contoso/“ funktioniert, aber „//contoso“ schlägt fehl). 

## <a name="build-a-quick-start-application"></a>Erstellen einer Schnellstart-App
Nachdem Sie nun über eine bei Azure AD B2C registrierte Anwendung verfügen, können Sie zum Einstieg eines der Schnellstarttutorials ausführen. Hier sind einige Vorschläge:

[!INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]


