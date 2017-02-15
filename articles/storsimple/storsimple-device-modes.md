---
title: "Ändern des StorSimple-Gerätemodus | Microsoft Docs"
description: "Beschreibt die StorSimple-Gerätemodi und erläutert, wie Sie den Gerätemodus mithilfe von Windows PowerShell für StorSimple ändern."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: e9d7d277-8a2f-45eb-9fef-355486e14cbc
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2016
ms.author: alkohli
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 33c65bf2eecff3914f3227c76f7d638a4507e1f6


---
# <a name="change-the-device-mode-on-your-storsimple-device"></a>Ändern des Gerätemodus auf Ihrem StorSimple-Gerät
Dieser Artikel enthält eine kurze Beschreibung der verschiedenen Modi, in denen StorSimple-Geräte ausgeführt werden können. Auf Ihrem StorSimple-Gerät stehen drei Modi zur Verfügung: der normale Modus, der Verwaltungsmodus und der Wiederherstellungsmodus. 

Dieser Artikel enthält Folgendes:

* Informationen zu den StorSimple-Gerätemodi
* Informationen zur Ermittlung des derzeit aktiven Modus des StorSimple-Geräts
* Informationen zum Ändern des normalen Modus in den Wartungsmodus und *umgekehrt*

Die oben aufgeführten Verwaltungsaufgaben können nur über die Windows PowerShell-Schnittstelle Ihres StorSimple-Geräts ausgeführt werden.

## <a name="about-storsimple-device-modes"></a>Informationen zu den StorSimple-Gerätemodi
Ihr StorSimple-Gerät kann im normalen Modus, im Wartungsmodus oder im Wiederherstellungsmodus betrieben werden. Nachfolgend sind alle diese Modi kurz beschrieben.

### <a name="normal-mode"></a>Normaler Modus
Dies ist der normale Betriebsmodus für ein vollständig konfiguriertes StorSimple-Gerät. Standardmäßig sollte sich das Gerät im normalen Modus befinden.

### <a name="maintenance-mode"></a>Wartungsmodus
Manchmal muss das StorSimple-Gerät in den Wartungsmodus versetzt werden. In diesem Modus können Sie Wartungsarbeiten auf dem Gerät ausführen und Updates installieren, die den Betrieb unterbrechen, wie etwa Updates für Festplattenfirmware.

Sie können das System nur über Windows PowerShell für StorSimple in den Wartungsmodus versetzen. Alle E/A-Anforderungen werden in diesem Modus angehalten. Auch werden die Dienste wie der NVRAM- (Non-Volatile Random Access Memory) oder der Clusterdienst beendet. Beide Controller werden neu gestartet, wenn Sie diesen Modus starten oder beenden. Wenn Sie den Wartungsmodus beenden, werden alle Dienste fortgesetzt und sollten fehlerfrei funktionieren. Dies kann einige Minuten dauern.

> [!NOTE]
> **Der Wartungsmodus wird nur auf einem einwandfrei funktionierenden Gerät unterstützt. Auf einem Gerät, auf dem ein Controller oder beide Controller nicht funktionieren, wird der Modus nicht unterstützt.**
> </br>
> 
> 

### <a name="recovery-mode"></a>Wiederherstellungsmodus
Der Wiederherstellungsmodus kann als „Abgesicherter Modus von Windows mit Netzwerkunterstützung“ beschrieben werden. Beim Wiederherstellungsmodus ist das Supportteam von Microsoft involviert, das Diagnoseschritte für das System ausführt. Das Hauptziel des Wiederherstellungsmodus besteht im Abrufen der Systemprotokolle.

Wenn Ihr System in den Wiederherstellungsmodus wechselt, sollten Sie sich mit dem Microsoft-Support in Verbindung setzen, um Informationen zu den nächsten Schritten zu erhalten. Wenn Sie weitere Informationen benötigen, rufen Sie [Microsoft-Support kontaktieren](storsimple-contact-microsoft-support.md)auf.

> [!NOTE]
> **Sie können das Gerät nicht in den Wiederherstellungsmodus versetzen. Wenn sich das Gerät in einem fehlerhaften Zustand befindet, versucht der Wiederherstellungsmodus das Gerät in einen Zustand zu versetzen, in dem Supportmitarbeiter von Microsoft das Gerät untersuchen können.**
> 
> 

## <a name="determine-storsimple-device-mode"></a>Ermitteln des StorSimple-Gerätemodus
#### <a name="to-determine-the-current-device-mode"></a>So ermitteln Sie den aktuellen Gerätemodus
1. Melden Sie sich bei der seriellen Gerätekonsole an, indem Sie die Schritte unter [Verwenden von PuTTY für das Herstellen einer Verbindung mit der seriellen Gerätekonsole](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console)ausführen.
2. Sehen Sie sich die Bannermeldung im Menü der seriellen Konsole des Geräts an. Diese Meldung gibt explizit an, ob sich das Gerät im Wartungsmodus oder im Wiederherstellungsmodus befindet. Wenn die Meldung keine bestimmten Informationen zum Systemmodus enthält, befindet sich das Gerät im normalen Modus.

## <a name="change-the-storsimple-device-mode"></a>Ändern des StorSimple-Gerätemodus
Sie können das StorSimple-Gerät (vom normalen Modus) in den Wartungsmodus versetzen, um Wartungsaufgaben auszuführen oder Wartungsmodus-Updates zu installieren. Führen Sie die folgenden Schritte aus, um den Wartungsmodus zu starten oder zu beenden.

> [!IMPORTANT]
> Vergewissern Sie sich vor dem Starten des Wartungsmodus, dass beide Gerätecontroller fehlerfrei funktionieren, indem Sie den **Hardwarestatus** auf der Seite **Wartung** im klassischen Azure-Portal überprüfen. Falls einer oder beide Controller nicht fehlerfrei funktionieren, wenden Sie sich für die nächsten Schritte an den Microsoft-Support. Wenn Sie weitere Informationen benötigen, rufen Sie [Microsoft-Support kontaktieren](storsimple-contact-microsoft-support.md)auf.
> 
> 

#### <a name="to-enter-maintenance-mode"></a>So wechseln Sie in den Wartungsmodus
1. Melden Sie sich bei der seriellen Gerätekonsole an, indem Sie die Schritte unter [Verwenden von PuTTY für das Herstellen einer Verbindung mit der seriellen Gerätekonsole](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console)ausführen.
2. Wählen Sie im Menü der seriellen Konsole Option 1 aus, d.h. die **Anmeldung mit Vollzugriff**. Geben Sie das **Geräteadministratorkennwort** an, wenn Sie dazu aufgefordert werden. Das Standardkennwort lautet `Password1`.
3. Geben Sie an der Eingabeaufforderung Folgendes ein: 
   
    `Enter-HcsMaintenanceMode`
4. Es wird eine Warnmeldung angezeigt, die Sie darüber informiert, dass durch den Wartungsmodus alle E/A-Anforderungen unterbrochen werden und die Verbindung mit dem klassischen Azure-Portal getrennt wird. Sie werden aufgefordert, dies zu bestätigen. Geben Sie **J** ein, um in den Wartungsmodus zu wechseln.
5. Beide Controller werden neu gestartet. Wenn der Neustart abgeschlossen ist, wird eine andere Meldung angezeigt, die besagt, dass das Gerät in den Wartungsmodus versetzt wird.

#### <a name="to-exit-maintenance-mode"></a>So beenden Sie den Wartungsmodus
1. Melden Sie sich bei der seriellen Gerätekonsole an. Überprüfen Sie anhand der Bannermeldung, ob sich Ihr Gerät im Wartungsmodus befindet.
2. Geben Sie an der Eingabeaufforderung Folgendes ein:
   
    `Exit-HcsMaintenanceMode`
3. Eine Warnmeldung und eine Bestätigungsmeldung werden angezeigt. Geben Sie **J** ein, um den Wartungsmodus zu beenden.
4. Beide Controller werden neu gestartet. Wenn der Neustart abgeschlossen ist, wird eine andere Meldung mit dem Hinweis angezeigt, dass das Gerät in den Normalmodus versetzt wird.

## <a name="next-steps"></a>Nächste Schritte
Hier erfahren Sie, wie Sie auf Ihrem StorSimple-Gerät [Updates im normalen Modus und im Wartungsmodus installieren](storsimple-update-device.md) .




<!--HONumber=Nov16_HO3-->


