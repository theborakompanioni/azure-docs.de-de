---
title: Erstellen eines Azure IoT Edge-Moduls mit Node.js | Microsoft-Dokumentation
description: Dieses Tutorial veranschaulicht, wie Sie ein BLE-Datenkonvertierungsmodul mit den neuesten Azure IoT Edge-NPM-Paketen und dem Yeoman-Generator schreiben.
services: iot-hub
author: sushi
manager: timlt
ms.service: iot-hub
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: js
ms.topic: article
ms.date: 06/28/2017
ms.author: sushi
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: 12e0702c31f36368fd95127abeede1a0d4ff2c87
ms.contentlocale: de-de
ms.lasthandoff: 07/01/2017

---
# <a name="create-an-azure-iot-edge-module-with-nodejs"></a>Erstellen eines Azure IoT Edge-Moduls mit Node.js

In diesem Tutorial wird gezeigt, wie Sie ein Modul für Azure IoT Edge in JS erstellen.

In diesem Tutorial zeigen wir Ihnen, wie Sie eine Umgebung einrichten und ein [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy)-Datenkonvertierungsmodul mit den neuesten Azure IoT Edge-NPM-Paketen schreiben.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Abschnitt richten Sie Ihre Umgebung für die Entwicklung des IoT Edge-Moduls ein. Dies gilt sowohl für das Betriebssystem *64-Bit-Windows* als auch für *64-Bit-Linux (Ubuntu 14+)*.

Die folgende Software ist erforderlich:
* [Git-Client](https://git-scm.com/downloads)
* [Node LTS](https://nodejs.org)
* `npm install -g yo`.
* `npm install -g generator-az-iot-gw-module`

## <a name="architecture"></a>Architektur

Die Azure IoT Edge-Plattform beruht maßgeblich auf der [Von-Neumann-Architektur](https://en.wikipedia.org/wiki/Von_Neumann_architecture). Dies bedeutet, dass die gesamte Azure IoT Edge-Architektur ein System darstellt, das Eingaben verarbeitet und Ausgaben erzeugt. Darüber hinaus fungiert jedes einzelne Modul als kleines E/A-Subsystem. In diesem Tutorial stellen wir die folgenden zwei Module vor:

1. Ein Modul, das ein simuliertes [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy)-Signal empfängt und in eine formatierte [JSON](https://en.wikipedia.org/wiki/JSON)-Nachricht konvertiert.
2. Ein Modul, das die empfangene [JSON](https://en.wikipedia.org/wiki/JSON)-Nachricht druckt.

Die folgende Abbildung zeigt den typischen End-to-End-Datenfluss für dieses Projekt:

![Datenfluss zwischen drei Modulen](media/iot-hub-iot-edge-create-module/dataflow.png "Eingabe: Simuliertes BLE-Modul; Verarbeitungskomponente: Konvertierungsmodul; Ausgabe: Druckermodul")

## <a name="set-up-the-environment"></a>Einrichten der Umgebung
Im folgenden erfahren Sie, wie Sie schnell eine Umgebung einrichten, um mit dem Schreiben Ihres ersten BLE-Konvertierungsmoduls mit JS zu beginnen.

### <a name="create-module-project"></a>Erstellen eines Modulprojekts
1. Öffnen Sie ein Befehlszeilenfenster und führen Sie `yo az-iot-gw-module` aus.
2. Führen Sie die auf dem Bildschirm angezeigten Schritte durch, um die Initialisierung Ihres Modulprojekts fertig zu stellen.

### <a name="project-structure"></a>Projektstruktur
Ein JS-Modulprojekt besteht aus folgenden Komponenten:

`modules` – Die angepassten JS-Modulquelldateien. Ersetzen Sie die Standardmodule `sensor.js` und `printer.js` durch Ihre eigenen Moduldateien.

`app.js` – Die Eingabedatei zum Starten der Edge-Instanz.

`gw.config.json` – Die Konfigurationsdatei zum Anpassen der von Edge zu ladenden Module.

`package.json` – Die Metadateninformationen für das Modulprojekt.

`README.md` – Die grundlegende Dokumentation für das Modulprojekt.


### <a name="package-file"></a>Paketdatei

Die Datei `package.json` deklariert alle für ein Modulprojekt erforderliche Metadateninformationen, wie Name, Version, Eintrag, Skripts, Runtime und Entwicklungsabhängigkeiten.

Der folgende Codeausschnitt zeigt, welche Konfiguration Sie für ein BLE-Konvertierungsbeispielprojekt vornehmen.
```json
{
  "name": "converter",
  "version": "1.0.0",
  "description": "BLE data converter sample for Azure IoT Edge.",
  "repository": {
    "type": "git",
    "url": "https://github.com/Azure-Samples/iot-edge-samples"
  },
  "main": "app.js",
  "scripts": {
    "start": "node app.js"
  },
  "author": "Microsoft Corporation",
  "license": "MIT",
  "dependencies": {
  },
  "devDependencies": {
    "azure-iot-gateway": "~1.1.3"
  }
}
```


### <a name="entry-file"></a>Eingabedatei
Die Datei `app.js` bestimmt, wie die Edge-Instanz initialisiert wird. In diesem Fall sind keine Änderungen erforderlich.

```javascript
(function() {
  'use strict';

  const Gateway = require('azure-iot-gateway');
  let config_path = './gw.config.json';

  // node app.js
  if (process.argv.length < 2) {
    throw 'Calling pattern should be node app.js.';
  }

  const gw = new Gateway(config_path);
  gw.run();
})();
```

### <a name="interface-of-module"></a>Oberfläche des Moduls
Ein Azure IoT Edge-Modul kann als Datenverarbeitungskomponente betrachtet werden, deren Aufgabe es ist, Eingaben zu empfangen, diese zu verarbeiten und Ausgaben zu erzeugen.

Bei den Eingaben kann es sich um Daten von Hardware (z.B. eines Bewegungsmelders), eine Nachricht von anderen Modulen oder sonstige Eingaben (z.B. eine in regelmäßigen Abständen generierte Zufallszahl von einem Timer) handeln.

Die Ausgabe ist mit der Eingabe vergleichbar, da sie ein bestimmtes Hardwareverhalten (z.B. eine blinkende LED), eine Nachricht an andere Module oder sonstige Ausgaben (z.B. einen Druckvorgang über die Konsole) auslösen kann.

Module kommunizieren über das Objekt `message` miteinander. Der **Inhalt** einer `message` ist ein Bytearray, das beliebige von Ihnen gewünschte Daten darstellen kann. In der `message` sind auch **Eigenschaften** vorhanden, die einfach eine Zuordnung von Zeichenfolgen darstellen. **Eigenschaften** können in Headern einer HTTP-Anforderung oder in den Metadaten einer Datei enthalten sein.

Um ein Azure IoT Edge-Modul in JS entwickeln zu können, müssen Sie ein neues Modulobjekt erstellen, das die erforderlichen `receive()`-Methoden implementiert. An dieser Stelle können Sie auch festlegen, dass die optionalen Methoden `create()`, `start()` oder `destroy()` implementiert werden soll. Der folgende Codeausschnitt zeigt die Grundstruktur des JS-Modulobjekts.

```javascript
'use strict';

module.exports = {
  broker: null,
  configuration: null,

  create: function (broker, configuration) {
    // Default implementation.
    this.broker = broker;
    this.configuration = configuration;

    return true;
  },

  start: function () {
    // Produce
  },

  receive: function (message) {
    // Consume
  },

  destroy: function () {
  }
};
```

### <a name="converter-module"></a>Konvertierungsmodul
| Eingabe                    | Prozessor                              | Ausgabe                 | Quelldatei            |
| ------------------------ | -------------------------------------- | ---------------------- | ---------------------- |
| Nachricht mit Temperaturdaten | Analysieren und Erstellen einer neuen JSON-Nachricht | Nachricht mit JSON-Struktur | `converter.js` |

Dieses Modul ist ein typisches Azure IoT Edge-Modul. Es akzeptiert Temperaturnachrichten von anderen Modulen (einem Hardwaremodul oder in diesem Fall unserem simulierten BLE-Modul) und normalisiert die Temperaturnachricht dann in eine strukturierte JSON-Nachricht (u.a. durch Anfügen der Nachrichten-ID, Festlegen der Eigenschaft, ob die Temperaturwarnung ausgelöst werden muss etc.).

```javascript
receive: function (message) {
  // Initialize the messageCount in global object at first time.
  if (!global.messageCount) {
    global.messageCount = 0;
  }

  // Read the content and properties objects from message.
  let rawContent = JSON.parse(Buffer.from(message.content).toString('utf8'));
  let rawProperties = message.properties;

  // Generate new properties object.
  let newProperties = {
    source: rawProperties.source,
    macAddress: rawProperties.macAddress,
    temperatureAlert: rawContent.temperature > 30 ? 'true' : 'false'
  };

  // Generate new content object.
  let newContent = {
    deviceId: 'Intel NUC Gateway',
    messageId: ++global.messageCount,
    temperature: rawContent.temperature
  };

  // Publish the new message to broker.
  this.broker.publish(
    {
      properties: newProperties,
      content: new Uint8Array(Buffer.from(JSON.stringify(newContent), 'utf8'))
    }
  );
},
```

### <a name="printer-module"></a>Druckermodul
| Eingabe                          | Prozessor | Ausgabe                     | Quelldatei          |
| ------------------------------ | --------- | -------------------------- | -------------------- |
| Alle Nachrichten von anderen Modulen | –       | Protokollieren der Nachricht an die Konsole | `printer.js` |

Dieses Modul ist einfach und selbsterklärend – es gibt die empfangenen Nachrichten (Eigenschaft, Inhalt) im Terminal-Fenster aus.

```javascript
receive: function (message) {
  let properties = JSON.stringify(message.properties);
  let content = Buffer.from(message.content).toString('utf8');

  console.log(`printer.receive.properties - ${properties}`);
  console.log(`printer.receive.content - ${content}\n`);
}
```

### <a name="configuration"></a>Konfiguration
Der letzte Schritt vor der Ausführung der Module ist die Konfiguration von Azure IoT Edge und die Herstellung der Verbindungen zwischen Modulen.

Zunächst müssen wir unser `node`-Ladeprogramm deklarieren (da Azure IoT Edge Ladeprogramme für verschiedene Sprachen unterstützt), das durch `name` in den nachfolgenden Abschnitten referenziert werden kann.

```json
"loaders": [
  {
    "type": "node",
    "name": "node"
  }
]
```

Nachdem wir unsere Ladeprogramme deklariert haben, müssen wir auch unsere Module deklarieren. Ähnlich wie bei der Deklaration der Ladeprogramme können sie ebenfalls durch das Attribut `name` referenziert werden. Bei der Deklaration eines Moduls müssen wir für jedes Modul das Ladeprogramm (das wir zuvor definiert haben) und den Einstiegspunkt (sollte der normalisierte Klassenname unseres Moduls sein) angeben. Das `simulated_device`-Modul ist ein systemeigenes Modul, das im Azure IoT Edge-Core-Runtime-Paket enthalten ist. Fügen Sie `args` in die JSON-Datei ein, auch wenn der Wert `null` lautet.

```json
"modules": [
  {
    "name": "simulated_device",
    "loader": {
      "name": "native",
      "entrypoint": {
        "module.path": "simulated_device"
      }
    },
    "args": {
      "macAddress": "01:02:03:03:02:01",
      "messagePeriod": 500
    }
  },
  {
    "name": "converter",
    "loader": {
      "name": "node",
      "entrypoint": {
        "main.path": "modules/converter.js"
      }
    },
    "args": null
  },
  {
    "name": "printer",
    "loader": {
      "name": "node",
      "entrypoint": {
        "main.path": "modules/printer.js"
      }
    },
    "args": null
  }
]
```

Am Ende der Konfiguration stellen wir die Verbindungen her. Jede Verbindung wird durch `source` und `sink` ausgedrückt. Sie sollten beide ein vordefiniertes Modul referenzieren. Die Ausgabenachricht des Moduls `source` wird an die Eingabe des Moduls `sink` weitergeleitet.

```json
"links": [
  {
    "source": "simulated_device",
    "sink": "converter"
  },
  {
    "source": "converter",
    "sink": "printer"
  }
]
```

## <a name="running-the-modules"></a>Ausführen der Module
1. `npm install`
2. `npm start`

Wenn Sie die Anwendung beenden möchten, drücken Sie die Taste `<Enter>`.

> [!IMPORTANT]
> Es wird davon abgeraten, die IoT Edge-Anwendung über STRG + C zu beenden. Denn auf diese Weise kann der Prozess unplanmäßig beendet werden.

