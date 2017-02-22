---
title: "Azure IoT Hub – IP-Verbindungsfilter | Microsoft-Dokumentation"
description: "Verwenden der IP-Filterung zum Blockieren von Verbindungen von bestimmten IP-Adressen für Ihren Azure IoT Hub. Sie können Verbindungen von einzelnen IP-Adressen oder Bereichen von IP-Adressen blockieren."
services: iot-hub
documentationcenter: 
author: BeatriceOltean
manager: timlt
editor: 
ms.assetid: f833eac3-5b5f-46a7-a47b-f4f6fc927f3f
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/05/2017
ms.author: boltean
translationtype: Human Translation
ms.sourcegitcommit: ddb729d29072724f691c178967b6181f6ce06df4
ms.openlocfilehash: a9207d116e9b7360865c950ba00210ed67c3e028


---

# <a name="use-ip-filters"></a>Verwenden von IP-Filtern

Die Sicherheit ist bei jeder IoT-Lösung, die auf Azure IoT Hub basiert, ein wichtiger Aspekt. Es kann sein, dass Sie bestimmte IP-Adressen im Rahmen Ihrer Sicherheitskonfiguration auf eine Negativ- oder Positivliste setzen müssen. Mit dem Feature _IP-Filter_ können Sie Regeln konfigurieren, mit denen Datenverkehr von bestimmten IPv4-Adressen abgelehnt oder zugelassen wird.

## <a name="when-to-use"></a>Einsatzgebiete

Wenn es hilfreich ist, die IoT Hub-Endpunkte für bestimmte IP-Adressen zu blockieren, gibt es zwei spezifische Anwendungsfälle:

- Wenn IoT Hub nur Datenverkehr von einem angegebenen Bereich mit IP-Adressen empfangen und den anderen Datenverkehr ablehnen soll. Ein Beispiel hierfür ist, wenn Sie IoT Hub mit [Azure Express Route] verwenden, um private Verbindungen zwischen IoT Hub und Ihrer lokalen Infrastruktur zu erstellen.
- Wenn Sie Datenverkehr von IP-Adressen ablehnen müssen, die vom IoT Hub-Administrator als verdächtig eingestuft wurden.

## <a name="how-filter-rules-are-applied"></a>Anwenden von Filterregeln

Die IP-Filterregeln werden auf IoT Hub-Dienstebene angewendet. Daher gelten die IP-Filterregeln für alle Verbindungen von Geräten und Back-End-Apps mit allen unterstützten Protokollen.

Alle Verbindungsversuche über eine IP-Adresse, die für eine IP-Ablehnungsregel in IoT Hub eine Übereinstimmung ergeben, werden mit dem Statuscode „401 – Nicht autorisiert“ und einer Beschreibung versehen. In der Antwortnachricht wird die IP-Regel nicht erwähnt.

## <a name="default-setting"></a>Standardeinstellung
Das Raster **IP-Filter** im Portal ist für ein IoT Hub standardmäßig leer. Diese Standardeinstellung bedeutet, dass der Hub Verbindungen über alle IP-Adressen akzeptiert. Die Standardeinstellung entspricht einer Regel, bei der der IP-Adressbereich 0.0.0.0/0 zulässig ist.

![][img-ip-filter-default]

## <a name="add-or-edit-an-ip-filter-rule"></a>Hinzufügen oder Bearbeiten einer IP-Filterregel

Wenn Sie eine IP-Filterregel hinzufügen, werden Sie zum Eingeben der folgenden Werte aufgefordert:

- Einen **Namen für die IP-Filterregel**, bei dem es sich um eine eindeutige alphanumerische Zeichenfolge mit maximal 128 Zeichen handeln muss (Groß-/Kleinschreibung wird berücksichtigt). Nur alphanumerische ASCII 7-Bit-Zeichen und die Zeichen `{'-', ':', '/', '\', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '''}` sind zulässig.
- Wählen Sie als **Aktion** für die IP-Filterregel**reject** (Ablehnen) oder **accept** (Zulassen).
- Geben Sie eine einzelne IPv4-Adresse oder einen Block mit IP-Adressen in CIDR-Notation ein. In CIDR-Notation steht 192.168.100.0/22 beispielsweise für die 1.024 IPv4-Adressen von 192.168.100.0 bis 192.168.103.255.

![][img-ip-filter-add-rule]

Nach dem Speichern der Regel wird eine Warnung mit dem Hinweis angezeigt, dass der Updatevorgang läuft.

![][img-ip-filter-save-new-rule]

Die Option **Hinzufügen** ist deaktiviert, wenn Sie das Maximum von zehn IP-Filterregeln erreichen.

Sie können eine vorhandene Regel bearbeiten, indem Sie auf die Zeile mit der Regel doppelklicken.

> [!NOTE]
> Das Ablehnen von IP-Adressen kann andere Azure-Dienste (z. B. Azure Stream Analytics, Azure Virtual Machines oder den Geräte-Explorer im Portal) an der Interaktion mit dem IoT Hub hindern.

## <a name="delete-an-ip-filter-rule"></a>Löschen einer IP-Filterregel

Wählen Sie zum Löschen einer IP-Filterregel im Raster mindestens eine Filterregel aus, und klicken Sie auf **Löschen**.

![][img-ip-filter-delete-rule]

## <a name="ip-filter-rule-evaluation"></a>Auswertung von IP-Filterregeln

IP-Filterregeln werden der Reihenfolge nach angewendet, und die erste Regel, die eine Übereinstimmung mit der IP-Adresse ergibt, bestimmt die Aktion (Zulassen oder Ablehnen).

Wenn Sie beispielsweise Adressen im Bereich 192.168.100.0/22 zulassen und alle anderen Adressen ablehnen möchten, sollte die erste Regel im Raster lauten, dass der Adressbereich 192.168.100.0/22 zulässig ist. Mit der nächsten Regel sollten alle Adressen abgelehnt werden, indem der Bereich 0.0.0.0/0 verwendet wird.

Sie können die Reihenfolge der IP-Filterregeln im Raster ändern, indem Sie auf die drei vertikal angeordneten Punkte am Anfang der Zeile klicken und Drag&Drop nutzen.

Klicken Sie auf **Speichern**, um die neue Reihenfolge der IP-Filterregeln zu speichern.

![][img-ip-filter-rule-order]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [Vorgangsüberwachung][lnk-monitor]
* [IoT Hub-Metriken][lnk-metrics]

<!-- Images -->
[img-ip-filter-default]: ./media/iot-hub-ip-filtering/ip-filter-default.png
[img-ip-filter-add-rule]: ./media/iot-hub-ip-filtering/ip-filter-add-rule.png
[img-ip-filter-save-new-rule]: ./media/iot-hub-ip-filtering/ip-filter-save-new-rule.png
[img-ip-filter-delete-rule]: ./media/iot-hub-ip-filtering/ip-filter-delete-rule.png
[img-ip-filter-rule-order]: ./media/iot-hub-ip-filtering/ip-filter-rule-order.png


<!-- Links -->

[IoT Hub developer guide]: iot-hub-devguide.md
[Azure Express Route]:  https://azure.microsoft.com/en-us/documentation/articles/expressroute-faqs/#supported-services

[lnk-monitor]: iot-hub-operations-monitoring.md
[lnk-metrics]: iot-hub-metrics.md


<!--HONumber=Feb17_HO3-->


