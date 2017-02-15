---
title: "Ändern des LED-Blinkverhaltens mit Nachrichten von Azure IoT Hub | Microsoft Docs"
description: "Passen Sie die Nachrichten an, um das Ein- und Aus-Verhalten der LED zu ändern."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Steuern der LED mit Arduino
ms.assetid: 387cd97e-b05e-43c4-b252-f68ad45d524a
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/8/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 1e892cee7b977c03b2539a6495b868cd8ab83748
ms.openlocfilehash: 69cfb48789ef72d6594bd3db68170f83c9d171b0


---
# <a name="change-the-on-and-off-behavior-of-the-led"></a>Ändern des Ein- und Aus-Verhaltens der LED
## <a name="what-you-will-do"></a>Aufgaben
Passen Sie die Nachrichten an, um das Ein- und Aus-Verhalten der LED zu ändern. Problemlösungen finden Sie auf der [Seite zur Problembehandlung][troubleshooting].

## <a name="what-you-will-learn"></a>Sie lernen Folgendes
Verwenden zusätzlicher Funktionen, um das Ein- und Aus-Verhalten der LED zu ändern

## <a name="what-you-need"></a>Erforderliches Element
Sie müssen [Ausführen einer Beispielanwendung auf dem Intel Edison zum Empfangen von C2D-Nachrichten][receive-cloud-to-device-messages] erfolgreich abgeschlossen haben.

## <a name="add-functions-to-appjs-and-gulpfilejs"></a>Hinzufügen von Funktionen zu „app.js“ und „gulpfile.js“
1. Öffnen Sie die Beispielanwendung in Visual Studio Code, indem Sie die folgenden Befehle ausführen:

   ```bash
   cd Lesson4
   code .
   ```
2. Öffnen Sie die Datei `app.js`, und fügen Sie dann hinter der blinkLED()-Funktion die folgenden Funktionen hinzu:

   ```javascript
   function turnOnLED() {
     myLed.write(1);
   }

   function turnOffLED() {
     myLed.write(0);
   }
   ```

   ![Datei „app.js“ mit hinzugefügten Funktionen](media/iot-hub-intel-edison-lessons/lesson4/updated_app_node.png)
3. Fügen Sie im switch-case-Block der `receiveMessageCallback`-Funktion die folgenden Bedingungen vor der Bedingung „case“ hinzu:

   ```javascript
   case 'on':
     turnOnLED();
     break;
   case 'off':
     turnOffLED();
     break;
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

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[receive-cloud-to-device-messages]: iot-hub-intel-edison-kit-node-lesson4-send-cloud-to-device-messages.md
[gulpfile]: media/iot-hub-intel-edison-lessons/lesson4/updated_gulpfile_node.png
[on-and-off]: media/iot-hub-intel-edison-lessons/lesson4/gulp_on_and_off_node.png



<!--HONumber=Dec16_HO2-->


