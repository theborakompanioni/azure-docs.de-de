---
title: Erste Schritte mit der Azure AD-Berichterstellungs-API | Microsoft Docs
description: Vorgehensweise zum Einstieg in die Azure Active Directory-Berichterstellungs-API
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8813b911-a4ec-4234-8474-2eef9afea11e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.translationtype: HT
ms.sourcegitcommit: c3ea7cfba9fbf1064e2bd58344a7a00dc81eb148
ms.openlocfilehash: b0ff68902f671518ff53772b1956272a7f6e1614
ms.contentlocale: de-de
ms.lasthandoff: 07/19/2017

---
# <a name="getting-started-with-the-azure-active-directory-reporting-api"></a>Erste Schritte mit der Berichterstellungs-API von Azure Active Directory

Azure Active Directory stellt eine Vielzahl von Berichten zur Verfügung. Die Daten dieser Berichte können für Ihre Anwendungen – beispielsweise SIEM-Systeme, Überwachungs- und Business Intelligence-Tools – sehr nützlich sein. Die Azure AD-Berichterstellungs-APIs bieten über einen Satz von REST-basierten APIs programmgesteuerten Zugriff auf die Daten. Sie können diese APIs über verschiedene Programmiersprachen und Tools aufrufen.

In diesem Artikel erhalten Sie die Informationen, die Sie zum Einstieg in die Azure AD-Berichterstellungs-APIs benötigen.
Im nächsten Abschnitt finden Sie ausführlichere Informationen zur Verwendung der APIs für Überwachung und Anmeldung. Alle weiteren APIs werden im Artikel [Azure AD reports and events (Azure AD-Berichte und -Ereignisse)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-reports-and-events-preview) behandelt.

Lesen Sie unsere häufig gestellten Fragen (FAQ) unter [FAQ](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-faq). Bei Problemen: [Stellen Sie ein Supportticket aus](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto)

## <a name="learning-map"></a>Lernschritte
1. **Vorbereiten** : Damit Sie die Beispielen in diesem Thema verwenden können, müssen die [Voraussetzungen zum Zugriff auf die Azure AD-Berichterstellungs-API](active-directory-reporting-api-prerequisites-azure-portal.md)erfüllt sein.
2. **Untersuchen** : Verschaffen Sie sich einen ersten Eindruck der Berichterstellungs-APIs:
   
   * [Verwenden der Beispiele für die Überwachungs-API](active-directory-reporting-api-audit-samples.md) 
   * [Verwenden der Beispiele für die Anmeldeaktivitätsbericht-API](active-directory-reporting-api-sign-in-activity-samples.md)
3. **Anpassen**: Erstellen Sie Ihre eigene Lösung: 
   
   * [Verwenden der Referenz zur Überwachungs-API](active-directory-reporting-api-audit-reference.md) 
   * [Verwenden der Referenz zur Anmeldeaktivitätsbericht-API](active-directory-reporting-api-sign-in-activity-reference.md)

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie alle verfügbaren Azure AD Graph-API-Endpunkte anzeigen möchten, navigieren Sie zu dieser Adresse: [https://graph.windows.net/tenant-name/activities/$metadata?api-version=beta](https://graph.windows.net/tenant-name/activities/$metadata?api-version=beta).


