---
title: Auswahl zwischen Azure MFA-Cloud und Server | Microsoft Dokumentation
description: "Entscheiden Sie sich für die am besten geeignete Multi-Factor Authentication-Sicherheitslösung, indem Sie sich fragen, was Sie sichern möchten und wo sich Ihre Benutzer befinden.  Wählen Sie dann zwischen Cloud, MFA-Server und AD FS aus."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: ec2270ea-13d7-4ebc-8a00-fa75ce6c746d
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/25/2017
ms.author: kgremban
ms.translationtype: HT
ms.sourcegitcommit: a0b98d400db31e9bb85611b3029616cc7b2b4b3f
ms.openlocfilehash: cc5a0a8ff789b878cd5afb62176ae8bcf3f80902
ms.contentlocale: de-de
ms.lasthandoff: 08/29/2017

---
# <a name="choose-the-azure-multi-factor-authentication-solution-for-you"></a>Auswählen einer geeigneten Azure Multi-Factor Authentication-Lösung
Da es mehrere Arten von Azure Multi-Factor Authentication (MFA) gibt, müssen Sie einige Fragen klären, um herauszufinden, welche Version für Sie die richtige ist.  Diese Fragen sollten Sie beantworten:

* [Was möchte ich sichern?](#what-am-i-trying-to-secure)
* [Wo befinden sich die Benutzer?](#where-are-the-users-located)
* [Welche Features benötige ich?](#what-featured-do-i-need)

In den folgenden Abschnitten erhalten Sie Informationen, die Ihnen helfen, die Antworten auf diese Fragen zu geben.

## <a name="what-am-i-trying-to-secure"></a>Was möchte ich sichern?
Um die richtige zweistufige Überprüfungslösung zu ermitteln, müssen Sie zunächst die Frage beantworten, was Sie über die zweite Methode zur Authentifizierung sichern möchten.  Handelt es sich um eine Anwendung in Azure?  Oder ist es ein RAS-System?  Die Antwort auf die Frage, wo Multi-Factor Authentication aktiviert werden muss, richtet sich danach, was Sie sichern möchten.  

| Was möchte ich sichern? | MFA in der Cloud | MFA-Server |
| --- |:---:|:---:|
| Microsoft-Apps |● |● |
| SaaS-Apps im Appkatalog |● |  |
| Über den Azure AD-App-Proxy veröffentlichte Webanwendungen |● |  |
| Nicht über den Azure AD-App-Proxy veröffentlichte IIS-Anwendungen | |● |
| Remotezugriff, z. B. VPN, RDG | ● | ● |

## <a name="where-are-the-users-located"></a>Wo befinden sich die Benutzer?
Außerdem hängt die Entscheidung für die richtige Lösung – in der Cloud oder lokal über den MFA-Server – davon ab, wo sich die Benutzer befinden.

| Benutzerstandort | MFA in der Cloud | MFA-Server |
| --- |:---:|:---:|
| Azure Active Directory |● | |
| Azure AD und lokales AD über einen Verbund mit AD FS |● |● |
| Azure AD und lokales AD mit DirSync, Azure AD Sync, Azure AD Connect ohne Kennwortsynchronisierung |● |● |
| Azure AD und lokales AD mit DirSync, Azure AD Sync, Azure AD Connect mit Kennwortsynchronisierung |● | |
| Lokales Active Directory | |● |

## <a name="what-features-do-i-need"></a>Welche Features benötige ich?
Die folgende Tabelle enthält eine Gegenüberstellung der Features von Multi-Factor Authentication in der Cloud und dem Multi-Factor Authentication-Server.

| Feature | MFA in der Cloud | MFA-Server |
| --- |:---:|:---:|
| Benachrichtigung in der mobilen App als zweite Stufe | ● | ● |
| Bestätigungscode in der mobilen App als zweite Stufe | ● | ● |
| Telefonanruf als zweite Stufe | ● | ● |
| Unidirektionale SMS als zweite Stufe | ● | ● |
| Bidirektionale SMS als zweite Stufe | | ● |
| Hardwaretoken als zweite Stufe | | ● |
| App-Kennwörter für Office 365-Clients, die MFA nicht unterstützen | ● | |
| Administrative Kontrolle über Authentifizierungsmethoden | ● | ● |
| PIN-Modus | | ● |
| Betrugswarnung |● | ● |
| MFA-Berichte |● | ● |
| Einmalumgehung | | ● |
| Benutzerdefinierte Begrüßungen für Telefonanrufe | ● | ● |
| Benutzerdefinierte Anrufer-ID für Telefonanrufe | ● | ● |
| Vertrauenswürdige IP-Adressen | ● | ● |
| Speichern der MFA für vertrauenswürdige Geräte | ● | |
| Bedingter Zugriff | ● | ● |
| Cache |  | ● |

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun den Unterschied zwischen Azure Multi-Factor Authentication in der Cloud und dem lokalen MFA-Server kennen, können Sie Azure Multi-Factor Authentication einrichten. **Wählen Sie das Symbol aus, das Ihrem Szenario entspricht.**

<center>

[![MFA in der Cloud](./media/multi-factor-authentication-get-started/cloud2.png)](multi-factor-authentication-get-started-cloud.md)  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[![MFA-Server](./media/multi-factor-authentication-get-started/server2.png)](multi-factor-authentication-get-started-server.md) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </center>

