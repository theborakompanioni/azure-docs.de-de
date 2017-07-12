---
title: "Anwendungsseite für eine Anwendungsproxyanwendung nicht richtig angezeigt | Microsoft-Dokumentation"
description: Hilfestellung bei Anzeigeproblemen einer Anwendungsproxyanwendung, die Sie mit Azure AD integriert haben
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
ms.openlocfilehash: d0b0ddba5ad9f8c584cd35c6f20edf76883b425d
ms.contentlocale: de-de
ms.lasthandoff: 04/17/2017

---

<a id="application-page-does-not-display-correctly-for-an-application-proxy-application" class="xliff"></a>

# Anwendungsseite für eine Anwendungsproxyanwendung nicht richtig angezeigt

Die Informationen in diesem Artikel helfen Ihnen beim Beheben von Problemen mit Azure Active Directory-Anwendungsproxyanwendungen, die auftreten, wenn Sie zur Seite navigieren und die Anzeige dort Fehler aufweist.

<a id="overview" class="xliff"></a>

## Übersicht
Wenn Sie eine Anwendungsproxyanwendung veröffentlichen, können Sie beim Zugriff auf die Anwendung nur Seiten in Ihrem Stammordner öffnen. Wenn die Seite nicht ordnungsgemäß angezeigt wird, fehlen der internen Stamm-URL für die Anwendung möglicherweise einige Seitenressourcen. Um dies zu beheben, stellen Sie sicher, dass Sie *alle* Ressourcen für die Seite als Teil Ihrer Anwendung veröffentlicht haben.

Sie können überprüfen, ob das Problem dadurch verursacht wird, indem Sie eine Netzwerküberwachungssoftware öffnen (z.B. Fiddler oder F12-Tools in Internet Explorer/Edge), die Seite laden und nach Fehlern des Typs 404 suchen. Diese Fehler zeigen die Seiten an, die derzeit nicht gefunden werden und möglicherweise noch veröffentlicht werden müssen.

Ein Beispiel für diesen Fall wäre eine Ausgabenanwendung, die mit der internen URL <http://myapps/expenses> veröffentlicht wurde, die jedoch das Stylesheet <http://myapps/style.css> verwendet. In diesem Fall wird das Stylesheet nicht in der Anwendung veröffentlicht, sodass das Laden der Ausgabenanwendung einen 404-Fehler beim Laden von „style.css“ auslöst. In diesem Beispiel kann das Problem durch eine Veröffentlichung der Anwendung mit der internen URL <http://myapp/> behoben werden.

<a id="problems-with-publishing-as-one-application" class="xliff"></a>

## Probleme mit der Veröffentlichung in einer einzelnen Anwendung

Wenn eine Veröffentlichung aller Ressourcen innerhalb derselben Anwendung nicht möglich ist, müssen Sie mehrere Anwendungen veröffentlichen und Links zwischen diesen aktivieren.

Zu diesem Zweck empfiehlt sich die Lösung [Benutzerdefinierte Domänen](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains). Diese Lösung erfordert jedoch, dass Sie das Zertifikat für Ihre Domäne besitzen und Ihre Anwendungen vollqualifizierte Domänennamen (FQDNs) verwenden. Weitere Optionen finden Sie in der [Dokumentation zur Problembehandlung bei fehlerhaften Links](https://microsoft-my.sharepoint.com/personal/harshja_microsoft_com/_layouts/15/guestaccess.aspx?guestaccesstoken=IxuG3mFVbnPWI3Yn4Qi7wCNi8VIfHS5mwPt5quh8DMw%3d&docid=2_14558cd6ddea34c1c9887dc640feb5831&rev=1).

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
[Veröffentlichen von Anwendungen mit Azure AD-Anwendungsproxy](application-proxy-publish-azure-portal.md)

