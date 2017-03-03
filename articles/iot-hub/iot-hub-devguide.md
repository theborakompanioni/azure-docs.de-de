---
title: "Entwicklerhandbuch für Azure IoT Hub | Microsoft Docs"
description: "Das Azure IoT Hub-Entwicklerhandbuch umfasst Erläuterungen zu Endpunkten, Sicherheit, Identitätsregistrierung, Geräteverwaltung, direkten Methoden, Gerätezwillingen, Dateiuploads, Aufträgen, die Abfragesprache von IoT Hub und Messaging."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: d534ff9d-2de5-4995-bb2d-84a02693cb2e
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/31/2017
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: 1915044f252984f6d68498837e13c817242542cf
ms.openlocfilehash: 4f12a4b3ba9c1d0b7ed10cf5d766bcea60205d24
ms.lasthandoff: 01/31/2017


---
# <a name="azure-iot-hub-developer-guide"></a>Entwicklungsleitfaden für Azure IoT Hub
Azure IoT Hub ist ein vollständig verwalteter Dienst, der eine zuverlässige und sichere bidirektionale Kommunikation zwischen Millionen von Geräten und einem Lösungs-Back-End ermöglicht.

Azure IoT Hub bietet Ihnen Folgendes:

* Eine sichere Kommunikation unter Verwendung von Zugriffssteuerung und Sicherheitsanmeldeinformationen auf Gerätebasis.
* Zahlreiche Optionen für hyperskalierbare Kommunikation zwischen Geräten und Cloud (Device-to-Cloud, D2C) sowie zwischen Cloud und Geräten (Cloud-to-Device, C2D)
* Abfragbarer Speicher mit Zustandsinformationen und Metdadaten auf Gerätebasis
* Eine problemlose Geräteanbindung mithilfe von Gerätebibliotheken für die gängigsten Sprachen und Plattformen

Dieses IoT Hub-Entwicklerhandbuch umfasst die folgenden Artikel:

* Unter [Senden und Empfangen von Nachrichten mit IoT Hub][devguide-messaging] werden die Messagingfunktionen (D2C und C2D) beschrieben, die mit IoT Hub zur Verfügung stehen. Der Artikel bietet auch Informationen zu Themen wie Nachrichtenweiterleitung, Nachrichtenformaten und unterstützten Kommunikationsprotokollen sowie den Portnummern, die diese verwenden.
* Die Informationen unter [Device-to-cloud communication guidance][lnk-d2c-guidance] (Leitfaden zur D2C-Kommunikation) erleichtern die Wahl zwischen D2C-Nachrichten, den gemeldeten Eigenschaften des Gerätezwillings und dem Dateiupload.
* Die Informationen unter [Cloud-to-device communication guidance][lnk-c2d-guidance] (Leitfaden zur C2D-Kommunikation) erleichtern die Wahl zwischen direkten Methoden, den gewünschten Eigenschaften des Gerätezwillings und C2D-Nachrichten.
* Unter [Upload files from a device][devguide-upload] (Hochladen von Dateien von einem Gerät) wird beschrieben, wie Sie Dateien von einem Gerät hochladen können. Dieser Artikel bietet auch Informationen zu Themen wie Benachrichtigungen, die beim Hochladevorgang gesendet werden können.
* Unter [Manage device identities in IoT Hub][devguide-identities] (Verwalten von Geräteidentitäten in IoT Hub) wird beschrieben, welche Informationen in jeder IoT Hub-Identitätsregistrierung gespeichert werden und wie Sie auf die Informationen zugreifen und sie ändern können.
* Unter [Verwalten des Zugriffs auf IoT Hub][devguide-security] wird das Sicherheitsmodell beschrieben, mit dem sowohl für Geräte als auch für Cloudkomponenten Zugriff auf IoT Hub-Funktionalität gewährt werden. Der Artikel enthält Informationen zum Verwenden von Token und X.509-Zertifikaten sowie Details zu den Berechtigungen, die Sie erteilen können.
* Unter [Grundlegendes zu Gerätezwillingen – Vorschau][devguide-device-twins] wird das Konzept von *Gerätezwillingen* und die damit verfügbar gemachte Funktionalität beschrieben, z.B. das Synchronisieren eines Geräts mit seinem Gerätezwilling. Der Artikel enthält Informationen zu den Daten, die in einem Gerätezwilling gespeichert sind.
* Unter [Aufrufen einer direkten Methode auf einem Gerät (Vorschau)][devguide-directmethods] werden der Lebenszyklus einer direkten Methode beschrieben, und es wird erläutert, wie Methoden auf einem Gerät von Ihrer Back-End-App aufgerufen und die direkte Methode auf Ihrem Gerät verarbeitet wird.
* Unter [Schedule jobs on multiple devices][devguide-jobs] (Planen von Aufträgen auf mehreren Geräten) wird beschrieben, wie Sie Aufträge auf mehreren Geräten planen können. Der Artikel erläutert das Übermitteln von Aufträgen zum Erledigen von Aufgaben wie dem Ausführen einer direkten Methode und Aktualisieren eines Geräts mithilfe eines Gerätezwillings. Außerdem wird erklärt, wie der Status eines Auftrags abgefragt wird.
* Unter [Reference - IoT Hub endpoints][devguide-endpoints] (Referenz – IoT Hub-Endpunkte) werden die verschiedenen Endpunkte beschrieben, die jeder IoT-Hub für Laufzeit- und Verwaltungsvorgänge bereitstellt. Außerdem wird beschrieben, wie Sie zusätzliche Endpunkte in Ihrem IoT Hub erstellen können, und wie Sie mit einem Feldgateway die Gerätekonnektivität mit Ihren IoT Hub-Endpunkten in nicht standardmäßigen Szenarien aktivieren.
* Unter [Referenz – Abfragesprache für Zwillinge und Aufträge][devguide-query] wird die IoT Hub-Abfragesprache beschrieben, mit der Sie von Ihrem Hub Informationen über Gerätezwillinge und Aufträge abrufen können.
* Unter [Reference - quotas and throttling][devguide-quotas] (Referenz – Kontingente und Drosselung) sind die im IoT Hub-Dienst festgelegten Kontingente samt Drosselungsverhalten zusammengefasst, die beim Überschreiten eines Kontingents erwartet werden können.
* [Referenz – Preise][devguide-pricing] enthält allgemeine Informationen zu verschiedenen SKUs und Preise für IoT Hub sowie Details zur Erfassung der verschiedenen IoT Hub-Funktionen als Nachrichten durch IoT Hub.
* Unter [Referenz – Geräte- und Dienst-SDKs][devguide-sdks] sind die Azure IoT SDKs aufgelistet, die Sie bei der Entwicklung von Geräte- und Dienst-Apps für die Interaktion mit Ihrer IoT Hub-Instanz verwenden können. Der Artikel enthält Links zur online verfügbaren API-Dokumentation.
* [IoT Hub-MQTT-Unterstützung][devguide-mqtt] enthält detaillierte Informationen zur Unterstützung des MQTT-Protokolls durch IoT Hub. Der Artikel beschreibt die Unterstützung des in die Azure IoT SDKs integrierten MQTT-Protokolls und bietet Informationen zur direkten Verwendung des MQTT-Protokolls.
* [Glossar][devguide-glossary]: Eine Liste der allgemeinen auf IoT Hub bezogenen Begriffe.

[devguide-messaging]: iot-hub-devguide-messaging.md
[devguide-upload]: iot-hub-devguide-file-upload.md
[devguide-identities]: iot-hub-devguide-identity-registry.md
[devguide-security]: iot-hub-devguide-security.md
[devguide-device-twins]: iot-hub-devguide-device-twins.md
[devguide-directmethods]: iot-hub-devguide-direct-methods.md
[devguide-jobs]: iot-hub-devguide-jobs.md
[devguide-endpoints]: iot-hub-devguide-endpoints.md
[devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[devguide-query]: iot-hub-devguide-query-language.md
[devguide-sdks]: iot-hub-devguide-sdks.md
[devguide-mqtt]: iot-hub-mqtt-support.md
[devguide-glossary]: iot-hub-devguide-glossary.md
[devguide-pricing]: iot-hub-devguide-pricing.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md


