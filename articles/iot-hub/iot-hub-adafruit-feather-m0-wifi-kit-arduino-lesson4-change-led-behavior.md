---
title: "Verbinden von Arduino (C) mit Azure IoT – Lektion 4: Ändern der App | Microsoft-Dokumentation"
description: "Passen Sie die Nachrichten an, um das Ein- und Aus-Verhalten der LED zu ändern."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Steuern der LED mit Arduino
ms.assetid: d7a25430-450e-43c4-a3ed-1eed995b8b7e
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/13/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 64e69df256404e98f6175f77357500b562d74318
ms.openlocfilehash: 3585dfbac8816140c0a62931920aff1a6bf7d540


---
# <a name="change-the-on-and-off-behavior-of-the-led"></a>Ändern des Ein- und Aus-Verhaltens der LED
## <a name="what-you-will-do"></a>Aufgaben
Passen Sie die Nachrichten an, um das Ein- und Aus-Verhalten der LED zu ändern. Wenn Probleme auftreten, suchen Sie auf der [Problembehandlungsseite](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) nach Lösungen für Ihr Arduino-Board Adafruit Feather M0 WiFi.

## <a name="what-you-will-learn"></a>Sie lernen Folgendes
Verwenden zusätzlicher Arduino-Funktionen, um das Ein- und Aus-Verhalten der LED zu ändern

## <a name="what-you-need"></a>Erforderliches Element
Sie müssen [Ausführen einer Beispielanwendung auf dem Arduino-Board zum Empfangen von C2D-Nachrichten][receive-cloud-to-device-messages] erfolgreich abgeschlossen haben.

## <a name="add-functions-to-mainc-and-gulpfilejs"></a>Hinzufügen von Funktionen zu „main.c“ und „gulpfile.js“
1. Öffnen Sie die Beispielanwendung in Visual Studio Code, indem Sie die folgenden Befehle ausführen:

   ```bash
   cd Lesson4
   code .
   ```
2. Öffnen Sie die Datei `app.ino`, und fügen Sie dann hinter der blinkLED()-Funktion die folgenden Funktionen hinzu:

   ```arduino
   static void turnOnLED()
   {
     digitalWrite(LED_PIN, HIGH);
   }

   static void turnOffLED()
   {
     digitalWrite(LED_PIN, LOW);
   }
   ```

   ![Datei „app.ino“ mit hinzugefügten Funktionen][app-ino-file]
3. Fügen Sie vor dem `else if`-Block der `receiveMessageCallback`-Funktion die folgenden Bedingungen hinzu:

   ```arduino
   else if (strcmp((const char*)value, "\"on\"") == 0)
   {
       turnOnLED();
   }
   else if (0 == strcmp((const char*)value, "\"off\"") == 0)
   {
       turnOffLED();
   }
   ```

   Sie haben jetzt die Beispielanwendung für das Reagieren auf weitere Anweisungen durch Nachrichten konfiguriert. Mit der on-Anweisung wird die LED eingeschaltet, und mit der off-Anweisung wird sie ausgeschaltet.
4. Öffnen Sie die Datei „gulpfile.js“, und fügen Sie dann vor der `sendMessage`-Funktion eine neue Funktion hinzu:

   ```javascript
   var buildCustomMessage = function (messageId) {
     if ((messageId & 1) && (messageId < MAX_MESSAGE_COUNT)) {
       return new Message(JSON.stringify({ command: 'on', messageId: messageId }));
     } else if (messageId < MAX_MESSAGE_COUNT) {
       return new Message(JSON.stringify({ command: 'off', messageId: messageId }));
     } else {
       return new Message(JSON.stringify({ command: 'stop', messageId: messageId }));
     }
   };
   ```

   ![Datei „Gulpfile.js“ mit hinzugefügter Funktion][gulp-file-js]
5. Ersetzen Sie in der `sendMessage`-Funktion die Zeile `var message = buildMessage(sentMessageCount);` durch die neue Zeile, die im folgenden Codeausschnitt gezeigt wird:

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. Speichern Sie alle Änderungen.

### <a name="deploy-and-run-the-sample-application"></a>Bereitstellen und Ausführen der Beispielanwendung
Führen Sie den folgenden Befehl aus, um die Beispielanwendung auf dem Arduino-Board bereitzustellen und auszuführen:

```bash
gulp run
# You can monitor the serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

Die LED sollte zwei Sekunden lang eingeschaltet und dann zwei Sekunden lang ausgeschaltet sein. Mit der letzten Nachricht („stop“) wird die Beispielanwendung beendet.

![Ein und aus][on-and-off]

Glückwunsch! Sie haben die Nachrichten, die von IoT Hub an das Arduino-Board gesendet werden, erfolgreich angepasst.

### <a name="summary"></a>Zusammenfassung
In diesem optionalen Abschnitt wird gezeigt, wie Nachrichten angepasst werden, damit die Beispielanwendung das Ein- und Aus-Verhalten der LED auf andere Weise steuern kann.

<!-- Images and links -->

[receive-cloud-to-device-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson4-send-cloud-to-device-messages.md
[app-ino-file]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/updated_app_ino.png
[gulp-file-js]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/updated_gulpfile_js.png
[on-and-off]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/gulp_on_and_off_arduino.png


<!--HONumber=Jan17_HO4-->


