---
title: Azure AD Connect in Microsoft Cloud Deutschland
description: "Azure AD Connect integriert Ihre lokalen Verzeichnisse in Azure Active Directory. Dadurch können Sie eine einzige Identität für in Azure AD integrierte in Office 365-, Azure- und SaaS-Anwendungen bereitstellen."
keywords: "Einführung in Azure AD Connect, Übersicht über Azure AD Connect, was ist Azure AD Connect, Active Directory installieren, Deutschland, Black Forest"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 2bcb0caf-5d97-46cb-8c32-bda66cc22dad
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/12/2017
ms.author: billmath
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 780728950199bac6a317767ef1db4462b3fe6ffd
ms.contentlocale: de-de
ms.lasthandoff: 07/04/2017

---
# Azure AD Connect in Microsoft Cloud Deutschland – Öffentliche Vorschau
<a id="azure-ad-connect-in-microsoft-cloud-germany---public-preview" class="xliff"></a>
## Einführung
<a id="introduction" class="xliff"></a>
Azure AD Connect ermöglicht die Synchronisierung zwischen Ihrer lokalen Active Directory-Instanz und Azure Active Directory.
Derzeit müssen viele Szenarien in [Microsoft Cloud Deutschland](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx) vom Bediener durchgeführt werden. Beachten Sie bei der Nutzung von Microsoft Cloud Deutschland Folgendes:

* Die folgenden URLs müssen auf einem Proxyserver geöffnet werden, damit die Synchronisierung erfolgreich durchgeführt werden kann:
  
  * *.microsoftonline.de
  * *.windows.net
  * * Zertifikatsperrlisten
* Wenn Sie sich bei Ihrem Azure AD-Verzeichnis anmelden, müssen Sie ein Konto in der Domäne „onmicrosoft.de“ verwenden.
* Folgende Funktionen stehen nicht zur Verfügung:
  * Azure AD Connect Health
  * Automatische Aktualisierungen
 
## Download
<a id="download" class="xliff"></a>
Sie können Azure AD Connect über das Blatt „Azure AD Connect“ im Portal herunterladen.  In der Anleitung unten ist angegeben, wie Sie das Blatt „Azure AD Connect“ finden.

### Blatt „Azure AD Connect“
<a id="the-azure-ad-connect-blade" class="xliff"></a>
Führen Sie die folgenden Schritte aus, nachdem Sie sich am Azure-Portal angemeldet haben:

1. Wählen Sie „Durchsuchen“.
2. Wählen Sie „Azure Active Directory“.
3. Wählen Sie „Azure AD Connect“.

Daraufhin sollte Folgendes angezeigt werden:

![Blatt „Azure AD Connect“](media/active-directory-aadconnect-germany/germany1.png)

In der folgenden Tabelle sind die Funktionen des Blatts beschrieben.

| Titel | Beschreibung |
| --- | --- |
| SYNCHRONISIERUNGSSTATUS |Gibt an, ob die Synchronisierung aktiviert oder deaktiviert ist. |
| LETZTE SYNCHRONISIERUNG |Gibt den Zeitpunkt der letzten erfolgreichen Synchronisierung an. |
| PARTNERDOMÄNEN |Gibt die Anzahl der derzeit konfigurierten Partnerdomänen an. |

## Installation
<a id="installation" class="xliff"></a>
Die Dokumentation zur Installation von Azure AD Connect finden Sie [hier](active-directory-aadconnect.md#install-azure-ad-connect).

## Erweiterte Funktionen und zusätzliche Informationen
<a id="advanced-features-and-additional-information" class="xliff"></a>
Falls Sie weitere Informationen und eine Anleitung zu benutzerdefinierten Einstellungen oder erweiterten Konfigurationseinstellungen benötigen, sollten Sie mit [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)beginnen.  Diese Seite enthält Informationen und Links zu weiteren Anleitungen.


