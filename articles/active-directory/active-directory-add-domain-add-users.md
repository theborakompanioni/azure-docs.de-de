---
title: "Zuweisen von Benutzern zu einer benutzerdefinierten Domäne in Azure Active Directory | Microsoft Docs"
description: "Erfahren Sie, wie Sie eine benutzerdefinierte Domäne in Azure Active Directory mit Benutzerkonten auffüllen."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 717b5a7c-7bc3-4ab1-98b5-4740b53338fe
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2016
ms.author: curtand
ms.translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: ccfa8d72b51c43e04d924d2b096e3cbc407af043
ms.contentlocale: de-de
ms.lasthandoff: 12/28/2016


---
# <a name="assign-users-to-a-custom-domain"></a>Zuweisen von Benutzern zu einer benutzerdefinierten Domäne
Nachdem Sie Ihre benutzerdefinierte Domäne mit Azure Active Directory hinzugefügt haben, müssen Sie die Benutzerkonten für diese Domäne hinzufügen, damit Sie mit deren Authentifizierung beginnen können.

## <a name="users-synced-in-from-a-directory-on-your-corporate-network"></a>Benutzer, die aus einem Verzeichnis in Ihrem Unternehmensnetzwerk synchronisiert werden
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


