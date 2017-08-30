---
title: "Azure App Service-Pläne – Detaillierte Übersicht | Microsoft Docs"
description: "Erfahren Sie, wie App Service-Pläne für Azure App Service funktionieren, und wie Ihre Verwaltungsumgebung davon profitiert."
keywords: App-Dienst, Azure App Service, Skalierung, skalierbar, App Services-Plan, App Service-Kosten
services: app-service
documentationcenter: 
author: btardif
manager: erikre
editor: 
ms.assetid: dea3f41e-cf35-481b-a6bc-33d7fc9d01b1
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: byvinyal
ms.translationtype: HT
ms.sourcegitcommit: 847eb792064bd0ee7d50163f35cd2e0368324203
ms.openlocfilehash: f97be571d104e3cc1c6ee732886fa7133ba0dc83
ms.contentlocale: de-de
ms.lasthandoff: 08/19/2017

---
# <a name="azure-app-service-plans-in-depth-overview"></a>Azure App Service-Pläne – Detaillierte Übersicht

App Service-Pläne stellen die Sammlung physischer Ressourcen dar, die zum Hosten Ihrer Apps verwendet werden.

In App Service-Plänen wird Folgendes definiert:

- Region („USA, Westen“, „USA, Osten“ usw.)
- Skalierung (Instanzenanzahl)
- Instanzgröße (klein, mittel, groß)
- SKU (Free, Shared, Basic, Standard, Premium)

Web-Apps, Mobile Apps, API-Apps und Funktionen-Apps (bzw. Funktionen) in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) werden jeweils unter einem App Service-Plan ausgeführt.  Apps im gleichen Abonnement und in der gleichen Region können den gleichen App Service-Plan nutzen. 

Alle einem **App Service-Plan** zugewiesenen Anwendungen teilen sich die durch den Plan definierten Ressourcen. Es spart Kosten, wenn Sie mehrere Apps in einem einzelnen App Service-Plan hosten.

Ihr **App Service-Plan** kann von den SKUs **Free** und **Shared** auf die SKUs **Basic**, **Standard** und **Premium** skaliert werden, um Zugriff auf weitere Ressourcen und Features zu erhalten.

Wenn Ihr App Service-Plan mindestens auf **Basic** festgelegt ist, können Sie die **Größe** und die Skalierung der virtuellen Computer steuern.

Wenn Ihr Plan also etwa für die Verwendung von zwei kleinen Instanzen der Standarddienstebene konfiguriert ist, werden alle Apps, die diesem Plan zugeordnet sind, unter beiden Instanzen ausgeführt. Außerdem haben die Apps Zugriff auf die Features der Standarddienstebene. Planinstanzen, auf denen Apps ausgeführt werden, sind vollständig verwaltbar und hochverfügbar.

> [!IMPORTANT]
> **SKU** und **Skalierung** des App Service-Plans haben Einfluss auf die Kosten (nicht auf die Anzahl gehosteter Apps).

Dieser Artikel beschäftigt sich mit zentralen Aspekten wie Tarif und Umfang eines App Service-Plans und deren Bedeutung für die App-Verwaltung.

## <a name="apps-and-app-service-plans"></a>Apps und App Service-Pläne

Jede App in App Service kann nur einem App Service-Plan gleichzeitig zugeordnet sein.

Apps und Pläne sind in einer **Ressourcengruppe** enthalten. Ressourcengruppen fungieren als Lebenszyklusgrenze für alle darin enthaltenen Ressourcen. Mit Ressourcengruppen können Sie alle Ihre Ressourcen in einer Anwendung gemeinsam verwalten.

Da eine einzelne Ressourcengruppe mehrere App Service-Pläne enthalten kann, können Sie verschiedene Apps unterschiedlichen physischen Ressourcen zuordnen.

Dadurch können Sie beispielsweise getrennte Ressourcen für die Entwicklungs-, Test- und Produktionsumgebung verwenden. Indem Sie separate Umgebungen für die Produktion und für die Entwicklung und das Testen verwenden, können Sie Ressourcen isolieren. Auf diese Weise konkurrieren Auslastungstests für eine neue Version Ihrer Apps nicht um dieselben Ressourcen wie Ihre Produktions-Apps, die für reale Kunden bereitstehen.

Wenn eine einzelne Ressourcengruppe mehrere Pläne enthält, können Sie auch eine Anwendung definieren, die sich über mehrere geografische Regionen erstreckt.

Eine hoch verfügbare Anwendung in zwei Regionen umfasst beispielsweise mindestens zwei Pläne – je einen pro Region – und eine App pro Plan. In diesem Fall sind alle Kopien der App in einer einzelnen Ressourcengruppe enthalten. Mit einer Ressourcengruppe mit mehreren Plänen und mehreren Apps können Sie die Integrität der Anwendung auf einfache Weise verwalten, steuern und anzeigen.

## <a name="create-an-app-service-plan-or-use-existing-one"></a>Erstellen eines App Service-Plans oder Verwenden eines vorhandenen Plans

Wenn Sie eine App erstellen, empfiehlt sich unter Umständen die Erstellung einer Ressourcengruppe. Wenn es sich bei der App um eine Komponente einer größeren Anwendung handelt, muss sie allerdings in der Ressourcengruppe erstellt werden, die der größeren Anwendung zugeordnet ist.

Zum Hosten der App können Sie einen vorhandenen Plan verwenden oder einen neuen erstellen. Das gilt sowohl für neue Anwendungen als auch für Apps, die Teil einer größeren Anwendung sind. Dies ist eher eine Frage der Kapazität und erwarteten Auslastung.

In folgenden Fällen empfiehlt es sich, die App in einem neuen App Service-Plan zu isolieren:

- Die App ist ressourcenintensiv.
- Für die App gelten andere Skalierungsfaktoren als für die anderen Apps, die in einen vorhandenen Plan gehostet werden.
- Die App benötigt Ressourcen in einer anderen geografischen Region.

Dadurch können Sie einen neuen Satz von Ressourcen für die App zuordnen und Ihre Apps noch präziser steuern.

## <a name="create-an-app-service-plan"></a>Wie erstelle ich einen Plan?

> [!TIP]
> Wenn Sie eine App Service-Umgebung verwenden, helfen Ihnen die Informationen im folgenden Artikel weiter: [Erstellen eines App Service-Plans in einer App Service-Umgebung](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan).

Sie können einen leeren App Service-Plan über die Browseroberfläche für App Service-Pläne oder im Rahmen der App-Erstellung erstellen.

Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **Neu** > **Web und mobil**, und wählen Sie dann die Option **Web-App** oder eine andere Art von App Service-App aus.

![Erstellen Sie eine App im Azure-Portal.][createWebApp]

Sie können dann den App Service-Plan für die neue Anwendung auswählen oder erstellen.

 ![Erstellen eines App Service-Plans][createASP]

Klicken Sie zum Erstellen eines App Service-Plans auf **[+] Neu erstellen**, geben Sie den Namen für den **App Service-Plan** ein, und wählen Sie einen geeigneten **Speicherort** aus. Klicken Sie auf **Tarif**, und wählen Sie einen geeigneten Tarif für den Dienst aus. Wählen Sie **Alle anzeigen** aus, um mehr Tarifoptionen anzuzeigen, z. B. **Free** und **Shared**. Klicken Sie nach dem Auswählen des Tarifs auf die Schaltfläche **Auswählen**.

## <a name="move-an-app-to-a-different-app-service-plan"></a>Wie kann ich eine App in einen anderen App Service-Plan verschieben?

Eine App kann über das [Azure-Portal](https://portal.azure.com) in einen anderen App Service-Plan verschoben werden. Das Verschieben von Apps zwischen Plänen ist möglich, wenn sich die Pläne in der gleichen Ressourcengruppe und geografischen Region befinden.

So verschieben Sie eine App in einen anderen Plan

- Navigieren Sie zur App, die Sie verschieben möchten.
- Wechseln Sie im **Menü** zum Abschnitt **App Service-Plan**.
- Wählen Sie **App Service-Plan ändern**, um den Vorgang zu starten.

Mit **App Service-Plan ändern** wird die Auswahlfunktion für den **App Service-Plan** geöffnet. Sie können nun einen vorhandenen Plan auswählen, in den diese App verschoben werden soll.

> [!IMPORTANT]
> Die Benutzeroberfläche des ausgewählten App Service-Plans wird anhand der folgenden Kriterien gefiltert:
> - In derselben Ressourcengruppe vorhanden
> - In derselben geografischen Region vorhanden
> - Im selben Webspace vorhanden
>
> Ein Webspace ist ein logisches Konstrukt in App Service, das eine Gruppierung von Serverressourcen definiert. Eine geografische Region (z.B. „USA, Westen“) enthält viele Webspaces, um Kunden mithilfe von App Service zuzuordnen. Derzeit können App Service-Ressourcen nicht zwischen Webspaces verschoben werden.
>

![Auswahlelement für App Service-Pläne][change]

Jeder Plan hat einen eigenen Tarif. Wenn Sie beispielsweise eine Website aus dem Free-Tarif in den Standard-Tarif verschieben, können alle zugewiesenen Apps die Features und Ressourcen des Standard-Tarifs nutzen.

## <a name="clone-an-app-to-a-different-app-service-plan"></a>Klonen einer App in einen anderen App Service-Plan

Wenn Sie die App in eine andere Region verschieben möchten, ist das Klonen der App eine Möglichkeit. Beim Klonen wird eine Kopie Ihrer App in einem neuen oder vorhandenen App Service-Plan in einer beliebigen Region erstellt.

Sie finden **App klonen** im Abschnitt **Entwicklungstools** des Menüs.

> [!IMPORTANT]
> Beim Klonen gelten einige Einschränkungen, über die Sie sich unter [Klonen der Azure App Service-App über das Azure-Portal](../app-service-web/app-service-web-app-cloning-portal.md)informieren können.

## <a name="scale-an-app-service-plan"></a>Skalieren eines App Service-Plans

Es gibt drei Möglichkeiten, einen Plan zu skalieren:

- **Ändern des Plantarifs:** Ein Plan im Basic-Tarif kann in den Standard-Tarif überführt werden, und alle zugewiesenen Apps können die Features des Standard-Tarifs nutzen.
- **Ändern der Instanzgröße des Plans:** Ein Plan der Preisstufe „Basic“ mit kleinen Instanzen kann beispielsweise geändert werden, um große Instanzen zu verwenden. Alle Apps, die diesem Plan zugeordnet sind, können jetzt die zusätzlichen Arbeitsspeicher- und CPU-Ressourcen nutzen, die aufgrund der größeren Instanzen verfügbar sind.
- **Ändern der Instanzanzahl des Plans:** Ein horizontal auf drei Instanzen hochskalierter Standard-Plan kann beispielsweise auf zehn Instanzen skaliert werden. Ein Premium-Plan kann horizontal auf 20 Instanzen hochskaliert werden (sofern verfügbar). Alle Apps, die diesem Plan zugeordnet sind, können jetzt die zusätzlichen Arbeitsspeicher- und CPU-Ressourcen nutzen, die aufgrund der höheren Anzahl von Instanzen verfügbar sind.

Sie können den Tarif und die Instanzgröße ändern, indem Sie für die App oder den App Service-Plan unter „Einstellungen“ auf die Option **Zentral hochskalieren** klicken. Änderungen gelten für den App Service-Plan und wirken sich auf alle Apps aus, die darunter gehostet werden.

 ![Festlegen von Werten zum zentralen Hochskalieren einer App][pricingtier]

## <a name="app-service-plan-cleanup"></a>Bereinigung des App Service-Plans

> [!IMPORTANT]
> Für **App Service-Pläne**, denen keine Apps zugeordnet sind, fallen trotzdem Gebühren an, da die Computekapazität weiterhin reserviert wird.

Um nach dem Löschen der letzten App, die unter einem App Service-Plan gehostet wird, die Berechnung von unerwarteten Gebühren zu vermeiden, wird der sich ergebende leere App Service-Plan ebenfalls gelöscht.

## <a name="summary"></a>Zusammenfassung

App Service-Pläne stellen einen Satz an Funktionen und Kapazitäten dar, die Sie App-übergreifend gemeinsam nutzen können. Mit App Service-Plänen können Sie bestimmte Apps flexibel einer bestimmten Gruppe von Ressourcen zuordnen und die Auslastung Ihrer Azure-Ressourcen weiter optimieren. Dadurch können Sie beispielsweise in Ihrer Testumgebung Geld sparen, indem Sie einen Plan für mehrere Apps verwenden. Sie können außerdem den Durchsatz für Ihre Produktionsumgebung maximieren, indem Sie sie über mehrere Regionen und Pläne skalieren.

## <a name="whats-changed"></a>Änderungen

- Informationen zu den Änderungen zwischen Websites und App Service finden Sie unter [Azure App Service und vorhandene Azure-Dienste](http://go.microsoft.com/fwlink/?LinkId=529714)

[pricingtier]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/appserviceplan-pricingtier.png
[assign]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/assing-appserviceplan.png
[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png

