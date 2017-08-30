---
title: "Einführung in die Geräteverwaltung in Azure Active Directory | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie mithilfe der Geräteverwaltung Kontrolle über die Geräte erhalten, die auf Ressourcen in Ihrer Umgebung zugreifen."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: markvi
ms.reviewer: jairoc
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: c8b8408f76534a808fd60e331282f8191905df58
ms.contentlocale: de-de
ms.lasthandoff: 08/23/2017

---
# <a name="introduction-to-device-management-in-azure-active-directory"></a>Einführung in die Geräteverwaltung in Azure Active Directory

In einer Welt, in der Mobilität und die Cloud an erster Stelle stehen, ermöglicht Azure Active Directory (Azure AD) das einmalige Anmelden an Geräten, Apps und Diensten an jedem Ort. Aufgrund der steigenden Zahl von Geräten, etwa in Form von „Bring Your Own Device“ (BYOD), gelten für IT-Spezialisten zwei gegensätzliche Zielsetzungen:

- Schaffen von Bedingungen für Endbenutzer, unter denen sie an jedem Ort und zu jeder Zeit produktiv sein können
- Lückenloses Schützen der Assets eines Unternehmens

Über Geräte erhalten Ihre Benutzer Zugriff auf Ihre Unternehmensressourcen. Zum Schutz der Unternehmensressourcen möchten Sie als IT-Administrator Kontrolle über diese Geräte haben. So können Sie sicherstellen, dass Ihre Benutzer auf Ihre Ressourcen über Geräte zugreifen, die Ihren Standards für Sicherheit und Konformität entsprechen. 

In diesem Thema wird erläutert, wie Sie dieses Ziel mithilfe der Geräteverwaltung in Azure AD erreichen können.

## <a name="getting-devices-under-the-control-of-azure-ad"></a>Steuern der Geräte über Azure AD

Um ein Gerät über Azure AD zu steuern, haben Sie zwei Möglichkeiten:

- Registrieren 
- Einbinden

Durch **Registrieren** eines Geräts bei Azure AD können Sie die Identität des Geräts verwalten. Wenn ein Gerät registriert wird, stellt Azure AD Device Registration eine Identität für das Gerät bereit, die bei der Anmeldung eines Benutzers bei Azure AD zum Authentifizieren des Geräts dient. Sie können die Identität zum Aktivieren oder Deaktivieren eines Geräts verwenden.

In Kombination mit einer Lösung für die Verwaltung mobiler Geräte, wie z.B. Microsoft Intune, werden die Geräteattribute in Azure AD mit zusätzlichen Informationen über das Gerät aktualisiert. So können Sie Regeln für den bedingten Zugriff erstellen, die erzwingen, dass der Zugriff von Geräten Ihren Standards für Sicherheit und Kompatibilität entspricht. Weitere Informationen zum Registrieren von Geräten in Microsoft Intune finden Sie unter „Registrieren von Geräten für die Verwaltung in Intune“.

Das **Einbinden** eines Geräts stellt eine Erweiterung der Registrierung eines Geräts dar. Das bedeutet, Sie profitieren von allen Vorteilen der Registrierung eines Geräts, und darüber hinaus wird der lokale Status des Geräts geändert. Durch Änderung des lokalen Status können sich Ihre Benutzer an einem Gerät anstatt mit einem persönlichen Konto mit einem Geschäfts-, Schul- oder Unikonto anmelden.

## <a name="azure-ad-registered-devices"></a>Bei Azure AD registrierte Geräte   

Ziel von bei Azure AD registrierten Geräten ist die Bereitstellung der Unterstützung für das **Bring Your Own Device (BYOD)**-Szenario. In diesem Szenario kann ein Benutzer auf die über Azure Active Directory gesteuerten Ressourcen Ihres Unternehmens über ein persönliches Gerät zugreifen.  

![Bei Azure AD registrierte Geräte](./media/device-management-introduction/03.png)

Der Zugriff basiert auf einem Geschäfts-, Schul- oder Unikonto, das auf dem Gerät eingegeben wurde.  
Unter Windows 10 können Benutzer beispielsweise ein Geschäfts-, Schul- oder Unikonto auf einem Personalcomputer, Tablet oder Telefon hinzufügen.  
Wenn ein Benutzer ein Geschäfts-, Schul- oder Unikonto hinzugefügt hat, ist das Gerät in Azure AD und optional in dem von Ihrem Unternehmen konfigurierten System für die Verwaltung mobiler Geräte registriert. Benutzer Ihres Unternehmens können einem persönlichen Gerät einfach ein Geschäfts-, Schul- oder Unikonto hinzufügen:

- Beim erstmaligen Zugriff auf eine Unternehmensanwendung
- Manuell über das Menü **Einstellungen** unter Windows 10 

Sie können bei Azure AD registrierte Geräte für Windows 10, iOS, Android und macOS konfigurieren.

## <a name="azure-ad-joined-devices"></a>In Azure AD eingebundene Geräte

Ziel von in Azure AD eingebundenen Geräten ist die Vereinfachung folgender Elemente:

- Windows-Bereitstellungen unternehmenseigener Geräte 
- Zugriff auf Unternehmens-Apps und Unternehmensressourcen über jegliche Windows-Geräte

![Bei Azure AD registrierte Geräte](./media/device-management-introduction/02.png)


Diese Ziele werden erreicht, indem Sie den Benutzern Self-Service-Funktionen zum Steuern unternehmenseigener Geräte über Azure AD bereitstellen.  
**Azure AD Join** ist für Unternehmen bestimmt, die neu auf die Cloud umstellen oder ganz auf die Cloud setzen. Dabei handelt es sich meist um kleine bis mittelgroße Unternehmen, die keine lokale Windows Server Active Directory-Infrastruktur haben. 

Die Implementierung von in Azure AD eingebundenen Geräten bietet die folgenden Vorteile:

- **Einmaliges Anmelden (SSO)** bei Ihren in Azure verwalteten SaaS-Apps und -Diensten. Für Benutzer werden beim Zugriff auf Arbeitsressourcen keine zusätzlichen Authentifizierungsaufforderungen angezeigt. Die SSO-Funktionalität ist auch dann verfügbar, wenn sie nicht mit dem Domänennetzwerk verbunden sind.

- **Für Unternehmen geeignetes Roaming** von Benutzereinstellungen auf allen eingebundenen Geräten. Benutzer müssen keine Verbindung mit einem Microsoft-Konto (z.B. Hotmail) herstellen, um geräteübergreifende Einstellungen zu sehen.

- **Zugriff auf Windows Store für Unternehmen** über das AD-Konto. Ihre Benutzer können aus einem Bestand von durch das Unternehmen vorausgewählten Anwendungen auswählen.

- Unterstützung von **Windows Hello** für einen sicheren und komfortablen Zugriff auf Arbeitsressourcen.

- **Einschränkung des Zugriffs** auf Apps nur über die Geräte, die die Konformitätsrichtlinie erfüllen.

Azure AD Join ist zwar hauptsächlich für Unternehmen vorgesehen, die über keine lokale Windows Server Active Directory-Infrastruktur verfügen, kann aber auch in folgenden Szenarien verwendet werden:

- Sie können keinen lokalen Domänenbeitritt verwenden, z.B. wenn Sie mobile Geräte wie Tablets und Telefone steuern möchten.

- Benutzer müssen hauptsächlich auf Office 365 oder andere in Azure AD integrierte SaaS-Apps zugreifen.

- Sie möchten eine Gruppe von Benutzern in Azure AD anstatt in Active Directory verwalten. Dies können z.B. Saisonkräfte, Auftragnehmer oder Studenten sein.

- Sie möchten Beitrittsfunktionen für Mitarbeiter in entfernten Niederlassungen mit eingeschränkter lokaler Infrastruktur bereitstellen.

Sie können in Azure AD eingebundene Geräte für Windows 10-Geräte konfigurieren.


## <a name="hybrid-azure-ad-joined-devices"></a>In Azure AD eingebundene Hybridgeräte

Seit über zehn Jahren verwenden viele Unternehmen den Domänenbeitritt für das lokale Active Directory, um Folgendes zu ermöglichen:

- Verwaltung unternehmenseigener Geräte über einen zentralen Ort für IT-Abteilungen

- Anmeldung von Benutzern bei Geräten mit ihrem Geschäfts-, Schul- oder Unikonto in Active Directory 

Unternehmen mit einem lokalen Fußabdruck nutzen normalerweise Verfahren für die Imageerstellung, um Geräte bereitzustellen, und häufig **System Center Configuration Manager (SCCM)** oder **Gruppenrichtlinien** für die Verwaltung der Geräte.

Wenn Ihre Umgebung über einen lokalen AD-Fußabdruck verfügt und Sie zudem die Funktionen von Azure Active Directory nutzen möchten, können Sie in Azure AD eingebundene Hybridgeräte implementieren. Hierbei handelt es sich um Geräte, die sowohl in Ihr lokales Active Directory als auch in Ihr Azure Active Directory eingebunden sind.

![Bei Azure AD registrierte Geräte](./media/device-management-introduction/01.png)


Verwenden Sie in Azure AD eingebundene Hybridgeräte in folgenden Fällen:

- Sie haben auf den Geräten Win32-Apps bereitgestellt, für die NTLM/Kerberos verwendet wird.

- Sie benötigen die Gruppenrichtlinie oder SCCM/DCM, um Geräte zu verwalten.

- Sie möchten weiterhin Lösungen für die Imageerstellung verwenden, um Geräte für Ihre Mitarbeiter zu konfigurieren.

Sie können in Azure AD eingebundene Hybridgeräte für Windows 10-Geräte und kompatible Geräte, z.B. mit Windows 8 und Windows 7, konfigurieren.

## <a name="summary"></a>Zusammenfassung

Mit der Geräteverwaltung in Azure AD haben Sie folgende Möglichkeiten: 

- Vereinfachen des Prozesses zum Steuern von Geräten über Azure AD

- Benutzerfreundlicher Zugriff auf die cloudbasierten Ressourcen Ihres Unternehmens für Ihre Benutzer

Als Faustregel sollten Sie Folgendes verwenden:

- Bei Azure AD registrierte Geräte für persönliche Geräte

- In Azure AD eingebundene Geräte für Geräte, die nicht in ein lokales AD eingebunden sind 

- In Azure AD eingebundene Hybridgeräte für Geräte, die in ein lokales AD eingebunden sind     




## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum gerätebasierten bedingten Zugriff finden Sie unter [Konfigurieren des gerätebasierten bedingten Zugriffs für Azure Active Directory](active-directory-conditional-access-policy-connected-applications.md).

- Informationen zum Einrichten von in Azure AD eingebundenen Hybridgeräten finden Sie unter [Konfigurieren von in Azure Active Directory eingebundenen Hybridgeräten](device-management-hybrid-azuread-joined-devices-setup.md).



