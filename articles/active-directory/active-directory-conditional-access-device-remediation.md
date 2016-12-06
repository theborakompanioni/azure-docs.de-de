---
title: "Problembehandlung für Azure Active Directory-Zugriffsprobleme | Microsoft Docs"
description: "Es werden die Schritte beschrieben, die Sie ausführen können, um Zugriffsprobleme in Bezug auf die Onlineressourcen Ihrer Organisation zu beheben."
services: active-directory
keywords: "gerätebasierter bedingter Zugriff, Geräteregistrierung, Geräteregistrierung aktivieren, Geräteregistrierung und MDM"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8ad0156c-0812-4855-8563-6fbff6194174
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/11/2016
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 77031b3bbe2372bf2cac34bac45368ac40911641
ms.openlocfilehash: 81dcda2e11ff98f7f21495a83043a546786979c7


---
# <a name="troubleshooting-for-azure-active-directory-access-issues"></a>Problembehandlung für Azure Active Directory-Zugriffsprobleme
Sie versuchen, auf das SharePoint Online-Intranet Ihrer Organisation zuzugreifen und erhalten den Fehler „Zugriff verweigert“. Was ist zu tun?


In diesem Artikel werden die Schritte beschrieben, mit denen Sie Zugriffsprobleme in Bezug auf die Onlineressourcen Ihrer Organisation beheben können.

Hilfe zur Behebung von Zugriffsproblemen bei Azure Active Directory (Azure AD) erhalten Sie in dem Abschnitt des Artikels zu Ihrer Geräteplattform:

* Windows-Gerät
* iOS-Gerät (In Kürze finden Sie hier auch Hilfe zu iPhones und iPads.)
* Android-Gerät (In Kürze finden Sie hier auch Hilfe zu Android-Smartphones und -Tablets.)

## <a name="access-from-a-windows-device"></a>Zugriff mit einem Windows-Gerät
Wenn Ihr Gerät mit einer der folgenden Plattformen ausgeführt wird, können Sie in den nächsten Abschnitten nach der Fehlermeldung suchen, die angezeigt wird, wenn Sie versuchen, auf eine Anwendung oder einen Dienst zuzugreifen:

* Windows 10
* Windows 8.1
* Windows 8
* Windows 7
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2

### <a name="device-is-not-registered"></a>Das Gerät ist nicht registriert
Wenn das Gerät nicht bei Azure AD registriert und die Anwendung durch eine gerätebasierte Richtlinie geschützt ist, wird ggf. eine Seite mit einer der folgenden Fehlermeldungen angezeigt:

![Meldungen vom Typ „Zugriff nicht möglich“ für nicht registrierte Geräte](./media/active-directory-conditional-access-device-remediation/01.png "Scenario")

Versuchen Sie Folgendes, wenn das Gerät einer Domäne der Active Directory-Instanz in Ihrer Organisation angehört:

1. Vergewissern Sie sich, dass Sie sich bei Windows mit Ihrem Geschäftskonto (Active Directory-Konto) anmelden.
2. Stellen Sie über ein VPN (Virtual Private Network) oder DirectAccess eine Verbindung mit Ihrem Unternehmensnetzwerk her.
3. Drücken Sie nach dem Herstellen der Verbindung Windows-Logo-Taste+L, um die Windows-Sitzung zu sperren.
4. Geben Sie die Anmeldeinformationen Ihres Geschäftskontos ein, um die Windows-Sitzung zu entsperren.
5. Warten Sie eine Minute, und versuchen Sie dann erneut, auf die Anwendung oder den Dienst zuzugreifen.
6. Sollte wieder die gleiche Seite angezeigt werden, klicken Sie auf den Link **Weitere Details**, und wenden Sie sich mit diesen Details an Ihren Administrator.

Falls Ihr Gerät keiner Domäne angehört und über Windows 10 verfügt, haben Sie zwei Möglichkeiten:

* Ausführen von Azure AD Join
* Hinzufügen Ihres Geschäfts-, Schul- oder Unikontos zu Windows

Informationen zu den Unterschieden dieser beiden Möglichkeiten finden Sie unter [Verwenden von Windows 10-Geräten an Ihrem Arbeitsplatz](active-directory-azureadjoin-windows10-devices.md).

Führen Sie zur Ausführung von Azure AD Join die folgenden Schritte für die Plattform aus, auf der Ihr Gerät ausgeführt wird. (Azure AD Join ist auf Windows Phones nicht verfügbar.)

**Windows 10 Anniversary Update**

1. Starten Sie die App **Einstellungen** .
2. Klicken Sie auf **Konten** > **Auf Arbeits- oder Schulkonto zugreifen**.
3. Klicken Sie auf **Verbinden**.
4. Klicken Sie auf **Dieses Gerät in Azure Active Directory einbinden**.
5. Führen Sie die Authentifizierung für Ihre Organisation durch, geben Sie bei Aufforderung die Daten für die Multi-Factor Authentication an, und führen Sie dann die angezeigten Schritte aus.
6. Melden Sie sich ab, und melden Sie sich dann mit Ihrem Geschäftskonto wieder an.
7. Versuchen Sie erneut, auf die Anwendung zuzugreifen.

**Windows 10 November 2015 Update**

1. Starten Sie die App **Einstellungen** .
2. Klicken Sie auf **System** > **Info**.
3. Klicken Sie auf **Azure AD beitreten**.
4. Führen Sie die Authentifizierung für Ihre Organisation durch, geben Sie bei Aufforderung die Daten für die Multi-Factor Authentication an, und führen Sie dann die angezeigten Schritte aus.
5. Melden Sie sich ab, und melden Sie sich dann mit Ihrem Geschäftskonto (Azure AD-Konto) wieder an.
6. Versuchen Sie erneut, auf die Anwendung zuzugreifen.

Führen Sie die folgenden Schritte aus, um Ihr Geschäfts-, Schul- oder Unikonto hinzuzufügen:

**Windows 10 Anniversary Update**

1. Starten Sie die App **Einstellungen** .
2. Klicken Sie auf **Konten** > **Auf Arbeits- oder Schulkonto zugreifen**.
3. Klicken Sie auf **Verbinden**.
4. Führen Sie die Authentifizierung für Ihre Organisation durch, geben Sie bei Aufforderung die Daten für die Multi-Factor Authentication an, und führen Sie dann die angezeigten Schritte aus.
5. Versuchen Sie erneut, auf die Anwendung zuzugreifen.

**Windows 10 November 2015 Update**

1. Starten Sie die App **Einstellungen** .
2. Klicken Sie auf **Konten** > **Ihre Konten**.
3. Klicken Sie auf **Geschäfts- oder Schulkonto hinzufügen**.
4. Führen Sie die Authentifizierung für Ihre Organisation durch, geben Sie bei Aufforderung die Daten für die Multi-Factor Authentication an, und führen Sie dann die angezeigten Schritte aus.
5. Versuchen Sie erneut, auf die Anwendung zuzugreifen.

Wenn das Gerät keiner Domäne angehört und über Windows 8.1 verfügt, können Sie mit den folgenden Schritten eine Arbeitsplatzeinbindung und eine Registrierung bei Microsoft Intune durchführen:

1. Öffnen Sie **PC-Einstellungen**.
2. Klicken Sie auf **Netzwerk** > **Arbeitsplatz**.
3. Klicken Sie auf **Verknüpfen**.
4. Führen Sie die Authentifizierung für Ihre Organisation durch, geben Sie bei Aufforderung die Daten für die Multi-Factor Authentication an, und führen Sie dann die angezeigten Schritte aus.
5. Klicken Sie auf **Einschalten**.
6. Versuchen Sie erneut, auf die Anwendung zuzugreifen.

### <a name="browser-is-not-supported"></a>Browser wird nicht unterstützt
Unter Umständen wird der Zugriff verweigert, wenn Sie versuchen, mit einem der folgenden Browser auf eine Anwendung oder einen Dienst zuzugreifen:

* Chrome, Firefox oder andere Browser, bei denen es sich nicht um Microsoft Edge oder Microsoft Internet Explorer unter Windows 10 oder Windows Server 2016 handelt
* Firefox unter Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2012 oder Windows Server 2008 R2

Eine Fehlerseite wird angezeigt, die wie folgt aussieht:

![Meldung vom Typ „Zugriff nicht möglich“ für nicht unterstützte Browser](./media/active-directory-conditional-access-device-remediation/02.png "Scenario")

Die einzige Abhilfe ist die Verwendung eines Browsers, der von der Anwendung für Ihre Geräteplattform unterstützt wird.

## <a name="next-steps"></a>Nächste Schritte
[Bedingter Zugriff mit Azure Active Directory](active-directory-conditional-access.md)




<!--HONumber=Nov16_HO2-->


