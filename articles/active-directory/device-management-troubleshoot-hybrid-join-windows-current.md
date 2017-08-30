---
title: "Beheben von Problemen mit Geräten unter Windows 10 und Windows Server 2016 mit Hybrideinbindung in Azure Active Directory | Microsoft-Dokumentation"
description: "Beheben von Problemen mit Geräten unter Windows 10 und Windows Server 2016 mit Hybrideinbindung in Azure Active Directory"
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
ms.date: 08/17/2017
ms.author: markvi
ms.reviewer: jairoc
ms.translationtype: HT
ms.sourcegitcommit: 847eb792064bd0ee7d50163f35cd2e0368324203
ms.openlocfilehash: 51962c14a3c32bbfa9a613fa203cc48cfea50c0b
ms.contentlocale: de-de
ms.lasthandoff: 08/19/2017

---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-windows-10-and-windows-server-2016-devices"></a>Beheben von Problemen mit Geräten unter Windows 10 und Windows Server 2016 mit Hybrideinbindung in Azure Active Directory 

Dieses Thema gilt für die folgenden Clients:

-   Windows 10
-   Windows Server 2016

Weitere Informationen zu anderen Windows-Clients finden Sie unter [Beheben von Problemen mit Geräten mit Hybrideinbindung in Azure Active Directory](device-management-troubleshoot-hybrid-join-windows-legacy.md).

In diesem Thema wird vorausgesetzt, dass Sie [Geräte mit Hybrideinbindung in Azure Active Directory konfiguriert haben](device-management-hybrid-azuread-joined-devices-setup.md), um die folgenden Szenarien zu unterstützen:

- Gerätebasierter bedingter Zugriff

- [Unternehmensroaming von Einstellungen](active-directory-windows-enterprise-state-roaming-overview.md)

- [Windows Hello for Business](active-directory-azureadjoin-passport-deployment.md)


Dieses Dokument enthält Anleitungen zur Problembehandlung zum Beheben potenzieller Probleme. 


Für Windows 10 und Windows Server 2016 unterstützt ein Azure Active Directory-Hybridbeitritt das Windows 10-Update vom November 2015 und höher. Es wird empfohlen, das Anniversary Update zu verwenden.

## <a name="step-1-retrieve-the-join-status"></a>Schritt 1: Abrufen des Beitrittsstatus 

**So rufen Sie den Beitrittsstatus ab**

1. Öffnen Sie die Eingabeaufforderung als Administrator.

2. Geben Sie **dsregcmd /status** ein.



    +----------------------------------------------------------------------+
    | Device State                                                         |  +----------------------------------------------------------------------+
    
        AzureAdJoined: YES
     EnterpriseJoined: NO DeviceId: 5820fbe9-60c8-43b0-bb11-44aee233e4e7 Thumbprint: B753A6679CE720451921302CA873794D94C6204A KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider: Microsoft Platform Crypto Provider TpmProtected: YES KeySignTest: : MUST Run elevated to test.
                  Idp: login.windows.net TenantId: 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName: Contoso AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/token MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl: https://portal.manage-beta.microsoft.com/?portalAction=Compliance SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ== JoinSrvVersion: 1.0 JoinSrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId: urn:ms-drs:enterpriseregistration.windows.net KeySrvVersion: 1.0 KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId: urn:ms-drs:enterpriseregistration.windows.net DomainJoined: YES DomainName: CONTOSO
    
    +----------------------------------------------------------------------+
    | User State                                                           |  +----------------------------------------------------------------------+
    
                 NgcSet: YES
               NgcKeyId: {C7A9AEDC-780E-4FDA-B200-1AE15561A46B}
        WorkplaceJoined: NO
          WamDefaultSet: YES
    WamDefaultAuthority: organizations         WamDefaultId: https://login.microsoft.com       WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} (AzureAd)           AzureAdPrt: YES



## <a name="step-2-evaluate-the-join-status"></a>Schritt 2: Bewerten des Beitrittsstatus 

Überprüfen Sie die folgenden Felder, und stellen Sie sicher, dass sie die erwarteten Werte aufweisen:

### <a name="azureadjoined--yes"></a>AzureAdJoined : YES  

Dieses Feld gibt an, ob das Gerät in Azure AD eingebunden ist. Wenn der Wert **NO** lautet, wurde der Azure AD-Beitritt noch nicht abgeschlossen. 

**Mögliche Ursachen:**

- Fehler bei der Authentifizierung des Computers für den Beitritt.

- In der Organisation gibt es einen HTTP-Proxy, der von dem Computer nicht gefunden werden kann.

- Der Computer kann Azure AD für die Authentifizierung oder Azure DRS für die Registrierung nicht erreichen.

- Der Computer befindet sich möglicherweise nicht im internen Unternehmensnetzwerk oder im VPN mit direktem Zugriff auf einen lokalen AD-Domänencontroller.

- Wenn der Computer über ein TPM verfügt, befindet sich dieses möglicherweise in einem fehlerhaften Zustand.

- Möglicherweise liegt eine Fehlkonfiguration in Diensten vor, die früher im Dokument aufgeführt wurden und die Sie erneut überprüfen müssen. Typische Beispiele:

    - Auf dem Verbundserver sind keine WS-Trust-Endpunkte aktiviert.

    - Ihr Verbundserver lässt möglicherweise keine eingehende Authentifizierung von Computern in Ihrem Netzwerk über die integrierte Windows-Authentifizierung zu.

    - Es gibt kein Dienstverbindungspunkt-Objekt, das auf Ihren verifizierten Domänennamen in Azure AD in der Active Directory-Gesamtstruktur verweist, zu der der Computer gehört.

---

### <a name="domainjoined--yes"></a>DomainJoined : YES  

Dieses Feld gibt an, ob das Gerät in ein lokales Active Directory eingebunden ist. Wenn der Wert **NO** lautet, kann das Gerät keinen Azure AD-Hybridbeitritt durchführen.  

---

### <a name="workplacejoined--no"></a>WorkplaceJoined : NO  

Dieses Feld gibt an, ob das Gerät bei Azure AD als privates Gerät registriert ist (markiert als *Workplace Join*). Dieser Wert sollte für in eine Domäne eingebundene Computer mit Hybrideinbindung in Azure AD **NO** lauten. Wenn der Wert **YES** lautet, wurde vor Abschluss der Hybrideinbindung in Azure AD ein Geschäfts-, Schul- oder Unikonto hinzugefügt. In diesem Fall wird das Konto ignoriert, wenn eine Version von Windows 10 (1607) mit Anniversary Update verwendet wird.

---

### <a name="wamdefaultset--yes-and-azureadprt--yes"></a>WamDefaultSet : YES and AzureADPrt : YES
  
Diese Felder geben an, ob der Benutzer sich bei der Anmeldung beim Gerät erfolgreich bei Azure AD authentifiziert hat. Wenn die Werte **NO** lauten, kann dies die folgenden Ursachen haben:

- Dem Gerät wurde bei der Registrierung ein ungültiger Speicherschlüssel (STK) im TPM zugewiesen (prüfen Sie den KeySignTest bei der Ausführung mit erhöhten Rechten).

- Alternative Anmelde-ID.

- HTTP-Proxy nicht gefunden.

## <a name="next-steps"></a>Nächste Schritte

Bei Fragen lesen Sie unter [Häufig gestellte Fragen zur Geräteverwaltung](device-management-faq.md) nach. 
