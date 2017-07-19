---
title: Erstellen eines Azure IoT Edge-Moduls mit Java | Microsoft-Dokumentation
description: In diesem Tutorial wird veranschaulicht, wie Sie ein BLE-Datenkonvertierungsmodul mit den neuesten Azure IoT Edge-Maven-Paketen schreiben.
services: iot-hub
author: junyi
manager: timlt
ms.service: iot-hub
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 06/28/2017
ms.author: junyi
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 682feb4d889ecd881abe1a70d36e0a5a4df3d910
ms.contentlocale: de-de
ms.lasthandoff: 07/04/2017

---
# <a name="create-an-azure-iot-edge-module-with-java"></a>Erstellen eines Azure IoT Edge-Moduls mit Java

In diesem Tutorial wird veranschaulicht, wie Sie ein Modul für Azure IoT Edge in Java erstellen können.

Es wird beschrieben, wie Sie die Umgebung einrichten und ein [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy)-Datenkonvertierungsmodul mit den neuesten Azure IoT Edge-Maven-Paketen schreiben.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Abschnitt richten Sie Ihre Umgebung für die Entwicklung des IoT Edge-Moduls ein. Dies gilt sowohl für Betriebssysteme vom Typ *Windows 64 Bit* als auch *Linux 64 Bit (Ubuntu/Debian 8)*.

Die folgende Software ist erforderlich:

* [Git-Client](https://git-scm.com/downloads).
* [**x64** JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* [Maven](https://maven.apache.org/install.html).

Öffnen Sie ein Befehlszeilenterminal-Fenster, und klonen Sie das folgende Repository:

1. `git clone https://github.com/Azure-Samples/iot-edge-samples.git`.
2. `cd iot-edge-samples/java/simulated_ble`

## <a name="overall-architecture"></a>Allgemeine Architektur

Die Azure IoT Edge-Plattform beruht maßgeblich auf der [Von-Neumann-Architektur](https://en.wikipedia.org/wiki/Von_Neumann_architecture). Dies bedeutet, dass die gesamte Azure IoT Edge-Architektur ein System darstellt, das Eingaben verarbeitet und Ausgaben erzeugt. Darüber hinaus fungiert jedes einzelne Modul auch als sehr kleines E/A-Subsystem. In diesem Tutorial stellen wir die folgenden beiden Module vor:

1. Ein Modul, das ein simuliertes [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy)-Signal empfängt und in eine formatierte [JSON](https://en.wikipedia.org/wiki/JSON)-Nachricht konvertiert.
2. Ein Modul, das die empfangene [JSON](https://en.wikipedia.org/wiki/JSON)-Nachricht ausgibt.

Die folgende Abbildung zeigt den typischen End-to-End-Datenfluss für dieses Projekt:

![Datenfluss zwischen drei Modulen](media/iot-hub-iot-edge-create-module/dataflow.png "Eingabe: Simuliertes BLE-Modul; Prozessor: Konvertierungsmodul; Ausgabe: Druckermodul")

## <a name="understanding-the-code"></a>Grundlegendes zum Code

### <a name="maven-project-structure"></a>Maven-Projektstruktur

Da Azure IoT Edge-Pakete auf Maven basieren, müssen wir eine typische Maven-Projektstruktur mit der Datei `pom.xml` erstellen.

POM erbt vom `com.microsoft.azure.gateway.gateway-module-base`-Paket, über das alle von einem Modulprojekt benötigten Abhängigkeiten deklariert werden, z.B. die Laufzeitbinärdateien, der Dateipfad für die Gatewaykonfiguration und das Ausführungsverhalten. Auf diese Weise sparen wir viel Zeit und können verhindern, dass Hunderte Codezeilen immer wieder neu geschrieben werden müssen.

Wir müssen die Datei „pom.xml“ aktualisieren, indem wir die erforderlichen Abhängigkeiten/Plug-Ins und den Namen der Konfigurationsdatei deklarieren, die von unserem Modul verwendet werden sollen. Dies ist im folgenden Codeausschnitt veranschaulicht.

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <!-- Inherit from parent -->
  <parent>
    <groupId>com.microsoft.azure.gateway</groupId>
    <artifactId>gateway-module-base</artifactId>
    <version>1.0.1</version>
  </parent>
  
  <groupId>com.microsoft.azure.gateway</groupId>
  <artifactId>ble-converter</artifactId>
  <version>1.0</version>
  <packaging>jar</packaging>

  <!-- Set the filename of the Azure IoT Edge configuration located
       under ./src/main/resources/gateway/ which is used in parent -->
  <properties>
    <gw.config.fileName>gw-config.json</gw.config.fileName>
  </properties>

  <!-- Re-declare dependencies used in parent -->
  <dependencies>
    <dependency>
      <groupId>com.microsoft.azure.gateway</groupId>
      <artifactId>gateway-java-binding</artifactId>
    </dependency>
    <dependency>
      <groupId>${dependency.runtime.group}</groupId>
      <artifactId>${dependency.runtime.name}</artifactId>
    </dependency>
  </dependencies>

  <!-- Re-declare plugins used in parent -->
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-dependency-plugin</artifactId>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-resources-plugin</artifactId>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-shade-plugin</artifactId>
      </plugin>
      <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>exec-maven-plugin</artifactId>
      </plugin>
    </plugins>
  </build>
</project>
```

### <a name="basic-understanding-of-an-azure-iot-edge-module"></a>Grundlagen eines Azure IoT Edge-Moduls

Ein Azure IoT Edge-Modul kann als Datenverarbeitungskomponente betrachtet werden, deren Aufgabe es ist, Eingaben zu empfangen, diese zu verarbeiten und Ausgaben zu erzeugen.

Bei den Eingaben kann es sich um Daten von Hardware (z.B. einem Bewegungsmelder), eine Meldung von anderen Modulen oder sonstige Eingaben (z.B. eine in regelmäßigen Abständen generierte Zufallszahl von einem Timer) handeln.

Die Ausgabe ist mit der Eingabe vergleichbar, da sie ein bestimmtes Hardwareverhalten (z.B. eine blinkende LED), eine Meldung an andere Module oder sonstige Ausgaben (z.B. die Ausgabe über die Konsole) auslösen kann.

Module kommunizieren mithilfe der `com.microsoft.azure.gateway.messaging.Message`-Klasse miteinander. Der **Inhalt** einer `Message` ist ein Bytearray, das beliebige von Ihnen gewünschte Daten darstellen kann. In der `Message` sind auch **Eigenschaften** vorhanden, die einfach eine Zuordnung von Zeichenfolgen darstellen. **Eigenschaften** können in Headern einer HTTP-Anforderung bzw. Metadaten einer Datei enthalten sein.

Zum Entwickeln eines Azure IoT Edge-Moduls in Java müssen Sie eine neue Modulklasse erstellen, die von `com.microsoft.azure.gateway.core.GatewayModule` erbt, und die erforderlichen abstrakten Methoden `receive()` und `destroy()` implementieren. An diesem Punkt können Sie festlegen, ob die optionalen Methoden `start()` oder `create()` implementiert werden soll. Der folgende Codeausschnitt veranschaulicht, wie Sie mit dem Erstellen eines Azure IoT Edge-Moduls beginnen.

```java
import com.microsoft.azure.gateway.core.Broker;
import com.microsoft.azure.gateway.core.GatewayModule;
import com.microsoft.azure.gateway.messaging.Message;

public class MyEdgeModule extends GatewayModule {
  public MyEdgeModule(long address, Broker broker, String configuration) {
    /* Let the GatewayModule do the dirty work of initialization. It's also
       a good time to parse your own configuration defined in Azure IoT Edge
       configuration file (typically ./src/main/resources/gateway/gw-config.json) */
    super(address, broker, configuration);
  }

  @Override
  public void start() {
    /* Acquire the resources you need. If you don't
       need any resources, you may omit this method. */
  }

  @Override
  public void destroy() {
    /* It's time to release all resources. This method is required. */
  }

  @Override
  public void receive(Message message) {
    /* Logic to process the input message. This method is required. */
    // ...
    /* Use publish() method to do the output. You are
       allowed to publish your new Message instance. */
    this.publish(message);
  }
}
```

### <a name="converter-module"></a>Konvertierungsmodul

| Eingabe                    | Prozessor                              | Ausgabe                 | Quelldatei            |
| ------------------------ | -------------------------------------- | ---------------------- | ---------------------- |
| Nachricht mit Temperaturdaten | Analysieren und Erstellen einer neuen JSON-Nachricht | Nachricht mit JSON-Struktur | `ConverterModule.java` |

Dieses Modul ist ein typisches Azure IoT Edge-Modul. Es akzeptiert Temperaturnachrichten von anderen Modulen (einem Hardwaremodul oder in diesem Fall unserem simulierten BLE-Modul) und normalisiert die Temperaturnachricht dann in eine strukturierte JSON-Nachricht (u.a. durch Anfügen der Nachrichten-ID, Festlegen der Eigenschaft dafür, ob die Temperaturwarnung ausgelöst werden muss, usw.).

```java
@Override
public void receive(Message message) {
  try {
    JSONObject messageFromBle = new JSONObject(new String(message.getContent()));
    double temperature = messageFromBle.getDouble("temperature");
    Map<String, String> inputProperties = message.getProperties();

    HashMap<String, String> properties = new HashMap<>();
    properties.put("source", inputProperties.get("source"));
    properties.put("macAddress", inputProperties.get("macAddress"));
    properties.put("temperatureAlert", temperature > 30 ? "true" : "false");

    String content = String.format(
        "{ \"deviceId\": \"Intel NUC Gateway\", \"messageId\": %d, \"temperature\": %f }",
        ++this.messageCount, temperature);

    this.publish(new Message(content.getBytes(), properties));
  } catch (Exception ex) {
    ex.printStackTrace();
  }
}
```

### <a name="printer-module"></a>Druckermodul

| Eingabe                          | Prozessor | Ausgabe                     | Quelldatei          |
| ------------------------------ | --------- | -------------------------- | -------------------- |
| Alle Nachrichten von anderen Modulen | –       | Protokollieren der Nachricht an die Konsole | `PrinterModule.java` |

Dies ist ein einfaches selbsterklärendes Modul, mit dem die empfangenen Nachrichten im Terminalfenster ausgegeben werden.

```java
@Override
public void receive(Message message) {
  System.out.println(message.toString());
}
```

### <a name="azure-iot-edge-configuration"></a>Azure IoT Edge-Konfiguration

Der letzte Schritt vor der Ausführung der Module ist die Konfiguration von Azure IoT Edge und die Herstellung der Verbindungen zwischen Modulen.

Zunächst müssen wir unser Java-Ladeprogramm deklarieren (da Azure IoT Edge Ladeprogramme für verschiedene Sprachen unterstützt), auf das in den nachfolgenden Abschnitten mit `name` verwiesen werden kann.

```json
"loaders": [{
  "type": "java",
  "name": "java",
  "configuration": {
    "jvm.options": {
      "library.path": "./"
    }
  }
}]
```

Nachdem wir unsere Ladeprogramme deklariert haben, müssen wir auch unsere Module deklarieren. Ähnlich wie bei der Deklaration der Ladeprogramme können sie ebenfalls mit dem Attribut `name` referenziert werden. Bei der Deklaration eines Moduls müssen wir für jedes Modul das Ladeprogramm (das wir zuvor definiert haben) und den Einstiegspunkt (sollte der normalisierte Klassenname unseres Moduls sein) angeben. Das `simulated_device`-Modul ist ein systemeigenes Modul, das im Azure IoT Edge-Core-Runtime-Paket enthalten ist. Sie sollten immer `args` in die JSON-Datei einfügen, auch wenn der Wert `null` lautet.

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
      "name": "java",
      "entrypoint": {
        "class.name": "com/microsoft/azure/gateway/ConverterModule",
        "class.path": "./ble-converter-1.0-with-deps.jar"
      }
    },
    "args": null
  },
  {
    "name": "print",
    "loader": {
      "name": "java",
      "entrypoint": {
        "class.name": "com/microsoft/azure/gateway/PrinterModule",
        "class.path": "./ble-converter-1.0-with-deps.jar"
      }
    },
    "args": null
  }
]
```

Am Ende der Konfiguration stellen wir die Verbindungen her. Jede Verbindung wird durch `source` und `sink` ausgedrückt. Sie sollten beide auf ein vordefiniertes Modul verweisen. Die Ausgabenachricht des Moduls `source` wird an die Eingabe des Moduls `sink` weitergeleitet.

```json
"links": [
  {
    "source": "simulated_device",
    "sink": "converter"
  },
  {
    "source": "converter",
    "sink": "print"
  }
]
```

## <a name="running-the-modules"></a>Ausführen der Module

Verwenden Sie `mvn package`, um alle Elemente im Ordner `target/` zu erstellen. `mvn clean package` wird ebenfalls empfohlen, um einen sauberen Build zu erhalten.

Verwenden Sie `mvn exec:exec`, um Azure IoT Edge auszuführen. Außerdem sollten Sie beobachten können, dass die Temperaturdaten und alle Eigenschaften mit einer feststehenden Rate auf der Konsole ausgegeben werden.

Wenn Sie die Anwendung beenden möchten, drücken Sie die Taste `<Enter>`.

> [!IMPORTANT]
> Es wird davon abgeraten, die IoT Edge-Gatewayanwendung mit STRG+C zu beenden. Denn auf diese Weise kann der Prozess unplanmäßig beendet werden.


