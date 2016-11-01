<properties
   pageTitle="Skalieren von Azure Functions | Microsoft Azure"
   description="Erfahren Sie, wie sich Azure Functions skalieren lässt, um die Anforderungen Ihrer ereignisgesteuerten Workloads zu erfüllen."
   services="functions"
   documentationCenter="na"
   authors="dariagrigoriu"
   manager="erikre"
   editor=""
   tags=""
   keywords="Azure Functions, Functions, Ereignisverarbeitung, Webhooks, dynamisches Compute, serverlose Architektur"/>

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="reference"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/03/2016"
   ms.author="dariagrigoriu"/>

# Skalieren von Azure Functions

## Einführung

Ein Vorteil von Azure Functions ist, dass Computeressourcen nur bei Bedarf genutzt werden. Das bedeutet, dass Sie nicht für virtuelle Computer im Leerlauf bezahlen oder Kapazitäten für den Fall reservieren müssen, dass sie möglicherweise benötigt werden. Stattdessen weist die Plattform Computeleistung zu, wenn Ihr Code ausgeführt wird, und skaliert diese bei Bedarf hoch, um die Last zu verarbeiten. Wenn der Code nicht mehr ausgeführt wird, wird die Leistung wieder herunterskaliert.

Der Mechanismus für diese neue Funktion ist der dynamische Serviceplan.

In diesem Artikel erhalten Sie einen Überblick über die Funktionsweise des dynamischen Serviceplans und die bedarfsgesteuerte Skalierung der Plattform zur Ausführung des Codes.

Wenn Sie noch nicht mit Azure Functions vertraut sind, lesen Sie den Artikel [Übersicht zu Azure Functions](functions-overview.md), um die Funktionsweise besser zu verstehen.

## Konfigurieren von Azure Functions

Zwei wichtige Einstellungen beziehen sich auf die Skalierung:

* [Azure App Service-Plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) oder dynamischer Serviceplan
* Größe des Arbeitsspeichers für die Ausführungsumgebung

Die Kosten einer Funktion ändern sich je nach Serviceplan, den Sie auswählen. Bei einem dynamischen Serviceplan richten sich die Kosten nach der Ausführungszeit, der Arbeitsspeichergröße und der Anzahl der Ausführungen. Gebühren fallen nur an, wenn Ihr Code tatsächlich ausgeführt wird.

Ein App Service-Plan hostet Ihre Funktionen auf vorhandenen virtuellen Computern, die auch zur Ausführung von anderem Code verwendet werden können. Die Bezahlung für diese virtuellen Computer erfolgt monatlich, und es entstehen keine weiteren Kosten für die Ausführung von Funktionen auf diesen Computern.

## Auswählen eines Serviceplans

Beim Erstellen von Funktionen können Sie zwischen der Ausführung nach einem dynamischen Serviceplan oder einem regulären [App Service-Plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) wählen. In einem App Service-Plan werden Ihre Funktionen auf einem dedizierten virtuellen Computer ausgeführt – auf die gleiche Weise, wie heute Web-Apps für Basic-, Standard- oder Premium-SKUs funktionieren. Dieser dedizierte virtuelle Computer ist Ihren Apps und Funktionen zugeordnet und steht immer zur Verfügung, unabhängig davon, ob Code aktiv ausgeführt wird oder nicht. Diese Option eignet sich besonders, wenn Sie über nicht ausgelastete virtuelle Computer verfügen, auf denen bereits anderer Code ausgeführt wird, oder wenn Sie Funktionen kontinuierlich oder nahezu kontinuierlich ausführen möchten. Mit einem virtuellen Computer werden Kosten von der Laufzeit und der Arbeitsspeichergröße entkoppelt. Daher können Sie die Kosten für viele Funktionen mit langer Ausführungsdauer auf die Kosten für einen oder mehrere virtuelle Computer begrenzen, auf denen sie ausgeführt werden.

[AZURE.INCLUDE [Dynamischer Serviceplan](../../includes/functions-dynamic-service-plan.md)]

<!---HONumber=AcomDC_0803_2016-->