<properties
	pageTitle="Azure Functions-Trigger mit Timer | Microsoft Azure"
	description="Erfahren Sie, wie Trigger mit Timer in Azure Functions verwendet werden."
	services="functions"
	documentationCenter="na"
	authors="christopheranderson"
	manager="erikre"
	editor=""
	tags=""
	keywords="Azure Functions, Funktionen, Ereignisverarbeitung, dynamisches Compute, serverlose Architektur"/>

<tags
	ms.service="functions"
	ms.devlang="multiple"
	ms.topic="reference"
	ms.tgt_pltfrm="multiple"
	ms.workload="na"
	ms.date="08/22/2016"
	ms.author="chrande; glenga"/>

# Azure Functions-Trigger mit Timer

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Dieser Artikel erläutert das Konfigurieren von Triggern mit Timer in Azure Functions. Trigger mit Timer rufen Funktionen anhand eines Zeitplans einmalig oder wiederholt auf.

[AZURE.INCLUDE [Einführung](../../includes/functions-bindings-intro.md)]

## „function.json“ für Trigger mit Timer

Die Datei *function.json* enthält einen Zeitplanausdruck. Der folgende Zeitplan führt die Funktion beispielsweise einmal pro Minute aus:

```json
{
  "bindings": [
    {
      "schedule": "0 * * * * *",
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Der Trigger mit Timer übernimmt automatisch das horizontale Hochskalieren über mehrere Instanzen: Nur eine einzelne Instanz einer bestimmten Timerfunktion wird für alle Instanzen ausgeführt.

## Format des Zeitplanausdrucks

Der Zeitplanausdruck ist ein [CRON-Ausdruck](http://en.wikipedia.org/wiki/Cron#CRON_expression) mit sechs Feldern: `{second} {minute} {hour} {day} {month} {day of the week}`.

In vielen der online verfügbaren CRON-Ausdrücke wird das Feld „{second}“ (Sekunde) weggelassen. Wenn Sie also etwas aus einem dieser Dokumente kopieren, ist eine entsprechende Anpassung erforderlich, um das zusätzliche Feld zu berücksichtigen.

Im Anschluss finden Sie weitere Beispiele für Zeitplanausdrücke:

Alle 5 Minuten auslösen:

```json
"schedule": "0 */5 * * * *"
```

Einmal zu jeder vollen Stunde auslösen:

```json
"schedule": "0 0 * * * *",
```

Einmal alle zwei Stunden auslösen:

```json
"schedule": "0 0 */2 * * *",
```

Zwischen 9:00 und 17:00 Uhr jeweils einmal pro Stunde auslösen:

```json
"schedule": "0 0 9-17 * * *",
```

Täglich um 9:30 Uhr auslösen:

```json
"schedule": "0 30 9 * * *",
```

Werktags um 9:30 Uhr auslösen:

```json
"schedule": "0 30 9 * * 1-5",
```

## C#-Codebeispiel für Trigger mit Timer

Dieses C#-Codebeispiel schreibt bei jedem Auslösen der Funktion ein Protokoll.

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");    
}
```

## Nächste Schritte

[AZURE.INCLUDE [Nächste Schritte](../../includes/functions-bindings-next-steps.md)]

<!---HONumber=AcomDC_0824_2016-->