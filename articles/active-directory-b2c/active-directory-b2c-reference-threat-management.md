---
title: "Verwalten von Bedrohungen für Azure B2C | Microsoft-Dokumentation"
description: Verfahren zur Erkennung und Verhinderung von DoS- und Kennwortangriffen in Azure B2C.
services: active-directory-b2c
documentationcenter: 
author: vigunase
manager: Ajith Alexander
editor: 
ms.assetid: 6df79878-65cb-4dfc-98bb-2b328055bc2e
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2016
ms.author: 
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: 5cca5fcd445e70b4eab9197258bb783e20016f76
ms.lasthandoff: 03/31/2017


---
# <a name="azure-ad-b2c-threat-management"></a>Azure AD B2C: Verwalten von Bedrohungen
Die Verwaltung von Bedrohungen umfasst Schutz gegen Angriffe auf das System und auf Netzwerke. Ein Denial-of-Service-Angriff (DoS) kann die Verfügbarkeit beeinträchtigen und Ressourcen für die vorgesehenen Benutzer nicht verfügbar machen. Kennwortangriffe führen zu nicht autorisiertem Zugriff auf Ressourcen. Microsoft Azure Active Directory B2C verfügt über integrierte Features zum Schutz Ihrer Daten vor diesen Bedrohungen auf verschiedene Weise. 

## <a name="denial-of-service-attack"></a>Denial-of-Service-Angriff

Azure AD B2C verwendet Erkennungs- und Abwehrtechniken wie SYN-Cookies und Raten- und Verbindungsbeschränkungen, um die zugrunde liegenden Ressourcen vor diesen Angriffen zu schützen.  

## <a name="password-attacks"></a>Kennwortangriffe

Azure AD B2C umfasst auch Maßnahmen für Kennwortangriffe.  Dazu gehören Brute-Force-Kennwortangriffe und Wörterbuchangriffe.  Kennwörter, die vom Benutzer festgelegt werden, müssen eine angemessene Komplexität aufweisen.  Azure AD B2C analysiert die Integrität der Anforderungen anhand unterschiedlicher Signale, um fundiert vorgesehene Benutzer von Hackern und Botnetzen zu unterscheiden. B2C bietet eine anspruchsvolle Strategie zum Sperren von Konten basierend auf den eingegebenen Kennwörtern und nach der Wahrscheinlichkeit eines Angriffs.

[Weitere Informationen zur Verwaltung von Bedrohungen durch Microsoft](https://www.microsoft.com/trustcenter/security/threatmanagement)
