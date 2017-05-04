---
title: "Azure Application Insights-Telemetriedatenmodell – Abhängigkeitstelemetrie | Microsoft-Dokumentation"
description: "Application Insights-Datenmodell für Abhängigkeitstelemetrie"
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: azakonov-ms
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/17/2017
ms.author: sergkanz
translationtype: Human Translation
ms.sourcegitcommit: 9eafbc2ffc3319cbca9d8933235f87964a98f588
ms.openlocfilehash: b86b1e1ff7c0370918b2509f98ad65265eb4186e
ms.lasthandoff: 04/22/2017


---
# <a name="dependency-telemetry-application-insights-data-model"></a>Abhängigkeitstelemetrie: Application Insights-Datenmodell

Abhängigkeitstelemetrie stellt eine Interaktion der überwachten Komponente mit einer Remotekomponente wie SQL oder einem HTTP-Endpunkt dar.

## <a name="name"></a>Name

Name des Befehls, der mit diesem Abhängigkeitsaufruf ausgelöst wird. Niedriger Kardinalitätswert. Beispiele sind der Name einer gespeicherten Prozedur und eine URL-Pfadvorlage.

## <a name="id"></a>ID

Bezeichner einer Instanz eines Aufrufs einer Abhängigkeit. Dient zur Korrelation mit dem Anforderungstelemetrieelement, das diesem Aufruf einer Abhängigkeit entspricht. Weitere Informationen hierzu finden Sie auf der Seite [Korrelation](/correlation.md).

## <a name="data"></a>Daten

Der durch diesen Aufruf einer Abhängigkeit ausgelöste Befehl. Beispiele sind eine SQL-Anweisung und HTTP-URL mit allen Abfrageparametern.

## <a name="type"></a>Typ

Name des Abhängigkeitstyps. Niedriger Kardinalitätswert für die logische Gruppierung von Abhängigkeiten und Interpretation der anderen Felder wie „commandName“ und „resultCode“. Beispiele sind die SQL, Azure-Tabelle und HTTP.

## <a name="target"></a>Ziel

Zielstandort eines Aufrufs einer Abhängigkeit. Beispiele sind Servername und Hostadresse. Weitere Informationen hierzu finden Sie auf der Seite [Korrelation](/correlation.md).

## <a name="duration"></a>Duration

Dauer der Anforderung im Format `DD.HH:MM:SS.MMMMMM`. Muss kleiner als `1000` Tage sein.

## <a name="result-code"></a>Ergebniscode

Ergebniscode eines Aufrufs einer Abhängigkeit. Beispiele sind SQL-Fehlercode und HTTP-Statuscode.

## <a name="success"></a>Erfolgreich

Angabe eines erfolgreichen oder fehlgeschlagenen Aufrufs.

## <a name="custom-properties"></a>Benutzerdefinierte Eigenschaften

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Benutzerdefinierte Messungen

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]


## <a name="next-steps"></a>Nächste Schritte

- Informationen zu den Application Insights-Typen und zum Datenmodell finden Sie unter [Datenmodell](/application-insights-data-model.md).
- Einrichten der Abhängigkeitsnachverfolgung für [.NET](/app-insights-asp-net-dependencies.md).
- Einrichten der Abhängigkeitsnachverfolgung für [Java](/app-insights-java-agent.md).
- Überprüfen der von Application Insights unterstützten [Plattformen](/app-insights-platforms.md).

