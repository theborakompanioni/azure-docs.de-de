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
ms.date: 09/08/2016
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 32feb93bf6b6b77d0b14206802c776da3a8eac91


---
# <a name="azure-ad-connect-in-microsoft-cloud-germany-public-preview"></a>Azure AD Connect in Microsoft Cloud Deutschland – Öffentliche Vorschau
## <a name="introduction"></a>Einführung
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
  * Rückschreiben von Kennwörtern

## <a name="download"></a>Download
Sie können Azure AD Connect über das Blatt „Azure AD Connect“ im Portal herunterladen.  In der Anleitung unten ist angegeben, wie Sie das Blatt „Azure AD Connect“ finden.

### <a name="the-azure-ad-connect-blade"></a>Blatt „Azure AD Connect“
Führen Sie die folgenden Schritte aus, nachdem Sie sich am Azure-Portal angemeldet haben:

1. Wählen Sie „Durchsuchen“.
2. Wählen Sie „Azure Active Directory“.
3. Wählen Sie „Azure AD Connect“.

Daraufhin sollte Folgendes angezeigt werden:

![Blatt „Azure AD Connect“](media\\active-directory-aadconnect-germany\\germany1.png)

In der folgenden Tabelle sind die Funktionen des Blatts beschrieben.

| Titel | Beschreibung |
| --- | --- |
| SYNCHRONISIERUNGSSTATUS |Gibt an, ob die Synchronisierung aktiviert oder deaktiviert ist. |
| LETZTE SYNCHRONISIERUNG |Gibt den Zeitpunkt der letzten erfolgreichen Synchronisierung an. |
| PARTNERDOMÄNEN |Gibt die Anzahl der derzeit konfigurierten Partnerdomänen an. |

## <a name="installation"></a>Installation
Die Dokumentation zur Installation von Azure AD Connect finden Sie [hier](active-directory-aadconnect.md#install-azure-ad-connect).

## <a name="advanced-features-and-additional-information"></a>Erweiterte Funktionen und zusätzliche Informationen
Falls Sie weitere Informationen und eine Anleitung zu benutzerdefinierten Einstellungen oder erweiterten Konfigurationseinstellungen benötigen, sollten Sie mit [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)beginnen.  Diese Seite enthält Informationen und Links zu weiteren Anleitungen.




<!--HONumber=Nov16_HO2-->


