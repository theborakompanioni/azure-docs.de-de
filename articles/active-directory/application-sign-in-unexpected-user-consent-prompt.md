---
title: Unerwartete Aufforderung zur Genehmigung bei der Anmeldung bei einer Anwendung | Microsoft-Dokumentation
description: "Informationen zum Behandeln von Problemen, wenn einem Benutzer eine Aufforderung zu einer von Ihnen nicht erwarteten Genehmigung für eine Anwendung angezeigt wird, die Sie in Azure AD integriert haben."
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
ms.openlocfilehash: f32de2a8868d853e7720cffa23df6b3c59ab4a49
ms.contentlocale: de-de
ms.lasthandoff: 04/17/2017

---

# <a name="unexpected-consent-prompt-when-signing-in-to-an-application"></a>Unerwartete Aufforderung zur Genehmigung bei der Anmeldung bei einer Anwendung

Viele Anwendungen, die in Azure Active Directory integriert sind, erfordern Berechtigungen für verschiedene Ressourcen, um ausgeführt werden zu können. Wenn diese Ressourcen auch in Azure Active Directory integriert sind, werden Berechtigungen für den Zugriff darauf häufig mithilfe des allgemeinen Azure AD-Genehmigungsframeworks angefordert. 

Dies führt zum Anzeigen einer Aufforderung zur Genehmigung bei der ersten Nutzung der Anwendung, was häufig nur einmal erfolgt. 

## <a name="scenarios-in-which-users-see-consent-prompts"></a>Szenarien, in denen Benutzern Aufforderungen zur Genehmigung angezeigt werden

Zusätzliche Aufforderungen können in verschiedenen Szenarien erwartet werden:

* Die von der Anwendung angeforderten Berechtigungen haben sich geändert.

* Der Benutzer, die die Anwendung ursprünglich genehmigt hat, war kein Administrator, und jetzt nutzt ein anderer Benutzer (ohne Administratorrechte) die Anwendung zum ersten Mal.

* Der Benutzer, der die Anwendung ursprünglich genehmigt hat, war ein Administrator, der jedoch die Genehmigung nicht im Auftrag der gesamten Organisation erteilt hat.

* Die Anwendung arbeitet mit einer [inkrementellen und dynamischen Genehmigung](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare#incremental-and-dynamic-consent), um zusätzliche Berechtigungen anzufordern, nachdem die Genehmigung anfänglich erteilt wurde. Dies erfolgt häufig, wenn optionale Features einer Anwendung zusätzliche Berechtigungen benötigen, die über diejenigen hinausgehen, die für die Basisfunktionen erforderlich sind.

* Die Genehmigung wurde aufgehoben, nachdem sie zunächst erteilt wurde.

* Der Entwickler hat die Anwendung so konfiguriert, dass bei jeder Nutzung eine Aufforderung zur Genehmigung angezeigt wird (Hinweis: Dies ist nicht empfehlenswert).

## <a name="next-steps"></a>Nächste Schritte

-   [Apps, Berechtigungen und Zustimmung in Azure Active Directory (Endpunkt, Version 1.0)](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)

-   [Bereiche, Berechtigungen und Zustimmung im Azure Active Directory (Endpunkt, Version 2.0)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)



