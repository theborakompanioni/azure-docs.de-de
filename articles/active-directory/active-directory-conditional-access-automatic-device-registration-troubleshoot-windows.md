---
title: "Problembehandlung bei der automatischen Registrierung von Computern, die in die Azure AD-Domäne eingebunden sind, für Windows 10 und Windows Server 2016| Microsoft-Dokumentation"
description: "Problembehandlung bei der automatischen Registrierung von Computern, die in die Azure AD-Domäne eingebunden sind, für Windows 10 und Windows Server 2016."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: d3688b6a71dcd9964839da3cda35413d00f362d1
ms.openlocfilehash: 836d43b17515beb565748f28603069c7c6a52fbf
ms.lasthandoff: 02/15/2017


---
# <a name="troubleshooting-auto-registration-of-domain-joined-computers-to-azure-ad--windows-10-and-windows-server-2016"></a>Beheben von Problemen bei der automatischen Registrierung von Computern, die in die Azure AD-Domäne eingebunden sind, für Windows 10 und Windows Server 2016

Dieses Thema gilt für die folgenden Clients:

-    Windows 10
-    Windows Server 2016

Für andere Windows-Clients finden Sie weitere Informationen unter [Beheben von Problemen bei der automatischen Registrierung von Computern, die in die Azure AD-Domäne eingebunden sind, für kompatible Windows-Clients](active-directory-conditional-access-automatic-device-registration-troubleshoot-windows-legacy.md).

In diesem Thema wird davon ausgegangen, dass Sie die automatische Registrierung von in die Domäne eingebundenen Geräten gemäß der Beschreibung unter [Konfigurieren der automatischen Registrierung von in die Domäne eingebundenen Windows-Geräten mit Azure Active Directory](active-directory-conditional-access-automatic-device-registration-get-started.md) konfiguriert haben, um die folgenden Szenarien zu unterstützen:

1.    [Gerätebasierter bedingter Zugriff](active-directory-conditional-access-automatic-device-registration-setup.md)

2.    [Unternehmensroaming von Einstellungen](active-directory-windows-enterprise-state-roaming-overview.md)

3.    [Windows Hello for Business](active-directory-azureadjoin-passport-deployment.md)


Dieses Dokument enthält Anleitungen zur Problembehandlung zum Beheben potenzieller Probleme. 

Die Registrierung wird im Windows 10-Update von November 2015 und höher unterstützt.  
Es wird empfohlen, das Anniversary Update zu verwenden, um die oben genannten Szenarien zu ermöglichen.

## <a name="step-1-retrieve-the-registration-status"></a>Schritt 1: Abrufen des Registrierungsstatus 

**So rufen Sie den Registrierungsstatus ab**

1. Öffnen Sie die Eingabeaufforderung als Administrator.

2. Geben Sie **dsregcmd /status** ein.



    +----------------------------------------------------------------------+
    | Device State                                                         |  +----------------------------------------------------------------------+
    
        AzureAdJoined : YES
     EnterpriseJoined : NO DeviceId : 5820fbe9-60c8-43b0-bb11-44aee233e4e7 Thumbprint : B753A6679CE720451921302CA873794D94C6204A KeyContainerId : bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider : Microsoft Platform Crypto Provider TpmProtected : YES KeySignTest: : MUST Run elevated to test.
                  Idp : login.windows.net TenantId : 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName : Contoso AuthCodeUrl : https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl : https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/token MdmUrl : https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl : https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl : https://portal.manage-beta.microsoft.com/?portalAction=Compliance SettingsUrl : eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ== JoinSrvVersion : 1.0 JoinSrvUrl : https://enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId : urn:ms-drs:enterpriseregistration.windows.net KeySrvVersion : 1.0 KeySrvUrl : https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId : urn:ms-drs:enterpriseregistration.windows.net DomainJoined : YES DomainName : CONTOSO
    
    +----------------------------------------------------------------------+
    | User State                                                           |  +----------------------------------------------------------------------+
    
                 NgcSet : YES
               NgcKeyId : {C7A9AEDC-780E-4FDA-B200-1AE15561A46B}
        WorkplaceJoined : NO
          WamDefaultSet : YES
    WamDefaultAuthority : organizations         WamDefaultId : https://login.microsoft.com       WamDefaultGUID : {B16898C6-A148-4967-9171-64D755DA8520} (AzureAd)           AzureAdPrt : YES



## <a name="step-2-evaluate-the-registration-status"></a>Schritt 2: Bewerten des Registrierungsstatus 

Überprüfen Sie die folgenden Felder, und stellen Sie sicher, dass sie die erwarteten Werte aufweisen:

### <a name="azureadjoined--yes"></a>AzureAdJoined : YES  

Dieses Feld zeigt, ob das Gerät bei Azure AD registriert ist. Wenn für den Wert „NO“ angezeigt wird, wurde die Registrierung nicht abgeschlossen. 

**Mögliche Ursachen:**

1.    Fehler bei der Authentifizierung des Computers für die Registrierung.

2.    In der Organisation gibt es einen HTTP-Proxy, der von dem Computer nicht gefunden werden kann.

3.    Der Computer kann Azure AD für die Authentifizierung oder Azure DRS für die Registrierung nicht erreichen.

4.    Der Computer befindet sich möglicherweise nicht im internen Unternehmensnetzwerk oder im VPN mit direktem Zugriff auf einen lokalen AD-Domänencontroller.

5.    Wenn der Computer über ein TPM verfügt, befindet sich dieses möglicherweise in einem fehlerhaften Zustand.

6.    Möglicherweise liegt eine Fehlkonfiguration in Diensten vor, die früher im Dokument aufgeführt wurden und die Sie erneut überprüfen müssen. Typische Beispiele:

    1.    Auf dem Verbundserver sind keine WS-Trust-Endpunkte aktiviert.

    2.    Ihr Verbundserver lässt möglicherweise keine eingehende Authentifizierung von Computern in Ihrem Netzwerk über die integrierten Windows-Authentifizierung zu.

    3.    Es gibt kein Dienstverbindungspunkt-Objekt, das auf Ihren verifizierten Domänennamen in Azure AD in der Active Directory-Gesamtstruktur verweist, zu der der Computer gehört.

---

### <a name="domainjoined--yes"></a>DomainJoined : YES  

Dieses Feld zeigt, ob das Gerät in ein lokales Active Directory eingebunden ist. Wenn für den Wert „NO“ angezeigt wird, kann das Gerät sich nicht automatisch bei Azure AD registrieren. Überprüfen Sie zunächst, ob das Gerät in das lokale Active Directory eingebunden wird, damit es sich bei Azure AD registrieren kann. Wenn Sie den Computer direkt in Azure AD einbinden möchten, wechseln Sie zu „Learn about capabilities of Azure Active Directory Join“ (Informationen zu den Funktionen des Azure Active Directory-Beitritts).

---

### <a name="workplacejoined--no"></a>WorkplaceJoined : NO  

Dieses Feld zeigt, ob das Gerät bei Azure AD als privates Gerät registriert ist (markiert als „mit dem Arbeitsplatz verknüpft“). Für diesen Wert sollte für einen in die Domäne eingebundenen, bei Azure AD registrierten Computer „NO“ angezeigt werden. Wird stattdessen „YES“ angezeigt, bedeutet das, dass vor dem Abschluss der Registrierung des Computers ein Geschäfts-, Schul- oder Unikonto hinzugefügt wurde. In diesem Fall wird das Konto bei Verwendung der Anniversary Update-Version von Windows 10 ignoriert (1607 beim Ausführen des WinVer-Befehls im Fenster „Ausführen“ oder in einem Eingabeaufforderungsfenster).

---

### <a name="wamdefaultset--yes-and-azureadprt--yes"></a>WamDefaultSet : YES and AzureADPrt : YES
  
Diese Felder zeigen, dass der Benutzer sich bei der Anmeldung beim Gerät erfolgreich bei Azure AD authentifiziert hat. Wenn sie den Wert „NO“ aufweisen, gibt es dafür möglicherweise folgende Ursachen:

1.    Dem Gerät wurde bei der Registrierung ein ungültiger Speicherschlüssel (STK) im TPM zugewiesen (prüfen Sie den KeySignTest bei der Ausführung mit erhöhten Rechten).

2.    Alternative Anmelde-ID.

3.    HTTP-Proxy nicht gefunden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Häufig gestellte Fragen zur automatischen Geräteregistrierung](active-directory-conditional-access-automatic-device-registration-faq.md). 
