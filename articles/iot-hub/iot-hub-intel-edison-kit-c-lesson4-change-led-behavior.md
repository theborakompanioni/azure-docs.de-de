---
title: "Verbinden von Intel Edison (C) mit Azure IoT – Lektion 4: Aktivieren der LED-Blinkfunktion | Microsoft-Dokumentation"
description: "Passen Sie die Nachrichten an, um das Ein- und Aus-Verhalten der LED zu ändern."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Steuern der LED mit Arduino
ms.assetid: 9826c55a-0e24-4296-ae54-29b7fe66436a
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/8/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 475b25f02715a60493e79ecd2170854019dfc4ac
ms.openlocfilehash: 278bdf74e2fa8f7074bb8f5ed8eae2d47402b299


---
# <a name="change-the-on-and-off-behavior-of-the-led"></a>Ändern des Ein- und Aus-Verhaltens der LED
## <a name="what-you-will-do"></a>Aufgaben
Passen Sie die Nachrichten an, um das Ein- und Aus-Verhalten der LED zu ändern. Problemlösungen finden Sie auf der [Seite zur Problembehandlung][troubleshooting].

## <a name="what-you-will-learn"></a>Sie lernen Folgendes
Verwenden zusätzlicher Funktionen, um das Ein- und Aus-Verhalten der LED zu ändern

## <a name="what-you-need"></a>Erforderliches Element
Sie müssen [Ausführen einer Beispielanwendung auf dem Intel Edison zum Empfangen von C2D-Nachrichten][receive-cloud-to-device-messages] erfolgreich abgeschlossen haben.

## <a name="add-functions-to-mainc-and-gulpfilejs"></a>Hinzufügen von Funktionen zu „main.c“ und „gulpfile.js“
1. Öffnen Sie die Beispielanwendung in Visual Studio Code, indem Sie die folgenden Befehle ausführen:

   ```bash
   cd Lesson4
   code .
   ```
2. Öffnen Sie die Datei `main.c`, und fügen Sie dann hinter der blinkLED()-Funktion die folgenden Funktionen hinzu:

   ```c
   static void turnOnLED()
   {
     mraa_gpio_write(context, 1);
   }

   static void turnOffLED()
   {
     mraa_gpio_write(context, 0);
   }
   ```

   ![Datei „main.c“ mit hinzugefügten Funktionen](media/iot-hub-intel-edison-lessons/lesson4/updated_app_c.png)

3. Fügen Sie vor dem `else if`-Block der `receiveMessageCallback`-Funktion die folgenden Bedingungen hinzu:

   ```c
   else if (0 == strcmp((const char*)value, "\"on\""))
   {
       turnOnLED();
   }
   else if (0 == strcmp((const char*)value, "\"off\""))
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
   }
   ```

   ![Datei „Gulpfile.js“ mit hinzugefügter Funktion][gulpfile]
5. Ersetzen Sie in der `sendMessage`-Funktion die Zeile `var message = buildMessage(sentMessageCount);` durch die neue Zeile, die im folgenden Codeausschnitt gezeigt wird:

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. Speichern Sie alle Änderungen.

### <a name="deploy-and-run-the-sample-application"></a>Bereitstellen und Ausführen der Beispielanwendung
Führen Sie den folgenden Befehl aus, um die Beispielanwendung auf dem Edison bereitzustellen und auszuführen:

```bash
gulp deploy && gulp run
```

Die LED sollte zwei Sekunden lang eingeschaltet und dann zwei Sekunden lang ausgeschaltet sein. Mit der letzten Nachricht („stop“) wird die Beispielanwendung beendet.

![Ein und aus][on-and-off]

Glückwunsch! Sie haben die Nachrichten, die von IoT Hub an den Edison gesendet werden, erfolgreich angepasst.

### <a name="summary"></a>Zusammenfassung
In diesem optionalen Abschnitt wird gezeigt, wie Nachrichten angepasst werden, damit die Beispielanwendung das Ein- und Aus-Verhalten der LED auf andere Weise steuern kann.

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[receive-cloud-to-device-messages]: iot-hub-intel-edison-kit-c-lesson4-send-cloud-to-device-messages.md
[gulpfile]: media/iot-hub-intel-edison-lessons/lesson4/updated_gulpfile_c.png
[on-and-off]: media/iot-hub-intel-edison-lessons/lesson4/gulp_on_and_off_c.png



<!--HONumber=Jan17_HO4-->


