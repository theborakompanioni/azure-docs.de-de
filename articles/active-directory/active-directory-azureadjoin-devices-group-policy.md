---
title: "Verbinden von in die Domäne eingebundenen Geräten mit Azure AD für Windows 10-Funktionen | Microsoft Docs"
description: "Erklärt, wie Gruppenrichtlinien konfiguriert werden müssen, damit Geräte durch Domänenbeitritt ins Unternehmensnetzwerk eingebunden werden können."
services: active-directory
documentationcenter: 
author: femila
manager: swadhwa
editor: 
tags: azure-classic-portal
ms.assetid: 2ff29f3e-5325-4f43-9baa-6ae8d6bad3e3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/21/2016
ms.author: femila
translationtype: Human Translation
ms.sourcegitcommit: 7d141adf04cfb99e57c63ba62de4a7dad9ab8326
ms.openlocfilehash: 290645b920bc4a83c610e80266854450b6e1509a


---
# <a name="connect-domain-joined-devices-to-azure-ad-for-windows-10-experiences"></a>Verbinden von in die Domäne eingebundenen Geräten mit Azure AD für Windows 10-Funktionen
Der Domänenbeitritt ist das herkömmliche Verfahren, mit dem Unternehmen in den letzten 15 oder mehr Jahren eine Verbindung für geschäftliche Geräte hergestellt haben. Benutzer konnten sich mit ihren Geschäfts- oder Schulkonten in Windows Server Active Directory (Active Directory) bei ihren Geräten anmelden, und die IT-Mitarbeiter konnten diese Geräte vollständig verwalten. Organisationen nutzen normalerweise Verfahren für die Imageerstellung, um Geräte für Benutzer bereitzustellen, und sie verwalten die Geräte im Allgemeinen mit System Center Configuration Manager (SCCM) oder mit Gruppenrichtlinien.

Ein Domänenbeitritt unter Windows 10 bietet nach der Verbindung von Geräten mit Azure Active Directory (Azure AD) folgende Vorteile:

* Einmaliges Anmelden (SSO, Single Sign-On) bei Azure AD-Ressourcen von jedem Ort aus
* Zugriff auf Windows Store des Unternehmens per Geschäfts- oder Schulkonto (kein Microsoft-Konto erforderlich)
* Für Unternehmen geeignetes geräteübergreifendes Roaming von Benutzereinstellungen per Geschäfts- oder Schulkonto (kein Microsoft-Konto erforderlich)
* Strenge Authentifizierung und bequeme Anmeldung beim Geschäfts- oder Schulkonto mit Microsoft Passport und Windows Hello
* Möglichkeit, den Zugriff auf Geräte zu beschränken, die den Gruppenrichtlinien der Organisation für Geräte entsprechen

## <a name="prerequisites"></a>Voraussetzungen
Beitritte zu einer Domäne sind weiterhin nützlich. Damit Sie jedoch die Vorteile von Azure AD nutzen können, z. B. für einmaliges Anmelden, Roaming von Einstellungen bei Geschäfts- oder Schulkonten, Zugriff auf den Windows Store mit Geschäfts- oder Schulkonten, benötigen Sie Folgendes:

* Azure AD-Abonnement
* Azure AD Connect, um das lokale Verzeichnis auf Azure AD zu erweitern
* Eine Richtlinie für Geräte, die einer Domäne beigetreten sind, zum Herstellen einer Verbindung mit Azure AD
* Windows 10 (Build 10551 oder neuer) für die Geräte

Zum Aktivieren von Microsoft Passport for Work und Windows Hello benötigen Sie zusätzlich Folgendes:

* Eine Public Key-Infrastruktur (PKI) zum Ausstellen von Benutzerzertifikaten.
* System Center Configuration Manager, Version 1509 für Technical Preview Weitere Informationen erhalten Sie unter [Microsoft System Center Configuration Manager Technical Preview](https://technet.microsoft.com/library/dn965439.aspx#BKMK_TP3Update) (Microsoft System Center Configuration Manager: Technische Vorschau) und [System Center Configuration Manager Team Blog](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx) (System Center Configuration Manager: Team-Blog). Dies ist erforderlich, um Benutzerzertifikate auf der Basis von Microsoft Passport-Schlüsseln bereitzustellen.

Alternativ zur PKI-Bereitstellung haben Sie folgende Möglichkeit:

* Verwenden einiger Domänencontroller mit Windows Server 2016 Active Directory Domain Services

Um einen bedingten Zugriff zu ermöglichen, können Sie Gruppenrichtlinieneinstellungen erstellen, die den Zugriff auf in die Domäne eingebundene Geräte ohne weitere Bereitstellungen zulassen. Um die Zugriffssteuerung anhand der Gerätecompliance zu verwalten, benötigen Sie Folgendes:

* System Center Configuration Manager, Version 1509 für Technical Preview, für Passport-Szenarien

## <a name="deployment-instructions"></a>Anweisungen zur Bereitstellung

Befolgen Sie für die Bereitstellung die Schritte in [Konfigurieren der automatischen Registrierung von in die Domäne eingebundenen Windows-Geräten mit Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).

## <a name="additional-information"></a>Zusätzliche Informationen
* [Windows 10 für Unternehmen: Möglichkeiten der geschäftlichen Nutzung von Geräten](active-directory-azureadjoin-windows10-devices-overview.md)
* [Erweitern von Cloudfunktionen auf Windows 10-Geräte über Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Weitere Informationen zu Verwendungsszenarios für Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Benutzererfahrungen beim Verknüpfen von in die Domäne eingebundenen Windows 10-Geräten mit Azure AD](active-directory-azureadjoin-devices-group-policy.md)
* [Einrichten von Azure AD Join](active-directory-azureadjoin-setup.md)




<!--HONumber=Dec16_HO4-->


