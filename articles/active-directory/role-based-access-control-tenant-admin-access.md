---
title: "Erhöhen der Zugriffsrechte für Mandantenadministratoren – Azure AD | Microsoft-Dokumentation"
description: Dieses Thema beschreibt die integrierten Rollen der rollenbasierten Zugriffssteuerung (RBAC).
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: rqureshi
ms.assetid: b547c5a5-2da2-4372-9938-481cb962d2d6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: kgremban
ms.translationtype: Human Translation
ms.sourcegitcommit: c308183ffe6a01f4d4bf6f5817945629cbcedc92
ms.openlocfilehash: 0fa44799a0bd49d3d96a1916f32e6452405abce8
ms.contentlocale: de-de
ms.lasthandoff: 05/17/2017


---
# <a name="elevate-access-as-a-tenant-admin-with-role-based-access-control"></a>Erhöhen der Rechte als Mandantenadministrator mit der rollenbasierten Zugriffssteuerung

Mit der rollenbasierten Zugriffssteuerung können Mandantenadministratoren vorübergehend erhöhte Zugriffsrechte erhalten, damit sie höhere Berechtigungen als üblich erteilen können. Ein Mandantenadministrator kann seine Rechte bei Bedarf auf die Rolle des Benutzerzugriffsadministrators erhöhen. Diese Rolle bietet dem Mandantenadministrator die Berechtigung, sich selbst oder anderen Benutzern Rollen im Bereich „/“ zu gewähren.

Dieses Feature ist wichtig, weil es dem Mandantenadministrator die Anzeige aller Abonnements ermöglicht, die in einer Organisation vorhanden sind. Zudem erlaubt es Automatisierungs-Apps (z.B. zur Fakturierung und Überwachung) den Zugriff auf alle Abonnements und die Bereitstellung einer exakten Übersicht über den Organisationsstatus im Hinblick auf Abrechnung oder Asset Management.  

## <a name="how-to-use-elevateaccess-to-give-tenant-access"></a>Verwenden von elevateAccess zum Erteilen von Zugriff an den Mandanten

Im grundlegenden Vorgang werden die folgenden Schritte verwendet:

1. Rufen Sie mithilfe von REST *elevateAccess* auf. So erhalten Sie die Rolle des Benutzerzugriffsadministrators im Bereich „/“.

    ```
    POST https://management.azure.com/providers/Microsoft.Authorization/elevateAccess?api-version=2016-07-01
    ```

2. Erstellen Sie eine [Rollenzuweisung](/rest/api/authorization/roleassignments), um eine beliebige Rolle in einem beliebigen Bereich zuzuweisen. Das folgende Beispiel zeigt die Eigenschaften für die Zuweisung der Leserrolle im Bereich „/“:

    ```
    { "properties":{
    "roleDefinitionId": "providers/Microsoft.Authorization/roleDefinitions/acdd72a7338548efbd42f606fba81ae7",
    "principalId": "cbc5e050-d7cd-4310-813b-4870be8ef5bb",
    "scope": "/"
    },
    "id": "providers/Microsoft.Authorization/roleAssignments/64736CA0-56D7-4A94-A551-973C2FE7888B",
    "type": "Microsoft.Authorization/roleAssignments",
    "name": "64736CA0-56D7-4A94-A551-973C2FE7888B"
    }
    ```

3. Solange Sie Benutzerzugriffsadministrator sind, können Sie auch Rollenzuweisungen im Bereich „/“ löschen.

4. Widerrufen Sie Ihre Berechtigungen als Benutzerzugriffsadministrator, bis sie wieder benötigt werden.


## <a name="how-to-undo-the-elevateaccess-action"></a>Rückgängigmachen der elevateAccess-Aktion

Beim Aufruf von *elevateAccess* erstellen Sie eine Rollenzuweisung für sich selbst. Um diese Berechtigungen zu widerrufen, müssen Sie die Zuweisung löschen.

1.  Rufen Sie [GET roleDefinitions](/rest/api/authorization/roledefinitions#RoleDefinitions_Get) mit „roleName = User Access-Administrator“ auf, um die Namens-GUID der Rolle des Benutzerzugriffsadministrators abzurufen. Das Ergebnis sieht in etwa wie folgt aus:

    ```
    {"value":[{"properties":{
    "roleName":"User Access Administrator",
    "type":"BuiltInRole",
    "description":"Lets you manage user access to Azure resources.",
    "assignableScopes":["/"],
    "permissions":[{"actions":["*/read","Microsoft.Authorization/*","Microsoft.Support/*"],"notActions":[]}],
    "createdOn":"0001-01-01T08:00:00.0000000Z",
    "updatedOn":"2016-05-31T23:14:04.6964687Z",
    "createdBy":null,
    "updatedBy":null},
    "id":"/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
    "type":"Microsoft.Authorization/roleDefinitions",
    "name":"18d7d88d-d35e-4fb5-a5c3-7773c20a72d9"}],
    "nextLink":null}
    ```

    Speichern Sie die GUID aus dem Parameter *name*, in diesem Fall **18d7d88d-d35e-4fb5-a5c3-7773c20a72d9**.

2. Rufen Sie [GET roleAssignments](/rest/api/authorization/roleassignments#RoleAssignments_Get) auf, wobei principalId Ihrer eigenen ObjectId entspricht. Dadurch werden all Ihre Zuweisungen im Mandanten aufgelistet. Suchen Sie nach der Zuweisung, deren Bereich „/“ lautet und deren RoleDefinitionId mit der Rollennamen-GUID endet, die Sie in Schritt 1 gefunden haben. Die Rollenzuweisung sieht in etwa folgendermaßen aus:

    ```
    {"value":[{"properties":{
    "roleDefinitionId":"/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
    "principalId":"{objectID}",
    "scope":"/",
    "createdOn":"2016-08-17T19:21:16.3422480Z",
    "updatedOn":"2016-08-17T19:21:16.3422480Z",
    "createdBy":"93ce6722-3638-4222-b582-78b75c5c6d65",
    "updatedBy":"93ce6722-3638-4222-b582-78b75c5c6d65"},
    "id":"/providers/Microsoft.Authorization/roleAssignments/e7dd75bc-06f6-4e71-9014-ee96a929d099",
    "type":"Microsoft.Authorization/roleAssignments",
    "name":"e7dd75bc-06f6-4e71-9014-ee96a929d099"}],
    "nextLink":null}
    ```

    Speichern Sie die GUID aus dem Parameter *name* erneut, in diesem Fall **e7dd75bc-06f6-4e71-9014-ee96a929d099**.

3. Rufen Sie zum Schluss [DELETE roleAssignments](/rest/api/authorization/roleassignments#RoleAssignments_DeleteById) auf, wobei roleAssignmentId der Namens-GUID entspricht, die Sie in Schritt 2 ermittelt haben.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie unter [Verwalten der rollenbasierten Zugriffssteuerung mit REST](role-based-access-control-manage-access-rest.md).

- [Verwalten von Zugriffszuweisungen](role-based-access-control-manage-assignments.md) im Azure-Portal

