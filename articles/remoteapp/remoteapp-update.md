---
title: Aktualisieren Ihrer Azure RemoteApp-Sammlung | Microsoft Doc
description: "Hier erfahren Sie, wie Sie Ihre Azure RemoteApp-Sammlung aktualisieren."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: e553d432-e581-48fe-b996-c432357eb64a
ms.service: remoteapp
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: compute
ms.date: 04/26/2017
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 41c8a13ebd008ed4f9d8a5399bf8e272bf0fd7b2
ms.lasthandoff: 03/31/2017


---
# <a name="update-a-collection-in-azure-remoteapp"></a>Aktualisieren einer Sammlung in Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp wird am 31. August 2017 eingestellt. Details finden Sie in der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .
> 
> 

Irgendwann kommt der Punkt, an dem eine Aktualisierung der Apps oder des Image in Ihrer Azure RemoteApp-Sammlung erforderlich wird. Wenn Sie eines der Images aus Ihrem Azure RemoteApp-Abonnement verwenden (ganz gleich, ob in einer Cloud- oder in einer Hybridsammlung), werden sämtliche Aktualisierungen von Azure RemoteApp abgewickelt, und Sie müssen sich um nichts weiter kümmern.

Wenn Sie dagegen ein benutzerdefiniertes (von Grund auf neu erstelltes oder angepasstes) Image verwenden, müssen Sie das Image und die Apps selbst pflegen. Wenn Sie Ihr Image oder irgendeine der darin enthaltenen Apps aktualisieren müssen, müssen Sie eine neue aktualisierte Version des Images erstellen. Anschließend müssen Sie das vorhandene Image in Ihrer Sammlung mit diesem neuen aktualisierten Image ersetzen.

In diesem Artikel erfahren Sie, wie Sie Ihre Sammlung aktualisieren. Es ist ganz einfach:

1. Aktualisieren Sie das Image, das Sie in Ihrer Sammlung verwendet haben. Wenden Sie alle erforderlichen Patches oder Updates an, und speichern Sie es anschließend unter einem neuen Namen.
2. [Laden Sie das Image in RemoteApp hoch](remoteapp-uploadimage.md), oder [importieren](remoteapp-image-on-azurevm.md) Sie es.
3. Klicken Sie auf der Sammlungsseite auf **Aktualisieren**.
4. Wählen Sie in der Liste **Vorlagenimage** das neue Image aus.
5. Nun wird es etwas kniffliger: Sie müssen entscheiden, wie mit den Benutzern verfahren werden soll, die zurzeit eine App aus der Sammlung verwenden. Folgende Optionen stehen zur Auswahl:
   
   * **Benutzern nach dem Update 60 Minuten geben**. Nach Abschluss der Aktualisierung werden alle aktiven Benutzer in einer Meldung aufgefordert, ihre Dateien zu speichern und sich ab- und wieder anzumelden. Nach 60 Minuten werden alle aktiven Benutzer, die sich nicht abgemeldet haben, automatisch abgemeldet. Die Benutzer können sich sofort wieder anmelden.
   * **Benutzer sofort abmelden**. Bei dieser Option werden alle Benutzer ohne Vorwarnung automatisch abgemeldet, sobald die Aktualisierung abgeschlossen ist. Wenn Sie diese Option auswählen, gehen den Benutzern unter Umständen Daten verloren. Sie können sich aber umgehend wieder bei der App anmelden.
6. Klicken Sie auf das Häkchen, um die Aktualisierung zu starten.


