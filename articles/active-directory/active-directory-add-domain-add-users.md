---
title: "Zuweisen von Benutzern zu einer benutzerdefinierten Domäne in Azure Active Directory | Microsoft Docs"
description: "Erfahren Sie, wie Sie eine benutzerdefinierte Domäne in Azure Active Directory mit Benutzerkonten auffüllen."
services: active-directory
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 717b5a7c-7bc3-4ab1-98b5-4740b53338fe
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.translationtype: HT
ms.sourcegitcommit: 349fe8129b0f98b3ed43da5114b9d8882989c3b2
ms.openlocfilehash: 39cb54a6637088c35c6aef864a804c24803f48ba
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

---
# <a name="assign-users-to-a-custom-domain"></a>Zuweisen von Benutzern zu einer benutzerdefinierten Domäne
Nachdem Sie Ihre benutzerdefinierte Domäne mit Azure Active Directory hinzugefügt haben, müssen Sie die Benutzerkonten für diese Domäne hinzufügen, damit Sie mit deren Authentifizierung beginnen können.

> [!IMPORTANT]
> Microsoft empfiehlt, für die Verwaltung von Azure AD anstelle des in diesem Artikel erwähnten klassischen Azure-Portals das [Azure AD Admin Center](https://aad.portal.azure.com) zu verwenden. Informationen zum Verwalten der Domänennamen im Azure AD Admin Center finden Sie unter [Verwalten von benutzerdefinierten Domänennamen in Azure Active Directory](active-directory-domains-manage-azure-portal.md).

## <a name="users-synced-from-a-on-premises-directory"></a>Aus einem lokalen Verzeichnis synchronisierte Benutzer
Wenn Sie bereits eine Verbindung zwischen Ihrem lokalen Active Directory und Azure Active Directory eingerichtet haben, kann die Synchronisierung Konten auffüllen. Weitere Informationen zum Synchronisieren von Azure Active Directory mit Ihrem lokalen Active Directory finden Sie unter [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md).

## <a name="users-added-and-managed-in-the-cloud"></a>Benutzer, die in der Cloud hinzugefügt und verwaltet werden
So ändern Sie die Domäne für ein vorhandenes Benutzerkonto

1. Öffnen Sie das klassische Azure-Portal mit einem Konto, das ein globaler Administrator oder ein Benutzeradministrator ist.
2. Öffnen Sie das Verzeichnis.
3. Klicken Sie auf die Registerkarte **Benutzer** .
4. Wählen Sie den Benutzer aus der Liste aus.
5. Ändern Sie die Domäne für den Benutzer, und wählen Sie dann **Speichern**aus.

Sie können dies auch mit [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) oder der [Graph-API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations) durchführen.

## <a name="select-a-custom-domain-when-creating-a-new-user"></a>Auswählen einer benutzerdefinierten Domäne beim Erstellen neuer Benutzer
1. Öffnen Sie das klassische Azure-Portal mit einem Konto, das ein globaler Administrator oder ein Benutzeradministrator ist.
2. Öffnen Sie das Verzeichnis.
3. Klicken Sie auf die Registerkarte **Benutzer** .
4. Wählen Sie in der Befehlsleiste **Hinzufügen**aus.
5. Wenn Sie den Benutzernamen hinzufügen, wählen Sie die benutzerdefinierte Domäne aus der Domänenliste aus.
6. Wählen Sie **Speichern**aus.

## <a name="next-steps"></a>Nächste Schritte
* [Verwenden von benutzerdefinierten Domänennamen zum Vereinfachen des Anmeldevorgangs für Benutzer](active-directory-add-domain.md)
* [Verwalten von benutzerdefinierten Domänennamen](active-directory-add-manage-domain-names.md)
* [Informationen zu den Konzepten der Domänenverwaltung in Azure AD](active-directory-add-domain-concepts.md)


