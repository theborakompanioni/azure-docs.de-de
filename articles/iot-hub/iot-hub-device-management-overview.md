---
title: "Übersicht über die Geräteverwaltung mit IoT Hub| Microsoft Docs"
description: "Dieser Artikel bietet anhand folgender Aspekte einen Überblick über die Geräteverwaltung in Azure IoT Hub: Lebenszyklus von Unternehmensgeräten, Neustart, Zurücksetzung auf Werkseinstellungen, Firmwareupdate, Konfiguration, Gerätezwillinge, Abfragen und Aufträge."
services: iot-hub
documentationcenter: 
author: bzurcher
manager: timlt
editor: 
ms.assetid: a367e715-55f6-4593-bd68-7863cbf0eb81
ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/03/2016
ms.author: bzurcher
translationtype: Human Translation
ms.sourcegitcommit: 00746fa67292fa6858980e364c88921d60b29460
ms.openlocfilehash: 4a80d22f0d306035dc3e710d9a1b04f0624e94d6


---
# <a name="overview-of-device-management-with-iot-hub"></a>Übersicht über die Geräteverwaltung mit IoT Hub
## <a name="introduction"></a>Einführung
Auf der Grundlage der Features und des Erweiterungsmodells von Azure IoT Hub können Geräte- und Back-End-Entwickler stabile Geräteverwaltungslösungen entwickeln. Das Spektrum der Geräte reicht von einfachen Sensoren und zweckgebundenen Microcontrollern bis hin zu leistungsfähigen Gateways zur Weiterleitung der Kommunikation für Gerätegruppen.  Die Anwendungsfälle und Anforderungen für IoT-Betreiber sind außerdem sehr branchenspezifisch.  Trotz dieser Vielfalt bietet die Geräteverwaltung mit Azure IoT Hub Funktionen, Muster und Codebibliotheken für unterschiedlichste Geräte und Endbenutzer.

Ein entscheidender Punkt bei der Erstellung einer erfolgreichen IoT-Unternehmenslösung ist die Entwicklung einer Strategie, mit der Bediener die kontinuierliche Verwaltung ihrer Geräte bewältigen können. IoT-Bediener benötigen unkomplizierte, zuverlässige Tools und Anwendungen, die es ihnen ermöglichen, sich ganz auf die strategischen Aspekte ihrer Arbeit zu konzentrieren. Dieser Artikel enthält Folgendes:

* Eine kurze Übersicht über die Geräteverwaltung von Azure IoT Hub
* Eine Beschreibung allgemeiner Geräteverwaltungsprinzipien
* Eine Beschreibung des Gerätelebenszyklus
* Eine Übersicht über allgemeine Geräteverwaltungsmuster

## <a name="device-management-principles"></a>Geräteverwaltungsprinzipien
IoT ist mit besonderen Geräteverwaltungsanforderungen verbunden. Daher müssen bei jeder professionellen Lösung folgende Prinzipien berücksichtigt werden:

![Geräteverwaltungsprinzipien – Grafik][img-dm_principles]

* **Skalierung und Automatisierung:** IoT-Lösungen müssen über unkomplizierte Tools verfügen, mit denen Routineaufgaben automatisiert werden können und eine relativ kleine Gruppe von Bedienern Millionen von Geräten verwalten kann. Bediener erwarten Tag für Tag, dass Gerätevorgänge per Remotezugriff und als Massenvorgang erledigt werden können und dass sie nur benachrichtigt werden, wenn Probleme direkte Aufmerksamkeit erfordern.
* **Flexibilität und Kompatibilität:** Das Spektrum der Geräte ist äußerst vielfältig. Verwaltungstools müssen an eine Vielzahl von Geräteklassen, -plattformen und -protokolle angepasst werden. Die Bediener müssen zahlreiche Arten von Geräten unterstützen können – von einfachen eingebetteten Chips für Einzelvorgänge bis hin zu leistungsfähigen Computern mit vollem Funktionsumfang.
* **Kontexterkennung:** IoT-Umgebungen sind dynamisch und verändern sich ständig. Die Zuverlässigkeit des Diensts ist von entscheidender Bedeutung. Bei den Vorgängen der Geräteverwaltung müssen SLA-Wartungsfenster, Netzwerk- und Stromversorgungszustände, Bedingungen während der Nutzung und die Geolocation von Geräten berücksichtigt werden, um sicherzustellen, dass sich Ausfallzeiten aufgrund von Wartungsarbeiten nicht negativ auf wichtige Geschäftsvorgänge auswirken oder zu gefährlichen Situationen führen.
* **Abdeckung zahlreicher Rollen:** Die Unterstützung der einzigartigen Workflows und Prozesse von IoT-Vorgängen ist sehr wichtig. Das Bedienpersonal muss trotz etwaiger Einschränkungen durch interne IT-Abteilungen harmonisch zusammenarbeiten  und relevante Echtzeitinformationen zu Gerätevorgängen auf nachhaltige Weise an Vorgesetzte und andere Personen mit Leitungsfunktion weitergeben.

## <a name="device-lifecycle"></a>Lebenszyklus von Geräten
Einige allgemeine Geräteverwaltungsphasen kommen in jedem IoT-Unternehmensprojekt vor. In Azure IoT umfasst der Gerätelebenszyklus fünf Phasen:

![Die fünf Gerätelebenszyklus-Phasen von Azure IoT: Planung, Bereitstellung, Konfiguration, Überwachung und Ausmusterung][img-device_lifecycle]

In jeder dieser fünf Phasen müssen mehrere Anforderungen von Gerätebedienern erfüllt werden, um eine umfassende Lösung zu erhalten:

* **Planung:** Bediener müssen in die Lage versetzt werden, ein Schema der Gerätemetadaten zu erstellen. Damit können sie eine Gruppe von Geräten einfach und präzise abfragen und Massenvorgänge zu Verwaltungszwecken durchführen. Mit dem Gerätezwilling können Sie die Gerätemetadaten in Form von Tags und Eigenschaften speichern.
  
    *Weiterführende Themen*: [Erste Schritte mit Gerätezwillingen][lnk-twins-getstarted], [Grundlegendes zu Gerätezwillingen][lnk-twins-devguide], [Tutorial: Verwenden von gewünschten Eigenschaften zum Konfigurieren von Geräten (Vorschau)][lnk-twin-properties]
* **Bereitstellung:** Neue Geräte können sicher für IoT Hub bereitgestellt werden, und Bediener können die Funktionen eines Geräts sofort ermitteln.  Verwenden Sie die IoT Hub-Identitätsregistrierung, um flexible Geräteidentitäten und Anmeldeinformationen zu erstellen, und führen Sie diesen Vorgang mithilfe eines Auftrags als Massenvorgang durch. Erstellen Sie Geräte, um ihre Funktionen und Bedingungen über Geräteeigenschaften im Gerätezwilling zu melden.
  
    *Weitere nützliche Informationen*: [Verwalten von Geräteidentitäten][lnk-identity-registry], [Massenverwaltung von IoT Hub-Geräte-Identitäten][lnk-bulk-identity], [Tutorial: Verwenden von gewünschten Eigenschaften zum Konfigurieren von Geräten (Vorschau)][lnk-twin-properties]
* **Konfiguration:** Massenvorgänge zum Ändern der Konfiguration und Firmwareupdates für Geräte können ohne Beeinträchtigung der Integrität und der Sicherheit durchgeführt werden. Verwenden Sie die gewünschten Eigenschaften, um diese Geräteverwaltungsvorgänge als Massenvorgang durchzuführen, oder verwenden Sie direkte Methoden und Broadcastaufträge.
  
    *Weitere nützliche Informationen*: [Verwenden von direkten Methoden][lnk-c2d-methods], [Aufrufen einer direkten Methode auf einem Gerät][lnk-methods-devguide], [Tutorial: Verwenden von gewünschten Eigenschaften zum Konfigurieren von Geräten (Vorschau)][lnk-twin-properties], [Planen und Übermitteln von Aufträgen][lnk-jobs], [Planen von Aufträgen auf mehreren Geräten][lnk-jobs-devguide]
* **Überwachung:** Überwachen Sie die allgemeine Geräteintegrität und den Status laufender Vorgänge, und weisen Sie Bediener auf Probleme hin, die ggf. ihre Aufmerksamkeit erfordern.  Wenden Sie den Gerätezwilling an, um es Geräten zu ermöglichen, Bedingungen und den Status von Updatevorgängen in Echtzeit zu melden. Erstellen Sie leistungsfähige Dashboardberichte, die mithilfe von Abfragen für den Gerätezwilling über neueste Probleme informieren.
  
    *Weitere nützliche Informationen*: [Tutorial: Verwenden von gewünschten Eigenschaften zum Konfigurieren von Geräten (Vorschau)][lnk-twin-properties], [Referenz – Abfragesprache für Zwillinge und Aufträge][lnk-query-language]
* **Ausmusterung:** Geräte werden nach einem Fehler oder Upgradezyklus oder am Ende der Lebensdauer ausgetauscht oder außer Betrieb gesetzt.  Verwenden Sie zur Verwaltung von Geräteinformationen den Gerätezwilling, falls das physische Gerät ausgetauscht wird (oder das Archiv, falls das Gerät ausgemustert wird). Verwenden Sie die IoT Hub-Identitätsregistrierung zum sicheren Sperren von Geräteidentitäten und Anmeldeinformationen.
  
    *Weitere nützliche Informationen*: [Tutorial: Verwenden von gewünschten Eigenschaften zum Konfigurieren von Geräten (Vorschau)][lnk-twin-properties], [Verwalten von Geräteidentitäten][lnk-identity-registry]

## <a name="device-management-patterns"></a>Geräteverwaltungsmuster
IoT Hub ermöglicht die folgenden Geräteverwaltungsmuster.  In den [Tutorials zur Geräteverwaltung][lnk-get-started] erfahren Sie ausführlicher, wie Sie diese Muster auf Ihr individuelles Szenario abstimmen und auf der Grundlage der grundlegenden Vorlagen neue Muster entwerfen.

* **Neustart:** Die Back-End-Anwendung informiert das Gerät mithilfe einer direkten Methode darüber, dass sie einen Neustart initiiert hat.  Das Gerät nutzt die gemeldeten Eigenschaften zum Aktualisieren des Neustartstatus des Geräts.
  
    ![Neustartmuster der Geräteverwaltung – Grafik][img-reboot_pattern]
* **Zurücksetzung auf Werkseinstellungen:** Die Back-End-Anwendung informiert das Gerät mithilfe einer direkten Methode darüber, dass sie einen Zurücksetzung auf die Werkseinstellungen initiiert hat.  Das Gerät nutzt die vom Gerätezwilling gemeldeten Eigenschaften zum Aktualisieren des Status der Zurücksetzung auf Werkseinstellung des Geräts.
  
    ![Muster für die Zurücksetzung auf Werkseinstellungen der Geräteverwaltung – Grafik][img-facreset_pattern]
* **Konfiguration:** Die Back-End-Anwendung verwendet die gewünschten Eigenschaften zum Konfigurieren der auf dem Gerät ausgeführten Software.  Das Gerät nutzt die gemeldeten Eigenschaften zum Aktualisieren des Konfigurationsstatus des Geräts.
  
    ![Konfigurationsmuster der Geräteverwaltung – Grafik][img-config_pattern]
* **Firmwareupdate:** Die Back-End-Anwendung informiert das Gerät mithilfe einer direkten Methode darüber, dass sie ein Firmwareupdate initiiert hat.  Das Gerät initiiert einen Prozess mit mehreren Schritten, um das Firmwareimage herunterzuladen, das Firmwareimage anzuwenden und schließlich wieder eine Verbindung mit dem IoT Hub-Dienst herzustellen.  Während dieses gesamten Prozesses mit mehreren Schritten, verwendet das Gerät die gemeldeten Eigenschaften, um die Fortschrittsanzeige und den Status des Geräts zu aktualisieren.
  
    ![Firmwareupdate-Muster der Geräteverwaltung – Grafik][img-fwupdate_pattern]
* **Meldung von Fortschritt und Status:** Das Anwendungs-Back-End führt Gerätezwillingsabfragen für eine Gruppe von Geräten durch, um den Status und Fortschritt von Aktionen zu melden, die auf dem Gerät ausgeführt werden.
  
    ![Muster für Fortschritts- und Statusmeldungen der Geräteverwaltung – Grafik][img-report_progress_pattern]

## <a name="next-steps"></a>Nächste Schritte
Auf der Grundlage der Funktionen, Muster und Codebibliotheken der IoT Hub-Geräteverwaltung können Sie IoT-Anwendungen erstellen, die in jeder Lebenszyklusphase des jeweiligen Geräts die Anforderungen professioneller IoT-Bediener erfüllen.

Weitere Informationen zu den Geräteverwaltungsfunktionen in IoT Hub finden Sie im Tutorial [Erste Schritte mit der Geräteverwaltung][lnk-get-started].

<!-- Images and links -->
[img-dm_principles]: media/iot-hub-device-management-overview/image4.png
[img-device_lifecycle]: media/iot-hub-device-management-overview/image5.png
[img-config_pattern]: media/iot-hub-device-management-overview/configuration-pattern.png
[img-facreset_pattern]: media/iot-hub-device-management-overview/facreset-pattern.png
[img-fwupdate_pattern]: media/iot-hub-device-management-overview/fwupdate-pattern.png
[img-reboot_pattern]: media/iot-hub-device-management-overview/reboot-pattern.png
[img-report_progress_pattern]: media/iot-hub-device-management-overview/report-progress-pattern.png

[lnk-twins-devguide]: iot-hub-devguide-device-twins.md
[lnk-get-started]: iot-hub-node-node-device-management-get-started.md
[lnk-twins-getstarted]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-hub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-query-language]: iot-hub-devguide-query-language.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-methods-devguide]: iot-hub-devguide-direct-methods.md
[lnk-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-jobs-devguide]: iot-hub-devguide-jobs.md



<!--HONumber=Nov16_HO5-->


