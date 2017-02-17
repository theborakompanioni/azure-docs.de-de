---
title: "Bedingte Ausdrücke des Azure CDN-Regelmoduls | Microsoft-Dokumentation"
description: "Referenzdokumentation zu den Übereinstimmungsbedingungen und Features des Azure CDN-Regelmoduls."
services: cdn
documentationcenter: 
author: Lichard
manager: akucer
editor: 
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
translationtype: Human Translation
ms.sourcegitcommit: dccb945e170bd3e3f23283359db25e574a2d4296
ms.openlocfilehash: 57e56c38e003cb83dcf44f455c4451d159db8a59


---

# <a name="azure-cdn-rules-engine-conditional-expressions"></a>Bedingte Ausdrücke des Azure CDN-Regelmoduls
Dieses Thema enthält ausführliche Beschreibungen der bedingten Ausdrücke für das [Regelmodul](cdn-rules-engine.md) des Azure Content Delivery Network (CDN).

Der erste Teil einer Regel ist der bedingte Ausdruck.

Bedingter Ausdruck | Beschreibung
-----------------------|-------------
IF | Ein IF-Ausdruck ist immer Teil der ersten Anweisung einer Regel. Wie alle anderen bedingten Ausdrücke auch, muss diese IF-Anweisung einer Übereinstimmung zugeordnet sein. Wenn keine zusätzlichen bedingten Ausdrücke definiert sind, wird mit dieser Übereinstimmung das Kriterium ermittelt, das erfüllt sein muss, bevor eine Reihe von Features auf eine Anforderung angewendet werden kann.
AND IF | Ein AND IF-Ausdruck kann nur nach den folgenden Arten von bedingten Ausdrücken hinzugefügt werden: IF, AND IF. Hiermit wird angegeben, dass eine weitere Bedingung vorhanden ist, die für die erste IF-Anweisung erfüllt sein muss.
ELSE IF| Mit einem ELSE IF-Ausdruck wird eine alternative Bedingung angegeben, die erfüllt sein muss, bevor ein spezifischer Funktionssatz für diese ELSE IF-Anweisung ausgeführt werden kann. Durch das Vorhandensein einer ELSE IF-Anweisung wird das Ende der vorherigen Anweisung angegeben. Der einzige bedingte Ausdruck, der nach einer ELSE IF-Anweisung stehen kann, ist eine weitere ELSE IF-Anweisung. Dies bedeutet, dass eine ELSE IF-Anweisung nur verwendet werden kann, um eine einzelne zusätzliche Bedingung anzugeben, die erfüllt sein muss.

**Beispiel**: ![CDN-Übereinstimmungsbedingung](./media/cdn-rules-engine-reference/cdn-rules-engine-conditional-expression.png)

 > [!TIP]
   > Eine Regel kann die von einer vorherigen Regel angegebenen Aktionen überschreiben. Beispiel: Mit einer Regel vom Typ „catch-all“ werden alle Anforderungen per tokenbasierter Authentifizierung geschützt. Eine weitere Regel kann direkt darunter erstellt werden, um eine Ausnahme für bestimmte Arten von Anforderungen festzulegen.

### <a name="next-steps"></a>Nächste Schritte
* [Übersicht über das Azure CDN](cdn-overview.md)
* [Regelmodul – Referenz](cdn-rules-engine-reference.md)
* [Rules Engine Match Conditions](cdn-rules-engine-reference-match-conditions.md) (Regelmodul – Übereinstimmungsbedingungen)
* [Rules Engine Features](cdn-rules-engine-reference-features.md) (Regelmodul – Features)
* [Überschreiben des HTTP-Standardverhaltens mithilfe des Regelmoduls](cdn-rules-engine.md)



<!--HONumber=Jan17_HO4-->


