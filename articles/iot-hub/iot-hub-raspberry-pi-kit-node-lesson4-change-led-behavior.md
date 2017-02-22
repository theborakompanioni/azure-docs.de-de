---
title: "Verbinden von Raspberry Pi (Node) mit Azure IoT – Lektion 4: Ändern der App | Microsoft-Dokumentation"
description: "Passen Sie die Nachrichten an, um das Ein- und Aus-Verhalten der LED zu ändern."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: LED bei Raspberry Pi steuern, Raspberry Pi LED-Steuerung, Raspberry Pi LED steuern
ms.assetid: 3b42a4ad-0197-42f6-8ca9-04c883e879e8
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/28/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 64e69df256404e98f6175f77357500b562d74318
ms.openlocfilehash: 53c9408bedc6c61cdb0e755b46b3090e8723e271


---
# <a name="change-the-on-and-off-behavior-of-the-led"></a>Ändern des Ein- und Aus-Verhaltens der LED
## <a name="what-you-will-do"></a>Aufgaben
Passen Sie die Nachrichten an, um das Ein- und Aus-Verhalten der LED zu ändern. Bei Problemen können Sie sich die Lösungen auf der [Seite zur Problembehandlung](iot-hub-raspberry-pi-kit-node-troubleshooting.md) ansehen.

## <a name="what-you-will-learn"></a>Sie lernen Folgendes
Verwenden zusätzlicher Node.js-Funktionen, um das Ein- und Aus-Verhalten der LED ändern

## <a name="what-you-need"></a>Erforderliches Element
Sie müssen [Run a sample application on Raspberry Pi to receive cloud-to-device messages](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md) (Ausführen einer Beispielanwendung auf Raspberry Pi zum Empfangen von C2D-Nachrichten) erfolgreich abgeschlossen haben.

## <a name="add-nodejs-functions"></a>Hinzufügen von Node.js-Funktionen
1. Öffnen Sie die Beispielanwendung in Visual Studio Code, indem Sie die folgenden Befehle ausführen:
   
   ```bash
   cd Lesson4
   code .
   ```
2. Öffnen Sie die Datei `app.js` und fügen Sie dann am Ende die folgenden Funktionen hinzu:
   
   ```javascript
   function turnOnLED() {
     wpi.digitalWrite(CONFIG_PIN, 1);
   }
   
   function turnOffLED() {
     wpi.digitalWrite(CONFIG_PIN, 0);
   }
   ```
   
   ![Datei „App.js“ mit hinzugefügten Funktionen](media/iot-hub-raspberry-pi-lessons/lesson4/updated_app_js.png)
3. Fügen Sie im switch-case-Block der `receiveMessageCallback`-Funktion die folgenden Bedingungen vor der Standardbedingung hinzu:
   
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
   
   ![Datei „Gulpfile.js“ mit hinzugefügter Funktion](media/iot-hub-raspberry-pi-lessons/lesson4/updated_gulpfile.png)
5. Ersetzen Sie in der `sendMessage`-Funktion die Zeile `var message = buildMessage(sentMessageCount);` durch die neue Zeile, die im folgenden Codeausschnitt gezeigt wird:
   
   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. Speichern Sie alle Änderungen.

### <a name="deploy-and-run-the-sample-application"></a>Bereitstellen und Ausführen der Beispielanwendung
Führen Sie den folgenden Befehl aus, um die Beispielanwendung auf dem Pi bereitzustellen und auszuführen:

```bash
gulp deploy && gulp run
```

Die LED sollte zwei Sekunden lang eingeschaltet und dann zwei Sekunden lang ausgeschaltet sein. Mit der letzten Nachricht („stop“) wird die Beispielanwendung beendet.

![Beispielanwendung mit Ein- und Aus-Nachrichten](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_on_and_off.png)

Glückwunsch! Sie haben die Nachrichten, die von IoT Hub an den Pi gesendet werden, erfolgreich angepasst.

### <a name="summary"></a>Zusammenfassung
In diesem optionalen Abschnitt wird gezeigt, wie Nachrichten angepasst werden, damit die Beispielanwendung das Ein- und Aus-Verhalten der LED auf andere Weise steuern kann.




<!--HONumber=Jan17_HO4-->


