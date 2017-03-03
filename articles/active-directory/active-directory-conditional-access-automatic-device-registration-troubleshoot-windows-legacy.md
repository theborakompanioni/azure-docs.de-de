---
title: "Problembehandlung bei der automatischen Registrierung von Computern, die in die Azure AD-Domäne eingebunden sind, für kompatible Windows-Clients | Microsoft-Dokumentation"
description: "Problembehandlung bei der automatischen Registrierung von Computern, die in die Azure AD-Domäne eingebunden sind, für kompatible Windows-Clients."
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
ms.sourcegitcommit: d0d213e2cc52f43c11efd49c51270aa42f8fad36
ms.openlocfilehash: 6c373adaee553e9eca94a978afc62d453f7db0f8
ms.lasthandoff: 02/15/2017


---
# <a name="troubleshooting-auto-registration-of-domain-joined-computers-to-azure-ad-for-windows-down-level-clients"></a>Beheben von Problemen bei der automatischen Registrierung von Computern, die in die Azure AD-Domäne eingebunden sind, für kompatible Windows-Clients 

Dieses Thema gilt nur für die folgenden Clients: 

- Windows 7 
- Windows 8.1 
- Windows Server 2008 R2 
- Windows Server 2012 
- Windows Server 2012 R2 
 

Für Windows 10 oder Windows Server 2016 finden Sie weitere Informationen unter [Beheben von Problemen bei der automatischen Registrierung von Computern, die in die Azure AD-Domäne eingebunden sind, für Windows 10 und Windows Server 2016](active-directory-conditional-access-automatic-device-registration-troubleshoot-windows.md).

In diesem Thema wird davon ausgegangen, dass Sie die automatische Registrierung von in die Domäne eingebundenen Geräten gemäß der Beschreibung unter [Konfigurieren der automatischen Registrierung von in die Domäne eingebundenen Windows-Geräten mit Azure Active Directory](active-directory-conditional-access-automatic-device-registration-get-started.md) konfiguriert haben.
 
Dieses Thema enthält Anleitungen zur Problembehandlung zum Beheben potenzieller Probleme.  
Für ein erfolgreiches Ergebnis sind einige Aspekte zu beachten: 

- Die Registrierung dieser Clients in Azure AD erfolgt pro Benutzer/Gerät. Beispiel: Wenn jdoe und jharnett sich auf diesem Gerät anmelden, wird für jeden dieser Benutzer eine separate Registrierung (DeviceID) auf der Registerkarte mit den Benutzerinformationen erstellt.  

- Die Registrierung dieser Clients ist standardmäßig so konfiguriert, dass sie entweder bei der Anmeldung oder beim Sperren/Entsperren versucht wird. Es kann zu einer fünfminütigen Verzögerung kommen, da dies über einen Task im Taskplaner ausgelöst wird. 

- Durch eine Neuinstallation des Betriebssystems oder durch manuelles Aufheben der Registrierung und erneutes Registrieren wird in Azure AD möglicherweise eine neue Registrierung erstellt, was im Azure-Portal zu mehreren Einträgen auf der Registerkarte mit den Benutzerinformationen führen kann. 


## <a name="step-1-retrieve-the-registration-status"></a>Schritt 1: Abrufen des Registrierungsstatus 

**So überprüfen Sie den Registrierungsstatus:**  

1. Öffnen Sie die Eingabeaufforderung als Administrator. 

2. Geben Sie `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"` ein

Dieser Befehl zeigt ein Dialogfeld an, das Ihnen ausführliche Informationen zum Einbindungsstatus bietet.

![Workplace Join für Windows](./media/active-directory-conditional-access-automatic-device-registration-troubleshoot-windows-legacy/01.png)


## <a name="step-2-evaluate-the-registration-status"></a>Schritt 2: Bewerten des Registrierungsstatus 

Wenn die Einbindung nicht erfolgreich war, enthält das Dialogfeld Informationen über das aufgetretene Problem.

**Folgende Probleme treten am häufigsten auf:**

- Ein falsch konfigurierter AD FS oder Azure AD

    ![Workplace Join für Windows](./media/active-directory-conditional-access-automatic-device-registration-troubleshoot-windows-legacy/02.png)

- Sie sind nicht als Domänenbenutzer angemeldet

    ![Workplace Join für Windows](./media/active-directory-conditional-access-automatic-device-registration-troubleshoot-windows-legacy/03.png)

- Ein Kontingent wurde erreicht

    ![Workplace Join für Windows](./media/active-directory-conditional-access-automatic-device-registration-troubleshoot-windows-legacy/04.png)

- Der Dienst antwortet nicht 

    ![Workplace Join für Windows](./media/active-directory-conditional-access-automatic-device-registration-troubleshoot-windows-legacy/05.png)

Die Statusinformationen finden Sie auch im Ereignisprotokoll unter **Anwendungs- und Dienstprotokoll\Microsoft-Workplace Join**.
  
**Bei einer fehlerhaften Registrierung liegen am häufigsten folgende Ursachen vor:** 

- Der Computer befindet sich nicht im internen Netzwerk der Organisation oder im VPN und besitzt keine Verbindung mit einem lokalen AD-Domänencontroller.

- Sie sind über ein lokales Computerkonto bei ihrem Computer angemeldet. 

- Probleme bei der Dienstkonfiguration: 

  - Der Verbundserver wurde für die Unterstützung von **WIAORMULTIAUTHN** konfiguriert. 

  - Es gibt kein Dienstverbindungspunkt-Objekt, das auf Ihren verifizierten Domänennamen in Azure AD in der Active Directory-Gesamtstruktur verweist, zu der der Computer gehört.

  - Ein Benutzer hat den Grenzwert von Geräten erreicht. Weitere Informationen finden Sie unter „Erste Schritte bei der Azure Active Directory-Geräteregistrierung“.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Häufig gestellte Fragen zur automatischen Geräteregistrierung](active-directory-conditional-access-automatic-device-registration-faq.md). 

