---
title: "Intel Edison an Cloud (C) – Verbinden von Intel Edison mit Azure IoT Hub | Microsoft-Dokumentation"
description: Verbinden Sie Intel Edition mit Azure IoT Hub, damit Intel Edison Daten an die Azure-Cloud sendet.
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Azure IoT Intel Edison, Intel Edison IoT Hub, Intel Edison Daten an Cloud senden, Intel Edison an Cloud
ms.assetid: 4885fa2c-c2ee-4253-b37f-ccd55f92b006
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 4/17/2017
ms.author: xshi
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 8c4e33a63f39d22c336efd9d77def098bd4fa0df
ms.openlocfilehash: fd6d04159c46439b5f1a91be7d2fa1b500bc92b3
ms.lasthandoff: 04/20/2017


---
# <a name="connect-intel-edison-to-azure-iot-hub-c"></a>Verbinden von Intel Edison mit Azure IoT Hub (C)

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

In diesem Tutorial machen Sie sich zuerst mit den Grundlagen der Verwendung des Intel Edison vertraut. Anschließend erfahren Sie, wie Sie Ihre Geräte mithilfe von [Azure IoT Hub](iot-hub-what-is-iot-hub.md) nahtlos mit der Cloud verbinden.

Sie haben noch kein Kit? Beginnen Sie [hier](https://azure.microsoft.com/develop/iot/starter-kits).

## <a name="what-you-do"></a>Aufgaben

* Richten Sie Intel Edison und Grove-Module ein.
* Erstellen Sie einen IoT Hub.
* Registrieren Sie ein Gerät für Edison in Ihren IoT Hub.
* Führen Sie eine Beispielanwendung auf Edison aus, um Sensordaten an Ihren IoT Hub zu senden.

Verbinden Sie Intel Edison mit einem von Ihnen erstellten IoT Hub. Führen Sie anschließend eine Beispielanwendung auf Edison aus, um Temperatur- und Luftfeuchtigkeitsdaten eines Grove-Temperatursensors zu erfassen. Senden Sie abschließend die Sensordaten an Ihren IoT Hub.

## <a name="what-you-learn"></a>Lerninhalt

* Erstellen eines Azure IoT Hubs und Abrufen der Verbindungszeichenfolge für Ihr neues Gerät
* Verbinden von Edison mit einem Grove-Temperatursensor
* Erfassen von Sensordaten durch Ausführen einer Beispielanwendung auf Edison
* Senden von Sensordaten an Ihren IoT Hub

## <a name="what-you-need"></a>Erforderliches Element

![Erforderliches Element](media/iot-hub-intel-edison-kit-c-get-started/0_kit.png)

* Intel Edison-Platine
* Arduino-Erweiterungsplatine
* Ein aktives Azure-Abonnement. Wenn Sie kein Azure-Konto besitzen, können Sie in nur wenigen Minuten ein [kostenloses Azure-Testkonto](https://azure.microsoft.com/free/) erstellen.
* Mac oder PC, auf dem Windows oder Linux ausgeführt wird
* Eine Internetverbindung.
* USB-Kabel (Micro B zu Typ A)
* Netzteil (Gleichstrom) Das Netzteil sollte wie folgt klassifiziert sein:
  - 7 - 15 V (Gleichstrom)
  - Mindestens 1500 mA
  - Der mittlere/innere Stift muss der positive Pol des Netzteils sein.

Die folgenden Elemente sind optional:

* Grove Base Shield V2
* Grove-Temperatursensor
* Grove-Kabel
* Im Lieferumfang enthaltene Abstandshalter und Schrauben, z.B. zwei Schrauben zum Befestigen des Moduls an der Erweiterungsplatine und vier Sätze von Schrauben und Kunststoff-Abstandshaltern.

> [!NOTE] 
Diese Elemente sind optional, da das Codebeispiel simulierte Sensordaten unterstützt.

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-intel-edison"></a>Einrichten von Intel Edison

### <a name="assemble-your-board"></a>Zusammenbauen der Platine

In diesem Abschnitt wird beschrieben, wie Sie das Intel® Edison-Modul an der Erweiterungsplatine anbringen.

1. Legen Sie das Intel® Edison-Modul in die weiße Umrandung auf der Erweiterungsplatine, und richten Sie die Öffnungen im Modul an den Schrauben auf der Erweiterungsplatine aus.

2. Drücken Sie das Modul direkt unterhalb der Wörter `What will you make?` herunter, bis es einrastet.

   ![Zusammenbauen der Platine 2](media/iot-hub-intel-edison-kit-c-get-started/1_assemble_board2.jpg)

3. Befestigen Sie das Modul mithilfe der im Lieferumfang enthaltenen Sechskantmuttern an der Erweiterungsplatine.

   ![Zusammenbauen der Platine 3](media/iot-hub-intel-edison-kit-c-get-started/2_assemble_board3.jpg)

4. Fügen Sie eine Schraube in eine der vier Ecköffnungen der Erweiterungsplatine ein. Drehen Sie einen der weißen Kunststoff-Abstandshalter auf die Schraube, und ziehen Sie ihn fest.

   ![Zusammenbau der Platine 4](media/iot-hub-intel-edison-kit-c-get-started/3_assemble_board4.jpg)

5. Wiederholen Sie diesen Schritt für die Abstandshalter der anderen drei Ecken.

   ![Zusammenbau der Platine 5](media/iot-hub-intel-edison-kit-c-get-started/4_assemble_board5.jpg)

Ihre Platine ist nun zusammengebaut.

   ![Zusammengebaute Platine](media/iot-hub-intel-edison-kit-c-get-started/5_assembled_board.jpg)

### <a name="connect-the-grove-base-shield-and-the-temperature-sensor"></a>Anschließen des Grove Base Shield und des Temperatursensors

1. Setzen Sie das Grove Base Shield auf die Platine. Vergewissern Sie sich, dass alle Stifte fest in der Platine sitzen.
   
   ![Grove Base Shield](media/iot-hub-intel-edison-kit-c-get-started/6_grove_base_sheild.jpg)

2. Schließen Sie den Grove-Temperatursensor über das Grove-Kabel an den Port **A0** des Grove Base Shield an.

   ![Anschließen des Temperatursensors](media/iot-hub-intel-edison-kit-c-get-started/7_temperature_sensor.jpg)
   
   ![Verbindung zwischen Edison und Sensor](media/iot-hub-intel-edison-kit-c-get-started/16_edion_sensor.png)

Der Sensor ist damit betriebsbereit.

### <a name="power-up-edison"></a>Einschalten des Edison

1. Stecken Sie das Netzteil ein.

   ![Einstecken des Netzteils](media/iot-hub-intel-edison-kit-c-get-started/8_plug_power.jpg)

2. Eine grüne LED (mit der Bezeichnung DS1 auf der Arduino*-Erweiterungsplatine) sollte aufleuchten und eingeschaltet bleiben.

3. Warten Sie eine Minute, bis die Platine hochgefahren ist.

   > [!NOTE]
   > Wenn Sie kein Gleichstromnetzteil haben, können Sie die Platine über einen USB-Anschluss mit Strom versorgen. Im Abschnitt `Connect Edison to your computer` finden Sie ausführliche Informationen hierzu. Die Stromversorgung Ihrer Platine auf diese Weise kann zu einem unvorhersehbaren Verhalten führen, insbesondere bei Verwenden eines WLAN oder Ansteuern von Motoren.

### <a name="connect-edison-to-your-computer"></a>Verbinden von Edison mit Ihrem Computer

1. Drücken Sie den Mikroschalter in Richtung der beiden Micro-USB-Anschlüsse herunter, um Edison in den Gerätemodus zu versetzen. Unterschiede zwischen dem Geräte- und Hostmodus werden [hier](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode) erläutert.

   ![Herunterdrücken des Mikroschalters](media/iot-hub-intel-edison-kit-c-get-started/9_toggle_down_microswitch.jpg)

2. Stecken Sie das Micro-USB-Kabel in den oberen Micro-USB-Anschluss ein.

   ![Oberer Micro-USB-Anschluss](media/iot-hub-intel-edison-kit-c-get-started/10_top_usbport.jpg)

3. Stecken Sie das andere Ende des USB-Kabels in den Computer ein.

   ![USB-Anschluss des Computers](media/iot-hub-intel-edison-kit-c-get-started/11_computer_usb.jpg)

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

   ![Anschließen des Temperatursensors](media/iot-hub-intel-edison-kit-c-get-started/12_configuration_tool.png)

Glückwunsch! Sie haben Edison erfolgreich konfiguriert.

## <a name="run-a-sample-application-on-intel-edison"></a>Ausführen einer Beispielanwendung auf Intel Edison

### <a name="prepare-the-azure-iot-device-sdk"></a>Vorbereiten des Azure IoT-Geräte-SDK

1. Verwenden Sie einen der folgenden SSH-Clients auf Ihrem Hostcomputer, um die Verbindung mit dem Intel Edison herzustellen. Verwenden Sie dabei die IP-Adresse des Konfigurationstools und das Kennwort, das Sie in diesem Tool festgelegt haben.
    - [PuTTY](http://www.putty.org/) für Windows.
    - Den integrierten SSH-Client unter Ubuntu oder macOS.

2. Klonen Sie die Beispiel-Client-App auf Ihrem Gerät. 
   
   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-intel-edison-client-app.git
   ```

3. Navigieren Sie dann zum Repositoryordner, und führen Sie den folgenden Befehl zum Erstellen des Azure IoT-SDK aus.

   ```bash
   cd iot-hub-c-intel-edison-client-app
   sed -i -e 's/\r$//' buildSDK.sh
   chmod 755 buildSDK.sh
   ./buildSDK.sh
   ```

### <a name="configure-the-sample-application"></a>Konfigurieren der Beispielanwendung

1. Öffnen Sie die Konfigurationsdatei durch Ausführen der folgenden Befehle:

   ```bash
   nano config.h
   ```

   ![Konfigurationsdatei](media/iot-hub-intel-edison-kit-c-get-started/13_configure_file.png)

   Es gibt zwei Argumente in dieser Datei, die Sie konfigurieren können. Das erste ist `INTERVAL`, welches das Zeitintervall zwischen zwei Nachrichten bestimmt, die an die Cloud gesendet werden. Das zweite heißt `SIMULATED_DATA` und ist ein boolescher Wert, der bestimmt, ob simulierte Sensordaten verwendet werden sollen.

   Wenn Sie **keinen Sensor haben**, legen Sie den Wert von `SIMULATED_DATA` auf `1` fest, damit die Beispielanwendung simulierte Sensordaten erstellt und nutzt.

2. Speichern und beenden Sie durch Drücken von Control+O > EINGABETASTE > Control+X.

### <a name="build-and-run-the-sample-application"></a>Erstellen und Ausführen der Beispielanwendung

1. Erstellen Sie die Beispielanwendung durch Ausführen des folgenden Befehls:

   ```bash
   cmake . && make
   ```
   ![Erstellen der Ausgabe](media/iot-hub-intel-edison-kit-c-get-started/14_build_output.png)

1. Führen Sie die Beispielanwendung durch Aufrufen des folgenden Befehls aus:

   ```bash
   sudo ./app '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   Stellen Sie sicher, dass Sie die Verbindungszeichenfolge des Geräts kopieren und zwischen einfachen Anführungszeichen einfügen.

Die folgende Ausgabe sollte angezeigt werden, die die Sensordaten und Nachrichten zeigt, die an Ihren IoT Hub gesendet werden.

![Ausgabe: von Intel Edison an Ihren IoT Hub gesendete Sensordaten](media/iot-hub-intel-edison-kit-c-get-started/15_message_sent.png)

## <a name="next-steps"></a>Nächste Schritte

Sie haben eine Beispielanwendung ausgeführt, die Sensordaten sammelt und an Ihren IoT Hub sendet.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
