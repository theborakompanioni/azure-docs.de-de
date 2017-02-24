---
title: Zuweisen von Gruppen zu Azure AD-Apps | Microsoft-Dokumentation
description: "Informationen zum Implementieren der Zuweisung von Gruppen für Azure-Anwendungen"
services: active-directory
documentationcenter: 
author: IHenkel
manager: femila
editor: 
ms.assetid: 29b5ba89-a1c7-4f1f-a294-248a40106617
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: inhenk
translationtype: Human Translation
ms.sourcegitcommit: e66b606433f8924a509f2d04dae67ff00ded6dca
ms.openlocfilehash: 81b9b66bb16e183b8c3c206a4a49b2f4845bc201


---
# <a name="azure-ad-and-applications-assign-groups-to-an-application"></a>Azure AD und Anwendungen: Zuweisen von Gruppen zu einer Anwendung
Bevor Sie einer Anwendung Benutzer und Gruppen zuweisen können, müssen Sie die Benutzerzuweisung erfordern. Informationen zum Erfordern der Benutzerzuweisung finden Sie im Artikel zum [Erfordern der Benutzerzuweisung](active-directory-applications-guiding-developers-requiring-user-assignment.md) .

In diesem Artikel wird davon ausgegangen, dass Sie bereits Gruppen in Active Directory für diese Anwendung erstellt haben.

## <a name="assigning-groups-to-an-application"></a>Zuweisen von Gruppen zu einer Anwendung
1. Melden Sie sich beim Azure-Portal mit dem Administratorkonto an.
2. Klicken Sie im Hauptmenü auf **Alle Elemente** .
3. Wählen Sie das Verzeichnis aus, das Sie für die Anwendung verwenden.
4. Klicken Sie auf die Registerkarte **Anwendungen** .
5. Wählen Sie die Anwendung aus der Liste der Programme aus, die diesem Verzeichnis zugeordnet sind.
6. Klicken Sie auf die Registerkarte **Benutzer und Gruppen** .
7. Filtern Sie die Liste der Gruppen in Active Directory mithilfe der Dropdownliste **Gruppen** .
8. Wählen Sie die Gruppe aus.
9. Klicken Sie auf **Zuweisen**.
10. Klicken Sie auf **Ja** , wenn Sie dazu aufgefordert werden.

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [active-directory-applications-guiding-developers-for-lob-applications-toc.md](../../includes/active-directory-applications-guiding-developers-for-lob-applications-toc.md)]



<!--HONumber=Feb17_HO1-->


