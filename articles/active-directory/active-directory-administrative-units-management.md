---
title: Verwalten von Verwaltungseinheiten in Azure Active Directory (Vorschau)
description: "Verwenden von Verwaltungseinheiten zum präziseren Delegieren von Berechtigungen in Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 8464cd6b-1d1a-470d-a4fb-ee29b8eab4c4
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/17/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: oldportal;it-pro;
ms.translationtype: HT
ms.sourcegitcommit: 847eb792064bd0ee7d50163f35cd2e0368324203
ms.openlocfilehash: e12a0aea8264b1ea67c26294ec5bbe9c404a171e
ms.contentlocale: de-de
ms.lasthandoff: 08/19/2017

---
# <a name="administrative-units-management-in-azure-ad---public-preview"></a>Verwaltung von Verwaltungseinheiten in Azure AD: Öffentliche Vorschau
Dieser Artikel beschreibt Verwaltungseinheiten – einen neuen Azure Active Directory-Container mit Ressourcen, die zum Delegieren administrativer Berechtigungen und Anwenden von Richtlinien für bestimmte Benutzergruppen verwendet werden können. In Azure Active Directory können Administratoren mit Verwaltungseinheiten Berechtigungen zu regionalen Administratoren delegieren oder eine Richtlinie auf präziser Ebene festlegen.

Dies ist hilfreich in Organisationen mit unabhängigen Bereichen, z. B. eine große Universität, die aus vielen autonomen Fakultäten besteht (eine Wirtschaftsfakultät, eine Ingenieursfakultät, usw.), die unabhängig voneinander sind. Solche Bereiche haben ihre eigenen IT-Administratoren, die den Zugriff kontrollieren, Benutzer verwalten und Richtlinien festlegen, die speziell für die jeweilige Abteilung gelten. Zentrale Administratoren wollen in der Lage sein, den Administratoren der einzelnen Bereiche Berechtigungen für die Benutzer in ihren Bereichen zu erteilen. Genauer gesagt kann ausgehend von diesem Beispiel ein zentraler Administrator z. B. eine Verwaltungseinheit für eine bestimmten Fakultät (Wirtschaftsfakultät) erstellen und sie nur mit den Benutzern der Wirtschaftsfakultät befüllen. Dann kann ein zentraler Administrator den IT-Mitarbeitern der Wirtschaftsfakultät eine bereichsbezogene Rolle hinzufügen. Oder anders ausgedrückt, er kann den IT-Mitarbeitern der Wirtschaftsfakultät Berechtigungen nur für die Verwaltungseinheit „Wirtschaftsfakultät“ gewähren.

> [!IMPORTANT]
> Sie können auf Verwaltungseinheiten bezogene Administratorrollen nur zuweisen, wenn Sie Azure Active Directory Premium aktivieren. Weitere Informationen finden Sie unter [Erste Schritte mit Azure AD Premium](active-directory-get-started-premium.md).
>


Aus Sicht des zentralen Administrators ist eine Verwaltungseinheit ein Verzeichnisobjekt, das erstellt und mit Ressourcen aufgefüllt werden kann. **In dieser Vorschauversion können diese Ressourcen nur Benutzer sein.** Sobald erstellt und aufgefüllt, kann die Verwaltungseinheit zum Einschränken der erteilten Berechtigung nur für Ressourcen in der Verwaltungseinheit verwendet werden.

## <a name="managing-administrative-units"></a>Verwalten von Verwaltungseinheiten
In dieser Vorschauversion können Sie Verwaltungseinheiten mit dem Azure Active Directory-Modul für Windows PowerShell-Cmdlets erstellen und verwalten. Weitere Informationen hierzu finden Sie unter [Verwenden von Verwaltungseinheiten](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0).

Weitere Informationen zu Softwareanforderungen und der Installation des Azure AD-Moduls sowie Informationen zu den Azure AD-Modul-Cmdlets zur Verwaltung von Verwaltungseinheiten, einschließlich Syntax, Beschreibung der Parameter und Beispielen, finden Sie unter [Working with Administrative Units](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0) (Verwenden von Verwaltungseinheiten).

## <a name="next-steps"></a>Nächste Schritte
[Azure Active Directory-Editionen](active-directory-editions.md)

