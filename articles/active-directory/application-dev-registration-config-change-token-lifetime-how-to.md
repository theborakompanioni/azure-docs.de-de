---
title: "Ändern der Standardwerte für die Tokengültigkeitsdauer für eine benutzerdefiniert entwickelte Anwendung | Microsoft-Dokumentation"
description: "Aktualisieren der Richtlinien für die Tokengültigkeitsdauer für Ihre Anwendung, die Sie in Azure AD entwickeln"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.translationtype: Human Translation
ms.sourcegitcommit: c300ba45cd530e5a606786aa7b2b254c2ed32fcd
ms.openlocfilehash: 54c30123bb582f515dfb0324cdfd897a6c4af5c0
ms.contentlocale: de-de
ms.lasthandoff: 04/14/2017

---


<a id="how-to-change-the-token-lifetime-defaults-for-a-custom-developed-application" class="xliff"></a>

# Ändern der Standardwerte für die Tokengültigkeitsdauer für eine benutzerdefiniert entwickelte Anwendung

Azure AD Premium ermöglicht App-Entwicklern und Mandantenadministratoren die Konfiguration der Gültigkeitsdauer von Token, die für nicht vertrauliche Clients ausgegeben werden. Die Richtlinien für die Tokengültigkeitsdauer werden mandantenübergreifend oder auf Basis des Ressourcenzugriffs festgelegt.

 * Um eine Richtlinie für die Tokengültigkeitsdauer festzulegen, müssen Sie das [Azure AD PowerShell-Modul](https://www.powershellgallery.com/packages/AzureADPreview) herunterladen.

 * Führen Sie den Befehl **Connect-AzureAD -Confirm** aus.

 * Im Folgenden finden Sie eine Beispielrichtlinie, die die maximale Alterung für das Einzelfaktor-Aktualisierungstoken festlegt. Erstellen der Richtlinie: ```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```

 * Lesen Sie das Dokument [Konfigurieren der Tokengültigkeitsdauer](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes), um zu erfahren, wie Sie andere benutzerdefinierte Token erstellen.

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
[Konfigurieren der Tokengültigkeitsdauer](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes)<br>

[Azure AD-Tokenreferenz](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-token-and-claims)


