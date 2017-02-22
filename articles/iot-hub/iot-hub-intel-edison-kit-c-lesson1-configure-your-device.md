---
title: "Verbinden von Intel Edison (C) mit Azure IoT – Lektion 1: Konfigurieren des Geräts | Microsoft-Dokumentation"
description: Konfigurieren von Intel Edison zur erstmaligen Verwendung
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Arduino-Setup, Arduino mit PC verbinden, Setup von Arduino, Arduino-Platine
ms.assetid: bb8aa45b-d3ff-4438-b9d6-a9725a45ade1
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/7/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 475b25f02715a60493e79ecd2170854019dfc4ac
ms.openlocfilehash: 800f1aed6d30d2bb871a6a9b55b6b95308932211


---
# <a name="configure-your-intel-edison"></a>Konfigurieren von Intel Edison
## <a name="what-you-will-do"></a>Aufgaben
Sie konfigurieren Intel Edison für die erstmalige Verwendung, indem Sie die Platine einbauen, diese einschalten und das Konfigurationstool in Ihrem Desktopbetriebssystem mit dem Ziel installieren, die Firmware von Edison einzuspielen, das dazugehörige Kennwort festzulegen und sie mit dem WLAN zu verbinden. Sollten Probleme auftreten, finden Sie Lösungen auf der [Seite zur Problembehandlung][troubleshooting].

## <a name="what-you-will-learn"></a>Sie lernen Folgendes
In diesem Artikel lernen Sie Folgendes:

* Einbauen und Einschalten der Edison-Platine
* Einspielen der Edison-Firmware, Festlegen des Kennworts und Herstellen der WLAN-Verbindung

## <a name="what-you-need"></a>Erforderliches Element
Für diesen Vorgang benötigen Sie die folgenden Teile aus dem Intel Edison Starter Kit:

* Intel® Edison-Modul
* Arduino-Erweiterungsplatine
* Im Lieferumfang enthaltene Abstandshalter und Schrauben, z.B. zwei Schrauben zum Befestigen des Moduls an der Erweiterungsplatine und vier Sätze von Schrauben und Kunststoff-Abstandshaltern.
* USB-Kabel (Micro B zu Typ A)
* Netzteil (Gleichstrom) Das Netzteil sollte wie folgt klassifiziert sein:
  - 7 -&15; V (Gleichstrom)
  - Mindestens 1500 mA
  - Der mittlere/innere Stift muss der positive Pol des Netzteils sein.

  ![Teile aus dem Starter Kit](media/iot-hub-intel-edison-lessons/lesson1/kit.png)

Außerdem benötigen Sie:

* Einen Computer (Windows, Mac oder Linux).
* Eine WLAN-Verbindung für Edison
* Eine Internetverbindung zum Herunterladen des Konfigurationstools

## <a name="assemble-your-board"></a>Zusammenbauen der Platine

In diesem Abschnitt wird beschrieben, wie Sie das Intel® Edison-Modul an der Erweiterungsplatine anbringen.

1. Legen Sie das Intel® Edison-Modul in die weiße Umrandung auf der Erweiterungsplatine, und richten Sie die Öffnungen im Modul an den Schrauben auf der Erweiterungsplatine aus.

2. Drücken Sie das Modul direkt unterhalb der Wörter `What will you make?` herunter, bis es einrastet.

   ![Zusammenbauen der Platine 2](media/iot-hub-intel-edison-lessons/lesson1/assemble_board2.jpg)

3. Befestigen Sie das Modul mithilfe der im Lieferumfang enthaltenen Sechskantmuttern an der Erweiterungsplatine.

   ![Zusammenbauen der Platine 3](media/iot-hub-intel-edison-lessons/lesson1/assemble_board3.jpg)

4. Fügen Sie eine Schraube in eine der vier Ecköffnungen der Erweiterungsplatine ein. Drehen Sie einen der weißen Kunststoff-Abstandshalter auf die Schraube, und ziehen Sie ihn fest.

   ![Zusammenbau der Platine 4](media/iot-hub-intel-edison-lessons/lesson1/assemble_board4.jpg)

5. Wiederholen Sie diesen Schritt für die Abstandshalter der anderen drei Ecken.

   ![Zusammenbau der Platine 5](media/iot-hub-intel-edison-lessons/lesson1/assemble_board5.jpg)

Ihre Platine ist nun zusammengebaut.

   ![Zusammengebaute Platine](media/iot-hub-intel-edison-lessons/lesson1/assembled_board.jpg)

## <a name="power-up-edison"></a>Einschalten des Edison

1. Stecken Sie das Netzteil ein.

   ![Einstecken des Netzteils](media/iot-hub-intel-edison-lessons/lesson1/plug_power.jpg)

2. Eine grüne LED (mit der Bezeichnung DS1 auf der Arduino*-Erweiterungsplatine) sollte aufleuchten und eingeschaltet bleiben.

3. Warten Sie eine Minute, bis die Platine hochgefahren ist.

   > [!NOTE]
   > Wenn Sie kein Gleichstromnetzteil haben, können Sie die Platine über einen USB-Anschluss mit Strom versorgen. Im Abschnitt `Connect Edison to your computer` finden Sie ausführliche Informationen hierzu. Die Stromversorgung Ihrer Platine auf diese Weise kann zu einem unvorhersehbaren Verhalten führen, insbesondere bei Verwenden eines WLAN oder Ansteuern von Motoren.

## <a name="connect-edison-to-your-computer"></a>Verbinden von Edison mit Ihrem Computer

1. Drücken Sie den Mikroschalter in Richtung der beiden Micro-USB-Anschlüsse herunter, um Edison in den Gerätemodus zu versetzen. Unterschiede zwischen dem Geräte- und Hostmodus werden [hier](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode) erläutert.

   ![Herunterdrücken des Mikroschalters](media/iot-hub-intel-edison-lessons/lesson1/toggle_down_microswitch.jpg)

2. Stecken Sie das Micro-USB-Kabel in den oberen Micro-USB-Anschluss ein.

   ![Oberer Micro-USB-Anschluss](media/iot-hub-intel-edison-lessons/lesson1/top_usbport.jpg)

3. Stecken Sie das andere Ende des USB-Kabels in den Computer ein.

   ![USB-Anschluss des Computers](media/iot-hub-intel-edison-lessons/lesson1/computer_usb.jpg)

4. Sie wissen, dass Ihre Platine vollständig initialisiert wurde, wenn auf dem Computer ein neues Laufwerk bereitgestellt und angezeigt wird (ähnlich wie beim Einsetzen einer SD-Karte in Ihren Computer).

## <a name="download-and-run-the-configuration-tool"></a>Herunterladen und Ausführen des Konfigurationstools
Laden Sie über [diesen Link](https://software.intel.com/en-us/iot/hardware/edison/downloads) das neueste Konfigurationstool herunter, das unter der Überschrift `Installers` aufgeführt ist. Führen Sie das Tool aus, und befolgen Sie die Anweisungen auf dem Bildschirm. Klicken Sie bei Bedarf auf „Weiter“.

### <a name="flash-firmware"></a>Einspielen der Firmware
1. Klicken Sie auf der Seite `Set up options` auf `Flash Firmware`.
2. Wählen Sie das Image aus, das auf Ihre Platine eingespielt werden soll, indem Sie einen der folgenden Schritte ausführen:
   - Wählen Sie zum Herunterladen und Einspielen des neuesten von Intel angebotenen Firmware-Images auf Ihrer Platine `Download the latest image version xxxx` aus.
   - Um auf Ihre Platine ein Image einzuspielen, das Sie bereits auf Ihrem Computer gespeichert haben, wählen Sie `Select the local image` aus. Navigieren Sie zum Image, das Sie auf Ihre Platine einspielen möchten, und wählen Sie es aus.
3. Das Setuptool versucht, das Einspielen auf Ihre Platine durchzuführen. Der gesamte Einspielvorgang kann bis zu 10 Minuten dauern.

### <a name="set-password"></a>Festlegen des Kennworts
1. Klicken Sie auf der Seite `Set up options` auf `Enable Security`.
2. Sie können für die Intel® Edison-Platine einen benutzerdefinierten Namen festlegen. Dies ist optional.
3. Geben Sie ein Kennwort für die Platine ein, und klicken Sie auf `Set password`.
4. Notieren Sie sich das Kennwort, das später verwendet wird.

### <a name="connect-wi-fi"></a>Verbinden mit dem WLAN
1. Klicken Sie auf der Seite `Set up options` auf `Connect Wi-Fi`. Warten Sie bis zu einer Minute, während der Computer verfügbare WLAN-Netzwerke sucht.
2. Wählen Sie in der Dropdownliste `Detected Networks` Ihr Netzwerk aus.
3. Wählen Sie in der Dropdownliste `Security` den Sicherheitstyp des Netzwerks aus.
4. Geben Sie Ihren Anmeldenamen und das Kennwort ein, und klicken Sie auf `Configure Wi-Fi`.
5. Notieren Sie sich die IP-Adresse, die später verwendet wird.

> [!NOTE]
> Vergewissern Sie sich, dass Edison mit dem gleichen Netzwerk wie der Computer verbunden ist. Ihr Computer verbindet sich mit Edison mithilfe der IP-Adresse.

Glückwunsch! Sie haben Edison erfolgreich konfiguriert.

## <a name="summary"></a>Zusammenfassung
In diesem Artikel haben Sie erfahren, wie Sie die Edison-Platine einbauen, ihre Firmware einspielen, ein Kennwort festlegen und mithilfe des Konfigurationstools eine WLAN-Verbindung herstellen. Beachten Sie, dass die LED noch nicht leuchtet. Der nächste Schritt ist das Installieren der erforderlichen Tools und Software als Vorbereitung für eine Beispielanwendung, die auf Edison ausgeführt wird.

## <a name="next-steps"></a>Nächste Schritte
[Abrufen der Tools][get-the-tools]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[get-the-tools]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-win32.md


<!--HONumber=Jan17_HO4-->


