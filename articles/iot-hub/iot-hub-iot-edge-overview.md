---
title: "Übersicht über Azure IoT Edge | Microsoft-Dokumentation"
description: Beschreibt die wichtigsten Architekturkonzepte in Azure IoT Edge, wie z.B. Gateways, Module und Broker.
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/02/2017
ms.author: dobett
ms.translationtype: Human Translation
ms.sourcegitcommit: 9edcaee4d051c3dc05bfe23eecc9c22818cf967c
ms.openlocfilehash: ecdd56c91a8fc2011b3d7abe93b9d27c1e1e0bef
ms.contentlocale: de-de
ms.lasthandoff: 06/08/2017


---
# <a name="azure-iot-edge-architectural-concepts"></a>Architekturkonzepte in Azure IoT Edge

Bevor Sie alle Beispielcodes untersuchen oder selbst mithilfe von IoT Edge ein Bereichsgateway erstellen, sollten Sie die wichtigsten Konzepte kennen, die der IoT Edge-Architektur zugrunde liegen.

## <a name="iot-edge-modules"></a>IoT Edge-Module

Sie erstellen ein Gateway mit Azure IoT Edge, indem Sie *IoT Edge-Module* erstellen und zusammenstellen. Module verwenden *Nachrichten*, um untereinander Daten auszutauschen. Ein Modul empfängt eine Nachricht, führt eine Aktion damit aus, transformiert sie optional in eine neue Nachricht und veröffentlicht sie dann zur Verarbeitung an andere Module, Einige Module erzeugen nur neue Nachrichten und verarbeiten niemals eingehende Nachrichten. Eine Modulkette erstellt eine Datenverarbeitungspipeline, wobei jedes Modul an einem bestimmten Punkt dieser Pipeline eine Transformation der Daten ausführt.

![Eine Modulkette im Gateway, die mit Azure IoT Edge erstellt wurde][1]

IoT Edge umfasst folgende Komponenten:

* Vordefinierte Module, die allgemeine Gatewayfunktionen ausführen.
* Die Schnittstellen, die ein Entwickler zum Schreiben benutzerdefinierter Module verwenden kann.
* Die Infrastruktur, die zur Bereitstellung und Ausführung eines Modulsatzes erforderlich ist.

Das SDK stellt eine Abstraktionsschicht bereit, mit deren Hilfe Sie Gateways erstellen können, die auf verschiedenen Betriebssystemen und Plattformen ausgeführt werden können.

![Azure IoT Edge-Abstraktionsschicht][2]

## <a name="messages"></a>Meldungen

Die Vorstellung, dass Module einander Nachrichten übergeben, ist eine praktische Möglichkeit, die Funktionsweise eines Gateways zu verstehen, spiegelt stellt den tatsächlichen Vorgang aber nicht exakt wider. IoT Edge-Module verwenden einen Broker für die Kommunikation miteinander. Module veröffentlichen Nachrichten für den Broker (Nachrichtenmuster, z.B. Bus oder Veröffentlichen/Abonnieren), und lassen den Broker anschließend die Nachrichten an die mit ihm verbundenen Module weiterleiten.

Ein Modul veröffentlicht über die **Broker_Publish**-Funktion eine Nachricht für den Broker. Der Broker übermittelt Nachrichten durch Aufrufen einer Rückruffunktion an ein Modul. Eine Nachricht besteht aus einem Satz aus Schlüssel-Wert-Eigenschaften und Inhalten, der als Arbeitsspeicherblock übergeben wird.

![Die Rolle des Brokers in Azure IoT Edge][3]

## <a name="message-routing-and-filtering"></a>Nachrichtenrouting und -filterung

Nachrichten können auf zwei Arten an die richtigen IoT Edge-Module weitergeleitet werden:

* Sie können eine Reihe von Links an den Broker übergeben, damit der Broker die Quelle und Senke für jedes Modul kennt.
* Ein Modul kann nach den Eigenschaften der Nachricht gefiltert werden.

Ein Modul soll nur auf Nachrichten reagieren, die für das Modul bestimmt sind. Die Links und Nachrichtenfilter bilden im Grunde eine Nachrichtenpipeline.

## <a name="next-steps"></a>Nächste Schritte

Unter [Untersuchen der Azure IoT Edge-Architektur][lnk-hello-world] finden Sie ein Beispiel, das Sie ausführen können, in dem diese Konzepte angewendet werden.

<!-- Images -->
[1]: media/iot-hub-iot-edge-overview/modules.png
[2]: media/iot-hub-iot-edge-overview/modules_2.png
[3]: media/iot-hub-iot-edge-overview/messages_1.png

<!-- Links -->
[lnk-hello-world]: ./iot-hub-linux-iot-edge-get-started.md
