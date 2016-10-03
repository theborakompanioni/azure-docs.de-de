<properties
   pageTitle="Installieren von Update 3 auf dem StorSimple-Gerät | Microsoft Azure"
   description="Erfahren Sie, wie Sie Update 3 für die StorSimple 8000-Serie auf Ihrem StorSimple-Gerät der 8000-Serie installieren."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="09/21/2016"
   ms.author="alkohli" />

# Installieren von Update 3 auf Ihrem StorSimple-Gerät

## Übersicht

In diesem Tutorial wird beschrieben, wie Sie Update 3 auf einem StorSimple-Gerät mit einer früheren Softwareversion über das klassische Azure-Portal und mithilfe der Hotfixmethode ausführen. Die Hotfixmethode wird verwendet, wenn ein Gateway auf einer anderen Netzwerkschnittstelle als DATA 0 des StorSimple-Geräts konfiguriert ist und Sie versuchen, das Update von einer Softwareversion durchzuführen, die noch nicht Update 1 enthält.

Update 3 bietet Updates für die Gerätesoftware, LSI-Treiber und -Firmware, Storport und Spaceport. Wenn ausgehend von Update 2 oder einer früheren Version ein Update erfolgt, sind außerdem Updates von iSCSI, WMI und in bestimmten Fällen der Firmware von Datenträgern erforderlich. Die Fehlerbehebungen von Gerätesoftware, WMI, iSCSI, LSI-Treiber, Spaceport und Storport sind unterbrechungsfreie Updates und können über das klassische Azure-Portal angewendet werden. Die Updates für die Datenträgerfirmware führen zu einer Betriebsunterbrechung und können nur über die Windows PowerShell-Schnittstelle des Geräts angewendet werden.

> [AZURE.IMPORTANT]

> - Vor der Installation wird ein Satz manueller und automatischer Vorabprüfungen durchgeführt, mit denen die Geräteintegrität in Bezug auf Hardwarestatus und Netzwerkkonnektivität ermittelt wird. Diese Vorabprüfungen werden nur ausgeführt, wenn Sie die Updates aus dem klassischen Azure-Portal ausführen.
> - Wir empfehlen, die Software- und Treiberupdates über das klassische Azure-Portal zu installieren. Sie sollten nur dann die Windows PowerShell-Schnittstelle des Geräts (zum Installieren der Updates) verwenden, wenn die Vorabprüfung für das Gateway im Portal fehlschlägt. Abhängig von der Version, von der aus Sie aktualisieren, dauert die Installation des Updates 1,5-2,5 Stunden. Wartungsmodusupdates müssen über die Windows PowerShell-Schnittstelle des Geräts ausgeführt werden. Da Updates im Wartungsmodus den Betrieb unterbrechen, führen sie zu einer Ausfallzeit für Ihr Gerät.
> - Stellen Sie bei Ausführung des optionalen StorSimple Snapshot Manager sicher, dass Sie die Snapshot Manager-Version auf Update 2 aktualisiert haben, bevor Sie das Gerät aktualisieren.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## Installieren von Update 3 über das klassische Azure-Portal

Führen Sie die folgenden Schritte aus, um Ihr Gerät auf [Update 3](storsimple-update3-release-notes.md) zu aktualisieren.


> [AZURE.NOTE]
Wenn Sie Update 2 oder höher anwenden (einschließlich Update 2.1), wird Microsoft zusätzliche Diagnoseinformationen vom Gerät abrufen können. Dadurch können wir besser Informationen auf dem Gerät sammeln und Probleme diagnostizieren, wenn unser Betriebsteam Geräte mit Problemen ermittelt. Durch Akzeptieren von Update 2 oder höher gestatten Sie uns die Bereitstellung dieses proaktiven Supports.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Stellen Sie sicher, dass auf Ihrem Gerät **Update 3 der StorSimple 8000-Serie (6.3.9600.17759)** ausgeführt wird. Das **Datum der letzten Aktualisierung** hat sich ebenfalls geändert.

	Wenn Sie eine Version vor Update 2 aktualisieren, werden Sie auch feststellen, dass Wartungsmodus-Updates verfügbar sind. (Diese Meldung wird nach der Updateinstallation unter Umständen noch bis zu 24 Stunden lang angezeigt.)

    Wartungsmodus-Updates unterbrechen die Verwendung, führen zu Ausfallzeiten des Geräts und können nur über die Windows PowerShell-Schnittstelle Ihres Geräts angewendet werden. In einigen Fällen ist die Datenträgerfirmware beim Ausführen von Update 1.2 unter Umständen bereits auf dem neuesten Stand, sodass keine Wartungsmodus-Updates installiert werden müssen.

	Wenn ein Update ausgehend von Update 2 oder höher erfolgt ist, sollte Ihr Gerät nun auf dem neuesten Stand sein. Sie können die verbleibenden Schritte überspringen.

13. Laden Sie die Wartungsmodus-Updates herunter, indem Sie mithilfe der unter [Herunterladen von Hotfixes](#to-download-hotfixes) angegebenen Schritte KB3121899 suchen und herunterladen, um Datenträger-Firmwareupdates zu installieren. (Die anderen Updates müssten inzwischen bereits installiert sein.)

13. Führen Sie die unter [Installieren und Überprüfen von Wartungsmodus-Hotfixes](#to-install-and-verify-maintenance-mode-hotfixes) angegebenen Schritte aus, um die Wartungsmodus-Updates zu installieren.

  

## Installieren von Update 3 als Hotfix

Gehen Sie wie folgt vor, falls beim Installieren über das klassische Azure-Portal ein Fehler bei der Gatewayprüfung auftritt. Die Überprüfung zeigt einen Fehler, weil Sie ein Gateway auf einer anderen Netzwerkschnittstelle als DATA 0 zugewiesen haben und auf Ihrem Gerät eine Softwareversion vor Update 1 ausgeführt wird.

Die Softwareversionen, die mithilfe dieser Methode aktualisiert werden können, sind:

- Update 0.1, 0.2, 0.3
- Update 1, 1.1, 1.2
- Update 2, 2.1, 2.2

> [AZURE.IMPORTANT]
>
> - Wenn auf dem Gerät die Releaseversion (GA) ausgeführt wird, wenden Sie sich an den [Microsoft Support](storsimple-contact-microsoft-support.md), um Hilfe bei diesem Update zu erhalten.

Das Hotfixverfahren umfasst die folgenden drei Schritte:

1.  Laden Sie die Hotfixes aus dem Microsoft Update-Katalog herunter.

2.  Installieren und überprüfen Sie die Hotfixes für den normalen Modus.

3.  Installieren und überprüfen Sie den Wartungsmodus-Hotfix (nur, wenn Sie ein Update von der Software durchführen, die noch nicht Update 2 enthält).


#### Herunterladen von Updates für Ihr Gerät

**Wenn auf Ihrem Gerät Update 2.1 oder 2.2 ausgeführt wird**, müssen Sie die folgenden Hotfixes in der vorgeschriebenen Reihenfolge herunterladen und installieren:

| Reihenfolge | KB | Beschreibung | Updatetyp | Installationszeit |
|--------|-----------|-------------------------|------------- |-------------|
| 1\. | KB3186843 | Softwareupdate &#42; | Regulär <br></br>Unterbrechungsfrei | ~ 45 Min. |
| 2\. | KB3186859 | LSI-Treiber und -Firmware | Regulär <br></br>Unterbrechungsfrei | ~ 20 Min. |
| 3\. | KB3121261 | Storport- und Spaceport-Fix </br> Windows Server 2012 R2 | Regulär <br></br>Unterbrechungsfrei | ~ 20 Min. |

&#42; *Beachten Sie, dass das Softwareupdate aus zwei Binärdateien besteht: Update für die Gerätesoftware mit dem Präfix `all-hcsmdssoftwareupdate` und CIS- und MDS-Agent mit dem Präfix `all-cismdsagentupdatebundle`. Das Update für die Gerätesoftware muss vor dem CIS- und MDS-Agent installiert werden. Sie müssen auch den aktiven Controller über das `Restart-HcsController`-Cmdlet neu starten, nachdem Sie das Update für den CIS- und MDS-Agent angewendet haben (und bevor Sie die verbleibenden Updates anwenden).*


**Wenn auf Ihrem Gerät Update 0.1, 0.2, 0.3, 1.0, 1.1, 1.2 oder 2.0 ausgeführt wird**, müssen Sie zusätzlich zu den Software-, LSI-Treiber- und -Firmware-Updates (in der vorherigen Tabelle gezeigt) die folgenden Hotfixes in der vorgeschriebenen Reihenfolge herunterladen und installieren:

| Reihenfolge | KB | Beschreibung | Updatetyp | Installationszeit |
|--------|-----------|-------------------------|------------- |-------------|
| 4\. | KB3146621 | iSCSI-Paket | Regulär <br></br>Unterbrechungsfrei | ~ 20 Min. |
| 5\. | KB3103616 | WMI-Paket | Regulär <br></br>Unterbrechungsfrei | ~ 12 Min. |


<br></br>

**Wenn auf Ihrem Gerät die Version 0.2, 0.3, 1.0, 1.1 und 1.2 ausführt wird**, müssen Sie zusätzlich zu den in den vorhergehenden Tabellen enthaltenen Updates auch Updates der Firmware von Datenträgern installieren. Sie können überprüfen, ob Sie Firmware-Updates für Datenträger benötigen, indem Sie das Cmdlet `Get-HcsFirmwareVersion` ausführen. Wenn Sie die folgenden Firmwareversionen ausführen (`XMGG`, `XGEG`, `KZ50`, `F6C2`, `VR08`), müssen Sie diese Updates nicht installieren.


| Reihenfolge | KB | Beschreibung | Updatetyp | Installationszeit |
|--------|-----------|-------------------------|------------- |-------------|
| 6\. | KB3121899 | Datenträgerfirmware | Wartung <br></br>Mit Unterbrechung | ~ 30 Min. |
 
<br></br>

> [AZURE.IMPORTANT]
>
> - Dieses Verfahren muss nur einmal ausgeführt werden, um Update 3 anzuwenden. Über das klassischen Azure-Portal können Sie nachfolgende Updates installieren.
> - Wenn Sie von Update 2.2 aktualisieren, dauert die gesamte Installation etwa 1,1 Stunden.
> - Stellen Sie vor dem Verwenden dieses Verfahrens zum Anwenden des Updates sicher, dass beide Gerätecontroller online und alle Hardwarekomponenten fehlerfrei sind.

Führen Sie die folgenden Schritte aus, um diese Datei herunterzuladen und die Hotfixes zu installieren.

[AZURE.INCLUDE [storsimple-install-update3-hotfix](../../includes/storsimple-install-update3-hotfix.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## Nächste Schritte

Weitere Informationen finden Sie unter [Versionsanmerkungen zu Update 3](storsimple-update3-release-notes.md).

<!---HONumber=AcomDC_0921_2016-->