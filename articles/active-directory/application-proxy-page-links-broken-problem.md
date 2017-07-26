---
title: "Links auf der Seite funktionieren nicht für eine Anwendungsproxyanwendung | Microsoft-Dokumentation"
description: Behandeln von Problemen mit fehlerhaften Links in Anwendungsproxyanwendungen, die Sie in Azure AD integriert haben
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
ms.sourcegitcommit: 165536dcaa7a5db1c913121895634659d70d5c9c
ms.openlocfilehash: e4b2556533fcb22556ca526d5815908fdefb0788
ms.contentlocale: de-de
ms.lasthandoff: 06/30/2017

---

# <a name="links-on-the-page-dont-work-for-an-application-proxy-application"></a>Links auf der Seite funktionieren nicht für eine Anwendungsproxyanwendung

In diesem Artikel finden Sie Hilfe zur Problembehandlung bei nicht ordnungsgemäß funktionierenden Links in der Azure Active Directory-Anwendungsproxyanwendung.

## <a name="overview"></a>Übersicht 
Nach dem Veröffentlichen einer Anwendungsproxy-App funktionieren standardmäßig nur Links in der Anwendung, die auf Ziele verweisen, die in der veröffentlichten Stamm-URL enthalten sind. Links innerhalb der Anwendungen funktionieren nicht, da die interne URL für die Anwendung möglicherweise nicht alle Ziele der Links in der Anwendung enthält.

**Warum geschieht dies?** Wenn Sie auf einen Link in einer Anwendung klicken, versucht der Anwendungsproxy, die URL entweder als interne URL innerhalb derselben Anwendung oder als extern verfügbare URL aufzulösen. Wenn der Link auf eine interne URL verweist, die sich nicht innerhalb derselben Anwendung befindet, gehört sie keinem der Buckets an. Dies führt zu einem Fehler, dass das Ziel nicht gefunden wurde.

## <a name="ways-you-can-resolve-broken-links"></a>Methoden zum Beheben fehlerhafter Links

Es gibt drei Möglichkeiten, dieses Problem zu beheben. Die im Folgenden aufgeführten Optionen sind nach zunehmender Komplexität aufgeführt.

1.  Stellen Sie sicher, dass die interne URL eine Stamm-URL ist, die alle relevanten Links für die Anwendung enthält. Dadurch werden alle Links als innerhalb derselben Anwendung veröffentlichter Inhalt aufgelöst.

    Wenn Sie die interne URL ändern, die Zielseite für Benutzer jedoch nicht geändert werden soll, ändern Sie die URL der Startseite in die zuvor veröffentlichte interne URL. Wechseln Sie dazu zu „Azure Active Directory -&gt; App-Registrierungen -&gt; Anwendung auswählen -&gt; Eigenschaften“. Auf der Registerkarte „Eigenschaften“ befindet sich das Feld „URL der Startseite“, in dem Sie die gewünschte Startseite eingeben können.

2.  Wenn Ihre Anwendung vollqualifizierte Domänennamen (FQDNs) verwendet, verwenden Sie [Benutzerdefinierte Domänen](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains), um Ihre Anwendungen zu veröffentlichen. Durch dieses Feature kann dieselbe URL sowohl intern als auch extern verwendet werden.

    Mit dieser Option wird sichergestellt, dass die Links in der Anwendung extern über den Anwendungsproxy zugänglich sind, da die Links mit internen URLs in der Anwendung auch extern erkannt werden. Beachten Sie, dass alle Links trotzdem zu einer veröffentlichten Anwendung gehören müssen. Mit dieser Option müssen die Links jedoch nicht zur gleichen Anwendung gehören und können mehreren Anwendungen angehören.

3.  Wenn keine dieser Optionen möglich ist, sollten Sie die Vorschau eines neuen Features verwenden, das URL-Übersetzung/-Umschreibung ermöglicht. Mit dieser Option können interne URLs oder Links, die im HTML-Text der Anwendungen vorliegen, übersetzt oder den veröffentlichten externen Anwendungsproxy-URLs zugeordnet werden. Dies funktioniert nur für Links in HTML oder CSS. Links, die über JS generiert werden, sind nicht inbegriffen. 

Daher wird dringend empfohlen, nach Möglichkeit die Lösung [Benutzerdefinierte Domänen](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains) zu nutzen. Wenn Sie an der Vorschau teilnehmen möchten, wenden Sie sich per E-Mail an <aadapfeedback@microsoft.com>, und geben Sie dabei die Anwendungs-IDs an.

## <a name="next-steps"></a>Nächste Schritte
[Verwenden von vorhandenen lokalen Proxyservern](application-proxy-working-with-proxy-servers.md)


