---
title: Versionshinweise zu StorSimple Virtual Array | Microsoft Docs
description: "Beschreibt wichtige offene Probleme und Lösungen für das StorSimple Virtual Array."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 84908160-2b8b-4f4f-a674-f39aaa0bd4de
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/13/2016
ms.author: alkohli
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: f3ea83e32af4de2637d12766ee8c51adfbf0d3a3


---
# <a name="storsimple-virtual-array-release-notes"></a>Versionshinweise zu StorSimple Virtual Array
## <a name="overview"></a>Übersicht
Die folgenden Versionshinweise identifizieren die kritischen offenen Probleme für die ab März 2016 allgemein verfügbare Version von Microsoft Azure StorSimple Virtual Array (auch als "lokales virtuelles StorSimple-Gerät" oder "virtuelles StorSimple-Gerät" bezeichnet). Diese Version bezieht sich auf Softwareversion 10.0.10271.0.

Die Versionshinweise werden fortlaufend aktualisiert, und wenn schwerwiegende Probleme festgestellt werden, die eine Problemumgehung erfordern, werden sie hinzugefügt. Lesen Sie vor der Bereitstellung Ihres virtuellen StorSimple-Geräts die Informationen in den Versionsanmerkungen sorgfältig durch. 

Die folgende Tabelle enthält eine Zusammenfassung der bekannten Probleme in dieser Version.

| Nr. | Funktion | Problem | Problemumgehung/Kommentare |
| --- | --- | --- | --- |
| **1.** |Aktualisierungen |In der Vorabversion erstellte virtuellen Geräte können nicht auf eine unterstützte Version für allgemeine Verfügbarkeit aktualisiert werden. |Für diese virtuellen Geräte muss mithilfe eines Notfallwiederherstellungs-Workflows ein Failover für die allgemein verfügbare Version ausgeführt werden. |
| **2.** |Bereitgestellter Datenträger |Nachdem Sie einen Datenträger mit einer bestimmten angegebenen Größe bereitgestellt und das entsprechende virtuelle StorSimple-Gerät erstellt haben, darf der Datenträger nicht erweitert oder verkleinert werden. Ein entsprechender Versuch führt zum Verlust aller Daten auf den lokalen Ebenen des Geräts. | |
| **3.** |Gruppenrichtlinie |Wenn ein Gerät Mitglied einer Domäne ist, kann das Anwenden einer Gruppenrichtlinie den Gerätebetrieb beeinträchtigen. |Stellen Sie sicher, dass sich Ihr virtuelles Array in einer eigenen Organisationseinheit (OU) für Active Directory befindet und keine Gruppenrichtlinienobjekte (GPO) darauf angewendet werden. |
| **4.** |Lokale Web-UI |Wenn in Internet Explorer (IE ESC) erweiterten Sicherheitsfeatures aktiviert sind, funktionieren einige lokale Web-UI-Seiten wie "Problembehandlung" oder "Wartung" möglicherweise nicht ordnungsgemäß. Außerdem funktionieren Schaltflächen auf diesen Seiten möglicherweise nicht. |Deaktivieren Sie erweiterte Sicherheitsfeatures in Internet Explorer. |
| **5.** |Lokale Web-UI |Auf einer virtuellen Hyper-V-Maschine werden die Netzwerkschnittstellen in der Web-UI als 10-Gbit/s-Schnittstellen angezeigt. |Dieses Verhalten ist eine Spiegelung von Hyper-V. Hyper-V zeigt immer 10 Gbit/s für virtuelle Netzwerkadapter an. |
| **6.** |Mehrstufige Volumes oder Freigaben |Bytebereichsperren für Anwendungen, die mit den mehrstufigen StorSimple-Volumes arbeiten, werden nicht unterstützt. Wenn Bytebereichsperren aktiviert sind, funktioniert die StorSimple-Staffelung nicht. |Empfohlene Maßnahmen:  <br></br>Deaktivieren Sie Bytebereichsperren in der Anwendungslogik.<br></br>Platzieren Sie die Daten für diese Anwendung nicht in mehrstufigen Volumes, sondern in lokalen Volumes.<br></br>*Nachteil*: Wenn lokale Volumes verwendet werden und Bytebereichssperren aktiviert sind, beachten Sie, dass das lokale Volume online sein kann, bevor die Wiederherstellung abgeschlossen ist. Wenn in solchen Fällen eine Wiederherstellung ausgeführt wird, müssen Sie auf den Abschluss des Wiederherstellungsvorgangs warten. |
| **7.** |Mehrstufige Freigaben |Das Arbeiten mit großen Dateien kann zu einer langsamen Abstufung führen. |Bei der Arbeit mit großen Dateien sollte die größte Datei nach Möglichkeit kleiner als 3 % der Größe der Dateifreigabe sein. |
| **8.** |Für Freigaben genutzte Kapazität |Möglicherweise wird eine Nutzung durch die Freigabe angezeigt, auch wenn diese keine Daten enthält. Dies liegt daran, dass die für Freigaben genutzte Kapazität Metadaten umfasst. | |
| **9.** |Notfallwiederherstellung |Sie können die Notfallwiederherstellung eines Dateiservers nur in der Domäne des Quellgeräts ausführen. Die Notfallwiederherstellung auf einem Zielgerät in einer anderen Domäne wird in dieser Version nicht unterstützt. |Dies wird in einer späteren Version implementiert werden. |
| **10.** |Azure PowerShell |Die virtuellen StorSimple-Geräte können in dieser Version nicht mithilfe von Azure PowerShell verwaltet werden. |Die gesamte Verwaltung der virtuellen Geräte sollte über das klassische Azure-Portal und die lokale Web-UI erfolgen. |
| **11.** |Kennwortänderung |Die Virtual Array-Gerätekonsole akzeptiert Eingaben nur über eine En-US-Tastatur. | |
| **12.** |CHAP |Einmal erstellte CHAP-Anmeldeinformationen können nicht wieder entfernt werden. Wenn Sie die CHAP-Anmeldeinformationen ändern, müssen Sie außerdem die Volumes offline und erneut online schalten, damit die Änderungen wirksam werden. |Dies wird in einer zukünftigen Version behoben. |
| **13.** |iSCSI-Server |Der für ein iSCSI-Volume „verwendete Speicher“ kann im StorSimple Manager-Dienst und im iSCSI-Host unterschiedlich angezeigt werden. |Der iSCSI-Host umfasst die Dateisystemansicht.<br></br>Dem Gerät werden die Blöcke angezeigt, die dem Volume bei maximaler Größe zugeordnet waren. |




<!--HONumber=Nov16_HO3-->


