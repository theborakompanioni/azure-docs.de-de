---
title: "Installieren von Update 2.2 auf dem StorSimple-Gerät | Microsoft Docs"
description: "Erfahren Sie, wie Sie Update 2.2 für die StorSimple 8000-Serie auf Ihrem StorSimple-Gerät der Serie 8000 installieren."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 047c7a4b-73d0-45ea-8d51-c54d71871392
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/02/2016
ms.author: alkohli
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: c60b4de88d60f0373d69fe2f3cee5ccf888b8d8c


---
# <a name="install-update-22-on-your-storsimple-device"></a>Installieren von Update 2.2 auf Ihrem StorSimple-Gerät
## <a name="overview"></a>Übersicht
In diesem Tutorial wird beschrieben, wie Sie Update 2.2 auf einem StorSimple-Gerät mit einer früheren Softwareversion über das klassische Azure-Portal und mithilfe der Hotfixmethode ausführen. Die Hotfixmethode wird verwendet, wenn ein Gateway auf einer anderen Netzwerkschnittstelle als DATA 0 des StorSimple-Geräts konfiguriert ist und Sie versuchen, das Update von einer Softwareversion durchzuführen, die noch nicht Update 1 enthält.

Update 2.2 enthält Gerätesoftware-, WMI- und iSCSI-Updates. Wenn Sie von Version 2.1 aktualisieren, muss nur das Update für die Gerätesoftware angewendet werden. Wenn Sie von einer Version aktualisieren, die noch nicht Update 2 enthält, müssen Sie ebenso Updates des LSI-Treibers, Spaceport, Storport und Datenträgerfirmware anwenden. Die Fehlerbehebungen von Gerätesoftware, WMI, iSCSI, LSI-Treiber, Spaceport und Storport sind unterbrechungsfreie Updates und können über das klassische Azure-Portal angewendet werden. Die Updates für die Datenträgerfirmware führen zu einer Betriebsunterbrechung und können nur über die Windows PowerShell-Schnittstelle des Geräts angewendet werden. 

> [!IMPORTANT]
> * Vor der Installation wird ein Satz manueller und automatischer Vorabprüfungen durchgeführt, mit denen die Geräteintegrität in Bezug auf Hardwarestatus und Netzwerkkonnektivität ermittelt wird. Diese Vorabprüfungen werden nur ausgeführt, wenn Sie die Updates aus dem klassischen Azure-Portal ausführen.
> * Wir empfehlen, die Software- und Treiberupdates über das klassische Azure-Portal zu installieren. Sie sollten nur dann die Windows PowerShell-Schnittstelle des Geräts (zum Installieren der Updates) verwenden, wenn die Vorabprüfung für das Gateway im Portal fehlschlägt. Abhängig von der Version, von der aus Sie aktualisieren, dauert die Installation des Updates 1,5-2,5 Stunden. Wartungsmodusupdates müssen über die Windows PowerShell-Schnittstelle des Geräts ausgeführt werden. Da Updates im Wartungsmodus den Betrieb unterbrechen, führen sie zu einer Ausfallzeit für Ihr Gerät.
> * Stellen Sie bei Ausführung des optionalen StorSimple Snapshot Manager sicher, dass Sie die Snapshot Manager-Version auf Update 2.2 aktualisiert haben, bevor Sie das Gerät aktualisieren.
> 
> 

[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-22-via-the-azure-classic-portal"></a>Installieren von Update 2.2 über das klassische Azure-Portal
Führen Sie die folgenden Schritte aus, um Ihr Gerät auf [Update 2.2](storsimple-update21-release-notes.md)zu aktualisieren.

> [!NOTE]
> Wenn Sie Update 2 oder höher anwenden (einschließlich Update 2.1), wird Microsoft zusätzliche Diagnoseinformationen vom Gerät abrufen können. Dadurch können wir besser Informationen auf dem Gerät sammeln und Probleme diagnostizieren, wenn unser Betriebsteam Geräte mit Problemen ermittelt. Durch Akzeptieren von Update 2 oder höher gestatten Sie uns die Bereitstellung dieses proaktiven Supports.
> 
> 

[!INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

1. Stellen Sie sicher, dass auf Ihrem Gerät Folgendes ausgeführt wird: **StorSimple 8000 Series Update 2.2 (6.3.9600.17708)**. Das **Datum der letzten Aktualisierung** hat sich ebenfalls geändert. 
   
   Wenn Sie eine Version vor Update 2 aktualisieren, werden Sie auch feststellen, dass Wartungsmodus-Updates verfügbar sind. (Diese Meldung wird nach der Updateinstallation unter Umständen noch bis zu 24 Stunden lang angezeigt.)
   
   Wartungsmodus-Updates unterbrechen die Verwendung, führen zu Ausfallzeiten des Geräts und können nur über die Windows PowerShell-Schnittstelle Ihres Geräts angewendet werden. In einigen Fällen ist die Datenträgerfirmware beim Ausführen von Update 1.2 unter Umständen bereits auf dem neuesten Stand, sodass keine Wartungsmodus-Updates installiert werden müssen.
   
   Wenn Sie ein Update von Update 2 durchführen, sollte Ihr Gerät nun auf dem neuesten Stand sein. Sie können die verbleibenden Schritte überspringen.
2. Laden Sie die Wartungsmodus-Updates herunter, indem Sie mithilfe der unter [Herunterladen von Hotfixes](#to-download-hotfixes) angegebenen Schritte nach KB3121899 suchen und es herunterladen, um Datenträger-Firmwareupdates zu installieren. (Die anderen Updates müssten inzwischen bereits installiert sein.)
3. Führen Sie die unter [Installieren und Überprüfen von Wartungsmodus-Hotfixes](#to-install-and-verify-maintenance-mode-hotfixes) angegebenen Schritte aus, um die Wartungsmodus-Updates zu installieren. 

## <a name="install-update-22-as-a-hotfix"></a>Installieren von Update 2.2 als Hotfix
Gehen Sie wie folgt vor, falls beim Installieren über das klassische Azure-Portal ein Fehler bei der Gatewayprüfung auftritt. Die Überprüfung zeigt einen Fehler, weil Sie ein Gateway auf einer anderen Netzwerkschnittstelle als DATA 0 zugewiesen haben und auf Ihrem Gerät eine Softwareversion vor Update 1 ausgeführt wird.

Die Softwareversionen, die mithilfe dieser Methode aktualisiert werden können, sind:

* Update 0.1, 0.2, 0.3
* Update 1, 1.1, 1.2
* Update 2, 2.1 

> [!IMPORTANT]
> * Wenn auf dem Gerät die Releaseversion (GA) ausgeführt wird, wenden Sie sich an den [Microsoft Support](storsimple-contact-microsoft-support.md) , um Hilfe bei diesem Update zu erhalten.
> 
> 

Das Hotfixverfahren umfasst die folgenden drei Schritte:

* Laden Sie die Hotfixes aus dem Microsoft Update-Katalog herunter.
* Installieren und überprüfen Sie die Hotfixes für den normalen Modus.
* Installieren und überprüfen Sie den Wartungsmodus-Hotfix (nur, wenn Sie ein Update von der Software durchführen, die noch nicht Update 2 enthält).

#### <a name="download-updates-for-a-device-running-update-21-software"></a>Herunterladen von Updates für ein Gerät mit der Update 2.1-Software
**Wenn auf Ihrem Gerät Update 2.1 ausgeführt wird**, müssen Sie nur das Update für die Gerätesoftware KB3179904 herunterladen. Installieren Sie nur die Binärdatei mit dem Präfix „all-hcsmdssoftwareudpate“. Installieren Sie das CIS- und das MDS-Agent-Update mit dem Präfix `all-cismdsagentupdatebundle`nicht. Andernfalls kommt es zu einem Fehler. Dies ist ein unterbrechungsfreies Update, E/A-Vorgänge werden nicht unterbrochen, und es treten keine Ausfallzeiten auf dem Gerät auf.

#### <a name="download-updates-for-a-device-running-update-2-software"></a>Herunterladen von Updates für ein Gerät mit der Update 2-Software
**Wenn auf Ihrem Gerät Update 2 ausgeführt wird**, müssen Sie die folgenden Hotfixes in der vorgeschriebenen Reihenfolge herunterladen und installieren:

| Reihenfolge | KB | Beschreibung | Updatetyp | Installationszeit |
| --- | --- | --- | --- | --- |
| 1. |KB3179904 |Softwareupdate &#42; |Regulär  <br></br>Unterbrechungsfrei |~ 45 Min. |
| 2. |KB3146621 |iSCSI-Paket |Regulär  <br></br>Unterbrechungsfrei |~ 20 Min. |
| 3. |KB3103616 |WMI-Paket |Regulär  <br></br>Unterbrechungsfrei |~ 12 Min. |

 &#42; *Beachten Sie, dass das Softwareupdate aus zwei Binärdateien besteht: aus einem Update für die Gerätesoftware mit dem Präfix `all-hcsmdssoftwareupdate` und dem CIS- und MDS-Agent mit dem Präfix `all-cismdsagentupdatebundle`. Das Update für die Gerätesoftware muss vor dem CIS- und MDS-Agent installiert werden. Sie müssen auch den aktiven Controller über das `Restart-HcsController`-Cmdlet neu starten, nachdem Sie das Update für den CIS- und MDS-Agent angewendet haben (und bevor Sie die verbleibenden Updates anwenden).* 

#### <a name="download-updates-for-a-device-running-pre-update-2-software"></a>Herunterladen von Updates für ein Gerät mit der Software vor Update 2
**Wenn auf Ihrem Gerät die Versionen 0.2, 0.3, 1.0 und 1.1 ausgeführt werden**, müssen Sie zusätzlich zu den Software-, iSCSI- und WMI-Updates das LSI-Treiber- und Firmwareupdate herunterladen und installieren. Dieses Update ist bereits installiert, wenn Sie Update 1.2 oder 2 ausführen. 

| Reihenfolge | KB | Beschreibung | Updatetyp | Installationszeit |
| --- | --- | --- | --- | --- |
| 4. |KB3121900 |LSI-Treiber und Firmware |Regulär  <br></br>Unterbrechungsfrei |~ 20 Min. |

<br></br>
**Wenn auf Ihrem Gerät die Versionen 0.2, 0.3, 1.0, 1.1 und 1.2 ausgeführt werden**, müssen Sie die Spaceport- und Storport-Fehlerbehebung herunterladen und installieren. Diese sind bereits installiert, wenn Sie Update 2 ausführen

| Reihenfolge | KB | Beschreibung | Updatetyp | Installationszeit |
| --- | --- | --- | --- | --- |
| 5. |KB3090322 |Spaceport-Fehlerbehebung  </br>  Windows Server 2012 R2 |Regulär  <br></br>Unterbrechungsfrei |~ 20 Min. |
| 6. |KB3080728 |Storport-Fehlerbehebung  </br>  Windows Server 2012 R2 |Regulär  <br></br>Unterbrechungsfrei |~ 20 Min. |

<br></br>
 Möglicherweise müssen Sie auch Updates für die Datenträgerfirmware installieren. Sie können überprüfen, ob Sie Updates für die Datenträgerfirmware benötigen, indem Sie das Cmdlet `Get-HcsFirmwareVersion` ausführen. Wenn Sie die folgenden Firmwareversionen ausführen (`XMGG`, `XGEG`, `KZ50`, `F6C2`, `VR08`), müssen Sie diese Updates nicht installieren.

| Reihenfolge | KB | Beschreibung | Updatetyp | Installationszeit |
| --- | --- | --- | --- | --- |
| 7. |KB3121899 |Datenträgerfirmware |Wartung  <br></br>Mit Unterbrechung |~ 30 Min. |

<br></br>

> [!IMPORTANT]
> * Dieses Verfahren muss nur einmal ausgeführt werden, um Update 2.2 anzuwenden. Über das klassischen Azure-Portal können Sie nachfolgende Updates installieren.
> * Wenn Sie von Update 2 aktualisieren, dauert die gesamte Installation etwa 1,5 Stunden.
> * Stellen Sie vor dem Verwenden dieses Verfahrens zum Anwenden des Updates sicher, dass beide Gerätecontroller online und alle Hardwarekomponenten fehlerfrei sind.
> 
> 

Führen Sie die folgenden Schritte aus, um diese Datei herunterzuladen und die Hotfixes zu installieren.

[!INCLUDE [storsimple-install-update21-hotfix](../../includes/storsimple-install-update21-hotfix.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter [Versionsanmerkungen zu Update 2.1](storsimple-update21-release-notes.md).




<!--HONumber=Nov16_HO3-->


