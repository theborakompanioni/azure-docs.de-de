---
title: 'Azure AD Connect-Synchronisierung: Aktivieren des Active Directory-Papierkorbs | Microsoft-Dokumentation'
description: "In diesem Thema wird die Verwendung des AD-Papierkorbfeatures für Azure AD Connect empfohlen."
services: active-directory
keywords: "AD-Papierkorb, versehentliches Löschen, Quellanker"
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: afec4207-74f7-4cdd-b13a-574af5223a90
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 9778db69e94e9f1d033cc8c16fdb9554df3eddcc
ms.lasthandoff: 04/03/2017


---
# <a name="azure-ad-connect-sync-enable-ad-recycle-bin"></a>Azure AD Connect-Synchronisierung: Aktivieren des AD-Papierkorbs
Es wird empfohlen, den Active Directory-Papierkorb für Ihre lokalen Active Directory-Instanzen, die mit Azure AD synchronisiert werden, zu aktivieren. 

Wenn Sie versehentlich ein lokales AD-Benutzerobjekt löschen und mit dem Feature wiederherstellen, stellt Azure AD das entsprechende Azure AD-Benutzerobjekt wieder her.  Informationen zum AD-Papierkorbfeature finden Sie im Artikel [Scenario Overview for Restoring Deleted Active Directory Objects](https://technet.microsoft.com/library/dd379542.aspx) (Szenarioübersicht über das Wiederherstellen gelöschter Active Directory-Objekte).

## <a name="benefits-of-enabling-the-ad-recycle-bin"></a>Vorteile der Aktivierung des AD-Papierkorbs
Dieses Feature unterstützt Sie folgendermaßen beim Wiederherstellen von Azure AD-Benutzerobjekten:

* Wenn Sie versehentlich ein lokales AD-Benutzerobjekt löschen, wird das entsprechende Azure AD-Benutzerobjekt im nächsten Synchronisierungszyklus gelöscht. Standardmäßig bewahrt Azure AD das gelöschte Azure AD-Benutzerobjekt für 30 Tage als „vorläufig gelöscht“ auf.

* Wenn Sie den lokalen AD-Papierkorb aktiviert haben, können Sie das gelöschte lokale AD-Benutzerobjekt wiederherstellen, ohne den Quellanker-Wert ändern zu müssen. Wenn das wiederhergestellte lokale AD-Benutzerobjekt mit Azure AD synchronisiert wird, stellt Azure AD das zugehörige, vorläufig gelöschte Azure AD-Benutzerobjekt wieder her. Informationen zum Quellanker-Attribut finden Sie im Artikel [Azure AD Connect: Designkonzepte](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#sourceanchor).

* Wenn Sie das Feature für den lokalen AD-Papierkorb nicht aktiviert haben, müssen Sie möglicherweise ein AD-Benutzerobjekt erstellen, um das gelöschte Objekt zu ersetzen. Wenn der Azure AD Connect-Synchronisierungsdienst für die Verwendung von vom System generierten AD-Attributen (z.B. „ObjectGuid“) für das Quellanker-Attribut konfiguriert wurde, hat das neu erstellte AD-Benutzerobjekt nicht denselben Quellanker-Wert wie das gelöschte AD-Benutzerobjekt. Wenn das neu erstellte AD-Benutzerobjekt mit Azure AD synchronisiert wurde, erstellt Azure AD ein neues Azure AD-Benutzerobjekt, anstatt das vorläufig gelöschte Azure AD-Benutzerobjekt wiederherzustellen.

> [!NOTE]
> Standardmäßig behält Azure AD gelöschte Azure AD-Benutzerobjekte im vorläufig gelöschten Zustand für 30 Tage bei, bevor sie dauerhaft gelöscht werden. Allerdings können Administratoren das Löschen dieser Objekte beschleunigen. Sobald die Objekte dauerhaft gelöscht wurden, können sie nicht mehr wiederhergestellt werden, selbst wenn das Feature für den lokalen AD-Papierkorb aktiviert ist.



## <a name="next-steps"></a>Nächste Schritte
**Übersichtsthemen**

* [Azure AD Connect-Synchronisierung: Grundlagen und Anpassung der Synchronisierung](active-directory-aadconnectsync-whatis.md)

* [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md)
