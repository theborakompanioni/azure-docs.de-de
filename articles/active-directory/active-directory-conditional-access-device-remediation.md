---
title: "Behandeln von Zugriffsproblemen im Azure-Portal auf einem Windows-Gerät | Microsoft-Dokumentation"
description: "Es werden die Schritte beschrieben, die Sie ausführen können, um Zugriffsprobleme in Bezug auf die Onlineressourcen Ihrer Organisation zu beheben."
services: active-directory
keywords: "gerätebasierter bedingter Zugriff, Geräteregistrierung, Geräteregistrierung aktivieren, Geräteregistrierung und MDM"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 8ad0156c-0812-4855-8563-6fbff6194174
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/06/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 988e7fe2ae9f837b661b0c11cf30a90644085e16
ms.openlocfilehash: 4b8627f3cdd13578f9b4c35297bf5d550dccfd07
ms.lasthandoff: 04/06/2017


---
# <a name="you-cant-get-there-from-here-on-a-windows-device"></a>Zugriffsprobleme auf einem Windows-Gerät

Beim Versuch, beispielsweise auf das SharePoint Online-Intranet Ihrer Organisation zuzugreifen, erscheint unter Umständen eine Seite mit dem Hinweis auf ein *Zugriffsproblem*. Diese Seite wird angezeigt, weil der Administrator eine Richtlinie für bedingten Zugriff konfiguriert hat, die unter bestimmten Bedingungen den Zugriff auf Ressourcen Ihrer Organisation unterbindet. Es kann zwar sein, dass sich das Problem nur in Zusammenarbeit mit dem Helpdesk oder Administrator beheben lässt, Sie können jedoch zunächst selbst ein paar Dinge ausprobieren.

Überprüfen Sie bei Verwendung eines **Windows-Geräts** Folgendes:

- Verwenden Sie einen unterstützten Browser?

- Wird auf Ihrem Gerät eine unterstützte Version von Windows ausgeführt?

- Ist Ihr Gerät kompatibel?






## <a name="supported-browser"></a>Unterstützte Browser

Wenn Ihr Administrator eine Richtlinie für bedingten Zugriff konfiguriert hat, können Sie nur über einen unterstützten Browser auf die Ressourcen Ihrer Organisation zugreifen. Auf Windows-Geräten werden ausschließlich **Internet Explorer** und **Edge** unterstützt.

Sie können ganz einfach ermitteln, ob das Ressourcenzugriffsproblem auf einen nicht unterstützten Browser zurückzuführen ist. Sehen Sie sich dazu den Detailbereich der Fehlerseite an:

![Meldung vom Typ „Zugriff nicht möglich“ für nicht unterstützte Browser](./media/active-directory-conditional-access-device-remediation/02.png "Szenario")

Die einzige Abhilfe ist die Verwendung eines Browsers, der von der Anwendung für Ihre Geräteplattform unterstützt wird. Eine vollständige Liste mit unterstützten Browsern finden Sie unter [Unterstützte Browser](active-directory-conditional-access-supported-apps.md#supported-browsers).  


## <a name="supported-versions-of-windows"></a>Unterstützte Windows-Versionen

Das Windows-Betriebssystem auf Ihrem Gerät muss folgende Kriterien erfüllen: 

- Wenn sich auf Ihrem Gerät ein Windows-Desktopbetriebssystem befindet, muss es sich dabei mindestens um Windows 7 handeln.
- Wenn sich auf Ihrem Gerät ein Windows-Serverbetriebssystem befindet, muss es sich dabei mindestens um Windows Server 2008 R2 handeln. 


## <a name="compliant-device"></a>Kompatibles Gerät

Der Administrator hat unter Umständen eine Richtlinie für bedingten Zugriff konfiguriert, die dafür sorgt, dass auf die Ressourcen Ihrer Organisation nur über kompatible Geräte zugegriffen werden kann. Kompatible Geräte müssen entweder Ihrem lokalen Active Directory oder Ihrem Azure Active Directory angehören.

Sie können ganz einfach ermitteln, ob das Ressourcenzugriffsproblem auf ein nicht kompatibles Gerät zurückzuführen ist. Sehen Sie sich dazu den Detailbereich der Fehlerseite an:
 
![Meldungen vom Typ „Zugriff nicht möglich“ für nicht registrierte Geräte](./media/active-directory-conditional-access-device-remediation/01.png "Szenario")


### <a name="is-your-device-joined-to-an-on-premises-active-directory"></a>Gehört Ihr Gerät einem lokalen Active Directory an?

**Falls Ihr Gerät einem lokalen Active Directory in Ihrer Organisation angehört, gehen Sie wie folgt vor:**

1. Vergewissern Sie sich, dass Sie sich bei Windows mit Ihrem Geschäftskonto (Active Directory-Konto) anmelden.
2. Stellen Sie über ein VPN (Virtual Private Network) oder DirectAccess eine Verbindung mit Ihrem Unternehmensnetzwerk her.
3. Drücken Sie nach dem Herstellen der Verbindung Windows-Logo-Taste+L, um die Windows-Sitzung zu sperren.
4. Entsperren Sie Ihre Windows-Sitzung, indem Sie die Anmeldeinformationen Ihres Geschäftskontos eingeben.
5. Warten Sie eine Minute, und versuchen Sie dann erneut, auf die Anwendung oder den Dienst zuzugreifen.
6. Sollte wieder die gleiche Seite angezeigt werden, klicken Sie auf den Link **Weitere Details**, und wenden Sie sich mit diesen Details an Ihren Administrator.


### <a name="is-your-device-not-joined-to-an-on-premises-active-directory"></a>Gehört Ihr Gerät keinem lokalen Active Directory an?

Falls Ihr Gerät keinem lokalen Active Directory angehört und über Windows 10 verfügt, haben Sie zwei Möglichkeiten:

* Ausführen von Azure AD Join
* Hinzufügen Ihres Geschäfts-, Schul- oder Unikontos zu Windows

Informationen zu den Unterschieden dieser beiden Möglichkeiten finden Sie unter [Verwenden von Windows 10-Geräten an Ihrem Arbeitsplatz](active-directory-azureadjoin-windows10-devices.md).  
Mögliche Szenarien:

- Gehört das Gerät Ihrer Organisation, führen Sie Azure AD Join aus.
- Handelt es sich um ein persönliches Gerät oder um ein Windows Phone, fügen Sie Ihr Geschäfts-, Schul- oder Unikonto zu Windows hinzu. 



#### <a name="azure-ad-join-on-windows-10"></a>Azure AD Join unter Windows 10

Die Schritte, die zur Verknüpfung Ihres Geräts mit Azure AD ausgeführt werden müssen, sind abhängig von der Windows 10-Version, die auf dem Gerät ausgeführt wird. Führen Sie den Befehl **winver** aus, um die Version Ihres Windows 10-Betriebssystems zu ermitteln: 

![Windows-Version](./media/active-directory-conditional-access-device-remediation/03.png )


**Windows 10 Anniversary Update (Version 1607):**

1. Starten Sie die App **Einstellungen** .
2. Klicken Sie auf **Konten** > **Auf Arbeits- oder Schulkonto zugreifen**.
3. Klicken Sie auf **Verbinden**.
4. Klicken Sie auf **Dieses Gerät in Azure Active Directory einbinden**.
5. Führen Sie die Authentifizierung für Ihre Organisation durch, geben Sie bei Aufforderung die Daten für die Multi-Factor Authentication an, und führen Sie dann die angezeigten Schritte aus.
6. Melden Sie sich ab, und melden Sie sich dann mit Ihrem Geschäftskonto wieder an.
7. Versuchen Sie erneut, auf die Anwendung zuzugreifen.

**Windows 10 November 2015 Update (Version 1511):**

1. Starten Sie die App **Einstellungen** .
2. Klicken Sie auf **System** > **Info**.
3. Klicken Sie auf **Azure AD beitreten**.
4. Führen Sie die Authentifizierung für Ihre Organisation durch, geben Sie bei Aufforderung die Daten für die Multi-Factor Authentication an, und führen Sie dann die angezeigten Schritte aus.
5. Melden Sie sich ab, und melden Sie sich dann mit Ihrem Geschäftskonto (Azure AD-Konto) wieder an.
6. Versuchen Sie erneut, auf die Anwendung zuzugreifen.


#### <a name="workplace-join-on-windows-81"></a>Workplace Join unter Windows 8.1

Wenn das Gerät keiner Domäne angehört und über Windows 8.1 verfügt, können Sie mit den folgenden Schritten eine Arbeitsplatzeinbindung und eine Registrierung bei Microsoft Intune durchführen:

1. Öffnen Sie **PC-Einstellungen**.
2. Klicken Sie auf **Netzwerk** > **Arbeitsplatz**.
3. Klicken Sie auf **Verknüpfen**.
4. Führen Sie die Authentifizierung für Ihre Organisation durch, geben Sie bei Aufforderung die Daten für die Multi-Factor Authentication an, und führen Sie dann die angezeigten Schritte aus.
5. Klicken Sie auf **Einschalten**.
6. Versuchen Sie erneut, auf die Anwendung zuzugreifen.



#### <a name="add-your-work-or-school-account-to-windows"></a>Hinzufügen Ihres Geschäfts-, Schul- oder Unikontos zu Windows 


**Windows 10 Anniversary Update (Version 1607):**

1. Starten Sie die App **Einstellungen** .
2. Klicken Sie auf **Konten** > **Auf Arbeits- oder Schulkonto zugreifen**.
3. Klicken Sie auf **Verbinden**.
4. Führen Sie die Authentifizierung für Ihre Organisation durch, geben Sie bei Aufforderung die Daten für die Multi-Factor Authentication an, und führen Sie dann die angezeigten Schritte aus.
5. Versuchen Sie erneut, auf die Anwendung zuzugreifen.


**Windows 10 November 2015 Update (Version 1511):**

1. Starten Sie die App **Einstellungen** .
2. Klicken Sie auf **Konten** > **Ihre Konten**.
3. Klicken Sie auf **Geschäfts- oder Schulkonto hinzufügen**.
4. Führen Sie die Authentifizierung für Ihre Organisation durch, geben Sie bei Aufforderung die Daten für die Multi-Factor Authentication an, und führen Sie dann die angezeigten Schritte aus.
5. Versuchen Sie erneut, auf die Anwendung zuzugreifen.





## <a name="next-steps"></a>Nächste Schritte
[Bedingter Zugriff mit Azure Active Directory](active-directory-conditional-access.md)


