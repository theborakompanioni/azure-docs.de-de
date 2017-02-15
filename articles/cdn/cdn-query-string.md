---
title: Steuern des Azure CDN-Zwischenspeicherverhaltens von Anforderungen mit Abfragezeichenfolgen | Microsoft Docs
description: Das Zwischenspeichern von Azure CDN-Abfragezeichenfolgen steuert, wie Dateien zwischengespeichert werden, wenn diese Abfragezeichenfolgen enthalten.
services: cdn
documentationcenter: 
author: camsoper
manager: erikre
editor: 
ms.assetid: 17410e4f-130e-489c-834e-7ca6d6f9778d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/28/2016
ms.author: casoper
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 664c734e17e87ce2e088055759345ebd9b325f11


---
# <a name="controlling-caching-behavior-of-cdn-requests-with-query-strings"></a>Steuern des Zwischenspeicherverhaltens von CDN-Anforderungen mit Abfragezeichenfolgen
> [!div class="op_single_selector"]
> * [Standard](cdn-query-string.md)
> * [Azure CDN Premium von Verizon](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a>Übersicht
Das Zwischenspeichern von Abfragezeichenfolgen steuert, wie Dateien zwischengespeichert werden, wenn sie Abfragezeichenfolgen enthalten.

> [!IMPORTANT]
> Die CDN-Produkte „Standard“ und „Premium“ bieten die gleiche Funktionalität zum Zwischenspeichern von Abfragezeichenfolgen, jedoch mit einer anderen Benutzeroberfläche.  Dieses Dokument beschreibt die Benutzeroberfläche für **Azure CDN Standard von Akamai** und **Azure CDN Standard von Verizon**.  Informationen zum Zwischenspeichern von Abfragezeichenfolgen mit **Azure CDN Premium von Verizon**finden Sie unter [Steuern des Zwischenspeicherverhaltens von CDN-Anforderungen mit Abfragezeichenfolgen – Premium](cdn-query-string-premium.md).
> 
> 

Die folgenden drei Modi sind verfügbar:

* **Abfragezeichenfolgen ignorieren**: Dies ist der Standardmodus.  Der CDN-Edgeknoten übergibt die Abfragezeichenfolge bei der ersten Anforderung vom Anforderer an den Ursprung und speichert das Objekt im Cache.  Alle nachfolgenden Anforderungen dieses Objekts, die vom Edgeknoten verarbeitet werden, ignorieren die Abfragezeichenfolge bis zum Ablauf des zwischengespeicherten Objekts.
* **Cachingumgehung für URLs mit Abfragezeichenfolgen**: In diesem Modus werden Anforderungen mit Abfragezeichenfolgen nicht auf dem CDN-Edgeknoten zwischengespeichert.  Der Edgeknoten ruft das Objekt direkt vom Ursprung ab und übergibt es bei jeder Anforderung an den Anforderer.
* **Cache für eindeutige URL**: In diesem Modus wird jede Anforderung mit einer Abfragezeichenfolge als eindeutiges Objekt mit eigenem Cache behandelt.  So wird z.B. die Antwort vom Ursprung für eine Anforderung für *foo.ashx?q=bar* auf dem Edgeknoten zwischengespeichert und für nachfolgende Caches mit der gleichen Abfragezeichenfolge zurückgegeben.  Eine Anforderung für *foo.ashx?q=etwasanderes* wird als separates Asset mit eigener Lebensdauer zwischengespeichert.

## <a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a>Ändern der Einstellungen für das Zwischenspeichern von Abfragezeichenfolgen für CDN-Standardprofile
1. Klicken Sie auf dem Blatt „CDN-Profil“ auf den CDN-Endpunkt, den Sie verwalten möchten.
   
    ![Blade-Endpunkte für CDN-Profile](./media/cdn-query-string/cdn-endpoints.png)
   
    Das Blatt „CDN-Endpunkt“ öffnet sich.
2. Klicken Sie auf die Schaltfläche **Konfigurieren** .
   
    ![Schaltfläche „Verwalten“ auf dem CDN-Profilblatt](./media/cdn-query-string/cdn-config-btn.png)
   
    Es wird das Blatt „CDN-Konfiguration“ geöffnet.
3. Wählen Sie eine Einstellung aus der Dropdown-Liste **Verhalten beim Zwischenspeichern von Abfragezeichenfolgen** .
   
    ![Zwischenspeicherungsoptionen für CDN-Abfragezeichenfolgen](./media/cdn-query-string/cdn-query-string.png)
4. Treffen Sie Ihre Auswahl, und klicken Sie auf **Speichern** .

> [!IMPORTANT]
> Diese Einstellungsänderungen sind vielleicht nicht sofort sichtbar, da die Verteilung der Registrierung über das CDN eine Weile dauern kann.  Geben Sie unter <b>Azure CDN von Akamai</b> ist die Weitergabe in der Regel innerhalb einer Minute abgeschlossen.  Bei <b>Azure CDN von Verizon</b>-Profilen ist die Weitergabe in der Regel in 90 Minuten abgeschlossen, in manchen Fällen kann es aber länger dauern.
> 
> 




<!--HONumber=Nov16_HO3-->


