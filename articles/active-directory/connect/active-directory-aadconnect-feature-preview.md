---
title: 'Azure AD Connect: Funktionen in der Vorschau | Microsoft Docs'
description: In diesem Thema werden Funktionen detaillierter beschrieben, die sich in Azure AD Connect in der Preview befinden.
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: c75cd8cf-3eff-4619-bbca-66276757cc07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.translationtype: Human Translation
ms.sourcegitcommit: b110b2e3041cf7839d1bf1c4d384b28b243efbd8
ms.openlocfilehash: c1653c769a6b42d18ffb0da71220ce06c6556587
ms.contentlocale: de-de
ms.lasthandoff: 02/07/2017

---
# Weitere Informationen zu den Funktionen in der Vorschau
<a id="more-details-about-features-in-preview" class="xliff"></a>
In diesem Thema wird beschrieben, wie Sie Funktionen verwenden, die sich derzeit in der Vorschau befinden.

## Gruppenrückschreiben
<a id="group-writeback" class="xliff"></a>
Mit der Option zum Gruppenrückschreiben, die unter den optionalen Features aufgeführt ist, können Sie **Office 365-Gruppen** in eine Gesamtstruktur zurückschreiben, in der Exchange installiert ist. Dies ist eine Gruppe, die immer in der Cloud verwaltet wird. Wenn Sie über lokales Exchange verfügen, können Sie diese Gruppen in die lokale Installation zurückschreiben, damit Benutzer mit einem lokalen Exchange-Postfach E-Mails von diesen Gruppen empfangen und an sie senden können.

Weitere Informationen zu Office 365-Gruppen und zu deren Verwendung finden Sie [hier](http://aka.ms/O365g).

Eine Office 365-Gruppe wird im lokalen AD DS als Verteilergruppe dargestellt. Auf dem lokalen Exchange-Server muss das kumulative Update 8 für Exchange Server 2013 (vom März 2015) oder Exchange 2016 installiert sein, damit dieser neue Gruppentyp erkannt wird.

**Hinweise während der Vorschau**

* Das Adressbuchattribut ist in der Vorschau derzeit nicht aufgefüllt. Ohne dieses Attribut ist diese Gruppe in der globalen Adressliste nicht sichtbar. Am einfachsten können Sie dieses Attribut mit dem Exchange PowerShell-Cmdlet `update-recipient`auffüllen.
* Nur Gesamtstrukturen mit dem Exchange-Schema sind gültige Ziele für Gruppen. Wenn kein Exchange erkannt wurde, kann das Gruppenrückschreiben nicht aktiviert werden.
* Derzeit werden nur Bereitstellungen unterstützt, bei der eine Exchange-Organisation eine einzelne Gesamtstruktur darstellt. Wenn Sie lokal über mehrere Exchange-Organisationen verfügen, benötigen Sie eine lokale Lösung für die Synchronisierung der globalen Adressliste, damit diese Gruppen in Ihren anderen Gesamtstrukturen angezeigt werden können.
* Das Feature „Gruppenrückschreiben“ verarbeitet keine Sicherheitsgruppen oder Verteilergruppen.

> [!NOTE]
> Für das Gruppenrückschreiben ist ein Azure AD Premium-Abonnement erforderlich.
> 
>

## Rückschreiben von Benutzern
<a id="user-writeback" class="xliff"></a>
> [!IMPORTANT]
> Die Vorschaufunktion „Rückschreiben von Benutzern“ wurde im Azure AD Connect-Update vom August 2015 entfernt. Wenn Sie diese Funktion aktiviert haben, sollten Sie sie deaktivieren.
>
>

## Nächste Schritte
<a id="next-steps" class="xliff"></a>
Fahren Sie mit Ihrer [benutzerdefinierten Installation von Azure AD Connect](active-directory-aadconnect-get-started-custom.md)fort.

Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md).

