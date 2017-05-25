---
title: "Einschränkungen von Azure Cloud Shell (Vorschau) | Microsoft-Dokumentation"
description: "Übersicht über die Einschränkungen von Azure Cloud Shell"
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
ms.sourcegitcommit: 44eac1ae8676912bc0eb461e7e38569432ad3393
ms.openlocfilehash: c007b73375c8c82248228f4e549c0ac95640d7ec
ms.contentlocale: de-de
ms.lasthandoff: 05/17/2017

---

# <a name="limitations-for-azure-cloud-shell"></a>Einschränkungen von Azure Cloud Shell
Für Azure Cloud Shell gelten die folgenden bekannten Einschränkungen:

## <a name="system-state-and-persistence"></a>Systemstatus und Persistenz
Der Computer mit der Cloud Shell-Sitzung besteht nur vorübergehend und wird recycelt, wenn die Sitzung zehn Minuten lang inaktiv ist. Für Cloud Shell muss eine Dateifreigabe bereitgestellt werden.
* Mit bereitgestelltem Speicher werden nur Änderungen am Verzeichnis `$Home` oder `clouddrive` gespeichert.
  * Dateifreigaben können nur innerhalb der [zugewiesenen Region](persisting-shell-storage.md#pre-requisites-for-manual-mounting) bereitgestellt werden.
  * Azure Files unterstützt nur LRS- und GRS-Speicherkonten.

## <a name="user-permissions"></a>Benutzerberechtigungen
Berechtigungen werden als reguläre Benutzer ohne sudo-Zugriff festgelegt. Jede Installation außerhalb von „$Home“ wird nicht gespeichert.
Bestimmte Befehle, etwa `git clone` im Verzeichnis `clouddrive`, verfügen nicht über die entsprechenden Berechtigungen, das Verzeichnis „$Home“ jedoch schon.

## <a name="browser-support"></a>Browserunterstützung
Cloud Shell unterstützt die aktuellen Versionen von Microsoft Edge, Microsoft Internet Explorer, Google Chrome, Mozilla Firefox und Apple Safari. Safari im privaten Modus wird nicht unterstützt.

## <a name="copy-and-paste"></a>Kopieren und Einfügen
Die Tastenkombinationen STRG+C und STRG+V ermöglichen anders als sonst auf Windows-Computern kein Kopieren bzw. Einfügen über die Zwischenablage. Verwenden Sie zum Kopieren und Einfügen STRG+EINFG bzw. UMSCHALT+EINFG.
Über einen Rechtsklick stehen ebenfalls Optionen zum Kopieren und Einfügen zur Verfügung. Dies ist jedoch vom browserspezifischen Zugriff auf die Zwischenablage abhängig.

## <a name="usage-limits"></a>Usage limits (Nutzungseinschränkungen)
Cloud Shell wurde für interaktive Anwendungsfälle entwickelt, daher werden lange Sitzungen ohne Interaktion ohne Warnung beendet.

## <a name="network-connectivity"></a>Netzwerkverbindung
Latenzen in Cloud Shell sind auf die lokale Internetanbindung zurückzuführen. Es wird versucht, Cloud Shell unter Verwendung aller gesendeten Anweisungen weiterhin auszuführen.

## <a name="next-steps"></a>Nächste Schritte
[Cloud Shell-Schnellstart](quickstart.md)
