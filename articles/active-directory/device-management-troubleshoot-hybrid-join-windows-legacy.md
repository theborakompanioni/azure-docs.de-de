---
title: "Beheben von Problemen mit Geräten mit Hybrideinbindung in Azure Active Directory | Microsoft-Dokumentation"
description: "Beheben von Problemen mit Geräten mit Hybrideinbindung in Azure Active Directory."
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
ms.openlocfilehash: 715fca79e488ae3759926181c244a42026f4a554
ms.contentlocale: de-de
ms.lasthandoff: 08/19/2017

---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-down-level-devices"></a>Beheben von Problemen mit Geräten mit Hybrideinbindung in Azure Active Directory 

Dieses Thema gilt nur für die folgenden Geräte: 

- Windows 7 
- Windows 8.1 
- Windows Server 2008 R2 
- Windows Server 2012 
- Windows Server 2012 R2 
 

Weitere Informationen zu Windows 10 und Windows Server 2016 finden Sie unter [Beheben von Problemen mit Geräten unter Windows 10 und Windows Server 2016 mit Hybrideinbindung in Azure Active Directory](device-management-troubleshoot-hybrid-join-windows-current.md).

In diesem Thema wird vorausgesetzt, dass Sie [Geräte mit Hybrideinbindung in Azure Active Directory konfiguriert haben](device-management-hybrid-azuread-joined-devices-setup.md), um die folgenden Szenarien zu unterstützen:

- Gerätebasierter bedingter Zugriff

- [Unternehmensroaming von Einstellungen](active-directory-windows-enterprise-state-roaming-overview.md)

- [Windows Hello for Business](active-directory-azureadjoin-passport-deployment.md) 





Dieses Thema enthält Anleitungen zur Problembehandlung zum Beheben potenzieller Probleme.  

**Wichtige Informationen:** 

- Die maximale Anzahl von Geräten pro Benutzer ist gerätespezifisch. Beispiel: Wenn *jdoe* und *jharnett* sich auf einem Gerät anmelden, wird für jeden dieser Benutzer eine separate Registrierung (DeviceID) auf der Registerkarte **USER** erstellt.  

- Bei der anfänglichen Gerätekonfiguration für die Registrierung bzw. den Beitritt von Geräten wird zunächst eine Anmeldung oder Sperren/Entsperren versucht. Es kann eine Verzögerung von bis zu 5 Minuten auftreten, die durch eine Aufgabe der Aufgabenplanung ausgelöst wird. 

- Durch eine Neuinstallation des Betriebssystems oder durch manuelles Aufheben der Registrierung und erneutes Registrieren wird in Azure AD möglicherweise eine neue Registrierung erstellt. Dies kann im Azure-Portal zu mehreren Einträgen auf der Registerkarte mit den Benutzerinformationen führen. 


## <a name="step-1-retrieve-the-registration-status"></a>Schritt 1: Abrufen des Registrierungsstatus 

**So überprüfen Sie den Registrierungsstatus:**  

1. Öffnen Sie die Eingabeaufforderung als Administrator. 

2. Geben Sie `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"` ein

Dieser Befehl zeigt ein Dialogfeld an, das Ihnen ausführliche Informationen zum Einbindungsstatus bietet.

![Workplace Join für Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/01.png)


## <a name="step-2-evaluate-the-hybrid-azure-ad-join-status"></a>Schritt 2: Bewerten des Status des Azure AD-Hybridbeitritts 

Wenn die Hybrideinbindung in Azure AD nicht erfolgreich war, enthält das Dialogfeld Informationen zum aufgetretenen Problem.

**Folgende Probleme treten am häufigsten auf:**

- Ein falsch konfigurierter AD FS oder Azure AD

    ![Workplace Join für Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/02.png)

- Sie sind nicht als Domänenbenutzer angemeldet

    ![Workplace Join für Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/03.png)

- Ein Kontingent wurde erreicht

    ![Workplace Join für Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/04.png)

- Der Dienst antwortet nicht 

    ![Workplace Join für Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/05.png)

Die Statusinformationen finden Sie auch im Ereignisprotokoll unter **Anwendungs- und Dienstprotokoll\Microsoft-Workplace Join**.
  
**Die häufigsten Ursachen für Fehler bei der Azure AD-Hybrideinbindung sind:** 

- Der Computer befindet sich nicht im internen Netzwerk der Organisation oder im VPN und besitzt keine Verbindung mit einem lokalen AD-Domänencontroller.

- Sie sind über ein lokales Computerkonto bei ihrem Computer angemeldet. 

- Probleme bei der Dienstkonfiguration: 

  - Der Verbundserver wurde für die Unterstützung von **WIAORMULTIAUTHN** konfiguriert. 

  - Es gibt kein Dienstverbindungspunkt-Objekt, das auf Ihren verifizierten Domänennamen in Azure AD in der Active Directory-Gesamtstruktur verweist, zu der der Computer gehört.

  - Ein Benutzer hat den Grenzwert von Geräten erreicht. 

## <a name="next-steps"></a>Nächste Schritte

Bei Fragen lesen Sie unter [Häufig gestellte Fragen zur Geräteverwaltung](device-management-faq.md) nach.  

