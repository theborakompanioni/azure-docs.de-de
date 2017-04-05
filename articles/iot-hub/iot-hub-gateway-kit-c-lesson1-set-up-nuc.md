---
title: "SensorTag-Gerät und Azure IoT Gateway – Lektion 1: Einrichten von Intel NUC | Microsoft-Dokumentation"
description: Es wird beschrieben, wie Sie Intel NUC als IoT Gateway zwischen einem Sensor und Azure IoT Hub einrichten, um Sensorinformationen zu sammeln und an IoT Hub zu senden.
services: iot-hub
documentationcenter: 
author: shizn
manager: yjianfeng
tags: 
keywords: IoT Gateway, Intel NUC, NUC-Computer, DE3815TYKE
ms.assetid: 917090d6-35c2-495b-a620-ca6f9c02b317
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 432752c895fca3721e78fb6eb17b5a3e5c4ca495
ms.openlocfilehash: af2dde245fdef2984465f0c8447b558a2c770618
ms.lasthandoff: 03/30/2017


---
# <a name="set-up-intel-nuc-as-an-iot-gateway"></a>Einrichten von Intel NUC als IoT Gateway

## <a name="what-you-will-do"></a>Aufgaben

- Einrichten von Intel NUC als IoT Gateway
- Installieren des Azure IoT Gateway SDK-Pakets auf dem Intel NUC
- Ausführen einer „hello_world“-Beispielanwendung auf dem Intel NUC, um die Gatewayfunktionalität zu überprüfen
    
  > Problemlösungen finden Sie auf der [Seite zur Problembehandlung](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Sie lernen Folgendes

In dieser Lektion lernen Sie Folgendes:

- Herstellen der Verbindung mit einem Intel NUC mithilfe von Peripheriegeräten
- Installieren und Aktualisieren der erforderlichen Pakete auf dem Intel NUC mit dem Smart Package Manager
- Ausführen einer „hello_world“-Beispielanwendung, um die Gatewayfunktionalität zu überprüfen

## <a name="what-you-need"></a>Erforderliches Element

- Ein Intel NUC Kit DE3815TYKE mit der Intel IoT Gateway Software Suite (Wind River Linux *7.0.0.13) ist vorinstalliert. [Klicken Sie hier, um das Groove IoT Commercial Gateway Kit zu erwerben](https://www.seeedstudio.com/Grove-IoT-Commercial-Gateway-Kit-p-2724.html).
- Ein Ethernet-Kabel
- Eine Tastatur
- Ein HDMI- oder VGA-Kabel
- Ein Monitor mit HDMI- oder VGA-Anschluss
- Optional: [Texas Instruments Sensor Tag (CC2650STK)](http://www.ti.com/tool/cc2650stk)

![Gateway-Kit](media/iot-hub-gateway-kit-lessons/lesson1/kit.png)

## <a name="connect-intel-nuc-with-the-peripherals"></a>Verbinden des Intel NUC mit den Peripheriegeräten

Die folgende Abbildung ist ein Beispiel für einen Intel NUC, der mit verschiedenen Peripheriegeräten verbunden ist:

1. Verbindung mit einer Tastatur
2. Verbindung mit einem Monitor per VGA- oder HDMI-Kabel
3. Verbindung mit einem drahtgebundenen Netzwerk per Ethernet-Kabel
4. Verbindung mit einer Stromversorgung per Netzkabel

![Verbindung des Intel NUC mit Peripheriegeräten](media/iot-hub-gateway-kit-lessons/lesson1/nuc.png)

## <a name="connect-to-the-intel-nuc-system-from-host-computer-via-secure-shell-ssh"></a>Herstellen der Verbindung mit dem Intel NUC-System vom Hostcomputer aus per Secure Shell (SSH)

Sie benötigen eine Tastatur und einen Monitor, um die IP-Adresse Ihres Intel NUC-Geräts abzurufen. Falls Ihnen die IP-Adresse bereits bekannt ist, können Sie mit Schritt 3 dieses Abschnitts fortfahren.

1. Schalten Sie das Intel NUC ein, indem Sie den Netzschalter betätigen, und melden Sie sich anschließend an.

   Standardbenutzername und -kennwort lauten jeweils `root`.

       > Hit the enter key on your keyboard if you see either of the following errors when you boot: 'A TPM error (7) occurred attempting to read a pcr value.' or 'Timeout, No TPM chip found, activating TPM-bypass!'

2. Rufen Sie die IP-Adresse des Intel NUC durch Ausführen des Befehls `ifconfig` auf dem Intel NUC-Gerät ab.

   Hier ist ein Beispiel für die Befehlsausgabe angegeben:

   ![Ausgabe von „ifconfig“ mit Intel NUC-IP-Adresse](media/iot-hub-gateway-kit-lessons/lesson1/ifconfig.png)

   In diesem Beispiel folgt auf `inet addr:` die IP-Adresse, die Sie benötigen, wenn Sie auf einem Hostcomputer eine Verbindung mit dem Intel NUC-Gerät herstellen.

3. Verwenden Sie einen der folgenden SSH-Clients auf Ihrem Hostcomputer, um die Verbindung mit dem Intel NUC herzustellen.

    - [PuTTY](http://www.putty.org/) für Windows.
    - Den integrierten SSH-Client unter Ubuntu oder macOS.

   Es ist effizienter und produktiver, ein Intel NUC von einem Hostcomputer aus zu betreiben. Sie benötigen die IP-Adresse, den Benutzernamen und das Kennwort des Intel NUC, um über einen SSH-Client eine Verbindung damit herzustellen. Es folgt ein Beispiel, bei dem ein SSH-Client unter macOS verwendet wird.
   ![SSH-Client unter MacOS](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)

## <a name="install-the-azure-iot-gateway-sdk-package"></a>Installieren des Azure IoT Gateway SDK-Pakets

Das Azure IoT Gateway SDK-Paket enthält die vorkompilierten Binärdateien des SDK und die jeweiligen Abhängigkeiten. Diese Binärdateien sind das Azure IoT Gateway SDK, das Azure IoT SDK und die entsprechenden Tools. Das Paket enthält auch eine „hello_world“-Beispielanwendung, die zum Überprüfen der Gatewayfunktionalität verwendet wird. Das SDK ist der Kern des Gateways. 

Führen Sie diese Schritte aus, um das Paket zu installieren.

1. Fügen Sie das IoT-Cloudrepository hinzu, indem Sie die folgenden Befehle in einem Terminalfenster ausführen:

   ```bash
   rpm --import http://iotdk.intel.com/misc/iot_pub.key
   smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
   ```

   Mit dem Befehl `rpm` wird der rpm-Schlüssel importiert. Mit dem Befehl `smart channel` wird der rpm-Kanal dem Smart Package Manager hinzugefügt. Bevor Sie den Befehl `smart update` ausführen, wird eine Ausgabe wie die folgende angezeigt.

   ![Ausgabe der Kanalbefehle „rpm“ und „smart“](media/iot-hub-gateway-kit-lessons/lesson1/rpm_smart_channel.png)

2. Führen Sie den intelligenten Updatebefehl aus:

   ```bash
   smart update
   ```

3. Führen Sie den folgenden Befehl aus, um das Azure IoT Gateway-Paket zu installieren:

   ```bash
   smart install packagegroup-cloud-azure -y
   ```

   `packagegroup-cloud-azure` ist der Name des Pakets. Der Befehl `smart install` wird zum Installieren des Pakets verwendet.


    > Führen den folgenden Befehl ein, wenn dieser Fehler angezeigt wird: „Öffentlicher Schlüssel nicht verfügbar“.

    ```bash
    smart config --set rpm-check-signatures=false
    smart install packagegroup-cloud-azure -y
    ```
   
   Nach dem Installieren des Pakets ist das Intel NUC bereit, als Gateway zu fungieren.

## <a name="run-the-azure-iot-gateway-sdk-helloworld-sample-application"></a>Ausführen der „hello_world“-Beispielanwendung des Azure IoT Gateway SDK

Bei der folgenden Beispielanwendung wird ein Gateway aus der Datei `hello_world.json` erstellt, und die grundlegenden Komponenten der Azure IoT Gateway SDK-Architektur werden verwendet, um alle fünf Sekunden eine „hello world“-Nachricht in einer Datei (log.txt) zu protokollieren.

Sie können das „Hello World“-Beispiel ausführen, indem Sie die folgenden Befehle aufrufen:

```bash
cd /usr/share/azureiotgatewaysdk/samples/hello_world/
./hello_world hello_world.json
```

Führen Sie die „Hello World“-Anwendung einige Minuten aus, und drücken Sie dann die EINGABETASTE, um sie zu beenden.
![Anwendungsausgabe](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)

> Sie können alle Fehler des Typs „Ungültiges Argumenthandle (NULL)“ ignorieren, die nach Drücken der EINGABETASTE angezeigt werden.

Sie können überprüfen, ob das Gateway erfolgreich ausgeführt wurde, indem Sie die „Datei log.txt“ öffnen, die nun in Ihrem Ordner „hello_world“ enthalten ist: ![Verzeichnisansicht von „log.txt“](media/iot-hub-gateway-kit-lessons/lesson1/logtxtdir.png)

Öffnen Sie „log.txt“ mit dem folgenden Befehl:

```bash
vim log.txt
```

Angezeigt wird der Inhalt von „log.txt“, bei dem es sich um eine Ausgabe der Protokollierungsmeldungen im JSON-Format handelt, die vom „Hello World-“Modul des Gateways alle fünf Sekunden geschrieben wurden.
![Verzeichnisansicht von „log.txt“](media/iot-hub-gateway-kit-lessons/lesson1/logtxtview.png)

Problemlösungen finden Sie auf der [Seite zur Problembehandlung](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="summary"></a>Zusammenfassung

Glückwunsch! Sie haben die Einrichtung des Intel NUC als Gateway abgeschlossen. Sie können jetzt mit der nächsten Lektion fortfahren, um den Hostcomputer einzurichten, einen Azure IoT Hub zu erstellen und das logische Azure IoT Hub-Gerät zu registrieren.

## <a name="next-steps"></a>Nächste Schritte
[Vorbereiten des Hostcomputers und von Azure IoT Hub](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)

