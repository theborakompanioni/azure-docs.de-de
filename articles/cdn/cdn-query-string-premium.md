---
title: "CDN – Steuern des Zwischenspeicherverhaltens von Anforderungen mit Abfragezeichenfolgen – Premium | Microsoft Docs"
description: Das Zwischenspeichern von Azure CDN-Abfragezeichenfolgen steuert, wie Dateien zwischengespeichert werden, wenn diese Abfragezeichenfolgen enthalten.
services: cdn
documentationcenter: 
author: camsoper
manager: erikre
editor: 
ms.assetid: 99db4a85-4f5f-431f-ac3a-69e05518c997
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/28/2016
ms.author: casoper
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 20efef4e60da6a0ab07eab22ff18e7ae9ab12a30


---
# <a name="controlling-caching-behavior-of-cdn-requests-with-query-strings---premium"></a>Steuern des Zwischenspeicherverhaltens von CDN-Anforderungen mit Abfragezeichenfolgen – Premium
> [!div class="op_single_selector"]
> * [Standard](cdn-query-string.md)
> * [Azure CDN Premium von Verizon](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a>Übersicht
Das Zwischenspeichern von Abfragezeichenfolgen steuert, wie Dateien zwischengespeichert werden, wenn sie Abfragezeichenfolgen enthalten.

> [!IMPORTANT]
> Die CDN-Produkte „Standard“ und „Premium“ bieten die gleiche Funktionalität zum Zwischenspeichern von Abfragezeichenfolgen, jedoch mit einer anderen Benutzeroberfläche.  Dieses Dokument beschreibt die Benutzeroberfläche für **Azure CDN Premium von Verizon**.  Informationen zum Zwischenspeichern von Abfragezeichenfolgen mit **Azure CDN Standard von Akamai** und **Azure CDN Standard von Verizon** finden Sie unter [Steuern des Zwischenspeicherverhaltens von CDN-Anforderungen mit Abfragezeichenfolgen](cdn-query-string.md).
> 
> 

Die folgenden drei Modi sind verfügbar:

* **standard-cache**: Dies ist der Standardmodus.  Der CDN-Edgeknoten übergibt die Abfragezeichenfolge bei der ersten Anforderung vom Anforderer an den Ursprung und speichert das Objekt im Cache.  Alle nachfolgenden Anforderungen dieses Objekts, die vom Edgeknoten verarbeitet werden, ignorieren die Abfragezeichenfolge bis zum Ablauf des zwischengespeicherten Objekts.
* **no-cache**: In diesem Modus werden Anforderungen mit Abfragezeichenfolgen nicht auf dem CDN-Edgeknoten zwischengespeichert.  Der Edgeknoten ruft das Objekt direkt vom Ursprung ab und übergibt es bei jeder Anforderung an den Anforderer.
* **unique-cache**: In diesem Modus wird jede Anforderung mit einer Abfragezeichenfolge als eindeutiges Asset mit eigenem Cache behandelt.  So wird z.B. die Antwort vom Ursprung für eine Anforderung für *foo.ashx?q=bar* auf dem Edgeknoten zwischengespeichert und für nachfolgende Caches mit der gleichen Abfragezeichenfolge zurückgegeben.  Eine Anforderung für *foo.ashx?q=etwasanderes* wird als separates Asset mit eigener Lebensdauer zwischengespeichert.

## <a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a>Ändern der Einstellungen für das Zwischenspeichern von Abfragezeichenfolgen für CDN-Premiumprofile
1. Klicken Sie auf dem Blatt „CDN-Profil“ auf die Schaltfläche **Verwalten** .
   
    ![Schaltfläche „Verwalten“ auf dem CDN-Profilblatt](./media/cdn-query-string-premium/cdn-manage-btn.png)
   
    Das CDN-Verwaltungsportal wird geöffnet.
2. Zeigen Sie auf die Registerkarte **HTTP Groß** und dann auf das Flyout **Cacheeinstellungen**.  Klicken Sie auf **Query-String Caching**.
   
    Die Zwischenspeicherungsoptionen für Abfragezeichenfolgen werden angezeigt.
   
    ![Zwischenspeicherungsoptionen für CDN-Abfragezeichenfolgen](./media/cdn-query-string-premium/cdn-query-string.png)
3. Treffen Sie Ihre Auswahl, und klicken Sie auf **Aktualisieren** .

> [!IMPORTANT]
> Diese Einstellungsänderungen sind vielleicht nicht sofort sichtbar, da die Verteilung der Registrierung über das CDN eine Weile dauern kann.  Bei <b>Azure CDN von Verizon</b>-Profilen ist die Weitergabe in der Regel in 90 Minuten abgeschlossen, in manchen Fällen kann es aber länger dauern.
> 
> 




<!--HONumber=Nov16_HO3-->


