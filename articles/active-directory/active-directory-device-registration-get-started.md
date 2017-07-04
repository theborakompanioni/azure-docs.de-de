---
title: "Konfigurieren der automatischen Registrierung von in die Domäne eingebundenen Windows-Geräten mit Azure Active Directory | Microsoft-Dokumentation"
description: "Einrichten Ihrer in Domänen eingebundenen Windows-Geräte für die automatische und im Hintergrund durchgeführte Registrierung bei Azure Active Directory."
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
ms.date: 06/23/2017
ms.author: markvi
ms.translationtype: Human Translation
ms.sourcegitcommit: 8a531f70f0d9e173d6ea9fb72b9c997f73c23244
ms.openlocfilehash: 9934902811354ffa4047d70d995a6dd44be0229b
ms.contentlocale: de-de
ms.lasthandoff: 03/10/2017


---
# <a name="get-started-with-azure-active-directory-device-registration"></a>Erste Schritte bei der Azure Active Directory-Geräteregistrierung

Die Azure Active Directory-Geräteregistrierung ist die Grundlage für gerätebasierte Szenarien für den bedingten Zugriff. Wenn ein Gerät registriert wird, stellt die Azure Active Directory-Geräteregistrierung eine Identität für das Gerät bereit, die bei der Benutzeranmeldung zum Authentifizieren des Geräts dient. Das authentifizierte Gerät und die Attribute des Geräts können anschließend verwendet werden, um bedingte Zugriffsrichtlinien für Anwendungen zu erzwingen, die in der Cloud und lokal gehostet werden.

In Kombination mit einer Lösung für die Verwaltung mobiler Geräte, wie z.B. Microsoft Intune, werden die Geräteattribute in Azure Active Directory mit zusätzlichen Informationen über das Gerät aktualisiert. So können Sie Regeln für den bedingten Zugriff erstellen, die erzwingen, dass der Zugriff von Geräten Ihren Standards für Sicherheit und Kompatibilität entspricht. Weitere Informationen zum Registrieren von Geräten in Microsoft Intune finden Sie unter „Registrieren von Geräten für die Verwaltung in Intune“.
Zu den Szenarien, die durch die Azure Active Directory-Geräteregistrierung ermöglicht werden, gehört die Unterstützung für iOS-, Android- und Windows-Geräte. Für die einzelnen Szenarien, in denen die Geräteregistrierung von Azure Active Directory eingesetzt wird, gelten ggf. spezielle Anforderungen und Einschränkungen hinsichtlich der Plattformunterstützung. 

Folgende Szenarien sind möglich:

- **Bedingter Zugriff für Office 365-Anwendungen mit Microsoft Intune:** IT-Administratoren können Geräterichtlinien für den bedingten Zugriff bereitstellen, um Unternehmensressourcen zu schützen und gleichzeitig Information-Workern auf kompatiblen Geräten den Zugriff auf die Dienste zu gestatten. Weitere Informationen finden Sie unter „Geräterichtlinien für den bedingten Zugriff auf Office 365-Dienste“.

- **Bedingter Zugriff auf lokal gehostete Anwendungen:** Sie können registrierte Geräte mit Zugriffsrichtlinien für Anwendungen verwenden, die für die Verwendung von AD FS mit Windows Server 2012 R2 konfiguriert sind. Weitere Informationen zum Einrichten des bedingten Zugriffs für lokale Systeme finden Sie unter [Einrichten des lokalen bedingten Zugriffs mithilfe der Azure Active Directory-Geräteregistrierung](active-directory-device-registration-on-premises-setup.md).

## <a name="setting-up-azure-active-directory-device-registration"></a>Einrichten der Azure Active Directory-Geräteregistrierung

Für die Einrichtung der Geräteregistrierung stehen Ihnen mehrere Optionen zur Verfügung:

- Geräte können beim Beitritt zur Azure AD-Domäne registriert werden. Erfahren Sie mehr über das Einbinden in Azure AD und über die Einstellungen, die Benutzer für den Beitritt zu einer Azure AD-Domäne benötigen.

- Geräte können registriert werden, wenn Benutzer auf einem persönlichen Gerät Geschäfts-, Schul- oder Unikonten in Windows hinzufügen oder wenn mobile Geräte eine Verbindung mit Arbeitsressourcen herstellen, für die eine Registrierung erforderlich ist. Um dies sicherzustellen, müssen Sie die Azure AD-Geräteregistrierung im Azure-Portal aktivieren. 

- Geräte können für herkömmliche in die Domäne eingebundene Computer mithilfe der automatischen Geräteregistrierung registriert werden. Hierzu müssen Sie zuerst Azure AD Connect einrichten, bevor Sie mit der automatischen Geräteregistrierung fortfahren.

Neueste Anleitungen finden Sie unter [Konfigurieren der automatischen Registrierung von in die Domäne eingebundenen Windows-Geräten mit Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).  
Sie können registrierte Geräte zudem mithilfe des Verwaltungsportals in Azure Active Directory anzeigen und aktivieren oder deaktivieren.

## <a name="enable-the-azure-active-directory-device-registration-service"></a>Aktivieren des Azure Active Directory-Geräteregistrierungsdiensts

**So aktivieren Sie den Azure Active Directory-Geräteregistrierungsdienst**

1.  Melden Sie sich als Administrator beim Microsoft Azure-Portal an.

2.  Wählen Sie im linken Bereich **Active Directory**aus.

3.  Wählen Sie auf der Registerkarte "Verzeichnis" Ihr Verzeichnis aus.

4.  Klicken Sie auf **Konfigurieren**.

5.  Scrollen Sie zu **Geräte**.

6.  Wählen Sie unter BENUTZER DÜRFEN IHRE GERÄTE FÜR AZURE AD REGISTRIEREN die Option ALLE.

7.  Wählen Sie die maximale Anzahl von Geräten aus, die Sie pro Benutzer autorisieren möchten.

Für die Registrierung bei Microsoft Intune oder bei der Verwaltung mobiler Geräte für Office 365 ist eine Geräteregistrierung erforderlich. Wenn Sie einen dieser Dienste konfiguriert haben, wird **ALLE** ausgewählt und die Schaltfläche **KEINE** deaktiviert. Stellen Sie sicher, dass sie ordnungsgemäß konfiguriert sind und über die entsprechenden Lizenzen verfügen.

Standardmäßig ist die zweistufige Authentifizierung für den Dienst nicht aktiviert. Die zweistufige Authentifizierung wird jedoch empfohlen, wenn ein Gerät registriert wird.

- Bevor Sie für diesen Dienst eine zweistufige Authentifizierung anfordern, müssen Sie in Azure Active Directory einen Anbieter für die zweistufige Authentifizierung konfigurieren und Ihre Benutzerkonten für Multi-Factor Authentication (MFA) konfigurieren. Weitere Informationen finden Sie unter "Hinzufügen von Multi-Factor Authentication zu Azure Active Directory".

- Wenn Sie die Active Directory-Verbunddienste (AD FS) mit Windows Server 2012 R2 verwenden, müssen Sie in AD FS ein Modul für die zweistufige Authentifizierung konfigurieren. Weitere Informationen finden Sie unter Verwenden von Multi-Factor Authentication mit den Active Directory-Verbunddiensten.

## <a name="view-and-manage-device-objects-in-azure-active-directory"></a>Anzeigen und Verwalten von Geräteobjekten in Azure Active Directory

Über das Azure-Administratorportal können Sie Geräte anzeigen, blockieren und Blockierungen aufheben. Ein blockiertes Gerät verliert den Zugriff auf Anwendungen, die so konfiguriert sind, dass nur registrierte Geräte für den Zugriff zulässig sind.

**So zeigen Sie Geräteobjekte in Azure Active Directory an und verwalten sie**
 
1.  Melden Sie sich als Administrator beim Microsoft Azure-Portal an.

2.  Wählen Sie im linken Bereich **Active Directory**aus.

3.  Wählen Sie Ihr Verzeichnis.

4.  Wählen Sie **Benutzer** aus. 

5.  Klicken Sie auf den Benutzer, dessen Geräte angezeigt werden sollen.

6.  Wählen Sie **Geräte**.

7.  Wählen Sie **Registrierte Geräte**.

Jetzt können Sie die registrierten Geräte des Benutzers anzeigen, blockieren oder deren Blockierung aufheben.
Lokale, in die Domäne eingebundene und automatisch registrierte Windows 10-Geräte werden auf der Registerkarte „Benutzer“ nicht angezeigt. Verwenden Sie den PowerShell-Befehl „Get-MsolDevice“, um all Ihre Unternehmensgeräte zu finden. 


## <a name="next-steps"></a>Nächste Schritte

Informationen zum Einrichten der automatischen Geräteregistrierung finden Sie unter [Konfigurieren der automatischen Registrierung von in die Domäne eingebundenen Windows-Geräten mit Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).



