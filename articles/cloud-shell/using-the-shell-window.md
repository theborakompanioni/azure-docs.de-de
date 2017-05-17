---
title: Verwenden des Azure Cloud Shell-Fensters (Vorschau) | Microsoft-Dokumentation
description: Exemplarische Vorgehensweise im Azure Cloud Shell-Fenster.
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: 
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: juluk
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 4e78dadb7fa036dcb4c5706fe774cbd85d57a746
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---

# <a name="using-the-shell-window"></a>Verwenden des Fensters der Shell
Dieses Dokument erläutert, wie das Fenster der Azure Cloud Shell verwendet wird.

## <a name="concurrent-sessions"></a>Gleichzeitige Sitzungen
Cloud Shell ermöglicht mehrere gleichzeitige Sitzungen auf individuellen Browserregisterkarten, wobei jede Sitzung als separater Bash-Prozess vorhanden sein kann.
Achten Sie beim Beenden einer Sitzung darauf, jedes Sitzungsfenster zu schließen, da die Prozesse unabhängig voneinander ausgeführt werden, obwohl sie auf dem gleichen Computer ausgeführt werden.

## <a name="restart-cloud-shell"></a>Neustarten der Cloud Shell
![](media/recycle.png)
* Klicken Sie auf das Neustartsymbol auf der Symbolleiste, um den Cloud Shell-Computer zurückzusetzen.

> ![WARNUNG] Durch den Neustart der Cloud Shell wird der Computerstatus zurückgesetzt, und alle Dateien, die nicht von clouddrive beibehalten werden, gehen verloren.

## <a name="minimize--maximize-cloud-shell-window"></a>Minimieren und Maximieren des Cloud Shell-Fensters
![](media/minmax.png)
* Klicken Sie auf das Symbol „Minimieren“ oben rechts im Fenster, um es auszublenden. Klicken Sie erneut auf das Cloud Shell-Symbol, um es einzublenden.
* Klicken Sie auf das Symbol „Maximieren“, um die maximale Höhe des Fensters festzulegen. Klicken Sie auf „Wiederherstellen“, um das Fenster in vorheriger Größe wiederherstellen.

## <a name="copy-and-paste"></a>Kopieren und Einfügen
* Windows: `Ctrl-insert` und `Shift-insert`, oder durch Klicken mit der rechten Maustaste zum Kopieren und Einfügen die Dropdownliste öffnen.
  * FireFox/IE unterstützen die Berechtigungen für die Zwischenablage möglicherweise nicht ordnungsgemäß.
* Mac OS: `Cmd-c` und `Cmd-v`, oder durch Klicken mit der rechten Maustaste zum Kopieren und Einfügen die Dropdownliste öffnen.

## <a name="resize-cloud-shell-window"></a>Ändern der Größe des Cloud Shell-Fensters
* Klicken Sie auf den oberen Rand der Symbolleiste, und ziehen Sie ihn nach oben oder unten, um die Größe des Cloud Shell-Fensters zu ändern.

## <a name="scrolling-text-display"></a>Scrollen der Textanzeige
* Scrollen mit Maus oder Touchpad

## <a name="exit-command"></a>Befehl „Beenden“
Ausführen von `exit` beendet die aktive Sitzung. Dieses Verhalten tritt standardmäßig nach 10 Minuten ohne Interaktion auf.

## <a name="next-steps"></a>Nächste Schritte
[Cloud Shell-Schnellstart](quickstart.md)  
