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
ms.date: 07/15/2017
ms.author: markvi
ms.custom: oldportal
ms.reviewer: dhanyahk
ms.translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 565321b6f3ad5988f383a701cb9d8bd5c9937795
ms.contentlocale: de-de
ms.lasthandoff: 12/28/2016

---
# Irreguläre Anmeldeaktivitäten
<a id="irregular-sign-in-activity" class="xliff"></a>
Irreguläre Anmeldungen sind Anmeldungen, die von Machine Learning-Algorithmen auf der Grundlage einer Bedingung für eine "unmögliche Strecke" in Kombination mit einem anomalen Standort und Gerät für die Anmeldung identifiziert wurden. Dies kann ein Hinweis darauf sein, dass sich ein Hacker erfolgreich mit diesem Konto angemeldet hat.
Wir senden eine E-Mail-Benachrichtigung an die globalen Administratoren, wenn wir 10 oder mehr anomale Anmeldeereignisse innerhalb eines Zeitraums von 30 Tagen oder weniger feststellen. Fügen Sie aad-alerts-noreply@mail.windowsazure.com unbedingt der Liste sicherer Absender hinzu.


