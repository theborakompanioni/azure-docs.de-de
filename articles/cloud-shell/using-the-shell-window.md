---
title: Verwenden des Azure Cloud Shell-Fensters (Vorschau) | Microsoft-Dokumentation
description: Exemplarische Vorgehensweise im Azure Cloud Shell-Fenster.
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: juluk
ms.translationtype: HT
ms.sourcegitcommit: b6c65c53d96f4adb8719c27ed270e973b5a7ff23
ms.openlocfilehash: a47961dfdaf178a6b793bd68105d9792a9275bb3
ms.contentlocale: de-de
ms.lasthandoff: 08/17/2017

---

# <a name="using-the-azure-cloud-shell-window"></a>Verwenden des Azure Cloud Shell-Fensters

Dieses Dokument erläutert, wie das Cloud Shell-Fenster verwendet wird.

## <a name="concurrent-sessions"></a>Gleichzeitige Sitzungen
Cloud Shell ermöglicht mehrere gleichzeitige Sitzungen auf individuellen Browserregisterkarten, wobei jede Sitzung als separater Bash-Prozess vorhanden sein kann.
Achten Sie beim Beenden einer Sitzung darauf, jedes Sitzungsfenster zu schließen, da die Prozesse unabhängig voneinander ausgeführt werden, obwohl sie auf dem gleichen Computer ausgeführt werden.

## <a name="restart-cloud-shell"></a>Neustarten der Cloud Shell
![](media/recycle.png)
> [!WARNING]
> Durch Neustart der Cloud Shell wird der Computerstatus zurückgesetzt und alle Dateien, die nicht von Ihrer Dateifreigabe beibehalten werden, gehen verloren.

* Klicken Sie auf das Neustartsymbol in der Symbolleiste, um eine neue Cloud Shell-Umgebung zu empfangen.

## <a name="minimize--maximize-cloud-shell-window"></a>Minimieren und Maximieren des Cloud Shell-Fensters
![](media/minmax.png)
* Klicken Sie auf das Symbol „Minimieren“ oben rechts im Fenster, um es auszublenden. Klicken Sie erneut auf das Cloud Shell-Symbol, um es einzublenden.
* Klicken Sie auf das Symbol „Maximieren“, um die maximale Höhe des Fensters festzulegen. Klicken Sie auf „Wiederherstellen“, um das Fenster in vorheriger Größe wiederherstellen.

## <a name="copy-and-paste"></a>Kopieren und Einfügen
* Windows: Drücken Sie `Ctrl-insert` zum Kopieren und `Shift-insert` zum Einfügen. Das Kopieren und Einfügen ist auch durch Öffnen der Dropdownliste mittels Rechtsklick möglich.
  * FireFox/IE unterstützen die Berechtigungen für die Zwischenablage möglicherweise nicht ordnungsgemäß.
* Mac OS: Drücken Sie `Cmd-c` zum Kopieren und `Cmd-v` zum Einfügen. Das Kopieren und Einfügen ist auch durch Öffnen der Dropdownliste mittels Rechtsklick möglich.

## <a name="resize-cloud-shell-window"></a>Ändern der Größe des Cloud Shell-Fensters
* Klicken Sie auf den oberen Rand der Symbolleiste, und ziehen Sie ihn nach oben oder unten, um die Größe des Cloud Shell-Fensters zu ändern.

## <a name="scrolling-text-display"></a>Scrollen der Textanzeige
* Scrollen Sie mit Ihrer Maus oder Ihrem Touchpad, um Terminaltext zu verschieben.

## <a name="exit-command"></a>Befehl „Beenden“
Ausführen von `exit` beendet die aktive Sitzung. Dieses Verhalten tritt standardmäßig nach 20 Minuten ohne Interaktion auf.

## <a name="next-steps"></a>Nächste Schritte
[Cloud Shell-Schnellstart](quickstart.md)

