---
title: 'Azure Active Directory B2C: Erweiterbares Richtlinienframework | Microsoft Docs'
description: "Ein Thema über das erweiterbare Richtlinienframework von Azure Active Directory B2C und die Erstellung verschiedener Richtlinientypen"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 0d453e72-7f70-4aa2-953d-938d2814d5a9
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
translationtype: Human Translation
ms.sourcegitcommit: 4b13c040a15bef2f04d2cd2126e2270d061898bd
ms.openlocfilehash: 55ed7d44006f18fa3573a5bef5e8f24107ca20fa


---
# <a name="azure-active-directory-b2c-extensible-policy-framework"></a>Azure Active Directory B2C: Erweiterbares Richtlinienframework
## <a name="the-basics"></a>Grundlagen
Das erweiterbare Richtlinienframework von Azure Active Directory (Azure AD) B2C ist der Hauptvorteil des Diensts. Die Richtlinien beschreiben Benutzeroberflächen im Zusammenhang mit der Kundenidentität, z. B. Registrierung, Anmeldung oder Profilbearbeitung. Eine Registrierungsrichtlinie ermöglicht es z. B., das Verhalten zu steuern, indem die folgenden Einstellungen konfiguriert werden:

* Kontotypen (Konten sozialer Netzwerke wie Facebook oder lokale Konten wie die E-Mail-Adresse), die Kunden zum Registrieren für die Anwendung verwenden können.
* Attribute (z. B. Vorname, Postleitzahl und Schuhgröße), die der Kunde während des Anmeldeprozesses angeben muss.
* Verwendung der Multi-Factor Authentication.
* Das Aussehen und Verhalten aller Registrierungsseiten.
* Informationen (als Ansprüche im Token ausgedrückt), die die Anwendung nach Abschluss der Richtlinienausführung erhält.

Sie können mehrere Richtlinien verschiedener Typen in Ihrem Mandanten erstellen und bei Bedarf in Ihrer Anwendung verwenden. Richtlinien können anwendungsübergreifend wiederverwendet werden. Dadurch können Entwickler Benutzeroberflächen im Zusammenhang mit der Kundenidentität mit minimalen oder ganz ohne Änderungen an ihrem Code definieren und ändern.

Richtlinien können über eine einfache Entwicklerschnittstelle verwendet werden. Ihre Anwendung löst eine Richtlinie mithilfe einer standardmäßigen HTTP-Authentifizierungsanforderung aus (übergibt einen Richtlinienparameter in der Anforderung) und empfängt ein benutzerdefiniertes Token als Antwort. Der einzige Unterschied zwischen Anforderungen, die eine Registrierungsrichtlinie aufrufen, und Anforderungen, die eine Anmelderichtlinie aufrufen, ist beispielsweise der Richtlinienname, der im Parameter „p“ der Abfragezeichenfolge verwendet wird:

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up policy

```

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in policy

```

Weitere Informationen zum Richtlinienframework finden Sie in diesem [Blogbeitrag](http://blogs.technet.com/b/ad/archive/2015/11/02/a-look-inside-azuread-b2c-with-kim-cameron.aspx).

## <a name="create-a-sign-up-policy"></a>Erstellen einer Registrierungsrichtlinie
Für die Registrierung bei Ihrer Anwendung müssen Sie eine Registrierungsrichtlinie erstellen. Diese Richtlinie beschreibt die Benutzeroberflächen, die Kunden bei der Registrierung durchlaufen, und den Inhalt der Token, die die Anwendung bei erfolgreichen Registrierungen erhält.

1. [Führen Sie diese Schritte aus, um im Azure-Portal zum Blatt „B2C-Funktionen“ zu navigieren](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klicken Sie auf **Registrierungsrichtlinien**.
3. Klicken Sie oben auf dem Blatt auf **+Hinzufügen** .
4. Der **Name** bestimmt den Namen der Registrierungsrichtlinie, die von der Anwendung verwendet wird. Geben Sie beispielsweise "SiUp" ein.
5. Klicken Sie auf **Identitätsanbieter** , und wählen Sie „E-Mail-Registrierung“ aus. Optional können Sie auch soziale Netzwerke als Identitätsanbieter auswählen, sofern bereits konfiguriert. Klicken Sie auf **OK**.
6. Klicken Sie auf **Registrierungsattribute**. Hier wählen Sie die Attribute aus, die der Kunde bei der Registrierung angeben soll. Wählen Sie z. B. "Land/Region", "Anzeigename" und "Postleitzahl" aus. Klicken Sie auf **OK**.
7. Klicken Sie auf **Anwendungsansprüche**. Hier wählen Sie die Ansprüche aus, die in den Token nach einer erfolgreichen Registrierung an die Anwendung zurückgegeben werden sollen. Wählen Sie z. B. "Anzeigename", "Identitätsanbieter", "Postleitzahl", "Benutzer ist neu" und "Objekt-ID des Benutzers" aus.
8. Klicken Sie auf **Erstellen**. Beachten Sie, dass die soeben erstellte Richtlinie als „**B2C_1_SiUp**“ (das Fragment **B2C\_1\_** wird automatisch hinzugefügt) auf dem Blatt **Registrierungsrichtlinien** angezeigt wird.
9. Öffnen Sie die Richtlinie, indem Sie auf „**B2C_1_SiUp**“ klicken.
10. Wählen Sie „Contoso B2C-App“ in der Dropdownliste **Anwendungen** und `https://localhost:44321/` in der Dropdownliste **Antwort-URL/Umleitungs-URI** aus.
11. Klicken Sie auf **Jetzt ausführen**. Eine neue Browserregisterkarte wird geöffnet, und Sie können die Benutzeroberfläche für die Registrierung bei Ihrer Anwendung durchgehen.
    
    > [!NOTE]
    > Es dauert bis zu einer Minute, bis die Erstellung und Aktualisierung von Richtlinien wirksam wird.
    > 
    > 

## <a name="create-a-sign-in-policy"></a>Erstellen einer Anmelderichtlinie
Für die Anmeldung bei Ihrer Anwendung müssen Sie eine Anmelderichtlinie erstellen. Diese Richtlinie beschreibt die Benutzeroberflächen, die Kunden bei der Anmeldung durchlaufen, und den Inhalt der Token, die die Anwendung bei erfolgreichen Anmeldungen erhält.

1. [Führen Sie diese Schritte aus, um im Azure-Portal zum Blatt „B2C-Funktionen“ zu navigieren](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klicken Sie auf **Anmelderichtlinien**.
3. Klicken Sie oben auf dem Blatt auf **+Hinzufügen** .
4. Der **Name** bestimmt den Namen der Anmelderichtlinie, die von der Anwendung verwendet wird. Geben Sie beispielsweise "SiIn" ein.
5. Klicken Sie auf **Identitätsanbieter** , und wählen Sie „Anmeldung für lokales Konto“ aus. Optional können Sie auch soziale Netzwerke als Identitätsanbieter auswählen, sofern bereits konfiguriert. Klicken Sie auf **OK**.
6. Klicken Sie auf **Anwendungsansprüche**. Hier wählen Sie die Ansprüche aus, die in den Token nach einer erfolgreichen Anmeldung an die Anwendung zurückgegeben werden sollen. Wählen Sie z.B. „Anzeigename“, „Identitätsanbieter“, „Postleitzahl“ und „Objekt-ID des Benutzers“ aus. Klicken Sie auf **OK**.
7. Klicken Sie auf **Erstellen**. Beachten Sie, dass die soeben erstellte Richtlinie als „**B2C_1_SiIn**“ (das Fragment **B2C\_1\_** wird automatisch hinzugefügt) auf dem Blatt **Anmelderichtlinien** angezeigt wird.
8. Öffnen Sie die Richtlinie, indem Sie auf „**B2C_1_SiIn**“ klicken.
9. Wählen Sie „Contoso B2C-App“ in der Dropdownliste **Anwendungen** und `https://localhost:44321/` in der Dropdownliste **Antwort-URL/Umleitungs-URI** aus.
10. Klicken Sie auf **Jetzt ausführen**. Eine neue Browserregisterkarte wird geöffnet, und Sie können die Benutzeroberfläche für die Anmeldung bei Ihrer Anwendung durchgehen.
    
    > [!NOTE]
    > Es dauert bis zu einer Minute, bis die Erstellung und Aktualisierung von Richtlinien wirksam wird.
    > 
    > 

## <a name="create-a-sign-up-or-sign-in-policy"></a>Erstellen einer Registrierungs- oder Anmelderichtlinie
Mit dieser Richtlinie werden die Registrierung und Anmeldung von Kunden in ein und derselben Konfiguration verarbeitet. Die Kunden werden je nach Kontext auf den jeweils entsprechenden Pfad geleitet (Registrierung oder Anmeldung). Außerdem wird mit der Richtlinie der Inhalt von Token beschrieben, die die Anwendung nach erfolgreichen Registrierungen oder Abmeldungen erhält.  Ein Codebeispiel für die Richtlinie zur Registrierung oder Anmeldung ist [hier verfügbar](active-directory-b2c-devquickstarts-web-dotnet-susi.md).

1. [Führen Sie diese Schritte aus, um im Azure-Portal zum Blatt „B2C-Funktionen“ zu navigieren](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klicken Sie auf **Registrierungs- oder Anmelderichtlinien**.
3. Klicken Sie oben auf dem Blatt auf **+Hinzufügen** .
4. Der **Name** bestimmt den Namen der Registrierungsrichtlinie, die von der Anwendung verwendet wird. Geben Sie z.B. „SiUpIn“ ein.
5. Klicken Sie auf **Identitätsanbieter** , und wählen Sie „E-Mail-Registrierung“ aus. Optional können Sie auch soziale Netzwerke als Identitätsanbieter auswählen, sofern bereits konfiguriert. Klicken Sie auf **OK**.
6. Klicken Sie auf **Registrierungsattribute**. Hier wählen Sie die Attribute aus, die der Kunde bei der Registrierung angeben soll. Wählen Sie z. B. "Land/Region", "Anzeigename" und "Postleitzahl" aus. Klicken Sie auf **OK**.
7. Klicken Sie auf **Anwendungsansprüche**. Hier wählen Sie die Ansprüche aus, die in den Token nach einer erfolgreichen Registrierung oder Anmeldung an die Anwendung zurückgegeben werden sollen. Wählen Sie z. B. "Anzeigename", "Identitätsanbieter", "Postleitzahl", "Benutzer ist neu" und "Objekt-ID des Benutzers" aus.
8. Klicken Sie auf **Erstellen**. Beachten Sie, dass die soeben erstellte Richtlinie als „**B2C_1_SiUpIn**“ (das Fragment **B2C\_1\_** wird automatisch hinzugefügt) auf dem Blatt **Registrierungs- oder Anmelderichtlinien** angezeigt wird.
9. Öffnen Sie die Richtlinie, indem Sie auf „**B2C_1_SiUpIn**“ klicken.
10. Wählen Sie „Contoso B2C-App“ in der Dropdownliste **Anwendungen** und `https://localhost:44321/` in der Dropdownliste **Antwort-URL/Umleitungs-URI** aus.
11. Klicken Sie auf **Jetzt ausführen**. Eine neue Browserregisterkarte wird geöffnet, und Sie können entsprechend der Konfiguration die Benutzeroberfläche für die Registrierung oder Anmeldung von Kunden durchgehen.
    
    > [!NOTE]
    > Es dauert bis zu einer Minute, bis die Erstellung und Aktualisierung von Richtlinien wirksam wird.
    > 
    > 

## <a name="create-a-profile-editing-policy"></a>Erstellen einer Richtlinie für die Profilbearbeitung
Um die Profilbearbeitung in Ihrer Anwendung zu ermöglichen, müssen Sie eine Richtlinie für die Profilbearbeitung erstellen. Diese Richtlinie beschreibt die Benutzeroberflächen, die Consumer bei der Profilbearbeitung durchgehen, und den Inhalt der Token, die die Anwendung beim erfolgreichen Abschluss erhält.

1. [Führen Sie diese Schritte aus, um im Azure-Portal zum Blatt „B2C-Funktionen“ zu navigieren](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klicken Sie auf **Richtlinien für die Profilbearbeitung**.
3. Klicken Sie oben auf dem Blatt auf **+Hinzufügen** .
4. Der **Name** bestimmt den Namen der Richtlinie für die Profilbearbeitung, die von der Anwendung verwendet wird. Geben Sie beispielsweise "SiPe" ein.
5. Klicken Sie auf **Identitätsanbieter** , und wählen Sie "E-Mail-Adresse" aus. Optional können Sie auch soziale Netzwerke als Identitätsanbieter auswählen, sofern bereits konfiguriert. Klicken Sie auf **OK**.
6. Klicken Sie auf **Profilattribute**. Hier wählen Sie Attribute aus, die der Kunde anzeigen und bearbeiten kann. Wählen Sie z. B. „Land/Region“, „Anzeigename“ und „Postleitzahl“ aus. Klicken Sie auf **OK**.
7. Klicken Sie auf **Anwendungsansprüche**. Hier wählen Sie die Ansprüche aus, die in den zurückgegebenen Token nach einer erfolgreichen Profilbearbeitung an die Anwendung zurückgegeben werden sollen. Wählen Sie z. B. "Anzeigename" und "Postleitzahl" aus.
8. Klicken Sie auf **Erstellen**. Beachten Sie, dass die soeben erstellte Richtlinie als „**B2C_1_SiPe**“ (das Fragment **B2C\_1\_** wird automatisch hinzugefügt) auf dem Blatt **Richtlinien für die Profilbearbeitung** angezeigt wird.
9. Öffnen Sie die Richtlinie, indem Sie auf „**B2C_1_SiPe**“ klicken.
10. Wählen Sie „Contoso B2C-App“ in der Dropdownliste **Anwendungen** und `https://localhost:44321/` in der Dropdownliste **Antwort-URL/Umleitungs-URI** aus.
11. Klicken Sie auf **Jetzt ausführen**. Eine neue Browserregisterkarte wird geöffnet, und Sie können die Benutzeroberfläche für die Profilbearbeitung in Ihrer Anwendung durchgehen.
    
    > [!NOTE]
    > Es dauert bis zu einer Minute, bis die Erstellung und Aktualisierung von Richtlinien wirksam wird.
    > 
    > 

## <a name="create-a-password-reset-policy"></a>Erstellen einer Richtlinie zur Kennwortrücksetzung
Um eine differenzierte Kennwortrücksetzung in Ihrer Anwendung zu aktivieren, müssen Sie eine Richtlinie zur Kennwortrücksetzung erstellen. Beachten Sie, dass die [hier](active-directory-b2c-reference-sspr.md) angegebene Option zur mandantenweiten Kennwortzurücksetzung immer noch für Anmeldungsrichtlinien gültig ist. Diese Richtlinie beschreibt die Benutzeroberflächen, die die Kunden während der Kennwortrücksetzung durchlaufen, und die Inhalte der Token, die die Anwendung beim erfolgreichen Abschluss erhält.

1. [Führen Sie diese Schritte aus, um im Azure-Portal zum Blatt „B2C-Funktionen“ zu navigieren](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klicken Sie auf **Richtlinien zur Kennwortrücksetzung**.
3. Klicken Sie oben auf dem Blatt auf **+Hinzufügen** .
4. Mit **Name** geben Sie den Namen der Richtlinie zur Kennwortzurücksetzung ein, die von der Anwendung verwendet wird. Geben Sie z. B. „SSPR“ ein.
5. Klicken Sie auf **Identitätsanbieter** , und wählen Sie „Kennwort mittels E-Mail-Adresse zurücksetzen“ aus. Klicken Sie auf **OK**.
6. Klicken Sie auf **Anwendungsansprüche**. Hier wählen Sie die Ansprüche aus, die in den zurückgegebenen Token nach einer erfolgreichen Kennwortrücksetzung an die Anwendung zurückgegeben werden sollen. Wählen Sie z. B. „Objekt-ID des Benutzers“.
7. Klicken Sie auf **Erstellen**. Beachten Sie, dass die soeben erstellte Richtlinie als „**B2C_1_SSPR**“ (das Fragment **B2C\_1\_** wird automatisch hinzugefügt) auf dem Blatt **Richtlinien für die Kennwortrücksetzung** angezeigt wird.
8. Öffnen Sie die Richtlinie, indem Sie auf „**B2C_1_SSPR**“ klicken.
9. Wählen Sie „Contoso B2C-App“ in der Dropdownliste **Anwendungen** und `https://localhost:44321/` in der Dropdownliste **Antwort-URL/Umleitungs-URI** aus.
10. Klicken Sie auf **Jetzt ausführen**. Eine neue Browserregisterkarte wird geöffnet, und Sie können die Benutzeroberfläche für die Kennwortrücksetzung in Ihrer Anwendung durchgehen.
    
    > [!NOTE]
    > Es dauert bis zu einer Minute, bis die Erstellung und Aktualisierung von Richtlinien wirksam wird.
    > 
    > 

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Token, Sitzung und einmaliges Anmelden – Konfiguration](active-directory-b2c-token-session-sso.md).




<!--HONumber=Dec16_HO4-->


