---
title: Eigenschaften von Interaktionen mit Azure Active Directory-Mandanten | Microsoft-Dokumentation
description: "Verwalten Ihrer Azure Active Directory-Mandanten, indem Sie Ihre Mandanten als vollständig unabhängige Ressourcen verstehen"
services: active-tenant
documentationcenter: 
author: curtand
manager: femila
editor: piotrci
ms.assetid: 2b862b75-14df-45f2-a8ab-2a3ff1e2eb08
ms.service: active-tenant
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/01/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017;it-pro
ms.translationtype: Human Translation
ms.sourcegitcommit: 9edcaee4d051c3dc05bfe23eecc9c22818cf967c
ms.openlocfilehash: 142bd7fea8d1a409662282b9b23a2e1598c9e86e
ms.contentlocale: de-de
ms.lasthandoff: 06/08/2017


---

# <a name="understand-how-multiple-azure-active-directory-tenants-interact"></a>Grundlegendes zum Interagieren mehrerer Azure Active Directory-Mandanten

In Azure Active Directory (Azure AD) ist jeder Mandant eine vollständig unabhängige Ressource: gleichgestellt, logisch unabhängig von anderen Mandanten, die Sie verwalten. Es gibt keine unter- und übergeordneten Beziehungen zwischen den Mandanten. Diese Unabhängigkeit zwischen den Mandanten beinhaltet Ressourcen-, Verwaltungs- und Synchronisierungsunabhängigkeit.

## <a name="resource-independence"></a>Ressourcenunabhängigkeit
* Wenn Sie eine Ressource in einem Mandanten erstellen oder löschen, hat dies keine Auswirkungen auf Ressourcen in einem anderen Mandanten. Eine teilweise geltende Ausnahme bilden externe Benutzer. 
* Wenn Sie einen Ihrer Domänennamen bei einem Mandanten verwenden, kann er bei keinem anderen Mandanten verwendet werden.

## <a name="administrative-independence"></a>Verwaltungsunabhängigkeit
Wenn ein Benutzer ohne Administratorberechtigungen aus dem Mandanten „Contoso“ einen Testmandanten „Test“ erstellt, geschieht Folgendes:

* Standardmäßig wird der Benutzer, der einen Mandanten erstellt, als externer Benutzer in diesem neuen Mandanten hinzugefügt, und ihm wird ihm die globale Administratorrolle in diesem Mandanten zugewiesen.
* Die Administratoren für den Mandanten „Contoso“ haben keine direkten Administratorberechtigungen für den Mandanten „Test“, sofern ihnen nicht ein Administrator dieses Mandanten die Berechtigungen explizit erteilt. Die Administratoren von „Contoso“ können allerdings den Zugriff auf den Mandanten „Test“ steuern, wenn sie das Benutzerkonto steuern, mit dem dieser Mandant erstellt wurde.
* Wenn Sie eine Administratorrolle für einen Benutzer in einem Mandanten hinzufügen oder entfernen, wirkt sich die Änderung nicht auf die Administratorrollen aus, über die der Benutzer in einem anderen Mandanten verfügt.

## <a name="synchronization-independence"></a>Synchronisierungsunabhängigkeit
Sie können jeden Azure AD-Mandanten unabhängig voneinander konfigurieren, damit Sie mit einer der folgenden Möglichkeiten synchronisierte Daten einer einzelnen Instanz erhalten:

* Dem Tool Azure AD Connect, um Daten mit einer AD-Gesamtstruktur zu synchronisieren
* Dem Connector des Azure Active Directory-Mandanten für Forefront Identity Manager, um Daten mit mindestens einer lokalen Gesamtstruktur und/oder anderen Datenquellen als Azure AD zu synchronisieren

## <a name="add-an-azure-ad-tenant"></a>Hinzufügen eines Azure AD-Mandanten
Um einen Azure AD-Mandanten im klassischen Azure-Portal hinzuzufügen, wählen Sie die Azure Active Directory-Erweiterung auf der linken Seite aus, und tippen Sie auf **Hinzufügen**.

> [!NOTE]
> Im Gegensatz zu anderen Azure-Ressourcen sind Ihre Mandanten keine untergeordneten Ressourcen eines Azure-Abonnements. Wenn Ihr Azure-Abonnement gekündigt oder abgelaufen ist, können Sie weiterhin mithilfe von Azure PowerShell, Azure Graph-API oder Office 365 Admin Center auf Ihre Mandantendaten zugreifen. Sie können dem Mandanten auch ein anderes Abonnement zuordnen.
>

## <a name="next-steps"></a>Nächste Schritte
Eine grobe Übersicht über Lizenzierungsprobleme und Best Practices im Zusammenhang mit Azure AD finden Sie unter [Worum handelt es sich bei der Lizenzierung von Azure Active Directory-Mandanten?](active-directory-licensing-whatis-azure-portal.md).

