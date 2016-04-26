<properties
   pageTitle="Übersicht zu Azure Functions | Microsoft Azure"
   description="Es wird beschrieben, wie Sie mit Azure Functions asynchrone Workloads optimieren können, indem Sie einfache Funktionen erstellen, die innerhalb weniger Minuten geschrieben werden können."
   services="functions"
   documentationCenter="na"
   authors="mattchenderson"
   manager="erikre"
   editor=""
   tags=""
   keywords="Azure Functions, Functions, Ereignisverarbeitung, Webhooks, dynamisches Compute, serverlose Architektur"/>

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="03/09/2016"
   ms.author="cfowler;mahender"/>
   
   
# Übersicht zu Azure Functions

## Eine schnellere Möglichkeit zur Verwendung von Funktionen

Schreiben Sie jede Funktion in weniger als einer Minute – entweder für einen einfachen Auftrag zur Bereinigung einer Datenbank oder für die Erstellung von Funktionalität, mit der Millionen von Nachrichten von verbundenen Geräten verarbeitet werden. Verwenden Sie Ihre bevorzugte Programmiersprache (C#, Node.JS, Python usw.). Bezahlen Sie nur für die Zeit, in der Ihr Code ausgeführt wird, und setzen Sie auf die flexiblen Möglichkeiten zur Skalierung von Azure.

Azure Functions ist eine Lösung für das einfache Ausführen kleiner Codeelemente oder „Funktionen“ in der Cloud. Sie können sich auf das Schreiben des Codes beschränken, der für das jeweilige Problem erforderlich ist, ohne sich über eine gesamte Anwendung oder die Infrastruktur für ihre Ausführung Gedanken machen zu müssen. Dadurch kann die Entwicklung noch produktiver werden, und Sie können in wenigen Minuten [mit Ihrer ersten Funktion beginnen](functions-create-first-azure-function.md).

## Welche Möglichkeiten bestehen mit Functions?

Azure Functions ist eine hervorragende Lösung für die Verarbeitung von Daten und das Integrieren von Systemen, Arbeiten mit dem Internet der Dinge (IoT) und Erstellen einfacher APIs und Microservices.

Functions enthält einen Katalog mit Vorlagen für häufige Szenarien, z.B.:

* Reagieren auf eine GitHub-Webhookanforderung
* Ändern der Größe eines Bilds, das in Azure Storage hochgeladen wurde
* Verwenden von Warteschlangen zur Verarbeitung von Bestellungen
* Und vieles mehr! 

Ausführlichere Informationen zur Funktionsweise von Functions und einige Beispielkonfigurationen finden Sie in der [Entwicklerreferenz](functions-reference.md).

## Merkmale

Azure Functions ist eine für Unternehmen geeignete Plattform mit vollem Funktionsumfang, mit deren Features komplizierte Integrations- und Konnektivitätsaufgaben leicht durchgeführt werden können. Mit diesem Kernsatz von Funktionen können Entwickler, die Azure Functions nutzen, noch produktiver werden, indem sie sich nur auf ihr Ziel konzentrieren, anstatt Infrastrukturelemente zusammenzusetzen.

Die folgenden Features sind in Azure Functions enthalten:
    
* **Freie Sprachwahl:** Schreiben Sie Funktionen mit C#, Node.js, Python, F#, PHP, Batch, Bash, Java oder beliebigen anderen ausführbaren Programmen.  
* **Preismodell mit nutzungsbasierter Bezahlung:** Bezahlen Sie nur für die Zeit, in der Ihr Code ausgeführt wird. Weitere Informationen finden Sie unter der Option „Dynamischer App Service-Plan“ unten im [Abschnitt zu den Preisen](#pricing).  
* **Eigene Abhängigkeiten (Bring Your Own Dependencies):** Functions unterstützt NuGet und NPM, sodass Sie Ihre bevorzugten Bibliotheken verwenden können.  
* **Integrierte Sicherheit:** Schützen Sie per HTTP ausgelöste Funktionen mit OAuth-Anbietern wie Azure Active Directory, Facebook, Google, Twitter und Microsoft-Konto.  
* **Integration ohne Code:** Profitieren Sie von der einfachen Nutzung von Azure-Diensten und SaaS-Angeboten (Software-as-a-Service). Beispiele finden Sie unten im [Abschnitt zu Integrationen](#integrations).  
* **Flexible Entwicklung:** Ändern Sie Ihre Funktionen mit einem Editor im Portal, oder richten Sie die fortlaufende Integration ein, und stellen Sie Ihren Code über GitHub, Visual Studio Team Services usw. bereit.  
* **Open Source:** Functions ist eine Open-Source-Anwendung und [bei GitHub verfügbar](https://github.com/azure/azure-webjobs-sdk-script).  

### <a name="integrations"></a>Integrationen

Azure Functions unterstützt eine Reihe von Integrationen in Azure und Drittanbieterdienste. Diese können Sie zum Auslösen Ihrer Funktion und zum Starten der Ausführung oder als Eingabe und Ausgabe für Ihren Code verwenden. Die folgende Tabelle enthält einige Beispielintegrationen, die von Azure Functions unterstützt werden.

[AZURE.INCLUDE [Dynamisches Compute](../../includes/functions-bindings.md)]

## <a name="pricing"></a>Was kostet Functions?

Es gibt zwei Möglichkeiten, Azure Functions auszuführen: mit einem dynamischen App Service-Plan und mit einem klassischen App Service-Plan.

Bei einem **dynamischen App Service-Plan** müssen Sie sich keine Gedanken über die Ressourcenverwaltung machen. Bei jeder Ausführung Ihrer Funktion stellt Azure alle erforderlichen Verarbeitungsressourcen bereit. Sie bezahlen nur für den Zeitraum, in dem Ihr Code ausgeführt wird. Eine Preisübersicht finden Sie auf der Seite [Azure Functions – Preise](/pricing/details/functions).

Bei einem **klassischen App Service-Plan** können Sie Funktionen genauso wie Ihre Web-, mobilen und API-Apps ausführen. Dies ist eine hervorragende Lösung, wenn Sie App Service bereits für andere Anwendungen verwenden. Ihre Funktionen können ohne zusätzliche Kosten unter demselben Plan ausgeführt werden. Ausführliche Informationen finden Sie auf der Seite mit der [App Service-Preisübersicht](/pricing/details/app-service/).

## Melden von Problemen

[AZURE.INCLUDE [Melden von Problemen](../../includes/functions-reporting-issues.md)]

<!---HONumber=AcomDC_0420_2016-->