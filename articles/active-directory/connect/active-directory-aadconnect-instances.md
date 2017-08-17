---
title: 'Azure AD Connect: Synchronisierungsdienstinstanzen | Microsoft Docs'
description: "Diese Seite beschreibt besondere Überlegungen für Azure AD-Instanzen."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: f340ea11-8ff5-4ae6-b09d-e939c76355a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 37df8d66d0a3ce738db2960d4283bdc672fb0fc4
ms.contentlocale: de-de
ms.lasthandoff: 07/04/2017

---
# <a name="azure-ad-connect-special-considerations-for-instances"></a>Azure AD Connect: Besondere Überlegungen zu Instanzen
Azure AD Connect wird am häufigsten mit der weltweiten Instanz von Azure AD und Office 365 verwendet. Es gibt jedoch noch weitere Instanzen, und für diese gelten andere Anforderungen an URLs sowie weitere besondere Überlegungen.

## <a name="microsoft-cloud-germany"></a>Microsoft Cloud Deutschland
Die [Microsoft Cloud Deutschland](http://www.microsoft.de/cloud-deutschland) ist eine unabhängige Cloud, die von einem deutschen Datentreuhänder betrieben wird.

| URLs, die im Proxyserver geöffnet werden müssen |
| --- |
| \*.microsoftonline.de |
| \*.windows.net |
| +Zertifikatsperrlisten |

Wenn Sie sich bei Ihrem Azure AD-Mandanten anmelden, müssen Sie ein Konto in der Domäne „onmicrosoft.de“ verwenden.

In der Microsoft Cloud Deutschland derzeit nicht enthaltene Features:

* **Azure AD Connect Health** ist nicht verfügbar.
* **Automatische Updates** sind nicht verfügbar.
* **Kennwortrückschreiben** ist für die Vorschauversion mit Azure AD Connect-Version 1.1.570.0 und höher verfügbar.
* Andere Azure AD Premium-Dienste sind nicht verfügbar.

## <a name="microsoft-azure-government-cloud"></a>Microsoft Azure Government-Cloud
Die [Microsoft Azure Government-Cloud](https://azure.microsoft.com/features/gov/) ist eine Cloud für US-Regierungsbehörden.

Diese Cloud wurde von früheren Versionen von DirSync unterstützt. Ab Build 1.1.180 von Azure AD Connect wird die nächste Generation der Cloud unterstützt. Diese Generation verwendet ausschließlich in den USA basierte Endpunkte und weist eine andere Liste von URLs auf, die Sie in Ihrem Proxyserver öffnen müssen.

| URLs, die im Proxyserver geöffnet werden müssen |
| --- |
| \*.microsoftonline.com |
| \*.microsoftonline.us |
| \*.gov.us.microsoftonline.com |
| +Zertifikatsperrlisten |

Azure AD Connect kann nicht automatisch erkennen, dass sich Ihr Azure AD-Mandant in der Government-Cloud befindet. Sie müssen daher folgende Aktionen ausführen, wenn Sie Azure AD Connect installieren.

1. Starten Sie die Azure AD Connect-Installation.
2. Wenn die erste Seite angezeigt wird, auf der Sie aufgefordert werden, den Endbenutzer-Lizenzvertrag zu akzeptieren, fahren Sie nicht fort, sondern lassen Sie den Installations-Assistenten offen.
3. Starten Sie den Registrierungs-Editor, und ändern Sie den Registrierungsschlüssel `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` in den Wert `2`.
4. Wechseln Sie zurück zum Azure AD Connect-Installations-Assistenten, akzeptieren Sie den Endbenutzer-Lizenzvertrag, und fahren Sie fort. Stellen Sie sicher, dass bei der Installation der Installationspfad zur **benutzerdefinierten Konfiguration** verwendet wird (nicht die Expressinstallation). Setzen Sie die Installation dann wie gewohnt fort.

In der Microsoft Azure Government-Cloud derzeit nicht enthaltene Features:

* **Azure AD Connect Health** ist nicht verfügbar.
* **Automatische Updates** sind nicht verfügbar.
* **Kennwortrückschreiben** ist für die Vorschauversion mit Azure AD Connect-Version 1.1.570.0 und höher verfügbar.
* Andere Azure AD Premium-Dienste sind nicht verfügbar.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md).

