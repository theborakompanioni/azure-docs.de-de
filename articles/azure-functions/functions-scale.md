<properties
   pageTitle="Skalieren von Azure Functions | Microsoft Azure"
   description="Erfahren Sie, wie sich Azure Functions skalieren lässt, um die Anforderungen Ihrer ereignisgesteuerten Workloads zu erfüllen."
   services="functions"
   documentationCenter="na"
   authors="eduardolaureano"
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
   ms.date="03/09/2016"
   ms.author="edlaure"/>
  
# Skalieren von Azure Functions
     
## Einführung

Einer der Vorteile von Azure Functions ist, dass Ihr ausgeführter Code Ressourcen nur dann verbraucht, wenn sie wirklich benötigt werden. Das bedeutet, dass Sie nicht für virtuelle Computer im Leerlauf bezahlen oder Kapazitäten für den Fall reservieren müssen, dass sie möglicherweise benötigt werden. Stattdessen weist die Plattform Rechenleistung zu, wenn Ihr Code ausgeführt wird, und skaliert diese bei Bedarf hoch, um die Last zu verarbeiten. Wenn der Code nicht mehr ausgeführt wird, wird die Leistung wieder herunterskaliert.

Der Mechanismus für diese neue Funktion ist der dynamische Serviceplan. Dieser neue Serviceplan stellt einen dynamischen Container für Ihren Code bereit, der sich bei Bedarf hochskalieren lässt. Ihnen wird nur die Arbeitsspeichermenge berechnet, die Ihr Code nutzt, sowie die Dauer der Ausführung, gemessen in Gigabytesekunden.

In diesem Artikel erhalten Sie einen Überblick über die Funktionsweise des dynamischen Serviceplans und die bedarfsgesteuerte Skalierung der Plattform zur Ausführung des Codes.

Wenn Sie noch nicht mit Azure Functions vertraut sind, lesen Sie die [Übersicht zu Azure Functions](functions-overview.md), um die Funktionsweise besser zu verstehen.

## Konfigurieren Ihrer Funktions-App

Im Zusammenhang mit der Skalierung müssen zwei wichtige Entscheidungen getroffen werden:

* [App Services-Plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) oder dynamischer Serviceplan 
* Größe des Arbeitsspeichers für die Ausführungsumgebung 

Die Kosten einer Funktion ändern sich je nach Art des Serviceplans, den Sie auswählen. Bei dynamischen Serviceplänen richten sich die Kosten nach der Ausführungszeit, der Arbeitsspeichergröße und der Anzahl der Ausführungen. Kosten entstehen nur, wenn Sie tatsächlich Code ausführen.

Bei regulären Serviceplänen können Sie Ihre Funktionen auf vorhandenen virtuellen Computern hosten, die auch zur Ausführung von anderem Code verwendet werden können. Die Bezahlung für diese virtuellen Computer erfolgt monatlich, und es entstehen keine weiteren Kosten für die Ausführung von Funktionen auf diesen Computern.

## Auswählen eines Serviceplans

Beim Erstellen von Funktions-Apps können Sie sich zwischen einem dynamischen Serviceplan (neu!) oder einem regulären [App Services-Plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) entscheiden. In einem App Services-Plan werden Ihre Funktionen auf einem dedizierten virtuellen Computer ausgeführt – auf die gleiche Weise, wie heute Web-Apps (für Basic-, Standard- oder Premium-SKUs) funktionieren. Dieser dedizierte virtuelle Computer ist Ihren Apps bzw. Funktionen zugeordnet und steht zur Verfügung, unabhängig davon, ob Code aktiv ausgeführt wird oder nicht. Diese Option eignet sich besonders, wenn Sie über virtuelle Computer verfügen, auf denen bereits anderer Code ausgeführt wird, die jedoch nicht vollständig ausgelastet sind, oder wenn Sie Funktionen kontinuierlich oder nahezu kontinuierlich ausführen möchten. Bei Verwendung eines virtuellen Computers hängen die Kosten nicht von der Laufzeit oder der Arbeitsspeichergröße ab – so können Sie die Kosten für eine große Anzahl von Funktionen mit langer Ausführungszeit auf die Kosten des virtuellen Computers bzw. der virtuellen Computer beschränken, auf dem/denen die Funktionen ausgeführt werden.

[AZURE.INCLUDE [Dynamischer Serviceplan](../../includes/functions-dynamic-service-plan.md)]

<!---HONumber=AcomDC_0427_2016-->