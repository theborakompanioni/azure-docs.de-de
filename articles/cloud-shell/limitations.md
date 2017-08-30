---
title: "Einschränkungen von Azure Cloud Shell (Vorschau) | Microsoft-Dokumentation"
description: "Übersicht über die Einschränkungen von Azure Cloud Shell"
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
ms.date: 08/21/2017
ms.author: juluk
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: e42841b126a9df9240bec3f489589d5ce4a6db80
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---

# <a name="limitations-of-azure-cloud-shell"></a>Einschränkungen von Azure Cloud Shell
Für Azure Cloud Shell gelten die folgenden bekannten Einschränkungen:

## <a name="system-state-and-persistence"></a>Systemstatus und Persistenz
Der Computer mit Ihrer Cloud Shell-Sitzung besteht nur vorübergehend und wird recycelt, wenn die Sitzung 20 Minuten lang inaktiv ist. Für Cloud Shell muss eine Dateifreigabe bereitgestellt werden. Daher muss Ihr Abonnement Speicherressourcen für den Zugriff auf Cloud Shell einrichten können. Weitere Überlegungen:
* Mit eingebundenem Speicher werden nur Änderungen am Verzeichnis `$Home` oder `clouddrive` gespeichert.
* Dateifreigaben können nur innerhalb Ihrer [zugewiesenen Region](persisting-shell-storage.md#mount-a-new-clouddrive) eingebunden werden.
* Azure Files unterstützt nur Konten mit lokal redundantem Speicher und georedundantem Speicher.

## <a name="user-permissions"></a>Benutzerberechtigungen
Berechtigungen werden als reguläre Benutzer ohne sudo-Zugriff festgelegt. Installationen außerhalb des Verzeichnisses `$Home` werden nicht gespeichert.
Bestimmte Befehle im Verzeichnis `git clone` (beispielsweise `clouddrive`) verfügen zwar nicht über die entsprechenden Berechtigungen, das Verzeichnis `$Home` hingegen schon.

## <a name="browser-support"></a>Browserunterstützung
Cloud Shell unterstützt die aktuellen Versionen von Microsoft Edge, Microsoft Internet Explorer, Google Chrome, Mozilla Firefox und Apple Safari. Safari im privaten Modus wird nicht unterstützt.

## <a name="copy-and-paste"></a>Kopieren und Einfügen
Die Tastenkombinationen STRG+C und STRG+V zum Kopieren und Einfügen in Cloud Shell auf Windows-Computern funktionieren nicht. Verwenden Sie stattdessen STRG+EINFG bzw. UMSCHALT+EINFG.

Optionen zum Kopieren und Einfügen stehen auch im Kontextmenü zur Verfügung. Die Rechtsklickfunktion ist jedoch vom browserspezifischen Zugriff auf die Zwischenablage abhängig.

## <a name="editing-bashrc"></a>Bearbeiten von „.bashrc“
Gehen Sie bei der Bearbeitung von „.bashrc“ vorsichtig vor, da sonst unerwartete Fehler in Cloud Shell auftreten können.

## <a name="bashhistory"></a>.bash_history
Der Verlauf der Bashbefehle ist möglicherweise aufgrund einer Unterbrechung der Cloud Shell-Sitzung oder gleichzeitiger Sitzungen inkonsistent.

## <a name="usage-limits"></a>Usage limits (Nutzungseinschränkungen)
Cloud Shell ist für interaktive Anwendungsfälle konzipiert. Daher werden lange Sitzungen ohne Interaktion ohne Vorwarnung beendet.

## <a name="network-connectivity"></a>Netzwerkverbindung
Latenzen in Cloud Shell sind auf die lokale Internetkonnektivität zurückzuführen. Es wird versucht, Cloud Shell unter Verwendung aller gesendeten Anweisungen weiterhin auszuführen.

## <a name="next-steps"></a>Nächste Schritte
[Cloud Shell-Schnellstart](quickstart.md)

