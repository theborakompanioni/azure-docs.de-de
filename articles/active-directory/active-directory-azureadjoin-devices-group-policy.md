---
title: "Verbinden von in die Domäne eingebundenen Geräten mit Azure AD für Windows 10-Funktionen | Microsoft Docs"
description: "Erklärt, wie Gruppenrichtlinien konfiguriert werden müssen, damit Geräte durch Domänenbeitritt ins Unternehmensnetzwerk eingebunden werden können."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 2ff29f3e-5325-4f43-9baa-6ae8d6bad3e3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: markvi
ms.reviewer: jairoc
ms.translationtype: Human Translation
ms.sourcegitcommit: 24d86e17a063164c31c312685c0742ec4a5c2f1b
ms.openlocfilehash: 39bd4fcbe1b9a197bcf5b8bb33bf6fffae2063fc
ms.contentlocale: de-de
ms.lasthandoff: 03/11/2017

---
# <a name="connect-domain-joined-devices-to-azure-ad-for-windows-10-experiences"></a>Verbinden von in die Domäne eingebundenen Geräten mit Azure AD für Windows 10-Funktionen
Der Domänenbeitritt ist das herkömmliche Verfahren, mit dem Unternehmen in den letzten 15 oder mehr Jahren eine Verbindung für geschäftliche Geräte hergestellt haben. Benutzer konnten sich mit ihren Geschäfts- oder Schulkonten in Windows Server Active Directory (Active Directory) bei ihren Geräten anmelden, und die IT-Mitarbeiter konnten diese Geräte vollständig verwalten. Organisationen nutzen normalerweise Verfahren für die Imageerstellung, um Geräte für Benutzer bereitzustellen, und sie verwalten die Geräte im Allgemeinen mit System Center Configuration Manager (SCCM) oder mit Gruppenrichtlinien.


Ein Domänenbeitritt unter Windows 10 bietet nach der Verbindung von Geräten mit Azure Active Directory (Azure AD) folgende Vorteile:

* Einmaliges Anmelden (SSO, Single Sign-On) bei Azure AD-Ressourcen von jedem Ort aus
* Zugriff auf Windows Store des Unternehmens per Geschäfts- oder Schulkonto (kein Microsoft-Konto erforderlich)
* Für Unternehmen geeignetes geräteübergreifendes Roaming von Benutzereinstellungen per Geschäfts- oder Schulkonto (kein Microsoft-Konto erforderlich)
* Strenge Authentifizierung und bequeme Anmeldung beim Geschäfts-, Schul- oder Unikonto mit Windows Hello for Business und Windows Hello
* Möglichkeit, den Zugriff auf Geräte zu beschränken, die den Gruppenrichtlinien der Organisation für Geräte entsprechen

## <a name="prerequisites"></a>Voraussetzungen
Beitritte zu einer Domäne sind weiterhin nützlich. Damit Sie jedoch die Vorteile von Azure AD nutzen können, z. B. für einmaliges Anmelden, Roaming von Einstellungen bei Geschäfts- oder Schulkonten, Zugriff auf den Windows Store mit Geschäfts- oder Schulkonten, benötigen Sie Folgendes:

* Azure AD-Abonnement
* Azure AD Connect, um das lokale Verzeichnis auf Azure AD zu erweitern
* Eine Richtlinie für Geräte, die einer Domäne beigetreten sind, zum Herstellen einer Verbindung mit Azure AD
* Windows 10 (Build 10551 oder neuer) für die Geräte

Zum Aktivieren von Windows Hello for Business und Windows Hello benötigen Sie zusätzlich Folgendes:

- Eine **Public Key-Infrastruktur (PKI)** zum Ausstellen von Benutzerzertifikaten

- **System Center Configuration Manager Current Branch** – Sie müssen die Version 1606 oder eine höhere Version installieren.  
Weitere Informationen finden Sie unter: 
    - [Dokumentation für System Center Configuration Manager](https://technet.microsoft.com/library/mt346023.aspx)
    - [System Center Configuration Manager-Teamblog](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx)
    - [Windows Hello for Business-Einstellungen in System Center Configuration Manager](https://docs.microsoft.com/sccm/protect/deploy-use/windows-hello-for-business-settings)

Alternativ zur PKI-Bereitstellung haben Sie folgende Möglichkeit:

* Verwenden einiger Domänencontroller mit Windows Server 2016 Active Directory Domain Services

Um einen bedingten Zugriff zu ermöglichen, können Sie Gruppenrichtlinieneinstellungen erstellen, die den Zugriff auf in die Domäne eingebundene Geräte ohne weitere Bereitstellungen zulassen. Um die Zugriffssteuerung anhand der Gerätecompliance zu verwalten, benötigen Sie Folgendes:

* System Center Configuration Manager Current Branch (1606 oder höher) für Windows Hello for Business-Szenarien

## <a name="deployment-instructions"></a>Anweisungen zur Bereitstellung

Befolgen Sie für die Bereitstellung die Schritte in [Konfigurieren der automatischen Registrierung von in die Domäne eingebundenen Windows-Geräten mit Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).

## <a name="next-step"></a>Nächster Schritt
* [Windows 10 für Unternehmen: Möglichkeiten der geschäftlichen Nutzung von Geräten](active-directory-azureadjoin-windows10-devices-overview.md)
* [Erweitern von Cloudfunktionen auf Windows 10-Geräte über Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Weitere Informationen zu Verwendungsszenarios für Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Benutzererfahrungen beim Verknüpfen von in die Domäne eingebundenen Windows 10-Geräten mit Azure AD](active-directory-azureadjoin-devices-group-policy.md)
* [Einrichten von Azure AD Join](active-directory-azureadjoin-setup.md)


