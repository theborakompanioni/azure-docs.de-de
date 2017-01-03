---
title: "Irreguläre Anmeldeaktivitäten"
description: "Ein Bericht mit Anmeldungen, die von unseren Algorithmen für maschinelles Lernen als anomal identifiziert wurden."
services: active-directory
documentationcenter: 
author: SSalahAhmed
manager: gchander
editor: 
ms.assetid: a5a270a9-a0eb-4a99-b04c-ccde3829ffe4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/04/2016
ms.author: saah;kenhoff
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 565321b6f3ad5988f383a701cb9d8bd5c9937795


---
# <a name="irregular-sign-in-activity"></a>Irreguläre Anmeldeaktivitäten
Irreguläre Anmeldungen sind Anmeldungen, die von Machine Learning-Algorithmen auf der Grundlage einer Bedingung für eine "unmögliche Strecke" in Kombination mit einem anomalen Standort und Gerät für die Anmeldung identifiziert wurden. Dies kann ein Hinweis darauf sein, dass sich ein Hacker erfolgreich mit diesem Konto angemeldet hat.
Wir senden eine E-Mail-Benachrichtigung an die globalen Administratoren, wenn wir 10 oder mehr anomale Anmeldeereignisse innerhalb eines Zeitraums von 30 Tagen oder weniger feststellen. Fügen Sie aad-alerts-noreply@mail.windowsazure.com unbedingt der Liste sicherer Absender hinzu.




<!--HONumber=Jan17_HO1-->


