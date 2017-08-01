---
title: Probleme bei der Anmeldung bei benutzerdefiniert entwickelten Anwendungen | Microsoft-Dokumentation
description: "Allgemeine Fehler, die dazu führen können, dass Sie sich nicht bei einer mit Azure AD entwickelten Anwendung anmelden können"
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
ms.sourcegitcommit: 0d6f6fb24f1f01d703104f925dcd03ee1ff46062
ms.openlocfilehash: 51fac8adc2fe11aac005b4c0c0e9b67bbca8fa6e
ms.contentlocale: de-de
ms.lasthandoff: 04/17/2017

---

# <a name="problems-signing-in-to-an-custom-developed-application"></a>Probleme bei der Anmeldung bei benutzerdefiniert entwickelten Anwendungen

Es gibt mehrere Fehler, die dazu führen können, dass Sie sich nicht einer App anmelden können. Der Hauptgrund für das Auftreten dieses Problems sind falsch konfigurierte Apps.

## <a name="errors-related-to--misconfigured-apps"></a>Fehler im Zusammenhang mit falsch konfigurierten Apps

* Stellen Sie sicher, dass die Konfigurationen im Portal mit der App übereinstimmen. Vergleichen Sie insbesondere Client-/Anwendungs-ID, Antwort-URLs, Clientgeheimnisse/-schlüssel und App-ID-URI.

* Vergleichen Sie die Ressource, auf die Sie zugreifen möchten, im Code mit den konfigurierten Berechtigungen auf der Registerkarte **Erforderliche Ressourcen**, um sicherzustellen, dass Sie nur Ressourcen anfordern, die Sie konfiguriert haben.

* Weitere Informationen zu ähnlichen Fehlern oder Problemen finden Sie unter [Azure AD bei Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory).

## <a name="next-steps"></a>Nächste Schritte

[Entwicklerhandbuch zu Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)<br>

[Genehmigen und Integrieren von Apps in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications>)<br>

[Genehmigen und Zuweisen von Berechtigungen für konvergierte Azure AD V2.0-Apps](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[Azure AD bei Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory>)

