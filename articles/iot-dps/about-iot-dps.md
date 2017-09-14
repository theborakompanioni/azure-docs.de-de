---
title: "Übersicht zum Azure IoT Hub Device Provisioning-Dienst (Vorschau) | Microsoft-Dokumentation"
description: "Beschreibt die Gerätebereitstellung in Azure mit dem Device Provisioning-Dienst und IoT Hub"
services: iot-dps
keywords: 
author: nberdy
ms.author: nberdy
ms.date: 09/05/2017
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: eeed445631885093a8e1799a8a5e1bcc69214fe6
ms.openlocfilehash: a9df3f4e27e0d6e11b9d85a44467f3c62f453121
ms.contentlocale: de-de
ms.lasthandoff: 09/07/2017

---

# <a name="provisioning-devices-with-azure-iot-hub-device-provisioning-service-preview"></a>Bereitstellung von Geräten mit dem Azure IoT Hub Device Provisioning-Dienst (Vorschau)
Microsoft Azure bietet einen umfangreichen Satz von integrierten öffentlichen Clouddiensten für alle Ihre IoT-Lösunganforderungen. Der IoT Hub Device Provisioning-Dienst ist ein Hilfsdienst für IoT Hub, der die JIT-Bereitstellung im richtigen IoT-Hub ohne manuelles Eingreifen ermöglicht, sodass Kunden Millionen von Geräten sicher und skalierbar bereitstellen können.

## <a name="when-to-use-device-provisioning-service"></a>Wann sollte der Device Provisioning-Dienst verwendet werden?
Es gibt viele Bereitstellungsszenarien, in denen der Device Provisioning-Dienst eine hervorragende Wahl ist, um Verbindungen von Geräten mit IoT Hub herzustellen und sie für IoT Hub zu konfigurieren, z.B.:

* Bereitstellung ohne manuelles Eingreifen für eine einzelne IoT-Lösung ohne werkseitige Hartcodierung von IoT Hub-Verbindungsinformationen (Anfangssetup)
* Mehrere Hubs übergreifender Lastenausgleich für Geräte
* Herstellen der Verbindung von Geräten mit der IoT-Lösung ihrer Besitzer auf Basis der Verkaufstransaktionsdaten (Mehrinstanzenfähigkeit)
* Herstellen der Verbindung von Geräten mit einer bestimmten IoT-Lösung abhängig vom Anwendungsfall (Lösungsisolation)
* Herstellen der Verbindung eines Geräts mit IoT Hub mit der geringsten Wartezeit (Geo-Sharding)
* Erneute Bereitstellung basierend auf einer Änderung im Gerät
* Wechseln der Schlüssel, die vom Gerät verwendet werden, um eine Verbindung mit IoT Hub herzustellen (wenn keine X. 509-Zertifikate verwendet werden, um Verbindungen herstellen)

## <a name="behind-the-scenes"></a>Abläufe im Hintergrund
Alle im vorherigen Abschnitt aufgelisteten Szenarien können mit dem Bereitstellungsdienst für die Bereitstellung ohne manuelles Eingreifen mit demselben Ablauf ausgeführt werden. Viele der normalerweise mit der Bereitstellung verbundenen manuellen Schritte sind beim Device Provisioning-Dienst automatisiert, um die Bereitstellung von IoT-Geräten zu beschleunigen und das Risiko eines manuellen Fehlers zu senken. Im Folgenden wird beschrieben, was bei der Bereitstellung eines Geräts im Hintergrund geschieht. Der erste Schritt ist manuell, alle folgenden Schritte sind automatisiert.

![Grundlegender Bereitstellungsablauf](./media/about-iot-dps/dps-provisioning-flow.png)

1. Der Gerätehersteller fügt die Geräteregistrierungsinformationen der Registrierungsliste im Azure-Portal hinzu.
2. Das Gerät nimmt mit dem werkseitig festgelegten Bereitstellungsdienstendpunkt Kontakt auf. Das Gerät übergibt dem Bereitstellungsdienst seine Identifikationsinformationen, um seine Identität nachzuweisen.
3. Der Bereitstellungsdienst überprüft die Identität des Geräts entweder mit einer Nonce-Herausforderung ([Trusted Platform Module](https://trustedcomputinggroup.org/work-groups/trusted-platform-module/)) oder einer standardmäßigen X.509-Überprüfung (X.509), indem er Registrierungs-ID und Schlüssel anhand des Registrierungslisteneintrags überprüft.
4. Der Bereitstellungsdienst registriert das Gerät bei einem IoT Hub und füllt den [gewünschten Zwillingsstatus](../iot-hub/iot-hub-devguide-device-twins.md) des Geräts aus.
5. Der IoT Hub gibt die Geräte-ID-Informationen an den Bereitstellungsdienst zurück.
6. Der Bereitstellungsdienst gibt die IoT Hub-Verbindungsinformationen an das Gerät zurück. Das Gerät kann nun direkt Daten an den IoT Hub senden.
7. Das Gerät stellt eine Verbindung mit dem IoT Hub her.
8. Das Gerät ruft den gewünschten Status von seinem Gerätezwilling im IoT Hub ab.

## <a name="provisioning-process"></a>Bereitstellung
Es gibt zwei unterschiedliche Schritte im Bereitstellungsprozess eines Geräts, bei denen der Device Provisioning-Dienst einen Teil übernimmt, der unabhängig ausgeführt werden kann:

* Der **Fertigungsschritt**, in dem das Gerät werkseitig erstellt und vorbereitet wird, und
* Der **Cloudeinrichtungsschritt**, in dem der Device Provisioning-Dienst für die automatisierte Bereitstellung konfiguriert wird.

Beide Schritte fügen sich nahtlos in bestehende Fertigungs- und Bereitstellungsprozesse ein. Der Device Provisioning-Dienst vereinfacht auch einige Bereitstellungsprozesse, die mit viel manueller Arbeit zum Abrufen der Verbindungsinformationen auf das Gerät verbunden sind.

### <a name="manufacturing-step"></a>Fertigungsschritt
Dieser Schritt umfasst alles, was in der Fertigung geschieht. An diesem Schritt sind Hardwareentwickler und -hersteller, Integrator und/oder Endhersteller des Geräts beteiligt. Dieser Schritt betrifft das Erstellen der Hardware selbst.

Der Device Provisioning-Dienst führt keinen neuen Schritt in den Fertigungsprozess ein; stattdessen bindet er ihn in den vorhandenen Schritt ein, der die grundlegende Software und (im Idealfall) das HSM auf dem Gerät installiert. Statt dass in diesem Schritt eine Geräte-ID erstellt wird, wird das Gerät einfach mit den Bereitstellungsdienstinformationen programmiert, sodass es den Bereitstellungsdienst aufruft, um seine Verbindungsinformationen/IoT-Lösungszuweisung zu erhalten, wenn es eingeschaltet wird.

Auch in diesem Schritt liefert der Hersteller dem Bereitsteller/Bediener des Geräts Indentifikationsschlüsselinformationen. Dies ist möglicherweise so einfach wie die Bestätigung, dass alle Geräte über ein X.509-Zertifikat verfügen, das von einer Stammzertifizierungsstelle generiert wurde, die vom Bereitsteller/Bediener des Geräts bereitgestellt wurde, bis zum Extrahieren des öffentlichen Teils eines TPM-Endorsement Keys aus jedem TPM-Gerät. Diese Dienste werden heute von vielen Hardwareherstellern angeboten.

### <a name="cloud-setup-step"></a>Cloudeinrichtungsschritt
In diesem Schritt wird die Cloud für die ordnungsgemäße automatische Bereitstellung konfiguriert. Im Allgemeinen sind zwei Arten von Benutzern am Cloudeinrichtungsschritt beteiligt: eine Person (ein Gerätebediener), die weiß, wie Geräte erstmalig eingerichtet werden müssen, und eine andere Person (ein Lösungsoperator), die weiß, wie Geräte auf die IoT Hubs aufgeteilt werden sollen.

Eine einmalige anfängliche Einrichtung der Bereitstellung muss ausgeführt werden, und diese Aufgabe übernimmt im Allgemeinen der Lösungsoperator. Sobald der Bereitstellungsdienst konfiguriert ist, muss er nicht geändert werden, solange sich der Anwendungsfall nicht ändert.

Wenn der Dienst für die automatische Bereitstellung konfiguriert ist, muss er auf das Registrieren von Geräten vorbereitet werden. Dieser Schritt erfolgt durch den Gerätebediener, der die gewünschte Konfiguration der Geräte kennt und dafür verantwortlich ist, sicherzustellen, dass der Bereitstellungsdienst ordnungsgemäß die Identität des Geräts nachweisen kann, wenn es seinen IoT Hub sucht. Die Gerätebediener übernimmt die Indentifikationsschlüsselinformationen vom Hersteller und fügt sie der Registrierungsliste hinzu. Die Registrierungsliste kann später aktualisiert werden, indem neue Einträge hinzugefügt oder vorhandene Einträge mit den neuesten Informationen zu den Geräten aktualisiert werden.

## <a name="registration-and-provisioning"></a>Registrierung und Bereitstellung
*Bereitstellung* kann abhängig von der Branche, wo der Begriff verwendet wird, verschiedene Bedeutungen haben. Im Kontext der Bereitstellung von IoT-Geräten für ihre Cloudlösung ist sie ein aus zwei Teilen bestehender Prozess:

1. Im ersten Teil wird durch Registrieren des Geräts eine erstmalige Verbindung des Geräts mit der IoT-Lösung hergestellt.
2. Im zweiten Schritt wird die richtige Konfiguration des Geräts basierend auf den spezifischen Anforderungen der Lösung, für die es registriert wurde, angewendet.

Erst nach Ausführung beider Schritte ist das Gerät vollständig bereitgestellt. Einige Clouddienste ermöglichen nur den ersten Schritt des Bereitstellungsvorgangs, das Registrieren von Geräten beim Endpunkt der IoT-Lösung, aber nicht die Erstkonfiguration. Der Device Provisioning-Dienst automatisiert beide Schritte und sorgt so für eine nahtlose Bereitstellung des Geräts.

## <a name="features-of-the-device-provisioning-service"></a>Features des Device Provisioning-Diensts
Der Device Provisioning-Dienst hat viele Features, die ihn zur idealen Lösung zur Bereitstellung von Geräten machen.

* Unterstützung des **sicheren Nachweises** für Identitäten auf X.509- und TPM-Basis.
* Eine **Registrierungsliste** mit der vollständigen Aufzeichnung von Geräten/Gerätegruppen, die sich zu einem beliebigen Zeitpunkt registrieren könnten. Die Registrierungsliste enthält Informationen zu der gewünschten Konfiguration des Geräts, sobald es registriert ist, und sie kann jederzeit aktualisiert werden.
* **Mehrere Zuordnungsrichtlinien**, um zu steuern, wie der Device Provisioning-Dienst zur Unterstützung Ihrer Szenarien Geräte IoT Hubs zuweist.
* **Überwachungs- und Diagnoseprotokolle**, um sicherzustellen, dass alles ordnungsgemäß funktioniert.
* **Unterstützung für mehrere Hubs**, sodass der Device Provisioning-Dienst Geräte mehreren IoT Hubs zuweisen kann. Der Device Provisioning-Dienst kann mehrere Azure-Abonnements übergreifend mit Hubs kommunizieren.

Weitere Informationen zu den Konzepten und Features in Verbindung mit der Gerätebereitstellung finden Sie unter [IoT Hub Device Provisioning Service device concepts](concepts-device.md) (IoT Hub Device Provisioning-Dienst: Gerätekonzepte), [IoT Hub Device Provisioning Service concepts](concepts-service.md) (IoT Hub Device Provisioning-Dienst: Konzepte) und [IoT Hub Device Provisioning Service security concepts](concepts-security.md) (IoT Hub Device Provisioning-Dienst: Sicherheitskonzepte).

## <a name="cross-platform-support"></a>Plattformübergreifende Unterstützung
Der Device Provisioning-Dienst ist wie alle Azure IoT-Dienste plattformübergreifend mit einer Vielzahl von Betriebssystemen einsetzbar. Die öffentliche Vorschau unterstützt nur eine begrenzte Anzahl der zahlreichen Sprachen/Protokolle, die unterstützt werden, wenn der Device Provisioning-Dienst allgemein verfügbar ist. Für die öffentliche Vorschau unterstützt der Device Provisioning-Dienst sowohl für Geräte- als auch Dienstvorgänge nur HTTPS-Verbindungen. Das Geräte-SDK ist in C geschrieben und das Dienst-SDK in C#.

## <a name="regions"></a>Regionen
Die öffentliche Vorschau des Device Provisioning-Diensts ist in „USA, Osten“, „Europa, Westen“ und „Asien, Südosten“ verfügbar. Wir führen für alle Dienste eine laufend aktualisierte Liste der vorhandenen und neu angekündigten Regionen.

* [Azure-Regionen](https://azure.microsoft.com/regions/)

## <a name="availability"></a>Availability
Wir sorgen während der öffentlichen Vorschau für bestmögliche Verfügbarkeit des Diensts. Während der Vorschau ist keine Vereinbarung zum Servicelevel (SLA) gültig. Die vollständige [Azure-SLA](https://azure.microsoft.com/support/legal/sla/) erläutert die garantierte Verfügbarkeit von Azure insgesamt.

## <a name="quotas"></a>Kontingente
Für jedes Azure-Abonnement gelten standardmäßig bestimmte Kontingentgrenzen, die den Umfang Ihrer IoT-Lösung beeinträchtigen könnten. Die aktuelle Grenze auf Abonnementbasis sind 10 Device Provisioning-Dienste pro Abonnement.

Weitere Informationen zu Kontingentgrenzen finden Sie hier:

* [Einschränkungen für Azure-Abonnementdienste](../azure-subscription-service-limits.md)

## <a name="related-azure-components"></a>Verwandte Azure-Komponenten
Der Device Provisioning-Dienst automatisiert die Bereitstellung von Geräten unter Azure IoT Hub. Erfahren Sie mehr über [IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/).

## <a name="next-steps"></a>Nächste Schritte
Sie haben jetzt einen Überblick über die Bereitstellung von IoT-Geräten in Azure erhalten. Der nächste Schritt ist, dass Sie ein End-to-End-IoT-Szenario testen.
> [!div class="nextstepaction"]
> [Einrichten des IoT Hub Device Provisioning-Diensts (Vorschauversion) mit dem Azure-Portal](quick-setup-auto-provision.md)
> [Erstellen und Bereitstellen eines simulierten Geräts mithilfe des IoT Hub Device Provisioning-Diensts (Vorschauversion)](quick-create-simulated-device.md)
> [Set up a device to provision using the Azure IoT Hub Device Provisioning Service](tutorial-set-up-device.md) (Einrichten eines Geräts für die Bereitstellung mit dem Azure IoT Hub Device Provisioning-Dienst)

